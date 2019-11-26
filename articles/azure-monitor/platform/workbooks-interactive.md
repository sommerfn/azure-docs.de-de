---
title: Erstellen interaktiver Berichte mit benutzerdefinierten Parametern in Azure Monitor-Arbeitsmappen | Microsoft-Dokumentation
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
ms.openlocfilehash: eeb6eb5d8b3ad6498ff90a9afe1fa4f2c18d30e5
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164448"
---
# <a name="interactive-workbooks"></a>Interaktive Arbeitsmappen

Mit Arbeitsmappen können Autoren interaktive Berichte und Erfahrungen für Ihre Kunden erstellen. Verschiedene Formen der Interaktivität werden unterstützt.

## <a name="parameter-changes"></a>Parameteränderungen
Wenn der Benutzer einer Arbeitsmappe einen Parameter aktualisiert, wird jedes Steuerelement, das diesen Parameter verwendet, automatisch aktualisiert und neu gezeichnet, um den neuen Zustand widerzuspiegeln. Auf diese Weise unterstützen die meisten Berichte im Azure-Portal die Interaktivität. Arbeitsmappen ermöglichen dies auf unkomplizierte Weise, und der Aufwand für Benutzer ist dabei minimal.

Erfahren Sie mehr über [Parameter in Arbeitsmappen](workbooks-parameters.md)

## <a name="grid-row-clicks"></a>Rasterzeilen-Klicks
Arbeitsmappen ermöglichen Benutzern das Entwickeln von Szenarien, bei denen durch Klicken auf eine Zeile in einem Raster nachfolgende Diagramme entsprechend dem Inhalt der Zeile aktualisiert werden. 

Beispielsweise kann ein Benutzer über ein Raster verfügen, in dem eine Liste von Anforderungen und einige Statistiken wie z. B. Fehleranzahlen aufgeführt werden. Dieses kann so eingerichtet werden, dass durch einen Klick auf eine Zeile, die einer Anforderung entspricht, darunter liegende detaillierte Diagramme aktualisiert werden, sodass ein Filter für die konkrete Anforderung gegeben ist.

### <a name="setting-up-interactivity-on-grid-row-click"></a>Einrichten der Interaktivität für Rasterzeilen-Klicks
1. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement _Bearbeiten_ klicken.
2. Klicken Sie auf den Link _Abfrage hinzufügen_, um der Arbeitsmappe ein Protokollabfrage-Steuerelement hinzuzufügen. 
3. Wählen Sie für den Abfragetyp _Protokoll_ aus, wählen Sie den Ressourcentyp (z. B. Application Insights) sowie die Zielressourcen aus.
4. Geben Sie über den Abfrage-Editor die KQL für Ihre Analyse ein.
    ```kusto
    requests
    | summarize AllRequests = count(), FailedRequests = countif(success == false) by Request = name
    | order by AllRequests desc    
    ```
5. Führen Sie `Run query` aus, um die Ergebnisse anzuzeigen.
6. Klicken Sie in der Fußzeile der Abfrage auf das Symbol _Erweiterte Einstellungen_ (das Zahnradsymbol). Dadurch wird der Bereich „Erweiterte Einstellungen“ geöffnet. 
7. Aktivieren Sie diese Einstellung: `When an item is selected, export a parameter`
    1. Zu exportierendes Feld: `Request`
    2. Parametername: `SelectedRequest`
    3. Standardwert: `All requests`
    
    ![Abbildung des erweiterten Editors mit Einstellungen zum Exportieren von Feldern als Parameter](./media/workbooks-interactive/advanced-settings.png)

8. Klicken Sie auf `Done Editing`.
9. Fügen Sie mit den Schritten 2 und 3 ein weiteres Abfragesteuerelement hinzu.
10. Geben Sie über den Abfrage-Editor die KQL für Ihre Analyse ein.
    ```kusto
    requests
    | where name == '{SelectedRequest}' or 'All Requests' == '{SelectedRequest}'
    | summarize ['{SelectedRequest}'] = count() by bin(timestamp, 1h)
    ```
11. Führen Sie `Run query` aus, um die Ergebnisse anzuzeigen.
12. Ändern Sie _Visualisierung_ in `Area chart`
12. Klicken Sie auf eine Zeile im ersten Raster. Sie stellen fest, dass im darunterliegenden Flächendiagramm zur ausgewählten Anforderung gefiltert wird.

Der erhaltene Bericht sieht im Bearbeitungsmodus wie folgt aus:

![Abbildung, die das Erstellen einer interaktiven Erfahrung mit Rasterzeilen-Klicks veranschaulicht](./media/workbooks-interactive//grid-click-create.png)

Die Abbildung unten zeigt einen ausführlicheren interaktiven Bericht im Lesemodus, der auf denselben Prinzipien basiert. Im Bericht werden mithilfe von Raster-Klicks Parameter exportiert; diese wiederum werden in zwei Diagrammen und einem Textblock verwendet.

![Abbildung, die das Erstellen einer interaktiven Erfahrung mit Rasterzeilen-Klicks veranschaulicht](./media/workbooks-interactive/grid-click-read-mode.png)

### <a name="exporting-the-contents-of-an-entire-row"></a>Exportieren des Inhalts einer gesamten Zeile
Gelegentlich ist es wünschenswert, den gesamten Inhalt der ausgewählten Zeile und nicht nur eine bestimmte Spalte zu exportieren. Legen Sie in derartigen Fällen die `Field to export`-Eigenschaft im obigen Schritt 7.1 nicht fest. Arbeitsmappen exportieren den gesamten Zeileninhalt als JSON-Code in den Parameter. 

Analysieren Sie im verweisenden KQL-Steuerelement mithilfe der `todynamic`-Funktion den JSON-Code und rufen Sie die einzelnen Spalten auf.

 ## <a name="grid-cell-clicks"></a>Rasterzellen-Klicks
In Arbeitsmappen können Autoren Interaktivität über eine spezielle Art von Rasterspalten-Renderer einrichten. Dieser wird als `link renderer` bezeichnet. Ein Linkrenderer konvertiert eine Rasterzelle gemäß dem Inhalt der Zelle in einen Link. Arbeitsmappen unterstützen viele Arten von Linkrenderern, u. a. Renderer, mit denen Blätter mit einer Ressourcenübersicht, Eigenschaftenbehälter-Viewer, App Insights-Suche, Nutzung, Ablaufverfolgung von Transaktionen usw. geöffnet werden können.

### <a name="setting-up-interactivity-using-grid-cell-clicks"></a>Einrichten der Interaktivität mithilfe von Rasterzellen-Klicks
1. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement _Bearbeiten_ klicken.
2. Klicken Sie auf den Link _Abfrage hinzufügen_, um der Arbeitsmappe ein Protokollabfrage-Steuerelement hinzuzufügen. 
3. Wählen Sie für den Abfragetyp _Protokoll_ aus, wählen Sie den Ressourcentyp (z. B. Application Insights) sowie die Zielressourcen aus.
4. Geben Sie über den Abfrage-Editor die KQL für Ihre Analyse ein.
    ```kusto
    requests
    | summarize Count = count(), Sample = any(pack_all()) by Request = name
    | order by Count desc
    ```
5. Führen Sie `Run query` aus, um die Ergebnisse anzuzeigen.
6. Klicken Sie auf _Spalteneinstellungen_, um den Bereich mit Einstellungen zu öffnen.
7. Legen Sie im Abschnitt _Spalten_ Folgendes fest:
    1. _Beispiel_ – Säulenrenderer: `Link`, Zu öffnende Ansicht: `Cell Details`, Linkbezeichnung: `Sample`
    2. _Anzahl_ – Säulenrenderer: `Bar`, Farbpalette: `Blue`, Minimalwert: `0`
    3. _Anforderung_ – Säulenrenderer: `Automatic`
    4. Klicken Sie auf _Speichern und schließen_, um die Änderungen anzuwenden.
8. Klicken Sie auf einen der `Sample`-Links im Raster. Dadurch wird ein Eigenschaftenbereich mit den Details zur getesteten Anforderung geöffnet.

    ![Abbildung, die das Erstellen einer interaktiven Erfahrung mit Rasterzellen-Klicks veranschaulicht](./media/workbooks-interactive/grid-cell-click-create.png)

### <a name="link-renderer-actions"></a>Aktionen des Linkrenderers
| Aktion des Links | Aktion beim Klicken |
|:------------- |:-------------|
| `Generic Details` | Zeigt die Zeilenwerte in einem Kontextblatt für das Eigenschaftenraster an. |
| `Cell Details` | Zeigt den Zellenwert in einem Kontextblatt für das Eigenschaftenraster an. Nützlich, wenn die Zelle einen dynamischen Typ mit Informationen enthält (z. B. JSON mit Anforderungseigenschaften wie Standort, Rolleninstanz usw.). |
| `Cell Details` | Zeigt den Zellenwert in einem Kontextblatt für das Eigenschaftenraster an. Nützlich, wenn die Zelle einen dynamischen Typ mit Informationen enthält (z. B. JSON mit Anforderungseigenschaften wie Standort, Rolleninstanz usw.). |
| `Custom Event Details` | Öffnet die Application Insights-Suchdetails mit der ID des benutzerdefinierten Ereignisses (itemId) in der Zelle. |
| `* Details` | Ähnlich wie „Benutzerdefinierte Ereignisdetails“, mit Ausnahme von Abhängigkeiten, Ausnahmen, Seitenaufrufen, Anforderungen und Ablaufverfolgungen. |
| `Custom Event User Flows` | Öffnet die Benutzerflows-Erfahrung von Application Insights, pivotiert für den Namen des benutzerdefinierten Ereignisses in der Zelle. |
| `* User Flows` | Ähnlich wie „Benutzerdefinierte Ereignisbenutzerflows“, mit Ausnahme von Ausnahmen, Seitenaufrufen und Anforderungen. |
| `User Timeline` | Öffnet die Benutzerzeitachse mit der Benutzer-ID (user_Id) in der Zelle. |
| `Session Timeline` | Öffnet die Sucherfahrung von Application Insights für den Wert in der Zelle (z. B. Suchen nach dem Text „abc“, wobei „abc“ der Wert in der Zelle ist). |
| `Resource overview` | Öffnet die Übersicht zur Ressource im Portal gemäß dem Wert der Ressourcen-ID in der Zelle. |

## <a name="conditional-visibility"></a>Bedingte Sichtbarkeit
Mithilfe der Arbeitsmappe können Benutzer bestimmte Steuerelemente auf der Grundlage der Parameterwerte einblenden oder ausblenden. Dadurch können Berichte je nach Eingaben und Telemetriestatus unterschiedlich aussehen. Ein Beispiel: Für Kunden wird eine Übersicht angezeigt, wenn alles in Ordnung ist. Sollte ein Fehler vorliegen, werden die kompletten Details angezeigt.

### <a name="setting-up-interactivity-using-conditional-visibility"></a>Einrichten der Interaktivität mithilfe der bedingten Sichtbarkeit
1. Führen Sie die Schritte im Abschnitt `Setting up interactivity on grid row click` aus, um zwei interaktive Steuerelemente einzurichten.
2. Fügen Sie einen neuen Parameter am Anfang hinzu:
    1. Name: `ShowDetails`
    2. Parametertyp: `Drop down`
    3. Erforderlich: `checked`
    4. Daten abrufen aus: `JSON`
    5. JSON-Eingabe: `["Yes", "No"]`
    6. Speichern Sie die Änderungen, um einen Commit auszuführen.
3. Legen Sie den Parameterwert auf `Yes` fest.
4. Klicken Sie im Abfragesteuerelement mit dem Flächendiagramm auf das Symbol _Erweiterte Einstellungen_ (das Zahnradsymbol).
5. Aktivieren Sie die Einstellung `Make this item conditionally visible`.
    1. Dieses Element ist sichtbar, wenn der Wert des `ShowDetails`-Parameters `equals` `Yes`
6. Klicken Sie auf _Bearbeitung abgeschlossen_, um einen Commit der Änderungen auszuführen.
7. Klicken Sie auf der Symbolleiste der Arbeitsmappe auf _Bearbeitung abgeschlossen_, um in den Lesemodus zu wechseln.
8. Ändern Sie den Wert des Parameters `ShowDetails` in `No`. Sie stellen fest, dass das Diagramm unten ausgeblendet wird.

In der folgenden Abbildung ist das Diagramm eingeblendet; dabei ist `ShowDetails` auf `Yes` festgelegt.

![Abbildung zum Veranschaulichen der bedingten Sichtbarkeit, wobei das Diagramm eingeblendet ist](./media/workbooks-interactive/conditional-visibility.png)

In der folgenden Abbildung ist das Diagramm ausgeblendet; dabei ist `ShowDetails` auf `No` festgelegt.

![Abbildung zum Veranschaulichen der bedingten Sichtbarkeit, wobei das Diagramm ausgeblendet ist](./media/workbooks-interactive/conditional-invisible.png)

## <a name="next-steps"></a>Nächste Schritte


* [Erfahren Sie mehr](workbooks-visualizations.md) über die vielen umfassenden Visualisierungsoptionen für Arbeitsmappen.
* [Steuern](workbooks-access-control.md) Sie den Zugriff auf Ihre Arbeitsmappenressourcen, und geben Sie diese frei.
