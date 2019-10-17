---
title: Aktionsregeln für Azure Monitor-Warnungen
description: In diesem Artikel wird erläutert, was Aktionsregeln in Azure Monitor sind und wie sie konfiguriert und verwaltet werden können.
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: robb
ms.subservice: alerts
ms.openlocfilehash: 19f17aff4f915f8a16ccf9d69b12a845d9493e96
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72299289"
---
# <a name="action-rules-preview"></a>Aktionsregeln (Vorschau)

Mit Aktionsregeln können Sie Aktionen in jedem Resource Manager-Bereich (Abonnement, Ressourcengruppe und Ressource) definieren oder unterdrücken. Sie verfügen über verschiedene Filter zum Eingrenzen auf eine bestimmte Teilmenge von Warnungsinstanzen, auf die Sie reagieren möchten.

## <a name="why-and-when-should-you-use-action-rules"></a>Wann und wieso sollten Sie Aktionsregeln verwenden?

### <a name="suppression-of-alerts"></a>Unterdrückung von Warnungen

Es gibt viele Szenarien, in denen es nützlich ist, die Benachrichtigungen zu unterdrücken, die von Warnungen generiert werden. Diese Szenarien reichen von der Unterdrückung während eines geplanten Wartungsfensters bis hin zur Unterdrückungen außerhalb der Geschäftszeiten. Angenommen, das Team von **ContosoVM** möchte Warnungsbenachrichtigungen für das kommende Wochenende unterdrücken, weil für **ContosoVM** eine Wartung geplant ist. 

Obwohl das Team jede Warnungsregel, die für **ContosoVM** manuell konfiguriert ist, deaktivieren kann (und sie nach der Wartung erneut aktivieren kann), ist dies kein einfacher Vorgang. Mit Aktionsregeln können Sie die Unterdrückung von Warnungen nach Maß definieren, sodass Sie den Zeitraum für die Unterdrückung flexibel konfigurieren können. Im vorherigen Beispiel kann das Team nun eine Aktionsregel für **ContosoVM** definieren, die alle Warnungsbenachrichtigungen für das Wochenende unterdrückt.


### <a name="actions-at-scale"></a>Aktionen nach Maß

Zwar können Sie mit Warnungsregeln die Aktionsgruppe definieren, die ausgelöst wird, wenn eine Warnung generiert wird, jedoch verfügen Kunden meist über eine allgemeine Aktionsgruppe für den gesamten Umfang ihrer Vorgänge. Beispielsweise würde ein Team, das für die Ressourcengruppe **ContosoRG** zuständig ist, vermutlich dieselbe Aktionsgruppe für alle innerhalb von **ContosoRG** definierten Warnungsregeln definieren. 

Aktionsregeln helfen Ihnen, diesen Vorgang zu vereinfachen. Durch die Definition von Aktionen im richtigen Maßstab kann eine Aktionsgruppe für jede Warnung ausgelöst werden, die für den konfigurierten Bereich generiert wird. Im vorherigen Beispiel kann das Team eine Aktionsregel für **ContosoRG** definieren, die die gleiche Aktionsgruppe für alle darin generierten Warnungen auslöst.

> [!NOTE]
> Aktionsregeln gelten zurzeit nicht für Azure Service Health-Warnungen.

## <a name="configuring-an-action-rule"></a>Konfigurieren einer Aktionsregel

Sie können die Funktion aufrufen, indem Sie in Azure Monitor auf der Startseite **Warnungen** die Option **Aktionen verwalten** auswählen. Wählen Sie anschließend **Aktionsregeln (Vorschau)** aus. Sie können durch Auswählen von **Aktionsregeln (Vorschau)** im Dashboard auf der Startseite für Warnungen auf die Regeln zugreifen.

![Aktionsregeln auf der Azure Monitor-Startseite](media/alerts-action-rules/action-rules-landing-page.png)

Wählen Sie **+ New Action Rule** (+ Neue Aktionsregel) aus. 

![Hinzufügen einer neuen Aktionsregel](media/alerts-action-rules/action-rules-new-rule.png)

Sie können eine Aktionsregel auch erstellen, während Sie eine Warnungsregel konfigurieren.

![Hinzufügen einer neuen Aktionsregel](media/alerts-action-rules/action-rules-alert-rule.png)

Nun sollte die Flussseite zum Erstellen von Aktionsregeln angezeigt werden. Konfigurieren Sie die folgenden Elemente: 

![Erstellungsflow für neue Aktionsregel](media/alerts-action-rules/action-rules-new-rule-creation-flow.png)

### <a name="scope"></a>`Scope`

Wählen Sie zunächst den Bereich aus (Azure-Abonnement, Ressourcengruppe oder Zielressource). Sie können auch eine Mehrfachauswahl durch Kombinieren von Bereichen innerhalb eines einzelnen Abonnements treffen.

![Bereich der Aktionsregel](media/alerts-action-rules/action-rules-new-rule-creation-flow-scope.png)

### <a name="filter-criteria"></a>Filterkriterien

Sie können außerdem mindestens einen Filter zum Eingrenzen auf eine bestimmte Teilmenge von Warnungen festlegen. 

Die folgenden Filter sind verfügbar: 

* **Schweregrad**: Wählen Sie mindestens einen Schweregrad für Warnungen aus. **Schweregrad = Sev1** bedeutet, dass die Aktionsregel für alle Warnungen mit dem Schweregrad Sev1 gilt.
* **Überwachungsdienst**: Ein Filter, der auf dem ursprünglichen Überwachungsdienst basiert. Dieser Filter bietet ebenfalls eine Mehrfachauswahl. **Überwachungsdienst = „Application Insights“** bedeutet beispielsweise, dass die Aktionsregel für alle auf „Application Insights“ basierenden Warnungen gilt.
* **Ressourcentyp**:  Ein Filter, der auf einem bestimmten Ressourcentyp basiert. Dieser Filter bietet ebenfalls eine Mehrfachauswahl. **Ressourcentyp = „Virtual Machines“** bedeutet, dass die Aktionsregel für alle virtuellen Computer gilt.
* **Warnungsregel-ID**: Eine Option zum Filtern nach bestimmten Warnungsregeln anhand der Resource Manager-ID der Warnungsregel.
* **Überwachungsbedingung**:  Ein Filter für Warnungsinstanzen mit der Überwachungsbedingung **Ausgelöst** oder **Aufgelöst**.
* **Beschreibung:** Eine Regex-Entsprechung (regulärer Ausdruck), die eine Zeichenfolgenübereinstimmung mit der Beschreibung definiert, die als Teil der Warnungsregel definiert ist. **Description contains 'prod'** findet beispielsweise alle Warnungen, die die Zeichenfolge „prod“ in ihren Beschreibungen enthalten.
* **Warnungskontext (Nutzlast)** : Eine Regex-Übereinstimmung, die eine Zeichenfolgenübereinstimmung mit den Warnungskontextfeldern der Nutzlast einer Warnung definiert. **Alert context (payload) contains 'Computer-01'** findet beispielsweise alle Warnungen, deren Nutzlasten die Zeichenfolge „Computer-01“ enthalten.

Diese Filter werden in Verbindung miteinander angewendet. Wenn Sie beispielsweise **Ressourcentyp' = „Virtual Machines** und **Schweregrad' = Sev0** festlegen, wird nach allen **Sev0**-Warnungen ausschließlich auf Ihren VMs gefiltert. 

![Filter für Aktionsregeln](media/alerts-action-rules/action-rules-new-rule-creation-flow-filters.png)

### <a name="suppression-or-action-group-configuration"></a>Konfiguration von Unterdrückung oder Aktionsgruppen

Konfigurieren Sie nun die Aktionsregel für die Unterdrückung von Warnungen oder die Unterstützung für Aktionsgruppen. Es können nicht beide Optionen ausgewählt werden. Die Konfiguration gilt für alle Warnungsinstanzen, die dem zuvor definierten Bereich und den Filtern entsprechen.

#### <a name="suppression"></a>Unterdrückung

Wenn Sie **Suppression** (Unterdrückung) auswählen, konfigurieren Sie den Zeitraum, für den Aktionen und Benachrichtigungen unterdrückt werden sollen. Wählen Sie eine der folgenden Optionen:
* **From now (Always)** (Ab jetzt (Immer)): Alle Benachrichtigungen werden unbefristet unterdrückt.
* **At a scheduled time** (Zur geplanten Zeit): Benachrichtigungen werden in einem bestimmten Zeitraum unterdrückt.
* **With a recurrence** (Mit Wiederholung): Unterdrückt Benachrichtigungen bei einem wiederkehrenden täglichen, wöchentlichen oder monatlichen Zeitplan.

![Unterdrückung von Aktionsregeln](media/alerts-action-rules/action-rules-new-rule-creation-flow-suppression.png)

#### <a name="action-group"></a>Aktionsgruppe

Fügen Sie bei Auswahl von **Aktionsgruppe** auf der Umschaltfläche eine vorhandene Aktionsgruppe hinzu, oder erstellen Sie eine neue. 

> [!NOTE]
> Sie können einer Aktionsregel nur eine Aktionsgruppe zuordnen.

![Hinzufügen oder Erstellen einer neuen Aktionsregel durch Auswählen der Aktionsgruppe](media/alerts-action-rules/action-rules-new-rule-creation-flow-action-group.png)

### <a name="action-rule-details"></a>Details der Aktionsregel

Konfigurieren Sie schließlich folgende Details für die Aktionsregel:
* NAME
* Ressourcengruppe, in der sie gespeichert wird
* BESCHREIBUNG 

## <a name="example-scenarios"></a>Beispielszenarien

### <a name="scenario-1-suppression-of-alerts-based-on-severity"></a>Szenario 1: Unterdrückung von Warnungen nach Schweregrad

Contoso möchte Benachrichtigungen für alle Warnungen des Schweregrads Sev4 auf allen VMs im Abonnement **ContosoSub** an Wochenenden unterdrücken.

**Lösung:** Erstellen Sie eine Aktionsregel mit:
* Bereich = **ContosoSub**
* Filter
    * Schweregrad = **Sev4**
    * Ressourcentyp = **Virtual Machines**
* Unterdrückung, deren Wiederholung auf wöchentlich festgelegt ist, und **Samstag** und **Sonntag** sind ausgewählt

### <a name="scenario-2-suppression-of-alerts-based-on-alert-context-payload"></a>Szenario 2: Unterdrückung von Warnungen nach Warnungskontext (Nutzlast)

Contoso möchte Benachrichtigungen für alle Protokollwarnungen, die für **Computer-01** in **ContosoSub** generiert werden, unbefristet unterdrücken, während Wartungsarbeiten ausgeführt werden.

**Lösung:** Erstellen Sie eine Aktionsregel mit:
* Bereich = **ContosoSub**
* Filter
    * Überwachungsdienst = **Log Analytics**
    * Warnungskontext (Nutzlast) enthält **Computer-01**
* Unterdrückung ist auf **From now (Always)** (Ab jetzt (Immer)) festgelegt

### <a name="scenario-3-action-group-defined-at-a-resource-group"></a>Szenario 3: In einer Ressourcengruppe definierte Aktionsgruppe

Contoso hat [eine Metrikwarnung auf Abonnementebene](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-overview#monitoring-at-scale-using-metric-alerts-in-azure-monitor) definiert. Contoso möchte aber die Aktionen definieren, die bei Warnungen speziell ausgelöst werden, die aus der Ressourcengruppe **ContosoRG** generiert wurden.

**Lösung:** Erstellen Sie eine Aktionsregel mit:
* Bereich = **ContosoRG**
* Keine Filter
* Aktionsgruppe auf **ContosoActionGroup** festgelegt

> [!NOTE]
> *In Aktionsregeln definierte Aktionsgruppen und Warnungsregeln arbeiten unabhängig voneinander, und es erfolgt keine Deduplizierung*. Wenn im zuvor beschriebenen Szenario eine Aktionsgruppe für die Warnungsregel definiert wurde, wird sie in Verbindung mit der in der Aktionsregel definierten Aktionsgruppe ausgelöst. 

## <a name="managing-your-action-rules"></a>Verwalten Ihrer Aktionsregeln

Sie können Ihre Aktionsregeln in der Listenansicht anzeigen und verwalten:

![Listenansicht für Aktionsregeln](media/alerts-action-rules/action-rules-list-view.png)

Hier können Sie Aktionsregeln je nach Bedarf aktivieren, deaktivieren und löschen, indem Sie das jeweilige Kontrollkästchen daneben aktivieren. Wenn Sie eine Aktionsregel auswählen, wird die zugehörige Konfigurationsseite geöffnet. Auf der Seite können Sie die Definition der Aktionsregel aktualisieren und die Regel aktivieren oder deaktivieren.

## <a name="best-practices"></a>Bewährte Methoden

Protokollwarnungen, die mit der Option [Anzahl von Ergebnissen](alerts-unified-log.md) erstellt werden, generieren eine einzige Warnungsinstanz anhand sämtlicher Suchergebnisse (die sich über mehrere Computer erstrecken können). Wenn in diesem Szenario eine Aktionsregel den Filter **Warnungskontext (Nutzlast)** verwendet, reagiert sie auf die Warnungsinstanz, sofern eine Übereinstimmung vorhanden ist. Wenn in Szenario 2, wie zuvor beschrieben, die Suchergebnisse für die generierte Protokollwarnung sowohl **Computer-01** als auch **Computer-02** enthalten, wird die gesamte Benachrichtigung unterdrückt. Es wird überhaupt keine Benachrichtigung für **Computer-02** generiert.

![Aktionsregeln und Protokollwarnungen (Anzahl von Ergebnissen)](media/alerts-action-rules/action-rules-log-alert-number-of-results.png)

Um Protokollwarnungen mit Aktionsregeln optimal zu nutzen, empfiehlt es sich, Protokollwarnungen mit der Option [Metrische Maßeinheit](alerts-unified-log.md) zu erstellen. Separate Benachrichtigungsinstanzen werden von dieser Option basierend auf dem definierten Gruppenfeld generiert. In Szenario 2 werden schließlich separate Warnungsinstanzen für **Computer-01** und **Computer-02** generiert. Aufgrund der Aktionsregel, die im Szenario beschrieben wird, wird nur die Benachrichtigung für **Computer-01** unterdrückt. Die Benachrichtigung für **Computer-02** wird weiterhin normal ausgelöst.

![Aktionsregeln und Protokollwarnungen (Anzahl von Ergebnissen)](media/alerts-action-rules/action-rules-log-alert-metric-measurement.png)

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="while-im-configuring-an-action-rule-id-like-to-see-all-the-possible-overlapping-action-rules-so-that-i-avoid-duplicate-notifications-is-it-possible-to-do-that"></a>Beim Konfigurieren einer Aktionsregel möchte ich alle möglichen einander überlappenden Aktionsregeln sehen, sodass ich doppelte Benachrichtigungen vermeiden kann. Ist dies möglich?

Nachdem Sie einen Bereich definiert haben, können Sie bei der Konfiguration einer Aktionsregel eine Liste von Aktionsregeln anzeigen, die sich für den gleichen Bereich überschneiden (falls vorhanden). Diese Überlappung kann wie folgt aussehen:

* Genaue Übereinstimmung: Beispielsweise können sich die von Ihnen definierte Aktionsregel und die überlappende Aktionsregel auf dasselbe Abonnement beziehen.
* Teilmenge: Die von Ihnen definierte Aktionsregel bezieht sich z.B. auf ein Abonnement, und die überlappende Aktionsregel bezieht sich auf eine Ressourcengruppe in diesem Abonnement.
* Obermenge: Die von Ihnen definierte Aktionsregel bezieht sich auf eine Ressourcengruppe, und die überlappende Aktionsregel bezieht sich auf das Abonnement, das diese Ressourcengruppe enthält.
* Schnittmenge: Die von Ihnen definierte Aktionsregel bezieht sich z.B. auf **VM1** und **VM2**, und die überlappende Aktionsregel bezieht sich auf **VM2** und **VM3**.

![Überlappende Aktionsregeln](media/alerts-action-rules/action-rules-overlapping.png)

### <a name="while-im-configuring-an-alert-rule-is-it-possible-to-know-if-there-are-already-action-rules-defined-that-might-act-on-the-alert-rule-im-defining"></a>Kann ich beim Konfigurieren einer Warnungsregel feststellen, ob bereits Aktionsregeln definiert sind, die möglicherweise Aktionen für die Warnungsregel ausführen, die ich gerade definiere?

Nachdem Sie die Zielressource für Ihre Warnungsregel definiert haben, können Sie die Liste der Aktionsregeln anzeigen, die für den gleichen Bereich gelten (falls vorhanden), indem Sie **Konfigurierte Aktionen anzeigen** im Abschnitt **Aktionen** auswählen. Diese Liste wird gemäß den folgenden Szenarien für den Bereich mit Daten aufgefüllt:

* Genaue Übereinstimmung: Beispielsweise können sich die von Ihnen definierte Warnungsregel und die Aktionsregel auf dasselbe Abonnement beziehen.
* Teilmenge: Die von Ihnen definierte Warnungsregel bezieht sich auf ein Abonnement, und die Aktionsregel bezieht sich auf eine Ressourcengruppe in diesem Abonnement.
* Obermenge: Die von Ihnen definierte Warnungsregel bezieht sich auf eine Ressourcengruppe, und die Aktionsregel bezieht sich auf das Abonnement, das die Ressourcengruppe enthält.
* Schnittmenge: Die von Ihnen definierte Warnungsregel bezieht sich beispielsweise auf **VM1** und **VM2**, und die Aktionsregel bezieht sich auf **VM2** und **VM3**.
    
![Überlappende Aktionsregeln](media/alerts-action-rules/action-rules-alert-rule-overlapping.png)

### <a name="can-i-see-the-alerts-that-have-been-suppressed-by-an-action-rule"></a>Kann ich die durch eine Aktionsregel unterdrückten Warnungen anzeigen?

Auf der [Seite mit der Liste der Warnungen](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-managing-alert-instances) kann eine weitere Spalte mit dem Namen **Suppression Status** (Unterdrückungsstatus) ausgewählt werden. Wenn die Benachrichtigung für eine Warnungsinstanz unterdrückt wurde, wird dieser Status in der Liste angezeigt.

![Unterdrückte Warnungsinstanzen](media/alerts-action-rules/action-rules-suppressed-alerts.png)

### <a name="if-theres-an-action-rule-with-an-action-group-and-another-with-suppression-active-on-the-same-scope-what-happens"></a>Was geschieht, wenn eine Aktionsregel mit einer Aktionsgruppe und eine weitere mit aktiver Unterdrückung im selben Bereich vorhanden sind?

Unterdrückung hat in einem Bereich immer Vorrang.

### <a name="what-happens-if-i-have-a-resource-thats-monitored-in-two-separate-action-rules-do-i-get-one-or-two-notifications-for-example-vm2-in-the-following-scenario"></a>Was geschieht, wenn ich eine Ressource verwende, die von zwei separaten Aktionsregeln überwacht wird? Erhalte ich dann eine oder zwei Benachrichtigungen? Beispielsweise **VM2** im folgenden Szenario:

      action rule AR1 defined for VM1 and VM2 with action group AG1
      action rule AR2 defined for VM2 and VM3 with action group AG1

Für jede Warnung zu „VM1“ und „VM3“ wird die Aktionsgruppe „AG1“ ein Mal ausgelöst. Für jede Warnung zu **VM2** wird die Aktionsgruppe „AG1“ zwei Mal ausgelöst (Aktionen werden von Aktionsregeln nicht dedupliziert). 

### <a name="what-happens-if-i-have-a-resource-monitored-in-two-separate-action-rules-and-one-calls-for-action-while-another-for-suppression-for-example-vm2-in-the-following-scenario"></a>Was geschieht, wenn ich eine Ressource in zwei separaten Aktionsregeln überwachen lasse, von denen eine eine Aktion ausführt und die andere unterdrückt wird? Beispielsweise **VM2** im folgenden Szenario:

      action rule AR1 defined for VM1 and VM2 with action group AG1 
      action rule AR2 defined for VM2 and VM3 with suppression

Für jede Warnung zu „VM1“ wird die Aktionsgruppe „AG1“ ein Mal ausgelöst. Aktionen und Benachrichtigungen für jede Warnung zu „VM2“ und „VM3“ werden unterdrückt. 

### <a name="what-happens-if-i-have-an-alert-rule-and-an-action-rule-defined-for-the-same-resource-calling-different-action-groups-for-example-vm1-in-the-following-scenario"></a>Was geschieht, wenn ich eine Warnungsregel und eine Aktionsregel für dieselbe Ressource definiert habe, die unterschiedliche Aktionsgruppen aufrufen? Beispielsweise **VM1** im folgenden Szenario:

      alert rule rule1 on VM1 with action group AG2
      action rule AR1 defined for VM1 with action group AG1 
 
Für jede Warnung zu „VM1“ wird die Aktionsgruppe „AG1“ ein Mal ausgelöst. Wird Warnungsregel „rule1“ ausgelöst, wird zusätzlich auch „AG2“ ausgelöst. In Aktionsregeln definierte Aktionsgruppen und Warnungsregeln arbeiten unabhängig voneinander, und es erfolgt keine Deduplizierung. 

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Warnungen in Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)
