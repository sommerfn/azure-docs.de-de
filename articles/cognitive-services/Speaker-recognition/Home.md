---
title: Was ist die Sprechererkennungs-API?
titleSuffix: Azure Cognitive Services
description: Sprecherüberprüfung und Sprecheridentifikation mit der Sprechererkennungs-API in Cognitive Services.
services: cognitive-services
author: dwlin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speaker-recognition
ms.topic: overview
ms.date: 10/01/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: f354793e3779f4d9f7be9bae8a21545a15ed1c4c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464921"
---
# <a name="speaker-recognition-api---preview"></a>Sprechererkennungs-API: Vorschauversion

Bei Sprechererkennungs-APIs handelt es sich um cloudbasierte APIs, die hochentwickelte Algorithmen zur Sprecherüberprüfung und -identifikation bereitstellen. Die Sprechererkennung ist in zwei Kategorien unterteilt: Sprecherüberprüfung und Sprecheridentifikation.

## <a name="speaker-verification"></a>Sprecherüberprüfung

Die Stimme hat unverwechselbare Eigenschaften, die einem Individuum zugeordnet werden können.  Anwendungen können in Szenarien wie Callcentern und Webdiensten die Stimme als zusätzlichen Faktor für die Überprüfung nutzen.

Sprecherüberprüfungs-APIs dienen als intelligentes Tool, um Benutzer anhand ihrer Stimme und gesprochenen Passphrase zu überprüfen.

### <a name="enrollment"></a>Registrierung

Die Registrierung für die Sprecherüberprüfung ist textabhängig. Das bedeutet, dass sich Sprecher für eine bestimmte Passphrase entscheiden und diese sowohl in der Registrierungs- als auch der Überprüfungsphase verwenden müssen.

In der Sprecherregistrierungsphase wird die Stimme des Sprechers mit einer bestimmten Aussage aufgezeichnet. Stimmmerkmale werden extrahiert, um eine eindeutige Stimmsignatur zu bilden, während die gewählte Aussage erkannt wird. Diese Sprecherregistrierungsdaten werden anschließend verwendet, um den Sprecher zu überprüfen. Die Sprecherregistrierungsdaten werden in einem abgesicherten System gespeichert. Der Kunde bestimmt, wie lange sie aufbewahrt werden sollen. Kunden können über API-Aufrufe Registrierungsdaten für einzelne Sprecher erstellen, aktualisieren und entfernen.  Beim Löschen des Abonnements werden auch alle mit dem Abonnement verbundenen Sprecherregistrierungsdaten gelöscht.

Kunden müssen sicherstellen, dass sie von den Benutzern die entsprechende Erlaubnis für die Sprecherüberprüfung erhalten haben.

### <a name="verification"></a>Überprüfung

In der Überprüfungsphase muss der Kunde die Sprecherüberprüfungs-API mit der ID der zu überprüfenden Person aufrufen.  Der Dienst extrahiert Stimmmerkmale und die Passphrase aus der Sprachaufzeichnung der Eingabe. Anschließend vergleicht der Dienst die Merkmale mit den entsprechenden Elementen der Sprecherregistrierungsdaten für den Sprecher, den der Kunde zu überprüfen versucht, und bestimmt etwaige Übereinstimmungen.  Als Antwort wird „Akzeptieren“ oder „Ablehnen“ mit unterschiedlichen Konfidenzniveaus zurückgegeben.  Der Kunde bestimmt dann, wie er die Ergebnisse nutzen möchte, um zu entscheiden, ob es sich bei dieser Person um den registrierten Sprecher handelt.

Der Schwellenwert des Konfidenzniveaus muss abhängig vom Szenario und anderen verwendeten Überprüfungsfaktoren festgelegt werden. Es wird empfohlen, mit dem Konfidenzniveau zu experimentieren und die für die jeweilige Anwendung geeignete Einstellung zu wählen. Die APIs dienen nicht zum Bestimmen, ob das Audio von einer echten Person oder einer Imitation oder Aufzeichnung eines registrierten Sprechers stammt.

Der Dienst speichert weder die Sprachaufzeichnung noch die extrahierten Stimmmerkmale, die während der Überprüfungsphase an den Dienst gesendet werden.

Weitere Informationen zur Sprecherüberprüfung finden Sie unter [Speaker Recognition API](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652) (Sprechererkennungs-API).

## <a name="speaker-identification"></a>Sprecheridentifikation

Anwendungen können mithilfe der Stimme feststellen, wer in einer Gruppe registrierter Sprecher spricht. Sprecheridentifikations-APIs können in Szenarien wie Produktivität von Besprechungen, Personalisierung und Transkription im Callcenter verwendet werden.

### <a name="enrollment"></a>Registrierung

Die Registrierung für die Sprecheridentifikation ist textunabhängig. Es spielt also keine Rolle, was der Sprecher im Audio sagt. Eine Passphrase ist nicht erforderlich.

In der Registrierungsphase wird die Stimme des Sprechers aufgezeichnet. Stimmmerkmale werden extrahiert, um eine eindeutige Stimmsignatur zu bilden. Die extrahierten Stimmmerkmale werden in einem abgesicherten System gespeichert. Der Kunde bestimmt, wie lange sie aufbewahrt werden sollen. Kunden können über API-Aufrufe Registrierungsdaten für einzelne Sprecher erstellen, aktualisieren und entfernen. Beim Löschen des Abonnements werden auch alle mit dem Abonnement verbundenen Sprecherregistrierungsdaten gelöscht.

Kunden müssen sicherstellen, dass sie von den Benutzern die entsprechende Erlaubnis zur Sprecheridentifikation erhalten haben.

### <a name="identification"></a>Identifikation

In der Identifikationsphase extrahiert der Sprecheridentifikationsdienst Stimmmerkmale in der Sprachaufzeichnung der Eingabe. Anschließend werden diese Merkmale mit den Registrierungsdaten in der angegebenen Sprecherliste verglichen. Wenn eine Übereinstimmung mit einem registrierten Sprecher gefunden wird, enthält die Antwort die ID des Sprechers mit einem Konfidenzniveau.  Andernfalls wird von der Antwort „Ablehnen“ zurückgegeben, wenn kein Sprecher mit einem registrierten Sprecher übereinstimmt.

Der Schwellenwert des Konfidenzniveaus muss abhängig vom Szenario festgelegt werden. Es wird empfohlen, mit dem Konfidenzniveau zu experimentieren und die für die jeweilige Anwendung geeignete Einstellung zu wählen. Die APIs dienen nicht zum Bestimmen, ob das Audio von einer echten Person oder einer Imitation oder Aufzeichnung eines registrierten Sprechers stammt.

Der Dienst speichert weder die Sprachaufzeichnung noch die extrahierten Stimmmerkmale, die während der Identifikationsphase an den Dienst gesendet werden.

Weitere Informationen zur Sprecheridentifikation finden Sie unter  [Speaker Recognition API](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e) (Sprechererkennungs-API).
