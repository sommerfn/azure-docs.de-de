---
title: Zulässige Zertifizierungsstellen für die Aktivierung von benutzerdefiniertem HTTPS für Azure CDN | Microsoft-Dokumentation
description: Wenn Sie Ihr eigenes Zertifikat zur Aktivierung von HTTPS in einer benutzerdefinierten Domäne verwenden, müssen Sie für die Erstellung eine zulässige Zertifizierungsstelle (CA) verwenden.
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
ms.topic: article
ms.date: 10/18/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 754941163ddce9512870f0b76a96207472e5b2aa
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593357"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Zulässige Zertifizierungsstellen für die Aktivierung von benutzerdefiniertem HTTPS für Azure CDN

Wenn Sie für eine benutzerdefinierte Azure CDN-Domäne (Azure Content Delivery Network) auf einem Endpunkt mit **Azure CDN Standard von Microsoft** die [HTTPS-Funktion durch Verwendung Ihres eigenen Zertifikats aktivieren](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates), müssen Sie für die Erstellung Ihres SSL-Zertifikats eine zulässige Zertifizierungsstelle (CA) verwenden. Bei der Verwendung einer unzulässigen Zertifizierungsstelle oder eines selbstsignierten Zertifikats wird Ihre Anforderung abgelehnt.

> [!NOTE]
> Die Option zum Verwenden Ihres eigenen Zertifikats zur Aktivierung von benutzerdefiniertem HTTPS ist nur für Profile mit **Azure CDN Standard von Microsoft** verfügbar. 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

