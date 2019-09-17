---
title: Lokales Bereitstellen der Remoteüberwachungslösung (über IntelliJ-IDE) – Azure | Microsoft-Dokumentation
description: In dieser Schrittanleitung wird gezeigt, wie der Solution Accelerator für die Remoteüberwachung zu Test- und Entwicklungszwecken mithilfe von IntelliJ auf Ihrem lokalen Computer bereitgestellt wird.
author: v-krghan
manager: dominicbetts
ms.author: v-krghan
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/24/2019
ms.topic: conceptual
ms.openlocfilehash: 2f3c11763bb2f406caf9d33275fc29b0d140da9a
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "70743331"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---intellij"></a>Lokales Bereitstellen des Solution Accelerators für die Remoteüberwachung – IntelliJ

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

In diesem Artikel wird gezeigt, wie der Solution Accelerator für die Remoteüberwachung zu Test- und Entwicklungszwecken auf Ihrem lokalen Computer bereitgestellt wird. Sie erfahren, wie Sie die Microservices in IntelliJ ausführen. Eine lokale Microservices-Bereitstellung verwendet die folgenden Clouddienste: IoT Hub, Azure Cosmos DB, Azure Streaming Analytics und Azure Time Series Insights.

Wenn Sie den Solution Accelerator für die Remoteüberwachung auf Ihrem lokalen Computer in Docker ausführen möchten, finden Sie entsprechende Informationen unter [Lokales Bereitstellen des Solution Accelerators für die Remoteüberwachung – Docker](iot-accelerators-remote-monitoring-deploy-local-docker.md).

## <a name="prerequisites"></a>Voraussetzungen

Für die Bereitstellung der vom Solution Accelerator für die Remoteüberwachung benötigten Azure-Dienste benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Einrichtung des Computers

Um die lokale Bereitstellung abzuschließen, müssen die folgenden Tools auf dem lokalen Entwicklungscomputer installiert sein:

* [Git-Client](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/)
* [IntelliJ Scala-Plug-In](https://plugins.jetbrains.com/plugin/1347-scala)
* [IntelliJ SBT-Plug-In](https://plugins.jetbrains.com/plugin/5007-sbt)
* [IntelliJ SBT Executor-Plug-In](https://plugins.jetbrains.com/plugin/7247-sbt-executor)
* [Nginx](https://nginx.org/en/download.html)
* [Node.js v8](https://nodejs.org/)

Node.js v8 ist eine Voraussetzung für die PCS-CLI, die von den Skripts zum Erstellen von Azure-Ressourcen genutzt wird. Verwenden Sie nicht Node.js v10.

> [!NOTE]
> Die IntelliJ-IDE ist für Windows und Mac verfügbar.

## <a name="download-the-source-code"></a>Herunterladen des Quellcodes

Die Quellcoderepositorys für die Remoteüberwachung umfassen den Quellcode und die Docker-Konfigurationsdateien, die Sie zum Ausführen der Docker-Images für die Microservices benötigen.

Um eine lokale Version des Repositorys zu klonen und zu erstellen, wechseln Sie über Ihre Befehlszeilenumgebung zu einem geeigneten Ordner auf Ihrem lokalen Computer. Führen Sie dann einen der folgenden Befehlssätze aus, um das Java-Repository zu klonen:

* Wenn Sie die neueste Version der Java-Microservice-Implementierungen herunterladen möchten, führen Sie den folgenden Befehl aus:

  ```cmd/sh
  git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git
  ```

* Wenn Sie die neuesten Submodule abrufen möchten, führen Sie die folgenden Befehle aus:

   ```cmd/sh
   cd azure-iot-pcs-remote-monitoring-java
   git submodule foreach git pull origin master
    ```

> [!NOTE]
> Diese Befehle laden den Quellcode für alle Microservices zusätzlich zu den Skripts herunter, mit denen Sie die Microservices lokal ausführen. Sie benötigen den Quellcode nicht zum Ausführen der Microservices in Docker. Der Quellcode ist jedoch nützlich, wenn Sie zu einem späteren Zeitpunkt planen, den Solution Accelerator zu ändern und Ihre Änderungen lokal zu testen.

## <a name="deploy-the-azure-services"></a>Bereitstellen von Azure-Diensten

In diesem Artikel erfahren Sie zwar, wie Sie die Microservices lokal ausführen, doch sie hängen von Azure-Diensten ab, die in der Cloud ausgeführt werden. Stellen Sie die Azure-Dienste mit dem nachfolgenden Skript bereit. Bei den Skriptbeispielen wird davon ausgegangen, dass Sie das Java-Repository auf einem Windows-Computer verwenden. Wenn Sie in einer anderen Umgebung arbeiten, passen Sie die Pfade, Dateierweiterungen und Pfadtrennzeichen entsprechend an.

### <a name="create-new-azure-resources"></a>Erstellen neuer Azure-Ressourcen

Wenn Sie die erforderlichen Azure-Ressourcen noch nicht erstellt haben, führen Sie die folgenden Schritte aus:

1. Wechseln Sie in Ihrer Befehlszeilenumgebung zum Ordner **\services\scripts\local\launch** in Ihrer geklonten Kopie des Repositorys.

1. Führen Sie die folgenden Befehle aus, um das CLI-Tool **pcs** zu installieren und sich bei Ihrem Azure-Konto anzumelden:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Führen Sie das Skript **start.cmd** aus. Das Skript fordert Sie zur Eingabe der folgenden Informationen auf:

   * Ein Lösungsname.
   * Das zu verwendende Azure-Abonnement.
   * Der Speicherort des zu verwendenden Azure-Rechenzentrums.

   Das Skript erstellt in Azure eine Ressourcengruppe mit dem Namen Ihrer Projektmappe. Diese Ressourcengruppe enthält die vom Solution Accelerator verwendeten Azure-Ressourcen. Sie können diese Ressourcengruppe löschen, wenn Sie die entsprechenden Ressourcen nicht mehr benötigen.

   Mit dem Skript werden auf dem lokalen Computer außerdem mehrere Umgebungsvariablen hinzugefügt. Jeder Variablenname hat das Präfix **PCS**. Diese Umgebungsvariablen liefern Details, die der Remoteüberwachung das Lesen ihrer Konfigurationswerte aus einer Azure Key Vault-Ressource ermöglichen.

   > [!TIP]
   > Nach Abschluss des Skripts werden die Umgebungsvariablen in der Datei **\<Ihr Stammordner\>\\.pcs\\\<Lösungsname\>.env** gespeichert. Sie können sie für zukünftige Bereitstellungen des Solution Accelerators verwenden. Beachten Sie, dass alle auf Ihrem lokalen Computer festgelegten Umgebungsvariablen die Werte in der Datei **services\\scripts\\local\\.env** überschreiben, wenn Sie **docker-compose** ausführen.

1. Schließen Sie die Befehlszeilenumgebung.

### <a name="use-existing-azure-resources"></a>Verwenden vorhandener Azure-Ressourcen

Wenn Sie die erforderlichen Azure-Ressourcen bereits erstellt haben, legen Sie die entsprechenden Umgebungsvariablen auf Ihrem lokalen Computer fest:
* **PCS_KEYVAULT_NAME**: Der Name der Key Vault-Ressource.
* **PCS_AAD_APPID**: Die ID der Azure Active Directory-Anwendung (Azure AD).
* **PCS_AAD_APPSECRET**: Das Geheimnis der Azure AD-Anwendung.

Konfigurationswerte werden aus dieser Key Vault-Ressource gelesen. Diese Umgebungsvariablen können in der Datei **\<Ihr Stammordner\>\\.pcs\\\<Lösungsname\>.env** aus der Bereitstellung gespeichert werden. Beachten Sie, dass die auf dem lokalen Computer festgelegten Umgebungsvariablen die Werte in der Datei **services\\scripts\\local\\.env** überschreiben, wenn Sie **docker-compose** ausführen.

Ein Teil der vom Microservice benötigten Konfiguration wird in einer Key Vault-Instanz gespeichert, die bei der ersten Bereitstellung erstellt wurde. Die entsprechenden Variablen im Schlüsseltresor sollten nach Bedarf geändert werden.

## <a name="run-the-microservices"></a>Ausführen der Microservices

In diesem Abschnitt führen Sie die Microservices für die Remoteüberwachung aus. Sie führen Folgendes aus:

* Die Web-Benutzeroberfläche nativ.
* Der Azure IoT-Gerätesimulationsdienst, Authentifizierungsdienst und Azure Stream Analytics Manager-Dienst in Docker.
* Die Microservices in IntelliJ.

### <a name="run-the-device-simulation-service"></a>Ausführen des Gerätesimulationsdiensts

Öffnen Sie ein neues Eingabeaufforderungsfenster. Stellen Sie sicher, dass Sie Zugriff auf die Umgebungsvariablen haben, die im vorhergehenden Abschnitt mit dem Skript **start.cmd** festgelegt wurden.

Führen Sie den folgenden Befehl aus, um den Docker-Container für den Gerätesimulationsdienst zu öffnen. Der Dienst simuliert Geräte für die Remoteüberwachungslösung.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="run-the-auth-service"></a>Ausführen des Authentifizierungsdiensts

Öffnen Sie ein neues Eingabeaufforderungsfenster, und führen Sie den folgenden Befehl aus, um den Docker-Container für den Authentifizierungsdienst zu öffnen. Mithilfe dieses Diensts können Sie die Benutzer verwalten, die für den Zugriff auf Azure IoT-Lösungen autorisiert sind.

```cmd
<path_to_cloned_repository>\services\auth\scripts\docker\run.cmd
```

### <a name="run-the-stream-analytics-manager-service"></a>Ausführen des Stream Analytics Manager-Diensts

Öffnen Sie ein neues Eingabeaufforderungsfenster, und führen Sie den folgenden Befehl aus, um den Docker-Container für den Stream Analytics Manager-Dienst zu öffnen. Mit diesem Dienst können Sie Stream Analytics-Aufträge verwalten. Diese Verwaltung umfasst das Festlegen der Auftragskonfiguration sowie das Starten, Beenden und Überwachen des Auftragsstatus.

```cmd
<path_to_cloned_repository>\services\asa-manager\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-your-local-machine"></a>Bereitstellen aller anderen Microservices auf Ihrem lokalen Computer

In den folgenden Schritten wird erläutert, wie Sie die Microservices für die Remoteüberwachung in IntelliJ ausführen.

#### <a name="import-a-project"></a>Importieren eines Projekts

1. Öffnen Sie die IntelliJ-IDE.
1. Wählen Sie **Projekt importieren** aus.
1. Wählen Sie **azure-iot-pcs-remote-monitoring-java\services\build.sbt**.

#### <a name="create-run-configurations"></a>Erstellen von Laufzeitkonfigurationen

1. Wählen Sie **Ausführen** > **Konfigurationen bearbeiten** aus.
1. Wählen Sie **Neue Konfiguration hinzufügen** > **SBT-Aufgabe** aus.
1. Geben Sie einen **Namen** und dann **Tasks** als **Ausführung** ein.
1. Wählen Sie das **Arbeitsverzeichnis** basierend auf dem auszuführenden Dienst aus.
1. Wählen Sie **Anwenden** > **OK** aus, um Ihre Auswahl zu speichern.
1. Erstellen Sie Laufzeitkonfigurationen für die folgenden Webdienste:
    * WebService (services\config)
    * WebService (services\device-telemetry)
    * WebService (services\iothub-manager)
    * WebService (services\storage-adapter)

Die folgende Abbildung zeigt als Beispiel, wie eine Konfiguration für einen Dienst hinzugefügt wird:

[![Screenshot des IntelliJ-IDE-Fensters „Run/Debug Configurations“ (Konfigurationen ausführen/debuggen). Er zeigt im linken Bereich die in der Liste „SBT-Aufgaben“ hervorgehobene Option „storageAdapter“ und im rechten Bereich Einträge in den Feldern „Name“, „Aufgaben“, „Arbeitsverzeichnis“ und „VM-Parameter“.](./media/deploy-locally-intellij/run-configurations.png)](./media/deploy-locally-intellij/run-configurations.png#lightbox)

#### <a name="create-a-compound-configuration"></a>Erstellen einer Kombinationskonfiguration

1. Wenn Sie alle Dienste zusammen ausführen möchten, wählen Sie **Neue Konfiguration hinzufügen** > **Verbund** aus.
1. Geben Sie einen **Namen** ein, und wählen Sie **SBT-Aufgaben hinzufügen** aus.
1. Wählen Sie **Anwenden** > **OK** aus, um Ihre Auswahl zu speichern.

Die folgende Abbildung zeigt als Beispiel, wie alle SBT-Aufgaben zu einer einzelnen Konfiguration hinzugefügt werden:

[![Screenshot des IntelliJ-IDE-Fensters „Run/Debug Configurations“ (Konfigurationen ausführen/debuggen). Er zeigt im linken Bereich die in der Liste „Verbund“ hervorgehobene Option „AllServices“ und im rechten Bereich die hervorgehobene Option „deviceTelemetry“ der SBT-Aufgabe.](./media/deploy-locally-intellij/all-services.png)](./media/deploy-locally-intellij/all-services.png#lightbox)

Wählen Sie **Ausführen** aus, um die Webdienste auf dem lokalen Computer zu erstellen und auszuführen.

Für jeden Webdienst wird ein Eingabeaufforderungsfenster und ein Webbrowserfenster geöffnet. An der Eingabeaufforderung wird die Ausgabe des gerade ausgeführten Diensts angezeigt. Im Browserfenster können Sie den Status überwachen. Schließen Sie die Eingabeaufforderungsfenster oder Webseiten nicht, da durch diese Aktionen der Webdienst beendet wird.

Um auf den Status der Dienste zuzugreifen, wechseln Sie zu den folgenden URLs:

* IoT-Hub-Manager: [http://localhost:9002/v1/status](http://localhost:9002/v1/status)
* Gerätetelemetrie: [http://localhost:9004/v1/status](http://localhost:9004/v1/status)
* Konfiguration: [http://localhost:9005/v1/status](http://localhost:9005/v1/status)
* Speicheradapter: [http://localhost:9022/v1/status](http://localhost:9022/v1/status)

### <a name="start-the-stream-analytics-job"></a>Starten des Stream Analytics-Auftrags

Führen Sie folgende Schritte aus, um den Stream Analytics-Auftrag zu starten:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Wechseln Sie zu der **Ressourcengruppe**, die für Ihre Lösung erstellt wurde. Der Name der Ressourcengruppe ist der Name der Projektmappe, den Sie beim Ausführen des Skripts **start.cmd** ausgewählt haben.
1. Wählen Sie in der Liste der Ressourcen **Stream Analytics-Auftrag** aus.
1. Wählen Sie auf der Seite **Übersicht** des Stream Analytics-Auftrags die Schaltfläche **Start** und dann **Starten** aus, um den Auftrag zu starten.

### <a name="run-the-web-ui"></a>Ausführen der Webbenutzeroberfläche

In diesem Schritt starten Sie die Webbenutzeroberfläche. Öffnen Sie ein neues Eingabeaufforderungsfenster. Überprüfen Sie, ob Sie Zugriff auf die Umgebungsvariablen haben, die mit dem Skript **start.cmd** festgelegt wurden. Wechseln Sie in Ihrer lokalen Kopie des Repositorys zum Ordner **webui**, und führen Sie die folgenden Befehle aus:

```cmd
npm install
npm start
```

Nach Abschluss des Befehls **Starten** zeigt Ihr Browser die Seite unter der Adresse [http://localhost:3000/dashboard](http://localhost:3000/dashboard) an. Die Fehler auf dieser Seite sind zu erwarten. Wenn Sie die Anwendung ohne Fehler anzeigen möchten, führen Sie die folgenden Schritte aus.

### <a name="configure-and-run-nginx"></a>Konfigurieren und Ausführen von Nginx

Richten Sie einen Reverseproxyserver ein, der die Webanwendung mit den Microservices verknüpft, die auf Ihrem lokalen Computer ausgeführt werden:

1. Kopieren Sie die Datei **nginx.conf** aus dem Ordner **webui\scripts\localhost** in Ihrer lokalen Kopie des Repositorys in das Installationsverzeichnis **nginx\conf**.
1. Führen Sie Nginx aus.

Weitere Informationen zum Ausführen von Nginx finden Sie unter [nginx for Windows (nginx für Windows)](https://nginx.org/en/docs/windows.html).

### <a name="connect-to-the-dashboard"></a>Verbinden mit dem Dashboard

Wechseln Sie für den Zugriff auf das Dashboard der Remoteüberwachungslösung in Ihrem Browser zu http://localhost:9000.

## <a name="clean-up"></a>Bereinigen

Um unnötige Gebühren zu vermeiden, entfernen Sie nach Abschluss Ihrer Tests die Clouddienste aus Ihrem Azure-Abonnement. Wechseln Sie zum Entfernen der Dienste zum [Azure-Portal](https://ms.portal.azure.com), und löschen Sie die Ressourcengruppe, die mit dem Skript **start.cmd** erstellt wurde.

Sie können auch die lokale Kopie des Remoteüberwachungsrepositorys löschen, die erstellt wurde, als Sie den Quellcode aus GitHub geklont haben.

## <a name="next-steps"></a>Nächste Schritte

Nach Bereitstellung der Remoteüberwachungslösung können Sie sich als Nächstes [mit den Funktionen des Lösungsdashboards vertraut machen](quickstart-remote-monitoring-deploy.md).
