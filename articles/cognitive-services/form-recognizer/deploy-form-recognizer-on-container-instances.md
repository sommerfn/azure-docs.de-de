---
title: Ausführen eines Containers für die Formularerkennung in Azure Container Instances
titleSuffix: Azure Cognitive Services
description: Stellen Sie den Container für die Formularerkennung in einer Azure Container Instance bereit, und testen Sie diesen in einem Webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: c45d0e316cf67d22095911fadfd3d84ef407e1b0
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72515637"
---
# <a name="deploy-the-form-recognizer-container-to-azure-container-instances"></a>Bereitstellen eines Containers für die Formularerkennung in Azure Container Instances

Erfahren Sie, wie Sie Container für die [Formularerkennung](form-recognizer-container-howto.md) von Cognitive Services für Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/) bereitstellen. Dieses Verfahren veranschaulicht die Erstellung der Azure-Ressource für die Formularerkennung. Anschließend wird das Abrufen des dazugehörigen Containerimage erläutert. Abschließend erklären wir, wie Sie die Orchestrierung von Ressource und Image über einen Browser ausführen können. Durch die Verwendung von Containern müssen Entwickler sich weniger um die Verwaltung der Infrastruktur kümmern und können ihre Aufmerksamkeit ganz auf die Anwendungsentwicklung lenken.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Anfordern des Zugriffs auf die Containerregistrierung

Sie müssen zuerst das [Formular zum Anfordern des Zugriffs auf Cognitive Services-Container für Formularerkennung](https://aka.ms/FormRecognizerRequestAccess) ausfüllen und senden, um Zugriff auf den Container anzufordern. Damit werden Sie auch für maschinelles Sehen registriert. Sie müssen sich für das Formular zum Anfordern von maschinelles Sehen nicht extra registrieren. 

[!INCLUDE [Request access](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Form Recognizer resource](includes/create-resource.md)]

[!INCLUDE [Create an Form Recognizer container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
