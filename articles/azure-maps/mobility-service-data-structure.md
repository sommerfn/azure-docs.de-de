---
title: Mobility Service-Datenstrukturen in Azure Maps | Microsoft-Dokumentation
description: Datenstrukturen in Azure Maps Mobility Service
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 29e8a9d7555ca836b6266879f3b3c1e32ffd3980
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735867"
---
# <a name="data-structures-in-azure-maps-mobility-service"></a>Datenstrukturen in Azure Maps Mobility Service

In diesem Artikel werden das Stadtbereichskonzept in [Azure Maps Mobility Service](https://aka.ms/AzureMapsMobilityService) sowie einige der allgemeinen Felder behandelt, die über die Dienste zurückgegeben werden, wenn Haltestellen und Linien öffentlicher Verkehrsmittel abgefragt werden. Es empfiehlt sich, diesen Artikel zu lesen, bevor Sie die Mobility Service-APIs verwenden. Die allgemeinen Felder werden weiter unten erläutert.

## <a name="metro-area"></a>Stadtbereich

Mobility Service-Daten werden in unterstützte Stadtbereiche unterteilt. Stadtbereiche basieren nicht auf Stadtgrenzen. Ein Stadtbereich kann mehrere Städte umfassen – etwa eine dicht besiedelte Stadt und die umliegenden Städte. Auch ein Land/eine Region kann ein Stadtbereich sein. 

`metroID` ist die ID eines Stadtbereichs, die zum Aufrufen der [API zum Abrufen von Stadtbereichsinformationen](https://aka.ms/AzureMapsMobilityMetroAreaInfo) verwendet werden kann, um unterstützte Arten von öffentlichen Verkehrsmitteln bzw. Routen sowie zusätzliche Details für den Stadtbereich anzufordern. Hierzu zählen beispielsweise entsprechende Anbieter und aktive Warnungen. Mithilfe der Azure Maps-API zum Abrufen von Stadtbereichsinformationen können Sie die unterstützten Stadtbereiche und Stadt-IDs (metroIDs) anfordern. IDs für den Stadtbereichs können sich ändern.

**metroID:** 522   **Name:** Seattle-Tacoma-Bellevue

![Seattle-metro-area](./media/mobility-service-data-structure/seattle-metro.png)

## <a name="stop-ids"></a>Haltestellen-IDs

Auf Haltestellen kann durch zwei Arten von IDs verwiesen werden: durch die als „stopKey“ bezeichnete [GFTS-ID](https://gtfs.org/) (General Transit Feed Specification) sowie durch die als „stopId“ bezeichnete Azure Maps-Haltestellen-ID. Bei Haltestellenverweisen über einen längeren Zeitraum empfiehlt sich die Verwendung der Azure Maps-Haltestellen-ID, da diese ID beständiger ist und voraussichtlich so lange unverändert bleibt, wie die physische Haltestelle vorhanden ist. Die GTFS-Haltestellen-ID wird häufiger aktualisiert – beispielsweise, wenn eine Änderung durch den GTFS-Anbieter vorgenommen oder eine neue GTFS-Version veröffentlicht wird, obwohl sich an der physischen Haltestelle nichts geändert hat.

Zum Einstieg können Sie mithilfe der [API zum Abrufen nahegelegener öffentlicher Verkehrsmittel](https://aka.ms/AzureMapsMobilityNearbyTransit) in der Nähe befindliche Haltestellen anfordern.

## <a name="line-groups-and-lines"></a>Liniengruppen und Linien

Zur besseren Verarbeitung von Änderungen, die vom [GTFS](https://gtfs.org/)-Datenmodell für Routen und Fahrten geerbt wurden, verwendet Mobility Service ein paralleles Datenmodell für Linien und Liniengruppen.


### <a name="line-groups"></a>Liniengruppen

Eine Liniengruppe ist eine Entität, in der alle Linien zusammengefasst werden, die logisch zur gleichen Gruppe gehören. Eine Liniengruppe enthält in der Regel zwei Linien: eine von Punkt A zu Punkt B und eine von Punkt B zu Punkt A. Beide gehören zum gleichen öffentlichen Verkehrsanbieter und haben die gleiche Liniennummer. Eine Liniengruppe kann aber auch mehr als zwei Linien oder nur eine einzelne Linie enthalten.


### <a name="lines"></a>Linien

Wie oben bereits erwähnt, setzt sich jede Liniengruppe aus einer Gruppe von Linien zusammen. Die einzelnen Linien beschreiben häufig jeweils eine Richtung, und jede Liniengruppe umfasst zwei Linien. Eine Liniengruppe kann aber auch mehrere Linien umfassen – etwa, wenn eine Linie manchmal durch ein bestimmtes Viertel führt, manchmal aber nicht, und in beiden Fällen die gleiche Liniennummer verwendet wird. Und auch Liniengruppen mit einer einzelnen Linie sind möglich (beispielsweise eine ringförmige Linie mit nur einer Richtung).

Zum Einstieg können Sie mithilfe der [API zum Abrufen von Linien öffentlicher Verkehrsmittel](https://aka.ms/AzureMapsMobilityTransitLine) Liniengruppen anfordern und später Detailinformationen zu Linien anzeigen.


## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie Sie mithilfe von Mobility Service Daten zu öffentlichen Verkehrsmitteln bzw. Routen anfordern:

> [!div class="nextstepaction"]
> [Anfordern von Daten zu öffentlichen Verkehrsmitteln bzw. Routen](how-to-request-transit-data.md)

Informieren Sie sich, wie Sie mithilfe des Mobility Service Echtzeitdaten anfordern:

> [!div class="nextstepaction"]
> [Anfordern von Echtzeitdaten](how-to-request-real-time-data.md)

Machen Sie sich mit der API-Dokumentation zum Mobility Service von Azure Maps vertraut:

> [!div class="nextstepaction"]
> [API-Dokumentation zum Mobility Service](https://aka.ms/AzureMapsMobilityService)
