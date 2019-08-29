---
title: Beispiel für eine Azure API Management-Richtlinie – Autorisieren des Zugriffs mithilfe eines Google-OAuth-Tokens | Microsoft-Dokumentation
description: Das Beispiel für eine Azure API Management-Richtlinie veranschaulicht, wie Sie den Zugriff auf Ihre Endpunkte mithilfe von Google als OAuth-Tokenanbieter autorisieren.
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
ms.openlocfilehash: 7acc9071008937cd85c628878b385f1f53707e53
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70071924"
---
# <a name="authorize-access-using-google-oauth-token"></a>Autorisieren des Zugriffs mithilfe von Google-OAuth-Token

Dieser Artikel zeigt ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Sie den Zugriff auf Ihre Endpunkte mithilfe von Google als OAuth-Tokenanbieter autorisieren. Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte unter [Festlegen oder Bearbeiten von Azure API Management-Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [API Management-Richtlinienbeispiele](../policy-samples.md).

## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/examples/Simple Google OAuth validate-jwt.policy.xml)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [API Management-Richtlinienbeispiele](../policy-samples.md)

