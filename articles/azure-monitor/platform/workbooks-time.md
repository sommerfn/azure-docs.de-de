---
title: Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen | Zeitparameter | Microsoft-Dokumentation
description: Vereinfachen der komplexen Berichterstellung mit vordefinierten und benutzerdefiniert parametrisierten Arbeitsmappen
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.service: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 906ebcc49eca72744abf5854a1b320407d0384dc
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164312"
---
# <a name="workbook-time-parameters"></a>Arbeitsmappen-Zeitparameter

Mithilfe von Zeitparametern können Benutzer den zeitlichen Kontext der Analyse festlegen; sie werden in fast allen Berichten verwendet. Sie sind relativ einfach einzurichten und zu verwenden und ermöglichen Autoren das Angeben der Zeitbereiche, die im Dropdown angezeigt werden soll. Auch benutzerdefinierte Zeitbereiche sind möglich. 

## <a name="creating-a-time-parameter"></a>Erstellen eines Zeitparameters
1. Beginnen Sie mit einer leeren Arbeitsmappe im Bearbeitungsmodus.
2. Wählen Sie unter den Links in der Arbeitsmappe die Option _Parameter hinzufügen_ aus.
3. Klicken Sie auf die blaue Schaltfläche _Parameter hinzufügen_.
4. Geben Sie im daraufhin angezeigten Bereich für den neuen Parameter Folgendes ein:
    1. Parametername: `TimeRange`
    2. Parametertyp: `Time range picker`
    3. Erforderlich: `checked`
    4. Verfügbare Zeitbereiche: „Letzte Stunde“, „Letzte 12 Stunden“, „Letzte 24 Stunden“, „Letzte 48 Stunden“, „Letzte 3 Tage“, „Letzte 7 Tage“ und „Auswahl benutzerdefinierter Zeitbereiche zulassen“
5. Wählen Sie auf der Symbolleiste die Option „Speichern“ aus, um den Parameter zu erstellen.

    ![Abbildung zum Erstellen eines Zeitbereichsparameters](./media/workbooks-time/time-settings.png)

So sieht die Arbeitsmappe im Lesemodus aus.

![Abbildung eines Zeitbereichsparameters im Lesemodus](./media/workbooks-time/parameters-time.png)

## <a name="referencing-a-time-parameter"></a>Verweisen auf einen Zeitparameter
### <a name="via-bindings"></a>Über Bindungen
1. Fügen Sie der Arbeitsmappe ein Abfragesteuerelement hinzu, und wählen Sie eine Application Insights-Ressource aus.
2. Die meisten Arbeitsmappen-Steuerelemente unterstützen eine _Zeitbereich_-Bereichsauswahl. Öffnen Sie das Dropdown _Zeitbereich_, und wählen Sie in der Gruppe der Zeitbereichsparameter unten `{TimeRange}` aus.
3. Dadurch wird der Zeitbereichsparameter an den Zeitbereich des Diagramms gebunden. Der Zeitbereich der Beispielabfrage ist nun auf „Letzte 24 Stunden“ festgelegt.
4. Ausführen einer Abfrage, um die Ergebnisse anzuzeigen

    ![Abbildung eines Zeitbereichsparameters, auf den über Bindungen verwiesen wird](./media/workbooks-time/time-binding.png)

### <a name="in-kql"></a>In einer KQL
1. Fügen Sie der Arbeitsmappe ein Abfragesteuerelement hinzu, und wählen Sie eine Application Insights-Ressource aus.
2. Geben Sie in der KQL einen Zeitbereichsfilter mit dem folgenden Parameter an: `| where timestamp {TimeRange}`
3. Dadurch wird die Abfrageauswertungszeit auf `| where timestamp > ago(1d)` erweitert, den Zeitbereichswert des Parameters.
4. Ausführen einer Abfrage, um die Ergebnisse anzuzeigen

    ![Abbildung eines Zeitbereichs, auf den in einer KQL verwiesen wird](./media/workbooks-time/time-in-code.png)

### <a name="in-text"></a>In Text 
1. Fügen Sie der Arbeitsmappe ein Textsteuerelement hinzu.
2. Geben Sie im Markdown `The chosen time range is {TimeRange:label}` ein.
3. Wählen Sie _Bearbeitung abgeschlossen_ aus.
4. Im Textsteuerelement wird Text angezeigt: _The chosen time range is Last 24 hours_ (Der ausgewählte Zeitbereich ist „Letzte 24 Stunden“)

## <a name="time-parameter-options"></a>Optionen für Zeitparameter
| Parameter | Erklärung | Beispiel |
| ------------- |:-------------|:-------------|
| `{TimeRange}` | Bezeichnung des Zeitbereichs | Letzte 24 Stunden |
| `{TimeRange:label}` | Bezeichnung des Zeitbereichs | Letzte 24 Stunden |
| `{TimeRange:value}` | Wert des Zeitbereichs | > ago(1d) |
| `{TimeRange:query}` | Abfrage des Zeitbereichs | > ago(1d) |
| `{TimeRange:start}` | Startzeit des Zeitbereichs | 3/20/2019 4:18 PM |
| `{TimeRange:end}` | Endzeit des Zeitbereichs | 3/21/2019 4:18 PM |
| `{TimeRange:grain}` | Intervall des Zeitbereichs | 30 m |


### <a name="using-parameter-options-in-a-query"></a>Verwenden von Parameteroptionen in einer Abfrage
```kusto
requests
| make-series Requests = count() default = 0 on timestamp from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain}
```

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr](workbooks-visualizations.md) über die vielen umfassenden Visualisierungsoptionen für Arbeitsmappen.
* [Steuern](workbooks-access-control.md) Sie den Zugriff auf Ihre Arbeitsmappenressourcen, und geben Sie diese frei.
