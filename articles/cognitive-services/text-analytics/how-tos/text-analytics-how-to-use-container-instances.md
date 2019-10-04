---
title: 'Ausführen von Azure Container Instances: Textanalyse'
titleSuffix: Azure Cognitive Services
description: Stellen Sie die Textanalysecontainer in Azure Container Instances bereit, und testen Sie sie in einem Webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: dapine
ms.openlocfilehash: 35812ec4585e3189282c5acf0aa09d309c33c7ed
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051560"
---
# <a name="deploy-a-text-analytics-container-to-azure-container-instances"></a>Bereitstellen eines Textanalysecontainers in Azure Container Instances

Hier erfahren Sie, wie Sie die Cognitive Services-Container für die [Textanalyse][install-and-run-containers] in [Azure Container Instances][container-instances] bereitstellen. Dieses Verfahren veranschaulicht die Erstellung einer Textanalyseressource, die Erstellung eines zugehörigen Standpunktanalysebilds und die Möglichkeit, diese Orchestrierung der beiden über einen Browser durchzuführen. Die Verwendung von Containern kann die Aufmerksamkeit der Entwickler von der Verwaltung der Infrastruktur auf die Anwendungsentwicklung lenken.

## <a name="prerequisites"></a>Voraussetzungen

* Verwenden Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

#### <a name="key-phrase-extractiontabkeyphrase"></a>[Schlüsselbegriffserkennung](#tab/keyphrase)

[!INCLUDE [Verify the Key Phrase Extraction container instance](../includes/verify-key-phrase-extraction-container.md)]

#### <a name="language-detectiontablanguage"></a>[Sprachenerkennung](#tab/language)

[!INCLUDE [Verify the Language Detection container instance](../includes/verify-language-detection-container.md)]

#### <a name="sentiment-analysistabsentiment"></a>[Standpunktanalyse](#tab/sentiment)

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

***

## <a name="next-steps"></a>Nächste Schritte 

* Verwenden weiterer [Cognitive Services-Container](../../cognitive-services-container-support.md)
* Verwenden des [Text Analytics Connected Service](../vs-text-connected-service.md)

[install-and-run-containers]: ./text-analytics-how-to-install-containers.md
[container-instances]: https://docs.microsoft.com/azure/container-instances