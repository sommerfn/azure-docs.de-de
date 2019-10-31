---
title: Einbinden eines virtuellen CoreOS-Computers in Azure AD Domain Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen virtuellen CoreOS-Computer in eine durch Azure AD Domain Services verwaltete Domäne einbinden.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 09/14/2019
ms.author: iainfou
ms.openlocfilehash: 4cdc2fff05270a296d9c4c9151f73cadeb2a1cfc
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72754394"
---
# <a name="join-a-coreos-virtual-machine-to-an-azure-ad-domain-services-managed-domain"></a>Einbinden eines virtuellen CoreOS-Computers in eine durch Azure AD Domain Services verwaltete Domäne

Damit Benutzer sich mit einem einzigen Satz von Anmeldeinformationen bei virtuellen Computern (VMs) in Azure anmelden können, binden Sie VMs in eine durch Azure Active Directory Domain Services (AD DS) verwaltete Domäne ein. Wenn Sie eine VM in eine durch Azure AD DS verwaltete Domäne einbinden, können Benutzerkonten und Anmeldeinformationen aus der Domäne zum Anmelden und Verwalten von Servern verwendet werden. Gruppenmitgliedschaften aus der durch Azure AD DS verwalteten Domäne werden ebenfalls angewendet, damit Sie den Zugriff auf Dateien oder Dienste auf der VM steuern können.

In diesem Artikel wird gezeigt, wie Sie eine CoreOS-VM in eine durch Azure AD DS verwaltete Domäne einbinden.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie die folgenden Ressourcen und Berechtigungen:

* Ein aktives Azure-Abonnement.
    * Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Einen mit Ihrem Abonnement verknüpften Azure Active Directory-Mandanten, der entweder mit einem lokalen Verzeichnis synchronisiert oder ein reines Cloudverzeichnis ist.
    * [Erstellen Sie einen Azure Active Directory-Mandanten][create-azure-ad-tenant], oder [verknüpfen Sie ein Azure-Abonnement mit Ihrem Konto][associate-azure-ad-tenant], sofern erforderlich.
* Eine verwaltete Azure Active Directory Domain Services-Domäne, die in Ihrem Azure AD-Mandanten aktiviert und konfiguriert ist.
    * Falls Sie keine solche Domäne haben, gehen Sie wie im ersten Tutorial beschrieben vor, um eine [Azure Active Directory Domain Services-Instanz zu erstellen und zu konfigurieren][create-azure-ad-ds-instance].
* Ein Benutzerkonto, das Mitglied der *Administratorengruppe für Azure AD-Domänencontroller* (AAD-DC-Administratoren) in Ihrem Azure AD-Mandanten ist.

## <a name="create-and-connect-to-a-coreos-linux-vm"></a>Erstellen einer CoreOS Linux-VM und Herstellen einer Verbindung

Wenn Sie über eine vorhandene CoreOS Linux-VM in Azure verfügen, stellen Sie über SSH eine Verbindung mit dieser VM her. Dann fahren Sie mit dem nächsten Schritt fort und beginnen mit der [Konfiguration der VM](#configure-the-hosts-file).

Wenn Sie eine CoreOS Linux-VM erstellen müssen oder eine Test-VM zur Verwendung mit diesem Artikel erstellen möchten, können Sie eine der folgenden Methoden verwenden:

* [Azure-Portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure-Befehlszeilenschnittstelle](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Achten Sie beim Erstellen der VM auf die Einstellungen des virtuellen Netzwerks, um sicherzustellen, dass die VM mit der durch Azure AD DS verwalteten Domäne kommunizieren kann:

* Stellen Sie die VM in demselben oder einem mittels Peering verbundenen virtuellen Netzwerk bereit, in dem Sie Azure AD Domain Services aktiviert haben.
* Stellen Sie die VM in einem anderen Subnetz als die Azure AD Domain Services-Instanz bereit.

Nachdem die VM bereitgestellt wurde, führen Sie die Schritte zum Herstellen einer Verbindung mit der VM über SSH aus.

## <a name="configure-the-hosts-file"></a>Konfigurieren der Datei „hosts“

Um sicherzustellen, dass der VM-Hostname ordnungsgemäß für die verwaltete Domäne konfiguriert ist, bearbeiten Sie die Datei */etc/hosts*, und legen Sie den Hostnamen fest:

```console
sudo vi /etc/hosts
```

Aktualisieren Sie in der Datei *hosts* die Adresse *localhost*. Siehe folgendes Beispiel:

* *contoso.com* steht für den DNS-Domänennamen Ihrer durch Azure AD DS verwalteten Domäne.
* *coreos* ist der Hostname Ihrer CoreOS-VM, die Sie in die verwaltete Domäne einbinden.

Aktualisieren Sie diese Namen mit Ihren eigenen Werten:

```console
127.0.0.1 coreos coreos.contoso.com
```

Anschließend speichern und beenden Sie die Datei *hosts* mit dem Befehl `:wq` im Editor.

## <a name="configure-the-sssd-service"></a>Konfigurieren des SSSD-Diensts

Aktualisieren Sie die SSSD-Konfigurationsdatei */etc/sssd/sssd.conf*.

```console
sudo vi /etc/sssd/sssd.conf
```

Geben Sie für die folgenden Parameter den Namen Ihrer eigenen durch Azure AD DS verwalteten Domäne an:

* *domains* in Großbuchstaben
* *[domain/CONTOSO]* mit CONTOSO in Großbuchstaben
* *ldap_uri*
* *ldap_search_base*
* *krb5_server*
* *krb5_realm* in Großbuchstaben

```console
[sssd]
config_file_version = 2
services = nss, pam
domains = CONTOSO.COM

[domain/CONTOSO.COM]
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
krb5_realm = CONTOSO.COM
```

## <a name="join-the-vm-to-the-managed-domain"></a>Einbinden der VM in die verwaltete Domäne

Nachdem Sie die SSSD-Konfigurationsdatei aktualisiert haben, binden Sie nun den virtuellen Computer in die verwaltete Domäne ein.

1. Verwenden Sie zunächst den Befehl `adcli info`, um zu überprüfen, ob Sie Informationen über die durch Azure AD DS verwaltete Domäne anzeigen können. Im folgenden Beispiel werden Informationen für die Domäne *CONTOSO.COM* abgerufen. Geben Sie den Namen Ihrer eigenen durch Azure AD DS verwalteten Domäne in Großbuchstaben an:

    ```console
    sudo adcli info CONTOSO.COM
    ```

   Wenn mit dem Befehl `adcli info` Ihre durch Azure AD DS verwaltete Domäne nicht gefunden werden kann, führen Sie die folgenden Schritte zur Problembehandlung aus:

    * Vergewissern Sie sich, dass die VM die Domäne erreichen kann. Versuchen Sie `ping contoso.com`, um zu überprüfen, ob eine positive Antwort zurückgegeben wird.
    * Überprüfen Sie, ob die VM in demselben oder einem mittels Peering verbundenen virtuellen Netzwerk bereitgestellt wurde, in dem die durch Azure AD DS verwaltete Domäne verfügbar ist.
    * Stellen Sie sicher, dass die DNS-Servereinstellungen für das virtuelle Netzwerk so aktualisiert wurden, dass auf die Domänencontroller der durch Azure AD DS verwalteten Domäne verwiesen wird.

1. Binden Sie jetzt die VM mit dem Befehl `adcli join` in die durch Azure AD DS verwaltete Domäne ein. Geben Sie einen Benutzer an, der zur Gruppe *AAD DC-Administratoren* gehört. Bei Bedarf [fügen Sie ein Benutzerkonto zu einer Gruppe in Azure AD hinzu](../active-directory/fundamentals/active-directory-groups-members-azure-portal.md).

    Auch hier muss der Name der durch Azure AD DS verwalteten Domäne in Großbuchstaben eingegeben werden. Im folgenden Beispiel wird das Konto mit dem Namen `contosoadmin@contoso.com` zum Initialisieren von Kerberos verwendet. Geben Sie das eigene Benutzerkonto ein, das Mitglied der Gruppe *AAD DC-Administratoren* ist.

    ```console
    sudo adcli join -D CONTOSO.COM -U contosoadmin@CONTOSO.COM -K /etc/krb5.keytab -H coreos.contoso.com -N coreos
    ```

    Der Befehl `adcli join` gibt keine Informationen zurück, wenn die VM erfolgreich in die durch Azure AD DS verwaltete Domäne eingebunden wurde.

1. Starten Sie den SSSD-Dienst, um die Konfiguration für das Einbinden in die Domäne anzuwenden:
  
    ```console
    sudo systemctl start sssd.service
    ```

## <a name="sign-in-to-the-vm-using-a-domain-account"></a>Anmelden bei der VM mit einem Domänenkonto

Um zu überprüfen, ob die VM erfolgreich in die durch Azure AD DS verwaltete Domäne eingebunden wurde, starten Sie eine neue SSH-Verbindung mithilfe eines Domänenbenutzerkontos. Vergewissern Sie sich, dass ein Basisverzeichnis erstellt wurde und die Gruppenmitgliedschaft aus der Domäne angewendet wird.

1. Erstellen Sie eine neue SSH-Verbindung über die Konsole. Verwenden Sie ein Domänenkonto, das der verwalteten Domäne angehört, mithilfe des Befehls `ssh -l` (z.B. `contosoadmin@contoso.com`), und geben Sie dann die Adresse Ihrer VM ein (z.B. *coreos.contoso.com*). Bei Verwendung von Azure Cloud Shell verwenden Sie die öffentliche IP-Adresse der VM anstelle des internen DNS-Namens.

    ```console
    ssh -l contosoadmin@CONTOSO.com coreos.contoso.com
    ```

1. Überprüfen Sie nun, ob die Gruppenmitgliedschaften ordnungsgemäß aufgelöst werden:

    ```console
    id
    ```

    Es sollten Ihre Gruppenmitgliedschaften aus der durch Azure AD DS verwalteten Domäne angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

Wenn beim Herstellen einer Verbindung der VM mit der durch Azure AD DS verwalteten Domäne oder bei der Anmeldung mit einem Domänenkonto Probleme auftreten, finden Sie weitere Informationen unter [Behandeln von Problemen mit dem Einbinden in eine Domäne](join-windows-vm.md#troubleshoot-domain-join-issues).

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
