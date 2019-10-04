---
title: Verwenden von Azure Files mit Linux | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie unter Linux eine Azure-Dateifreigabe über SMB einbinden.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 03/29/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 9674df346ff0b5c046df26fe85bf7ba0bbb802e6
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69900308"
---
# <a name="use-azure-files-with-linux"></a>Verwenden von Azure Files mit Linux

[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können mithilfe des [SMB-Kernelclients](https://wiki.samba.org/index.php/LinuxCIFS) in Linux-Distributionen eingebunden werden. Dieser Artikel veranschaulicht zwei Möglichkeiten zum Einbinden einer Azure-Dateifreigabe: bedarfsgesteuert mit dem Befehl `mount` oder beim Start durch Erstellen eines Eintrags in `/etc/fstab`.

> [!NOTE]  
> Wenn Sie eine Azure-Dateifreigabe außerhalb der Azure-Region einbinden möchten, in der sie gehostet wird (beispielsweise lokal oder in einer anderen Azure-Region), muss das Betriebssystem die Verschlüsselungsfunktionen von SMB 3.0 unterstützen.

## <a name="prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Voraussetzungen für das Einbinden einer Azure-Dateifreigabe in Linux und das Paket „cifs-utils“
<a id="smb-client-reqs"></a>

* **Ein vorhandenes Azure Storage-Konto und eine Dateifreigabe**: Sie benötigen ein Speicherkonto und eine Dateifreigabe, um die Schritte in diesem Artikel ausführen zu können. Falls Sie noch keine erstellt haben, finden Sie Anweisungen in unseren Schnellstarts zum Thema: [Erstellen von Dateifreigaben: CLI](storage-how-to-use-files-cli.md).

* **Name und Schlüssel Ihres Speicherkontos** Sie benötigen den Namen und Schlüssel Ihres Speicherkontos, um die Schritte in diesem Artikel ausführen zu können. Falls Sie einen mithilfe des CLI-Schnellstarts erstellt haben, sollten Sie ihn bereits haben. Andernfalls sollten Sie den zuvor verlinkten CLI-Schnellstart konsultieren, um zu erfahren, wie Sie Ihren Speicherkontoschlüssel abrufen können.

* **Wählen Sie eine Linux-Distribution gemäß Ihren Anforderungen an die Einbindung aus.**  
      Azure Files kann entweder über SMB 2.1 oder über SMB 3.0 eingebunden werden. Bei Verbindungen von lokalen Clients oder von Clients in anderen Azure-Regionen müssen Sie SMB 3.0 verwenden; Azure Files lehnt SMB 2.1 (oder SMB 3.0 ohne Verschlüsselung) ab. Wenn Sie von einem virtuellen Computer in derselben Azure-Region aus auf die Azure-Dateifreigabe zugreifen, können Sie vielleicht mit SMB 2.1 auf Ihre Dateifreigabe zugreifen, wenn und nur dann, wenn *Sichere Übertragung erforderlich* für das Speicherkonto deaktiviert ist, das die Azure-Dateifreigabe hostet. Es sollte immer die sichere Datenübertragung erforderlich sein und nur SMB 3.0 mit Verschlüsselung verwendet werden dürfen.

    Die Unterstützung der SMB 3.0-Verschlüsselung wurde in der Linux-Kernelversion 4.11 eingeführt und nachträglich für ältere Kernelversionen gängiger Linux-Distributionen portiert. Zum Zeitpunkt der Veröffentlichung dieses Dokuments unterstützen die folgenden Distributionen aus dem Azure-Katalog die Einbindungsoption, die in den Tabellenüberschriften angegeben ist. 

### <a name="minimum-recommended-versions-with-corresponding-mount-capabilities-smb-version-21-vs-smb-version-30"></a>Empfohlene Mindestversionen mit entsprechenden Einbindungsmöglichkeiten (SMB-Versionen 2.1 und 3.0 im Vergleich)

|   | SMB 2.1 <br>(Einbindungen auf VMs innerhalb derselben Azure-Region) | SMB 3.0 <br>(Einbindungen aus einer lokalen Region und regionsübergreifend) |
| --- | :---: | :---: |
| Ubuntu Server | 14.04+ | 16.04 und höher |
| RHEL | 7 und höher | 7.5 und höher |
| CentOS | 7 und höher |  7.5 und höher |
| Debian | 8 und höher | 9+<sup>*</sup> |
| openSUSE | 13.2 und höher | 42.3+ |
| SUSE Linux Enterprise Server | 12 | 12 SP3 und höher |

<sup>*</sup>Möglicherweise muss ein zurückportierter Kernel verwendet werden. Diese können Sie mit `sudo apt-get install linux-image-cloud-amd64` installieren.

Sollte Ihre Linux-Distribution hier nicht aufgeführt sein, können Sie die Linux-Kernelversion mit dem folgenden Befehl ermitteln:

```bash
uname -r
```

* <a id="install-cifs-utils"></a>**Das Paket „cifs-utils“ ist installiert.**  
    Das Paket „cifs-utils“ kann mithilfe des Paket-Managers für die Linux-Distribution Ihrer Wahl installiert werden. 

    Verwenden Sie bei auf **Ubuntu** und **Debian** basierenden Distributionen den Paket-Manager `apt-get`:

    ```bash
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    Verwenden Sie unter **RHEL** und **CentOS**den Paket-Manager `yum`:

    ```bash
    sudo yum install cifs-utils
    ```

    Verwenden Sie unter **OpenSUSE** den Paket-Manager `zypper`:

    ```bash
    sudo zypper install cifs-utils
    ```

    Verwenden Sie bei anderen Distributionen den entsprechenden Paket-Manager, oder [kompilieren Sie den Quellcode](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* **Festlegen der Verzeichnis-/Dateiberechtigungen der eingebundenen Freigabe:** In den folgenden Beispielen wird die Berechtigung `0777` verwendet, um allen Benutzern die Berechtigungen „Lesen“, „Schreiben“ und „Ausführen“ zu erteilen. Sie können sie bei Bedarf durch andere [chmod-Berechtigungen](https://en.wikipedia.org/wiki/Chmod) ersetzen. Dadurch wird aber möglicherweise der Zugriff eingeschränkt. Wenn Sie andere Berechtigungen verwenden, sollten Sie auch die Verwendung von uid und gid in Betracht ziehen, um den Zugriff für lokale Benutzer und Gruppen Ihrer Wahl beizubehalten.

> [!NOTE]
> Wenn Sie nicht explizit Verzeichnis- und Dateiberechtigungen mit „dir_mode“ und „file_mode“ zuweisen, wird standardmäßig „0755“ verwendet.

* **Stellen Sie sicher, Port 445 geöffnet ist**: SMB kommuniziert über den TCP-Port 445. Vergewissern Sie sich, dass der TCP-Port 445 des Clientcomputers nicht durch die Firewall blockiert wird.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Bedarfsgesteuertes Einbinden der Azure-Dateifreigabe mit `mount`

1. **[Installieren Sie das Paket „cifs-utils“ für Ihre Linux-Distribution](#install-cifs-utils)** .

1. **Erstellen Sie einen Ordner für den Bereitstellungspunkt**: Ein Ordner für einen Bereitstellungspunkt kann zwar an einem beliebigen Ort im Dateisystem erstellt werden, aber normalerweise erfolgt die Erstellung in einem neuen Ordner. Mit dem folgenden Befehl wird beispielsweise ein neues Verzeichnis erstellt. Ersetzen Sie **<storage_account_name>** und **<file_share_name>** durch die entsprechenden Informationen für Ihre Umgebung:

    ```bash
    mkdir -p <storage_account_name>/<file_share_name>
    ```

1. **Verwenden Sie den Einbindungsbefehl, um die Azure-Dateifreigabe einzubinden**: Denken Sie daran, die Platzhalter **<storage_account_name>** , **<share_name>** , **<smb_version>** , **<storage_account_key>** und **<mount_point>** durch die entsprechenden Informationen für Ihre Umgebung zu ersetzen. Wenn Ihre Linux-Distribution SMB 3.0 mit Verschlüsselung unterstützt (siehe [Grundlegendes zu SMB-Clientanforderungen](#smb-client-reqs)), sollten Sie für **<smb_version>** den Wert **3.0** verwenden. Bei Linux-Distributionen ohne Unterstützung von SMB 3.0 mit Verschlüsselung muss für **<smb_version>** der Wert **2.1** verwendet werden. Eine Azure-Dateifreigabe kann nur außerhalb einer Azure-Region mit SMB 3.0 eingebunden werden (gilt auch in der lokalen Umgebung sowie in einer anderen Azure-Region). Wenn Sie möchten, können Sie die Verzeichnis- und Dateiberechtigungen der eingebundenen Freigabe ändern, dies könnte aber den Zugriff einschränken.

    ```bash
    sudo mount -t cifs //<storage_account_name>.file.core.windows.net/<share_name> <mount_point> -o vers=<smb_version>,username=<storage_account_name>,password=<storage_account_key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Wenn Sie die Azure-Dateifreigabe nicht mehr benötigen, können Sie `sudo umount <mount_point>` verwenden, um die Einbindung der Freigabe aufzuheben.

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Erstellen eines permanenten Bereitstellungspunkts für die Azure-Dateifreigabe mit `/etc/fstab`

1. **[Installieren Sie das Paket „cifs-utils“ für Ihre Linux-Distribution](#install-cifs-utils)** .

1. **Erstellen Sie einen Ordner für den Bereitstellungspunkt**: Ein Ordner für einen Bereitstellungspunkt kann zwar an einem beliebigen Ort im Dateisystem erstellt werden, aber normalerweise erfolgt die Erstellung in einem neuen Ordner. Beachten Sie unabhängig vom Ort der Erstellung den absoluten Pfad des Ordners. Mit dem folgenden Befehl wird beispielsweise ein neues Verzeichnis erstellt. Ersetzen Sie **<storage_account_name>** und **<file_share_name>** durch die entsprechenden Informationen für Ihre Umgebung.

    ```bash
    sudo mkdir -p <storage_account_name>/<file_share_name>
    ```

1. **Erstellen Sie eine Datei mit Anmeldeinformationen, um den Benutzernamen (den Namen des Speicherkontos) und das Kennwort (den Schlüssel des Speicherkontos) für die Dateifreigabe zu speichern.** Ersetzen Sie **<storage_account_name>** und **<storage_account_key>** durch die entsprechenden Informationen für Ihre Umgebung.

    ```bash
    if [ ! -d "/etc/smbcredentials" ]; then
    sudo mkdir /etc/smbcredentials
    fi
    if [ ! -f "/etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred" ]; then
    sudo bash -c 'echo "username=<STORAGE ACCOUNT NAME>" >> /etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred'
    sudo bash -c 'echo "password=7wRbLU5ea4mgc<DRIVE LETTER>PIpUCNcuG9gk2W4S2tv7p0cTm62wXTK<DRIVE LETTER>CgJlBJPKYc4VMnwhyQd<DRIVE LETTER>UT<DRIVE LETTER>yR5/RtEHyT/EHtg2Q==" >> /etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred'
    fi
    ```

1. **Ändern Sie die Berechtigungen in der Datei mit den Anmeldeinformationen so, dass nur „root“ die Kennwortdatei lesen oder ändern kann.** Da der Speicherkontoschlüssel eigentlich ein Superadministratorkennwort für das Speicherkonto ist, ist es wichtig, die Berechtigungen für die Datei so festzulegen, dass nur „root“ darauf zugreifen kann, damit Benutzer mit geringeren Rechten den Speicherkontoschlüssel nicht abrufen können.   

    ```bash
    sudo chmod 600 /etc/smbcredentials/<storage_account_name>.cred
    ```

1. **Fügen Sie mit dem folgenden Befehl die folgende Zeile an `/etc/fstab` an**: Denken Sie daran, die Platzhalter **<storage_account_name>** , **<share_name>** , **<smb_version>** und **<mount_point>** durch die entsprechenden Informationen für Ihre Umgebung zu ersetzen. Wenn Ihre Linux-Distribution SMB 3.0 mit Verschlüsselung unterstützt (siehe [Grundlegendes zu SMB-Clientanforderungen](#smb-client-reqs)), sollten Sie für **<smb_version>** den Wert **3.0** verwenden. Bei Linux-Distributionen ohne Unterstützung von SMB 3.0 mit Verschlüsselung muss für **<smb_version>** der Wert **2.1** verwendet werden. Eine Azure-Dateifreigabe kann nur außerhalb einer Azure-Region mit SMB 3.0 eingebunden werden (gilt auch in der lokalen Umgebung sowie in einer anderen Azure-Region).

    ```bash
    sudo bash -c 'echo "//<STORAGE ACCOUNT NAME>.file.core.windows.net/<FILE SHARE NAME> /mount/<STORAGE ACCOUNT NAME>/<FILE SHARE NAME> cifs _netdev,nofail,vers=3.0,credentials=/etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'

    sudo mount /mount/<STORAGE ACCOUNT NAME>/<FILE SHARE NAME>
    ```

> [!Note]  
> Mithilfe von `sudo mount -a` können Sie die Azure-Dateifreigabe nach der Bearbeitung von `/etc/fstab` einbinden, anstatt einen Neustart durchzuführen.

## <a name="feedback"></a>Feedback

Linux-Benutzer, wir möchten von Ihnen hören!

Die Benutzergruppe „Azure Files for Linux“ bietet ein Forum, in dem Sie Ihr Feedback zu File Storage für Linux geben können. Senden Sie eine E-Mail an [Azure Files Linux Users](mailto:azurefileslinuxusers@microsoft.com), um der Benutzergruppe beizutreten.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Files finden Sie unter diesen Links:

* [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)
* [Häufig gestellte Fragen](../storage-files-faq.md)
* [Problembehandlung](storage-troubleshoot-linux-file-connection-problems.md)
