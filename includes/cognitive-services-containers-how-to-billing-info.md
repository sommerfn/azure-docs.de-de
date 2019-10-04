---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: a24300958c27daaaf49cc3045a5e99d77c938ab7
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704233"
---
Abfragen des Containers werden gemäß des Tarifs der Azure-Ressource in Rechnung gestellt, die für `<ApiKey>` verwendet wird.

Für die Ausführung von Azure Cognitive Services-Containern besteht keine Lizenz, wenn sie nicht zu Messzwecken mit dem Abrechnungsendpunkt verbunden sind. Sie müssen sicherstellen, dass die Container jederzeit Abrechnungsinformationen an den Abrechnungsendpunkt übermitteln können. Cognitive Services-Container senden keine Kundendaten (z. B. das analysierte Bild oder den analysierten Text) an Microsoft. 

### <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure

Der Container benötigt die Abrechnungsargumentwerte, um ausgeführt werden zu können. Diese Werte ermöglichen es dem Container, eine Verbindung mit dem Abrechnungsendpunkt herzustellen. Der Container meldet die Nutzung etwa alle 10 bis 15 Minuten. Wenn der Container nicht innerhalb des zulässigen Zeitfensters eine Verbindung mit Azure herstellt, wird der Container weiterhin ausgeführt, verarbeitet aber keine Anfragen, bis der Abrechnungsendpunkt wiederhergestellt wurde. Es erfolgen 10 Verbindungsversuche im gleichen Zeitintervall von 10 bis 15 Minuten. Wenn die Verbindung mit dem Abrechnungsendpunkt nicht innerhalb der 10 Versuche hergestellt werden kann, wird die Ausführung des Containers beendet. 

### <a name="billing-arguments"></a>Abrechnungsargumente

Alle drei der folgenden Optionen müssen mit gültigen Werten angegeben werden, damit der Container mit dem Befehl `docker run` gestartet wird:

| Option | BESCHREIBUNG |
|--------|-------------|
| `ApiKey` | Der API-Schlüssel der Cognitive Services-Ressource, der zum Nachverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss für die bereitgestellte Ressource, die in `Billing` angegeben wurde, auf einen API-Schlüssel festgelegt werden. |
| `Billing` | Der Endpunkt der Cognitive Services-Ressource, der zum Nachverfolgen von Abrechnungsinformationen verwendet wird.<br/>Der Wert dieser Option muss auf den Endpunkt-URI einer bereitgestellten Azure-Ressource festgelegt werden.|
| `Eula` | Gibt an, dass Sie die Lizenz für den Container akzeptiert haben.<br/>Der Wert dieser Option muss auf **accept** festgelegt werden. |


