---
title: Was ist die Sprechererkennungs-API?
titleSuffix: Azure Cognitive Services
description: Verwenden Sie fortschrittliche Algorithmen zur Sprecherüberprüfung und Sprecheridentifikation mit der Sprechererkennungs-API in Cognitive Services.
services: cognitive-services
author: dwlin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speaker-recognition
ms.topic: overview
ms.date: 10/01/2018
ms.author: nitinme
ms.openlocfilehash: 15fc320a5b76a50def634d937a02fa639dce3739
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67845869"
---
# <a name="speaker-recognition-api"></a>Sprechererkennungs-API

Willkommen bei den Sprechererkennungs-APIs in Azure Cognitive Services. Bei Sprechererkennungs-APIs handelt es sich um cloudbasierte APIs, die modernste Algorithmen zur Sprecherüberprüfung und Sprecheridentifikation bereitstellen. Die Sprechererkennung lässt sich in zwei Kategorien unterteilen: Sprecherüberprüfung und Sprecheridentifikation.


## <a name="speaker-verification"></a>Sprecherüberprüfung

Jede menschliche Stimme verfügt über eindeutige Merkmale, die zum Identifizieren einer Person verwendet werden können – genauso wie ein Fingerabdruck.  Das Verwenden von Stimme und Sprache als Signal für die Zugriffssteuerung hat sich als innovatives Werkzeug erwiesen, mit dem sich ein höheres Niveau an Sicherheit und mehr Komfort bei der Authentifizierung für die Kunden erreichen lässt.

Die Sprecherüberprüfungs-APIs können Benutzer anhand ihrer Stimme und Sprache automatisch authentifizieren.

### <a name="enrollment"></a>Registrierung

Die Registrierung für die Sprecherüberprüfung ist textabhängig. Das bedeutet, Sprecher müssen sich für eine bestimmte Passphrase entscheiden und diese sowohl bei der Registrierung als auch bei der Überprüfung verwenden.

Bei der Registrierung wird die Stimme des Sprechers aufgezeichnet, während dieser eine bestimmte Phrase spricht. Anschließend wird eine Reihe von Merkmalen extrahiert, und die gewählte Phrase wird erkannt. Die extrahierten Merkmale bilden zusammen mit der gewählten Phrase eine eindeutige Stimmsignatur.

### <a name="verification"></a>Überprüfung

Bei der Überprüfung werden Spracheingabe und Passphrase mit der Stimmsignatur und der Passphrase aus der Registrierung abgeglichen, um zu ermitteln, ob sie von derselben Person stammen und ob die korrekte Passphrase verwendet wurde.

Weitere Informationen zur Sprecherüberprüfung finden Sie unter  [Speaker Recognition API](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652) (Sprechererkennungs-API).

## <a name="speaker-identification"></a>Sprecheridentifikation

Sprecheridentifikations-APIs können die sprechende Person anhand einer Audiodatei automatisch erkennen, sofern sie zur einer Gruppe erwarteter Sprecher gehört. Das eingehende Audiosignal wird mit den Daten der bereitgestellten Gruppe von Sprechern abgeglichen, und im Fall einer Übereinstimmung wird die Identität des Sprechers zurückgegeben.

Alle Sprecher müssen zuerst einen Registrierungsprozess absolvieren, bei dem ihre Stimme im System erfasst und ihr stimmlicher Fingerabdruck erstellt wird.


### <a name="enrollment"></a>Registrierung

Die Registrierung für die Sprecheridentifikation ist textunabhängig. Es spielt also keine Rolle, was der Sprecher im Audio sagt. Die Stimme des Sprechers wird aufgezeichnet, und es wird eine Reihe von Merkmalen extrahiert, um eine eindeutige Stimmsignatur zu erhalten.


### <a name="recognition"></a>Erkennung

Für die Erkennung werden das Audio des unbekannten Sprechers sowie die Gruppe potenzieller Sprecher bereitgestellt. Die Spracheingabe wird mit allen Sprechern verglichen, um zu ermitteln, zu wem die Stimme gehört. Wird eine Übereinstimmung gefunden, wird die Identität des Sprechers zurückgegeben.

Weitere Informationen zur Sprecheridentifikation finden Sie unter  [Speaker Recognition API](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e) (Sprechererkennungs-API).
