---
title: Ausführen von Azure Container Instances
titleSuffix: Text Analytics - Azure Cognitive Services
description: Stellen Sie die Textanalysecontainer mit dem Bild der Stimmungsanalyse bei den Azure Container Instances bereit, und testen Sie sie in einem Webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: a82b5a1cbed662289d3a406a61fdd19a3ca8861b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454787"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-container-instances-aci"></a>Bereitstellen eines Containers für die Stimmungsanalyse für Azure Container Instances (ACI)

Erfahren Sie, wie Sie den [Textanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers)-Container für Cognitive Services mit dem Stimmungsanalysebild für Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/) (ACI) bereitstellen. Dieses Verfahren veranschaulicht die Erstellung einer Textanalyseressource, die Erstellung eines zugehörigen Stimmungsanalysebilds und die Möglichkeit, diese Orchestrierung der beiden über einen Browser durchzuführen. Die Verwendung von Containern kann die Aufmerksamkeit der Entwickler von der Verwaltung der Infrastruktur auf die Anwendungsentwicklung lenken.

## <a name="prerequisites"></a>Voraussetzungen

* Verwenden Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances (ACI)](../../containers/includes/create-aci-resource.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Nächste Schritte 

* Verwenden weiterer [Cognitive Services-Container](../../cognitive-services-container-support.md)
* Verwenden des [Text Analytics Connected Service](../vs-text-connected-service.md)
