---
title: Weitere Informationen zu Dropdownparametern in Azure Monitor-Arbeitsmappen | Microsoft-Dokumentation
description: Vereinfachen der komplexen Berichterstellung mit vordefinierten und benutzerdefiniert parametrisierten Arbeitsmappen, die Dropdownparameter enthalten
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.service: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 20a1c5f4b4ef12f81e801769a2ee1b5f08860e38
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164220"
---
# <a name="workbook-drop-down-parameters"></a>Dropdownparameter in Arbeitsmappen

Mithilfe von Dropdowns können Benutzer einen oder mehrere Eingabewerte aus einer bekannten Gruppe erfassen (Sie wählen z. B. eine der Anforderungen Ihrer App aus). Dropdowns bieten eine benutzerfreundliche Möglichkeit, beliebige Eingaben von Benutzern zu erfassen. Dropdowns sind besonders nützlich, um Filtervorgänge in interaktiven Berichten zu ermöglichen. 

Sie können ein Dropdown am einfachsten angeben, indem Sie in der Parametereinstellung eine statische Liste angeben. Interessanter wird es, wenn Sie die Liste dynamisch über eine KQL-Abfrage abrufen. Mit Parametereinstellungen können Sie außerdem angeben, ob es sich um eine Einfach- oder Mehrfachauswahl handelt. Bei einer Mehrfachauswahl können Sie angeben, wie die Ergebnisse formiert werden sollen (Trennzeichen, Anführungszeichen usw.).

## <a name="creating-a-static-drop-down-parameter"></a>Erstellen eines statischen Dropdownparameters

1. Beginnen Sie mit einer leeren Arbeitsmappe im Bearbeitungsmodus.
2. Wählen Sie unter den Links in der Arbeitsmappe die Option _Parameter hinzufügen_ aus.
3. Klicken Sie auf die blaue Schaltfläche _Parameter hinzufügen_.
4. Geben Sie im daraufhin angezeigten Bereich für den neuen Parameter Folgendes ein:
    1. Parametername: `Environment`
    2. Parametertyp: `Drop down`
    3. Erforderlich: `checked`
    4. `multiple selection` zulassen: `unchecked`
    5. Daten abrufen aus: `JSON`
5. Fügen Sie im Textblock der JSON-Eingabe diesen JSON-Codeausschnitt ein:
    ```json
    [
        { "value":"dev", "label":"Development" },
        { "value":"ppe", "label":"Pre-production" },
        { "value":"prod", "label":"Production", "selected":true }
    ]
    ```
6. Klicken Sie auf die blaue Schaltfläche `Update`.
7. Wählen Sie auf der Symbolleiste die Option „Speichern“ aus, um den Parameter zu erstellen.
8. Der Environment-Parameter ist nun ein Dropdown mit den drei Werten.

    ![Abbildung zum Erstellen eines statischen Dropdowns](./media/workbook-dropdowns/dropdown-create.png)

## <a name="creating-a-static-dropdown-with-groups-of-items"></a>Erstellen eines statischen Dropdowns mit Gruppen von Elementen
Wenn Ihr Abfrageergebnis/JSON-Code ein Feld „group“ enthält, werden im Dropdown Gruppen von Werten angezeigt. Orientieren Sie sich am obigen Beispiel, verwenden Sie jedoch stattdessen den folgenden JSON-Code:
```json
[
    { "value":"dev", "label":"Development", "group":"Development" },
    { "value":"dev-cloud", "label":"Development (Cloud)", "group":"Development" },
    { "value":"ppe", "label":"Pre-production", "group":"Test" },
    { "value":"ppe-test", "label":"Pre-production (Test)", "group":"Test" },
    { "value":"prod1", "label":"Prod 1", "selected":true, "group":"Production" },
    { "value":"prod2", "label":"Prod 2", "group":"Production" }
]
```
    ![Image showing an example of a grouped dropdown](./media/workbook-dropdowns/grouped-dropDown.png)


## <a name="creating-a-dynamic-drop-down-parameter"></a>Erstellen eines dynamischen Dropdownparameters
1. Beginnen Sie mit einer leeren Arbeitsmappe im Bearbeitungsmodus.
2. Wählen Sie unter den Links in der Arbeitsmappe die Option _Parameter hinzufügen_ aus.
3. Klicken Sie auf die blaue Schaltfläche _Parameter hinzufügen_.
4. Geben Sie im daraufhin angezeigten Bereich für den neuen Parameter Folgendes ein:
    1. Parametername: `RequestName`
    2. Parametertyp: `Drop down`
    3. Erforderlich: `checked`
    4. `multiple selection` zulassen: `unchecked`
    5. Daten abrufen aus: `Query`
5. Fügen Sie im Textblock der JSON-Eingabe diesen JSON-Codeausschnitt ein:

    ```kusto
        requests
        | summarize by name
        | order by name asc
    ```
1. Klicken Sie auf die blaue Schaltfläche `Run Query`.
2. Wählen Sie auf der Symbolleiste die Option „Speichern“ aus, um den Parameter zu erstellen.
3. Der RequestName-Parameter wird zum Dropdown mit den Namen aller Anforderungen in der App.

    ![Abbildung zum Erstellen eines dynamischen Dropdowns](./media/workbook-dropdowns/dropdown-dynamic.png)

## <a name="referencing-drop-down-parameter"></a>Verweisen auf Dropdown-Parameter
### <a name="in-kql"></a>In einer KQL
1. Fügen Sie der Arbeitsmappe ein Abfragesteuerelement hinzu, und wählen Sie eine Application Insights-Ressource aus.
2. Geben Sie im KQL-Editor diesen Codeausschnitt ein:

    ```kusto
        requests
        | where name == '{RequestName}'
        | summarize Requests = count() by bin(timestamp, 1h)

    ```
3. Dies wird bei der Auswertung der Abfrage wie folgt erweitert:

    ```kusto
        requests
        | where name == 'GET Home/Index'
        | summarize Requests = count() by bin(timestamp, 1h)
    ```

4. Führen Sie die Abfrage aus, um die Ergebnisse anzuzeigen. Optional können Sie die Ergebnisse auch als Diagramm rendern lassen.

    ![Abbildung eines Dropdowns, auf das in einer KQL verwiesen wird](./media/workbook-dropdowns/dropdown-reference.png)


## <a name="parameter-value-label-selection-and-group"></a>Parameterwert, Bezeichnung, Auswahl und Gruppe
Die im obigen dynamischen Dropdownparameter verwendete Abfrage gibt einfach eine Liste von Werten zurück, die im Dropdown ordnungsgemäß gerendert werden. Was jedoch, wenn Sie einen anderen Anzeigenamen wünschen oder wenn einer dieser Werte ausgewählt sein soll? Dropdownparameter ermöglichen dies über die Spalten für Wert, Bezeichnung, Auswahl und Gruppe.

Im unten stehenden Beispiel wird veranschaulicht, wie eine Liste von Application Insights-Abhängigkeiten abgerufen wird, deren Anzeigenamen mit einem Emoji formatiert sind, wobei die erste Abhängigkeit ausgewählt ist und die Ergebnisse nach Vorgangsnamen gruppiert sind.

```kusto
dependencies
| summarize by operation_Name, name
| where name !contains ('.')
| order by name asc
| serialize Rank = row_number()
| project value = name, label = strcat('🌐 ', name), selected = iff(Rank == 1, true, false), group = operation_Name
```
    ![Image showing a drop-down parameter using value, label, selection and group options](./media/workbook-dropdowns/dropdown-more-options.png)


## <a name="drop-down-parameter-options"></a>Optionen für Dropdownparameter
| Parameter | Erklärung | Beispiel |
| ------------- |:-------------|:-------------|
| `{DependencyName}` | Ausgewählter Wert | GET fabrikamaccount |
| `{DependencyName:label}` | Ausgewählte Bezeichnung | 🌐 GET fabrikamaccount |
| `{DependencyName:value}` | Ausgewählter Wert | GET fabrikamaccount |

## <a name="multiple-selection"></a>Mehrfachauswahl
In den bisherigen Beispielen wurde der Parameter explizit so festgelegt, dass nur ein Wert im Dropdown ausgewählt wird. Dropdownparameter unterstützen auch `multiple selection`; hierzu muss einfach nur die Option `Allow multiple selection` aktiviert werden. 

Benutzer können über die Einstellungen `delimiter` und `quote with` auch das Format der Ergebnisse angeben. In der Standardeinstellung werden die Werte lediglich als Sammlung in diesem Format zurückgegeben: 'a', 'b', 'c'. Zudem gibt es die Möglichkeit, die Anzahl der Auswahlmöglichkeiten einzuschränken.

Die KQL, die auf den Parameter verweist, muss so geändert werden, dass sie das Format des Ergebnisses verarbeitet. Die gängigste Möglichkeit, dies zu erreichen, ist über den `in`-Operator.

```kusto
dependencies
| where name in ({DependencyName})
| summarize Requests = count() by bin(timestamp, 1h), name
```

Im Folgenden finden Sie ein Beispiel für ein Mehrfachauswahl-Dropdown:

![Abbildung eines Mehrfachauswahl-Dropdownparameters](./media/workbook-dropdowns/dropdown-multiselect.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr](workbooks-visualizations.md) über die vielen umfassenden Visualisierungsoptionen für Arbeitsmappen.
* [Steuern](workbooks-access-control.md) Sie den Zugriff auf Ihre Arbeitsmappenressourcen, und geben Sie diese frei.
