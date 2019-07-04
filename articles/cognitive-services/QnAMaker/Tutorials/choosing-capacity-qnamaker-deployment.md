---
title: 'Ressourcenkapazität für die Bereitstellung: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Bevor Sie Ihren QnA Maker-Dienst erstellen, sollten Sie sich entscheiden, welcher Tarif der oben aufgeführten Dienste für Ihre Zwecke geeignet ist.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/24/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 3126613a9dca00d221610dc46116dd409c65d53d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446629"
---
# <a name="choosing-capacity-for-your-qna-maker-deployment"></a>Wählen der Kapazität für Ihre QnA Maker-Bereitstellung

Der QnA Maker-Dienst weist Abhängigkeiten von drei Azure-Ressourcen auf:
1.  App Service (für die Laufzeit)
2.  Azure Search (für das Speichern von Fragen und Antworten)
3.  App Insights (optional, zum Speichern von Chatprotokollen und Telemetriedaten)

Bevor Sie Ihren QnA Maker-Dienst erstellen, sollten Sie sich entscheiden, welcher Tarif der oben aufgeführten Dienste für Ihre Zwecke geeignet ist. 

Es gibt in der Regel drei Parameter, die Sie berücksichtigen müssen:

1. **Den Durchsatz, den Sie vom Dienst benötigen:** Wählen Sie den passenden [App-Plan](https://azure.microsoft.com/pricing/details/app-service/plans/) für Ihren App Service auf der Grundlage Ihrer Anforderungen aus. Sie können die App [zentral hochskalieren](https://docs.microsoft.com/azure/app-service/web-sites-scale) oder zentral herunterskalieren. Dies sollte auch Einfluss auf Ihre Wahl der Azure Search SKU haben; weitere Details dazu finden Sie [hier](https://docs.microsoft.com/azure/search/search-sku-tier).

1. **Größe und Anzahl von Wissensdatenbanken:** Wählen Sie eine geeignete [Azure Search-SKU](https://azure.microsoft.com/pricing/details/search/) für Ihr Szenario aus. Sie können N-1 Wissensdatenbanken in einem bestimmten Tarif veröffentlichen, wobei N die Anzahl der im Tarif maximal zulässigen Indizes ist. Überprüfen Sie außerdem die maximale Größe und Anzahl der in den einzelnen Tarifen zulässigen Dokumente.

    Wenn Ihr Tarif beispielsweise 15 zulässige Indizes aufweist, können Sie 14 Wissensdatenbanken veröffentlichen (1 Index pro veröffentlichter Wissensdatenbank). Der fünfzehnte Index wird für alle Wissensdatenbanken zum Erstellen und Testen verwendet. 

1. **Anzahl von Dokumenten als Quellen:** Die kostenlose SKU des QnA Maker-Verwaltungsdiensts begrenzt die Anzahl der Dokumente, die über das Portal und die APIs verwaltet werden können, auf 3 (bei einer Größe von jeweils 1 MB). Die Standard-SKU ist in der Anzahl der Dokumente, die Sie verwalten können, nicht begrenzt. Ausführlichere Informationen finden Sie [hier](https://aka.ms/qnamaker-pricing).

Die folgende Tabelle gibt Ihnen einige allgemeine Richtlinien.

|                        | QnA Maker Management | App Service | Azure Search | Einschränkungen                      |
| ---------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| Experimentieren        | Kostenlose SKU             | Free-Tarif   | Free-Tarif    | Veröffentlichen von bis zu 2 KB bei einer Größe von 50 MB  |
| Entwicklungs-/Testumgebung   | Standard-SKU         | Shared      | Basic        | Veröffentlichen von bis zu 14 KB bei einer Größe von 2 GB    |
| Produktionsumgebung | Standard-SKU         | Basic       | Standard     | Veröffentlichen von bis zu 49 KB bei einer Größe von 25 GB |

Informationen zum Ausführen von Upgrades für Ihren QnA Maker-Stapel finden Sie unter [Upgrade Ihres QnA Maker-Diensts](../How-To/upgrade-qnamaker-service.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Upgrade Ihres QnA Maker-Diensts](../How-To/upgrade-qnamaker-service.md)
