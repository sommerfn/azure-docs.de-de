---
title: " Verbinden Ihres Entwicklungscomputers mit einem AKS-Cluster"
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 11/04/2019
ms.topic: conceptual
description: Erfahren Sie, wie Sie Ihren Entwicklungscomputer mithilfe von Azure Dev Spaces mit einem AKS-Cluster verbinden.
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 0adaa4266978cf9c9ef42359c0cf4f81c5c5f056
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73571770"
---
# <a name="connect-your-development-machine-to-an-aks-cluster-preview"></a>Verbinden Ihres Entwicklungscomputers mit einem AKS-Cluster (Vorschauversion)

Azure Dev Spaces ermöglicht das Ausführen und Debuggen von Code mit oder ohne Container auf dem Entwicklungscomputer, während weiterhin eine Verbindung mit dem Kubernetes-Cluster mit den restlichen Anwendungen oder Diensten besteht. Durch Verbinden des Entwicklungscomputers mit dem Cluster können Sie Ihre Anwendung schnell entwickeln und End-to-End-Tests durchführen, ohne eine Docker- oder Kubernetes-Konfiguration erstellen zu müssen. Außerdem können Sie eine Verbindung mit dem AKS-Cluster herstellen, ohne dass sich das auf andere Workloads oder Benutzer auswirkt, die denselben Cluster verwenden.

Mit Azure Dev Spaces wird der Datenverkehr zwischen dem verbundenen AKS-Cluster und dem Entwicklungscomputer umgeleitet. Diese Umleitung des Datenverkehrs ermöglicht es, dass Code auf dem Entwicklungscomputer und die im AKS-Cluster ausgeführten Dienste so kommunizieren, als befänden sie sich im gleichen AKS-Cluster. Da der Code auf dem Entwicklungscomputer ausgeführt wird, sind Sie auch flexibel bei den Entwicklungstools, mit denen Sie den Code ausführen und debuggen. Azure Dev Spaces bietet außerdem die Möglichkeit, die für Pods im AKS-Cluster verfügbaren Umgebungsvariablen und eingebundenen Dateien auf dem Entwicklungscomputer zu replizieren.

In diesem Leitfaden lernen Sie Folgendes:

* Einrichten von Azure Dev Spaces in einem verwalteten Kubernetes-Cluster in Azure
* Bereitstellen einer umfangreichen Anwendung mit mehreren Microservices in einem Entwicklungsbereich.
* Verwenden von Azure Dev Spaces zum Umleiten von Datenverkehr zwischen dem AKS-Cluster und dem auf dem Entwicklungscomputer ausgeführten Code

> [!IMPORTANT]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. Vorschauversionen werden Ihnen zur Verfügung gestellt, wenn Sie die [zusätzlichen Nutzungsbedingungen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) akzeptieren. Einige Aspekte dieses Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## <a name="before-you-begin"></a>Voraussetzungen

In dieser Anleitung wird die [Azure Dev Spaces-Beispielanwendung „Bike Sharing“](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) verwendet, um zu veranschaulichen, wie Sie Ihren Entwicklungscomputer mit einem AKS-Cluster verbinden. Befolgen Sie die Anweisungen in der [Infodatei der Azure Dev Spaces-Beispielanwendung „Bike Sharing“](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/README.md), um die Beispielanwendung auszuführen. Auch wenn Sie eine eigene Anwendung in einem AKS-Cluster verwenden, können Sie die folgenden Schritte ausführen und dabei die Namen Ihrer eigenen Dienste und Pods verwenden.

### <a name="limitations"></a>Einschränkungen

* UDP wird derzeit nicht unterstützt.

### <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* [Azure-CLI installiert][azure-cli].
* [Visual Studio Code][vs-code] mit installierter [Azure Dev Spaces-Erweiterung][azds-vs-code] und ausgeführt unter macOS oder Windows 10.
* Ausführung der [Azure Dev Spaces-Beispielanwendung „Bike Sharing“](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) oder Ihrer eigenen Anwendung in einem AKS-Cluster.

## <a name="connect-your-development-machine"></a>Verbinden Ihres Entwicklungscomputers

Öffnen Sie *dev-spaces/samples/BikeSharingApp/Bikes* in Visual Studio Code, und verwenden Sie die Azure Dev Spaces-Erweiterung, um den Entwicklungscomputer mit dem AKS-Cluster zu verbinden.

Zum Verwenden der Azure Dev Spaces-Erweiterung öffnen Sie die Befehlspalette in Visual Studio Code, indem Sie auf *Ansicht* und dann auf *Befehlspalette* klicken. Beginnen Sie mit der Eingabe von `Azure Dev Spaces: Redirect`, und klicken Sie auf `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]`, `Azure Dev Spaces: Redirect an existing Kubernetes pod to my machine [Preview]` oder `Azure Dev Spaces: Redirect a new Kubernetes pod to my machine [Preview]`.

![Befehle](../media/how-to-connect/connect-commands.png)

### <a name="select-a-redirection-option"></a>Auswählen einer Umleitungsoption

Wenn Sie `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` ausführen, werden Sie aufgefordert, einen vorhandenen Kubernetes-Dienst auszuwählen:

![Auswählen eines Diensts](../media/how-to-connect/connect-choose-service.png)

Mit dieser Option wird der gesamte Datenverkehr im AKS-Cluster für den Dienst an die Version der Anwendung umgeleitet, die auf dem Entwicklungscomputer ausgeführt wird. Wenn für diesen Dienst mehrere Pods im AKS-Cluster ausgeführt werden, wird der gesamte Datenverkehr für den Dienst nur an den Entwicklungscomputer geleitet. Mit Azure Dev Spaces wird außerdem der gesamte ausgehende Datenverkehr der Anwendung zurück an den AKS-Cluster geleitet.

Wenn Sie `Azure Dev Spaces: Redirect an existing Kubernetes pod to my machine [Preview]` ausführen, werden Sie aufgefordert, einen spezifischen Pod auszuwählen:

![Auswählen eines Pods](../media/how-to-connect/connect-choose-pod.png)

Mit dieser Option wird eine Verbindung mit einem bestimmten Pod hergestellt. Diese Option ist nützlich für die Interaktion mit Pods, die keinen Datenverkehr senden oder empfangen, und die Replikation beendeter Pods. Wenn der Pod Datenverkehr sendet und empfängt, verhält sich diese Option ähnlich wie `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]`, d. h., der gesamte Datenverkehr im AKS-Cluster wird für alle Pods umgeleitet, die mit dem Dienst des ausgewählten Pods verbunden sind.

Wenn Sie `Azure Dev Spaces: Redirect a new Kubernetes pod to my machine [Preview]` ausführen, werden Sie nicht aufgefordert, einen vorhandenen Pod oder Dienst auszuwählen. Mit dieser Option wird der gesamte ausgehende Datenverkehr der Anwendung, die auf dem Entwicklungscomputer ausgeführt wird, an den AKS-Cluster umgeleitet.

Wählen Sie für dieses Beispiel `Azure Dev Spaces: Redirect an existing Kubernetes service to my machine [Preview]` und den Dienst *bikes* aus.

### <a name="select-a-connection-mode"></a>Auswählen eines Verbindungsmodus

Nach dem Auswählen der Umleitungsoption werden Sie aufgefordert, den Verbindungsmodus *Ersetzen* oder *Klonen* auszuwählen.

![„Ersetzen“ oder „Klonen“](../media/how-to-connect/connect-replace-clone.png)

Mit der Option *Ersetzen* wird der aktuelle Pod oder Dienst im AKS-Cluster ersetzt und der gesamte Datenverkehr für den Dienst an den Entwicklungscomputer umgeleitet. Diese Option kann zur Unterbrechung anderer Dienste im AKS-Cluster führen, die mit dem umgeleiteten Dienst interagieren. Diese Dienste werden möglicherweise nicht ausgeführt, bis Sie die Anwendung auf dem Entwicklungscomputer starten. Mit der Option *Klonen* können Sie einen vorhandenen untergeordneten Entwicklungsbereich auswählen oder einen neuen untergeordneten Entwicklungsbereich erstellen, um den Datenverkehr für einen Pod oder Dienst zum Entwicklungscomputer umzuleiten. Mit dieser Option können Sie isoliert arbeiten, ohne dass andere Dienste unterbrochen werden, da nur der Datenverkehr an den untergeordneten Entwicklungsbereich an den Entwicklungscomputer umgeleitet wird. Für die Option *Klonen* muss für den AKS-Cluster Azure Dev Spaces aktiviert sein.

Wählen Sie für dieses Beispiel *Ersetzen* aus.

> [!NOTE]
> Wenn der Pod des vorhandenen Diensts mehrere Container enthält, werden Sie auch aufgefordert, den Container der Anwendung auszuwählen.

### <a name="select-a-port-for-your-application"></a>Auswählen eines Ports für Ihre Anwendung

Nach Auswahl des Verbindungsmodus werden Sie aufgefordert, den TCP-Port für die lokale Anwendung einzugeben. Wenn für die Anwendung mehrere Ports geöffnet werden, trennen Sie diese durch ein Komma, z. B. *80,81*. Wenn in der Anwendung keine Netzwerkanforderungen akzeptiert werden, geben Sie *0* ein. Geben Sie für dieses Beispiel *3000* ein.

![Auswählen des Verbindungsports](../media/how-to-connect/connect-choose-port.png)

### <a name="confirm-you-are-connected"></a>Überprüfen der Verbindung

Nachdem Sie den TCP-Port der Anwendung ausgewählt haben, wird in Azure Dev Spaces eine Verbindung mit dem AKS-Cluster hergestellt. Azure Dev Spaces fügt einen Agent im AKS-Cluster ein, um Datenverkehr zwischen dem AKS-Cluster und dem Entwicklungscomputer umzuleiten. Das Einrichten dieser Verbindung kann einige Minuten dauern. In Azure Dev Spaces wird außerdem Administratorzugriff angefordert, um die Datei *hosts* auf dem Entwicklungscomputer ändern zu können.

> [!IMPORTANT]
> Nachdem mit Azure Dev Spaces eine Verbindung mit dem AKS-Cluster hergestellt wurde, werden die anderen Dienste im AKS-Cluster möglicherweise erst dann ordnungsgemäß ausgeführt, wenn Sie den Dienst auf dem Entwicklungscomputer starten. Wenn der Dienst eine Abhängigkeit aufweist, die auf dem Entwicklungscomputer nicht verfügbar ist, müssen Sie außerdem möglicherweise die Anwendung ändern oder eine [zusätzliche Konfiguration](#additional-configuration) vornehmen.

In Azure Dev Spaces wird nach dem Herstellen einer Verbindung mit dem AKS-Cluster ein Terminalfenster mit dem Titel *AZDS Connect – Bikes* (AZDS-Verbindung – Bikes) angezeigt. In diesem Terminalfenster sind alle im AKS-Cluster konfigurierten Umgebungsvariablen und DNS-Einträge aufgeführt. Sämtlicher Code, den Sie in diesem Terminalfenster ausführen oder der im Visual Studio Code-Debugger verwendet wird, ist mit dem AKS-Cluster verbunden.

![Terminal](../media/how-to-connect/connect-terminal.png)

Außerdem wird in Azure Dev Spaces das Fenster *Dev Spaces Connect* mit der gesamten zugehörigen Ausgabe erstellt.

![Output](../media/how-to-connect/connect-output.png)

In Azure Dev Spaces wird auf einer Statusleiste auch der Verbindungsstatus angezeigt.

![Status](../media/how-to-connect/connect-status.png)

Überprüfen Sie, ob auf der Statusleiste *Dev Spaces: Connected to dev/bikes on local port 3000* (Dev Spaces: Verbunden mit „dev/bikes“ am lokalen Port 3000) angezeigt wird.

### <a name="configure-your-application-on-your-development-machine"></a>Konfigurieren der Anwendung auf dem Entwicklungscomputer

Öffnen Sie das Terminalfenster *AZDS Connect – Bikes*, und führen Sie `npm install` aus:

```console
$ npm install

> fsevents@1.2.9 install ./dev-spaces/samples/BikeSharingApp/Bikes/node_modules/fsevents
> node install
...
```


Klicken Sie auf *Debuggen* und dann auf *Konfigurationen öffnen*. Wenn Sie zur Auswahl einer Umgebung aufgefordert werden, wählen Sie *Node.js* aus. Dadurch wird eine `.vscode/launch.json`-Datei erstellt. Ersetzen Sie den Inhalt dieser Datei durch folgenden Code:

```json
{
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch via NPM",
            "runtimeExecutable": "npm",
            "runtimeArgs": [
                "run-script",
                "debug"
            ],
            "port": 9229
        }
    ]
}
```

Öffnen Sie [package.json](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/package.json), und fügen Sie ein Debugskript hinzu:

```json
  "devDependencies": {
    "nodemon": "^1.18.10"
  },
  "scripts": {
    "debug": "node --nolazy --inspect-brk=9229 server.js"
  }
```

### <a name="start-your-application-on-your-development-machine"></a>Starten der Anwendung auf dem Entwicklungscomputer

Klicken Sie links auf das Symbol *Debuggen* und dann oben auf die Startschaltfläche neben *Über NPM starten*.

![Über NPM starten](../media/how-to-connect/launch-npm.png)

Die Anwendung wird gestartet. Mit Azure Dev Spaces wird der Datenverkehr zwischen dem AKS-Cluster und dem Entwicklungscomputer umgeleitet. In der *Debugging-Konsole* werden Meldungen ähnlich der folgenden angezeigt:

```console
/usr/local/bin/npm run-script debug 
...
Debugger attached.
Collection: bikes
MongoDB connection string: mongodb://databases-mongo
Connected to MongoDB
Listening on port 3000
```

Navigieren Sie zum Dienst *bikesharingweb*, indem Sie auf die Azure Dev Spaces-Statusleiste klicken und die öffentliche URL der Anwendung auswählen. Sie finden die öffentliche URL auch über den Befehl `azds list-uris`, den Sie zuvor ausgeführt haben. Wenn Sie im Cluster nicht Azure Dev Spaces verwenden, verwenden Sie die IP-Adresse oder die URL der Anwendung für den verwendeten Namespace. Im obigen Beispiel lautet die öffentliche URL für den Dienst *bikesharingweb* `http://dev.bikesharingweb.fedcab0987.eus.azds.io/`. Wählen Sie *Aurelia Briggs (customer)* als Benutzer und dann ein zu mietendes Fahrrad aus.

### <a name="set-a-break-point"></a>Festlegen eines Haltepunkts

Öffnen Sie [server.js](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/Bikes/server.js#L233), und klicken Sie auf die Zeile 233, um den Cursor darin zu platzieren. Drücken Sie zum Festlegen eines Haltepunkts die Taste *F9*, oder klicken Sie auf *Debuggen* und dann auf *Haltepunkt umschalten*.

Navigieren Sie zum Dienst *bikesharingweb*, indem Sie die öffentliche URL öffnen. Wählen Sie *Aurelia Briggs (customer)* als Benutzer und dann ein zu mietendes Fahrrad aus. Beachten Sie, dass das Bild für das Fahrrad nicht geladen wird. Wechseln Sie zurück zu Visual Studio Code. Zeile 233 ist hervorgehoben. Durch den festgelegten Haltepunkt wurde der Dienst in Zeile 233 angehalten. Drücken Sie zum Fortsetzen des Diensts *F5*, oder klicken Sie auf *Debuggen* und dann auf *Weiter*. Wechseln Sie wieder zum Browser, und überprüfen Sie, ob ein Platzhalterbild für das Fahrrad angezeigt wird.

Entfernen Sie den Haltepunkt, indem Sie den Cursor in `server.js` in die Zeile 233 setzen und *F9* drücken.

### <a name="update-your-application"></a>Aktualisieren Ihrer Anwendung

Bearbeiten Sie `server.js`, um die Zeilen 232 und 233 zu entfernen:

```javascript
    // Hard code image url *FIX ME*
    theBike.imageUrl = "/static/logo.svg";
```

Der Abschnitt sollte jetzt folgendermaßen aussehen:

```javascript
    var theBike = result;
    theBike.id = theBike._id;
    delete theBike._id;
```

Speichern Sie die Änderungen, und klicken Sie auf *Debuggen* und dann auf *Debugging erneut starten*. Aktualisieren Sie den Browser, und vergewissern Sie sich, dass kein Platzhalterbild für das Fahrrad mehr angezeigt wird.

Klicken Sie auf *Debuggen* und dann auf *Debuggen beenden*, um den Debugger zu beenden. Klicken Sie auf die Azure Dev Spaces-Statusleiste, um die Verbindung mit dem AKS-Cluster zu trennen.

## <a name="additional-configuration"></a>Zusätzliche Konfiguration

Mit Azure Dev Spaces können ohne zusätzliche Konfiguration der Datenverkehr weitergeleitet und Umgebungsvariablen repliziert werden. Wenn Sie Dateien herunterladen möchten, die in den Container im AKS-Cluster eingebunden sind, z. B. eine ConfigMap-Datei, können Sie eine Datei `azds-local.env` erstellen, um diese Dateien auf den Entwicklungscomputer herunterzuladen.

Beispiel für `azds-local.env`:

```
# This downloads the "whitelist" volume from the container,
# saves it to a temporary directory on your development machine,
# and sets the full path to an environment variable called WHITELIST_PATH.

WHITELIST_PATH=${volumes.whitelist}/whitelist

# This downloads a file from the container's 'default-token-<any>' mount directory 
# to /var/run/secrets/kubernetes.io/serviceaccount on your development machine.

KUBERNETES_IN_CLUSTER_CONFIG_OVERWRITE=${volumes.default-token-*|/var/run/secrets/kubernetes.io/serviceaccount}


# This makes the myapp1 service available to your development machine
# regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP1_SERVICE_HOST.

MYAPP1_SERVICE_HOST=${services.myapp1}

# If the myapp1 service is made available in this way, 
# you can also access it using "myapp1" and "myapp1.svc.cluster.local"
# in addition to the IP in the MYAPP1_SERVICE_HOST environment variable.
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie mithilfe von Azure Dev Spaces und GitHub-Aktionen Änderungen eines Pull Request direkt in AKS testen können, bevor der Pull Request im Hauptzweig des Repositorys zusammengeführt wird.

> [!div class="nextstepaction"]
> [GitHub-Aktionen und Azure Kubernetes Service][gh-actions]

[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azure-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[bike-sharing-github]: https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp
[gh-actions]: github-actions.md
[helm-installed]: https://helm.sh/docs/using_helm/#installing-helm
[supported-regions]: ../about.md#supported-regions-and-configurations
[team-quickstart]: ../quickstart-team-development.md
[vs-code]: https://code.visualstudio.com/download