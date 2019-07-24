---
title: Ausführen von Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Stellen Sie den Container für den Speech-Dienst in einer Azure Container Instance bereit, und testen Sie diesen in einem Webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 062765be22135b12abb29ff6f7ce8a772c67adae
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711259"
---
# <a name="deploy-the-speech-service-container-to-azure-container-instances"></a>Bereitstellen eines Containers für den Speech-Dienst in Azure Container Instances

Erfahren Sie, wie Sie Container für den [Speech-Dienst](speech-container-howto.md) von Cognitive Services in Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/) bereitstellen. Dieses Verfahren veranschaulicht die Erstellung einer Azure-Ressource für den Speech-Dienst. Anschließend wird das Abrufen des dazugehörigen Containerimages erläutert. Abschließend erklären wir, wie Sie die Orchestrierung von Ressource und Image über einen Browser ausführen können. Durch die Verwendung von Containern müssen Entwickler sich weniger um die Verwaltung der Infrastruktur kümmern und können ihre Aufmerksamkeit ganz auf die Anwendungsentwicklung lenken.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Anfordern des Zugriffs auf die Containerregistrierung

Sie müssen zuerst das [Formular zum Anfordern von Cognitive Services-Speech-Containern](https://aka.ms/speechcontainerspreview/) ausfüllen und übermitteln, um Zugriff auf den Container anzufordern. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Speech Service resource](includes/create-speech-resource.md)]

[!INCLUDE [Create an Speech Service container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
