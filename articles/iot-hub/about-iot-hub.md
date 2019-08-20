---
title: Einführung in Azure IoT Hub| Microsoft-Dokumentation
description: Erfahren Sie mehr über Azure IoT Hub. Dieser IoT-Dienst ist für eine skalierbare Datenerfassung, Geräteverwaltung und Sicherheit konzipiert.
author: nberdy
ms.author: nberdy
ms.date: 08/08/2019
ms.topic: overview
ms.custom: mvc
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: 0dde7bd9bd3372f0f5eddbbd56727eca3ae4a9dd
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "68950464"
---
# <a name="what-is-azure-iot-hub"></a>Was ist Azure IoT Hub?

IoT Hub ist ein in der Cloud gehosteter, verwalteter Dienst, der als zentraler Nachrichtenhub für die bidirektionale Kommunikation zwischen Ihrer IoT-Anwendung und den Geräten dient, die von der Anwendung verwaltet werden. Sie können Azure IoT Hub verwenden, um IoT-Lösungen mit zuverlässiger und sicherer Kommunikation zwischen Millionen von IoT-Geräten und einem in der Cloud gehosteten Lösungs-Back-End zu erstellen. Sie können praktisch jedes Gerät mit IoT Hub verbinden.

IoT Hub unterstützt die Kommunikation sowohl vom Gerät an die Cloud als auch von der Cloud an das Gerät. IoT Hub unterstützt mehrere Messagingmuster wie z.B. Gerät-zu-Cloud-Telemetrie, Dateiuploads von Geräten und Anforderungs-Antwort-Methoden zum Steuern der Geräte über die Cloud. Die IoT Hub-Überwachung unterstützt Sie dabei, die Integrität Ihrer Lösung sicherzustellen, indem Ereignisse wie Geräteerstellung, Geräteausfälle und Geräteverbindungen nachverfolgt werden.

Mit den IoT Hub-Funktionen können Sie skalierbare IoT-Lösungen mit vollem Funktionsumfang erstellen, z.B. für die Verwaltung von Werkzeugen und Maschinen für die industrielle Fertigung, für die Nachverfolgung wertvoller Ressourcen im Gesundheitswesen oder für die Überwachung der Nutzung von Bürogebäuden.

## <a name="scale-your-solution"></a>Skalieren Ihrer Lösung

IoT Hub lässt sich auf Millionen von gleichzeitig verbundenen Geräten und Millionen von Ereignissen pro Sekunde skalieren, um Ihre IoT-Workloads zu unterstützen. Weitere Informationen zum Skalieren Ihres IoT-Hubs finden Sie unter [Skalieren von Azure IoT Hub](iot-hub-scaling.md?branch=release-iotbasic). Weitere Informationen zu den verschiedenen Dienstebenen von IoT Hub und der besten Option für Ihre Skalierbarkeitsanforderungen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/iot-hub/).

## <a name="secure-your-communications"></a>Sichern der Kommunikation

IoT Hub bietet einen sicheren Kommunikationskanal, über den Ihre Geräte Daten senden können.

* Dank der gerätebasierten Authentifizierung kann jedes Gerät eine sichere Verbindung mit IoT Hub herstellen und sicher verwaltet werden.

* Sie verfügen über vollständige Kontrolle über den Gerätezugriff und können Verbindungen pro Gerät steuern.

* Der [IoT Hub Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps/) stellt Geräte automatisch für die richtige IoT Hub-Instanz bereit, wenn das Gerät zum ersten Mal gestartet wird.

* Mehrere Authentifizierungstypen unterstützen eine Vielzahl von Gerätefunktionen:

  * Tokenbasierte SAS-Authentifizierung, damit Sie schnell mit Ihrer IoT-Lösung loslegen können.

  * Individuelle X.509-Zertifikate für eine sichere, standardbasierte Authentifizierung.

  * Authentifizierung über X.509-Zertifizierungsstellen für eine einfache, standardbasierte Registrierung.

## <a name="route-device-data"></a>Weiterleiten von Gerätedaten

Integrierte Funktionen für die Nachrichtenweiterleitung sorgen für Flexibilität bei der Einrichtung einer automatischen regelbasierten Nachrichtenauffächerung:

* Verwenden Sie die [Nachrichtenweiterleitung](iot-hub-devguide-messages-d2c.md), um zu steuern, wohin Ihr Hub Gerätetelemetriedaten sendet.

* Bei der Weiterleitung von Nachrichten an mehrere Endpunkte fallen keine zusätzlichen Kosten an.

* Routingregeln, für die keinerlei Code erforderlich ist, ersetzen benutzerdefinierten Nachrichtenverteilercode.

## <a name="integrate-with-other-services"></a>Integration in andere Dienste

Sie können IoT Hub in andere Azure-Dienste integrieren, um vollständige End-to-End-Lösungen zu erstellen. Verwenden Sie z.B. Folgendes:

* [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/): Mit diesem Dienst können Sie schnell, zuverlässig, skalierbar und sicher auf kritische Ereignisse reagieren.

* [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/): Mit diesem Dienst automatisieren Sie Ihre Geschäftsprozesse.

* [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/): Mit diesem Dienst fügen Sie Ihrer Lösung Modelle für maschinelles Lernen und KI hinzu.

* [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/): Mit diesem Dienst führen Sie in Echtzeit Analyseberechnungen für die Datenströme aus Ihren Geräten aus.

## <a name="configure-and-control-your-devices"></a>Konfigurieren und Steuern von Geräten

Sie können Ihre mit IoT Hub verbundenen Geräte mit einer Vielzahl von integrierten Funktionen verwalten.

* Sie können Metadaten und Statusinformationen für all Ihre Geräte speichern, synchronisieren und abfragen.

* Legen Sie den Gerätestatus entweder pro Gerät oder basierend auf gemeinsamen Gerätemerkmalen fest.

* Dank der Integration von Nachrichtenweiterleitungsfunktionen können Sie die Reaktion auf eine von einem Gerät gemeldete Statusänderung automatisieren.

## <a name="make-your-solution-highly-available"></a>Hochverfügbarkeit für Ihre Lösung

Für [IoT Hub besteht eine Vereinbarung zum Servicelevel](https://azure.microsoft.com/support/legal/sla/iot-hub/) von 99,9 %. Die vollständige [Azure-SLA](https://azure.microsoft.com/support/legal/sla/) erläutert die garantierte Verfügbarkeit von Azure insgesamt.

## <a name="connect-your-devices"></a>Verbinden von Geräten

Verwenden Sie die Bibliotheken des [Azure IoT-Geräte-SDK](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks), um Anwendungen zu erstellen, die auf Ihren Geräten ausgeführt werden und mit IoT Hub interagieren. Zu den unterstützten Plattformen gehören verschiedene Linux-Distributionen, Windows und Echtzeit-Betriebssysteme. Unterstützte Sprachen:

* C
* C#
* Java
* Python
* Node.js

IoT Hub und die Geräte-SDKs unterstützen die folgenden Protokolle zum Verbinden von Geräten:

* HTTPS
* AMQP
* AMQP über WebSockets
* MQTT
* MQTT über WebSockets

Falls Ihre Lösung die Gerätebibliotheken nicht nutzen kann, können die Geräte die Protokolle MQTT v3.1.1, HTTPS 1.1 oder AMQP 1.0 verwenden, um nativ eine Verbindung mit Ihrem Hub herzustellen.

Wenn Ihre Lösung keines der unterstützten Protokolle nutzen kann, können Sie IoT Hub so erweitern, dass benutzerdefinierte Protokolle unterstützt werden:

* Verwenden Sie [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/), um ein Bereichsgateway zu erstellen, das die Protokollübersetzung im Edge ausführt.

* Passen Sie das [Azure IoT-Protokollgateway](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) so an, dass die Protokollübersetzung in der Cloud erfolgt.

## <a name="quotas-and-limits"></a>Kontingente und Grenzwerte

Für jedes Azure-Abonnement gelten standardmäßig bestimmte Kontingentgrenzen, um den Missbrauch von Diensten zu verhindern. Diese Grenzwerte können den Umfang Ihrer IoT-Lösung beeinträchtigen. Der derzeitige Grenzwert liegt bei 50 IoT Hub-Instanzen pro Abonnement. Wenden Sie sich an den Support, um eine Erhöhung des Kontingents anzufordern. Weitere Informationen finden Sie unter [IoT Hub-Kontingente und -Drosselung](iot-hub-devguide-quotas-throttling.md). Weitere Details zu Kontingentgrenzen finden Sie in einem der folgenden Artikel:

* [Einschränkungen bei Azure-Abonnementdiensten](../azure-subscription-service-limits.md)

* [IoT Hub throttling and you](https://azure.microsoft.com/blog/iot-hub-throttling-and-you/)

## <a name="next-steps"></a>Nächste Schritte

Um eine umfassende IoT-Lösung zu testen, lesen Sie die Schnellstartanleitungen zu IoT Hub:

* [Schnellstart: Senden von Telemetriedaten von einem Gerät an einen IoT-Hub](quickstart-send-telemetry-node.md)