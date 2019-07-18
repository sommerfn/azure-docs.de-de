---
title: 'Azure Backup: Überwachen von Azure Backup mit Azure Monitor'
description: Überwachen von Azure Backup-Workloads und Erstellen von benutzerdefinierten Warnungen mit Azure Monitor
services: backup
author: pvrk
manager: shivamg
keywords: Log Analytics; Azure Backup; Warnungen Diagnoseeinstellungen; Aktionsgruppen
ms.service: backup
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: pullabhk
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: 7c53d8fe0ee5bbfdbe180aa4d18d8c7b7fab29c2
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295296"
---
# <a name="monitoring-at-scale-using-azure-monitor"></a>Überwachen im gewünschten Umfang mithilfe von Azure Monitor

Der [integrierte Artikel zu Überwachung und Warnungen](backup-azure-monitoring-built-in-monitor.md) listet die Überwachungs- und Warnungsfunktionen in einem einzigen Tresor der Wiederherstellungsdienste auf, der ohne zusätzliche Verwaltungsinfrastruktur angeboten wird. Der integrierte Dienst ist jedoch auf die folgenden Szenarien beschränkt.

- Abonnementübergreifende Überwachung von Daten aus mehreren RS-Tresore
- Wenn E-Mail NICHT der bevorzugte Benachrichtigungskanal ist
- Wenn Benutzer Warnungen für mehrere Szenarien erhalten möchten
- Wenn Sie Informationen aus einer lokalen Komponente wie System Center DPM (SC-DPM) in Azure anzeigen möchten, die nicht in [Sicherungsaufträgen](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) oder [Sicherungswarnungen](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault) im Portal angezeigt werden.

## <a name="using-log-analytics-workspace"></a>Verwenden des Log Analytics-Arbeitsbereichs

> [!NOTE]
> Daten aus Sicherungen von virtuellen Azure-Computern, MAB Agent, System Center DPM (SC-DPM), SQL-Sicherungen in virtuellen Azure-Computern und Sicherungen der Azure-Dateifreigabe werden über Diagnoseeinstellungen in den Log Analytics-Arbeitsbereich übertragen. Die Unterstützung für Microsoft Azure Backup Server (MABS) ist in Kürze verfügbar.

Wir nutzen die Möglichkeiten von zwei Azure-Diensten – **Diagnoseeinstellungen** (um Daten von mehreren Azure Resource Manager-Ressourcen an eine andere Ressource zu senden) und **Log Analytics** (LA – um benutzerdefinierte Benachrichtigungen zu generieren, in denen Sie andere Benachrichtigungskanäle über Aktionsgruppen definieren können) für die Überwachung. In den folgenden Abschnitten wird beschrieben, wie Sie LA zur Überwachung von Azure Backup im gewünschten Umfang verwenden können.

### <a name="configuring-diagnostic-settings"></a>Konfigurieren von Diagnoseeinstellungen

Eine Azure Resource Manager-Ressource, wie der Tresor von Azure Recovery Services, zeichnet alle möglichen Informationen über geplante Vorgänge und benutzergesteuerte Vorgänge als Diagnosedaten auf. Klicken Sie im Abschnitt „Überwachung“ auf die Schaltfläche „Diagnoseeinstellungen“, und geben Sie das Ziel für die Diagnosedaten des RS-Tresors an.

![Diagnoseeinstellung des RS-Tresors mit LA als Ziel](media/backup-azure-monitoring-laworkspace/rs-vault-diagnostic-setting.png)

Sie können einen LA-Arbeitsbereich aus einem anderen Abonnement als Ziel auswählen. *Durch die Auswahl desselben LA-Arbeitsbereichs für mehrere RS-Tresore können Sie Tresore über abonnementübergreifend zentral überwachen.* Wählen Sie als Protokoll „AzureBackupReport“, um alle Azure Backup-bezogenen Informationen an den LA-Arbeitsbereich zu leiten.

> [!IMPORTANT]
> Nachdem Sie die Konfiguration abgeschlossen haben, sollten Sie 24 Stunden warten, bis der erste Datenpush abgeschlossen ist. Danach erfolgt das Pushen aller Ereignisse wie im Abschnitt [Häufigkeit](#diagnostic-data-update-frequency) beschrieben.

### <a name="deploying-solution-to-log-analytics-workspace"></a>Bereitstellen einer Lösung für den Log Analytics-Arbeitsbereich

Sobald sich die Daten innerhalb des LA-Arbeitsbereichs befinden, stellen Sie [eine GitHub-Vorlage](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) in LA bereit, um die Daten zu visualisieren. Stellen Sie sicher, dass Sie die gleiche Ressourcengruppe, den gleichen Arbeitsbereichsnamen und den gleichen Speicherort angeben, um den Arbeitsbereich ordnungsgemäß zu bestimmen, und installieren Sie dann diese Vorlage.

> [!NOTE]
> Benutzern, die keine Warnungen oder Sicherungs-/Wiederherstellungsaufträge in ihrem LA-Arbeitsbereich haben, wird im Portal möglicherweise ein Fehler mit dem Code „BadArgumentError“ angezeigt. Benutzer können diesen Fehler ignorieren und die Lösung weiterhin verwenden. Sobald der Datenfluss des entsprechenden Typs in den Arbeitsbereich beginnt, zeigen die Visualisierungen das Gleiche an, und Benutzern wird dieser Fehler nicht mehr angezeigt.

### <a name="view-azure-backup-data-using-log-analytics-la"></a>Anzeigen von Azure Backup-Daten mit Log Analytics (LA)

Sobald die Vorlage bereitgestellt ist, wird die Lösung zur Überwachung von Azure Backup im Übersichtsbereich des Arbeitsbereichs angezeigt. Diesen können Sie wie folgt durchlaufen:

- Azure Monitor -> „Mehr“ Abschnitt „Insights“, und wählen Sie den relevanten Arbeitsbereich oder
- Log Analytics-Arbeitsbereiche -> wählen Sie den relevanten Arbeitsbereich -> „Zusammenfassung für Arbeitsbereich „ im Abschnitt „Allgemein“.

![AzureBackupLAMonitoringTile](media/backup-azure-monitoring-laworkspace/la-azurebackup-azuremonitor-tile.png)

Beim Anklicken der Kachel öffnet die Designervorlage eine Reihe von Graphen über die grundlegenden Überwachungsdaten von Azure Backup, wie zum Beispiel

#### <a name="all-backup-jobs"></a>Alle Sicherungsaufträge

![LABackupJobs](media/backup-azure-monitoring-laworkspace/la-azurebackup-allbackupjobs.png)

#### <a name="restore-jobs"></a>Wiederherstellungsaufträge

![LARestoreJobs](media/backup-azure-monitoring-laworkspace/la-azurebackup-restorejobs.png)

#### <a name="inbuilt-azure-backup-alerts-for-azure-resources"></a>Integrierte Azure Backup-Warnungen für Azure-Ressourcen

![LAInbuiltAzureBackupAlertsForAzureResources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts.png)

#### <a name="inbuilt-azure-backup-alerts-for-on-prem-resources"></a>Integrierte Azure Backup-Warnungen für lokale Ressourcen

![LAInbuiltAzureBackupAlertsForOnPremResources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts-onprem.png)

#### <a name="active-datasources"></a>Aktive Datenquellen

![LAActiveBackedUpEntities](media/backup-azure-monitoring-laworkspace/la-azurebackup-activedatasources.png)

#### <a name="rs-vault-cloud-storage"></a>RS-Tresor-Cloud-Speicher

![LARSVaultCloudStorage](media/backup-azure-monitoring-laworkspace/la-azurebackup-cloudstorage-in-gb.png)

Die obigen Graphen werden mit der Vorlage bereitgestellt, und der Kunde kann weitere Graphen bearbeiten/hinzufügen.

> [!IMPORTANT]
> Wenn Sie die Vorlage bereitstellen, wird eine schreibgeschützte Sperre erstellt, die Sie entfernen müssen, um die Vorlage zu bearbeiten und zu speichern. Informationen zum Entfernen von Sperren finden Sie im Bereich „Sperren“ im Abschnitt „Einstellungen“ des Log Analytics-Arbeitsbereichs.

### <a name="create-alerts-using-log-analytics"></a>Erstellen von Warnungen mit Log Analytics

Azure Monitor ermöglicht es Benutzern, ihre eigenen Warnungen aus dem LA-Arbeitsbereich zu erstellen, wo Sie *die Azure-Aktionsgruppen nutzen können, um Ihren bevorzugten Benachrichtigungsmechanismus auszuwählen*. Klicken Sie auf eine der obigen Graphen, um den Abschnitt „Protokolle“ im LA-Arbeitsbereich zu öffnen, ***in dem Sie die Abfragen bearbeiten und darüber hinaus Warnmeldungen erstellen können***.

![LACreateAlerts](media/backup-azure-monitoring-laworkspace/la-azurebackup-customalerts.png)

Wenn Sie wie oben gezeigt auf „Neue Warnungsregel“ klicken, wird der Bildschirm zur Erstellung von Azure Monitor-Warnungen geöffnet.

Wie Sie unten sehen können, ist die Ressource bereits als LA-Arbeitsbereich gekennzeichnet und die Integration von Aktionsgruppen wird bereitgestellt.

![LAAzureBackupCreateAlert](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

> [!IMPORTANT]
> Bitte beachten Sie, dass die relevanten Auswirkungen auf die Kosten der Erstellung dieser Abfrage [hier](https://azure.microsoft.com/pricing/details/monitor/) aufgeführt sind.

#### <a name="alert-condition"></a>Warnungsbedingung

Der wichtigste Aspekt ist die Bedingung, unter der die Warnung ausgelöst wird. Wenn Sie auf „Bedingung“ klicken, wird die Kusto-Abfrage automatisch im Bildschirm „Protokolle“ geladen, wie unten gezeigt, und Sie können sie an Ihr Szenario anpassen. Einige beispielhafte Kusto-Abfragen finden Sie im Abschnitt [unten](#sample-kusto-queries).

![LAAzureBackupAlertCondition](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

Bearbeiten Sie die Kusto-Abfrage, wählen Sie bei Bedarf den richtigen Schwellenwert (der bestimmt, wann die Warnung ausgelöst wird), den richtigen Zeitraum (Zeitfenster, in dem die Abfrage ausgeführt wird) und die Häufigkeit. Beispiel:  Wenn der Schwellenwert größer als 0 ist, der Zeitraum 5 Minuten und die Häufigkeit 5 Minuten beträgt, dann wird die Regel mit „Die Abfrage alle 5 Minuten für die letzten 5 Minuten ausführen und wenn die Anzahl der Ergebnisse größer als 0 ist, mich über die ausgewählte Aktionsgruppe benachrichtigen“ übersetzt.

#### <a name="action-group-integration"></a>Integration der Aktionsgruppe

Aktionsgruppen geben die Benachrichtigungskanäle an, die dem Benutzer zur Verfügung stehen. Wenn Sie im Abschnitt „Aktionsgruppe“ auf „Neue erstellen“ klicken, wird eine Liste der verfügbaren Benachrichtigungsmechanismen angezeigt.

![LAAzureBackupNewActionGroup](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

In der Dokumentation zum Azure Monitor erfahren Sie mehr über die [Warnungen vom LA-Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) und über die [Aktionsgruppen](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups).

So können Sie alle Warnungs- und Überwachungsanforderungen von LA allein erfüllen oder als ergänzende Technik zu den integrierten Benachrichtigungsmechanismen nutzen.

### <a name="sample-kusto-queries"></a>Kusto-Beispielabfragen

Die Standardgraphen zeigen Ihnen Kusto-Abfragen für einfache Szenarien, auf deren Grundlage Sie Warnungen erstellen können. Sie können sie auch ändern, um die Daten zu erhalten, zu denen Sie Warnungen erhalten möchten. Hier stellen wir Ihnen einige exemplarische Kusto-Abfragen zur Verfügung, die Sie in das Fenster „Protokolle“ einfügen und dann eine Warnung für die Abfrage erstellen können.

#### <a name="all-successful-backup-jobs"></a>Alle erfolgreiche Sicherungsaufträge

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| where OperationName == "Job" and JobOperation_s == "Backup"
| where JobStatus_s == "Completed"
````

#### <a name="all-failed-backup-jobs"></a>Alle fehlgeschlagenen Sicherungsaufträge

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| where OperationName == "Job" and JobOperation_s == "Backup"
| where JobStatus_s == "Failed"
````

#### <a name="all-successful-azure-vm-backup-jobs"></a>Alle erfolgreiche Azure VM-Sicherungsaufträge

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "VM" and BackupManagementType_s == "IaaSVM"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

#### <a name="all-successful-sql-log-backup-jobs"></a>Alle erfolgreichen Sicherungsaufträge für das SQL-Protokoll

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s == "Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "SQLDataBase" and BackupManagementType_s == "AzureWorkload"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

#### <a name="all-successful-mab-agent-backup-jobs"></a>Alle erfolgreiche MAB Agent-Sicherungsaufträge

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "FileFolder" and BackupManagementType_s == "MAB"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

### <a name="diagnostic-data-update-frequency"></a>Häufigkeit der Aktualisierung der Diagnosedaten

Die Diagnosedaten aus dem Tresor werden mit einer gewissen Verzögerung in den LA-Arbeitsbereich übertragen. Jedes Ereignis geht ***nach dem Pushen aus dem RS-Tresor mit einer Verzögerung von 20 bis 30 Minuten*** im LA-Arbeitsbereich ein.

- Die im Sicherungsdienst integrierten Warnungen (lösungsübergreifend) werden sofort nach ihrer Erstellung ausgelöst. Das bedeutet, dass sie typischerweise nach einer Verzögerung von 20 bis30 Minuten im LA-Arbeitsbereich angezeigt werden.
- Ad-hoc-Sicherungsaufträge und Wiederherstellungsaufträge (lösungsübergreifend) werden verschoben, sobald sie ***abgeschlossen sind***.
- Die geplanten Sicherungsaufträge aus allen Lösungen (außer SQL-Sicherung) werden verschoben, sobald sie  ***abgeschlossen*** sind.
- Da wir alle 15 Minuten Protokollsicherungen erstellen können, werden die Informationen bei einer SQL-Sicherung für alle abgeschlossenen geplanten Sicherungsaufträge, einschließlich der Protokolle, gebündelt und alle 6 Stunden übertragen.
- Alle anderen Informationen wie Sicherungselemente, Richtlinien, Wiederherstellungspunkte, Speicher usw. werden über lösungsübergreifend **mindestens einmal täglich** übertragen.
- Eine Änderung der Sicherungskonfiguration wie Änderung der Richtlinie, Bearbeitungsrichtlinie usw. löst einen Push aller zugehörigen Sicherungsinformationen aus.

## <a name="using-rs-vaults-activity-logs"></a>Verwenden der Aktivitätsprotokoll des RS-Tresors

Sie können auch Aktivitätsprotokolle verwenden, um Benachrichtigungen zu Ereignissen zu erhalten, etwa zur erfolgreichen Sicherung.

> [!CAUTION]
> **Beachten Sie, dass dies nur für Azure-VM-Sicherungen anwendbar ist.** Sie können dies nicht für andere Lösungen wie Azure Backup Agent, SQL-Sicherungen innerhalb von Azure, Azure Files usw. verwenden.

### <a name="sign-in-into-azure-portal"></a>Anmelden beim Azure-Portal

Melden Sie sich beim Azure-Portal an, rufen Sie den relevanten Azure Recovery Services-Tresor auf, und klicken Sie in den Eigenschaften auf den Abschnitt „Aktivitätsprotokoll“.

### <a name="identify-appropriate-log-and-create-alert"></a>Bestimmen des geeigneten Protokolls und Erstellen von Warnungen

Wenden Sie die in der folgenden Abbildung gezeigten Filter an, um zu überprüfen, ob Sie Aktivitätsprotokolle für erfolgreiche Sicherungen empfangen. Ändern Sie den Zeitraum zum Anzeigen von Datensätzen entsprechend.

![Aktivitätsprotokolle für Azure-VM-Sicherungen](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

Wenn Sie auf den Namen des Vorgangs klicken, werden der Vorgang und zugehörige Details angezeigt.

![Neue Warnungsregel](media/backup-azure-monitoring-laworkspace/new-alert-rule.png)

Klicken Sie auf **Neue Warnungsregel**, um den Bildschirm **Regel erstellen** zu öffnen. Dort können Sie Warnungen anhand der in [diesem Artikel](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log) beschriebenen Schritte erstellen.

Hier ist die Ressource der Recovery Services-Tresor selbst. Daher müssen Sie die gleiche Aktion für alle Tresore wiederholen, in denen Sie über Aktivitätsprotokolle informiert werden möchten. Die Bedingung hat keinen Schwellenwert, keinen Zeitraum und keine Häufigkeit, da es sich um eine ereignisbasierte Warnung handelt. Sobald das entsprechende Aktivitätsprotokoll erstellt wurde, wird die Warnung ausgelöst.

## <a name="recommendation"></a>Empfehlung

***Alle Warnungen, die aus Aktivitätsprotokollen und LA-Arbeitsbereichen erstellt wurden, können im Azure Monitor im Bereich „Warnungen“ auf der linken Seite angezeigt werden.***

Während die Benachrichtigung über Aktivitätsprotokolle verwendet werden kann, empfiehlt der ***Azure Backup-Dienst dringend, LA für die Überwachung im gewünschten Umfang zu verwenden und NICHT Aktivitätsprotokolle. Dafür gibt es folgende Gründe***.

- **Begrenzte Szenarien:** Gilt nur für Azure-VM-Sicherungen und sollte für jeden RS-Tresor wiederholt werden.
- **Definitionsübereinstimmung:** Die Aktivität für geplante Sicherungen entspricht nicht der neuesten Definition von Aktivitätsprotokollen und richtet sich nach [Diagnoseprotokollen](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview#what-you-can-do-with-diagnostic-logs). Dies führt zu unerwarteten Auswirkungen, wenn das Datapumping über den Aktivitätsprotokollkanal wie unten beschrieben geändert wird.
- **Probleme mit dem Aktivitätsprotokollkanal:** Wir haben jetzt ein neues Modell eingesetzt, um Aktivitätsprotokolle aus Azure Backup in Recovery Services-Tresore zu verschieben. Leider hat dies die Generierung von Aktivitätsprotokollen in Azure-Sovereign Clouds beeinträchtigt. Wenn Benutzer von Azure-Sovereign Clouds Warnungen aus den Aktivitätsprotokollen über Azure Monitor erstellt/konfiguriert haben, würden sie nicht ausgelöst werden. Außerdem würden in allen öffentlichen Azure-Regionen, wenn ein Benutzer Recovery Services-Aktivitätsprotokolle wie [hier](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs) erwähnt in einem Log Analytics-Arbeitsbereich erfasst, auch diese Protokolle nicht angezeigt.

Daher wird dringend empfohlen, den Log Analytic-Arbeitsbereich für die Überwachung und Warnung aller Ihrer mit Azure Backup geschützten Workloads zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Erstellen von benutzerdefinierten Abfragen finden Sie in [Log Analytics-Datenmodell](backup-azure-log-analytics-data-model.md).
