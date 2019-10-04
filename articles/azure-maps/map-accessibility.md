---
title: Erstellen einer zugänglichen Anwendung mit Azure Maps | Microsoft-Dokumentation
description: Erstellen einer zugänglichen Anwendung mithilfe von Azure Maps
services: azure-maps
author: chgennar
ms.author: chgennar
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
manager: timlt
ms.openlocfilehash: 8865027c895be09150797608e43184f1fdefb267
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638786"
---
# <a name="building-an-accessible-application"></a>Erstellen einer zugänglichen Anwendung

In diesem Artikel wird gezeigt, wie Sie eine Kartenanwendung erstellen, die von einer Sprachausgabe verwendet werden kann.

## <a name="understand-the-code"></a>Grundlegendes zum Code

<iframe height='500' scrolling='no' title='Erstellen einer zugänglichen Anwendung' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Weitere Informationen erhalten Sie über den Stift <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>Erstellen einer zugänglichen Anwendung</a> von Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) unter <a href='https://codepen.io'>CodePen</a>.
</iframe>

Die Karte ist bereits mit Barrierefreiheitsfeatures ausgestattet. Benutzer können über die Tastatur auf der Karte navigieren. Wenn eine Sprachausgabe ausgeführt wird, benachrichtigt die Karte den Benutzer über Änderungen an ihrem Zustand.
So werden die Benutzer z. B. über Kartenänderungen informiert, wenn die Karte geschwenkt oder vergrößert bzw. verkleinert wird. Zusätzliche Informationen, die auf die allgemeine Karte platziert werden, sollten über entsprechende Textinformationen für Sprachausgabenbenutzer verfügen.

Eine Möglichkeit dazu ist die Verwendung von [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest). Im Suchbeispiel oben wird der Karte ein Popupmenü mit Textinformationen für jede Stecknadel, die auf der Karte platziert wird, hinzugefügt. Mithilfe der [attach](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#attach)-Methode von [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) kann das Popupmenü von einer Sprachausgabe gelesen werden, ohne dass es auf der Karte visuell dargestellt werden muss.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu der in diesem Artikel verwendeten Popup-Klasse und deren Methoden:

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Schauen Sie sich weitere Codebeispiele an:

> [!div class="nextstepaction"]
> [Seite für Beispielcode](https://aka.ms/AzureMapsSamples)
