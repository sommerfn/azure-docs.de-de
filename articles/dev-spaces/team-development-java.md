---
title: Teamentwicklung mit Azure Dev Spaces mit Java und VS Code
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: stepro
ms.author: stephpr
ms.date: 08/01/2018
ms.topic: tutorial
description: Schnelle Kubernetes-Entwicklung mit Containern und Microservices in Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s '
manager: gwallace
ms.openlocfilehash: 7b4785e1fbcc9f7946526920113b453c0a811c42
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707169"
---
[!INCLUDE [](../../includes/devspaces-team-development-1.md)]

### <a name="make-a-code-change"></a>Vornehmen einer Codeänderung
Wechseln Sie ins VS Code-Fenster für `mywebapi`, und ändern Sie den Code der `String index()`-Methode in `src/main/java/com/ms/sample/mywebapi/Application.java` etwa wie folgt:

```java
@RequestMapping(value = "/", produces = "text/plain")
public String index() {
    return "Hello from mywebapi says something new";
}
```

[!INCLUDE [](../../includes/devspaces-team-development-2.md)]
