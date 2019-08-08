---
title: Verwendung des Azurite Open-Source-Emulators für die Entwicklung und das Testen von Blob-Speichern (Vorschauversion)
description: Der Azurite Open-Source-Emulator (Vorschauversion) bietet eine kostenlose lokale Umgebung zum Testen Ihrer Azure Blob-Speicheranwendungen.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 06/12/2019
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.openlocfilehash: ebecd6cf9af5395e4da2b395ca9b2ff974a75409
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68721698"
---
# <a name="use-the-azurite-open-source-emulator-for-blob-storage-development-and-testing-preview"></a>Verwendung des Azurite Open-Source-Emulators für die Entwicklung und das Testen von Blob-Speichern (Vorschauversion)

Der Azurite Open-Source-Emulator Version 3 bietet eine kostenlose lokale Umgebung zum Testen Ihrer Azure Blob-Speicheranwendungen. Wenn Sie mit der Funktion der Anwendung im Emulator auf lokaler Ebene zufrieden sind, können Sie zur Verwendung eines Azure-Speicherkontos in der Cloud wechseln. Der Emulator bietet plattformübergreifenden Support für Windows, Linux und MacOS. Azurite v3 unterstützt APIs, die vom Azure Blobdienst implementiert wurden.

Azurite ist die Speicheremulator-Plattform der Zukunft. Azurite ersetzt den[ Azure-Speicheremulator](storage-use-emulator.md). Azurite wird weiterhin aktualisiert, um die neuesten Versionen der Azure Storage-APIs zu unterstützen.

Es gibt verschiedene Möglichkeiten, Azurite auf Ihrem lokalen System zu installieren und auszuführen:

  1. [Installieren und Ausführen der Azurite-Erweiterung für Visual Studio Code](#install-and-run-the-azurite-visual-studio-code-extension)
  1. [Installieren und Ausführen von Azurite mit NPM](#install-and-run-azurite-by-using-npm)
  1. [Installieren und Ausführen vom Azurite Docker-Image](#install-and-run-the-azurite-docker-image)
  1. [Klonen, Erstellen und Ausführen von Azurite aus dem GitHub-Repository](#clone-build-and-run-azurite-from-the-github-repository)

## <a name="install-and-run-the-azurite-visual-studio-code-extension"></a>Installieren und Ausführen der Azurite-Erweiterung für Visual Studio Code

Wählen Sie in Visual Studio Code den Bereich **„EXTENSIONS“** aus und suchen Sie dann nach *Azurite* unter **„EXTENSIONS:MARKETPLACE“** .

![Visual Studio Code Marketplace für Erweiterungen](media/storage-use-azurite/azurite-vs-code-extension.png)

Alternativ können Sie auch in Ihrem Browser den [VS Code-Store für Erweiterungen](https://marketplace.visualstudio.com/items?itemName=Azurite.azurite) aufrufen. Wählen Sie die Schaltfläche **„Installieren“** , um Visual Studio Code zu öffnen, und wechseln Sie dann direkt zur Azurite-Erweiterungsseite.

Sie können Azurite schnell starten oder schließen, indem Sie auf **Azurite Blob-Dienst** in der VS Code-Statusleiste klicken oder die folgenden Befehle in der VS Code-Befehlspalette ausführen. Um die Befehlspalette zu öffnen, drücken Sie **F1** in VS Code.

Die Erweiterung unterstützt die folgenden Visual Studio Code-Befehle:

   * **Azurite: Starten** – Alle Azurite-Dienste starten
   * **Azurite: Schließen** – Alle Azurite-Dienste schließen
   * **Azurite: Bereinigen** – Dauerhaften Daten aller Azurite-Dienste zurücksetzen
   * **Azurite: Starten** – Starten des Blobdiensts
   * **Azurite: Schließen** – Schließen des Blobdiensts
   * **Azurite: Bereinigen** – Bereinigen des Blobdiensts

Um Azurite in Visual Studio Code zu konfigurieren, wählen Sie den Erweiterungsbereich und klicken Sie mit der rechten Maustaste auf **Azurite**. Wählen Sie **„Erweiterungseinstellungen konfigurieren“** .

![Konfigurieren der Erweiterungseinstellungen für Azurite](media/storage-use-azurite/azurite-configure-extension-settings.png)

Die folgenden Einstellungen werden unterstützt:

   * **Azurite: Blob-Host** – Der Überwachungsendpunkt des Blobdiensts. Die Standardeinstellung ist 127.0.0.1.
   * **Azurite: Blob-Port** – Der Überwachungsport des Blobdiensts. Der Standardport ist 10000.
   * **Azurite: Debuggen** – Ausgabe des Debugprotokolls an den Azurite-Kanal. Der Standardwert ist **false**.
   * **Azurite: Speicherort** – Der Speicherpfad des Arbeitsbereichs. Der Standard ist der Arbeitsordner für Visual Studio Code.
   * **Azurite: Lautlos** – Der Lautlosmodus deaktiviert das Zugriffsprotokoll. Der Standardwert ist **false**.

## <a name="install-and-run-azurite-by-using-npm"></a>Installieren und Ausführen von Azurite mit NPM

Diese Installationsmethode setzt voraus, dass Sie [Node.js Version 8.0 oder höher](https://nodejs.org) installiert haben. **npm** ist das Paketverwaltungstool, das bei jeder Installation von Node.js enthalten ist. Führen Sie nach der Installation von Node.js den folgenden **npm-Befehl** aus, um Azurite zu installieren.

```console
npm install -g azurite
```

Nachdem Sie Azurite installiert haben, finden Sie weitere Informationen unter [„Azurite aus der Befehlszeile ausführen“](#run-azurite-from-a-command-line).

## <a name="install-and-run-the-azurite-docker-image"></a>Installieren und Ausführen vom Azurite Docker-Image

Verwenden Sie [DockerHub](https://hub.docker.com/), um das [neueste Azurite-Image](https://hub.docker.com/_/microsoft-azure-storage-azurite) mit dem folgenden Befehl zu laden:

```console
docker pull mcr.microsoft.com/azure-storage/azurite
```

**Ausführen des Azurite Docker-Images**:

Der folgende Befehl führt das Azurite Docker-Image aus. Der Parameter `-p 10000:10000` leitet Anforderungen vom Port 10000 des Host-Computers an die Docker-Instanz weiter.

```console
docker run -p 10000:10000 mcr.microsoft.com/azure-storage/azurite
```

**Geben Sie den Speicherort für den Arbeitsbereich an**:

Im folgenden Beispiel gibt der Parameter `-v c:/azurite:/data` `c:/azurite` als persistenten Datenspeicherort für Azurite an.

```console
docker run -p 10000:10000 -v c:/azurite:/data mcr.microsoft.com/azure-storage/azurite
```

**Alle Azurite-Parameter einstellen:**

Dieses Beispiel zeigt, wie Sie alle Befehlszeilenparameter einrichten. Alle der folgenden Parameter sollten auf einer einzigen Befehlszeile platziert werden.

```console
docker run -p 8888:8888
           -v c:/azurite:/workspace mcr.microsoft.com/azure-storage/azurite azurite
           -l /workspace
           -d /workspace/debug.log
           --blobPort 8888
           --blobHost 0.0.0.0
```

Weitere Informationen zur Konfiguration von Azurite beim Start finden Sie unter [„Befehlszeilenoptionen“](#command-line-options).

## <a name="clone-build-and-run-azurite-from-the-github-repository"></a>Klonen, Erstellen und Ausführen von Azurite aus dem GitHub-Repository

Diese Installationsmethode setzt voraus, dass Sie [Git](https://git-scm.com/) installiert haben. Klonen Sie das [GitHub-Repository](https://github.com/azure/azurite) für das Azurite-Projekt mit dem folgenden Konsolenbefehl.

```console
git clone https://github.com/Azure/Azurite.git
```

Nachdem Sie den Quellcode geklont haben, führen Sie folgende Befehle aus dem Stammverzeichnis des geklonten Repos aus, um Azurite zu erstellen und zu installieren.

```console
npm install
npm run build
npm install -g
```

Nachdem Sie Azurite installiert und erstellt haben, finden Sie weitere Informationen unter [„Azurite aus der Befehlszeile ausführen“](#run-azurite-from-a-command-line).

## <a name="run-azurite-from-a-command-line"></a>Azurite aus der Befehlszeile ausführen

> [!NOTE]
> Azurite kann nicht über die Befehlszeile ausgeführt werden, wenn Sie nur die Visual Studio Code-Erweiterung installiert haben. Verwenden Sie stattdessen die VS Code-Befehlspalette. Weitere Informationen finden Sie unter [„Installieren und Ausführen der Azurite Visual Studio Code-Erweiterung“](#install-and-run-the-azurite-visual-studio-code-extension).

Um sofort mit der Befehlszeile zu beginnen, erstellen Sie ein Verzeichnis namens **c:\azurite** und starten Sie dann Azurite, indem Sie den folgenden Befehl ausführen:

```console
azurite --silent --location c:\azurite --debug c:\azurite\debug.log
```

Dieser Befehl weist Azurite an, alle Daten in einem bestimmten Verzeichnis, nämlich **c:\azurite**, zu speichern. Wenn die Option **--location** weggelassen wird, wird das aktuelle Arbeitsverzeichnis verwendet.

## <a name="command-line-options"></a>Befehlszeilenoptionen

Dieser Abschnitt beschreibt die Befehlszeilenschalter, die beim Start von Azurite verfügbar sind. Alle Befehlszeilenschalter sind optional.

```console
C:\Azurite> azurite [--blobHost <IP address>] [--blobPort <port address>]
    [-l | --location <workspace path>] [-s | --silent] [-d | --debug <log file path>]
```

Der Schalter **-l** ist eine Abkürzung für **--location**, **-s** ist eine Abkürzung für **--silent** und **-d** ist eine Abkürzung für **--debug**.

### <a name="listening-host"></a>Lauschender Host

**Optional** Standardmäßig überwacht Azurite 127.0.0.0.0.1 als lokalen Server. Verwenden Sie den Schalter **--blobHost**, um die Adresse nach Ihren Wünschen festzulegen.

Nimmt nur Anfragen nur auf dem Computer Rechner an:

```console
azurite --blobHost 127.0.0.1
```

Zulassen von Remoteanforderungen:

```console
azurite --blobHost 0.0.0.0
```

> [!CAUTION]
> Wenn Sie Remoteanforderungen zulassen, kann es sein, dass Ihr System anfälliger für externe Bedrohungen wird.

### <a name="listening-port-configuration"></a>Konfiguration des Lauschports

**Optional** Standardmäßig wartet Azurite auf den Blob-Dienst auf Port 10000. Verwenden Sie den Schalter **--blobPort**, um den gewünschten Lauschport anzugeben.

> [!NOTE]
> Nachdem Sie einen benutzerdefinierten Port verwendet haben, müssen Sie die Verbindungszeichenfolge oder die entsprechende Konfiguration in Ihren Azure Storage-Tools oder SDKs aktualisieren.

Anpassen des Blob-Dienst-Lauschports:

```console
azurite --blobPort 8888
```

Lassen Sie das System automatisch einen verfügbaren Port auswählen:

```console
azurite --blobPort 0
```

Der verwendete Port wird beim Start von Azurite angezeigt.

### <a name="workspace-path"></a>Arbeitsbereichpfad

**Optional** Azurite speichert während der Ausführung Daten auf der lokalen Festplatte. Verwenden Sie den Schalter **--location**, um einen Pfad als Arbeitsbereichspeicherort anzugeben. Standardmäßig wird das aktuelle Arbeitsverzeichnis des Prozesses verwendet.

```console
azurite --location c:\azurite
```

```console
azurite -l c:\azurite
```

### <a name="access-log"></a>Zugriffsprotokoll

**Optional** Standardmäßig wird das Zugriffsprotokoll im Konsolenfenster angezeigt. Deaktivieren Sie die Anzeige des Zugriffsprotokolls mit dem Schalter **--silent**.

```console
azurite --silent
```

```console
azurite -s
```

### <a name="debug-log"></a>Debugprotokoll

**Optional** Das Debugprotokoll enthält detaillierte Informationen zu jeder Anforderung und jeder Ausnahmestapelüberwachung. Aktivieren Sie das Debugprotokoll, indem Sie dem Schalter **--debug** einen gültigen lokalen Dateipfad zuweisen.

```console
azurite --debug path/debug.log
```

```console
azurite -d path/debug.log
```

## <a name="authorization-for-tools-and-sdks"></a>Autorisierung für Tools und SDKs

Verbinden Sie sich mit Azurite über Azure Storage SDKs oder Tools wie [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/), indem Sie eine beliebige Authentifizierungsstrategie verwenden. Authentifizierung ist erforderlich. Azurite unterstützt die Autorisierung mit Shared Key und Shared Access Signatures (SAS). Azurite unterstützt auch den anonymen Zugriff auf öffentliche Container.

### <a name="well-known-storage-account-and-key"></a>Bekannte Speicherkonten und Schlüssel

Sie können den folgenden Kontonamen und Schlüssel mit Azurite verwenden. Dies ist dasselbe bekannte Konto und der Schlüssel, die auch vom älteren Azure-Speicheremulator verwendet werden.

* Kontoname: `devstoreaccount1`
* Kontoschlüssel: `Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==`

> [!NOTE]
> Zusätzlich zur SharedKey-Authentifizierung unterstützt Azurite die SAS-Authentifizierung von Konten und Diensten. Anonymer Zugriff ist auch möglich, wenn ein Container so eingestellt ist, dass er öffentlichen Zugriff ermöglicht.

### <a name="connection-string"></a>Verbindungszeichenfolge

Am einfachsten können Sie über Ihre Anwendung eine Verbindung mit Azurite herstellen, indem Sie eine Verbindungszeichenfolge in der Konfigurationsdatei Ihrer Anwendung konfigurieren, die auf die Verknüpfung *UseDevelopmentStorage=true* verweist. Eine Verbindungszeichenfolge, die auf die *app.config*-Datei verweist, kann beispielsweise wie folgt aussehen:

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

Weitere Informationen hierzu finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](storage-configure-connection-string.md).

### <a name="storage-explorer"></a>Storage-Explorer

Verbinden Sie sich im Azure Storage Explorer mit Azurite, indem Sie auf das Symbol **„Konto hinzufügen“** klicken, dann **„Verbinden mit einem lokalen Emulator“** auswählen und auf **„Verbinden“** klicken.

## <a name="differences-between-azurite-and-azure-storage"></a>Unterschiede zwischen Azurite und Azure Storage

Es gibt funktionale Unterschiede zwischen einer lokalen Instanz auf Azurite und einem Azure Storage-Konto in der Cloud.

### <a name="endpoint-and-connection-url"></a>Endpunkt und Verbindungs-URL

Die Dienstendpunkte für Azurite unterscheiden sich von denen eines Azure-Speicherkontos. Der lokale Computer führt keine Domänennamenauflösung durch, so dass Azuriteendpunkte lokale Adressen sein müssen.

Wenn Sie eine Ressource in einem Azure Storage-Konto adressieren, ist der Kontoname Teil des URI-Hostnamens. Die adressierte Ressource ist Teil des URI-Pfades:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

Die folgende URI ist eine gültige Adresse für einen Blob in einem Azure Storage-Konto:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

Da der lokale Computer Domänennamen nicht auflöst, ist der Kontoname Teil des URI-Pfads und nicht des Hostnamens. Verwenden Sie das folgende URI-Format für eine Ressource in Azurite:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Die folgende Adresse kann beispielsweise für den Zugriff auf einen Blob in Azurite verwendet werden:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

### <a name="scaling-and-performance"></a>Skalierung und -Leistung

Azurite ist kein skalierbarer Speicherdienst und unterstützt keine große Anzahl von gleichzeitigen Clients. Es gibt keine Leistungsgarantie. Azurite ist zu Entwicklungs- und Testzwecken gedacht.

### <a name="error-handling"></a>Fehlerbehandlung

Azurite ist mit der Fehlerbehandlungslogik von Azure Storage abgestimmt, aber es gibt trotzdem Unterschiede. Beispielsweise können Fehlermeldungen unterschiedlich ausfallen, während die Fehlerstatuscodes zusammenpassen.

### <a name="ra-grs"></a>RA-GRS

Azurite unterstützt die geografisch redundante Replikation (RA-GRS) mit Lesezugriff. Für Speicherressourcen können Sie auf den sekundären Speicherort zugreifen, indem Sie **-secondary** an den Kontonamen anfügen. Beispielsweise kann die folgende Adresse für den Zugriff auf ein Blob mithilfe des sekundären Speicherorts mit Lesezugriff in Azurite verwendet werden:

`http://127.0.0.1:10000/devstoreaccount1-secondary/mycontainer/myblob.txt`

## <a name="azurite-is-open-source"></a>Azurite ist Open-Source

Wir freuen uns über Beiträge und Vorschläge für Azurite. Auf der Azurite [GitHub-Projekt](https://github.com/Azure/Azurite/projects)seite oder unter [„GitHub-Issues“](https://github.com/Azure/Azurite/issues) finden Sie Meilensteine und Arbeitselemente, die wir für zukünftige Features und Fehlerbehebungen nachverfolgen. Detaillierte Arbeitselemente werden auch in GitHub verfolgt.

## <a name="next-steps"></a>Nächste Schritte

* [„Verwenden des Azure-Speicheremulators für Entwicklung und Tests“](storage-use-emulator.md) beschreibt den alten Azure-Speicheremulator, der durch Azurite ersetzt wird.
* [„Konfigurieren von Azure Storage-Verbindungszeichenfolgen“](storage-configure-connection-string.md) erklärt, wie Sie eine gültige Azure Storage-Verbindungszeichenfolge zusammenstellen können.
