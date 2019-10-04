---
title: Festlegen von Zeichenoptionen in Azure Maps | Microsoft-Dokumentation
description: Festlegen von Zeichnungsoptionen mit dem Azure Maps Web SDK
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: d85525274db7818737b62ad4e9ea2026b8aef3b2
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309741"
---
# <a name="set-drawing-options"></a>Festlegen von Optionen zum Zeichnen

In diesem Artikel erfahren Sie, wie die verschiedenen Optionen des [Zeichnungs-Managers](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-) die Benutzererfahrung ändern. Sie können Optionen für den Zeichnungs-Manager angeben, wenn Sie ihn instanziieren, oder alternativ die **drawingManager.setOptions()** -Funktion verwenden, um Optionen festzulegen.


## <a name="set-drawing-mode"></a>Festlegen des Zeichnungsmodus

Mit dem folgenden Code wird eine Instanz des Zeichnungs-Managers erstellt und die Option für den **Zeichnungsmodus** festgelegt. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon"
});
```

Der nachstehende Code ist ein vollständiges, ausführbares Beispiel zum Festlegen eines Zeichnungsmodus für den Zeichnungs-Manager. Klicken Sie auf die Karte, um mit dem Zeichnen eines Polygons zu beginnen.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Zeichnen eines Polygons" src="//codepen.io/azuremaps/embed/YzKVKRa/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/YzKVKRa/'>Draw a polygon</a> (Zeichnen eines Polygons) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="set-interaction-type"></a>Festlegen des Interaktionstyps

Der folgende Code legt den Typ von Zeichnungsinteraktion fest, dem der Zeichnungs-Manager entsprechen soll. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon",
    interactionType: "freehand"
});
```

Nachstehend finden Sie ein Codebeispiel, das eine Funktion zum freien Zeichnen auf der Karte durch Drücken der linken Maustaste und Bewegen der Maus implementiert. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Freihandzeichnung" src="//codepen.io/azuremaps/embed/ZEzKoaj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/ZEzKoaj/'>Free-hand drawing</a> (Freihandzeichnung) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="customizing-drawing-options"></a>Anpassen von Zeichnungsoptionen

In den vorherigen Beispielen wurde gezeigt, wie Zeichnungsoptionen beim Instanziieren des Zeichnungs-Managers angepasst werden. Sie können die Optionen für den Zeichnungs-Manager auch mithilfe der **drawingManager.setOptions()** -Funktion festlegen. Nachstehend finden Sie ein Tool zum Testen der Anpassung aller Optionen für den Zeichnungs-Manager mit der setOptions-Funktion.

<br/>

<iframe height="685" title="Anpassen des Zeichnungs-Managers" src="//codepen.io/azuremaps/embed/LYPyrxR/?height=600&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/LYPyrxR/'>Get shape data</a> (Abrufen von Formdaten) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Zeichnungs-Manager](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Zeichnungssymbolleiste](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
