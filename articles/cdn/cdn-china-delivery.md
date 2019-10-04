---
title: Inhaltsübermittlung nach China mit Azure CDN | Microsoft-Dokumentation
description: Informationen zur Verwendung von Azure Content Delivery Network (CDN) zur Übermittlung von Inhalten an Benutzer in China.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: a63914116f5ef9922ac05745764e0ad505dc3c91
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593946"
---
# <a name="china-content-delivery-with-azure-cdn"></a>Inhaltsübermittlung nach China mit Azure CDN

Azure Content Delivery Network (CDN) global kann Benutzern in China mit Point of Presence-Speicherorten (POP) in der Nähe von China oder einem beliebigen POP, der für Anforderungen aus China die beste Leistung bietet, Inhalte übermitteln. China ist jedoch ein bedeutender Markt für Ihre Kunden, und sie benötigen hohe Leistung – also sollten Sie stattdessen Azure CDN China verwenden.

Azure CDN China unterscheidet sich von Azure CDN global darin, dass Inhalte von POPs in China durch die Partnerschaft mit einer Reihe lokaler Anbieter übermittelt werden. Aufgrund der chinesischen Konformität und Vorschriften müssen Sie ein gesondertes Abonnement zur Verwendung von Azure CDN China registrieren, und Ihre Websites benötigen eine ICP-Lizenz. Die Benutzeroberfläche von Portal und API zum Aktivieren und Verwalten der Übermittlung von Inhalten ist bei Azure CDN global und Azure CDN China identisch.

## <a name="comparison-of-azure-cdn-global-and-azure-cdn-china"></a>Vergleich von Azure CDN global und Azure CDN China

Azure CDN global und Azure CDN China weisen die folgenden Features auf:

- Azure CDN global:

     - Portal: https://portal.azure.com  

     - Führt die Übermittlung von Inhalten außerhalb Chinas durch.

     - Vier Tarife: Microsoft Standard, Verizon Standard, Verizon Premium und Akamai Standard

     - [Documentation](https://docs.microsoft.com/azure/cdn/)

- Azure CDN China:

     - Portal: https://portal.azure.cn

     - Führt die Übermittlung von Inhalten innerhalb von China durch.

     - Zwei Tarife: Standard und Premium

     - [Documentation](https://docs.azure.cn/en-us/cdn/)
 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure CDN China finden Sie unter:

- [Content Delivery Network (CDN)](https://www.azure.cn/en-us/home/features/cdn/)

- [Übersicht zum Azure Content Delivery Network](https://docs.azure.cn/en-us/cdn/cdn-overview)

- [Verwenden von Azure CDN](https://docs.azure.cn/en-us/cdn/cdn-how-to-use)

- [Azure-Dienstverfügbarkeit in China](https://docs.microsoft.com/azure/china/concepts-service-availability)



