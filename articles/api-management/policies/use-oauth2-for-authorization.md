---
title: Beispiel für eine Azure API Management-Richtlinie – Verwenden von OAuth2 für die Autorisierung zwischen dem Gateway und einem Back-End | Microsoft-Dokumentation
description: Das Beispiel für eine Azure API Management-Richtlinie veranschaulicht, wie Sie OAuth2 für die Autorisierung zwischen dem Gateway und einem Back-End verwenden. Dieses zeigt, wie Sie ein Zugriffstoken aus AAD abrufen und an das Back-End weiterleiten.
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
ms.openlocfilehash: fac10b728e4b7f09ec1019c3257f7c9e5d6e7714
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70071857"
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>Verwenden von OAuth2 für die Autorisierung zwischen dem Gateway und einem Back-End

Dieser Artikel zeigt ein Beispiel für eine Azure API Management-Richtlinie, das veranschaulicht, wie Sie OAuth2 für die Autorisierung zwischen dem Gateway und einem Back-End verwenden. Dieses zeigt, wie Sie ein Zugriffstoken aus AAD abrufen und an das Back-End weiterleiten. 

Um den Code einer Richtlinie festzulegen oder zu bearbeiten, führen Sie die Schritte unter [Festlegen oder Bearbeiten von Azure API Management-Richtlinien](../set-edit-policies.md) aus. Weitere Beispiele finden Sie unter [API Management-Richtlinienbeispiele](../policy-samples.md).

Das folgende Skript verwendet Eigenschaften, die in {{property}} angezeigt werden. Weitere Informationen über Eigenschaften und deren Verwendung in API Management-Richtlinien finden Sie in [diesem](../api-management-howto-properties.md) Thema.
 
## <a name="policy"></a>Richtlinie

Fügen Sie den Code in den Block **inbound** ein.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get OAuth2 access token from AAD and forward it to the backend.policy.xml)]
  
## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu APIM-Richtlinien:

+ [Transformationsrichtlinien](../api-management-transformation-policies.md)
+ [API Management-Richtlinienbeispiele](../policy-samples.md)

