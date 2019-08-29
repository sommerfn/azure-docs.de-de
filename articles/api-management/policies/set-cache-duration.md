---
title: Beispiel für eine Azure API Management-Richtlinie – Festlegen der Cachedauer für Antworten | Microsoft-Dokumentation
description: Das Beispiel für eine Azure API Management-Richtlinie veranschaulicht, wie Sie die Cachedauer für Antworten mit dem maxAge-Wert in dem vom Back-End gesendeten Cache-Control-Header festlegen.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 67d2f334e2088d96543fabd3b99cac71d95630eb
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70071967"
---
# <a name="set-response-cache-duration"></a>Festlegen der Aufbewahrungsdauer von Antworten im Cache

Dieser Artikel zeigt ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Sie die Aufbewahrungsdauer von Antworten im Cache mit dem maxAge-Wert im vom Back-End gesendeten Cache-Control-Header festlegen. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte unter [Festlegen oder Bearbeiten von Azure API Management-Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [API Management-Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/examples/Set cache duration using response cache control header.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [API Management-Richtlinienbeispiele](../policy-samples.md)

