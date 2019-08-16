---
title: Funktion von Visual Studio Code mit Azure Dev Spaces
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 07/08/2019
ms.topic: conceptual
description: Funktion von Visual Studio Code mit Azure Dev Spaces
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: 7809f0215137b0c473e13143404a031497906ebf
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68725811"
---
# <a name="how-visual-studio-code-works-with-azure-dev-spaces"></a>Funktion von Visual Studio Code mit Azure Dev Spaces

Sie können Visual Studio Code und die [Erweiterung Azure Dev Spaces][azds-extension] verwenden, um Ihre Dienste mit Azure Dev Spaces vorzubereiten, auszuführen und zu debuggen. Mit Visual Studio Code und der Erweiterung Azure Dev Spaces können Sie:

* Ressourcen zum Ausführen und Debuggen von Diensten in AKS generieren
* Ihre Java-, Node.js- und .NET Core-Dienste in einem Entwicklungsbereich ausführen
* Ihre in einem Entwicklungsbereich ausgeführten Java-, Node.js- und .NET Core-Dienste direkt debuggen

## <a name="generate-assets"></a>Ressourcen generieren

Visual Studio Code und die Erweiterung Azure Dev Spaces generieren die folgenden Ressourcen für Ihr Projekt:

* Dockerfiles für Java-Anwendungen, die Maven, Node.js- und .NET Core-Anwendungen verwenden
* Helm-Diagramme für fast jede Sprache mit einer Dockerfile-Datei
* Eine `azds.yaml`-Datei, d.h. die [Azure Dev Spaces-Konfigurationsdatei][azds-yaml] für Ihr Projekt
* Ein `.vscode`-Ordner mit der Visual Studio Code-Startkonfiguration Ihres Projekts für Java-Anwendungen mit Maven, Node.js-Anwendungen und .NET Core-Anwendungen

Die Dockerfile-, Helm-Diagramm- und `azds.yaml`-Dateien sind die gleichen Ressourcen, die bei der Ausführung von `azds prep` generiert werden. Diese Dateien können auch außerhalb von Visual Studio Code verwendet werden, um Ihr Projekt in AKS auszuführen, wie z.B. die Ausführung von `azds up`. Der Ordner `.vscode` wird nur von Visual Studio Code verwendet, um Ihr Projekt von Visual Studio Code aus in AKS auszuführen.

## <a name="run-your-service-in-aks"></a>Ausführen Ihres Diensts in AKS

Nachdem Sie die Ressourcen für Ihr Projekt generiert haben, können Sie Ihre Java-, Node.js- und .NET Core-Dienste in einem vorhandenen Entwicklungsbereich von Visual Studio Code aus ausführen. Auf der Seite *Debuggen* von Visual Studio Code können Sie die Startkonfiguration aus dem `.vscode`-Verzeichnis aufrufen, um Ihr Projekt auszuführen.

Sie müssen Ihren AKS-Cluster erstellen und Azure Dev Spaces in Ihrem Cluster außerhalb von Visual Studio Code aktivieren. Sie können z.B. die Azure-Befehlszeilenschnittstelle oder das Azure-Portal verwenden, um dieses Setup durchzuführen. Sie können vorhandene Dockerfiles, Helm-Diagramme und `azds.yaml`-Dateien, die außerhalb von Visual Studio Code erstellt wurden, wie beispielsweise die durch Ausführung von `azds prep` erzeugten Ressourcen, wiederverwenden. Wenn Sie Ressourcen wiederverwenden, die außerhalb von Visual Studio Code erstellt wurden, benötigen Sie immer noch ein `.vscode`-Verzeichnis. Dieses `.vscode`-Verzeichnis kann durch Visual Studio Code und die Erweiterung Azure Dev Spaces regeneriert werden und überschreibt nicht Ihre vorhandenen Ressourcen.

Für .NET Core-Projekte muss die [C#-Erweiterung][csharp-extension] installiert sein, um Ihren .NET-Dienst von Visual Studio Code aus auszuführen. Auch für Java-Projekte, die Maven verwenden, müssen die [Java-Debugger für Azure Dev Spaces-Erweiterung][java-extension] installiert und [Maven installiert und konfiguriert][maven] sein, damit Sie Ihren Java-Dienst von Visual Studio Code aus ausführen können.

## <a name="debug-your-service-in-aks"></a>Debuggen Ihres Diensts in AKS

Nachdem Sie Ihr Projekt gestartet haben, können Sie Ihre Java-, Node.js- und.NET-Core-Dienste, die in einem Entwicklungsbereich direkt aus Visual Studio Code ausgeführt werden, debuggen. Die Startkonfiguration im Verzeichnis `.vscode` stellt die zusätzlichen Debuginformationen für die Ausführung eines Dienstes mit aktiviertem Debugging in einem Entwicklungsbereich zur Verfügung. Visual Studio Code wird auch dem Debugprozess in dem in Ihrem Entwicklungsbereich ausgeführten Container angefügt, sodass Sie Haltepunkte setzen, Variablen überprüfen und andere Debugoperationen durchführen können.


## <a name="use-visual-studio-code-with-azure-dev-spaces"></a>Verwenden von Visual Studio Code mit Azure Dev Spaces

Sie können sich über den Einsatz von Visual Studio Code mit der Erweiterung Azure Dev Spaces in den folgenden Schnellstarts informieren:

* [Debuggen und Iterieren mit Visual Studio Code und Java in Kubernetes mithilfe von Azure Dev Spaces][quickstart-java]
* [Entwickeln mit .NET Core unter Kubernetes mit Azure Dev Spaces (Visual Studio Code)][quickstart-netcore]
* [Debuggen und Iterieren mit Visual Studio Code und Node.js in Kubernetes mithilfe von Azure Dev Spaces][quickstart-node]

[azds-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azds-yaml]: how-dev-spaces-works.md#prepare-your-code
[csharp-extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp
[java-extension]: https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds
[maven]: https://maven.apache.org
[quickstart-java]: quickstart-java.md
[quickstart-netcore]: quickstart-netcore.md
[quickstart-node]: quickstart-nodejs.md
