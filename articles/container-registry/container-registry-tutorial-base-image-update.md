---
title: 'Tutorial: Auslösen der Imageerstellung bei der Aktualisierung eines Basisimages – Azure Container Registry'
description: In diesem Tutorial erfahren Sie, wie Sie eine Azure Container Registry-Aufgabe so konfigurieren, dass automatisch Buildvorgänge für Containerimages in der Cloud ausgelöst werden, wenn ein Basisimage aktualisiert wird.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 1beb66d8491b7dac84e9531558f8967e22086575
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73931686"
---
# <a name="tutorial-automate-container-image-builds-when-a-base-image-is-updated-in-an-azure-container-registry"></a>Tutorial: Automatisieren von Buildvorgängen für Containerimages nach der Aktualisierung eines Basisimages in einer Azure-Containerregistrierung 

ACR Tasks unterstützt das automatisierte Ausführen eines Buildvorgangs, wenn das Basisimage eines Containers aktualisiert wird – beispielsweise, wenn Sie das Betriebssystem oder das Anwendungsframework in einem Ihrer Basisimages patchen. In diesem Tutorial erfahren Sie, wie Sie eine Aufgabe in ACR Tasks erstellen, die einen Buildvorgang in der Cloud auslöst, wenn das Basisimage eines Containers an Ihre Registrierung gepusht wurde.

Dieser letzte Teil der Tutorialreihe umfasst Folgendes:

> [!div class="checklist"]
> * Erstellen des Basisimages
> * Erstellen einer Buildaufgabe für ein Anwendungsimage
> * Aktualisieren des Basisimages, um eine Aufgabe für das Anwendungsimage auszulösen
> * Anzeigen der ausgelösten Aufgabe
> * Überprüfen des aktualisierten Anwendungsimages

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die Azure CLI lokal verwenden möchten, muss bei Ihnen mindestens die Azure CLI-Version **2.0.46** installiert sein. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Installieren und Aktualisieren der Befehlszeilenschnittstelle finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli].

## <a name="prerequisites"></a>Voraussetzungen

### <a name="complete-the-previous-tutorials"></a>Arbeiten Sie die vorhergehenden Tutorials durch.

In diesem Tutorial wird vorausgesetzt, dass Sie bereits die Schritte aus den ersten beiden Tutorials der Reihe absolviert haben:

* Erstellen einer Azure-Containerregistrierung
* Forken des Beispielrepositorys
* Klonen des Beispielrepositorys
* Erstellen eines persönlichen GitHub-Zugriffstokens

Absolvieren Sie bei Bedarf die ersten beiden Tutorials, bevor Sie mit diesem Tutorial fortfahren:

[Erstellen von Containerimages in der Cloud mit Azure Container Registry Tasks](container-registry-tutorial-quick-task.md)

[Automatisieren von Buildvorgängen für Containerimages mit Azure Container Registry Tasks](container-registry-tutorial-build-task.md)

### <a name="configure-the-environment"></a>Konfigurieren der Umgebung

Geben Sie für die folgenden Shell-Umgebungsvariablen geeignete Werte für Ihre Umgebung an. Dieser Schritt ist zwar nicht zwingend erforderlich, vereinfacht aber das Ausführen der mehrzeiligen Azure CLI-Befehle in diesem Tutorial. Wenn Sie diese Umgebungsvariablen nicht angeben, müssen Sie sie später jedes Mal manuell ersetzen, wenn sie in einem der Beispielbefehle vorkommen.

```azurecli-interactive
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the second tutorial
```

## <a name="base-images"></a>Basisimages

Die meisten Containerimages werden mithilfe von Dockerfiles definiert. Dockerfiles geben als Basis ein übergeordnetes Image an, das häufig als *Basisimage* bezeichnet wird. Basisimages enthalten in der Regel das Betriebssystem (also beispielsweise [Alpine Linux][base-alpine] oder [Windows Nano Server][base-windows]), auf das die restlichen Ebenen des Containers angewendet werden. Sie können auch Anwendungsframeworks wie [Node.js][base-node] oder [.NET Core][base-dotnet] enthalten.

### <a name="base-image-updates"></a>Basisimageaktualisierungen

Ein Basisimage wird häufig von der für das Image zuständigen Person aktualisiert, um neue Features oder Verbesserungen für das enthaltene Betriebssystem oder Framework zu integrieren. Ein weiterer häufiger Grund für die Aktualisierung eines Basisimages sind Sicherheitspatches.

Wenn ein Basisimage aktualisiert wird, müssen sämtliche darauf basierende Containerimages in Ihrer Registrierung neu erstellt werden, um die neuen Features und Korrekturen zu integrieren. ACR Tasks ermöglicht die automatische Erstellung von Images nach der Aktualisierung eines Containerbasisimages.

### <a name="tasks-triggered-by-a-base-image-update"></a>Aufgaben, die durch eine Aktualisierung des Basisimages ausgelöst werden

* Bei Imagebuilds aus einem Dockerfile erkennt ein ACR-Task Abhängigkeiten von Basisimages an den folgenden Speicherorten:

  * Dieselbe Azure-Containerregistrierung, in der der Task ausgeführt wird
  * Eine andere Azure-Containerregistrierung in derselben Region 
  * Ein öffentliches Repository in Docker Hub 
  * Ein öffentliches Repository in Microsoft Container Registry

   Wenn sich das in der Anweisung `FROM` angegebene Basisimage in einem dieser Speicherorte befindet, fügt die ACR-Aufgabe einen Hook hinzu, um sicherzustellen, dass das Image nach jeder Aktualisierung seiner Basis neu erstellt wird.

* Derzeit verfolgt ein ACR-Task nur Basisimageaktualisierungen für Anwendungsimages (*Runtime*) nach. Basisimageaktualisierungen für Zwischenimages (*Buildzeit*), die in mehrstufigen Dockerfiles verwendet werden, werden nicht nachverfolgt.  

* Wenn Sie eine ACR-Aufgabe mit dem Befehl [az acr task create][az-acr-task-create] erstellen, wird sie standardmäßig für die Auslösung bei einer Aktualisierung des Basisimages *aktiviert*. Das heißt, die `base-image-trigger-enabled`-Eigenschaft wird auf TRUE festgelegt. Wenn Sie dieses Verhalten in einer Aufgabe deaktivieren möchten, ändern Sie den Wert der Eigenschaft in FALSE. Führen Sie beispielsweise den folgenden Befehl [az acr task update][az-acr-task-update] aus:

  ```azurecli
  az acr task update --myregistry --name mytask --base-image-trigger-enabled False
  ```

* Damit eine ACR-Aufgabe die Abhängigkeiten eines Containerimages (einschließlich des Basisimages) bestimmen und nachverfolgen kann, müssen Sie zunächst die Aufgabe **mindestens einmal** auslösen. Lösen Sie z. B. die Aufgabe mit dem Befehl [az acr task run][az-acr-task-run] manuell aus.

* Damit eine Aufgabe bei der Aktualisierung des Basisimages ausgelöst wird, muss das Basisimage ein *stabiles* Tag enthalten, z. B. `node:9-alpine`. Dieses Tagging ist typisch für ein Basisimage, das mit Betriebssystem- und Frameworkpatches auf eine aktuelle stabile Version aktualisiert wird. Wenn das Basisimage mit einem neuen Versionstag aktualisiert wird, wird keine Aufgabe ausgelöst. Weitere Informationen zur Imagemarkierung finden Sie in der [Anleitung zu bewährten Methoden](container-registry-image-tag-version.md). 

### <a name="base-image-update-scenario"></a>Aktualisierungsszenario für das Basisimage

In diesem Tutorial durchlaufen Sie ein Aktualisierungsszenario für ein Basisimage. Das [Codebeispiel][code-sample] enthält zwei Dockerfiles: ein Anwendungsimage und ein als Basis angegebenes Image. In den folgenden Abschnitten erstellen Sie eine ACR-Aufgabe, die automatisch einen Buildvorgang für das Anwendungsimage auslöst, wenn eine neue Version des Basisimages an die gleiche Containerregistrierung gepusht wird.

[Dockerfile-app:][dockerfile-app] Eine kleine Node.js-Webanwendung, die eine statische Webseite rendert, auf der die zugrunde liegende Node.js-Version angezeigt wird. Die Versionszeichenfolge wird simuliert: Sie zeigt den Inhalt der im Basisimage definierten Umgebungsvariablen `NODE_VERSION` an.

[Dockerfile-base:][dockerfile-base] Das Image, das von `Dockerfile-app` als Basis angegeben wird. Es basiert auf einem [Node][base-node]-Image und enthält die Umgebungsvariable `NODE_VERSION`.

In den folgenden Abschnitten erstellen Sie eine Aufgabe, aktualisieren den Wert `NODE_VERSION` in der Dockerfile des Basisimages und erstellen anschließend das Basisimage mithilfe von ACR Tasks. Wenn die ACR-Aufgabe das neue Basisimage an Ihre Registrierung pusht, wird automatisch ein Buildvorgang für das Anwendungsimage ausgelöst. Außerdem können Sie das Containerimage optional lokal ausführen, um sich die verschiedenen Versionszeichenfolgen in den erstellten Images anzusehen.

In diesem Tutorial erstellt und pusht die ACR-Aufgabe ein Anwendungscontainerimage, das in einem Dockerfile angegeben ist. ACR Tasks kann mithilfe einer YAML-Datei, in der Schritte zum Erstellen, Pushen und optional zum Testen mehrerer Container definiert werden, auch [Aufgaben mit mehreren Schritten](container-registry-tasks-multi-step.md) ausführen.

## <a name="build-the-base-image"></a>Erstellen des Basisimages

Erstellen Sie zunächst das Basisimage mit einer *Schnellaufgabe* von ACR Tasks. Wie im [ersten Tutorial](container-registry-tutorial-quick-task.md) der Reihe bereits erläutert, wird dabei das Image erstellt und an Ihre Containerregistrierung gepusht, sofern der Buildvorgang erfolgreich war.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

## <a name="create-a-task"></a>Erstellt eine Aufgabe.

Erstellen Sie als Nächstes mit [az acr task create][az-acr-task-create] eine Aufgabe:

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name taskhelloworld \
    --image helloworld:{{.Run.ID}} \
    --arg REGISTRY_NAME=$ACR_NAME.azurecr.io \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --file Dockerfile-app \
    --git-access-token $GIT_PAT
```

> [!IMPORTANT]
> Wenn Sie in der Vorschauversion bereits Aufgaben mit dem Befehl `az acr build-task` erstellt haben, müssen diese Aufgaben mit dem Befehl [az acr task][az-acr-task] neu erstellt werden.

Diese Aufgabe ähnelt der Schnellaufgabe, die Sie im [vorherigen Tutorial](container-registry-tutorial-build-task.md) erstellt haben. Sie weist ACR Tasks an, einen Buildvorgang für ein Image auszulösen, wenn Commits an das durch `--context` angegebene Repository gepusht werden. Während das im vorherigen Tutorial zum Erstellen des Images verwendete Dockerfile ein öffentliches Basisimage angibt (`FROM node:9-alpine`), gibt das Dockerfile [Dockerfile-app][dockerfile-app] in dieser Aufgabe ein Basisimage in der gleichen Registrierung an:

```Dockerfile
FROM ${REGISTRY_NAME}/baseimages/node:9-alpine
```

Mit dieser Konfiguration ist es einfach, weiter unten in diesem Tutorial einen Frameworkpatch im Basisimage zu simulieren.

## <a name="build-the-application-container"></a>Erstellen des Anwendungscontainers

Verwenden Sie [az acr task run][az-acr-task-run], um die Aufgabe manuell auszulösen und das Anwendungsimage zu erstellen. Mit diesem Schritt wird sichergestellt, dass die Aufgabe die Abhängigkeit des Anwendungsimages vom Basisimage nachverfolgt.

```azurecli-interactive
az acr task run --registry $ACR_NAME --name taskhelloworld
```

Notieren Sie sich nach Abschluss der Aufgabe die **Ausführungs-ID** (beispielsweise „da6“), falls Sie den folgenden optionalen Schritt ausführen möchten.

### <a name="optional-run-application-container-locally"></a>Optional: Lokales Ausführen des Anwendungscontainers

Wenn Sie lokal (also nicht in Cloud Shell) arbeiten und Docker installiert ist, können Sie vor der Neuerstellung des Basisimages den Container ausführen, um die Anwendung in einem Webbrowser anzuzeigen. Bei Verwendung von Cloud Shell können Sie diesen Abschnitt überspringen. (Cloud-Shell unterstützt weder `az acr login` noch `docker run`.)

Authentifizieren Sie sich zunächst mit [az acr login][az-acr-login] bei der Containerregistrierung:

```azurecli
az acr login --name $ACR_NAME
```

Verwenden Sie nun `docker run`, um den Container lokal auszuführen. Ersetzen Sie **\<run-id\>** durch die Ausführungs-ID aus der Ausgabe des vorherigen Schritts (beispielsweise „da6“). Dieses Beispiel legt `myapp` als Namen für den Container fest und fügt den Parameter `--rm` ein, damit der Container entfernt wird, wenn Sie ihn beenden.

```bash
docker run -d -p 8080:80 --name myapp --rm $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Navigieren Sie in Ihrem Browser zu `http://localhost:8080`. Daraufhin sollte eine Webseite mit der Node.js-Versionsnummer angezeigt werden, wie im folgenden Beispiel zu sehen. In einem späteren Schritt passen Sie die Version an, indem Sie der Versionszeichenfolge ein „a“ hinzufügen.

![Screenshot der im Browser gerenderten Beispielanwendung][base-update-01]

Führen Sie den folgenden Befehl aus, um den Container zu beenden und zu entfernen:

```bash
docker stop myapp
```

## <a name="list-the-builds"></a>Auflisten der Builds

Führen Sie als Nächstes den Befehl [az acr task list-runs][az-acr-task-list-runs] aus, um eine Liste der Aufgabenausführungen anzuzeigen, die ACR Tasks für Ihre Registrierung abgeschlossen hat:

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Wenn Sie das vorherige Tutorial absolviert (und die Registrierung nicht gelöscht) haben, sollte die Ausgabe in etwa wie im folgenden Beispiel aussehen. Notieren Sie sich die Anzahl der Aufgabenausführungen und die letzte Ausführungs-ID, um die Angaben nach der Aktualisierung des Basisimages im nächsten Abschnitt vergleichen zu können.

```console
$ az acr task list-runs --registry $ACR_NAME --output table

RUN ID    TASK            PLATFORM    STATUS     TRIGGER     STARTED               DURATION
--------  --------------  ----------  ---------  ----------  --------------------  ----------
da6       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual      2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit  2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual      2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual      2018-09-17T22:29:59Z  00:00:57
```

## <a name="update-the-base-image"></a>Aktualisieren der Basisimages

Hier simulieren Sie einen Frameworkpatch im Basisimage. Bearbeiten Sie **Dockerfile-base**, indem Sie die in `NODE_VERSION` definierte Versionsnummer um ein „a“ ergänzen:

```Dockerfile
ENV NODE_VERSION 9.11.2a
```

Führen Sie eine Schnellaufgabe aus, um das geänderte Basisimage zu erstellen. Notieren Sie sich die **Ausführungs-ID** aus der Ausgabe.

```azurecli-interactive
az acr build --registry $ACR_NAME --image baseimages/node:9-alpine --file Dockerfile-base .
```

Sobald die ACR-Aufgabe das neue Basisimage nach Abschluss des Buildvorgangs an Ihre Registrierung gepusht hat, wird ein Buildvorgang für das Anwendungsimage ausgelöst. Es dauert möglicherweise einen Moment, bis die zuvor erstellte Aufgabe den Buildvorgang für das Anwendungsimage auslöst, da das neue und gepushte Basisimage erkannt werden muss.

## <a name="list-updated-build"></a>Auflisten des aktualisierten Builds

Listen Sie nach der Aktualisierung des Basisimages erneut Ihre Aufgabenausführungen auf, und vergleichen Sie sie mit der vorherigen Liste. Sollte sich die Ausgabe nicht unterscheiden, führen Sie den Befehl in regelmäßigen Abständen erneut aus, bis die neue Aufgabenausführung in der Liste angezeigt wird.

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

Die Ausgabe sieht in etwa wie folgt aus. Als „TRIGGER“ (Auslöser) für den zuletzt ausgeführten Build sollte „Image Update“ (Imageaktualisierung) angegeben sein, was bedeutet, dass die Aufgabe durch die Schnellaufgabe des Basisimages ausgelöst wurde.

```console
$ az acr task list-runs --registry $ACR_NAME --output table

Run ID    TASK            PLATFORM    STATUS     TRIGGER       STARTED               DURATION
--------  --------------  ----------  ---------  ------------  --------------------  ----------
da8       taskhelloworld  Linux       Succeeded  Image Update  2018-09-17T23:11:50Z  00:00:33
da7                       Linux       Succeeded  Manual        2018-09-17T23:11:27Z  00:00:35
da6       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T23:07:22Z  00:00:38
da5                       Linux       Succeeded  Manual        2018-09-17T23:06:33Z  00:00:31
da4       taskhelloworld  Linux       Succeeded  Git Commit    2018-09-17T23:03:45Z  00:00:44
da3       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:55:35Z  00:00:35
da2       taskhelloworld  Linux       Succeeded  Manual        2018-09-17T22:50:59Z  00:00:32
da1                       Linux       Succeeded  Manual        2018-09-17T22:29:59Z  00:00:57
```

Wenn Sie im folgenden optionalen Schritt den neu erstellten Container ausführen möchten, um die aktualisierte Versionsnummer anzuzeigen, notieren Sie sich den Wert unter **RUN ID** für den durch die Imageaktualisierung ausgelösten Buildvorgang („da8“ in der obigen Ausgabe).

### <a name="optional-run-newly-built-image"></a>Optional: Ausführen des neu erstellten Images

Wenn Sie lokal (also nicht in Cloud Shell) arbeiten und Docker installiert ist, können Sie nach Abschluss des Buildvorgangs das neue Anwendungsimage ausführen. Ersetzen Sie `<run-id>` durch die Ausführungs-ID aus dem vorherigen Schritt. Bei Verwendung von Cloud Shell können Sie diesen Abschnitt überspringen. (Cloud-Shell unterstützt `docker run` nicht.)

```bash
docker run -d -p 8081:80 --name updatedapp --rm $ACR_NAME.azurecr.io/helloworld:<run-id>
```

Navigieren Sie in Ihrem Browser zu http://localhost:8081. Daraufhin sollte die Webseite mit der aktualisierten Node.js-Versionsnummer (mit hinzugefügtem „a“) angezeigt werden:

![Screenshot der im Browser gerenderten Beispielanwendung][base-update-02]

Beachten Sie, dass Sie Ihr **Basisimage** mit einer neuen Versionsnummer aktualisiert haben und das zuletzt erstellte **Anwendungsimage** die neue Version anzeigt. ACR Tasks hat die Änderung aus dem Basisimage übernommen und Ihr Anwendungsimage automatisch neu erstellt.

Führen Sie den folgenden Befehl aus, um den Container zu beenden und zu entfernen:

```bash
docker stop updatedapp
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Führen Sie die folgenden Befehle aus, um alle Ressourcen zu entfernen, die Sie im Rahmen dieser Tutorialreihe erstellt haben (einschließlich der Containerregistrierung, der Containerinstanz, des Schlüsseltresors und des Dienstprinzipals):

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie mithilfe einer Aufgabe automatisch Buildvorgänge für Containerimages auslösen, wenn das Basisimage des Images aktualisiert wurde. Der nächste Artikel enthält Informationen zur Authentifizierung für Ihre Containerregistrierung.

> [!div class="nextstepaction"]
> [Authentifizieren mit einer privaten Docker-Containerregistrierung](container-registry-authentication.md)

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[code-sample]: https://github.com/Azure-Samples/acr-build-helloworld-node
[dockerfile-app]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-app
[dockerfile-base]: https://github.com/Azure-Samples/acr-build-helloworld-node/blob/master/Dockerfile-base

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-update]: /cli/azure/acr/task#az-acr-task-update
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-task-list-runs]: /cli/azure/acr
[az-acr-task]: /cli/azure/acr

<!-- IMAGES -->
[base-update-01]: ./media/container-registry-tutorial-base-image-update/base-update-01.png
[base-update-02]: ./media/container-registry-tutorial-base-image-update/base-update-02.png
