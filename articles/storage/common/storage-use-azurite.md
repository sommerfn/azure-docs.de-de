---
title: Verwenden des Azurite-Open-Source-Emulators für die Entwicklung und das Testen mit Azure Storage (Vorschauversion)
description: Der Azurite-Open-Source-Emulator (Vorschauversion) bietet eine kostenlose lokale Umgebung zum Testen Ihrer Azure Storage-Anwendungen.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/31/2019
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.openlocfilehash: e611afd6f10154636eb2e0dd08437b4f7468d6b3
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309534"
---
# <a name="use-the-azurite-open-source-emulator-for-azure-storage-development-and-testing-preview"></a>Verwenden des Azurite-Open-Source-Emulators für die Entwicklung und das Testen mit Azure Storage (Vorschauversion)

Der Azurite-Open-Source-Emulator, Version 3.2, bietet eine kostenlose lokale Umgebung zum Testen Ihrer Azure-Blob- und -Warteschlangen-Speicheranwendungen. Wenn Sie mit der Funktion Ihrer Anwendung auf lokaler Ebene zufrieden sind, können Sie zur Verwendung eines Azure Storage-Kontos in der Cloud übergehen. Der Emulator bietet plattformübergreifenden Support für Windows, Linux und macOS. Azurite v3 unterstützt APIs, die vom Azure-Blob-Dienst implementiert wurden.

Azurite ist die Speicheremulatorplattform der Zukunft. Azurite ersetzt den [Azure-Speicheremulator](storage-use-emulator.md). Azurite wird weiterhin aktualisiert, um die neuesten Versionen der Azure Storage-APIs zu unterstützen.

Es gibt verschiedene Möglichkeiten, Azurite auf Ihrem lokalen System zu installieren und auszuführen:

  1. [Installieren und Ausführen der Azurite-Erweiterung für Visual Studio Code](#install-and-run-the-azurite-visual-studio-code-extension)
  1. [Installieren und Ausführen von Azurite mit NPM](#install-and-run-azurite-by-using-npm)
  1. [Installieren und Ausführen des Azurite-Docker-Images](#install-and-run-the-azurite-docker-image)
  1. [Klonen, Erstellen und Ausführen von Azurite aus dem GitHub-Repository](#clone-build-and-run-azurite-from-the-github-repository)

## <a name="install-and-run-the-azurite-visual-studio-code-extension"></a>Installieren und Ausführen der Azurite-Erweiterung für Visual Studio Code

Wählen Sie in Visual Studio Code den Bereich **EXTENSIONS** aus, und suchen Sie dann unter **EXTENSIONS:MARKETPLACE** nach *Azurite*.

![Visual Studio Code-Marketplace für Erweiterungen](media/storage-use-azurite/azurite-vs-code-extension.png)

Alternativ können Sie auch in Ihrem Browser den [VS Code-Marketplace für Erweiterungen](https://marketplace.visualstudio.com/items?itemName=Azurite.azurite) aufrufen. Wählen Sie die Schaltfläche **Installieren** aus, um Visual Studio Code zu öffnen, und wechseln Sie dann direkt zur Azurite-Erweiterungsseite.

Sie können Azurite schnell starten oder schließen, indem Sie auf der VS Code-Statusleiste auf **Azurite Blob Service** (Azurite Blob-Dienst) oder **Azurite Queue Service** (Azurite-Warteschlangendienst) klicken oder die folgenden Befehle in der VS Code-Befehlspalette ausführen. Um die Befehlspalette zu öffnen, drücken Sie in VS Code **F1**.

Die Erweiterung unterstützt die folgenden Visual Studio Code-Befehle:

   * **Azurite: Starten** – alle Azurite-Dienste starten
   * **Azurite: Schließen** – alle Azurite-Dienste schließen
   * **Azurite: Bereinigen** – dauerhafte Daten aller Azurite-Dienste zurücksetzen
   * **Azurite: Blob-Dienst starten** – Blob-Dienst starten
   * **Azurite: Blob-Dienst schließen** – Blob-Dienst schließen
   * **Azurite: Blob-Dienst bereinigen** – Blob-Dienst bereinigen
   * **Azurite: Warteschlangendienst starten** – Warteschlangendienst starten
   * **Azurite: Warteschlangendienst schließen** – Warteschlangendienst schließen
   * **Azurite: Warteschlangendienst bereinigen** – Warteschlangendienst bereinigen

Um Azurite in Visual Studio Code zu konfigurieren, wählen Sie den Erweiterungsbereich aus. Wählen Sie das Symbol **Verwalten** (Zahnrad) für **Azurite** aus. Wählen Sie **Configure Extension Settings** (Erweiterungseinstellungen konfigurieren) aus.

![Konfigurieren der Erweiterungseinstellungen für Azurite](media/storage-use-azurite/azurite-configure-extension-settings.png)

Die folgenden Einstellungen werden unterstützt:

   * **Azurite: Blobhost** – der Lauschendpunkt des Blob-Diensts. Die Standardeinstellung ist 127.0.0.1.
   * **Azurite: Blobport** – der Lauschport des Blob-Diensts. Der Standardport ist 10000.
   * **Azurite: Debuggen** – Ausgabe des Debugprotokolls an den Azurite-Kanal. Der Standardwert ist **false**.
   * **Azurite: Speicherort** – der Speicherpfad des Arbeitsbereichs. Der Standard ist der Arbeitsordner für Visual Studio Code.
   * **Azurite: Warteschlangenhost** – der Lauschendpunkt des Warteschlangendiensts. Die Standardeinstellung ist 127.0.0.1.
   * **Azurite: Warteschlangenport** – der Lauschport des Warteschlangendiensts. Der Standardport ist 10001.
   * **Azurite: Lautlos** – Zugriffsprotokoll im Lautlosmodus deaktivieren. Der Standardwert ist **false**.

## <a name="install-and-run-azurite-by-using-npm"></a>Installieren und Ausführen von Azurite mit NPM

Diese Installationsmethode setzt voraus, dass Sie [mindestens Version 8.0 von Node.js](https://nodejs.org) installiert haben. **npm** ist das Paketverwaltungstool, das bei jeder Installation von Node.js enthalten ist. Führen Sie nach der Installation von Node.js den folgenden **npm**-Befehl aus, um Azurite zu installieren.

```console
npm install -g azurite
```

Nachdem Sie Azurite installiert haben, finden Sie weitere Informationen unter [Ausführen von Azurite über die Befehlszeile](#run-azurite-from-a-command-line).

## <a name="install-and-run-the-azurite-docker-image"></a>Installieren und Ausführen des Azurite-Docker-Images

Verwenden Sie [DockerHub](https://hub.docker.com/), um das [neueste Azurite-Image](https://hub.docker.com/_/microsoft-azure-storage-azurite) mit dem folgenden Befehl zu laden:

```console
docker pull mcr.microsoft.com/azure-storage/azurite
```

**Ausführen des Azurite-Docker-Images:**

Der folgende Befehl führt das Azurite-Docker-Image aus. Der Parameter `-p 10000:10000` leitet Anforderungen von Port 10000 des Hostcomputers an die Docker-Instanz weiter.

```console
docker run -p 10000:10000 -p 10001:10001 mcr.microsoft.com/azure-storage/azurite
```

**Angeben des Speicherorts für den Arbeitsbereich:**

Im folgenden Beispiel ist im Parameter `-v c:/azurite:/data` der persistente Datenspeicherort *c:/azurite* für Azurite angegeben. Vor dem Ausführen des Docker-Befehls muss das Verzeichnis *c:/azurite* erstellt werden.

```console
docker run -p 10000:10000 -p 10001:10001 -v c:/azurite:/data mcr.microsoft.com/azure-storage/azurite
```

**Ausführen nur des Blob-Diensts**

```console
docker run -p 10000:10000 mcr.microsoft.com/azure-storage/azurite
    azurite-blob --blobHost 0.0.0.0 --blobPort 10000
```

**Festlegen aller Azurite-Parameter:**

Dieses Beispiel zeigt, wie Sie alle Befehlszeilenparameter festlegen. Alle folgenden Parameter sollten in einer einzigen Befehlszeile platziert werden.

```console
docker run -p 8888:8888
           -p 9999:9999
           -v c:/azurite:/workspace mcr.microsoft.com/azure-storage/azurite azurite
           -l /workspace
           -d /workspace/debug.log
           --blobPort 8888
           --blobHost 0.0.0.0
           --queuePort 9999
           --queueHost 0.0.0.0
```

Weitere Informationen zum Konfigurieren von Azurite beim Start finden Sie unter [Befehlszeilenoptionen](#command-line-options).

## <a name="clone-build-and-run-azurite-from-the-github-repository"></a>Klonen, Erstellen und Ausführen von Azurite aus dem GitHub-Repository

Diese Installationsmethode setzt voraus, dass Sie [Git](https://git-scm.com/) installiert haben. Klonen Sie das [GitHub-Repository](https://github.com/azure/azurite) für das Azurite-Projekt mit dem folgenden Konsolenbefehl.

```console
git clone https://github.com/Azure/Azurite.git
```

Nachdem Sie den Quellcode geklont haben, führen Sie folgende Befehle im Stammverzeichnis des geklonten Repositorys aus, um Azurite zu erstellen und zu installieren.

```console
npm install
npm run build
npm install -g
```

Nachdem Sie Azurite installiert und erstellt haben, finden Sie weitere Informationen unter [Ausführen von Azurite über die Befehlszeile](#run-azurite-from-a-command-line).

## <a name="run-azurite-from-a-command-line"></a>Ausführen von Azurite über die Befehlszeile

> [!NOTE]
> Azurite kann nicht über die Befehlszeile ausgeführt werden, wenn Sie nur die Visual Studio Code-Erweiterung installiert haben. Verwenden Sie stattdessen die VS Code-Befehlspalette. Weitere Informationen finden Sie unter [Installieren und Ausführen der Azurite-Erweiterung für Visual Studio Code](#install-and-run-the-azurite-visual-studio-code-extension).

Um sofort mit der Befehlszeile zu beginnen, erstellen Sie das Verzeichnis **c:\azurite**, und starten Sie dann Azurite mit dem folgenden Befehl:

```console
azurite --silent --location c:\azurite --debug c:\azurite\debug.log
```

Dieser Befehl weist Azurite an, alle Daten im Verzeichnis **c:\azurite** zu speichern. Wenn die Option **--location** weggelassen wird, wird das aktuelle Arbeitsverzeichnis verwendet.

## <a name="command-line-options"></a>Befehlszeilenoptionen

Dieser Abschnitt beschreibt die Befehlszeilenoptionen, die zum Starten von Azurite verfügbar sind. Alle Befehlszeilenoptionen sind optional.

```console
C:\Azurite> azurite [--blobHost <IP address>] [--blobPort <port address>] 
    [-d | --debug <log file path>] [-l | --location <workspace path>]
    [--queueHost <IP address>] [--queuePort <port address>]
    [-s | --silent] [-h | --help]
```

**-d** ist eine Abkürzung für **--debug**, **-l** ist eine Abkürzung für **--location**, **-s** ist eine Abkürzung für **--silent** und **-h** ist eine Abkürzung für **--help**.

### <a name="blob-listening-host"></a>Bloblauschhost

**Optional:** Standardmäßig lauscht Azurite an 127.0.0.1 als lokalem Server. Verwenden Sie die Option **--blobHost**, um die Adresse entsprechend Ihren Anforderungen festzulegen.

Nur Anforderungen auf dem lokalen Computer annehmen:

```console
azurite --blobHost 127.0.0.1
```

Remoteanforderungen annehmen:

```console
azurite --blobHost 0.0.0.0
```

> [!CAUTION]
> Wenn Sie Remoteanforderungen zulassen, wird Ihr System eventuell anfälliger für externe Bedrohungen.

### <a name="blob-listening-port-configuration"></a>Konfiguration des Bloblauschports

**Optional:** Standardmäßig lauscht Azurite an Port 10000 auf den Blob-Dienst. Verwenden Sie die Option **--blobPort**, um den gewünschten Lauschport anzugeben.

> [!NOTE]
> Nachdem Sie einen benutzerdefinierten Port verwendet haben, müssen Sie die Verbindungszeichenfolge oder die entsprechende Konfiguration in Ihren Azure Storage-Tools oder -SDKs aktualisieren.

Lauschport für den Blob-Dienst anpassen:

```console
azurite --blobPort 8888
```

Automatische Auswahl eines verfügbaren Ports durch das System:

```console
azurite --blobPort 0
```

Der verwendete Port wird beim Start von Azurite angezeigt.

### <a name="queue-listening-host"></a>Warteschlangen-Lauschhost

**Optional:** Standardmäßig lauscht Azurite an 127.0.0.1 als lokalem Server. Verwenden Sie die Option **--queueHost**, um die Adresse entsprechend Ihren Anforderungen festzulegen.

Nur Anforderungen auf dem lokalen Computer annehmen:

```console
azurite --queueHost 127.0.0.1
```

Remoteanforderungen annehmen:

```console
azurite --queueHost 0.0.0.0
```

> [!CAUTION]
> Wenn Sie Remoteanforderungen zulassen, wird Ihr System eventuell anfälliger für externe Bedrohungen.

### <a name="queue-listening-port-configuration"></a>Konfiguration des Warteschlangen-Lauschports

**Optional:** Standardmäßig lauscht Azurite an Port 10001 auf den Warteschlangendienst. Verwenden Sie die Option **--queuePort**, um den gewünschten Lauschport anzugeben.

> [!NOTE]
> Nachdem Sie einen benutzerdefinierten Port verwendet haben, müssen Sie die Verbindungszeichenfolge oder die entsprechende Konfiguration in Ihren Azure Storage-Tools oder -SDKs aktualisieren.

Lauschport für den Warteschlangendienst anpassen:

```console
azurite --queuePort 8888
```

Automatische Auswahl eines verfügbaren Ports durch das System:

```console
azurite --queuePort 0
```

Der verwendete Port wird beim Start von Azurite angezeigt.

### <a name="workspace-path"></a>Arbeitsbereichspfad

**Optional:** Azurite speichert während der Ausführung Daten auf dem lokalen Datenträger. Verwenden Sie die Option **--location**, um einen Pfad als Speicherort für den Arbeitsbereich anzugeben. Standardmäßig wird das Arbeitsverzeichnis des aktuellen Prozesses verwendet.

```console
azurite --location c:\azurite
```

```console
azurite -l c:\azurite
```

### <a name="access-log"></a>Zugriffsprotokoll

**Optional:** Standardmäßig wird das Zugriffsprotokoll im Konsolenfenster angezeigt. Deaktivieren Sie die Anzeige des Zugriffsprotokolls mit der Option **--silent**.

```console
azurite --silent
```

```console
azurite -s
```

### <a name="debug-log"></a>Debugprotokoll

**Optional:** Das Debugprotokoll enthält ausführliche Informationen zu jeder Anforderung und jeder Ausnahmestapelüberwachung. Aktivieren Sie das Debugprotokoll, indem Sie für die Option **--debug** einen gültigen lokalen Dateipfad angeben.

```console
azurite --debug path/debug.log
```

```console
azurite -d path/debug.log
```

## <a name="authorization-for-tools-and-sdks"></a>Autorisierung für Tools und SDKs

Stellen Sie über Azure Storage SDKs oder Tools wie [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) mithilfe einer beliebigen Authentifizierungsstrategie eine Verbindung mit Azurite her. Eine Authentifizierung ist erforderlich. Azurite unterstützt die Autorisierung mit gemeinsamem Schlüssel und Shared Access Signatures (SAS). Außerdem unterstützt Azurite anonymen Zugriff auf öffentliche Container.

### <a name="well-known-storage-account-and-key"></a>Bekannte Speicherkonten und Schlüssel

Sie können den folgenden Kontonamen und Schlüssel mit Azurite verwenden. Dies ist dasselbe bekannte Konto und der Schlüssel, die auch vom älteren Azure-Speicheremulator verwendet werden.

* Kontoname: `devstoreaccount1`
* Kontoschlüssel: `Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==`

> [!NOTE]
> Zusätzlich zur SharedKey-Authentifizierung unterstützt Azurite auch die SAS-Authentifizierung mit Konten und Diensten. Außerdem ist anonymer Zugriff möglich, wenn für einen Container öffentlicher Zugriff zugelassen wird.

### <a name="connection-string"></a>Verbindungszeichenfolge

Am einfachsten können Sie über Ihre Anwendung eine Verbindung mit Azurite herstellen, indem Sie in der Konfigurationsdatei der Anwendung eine Verbindungszeichenfolge konfigurieren, die auf die Verknüpfung *UseDevelopmentStorage=true* verweist. Eine Verbindungszeichenfolge in der Datei *app.config* kann beispielsweise wie folgt aussehen:

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

Weitere Informationen hierzu finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](storage-configure-connection-string.md).

### <a name="storage-explorer"></a>Storage-Explorer

Stellen Sie in Azure Storage-Explorer eine Verbindung mit Azurite her, indem Sie auf das Symbol **Konto hinzufügen** klicken, dann **Attach to a local emulator** (An lokalen Emulator anfügen) auswählen und auf **Verbinden** klicken.

## <a name="differences-between-azurite-and-azure-storage"></a>Unterschiede zwischen Azurite und Azure Storage

Es gibt funktionale Unterschiede zwischen einer lokalen Azurite-Instanz und einem Azure Storage-Konto in der Cloud.

### <a name="endpoint-and-connection-url"></a>Endpunkt und Verbindungs-URL

Die Dienstendpunkte für Azurite unterscheiden sich von denen eines Azure Storage-Kontos. Der lokale Computer führt keine Auflösung von Domänennamen durch, sodass Azurite-Endpunkte lokale Adressen sein müssen.

Wenn Sie eine Ressource in einem Azure Storage-Konto ansprechen, ist der Kontoname Teil des URI-Hostnamens. Die angesprochene Ressource ist Teil des URI-Pfades:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

Der folgende URI ist eine gültige Adresse für ein Blob in einem Azure Storage-Konto:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

Da der lokale Computer keine Domänennamen auflöst, ist der Kontoname Teil des URI-Pfads und nicht des Hostnamens. Verwenden Sie für Ressourcen in Azurite das folgende URI-Format:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Die folgende Adresse kann beispielsweise für den Zugriff auf ein Blob in Azurite verwendet werden:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

### <a name="scaling-and-performance"></a>Skalierung und Leistung

Azurite ist kein skalierbarer Speicherdienst und unterstützt keine große Anzahl gleichzeitiger Clients. Es gibt keine Leistungsgarantie. Azurite ist für Entwicklungs- und Testzwecke ausgelegt.

### <a name="error-handling"></a>Fehlerbehandlung

Azurite ist mit der Fehlerbehandlungslogik von Azure Storage abgestimmt, es gibt jedoch Unterschiede. Beispielsweise können Fehlermeldungen bei übereinstimmenden Fehlerstatuscodes unterschiedlich ausfallen.

### <a name="ra-grs"></a>RA-GRS

Azurite unterstützt die georedundante Replikation mit Lesezugriff (Read-Access Geo-Redundant Replication, RA-GRS). Greifen Sie für Speicherressourcen auf den sekundären Speicherort zu, indem Sie **-secondary** an den Kontonamen anfügen. Beispielsweise kann die folgende Adresse mithilfe des schreibgeschützten sekundären Speicherorts in Azurite für den Zugriff auf ein Blob verwendet werden:

`http://127.0.0.1:10000/devstoreaccount1-secondary/mycontainer/myblob.txt`

## <a name="azurite-is-open-source"></a>Azurite ist Open Source.

Wir freuen uns über Beiträge und Vorschläge für Azurite. Auf der Seite des [GitHub-Projekts](https://github.com/Azure/Azurite/projects) und der [GitHub-Problemseite](https://github.com/Azure/Azurite/issues) für Azurite finden Sie Meilensteine und Arbeitselemente, die wir für zukünftige Features und Fehlerbehebungen berücksichtigen. Außerdem werden in GitHub detaillierte Arbeitselemente nachverfolgt.

## <a name="next-steps"></a>Nächste Schritte

* Unter [Verwenden des Azure-Speicheremulators für Entwicklung und Tests](storage-use-emulator.md) wird der veraltete Azure-Speicheremulator beschrieben, der durch Azurite ersetzt wird.
* Unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](storage-configure-connection-string.md) finden Sie eine Erläuterung des Aufbaus einer gültigen Azure Storage-Verbindungszeichenfolge.
