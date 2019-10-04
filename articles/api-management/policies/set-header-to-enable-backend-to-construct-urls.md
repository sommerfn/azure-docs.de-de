---
title: Beispiel für eine Azure API Management-Richtlinie – Hinzufügen eines Forwarded-Headers | Microsoft-Dokumentation
description: Das Beispiel für eine Azure API Management-Richtlinie veranschaulicht, wie Sie einen Forwarded-Header in die eingehende Anforderung einfügen, damit die Back-End-API die richtigen URLs erstellen kann.
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
ms.openlocfilehash: e525029ae8eab086d11126a4e18958423e207aa1
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70067502"
---
# <a name="add-a-forwarded-header"></a>Hinzufügen eines Forwarded-Headers

Dieser Artikel zeigt ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Sie einen Forwarded-Header in die eingehende Anforderung einfügen, damit die Back-End-API die richtigen URLs erstellen kann. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte in [Festlegen oder Bearbeiten von Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [API Management-Richtlinienbeispiele](../policy-samples.md).

## <a name="code"></a>Code

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/examples/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [API Management-Richtlinienbeispiele](../policy-samples.md)
