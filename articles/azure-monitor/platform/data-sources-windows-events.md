---
title: Erfassen und Analysieren von Windows-Ereignisprotokollen in Azure Monitor | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie das Sammeln von Windows-Ereignisprotokollen mit Azure Monitor konfigurieren und Details zu den von ihnen erstellten Datensätzen finden.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/28/2018
ms.openlocfilehash: dd8f1e0e79f85c5d91966bcba13052f297422e67
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72932403"
---
# <a name="windows-event-log-data-sources-in-azure-monitor"></a>Datenquellen für Windows-Ereignisprotokolle in Azure Monitor
Windows-Ereignisprotokolle sind eine der häufigsten [Datenquellen](agent-data-sources.md) zum Sammeln von Daten mithilfe von Windows-Agents, da viele Anwendungen Daten in das Windows-Ereignisprotokoll schreiben.  Sie können Ereignisse aus Standardprotokollen wie beispielsweise dem System- und dem Anwendungsprotokoll sammeln und darüber hinaus benutzerdefinierte Protokolle angeben, die von den zu überwachenden Anwendungen erstellt werden.

![Windows-Ereignisse](media/data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Konfigurieren der Windows-Ereignisprotokolle
Sie konfigurieren Windows-Ereignisprotokolle über das [Menü „Daten“ in „Erweiterte Einstellungen“](agent-data-sources.md#configuring-data-sources).

Azure Monitor sammelt nur Ereignisse aus den Windows-Ereignisprotokollen, die in den Einstellungen angegeben wurden.  Sie können ein Ereignisprotokoll hinzufügen, indem Sie den Namen des Protokolls eingeben und auf **+** klicken.  Für jedes Protokoll werden nur die Ereignisse mit den ausgewählten Schweregraden gesammelt.  Überprüfen Sie die Schweregrade für das Protokoll, aus dem Sie Daten sammeln möchten.  Sie können keine zusätzlichen Kriterien zum Filtern von Ereignissen bereitstellen.

Während der Eingabe des Namens des Ereignisprotokolls bietet Azure Monitor Vorschläge gängiger Ereignisprotokollnamen an. Wenn das Protokoll, das Sie hinzufügen möchten, nicht in der Liste enthalten ist, können Sie es dennoch durch Eingabe des vollständigen Namens des Protokolls hinzufügen. Mithilfe der Ereignisanzeige können Sie den vollständigen Namen des Protokolls finden. Öffnen Sie in der Ereignisanzeige die Seite *Eigenschaften* des Protokolls, und kopieren Sie die Zeichenfolge aus dem Feld *Vollständiger Name*.

![Windows-Ereignisse konfigurieren](media/data-sources-windows-events/configure.png)

> [!NOTE]
> Kritische Ereignisse aus dem Windows-Ereignisprotokoll weisen in Azure Monitor-Protokollen den Schweregrad „Fehler“ auf.

## <a name="data-collection"></a>Datensammlung
Azure Monitor sammelt jedes Ereignis mit einem ausgewählten Schweregrad aus einem überwachten Ereignisprotokoll, sobald das Ereignis erstellt wird.  Der Agent zeichnet seine Position in jedem Ereignisprotokoll auf, aus dem er Daten sammelt.  Wenn der Agent für einen bestimmten Zeitraum offline geht, sammelt es Ereignisse ab dem Zeitpunkt der letzten Sammlung, unabhängig davon, ob die Ereignisse erstellt wurden, während der Agent offline war.  Es kann vorkommen, dass diese Ereignisse nicht erfasst werden, falls das Ereignisprotokoll umgebrochen wird und nicht erfasste Ereignisse überschrieben werden, während der Agent offline ist.

>[!NOTE]
>Azure Monitor erfasst keine Überwachungsereignisse, die von SQL Server aus der Quelle *MSSQLSERVER* mit der Ereignis-ID 18453 erstellt wurden und die Schlüsselwörter *Klassisch* oder *Überwachung erfolgreich* sowie das Schlüsselwort *0xa0000000000000* enthalten.
>

## <a name="windows-event-records-properties"></a>Eigenschaften von Windows-Ereignisdatensätzen
Windows-Ereignisdatensätze weisen den Typ **Event** auf und besitzen die in der folgenden Tabelle aufgeführten Eigenschaften:

| Eigenschaft | BESCHREIBUNG |
|:--- |:--- |
| Computer |Name des Computers, auf dem das Ereignis gesammelt wurde. |
| EventCategory |Kategorie des Ereignisses. |
| EventData |Alle Ereignisdaten im Rohdatenformat. |
| EventId |Nummer des Ereignisses. |
| EventLevel |Schweregrad des Ereignisses in numerischer Form. |
| EventLevelName |Schweregrad des Ereignisses in Textform. |
| EventLog |Name des Ereignisprotokolls, aus dem das Ereignis gesammelt wurde. |
| ParameterXml |Ereignisparameterwerte in XML-Format. |
| ManagementGroupName |Name der Verwaltungsgruppe für System Center Operations Manager-Agents.  Bei anderen Agents lautet dieser Wert „`AOI-<workspace ID>`“. |
| RenderedDescription |Ereignisbeschreibung mit Parameterwerten. |
| `Source` |Quelle des Ereignisses. |
| SourceSystem |Typ des Agents, auf dem das Ereignis gesammelt wurde. <br> OpsManager: Windows-Agent (entweder Direktverbindung oder von Operations Manager verwaltet) <br> Linux: Alle Linux-Agents  <br> AzureStorage – Azure-Diagnose |
| TimeGenerated |Datum und Uhrzeit, zu der das Ereignis in Windows erstellt wurde. |
| UserName |Benutzername des Kontos, in dem das Ereignis protokolliert wurde. |

## <a name="log-queries-with-windows-events"></a>Protokollabfragen mit Windows-Ereignissen
Die folgende Tabelle zeigt verschiedene Beispiele für Protokollabfragen, die Windows-Ereignisdatensätze abrufen.

| Abfragen | BESCHREIBUNG |
|:---|:---|
| Ereignis |Alle Windows-Ereignisse. |
| Event &#124; where EventLevelName == "error" |Alle Windows-Ereignisse mit dem Schweregrad „error“. |
| Event &#124; summarize count() by Source |Anzahl von Windows-Ereignissen nach Quelle. |
| Event &#124; where EventLevelName == "error" &#124; summarize count() by Source |Anzahl von Windows-Fehlerereignissen nach Quelle. |


## <a name="next-steps"></a>Nächste Schritte
* Konfigurieren Sie Log Analytics für die Sammlung von Daten aus anderen [Datenquellen](agent-data-sources.md) zur Analyse.
* Erfahren Sie mehr über [Protokollabfragen](../log-query/log-query-overview.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.  
* Konfigurieren Sie die [Sammlung von Leistungsindikatoren](data-sources-performance-counters.md) aus Ihren Windows-Agents.
