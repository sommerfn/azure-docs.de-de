---
title: Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen | Microsoft-Dokumentation
description: Vereinfachen der komplexen Berichterstellung mit vordefinierten und benutzerdefiniert parametrisierten Arbeitsmappen
author: mrbullwinkle
manager: carmonm
services: azure-monitor
ms.service: azure-monitor
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 2cbc128e59472e36e2b5685efdf4cd02144edf15
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164440"
---
# <a name="azure-monitor-workbooks"></a>Azure Monitor-Arbeitsmappen

Arbeitsmappen bieten einen flexiblen Bereich für die Datenanalyse und die Erstellung umfassender visueller Berichte innerhalb des Azure-Portals. Mit ihnen können Sie mehrere Datenquellen in Azure nutzen und sie zu vereinheitlichten interaktiven Oberflächen kombinieren. 

## <a name="data-sources"></a>Datenquellen

Arbeitsmappen können Daten aus mehreren Quellen in Azure abfragen. Die Ersteller von Arbeitsmappen können diese Daten transformieren, um Einblicke in die Verfügbarkeit, Leistung, Nutzung und Gesamtintegrität der zugrunde liegenden Komponenten zu erhalten. Beispielsweise können Leistungsprotokolle von virtuellen Computern analysiert werden, um Fälle von hoher CPU-Auslastung oder geringem Arbeitsspeicher zu ermitteln, und die Ergebnisse können dann als Raster in einem interaktiven Bericht angezeigt werden.
  
Die eigentliche Stärke von Arbeitsmappen ist jedoch die Möglichkeit, Daten aus unterschiedlichen Quellen in einem einzigen Bericht zu kombinieren. Dies ermöglicht die Erstellung von zusammengesetzten Ressourcenansichten oder ressourcenübergreifenden Verknüpfungen für umfassendere Daten und Erkenntnisse, die andernfalls nicht möglich wären.

Arbeitsmappen sind derzeit mit den folgenden Datenquellen kompatibel:

* [Protokolle](workbooks-data-sources.md#logs)
* [Metriken](workbooks-data-sources.md#metrics)
* [Azure Resource Graph](workbooks-data-sources.md#azure-resource-graph)
* [Warnungen (Vorschau)](workbooks-data-sources.md#alerts-preview)
* [Workloadintegrität (Vorschau)](workbooks-data-sources.md#workload-health-preview)
* [Azure Resource Health (Vorschau)](workbooks-data-sources.md#azure-resource-health)
* [Azure Data Explorer (Vorschau)](workbooks-data-sources.md#azure-data-explorer-preview)

## <a name="visualizations"></a>Visualisierungen

Arbeitsmappen bieten eine Vielzahl von Funktionen zur Visualisierung Ihrer Daten. Ausführliche Beispiele der einzelnen Visualisierungstypen stehen über die folgenden Beispiellinks bereit:

* [Text](workbooks-visualizations.md#text)
* [Diagramme](workbooks-visualizations.md#charts)
* [Raster](workbooks-visualizations.md#grids)
* [Kacheln](workbooks-visualizations.md#tiles)
* [Strukturen](workbooks-visualizations.md#trees)
* [Graphs](workbooks-visualizations.md#graphs)

![Visualisierungsbeispiele für Arbeitsmappen](./media/workbooks-overview/visualizations.png)

## <a name="getting-started"></a>Erste Schritte

Zum Erkunden der Oberfläche für Arbeitsmappen navigieren Sie zunächst zum Azure Monitor-Dienst. Dazu können Sie **Monitor** in das Suchfeld im Azure-Portal eingeben.

Wählen Sie dann **Arbeitsmappen (Vorschau)** aus.

![Screenshot der Schaltfläche „Arbeitsmappen (Vorschau)“ in einem roten Kasten](./media/workbooks-overview/workbooks-preview.png)

### <a name="gallery"></a>Katalog

Dadurch gelangen Sie zum Arbeitsmappenkatalog:

![Screenshot des Azure Monitor-Arbeitsmappenkatalogs](./media/workbooks-overview/gallery.png)

### <a name="workbooks-versus-workbook-templates"></a>Arbeitsmappen im Vergleich zu Arbeitsmappenvorlagen

Es werden eine _Arbeitsmappe_ in Grün und eine Reihe von _Arbeitsmappenvorlagen_ in Violett angezeigt. Vorlagen dienen als zusammengestellte Berichte, die von mehreren Benutzern und Teams flexibel wiederverwendet werden können. Beim Öffnen einer Vorlage wird eine temporäre Arbeitsmappe mit dem Inhalt der Vorlage erstellt. 

Sie können die Parameter der vorlagenbasierten Arbeitsmappe anpassen und Analysen durchführen, ohne befürchten zu müssen, dass die zukünftige Berichtsoberfläche für Kollegen verändert wird. Wenn Sie eine Vorlage öffnen, einige Anpassungen vornehmen und dann das Speichersymbol auswählen, speichern Sie die Vorlage als Arbeitsmappe, die dann in Grün angezeigt wird, während die ursprüngliche Vorlage unverändert bleibt. 

Vorlagen unterscheiden sich auch von gespeicherten Arbeitsmappen. Beim Speichern einer Arbeitsmappe wird eine zugeordnete Azure Resource Manager-Ressource erstellt, während der beim Öffnen einer Vorlage erstellten temporären Arbeitsmappe keine eindeutige Ressource zugeordnet ist. Weitere Informationen zur Verwaltung der Zugriffssteuerung in Arbeitsmappen finden Sie im [Artikel zur Zugriffssteuerung für Arbeitsmappen](workbooks-access-control.md).

### <a name="exploring-a-workbook-template"></a>Erkunden einer Arbeitsmappenvorlage

Wählen Sie **Analyse der Anwendungsfehler** aus, um eine der Standardvorlagen für Anwendungsarbeitsmappen anzuzeigen.

![Screenshot der Vorlage zur Anwendungsfehleranalyse](./media/workbooks-overview/failure-analysis.png)

Wie bereits erwähnt, wird beim Öffnen der Vorlage eine temporäre Arbeitsmappe erstellt, mit der Sie interagieren können. Standardmäßig wird die Arbeitsmappe im Lesemodus geöffnet, in dem nur die Informationen für die gewünschte Analyse angezeigt werden, die vom ursprünglichen Vorlagenautor erstellt wurden.

Im Fall dieser speziellen Arbeitsmappe ist die Oberfläche interaktiv. Sie können das Abonnement, die Ziel-Apps und den Zeitbereich der Daten anpassen, die Sie anzeigen möchten. Nachdem Sie diese Auswahl getroffen haben, ist auch das Raster der HTTP-Anforderungen interaktiv. Dadurch werden bei Auswahl einer einzelnen Zeile die in den beiden Diagrammen am unteren Rand des Berichts dargestellten Daten geändert.

### <a name="editing-mode"></a>Bearbeitungsmodus

Um zu verstehen, wie diese Arbeitsmappenvorlage zusammengesetzt ist, müssen Sie in den Bearbeitungsmodus wechseln. Dazu wählen Sie **Bearbeiten** aus. 

![Screenshot der Vorlage zur Anwendungsfehleranalyse](./media/workbooks-overview/edit.png)

Nachdem Sie in den Bearbeitungsmodus gewechselt haben, wird eine Reihe von Feldern des Typs **Bearbeiten** rechts neben den einzelnen Aspekten der Arbeitsmappe angezeigt.

![Screenshot der Schaltfläche „Bearbeiten“](./media/workbooks-overview/edit-mode.png)

Wenn Sie die Schaltfläche „Bearbeiten“ direkt unterhalb des Rasters mit Anforderungsdaten auswählen, sehen Sie, dass dieser Teil der Arbeitsmappe aus einer Kusto-Abfrage für Daten von einer Application Insights-Ressource besteht.

![Screenshot der zugrunde liegenden Kusto-Abfrage](./media/workbooks-overview/kusto.png)

Wenn Sie auf die anderen Schaltflächen **Bearbeiten** auf der rechten Seite klicken, wird eine Reihe von Kernkomponenten angezeigt, aus denen sich Arbeitsmappen zusammensetzen, wie markdownbasierte [Textfelder](workbooks-visualizations.md#text), Benutzeroberflächenelemente für die [Parameterauswahl](workbooks-parameters.md) und andere [Diagramm-/Visualisierungstypen](workbooks-visualizations.md). 

Das Erkunden der vordefinierten Vorlagen im Bearbeitungsmodus, das anschließende Anpassen an Ihre Anforderungen und das Speichern Ihrer eigenen benutzerdefinierten Arbeitsmappe ist eine hervorragende Möglichkeit, um zu erfahren, welche Möglichkeiten Azure Monitor-Arbeitsmappen bieten.

## <a name="pinning-visualizations"></a>Anheften von Visualisierungen

Die Schritte für Text, Abfrage und Metriken in einer Arbeitsmappe können mithilfe der Schaltfläche „Anheften“ an diese Elemente angeheftet werden, während sich die Arbeitsmappe im entsprechenden Modus befindet oder wenn der Arbeitsmappenautor Einstellungen für dieses Element aktiviert hat, damit das Symbol zum Anheften angezeigt wird. 

Zum Zugreifen auf den Modus für Anheften klicken Sie auf **Bearbeiten**, um in den Bearbeitungsmodus zu wechseln, und wählen Sie dann in der oberen Leiste das blaue Stecknadelsymbol aus. Daraufhin wird auf der rechten Seite des Bildschirms ein einzelnes Stecknadelsymbol oberhalb des Felds *Bearbeiten* für den entsprechenden Arbeitsmappenteil angezeigt.

![Anheften](./media/workbooks-overview/pin-experience.png)

> [!NOTE]
> Der Zustand der Arbeitsmappe wird zum Zeitpunkt des Anheftens gespeichert, und angeheftete Arbeitsmappen auf einem Dashboard werden beim Ändern der zugrunde liegenden Arbeitsmappe nicht aktualisiert. Um einen angehefteten Arbeitsmappenteil zu aktualisieren, müssen Sie diesen Teil löschen und erneut anheften.

## <a name="dashboard-time-ranges"></a>Zeitbereiche auf dem Dashboard

Bei angehefteten Abfrageteilen der Arbeitsmappe wird der Zeitbereich des Dashboards berücksichtigt, sofern das angeheftete Element für die Verwendung eines *Zeitbereichsparameters* konfiguriert ist. Der Zeitbereichswert des Dashboards wird als Wert für den Zeitbereichsparameter verwendet, und jede Änderung des Zeitbereichs des Dashboards bewirkt eine Aktualisierung des angehefteten Elements. Wenn ein angehefteter Teil den Zeitbereich des Dashboards verwendet, wird bei jeder Änderung des Zeitbereichs der Untertitel des angehefteten Teils aktualisiert, um den Zeitbereich des Dashboards anzuzeigen. 

Außerdem werden angeheftete Arbeitsmappenteile, die einen Zeitbereichsparameter verwenden, automatisch mit einer Rate aktualisiert, die durch den Zeitbereich des Dashboards bestimmt wird. Der letzte Zeitpunkt, zu dem die Abfrage ausgeführt wurde, wird im Untertitel des angehefteten Teils angezeigt.

Wenn ein angehefteter Schritt einen explizit festgelegten Zeitbereich aufweist (keinen Zeitbereichsparameter verwendet), wird unabhängig von den Einstellungen des Dashboards immer dieser Zeitbereich für das Dashboard verwendet. Der Untertitel des angehefteten Teils zeigt dann nicht den Zeitbereich des Dashboards an, und die Abfrage wird auf dem Dashboard nicht automatisch aktualisiert. Der Untertitel zeigt den Zeitpunkt an, zu dem die Abfrage zuletzt ausgeführt wurde.

> [!NOTE]
> Abfragen, bei denen die Datenquelle *Zusammenführen* verwendet wird, werden beim Anheften an Dashboards derzeit nicht unterstützt.

## <a name="sharing-workbook-templates"></a>Freigeben von Arbeitsmappenvorlagen

Sobald Sie mit dem Erstellen Ihrer eigenen Arbeitsmappenvorlagen beginnen, können Sie diese für die breite Öffentlichkeit freigeben. Wenn Sie weitere Informationen wünschen und andere Vorlagen erkunden möchten, die nicht Teil der Azure Monitor-Standardkatalogansicht sind, besuchen Sie unser [GitHub-Repository](https://github.com/Microsoft/Application-Insights-Workbooks/blob/master/README.md). Zum Durchsuchen vorhandener Arbeitsmappen besuchen Sie die [Arbeitsmappenbibliothek](https://github.com/microsoft/Application-Insights-Workbooks/tree/master/Workbooks) auf GitHub.

## <a name="next-step"></a>Nächster Schritt

* [Erfahren Sie mehr](workbooks-visualizations.md) über die vielen umfassenden Visualisierungsoptionen für Arbeitsmappen.
* [Steuern](workbooks-access-control.md) Sie den Zugriff auf Ihre Arbeitsmappenressourcen, und geben Sie diese frei.
