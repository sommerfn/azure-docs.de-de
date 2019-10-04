---
title: Architektonische Konzepte in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Artikel werden die wichtigsten Konzepte in Bezug auf die Architektur von Azure IoT Central vorgestellt.
author: dominicbetts
ms.author: dobett
ms.date: 05/31/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: fab65828228bdea8f0367f8928b83934bf0a7a4f
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69995872"
---
# <a name="azure-iot-central-architecture-preview-features"></a>Azure IoT Central-Architektur (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Dieser Artikel enthält eine Übersicht über die Architektur von Microsoft Azure IoT Central.

![Architektur im Überblick](media/concepts-architecture-pnp/architecture.png)

## <a name="devices"></a>Geräte

Geräte tauschen Daten mit Ihrer Azure IoT Central-Anwendung aus. Ein Gerät kann Folgendes durchführen:

- Senden von Messungen wie Telemetrie
- Synchronisieren von Einstellungen mit Ihrer Anwendung

In Azure IoT Central werden die Daten, die ein Gerät mit Ihrer Anwendung austauschen kann, in einer Gerätevorlage angegeben. Weitere Informationen zu Gerätevorlagen finden Sie unter [Metadatenverwaltung](#metadata-management).

Um mehr darüber zu erfahren, wie Geräte eine Verbindung mit Ihrer Azure IoT Central-Anwendung herstellen können, lesen Sie [Gerätekonnektivität](concepts-connectivity-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="cloud-gateway"></a>Cloudgateway

Azure IoT Central verwendet Azure IoT Hub als Cloudgateway, das Gerätekonnektivität ermöglicht. IoT Hub ermöglicht Folgendes:

- Datenerfassung im großen Maßstab in der Cloud
- Geräteverwaltung
- Sichere Gerätekonnektivität

Weitere Informationen zu IoT Hub finden Sie unter [Dokumentation zu IoT Hub](https://docs.microsoft.com/azure/iot-hub/).

Weitere Informationen zur Gerätekonnektivität in Azure IoT Central finden Sie unter [Gerätekonnektivität](concepts-connectivity-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="data-stores"></a>Datenspeicher

Azure IoT Central speichert Anwendungsdaten in der Cloud. Zu den gespeicherten Anwendungsdaten zählen Folgende:

- Gerätevorlagen
- Geräteidentitäten
- Gerätemetadaten
- Benutzer- und Rollendaten

Azure IoT Central verwendet einen Zeitreihenspeicher für die Messdaten, die von Ihren Geräten gesendet werden. Die Zeitreihendaten von Geräten werden vom Analysedienst verwendet.

## <a name="analytics"></a>Analytics

Der Analysedienst ist für das Generieren von benutzerdefinierten Berichterstellungsdaten zuständig, die die Anwendung anzeigt. Ein Operator kann die [Analysen anpassen](howto-create-analytics.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json), die in der Anwendung angezeigt werden. Der Analysedienst basiert auf [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) und verarbeitet die von Ihren Geräten gesendeten Messdaten.

## <a name="rules-and-actions"></a>Regeln und Aktionen

[Regeln und Aktionen](howto-create-telemetry-rules.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) werden zur Automatisierung von Aufgaben in der Anwendung ausgiebig zusammen verwendet. Ein Ersteller kann Regeln basierend auf Gerätetelemetrie definieren, wie etwa Temperaturen, die einen definierten Schwellenwert definieren. Azure IoT Central verwendet einen Datenstromprozessor, um zu bestimmen, wann die Regelbedingungen erfüllt sind. Wenn eine Regelbedingung erfüllt ist, löst diese eine vom Ersteller definierte Aktion aus. Eine Aktion kann z.B. eine E-Mail senden, um einen Techniker darüber zu benachrichtigen, dass die Temperatur in einem Gerät zu hoch ist.

## <a name="metadata-management"></a>Metadatenverwaltung

In einer Azure IoT Central-Anwendung definieren Gerätevorlagen das Verhalten und die Funktion der Gerätetypen. So gibt z.B. die Gerätevorlage eines Kühlschranks die Telemetrie an, die ein Kühlschrank an Ihre Anwendung sendet.

![Vorlagenarchitektur](media/concepts-architecture-pnp/template-architecture.png)

In einer Gerätevorlage einer IoT Central-Vorschauanwendung gilt Folgendes:

- **Gerätefunktionsmodelle** geben die Funktionen eines Geräts an, z. B. die vom Gerät gesendeten Telemetriedaten, die Eigenschaften, die den Gerätestatus definieren, und die Befehle, auf die das Gerät reagiert. Gerätefunktionen sind in einer oder mehreren Schnittstellen organisiert. Weitere Informationen zu Gerätefunktionsmodellen finden Sie in der Dokumentation zu [IoT Plug & Play](../iot-pnp/overview-iot-plug-and-play.md).
- **Cloudeigenschaften** geben die Eigenschaften an, die IoT Central für ein Gerät speichert. Diese Eigenschaften werden nur in IoT Central gespeichert und nie an ein Gerät gesendet.
- **Ansichten** geben die Dashboards und Formulare an, die der Generator erstellt, damit der Operator die Geräte überwachen und verwalten kann.
- Durch **Anpassungen** kann der Generator einige der Definitionen im Gerätefunktionsmodell überschreiben, damit Sie für die IoT Central-Anwendung relevanter werden.

Eine Anwendung kann ein oder mehrere simulierte und echte Geräte basierend auf den einzelnen Gerätevorlagen verwenden.

## <a name="data-export"></a>Datenexport

In einer Azure IoT Central-Anwendung können Sie einen [fortlaufenden Export Ihrer Daten](howto-export-data-event-hubs-service-bus-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) auf Ihre eigenen Azure Event Hubs und Azure Service Bus-Instanzen durchführen. Außerdem können Sie Ihre Daten in regelmäßigen Abständen in Ihr Azure-Blobspeicherkonto exportieren. Mit IoT Central können Messungen, Geräte und Gerätevorlagen exportiert werden.

## <a name="batch-device-updates"></a>Geräteupdates als Batchvorgang

In einer Azure IoT Central-Anwendung können Sie [Aufträge erstellen und ausführen](howto-run-a-job.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json), um verbundene Geräte zu verwalten. Mit diesen Aufträgen können Sie Massenaktualisierungen an Geräteeigenschaften oder -einstellungen vornehmen oder Befehle ausführen. Sie können beispielsweise einen Auftrag erstellen, um die Lüfterdrehzahl für mehrere Verkaufsautomaten mit Kühlung zu erhöhen.

## <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)

Ein Administrator kann mithilfe der vordefinierten Rollen [Zugriffsregeln für eine Azure IoT Central-Anwendung definieren](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json). Ein Administrator kann Benutzern Rollen zuweisen, die bestimmen, auf welche Anwendungsbereiche der Benutzer Zugriff hat.

## <a name="security"></a>Sicherheit

Zu den Sicherheitsfeatures in Azure IoT Central gehören Folgende:

- Während der Übertragung und im Ruhezustand verschlüsselte Daten
- Die von Azure Active Directory oder vom Microsoft-Konto bereitgestellte Authentifizierung. Unterstützung für zweistufige Authentifizierung
- Vollständige Mandantenisolation
- Sicherheit auf Geräteebene

## <a name="ui-shell"></a>UI Shell

Die UI Shell ist eine moderne, reaktionsfähige und browserbasierte HTML5-Anwendung.
Ein Administrator kann die Benutzeroberfläche der Anwendung anpassen, indem er benutzerdefinierte Designs anwendet und die Hilfelinks so ändert, dass sie auf die eigenen benutzerdefinierten Hilferessourcen verweisen. Weitere Informationen zur Anpassung der Benutzeroberfläche finden Sie im Artikel [Anpassen der Azure IoT Central-Benutzeroberfläche](howto-customize-ui.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

Ein Bediener kann personalisierte Anwendungsdashboards erstellen. Sie können über mehrere Dashboards mit verschiedene Daten verfügen und zwischen diesen wechseln.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über die Architektur von Azure IoT Central erfahren haben, besteht der nächste empfohlene Schritt darin, sich mit der [Gerätekonnektivität](concepts-connectivity-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) in Azure IoT Central vertraut zu machen.