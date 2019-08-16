---
title: Allgemeine Fragen zur Azure Site Recovery-Überwachung
description: Sie finden hier Antworten auf häufig gestellte Fragen zur Azure Site Recovery-Überwachung mithilfe der integrierten Überwachung und Azure Monitor (Log Analytics).
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 07/31/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: c1d30a9cdd2cd6ca288edd609a2e2e7bee9174d7
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718161"
---
# <a name="common-questions-about-site-recovery-monitoring"></a>Allgemeine Fragen zur Site Recovery-Überwachung

In diesem Artikel werden häufig gestellte Fragen zur Überwachung von Azure [Site Recovery](site-recovery-overview.md) mithilfe der integrierten Site Recovery-Überwachung und Azure Monitor (Log Analytics) beantwortet.

## <a name="general"></a>Allgemein

### <a name="how-is-the-rpo-value-logged-different-from-the-latest-available-recovery-point"></a>Wie unterscheidet sich der protokollierte RPO-Wert vom letzten verfügbaren Wiederherstellungspunkt?

Site Recovery verwendet einen mehrstufigen asynchronen Prozess zum Replizieren von Computern in Azure.

- Im vorletzten Schritt der Replikation werden aktuelle Änderungen auf dem Computer sowie Metadaten in ein Protokoll-/Cachespeicherkonto kopiert.
- Diese Änderungen werden zusammen mit dem Tag zum Identifizieren eines wiederherstellbaren Punkts in das Speicherkonto/den verwalteten Datenträger in der Zielregion geschrieben.
- Site Recovery kann nun einen wiederherstellbaren Punkt für den Computer generieren.
- An diesem Punkt ist die RPO für die Änderungen erfüllt, die bisher in das Speicherkonto hochgeladen wurden. Mit anderen Worten: Die RPO für den Computer entspricht an diesem Punkt der Zeitspanne, die seit dem Zeitstempel verstrichen ist, der dem wiederherstellbaren Punkt entspricht.
- Site Recovery wählt nun die hochgeladenen Daten im Speicherkonto aus und wendet sie auf die Replikatdatenträger an, die für den Computer erstellt wurden.
- Site Recovery generiert dann einen Wiederherstellungspunkt und macht diesen Punkt beim Failover für eine Wiederherstellung verfügbar.
- Daher gibt der letzte verfügbare Wiederherstellungspunkt den Zeitstempel an, der dem letzten Wiederherstellungspunkt entspricht, der bereits verarbeitet und auf die Replikatdatenträger angewandt wurde.


Eine falsche Systemzeit auf dem replizierenden Quellcomputer oder auf den lokalen Infrastrukturservern verfälscht den berechneten RPO-Wert. Um sicherzustellen, dass die RPO-Werte genau berichtet werden, achten Sie darauf, dass die Systemuhr auf allen Servern und Computern korrekt eingestellt ist.



## <a name="inbuilt-site-recovery-logging"></a>Integrierte Site Recovery-Protokollierung


### <a name="why-is-the-vm-count-in-the-vault-infrastructure-view-different-from-the-total-count-shown-in-replicated-items"></a>Warum unterscheidet sich die Anzahl der virtuellen Computer in der Tresorinfrastrukturansicht von der Gesamtanzahl, die in den replizierten Elementen angezeigt wird?

Die Tresorinfrastrukturansicht ist nach Replikationsszenarien begrenzt. Nur Computer im aktuell ausgewählten Replikationsszenario werden in die Anzahl für die Ansicht einbezogen. Darüber hinaus werden nur virtuelle Computer gezählt, die für die Replikation in Azure konfiguriert sind. Computer, für die ein Failover ausgeführt wurde, oder Computer, die zurück am lokalen Standort repliziert werden, werden nicht in die Ansicht aufgenommen.

### <a name="why-is-the-count-of-replicated-items-in-essentials-different-from-the-total-count-of-replicated-items-on-the-dashboard"></a>Warum unterscheidet sich die Anzahl der replizierten Elemente in der Zusammenfassung von der Gesamtanzahl der auf dem Dashboard angezeigten replizierten Elemente?

Nur die Computer, für die die erste Replikation abgeschlossen wurde, sind in der angezeigten Anzahl in der Zusammenfassung enthalten. Die replizierten Elemente enthalten insgesamt alle Computer im Tresor, einschließlich der Computer, für die die erste Replikation gerade ausgeführt wird.

## <a name="azure-monitor-logging"></a>Azure Monitor-Protokollierung


### <a name="how-often-does-site-recovery-send-diagnostic-logs-to-azure-monitor-log"></a>Wie oft sendet Site Recovery Diagnoseprotokolle an Azure Monitor-Protokolle? 

- AzureSiteRecoveryReplicationStats und AzureSiteRecoveryRecoveryPoints werden alle 15 Minuten gesendet.  
- AzureSiteRecoveryReplicationDataUploadRate und AzureSiteRecoveryProtectedDiskDataChurn werden alle fünf Minuten gesendet. 
- AzureSiteRecoveryJobs wird beim Auslösen und beim Abschluss eines Auftrags gesendet.
- AzureSiteRecoveryEvents wird immer dann gesendet, wenn ein Ereignis generiert wird. 
- AzureSiteRecoveryReplicatedItems wird bei jeder Umgebungsänderung gesendet. In der Regel beträgt die Datenaktualisierungszeit 15 Minuten nach einer Änderung. 

### <a name="how-long-is-data-kept-in-azure-monitor-logs"></a>Wie lange werden Daten in Azure Monitor-Protokollen aufbewahrt? 

Standardmäßig beträgt die Speicherdauer 31 Tage. Sie können den Zeitraum im Abschnitt **Nutzung und geschätzte Kosten** im Log Analytics-Arbeitsbereich verlängern. Klicken Sie auf **Datenaufbewahrung**, und wählen Sie den Zeitraum aus.

### <a name="whats-the-size-of-the-diagnostic-logs"></a>Wie groß sind die Diagnoseprotokolle? 

In der Regel beträgt die Größe eines Protokolls 15–20 KB. 


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie die Überwachung mit der [integrierten Site Recovery-Überwachung](site-recovery-monitor-and-troubleshoot.md) oder [Azure Monitor](monitor-log-analytics.md) durchführen.


