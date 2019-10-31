---
title: Einbinden eines virtuellen Ubuntu-Computers in Azure AD Domain Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen virtuellen Ubuntu Linux-Computer in eine durch Azure AD Domain Services verwaltete Domäne einbinden.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 804438c4-51a1-497d-8ccc-5be775980203
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 09/15/2019
ms.author: iainfou
ms.openlocfilehash: 9fb41b08cb29a68b39fb416b4b7b7bcce9e821dd
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72754345"
---
# <a name="join-an-ubuntu-linux-virtual-machine-to-an-azure-ad-domain-services-managed-domain"></a>Einbinden eines virtuellen Ubuntu Linux-Computers in eine durch Azure AD Domain Services verwaltete Domäne

Damit Benutzer sich mit einem einzigen Satz von Anmeldeinformationen bei virtuellen Computern (VMs) in Azure anmelden können, binden Sie VMs in eine durch Azure Active Directory Domain Services (AD DS) verwaltete Domäne ein. Wenn Sie eine VM in eine durch Azure AD DS verwaltete Domäne einbinden, können Benutzerkonten und Anmeldeinformationen aus der Domäne zum Anmelden und Verwalten von Servern verwendet werden. Gruppenmitgliedschaften aus der durch Azure AD DS verwalteten Domäne werden ebenfalls angewendet, damit Sie den Zugriff auf Dateien oder Dienste auf der VM steuern können.

In diesem Artikel wird gezeigt, wie Sie eine Ubuntu Linux-VM in eine durch Azure AD DS verwaltete Domäne einbinden.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie die folgenden Ressourcen und Berechtigungen:

* Ein aktives Azure-Abonnement.
    * Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Einen mit Ihrem Abonnement verknüpften Azure Active Directory-Mandanten, der entweder mit einem lokalen Verzeichnis synchronisiert oder ein reines Cloudverzeichnis ist.
    * [Erstellen Sie einen Azure Active Directory-Mandanten][create-azure-ad-tenant], oder [verknüpfen Sie ein Azure-Abonnement mit Ihrem Konto][associate-azure-ad-tenant], sofern erforderlich.
* Eine verwaltete Azure Active Directory Domain Services-Domäne, die in Ihrem Azure AD-Mandanten aktiviert und konfiguriert ist.
    * Falls Sie keine solche Domäne haben, gehen Sie wie im ersten Tutorial beschrieben vor, um eine [Azure Active Directory Domain Services-Instanz zu erstellen und zu konfigurieren][create-azure-ad-ds-instance].
* Ein Benutzerkonto, das Mitglied der *Administratorengruppe für Azure AD-Domänencontroller* (AAD-DC-Administratoren) in Ihrem Azure AD-Mandanten ist.

## <a name="create-and-connect-to-an-ubuntu-linux-vm"></a>Erstellen einer Ubuntu Linux-VM und Herstellen einer Verbindung

Wenn Sie über eine vorhandene Ubuntu Linux-VM in Azure verfügen, stellen Sie über SSH eine Verbindung mit dieser VM her. Dann fahren Sie mit dem nächsten Schritt fort und beginnen mit der [Konfiguration der VM](#configure-the-hosts-file).

Wenn Sie eine Ubuntu Linux-VM erstellen müssen oder eine Test-VM zur Verwendung mit diesem Artikel erstellen möchten, können Sie eine der folgenden Methoden verwenden:

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
* *ubuntu* ist der Hostname Ihrer Ubuntu-VM, die Sie in die verwaltete Domäne einbinden.

Aktualisieren Sie diese Namen mit Ihren eigenen Werten:

```console
127.0.0.1 ubuntu.contoso.com ubuntu
```

Anschließend speichern und beenden Sie die Datei *hosts* mit dem Befehl `:wq` im Editor.

## <a name="install-required-packages"></a>Installieren erforderlicher Pakete

Die VM benötigt einige zusätzliche Pakete, damit sie in die durch Azure AD DS verwalteten Domäne eingebunden werden kann. Zum Installieren und Konfigurieren dieser Pakete aktualisieren und installieren Sie die Tools zum Einbinden in eine Domäne mit `apt-get`.

Während der Kerberos-Installation werden Sie vom Paket *krb5-user* aufgefordert, den Bereichsnamen in Großbuchstaben einzugeben. Wenn der Name Ihrer durch Azure AD DS verwalteten Domäne z.B. *contoso.com* lautet, geben Sie als Bereich *CONTOSO.COM* ein. Die Installation schreibt die Abschnitte `[realm]` und `[domain_realm]` in die Konfigurationsdatei */etc/krb5.conf*. Stellen Sie sicher, dass Sie den Bereich in Großbuchstaben angeben:

```console
sudo apt-get update
sudo apt-get install krb5-user samba sssd sssd-tools libnss-sss libpam-sss ntp ntpdate realmd adcli
```

## <a name="configure-network-time-protocol-ntp"></a>Konfigurieren von NTP (Network Time Protocol)

Damit die Domänenkommunikation ordnungsgemäß funktioniert, müssen Datum und Uhrzeit Ihrer Ubuntu-VM mit der durch Azure AD DS verwalteten Domäne synchronisiert werden. Fügen Sie den NTP-Hostnamen Ihrer durch Azure AD DS verwalteten Domäne der Datei */etc/ntp.conf* hinzu.

1. Öffnen Sie die Datei *ntp.conf* mit einem Editor:

    ```console
    sudo vi /etc/ntp.conf
    ```

1. Erstellen Sie in der Datei *ntp.conf* eine Zeile, um den DNS-Namen Ihrer durch Azure AD DS verwalteten Domäne hinzuzufügen. Im folgenden Beispiel wird ein Eintrag für *contoso.com* hinzugefügt. Verwenden Sie den eigenen DNS-Namen:

    ```console
    server contoso.com
    ```

    Anschließend speichern und beenden Sie die Datei *ntp.conf* mit dem Befehl `:wq` im Editor.

1. Um sicherzustellen, dass die VM mit der durch Azure AD DS verwalteten Domäne synchronisiert wird, sind die folgenden Schritte erforderlich:

    * Stoppen des NTP-Servers
    * Aktualisieren von Datum und Uhrzeit aus der verwalteten Domäne
    * Starten des NTP-Diensts

    Führen Sie für diese Schritte die folgenden Befehle aus. Verwenden Sie beim Befehl `ntpdate` Ihren eigenen DNS-Namen:

    ```console
    sudo systemctl stop ntp
    sudo ntpdate contoso.com
    sudo systemctl start ntp
    ```

## <a name="join-vm-to-the-managed-domain"></a>Einbinden der VM in die verwaltete Domäne

Nachdem Sie die erforderlichen Pakete auf der VM installiert und NTP konfiguriert haben, binden Sie die VM in die durch Azure AD DS verwaltete Domäne ein.

1. Verwenden Sie den Befehl `realm discover`, um die durch Azure AD DS verwaltete Domäne zu ermitteln. Im folgenden Beispiel wird der Bereich *CONTOSO.COM* erkannt. Geben Sie den Namen Ihrer eigenen durch Azure AD DS verwalteten Domäne in Großbuchstaben an:

    ```console
    sudo realm discover CONTOSO.COM
    ```

   Wenn mit dem Befehl `realm discover` Ihre durch Azure AD DS verwaltete Domäne nicht gefunden werden kann, führen Sie die folgenden Schritte zur Problembehandlung aus:

    * Vergewissern Sie sich, dass die VM die Domäne erreichen kann. Versuchen Sie `ping contoso.com`, um zu überprüfen, ob eine positive Antwort zurückgegeben wird.
    * Überprüfen Sie, ob die VM in demselben oder einem mittels Peering verbundenen virtuellen Netzwerk bereitgestellt wurde, in dem die durch Azure AD DS verwaltete Domäne verfügbar ist.
    * Stellen Sie sicher, dass die DNS-Servereinstellungen für das virtuelle Netzwerk so aktualisiert wurden, dass auf die Domänencontroller der durch Azure AD DS verwalteten Domäne verwiesen wird.

1. Initialisieren Sie nun Kerberos mit dem Befehl `kinit`. Geben Sie einen Benutzer an, der zur Gruppe *AAD DC-Administratoren* gehört. Bei Bedarf [fügen Sie ein Benutzerkonto zu einer Gruppe in Azure AD hinzu](../active-directory/fundamentals/active-directory-groups-members-azure-portal.md).

    Auch hier muss der Name der durch Azure AD DS verwalteten Domäne in Großbuchstaben eingegeben werden. Im folgenden Beispiel wird das Konto mit dem Namen `contosoadmin@contoso.com` zum Initialisieren von Kerberos verwendet. Geben Sie das eigene Benutzerkonto ein, das Mitglied der Gruppe *AAD DC-Administratoren* ist:

    ```console
    kinit contosoadmin@CONTOSO.COM
    ```

1. Zum Schluss binden Sie den Computer mit dem Befehl `realm join` in die durch Azure AD DS verwaltete Domäne ein. Verwenden Sie dasselbe Benutzerkonto, das Mitglied der Gruppe *AAD DC-Administratoren* ist und im vorherigen Befehl `kinit` angegeben wurde, z.B. `contosoadmin@CONTOSO.COM`:

    ```console
    sudo realm join --verbose CONTOSO.COM -U 'contosoadmin@CONTOSO.COM' --install=/
    ```

Das Einbinden der VM in die durch Azure AD DS verwaltete Domäne dauert einen Moment. Die folgende Beispielausgabe zeigt, dass die VM erfolgreich in die durch Azure AD DS verwaltete Domäne eingebunden wurde:

```output
Successfully enrolled machine in realm
```

Wenn für die VM der Prozess der Einbindung in die Domäne nicht erfolgreich abgeschlossen werden kann, stellen Sie sicher, dass die Netzwerksicherheitsgruppe der VM ausgehenden Kerberos-Datenverkehr über TCP und UDP-Port 464 an das Subnetz des virtuellen Netzwerks für Ihre durch Azure AD DS verwaltete Domäne zulässt.

## <a name="update-the-sssd-configuration"></a>Aktualisieren der SSSD-Konfiguration

Eines der in einem vorherigen Schritt installierten Pakete war das SSSD-Paket (System Security Services Daemon). Wenn ein Benutzer versucht, sich mit Domänenanmeldeinformationen bei einer VM anzumelden, leitet SSSD die Anforderung an einen Authentifizierungsanbieter weiter. In diesem Szenario wird Azure AD DS von SSSD zum Authentifizieren der Anforderung verwendet.

1. Öffnen Sie die Datei *sssd_conf* mit einem Editor:

    ```console
    sudo vi /etc/sssd/sssd.conf
    ```

1. Kommentieren Sie die Zeile für *use_fully_qualified_names* folgendermaßen aus:

    ```console
    # use_fully_qualified_names = True
    ```

    Anschließend speichern und beenden Sie die Datei *sssd_conf* mit dem Befehl `:wq` im Editor.

1. Starten Sie den SSSD-Dienst neu, um die Änderung zu übernehmen:

    ```console
    sudo service sssd restart
    ```

## <a name="configure-user-account-and-group-settings"></a>Konfigurieren von Benutzerkonto- und Gruppeneinstellungen

Nachdem die VM in die durch Azure AD DS verwaltete Domäne eingebunden und für die Authentifizierung konfiguriert wurde, müssen einige Benutzerkonfigurationen durchgeführt werden. Diese Konfigurationsänderungen umfassen das Zulassen der kennwortbasierten Authentifizierung und das automatische Erstellen von Basisverzeichnissen auf der lokalen VM bei der ersten Anmeldung von Domänenbenutzern.

### <a name="allow-password-authentication-for-ssh"></a>Zulassen der Kennwortauthentifizierung für SSH

Standardmäßig können sich Benutzer nur mithilfe der auf einen öffentlichen Schlüssel basierenden SSH-Authentifizierung bei einer VM anmelden. Die kennwortbasierte Authentifizierung schlägt fehl. Wenn Sie die VM in eine durch Azure AD DS verwalteten Domäne einbinden, müssen diese Domänenkonten die kennwortbasierte Authentifizierung verwenden. Aktualisieren Sie die SSH-Konfiguration wie folgt, um die kennwortbasierte Authentifizierung zuzulassen.

1. Öffnen Sie die Datei *sshd_conf* mit einem Editor:

    ```console
    sudo vi /etc/ssh/sshd_config
    ```

1. Aktualisieren Sie die Zeile für *PasswordAuthentication* in *yes*:

    ```console
    PasswordAuthentication yes
    ```

    Anschließend speichern und beenden Sie die Datei *sshd_conf* mit dem Befehl `:wq` im Editor.

1. Starten Sie den SSH-Dienst neu, um die Änderungen zu übernehmen und Benutzern das Anmelden mit einem Kennwort zu ermöglichen:

    ```console
    sudo systemctl restart ssh
    ```

### <a name="configure-automatic-home-directory-creation"></a>Konfigurieren der automatischen Erstellung des Basisverzeichnisses

Führen Sie die folgenden Schritte aus, um die automatische Erstellung des Basisverzeichnisses bei der ersten Anmeldung eines Benutzers zu aktivieren:

1. Öffnen Sie die Datei */etc/pam.d/common-session* in einem Editor:

    ```console
    sudo vi /etc/pam.d/common-session
    ```

1. Fügen Sie die folgende Zeile unterhalb der Zeile `session optional pam_sss.so` in diese Datei ein:

    ```console
    session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
    ```

    Anschließend speichern und beenden Sie die Datei *common-session* mit dem Befehl `:wq` im Editor.

### <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>Erteilen von sudo-Berechtigungen für die Gruppe „AAD DC-Administratoren“

Um Mitgliedern der Gruppe *AAD DC-Administratoren* Administratorrechte für die Ubuntu-VM zu erteilen, fügen Sie */etc/sudoers* einen Eintrag hinzu. Nach dem Hinzufügen können Mitglieder der Gruppe *AAD DC-Administratoren* den Befehl `sudo` auf der Ubuntu-VM verwenden.

1. Öffnen Sie die Datei *sudoers* zur Bearbeitung:

    ```console
    sudo visudo
    ```

1. Fügen Sie den folgenden Eintrag am Ende der Datei */etc/sudoers* hinzu:

    ```console
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators ALL=(ALL) NOPASSWD:ALL
    ```

    Anschließend speichern und beenden Sie den Editor mit dem Befehl `Ctrl-X`.

## <a name="sign-in-to-the-vm-using-a-domain-account"></a>Anmelden bei der VM mit einem Domänenkonto

Um zu überprüfen, ob die VM erfolgreich in die durch Azure AD DS verwaltete Domäne eingebunden wurde, starten Sie eine neue SSH-Verbindung mithilfe eines Domänenbenutzerkontos. Vergewissern Sie sich, dass ein Basisverzeichnis erstellt wurde und die Gruppenmitgliedschaft aus der Domäne angewendet wird.

1. Erstellen Sie eine neue SSH-Verbindung über die Konsole. Verwenden Sie ein Domänenkonto, das der verwalteten Domäne angehört, mithilfe des Befehls `ssh -l` (z.B. `contosoadmin@contoso.com`), und geben Sie dann die Adresse Ihrer VM ein (z.B. *ubuntu.contoso.com*). Bei Verwendung von Azure Cloud Shell verwenden Sie die öffentliche IP-Adresse der VM anstelle des internen DNS-Namens.

    ```console
    ssh -l contosoadmin@CONTOSO.com ubuntu.contoso.com
    ```

1. Wenn Sie erfolgreich eine Verbindung mit der VM hergestellt haben, vergewissern Sie sich, dass das Basisverzeichnis ordnungsgemäß initialisiert wurde:

    ```console
    pwd
    ```

    Sie sollten sich mit Ihrem eigenen Verzeichnis, das dem Benutzerkonto entspricht, im Verzeichnis */home* befinden.

1. Überprüfen Sie nun, ob die Gruppenmitgliedschaften ordnungsgemäß aufgelöst werden:

    ```console
    id
    ```

    Es sollten Ihre Gruppenmitgliedschaften aus der durch Azure AD DS verwalteten Domäne angezeigt werden.

1. Wenn Sie sich als Mitglied der Gruppe *AAD DC-Administratoren* bei der VM angemeldet haben, überprüfen Sie, ob Sie den Befehl `sudo` ordnungsgemäß verwenden können:

    ```console
    sudo apt-get update
    ```

## <a name="next-steps"></a>Nächste Schritte

Wenn beim Herstellen einer Verbindung der VM mit der durch Azure AD DS verwalteten Domäne oder bei der Anmeldung mit einem Domänenkonto Probleme auftreten, finden Sie weitere Informationen unter [Behandeln von Problemen mit dem Einbinden in eine Domäne](join-windows-vm.md#troubleshoot-domain-join-issues).

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
