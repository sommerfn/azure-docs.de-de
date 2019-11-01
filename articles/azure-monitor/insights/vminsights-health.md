---
title: Grundlegendes zur Integrität Ihrer Azure-VMs | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Integrität der VM und des zugrunde liegenden Betriebssystems mithilfe von Azure Monitor für VMs beurteilen können.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 10/15/2019
ms.openlocfilehash: e19ba55e48c537974ad4136d40505514b92d387d
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73162291"
---
# <a name="understand-the-health-of-your-azure-virtual-machines"></a>Grundlegendes zur Integrität Ihrer Azure-VMs

Azure enthält Dienste für bestimmte Rollen oder Aufgaben im Überwachungsbereich. Es sind jedoch keine ausführlichen Integritätseinblicke für Betriebssysteme enthalten, die auf Azure-VMs gehostet werden. Sie können Azure Monitor zwar für verschiedene Situationen einsetzen, die Plattform ist jedoch nicht darauf ausgelegt, die Integrität von Kernkomponenten oder die Gesamtintegrität der VMs zu modellieren und darzustellen.

Mithilfe der Integritätsfunktionen von Azure Monitor für VMs können Sie die Verfügbarkeit und Leistung eines Windows- oder Linux-Gastbetriebssystems aktiv überwachen. Das Integritätsfeature nutzt ein Modell, das die Hauptkomponenten und ihre Beziehungen darstellt und Kriterien angibt, anhand derer die Integrität der Komponente bewertet wird. Wenn ein fehlerhafter Zustand erkannt wird, wird eine Warnung gesendet.

Die Anzeige des Integritätsgesamtzustands von Azure-VMs und des zugrunde liegenden Betriebssystems kann aus zwei verschiedenen Perspektiven erfolgen: direkt in einer VM oder für alle in einer Ressourcengruppe enthaltenen VMs mithilfe von Azure Monitor.

In diesem Artikel wird veranschaulicht, wie Sie Integritätsprobleme schnell bewerten, untersuchen und beheben können, wenn Sie vom Integritätsfeature von Azure Monitor für VMs erkannt werden.

Informationen zum Konfigurieren von Azure Monitor for VMs finden Sie unter [Enable Azure Monitor for VMs](vminsights-enable-overview.md) (Aktivieren von Azure Monitor for VMs).

## <a name="monitoring-configuration-details"></a>Details der Überwachungskonfiguration

In diesem Abschnitt werden die standardmäßig angewendeten Integritätskriterien für die Überwachung von Azure-VMs mit Windows und Linux beschrieben. Alle Integritätskriterien sind dafür vorkonfiguriert, eine Warnung zu senden, wenn ein fehlerhafter Zustand erkannt wird.

| Überwachungsname | Häufigkeit (Minuten) | Rückschaudauer (Minuten) | Operator | Schwellenwert | Benachrichtigung bei Status | severity | Workloadkategorie | 
|--------------|-----------|----------|----------|-----------|----------------|----------|-------------------|
| Logischer Datenträger – online | 5 | 15 | <> | 1 (wahr) | Kritisch | Sev1 | Linux | 
| Logischer Datenträger – verfügbarer Speicherplatz | 5 | 15 | < | 200 MB (Warnung)<br> 100 MB (Kritisch) | Warnung | Sev1<br> Sev2 | Linux | 
| Logischer Datenträger – freie I-Knoten (%) | 5 | 15 | < | 5 % | Kritisch | Sev1 | Linux | 
| Logischer Datenträger – verfügbarer Speicherplatz (%) | 5 | 15 | < | 5 % | Kritisch | Sev1 | Linux | 
| Netzwerkadapterstatus | 5 | 15 | <> | 1 (wahr) | Warnung | Sev2 | Linux | 
| Betriebssystem – verfügbare Megabyte Arbeitsspeicher | 5 | 10 | < | 2,5 MB | Kritisch | Sev1 | Linux | 
| Datenträger Durchschn. Datenträger s/gelesen | 5 | 25 | > | 0,05 s | Kritisch | Sev1 | Linux | 
| Datenträger Durchschn. Datenträger s/übertragen | 5 | 25 | > | 0,05 s | Kritisch | Sev1 | Linux | 
| Datenträger Durchschn. Datenträger s/geschrieben | 5 | 25 | > | 0,05 s | Kritisch | Sev1 | Linux | 
| Datenträgerstatus | 5 | 25 | <> | 1 (wahr) | Kritisch | Sev1 | Linux | 
| Betriebssystem – Prozessorzeit (%) gesamt | 5 | 10 | >= | 95 % | Kritisch | Sev1 | Linux | 
| CPU-Auslastung (%) gesamt | 5 | 10 | >= | 95 % | Kritisch | Sev1 | Windows | 
| Fehler oder Beschädigung im Dateisystem | 60 | 60 | <> | 4 | Kritisch | Sev1 | Windows | 
| Logischer Datenträger – mittlere Dauer pro Lesevorgang | 1 | 15 | > | 0,04 s | Warnung | Sev2 | Windows | 
| Logischer Datenträger – mittlere Dauer pro Übertragung | 1 | 15 | > | 0,04 s | Warnung | Sev2 | Windows | 
| Logischer Datenträger – mittlere Dauer pro Schreibvorgang (logischer Datenträger) | 1 | 15 | > | 0,04 s | Warnung | Sev2 | Windows | 
| Aktuelle Länge der Datenträgerwarteschlange (Logischer Datenträger) | 5 | 60 | >= | 32 | Warnung | Sev2 | Windows | 
| Logischer Datenträger – verfügbarer Speicherplatz (MB) | 15 | 60 | > | 500 MB – Warnung<br> 300 MB – Kritisch | Kritisch | Sev1<br> Sev2 | Windows | 
| Logischer Datenträger – verfügbarer Speicherplatz (%) | 15 | 60 | > | 10 % – Warnung<br> 5 % – Kritisch | Kritisch | Sev1<br> Sev2 | Windows |
| Logischer Datenträger – Leerlaufzeit (%) | 15 | 360 | <= | 20% | Warnung | Sev2 | Windows | 
| Genutzte Bandbreite (%) beim Lesen | 5 | 60 | >= | 60 % | Warnung | Sev2 | Windows | 
| Genutzte Bandbreite (%) gesamt | 5 | 60 | >= | 75 % | Warnung | Sev2 | Windows | 
| Genutzte Bandbreite (%) beim Schreiben | 5 | 60 | >= | 60 % | Warnung | Sev2 | Windows | 
| Integrität des DHCP-Clientdiensts | 5 | 12 | <> | 4 (bei Ausführung) | Kritisch | Sev1 | Windows | 
| Integrität des DNS-Clientdiensts | 5 | 12 | <> | 4 (bei Ausführung) | Kritisch | Sev1 | Windows | 
| Integrität des Windows-Ereignisprotokolldiensts | 5 | 12 | <> | 4 (bei Ausführung) | Kritisch | Sev1 | Windows | 
| Integrität des Windows-Firewalldiensts | 5 | 12 | <> | 4 (bei Ausführung) | Kritisch | Sev1 | Windows | 
| Integrität des RPC-Diensts | 5 | 12 | <> | 4 (bei Ausführung) | Kritisch | Sev1 | Windows | 
| Integrität des Serverdiensts | 5 | 12 | <> | 4 (bei Ausführung) | Kritisch | Sev1 | Windows | 
| Integrität des Windows-Remoteverwaltungsdiensts | 5 | 12 | <> | 4 (bei Ausführung) | Kritisch | Sev1 | Windows | 
| Verfügbare Megabyte Arbeitsspeicher | 5 | 10 | < | 100 MB | Kritisch | Sev1 | Windows | 
| Freie Tabelleneinträge auf Systemseite | 5 | 10 | <= | 5\.000 | Kritisch | Sev1 | Windows | 
| Speicherseiten pro Sekunde | 5 | 10 | >= | 5000/s | Warnung | Sev1 | Windows | 
| Verwendeter Prozentsatz des zugesicherten Speichers | 5 | 10 | > | 80 % | Kritisch | Sev1 | Windows | 
| Datenträger – mittlere Dauer pro Übertragung | 1 | 15 | > | 0,04 s | Warnung | Sev2 | Windows | 
| Datenträger – mittlere Dauer pro Schreibvorgang | 1 | 15 | > | 0,04 s | Warnung | Sev2 | Windows | 
| Aktuelle Warteschlangenlänge | 5 | 60 | >= | 32 | Warnung | Sev2 | Windows | 
| Datenträger – Leerlaufzeit (%) | 5 | 60 | >= | 20% | Warnung | Sev2 | Windows | 

>[!NOTE]
>Die Rückschaudauer gibt an, wie oft das Rückschaufenster die Metrikwerte überprüft, z.B. in den letzten fünf Minuten.  

>[!NOTE]
>Die Häufigkeit gibt an, wie oft die Metrikwarnung überprüft, ob die Bedingungen erfüllt sind, z.B. jede Minute.  Dies ist die Rate, mit der das Integritätskriterium ausgeführt wird, und die Rückschau ist die Dauer, über die das Integritätskriterium ausgewertet wird. Beispielsweise wird das Integritätskriterium ausgewertet, wenn die Bedingung **CPU-Auslastung** mit einer Häufigkeit von 5 Minuten mehr als 95 Prozent beträgt und 15 Minuten (3 aufeinanderfolgende Evaluierungszyklen) lang bei mehr als 95 % verbleibt. In diesem Fall wird der Status auf den Schweregrad „Kritisch“ aktualisiert, wenn dies bisher noch nicht geschehen ist.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich an.

## <a name="introduction-to-azure-monitor-for-vms-health"></a>Einführung in die Integrität mit Azure Monitor für VMs

Bevor Sie das Integritätsfeature für eine oder mehrere VMs einsetzen, müssen Sie sich damit vertraut machen, wie die Informationen dargestellt werden und was die Visualisierungen bedeuten.

### <a name="view-health-directly-from-a-vm"></a>Direktes Anzeigen der Integrität in einer VM

Wählen Sie im linken Bereich der VM **Einblicke (Vorschau)** aus, um die Integrität einer Azure-VM anzuzeigen. Auf der Seite mit den Einblicken in die VM ist standardmäßig die Registerkarte **Integrität** geöffnet, die die Integritätsansicht der VM anzeigt.

![Azure Monitor for VMs-Integritätsübersicht einer ausgewählten Azure-VM](./media/vminsights-health/vminsights-directvm-health-01.png)

Die Tabelle im Abschnitt **Integrität der Gast-VM** enthält das Integritätsrollup der Leistungskomponenten, die anhand von Integritätskriterien für den virtuellen Computer überwacht werden, sowie die Gesamtanzahl von VM-Integritätswarnungen, die von fehlerhaften Komponenten ausgelöst wurden. Zu diesen Komponenten zählen die **CPU**, der **Arbeitsspeicher**, die **Festplatte** und das **Netzwerk**. Erweitern Sie das Chevron neben „Integrität der Gast-VM“, um die Integrität der Komponenten anzuzeigen.

![Azure Monitor für VMs: Komponentenintegritätsstatus eines ausgewählten virtuellen Azure-Computers](./media/vminsights-health/vminsights-directvm-health-02.png)

Wenn Sie den Status neben der gewünschten Komponente auswählen, wird die Oberfläche für die Integritätsdiagnose im Kontext der ausgewählten Komponente geöffnet. Dort werden die einzelnen Statusaspekte der Komponente angezeigt und die für die Integritätsberechnung verwendeten Integritätskriterien beschrieben. Weitere Informationen finden Sie unter [Integritätsdiagnose](#health-diagnostics) im Abschnitt zur Verwendung von Integritätskriterien. Weitere Informationen finden Sie unter [Grundlegendes zur Integrität Ihrer Azure-VMs](#alerts).

Die für eine VM definierten Integritätszustände sind in der folgenden Tabelle beschrieben:

|Symbol |Integritätsstatus |Bedeutung |
|-----|-------------|---------------|
| |Healthy |Diese VM entspricht den definierten Integritätsbedingungen. Dieser Zustand bedeutet, dass keine Probleme erkannt wurden und die VM ordnungsgemäß funktioniert. Bei einem übergeordneten Rollupmonitor wird ein Rollup der Integrität durchgeführt, der den Zustand des untergeordneten Elements im günstigsten und ungünstigsten Fall wiedergibt.|
| |Kritisch |Der Zustand entspricht nicht den definierten Integritätsbedingungen. Das bedeutet, dass mindestens ein kritischer Fehler erkannt wurde. Diese Probleme müssen behoben werden, um die normale Funktionalität wiederherzustellen. Bei einem übergeordneten Rollupmonitor wird ein Rollup des Integritätszustand durchgeführt, der den Zustand des untergeordneten Elements im günstigsten und ungünstigsten Fall wiedergibt.|
| |Warnung |Der Zustand liegt zwischen zwei Schwellenwerten für die definierte Integritätsbedingung, wenn ein Wert den Zustand „Warnung“ und der andere Wert den Zustand „Kritisch“ angibt (drei Schwellenwerte für den Integritätszustand können konfiguriert werden), oder wenn ein nicht kritisches Problem zu kritischen Problemen führen kann, wenn es nicht behoben wird. Bei einem übergeordneten Rollupmonitor gibt das übergeordnete Element den Zustand „Warnung“ wieder, wenn mindestens eines der untergeordneten Elemente den Zustand „Warnung“ aufweist. Wenn ein untergeordnetes Element den Zustand „Kritisch“ und ein anderes untergeordnetes Element den Zustand „Warnung“ aufweist, weist das übergeordnete Rollup den Integritätszustand „Kritisch“ aus.|
| |Unknown |Der Zustand kann aus unterschiedlichen Gründen nicht berechnet werden. Im folgenden Abschnitt finden Sie zusätzliche Details und Lösungsansätze. |

Der Integritätszustand „Unbekannt“ kann durch die folgenden Probleme verursacht werden:

- Der Agent wurde neu konfiguriert und sendet keine Berichtsdaten mehr an den Arbeitsbereich, der beim Aktivieren von Azure Monitor für VMs angegeben wurde. Informationen zur Konfiguration des Agents zum Senden von Berichten an den Arbeitsbereich finden Sie unter [Hinzufügen oder Entfernen von Arbeitsbereichen](../platform/agent-manage.md#adding-or-removing-a-workspace).
- Die VM wurde gelöscht.
- Der Arbeitsbereich, der Azure Monitor für VMs zugeordnet ist, wurde gelöscht. Sie können den Arbeitsbereich wiederherstellen, wenn Sie Premier Support-Vorteile nutzen können. Navigieren Sie zu [Premier](https://premier.microsoft.com/), und öffnen Sie eine Supportanfrage.
- Die Abhängigkeiten der Lösung wurden gelöscht. Sie können die ServiceMap- und InfrastructureInsights-Lösungen in Ihrem Log Analytics-Arbeitsbereich wieder aktivieren, indem Sie ServiceMap-Lösung über die [Azure Resource Manager-Vorlage](vminsights-enable-at-scale-powershell.md#install-the-servicemap-solution) neu installieren. Wenn Sie die InfastructureInsights-Lösung neu installieren möchten, senden Sie eine E-Mail an vminsights@microsoft.com. 
- Die VM wurde heruntergefahren.
- Der Azure-VM-Dienst ist nicht verfügbar, oder es wird gerade eine Wartung durchgeführt.
- Für den Arbeitsbereich wurde das [Tages- oder Aufbewahrungslimit für Daten](../platform/manage-cost-storage.md) erreicht.

Wenn Sie auf **Integritätsdiagnose anzeigen** klicken, wird eine Seite geöffnet, auf der alle Komponenten der VM mit den ihnen zugeordneten Integritätskriterien, Zustandsänderungen und anderen Problemen dargestellt sind, die durch Überwachung der mit der VM zusammenhängenden Komponenten gefunden wurden.

Weitere Informationen finden Sie unter [Integritätsdiagnose](#health-diagnostics).

Im Bereich **Integrität** stellt eine Tabelle den Integritätsrollup der Leistungskomponenten dar, die anhand von Integritätskriterien überwacht werden. Zu diesen Komponenten zählen die **CPU**, der **Arbeitsspeicher**, die **Festplatte** und das **Netzwerk**. Wenn Sie alle Komponenten auswählen, wird eine Seite geöffnet, auf der alle Überwachungskriterien und der Integritätszustand der entsprechenden Komponente aufgeführt werden.

Wenn Sie über eine Azure-VM, auf der Windows ausgeführt wird, auf die Integrität zugreifen, wird der Integritätszustand der fünf Windows-Kerndienste unter **Integrität der Kerndienste** angezeigt. Durch Auswählen eines Diensts wird eine Seite geöffnet, auf der die Integritätskriterien zur Überwachung der betreffenden Komponente und ihr Integritätszustand aufgelistet werden.

Wenn Sie auf den Namen des Integritätskriteriums klicken, wird der Eigenschaftenbereich geöffnet. In diesem Bereich können Sie überprüfen, welche Konfigurationsdetails vorliegen und ob dem Integritätskriterium eine Azure Monitor-Warnung zugeordnet ist.

Weitere Informationen finden Sie unter [Integritätsdiagnose](#health-diagnostics) im Abschnitt zur Verwendung von Integritätskriterien.

### <a name="aggregate-vm-perspective"></a>Aggregieren der VM-Perspektive

Sie können die Integritätssammlung aller VMs in einer Ressourcengruppe anzeigen, indem Sie in der Navigationsliste im Portal auf **Azure Monitor** und dann auf **Virtuelle Computer (Vorschau)** klicken.

![VM Insights-Überwachungsansicht in Azure Monitor](./media/vminsights-health/vminsights-aggregate-health.png)

Wählen Sie in den Dropdownlisten **Abonnement** und **Ressourcengruppe** die Ressourcengruppe aus, die die zur Gruppe gehörenden VMs enthält, um ihren gemeldeten Integritätszustand anzuzeigen. Ihre Auswahl gilt nur für das Integritätsfeature, nicht für die Registerkarten **Leistung** oder **Zuordnung**.

Auf der Registerkarte **Integrität** werden die folgenden Informationen angezeigt:

* Wie viele VMs sich in einem kritischen oder fehlerhaften Zustand im Vergleich zur Anzahl der VMs in fehlerfreiem Zustand oder zu denjenigen befinden, die keine Daten übermitteln (Bezeichnung: unbekannter Zustand)
* Welche und wie viele VMs(nach Betriebssystem) einen fehlerhaften Zustand melden
* Wie viele VMs aufgrund eines bei einem Prozessor, einem Datenträger, beim Arbeitsspeicher oder bei einem Netzwerkadapter erkannten Problems fehlerhaft sind (nach Integritätszustand)
* Wie viele VMs aufgrund eines bei einem Kerndienst des Betriebssystems erkannten Problems fehlerhaft sind (nach Integritätszustand)

Auf der Registerkarte **Integrität** werden die kritischen Fehler angezeigt, die von der Überwachung der Integritätskriterien für die VM erkannt wurden. Dort können Sie auch Warnungsdetails überprüfen und die zugehörige Dokumentation aufrufen. Mithilfe dieser Artikel können Sie die Probleme diagnostizieren und beheben. Klicken Sie auf einen der Schweregrade, um die Seite [Alle Warnungen](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) gefiltert nach diesem Schweregrad zu öffnen.

Die Liste **VM-Verteilung nach Betriebssystem** zeigt die VMs nach Windows-Edition oder Linux-Distribution zusammen mit den jeweiligen Versionen an. In jeder Betriebssystemkategorie sind die VMs anhand der Integrität weiter aufgeschlüsselt.

![Distributionsperspektive von virtuellen Maschinen in VM Insights](./media/vminsights-health/vminsights-vmdistribution-by-os.png)

Wählen Sie eine Spalte aus (**VM-Anzahl**, **Kritisch**, **Warnung**, **Fehlerfrei** oder **Unbekannt**). Überprüfen Sie die Liste der gefilterten Ergebnisse auf der Seite **Virtuelle Computer**, die der ausgewählten Spalte entsprechen.

Wenn Sie beispielsweise alle VMs überprüfen möchten, auf denen Red Hat Enterprise Linux 7.5 ausgeführt wird, müssen Sie für **VM-Anzahl** den Wert für dieses Betriebssystem auswählen. Dann werden die VMs aufgelistet, die diesem Filter entsprechen, und deren Integritätszustand wird angezeigt.

![Beispielrollup von Red Hat Linux-VMs](./media/vminsights-health/vminsights-rollup-vm-rehl-01.png)

Aktivieren Sie das Kontrollkästchen **Integrität anzeigen**. Daraufhin wird der Integritätszustand für die gefilterten Ergebnisse in der Tabelle zurückgegeben.  

![Beispiel für den Integritätszustand virtueller Red Hat Linux-Computer](./media/vminsights-health/vminsights-rollup-vm-rehl-02.png)

Sie können für ein beliebiges Element in der Liste auf den entsprechenden Integritätszustand klicken, um die Integritätsdiagnose zu starten. Dort wird gezeigt, wie die Integrität für den ausgewählten virtuellen Computer ausgewertet wird. 

Wenn Sie auf der Seite **Virtuelle Computer** für die Spalte **VM-Name** den Namen einer VM auswählen, werden Sie zur Seite **VM-Instanz** weitergeleitet. Diese Seite enthält weiter Informationen zu den Warnungen und Problemen mit Integritätskriterien, die sich auf die ausgewählte VM auswirken. Sie können diese Details zum Integritätszustand filtern, indem Sie auf das Symbol **Integritätszustand** in der oberen linken Seitenecke klicken. Dann wird angezeigt, welche Komponenten Fehler aufweisen. Sie können auch die Warnungen zur VM-Integrität anzeigen, die von einer fehlerhaften Komponente ausgelöst wurden (nach Schweregrad der Warnung).

Klicken Sie in der Ansicht **VM-Liste** auf den Namen einer VM, um die Seite **Integrität** für diese VM zu öffnen. Gehen Sie dabei ähnlich wie beim direkten Navigieren zu **Einblicke (Vorschau)** über die VM vor.

![VM Insights einer ausgewählten Azure Virtual Machine](./media/vminsights-health/vminsights-directvm-health.png)

Auf der Seite **Virtuelle Computer (Vorschau)** in Azure Monitor werden der Rollupintegritätszustand für den virtuellen Computer und Warnungen angezeigt. Dieser Integritätszustand wird nach Schweregrad kategorisiert. Dieser steht für VM-Integritätswarnungen, die ausgelöst werden, wenn der Integritätszustand den Kriterien entsprechend von „Fehlerfrei“ zu „Fehlerhaft“ wechselt. Wenn Sie auf **VMs in kritischem Zustand** klicken, wird eine Seite mit einer Liste einer oder mehrerer VMs geöffnet, die einen kritischen Integritätszustand aufweisen.

Wenn Sie auf den Integritätszustand für eine der VMs in der Liste klicken, wird die Ansicht **Integritätsdiagnose** für diese VM angezeigt. Über diese Ansicht können Sie herausfinden, welche Integritätskriterien ein Problem mit dem Integritätszustand widerspiegeln. Wenn die Seite **Integritätsdiagnose** geöffnet wird, werden alle VM-Komponenten und die ihnen zugeordneten Integritätskriterien mit dem aktuellen Integritätszustand angezeigt.

Weitere Informationen finden Sie unter [Integritätsdiagnose](#health-diagnostics).

Wenn Sie **Alle Integritätskriterien anzeigen** auswählen, wird eine Seite geöffnet, die eine Liste aller für dieses Feature verfügbaren Integritätskriterien anzeigt. Die Informationen können auf der Grundlage der folgenden Optionen weiter gefiltert werden:

* **Art:** Es gibt drei Arten von Integritätskriterien für die Bewertung von Zuständen und für den Rollup des Gesamtintegritätszustands der überwachten VM:
    - **Einheit:** Diese Option überprüft einen Aspekt einer VM. Dieses Integritätskriterium kann beispielsweise einen Leistungsindikator überprüfen, um die Leistung der Komponente zu bestimmen, ein Skript auszuführen, um eine synthetische Transaktion durchzuführen oder nach einem Ereignis zu suchen, das auf einen Fehler hinweist. Standardmäßig ist der Filter auf „Einheit“ festgelegt.
    - **Abhängigkeit:** Stellt einen Integritätsrollup zwischen verschiedenen Entitäten bereit. Durch diese Integritätskriterien ist es möglich, dass die Integrität einer Entität von der Integrität einer anderen Entitätsart abhängt, auf die sie für erfolgreichen Betrieb angewiesen ist.
    - **Aggregat:** Diese Option stellt einen kombinierten Integritätszustand ähnlicher Integritätskriterien bereit. Die Integritätskriterien „Einheit“ und „Abhängigkeit“ sind normalerweise unter einem zusammengefassten Integritätskriterium konfiguriert. Nicht nur wird eine bessere allgemeine Organisation der vielen verschiedenen für eine Entität angezielten Integritätskriterien ermöglicht, ein zusammengefasstes Integritätskriterium stellt auch einen eindeutigen Integritätsstatus für verschiedene Kategorien der Entitäten zur Verfügung.

* **Kategorie:** Integritätskriterium, das zum Gruppieren ähnlicher Kriterien für die Berichterstellung verwendet wird. Die Kategorien lauten **Verfügbarkeit** und **Leistung**.

Sie können die fehlerhaften Instanzen anzeigen, indem Sie auf einen Wert in der Spalte **Fehlerhafte Komponente** klicken. Auf der Seite werden die Komponenten in einer Tabelle aufgelistet, die einen kritischen Integritätszustand aufweisen.

## <a name="health-diagnostics"></a>Integritätsdiagnose

Auf der Seite **Integritätsdiagnose** können Sie das Integritätsmodell einer VM visualisieren. Auf dieser Seite werden alle VM-Komponenten, die zugeordneten Integritätskriterien, Zustandsveränderungen und andere relevante Probleme, die anhand von überwachten Komponenten identifiziert wurden, die mit der VM in Zusammenhang stehen.

![Beispiel der Seite „Integritätsdiagnose“ für eine VM](./media/vminsights-health/health-diagnostics-page-01.png)

Sie können die Integritätsdiagnose folgendermaßen starten:

* Nach Rollupintegritätszustand für alle VMs aus der VM-Aggregatperspektive in Azure Monitor:

    1. Klicken Sie auf der Seite **Integrität** im Abschnitt **Integrität der Gast-VM** auf das Symbol für den Zustand **Kritisch**, **Warnung**, **Fehlerfrei** oder **Unbekannt**
    2. Navigieren Sie zu der Seite, auf der alle VMs aufgelistet werden, die der Kategorie entsprechen, nach der gefiltert wurde.
    3. Klicken Sie auf den Wert in der Spalte **Integritätszustand**, um die Integritätsdiagnose für diese VM zu öffnen.

* Nach dem Betriebssystem aus der VM-Aggregatperspektive in Azure Monitor: Unter **VM-Distribution** bewirkt das Auswählen eines der Spaltenwerte das Öffnen der Seite **Virtual Machines** und die Rückgabe einer Liste in der Tabelle, die der Filterkategorie entspricht. Klicken Sie auf den Wert in der Spalte **Integritätszustand**, um die Integritätsdiagnose für die ausgewählte VM zu öffnen.
 
* Indem Sie in der Gast-VM auf der Registerkarte **Integrität** in Azure Monitor für VMs auf **Integritätsdiagnose anzeigen** klicken

In Integritätsdiagnosen sind die Integritätsinformationen nach den Kategorien „Verfügbarkeit“ und „Leistung“ geordnet.
 
Alle Integritätskriterien, die für eine Komponente wie einen logischen Datenträger oder die CPU definiert wurden, können anzeigt werden, ohne nach diesen beiden Kategorien zu filtern. Diese Ansichten können in einer Gesamtansicht der Kriterien dargestellt werden. Sie können die Ergebnisse auch nach Kategorie filtern, indem Sie **Verfügbarkeit** oder **Leistung** auswählen.

Die Kategorie, für die das Kriterium gilt, befindet sich neben der Spalte **Integritätskriterien**. Wenn die Kriterien nicht mit der ausgewählten Kategorie übereinstimmen, wird in der Spalte **Integritätskriterien** die Meldung **Für die ausgewählte Kategorie ist kein Integritätskriterium verfügbar** angezeigt.

Der Zustand eines Integritätskriteriums wird durch vier Eigenschaften definiert: **Kritisch**, **Warnung**, **Fehlerfrei** und **Unbekannt**. Die ersten drei Zustände sind konfigurierbar. Dies bedeutet, dass Sie die Schwellenwerte der Monitore direkt über dem Konfigurationsbereich **Integritätskriterien** ändern können. Sie können die Konfiguration auch vornehmen, indem Sie den Schwellenwert mithilfe des [Vorgangs zur Monitoraktualisierung](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update) der Azure Monitor-REST-API ändern. **Unbekannt** kann nicht konfiguriert werden und ist für bestimmte Szenarios reserviert.

Die Seite **Integritätsdiagnose** ist in drei Hauptabschnitte unterteilt:

* Komponentenmodell
* Integritätskriterien
* Zustandsänderungen

![Abschnitte auf der Seite „Integritätsdiagnose“](./media/vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>Komponentenmodell

Die Spalte ganz links auf der Seite **Integritätsdiagnose** enthält das **Komponentenmodell**. In dieser Spalte werden alle der VM zugeordneten Komponenten zusammen mit dem jeweiligen aktuellen Integritätszustand angezeigt.

Im folgenden Beispiel sind die erkannten Komponenten **Datenträger**, **Logischer Datenträger**, **Prozessor**, **Arbeitsspeicher** und **Betriebssystem**. Mehrere Instanzen dieser Komponenten werden ermittelt und in dieser Spalte angezeigt.

In der folgenden Abbildung werden beispielsweise die zwei Instanzen der logischen Datenträger **C:** und **D:** der VM angezeigt, die beide den Zustand „Fehlerfrei“ aufweisen.

![Beispiel für die Darstellung eines Komponentenmodells in der Integritätsdiagnose](./media/vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>Integritätskriterien

Die mittlere Spalte auf der Seite „Integritätsdiagnose“ heißt **Integritätskriterien**. Das für die VM definierte Integritätsmodell wird als Hierarchiebaum angezeigt. Das Integritätsmodell für einen virtuellen Computer besteht aus Einheits- und Aggregatintegritätskriterien.

![Beispiel für die Darstellung von Integritätskriterien in der Integritätsdiagnose](./media/vminsights-health/health-diagnostics-page-healthcriteria.png)

Ein Integritätskriterium überprüft die Integrität der überwachten Instanz anhand bestimmter Kriterien, bei denen es sich z. B. um einen Schwellenwert oder den Zustand einer Entität handeln kann. Ein Integritätskriterium weist wie weiter oben beschrieben entweder zwei oder drei Schwellenwerte für den Integritätszustand auf. Das Integritätskriterium kann sich nur in einem seiner möglichen Zustände befinden.

Im Integritätsmodell werden Kriterien festgelegt, anhand derer die Integrität des gesamten Ziels und der Komponenten des Ziels bestimmt wird. Diese Hierarchie der Kriterien wird im Abschnitt **Integritätskriterien** der Seite **Integritätsdiagnose** veranschaulicht.

Die Richtlinie für den Integritätsrollup ist Bestandteil der Konfiguration der Aggregatintegritätskriterien (Standardwert: **Worst-of** (Schlechtester)). Sie finden die standardmäßig festgelegten Integritätskriterien, die im Rahmen dieses Features ausgeführt werden, im Abschnitt [Details der Überwachungskonfiguration](#monitoring-configuration-details) dieses Artikels.

Sie können auch die Funktion der Azure Monitor-REST-API zum [Auflisten der überwachten Instanzen nach Ressource](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitorinstances/listbyresource) verwenden, um eine Liste der Integritätskriterien zu erhalten. Zu diesen Kriterien zählen die Konfigurationsdetails, die auf die Ressource der Azure-VM angewendet werden.

Sie können die Konfiguration des Integritätskriteriums **Einheit** bearbeiten, indem Sie auf die Auslassungspunkte auf der rechten Seite klicken. Klicken Sie auf **Details anzeigen**, um den Konfigurationsbereich zu öffnen.

![Beispiel für das Konfigurieren eines Integritätskriteriums](./media/vminsights-health/health-diagnostics-vm-example-02.png)

Durch Verwendung des Beispiels **Mittlere Dauer pro Schreibvorgang** im Konfigurationsbereich für das ausgewählte Integritätskriterium kann der zugehörige Schwellenwert mit einem anderen numerischen Wert konfiguriert werden. Es handelt sich um eine Überwachung mit zwei Zuständen. Das bedeutet, dass nur eine Änderung von **Fehlerfrei** in **Warnung** möglich ist.

Andere Integritätskriterien können manchmal drei Zustände aufweisen. Bei diesen können Sie den Wert für den Warnungsschwellenwert und den Schwellenwert für den kritischen Zustand konfigurieren. Sie können einen Schwellenwert auch ändern, indem Sie die Funktion der Azure Monitor-REST-API zum [Festlegen der Überwachungskonfiguration](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update) verwenden.

>[!NOTE]
>Durch das Anwenden einer Konfigurationsänderung auf eine Instanz wird diese auf alle überwachten Instanzen angewendet. Wenn Sie beispielsweise **Disk -1 D:** (Datenträger –1 D:) auswählen und den Schwellenwert **Average Disk Seconds Per Write** (Durchschnittliche Dauer pro Schreibvorgang) ändern, wird die Änderung auf alle auf der VM erkannten und überwachten Instanzen angewendet.


![Beispiel für das Konfigurieren eines Integritätskriteriums für einen Einheitenmonitor](./media/vminsights-health/health-diagnostics-criteria-config-01.png)

Wenn Sie mehr zu Integritätskriterien erfahren möchten, können Sie anhand der aufgeführten Artikel Probleme, Ursachen und Lösungen ermitteln. Klicken Sie auf der Seite auf **Informationen anzeigen**, um den entsprechenden Artikel aufzurufen.

In der [Dokumentation zur Integrität mit Azure Monitor](https://docs.microsoft.com/azure/monitoring/infrastructure-health/) finden Sie alle relevanten Artikel und Artikel zur Integrität mit Azure Monitor für VMs.

### <a name="state-changes"></a>Zustandsänderungen

Ganz rechts auf der Seite **Integritätsdiagnose** befindet sich die Spalte **Zustandsänderungen**. Sie listet alle Zustandsänderungen auf, die den im Abschnitt **Integritätskriterien** ausgewählten Integritätskriterien zugeordnet sind, oder die Zustandsänderungen einer VM, wenn in der Spalte **Komponentenmodell** oder **Integritätskriterien** der Tabelle eine VM ausgewählt ist.

![Beispiel für die Darstellung von Zustandsänderungen in der Integritätsdiagnose](./media/vminsights-health/health-diagnostics-page-statechanges.png)

Im folgenden Abschnitt werden der Zustand eines Integritätskriteriums sowie die zugewiesene Zeit dargestellt. Hierbei wird der aktuelle Zustand im oberen Bereich der Spalte angezeigt.

### <a name="association-of-component-model-health-criteria-and-state-changes-columns"></a>Zuordnung der Spalten „Komponentenmodell“, „Integritätskriterien“ und „Zustandsänderung“

Die drei Spalten sind miteinander verknüpft. Wenn Sie eine Instanz in der Spalte **Komponentenmodell** auswählen, wird die Spalte **Integritätskriterien** nach dieser Komponentenansicht gefiltert. Die Spalte **Zustandsänderungen** wird anhand der ausgewählten Integritätskriterien entsprechend aktualisiert.

![Beispiel zum Auswählen einer überwachten Instanz mit Ergebnissen](./media/vminsights-health/health-diagnostics-vm-example-01.png)

Wenn Sie beispielsweise *Disk - 1 D:* (Datenträger –1 D:) in der Liste unter **Komponentenmodell** auswählen, wird **Integritätskriterien** nach *Disk - 1D:* (Datenträger –1 D:) gefiltert. Unter **Zustandsänderungen** wird dann der Zustand je nach Verfügbarkeit von *Disk - 1D:* (Datenträger –1 D:) angezeigt.

Sie können den aktualisierten Integritätszustand anzeigen, wenn Sie die Seite „Integritätsdiagnose“ aktualisieren, indem Sie auf **Aktualisieren** klicken. Wenn basierend auf dem vordefinierten Abrufintervall eine Aktualisierung des Integritätsstatus des Integritätskriteriums erfolgt, können Sie mit dieser Aufgabe Wartezeiten vermeiden und den aktuellen Integritätsstatus anzeigen. **Zustand der Integritätskriterien** ist ein Filter, mit dem Sie die Ergebnisse je nach ausgewähltem Integritätszustand eingrenzen können: „Fehlerfrei“, „Warnung“, „Kritisch“, „Unbekannt“ und „Alle“. Die Zeitangabe **Letzte Aktualisierung** oben rechts stellt den Zeitpunkt der letzten Aktualisierung der Seite „Integritätsdiagnose“ dar.

## <a name="alerts"></a>Alerts

Die Integrität von Azure Monitor für VMs ist mit den [Azure-Warnungen](../../azure-monitor/platform/alerts-overview.md) integriert. Es wird eine Warnung ausgelöst, wenn für die vordefinierten Kriterien erkannt wird, dass der Zustand sich von „Fehlerfrei“ in „Fehlerhaft“ ändert. Die Warnungen werden nach Schweregrad definiert. Dabei ist „Sev0“ der höchste Schweregrad und „Sev4“ der niedrigste.

Warnungen sind keiner Aktionsgruppe zugeordnet, damit Sie benachrichtigt werden, wenn die Warnung ausgelöst wurde. Ein Benutzer mit der Rolle „Besitzer“ im Abonnementbereich muss Benachrichtigungen konfigurieren, indem er die Schritte im Abschnitt [Konfigurieren von Warnungen](#configure-alerts) ausführt.

Die gesamten VM-Integritätswarnungen stehen nach Schweregrad kategorisiert auf dem Dashboard **Integrität** im Bereich **Warnungen** zur Verfügung. Wenn Sie entweder die Gesamtzahl der Warnungen oder die einem Schweregrad entsprechende Anzahl auswählen, wird die Seite **Warnungen** geöffnet und listet alle Warnungen auf, die Ihrer Auswahl entsprechen.

Wenn Sie beispielsweise die Zeile auswählen, die **Schweregrad 1** entspricht, wird eine Ansicht angezeigt, die folgender ähnelt:

![Beispiel für alle Warnungen mit Schweregrad 1](./media/vminsights-health/vminsights-sev1-alerts-01.png)

Die Seite **Alle Warnungen** ist nicht eingegrenzt, sodass nicht nur die Warnungen angezeigt werden, die Ihrer Auswahl entsprechen. Sie ist auch nach **Ressourcentyp** gefiltert, damit nur Integritätswarnungen angezeigt werden, die durch eine VM-Ressource ausgelöst wurden. Dieses Format spiegelt sich in der Liste der Warnungen in der Spalte **Zielressource** wider. Dort wird die Azure-VM angezeigt, für die eine Warnung ausgegeben wurde, weil eine Bedingung erfüllt wurde, die als fehlerhaft gilt.

Warnungen, die von anderen Ressourcentypen oder Diensten ausgelöst wurden, sollen in dieser Ansicht nicht angezeigt werden. Zu diesen Warnungen zählen Protokollwarnungen. Diese basieren auf Protokollabfragen oder Metrikwarnungen, die normalerweise über die Azure Monitor-Standardseite [Alle Warnungen](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) angezeigt werden.

Zum Filtern dieser Ansicht können Sie Werte in den Dropdownmenüs am oberen Rand der Seite auswählen.

|Column |BESCHREIBUNG |
|-------|------------|
|Subscription |Wählen Sie ein Azure-Abonnement aus. Nur Warnungen im ausgewählten Abonnement sind in der Ansicht enthalten. |
|Ressourcengruppe |Wählen Sie eine einzelne Ressourcengruppe aus. Nur Warnungen mit Zielen in der ausgewählten Ressourcengruppe sind in der Ansicht enthalten. |
|Ressourcentyp |Wählen Sie mindestens einen Ressourcentyp aus. Standardmäßig sind in dieser Ansicht nur Warnungen mit dem Ziel **Virtuelle Computer** ausgewählt und enthalten. Diese Spalte ist nur verfügbar, nachdem eine Ressourcengruppe angegeben wurde. |
|Resource |Wählen Sie eine Ressource aus. Nur Warnungen mit dieser Ressource als Ziel sind in der Ansicht enthalten. Diese Spalte ist nur verfügbar, wenn ein Ressourcentyp angegeben wurde. |
|severity |Wählen Sie einen Warnungsschweregrad oder **Alle** aus, um Warnungen aller Schweregrade einzuschließen. |
|Überwachungsbedingung |Wählen Sie eine Überwachungsbedingung aus, um Warnungen danach zu filtern, ob sie vom System ausgelöst oder behoben wurden, falls die Bedingung nicht mehr aktiv ist. Wählen Sie alternativ **Alle** aus, um Warnungen in allen Zuständen einzuschließen. |
|Warnungsstatus |Wählen Sie einen Warnungszustand aus (**Neu**, **Bestätigt**, **Geschlossen**), oder wählen Sie **Alle** aus, um Warnungen mit jedem Zustand einzuschließen. |
|Überwachungsdienst |Wählen Sie einen Dienst oder **Alle** aus, um alle Dienste einzuschließen. Für dieses Feature werden nur Warnungen von „VM Insights“ unterstützt.|
|Zeitbereich| Nur Warnungen, die innerhalb des ausgewählten Zeitfensters ausgelöst wurden, sind in der Ansicht enthalten. Unterstützte Werte sind die letzte Stunde, die letzten 24 Stunden, die letzten 7 Tage und die letzten 30 Tage. |

Die Seite **Warnungsdetails** wird angezeigt, wenn Sie auf eine Warnung klicken. Sie enthält Details zur Warnung und ermöglicht Ihnen das Ändern des Zustands.

Weitere Informationen zum Verwalten von Warnungen finden Sie unter [Erstellen, Anzeigen und Verwalten von Warnungen mithilfe von Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

>[!NOTE]
>Derzeit ist es nicht möglich, über das Portal neue Warnungen anhand von Integritätskriterien zu erstellen oder vorhandene Integritätswarnungsregeln in Azure Monitor zu ändern.


![Bereich „Warnungsdetails“ für eine ausgewählte Warnung](./media/vminsights-health/alert-details-pane-01.png)

Sie können den Warnungszustand außerdem für eine oder mehrere Warnungen ändern, indem Sie in der oberen linken Ecke der Seite **Alle Warnungen** den Befehl **Status ändern** auswählen. Wählen Sie im Bereich **Warnungsstatus ändern** einen Status aus, fügen Sie eine Beschreibung der Änderung im Feld **Kommentar** hinzu, und klicken Sie dann auf **OK**, um Ihre Änderungen zu übernehmen. Während die Informationen überprüft und die Änderungen angewendet werden, können Sie den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen.

### <a name="configure-alerts"></a>Konfigurieren von Warnungen
Manche Aspekte der Warnungsverwaltung können nicht über das Azure-Portal verwaltet werden. Diese müssen mithilfe der [Azure Monitor-REST-API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components) vorgenommen werden. Dies gilt insbesondere in folgenden Fällen:

- Aktivieren oder Deaktivieren einer Warnung für Integritätskriterien
- Einrichten von Benachrichtigungen für Warnungen zu Integritätskriterien

In allen Beispielen wird [ARMClient](https://github.com/projectkudu/armclient) auf Ihrem Windows-Computer verwendet. Wenn Sie nicht mit dieser Vorgehensweise vertraut sind, siehe [Verwenden von ARMClient](../platform/rest-api-walkthrough.md#use-armclient).

#### <a name="enable-or-disable-an-alert-rule"></a>Aktivieren oder Deaktivieren einer Warnungsregel

Sie können eine Warnung für ein bestimmtes Integritätskriterium aktivieren oder deaktivieren, indem Sie die Eigenschaft **alertGeneration** der Integritätskriterien in den Wert **Disabled** oder **Enabled** ändern.

Sie können den Wert *monitorId* eines bestimmten Integritätskriteriums zu ermitteln, indem Sie wie im folgenden Beispiel veranschaulicht den Wert für das Kriterium **LogicalDisk_AvgDiskSecPerTransfer** abfragen:

1. Geben Sie in einem Terminalfenster **armclient.exe login**ein. Dabei werden Sie dazu aufgefordert, sich bei Azure anzumelden.

2. Geben Sie den folgenden Befehl ein, um alle auf einer bestimmten VM aktiven Integritätskriterien abzurufen und den Wert für die Eigenschaft *monitorId* zu bestimmen:

    ```
    armclient GET "subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/monitors?api-version=2018-08-31-preview"
    ```

    Das folgende Beispiel zeigt die Ausgabe des Befehls *armclient GET* an. Notieren Sie sich den Wert von *MonitorId*. Dieser Wert ist im nächsten Schritt erforderlich, bei dem Sie die ID der Integritätskriterien angeben und ihre Eigenschaft ändern müssen, um eine Warnung zu erstellen.

    ```
    "id": "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourcegroups/Lab/providers/Microsoft.Compute/virtualMachines/SVR01/providers/Microsoft.WorkloadMonitor/monitors/ComponentTypeId='LogicalDisk',MonitorId='Microsoft_LogicalDisk_AvgDiskSecPerRead'",
      "name": "ComponentTypeId='LogicalDisk',MonitorId='Microsoft_LogicalDisk_AvgDiskSecPerRead'",
      "type": "Microsoft.WorkloadMonitor/virtualMachines/monitors"
    },
    {
      "properties": {
        "description": "Monitor the performance counter LogicalDisk\\Avg Disk Sec Per Transfer",
        "monitorId": "Microsoft_LogicalDisk_AvgDiskSecPerTransfer",
        "monitorName": "Microsoft.LogicalDisk.AvgDiskSecPerTransfer",
        "monitorDisplayName": "Average Logical Disk Seconds Per Transfer",
        "parentMonitorName": null,
        "parentMonitorDisplayName": null,
        "monitorType": "Unit",
        "monitorCategory": "PerformanceHealth",
        "componentTypeId": "LogicalDisk",
        "componentTypeName": "LogicalDisk",
        "componentTypeDisplayName": "Logical Disk",
        "monitorState": "Enabled",
        "criteria": [
          {
            "healthState": "Warning",
            "comparisonOperator": "GreaterThan",
            "threshold": 0.1
          }
        ],
        "alertGeneration": "Enabled",
        "frequency": 1,
        "lookbackDuration": 17,
        "documentationURL": "https://aka.ms/Ahcs1r",
        "configurable": true,
        "signalType": "Metrics",
        "signalName": "VMHealth_Avg. Logical Disk sec/Transfer"
      },
      "etag": null,
    ```

3. Geben Sie den folgenden Befehl ein, um die Eigenschaft *alertGeneration* zu ändern:

    ```
    armclient patch subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/monitors/Microsoft_LogicalDisk_AvgDiskSecPerTransfer?api-version=2018-08-31-preview "{'properties':{'alertGeneration':'Disabled'}}"
    ```   

4. Geben Sie den in Schritt 2 verwendeten GET-Befehl ein, um zu überprüfen, ob der Wert der Eigenschaft auf **Disabled** festgelegt ist.

#### <a name="associate-an-action-group-with-health-criteria"></a>Zuordnen einer Aktionsgruppe zu Integritätskriterien

Die Funktion „Integrität“ von Azure Monitor für VMs unterstützt SMS-und E-Mail-Benachrichtigungen, wenn Warnungen generiert werden, weil Integritätskriterien den Zustand „Fehlerhaft“ aufweisen. Sie können Benachrichtigungen konfigurieren, indem Sie den Namen der Aktionsgruppe notieren, die für das Senden von SMS- oder E-Mail-Benachrichtigungen konfiguriert ist.

>[!NOTE]
>Diese Aktion muss für jede überwachte VM erfolgen, für die Sie eine Benachrichtigung erhalten möchten. Sie wird nicht auf alle VMs in einer Ressourcengruppe angewendet.

1. Geben Sie in einem Terminalfenster *armclient.exe login* ein. Dabei werden Sie dazu aufgefordert, sich bei Azure anzumelden.

2. Geben Sie den folgenden Befehl ein, um einer Aktionsgruppe Warnungsregeln zuzuordnen:
 
    ```
    $payload = "{'properties':{'ActionGroupResourceIds':['/subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/microsoft.insights/actionGroups/actiongroupName']}}"
    armclient PUT https://management.azure.com/subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/notificationSettings/default?api-version=2018-08-31-preview $payload
    ```

3. Sie können überprüfen, ob der Wert der Eigenschaft **actionGroupResourceIds** erfolgreich aktualisiert wurde, indem Sie den folgenden Befehl eingeben:

    ```
    armclient GET "subscriptions/subscriptionName/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/notificationSettings?api-version=2018-08-31-preview"
    ```

    Die Ausgabe sollte wie im folgenden Kriterium aussehen:
    
    ```
    {
      "value": [
        {
          "properties": {
            "actionGroupResourceIds": [
              "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourceGroups/Lab/providers/microsoft.insights/actionGroups/Lab-IT%20Ops%20Notify"
            ]
          },
          "etag": null,
          "id": "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourcegroups/Lab/providers/Microsoft.Compute/virtualMachines/SVR01/providers/Microsoft.WorkloadMonitor/notificationSettings/default",
          "name": "notificationSettings/default",
          "type": "Microsoft.WorkloadMonitor/virtualMachines/notificationSettings"
        }
      ],
      "nextLink": null
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Ermitteln von Beschränkungen und der Gesamtleistung einer VM finden Sie unter [Direktes Anzeigen der Leistung in einer Azure-VM](vminsights-performance.md).

- Weitere Informationen zu ermittelten Anwendungsabhängigkeiten finden Sie unter [Anzeigen der Zuordnung in Azure Monitor](vminsights-maps.md).
