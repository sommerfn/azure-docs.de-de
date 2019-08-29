---
title: 'Azure Active Directory Domain Services: Verknüpfen eines virtuellen Computers mit CoreOS Linux | Microsoft-Dokumentation'
description: Einbinden einer CoreOS Linux-VM in Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: c1f3d1ec7bb9e9f449cea3f9aa36ca8f80348c6e
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69612820"
---
# <a name="join-a-coreos-linux-virtual-machine-to-a-managed-domain"></a>Einbinden einer CoreOS Linux-VM in eine verwaltete Domäne
In diesem Artikel wird beschrieben, wie Sie eine CoreOS Linux-VM in eine durch Azure AD Domain Services verwaltete Domäne einbinden.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Voraussetzungen
Um die in diesem Artikel beschriebenen Aufgaben auszuführen, benötigen Sie Folgendes:
1. Ein gültiges **Azure-Abonnement**.
2. Ein **Azure AD-Verzeichnis** – entweder synchronisiert mit einem lokalen Verzeichnis oder als reines Cloud-Verzeichnis
3. **Azure AD Domain Services** müssen für das Azure AD-Verzeichnis aktiviert sein. Wenn dies noch nicht der Fall ist, führen Sie alle Aufgaben im Leitfaden [Erste Schritte](tutorial-create-instance.md)aus.
4. Stellen Sie sicher, dass Sie die IP-Adressen der verwalteten Domäne nicht als DNS-Server für das virtuelle Netzwerk konfiguriert haben. Weitere Informationen finden Sie unter [Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk](tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network).
5. Führen Sie die Schritte aus, die zum [Synchronisieren der Kennwörter für Ihre mit Azure AD Domain Services verwaltete Domäne](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds) erforderlich sind.


## <a name="provision-a-coreos-linux-virtual-machine"></a>Bereitstellen einer CoreOS Linux-VM
Stellen Sie mithilfe einer der folgenden Methoden eine CoreOS-VM in Azure bereit:
* [Azure-Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure-Befehlszeilenschnittstelle](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

In diesem Artikel wird die **stabile Version des CoreOS Linux**-VM-Images in Azure verwendet.

> [!IMPORTANT]
> * Stellen Sie den virtuellen Computer in **demselben virtuellen Netzwerk bereit, in dem Sie Azure AD Domain Services aktiviert haben**.
> * Wählen Sie ein **anderes Subnetz** als das aus, in dem Sie Azure AD Domain Services aktiviert haben.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Herstellen einer Remoteverbindung mit dem neu bereitgestellten virtuellen Linux-Computer
Die CoreOS-VM wurde in Azure bereitgestellt. Die nächste Aufgabe besteht darin, über das lokale Administratorkonto, das während der Bereitstellung des virtuellen Computers erstellt wurde, eine Remoteverbindung mit dem virtuellen Computer herzustellen.

Befolgen Sie die Anweisungen im Artikel [Anmelden bei einem virtuellen Computer mit Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Konfigurieren der Hostdatei auf dem virtuellen Linux-Computer
Bearbeiten Sie in Ihrem SSH-Terminal die Datei „/etc/hosts“, und aktualisieren Sie die IP-Adresse und den Hostnamen Ihres Computers.

```console
sudo vi /etc/hosts
```

Geben Sie in der Hostdatei die folgenden Wert ein:

```console
127.0.0.1 contoso-coreos.contoso.com contoso-coreos
```

„contoso.com“ steht für den DNS-Domänennamen Ihrer verwalteten Domäne. „contoso-coreos“ steht für den Hostnamen der CoreOS-VM, die Sie in die verwaltete Domäne einbinden.


## <a name="configure-the-sssd-service-on-the-linux-virtual-machine"></a>Konfigurieren des SSSD-Diensts in der Linux-VM
Aktualisieren Sie als Nächstes Ihre SSSD-Konfigurationsdatei („/etc/sssd/sssd.conf“), sodass diese mit der folgenden Datei übereinstimmt:

```console
[sssd]
config_file_version = 2
services = nss, pam
domains = contoso.COM

[domain/contoso.COM]
id_provider = ad
auth_provider = ad
chpass_provider = ad

ldap_uri = ldap://contoso.com
ldap_search_base = dc=contoso,dc=com
ldap_schema = rfc2307bis
ldap_sasl_mech = GSSAPI
ldap_user_object_class = user
ldap_group_object_class = group
ldap_user_home_directory = unixHomeDirectory
ldap_user_principal = userPrincipalName
ldap_account_expire_policy = ad
ldap_force_upper_case_realm = true
fallback_homedir = /home/%d/%u

krb5_server = contoso.com
krb5_realm = contoso.COM
```

Ersetzen Sie „contoso.com“ mit dem DNS-Domänennamen Ihrer verwalteten Domäne. Stellen Sie sicher, dass Sie den Domänennamen in der Konfigurationsdatei in Großbuchstaben angeben.


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Einbinden der virtuellen Linux-Computers in die verwaltete Domäne
Nachdem die erforderlichen Pakete auf dem virtuellen Linux-Computer installiert sind, besteht die nächste Aufgabe darin, den virtuellen Computer in die verwaltete Domäne einzubinden.

```console
sudo adcli join -D contoso.COM -U bob@contoso.COM -K /etc/krb5.keytab -H contoso-coreos.contoso.com -N coreos
```


> [!NOTE]
> **Problembehandlung:** Gehen Sie folgendermaßen vor, wenn Ihre verwaltete Domäne über *adcli* nicht gefunden werden kann:
>   * Stellen Sie durch Pingen sicher, dass die VM die Domäne erreichen kann.
>   * Überprüfen Sie, ob die VM tatsächlich in demselben virtuellen Netzwerk bereitgestellt wurde, in dem die verwaltete Domäne verfügbar ist.
>   * Überprüfen Sie, ob Sie die DNS-Servereinstellungen aktualisiert haben, sodass das virtuelle Netzwerk auf die Domänencontroller der verwalteten Domäne verweist.

Starten Sie den SSSD-Dienst. Geben Sie in Ihrem SSH-Terminal folgenden Befehl ein:
  
```console
sudo systemctl start sssd.service
```


## <a name="verify-domain-join"></a>Überprüfen des Domänenbeitritts
Überprüfen Sie, ob der Computer der verwalteten Domäne erfolgreich beigetreten ist. Stellen Sie über eine andere SSH-Verbindung eine Verbindung mit der Domäne her, der die CoreOS-VM beigetreten ist. Verwenden Sie ein Domänenbenutzerkonto, und überprüfen Sie anschließend, ob das Benutzerkonto ordnungsgemäß aufgelöst wurde.

1. Geben Sie in Ihrem SSH-Terminal den folgenden Befehl ein, um über SSH eine Verbindung mit der der Domäne beigetretenen CoreOS-VM herzustellen. Verwenden Sie ein Domänenkonto, das zu der verwalteten Domäne gehört (in diesem Fall z.B. 'bob@contoso.COM').
    
    ```console
    ssh -l bob@contoso.COM contoso-coreos.contoso.com
    ```

2. Geben Sie in Ihrem SSH-Terminal den folgenden Befehl ein, um zu ermitteln, ob das Basisverzeichnis ordnungsgemäß initialisiert wurde.
    
    ```console
    pwd
    ```

3. Geben Sie in Ihrem SSH-Terminal den folgenden Befehl ein, um zu ermitteln, ob die Gruppenmitgliedschaften ordnungsgemäß aufgelöst wurden.
   
    ```console
    id
    ```


## <a name="troubleshooting-domain-join"></a>Problembehandlung beim Domänenbeitritt
Informationen finden Sie im Artikel [Problembehandlung beim Domänenbeitritt](join-windows-vm.md#troubleshoot-domain-join-issues) .

## <a name="related-content"></a>Verwandte Inhalte
* [Azure AD-Domänendienste – Leitfaden zu den ersten Schritten](tutorial-create-instance.md)
* [Einbinden eines virtuellen Windows Server-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-windows-vm.md)
* [Anmelden bei einem virtuellen Computer, auf dem Linux ausgeführt wird](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
