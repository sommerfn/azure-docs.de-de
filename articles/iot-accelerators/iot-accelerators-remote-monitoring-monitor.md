---
title: 'Überwachen von Geräten in der Remoteüberwachungslösung: Azure | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie Ihre IoT-Geräte mit dem Solution Accelerator für die Remoteüberwachung überwachen.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 400a71b11fde210b889d938041e88c5ebe73c1dc
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73890869"
---
# <a name="tutorial-monitor-your-iot-devices"></a>Tutorial: Überwachen Ihrer IoT-Geräte

In diesem Tutorial verwenden Sie den Solution Accelerator für die Remoteüberwachung, um Ihre verbundenen IoT-Geräte zu überwachen. Sie nutzen das Lösungsdashboard, um Telemetriedaten, Geräteinformationen, Warnungen und KPIs anzuzeigen.

Im Tutorial werden zwei Geräte verwendet, die LKW simulieren und Telemetriedaten zu Standort, Geschwindigkeit und Temperatur der Ladung senden. Die LKW werden von der Organisation Contoso verwaltet und sind mit dem Solution Accelerator für die Remoteüberwachung verbunden. Als Contoso-Bediener müssen Sie den Standort und das Verhalten von einem Ihrer LKW (truck-02) während der Fahrt überwachen.

In diesem Tutorial führen Sie Folgendes durch:

>[!div class="checklist"]
> * Filtern der Geräte im Dashboard
> * Anzeigen von Echtzeit-Telemetriedaten
> * Anzeigen von Gerätedetails
> * Anzeigen von Benachrichtigungen Ihrer Geräte
> * Anzeigen der System-KPIs

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="choose-the-devices-to-display"></a>Auswählen der anzuzeigenden Geräte

Verwenden Sie Filter, um festzulegen, welche verbundenen Geräte auf der Seite **Dashboard** angezeigt werden. Um nur die **Truck**-Geräte anzuzeigen, wählen Sie in der Dropdownliste zum Filtern den integrierten Filter **Trucks** aus:

[![Filtern nach LKW im Dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-expanded.png#lightbox)

Wenn Sie einen Filter anwenden, werden nur die Geräte, die den Filterbedingungen entsprechen, auf der Karte und im Telemetriebereich angezeigt. Sie sehen, dass zwei LKW mit dem Solution Accelerator verbunden sind, darunter auch „truck-02“:

[![Nur LKW werden auf der Karte angezeigt](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-expanded.png#lightbox)

Klicken Sie auf **Manage device groups** (Gerätegruppen verwalten), um Filter zu erstellen, zu bearbeiten und zu löschen.

## <a name="view-real-time-telemetry"></a>Anzeigen von Echtzeit-Telemetriedaten

Der Solution Accelerator stellt im Diagramm auf der Seite **Dashboard** Echtzeit-Telemetriedaten dar. Oben im Diagramm mit den Telemetriedaten werden die verfügbaren Telemetriedatentypen für die Geräte angezeigt, z.B. „truck-02“, die mit dem aktuellen Filter ausgewählt wurden. Standardmäßig wird im Diagramm der Breitengrad der LKW angezeigt, und „truck-02“ ist scheinbar gerade nicht in Bewegung:

[![Typen von LKW-Telemetriedaten](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-expanded.png#lightbox)

Klicken Sie auf **Temperatur**, um für die LKW die Telemetriedaten zur Temperatur anzuzeigen. Sie sehen, wie sich die Temperatur für „truck-02“ während der letzten Stunde verändert hat:

[![Darstellung der Telemetriedaten für die LKW-Temperatur](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-expanded.png#lightbox)

## <a name="view-the-map"></a>Anzeigen der Karte

Auf der Karte werden Informationen zu den simulierten Trucks angezeigt, die durch den aktuellen Filter ausgewählt sind. Sie können die Karte zoomen und schwenken, um Positionen mehr oder weniger detailliert anzuzeigen. Mit der Farbe eines Gerätesymbols auf der Karte wird angegeben, ob für das Gerät **Alerts** (Benachrichtigungen) (dunkelblau) oder **Warnings** (Warnungen) (rot) vorliegen. Eine Zusammenfassung der Anzahl von **Benachrichtigungen** und **Warnungen** wird auf der linken Seite der Karte angezeigt.

Zum Anzeigen der Details für „truck-02“ können Sie die Karte schwenken und zoomen, um den LKW zu finden, und diesen anschließend in der Karte auswählen. Klicken Sie dann auf die Gerätebezeichnung, um das Panel **Device details** (Gerätedetails) zu öffnen. Zu den Gerätedetails zählt Folgendes:

* Aktuelle Telemetriewerte
* Vom Gerät unterstützte Methoden
* Geräteeigenschaften

[![Anzeigen von Gerätedetails im Dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-expanded.png#lightbox)

## <a name="view-alerts"></a>Anzeigen von Warnungen

Im Bereich **Benachrichtigungen** werden detaillierte Informationen zu den neuesten Benachrichtigungen von den Geräten angezeigt. Die Benachrichtigungen für „truck-02“ weisen auf eine Ladungstemperatur hin, die höher als normal ist:

[![Anzeigen von Gerätebenachrichtigungen im Dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-expanded.png#lightbox)

Mit einem Filter können Sie die Zeitspanne für aktuelle Benachrichtigungen anpassen. Standardmäßig werden die Benachrichtigungen der letzten Stunde angezeigt.

## <a name="view-the-system-kpis"></a>Anzeigen der System-KPIs

Auf der Seite **Dashboard** werden vom Solution Accelerator berechnete System-KPIs im Panel **Analytics** (Analyse) angezeigt:

[![Dashboard-KPIs](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-expanded.png#lightbox)

Im Dashboard werden drei KPIs für die Benachrichtigungen angezeigt, die mit den aktuellen Filtern für Geräte und die Zeitspanne ausgewählt wurden:

* Die Anzahl von aktiven Benachrichtigungen für die Regeln, die die meisten Benachrichtigungen ausgelöst haben.
* Der Anteil von Benachrichtigungen nach dem Gerätetyp.
* Der Prozentsatz an Benachrichtigungen, die kritisch sind.

Für „truck-02“ handelt es sich bei allen Benachrichtigungen um Warnungen, dass die Temperatur der Ladung höher als normal ist.

Die gleichen Filter, mit denen die Zeitspanne für Benachrichtigungen festgelegt und mit denen gesteuert wird, welche Geräte angezeigt werden, bestimmen die Art der Aggregierung von KPIs. Standardmäßig werden die während der letzten Stunde aggregierten KPIs im Bereich angezeigt.

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde veranschaulicht, wie Sie die Seite **Dashboard** im Solution Accelerator für die Remoteüberwachung verwenden, um die simulierten LKW zu filtern und zu überwachen. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie den Solution Accelerator zum Erkennen von Problemen für Ihre verbundenen Geräte verwenden.

> [!div class="nextstepaction"]
> [Erkennen von Problemen mit Geräten, die mit der Überwachungslösung verbunden sind](iot-accelerators-remote-monitoring-automate.md)