---
title: Behandeln von Mausereignissen mit Azure Maps | Microsoft-Dokumentation
description: Erstellen einer interaktiven Web SDK-Karte mit Kartenereignissen
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 37a3fc3178fe5caeacedfd355a6065ee189a5890
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976544"
---
# <a name="interact-with-the-map---mouse-events"></a>Interagieren mit der Karte – Mausereignisse

In diesem Artikel erfahren Sie, wie Sie die Eigenschaft [map class events](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?#events) verwenden, um Ereignisse auf der Karte und auf verschiedenen Ebenen der Karte hervorzuheben. Außerdem wird beschrieben, wie Sie die Eigenschaft „map class events“ verwenden, um Ereignisse bei der Interaktion mit einem HTML-Marker hervorzuheben.

## <a name="interact-with-the-map"></a>Interaktion mit der Karte

Experimentieren Sie mit der unten gezeigten Karte, und sehen Sie die entsprechenden Mausereignisse hervorgehoben auf der rechten Seite. Sie können auf die Registerkarte **JS** klicken, um den JavaScript-Code anzuzeigen und zu bearbeiten. Außerdem können Sie auf die Schaltfläche **Auf CodePen bearbeiten** klicken und den Code auf CodePen bearbeiten.

<br/>

<iframe height='600' scrolling='no' title='Interaktion mit der Karte – Mausereignisse' src='//codepen.io/azuremaps/embed/bLZEWd/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/bLZEWd/'>Interact with the map – mouse events</a> (Interagieren mit der Karte – Mausereignisse) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="interact-with-map-layers"></a>Interaktion mit Kartenebenen

Der folgende Code hebt den Namen der Ereignisse hervor, die bei der Interaktion mit der Symbolebene ausgelöst werden. Die Symbol-, Blasen-, Linien- und Polygonebenen unterstützen den gleichen Satz von Ereignissen. Die Wärmebild- und Kachelebenen unterstützen keines dieser Ereignisse.

<br/>

<iframe height='600' scrolling='no' title='Interaktion mit der Karte – Ebenenereignisse' src='//codepen.io/azuremaps/embed/bQRRPE/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/bQRRPE/'>Interacting with the map – Layer Events</a> (Interaktion mit der Karte – Ebenenereignisse) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="interact-with-html-marker"></a>Interaktion mit dem HTML-Marker

Der folgende Code fügt Javascript-Kartenereignisse zu einem HTML-Marker hinzu. Er hebt außerdem den Namen der Ereignisse hervor, die bei der Interaktion mit dem HTML-Marker ausgelöst werden.

<br/>

<iframe height='500' scrolling='no' title='Interaktion mit der Karte – HTML Markerereignisse' src='//codepen.io/azuremaps/embed/VVzKJY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen finden Sie unter dem Pen <a href='https://codepen.io/azuremaps/pen/VVzKJY/'>Interacting with the map - HTML Marker events</a> (Interaktion mit der Karte – HTML Markerereignisse) von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) auf <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Nächste Schritte

Die folgenden Artikel enthalten vollständige Codebeispiele:

> [!div class="nextstepaction"]
> [Verwenden des Moduls „Dienste“ von Azure Maps](./how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Codebeispiele](https://docs.microsoft.com/samples/browse/?products=azure-maps)
