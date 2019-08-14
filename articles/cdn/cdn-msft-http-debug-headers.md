---
title: Debuggen von HTTP-Headern für Azure CDN von Microsoft | Microsoft-Dokumentation
description: Debugcache-Anforderungsheaders stellen zusätzliche Informationen zur Cacherichtlinie bereit, die auf das angeforderte Objekt angewendet wird. Diese Header sind spezifisch für Azure CDN von Microsoft.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2019
ms.author: magattus
ms.openlocfilehash: 297c65c1cd89163b8663819f844dc6c2a83fd1bf
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68814075"
---
# <a name="debug-http-header-for-azure-cdn-from-microsoft"></a>Debuggen von HTTP-Headern für Azure CDN von Microsoft
Der Debugantwortheader (`X-Cache`) stellt Details zu der Ebene des CDN-Stapels bereit, aus der der Inhalt bereitgestellt wurde. Dieser Header ist spezifisch für Azure CDN von Microsoft.

### <a name="response-header-format"></a>Format des Antwortheaders

Header | BESCHREIBUNG
-------|------------
X-Cache: TCP_HIT | Dieser Header wird zurückgegeben, wenn der Inhalt aus dem CDN-Edgecache bereitgestellt wird. 
X-Cache: TCP_REMOTE_HIT | Dieser Header wird zurückgegeben, wenn der Inhalt aus dem regionalen CDN-Cache (Origin Shield-Ebene) bereitgestellt wird.
X-Cache: TCP_MISS | Dieser Header wird zurückgegeben, wenn ein Cachefehler vorliegt und der Inhalt vom Origin bereitgestellt wird. 


