---
title: Zeitreihenmodell in Azure Time Series Insights (Vorschauversion) | Microsoft-Dokumentation
description: Grundlegendes zum Zeitreihenmodell in Azure Time Series.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 10/25/2019
ms.custom: seodec18
ms.openlocfilehash: 3c9bface359df020cea0bfff8f82e25e25efbc47
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73585248"
---
# <a name="time-series-model"></a>Time Series-Modell

Dieser Artikel beschreibt das Zeitreihenmodell sowie die zugehörigen Funktionen und erläutert, wie Sie selbst Modelle in der Azure Time Series Insights Preview-Umgebung erstellen und aktualisieren.

> [!TIP]
>  * Wechseln Sie zur Demoumgebung  [Contoso Wind Farm](https://insights.timeseries.azure.com/preview/samples), um ein Livebeispiel für ein Zeitreihenmodell zu sehen.
> * Informieren Sie sich über den [Azure Time Series Insights Preview-Explorer](time-series-insights-update-explorer.md), um zu erfahren, wie Sie in der Benutzeroberfläche ihres Zeitreihenmodells navigieren können.

## <a name="summary"></a>Zusammenfassung

Üblicherweise fehlt den von IoT-Geräten erfassten Daten Kontextinformationen, was es erschwert, Sensoren schnell zu finden und zu analysieren. Das Hauptargument für ein Zeitreihenmodell ist die Vereinfachung der Suche nach und Analyse von IoT- oder Zeitreihendaten. Dieses Ziel wird erreicht, indem das Modell die Zusammenstellung, Wartung und Anreicherung von Zeitreihendaten ermöglicht, um die Vorbereitung von Datasets für die Analyse zu unterstützen, die direkt vom Consumer eingesetzt werden können.

## <a name="scenario-contosos-new-smart-oven"></a>Szenario: Der neue intelligente Herd von Contoso

**Sehen Sie sich dieses fiktive Szenario eines intelligenten Herds von Contoso an.** In diesem Szenario nehmen wir an, dass jeder intelligente Herd von Contoso über fünf Temperatursensoren verfügt: einen für jede der vier Herdplatten und einen für den Backofen. Bis vor Kurzem hat jeder Contoso-Temperatursensor Daten individuell gesendet, gespeichert und visualisiert. Zur Überwachung der Küchengeräte verwendete Contoso für jeden einzelnen Sensor ein einfaches Diagramm.

Contoso war zwar mit der ursprünglichen Daten- und Visualisierungslösung zufrieden, allerdings wurden bald einige Einschränkungen offensichtlich:

* Kunden wollten wissen, wie heiß der Herd insgesamt wird, wenn die meisten Platten eingeschaltet sind. Contoso hatte einige Schwierigkeiten, die Bedingungen des Herds zu analysieren und eine einheitliche Antwort zu finden.
* Die Techniker von Contoso wollten sicherstellen, dass es nicht zu einem Leistungsabfall kommt, wenn alle Herdplatten gleichzeitig eingeschaltet sind. Es war gar nicht so einfach, die richtigen Querverbindungen zu ermitteln und herauszufinden, welche Temperatur- und Spannungssensoren einander zugeordnet sind und wie sich diese im Laden lokalisieren lassen.
* Das Qualitätssicherungsteam von Contoso wollte den Entwicklungsverlauf überwachen und die beiden Sensorversionen miteinander vergleichen. Herauszufinden, welche Daten zu welcher Sensorversion gehörten, war ebenfalls nicht leicht.

Ohne die Möglichkeit, ein übergreifendes Zeitreihenmodell für den intelligenten Herd zu strukturieren, zu organisieren und zu definieren, verwaltete jeder Sensor nicht lokalisierte, isolierte und wenig informative Datenpunkte. Da jedes Dataset unabhängig von den anderen vorlag, war es ziemlich kompliziert, aus diesen Datenpunkten umsetzbare Erkenntnisse zu ziehen.

Diese Einschränkungen machten deutlich, wie wichtig intelligente Tools zur Aggregierung und Visualisierung von Daten für den neuen Herd von Contoso waren:

* Die Datenvisualisierung ist nützlich, wenn Sie Daten in einer komfortablen Ansicht zuordnen und miteinander kombinieren können. Ein Beispiel hierfür ist die parallele Anzeige von Spannungs- und Temperatursensoren.
* Die Verwaltung mehrdimensionaler Daten für eine Vielzahl von Entitäten sowie von Funktionen für Vergleiche, Zoomen und Zeitbereiche kann ausgesprochen knifflig sein.

Ein **Zeitreihenmodell bietet eine praktische Lösung** für viele Szenarien in diesem fiktiven Beispiel:

[![Diagramme aus Zeitreihenmodellen](media/v2-update-tsm/tsi-charting.png)](media/v2-update-tsm/tsi-charting.png#lightbox)

* Das Zeitreihenmodell spielt eine wichtige Rolle bei Abfragen und Navigation, weil es Daten in Kontext bringt, indem es Vergleiche über Zeitbereiche hinweg und zwischen verschiedenen Arten von Sensoren und Geräten ermöglicht.
* Der Kontext wird für Daten weiter konkretisiert, weil im Zeitreihenmodell gespeicherte Daten Berechnungen von Zeitreihenabfragen als Variablen beibehalten und diese zur Abfragezeit verwenden.
* Das Zeitreihenmodell organisiert und aggregiert Daten, um Funktionen für Visualisierung und Verwaltung zu verbessern.

### <a name="key-capabilities"></a>Wichtige Funktionen

Mit dem Ziel, die Verwaltung der Zeitreihenkontextualisierung einfach und mühelos zu gestalten, ermöglicht das Zeitreihenmodell die folgenden Funktionen in Time Series Insights Preview. Es ermöglicht Folgendes:

* Erstellen und Verwalten von Berechnungen oder Formeln auf Basis von Skalarfunktionen, Aggregieren von Vorgängen usw.
* Definieren von Beziehungen zwischen über- und untergeordneten Elementen, um Navigation, Suche und Verweise zu ermöglichen.
* Definieren von Eigenschaften, die den Instanzen zugeordnet sind – definiert als *Instanzfelder* –, und Verwenden dieser Eigenschaften zum Erstellen von Hierarchien.

### <a name="components"></a>Komponenten

Das Zeitreihenmodell besteht aus drei Kernkomponenten:

* [Zeitreihenmodellinstanzen](#time-series-model-instances)
* [Zeitreihenmodellhierarchien](#time-series-model-hierarchies)
* [Zeitreihenmodelltypen](#time-series-model-types)

Diese Komponenten werden kombiniert, um ein Zeitreihenmodell anzugeben und Ihre Azure Time Series Insights-Daten zu organisieren.

[![Übersicht über Zeitreihenmodelle](media/v2-update-tsm/tsm.png)](media/v2-update-tsm/tsm.png#lightbox)

Ein Zeitreihenmodell kann über die Schnittstelle [Time Series Insights Preview](time-series-insights-update-how-to-tsm.md) erstellt und verwaltet werden. Die Einstellungen eines Zeitreihenmodells können über die [Modelleinstellungs-API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api) verwaltet werden.

## <a name="time-series-model-instances"></a>Zeitreihenmodellinstanzen

*Instanzen* von Zeitreihenmodellen sind virtuelle Darstellungen der Zeitreihe selbst.

In den meisten Fällen werden die Instanzen durch die **deviceId** oder die **assetId** eindeutig identifiziert, die als Zeitreihen-IDs gespeichert sind.

Instanzen sind beschreibende Informationen zugeordnet, die als *Instanzeigenschaften* bezeichnet werden – z. B. ID, Typ, Name, Beschreibung, Hierarchien und Instanzfelder einer Zeitreihe. Instanzeigenschaften enthalten mindestens Hierarchieinformationen.

*Instanzfelder* sind eine Sammlung beschreibender Informationen, die Werte für Hierarchieebenen sowie für Hersteller, Bediener usw. umfassen können.

Nachdem eine Ereignisquelle für die Time Series Insights-Umgebung konfiguriert wurde, werden Instanzen in einem Zeitreihenmodell automatisch entdeckt und erstellt. Die Instanzen können mit Zeitreihenmodellabfragen über den Time Series Insights-Explorer erstellt oder aktualisiert werden.

Die Demoumgebung [Contoso Wind Farm](https://insights.timeseries.azure.com/preview/samples) stellt zahlreiche Beispiele für Live-Instanzen bereit.

[![Zeitreihenmodellinstanzen](media/v2-update-tsm/instance.png)](media/v2-update-tsm/instance.png#lightbox)

### <a name="instance-properties"></a>Instanzeigenschaften

Instanzen werden durch **timeSeriesId**, **typeId**, **name**, **description**, **hierarchyIds** und **instanceFields** definiert. Jede Instanz wird nur einem *Typ* sowie einer oder mehreren *Hierarchien* zugeordnet.

| Eigenschaft | BESCHREIBUNG |
| --- | ---|
| timeSeriesId | Der UUID der Zeitreihe, der die Instanz zugeordnet ist. |
| typeId | Der UUID des Zeitreihenmodelltyps, dem die Instanz zugeordnet ist. Standardmäßig werden alle entdeckten neuen Instanzen einem Standardtyp zugeordnet.
| name | Die **name**-Eigenschaft ist optional, und die Groß-/Kleinschreibung wird berücksichtigt. Wenn für **name** kein Wert verfügbar ist, wird die Eigenschaft standardmäßig auf **timeSeriesId** festgelegt. Auch wenn ein Name angegeben wurde, ist **timeSeriesId** weiterhin im [Well](time-series-insights-update-explorer.md#preview-well) verfügbar. |
| description | Eine Textbeschreibung der Instanz. |
| hierarchyIds | Definiert, zu welchen Hierarchien die Instanz gehört. |
| instanceFields | **instanceFields** sind Eigenschaften einer Instanz sowie aller statischen Daten, die eine Instanz definieren. Sie definieren Werte von Hierarchie- oder Nicht-Hierarchie-Eigenschaften, während sie gleichzeitig die Indizierung unterstützen, um Suchvorgänge auszuführen. |

> [!NOTE]
> Hierarchien werden mithilfe von Instanzfeldern erstellt. Für weitere Definitionen von Instanzeigenschaften können weitere **instanceFields** hinzugefügt werden.

Instanzen weisen die folgende JSON-Darstellung auf:

```JSON
{
    "timeSeriesId": ["PU2"],
    "typeId": "545314a5-7166-4b90-abb9-fd93966fa39b",
    "hierarchyIds": ["95f0a8d1-a3ef-4549-b4b3-f138856b3a12"],
    "description": "Pump #2",
    "instanceFields": {
        "Location": "Redmond",
        "Fleet": "Fleet 5",
        "Unit": "Pump Unit 3",
        "Manufacturer": "Contoso",
        "ScalePres": "0.54",
        "scaleTemp": "0.54"
    }
}
```

> [!TIP]
> Informationen zur Time Series Insights Instance-API und zur Unterstützung von CRUD-Vorgängen (Create, Read, Update, Delete) finden Sie im Artikel [Datenabfragen](time-series-insights-update-tsq.md#time-series-model-query-tsm-q-apis) und in der [API-REST-Dokumentation für Instanzen](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api).

## <a name="time-series-model-hierarchies"></a>Zeitreihenmodellhierarchien

*Hierarchien* von Zeitreihenmodellen organisieren Instanzen durch Angabe von Eigenschaftsnamen und Beziehungen.

Sie können in jeder Time Series Insights-Umgebung mehrere Hierarchien konfigurieren. Eine Zeitreihenmodellinstanz kann einer einzelnen Hierarchie oder mehreren Hierarchien (m:n-Beziehung) zugeordnet werden.

Die Clientschnittstelle der Demoumgebung [Contoso Wind Farm](https://insights.timeseries.azure.com/preview/samples) zeigt eine standardmäßige Instanz- und Typhierarchie an.

[![Zeitreihenmodellhierarchien](media/v2-update-tsm/hierarchy.png)](media/v2-update-tsm/hierarchy.png#lightbox)

### <a name="hierarchy-definition"></a>Hierarchiedefinition

Hierarchien werden durch **id**, **name** und **source** der Hierarchie definiert.

| Eigenschaft | BESCHREIBUNG |
| ---| ---|
| id | Der eindeutige Bezeichner für die Hierarchie, der z. B. beim Definieren einer Instanz verwendet wird. |
| name | Eine Zeichenfolge zum Angeben eines Namens für die Hierarchie. |
| source | Gibt die Organisationshierarchie bzw. den Organisationspfad an – eine Anordnung der über- und untergeordneten Elemente einer Hierarchie, die von Benutzern erstellt werden soll. Die über- und untergeordneten Eigenschaften entsprechen Instanzfeldern. |

Hierarchien werden folgendermaßen in JSON dargestellt:

```JSON
{
  "hierarchies": [
    {
      "id": "6e292e54-9a26-4be1-9034-607d71492707",
      "name": "Location",
      "source": {
        "instanceFieldNames": [
          "state",
          "city"
        ]
      }
    },
    {
      "id": "a28fd14c-6b98-4ab5-9301-3840f142d30e",
      "name": "ManufactureDate",
      "source": {
        "instanceFieldNames": [
          "year",
          "month"
        ]
      }
    }
  ]
}
```

Im oben stehenden JSON-Beispiel gilt:

* `Location` definiert eine Hierarchie mit übergeordneten `states`-Elementen und untergeordneten `cities`-Elementen. Jedes `location`-Element kann mehrere `states`-Elemente enthalten, die wiederum mehrere `cities`-Elemente umfassen können.
* `ManufactureDate` definiert eine Hierarchie mit übergeordneten `year`-Elementen und untergeordneten `month`-Elementen. Jedes `ManufactureDate`-Element kann mehrere `years`-Elemente enthalten, die wiederum mehrere `months`-Elemente umfassen können.

> [!TIP]
> Informationen zur Time Series Insights Instance-API und zur Unterstützung von CRUD-Vorgängen (Create, Read, Update, Delete) finden Sie im Artikel [Datenabfragen](time-series-insights-update-tsq.md#time-series-model-query-tsm-q-apis) und in der [API-REST-Dokumentation für Hierarchien](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api).

### <a name="hierarchy-example"></a>Beispiel für eine Hierarchie

Stellen Sie sich ein Beispiel vor, in dem die Hierarchie **H1** in ihrer **instanceFieldNames**-Definition die Elemente `building`, `floor` und `room` enthält.

```JSON
{
  "id": "aaaaaa-bbbbb-ccccc-ddddd-111111",
  "name": "H1",
  "source": {
    "instanceFieldNames": [
      "building",
      "floor",
      "room"
    ]
  }
}
```

Mit den in der vorherigen Definition verwendeten Instanzfeldern und verschiedenen Zeitreihen ergeben sich die in der folgenden Tabelle gezeigten Hierarchieattribute und Werte:

| Time Series-ID | Instanzenfelder |
| --- | --- |
| ID1 | "building" = "1000", "floor" = "10", "room" = "55"  |
| ID2 | "building" = "1000", "room" = "55" |
| ID3 | "floor" = "10" |
| ID4 | "building" = "1000", "floor" = "10"  |
| ID5 | Weder „building“ noch „floor“ noch „room“ wurden festgelegt. |

Die Zeitreihen **ID1** und **ID4** werden im [Azure Time Series Insights-Explorer](time-series-insights-update-explorer.md) als Teil der Hierarchie **H1** angezeigt, weil sie vollständig definierte und richtig angeordnete Parameter *building*, *floor* und *room* aufweisen.

Die anderen Zeitreihen sind unter *Unparented Instances* klassifiziert, weil sie der angegebenen Datenhierarchie nicht entsprechen.

## <a name="time-series-model-types"></a>Zeitreihenmodelltypen

Zeitreihenmodell*typen* helfen Ihnen beim Definieren von Variablen oder Formeln zur Durchführung von Berechnungen. Typen sind einer bestimmten Time Series Insights-Instanz zugeordnet.

Ein Typ kann eine oder mehrere Variablen enthalten. Beispielsweise könnte eine Zeitreihenmodellinstanz den Typ *Temperatursensor* aufweisen, der aus den Variablen *durchschnittliche Temperatur*, *Mindesttemperatur* und *Höchsttemperatur* besteht.

Die Demoumgebung [Contoso Wind Farm](https://insights.timeseries.azure.com/preview/samples) visualisiert verschiedene Zeitreihenmodelltypen mit ihren jeweiligen Instanzen.

[![Zeitreihenmodelltypen](media/v2-update-tsm/types.png)](media/v2-update-tsm/types.png#lightbox)

> [!TIP]
> Informationen zur Time Series Insights Instance-API und zur Unterstützung von CRUD-Vorgängen (Create, Read, Update, Delete) finden Sie im Artikel [Datenabfragen](time-series-insights-update-tsq.md#time-series-model-query-tsm-q-apis) und in der [API-REST-Dokumentation für Typen](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api).

### <a name="type-properties"></a>Typeigenschaften

Zeitreihenmodelltypen werden durch **id**, **name**, **description** und **variables** definiert.

| Eigenschaft | BESCHREIBUNG |
| ---| ---|
| id | Der UUID des Typs. |
| name | Eine Zeichenfolge zum Angeben eines Namens für den Typ. |
| description | Eine Zeichenfolgenbeschreibung für den Typ. |
| variables | Geben Variablen an, die dem Typ zugeordnet sind. |

Typen entsprechen dem folgenden JSON-Beispiel:

```JSON
{
  "types": [
    {
      "id": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
      "name": "DefaultType",
      "description": "Default type",
      "variables": {
        "EventCount": {
          "kind": "aggregate",
          "value": null,
          "filter": null,
          "aggregation": {
            "tsx": "count()"
          }
        },
        "Interpolated Speed": {
          "kind": "numeric",
          "value": {
              "tsx": "$event.[speed].Double"
          },
          "filter": null,
          "interpolation": {
              "kind": "step",
              "boundary": {
                  "span": "P1D"
              }
          },
          "aggregation": {
              "tsx": "left($value)"
          }
        }
      }
    }
  ]
}
```

### <a name="variables"></a>Variables

Time Series Insights-Typen können über viele Variablen verfügen, die Formel- und Berechnungsregeln bei Ereignissen angeben.

Jede Variable kann eine der drei folgenden *Arten* aufweisen: *numerisch*, *kategorisch* und *aggregiert*.

* **Numerische** Variablen arbeiten mit kontinuierlichen Werten. 
* **Kategorische** Variablen arbeiten mit einem definierten Satz diskreter Werte.
* Bei **aggregierten** Werten werden mehrere Variablen einer einzigen Art kombiniert (entweder nur „numerisch“ oder nur „kategorisch“).

Die folgende Tabelle zeigt die Eigenschaften, die für die jeweilige Variablenart relevant sind.

[![Zeitreihenmodelltypen](media/v2-update-tsm/variable-table.png)](media/v2-update-tsm/variable-table.png#lightbox)

#### <a name="numeric-variables"></a>Numerische Variablen

| Variableneigenschaft | BESCHREIBUNG |
| --- | ---|
| Variablenfilter | Filter sind optionale Bedingungsklauseln, um die Anzahl von Zeilen zu beschränken, die bei der Berechnung berücksichtigt werden. |
| Variablenwert | Für Berechnungen verwendete Telemetriewerte, die aus Geräten oder Sensoren stammen oder mithilfe von Zeitreihenausdrücken transformiert werden. Numerische Variablen müssen den Typ *Double* aufweisen.|
| Variableninterpolation | Die Interpolation gibt an, wie ein Signal anhand von vorhandenen Daten rekonstruiert wird. Für numerische Variablen sind die Interpolationsoptionen *Schritt* und *linear* verfügbar. |
| Variablenaggregation | Unterstützt die Berechnung mithilfe von *Avg*, *Min*, *Max*, *Sum*, *Count*, *First*, *Last* sowie zeitgewichteter Operatoren (*Avg*, *Min*, *Max*, *Sum*, *Left*). |

Variablen entsprechen dem folgenden JSON-Beispiel:

```JSON
"Interpolated Speed": {
  "kind": "numeric",
  "value": {
    "tsx": "$event.[speed].Double"
  },
  "filter": null,
  "interpolation": {
    "kind": "step",
    "boundary": {
      "span": "P1D"
    }
  },
  "aggregation": {
    "tsx": "left($value)"
  }
}
```

#### <a name="categorical-variables"></a>Kategorische Variablen

| Variableneigenschaft | BESCHREIBUNG |
| --- | ---|
| Variablenfilter | Filter sind optionale Bedingungsklauseln, um die Anzahl von Zeilen zu beschränken, die bei der Berechnung berücksichtigt werden. |
| Variablenwert | Für Berechnungen verwendete Telemetriewerte, die aus Geräten oder Sensoren stammen. Kategorische Variablen müssen entweder den Typ *Long* oder den Typ *String* aufweisen. |
| Variableninterpolation | Die Interpolation gibt an, wie ein Signal anhand von vorhandenen Daten rekonstruiert wird. Die Option *Schrittinterpolation* ist für kategorische Variablen verfügbar. |
| Variablenkategorien | Kategorien sorgen für die Zuordnung zwischen den aus einem Gerät oder Sensor stammenden Werten und einer Bezeichnung. |
| Standardkategorie für Variablen | Die Standardkategorie steht für alle Werte zur Verfügung, die nicht in der Eigenschaft „Kategorien“ zugeordnet werden. |

Variablen entsprechen dem folgenden JSON-Beispiel:

```JSON
"Status": {
  "kind": "categorical",
  "value": "toLong($event.[Status].Double)",
  "interpolation": {
    "kind": "step",
    "boundary": {
      "span" : "PT1M"
    }
  },
  "categories": [
    {
      "values": [0, 1, 2, 3],
      "label": "Good"
    },
    {
      "values": [3],
      "label": "Bad"
    }
  ],
  "defaultCategory": {
    "label": "Not Applicable"
  }
}
```

#### <a name="aggregate-variables"></a>Aggregieren von Variablen

| Variableneigenschaft | BESCHREIBUNG |
| --- | ---|
| Variablenfilter | Filter sind optionale Bedingungsklauseln, um die Anzahl von Zeilen zu beschränken, die bei der Berechnung berücksichtigt werden. |
| Variablenaggregation | Unterstützt Berechnungen mit *Avg*, *Min*, *Max*, *Sum*, *Count*, *First*, *Last*. |

Variablen entsprechen dem folgenden JSON-Beispiel:

```JSON
"Aggregate Speed": {
  "kind": "aggregate",
  "filter": null,
  "aggregation": {
    "tsx": "avg($event.Speed.Double)"
  }
}
```

Variablen werden in der Typdefinition eines Zeitreihenmodells gespeichert und können inline über [Abfrage-APIs](time-series-insights-update-tsq.md) bereitgestellt werden, um die gespeicherte Definition zu überschreiben.

## <a name="next-steps"></a>Nächste Schritte

- Informationen finden Sie unter [Speicherung und Datenerfassung in Azure Time Series Insights Preview](./time-series-insights-update-storage-ingress.md).
- Unter [Datenmodellierung in Azure Time Series Insights Preview](./time-series-insights-update-how-to-tsm.md) erfahren Sie mehr über allgemeine Zeitreihenmodellvorgänge.
- Lesen Sie auch die neue Referenzdokumentation zu [Zeitreihenmodellen](https://docs.microsoft.com/rest/api/time-series-insights/preview-model).
