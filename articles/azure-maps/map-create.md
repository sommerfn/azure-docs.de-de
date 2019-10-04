---
title: Erstellen einer Karte mit Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit dem Azure Maps Web SDK eine Karte erstellen.
author: jingjing-z
ms.author: jinzh
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 932c244ff41e757413a05cde019ee7ee1a82232d
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976120"
---
# <a name="create-a-map"></a>Erstellen einer Karte

In diesem Artikel erfahren Sie, wie Sie eine Karte erstellen und animieren.  

## <a name="loading-a-map"></a>Laden einer Karte

Um eine Karte zu laden, erstellen Sie eine neue Instanz der [Map-Klasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest). Beim Initialisieren der Karte werden eine DIV-Element-ID zum Rendern der Karte und eine Reihe von Optionen übergeben, die beim Laden der Karte verwendet werden können. Wenn im `atlas`-Namespace keine standardmäßigen Authentifizierungsinformationen angegeben sind, müssen diese Informationen beim Laden der Karte in den Kartenoptionen angegeben werden. Die Karte lädt aus Leistungsgründen verschiedene Ressourcen asynchron. Fügen Sie daher nach dem Erstellen der Karteninstanz ein `ready`- oder ein `load`-Ereignis zur Karte hinzu, und fügen Sie dann zusätzlichen Code hinzu, der in diesem Ereignishandler mit der Karte interagiert. Das `ready`-Ereignis wird ausgelöst, sobald genügend Ressourcen geladen wurden, um eine programmgesteuerte Interaktion mit der Karte zu ermöglichen. Das `load`-Ereignis wird ausgelöst, nachdem die anfängliche Kartenansicht vollständig geladen wurde. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Grundlegende Kartenladefunktion" src="//codepen.io/azuremaps/embed/rXdBXx/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/rXdBXx/'>Basic map load</a> (Grundlegende Kartenladefunktion) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Auf einer Seite können mehrere Karten geladen werden, und jede dieser Karten kann unterschiedliche oder die gleichen Authentifizierungs- und Spracheinstellungen aufweisen.

## <a name="show-a-single-copy-of-the-world"></a>Anzeigen nur einer Weltkarte

Wenn der Maßstab einer Karte auf einem Breitbildschirm verkleinert wird, werden mehrere Kopien der Weltkarte horizontal angezeigt. Dies ist in den meisten Szenarien in Ordnung, aber bei einigen Anwendungen ist es wünschenswert, nur eine Kopie anzuzeigen. Dies lässt sich durch Einrichten der Kartenoption `renderWorldCopies` auf `false` erreichen.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="renderWorldCopies = false" src="//codepen.io/azuremaps/embed/eqMYpZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Sehen Sie sich den Pen <a href='https://codepen.io/azuremaps/pen/eqMYpZ/'>renderWorldCopies = false</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a> an.
</iframe>

## <a name="controlling-the-map-camera"></a>Steuern der Kartenkamera

Es gibt zwei Möglichkeiten, um den angezeigten Kartenbereich mithilfe der Kamera festzulegen. Sie können Kameraoptionen wie „Zentrieren“ oder „Zoomen“ beim Laden der Karte festlegen. Sie können auch jederzeit, nachdem die Karte geladen wurde, die Option `setCamera` aufrufen, um die Kartenansicht programmgesteuert zu aktualisieren.  

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Festlegen der Kamera

Im folgenden Code wird ein [Map-Objekt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) erstellt, und die Optionen für Zentrieren und Zoomen werden festgelegt. Karteneigenschaften wie Zentrieren und Zoomfaktor gehören zu [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions).

<br/>

<iframe height='500' scrolling='no' title='Erstellen einer Karte über „CameraOptions“' src='//codepen.io/azuremaps/embed/qxKBMN/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen zum <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>Erstellen einer Karte über `CameraOptions` </a>von Azure Location Based Services (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Festlegen der Kameragrenzen

Im folgenden Code wird über `new atlas.Map()` ein [Map-Objekt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) konstruiert. Karteneigenschaften wie `CameraBoundsOptions` können mit der Funktion [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) der Map-Klasse definiert werden. Eigenschaften von Begrenzungen und Abständen werden mit `setCamera` festgelegt.

<br/>

<iframe height='500' scrolling='no' title='Erstellen einer Karte über „CameraBoundsOptions“' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen zum <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>Erstellen einer Karte über `CameraBoundsOptions` </a>von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="animate-map-view"></a>Animieren der Kartenansicht

Im folgenden Code erstellt der erste Codeblock eine Karte und legt Kartenstil, Zentrierung und Zoomwerte fest. Im zweiten Codeblock wird ein Klickereignishandler für die Schaltfläche zum Animieren erstellt. Beim Klicken auf diese Schaltfläche wird die setCamera-Funktion mit zufällig generierten Werten für [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions) und [AnimationOptions](/javascript/api/azure-maps-control/atlas.animationoptions) aufgerufen.

<br/>

<iframe height='500' scrolling='no' title='Animieren der Kartenansicht' src='//codepen.io/azuremaps/embed/WayvbO/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter <a href='https://codepen.io/azuremaps/pen/WayvbO/'>Animieren der Kartenansicht</a> in Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="try-out-the-code"></a>Testen Sie den Code

Sehen Sie sich den obigen Beispielcode an. Sie können den JavaScript-Code links auf der Registerkarte **JS** bearbeiten und sich die Änderungen an der Kartenansicht rechts auf der Registerkarte **Ergebnis** ansehen. Außerdem können Sie auf die Schaltfläche **Auf CodePen bearbeiten** klicken, und den Code auf CodePen bearbeiten.

<a id="relatedReference"></a>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu den in diesem Artikel verwendeten Klassen und Methoden:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions)

> [!div class="nextstepaction"]
> [AnimationOptions](/javascript/api/azure-maps-control/atlas.animationoptions)

Sehen Sie sich Codebeispiele an, die zeigen, wie Sie Ihrer App Funktionen hinzufügen:

> [!div class="nextstepaction"]
> [Ändern des Kartenstils](choose-map-style.md)

> [!div class="nextstepaction"]
> [Hinzufügen von Steuerelementen zur Karte](map-add-controls.md)

> [!div class="nextstepaction"]
> [Codebeispiele](https://docs.microsoft.com/samples/browse/?products=azure-maps)