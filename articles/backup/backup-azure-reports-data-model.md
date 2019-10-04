---
title: Datenmodell für Azure Backup
description: In diesem Artikel werden die Details des Power BI-Datenmodells für Azure Backup-Berichte vorgestellt.
ms.reviewer: adigan
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/26/2017
ms.author: dacurwin
ms.openlocfilehash: 96adca2da28517c28ba3583f5d15f07311d2792a
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "68954512"
---
# <a name="data-model-for-azure-backup-reports"></a>Datenmodell für Azure Backup-Berichte
In diesem Artikel wird das Power BI-Datenmodell beschrieben, das zum Erstellen von Azure Backup-Berichten verwendet wird. Mit diesem Datenmodell können Sie vorhandene Berichte basierend auf entsprechenden Feldern filtern und, was noch wichtiger ist, eigene Berichte mithilfe der Tabellen und Felder im Modell erstellen. 

## <a name="creating-new-reports-in-power-bi"></a>Erstellen neuer Berichte in Power BI
Power BI bietet Anpassungsfunktionen, mit deren Hilfe Sie [Berichte mithilfe des Datenmodells erstellen](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/) können.

## <a name="using-azure-backup-data-model"></a>Verwenden des Azure Backup-Datenmodells
Sie können die folgenden Felder im Datenmodell nutzen, um Berichte zu erstellen und vorhandene Berichte anzupassen.

### <a name="alert"></a>Warnung
Diese Tabelle enthält grundlegende Felder und Aggregationen für verschiedene warnungsbezogene Felder.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Ganze Zahl |Anzahl der Warnungen, die im gewählten Zeitraum erstellt wurden |
| %ActiveAlertsCreatedInPeriod |Prozentsatz |Prozentsatz aktiver Warnungen im gewählten Zeitraum |
| %CriticalAlertsCreatedInPeriod |Prozentsatz |Prozentsatz kritischer Warnungen im gewählten Zeitraum |
| AlertOccurrenceDate |Date |Datum der Erstellung der Warnung |
| AlertSeverity |Text |Schweregrad der Warnung Beispiel: Critical (Kritisch). |
| AlertStatus |Text |Status der Warnung. Beispiel: Active (Aktiv). |
| AlertType |Text |Typ der generierten Warnung. Beispiel: Backup (Sicherung). |
| AlertUniqueId |Text |Eindeutige ID der generierten Warnung |
| AsOnDateTime |Date/Time |Uhrzeit der letzten Aktualisierung der ausgewählten Zeile |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Dezimalzahl |Durchschnittliche Zeit (in Minuten) zum Beheben der Warnung für den gewählten Zeitraum |
| EntityState |Text |Aktueller Status des Warnungsobjekts. Beispielsweise Active (Aktiv), Deleted (Gelöscht). |

### <a name="backup-item"></a>Sicherungselement
Diese Tabelle enthält grundlegende Felder und Aggregationen für verschiedene auf Sicherungselemente bezogene Felder.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| #BackupItems |Ganze Zahl |Anzahl der Sicherungselemente |
| #UnprotectedBackupItems |Ganze Zahl |Anzahl der Sicherungselemente, deren Schutz beendet wurde oder die für Sicherungen konfiguriert sind, aber Sicherungen nicht gestartet wurden|
| AsOnDateTime |Date/Time |Uhrzeit der letzten Aktualisierung der ausgewählten Zeile |
| BackupItemFriendlyName |Text |Anzeigename des Sicherungselements |
| BackupItemId |Text |ID des Sicherungselements |
| BackupItemName |Text |Name des Sicherungselements |
| BackupItemType |Text |Typ des Sicherungselements. Beispiel: VM, FileFolder. |
| EntityState |Text |Aktueller Status des Sicherungselementobjekts. Beispielsweise Active (Aktiv), Deleted (Gelöscht). |
| LastBackupDateTime |Date/Time |Zeitpunkt der letzten Sicherung des ausgewählten Sicherungselements |
| LastBackupState |Text |Status der letzten Sicherung des ausgewählten Sicherungselements. Beispiel: Successful (Erfolgreich), Failed (Fehler). |
| LastSuccessfulBackupDateTime |Date/Time |Zeitpunkt der letzten erfolgreichen Sicherung des ausgewählten Sicherungselements |
| ProtectionState |Text |Aktueller Schutzstatus des Sicherungselements. Beispiel: Protected (Geschützt), ProtectionStopped (Schutz angehalten). |

### <a name="calendar"></a>Kalender
Diese Tabelle enthält Details zu kalenderbezogenen Feldern.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| Date |Date |Zum Filtern von Daten ausgewähltes Datum |
| DateKey |Text |Eindeutiger Schlüssel jedes Datumselements |
| DayDiff |Dezimalzahl |Unterschied in Bezug auf den Tag für das Filtern von Daten. Beispiel: 0 gibt die Daten des aktuellen Tags, -1 die Daten des Vortages an. 0 und -1 geben Daten des aktuellen und des Vortags an.  |
| Month (Monat) |Text |Der gewählte Monat des Jahres zum Filtern von Daten. Der Monat beginnt am 1. und endet am 31. Tag. |
| MonthDate | Date |Zum Filtern von Daten ausgewähltes Datum im Monat, an dem der Monat endet |
| MonthDiff |Dezimalzahl |Unterschied in Bezug auf den Monat für das Filtern von Daten. Beispiel: 0 gibt die Daten des aktuellen Monats, -1 die Daten des Vormonats an. 0 und -1 geben Daten des aktuellen und des Vormonats an. |
| Woche |Text |Zum Filtern von Daten ausgewählte Woche. Die Woche beginnt am Sonntag und endet am Samstag. |
| WeekDate |Date |Zum Filtern von Daten ausgewähltes Datum in der Woche, an dem die Woche endet |
| WeekDiff |Dezimalzahl |Unterschied in Bezug auf die Woche für das Filtern von Daten. Beispiel: 0 gibt die Daten der aktuellen Woche, -1 die Daten der Vorwoche an. 0 und -1 geben Daten der aktuellen und der Vorwoche an. |
| Jahr |Text |Zum Filtern von Daten ausgewähltes Kalenderjahr |
| YearDate |Date |Zum Filtern von Daten ausgewähltes Datum im Jahr, an dem das Jahr endet |

### <a name="job"></a>Auftrag
Diese Tabelle enthält grundlegende Felder und Aggregationen für verschiedene auftragsbezogene Felder.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| #JobsCreatedInPeriod |Ganze Zahl |Anzahl der Aufträge, die im gewählten Zeitraum erstellt wurden |
| %FailuresForJobsCreatedInPeriod |Prozentsatz |Prozentsatz der gesamten Auftragsfehler im ausgewählten Zeitraum |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Dezimalzahl |80. Quantilwert der übertragenen Daten (in MB) für **Sicherungsaufträge**, die im ausgewählten Zeitraum erstellt wurden |
| AsOnDateTime |Date/Time |Uhrzeit der letzten Aktualisierung der ausgewählten Zeile |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Dezimalzahl |Durchschnittliche Zeit in Minuten für **abgeschlossene Sicherungsaufträge**, die im ausgewählten Zeitraum erstellt wurden |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Dezimalzahl |Durchschnittliche Zeit in Minuten für **abgeschlossene Wiederherstellungsaufträge**, die im ausgewählten Zeitraum erstellt wurden |
| BackupStorageDestination |Text |Ziel des Sicherungsspeichers. Beispiel: Cloud, Disk (Datenträger).  |
| EntityState |Text |Aktueller Status des Auftragsobjekts. Beispielsweise Active (Aktiv), Deleted (Gelöscht). |
| JobFailureCode |Text |Zeichenfolge mit dem Fehlercode zum Angeben des Grunds des Auftragsfehlers |
| JobOperation |Text |Vorgang, für den der Auftrag ausgeführt wird. Beispiel: Backup (Sicherung), Restore (Wiederherstellung), Configure Backup (Sicherung konfigurieren). |
| JobStartDate |Date |Datum des Starts des Auftrags |
| JobStartTime |Time |Uhrzeit des Starts des Auftrags |
| Auftragsstatus |Text |Status des beendeten Auftrags. Beispiel: Completed (Abgeschlossen), Failed (Fehler). |
| JobUniqueId |Text |Eindeutige ID zur Bezeichnung des Auftrags |

### <a name="policy"></a>Richtlinie
Diese Tabelle enthält grundlegende Felder und Aggregationen für verschiedene richtlinienbezogene Felder.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| #Policies |Ganze Zahl |Anzahl der Sicherungsrichtlinien im System |
| #PoliciesInUse |Ganze Zahl |Anzahl der Richtlinien, die derzeit für das Konfigurieren von Sicherungen verwendet werden |
| AsOnDateTime |Date/Time |Uhrzeit der letzten Aktualisierung der ausgewählten Zeile |
| BackupDaysOfTheWeek |Text |Tag der Woche, für den Sicherungen geplant sind |
| BackupFrequency |Text |Häufigkeit der Ausführung von Sicherungen. Beispiel: Täglich, Wöchentlich. |
| BackupTimes |Text |Datum und Uhrzeit geplanter Sicherungen |
| DailyRetentionDuration |Ganze Zahl |Gesamte Beibehaltungsdauer in Tagen für konfigurierte Sicherungen |
| DailyRetentionTimes |Text |Konfiguration von Datum und Uhrzeit der täglichen Beibehaltung |
| EntityState |Text |Aktueller Status des Richtlinienobjekts. Beispielsweise Active (Aktiv), Deleted (Gelöscht). |
| MonthlyRetentionDaysOfTheMonth |Text |Datumsangaben des Monats, die für die monatliche Beibehaltung ausgewählt sind |
| MonthlyRetentionDaysOfTheWeek |Text |Tage der Woche, die für die monatliche Beibehaltung ausgewählt sind |
| MonthlyRetentionDuration |Dezimalzahl |Gesamte Beibehaltungsdauer in Monaten für konfigurierte Sicherungen |
| MonthlyRetentionFormat |Text |Typ der Konfiguration für die monatliche Beibehaltung. Beispiel: „daily“ für tägliche Ausführung, „weekly“ für wöchentliche Ausführung. |
| MonthlyRetentionTimes |Text |Konfiguration von Datum und Uhrzeit der monatlichen Beibehaltung |
| MonthlyRetentionWeeksOfTheMonth |Text |Wochen des Monats, in denen die monatliche Beibehaltung konfiguriert ist. Beispiel: Erste, Letzte usw. |
| PolicyName |Text |Name der definierten Richtlinie |
| PolicyUniqueId |Text |Eindeutige ID zur Bezeichnung der Richtlinie |
| RetentionType |Text |Typ der Aufbewahrungsrichtlinie. Beispiel: Täglich, Wöchentlich, Monatlich, Jährlich. |
| WeeklyRetentionDaysOfTheWeek |Text |Tage der Woche, die für die wöchentliche Beibehaltung ausgewählt sind |
| WeeklyRetentionDuration |Dezimalzahl |Gesamte wöchentliche Beibehaltungsdauer in Wochen für konfigurierte Sicherungen |
| WeeklyRetentionTimes |Text |Konfiguration von Datum und Uhrzeit der wöchentlichen Beibehaltung |
| YearlyRetentionDaysOfTheMonth |Text |Datumsangaben des Monats, die für die jährliche Beibehaltung ausgewählt sind |
| YearlyRetentionDaysOfTheWeek |Text |Tage der Woche, die für die jährliche Beibehaltung ausgewählt sind |
| YearlyRetentionDuration |Dezimalzahl |Gesamte Beibehaltungsdauer in Jahren für konfigurierte Sicherungen |
| YearlyRetentionFormat |Text |Typ der Konfiguration für die jährliche Beibehaltung. Beispiel: „daily“ für tägliche Ausführung, „weekly“ für wöchentliche Ausführung. |
| YearlyRetentionMonthsOfTheYear |Text |Monate des Jahres, die für die jährliche Beibehaltung ausgewählt sind |
| YearlyRetentionTimes |Text |Konfiguration von Datum und Uhrzeit der jährlichen Beibehaltung |
| YearlyRetentionWeeksOfTheMonth |Text |Wochen des Monats, in denen die jährliche Beibehaltung konfiguriert ist. Beispiel: Erste, Letzte usw. |

### <a name="protected-server"></a>Geschützter Server
Diese Tabelle enthält grundlegende Felder und Aggregationen für verschiedene Felder, die sich auf geschützte Server beziehen.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| #ProtectedServers |Ganze Zahl |Anzahl geschützter Server |
| AsOnDateTime |Date/Time |Uhrzeit der letzten Aktualisierung der ausgewählten Zeile |
| AzureBackupAgentOSType |Text |Betriebssystemtyp des Azure Backup-Agents |
| AzureBackupAgentOSVersion |Text |Betriebssystemversion des Azure Backup-Agents |
| AzureBackupAgentUpdateDate |Text |Datum, an dem der Agent-Backup-Agent aktualisiert wurde |
| AzureBackupAgentVersion |Text |Nummer der Backup-Agent-Version |
| BackupManagementType |Text |Anbietertyp für die durchgeführte Sicherung. Beispielsweise IaaSVM, FileFolder. |
| EntityState |Text |Aktueller Status des Objekts des geschützten Servers. Beispielsweise Active (Aktiv), Deleted (Gelöscht). |
| ProtectedServerFriendlyName |Text |Anzeigename des geschützten Servers |
| ProtectedServerName |Text |Name des geschützten Servers |
| ProtectedServerType |Text |Typ des geschützten Servers, der gesichert wird. Beispielsweise IaaSVMContainer. |
| ProtectedServerName |Text |Name des geschützten Servers, zu dem das Sicherungselement gehört |
| RegisteredContainerId |Text |ID des Containers, der für die Sicherung registriert ist |

### <a name="storage"></a>Storage
Diese Tabelle enthält grundlegende Felder und Aggregationen für verschiedene speicherbezogene Felder.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| #ProtectedInstances |Dezimalzahl |Anzahl der geschützten Instanzen, die zum Berechnen von Front-End-Speicher in der Abrechnung verwendet werden. Die Berechnung erfolgt basierend auf dem letzten Wert im ausgewählten Zeitraum. |
| AsOnDateTime |Date/Time |Uhrzeit der letzten Aktualisierung der ausgewählten Zeile |
| CloudStorageInMB |Dezimalzahl |Von Sicherungen belegter Sicherungsspeicher in der Cloud. Die Berechnung erfolgt basierend auf dem letzten Wert im ausgewählten Zeitraum. |
| EntityState |Text |Aktueller Status des Objekts. Beispielsweise Active (Aktiv), Deleted (Gelöscht). |
| LastUpdatedDate |Date |Datum, an dem ausgewählte Zeile zuletzt aktualisiert wurde |

### <a name="time"></a>Time
Diese Tabelle enthält Details zu uhrzeitbezogenen Feldern.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| Hour |Time |Stunde des Tages. Beispiel: 13:00:00 Uhr |
| HourNumber |Dezimalzahl |Stundenangabe am Tag. Beispiel: 13:00 |
| Minute |Dezimalzahl |Minute der Stunde |
| PeriodOfTheDay |Text |Zeitfenster des Tages. Beispiel: 00–03 Uhr |
| Time |Time |Uhrzeit. Beispiel: 00:00:01 Uhr |
| TimeKey |Text |Schlüsselwert zum Darstellen der Zeit |

### <a name="vault"></a>Tresor
Diese Tabelle enthält grundlegende Felder und Aggregationen für verschiedene tresorbezogene Felder.

| Feld | Datentyp | BESCHREIBUNG |
| --- | --- | --- |
| #Vaults |Ganze Zahl |Anzahl der Tresore |
| AsOnDateTime |Date/Time |Uhrzeit der letzten Aktualisierung der ausgewählten Zeile |
| AzureDataCenter |Text |Rechenzentrum, in dem sich der Tresor befindet |
| EntityState |Text |Aktueller Status des Tresorobjekts. Beispielsweise Active (Aktiv), Deleted (Gelöscht). |
| StorageReplicationType |Text |Typ der Speicherreplikation für den Tresor. Beispiel: GeoRedundant. |
| SubscriptionId |Text |Abonnement-ID des Kunden, die zur Erstellung von Berichten ausgewählt ist |
| VaultName |Text |Name des Tresors |
| VaultTags |Text |Dem Tresor zugeordnete Tags |

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie das Datenmodell zum Erstellen von Azure Backup-Berichten überprüft haben, finden Sie in den folgenden Artikeln weitere ausführliche Informationen zum Erstellen und Anzeigen von Berichten in Power BI.

* [Erstellen von Berichten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Filtern von Berichten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
