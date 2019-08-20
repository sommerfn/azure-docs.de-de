---
title: Erstellen einer Node.js-Web-App
description: Node.js-Bereitstellung in Azure App Service
author: KarlErickson
ms.author: karler
ms.date: 07/18/2019
ms.topic: quickstart
ms.service: app-service
ms.devlang: javascript
ms.openlocfilehash: ced2977509f16f8dab2abe5546e19b7e05fb2a3d
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68975806"
---
# <a name="create-a-nodejs-app-in-azure"></a>Erstellen einer Node.js-App in Azure

Von Azure App Service wird ein hochgradig skalierbarer Webhostingdienst mit Self-Patching bereitgestellt. In diesem Schnellstart-Artikel wird erläutert, wie Sie eine Node.js-App in Azure App Service bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Konto besitzen, [registrieren Sie sich jetzt](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension) für ein kostenloses Konto mit 200 US-Dollar Azure-Guthaben, um verschiedene Dienstkombinationen auszuprobieren.

[Visual Studio Code](https://code.visualstudio.com/) sowie [Node.js und npm](https://nodejs.org/en/download) – der Node.js-Paket-Manager – müssen installiert sein.

Sie müssen auch die [Azure App Service-Erweiterung](vscode:extension/ms-azuretools.vscode-azureappservice) installieren, mit der Sie Linux-Web-Apps im Azure-PaaS-Dienst (Platform-as-a-Service) erstellen, verwalten und bereitstellen können.

### <a name="sign-in"></a>Anmelden

Melden Sie sich bei Ihrem Azure-Konto an, sobald die Erweiterung installiert ist. Klicken Sie in der Aktivitätsleiste auf das Azure-Logo, um den **AZURE APP SERVICE-Explorer** anzuzeigen. Klicken Sie auf **Bei Azure anmelden...** , und befolgen Sie die Anweisungen.

![Anmelden bei Azure](./media/quickstart-nodejs/sign-in.png)

### <a name="troubleshooting"></a>Problembehandlung

Wenn der Fehler **Abonnement mit dem Namen [Abonnement-ID] nicht gefunden** angezeigt wird, kann dies daran liegen, dass Ihr Computer sich hinter einem Proxy befindet und die Azure-API nicht erreichen kann. Konfigurieren Sie die Umgebungsvariablen `HTTP_PROXY` und `HTTPS_PROXY` mithilfe von `export` in Ihrem Terminal mit Ihren Proxyinformationen.

```sh
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Wenn sich das Problem durch Festlegen der Umgebungsvariable nicht beheben lässt, wenden Sie sich an uns, indem Sie unten auf die Schaltfläche **Ich bin auf ein Problem gestoßen** klicken.

### <a name="prerequisite-check"></a>Prüfen der Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass alle erforderlichen Komponenten installiert und konfiguriert sind.

In VS Code sehen Sie Ihre Azure-E-Mail-Adresse in der Statusleiste und Ihr Abonnement im **AZURE APP SERVICE-Explorer**.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=getting-started)

## <a name="create-your-nodejs-application"></a>Erstellen der Node.js-Anwendung

Als Nächstes erstellen Sie eine Node.js-Anwendung, die in der Cloud bereitgestellt werden kann. In dieser Schnellstartanleitung wird ein Anwendungsgenerator verwendet, um in einem Terminal schnell ein Gerüst für die Anwendung zu erstellen.

> [!TIP]
> Wenn Sie das [Node.js-Tutorial](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial) bereits abgeschlossen haben, können Sie mit dem Schritt [Bereitstellen der Website](#deploy-the-website) fortfahren.

### <a name="install-the-express-generator"></a>Installieren des Express-Generators

[Express](https://www.expressjs.com) ist ein beliebtes Framework für die Erstellung und Ausführung von Node.js-Anwendungen. Sie können eine neue Express-Anwendung (bzw. ihr Gerüst) mithilfe des Tools [Express-Generator](https://expressjs.com/en/starter/generator.html) erstellen. Der Express-Generator wird als npm-Modul ausgeliefert und mithilfe des npm-Befehlszeilentools `npm` installiert.

```bash
npm install -g express-generator
```

Die Option `-g` installiert den Express-Generator global auf Ihrem Computer, sodass Sie ihn von überall aus ausführen können.

### <a name="scaffold-a-new-application"></a>Gerüst für eine neue Anwendung

Als Nächstes erstellen Sie ein Gerüst für eine neue Express-Anwendung namens `myExpressApp`, indem Sie Folgendes ausführen:

```bash
express myExpressApp --view pug --git
```

Die `--view pug --git`-Parameter weisen den Generator an, die Vorlagen-Engine [pug](https://pugjs.org/api/getting-started.html) zu verwenden (früher als `jade` bezeichnet) und eine `.gitignore`-Datei zu erstellen.

Um alle Abhängigkeiten der Anwendung zu installieren, wechseln Sie zum neuen Ordner und führen `npm install` aus.

```bash
cd myExpressApp
npm install
```

### <a name="run-the-application"></a>Ausführen der Anwendung

Als Nächstes stellen Sie sicher, dass die Anwendung ausgeführt wird. Starten Sie die Anwendung über das Terminal, indem Sie den Befehl `npm start` zum Starten des Servers verwenden.

```bash
npm start
```

Öffnen Sie jetzt Ihren Browser, und navigieren Sie zu [http://localhost:3000](http://localhost:3000). Dort sollte ein Bildschirm wie der folgende angezeigt werden:

![Ausführen der Express-Anwendung](./media/quickstart-nodejs/express.png)

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-the-website"></a>Bereitstellen der Website

In diesem Abschnitt stellen Sie Ihre Node.js-Website mithilfe von VS Code und der Azure App Service-Erweiterung bereit. Diese Schnellstartanleitung verwendet das einfachste Bereitstellungsmodell, bei dem Ihre App gezippt und unter Linux in einer Azure-Web-App bereitgestellt wird.

### <a name="deploy-using-azure-app-service"></a>Bereitstellen mit Azure App Service

Öffnen Sie zunächst Ihren Anwendungsordner in VS Code.

```bash
code .
```

Klicken Sie im **AZURE APP SERVICE-Explorer** auf den blauen, nach oben weisenden Pfeil, um Ihre App in Azure bereitzustellen.

![Bereitstellen in Web-App](./media/quickstart-nodejs/deploy.png)

> [!TIP]
> Sie können die Anwendung auch über die **Befehlspalette** (STRG+UMSCHALT+P) bereitstellen, indem Sie „deploy to web app“ eingeben und den Befehl **Azure App Service: Bereitstellen in Web-App** ausführen.

1. Wählen Sie das Verzeichnis aus, das Sie zurzeit geöffnet haben: `myExpressApp`.

2. Klicken Sie auf **Neue Web-App erstellen**.

3. Geben Sie einen global eindeutigen Namen für Ihre Web-App ein, und drücken Sie die EINGABETASTE. Gültige Zeichen für den Namen einer App sind die Buchstaben a-z, die Ziffern 0-9 und der Bindestrich (-).

4. Wählen Sie Ihre **Node.js-Version** aus. Empfohlen wird LTS.

    Der Benachrichtigungskanal zeigt die Azure-Ressourcen an, die für Ihre App erstellt werden.

Klicken Sie auf **Ja**, wenn Sie aufgefordert werden, Ihre Konfiguration für die Ausführung von `npm install` auf dem Zielserver zu aktualisieren. Ihre App wird anschließend bereitgestellt.

![Konfigurierte Bereitstellung](./media/quickstart-nodejs/server-build.png)

Wenn die Bereitstellung startet, werden Sie aufgefordert, Ihren Arbeitsbereich zu aktualisieren, sodass spätere Bereitstellungen automatisch die gleiche App Service-Web-App als Ziel verwenden. Klicken Sie auf **Ja**, um sicherzustellen, dass Ihre Änderungen für die richtige App bereitgestellt werden.

![Konfigurierte Bereitstellung](./media/quickstart-nodejs/save-configuration.png)

> [!TIP]
> Stellen Sie sicher, dass Ihre Anwendung an dem Port lauscht, der von der PORT-Umgebungsvariable `process.env.PORT` bereitgestellt wird.

### <a name="browse-the-website"></a>Website durchsuchen

Sobald die Bereitstellung abgeschlossen ist, klicken Sie in der Aufforderung auf **Website durchsuchen**, um Ihre neu bereitgestellte Website anzuzeigen.

### <a name="troubleshooting"></a>Problembehandlung

Wenn der Fehler **Sie sind nicht berechtigt, dieses Verzeichnis oder diese Seite anzuzeigen.** angezeigt wird, wurde die Anwendung wahrscheinlich nicht ordnungsgemäß gestartet. Fahren Sie mit dem nächsten Abschnitt fort, und sehen Sie sich die Protokollausgabe an, um den Fehler zu suchen und zu beheben. Wenn sich das Problem nicht beheben lässt, wenden Sie sich an uns, indem Sie unten auf die Schaltfläche **Ich bin auf ein Problem gestoßen** klicken. Wir helfen Ihnen gerne weiter.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="updating-the-website"></a>Aktualisieren der Website

Sie können Änderungen an dieser App bereitstellen, indem Sie den gleichen Prozess ausführen und die vorhandene App auswählen anstatt eine neue zu erstellen.

## <a name="viewing-logs"></a>Anzeigen von Protokollen

In diesem Abschnitt erfahren Sie, wie Sie die Protokolle der ausgeführten Website anzeigen. Alle Aufrufe von `console.log` auf der Website werden in Visual Studio Code im Ausgabefenster angezeigt.

Suchen Sie die App im **AZURE APP SERVICE-Explorer**, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Streamingprotokolle anzeigen** aus.

Wenn Sie dazu aufgefordert werden, aktivieren Sie die Protokollierung, und starten Sie die Anwendung neu. Sobald die App neu gestartet wurde, wird das VS Code-Ausgabefenster mit einer Verbindung mit dem Protokollstream geöffnet.

![Anzeigen von Streamingprotokollen](./media/quickstart-nodejs/view-logs.png)

![Aktivieren von Protokollierung und Neustart](./media/quickstart-nodejs/enable-restart.png)

Nach einigen Sekunden sehen Sie eine Meldung, die Sie darüber informiert, dass eine Verbindung mit dem Protokollstreamingdienst hergestellt wurde.

```bash
Connecting to log-streaming service...
2017-12-21 17:33:51.428 INFO  - Container practical-mustache_2 for site practical-mustache initialized successfully.
2017-12-21 17:33:56.500 INFO  - Container logs
```

Laden Sie die Seite im Browser einige Male neu, um die Protokollausgabe anzuzeigen.

```bash
2017-12-21 17:35:17.774 INFO  - Container logs
2017-12-21T17:35:14.955412230Z GET / 304 141.798 ms - -
2017-12-21T17:35:15.248930479Z GET /stylesheets/style.css 304 3.180 ms - -
2017-12-21T17:35:15.378623115Z GET /favicon.ico 404 53.839 ms - 995
```

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=tailing-logs)

## <a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch! Sie haben diese Schnellstartanleitung erfolgreich abgeschlossen!

Sehen Sie sich als Nächstes die anderen Azure-Erweiterungen an.

* [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
* [Azure-Funktionen](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
* [Docker-Tools](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
* [Azure CLI-Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
* [Azure Resource Manager Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

Sie können auch alle Erweiterungen auf einmal erhalten, indem Sie das [Node Pack for Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) installieren.
