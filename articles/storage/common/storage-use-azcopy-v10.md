---
title: Kopieren oder Verschieben von Daten in Azure Storage mit AzCopy v10 | Microsoft-Dokumentation
description: AzCopy ist ein Befehlszeilenhilfsprogramm, das Sie verwenden können, um Daten in ein oder aus einem Speicherkonto zu kopieren. Dieser Artikel hilft Ihnen, AzCopy herunterzuladen, eine Verbindung mit Ihrem Speicherkonto herzustellen und dann Dateien zu übertragen.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 94aca33b2f12c1c39297221a856296dcca052b0f
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565801"
---
# <a name="get-started-with-azcopy"></a>Erste Schritte mit AzCopy

AzCopy ist ein Befehlszeilenhilfsprogramm, das Sie verwenden können, um Blobs oder Dateien in oder aus einem Speicherkonto zu kopieren. Dieser Artikel hilft Ihnen, AzCopy herunterzuladen, eine Verbindung mit Ihrem Speicherkonto herzustellen und dann Dateien zu übertragen.

> [!NOTE]
> AzCopy **V10** ist die aktuell unterstützte Version von AzCopy.
>
> Wenn Sie AzCopy **v8.1** verwenden müssen, finden Sie Informationen hierzu im Abschnitt [Verwenden der vorherige Version von AzCopy](#previous-version) in diesem Artikel.

<a id="download-and-install-azcopy" />

## <a name="download-azcopy"></a>Herunterladen von AzCopy

Laden Sie zunächst die ausführbare Datei für AzCopy V10 in ein beliebiges Verzeichnis auf Ihrem Computer herunter. 

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (ZIP)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (TAR)
- [macOS](https://aka.ms/downloadazcopy-v10-mac) (ZIP)

> [!NOTE]
> Wenn Sie Daten in und aus Ihrem [Azure Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) kopieren möchten, installieren Sie [AzCopy, Version 7.3](https://aka.ms/downloadazcopynet).

## <a name="run-azcopy"></a>Ausführen von AzCopy

Erwägen Sie aus Gründen der Bedienfreundlichkeit, den Speicherort des Verzeichnisses mit der ausführbaren AzCopy-Datei der Einfachheit halber dem Systempfad hinzuzufügen. Auf diese Weise können Sie in jedem beliebigen Verzeichnis auf Ihrem System `azcopy` eingeben.

Falls Sie das AzCopy-Verzeichnis nicht zum Pfad hinzufügen, müssen Sie in das Verzeichnis der ausführbaren AzCopy-Datei wechseln und in Windows PowerShell-Eingabeaufforderungen `azcopy` oder `.\azcopy` eingeben.

Um eine Liste der AzCopy-Befehle anzuzeigen, geben Sie `azcopy -h` ein, und drücken Sie dann die EINGABETASTE.

Wenn Sie weitere Informationen zu einem bestimmten Befehl erhalten möchten, fügen Sie einfach den Namen des Befehls ein (etwa `azcopy list -h`).

![Inlinehilfe](media/storage-use-azcopy-v10/azcopy-inline-help.png)

> [!NOTE] 
> Als Besitzer Ihres Azure Storage-Kontos erhalten Sie nicht automatisch Berechtigungen für den Zugriff auf Daten. Bevor Sie etwas Sinnvolles mit AzCopy machen können, müssen Sie entscheiden, wie Sie dem Speicherdienst die Autorisierungsanmeldeinformationen bereitstellen werden. 

## <a name="choose-how-youll-provide-authorization-credentials"></a>Auswählen, wie Sie die Autorisierungsanmeldeinformationen bereitstellen

Sie können die Autorisierungsanmeldeinformationen bereitstellen, indem Sie Azure Active Directory (AD) oder ein SAS-Token (Shared Access Signature) verwenden.

Verwenden Sie diese Tabelle als Richtlinie:

| Speichertyp | Derzeit unterstützte Methode der Autorisierung |
|--|--|
|**Blob Storage** | Azure AD und SAS |
|**Blob-Speicher (hierarchischer Namespace)** | Azure AD und SAS |
|**File Storage** | Nur SAS |

### <a name="option-1-use-azure-ad"></a>Option 1: Verwenden von Azure AD

Mithilfe von Azure AD können Sie ein einziges Mal Anmeldeinformationen bereitstellen, statt jedem Befehl einen SAS-Token anfügen zu müssen.  

Die Ebene der Autorisierung, die Sie benötigen, hängt davon ab, ob Sie planen, Dateien hoch- oder nur herunterzuladen.

Wenn Sie nur Dateien herunterladen wollen, dann überprüfen Sie einfach, ob der [Leser von Speicherblobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader) Ihrer Benutzeridentität oder Ihrem Dienstprinzipal zugewiesen wurde. 

> [!NOTE]
> Benutzeridentitäten und Dienstprinzipale sind jeweils eine Art von *Sicherheitsprinzipalen*. Deswegen verwenden wir in diesem Artikel von jetzt an den Begriff *„Sicherheitsprinzipal“* .

Wenn Sie Dateien hochladen möchten, dann überprüfen Sie, ob Ihrem Sicherheitsprinzipal eine dieser Rollen zugewiesen wurde:

- [Mitwirkender an Speicherblobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Besitzer von Speicherblobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Diese Rollen können Ihrer Identität in jedem dieser Bereiche zugewiesen werden:

- Container (Dateisystem)
- Speicherkonto
- Resource group
- Subscription

Informationen zum Überprüfen und Zuweisen von Rollen finden Sie unter [Gewähren von Zugriff auf Azure Blob- und -Warteschlangendaten mit RBAC im Azure-Portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE] 
> Denken Sie daran, dass die Rollenzuweisung für die rollenbasierte Zugriffssteuerung bis zu fünf Minuten dauern kann.

Ihrem Sicherheitsprinzipal muss keine dieser Rollen zugewiesen sein, wenn Ihr Sicherheitsprinzipal der Zugriffssteuerungsliste (ACL) des Zielcontainers oder des Verzeichnisses hinzugefügt wurde. In ACL benötigt Ihr Sicherheitsprinzipal Schreibberechtigungen für das Zielverzeichnis und Ausführungsberechtigungen für den Container und jedes übergeordnete Verzeichnis.

Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

#### <a name="authenticate-a-user-identity"></a>Authentifizieren einer Benutzeridentität

Nachdem Sie überprüft haben, ob Ihrer Benutzeridentität die notwendige Berechtigungsstufe gewährt wurde, öffnen Sie eine Eingabeaufforderung, geben Sie den folgenden Befehl ein, und drücken Sie dann die EINGABETASTE.

```azcopy
azcopy login
```

Wenn Sie mehr als einer Organisation angehören, dann sollten Sie die Mandanten-ID der Organisation angeben, zu der das Speicherkonto gehört.

```azcopy
azcopy login --tenant-id=<tenant-id>
```

Ersetzen Sie den Platzhalter `<tenant-id>` mit der Mandanten-ID der Organisation, zu der das Speicherkonto gehört. Wählen Sie **Azure Active Directory > Eigenschaften > Verzeichnis-ID** im Azure-Portal aus, um die Mandanten-ID zu finden.

Dieser Befehl gibt einen Authentifizierungscode und die URL einer Website zurück. Öffnen Sie die Website, geben Sie den Code ein, und wählen Sie dann die Schaltfläche **Weiter** aus.

![Erstellen eines Containers](media/storage-use-azcopy-v10/azcopy-login.png)

Daraufhin wird ein Anmeldefenster geöffnet. Melden Sie sich in diesem Fenster mit Ihren Azure-Kontoanmeldeinformationen bei Ihrem Azure-Konto an. Wenn Sie sich erfolgreich angemeldet haben, können Sie das Browserfenster schließen und mit der Verwendung von AzCopy beginnen.

<a id="service-principal" />

#### <a name="authenticate-a-service-principal"></a>Dienstprinzipal authentifizieren

Das ist eine gute Option, wenn Sie AzCopy in einem Skript verwenden wollen, das ohne Benutzerinteraktion ausgeführt wird. 

Sie müssen sich interaktiv mindestens ein Mal anmelden, bevor Sie das Skript ausführen, damit Sie AzCopy die Anmeldedaten Ihres Dienstprinzipals bereitstellen können.  Diese Anmeldedaten werden in einer gesicherten, verschlüsseln Datei gespeichert, damit Ihr Skript diese vertraulichen Daten nicht weitergeben muss.

Sie können sich mit dem geheimen Clientschlüssel in Ihrem Konto anmelden oder mit dem Kennwort eines Zertifikats, das für die Appregistrierung Ihres Dienstprinzipals verwendet wird. 

Weitere Informationen zum Erstellen eines Dienstprinzipals finden Sie unter [Vorgehensweise: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Weitere Informationen zu Dienstprinzipalen im Allgemeinen finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals).

##### <a name="using-a-client-secret"></a>Verwenden eines geheimen Clientschlüssels

Beginnen Sie, indem Sie die `AZCOPY_SPA_CLIENT_SECRET`-Umgebungsvarible auf den geheimen Clientschlüssel der Appregistrierung Ihres Dienstprinzipals festlegen. 

> [!NOTE]
> Stellen Sie sicher, dass Sie diesen Wert in Ihrer Eingabeaufforderung einrichten und nicht in den Einstellungen der Umgebungsvariable Ihres Betriebssystems. So ist der Wert nur für die aktuelle Sitzung verfügbar.

Dieses Beispiel zeigt Ihnen, wie dies in PowerShell geht.

```azcopy
$env:AZCOPY_SPA_CLIENT_SECRET="$(Read-Host -prompt "Enter key")"
```

> [!NOTE]
> Erwägen Sie, eine Eingabeaufforderung wie in diesem Beispiel zu verwenden. So erscheint der geheime Clientschlüssel nicht im Befehlsverlauf Ihrer Konsole. 

Geben Sie dann den folgenden Befehl ein, und drücken Sie die EINGABETASTE.

```azcopy
azcopy login --service-principal --application-id <application-id>
```

Ersetzen Sie den Platzhalter `<application-id>` mit der Anwendungs-ID der Appregistrierung Ihres Dienstprinzipals.

##### <a name="using-a-certificate"></a>Verwenden eines Zertifikats

Wenn Sie lieber Ihre eigenen Anmeldedaten für die Autorisierung verwenden möchten, dann können Sie ein Zertifikat in Ihre Appregistrierung hochladen und dieses zum Anmelden verwenden.

Sie müssen nicht nur Ihr Zertifikat in Ihre Appregistrierung hochladen, sondern benötigen außerdem eine Kopie des Zertifikat, die auf dem Computer oder dem virtuellen Computer gespeichert ist, auf dem AzCopy ausgeführt wird. Diese Kopie des Zertifikats sollte das .PFX oder .PEM-Format haben und den privaten Schlüssel enthalten. Der private Schlüssel sollte mit einem Kennwort geschützt sein. Wenn Sie Windows verwenden und Ihre Zertifikat nur in einem Zertifikatspeicher ist, dann stellen Sie sicher, dass Sie das Zertifikat als PFX-Datei exportieren (einschließlich des privaten Schlüssels). Eine Anleitung dazu finden Sie unter [„Export-PfxCertificate“](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate?view=win10-ps)

Richten Sie jetzt die `AZCOPY_SPA_CERT_PASSWORD`-Umgebungsvariable mit dem Zertifikatkennwort ein.

> [!NOTE]
> Stellen Sie sicher, dass Sie diesen Wert in Ihrer Eingabeaufforderung einrichten und nicht in den Einstellungen der Umgebungsvariable Ihres Betriebssystems. So ist der Wert nur für die aktuelle Sitzung verfügbar.

Dieses Beispiel zeigt Ihnen, wie dies in PowerShell geht.

```azcopy
$env:AZCOPY_SPA_CERT_PASSWORD="$(Read-Host -prompt "Enter key")"
```

Geben Sie dann den folgenden Befehl ein, und drücken Sie die EINGABETASTE.

```azcopy
azcopy login --service-principal --certificate-path <path-to-certificate-file>
```

Ersetzen Sie den Platzhalter `<path-to-certificate-file>` mit einem relativen oder vollqualifizierten Pfad zur Zertifikatdatei ein. AzCopy speichert den Pfad zu diesem Zertifikat, speichert jedoch keine Kopie des Zertifikats. Achten Sie darauf, dass das Zertifikat dort bleibt, wo es ist.

> [!NOTE]
> Erwägen Sie, eine Eingabeaufforderung wie in diesem Beispiel zu verwenden. So erscheint das Kennwort nicht im Befehlsverlauf Ihrer Konsole. 

### <a name="option-2-use-a-sas-token"></a>Option 2: Verwenden eines SAS-Token

Sie können jeder Quell- oder Ziel-URL, die Sie in Ihren AzCopy-Befehlen verwenden, ein SAS-Token anfügen.

Dieser Beispielbefehl kopiert Daten rekursiv aus einem lokalen Verzeichnis in einen Blob-Container. Am Ende der Container-URL wird ein fiktives SAS-Token angefügt.

```azcopy
azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1/?sv=2018-03-28&ss=bjqt&srt=sco&sp=rwddgcup&se=2019-05-01T05:01:17Z&st=2019-04-30T21:01:17Z&spr=https&sig=MGCXiyEzbtttkr3ewJIh2AR8KrghSy1DGM9ovN734bQF4%3D" --recursive=true
```

Weitere Informationen zu SAS-Tokens und wie Sie eins erhalten, finden Sie unter [Verwenden von SAS (Shared Access Signatures)](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

## <a name="transfer-files"></a>Übertragen von Dateien

Nachdem Sie Ihre Identität authentifiziert oder ein SAS-Token abgerufen haben, können Sie mit der Übertragung von Dateien beginnen.

Beispielbefehle finden Sie in diesen Artikeln.

- [Übertragen von Daten mit AzCopy und Blob Storage](storage-use-azcopy-blobs.md)

- [Übertragen von Daten mit AzCopy und Dateispeicher](storage-use-azcopy-files.md)

- [Übertragen von Daten mit AzCopy und Amazon S3-Buckets](storage-use-azcopy-s3.md)

- [Übertragen von Daten mit AzCopy und Azure Stack-Speicher](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-transfer#azcopy)

## <a name="use-azcopy-in-a-script"></a>Verwenden von AzCopy in einem Skript

Sie müssen sich interaktiv mindestens ein Mal anmelden, bevor Sie das Skript ausführen, damit Sie AzCopy die Anmeldedaten Ihres Dienstprinzipals bereitstellen können.  Diese Anmeldedaten werden in einer gesicherten, verschlüsseln Datei gespeichert, damit Ihr Skript diese vertraulichen Daten nicht weitergeben muss. Beispiele finden Sie im Abschnitt [„Authentifizierung Ihres Dienstprinzipals“](#service-principal) in diesem Artikel.

Im Laufe der Zeit wird der [Downloadlink](#download-and-install-azcopy) von AzCopy auf neue Versionen von AzCopy verweisen. Wenn Ihr Skript AzCopy herunterlädt, dann kann es sein, dass es nicht mehr funktioniert, wenn eine neuere Version von AzCopy Funktionen verändert, die für Ihr Skript wichtig sind. 

Um solche Probleme zu vermeiden, sollten Sie sich einen statischen (unveränderlichen) Link zur aktuellen Version AzCopy besorgen. So lädt Ihr Skript jedes Mal genau dieselbe Version von AzCopy herunter, wenn es ausgeführt wird.

Mit diesem Befehl erhalten Sie den Link:

| Betriebssystem  | Get-Help |
|--------|-----------|
| **Linux** | `curl -v https://aka.ms/downloadazcopy-v10-linux` |
| **Windows** | `(curl https://aka.ms/downloadazcopy-v10-windows -MaximumRedirection 0 -ErrorAction silentlycontinue).RawContent` |

> [!NOTE]
> Bei Linux entfernt `--strip-components=1` im `tar`-Befehl den oberen Ordner, der den Versionsnamen enthält, und extrahiert stattdessen die Binärdatei direkt in den aktuellen Ordner. So kann das Skript mit einer neuen Version von `azcopy` aktualisiert werden, indem nur die `wget`-URL aktualisiert wird.

Die URL erscheint in der Ausgabe dieses Befehls. Ihr Skript kann dann AzCopy anhand dieser URL herunterladen.

| Betriebssystem  | Get-Help |
|--------|-----------|
| **Linux** | `wget -O azcopyv10.tar https://azcopyvnext.azureedge.net/release20190301/azcopy_linux_amd64_10.0.8.tar.gz tar -xf azcopyv10.tar --strip-components=1 ./azcopy` |
| **Windows** | `Invoke-WebRequest https://azcopyvnext.azureedge.net/release20190517/azcopy_windows_amd64_10.1.2.zip -OutFile azcopyv10.zip <<Unzip here>>` |

## <a name="use-azcopy-in-storage-explorer"></a>Verwenden von AzCopy im Storage-Explorer

Wenn Sie die Leistungsvorteile von AzCopy nutzen möchten, aber lieber Storage-Explorer statt der Befehlszeile verwenden, um mit Ihren Dateien zu interagieren, aktivieren Sie AzCopy in Storage-Explorer. 

Wählen Sie in Storage-Explorer **Preview**->**Use AzCopy for Improved Blob Upload and Download** (AzCopy für verbessertes Hoch- und Herunterladen von Blobs verwenden) aus.

![Aktivieren von AzCopy als Übertragungsmodul in Azure Storage-Explorer](media/storage-use-azcopy-v10/enable-azcopy-storage-explorer.jpg)

> [!NOTE]
> Sie müssen diese Einstellung nicht aktivieren, wenn Sie einen hierarchischen Namespace in Ihrem Speicherkonto aktiviert haben. Der Grund hierfür ist, dass Storage-Explorer AzCopy automatisch in Speicherkonten verwendet, die einen hierarchischen Namespace haben.  

Storage-Explorer verwendet Ihren Kontoschlüssel, um Vorgänge auszuführen. Nachdem Sie sich also bei Storage-Explorer anmelden, müssen Sie keine weiteren Autorisierungsdaten angeben.

<a id="previous-version" />

## <a name="use-the-previous-version-of-azcopy"></a>Verwenden der vorherigen Version von AzCopy

Wenn Sie die vorherige Version von AzCopy (AzCopy v8.1) verwenden müssen, werden Sie unter den folgenden Links fündig:

- [AzCopy unter Windows (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy)

- [AzCopy unter Linux (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux)

## <a name="configure-optimize-and-troubleshoot-azcopy"></a>Konfigurieren, Optimieren und Problembehandlung in AzCopy

Siehe [Konfigurieren, Optimieren und Problembehandlung in AzCopy](storage-use-azcopy-configure.md).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Fragen haben, Probleme melden oder allgemeines Feedback bereitstellen möchten, können Sie dies [auf der GitHub-Seite](https://github.com/Azure/azure-storage-azcopy) tun.
