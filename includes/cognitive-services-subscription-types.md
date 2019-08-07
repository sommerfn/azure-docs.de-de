---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: 674dd30ff3e493ec4c4036f032f82624a6ca5749
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334228"
---
## <a name="azure-cognitive-service-resource-types"></a>Azure Cognitive Services-Ressourcentypen

> [!NOTE]
> Abonnementbesitzer können die Erstellung einer Cognitive Services-Ressource für Ressourcengruppen und Abonnements durch Anwenden von [Azure-Richtlinien](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition) deaktivieren, indem die Richtliniendefinition „Nicht zulässige Ressourcentypen“ zugewiesen und **Microsoft.CognitiveServices/accounts** als Zielressourcentyp angegeben wird.

Sie können über zwei unterschiedliche Ressourcen auf Azure Cognitive Services zugreifen: Eine Ressource für mehrere Dienste oder eine Ressource für nur einen Dienst. Mit diesen Abonnements können Sie entweder eine Verbindung mit einem einzelnen Dienst oder mit mehreren Diensten auf einmal herstellen.

### <a name="multi-service-resource"></a>Ressource für mehrere Dienste

>[!WARNING]
> Zum derzeitigen Zeitpunkt unterstützen diese Dienste Schlüssel für mehrere Dienste **nicht**: QnA Maker, Speech-Dienste, Custom Vision und Anomalieerkennung.

Abonnieren einer Cognitive Services-Ressource für mehrere Dienste:
* Ermöglicht die Verwendung einer einzelnen Azure-Ressource für einen Großteil von Azure Cognitive Services.
* Konsolidiert die Abrechnung der von Ihnen verwendeten Dienste. Weitere Informationen finden Sie unter [Cognitive Services – Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).

### <a name="single-service-resource"></a>Ressource für einen einzelnen Dienst

Ressourcen für einen einzelnen Dienst (wie maschinelles Sehen oder Speech-Dienste) sind auf den angegebenen Dienst beschränkt.