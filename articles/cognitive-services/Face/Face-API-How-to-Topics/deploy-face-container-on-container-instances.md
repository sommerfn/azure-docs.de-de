---
title: Ausführen des Containers für die Gesichtserkennung in Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Stellen Sie den Container für die Gesichtserkennung in einer Azure Container Instance bereit, und testen Sie diesen in einem Webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 2a596ea1f6f6405078d0e031de00212995342a19
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72516808"
---
# <a name="deploy-the-face-container-to-azure-container-instances"></a>Bereitstellen eines Containers für die Gesichtserkennung in Azure Container Instances

Erfahren Sie, wie Sie Container für die [Gesichtserkennung](../face-how-to-install-containers.md) von Cognitive Services für Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/) bereitstellen. Dieses Verfahren veranschaulicht die Erstellung der Azure-Ressource für die Gesichtserkennung. Anschließend wird das Abrufen des dazugehörigen Containerimage erläutert. Abschließend erklären wir, wie Sie die Orchestrierung von Ressource und Image über einen Browser ausführen können. Die Verwendung von Containern kann die Aufmerksamkeit der Entwickler von der Verwaltung der Infrastruktur auf die Anwendungsentwicklung lenken.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Anfordern des Zugriffs auf die private Containerregistrierung

[!INCLUDE [Request access to private container registry](../../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Create a Cognitive Services Face resource](../includes/create-face-resource.md)]

[!INCLUDE [Create an Face container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]