---
title: Informationen zu Azure IoT Hub-Endpunkten | Microsoft-Dokumentation
description: 'Entwicklerhandbuch: Referenzinformationen zu IoT Hub-Endpunkten mit Geräte- und Dienstanbindung'
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 689a702863dda53870f775bd8520d5dd406d242f
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68640603"
---
# <a name="reference---iot-hub-endpoints"></a>Referenz: IoT Hub-Endpunkte

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="iot-hub-names"></a>IoT Hub-Namen

Den Hostnamen der IoT Hub-Instanz, die Ihre Endpunkte hostet, finden Sie im Portal auf der Seite **Übersicht**. Standardmäßig sieht der DNS-Name einer IoT Hub-Instanz wie folgt aus: `{your iot hub name}.azure-devices.net`.

## <a name="list-of-built-in-iot-hub-endpoints"></a>Liste der integrierten IoT Hub-Endpunkte

Azure IoT Hub ist ein mehrinstanzenfähiger Dienst, der seine Funktionen für zahlreiche Akteure bereitstellt. Das nachstehende Diagramm zeigt die verschiedenen Endpunkte, die von IoT Hub verfügbar gemacht werden.

![IoT Hub-Endpunkte](./media/iot-hub-devguide-endpoints/endpoints.png)

Die Endpunkte werden in der folgende Liste beschrieben:

* **Ressourcenanbieter**. Der IoT Hub-Ressourcenanbieter macht eine [Azure Resource Manager-Schnittstelle](../azure-resource-manager/resource-group-overview.md) verfügbar. Über diese Schnittstelle können Azure Abonnementbesitzer IoT Hub-Instanzen erstellen und löschen sowie IoT Hub-Eigenschaften aktualisieren. IoT Hub-Eigenschaften steuern [Sicherheitsrichtlinien auf Hubebene](iot-hub-devguide-security.md#access-control-and-permissions) (im Gegensatz zur Zugriffssteuerung auf Geräteebene) und funktionale Optionen für das Messaging zwischen Cloud und Gerät (Cloud-to-Device, C2D) sowie Gerät und Cloud (Device-to-Cloud, D2C). Der IoT Hub-Ressourcenanbieter ermöglicht außerdem das [Exportieren von Geräteidentitäten](iot-hub-devguide-identity-registry.md#import-and-export-device-identities).

* **Geräteidentitätsverwaltung**. Jede IoT Hub-Instanz macht eine Gruppe von HTTPS-REST-Endpunkten zum Verwalten von Geräteidentitäten (zum Erstellen, Abrufen, Aktualisieren und Löschen) verfügbar. [Geräteidentitäten](iot-hub-devguide-identity-registry.md) werden zur Geräteauthentifizierung und für die Zugriffssteuerung eingesetzt.

* **Verwaltung von Gerätezwillingen**. Jede IoT Hub-Instanz macht eine Gruppe von dienstseitigen HTTPS-REST-Endpunkten zum Abfragen und Aktualisieren von [Gerätezwillingen](iot-hub-devguide-device-twins.md) (Updatetags und Eigenschaften) verfügbar.

* **Auftragsverwaltung**. Jede IoT Hub-Instanz macht eine Gruppe von dienstseitigen HTTPS-REST-Endpunkten zum Abfragen und Verwalten von [Aufträgen](iot-hub-devguide-jobs.md) verfügbar.

* **Geräteendpunkte**. Für jedes Gerät in der Identitätsregistrierung macht IoT Hub eine Reihe von Endpunkten verfügbar:

  * *Senden von D2C-Nachrichten*. Ein Gerät verwendet diesen Endpunkt, um [D2C-Nachrichten zu senden](iot-hub-devguide-messages-d2c.md).

  * *Empfangen von C2D-Nachrichten*. Ein Gerät verwendet diesen Endpunkt, um gezielte [C2D-Nachrichten zu empfangen](iot-hub-devguide-messages-c2d.md).

  * *Einleiten von Dateiuploads*. Ein Gerät verwendet diesen Endpunkt zum Empfangen eines Azure Storage-SAS-URIs von IoT Hub, um [eine Datei hochzuladen](iot-hub-devguide-file-upload.md).

  * *Abrufen und Aktualisieren der Eigenschaften von Gerätezwillingen*. Ein Gerät verwendet diesen Endpunkt für den Zugriff auf die Eigenschaften seines [Gerätezwillings](iot-hub-devguide-device-twins.md).

  * *Empfangen von Anforderungen direkter Methoden*. Ein Gerät verwendet diesen Endpunkt zum Lauschen auf Anforderungen [direkter Methoden](iot-hub-devguide-direct-methods.md).

    Diese Endpunkte werden über die Protokolle [MQTT v3.1.1](https://mqtt.org/), HTTPS 1.1 und [AMQP 1.0](https://www.amqp.org/) verfügbar gemacht. AMQP ist auch über [WebSockets](https://tools.ietf.org/html/rfc6455) an Port 443 verfügbar.

* **Dienstendpunkte**. Jede IoT Hub-Instanz macht eine Reihe von Endpunkten verfügbar, über die Ihr Lösungs-Back-End mit Ihren Geräten kommunizieren kann. Mit einer einzigen Ausnahme werden diese Endpunkte nur über das Protokoll [AMQP](https://www.amqp.org/) verfügbar gemacht. Der Endpunkt für den Methodenaufruf wird über das HTTPS-Protokoll verfügbar gemacht.
  
  * *Empfangen von D2C-Nachrichten*. Dieser Endpunkt ist kompatibel mit [Azure Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Ein Back-End-Dienst kann ihn zum Lesen der [D2C-Nachrichten](iot-hub-devguide-messages-d2c.md) verwenden, die Ihre Geräte senden. Zusätzlich zu diesem integrierten Endpunkt können Sie für Ihren IoT-Hub benutzerdefinierte Endpunkte erstellen.
  
  * *Senden von C2D-Nachrichten und Empfangen von Übermittlungsbestätigungen*. Diese Endpunkte ermöglichen Ihrem Lösungs-Back-End das Senden von zuverlässigen [C2D-Nachrichten](iot-hub-devguide-messages-c2d.md) sowie das Empfangen zugehöriger Übermittlungs- oder Ablaufbestätigungen.
  
  * *Empfangen von Dateibenachrichtigungen*. Dieser Messaging-Endpunkt ermöglicht den Empfang von Benachrichtigungen, wenn Ihre Geräte erfolgreich eine Datei hochgeladen haben. 
  
  * *Aufruf direkter Methoden*. Dieser Endpunkt ermöglicht einem Back-End-Dienst das Aufrufen einer [direkten Methode](iot-hub-devguide-direct-methods.md) auf einem Gerät.
  
  * *Empfangen von Vorgangsüberwachungsereignissen*. Mit diesem Endpunkt können Sie Vorgangsüberwachungsereignisse empfangen, wenn Ihr IoT Hub für deren Ausgabe konfiguriert wurde. Weitere Informationen finden Sie unter [IoT Hub-Vorgangsüberwachung](iot-hub-operations-monitoring.md).

Im Artikel [Azure IoT SDKs](iot-hub-devguide-sdks.md) werden die verschiedenen Möglichkeiten zum Zugriff auf diese Endpunkte beschrieben.

Alle IoT Hub-Endpunkte verwenden das [TLS](https://tools.ietf.org/html/rfc5246)-Protokoll, und Endpunkte werden niemals für unverschlüsselte/unsichere Kanäle verfügbar gemacht.

## <a name="custom-endpoints"></a>Benutzerdefinierte Endpunkte

Sie können vorhandene Azure-Dienste in Ihrem Abonnement mit Ihrem IoT-Hub verknüpfen, die als Endpunkte für das Nachrichtenrouting fungieren. Diese Endpunkte fungieren als Dienstendpunkte und werden als Senken für Nachrichtenrouten verwendet. Geräte können nicht direkt auf die zusätzlichen Endpunkte schreiben. Erfahren Sie mehr über das [Nachrichtenrouting](../iot-hub/iot-hub-devguide-messages-d2c.md).

IoT Hub unterstützt derzeit folgende Azure-Dienste als zusätzliche Endpunkte:

* Azure Storage-Container
* Event Hubs
* Service Bus-Warteschlangen
* Service Bus-Themen

Informationen zur Beschränkung der Anzahl von Endpunkten, die Sie hinzufügen können, finden Sie unter [Kontingente und Drosselung](iot-hub-devguide-quotas-throttling.md).

Sie können die REST-API [Get Endpoint Health](https://docs.microsoft.com/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth) (Endpunktintegrität abrufen) verwenden, um den Integritätsstatus der Endpunkte abzurufen. Wir empfehlen Ihnen die Verwendung von [IoT Hub-Metriken](iot-hub-metrics.md) zur Latenz bei der Nachrichtenweiterleitung, um Fehler zu identifizieren und zu debuggen, wenn die Integrität des Endpunkts „Inaktiv“ oder „Fehlerhaft“ lautet. Es ist zu erwarten, dass die Latenz höher ist, wenn der Endpunkt einen dieser Status aufweist.

|Integritätsstatus|BESCHREIBUNG|
|---|---|
|healthy|Der Endpunkt akzeptiert Nachrichten erwartungsgemäß.|
|Fehlerhaft|Der Endpunkt akzeptiert Nachrichten nicht erwartungsgemäß, und IoT Hub versucht, Daten erneut an diesen Endpunkt zu senden. Der Status eines fehlerhaften Endpunkts wird zu fehlerfrei aktualisiert, wenn IoT Hub einen möglicherweise konsistenten Integritätsstatus erreicht hat.|
|unknown|IoT Hub hat keine Verbindung mit dem Endpunkt hergestellt. Von diesem Endpunkt wurden keine Nachrichten übermittelt oder zurückgewiesen.|
|Inaktiv|Der Endpunkt nimmt auch nach dem wiederholten Senden von Nachrichten für den Wiederholungszeitraum durch den IoT Hub keine Nachrichten an.|

## <a name="field-gateways"></a>Bereichsgateways

Bei einer IoT-Lösung ist zwischen Ihren Geräten und IoT Hub-Endpunkten ein *Bereichsgateway* angeordnet. Es befindet sich normalerweise in der Nähe Ihrer Geräte. Ihre Geräte kommunizieren direkt mit dem Bereichsgateway, indem sie ein von den Geräten unterstütztes Protokoll nutzen. Das Bereichsgateway verbindet sich mit einem IoT Hub-Endpunkt über ein Protokoll, das von IoT Hub unterstützt wird. Bei einem Bereichsgateway kann es sich um ein dediziertes Hardwaregerät oder um einen energiesparenden Computer mit benutzerdefinierter Gatewaysoftware handeln.

Sie können [Azure IoT Edge](/azure/iot-edge/) verwenden, um ein Bereichsgateway zu implementieren. IoT Edge ermöglicht es unter anderem, die Kommunikation mehrerer Geräte im Multiplexverfahren über die gleiche IoT Hub-Verbindung zu übertragen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Referenzthemen in diesem IoT Hub-Entwicklungsleitfaden:

* [IoT Hub-Abfragesprache für Gerätezwillinge, Aufträge und Nachrichtenrouting](iot-hub-devguide-query-language.md)
* [Kontingente und Drosselung](iot-hub-devguide-quotas-throttling.md)
* [IoT Hub-MQTT-Unterstützung](iot-hub-mqtt-support.md)
* [Grundlegendes zur IP-Adresse des IoT-Hubs](iot-hub-understand-ip-address.md)