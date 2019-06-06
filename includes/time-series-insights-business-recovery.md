---
title: Includedatei
description: Includedatei
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: e87a82e985ed1d1794f9da00546f167ef01e1779
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2019
ms.locfileid: "66147452"
---
## <a name="business-disaster-recovery"></a>Business Disaster Recovery

Dieser Abschnitt beschreibt Features von Azure Time Series Insights, die den Betrieb von Apps und Diensten auch im Katastrophenfall sicherstellen (**Business Disaster Recovery**).

### <a name="high-availability"></a>Hochverfügbarkeit

Als Azure-Dienst bietet Time Series Insights bestimmte **Hochverfügbarkeitsfeatures**, indem Redundanzen auf Azure-Regionsebene bereitgestellt werden. Beispielsweise unterstützt Microsoft Azure dank des Features für die **regionsübergreifende Verfügbarkeit** von Azure Funktionen für die Notfallwiederherstellung.

Zu den zusätzlichen Hochverfügbarkeitsfeatures von Azure (auch für alle Zeitreihen-Insights-Instanzen verfügbar) gehören:

1. **Failover**: Azure bietet [Georeplikation und Lastenausgleich](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
1. **Daten-** und **Speicherwiederherstellung**: Azure bietet [mehrere Optionen zum Beibehalten und Wiederherstellen von Daten](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
1. Features für **Sitewiederherstellung** über [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Stellen Sie sicher, dass diese Azure-Features aktiviert sind, um eine globale, regionsübergreifende Hochverfügbarkeit für Ihre Geräte und Benutzer zu gewährleisten.

> [!NOTE]
> Wenn Azure konfiguriert ist, um **regionsübergreifende Verfügbarkeit** zu aktivieren, ist innerhalb von Azure Time Series Insights keine zusätzliche Konfiguration der regionsübergreifenden Verfügbarkeit erforderlich.

### <a name="iot-and-event-hubs"></a>IoT und Event Hubs

Einige Azure IoT-Dienste enthalten auch integrierte Business Disaster Recovery-Features:

1. [Hochverfügbarkeit und Notfallwiederherstellung von IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr) einschließlich intraregionaler Redundanz.
1. [Event Hub-Richtlinien](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).
1. [Azure Storage-Redundanz](https://docs.microsoft.com/azure/storage/common/storage-redundancy).

Die Integration von Time Series Insights mit diesen anderen Diensten bietet zusätzliche Möglichkeiten für die Notfallwiederherstellung. Beispielsweise können an Ihren Event Hub gesendete Telemetriedaten in einer Sicherungsdatenbank von Azure Blob Storage gespeichert werden.

### <a name="time-series-insights"></a>Time Series Insights

Es gibt mehrere Möglichkeiten, den Betrieb Ihrer Daten, Anwendungen und Dienste von Time Series Insights aufrechtzuerhalten, auch wenn sie gestört sind. Möglicherweise ist auch eine vollständige, duplizierte Sicherungskopie Ihrer Azure Time Series-Umgebung erforderlich:

1. Als eine Time Series Insights-spezifische **Failoverinstanz** zur der Daten und Datenverkehr umgeleitet werden.
1. Für Überwachungs- und Datensicherungszwecke.

Im Allgemeinen ist der beste Weg, eine Time Series Insights-Umgebung zu duplizieren, die Erstellung einer zweiten Time Series Insights-Umgebung in einer Sicherungs-Azure-Region. Ereignisse werden auch von Ihrer primären Ereignisquelle Ereignisse an diese sekundäre Umgebung gesendet. Stellen Sie sicher, dass Sie eine zweite, dedizierte Consumergruppe verwenden und die Business Disaster Recovery-Richtlinien dieser Quelle (siehe oben) befolgen.

Insbesondere beim Erstellen einer Duplikatumgebung:

1. Erstellen Sie eine Umgebung in einer zweiten Region ([Erstellen einer neuen Time Series Insights-Umgebung über das Azure-Portal](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started)).
1. Erstellen Sie eine zweite dedizierte Consumergruppe für Ihre Ereignisquelle.
1. Verbinden Sie diese Ereignisquelle mit der neuen Umgebung, und stellen Sie sicher, dass Sie die zweite, dedizierte Consumergruppe angeben.
1. Machen Sie sich mit der [IoT Hub-](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) und [Event Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access)-Dokumentation für Time Series Insights vertraut.

Letzter Schritt:

* Wenn Ihre primäre Region während eines Notfalls betroffen ist, leiten Sie den Betrieb in die Time Series Insights-Sicherungsumgebung um.
* Verwenden Sie Ihre zweite Region, um alle Time Series Insights-Telemetrie- und Abfragedaten zu sichern und wiederherzustellen.

> [!IMPORTANT]
> * Beachten Sie, dass es im Falle eines Failovers zu einer Verzögerung kommen kann.
> * Ein Failover kann auch eine momentane Spitze bei der Nachrichtenverarbeitung verursachen, wenn der Betrieb und Vorgänge umgeleitet werden.
> * Weitere Informationen finden Sie unter [Mildern von Wartezeiten in Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).