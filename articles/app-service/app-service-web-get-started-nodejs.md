---
title: 'Erstellen einer Node.js-Web-App: Azure App Service | Microsoft-Dokumentation'
description: Stellen Sie in wenigen Minuten Ihre erste „Hallo Welt“-Node.js-App in einer Azure App Service-Web-App bereit.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 09/30/2019
ms.author: cephalin
ms.custom: seodec18
experimental: false
experiment_id: a231f2b4-2625-4d
ms.openlocfilehash: 380e587fc8c921b395d63d1dbca10e2f5fb1b9ba
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72433207"
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Erstellen einer Node.js-Web-App in Azure 

Von Azure App Service wird ein hochgradig skalierbarer Webhostingdienst mit Self-Patching bereitgestellt. In diesem Schnellstart-Artikel wird erläutert, wie Sie eine Node.js-App in Azure App Service bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Konto besitzen, [registrieren Sie sich jetzt](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-app-service-extension&mktingSource=vscode-tutorial-app-service-extension) für ein kostenloses Konto mit 200 US-Dollar Azure-Guthaben, um verschiedene Dienstkombinationen auszuprobieren.

[Visual Studio Code](https://code.visualstudio.com/) sowie [Node.js und npm](https://nodejs.org/en/download) – der Node.js-Paket-Manager – müssen installiert sein.

Sie müssen auch die [Azure App Service-Erweiterung](vscode:extension/ms-azuretools.vscode-azureappservice) installieren, mit der Sie Linux-Web-Apps im Azure-PaaS-Dienst (Platform-as-a-Service) erstellen, verwalten und bereitstellen können.

### <a name="sign-in"></a>Anmelden

Melden Sie sich bei Ihrem Azure-Konto an, sobald die Erweiterung installiert ist. Wählen Sie in der Aktivitätsleiste das Azure-Logo aus, um den **AZURE APP SERVICE**-Explorer anzuzeigen. Wählen Sie **Bei Azure anmelden...** aus, und befolgen Sie die Anweisungen.

![Anmelden bei Azure](containers/media/quickstart-nodejs/sign-in.png)

### <a name="troubleshooting"></a>Problembehandlung

Wenn der Fehler **Abonnement mit dem Namen [Abonnement-ID] nicht gefunden** angezeigt wird, kann dies daran liegen, dass Ihr Computer sich hinter einem Proxy befindet und die Azure-API nicht erreichen kann. Konfigurieren Sie die Umgebungsvariablen `HTTP_PROXY` und `HTTPS_PROXY` mithilfe von `export` in Ihrem Terminal mit Ihren Proxyinformationen.

```sh
export HTTPS_PROXY=https://username:password@proxy:8080
export HTTP_PROXY=http://username:password@proxy:8080
```

Wenn sich das Problem durch Festlegen der Umgebungsvariable nicht beheben lässt, wenden Sie sich an uns, indem Sie unten die Schaltfläche **Ich bin auf ein Problem gestoßen** auswählen.

### <a name="prerequisite-check"></a>Prüfen der Voraussetzungen

Bevor Sie fortfahren, stellen Sie sicher, dass alle erforderlichen Komponenten installiert und konfiguriert sind.

In VS Code sehen Sie Ihre Azure-E-Mail-Adresse in der Statusleiste und Ihr Abonnement im **AZURE APP SERVICE-Explorer**.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=getting-started)

## <a name="create-your-nodejs-application"></a>Erstellen der Node.js-Anwendung

Als Nächstes erstellen Sie eine Node.js-Anwendung, die in der Cloud bereitgestellt werden kann. In dieser Schnellstartanleitung wird ein Anwendungsgenerator verwendet, um in einem Terminal schnell ein Gerüst für die Anwendung zu erstellen.

> [!TIP]
> Wenn Sie das [Node.js-Tutorial](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial) bereits abgeschlossen haben, können Sie mit dem Schritt [Bereitstellen in Azure](#deploy-to-azure) fortfahren.

### <a name="scaffold-a-new-application-with-the-express-generator"></a>Gerüst einer neuen Anwendung mit dem Express-Generator

[Express](https://www.expressjs.com) ist ein beliebtes Framework für die Erstellung und Ausführung von Node.js-Anwendungen. Sie können eine neue Express-Anwendung (bzw. ihr Gerüst) mithilfe des Tools [Express-Generator](https://expressjs.com/en/starter/generator.html) erstellen. Der Express-Generator wird als npm-Modul ausgeliefert und kann direkt (ohne Installation) mithilfe des npm-Befehlszeilentools `npx` ausgeführt werden.

```bash
npx express-generator myExpressApp --view pug --git
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

![Ausführen der Express-Anwendung](containers/media/quickstart-nodejs/express.png)

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=create-app)

## <a name="deploy-to-azure"></a>Bereitstellen in Azure

In diesem Abschnitt stellen Sie Ihre Node.js-App mithilfe von VS Code und der Azure App Service-Erweiterung bereit. Diese Schnellstartanleitung verwendet das einfachste Bereitstellungsmodell, bei dem Ihre App gezippt und unter Linux in einer Azure-Web-App bereitgestellt wird.

### <a name="deploy-using-azure-app-service"></a>Bereitstellen mit Azure App Service

Öffnen Sie zunächst Ihren Anwendungsordner in VS Code.

```bash
code .
```

Wählen Sie im **AZURE APP SERVICE-Explorer** den blauen, nach oben weisenden Pfeil aus, um Ihre App in Azure bereitzustellen.

![Bereitstellen in Web-App](containers/media/quickstart-nodejs/deploy.png)

> [!TIP]
> Sie können die Anwendung auch über die **Befehlspalette** (STRG+UMSCHALT+P) bereitstellen, indem Sie „deploy to web app“ eingeben und den Befehl **Azure App Service: Bereitstellen in Web-App** ausführen.

1. Wählen Sie das Verzeichnis aus, das Sie zurzeit geöffnet haben: `myExpressApp`.

1. Wählen Sie eine Erstellungsoption basierend auf dem Betriebssystem aus, auf dem die Bereitstellung erfolgen soll:

    - Linux: Klicken Sie auf **Neue Web-App erstellen**.
    - Windows: Wählen Sie **Create new Web App...Advanced** (Neue Web-App erstellen...Erweitert) aus.

1. Geben Sie einen global eindeutigen Namen für Ihre Web-App ein, und drücken Sie die EINGABETASTE. Gültige Zeichen für den Namen einer App sind die Buchstaben a-z, die Ziffern 0-9 und der Bindestrich (-).

1. Wenn Linux Ihre Zielplattform ist, wählen Sie eine Node.js-Version aus, wenn Sie dazu aufgefordert werden. Eine **LTS**-Version wird empfohlen.

1. Wenn Sie Windows als Zielplattform mithilfe der Option *Advanced* (Erweitert)* verwenden, folgen Sie den zusätzlichen Eingabeaufforderungen:
    1. Wählen Sie **Neue Ressourcengruppe erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein.
    1. Wählen Sie als Betriebssystem **Windows** aus.
    1. Wählen Sie einen vorhandenen App Service-Plan aus, oder erstellen Sie einen neuen Plan. Sie können einen Tarif auswählen, wenn Sie einen neuen Plan erstellen.
    1. Wählen Sie **Vorerst überspringen** aus, wenn eine Eingabeaufforderung zu Application Insights sehen.
    1. Wählen Sie eine Region in Ihrer Nähe oder in der Nähe von Ressourcen aus, auf die Sie zugreifen möchten.

1. Wenn Sie auf alle Eingabeaufforderungen reagiert haben, zeigt der Benachrichtigungskanal die Azure-Ressourcen an, die für Ihre App erstellt werden.

1. Wählen Sie **Ja** aus, wenn Sie aufgefordert werden, Ihre Konfiguration für die Ausführung von `npm install` auf dem Zielserver zu aktualisieren. Ihre App wird anschließend bereitgestellt.

    ![Konfigurierte Bereitstellung](containers/media/quickstart-nodejs/server-build.png)

1. Wenn die Bereitstellung startet, werden Sie aufgefordert, Ihren Arbeitsbereich zu aktualisieren, sodass spätere Bereitstellungen automatisch die gleiche App Service-Web-App als Ziel verwenden. Klicken Sie auf **Ja**, um sicherzustellen, dass Ihre Änderungen für die richtige App bereitgestellt werden.

    ![Konfigurierte Bereitstellung](containers/media/quickstart-nodejs/save-configuration.png)

> [!TIP]
> Stellen Sie sicher, dass Ihre Anwendung an dem Port lauscht, der von der PORT-Umgebungsvariable `process.env.PORT` bereitgestellt wird.

### <a name="browse-the-app-in-azure"></a>Durchsuchen der App in Azure

Wählen Sie nach Abschluss der Bereitstellung an der Eingabeaufforderung **Website durchsuchen** aus, um Ihre neu bereitgestellte Web-App anzuzeigen.

### <a name="troubleshooting"></a>Problembehandlung

Wenn der Fehler **Sie sind nicht berechtigt, dieses Verzeichnis oder diese Seite anzuzeigen.** angezeigt wird, wurde die Anwendung wahrscheinlich nicht ordnungsgemäß gestartet. Fahren Sie mit dem nächsten Abschnitt fort, und sehen Sie sich die Protokollausgabe an, um den Fehler zu suchen und zu beheben. Wenn sich das Problem nicht beheben lässt, wenden Sie sich an uns, indem Sie unten die Schaltfläche **Ich bin auf ein Problem gestoßen** auswählen. Wir helfen Ihnen gerne weiter.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

### <a name="update-the-app"></a>Aktualisieren der App

Sie können Änderungen an dieser App bereitstellen, indem Sie den gleichen Prozess ausführen und die vorhandene App auswählen anstatt eine neue zu erstellen.

## <a name="viewing-logs"></a>Anzeigen von Protokollen

In diesem Abschnitt erfahren Sie, wie Sie die Protokolle der ausgeführten App Service-App anzeigen (bzw. verfolgen). Alle Aufrufe von `console.log` in der App werden in Visual Studio Code im Ausgabefenster angezeigt.

Suchen Sie die App im **AZURE APP SERVICE-Explorer**, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Streamingprotokolle anzeigen** aus.

Wenn Sie dazu aufgefordert werden, aktivieren Sie die Protokollierung, und starten Sie die Anwendung neu. Sobald die App neu gestartet wurde, wird das VS Code-Ausgabefenster mit einer Verbindung mit dem Protokollstream geöffnet.

![Anzeigen von Streamingprotokollen](containers/media/quickstart-nodejs/view-logs.png)

![Aktivieren von Protokollierung und Neustart](containers/media/quickstart-nodejs/enable-restart.png)

Nach einigen Sekunden sehen Sie eine Meldung, die Sie darüber informiert, dass eine Verbindung mit dem Protokollstreamingdienst hergestellt wurde. Aktualisieren Sie die Seite mehrmals, um weitere Aktivitäten anzuzeigen.

    ```bash
    2019-09-20 20:37:39.574 INFO  - Initiating warmup request to container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node
    2019-09-20 20:37:55.011 INFO  - Waiting for response to warmup request for container msdocs-vscode-node_2_00ac292a. Elapsed time = 15.4373071 sec
    2019-09-20 20:38:08.233 INFO  - Container msdocs-vscode-node_2_00ac292a for site msdocs-vscode-node initialized successfully and is ready to serve requests.
    2019-09-20T20:38:21  Startup Request, url: /Default.cshtml, method: GET, type: request, pid: 61,1,7, SCM_SKIP_SSL_VALIDATION: 0, SCM_BIN_PATH: /opt/Kudu/bin, ScmType: None
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
