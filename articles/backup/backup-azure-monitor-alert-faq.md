---
title: Azure Backup-Überwachungswarnungen – häufig gestellte Fragen
description: In diesem Artikel finden Sie Antworten auf häufige Fragen zu den Berichten von Azure Backup-Überwachungswarnungen und Azure Backup.
ms.reviewer: srinathv
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: dacurwin
ms.openlocfilehash: dc0225092d23371ca97dfedd48a2d3ffcf85a9be
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73747397"
---
# <a name="azure-backup-monitoring-alert---faq"></a>Azure Backup-Überwachungswarnungen – häufig gestellte Fragen

In diesem Artikel werden häufig gestellte Fragen zur Azure-Überwachungswarnung beantwortet.

## <a name="configure-azure-backup-reports"></a>Konfigurieren von Azure Backup-Berichten

### <a name="how-do-i-check-if-reporting-data-has-started-flowing-into-a-storage-account"></a>Wie überprüfe ich, ob bereits Berichtsdaten an ein Speicherkonto übermittelt werden?

Öffnen Sie das konfigurierte Speicherkonto, und wählen Sie Container aus. Wenn der Container einen Eintrag für „insights-logs-azurebackupreport“ aufweist, bedeutet dies, dass Berichtsdaten übermittelt werden.

### <a name="what-is-the-frequency-of-data-push-to-a-storage-account-and-the-azure-backup-content-pack-in-power-bi"></a>Wie oft werden Daten an das Speicherkonto und das Azure Backup-Inhaltspaket in Power BI mithilfe von Push übertragen?

  Nach dem Aktivieren dauert es etwa 24 Stunden, bis Daten mithilfe von Push an ein Speicherkonto übertragen werden. Nach dem ersten Push gelten für die Aktualisierungshäufigkeit der Daten die Angaben in der folgenden Abbildung.

* Daten zu **Aufträgen**, **Warnungen**, **Sicherungselementen**, **Tresoren**, **geschützten Servern** und **Richtlinien** werden beim Protokollieren mithilfe von Push an das Speicherkonto des Kunden übertragen.

* Daten bezüglich des **Speichers** werden alle 24 Stunden mithilfe von Push an das Speicherkonto des Kunden übertragen.

    ![Übermittlungshäufigkeit von Azure Backup-Berichtsdaten](./media/backup-azure-configure-reports/reports-data-refresh-cycle.png)

* Power BI verfügt über eine [täglich geplante Aktualisierung](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/#what-can-be-refreshed). Sie können die Daten in Power BI für das Inhaltspaket manuell aktualisieren.

### <a name="how-long-can-i-retain-reports"></a>Wie lange kann ich Berichte aufbewahren?

Beim Einrichten eines Speicherkontos können Sie einen Aufbewahrungszeitraum für Berichtsdaten auswählen. Führen Sie Schritt 6 im Abschnitt [Konfigurieren des Speicherkontos für Berichte](backup-azure-configure-reports.md#configure-storage-account-for-reports) aus. Außerdem können Sie [Berichte in Excel analysieren](https://powerbi.microsoft.com/documentation/powerbi-service-analyze-in-excel/) und sie Ihren Anforderungen entsprechend für einen längeren Zeitraum aufbewahren.

### <a name="will-i-see-all-my-data-in-reports-after-i-configure-the-storage-account"></a>Werden alle meine Daten in Berichten nach dem Konfigurieren des Speicherkontos angezeigt?

 Alle Daten, die nach der Konfiguration des Speicherkontos generiert werden, werden mithilfe von Push an das Speicherkonto übertragen und stehen in den Berichten zur Verfügung. Aufträge, die sich in Bearbeitung befinden, werden nicht mithilfe von Push für die Berichterstellung übertragen. Wenn ein Auftrag abgeschlossen wird oder fehlschlägt, wird er an die Berichte gesendet.

### <a name="if-i-already-configured-the-storage-account-to-view-reports-can-i-change-the-configuration-to-use-another-storage-account"></a>Ich habe das Speicherkonto bereits so konfiguriert, dass Berichte angezeigt werden. Kann ich die Konfiguration ändern, um ein anderes Speicherkonto zu verwenden?

Ja, Sie können die Konfiguration ändern und auf ein anderes Speicherkonto zeigen. Verwenden Sie beim Verbinden mit dem Azure Backup-Inhaltspaket das neu konfigurierte Speicherkonto. Sobald ein anderes Speicherkonto konfiguriert ist, werden neue Daten an dieses Speicherkonto übermittelt. Daten, die vor der Konfigurationsänderung stammen, verbleiben im älteren Speicherkonto.

### <a name="can-i-view-reports-across-vaults-and-subscriptions"></a>Kann ich Berichte für verschiedene Tresore und Abonnements anzeigen?

Ja, Sie können dasselbe Speicherkonto für verschiedene Tresore konfigurieren, um tresorübergreifende Berichte anzuzeigen. Sie können auch dasselbe Speicherkonto für Tresore in verschiedenen Abonnements verwenden. Dieses Speicherkonto lässt sich dann beim Verbinden mit dem Azure Backup-Inhaltspaket in Power BI verwenden, um die Berichte anzuzeigen. Das ausgewählte Speicherkonto muss sich in der gleichen Region wie der Recovery Services-Tresor befinden.

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Wie lange dauert es, bis der Auftragsstatus des Azure Backup-Agents im Portal angezeigt wird?

Es kann bis zu 15 Minuten dauern, bis der Status des Azure Backup-Agent-Auftrags im Azure-Portal angezeigt wird.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Wie lange dauert es, bis eine Warnung ausgelöst wird, wenn ein Sicherungsauftrag nicht erfolgreich ist?

Innerhalb von 20 Minuten nach dem Azure-Sicherungsfehler wird eine Warnung ausgelöst.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Kann es vorkommen, dass eine E-Mail nicht gesendet wird, wenn Benachrichtigungen konfiguriert sind?

Ja. In den folgenden Situationen werden keine Benachrichtigungen gesendet.

* Stündliche Benachrichtigungen wurden konfiguriert, und eine Warnung wird ausgelöst und innerhalb dieser Stunde gelöst.
* Ein Auftrag wurde abgebrochen.
* Bei einem zweiten Sicherungsauftrag tritt ein Fehler auf, weil der ursprüngliche Sicherungsauftrag noch ausgeführt wird.

## <a name="recovery-services-vault"></a>Recovery Services-Tresor

### <a name="how-long-does-it-take-for-the-azure-backup-agent-job-status-to-reflect-in-the-portal"></a>Wie lange dauert es, bis der Auftragsstatus des Azure Backup-Agents im Portal angezeigt wird?

Es kann bis zu 15 Minuten dauern, bis der Status des Azure Backup-Agent-Auftrags im Azure-Portal angezeigt wird.

### <a name="when-a-backup-job-fails-how-long-does-it-take-to-raise-an-alert"></a>Wie lange dauert es, bis eine Warnung ausgelöst wird, wenn ein Sicherungsauftrag nicht erfolgreich ist?

Innerhalb von 20 Minuten nach dem Azure-Sicherungsfehler wird eine Warnung ausgelöst.

### <a name="is-there-a-case-where-an-email-wont-be-sent-if-notifications-are-configured"></a>Kann es vorkommen, dass eine E-Mail nicht gesendet wird, wenn Benachrichtigungen konfiguriert sind?

Ja. In den folgenden Situationen werden keine Benachrichtigungen gesendet:

* Stündliche Benachrichtigungen wurden konfiguriert, und eine Warnung wird ausgelöst und innerhalb dieser Stunde gelöst.
* Ein Auftrag wurde abgebrochen.
* Bei einem zweiten Sicherungsauftrag tritt ein Fehler auf, weil der ursprüngliche Sicherungsauftrag noch ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die anderen häufig gestellten Fragen:

* [Gängige Fragen](backup-azure-vm-backup-faq.md) zu Azure VM-Sicherungen
* [Gängige Fragen](backup-azure-file-folder-backup-faq.md) zum Azure Backup-Agent
