---
title: Legacy-Komponenten von Azure Media Services | Microsoft-Dokumentation
description: In diesem Thema werden die Legacy-Komponenten von Azure Media Services behandelt.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2019
ms.author: juliako
ms.openlocfilehash: e4ff157f58a68c68f8610c6c473f5d69897650ad
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71338417"
---
# <a name="azure-media-services-legacy-components"></a>Legacy-Komponenten von Azure Media Services

Im Laufe der Zeit wurden die Media Service-Komponenten kontinuierlich verfeinert und verbessert. Infolgedessen wurden einige Legacy-Komponenten eingestellt. Die Anweisungen zum Migrieren Ihrer Anwendung von der Legacy-Komponente zu einer aktuellen Komponente finden Sie in den folgenden Artikeln.

## <a name="legacy-components-and-migration-guidance"></a>Legacy-Komponenten und Hinweise zur Migration

Wir kündigen die Einstellung der Medienprozessoren *Windows Azure Media Encoder* (WAME) und *Azure Media Encoder* (AME) an. Diese Prozessoren werden am 30. November 2019 eingestellt.

* [Migrieren von Windows Azure Media Encoder zu Media Encoder Standard](migrate-windows-azure-media-encoder.md)
* [Migrieren von Azure Media Encoder zu Media Encoder Standard](migrate-azure-media-encoder.md)

Wir kündigen ferner die Einstellung von *Azure Media Indexer v1* und *Azure Media Indexer v2 Preview* an. Der [Azure Media Indexer v1](media-services-index-content.md)-Medienprozessor wird am 1. Oktober 2020 eingestellt. Die [Azure Media Indexer v2 Preview](media-services-process-content-with-indexer2.md)-Medienprozessoren werden am 1. Januar 2019 eingestellt.  [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) ersetzt diese Legacy-Medienprozessoren.

* [Migration von Azure Media Indexer v1 und Azure Media Indexer v2 zu Azure Media Services Video Indexer](migrate-indexer-v1-v2.md).

## <a name="next-steps"></a>Nächste Schritte

[Hinweise zur Migration von Media Services v2 zu v3](../latest/migrate-from-v2-to-v3.md)
