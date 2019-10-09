---
title: Kopieren oder Verschieben von Daten in Azure Storage mit AzCopy v10 | Microsoft-Dokumentation
description: AzCopy ist ein Befehlszeilenhilfsprogramm, das Sie verwenden können, um Daten in ein oder aus einem Speicherkonto zu kopieren. Dieser Artikel hilft Ihnen, AzCopy herunterzuladen, eine Verbindung mit Ihrem Speicherkonto herzustellen und dann Dateien zu übertragen.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: bb816658faff9fb924d075e0fca17e9643c18e40
ms.sourcegitcommit: 8bae7afb0011a98e82cbd76c50bc9f08be9ebe06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71694758"
---
# <a name="get-started-with-azcopy"></a>Erste Schritte mit AzCopy

AzCopy ist ein Befehlszeilenhilfsprogramm, das Sie verwenden können, um Blobs oder Dateien in oder aus einem Speicherkonto zu kopieren. Dieser Artikel hilft Ihnen, AzCopy herunterzuladen, eine Verbindung mit Ihrem Speicherkonto herzustellen und dann Dateien zu übertragen.

> [!NOTE]
> AzCopy **V10** ist die aktuell unterstützte Version von AzCopy.
>
> Wenn Sie AzCopy **v8.1** verwenden müssen, finden Sie Informationen hierzu im Abschnitt [Verwenden der vorherige Version von AzCopy](#previous-version) in diesem Artikel.

<a id="download-and-install-azcopy" />

## <a name="download-azcopy"></a>Herunterladen von AzCopy

Laden Sie zunächst die ausführbare Datei für AzCopy V10 in ein beliebiges Verzeichnis auf Ihrem Computer herunter. AzCopy V10 ist nur eine ausführbare Datei, sodass keine Installation erforderlich ist.

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (ZIP)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (TAR)
- [macOS](https://aka.ms/downloadazcopy-v10-mac) (ZIP)

Diese Dateien werden als ZIP- (Windows und Mac) oder TAR-Datei (Linux) komprimiert.

Mit diesen Befehlen können Sie die TAR-Datei unter Linux herunterladen und dekomprimieren.

```bash
wget -O azcopy.tar.gz https://aka.ms/downloadazcopy-v10-linux
tar -xf azcopy.tar.gz
```

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

### <a name="option-1-use-azure-active-directory"></a>Option 1: Verwenden von Azure Active Directory

Mithilfe von Azure Active Directory können Sie ein einziges Mal Anmeldeinformationen bereitstellen, statt jedem Befehl ein SAS-Token anfügen zu müssen.  

> [!NOTE]
> Wenn Sie im aktuellen Release Blobs zwischen Speicherkonten kopieren möchten, müssen Sie an jede Quell-URL ein SAS-Token anfügen. Nur in der Ziel-URL kann das SAS-Token weggelassen werden. Beispiele finden Sie unter [Kopieren von Blobs zwischen Speicherkonten](storage-use-azcopy-blobs.md).

Die Ebene der Autorisierung, die Sie benötigen, hängt davon ab, ob Sie planen, Dateien hoch- oder nur herunterzuladen.

Wenn Sie nur Dateien herunterladen möchten, überprüfen Sie, ob der [Leser von Speicherblobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader) Ihrer Benutzeridentität, der verwalteten Identität oder dem Dienstprinzipal zugewiesen wurde.

> Benutzeridentitäten, verwaltete Identitäten und Dienstprinzipale sind jeweils eine Form von *Sicherheitsprinzipalen*. Daher verwenden wir in diesem Artikel von jetzt an den Begriff *Sicherheitsprinzipal*.

Wenn Sie Dateien hochladen möchten, dann überprüfen Sie, ob Ihrem Sicherheitsprinzipal eine dieser Rollen zugewiesen wurde:

- [Mitwirkender an Speicherblobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Besitzer von Speicherblobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Diese Rollen können Ihrem Sicherheitsprinzipal in jedem dieser Bereiche zugewiesen werden:

- Container (Dateisystem)
- Speicherkonto
- Resource group
- Subscription

Informationen zum Überprüfen und Zuweisen von Rollen finden Sie unter [Gewähren von Zugriff auf Azure Blob- und -Warteschlangendaten mit RBAC im Azure-Portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Denken Sie daran, dass die Rollenzuweisung für die rollenbasierte Zugriffssteuerung bis zu fünf Minuten dauern kann.

Ihrem Sicherheitsprinzipal muss keine dieser Rollen zugewiesen sein, wenn Ihr Sicherheitsprinzipal der Zugriffssteuerungsliste (ACL) des Zielcontainers oder des Verzeichnisses hinzugefügt wurde. In der Zugriffssteuerungsliste benötigt Ihr Sicherheitsprinzipal Schreibberechtigungen für das Zielverzeichnis und Ausführungsberechtigungen für den Container und jedes übergeordnete Verzeichnis.

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

#### <a name="authenticate-a-service-principal"></a>Authentifizieren eines Dienstprinzipals

Dies ist eine gute Option, wenn Sie AzCopy in einem Skript verwenden möchten, das ohne Benutzerinteraktion ausgeführt wird, vor allem bei der lokalen Ausführung. Wenn Sie AzCopy auf virtuellen Computern ausführen möchten, die in Azure ausgeführt werden, lässt sich eine verwaltete Dienstidentität einfacher verwalten. Weitere Informationen finden Sie im Abschnitt [Authentifizieren einer verwalteten Identität](#managed-identity) in diesem Artikel.

Sie müssen sich interaktiv mindestens ein Mal anmelden, bevor Sie ein Skript ausführen, damit Sie in AzCopy die Anmeldeinformationen Ihres Dienstprinzipals bereitstellen können.  Diese Anmeldedaten werden in einer gesicherten, verschlüsseln Datei gespeichert, damit Ihr Skript diese vertraulichen Daten nicht weitergeben muss.

Sie können sich mit einem geheimen Clientschlüssel oder mit dem Kennwort eines Zertifikats, das für die App-Registrierung Ihres Dienstprinzipals verwendet wird, bei Ihrem Konto anmelden.

Weitere Informationen zum Erstellen eines Dienstprinzipals finden Sie unter [Vorgehensweise: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Weitere Informationen zu Dienstprinzipalen im Allgemeinen finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals).

##### <a name="using-a-client-secret"></a>Verwenden eines geheimen Clientschlüssels

Beginnen Sie, indem Sie die `AZCOPY_SPA_CLIENT_SECRET`-Umgebungsvarible auf den geheimen Clientschlüssel der App-Registrierung Ihres Dienstprinzipals festlegen.

> [!NOTE]
> Stellen Sie sicher, dass Sie diesen Wert in Ihrer Eingabeaufforderung einrichten und nicht in den Einstellungen der Umgebungsvariable Ihres Betriebssystems. So ist der Wert nur für die aktuelle Sitzung verfügbar.

Dieses Beispiel zeigt Ihnen, wie dies in PowerShell geht.

```azcopy
$env:AZCOPY_SPA_CLIENT_SECRET="$(Read-Host -prompt "Enter key")"
```

> [!NOTE]
> Erwägen Sie, eine Eingabeaufforderung wie in diesem Beispiel zu verwenden. So erscheint das Kennwort nicht im Befehlsverlauf Ihrer Konsole.  

Geben Sie dann den folgenden Befehl ein, und drücken Sie die EINGABETASTE.

```azcopy
azcopy login --service-principal --application-id <application-id> --tenant-id=<tenant-id>
```

Ersetzen Sie den Platzhalter `<application-id>` mit der Anwendungs-ID der App-Registrierung Ihres Dienstprinzipals. Ersetzen Sie den Platzhalter `<tenant-id>` mit der Mandanten-ID der Organisation, zu der das Speicherkonto gehört. Wählen Sie **Azure Active Directory > Eigenschaften > Verzeichnis-ID** im Azure-Portal aus, um die Mandanten-ID zu finden. 

##### <a name="using-a-certificate"></a>Verwenden eines Zertifikats

Wenn Sie lieber Ihre eigenen Anmeldedaten für die Autorisierung verwenden möchten, dann können Sie ein Zertifikat in Ihre App-Registrierung hochladen und dieses zum Anmelden verwenden.

Sie müssen nicht nur Ihr Zertifikat in Ihre App-Registrierung hochladen, sondern benötigen außerdem eine Kopie des Zertifikat, die auf dem Computer oder dem virtuellen Computer gespeichert ist, auf dem AzCopy ausgeführt wird. Diese Kopie des Zertifikats sollte das .PFX oder .PEM-Format haben und den privaten Schlüssel enthalten. Der private Schlüssel sollte mit einem Kennwort geschützt sein. Wenn Sie Windows verwenden und Ihre Zertifikat nur in einem Zertifikatspeicher ist, dann stellen Sie sicher, dass Sie das Zertifikat als PFX-Datei exportieren (einschließlich des privaten Schlüssels). Eine Anleitung dazu finden Sie unter [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate?view=win10-ps).

Richten Sie jetzt die `AZCOPY_SPA_CERT_PASSWORD`-Umgebungsvariable mit dem Zertifikatkennwort ein.

> [!NOTE]
> Stellen Sie sicher, dass Sie diesen Wert in Ihrer Eingabeaufforderung einrichten und nicht in den Einstellungen der Umgebungsvariable Ihres Betriebssystems. So ist der Wert nur für die aktuelle Sitzung verfügbar.

Dieses Beispiel zeigt Ihnen, wie diese Aufgabe in PowerShell ausgeführt wird.

```azcopy
$env:AZCOPY_SPA_CERT_PASSWORD="$(Read-Host -prompt "Enter key")"
```

Geben Sie dann den folgenden Befehl ein, und drücken Sie die EINGABETASTE.

```azcopy
azcopy login --service-principal --certificate-path <path-to-certificate-file> --tenant-id=<tenant-id>
```

Ersetzen Sie den Platzhalter `<path-to-certificate-file>` mit einem relativen oder vollqualifizierten Pfad zur Zertifikatdatei ein. AzCopy speichert den Pfad zu diesem Zertifikat, speichert jedoch keine Kopie des Zertifikats. Achten Sie darauf, dass das Zertifikat dort bleibt, wo es ist. Ersetzen Sie den Platzhalter `<tenant-id>` mit der Mandanten-ID der Organisation, zu der das Speicherkonto gehört. Wählen Sie **Azure Active Directory > Eigenschaften > Verzeichnis-ID** im Azure-Portal aus, um die Mandanten-ID zu finden.

> [!NOTE]
> Erwägen Sie, eine Eingabeaufforderung wie in diesem Beispiel zu verwenden. So erscheint das Kennwort nicht im Befehlsverlauf Ihrer Konsole. 

<a id="managed-identity" />

#### <a name="authenticate-a-managed-identity"></a>Authentifizieren einer verwalteten Identität

Dies ist eine gute Option, wenn Sie AzCopy in einem Skript verwenden möchten, das ohne Benutzerinteraktion und über einen virtuellen Azure-Computer ausgeführt wird. Bei Verwendung dieser Option müssen Sie keine Anmeldeinformationen auf dem virtuellen Computer speichern.

Sie können sich bei Ihrem Konto anmelden, indem Sie eine systemweite verwaltete Identität verwenden, die Sie auf dem virtuellen Computer aktiviert haben, oder indem Sie die Client-ID, Objekt-ID oder Ressourcen-ID einer vom Benutzer zugewiesenen verwalteten Identität verwenden, die Sie dem virtuellen Computer zugewiesen haben.

Weitere Informationen zum Aktivieren einer systemweiten verwalteten Identität oder zum Erstellen einer vom Benutzer zugewiesenen verwalteten Identität finden unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Computer über das Azure-Portal](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm).

##### <a name="using-a-system-wide-managed-identity"></a>Verwenden einer systemweiten verwalteten Identität

Stellen Sie zunächst sicher, dass Sie eine systemweite verwaltete Identität auf Ihrem virtuellen Computer aktiviert haben. Siehe [Systemseitig zugewiesene verwaltete Identität](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity).

Geben Sie dann an der Befehlskonsole den folgenden Befehl ein, und drücken Sie die EINGABETASTE.

```azcopy
azcopy login --identity
```

##### <a name="using-a-user-assigned-managed-identity"></a>Verwenden einer benutzerseitig zugewiesenen verwalteten Identität

Stellen Sie zunächst sicher, dass Sie eine vom Benutzer zugewiesene verwaltete Identität auf Ihrem virtuellen Computer aktiviert haben. Siehe [Benutzerseitig zugewiesene verwaltete Identität](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#user-assigned-managed-identity).

Geben Sie dann an der Befehlskonsole einen der folgenden Befehle ein, und drücken Sie die EINGABETASTE.

```azcopy
azcopy login --identity --identity-client-id "<client-id>"
```

Ersetzen Sie den Platzhalter `<client-id>` durch die Client-ID der benutzerseitig zugewiesenen verwalteten Identität.

```azcopy
azcopy login --identity --identity-object-id "<object-id>"
```

Ersetzen Sie den Platzhalter `<object-id>` durch die Objekt-ID der benutzerseitig zugewiesenen verwalteten Identität.

```azcopy
azcopy login --identity --identity-resource-id "<resource-id>"
```

Ersetzen Sie den Platzhalter `<resource-id>` durch die Ressourcen-ID der benutzerseitig zugewiesenen verwalteten Identität.

### <a name="option-2-use-a-sas-token"></a>Option 2: Verwenden eines SAS-Token

Sie können jeder Quell- oder Ziel-URL, die Sie in Ihren AzCopy-Befehlen verwenden, ein SAS-Token anfügen.

Dieser Beispielbefehl kopiert Daten rekursiv aus einem lokalen Verzeichnis in einen Blob-Container. Am Ende der Container-URL wird ein fiktives SAS-Token angefügt.

```azcopy
azcopy copy "C:\local\path" "https://account.blob.core.windows.net/mycontainer1/?sv=2018-03-28&ss=bjqt&srt=sco&sp=rwddgcup&se=2019-05-01T05:01:17Z&st=2019-04-30T21:01:17Z&spr=https&sig=MGCXiyEzbtttkr3ewJIh2AR8KrghSy1DGM9ovN734bQF4%3D" --recursive=true
```

Weitere Informationen zu SAS-Tokens und wie Sie eins erhalten, finden Sie unter [Verwenden von SAS (Shared Access Signatures)](https://docs.microsoft.com/azure/storage/common/storage-sas-overview).

## <a name="transfer-files"></a>Übertragen von Dateien

Nachdem Sie Ihre Identität authentifiziert oder ein SAS-Token abgerufen haben, können Sie mit der Übertragung von Dateien beginnen.

Beispielbefehle finden Sie in diesen Artikeln.

- [Übertragen von Daten mit AzCopy und Blob Storage](storage-use-azcopy-blobs.md)

- [Übertragen von Daten mit AzCopy und Dateispeicher](storage-use-azcopy-files.md)

- [Übertragen von Daten mit AzCopy und Amazon S3-Buckets](storage-use-azcopy-s3.md)

- [Übertragen von Daten mit AzCopy und Azure Stack-Speicher](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-transfer#azcopy)

## <a name="use-azcopy-in-a-script"></a>Verwenden von AzCopy in einem Skript

### <a name="obtain-a-static-download-link"></a>Abrufen eines statischen Downloadlinks

Im Laufe der Zeit wird der [Downloadlink](#download-and-install-azcopy) von AzCopy auf neue Versionen von AzCopy verweisen. Wenn Ihr Skript AzCopy herunterlädt, dann kann es sein, dass es nicht mehr funktioniert, wenn eine neuere Version von AzCopy Funktionen verändert, die für Ihr Skript wichtig sind.

Um solche Probleme zu vermeiden, sollten Sie sich einen statischen (unveränderlichen) Link zur aktuellen Version AzCopy besorgen. So lädt Ihr Skript jedes Mal genau dieselbe Version von AzCopy herunter, wenn es ausgeführt wird.

Mit diesem Befehl erhalten Sie den Link:

| Betriebssystem  | Get-Help |
|--------|-----------|
| **Linux** | `curl -v https://aka.ms/downloadazcopy-v10-linux` |
| **Windows** | `(curl https://aka.ms/downloadazcopy-v10-windows -MaximumRedirection 0 -ErrorAction silentlycontinue).RawContent` |

> [!NOTE]
> Bei Linux entfernt `--strip-components=1` im `tar`-Befehl den oberen Ordner, der den Versionsnamen enthält, und extrahiert stattdessen die Binärdatei direkt in den aktuellen Ordner. So kann das Skript mit einer neuen Version von `azcopy` aktualisiert werden, indem nur die `wget`-URL aktualisiert wird.

Die URL wird in der Ausgabe dieses Befehls angezeigt. Ihr Skript kann dann AzCopy anhand dieser URL herunterladen.

| Betriebssystem  | Get-Help |
|--------|-----------|
| **Linux** | `wget -O azcopyv10.tar https://azcopyvnext.azureedge.net/release20190301/azcopy_linux_amd64_10.0.8.tar.gz tar -xf azcopyv10.tar --strip-components=1 ./azcopy` |
| **Windows** | `Invoke-WebRequest https://azcopyvnext.azureedge.net/release20190517/azcopy_windows_amd64_10.1.2.zip -OutFile azcopyv10.zip <<Unzip here>>` |

### <a name="escape-special-characters-in-sas-tokens"></a>Verwenden von Escapezeichen für Sonderzeichen in SAS-Token

In Batchdateien mit der Erweiterung `.cmd` müssen Sie die `%`-Zeichen, die in SAS-Token vorkommen, mit Escapezeichen versehen. Sie können dazu ein zusätzliches `%`-Zeichen neben vorhandenen `%`-Zeichen in der SAS-Tokenzeichenfolge hinzufügen.

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
