---
title: Erstellen interaktiver Berichte mit Visualisierungen von Azure Monitor-Arbeitsmappen | Microsoft-Dokumentation
description: In diesem Artikel werden sämtliche Komponenten der Visualisierungen von Azure Monitor-Arbeitsmappen erläutert, einschließlich von Text, Diagrammen, Rastern, Strukturen und Graphs.
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.service: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: d878e8b5dd49ba517f85ebb74332bc6a245c33ca
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164172"
---
# <a name="azure-monitor-workbook-visualizations"></a>Visualisierungen von Azure Monitor-Arbeitsmappen

Azure Monitor-Arbeitsmappen unterstützen verschiedene Arten von Visualisierungen, die Ihren Anforderungen an die Berichterstellung gerecht werden. In diesem Artikel finden Sie Beispiele für jede Art von Visualisierung.

## <a name="text"></a>Text

Autoren können Textblöcke in Ihre Arbeitsmappen einschließen. Beim Text kann es sich um Analysen von Telemetriedaten durch einen Bearbeiter, Informationen, anhand derer Benutzer Ihre Daten interpretieren können, Abschnittsüberschriften usw. handeln.

![Screenshot der Apdex-Tabelle mit Text](./media/workbooks-visualizations/apdex.png)

Text wird über ein Markdown-Steuerelement hinzugefügt, mit dem die Formatierung umfassend gesteuert werden kann.

![Screenshot des unformatierten Markdowns, aus dem die gerenderte Tabelle erstellt wird](./media/workbooks-visualizations/markdown.png)

### <a name="add-a-text-control"></a>Hinzufügen eines Textsteuerelements

1. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement **Bearbeiten** klicken.
2. Klicken Sie auf den Link **Text hinzufügen**, um der Arbeitsmappe ein Textsteuerelement hinzuzufügen.
3. Fügen Sie dem Steuerelement Markdown hinzu.
4. Klicken Sie auf die Schaltfläche **Bearbeitung abgeschlossen**, um den formatierten Text anzuzeigen.

> [!TIP]
> Verwenden Sie dieses [Markdown-Cheat Sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet), um sich über verschiedene Formatierungsoptionen zu informieren.

## <a name="charts"></a>Diagramme

In Arbeitsmappen können Überwachungsdaten in Diagrammen präsentiert werden. Unterstützte Diagrammtypen sind Liniendiagramm, Balkendiagramm, Balkendiagramm (kategorisch), Flächendiagramm, Punktdiagramm, Kreisdiagramm und Zeitdiagramm. Autoren können Höhe, Breite, Farbpalette, Legende, Titel, Meldung „Keine Daten“ usw. für das Diagramm anpassen.

Arbeitsmappen unterstützen Diagramme für Protokolle und Metrikdatenquellen. 

### <a name="adding-a-log-chart"></a>Hinzufügen eines Diagramms für Protokolldaten

1. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement **Bearbeiten** klicken.
2. Klicken Sie auf den Link **Abfrage hinzufügen**, um der Arbeitsmappe ein Protokollabfrage-Steuerelement hinzuzufügen.
3. Wählen Sie für den Abfragetyp **Protokoll** aus, wählen Sie den Ressourcentyp (z. B. Application Insights) sowie die Zielressourcen aus.
4. Geben Sie über den Abfrage-Editor die [KQL](https://docs.microsoft.com/azure/kusto/query/) für Ihre Analyse ein (z. B. Trend der Anforderungen).
5. Legen Sie für die Visualisierung eine der folgenden Optionen fest: **Fläche**, **Balken**, **Balkendiagramm (kategorisch)** , **Linie**, **Kreis**, **Punkt** oder **Zeit**.
6. Legen Sie ggf. weitere Parameter fest, beispielsweise Zeitraum, Visualisierung, Größe, Farbpalette und Legende.

![Screenshot eines Diagramms für Protokolldaten im Bearbeitungsmodus](./media/workbooks-visualizations/log-chart.png)

#### <a name="log-chart-parameters"></a>Parameter des Diagramms für Protokolldaten

| Parameter | Erklärung | Beispiel |
| ------------- |:-------------|:-------------|
| `Query Type` | Typ der zu verwendenden Abfrage | Protokoll, Azure Resource Graph usw. |
| `Resource Type` | Typ der Zielressource | Application Insights, Log Analytics oder Azure |
| `Resources` | Satz von Ressourcen, aus denen der Metrikwert abgerufen werden soll | MyApp1 |
| `Time Range` | Zeitfenster für die Anzeige des Diagramms für Protokolldaten | Letzte Stunde, Letzte 24 Stunden usw. |
| `Visualization` | Zu verwendende Visualisierung | Fläche, Balken, Linie, Kreis, Punkt, Zeit, Balkendiagramm (kategorisch) |
| `Size` | Vertikale Größe des Steuerelements | Klein, mittel, groß oder vollständig |
| `Color palette` | Im Diagramm zu verwendende Farbpalette. Wird im Modus mit mehreren Metriken oder segmentierten Modus ignoriert. | Blau, grün, rot usw. |
| `Legend` | Für die Legende zu verwendende Aggregationsfunktion | Summe oder Durchschnitt der Werte oder Max, Min, Erster Wert, Letzter Wert |
| `Query` | Beliebige KQL-Abfrage, die Daten in dem von der Diagrammvisualisierung erwarteten Format zurückgibt | _requests \| make-series Requests = count() default = 0 on timestamp from ago(1d) to now() step 1h_ |

### <a name="adding-a-metric-chart"></a>Hinzufügen eines Diagramms für Metrikdaten

1. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement **Bearbeiten** klicken.
2. Klicken Sie auf den Link **Metrik hinzufügen**, um der Arbeitsmappe ein Metriksteuerelement hinzuzufügen.
3. Wählen Sie einen Ressourcentyp (z. B. Speicherkonto), die Zielressourcen, den Namespace und Name für die Metrik sowie die zu verwendende Aggregation aus.
4. Legen Sie ggf. weitere Parameter fest, beispielsweise Zeitraum, „Aufteilen nach“, Visualisierung, Größe und Farbpalette.

![Screenshot eines Diagramms für Metrikdaten im Bearbeitungsmodus](./media/workbooks-visualizations/metric-chart.png)

#### <a name="metric-chart-parameters"></a>Parameter des Diagramms für Metrikdaten

| Parameter | Erklärung | Beispiel |
| ------------- |:-------------|:-------------|
| `Resource Type` | Typ der Zielressource | Speicher oder VM. |
| `Resources` | Satz von Ressourcen, aus denen der Metrikwert abgerufen werden soll | MyStorage1 |
| `Namespace` | Der Namespace für die Metrik | Speicher > Blob |
| `Metric` | Die zu visualisierende Metrik | Speicher > Blob > Transaktionen |
| `Aggregation` | Auf die Metrik anzuwendende Aggregationsfunktion | Summe, Anzahl, Durchschnitt usw. |
| `Time Range` | Zeitfenster für die Anzeige der Metrik | Letzte Stunde, Letzte 24 Stunden usw. |
| `Visualization` | Zu verwendende Visualisierung | Fläche, Balken, Linie, Punkt, Raster |
| `Split By` | Optionales Aufteilen der Metrik in einer Dimension | Transaktionen nach Geotyp |
| `Size` | Vertikale Größe des Steuerelements | Klein, mittel oder groß |
| `Color palette` | Im Diagramm zu verwendende Farbpalette. Wird ignoriert, wenn der `Split by`-Parameter verwendet wird. | Blau, grün, rot usw. |

## <a name="grids"></a>Raster

Raster und Tabellen werden häufig genutzt, um Benutzern Daten zu präsentieren. In Arbeitsmappen können Benutzer die Spalten des Rasters individuell formatieren, um eine attraktive Benutzeroberfläche für ihre Berichte zu schaffen.

Das unten stehende Beispiel veranschaulicht ein Raster, in dem komplexe Informationen mithilfe von Symbolen, Wärmebildern und Sparkbars präsentiert werden. Die Arbeitsmappe bietet auch eine Sortierfunktion, ein Suchfeld sowie eine Schaltfläche zum Wechseln zur Analyse.

![Screenshot eines auf Protokolldaten basierenden Rasters](./media/workbooks-visualizations/grid.png)

### <a name="adding-a-log-based-grid"></a>Hinzufügen eines auf Protokolldaten basierenden Rasters

1. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement **Bearbeiten** klicken.
2. Klicken Sie auf den Link **Abfrage hinzufügen**, um der Arbeitsmappe ein Protokollabfrage-Steuerelement hinzuzufügen.
3. Wählen Sie für den Abfragetyp **Protokoll** aus, wählen Sie den Ressourcentyp (z. B. Application Insights) sowie die Zielressourcen aus.
4. Geben Sie über den Abfrage-Editor die KQL für Ihre Analyse ein (z. B. VMs, deren Arbeitsspeicher unter einem Schwellenwert liegt).
5. Legen Sie die Visualisierung auf **Raster** fest.
6. Legen Sie ggf. weitere Parameter fest, beispielsweise Zeitraum, Größe, Farbpalette und Legende.

![Screenshot für Abfrage eines auf Protokolldaten basierenden Rasters](./media/workbooks-visualizations/grid-query.png)

## <a name="tiles"></a>Kacheln

Kacheln sind hilfreich, wenn Sie Zusammenfassungsdaten in Arbeitsmappen präsentieren möchten. Die Abbildung unten veranschaulicht einen allgemeinen Anwendungsfall für Kacheln: eine Zusammenfassung auf der App-Ebene über einem detaillierten Raster.

![Screenshot einer Ansicht mit Zusammenfassung in einer Kachel](./media/workbooks-visualizations/tiles-summary.png)

In Kacheln in Arbeitsmappen können ein Titel, Untertitel, großer Text, Symbole, metrikbasierte Farbverläufe, Sparklines/Sparkbars, Fußzeile usw. angezeigt werden.

### <a name="adding-a-tile"></a>Hinzufügen einer Kachel

1. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement _Bearbeiten_ klicken.
2. Klicken Sie auf den Link **Abfrage hinzufügen**, um der Arbeitsmappe ein Protokollabfrage-Steuerelement hinzuzufügen. 
3. Wählen Sie für den Abfragetyp **Protokoll** aus, wählen Sie den Ressourcentyp (z. B. Application Insights) sowie die Zielressourcen aus.
4. Geben Sie über den Abfrage-Editor die KQL für Ihre Analyse ein.
    ```kusto
    requests
    | summarize Requests = count() by appName, name
    | top 7 by Requests desc
    ```
5. Legen Sie die Größe auf **Voll** fest.
6. Legen Sie die Visualisierung auf **Kacheln** fest.
7. Klicken Sie auf die Schaltfläche **Kacheleinstellungen**, um den Bereich mit Einstellungen zu öffnen.
8. Legen Sie in **Kachelfelder** Folgendes fest:
    * Titel: `name`
    * Links: `Requests`, Renderer: `Big Number`, Farbpalette: `Green to Red`, Minimalwert: `0`
    * Unten: `appName`
9. Klicken Sie unten im Bereich auf die Schaltfläche **Speichern und schließen**.

![Screenshot einer Ansicht mit Zusammenfassung in einer Kachel](./media/workbooks-visualizations/tile-settings.png)

So sehen Kacheln im Lesemodus aus:

![Screenshot einer Ansicht mit Zusammenfassung in einer Kachel](./media/workbooks-visualizations/tiles-read-mode.png)

## <a name="trees"></a>Strukturen

Arbeitsmappen unterstützen hierarchische Ansichten über Strukturraster. In Strukturen können einige Zeilen für einen Drilldown auf die nächste Ebene erweitert werden.

Das unten stehende Beispiel zeigt Metriken zur Containerintegrität (Arbeitssatzgröße), die als Strukturraster visualisiert sind. Die Knoten der obersten Ebene sind hier AKS-Knoten (Azure Kubernetes Service), die nächste Ebene sind Pods, und die letzte Ebene sind Container. Beachten Sie, dass Sie die Spalten weiterhin wie in einem Raster formatieren können (Wärmebild, Symbole, Link). Die zugrunde liegende Datenquelle ist in diesem Fall ein Log Analytics-Arbeitsbereich mit AKS-Protokollen.

![Screenshot einer Ansicht mit Zusammenfassung in einer Kachel](./media/workbooks-visualizations/trees.png)

### <a name="adding-a-tree-grid"></a>Hinzufügen eines Strukturrasters
1. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement _Bearbeiten_ klicken.
2. Klicken Sie auf den Link **Abfrage hinzufügen**, um der Arbeitsmappe ein Protokollabfrage-Steuerelement hinzuzufügen. 
3. Wählen Sie für den Abfragetyp **Protokoll** aus, wählen Sie den Ressourcentyp (z. B. Application Insights) sowie die Zielressourcen aus.
4. Geben Sie über den Abfrage-Editor die KQL für Ihre Analyse ein.
    ```kusto
    requests
    | summarize Requests = count() by ParentId = appName, Id = name
    | extend Kind = 'Request', Name = strcat('🌐 ', Id)
    | union (requests
    | summarize Requests = count() by Id = appName
    | extend Kind = 'Request', ParentId = '', Name = strcat('📱 ', Id))
    | project Name, Kind, Requests, Id, ParentId
    | order by Requests desc
    ```
5. Legen Sie die Visualisierung auf **Raster** fest.
6. Klicken Sie auf die Schaltfläche **Spalteneinstellungen**, um den Bereich mit Einstellungen zu öffnen.
7. Legen Sie unten im Abschnitt **Einstellungen für „Struktur“/„Gruppieren nach“** Folgendes fest:
    * Strukturtyp: `Parent/Child`
    * ID-Feld: `Id`
    * Übergeordnetes ID-Feld: `ParentId`
    * Erweiterung anzeigen für: `Name`
    * Oberste Ebene der Struktur erweitern: `checked`
8. Legen Sie im Abschnitt _Spalten_ oben Folgendes fest:
    * _ID_ – Säulenrenderer: `Hidden`
    * _Übergeordnete ID_ – Säulenrenderer: `Hidden`
    * _Anforderungen_ – Säulenrenderer: `Bar`, Farbe: `Blue`, Minimalwert: `0`
9. Klicken Sie unten im Bereich auf die Schaltfläche _Speichern und schließen_.    

![Screenshot einer Ansicht mit Zusammenfassung in einer Kachel](./media/workbooks-visualizations/tree-settings.png)

### <a name="tree-settings"></a>Struktureinstellungen

| Einstellung | Erklärung |
|:------------- |:-------------|
| `Id Field` | Die eindeutige ID jeder Zeile im Raster. |
| `Parent Id Field` | Die ID des übergeordneten Elements der aktuellen Zeile. |
| `Show the expander on` | Die Spalte, für welche die Strukturerweiterung angezeigt werden soll. In Strukturrastern wird das Feld mit der ID und der übergeordneten ID normalerweise ausgeblendet, da diese nicht allzu lesbar sind. Stattdessen wird die Erweiterung in einem Feld mit einem besser lesbaren Wert (wie z. B. dem Namen der Entität) angezeigt. |
| `Expand the top level of the tree` | Wenn diese Option aktiviert ist, wird der Strukturraster auf der obersten Ebene erweitert. Die Option ist nützlich, wenn Sie standardmäßig weitere Informationen anzeigen möchten. |

## <a name="graphs"></a>Graphs

Arbeitsmappen unterstützen das Visualisieren beliebiger Graphs auf der Grundlage von Daten aus Protokollen, um die Beziehungen zwischen überwachten Entitäten aufzuzeigen.

Der unten stehende Graph zeigt Daten, die von einem Computer über diverse Ports an externe Computer ausgehen bzw. von externen Computern eingehen. Sie sind nach Typ gefärbt (Computer, Port, externe IP), und die Kantenstärken entsprechen dem Umfang des dazwischenliegenden Datenflusses. Die zugrunde liegenden Daten stammen aus einer KQL-Abfrage von VM-Verbindungen.

![Screenshot einer Ansicht mit Zusammenfassung in einer Kachel](./media/workbooks-visualizations/graph.png)

### <a name="adding-a-graph"></a>Hinzufügen eines Graphs
1. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement _Bearbeiten_ klicken.
2. Klicken Sie auf den Link **Abfrage hinzufügen**, um der Arbeitsmappe ein Protokollabfrage-Steuerelement hinzuzufügen. 
3. Wählen Sie für den Abfragetyp **Protokoll** aus, wählen Sie den Ressourcentyp (z. B. Application Insights) sowie die Zielressourcen aus.
4. Geben Sie über den Abfrage-Editor die KQL für Ihre Analyse ein.
    ```kusto
    let data = dependencies
    | summarize Calls = count() by App = appName, Request = operation_Name, Dependency = name
    | extend RequestId = strcat(App, '::', Request);
    let links = data
    | summarize Calls = sum(Calls) by App, RequestId
    | project SourceId = App, TargetId = RequestId, Calls, Kind = 'App -> Request'
    | union (data
        | project SourceId = RequestId, TargetId = Dependency, Calls, Kind = 'Request -> Dependency');
    let nodes = data
    | summarize Calls = sum(Calls) by App
    | project Id = App, Name = App, Calls, Kind = 'App'
    | union (data
        | summarize Calls = sum(Calls) by RequestId, Request
        | project Id = RequestId, Name = Request, Calls, Kind = 'Request')
    | union (data
        | summarize Calls = sum(Calls) by Dependency
        | project Id = Dependency, Name = Dependency, Calls, Kind = 'Dependency');
    nodes
    | union (links)
    ```
7. Legen Sie die Visualisierung auf **Graph** fest.
8. Klicken Sie auf die Schaltfläche **Grapheinstellungen**, um den Bereich mit Einstellungen zu öffnen.
9. Legen Sie in _Layout Fields_ (Layout-Felder) unten Folgendes fest:
    * `Node Id`: `Id`
    * `Source Id`: `SourceId`
    * `Target Id`: `TargetId`
    * `Edge Label`: `None`
    * `Edge Size`: `Calls`
    * `Node Size`: `None`
    * `Coloring Type`: `Categorical`
    * `Node Color Field`: `Kind`
    * `Color palette`: `Pastel`
10. Legen Sie oben in _Einstellungen für Knotenformat_ Folgendes fest:
    * _Inhalt oben_ – Spalte verwenden: `Name`, Säulenrenderer: `Text`
    * _Inhalt zentriert_ – Spalte verwenden: `Calls`, Säulenrenderer: `Big Number`, Farbpalette: `None`
    * _Inhalt unten_ – Spalte verwenden: `Kind`, Säulenrenderer: `Text`
10. Klicken Sie unten im Bereich auf die Schaltfläche _Speichern und schließen_.

![Screenshot einer Ansicht mit Zusammenfassung in einer Kachel](./media/workbooks-visualizations/graph-settings.png)

## <a name="next-steps"></a>Nächste Schritte

* [Stellen](workbooks-automate.md) Sie Arbeitsmappen mit Azure Resource Manager bereit.
* [Steuern](workbooks-access-control.md) Sie den Zugriff auf Ihre Arbeitsmappenressourcen, und geben Sie diese frei.