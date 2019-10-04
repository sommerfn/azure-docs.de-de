---
title: Verbrauchsmodell in Azure Maps | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Verbrauchsmodell in Azure Maps
author: subbarayudukamma
ms.author: skamma
ms.date: 05/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 5f75f656312c11a4668ca9ef9fe7b2a61a7d13e8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60797900"
---
# <a name="consumption-model"></a>Nutzungsmodell

Die Onlinestreckenplanung bietet eine Reihe von Parametern für eine ausführliche Beschreibung eines fahrzeugspezifischen Verbrauchsmodells.
Abhängig vom Wert für **vehicleEngineType** werden zwei grundlegende Verbrauchsmodelle unterstützt: _Combustion_ und _Electric_. Sie können in einer Anforderung keine Parameter angeben, die zu unterschiedlichen Modellen gehören.
Das Verbrauchsmodell kann nicht mit den **travelMode**-Werten _bicycle_ und _pedestrian_ verwendet werden.

## <a name="parameter-constraints-for-consumption-model"></a>Parametereinschränkungen für das Verbrauchsmodell

In beiden Verbrauchsmodellen gilt: Wenn einige Parameter explizit angeben werden, müssen auch andere angegeben werden. Diese Abhängigkeiten sind:

* Alle Parameter erfordern, dass der Benutzer **constantSpeedConsumption** angibt. Sie können keinen anderen Verbrauchsmodellparameter außer **vehicleWeight** angeben, wenn **constantSpeedConsumption** nicht angegeben ist.
* **accelerationEfficiency** und **decelerationEfficiency** müssen immer als Paar (d.h. beide oder keiner) angegeben werden.
* Wenn **accelerationEfficiency** und **decelerationEfficiency** angegeben sind, darf das Produkt ihrer Werte nicht größer als 1 sein (um eine unaufhörliche Bewegung zu verhindern).
* **uphillEfficiency** und **downhillEfficiency** müssen immer als Paar (d.h. beide oder keiner) angegeben werden.
* Wenn **uphillEfficiency** und **downhillEfficiency** angegeben sind, darf das Produkt ihrer Werte nicht größer als 1 sein (um eine unaufhörliche Bewegung zu verhindern).
* Wenn die \*__Efficiency__-Parameter vom Benutzer angegeben werden, muss auch **vehicleWeight** angegeben werden. Wenn **vehicleEngineType** den Wert _combustion_ hat, muss auch **fuelEnergyDensityInMJoulesPerLiter** angegeben werden.
* **maxChargeInkWh** und **currentChargeInkWh** müssen immer als Paar (d.h. beide oder keiner) angegeben werden.

> [!NOTE]
> Wenn nur **constantSpeedConsumption** angegeben ist, werden keine anderen Verbrauchsaspekte wie Steigungen und Fahrzeugbeschleunigung für Verbrauchsberechnungen berücksichtigt.

## <a name="combustion-consumption-model"></a>Verbrauchsmodell „Combustion“

Das Verbrauchsmodell „Combustion“ wird verwendet, wenn **vehicleEngineType** auf _combustion_ festgelegt ist.
Die Liste der Parameter, die zu diesem Modell gehören, ist unten aufgeführt. Eine detaillierte Beschreibung finden Sie im Abschnitt „Parameter“.

* constantSpeedConsumptionInLitersPerHundredkm
* vehicleWeight
* currentFuelInLiters
* auxiliaryPowerInLitersPerHour
* fuelEnergyDensityInMJoulesPerLiter
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="electric-consumption-model"></a>Verbrauchsmodell „Electric“

Das Verbrauchsmodell „Electric“ wird verwendet, wenn **vehicleEngineType** auf _electric_ festgelegt ist.
Die Liste der Parameter, die zu diesem Modell gehören, ist unten aufgeführt. Eine detaillierte Beschreibung finden Sie im Abschnitt „Parameter“.

* constantSpeedConsumptionInkWhPerHundredkm
* vehicleWeight
* currentChargeInkWh
* maxChargeInkWh
* auxiliaryPowerInkW
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="sensible-values-of-consumption-parameters"></a>Sinnvolle Werte für Verbrauchsparameter

Ein bestimmter Satz von Verbrauchsparametern kann abgelehnt werden, auch wenn er alle expliziten Anforderungen erfüllt, die oben angegebenen sind. Dies geschieht, wenn der Wert eines bestimmten Parameters oder eine Kombination von Werten mehrerer Parameter wahrscheinlich zu unangemessenen Größen von Verbrauchswerten führt. In diesem Fall ist die Ursache wahrscheinlich ein Eingabefehler, da sorgfältig darauf geachtet wird, alle sinnvollen Werte für Verbrauchsparameter zu berücksichtigen. Für den Fall, dass ein bestimmter Satz von Verbrauchsparametern abgelehnt wird, enthält die zugehörige Fehlermeldung einen Erläuterungstext zu den Ursachen.
Die detaillierten Beschreibungen der Parameter enthalten Beispiele für sinnvolle Werte für beide Modelle.
