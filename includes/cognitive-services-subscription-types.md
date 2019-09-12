---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 3b808f4eb853cf05eb08cd1acf08dedccabf71a6
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70274645"
---
## <a name="azure-cognitive-service-resource-types"></a>Azure Cognitive Services-Ressourcentypen

<!-- > [!NOTE]
> Subscription owners can disable the creation of Cognitive Services resources for resource groups and subscriptions by applying [Azure policy](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition), assigning a “Not allowed resource types” policy definition, and specifying **Microsoft.CognitiveServices/accounts** as the target resource type. -->
Sie können über zwei unterschiedliche Ressourcen auf Azure Cognitive Services zugreifen: Eine Ressource für mehrere Dienste oder eine Ressource für nur einen Dienst. Mit diesen Abonnements können Sie entweder eine Verbindung mit einem einzelnen Cognitive Services-Dienst oder mit mehreren Cognitive Services auf einmal herstellen.

### <a name="multi-service-resource"></a>Ressource für mehrere Dienste

Abonnieren einer Cognitive Services-Ressource für mehrere Dienste:
* Ermöglicht die Verwendung einer einzelnen Azure-Ressource für einen Großteil von Azure Cognitive Services.
* Sie erhalten einen einzelnen Schlüssel, der bei mehreren Azure Cognitive Services verwendet werden kann.
* Konsolidiert die Abrechnung der von Ihnen verwendeten Dienste. Weitere Informationen finden Sie unter [Cognitive Services – Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).

>[!WARNING]
> Zum derzeitigen Zeitpunkt unterstützen diese Dienste Schlüssel für mehrere Dienste **nicht**: QnA Maker, Speech-Dienste, Custom Vision und Anomalieerkennung.

### <a name="single-service-resource"></a>Ressource für einen einzelnen Dienst

Abonnieren einer Ressource für einen einzelnen Cognitive Services-Dienst:
* Ermöglicht Ihnen die Verwendung eines angegebenen Cognitive Services-Diensts, für den Sie die Ressource erstellen (z.B. maschinelles Sehen oder Speech-Dienste).
* Sie erhalten einen Schlüssel, der für den Cognitive Services-Dienst spezifisch ist, für den Sie eine Ressource erstellen.