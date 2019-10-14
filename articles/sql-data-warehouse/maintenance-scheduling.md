---
title: Azure-Wartungszeitpläne (Vorschau) | Microsoft-Dokumentation
description: Wartungszeitpläne ermöglichen es Kunden, die notwendigen geplanten Wartungsereignisse in ihre Planungen einzubeziehen, mit denen der Azure SQL Data Warehouse-Dienst neue Features, Upgrades und Patches einführt.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 07/16/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: 0e5df583112bbb6db9651004e5deaceb6b5b9d12
ms.sourcegitcommit: 7868d1c40f6feb1abcafbffcddca952438a3472d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71958891"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Verwenden von Wartungszeitplänen zum Verwalten der Updates und Wartung von Diensten

Wartungszeitpläne sind jetzt für alle Azure SQL Data Warehouse-Regionen verfügbar. Dieses Wartungszeitplan-Feature kann in Benachrichtigungen über eine geplante Wartung von Service Health, in Resource Health Check Monitor und in den Zeitplanungsdienst für Azure SQL Data Warehouse-Wartungen integriert werden.

Sie verwenden Wartungszeitpläne, um ein passendes Zeitfenster für den Erhalt neuer Features, Upgrades und Patches zu wählen. Sie wählen ein primäres und ein sekundäres Wartungsfenster für einen Zeitraum von sieben Tagen. Zum Verwenden dieses Features müssen Sie ein primäres und sekundäres Fenster in separaten Tagesbereiche festlegen.

Sie können beispielsweise ein primäres Fenster von Samstag 22:00 Uhr bis Sonntag 01:00 Uhr und dann ein sekundäres Fenster von Mittwoch 19:00 Uhr bis 22:00 Uhr planen. Wenn SQL Data Warehouse die Wartung nicht während Ihres primären Wartungsfensters durchführen kann, wird versucht, die Wartung während des sekundären Wartungsfensters durchzuführen. Dienstwartungen können sowohl im primären als auch in sekundären Fenstern auftreten. Um einen schnellen Abschluss aller Wartungsarbeiten zu gewährleisten, könnten DW400(c) und niedrigere Data Warehouse-Ebenen die Wartung außerhalb eines bestimmten Wartungsfensters abschließen.

Auf alle neu erstellten Azure SQL Data Warehouse-Instanzen muss während der Bereitstellung ein systemdefinierter Wartungszeitplan angewendet werden. Der Zeitplan kann direkt nach Abschluss der Bereitstellung bearbeitet werden.

Jedes Wartungsfenster kann zwischen drei und acht Stunden lang sein. Die Wartung kann zu einem beliebigen Zeitpunkt des Fensters erfolgen. Beim Start der Wartung werden alle aktiven Sitzungen abgebrochen, und für Transaktionen, für die kein Commit ausgeführt wurde, wird ein Rollback durchgeführt. Es kommt ggf. zu mehreren kurzzeitigen Verbindungsverlusten, während der Dienst neuen Code in Ihrem Data Warehouse bereitstellt. Sie werden umgehend benachrichtigt, wenn die Wartung für Ihr Data Warehouse abgeschlossen wurde.

 Alle Wartungsvorgänge sollten innerhalb der geplanten Wartungsfenster abgeschlossen werden. Außerhalb der angegebenen Wartungsfenster werden ohne vorherige Benachrichtigung keine Wartungen durchgeführt. Wenn Ihr Data Warehouse während einer geplanten Wartung angehalten wird, wird es während des Wiederaufnahmevorgangs aktualisiert. 

## <a name="alerts-and-monitoring"></a>Benachrichtigungen und Überwachung

Dank der Integration in Service Health-Benachrichtigungen und in Resource Health Check Monitor sind Kunden immer über bevorstehende Wartungsaktivitäten informiert. Für die neue Automatisierung wird Azure Monitor genutzt. Sie können entscheiden, wie Sie über anstehende Wartungsereignisse benachrichtigt werden möchten. Außerdem können Sie auswählen, welche automatisierten Abläufe Ihnen helfen, Ausfallzeiten zu bewältigen und die Auswirkungen auf den Betrieb zu minimieren.

Eine Benachrichtigung 24 Stunden im Voraus geht allen Wartungsereignissen voran, die nicht für DWC400c und niedrigere Ebenen gelten. Stellen Sie zum Verringern der Ausfallzeiten von Instanzen sicher, dass Ihr Data Warehouse vor dem gewählten Wartungszeitraum keine Transaktionen mit langer Ausführungsdauer aufweist.

> [!NOTE]
> Wenn ein zeitkritisches Update erforderlich ist, können die Vorabbenachrichtigung deutlich früher gesendet werden.

Wenn Sie eine Vorankündigung zur Durchführung von Wartungsarbeiten erhalten haben, die Wartung von SQL Data Warehouse im vorgesehenen Zeitraum aber nicht durchgeführt werden kann, erhalten Sie eine Widerrufsbenachrichtigung. Die Wartung wird dann während des nächsten geplanten Wartungszeitraums durchgeführt.

Alle aktiven Wartungsereignisse werden im Abschnitt **Service Health – Geplante Wartung** angezeigt. Der Service Health-Verlauf enthält eine vollständige Aufzeichnung der vergangenen Ereignisse. Sie können die Wartung während eines aktiven Ereignisses über das Dashboard des Azure Service Health Check-Portals überwachen.

### <a name="maintenance-schedule-availability"></a>Verfügbarkeit des Wartungszeitplans

Auch wenn Wartungszeitpläne in Ihrer ausgewählten Region noch nicht verfügbar sind, können Sie Ihren Wartungszeitplan jederzeit anzeigen und bearbeiten. Sobald Wartungszeitpläne in Ihrer Region verfügbar sind, wird der identifizierte Zeitplan in Ihrem Data Warehouse sofort aktiv.

## <a name="view-a-maintenance-schedule"></a>Anzeigen eines Wartungszeitplans 

### <a name="portal"></a>Portal

Alle neu erstellten Azure SQL Data Warehouse-Instanzen verfügen über ein achtstündiges primäres und sekundäres Wartungsfenster, das während der Bereitstellung angewendet wird. Wie oben angegeben, können Sie die Fenster nach Abschluss der Bereitstellung ändern. Außerhalb der angegebenen Wartungsfenster werden ohne vorherige Benachrichtigung keine Wartungen durchgeführt.

Führen Sie die folgenden Schritte aus, um den Wartungszeitplan anzuzeigen, der auf Ihr Data Warehouse angewendet wurde:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2.  Wählen Sie das Data Warehouse aus, das Sie anzeigen möchten. 
3.  Das ausgewählte Data Warehouse wird auf dem Blatt „Übersicht“ geöffnet. Der Wartungszeitplan, der auf das Data Warehouse angewandt wurde, wird unter **Wartungszeitplan** angezeigt.

![Blatt „Übersicht“](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="change-a-maintenance-schedule"></a>Ändern des Wartungszeitplans 

### <a name="portal"></a>Portal
Ein Wartungszeitplan kann jederzeit aktualisiert oder geändert werden. Falls die ausgewählte Instanz gerade einen aktiven Wartungszyklus durchläuft, werden die Einstellungen gespeichert. Sie werden dann im nächsten identifizierten Wartungszeitraum angewendet. Weitere Informationen zum Überwachen Ihrer Data Warehouse-Daten während eines aktiven Verwaltungsereignisses finden Sie [hier](https://docs.microsoft.com/azure/service-health/resource-health-overview). 

### <a name="identifying-the-primary-and-secondary-windows"></a>Angeben des primären und sekundären Fensters

Primäre und sekundäre Fenster müssen jeweils separate Tagesbereiche besitzen. Ein Beispiel wäre etwa ein primäres Fenster von Dienstag bis Donnerstag und ein sekundäres Fenster von Samstag bis Sonntag.

Führen Sie die folgenden Schritte aus, um den Wartungszeitplan für Ihr Data Warehouse zu ändern:
1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2.  Wählen Sie das Data Warehouse aus, das Sie aktualisieren möchten. Die Seite wird auf dem Blatt „Übersicht“ geöffnet. 
3.  Klicken Sie auf dem Übersichtsblatt auf den Link **Maintenance Schedule (preview) summary** (Wartungszeitplan (Vorschauversion) – Zusammenfassung), um die Seite mit den Wartungszeitplaneinstellungen zu öffnen. Alternativ können Sie auch im Ressourcenmenü auf der linken Seite auf **Wartungszeitplan** klicken.  

    ![Optionen auf dem Blatt „Übersicht“](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. Geben Sie mithilfe der Optionen im oberen Seitenbereich die bevorzugte Zeitspanne (in Tagen) für Ihr primäres Wartungsfenster an. Dadurch bestimmen Sie, ob Ihr primäres Fenster Werktage oder Wochenendtage abdeckt. Die Dropdownwerte werden auf der Grundlage Ihrer Auswahl aktualisiert. Während der Vorschauphase unterstützen einige Regionen ggf. noch nicht alle verfügbaren Optionen für **Tag**.

   ![Blatt „Wartungseinstellungen“](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Wählen Sie über die Dropdown-Listenfelder Ihr bevorzugtes primäres und sekundäres Wartungsfenster aus:
   - **Tag**: Der bevorzugte Tag, an dem die Wartungsarbeiten innerhalb des ausgewählten Zeitfensters durchgeführt werden sollen.
   - **Startzeit**: Die bevorzugte Startzeit für das Wartungsfenster.
   - **Zeitfenster**: Die bevorzugte Dauer des Zeitfensters.

   Der Bereich **Zusammenfassung des Zeitplans** am unteren Rand des Blatts wird anhand der ausgewählten Werte aktualisiert. 
  
6. Wählen Sie **Speichern** aus. Eine Meldung bestätigt, dass Ihr neuer Zeitplan jetzt aktiv ist. 

   Wenn Sie einen Zeitplan in einer Region speichern, in der keine Wartungszeitpläne unterstützt werden, wird die folgende Meldung angezeigt. Ihre Einstellungen werden gespeichert und angewendet, sobald das Feature auch in Ihrer Region verfügbar ist.    

   ![Meldung zur regionalen Verfügbarkeit](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) zum Erstellen, Anzeigen und Verwalten von Warnungen mit Azure Monitor
- [Weitere Informationen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) zu Webhookaktionen für Protokollwarnungsregeln
- [Weitere Informationen ](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) zum Erstellen und Verwalten von Aktionsgruppen.
- [Weitere Informationen](https://docs.microsoft.com/azure/service-health/service-health-overview) zu Azure Service Health