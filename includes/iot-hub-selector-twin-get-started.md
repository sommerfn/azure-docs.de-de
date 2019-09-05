---
author: wesmc7777
ms.author: wesmc
ms.service: iot-hub
ms.topic: include
ms.date: 08/20/2019
ms.openlocfilehash: c412f7f3e20e4d04083e457bfb245b850b65e126
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70050404"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [.NET](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Gerätezwillinge sind JSON-Dokumente, in denen Gerätestatusinformationen gespeichert werden, einschließlich Metadaten, Konfigurationen und Bedingungen. Von IoT Hub wird für jedes Gerät, das eine Verbindung herstellt, dauerhaft ein Gerätezwilling gespeichert.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Verwenden Sie Gerätezwillinge für Folgendes:

* Speichern von Gerätemetadaten von Ihrem Lösungs-Back-End

* Melden von aktuellen Zustandsinformationen wie verfügbare Funktionen und Bedingungen, z.B. die verwendete Verbindungsmethode, von Ihrer Geräte-App

* Synchronisieren des Zustands von Workflows mit langer Ausführungsdauer, z.B. Firmware- und Konfigurationsupdates, zwischen einer Geräte-App und einer Back-End-App

* Abfragen von Metadaten, Konfiguration oder Status des Geräts

Gerätezwillinge sind für die Synchronisierung und zum Abfragen von Gerätekonfigurationen und -bedingungen ausgelegt. Weitere Informationen dazu, wann Sie Gerätezwillinge verwenden sollten, finden Sie unter [Grundlegendes zu Gerätezwillingen](../articles/iot-hub/iot-hub-devguide-device-twins.md).

Gerätezwillinge werden in einem IoT-Hub gespeichert und enthalten die folgenden Elemente:

* **Tags**. Gerätemetadaten, auf die nur vom Lösungs-Back-End zugegriffen werden kann.

* **Gewünschte Eigenschaften** JSON-Objekte, die vom Lösungs-Back-End geändert und von der Geräte-App überwacht werden können.

* **Gemeldete Eigenschaften** JSON-Objekte, die von der Geräte-App geändert und vom Lösungs-Back-End gelesen werden können.

Tags und Eigenschaften können keine Arrays enthalten, aber Objekte können geschachtelt werden.

Die folgende Abbildung zeigt die Organisation des Gerätezwillings:

![Abbildung eines Gerätezwillings mit Funktionen](./media/iot-hub-selector-twin-get-started/twin.png)

Außerdem können mit dem Lösungs-Back-End Gerätezwillinge basierend auf allen obigen Daten abgefragt werden.
Weitere Informationen zu Gerätezwillingen finden Sie unter [Grundlegendes zu Gerätezwillingen](../articles/iot-hub/iot-hub-devguide-device-twins.md). Weitere Informationen zu Abfragen finden Sie unter [IoT Hub-Abfragesprache](../articles/iot-hub/iot-hub-devguide-query-language.md).


Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen Sie eine Back-End-App, mit der einem Gerätezwilling Tags hinzufügt werden, und eine simulierte Geräte-App, die ihren Konnektivitätskanal als gemeldete Eigenschaft auf dem Gerätezwilling meldet.

* Fragen Sie Geräte von Ihrer Back-End-App ab, indem Sie Filter für die zuvor erstellten Tags und Eigenschaften verwenden.
