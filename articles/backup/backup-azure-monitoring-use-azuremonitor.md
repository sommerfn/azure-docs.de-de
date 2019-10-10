---
title: 'Azure Backup: Überwachen von Azure Backup mit Azure Monitor'
description: Überwachen von Azure Backup-Workloads und Erstellen von benutzerdefinierten Warnungen mit Azure Monitor.
ms.reviewer: pullabhk
author: dcurwin
manager: carmonm
keywords: Log Analytics; Azure Backup; Warnungen Diagnoseeinstellungen; Aktionsgruppen
ms.service: backup
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: dacurwin
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: ba2288ecebbeda97b3cd9c24ae930be6af193ab8
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177714"
---
# <a name="monitor-at-scale-by-using-azure-monitor"></a>Überwachen im richtigen Maßstab mithilfe von Azure Monitor

Azure Backup stellt [integrierte Überwachungs-und Warnungsfunktionen](backup-azure-monitoring-built-in-monitor.md) in einem Recovery Services-Tresor bereit. Diese Funktionen sind ohne zusätzliche Verwaltungsinfrastruktur verfügbar. Dieser integrierte Dienst ist jedoch in den folgenden Szenarien eingeschränkt:

- Wenn Sie Daten aus mehreren Recovery Services-Tresoren über mehrere Abonnements hinweg überwachen
- Wenn E-Mail *nicht* der bevorzugte Benachrichtigungskanal ist
- Wenn Benutzer Warnungen für mehrere Szenarien erhalten möchten
- Wenn Sie Informationen von einer lokalen Komponente wie System Center Data Protection Manager in Azure anzeigen möchten, die das Portal nicht in [**Sicherungsaufträgen**](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) oder [**Sicherungswarnungen**](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault) anzeigt

## <a name="using-log-analytics-workspace"></a>Verwenden des Log Analytics-Arbeitsbereichs

> [!NOTE]
> Daten aus Azure VM-Sicherungen, dem Azure Backup-Agent, System Center Data Protection Manager, SQL-Sicherungen in Azure VMS und Azure Files-Freigabesicherungen werden über Diagnoseeinstellungen an den Log Analytics-Arbeitsbereich übertragen. 

Um eine bedarfsorientierte Überwachung bzw. einen geeigneten Bericht auszuführen, benötigen Sie die Funktionen von zwei Azure-Diensten. *Diagnoseeinstellungen* senden Daten aus mehreren Azure Resource Manager-Ressourcen an eine andere Ressource. *Log Analytics* generiert benutzerdefinierte Warnungen, in denen Sie Aktionsgruppen verwenden können, um andere Benachrichtigungskanäle zu definieren. 

In den folgenden Abschnitten wird beschrieben, wie Sie Log Analytics zur Überwachung von Azure Backup im gewünschten Umfang verwenden können.

### <a name="configure-diagnostic-settings"></a>Konfigurieren von Diagnoseeinstellungen

Azure Resource Manager-Ressourcen, etwa der Azure Recovery-Tresor, zeichnen Informationen zu geplanten Vorgängen und benutzergesteuerten Vorgängen als Diagnosedaten auf. 

Wählen Sie im Überwachungsabschnitt **Diagnoseeinstellungen** aus, und geben Sie das Ziel für die Diagnosedaten des Recovery Services-Tresors an.

![Die Diagnoseeinstellung des Recovery Services-Tresors mit dem Ziel Log Analytics](media/backup-azure-monitoring-laworkspace/rs-vault-diagnostic-setting.png)

Sie können einen Log Analytics-Arbeitsbereich aus einem anderen Abonnement als Ziel zuweisen. Um Tresore über Abonnements hinweg an einem zentralen Ort zu überwachen, wählen Sie den gleichen Log Analytics-Arbeitsbereich für mehrere Recovery Services-Tresore aus. Um alle Informationen im Zusammenhang mit Azure Backup an den Log Analytics-Arbeitsbereich weiterzuleiten, wählen Sie **AzureBackupReport** als Protokoll aus.

> [!IMPORTANT]
> Nachdem Sie die Konfiguration abgeschlossen haben, sollten Sie 24 Stunden warten, bis der erste Datenpushvorgang abgeschlossen ist. Nach diesem anfänglichen Datenpushvorgang werden alle Ereignisse gemäß der Beschreibung weiter unten in diesem Artikel im Abschnitt [Häufigkeit](#diagnostic-data-update-frequency) gepusht.

### <a name="deploy-a-solution-to-the-log-analytics-workspace"></a>Bereitstellen einer Lösung im Log Analytics-Arbeitsbereich

> [!IMPORTANT]
> Wir haben eine aktualisierte [Vorlage](https://azure.microsoft.com/resources/templates/101-backup-la-reporting/) zur Mehrfachansicht für die LA-basierte Überwachung und Berichterstellung in Azure Backup veröffentlicht. Beachten Sie, dass Benutzer, die die [frühere Lösung](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) verwendet haben, diese auch nach der Bereitstellung der neuen Lösung in Ihren Arbeitsbereichen sehen werden. Die alte Lösung kann jedoch aufgrund einiger geringfügiger Schemaänderungen falsche Ergebnisse liefern. Es ist daher erforderlich, dass Benutzer die neue Vorlage bereitstellen.

Nachdem sich die Daten im Log Analytics-Arbeitsbereich befinden, stellen Sie eine [GitHub-Vorlage](https://azure.microsoft.com/resources/templates/101-backup-la-reporting/) für Log Analytics bereit, um die Daten zu visualisieren. Um den Arbeitsbereich ordnungsgemäß zu identifizieren, stellen Sie sicher, dass Sie für ihn die gleiche Ressourcengruppe, den gleichen Arbeitsbereichsnamen und den gleichen Speicherort des Arbeitsbereichs verwenden. Installieren Sie dann diese Vorlage für den Arbeitsbereich.

### <a name="view-azure-backup-data-by-using-log-analytics"></a>Anzeigen von Azure Backup-Daten mit Log Analytics

Nach der Bereitstellung der Vorlage wird die Lösung zur Überwachung und Berichterstellung von Azure Backup im Übersichtsbereich des Arbeitsbereichs angezeigt. Gehen Sie folgendermaßen vor, um zur Zusammenfassung zu gelangen:

- **Azure Monitor**: Wählen Sie im Abschnitt **Insights** die Option **Weitere** aus, und wählen Sie dann den entsprechenden Arbeitsbereich aus.
- **Log Analytics-Arbeitsbereiche**: Wählen Sie den relevanten Arbeitsbereich aus, und wählen Sie dann unter **Allgemein** die Option **Zusammenfassung des Arbeitsbereichs** aus.

![Die Kachel „Log Analytics: Überwachung und Berichte“](media/backup-azure-monitoring-laworkspace/la-azurebackup-overview-dashboard.png)

Durch Auswählen einer der Übersichtskacheln können Sie weitere Informationen anzeigen. Dies sind einige der Berichte, die angezeigt werden:

* Nicht-Protokollsicherungsaufträge

   ![Log Analytics-Diagramme für Sicherungsaufträge](media/backup-azure-monitoring-laworkspace/la-azurebackup-backupjobsnonlog.png)

* Warnungen aus der Sicherung von Azure-Ressourcen

   ![Log Analytics-Diagramm für Wiederherstellungsaufträge](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertsazure.png)

Analog dazu können Sie durch Klicken auf die anderen Kacheln Berichte zu Wiederherstellungsaufträgen, Cloudspeicher, Sicherungselementen, Warnungen von lokalen Ressourcensicherungen und Protokollsicherungsaufträgen anzeigen.
 
Diese Diagramme werden mit der Vorlage bereitgestellt. Wenn erforderlich, können Sie die Diagramme bearbeiten oder weitere Diagramme hinzufügen.

### <a name="create-alerts-by-using-log-analytics"></a>Erstellen von Warnungen mit Log Analytics

In Azure Monitor können Sie Ihre eigenen Warnungen in einem Log Analytics-Arbeitsbereich erstellen. Im Arbeitsbereich verwenden Sie *Azure-Aktionsgruppen*, um Ihren bevorzugten Benachrichtigungsmechanismus auszuwählen. 

> [!IMPORTANT]
> Weitere Informationen zu den Kosten für das Erstellen dieser Abfrage finden Sie unter[Azure Monitor-Preise](https://azure.microsoft.com/pricing/details/monitor/).

Wählen Sie eines der Diagramme aus, um den Abschnitt **Protokolle** im Log Analytics-Arbeitsbereich zu öffnen. Bearbeiten Sie im Abschnitt **Protokolle** die Abfragen, und erstellen Sie Warnungen für sie.

![Erstellen einer Warnung in einem Log Analytics-Arbeitsbereich](media/backup-azure-monitoring-laworkspace/la-azurebackup-customalerts.png)

Wenn Sie **Neue Warnungsregel** auswählen, wird die Azure Monitor-Seite für die Warnungserstellung geöffnet. Die folgende Abbildung zeigt dies. Hier ist die Ressource bereits als Log Analytics-Arbeitsbereich gekennzeichnet, und die Integration von Aktionsgruppen wird bereitgestellt.

![Die Log Analytics-Seite für die Warnungserstellung](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

#### <a name="alert-condition"></a>Warnungsbedingung

Das definierende Merkmal einer Warnung ist die auslösende Bedingung. Wählen Sie **Bedingung** aus, um die Kusto-Abfrage auf der Seite **Protokolle** automatisch zu laden, wie in der folgenden Abbildung dargestellt. Hier können Sie die Bedingung so bearbeiten, dass sie Ihren Anforderungen entspricht. Weitere Informationen finden Sie unter [Kusto-Beispielabfragen](#sample-kusto-queries).

![Einrichten einer Warnungsbedingung](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

Bei Bedarf können Sie die Kusto-Abfrage bearbeiten. Wählen Sie einen Schwellenwert, einen Zeitraum und eine Häufigkeit aus. Der Schwellenwert bestimmt, wann die Warnung ausgelöst wird. Der Zeitraum ist das Zeitfenster, in dem die Abfrage ausgeführt wird. Wenn der Schwellenwert z.B. größer als 0 ist, der Zeitraum 5 Minuten und die Häufigkeit 5 Minuten beträgt, führt die Regel die Abfrage alle 5 Minuten aus, wobei die letzten 5 Minuten überprüft werden. Wenn die Anzahl der Ergebnisse größer als 0 ist, werden Sie über die ausgewählte Aktionsgruppe benachrichtigt.

#### <a name="alert-action-groups"></a>Warnungsaktionsgruppen

Verwenden Sie eine Aktionsgruppe, um einen Benachrichtigungskanal anzugeben. Wählen Sie unter **Aktionsgruppen** die Option **Neu erstellen** aus, um die verfügbaren Benachrichtigungsmechanismen anzuzeigen.

![Verfügbare Benachrichtigungsmechanismen im Fenster „Aktionsgruppe hinzufügen“](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

Sie können alle Warnungs- und Überwachungsanforderungen von Log Analytics allein erfüllen, oder Sie können Log Analytics verwenden, um integrierte Benachrichtigungen zu ergänzen.

Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Protokollwarnungen mithilfe von Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) und [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups).

### <a name="sample-kusto-queries"></a>Kusto-Beispielabfragen

Die Standarddiagramme zeigen Ihnen Kusto-Abfragen für einfache Szenarien, auf deren Grundlage Sie Warnungen erstellen können. Sie können die Abfragen auch ändern, um die Daten abzurufen, zu denen Sie Warnungen erhalten möchten. Fügen Sie die folgenden Kusto-Beispielabfragen auf der Seite **Protokolle** ein, und erstellen Sie dann Warnungen für die Abfragen:

* Alle erfolgreiche Sicherungsaufträge

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Completed"
    ````
    
* Alle fehlgeschlagenen Sicherungsaufträge

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Failed"
    ````
    
* Alle erfolgreiche Azure VM-Sicherungsaufträge

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

* Alle erfolgreichen Sicherungsaufträge für das SQL-Protokoll

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

* Alle erfolgreiche Aufträge des Azure Backup-Agents

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

Die Diagnosedaten aus dem Tresor werden mit einer gewissen Verzögerung in den Log Analytics-Arbeitsbereich übertragen. Jedes Ereignis erreicht den Log Analytics-Arbeitsbereich *20 bis 30 Minuten* nach dem Übertragung aus dem Recovery Services-Tresor. Hier finden Sie weitere Details zu dieser Verzögerung:

- Lösungsübergreifend werden die im Sicherungsdienst integrierten Warnungen sofort nach ihrer Erstellung gepusht. Daher werden sie normalerweise nach 20 bis 30 Minuten im Log Analytics-Arbeitsbereich angezeigt.
- Lösungsübergreifend werden Ad-hoc-Sicherungsaufträge und Wiederherstellungsaufträge gepusht, sobald sie *abgeschlossen sind*.
- Die geplanten Sicherungsaufträge aus allen Lösungen (außer SQL-Sicherung) werden gepusht, sobald sie  *abgeschlossen* sind.
- Da Protokollsicherungen alle 15 Minuten auftreten können, werden für SQL-Sicherungen Informationen für alle abgeschlossenen geplanten Sicherungsaufträge (einschließlich Protokolle) als Batch gesammelt und alle 6 Stunden gepusht.
- Alle anderen Informationen wie Sicherungselemente, Richtlinien, Wiederherstellungspunkte, Speicher usw. werden lösungsübergreifend *mindestens einmal täglich* gepusht.
- Eine Änderung der Sicherungskonfiguration (wie einer Änderung der Richtlinie oder Bearbeitungsrichtlinie) löst einen Push aller zugehörigen Sicherungsinformationen aus.

## <a name="using-the-recovery-services-vaults-activity-logs"></a>Verwenden der Aktivitätsprotokolle des Recovery Services-Tresors

> [!CAUTION]
> Die folgenden Schritte gelten nur für *Azure-VM-Sicherungen.* Diese Schritte können nicht für Lösungen wie den Azure Backup-Agent, SQL-Sicherungen in Azure oder Azure Files verwendet werden.

Sie können auch Aktivitätsprotokolle verwenden, um Benachrichtigungen zu Ereignissen zu erhalten, etwa zur erfolgreichen Sicherung. Führen Sie zunächst die folgenden Schritte aus:

1. Melden Sie sich beim Azure-Portal an.
1. Öffnen Sie den relevanten Recovery Services-Tresor. 
1. Öffnen Sie in den Eigenschaften des Tresors den Abschnitt **Aktivitätsprotokoll**.

Identifizieren Sie das geeignete Protokoll, und erstellen Sie eine Warnung:

1. Überprüfen Sie, ob Sie Aktivitätsprotokolle für erfolgreiche Sicherungen erhalten, indem Sie die in der folgenden Abbildung gezeigten Filter anwenden. Ändern Sie den Wert für die **Zeitspanne** nach Bedarf, um Datensätze anzuzeigen.

   ![Filtern, um Aktivitätsprotokolle für Azure-VM-Sicherungen zu ermitteln](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

1. Wählen Sie den Vorgangsnamen aus, um die relevanten Details anzuzeigen.
1. Wählen Sie **Neue Warnungsregel** aus, um die Seite **Regel erstellen** zu öffnen. 
1. Erstellen Sie eine Warnung anhand der unter [Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log) beschriebenen Schritte.

   ![Neue Warnungsregel](media/backup-azure-monitoring-laworkspace/new-alert-rule.png)

Hier ist die Ressource der Recovery Services-Tresor selbst. Wiederholen Sie die gleichen Schritte für alle Tresore, in denen Sie über Aktivitätsprotokolle benachrichtigt werden möchten. Die Bedingung weist keinen Schwellenwert, keinen Zeitraum und keine Häufigkeit auf, da diese Warnung auf Ereignissen basiert. Sobald das entsprechende Aktivitätsprotokoll erstellt wurde, wird die Warnung ausgelöst.

## <a name="using-log-analytics-to-monitor-at-scale"></a>Verwenden von Log Analytics zum Überwachen im richtigen Maßstab

In Azure Monitor können Sie alle Warnungen anzeigen, die aus Aktivitätsprotokollen und Log Analytics-Arbeitsbereichen erstellt wurden. Öffnen Sie einfach den Bereich **Warnungen** auf der linken Seite.

Obwohl Sie Benachrichtigungen über Aktivitätsprotokolle erhalten können, empfehlen wir dringend die Verwendung von Log Analytics anstelle von Aktivitätsprotokollen für die Überwachung im richtigen Maßstab. Dies ist der Grund:

- **Eingeschränkte Szenarien**: Benachrichtigungen über Aktivitätsprotokolle gelten nur für Azure-VM-Sicherungen. Die Benachrichtigungen müssen für jeden Recovery Services-Tresor eingerichtet werden.
- **Definitionsübereinstimmung**: Die geplante Sicherungsaktivität passt nicht zur aktuellen Definition von Aktivitätsprotokollen. Stattdessen erfolgt die Ausrichtung an [Diagnoseprotokollen](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-workspace#what-you-can-do-with-resource-logs-in-a-workspace). Diese Ausrichtung führt zu unerwarteten Auswirkungen, wenn sich die Daten, die durch den Aktivitätsprotokollkanal fließen, ändern.
- **Probleme mit dem Aktivitätsprotokollkanal**: In Recovery Services-Tresoren folgen Aktivitätsprotokolle, die aus Azure Backup gepusht werden, einem neuen Modell. Leider wirkt sich diese Änderung auf die Generierung von Aktivitätsprotokollen in Azure Government, Azure Deutschland und Azure China 21Vianet aus. Wenn Benutzer dieser Clouddienste Warnungen aus Aktivitätsprotokollen in Azure Monitor erstellen oder konfigurieren, werden die Warnungen nicht ausgelöst. Außerdem werden diese Protokolle in allen öffentlichen Azure-Regionen nicht angezeigt, wenn ein Benutzer [Recovery Services-Aktivitätsprotokolle in einem Log Analytics-Arbeitsbereich erfasst](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs).

Verwenden Sie einen Log Analytics-Arbeitsbereich für die ordnungsgemäße Überwachung und Warnungserstellung für alle Ihre durch Azure Backup geschützten Workloads.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Erstellen von benutzerdefinierten Abfragen finden Sie unter [Log Analytics-Datenmodell](backup-azure-log-analytics-data-model.md).
