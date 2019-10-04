---
title: Kartenstilfunktionen in Azure Maps | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zu stilbezogenen Funktionen von Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: eb667c398be0bd51e05a6b65d416d5bce54e4386
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881963"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Auswählen eines Kartenstils in Azure Maps

Viele der [unterstützten Kartenstile in Azure Maps](./supported-map-styles.md) sind im Web SDK verfügbar. In diesem Artikel wird gezeigt, wie Sie mit den stilbezogenen Funktionen einen Stil für eine geladene Karte festlegen, einen neuen Stil festlegen und das Stilauswahlsteuerelement verwenden.

## <a name="set-style-on-map-load"></a>Festlegen des Stils für die geladene Karte

Im folgenden Code wird die Option `style` der Karte bei der Initialisierung auf `grayscale_dark` festgelegt:

<br/>

<iframe height='500' scrolling='no' title='Festlegen des Stils für eine geladene Karte' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>Setting the style on map load</a> A PEN BY Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf der <a href='https://codepen.io'>CodePen-Website</a>.
</iframe>

## <a name="update-the-style"></a>Aktualisieren des Stils

Im folgenden Code wird nach dem Laden einer Karteninstanz der Kartenstil mithilfe der Funktion [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) der Karte von `road` auf `satellite` aktualisiert:

<br/>

<iframe height='500' scrolling='no' title='Aktualisieren des Stils' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>Updating the style</a> A PEN BY Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf der <a href='https://codepen.io'>CodePen-Website</a>.
</iframe>

## <a name="add-the-style-picker"></a>Hinzufügen der Stilauswahl

Der folgende Code fügt der Karte ein Element vom Typ [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) hinzu, damit der Benutzer mühelos zwischen den verschiedenen Kartenstilen wechseln kann: 

<br/>

<iframe height='500' scrolling='no' title='Hinzufügen der Stilauswahl' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>Adding the style picker</a> A PEN BY Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf der <a href='https://codepen.io'>CodePen-Website</a>.
</iframe>

> [!TIP]
> Standardmäßig listet das Steuerelement für die Stilauswahl alle verfügbaren Stile auf, wenn standardmäßig der S0-Tarif Azure Maps verwendet wird. Soll die Liste weniger Stile enthalten, können Sie an die Option `mapStyle` der Stilauswahl ein Array mit den Stilen übergeben, die in der Liste angezeigt werden sollen. Wenn Sie S1 verwenden und alle verfügbaren Stile anzeigen möchten, legen Sie die Option `mapStyles` der Stilauswahl auf `"all"` fest.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Fügen Sie Ihren Karten Steuerelemente hinzu:

> [!div class="nextstepaction"]
> [Hinzufügen von Kartensteuerelementen](map-add-controls.md)

> [!div class="nextstepaction"]
> [Hinzufügen von Stecknadeln zur Karte](map-add-pin.md)
