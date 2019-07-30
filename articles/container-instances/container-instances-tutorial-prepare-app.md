---
title: Tutorial – Vorbereiten eines Containerimages für Azure Container Instances
description: Tutorial für Azure Container Instances (Teil 1 von 3) – Vorbereiten einer App in einem Containerimage für die Bereitstellung in Azure Container Instances
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: tutorial
ms.date: 03/21/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 719237f63d387cf56ab7947f8f168e0aa4351376
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325581"
---
# <a name="tutorial-create-a-container-image-for-deployment-to-azure-container-instances"></a>Tutorial: Erstellen eines Containerimages für die Bereitstellung in Azure Container Instances

Azure Container Instances ermöglicht die Bereitstellung von Docker-Containern in der Azure-Infrastruktur ohne Bereitstellung virtueller Computer oder Einführung eines übergeordneten Diensts. In diesem Tutorial verpacken Sie eine kleine Node.js-Webanwendung in einem Containerimage, das mit Azure Container Instances ausgeführt werden kann.

Dieser Artikel ist der erste Teil der Reihe und umfasst Folgendes:

> [!div class="checklist"]
> * Klonen des Anwendungsquellcodes von GitHub
> * Erstellen eines Containerimages aus der Anwendungsquelle
> * Testen der Images in einer lokalen Docker-Umgebung

Im zweiten und dritten Tutorialteil laden Sie eigene Images in Azure Container Registry hoch und stellen sie in Azure Container Instances bereit.

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [container-instances-tutorial-prerequisites](../../includes/container-instances-tutorial-prerequisites.md)]

## <a name="get-application-code"></a>Abrufen von Anwendungscode

Die Beispielanwendung in diesem Tutorial ist eine einfache Web-App, die in [Node.js][nodejs] erstellt wird. Die Anwendung dient als statische HTML-Seite und sieht in etwa wie im folgenden Screenshot aus:

![Tutorial-App im Browser][aci-tutorial-app]

Verwenden Sie Git, um das Repository der Beispielanwendung zu klonen:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

Sie können das [ZIP-Archiv auch direkt von GitHub herunterladen][aci-helloworld-zip].

## <a name="build-the-container-image"></a>Erstellen des Containerimages

Die Dockerfile in der Beispielanwendung veranschaulicht, wie der Container erstellt wird. Ausgangspunkt ist ein [offizielles Node.js-Image][docker-hub-nodeimage] auf der Grundlage von Alpine Linux (einer kleinen Distribution, die sich gut für Container eignet).based on [Alpine Linux][alpine-linux] Anschließend werden die Anwendungsdateien in den Container kopiert und Abhängigkeiten mithilfe des Knotenpaket-Managers installiert, bevor schließlich die Anwendung gestartet wird.

```Dockerfile
FROM node:8.9.3-alpine
RUN mkdir -p /usr/src/app
COPY ./app/ /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Verwenden Sie den Befehl [docker build][docker-build], um das Containerimage zu erstellen, und kennzeichnen Sie es als *aci-tutorial-app*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Die Ausgabe des Befehls [docker build][docker-build] ähnelt dem folgenden (zur besseren Lesbarkeit gekürzten) Beispiel:

```console
$ docker build ./aci-helloworld -t aci-tutorial-app
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.9.3-alpine
8.9.3-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.9.3-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Verwenden Sie den Befehl [docker images][docker-images], um das erstellte Image anzuzeigen:

```bash
docker images
```

Ihr neu erstelltes Image sollte in der Liste angezeigt werden:

```console
$ docker images
REPOSITORY          TAG       IMAGE ID        CREATED           SIZE
aci-tutorial-app    latest    5c745774dfa9    39 seconds ago    68.1 MB
```

## <a name="run-the-container-locally"></a>Lokales Ausführen des Containers

Verwenden Sie vor dem Bereitstellen des Containers in Azure Container Instances den Befehl [docker run][docker-run], um ihn lokal auszuführen und sich zu vergewissern, dass dies funktioniert. Mit dem Switch `-d` können Sie den Container im Hintergrund ausführen, und mit `-p` können Sie einen beliebigen Port auf Ihrem Computer dem Port 80 des Containers zuordnen.

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

In der Ausgabe des Befehls `docker run` wird die ID des ausgeführten Containers angezeigt, wenn der Befehl erfolgreich war:

```console
$ docker run -d -p 8080:80 aci-tutorial-app
a2e3e4435db58ab0c664ce521854c2e1a1bda88c9cf2fcff46aedf48df86cccf
```

Navigieren Sie in Ihrem Browser jetzt zu `http://localhost:8080`, um sicherzustellen, dass der Container ausgeführt wird. Es wird eine Webseite angezeigt, die in etwa wie folgt aussieht:

![Lokales Ausführen der App im Browser][aci-tutorial-app-local]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Containerimage erstellt, das in Azure Container Instances bereitgestellt werden kann, und sich vergewissert, dass die lokale Ausführung möglich ist. Bisher haben Sie Folgendes durchgeführt:

> [!div class="checklist"]
> * Anwendungsquelle von GitHub wurde geklont.
> * Erstellen eines Containerimages aus der Anwendungsquelle
> * Container wurde lokal getestet.

Fahren Sie mit dem nächsten Tutorial der Reihe fort, um sich über das Speichern Ihres Containerimages in Azure Container Registry zu informieren:

> [!div class="nextstepaction"]
> [Übertragen des Images an Azure Container Registry per Pushvorgang](container-instances-tutorial-prepare-acr.md)

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png

<!-- LINKS - External -->
[aci-helloworld-zip]: https://github.com/Azure-Samples/aci-helloworld/archive/master.zip
[alpine-linux]: https://alpinelinux.org/
[docker-build]: https://docs.docker.com/engine/reference/commandline/build/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-hub-nodeimage]: https://store.docker.com/images/node
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[nodejs]: https://nodejs.org

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
