---
title: Hinzufügen von Steuerelementen zu Azure Maps | Microsoft-Dokumentation
description: Hinzufügen von Steuerelementen für Zoom, Neigung und Drehen sowie einer Stilauswahl in Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 7267f77ed3d296ac586dcfd0f525b94d5e6eb7a0
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976222"
---
# <a name="add-controls-to-a-map"></a>Hinzufügen von Steuerelementen zu einer Karte

In diesem Artikel wird gezeigt, wie Sie einer Karte Steuerelemente hinzufügen. Außerdem erfahren Sie, wie Sie eine Karte mit allen Steuerelementen und einer [Stilauswahl](https://docs.microsoft.com/azure/azure-maps/choose-map-style) erstellen.

## <a name="add-zoom-control"></a>Hinzufügen eines Zoomsteuerelements

Ein Zoomsteuerelement fügt Schaltflächen zum Vergrößern und Verkleinern der Karte hinzu. Das folgende Codebeispiel erstellt eine Instanz der Klasse [ZoomControl](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) und fügt sie in der rechten unteren Ecke der Karte hinzu.

```javascript
//Construct a zoom control and add it to the map.
map.controls.add(new atlas.control.ZoomControl(), {
    position: 'bottom-right'
});
```

Nachfolgend finden Sie das vollständige Beispiel für ausführbaren Code der obigen Funktionalität.

<br/>

<iframe height='500' scrolling='no' title='Hinzufügen eines Zoomsteuerelements' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>Adding a zoom control</a> (Hinzufügen eines Zoomsteuerelements) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) bei <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-pitch-control"></a>Hinzufügen eines Neigungssteuerelements

Ein Neigungssteuerelement fügt Schaltflächen zum Kippen der Neigung für eine Karte relativ zum Horizont hinzu. Das folgende Codebeispiel erstellt eine Instanz der Klasse [PitchControl](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) und fügt sie in der rechten oberen Ecke der Karte hinzu.

```javascript
//Construct a pitch control and add it to the map.
map.controls.add(new atlas.control.PitchControl(), {
    position: 'top-right'
});
```

Nachfolgend finden Sie das vollständige Beispiel für ausführbaren Code der obigen Funktionalität.

<br/>

<iframe height='500' scrolling='no' title='Hinzufügen eines Neigungssteuerelements' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>Adding a pitch control</a> (Hinzufügen eines Neigungssteuerelements) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) bei <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-compass-control"></a>Hinzufügen eines Kompasssteuerelements

Ein Kompasssteuerelement fügt eine Schaltfläche zum Drehen der Karte hinzu. Das folgende Codebeispiel erstellt eine Instanz der Klasse [Compass Control](/javascript/api/azure-maps-control/atlas.control.compasscontrol) und fügt sie in der linken unteren Ecke der Karte hinzu.

```javascript
//Construct a compass control and add it to the map.
map.controls.add(new atlas.control.Compass(), {
    position: 'bottom-left'
});
```

Nachfolgend finden Sie das vollständige Beispiel für ausführbaren Code der obigen Funktionalität.

<br/>

<iframe height='500' scrolling='no' title='Hinzufügen eines Drehsteuerelements' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>Adding a rotate control</a> (Hinzufügen eines Drehsteuerelements) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) bei <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="a-map-with-all-controls"></a>Eine Karte mit allen Steuerelementen

Das folgende Codebeispiel fügt die Stilauswahl-, Zoom-, Neigungs- und Kompasssteuerelemente in die untere rechte Ecke der Karte hinzu. Beachten Sie, wie sie automatisch gestapelt werden. Die Reihenfolge der Steuerelementobjekte im Skript bestimmt die Reihenfolge, in der sie auf der Karte angezeigt werden. Wenn Sie die Reihenfolge der Steuerelemente auf der Karte ändern möchten, ändern Sie ihre Reihenfolge im Skript.

<br/>

<iframe height='500' scrolling='no' title='Eine Karte mit allen Steuerelementen' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>A map with all the controls</a> (Eine Karte mit allen Steuerelementen) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) bei <a href='https://codepen.io'>CodePen</a>.
</iframe>

Das Stilauswahl-Steuerelement wird von der [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol)-Klasse definiert. Weitere Informationen zum Verwenden des Stilauswahl-Steuerelements finden Sie unter [Auswählen eines Kartenstils.](choose-map-style.md)

## <a name="customize-controls"></a>Anpassen von Steuerelementen

Hier ist ein Tool, mit dem die verschiedenen Optionen zum Anpassen der Steuerelemente getestet werden können.

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Optionen für die Navigationssteuerelemente" src="//codepen.io/azuremaps/embed/LwBZMx/?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/LwBZMx/'>Optionen für die Navigationssteuerelemente</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

Wenn Sie benutzerdefinierte Navigationssteuerelemente erstellen möchten, erstellen Sie eine Klasse, die sich von der `atlas.Control`-Klasse erstreckt, oder erstellen Sie ein HTML-Element und positionieren Sie es über der Kartenverteilung. Mit diesem UI-Steuerelement können Sie die `setCamera`-Funktion der Karte zum Verschieben der Karte aufrufen. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Compass Control](/javascript/api/azure-maps-control/atlas.control.compasscontrol)

> [!div class="nextstepaction"]
> [PitchControl](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) 

> [!div class="nextstepaction"]
> [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) 

> [!div class="nextstepaction"]
> [ZoomControl](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) 

Die folgenden Artikel enthalten den vollständigen Code:

> [!div class="nextstepaction"]
> [Hinzufügen von Stecknadeln zur Karte](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Hinzufügen eines Popups](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Linienebene](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Polygonebene](map-add-shape.md)

> [!div class="nextstepaction"]
> [Hinzufügen einer Blasenebene](map-add-bubble-layer.md)

