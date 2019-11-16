---
title: Unterstützte Kartenstile in Azure Maps | Microsoft-Dokumentation
description: In Azure Maps unterstützte Kartenstile
author: walsehgal
ms.author: v-musehg
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: cd16ee87b213c2bfa4c541c4d3c453a4499855d0
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73887735"
---
# <a name="azure-maps-supported-map-styles"></a>Unterstützte Kartenstile in Azure Maps
Azure Maps unterstützt mehrere integrierte Kartenstile, die im Folgenden beschrieben werden.

## <a name="road"></a>Straße
Der Kartenstil **Straße** ist eine Standardkarte, die Straßen, natürliche und künstliche Strukturen zusammen mit den Bezeichnungen für diese Strukturen anzeigt.

![Straße](./media/supported-map-styles/road.png)

**Anwendbare APIs:**
* [Kartenbild](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Kartenkachel](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="blank-and-blank_accessible"></a>„blank“ und „blank_accessible“

Die Kartenstile **blank** und **blank_accessible** stellen einen leeren Zeichenbereich bereit, in dem Daten visualisiert werden können. Der Stil **blank_accessible** wird weiterhin Updates für die Sprachausgabe mit Details zu dem Standort bereitstellen, an dem sich die Karte befindet, obwohl die Basiskarte nicht angezeigt wird.

> [!Note]
> Im Web-SDK können Sie die Hintergrundfarbe der Karte ändern, indem Sie den CSS-Stil `background-color` des DIV-Kartenelements festlegen.

**Anwendbare APIs:**
* Web-SDK-Kartensteuerelement

## <a name="satellite"></a>Satellit 
Der Kartenstil **Satellit** ist eine Kombination aus Satellitenbildern und Luftaufnahmen.

![Satellit](./media/supported-map-styles/satellite.png)

**Anwendbare APIs:**
* [Satellitenkachel](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="satellite_road_labels"></a>Satellite_Road_Labels
Dieser Kartenstil ist eine Hybriddarstellung, bei der Straßen und Bezeichnungen Satellitenbilder und Luftaufnahmen überlagern.

![Satellite_Road_Labels](./media/supported-map-styles/satellite_road_labels.png)

**Anwendbare APIs:**
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="grayscale_dark"></a>grayscale_dark
**grayscale_dark** ist eine dunkle Version des Kartenstils „Straße“.

![Gray_Scale](./media/supported-map-styles/grayscale_dark.png)

**Anwendbare APIs:**
* Web-SDK-Kartensteuerelement 
* Android-Kartensteuerelement


## <a name="grayscale_light"></a>grayscale_light
**grayscale_light** ist eine helle Version des Stils „Straßenkarte“.

![grayscale light](./media/supported-map-styles/grayscale_light.png)

**Anwendbare APIs:**
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement


## <a name="night"></a>Nacht
**Nacht** ist eine dunkle Version des Kartenstils „Straße“ mit farbigen Straßen und Symbolen.

![Nacht](./media/supported-map-styles/night.png)

**Anwendbare APIs:**
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement

## <a name="road_shaded_relief"></a>road_shaded_relief
**road shaded relief** (Relief mit schattierten Straßen) ist eine Azure Maps-Hauptformatvorlage mit Konturen der Erde.

![schattiertes Relief](./media/supported-map-styles/shaded-relief.png)

**Anwendbare APIs:**
* [Kartenkachel](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Web-SDK-Kartensteuerelement
* Android-Kartensteuerelement


## <a name="next-steps"></a>Nächste Schritte

Informationen, wie ein Kartenstil in Azure Maps festgelegt wird:

> [!div class="nextstepaction"]
> [Auswählen eines Kartenstils](https://docs.microsoft.com/azure/azure-maps/choose-map-style)