---
title: Hinzufügen einer Zeichnungssymbolleiste zu Azure Maps | Microsoft-Dokumentation
description: Hinzufügen einer Zeichnungssymbolleiste zu einer Karte mithilfe des Azure Maps Web SDK
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 6bc754c9a4f333da85e57c5ad9780da8df93e895
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309722"
---
# <a name="add-a-drawing-tools-toolbar-to-a-map"></a>Hinzufügen einer Symbolleiste mit Zeichentools zu einer Karte

In diesem Artikel erfahren Sie, wie Sie das Modul mit Zeichentools verwenden und die Zeichnungssymbolleiste auf der Karte anzeigen. Das Steuerelement [DrawingToolbar](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest) fügt die Zeichnungssymbolleiste auf der Karte hinzu. Erfahren Sie, wie Karten mit nur einem und mit allen Zeichnungstools erstellt werden und wie das Rendering der Zeichnungsformen im Zeichnungs-Manager angepasst wird.

## <a name="add-drawing-toolbar"></a>Hinzufügen einer Symbolleiste zum Zeichnen

Mit dem folgenden Code wird eine Instanz des Zeichnungs-Managers erstellt und die Symbolleiste auf der Karte angezeigt.

```Javascript
//Create an instance of the drawing manager and display the drawing toolbar.
drawingManager = new atlas.drawing.DrawingManager(map, {
        toolbar: new atlas.control.DrawingToolbar({
            position: 'top-right',
            style: 'dark'
        })
    });
```

Nachstehend finden Sie das vollständige, ausführbare Codebeispiel für die oben erläuterte Funktionalität:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Hinzufügen einer Symbolleiste zum Zeichnen" src="//codepen.io/azuremaps/embed/ZEzLeRg/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/ZEzLeRg/'>Add drawing toolbar</a> (Hinzufügen einer Zeichnungssymbolleiste) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="limit-displayed-toolbar-options"></a>Begrenzen der angezeigten Symbolleistenoptionen

Mit dem folgenden Code wird eine Instanz des Zeichnungs-Managers erstellt und die Symbolleiste mit nur einem Polygonzeichnungstool auf der Karte angezeigt. 

```Javascript
//Create an instance of the drawing manager and display the drawing toolbar with polygon drawing tool.
drawingManager = new atlas.drawing.DrawingManager(map, {
        toolbar: new atlas.control.DrawingToolbar({
            position: 'top-right',
            style: 'light',
            buttons: ["draw-polygon"]
        })
    });
```

Nachstehend finden Sie das vollständige, ausführbare Codebeispiel für die oben erläuterte Funktionalität:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Hinzufügen eines Polygonzeichnungstools" src="//codepen.io/azuremaps/embed/OJLWWMy/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/OJLWWMy/'>Add a polygon drawing tool</a> (Hinzufügen eines Polygonzeichnungstools) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="change-drawing-rendering-style"></a>Ändern des Zeichnungsrenderingstils

Der folgende Code ruft die Renderingebenen aus dem Zeichnungs-Manager ab und ändert die zugehörigen Optionen, um den Renderingstil für das Zeichnen zu ändern. In diesem Fall werden Punkte mit einem blauen Markersymbol gerendert, Zeilen werden rot und vier Pixel breit gerendert, Polygone weisen eine grüne Füllfarbe und einen orangefarbenen Umriss auf.

```Javascript
var layers = drawingManager.getLayers();
    layers.pointLayer.setOptions({
        iconOptions: {
            image: 'marker-blue'
        }
    });
    layers.lineLayer.setOptions({
        strokeColor: 'red',
        strokeWidth: 4
    });
    layers.polygonLayer.setOptions({
        fillColor: 'green'
    });
    layers.polygonOutlineLayer.setOptions({
        strokeColor: 'orange'
    });
```

Nachstehend finden Sie das vollständige, ausführbare Codebeispiel für die oben erläuterte Funktionalität:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Ändern des Zeichnungsrenderingstils" src="//codepen.io/azuremaps/embed/OJLWpyj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/OJLWpyj/'>Change drawing rendering style</a> (Ändern des Zeichnungsrenderingstils) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Zeichnungssymbolleiste](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Zeichnungs-Manager](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)