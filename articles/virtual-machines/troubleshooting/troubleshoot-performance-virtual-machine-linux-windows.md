---
title: Generische Problembehandlung bei Leistungsproblemen virtueller Azure-Computer mit Linux oder Windows
description: Dieser Artikel beschreibt die allgemeine Behandlung von Leistungsproblemen bei virtuellen Computern (VMs) durch Überwachen und Beobachten von Leistungsengpässen und bietet mögliche Korrekturen für Probleme, die auftreten können.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/18/2019
ms.author: v-miegge
ms.openlocfilehash: 3fdac123ee7bda9d91d96940aebd6bddf4ea00f8
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72790793"
---
# <a name="generic-performance-troubleshooting-for-azure-virtual-machine-running-linux-or-windows"></a>Generische Problembehandlung der Leistung virtueller Azure-Computer mit Linux oder Windows

Dieser Artikel beschreibt die allgemeine Behandlung von Leistungsproblemen bei virtuellen Computern (VMs) durch Überwachen und Beobachten von Leistungsengpässen und bietet mögliche Korrekturen für Probleme, die auftreten können. Neben Überwachung können Sie auch Perfinsights verwenden, um einen Bericht mit Best Practice-Empfehlungen und wichtigen Engpässen rund um E/A, CPU und Arbeitsspeicher bereitzustellen. PerfInsights ist sowohl für [Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfInsights)- als auch für [Linux](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfinsights-linux)-VMs in Azure verfügbar.

In diesem Artikel wird die Verwendung von Überwachung zur Diagnose von Leistungsengpässen erläutert.

## <a name="enabling-monitoring"></a>Aktivieren der Überwachung

### <a name="azure-iaas-virtual-machine-monitoring"></a>Überwachung von virtuellen Azure-IaaS-Computern

Zum Überwachen der Gast-VM verwenden Sie die Azure-VM-Überwachung, die Sie bei bestimmten Ressourcenbedingungen auf allgemeiner Ebene warnt. Informationen zum Überprüfen, ob die VM-Diagnose aktiviert ist, finden Sie in der [Übersicht über Azure-Ressourcenprotokolle](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-overview#collecting-resource-logs). Wenn Sie diese Meldung sehen, ist die Diagnose wahrscheinlich nicht aktiviert:

![Überwachung ist nicht aktiviert](media/troubleshoot-performance-virtual-machine-linux-windows/1-virtual-machines-monitoring-not-enabled.png)
 
### <a name="enable-vm-diagnostics-through-microsoft-azure-portal"></a>Aktivieren der VM-Diagnose im Microsoft Azure-Portal

So aktivieren Sie die VM-Diagnose:

1. Navigieren Sie zum virtuellen Computer.
2. Klicken Sie auf **Diagnoseeinstellungen**.
3. Wählen Sie das Speicherkonto aus, und klicken Sie auf **Überwachung auf Gastebene aktivieren**.

   ![Auf „Einstellungen“ und dann auf „Diagnose“ klicken](media/troubleshoot-performance-virtual-machine-linux-windows/2-virtual-machines-diagnostics.png)

Sie können das Speicherkonto, das für die Diagnoseeinrichtung verwendet wird, über die Registerkarte **Agent** unter **Diagnoseeinstellungen** überprüfen.

![Überprüfen des Speicherkontos](media/troubleshoot-performance-virtual-machine-linux-windows/3-check-storage-account.png)

### <a name="enable-storage-account-diagnostics-through-azure-portal"></a>Aktivieren der Speicherkontodiagnose im Azure-Portal

Der Speicher ist eine sehr wichtige Ebene, wenn wir beabsichtigen, E/A-Leistung für einen virtuellen Computer in Azure zu analysieren. Für speicherbezogene Metriken müssen wir die Diagnose als zusätzlichen Schritt aktivieren. Diese kann auch aktiviert werden, wenn nur die speicherbezogenen Leistungsindikatoren analysiert werden sollen.

1. Bestimmen Sie, welches Speicherkonto (oder welche Speicherkonten) Ihre VM verwendet, indem Sie die VM auswählen. Klicken Sie auf **Einstellungen** und dann auf **Datenträger**:

   ![Auf „Einstellungen“ und dann auf „Datenträger“ klicken](media/troubleshoot-performance-virtual-machine-linux-windows/4-storage-disks-disks-selection.png)

2. Navigieren Sie im Portal zu dem Speicherkonto (oder den Speicherkonten) der VM, und arbeiten Sie sich durch die folgenden Schritte:

   1. Klicken Sie für das Speicherkonto, das Sie mit dem oben beschriebenen Schritt ermittelt haben, auf „Übersicht“.
   2. Standardmetriken werden angezeigt. 

    ![Standardmetriken](media/troubleshoot-performance-virtual-machine-linux-windows/5-default-metrics.png)

3. Klicken Sie auf eine der Metriken, damit ein weiteres Blatt mit weiteren Optionen zum Konfigurieren und Hinzufügen von Metriken angezeigt wird.

   ![Metriken hinzufügen](media/troubleshoot-performance-virtual-machine-linux-windows/6-add-metrics.png)

So konfigurieren Sie diese Optionen:

1.  Klicken Sie auf **Metriken**.
2.  Wählen Sie die **Ressource** (Speicherkonto) aus.
3.  Wählen Sie den **Namespace** aus.
4.  Wählen Sie **Metrik** aus.
5.  Wählen Sie den Typ der **Aggregation** aus.
6.  Sie können diese Ansicht an das Dashboard anheften.

## <a name="observing-bottlenecks"></a>Beobachten von Engpässen

Nachdem wir den anfänglichen Einrichtungsvorgang für die erforderlichen Metriken abgeschlossen und die Diagnose für die VM und das zugehörige Speicherkonto aktiviert haben, können wir in die Analysephase wechseln.

### <a name="accessing-the-monitoring"></a>Zugriff auf die Überwachung

Wählen Sie die zu untersuchende Azure-VM und dann **Überwachung** aus.

![Überwachung auswählen](media/troubleshoot-performance-virtual-machine-linux-windows/7-select-monitoring.png)
 
### <a name="timelines-of-observation"></a>Beobachtungszeitskalen

Um zu bestimmen, ob Ressourcenengpässe vorliegen, überprüfen Sie Ihre Daten. Wenn Sie herausfinden, dass Ihr Computer einwandfrei gelaufen ist, in jüngster Zeit aber nachlassende Leistung gemeldet wurde, überprüfen Sie einen Zeitbereich der Daten, der Leistungsmetriken vor der gemeldeten Änderung sowie während und nach dem Problem umfasst.

### <a name="check-for-cpu-bottleneck"></a>Überprüfen der CPU auf Engpässe

![Überprüfen der CPU auf Engpässe](media/troubleshoot-performance-virtual-machine-linux-windows/8-cpu-bottleneck-time-range.png)

1. Bearbeiten Sie das Diagramm.
2. Legen Sie den Zeitraum fest.
3. Anschließend müssen Sie den Leistungsindikator hinzufügen: CPU-Prozentsatz Gastbetriebssystem
4. Speichern Sie.

### <a name="cpu-observe-trends"></a>CPU-Beobachtungstrends

Seien Sie sich bei der Untersuchung von Leistungsproblemen der Trends bewusst, und bemühen Sie sich darum, sie zu verstehen, wenn sie Sie betreffen. In den nächsten Abschnitten verwenden wir die Überwachungsdiagramme aus dem Portal, um Trends zu veranschaulichen. Sie können außerdem für Querverweise auf abweichendes Ressourcenverhalten innerhalb des gleichen Zeitraums nützlich sein. Klicken Sie zum Anpassen der Diagramme auf [Azure Monitor-Datenplattform](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform).

Spitzen: Spitzen können mit einer geplanten Aufgabe/einem bekannten Ereignis zusammenhängen. Wenn Sie die Aufgabe identifizieren können, bestimmen Sie, ob die Aufgabe auf der erforderlichen Leistungsstufe ausgeführt wird. Wenn die Leistung akzeptabel ist, müssen Sie möglicherweise keine weiteren Ressourcen zuweisen.

Steiler Anstieg und Plateau: Zeigt oftmals eine neue Workload an. Wenn die Workload nicht bekannt ist, aktivieren Sie Überwachung in der VM, um herauszufinden, welcher Prozess (oder Prozesse) dieses Verhalten verursacht. Sobald der Prozess bekannt ist, bestimmen Sie, ob sein höherer Ressourcenverbrauch durch ineffizienten Code oder normalen Verbrauch verursacht wird. Wenn es sich um normalen Verbrauch handelt, bestimmen Sie, ob der Prozess auf der normalen Leistungsstufe ausgeführt wird.

Plateau: Bestimmen Sie, ob Ihre VM im Betrieb schon immer dieses Niveau erreichte oder sie erst nach dem Aktivieren der Diagnose auf diesem Niveau ausgeführt wurde. Wenn dies der Fall ist, identifizieren Sie die Prozesse, die das Problem verursachen, und erwägen Sie, mehr von der betroffenen Ressourcen hinzuzufügen.

Stetige Zunahme: Ein ständiger Anstieg im Ressourcenverbrauch ist oftmals entweder auf ineffizienten Code oder auf einen Prozess zurückzuführen, der mehr Benutzerworkload übernimmt.

### <a name="high-cpu-utilization-remediation"></a>Wartungsmaßnahmen bei hoher CPU-Auslastung

Wenn Ihre Anwendung oder Ihr Prozess nicht auf der richtigen Leistungsstufe ausgeführt wird und Sie eine konstante CPU-Auslastung von über 95 % beobachten, können Sie eine der beiden folgenden Aufgaben ausführen:

* Zur sofortigen Abhilfe: Erhöhen Sie die Größe der VM auf eine Größe, die mehr Kerne aufweist
* Verstehen des Problems: Bestimmen Sie die Anwendung/den Prozess, und führen Sie eine entsprechende Problembehandlung durch.

Wenn Sie die VM vergrößert haben und die CPU immer noch zu 95 % ausgelastet ist, bestimmen Sie, ob diese Einstellung bessere Leistung oder einen höheren Anwendungsdurchsatz auf einer akzeptablen Stufe bietet. Führen Sie andernfalls eine Problembehandlung der einzelnen Anwendung/des einzelnen Prozesses durch.

Sie können Perfinsights für [Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfInsights) oder [Linux](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfinsights-linux) verwenden, um zu analysieren, welcher Prozess für den CPU-Verbrauch verantwortlich ist. 

## <a name="check-for-memory-bottleneck"></a>Prüfen auf Speicherengpass

So zeigen Sie die Metriken an:

1. Fügen Sie einen Abschnitt hinzu.
2. Fügen Sie eine Kachel hinzu.
3. Öffnen Sie den Katalog.
4. Wählen Sie „Speicherauslastung“ aus, und ziehen Sie das Objekt auf die Kachel. Wenn die Kachel angedockt ist, klicken Sie mit der rechten Maustaste, und wählen Sie **6x4** aus.

### <a name="memory-observe-trends"></a>Arbeitsspeicher-Beobachtungstrends

Die Speicherauslastung zeigt Ihnen, wie viel Arbeitsspeicher von der VM verbraucht wird. Verstehen Sie den Trend, und überprüfen Sie, ob er sich den Zeiten zuordnen lässt, zu denen Sie Probleme beobachten. Sie sollten zu jeder Zeit mehr als 100 MB verfügbaren Arbeitsspeicher haben.

Steiler Anstieg und Plateau/Gleichbleibend hohe Auslastung: Hohe Speicherauslastung ist möglicherweise nicht die Ursache schlechter Leistung, da einige Anwendungen, wie etwa relationale Datenbank-Engines, eine große Menge Arbeitsspeicher zuordnen, und diese Auslastung ist möglicherweise nicht ausreichend. Wenn mehrere speicherhungrige Anwendungen ausgeführt werden, kann jedoch schlechte Leistung aufgrund der Konkurrenz um Arbeitsspeicher auftreten, was zu Zuschneiden und Auslagern auf Datenträger führen kann. Diese schlechte Leistung ist oftmals in erheblichem Maß ursächlich für die Leistung von Anwendungen.

Ständig zunehmender Verbrauch: Dieses Verbrauchsmuster stellt ein mögliches „Warmlaufen“ von Anwendungen dar, das sich häufig bei neu gestarteten Datenbank-Engines findet. Es könnte jedoch ebenso ein Hinweis auf ein Speicherleck in einer Anwendung sein. Identifizieren Sie die Anwendung, und verstehen Sie, ob es sich um ein erwartetes Verhalten handelt.

Nutzung der Auslagerungsdatei: Überprüfen Sie, ob die Windows-Auslagerungsdatei (die sich unter D:\) befindet) oder die Linux-Auslagerungsdatei (auf `/dev/sdb`) in hohem Maß verwendet werden. Wenn auf diesen Volumes nichts außer diesen Dateien vorhanden ist, prüfen Sie diese Datenträger auf hohe Vorkommen von Lese-/Schreibvorgängen. Dieses Problem weist auf einen Zustand mit wenig Arbeitsspeicher hin.

### <a name="high-memory-utilization-remediation"></a>Wartungsmaßnahme bei hoher Speicherauslastung

Um hohe Speicherauslastung zu beheben, führen Sie eine der folgenden Aufgaben aus:

* Sofortige Abhilfe bei Nutzung der Auslagerungsdatei: Erhöhen Sie die Größe der VM-Datei auf eine mit mehr Arbeitsspeicher, und führen Sie dann eine Überwachung durch.
* Verstehen des Problems: Spüren Sie Anwendungen/Prozesse auf, und führen Sie dann eine Problembehandlung aus, um Anwendungen mit hohem Speicherverbrauch zu ermitteln.
* Wenn Sie die Anwendung kennen, prüfen Sie, ob die Speicherzuweisung gedeckelt werden kann.

Wenn Sie nach dem Upgrade auf eine größere VM feststellen, dass Sie immer noch eine konstante ständige Zunahme bis 100 % verzeichnen, identifizieren Sie die Anwendung/den Prozess, und führen Sie eine Problembehandlung durch.

Sie können Perfinsights für [Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfInsights) oder [Linux](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfinsights-linux) verwenden, um zu analysieren, welcher Prozess für den Arbeitsspeicherverbrauch verantwortlich ist. 

## <a name="check-for-disk-bottleneck"></a>Überprüfen der Datenträger auf Engpässe

Um das Speichersubsystem der VM zu überprüfen, überprüfen Sie die Diagnoseprotokolle auf der Azure VM-Ebene mithilfe der Leistungsindikatoren in der VM-Diagnose und ebenfalls der Speicherkontodiagnose.

Für die VM-spezifische Problembehandlung können Sie Perfinsights für [Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfInsights) oder [Linux](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfinsights-linux) verwenden, um zu analysieren, welcher Prozess für die E/A-Vorgänge verantwortlich ist. 

Beachten Sie, dass keine Leistungsindikatoren für Zonenredundanz und Premium-Speicherkonten vorhanden sind. Bei Problemen im Zusammenhang mit diesen Leistungsindikatoren öffnen Sie eine Supportanfrage.

### <a name="viewing-storage-account-diagnostics-in-monitoring"></a>Anzeigen der Speicherkontodiagnose in der Überwachung

Um an den Elementen unten zu arbeiten, wechseln Sie zum Speicherkonto für die VM im Portal:

![Anzeigen der Speicherkontodiagnose in der Überwachung](media/troubleshoot-performance-virtual-machine-linux-windows/9-virtual-machine-storage-account.png)

1. Bearbeiten Sie das Überwachungsdiagramm.
2. Legen Sie den Zeitraum fest.
3. Fügen Sie die in den Schritten unten beschriebenen Leistungsindikatoren hinzu.
4. Speichern Sie die Änderungen.

### <a name="disk-observe-trends-standard-storage-only"></a>Beobachtungstrends bei Datenträgern (nur Standardspeicher)

Um Probleme mit dem Speicher zu erkennen, sehen Sie sich die Leistungsmetriken der Speicherkontodiagnose und der VM-Diagnose an.

Suchen Sie für jede Überprüfung unten nach Schlüsseltrends, wenn die Probleme innerhalb des Zeitraums dieses Problems auftreten.

#### <a name="check-azure-storage-availability--add-the-storage-account-metric-availability"></a>Überprüfen der Verfügbarkeit von Azure Storage – Hinzufügen der Speicherkontometrik: Verfügbarkeit

Wenn Sie ein Nachlassen der Verfügbarkeit finden, liegt möglicherweise ein Problem mit der Plattform vor, überprüfen Sie den [Azure-Status](https://azure.microsoft.com/status/). Wenn hier kein Problem angezeigt wird, öffnen Sie eine neue Supportanfrage.

#### <a name="check-for-azure-storage-timeout---add-the-storage-account-metrics"></a>Prüfen auf Timeouts bei Azure Storage Hinzufügen der Speicherkontometriken:

* ClientTimeOutError
* ServerTimeOutError
* AverageE2ELatency
* AverageServerLatency
* TotalRequests

Werte in den *TimeOutError-Metriken zeigen an, dass ein E-/A-Vorgang zu lange benötigte und ein Timeout auftrat. Das Durcharbeiten der nächsten Schritte hilft Ihnen bei der Bestimmung der möglichen Ursachen.

AverageServerLatency steigt gleichzeitig an – die TimeOutErrors stellen möglicherweise ein Plattformproblem dar. Öffnen Sie in dieser Situation eine neue Supportanfrage.

AverageE2ELatency stellt die Clientlatenz dar. Überprüfen Sie, welche Leistung die Anwendung bei IOPS erbringt. Suchen Sie nach einem Anstieg oder konstant hohen Metriken für TotalRequests. Diese Metrik stellt IOPS dar. Wenn Sie sich den Grenzen des Speicherkontos oder der einzelnen virtuellen Festplatte annähern, ist die Latenz möglicherweise auf Drosselung zurückzuführen.

#### <a name="check-for-azure-storage-throttling---add-the-storage-account-metrics-throttlingerror"></a>Überprüfung auf Drosselung von Azure Storage: Fügen Sie diese Speicherkontometrik hinzu: ThrottlingError

Werte für die Drosselung weisen darauf hin, dass eine Einschränkungen auf der Ebene des Speicherkontos auftritt, was bedeutet, dass Sie sich dem IOPS-Grenzwert des Kontos annähern. Sie können bestimmen, ob Sie sich dem IOPs-Schwellenwert annähern, indem Sie die Metrik **TotalRequests** überprüfen.

Beachten Sie, dass jede virtuelle Festplatte einen Grenzwert von 500 IOPS oder 60 MBit aufweist, jedoch durch den kumulativen Grenzwert von 20.000 IOPS pro Speicherkonto gebunden ist.

Auf der Grundlage dieser Metrik können Sie nicht bestimmen, welcher Blob die Drosselung bewirkt und welche Blobs von ihr betroffen sind. Klar ist aber, dass Sie entweder den IOPS-Grenzwert oder den eingehenden/ausgehenden Grenzwert des Speicherkontos erreichen.

Um zu bestimmen, ob Sie den IOPS-Grenzwert erreichen, wechseln Sie zur Speicherkontodiagnose, und überprüfen Sie TotalRequests, um festzustellen, ob Sie sich an die 20 tausend TotalRequests annähern. Bestimmen Sie entweder eine Änderung im Muster, ob Sie diesen Grenzwert zum ersten Mal erreichen oder ob dieser Grenzwert zu bestimmten Zeiten erreicht wird.

Mit neuen Datenträgerangeboten unter Storage Standard können die IOPS- und Durchsatzlimits abweichen, aber das kumulative Limit für das Storage Standard-Speicherkonto beträgt 20.000 IOPS (Storage Premium hat unterschiedliche Grenzwerte auf Konto- oder Datenträgerebene). Weitere Informationen zu Storage Standard-Datenträgerangeboten und Grenzwerten pro Datenträger:

* [Skalierbarkeits- und Leistungsziele für VM-Datenträger unter Windows](https://docs.microsoft.com/azure/virtual-machines/windows/disk-scalability-targets).

#### <a name="references"></a>Referenzen

* [Skalierbarkeitsziele für Festplatten virtueller Computer](https://azure.microsoft.com/documentation/articles/storage-scalability-targets/#scalability-targets-for-virtual-machine-disks)

Die Bandbreite des Speicherkontos wird mithilfe der Speicherkontometriken gemessen: TotalIngress und TotalEgress. Je nach Redundanztyp und Region gelten verschiedene Schwellenwerte für die Bandbreite.

* [Skalierbarkeitsziele für Blobs, Warteschlangen, Tabellen und Dateien](https://azure.microsoft.com/documentation/articles/storage-scalability-targets/#scalability-targets-for-blobs-queues-tables-and-files)

Vergleichen Sie TotalIngress und TotalEgress mit den Grenzwerten für Ingress und Egress für den Redundanztyp und die Region des Speicherkontos.

Überprüfen Sie die Grenzwerte für den Durchsatz der mit der VM verbundenen VHDs. Fügen Sie die VM-Metriken „Datenträgerlesevorgänge“ und „Datenträgerschreibvorgänge“ hinzu.

Neue Datenträgerangebote in Storage Standard weisen unterschiedliche IOPS- und Durchsatzlimits auf (IOPS werden nicht pro VHD verfügbar gemacht). Sehen Sie sich die Daten an, um festzustellen, ob Sie auf der Grundlage der Datenlese- und -schreibvorgänge die Grenzwerte des kombinierten Durchsatzes in MB der VHD(s) erreichen, und optimieren Sie dann Ihre VM-Speicherkonfiguration, um eine Skalierung jenseits der Grenzwerte einzelner VHDs zu erreichen. Weitere Informationen zu Storage Standard-Datenträgerangeboten und Grenzwerten pro Datenträger:

* [Skalierbarkeits- und Leistungsziele für VM-Datenträger unter Windows](https://docs.microsoft.com/azure/virtual-machines/windows/disk-scalability-targets).

### <a name="high-disk-utilizationlatency-remediation"></a>Wartung bei hoher Datenträgerverwendung/Latenz

Verringern der Clientlatenz und Optimieren der VM-E/A, um eine Skalierung jenseits der VHD-Grenzwerte zu erreichen

* [Optimieren der E/A für Windows in Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-performance-best-practices/)

* [Optimieren der E/A für Linux in Azure](https://blogs.msdn.microsoft.com/igorpag/2014/10/23/azure-storage-secrets-and-linux-io-optimizations/)

#### <a name="reduce-throttling"></a>Verringern der Drosselung

Wenn die oberen Grenzwerte von Speicherkonten erreicht werden, gleichen Sie die VHDs zwischen den einzelnen Speicherkonten neu aus. Informationen hierzu finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](https://azure.microsoft.com/documentation/articles/storage-scalability-targets/).

### <a name="increase-throughput-and-reduce-latency"></a>Vergrößern des Durchsatzes und Verringern der Latenz

Wenn Sie eine für Latenz anfällige Anwendung ausführen und hohen Durchsatz benötigen, migrieren Sie Ihre VHDs zu Azure Premium Storage, indem Sie VMs der DS- und GS-Reihe verwenden.

Die spezifischen Szenarien werden in diesen Artikeln besprochen:

* [Migrieren zu Azure Premium-Speicher](https://azure.microsoft.com/documentation/articles/storage-migration-to-premium-storage/)

* [Verwenden von Azure Storage Premium mit SQL Server](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-use-premium-storage/)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, wenden Sie sich an die Azure-Experten im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/).

Alternativ dazu können Sie eine Azure-Supportanfrage erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
