---
title: Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen | Erstellen von Parametern | Microsoft-Dokumentation
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
ms.openlocfilehash: c221abf423a21d424bd5198696a61d7ec83521e9
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164388"
---
# <a name="workbook-parameters"></a>Arbeitsmappenparameter

Mithilfe von Parametern können Arbeitsmappenautoren Eingaben von den Consumern erfassen und in anderen Teilen der Arbeitsmappe darauf verweisen, normalerweise um das Resultset einzugrenzen oder das richtige visuelle Element festzulegen. Dies ist eine wichtige Funktion, die es Autoren ermöglicht, interaktive Berichte und Benutzeroberflächen zu erstellen. 

Mithilfe von Arbeitsmappen können Sie steuern, wie die Steuerelemente für Parameter den Consumern angezeigt werden: Textfeld oder Dropdownliste, Einzel- oder Mehrfachauswahl, Werte aus Text, JSON, KQL oder Azure Resource Graph usw.  

Unterstützte Parametertypen sind unter anderem:
* [Zeit](workbooks-time.md): Ermöglicht einem Benutzer die Auswahl vordefinierter Zeitbereiche oder eines benutzerdefinierten Bereichs.
* [Dropdown](workbooks-dropdowns.md): Ermöglicht einem Benutzer die Auswahl eines Werts oder einer Gruppe von Werten.
* [Text](workbooks-text.md): Ermöglicht einem Benutzer die Eingabe von beliebigem Text.
* [Resource](workbooks-resources.md): Ermöglicht einem Benutzer die Auswahl einer oder mehrerer Azure-Ressourcen.
* [Abonnement](workbooks-resources.md): Ermöglicht einem Benutzer die Auswahl einer oder mehrerer Azure-Abonnementressourcen.
* Ressourcentyp: Ermöglicht einem Benutzer die Auswahl eines oder mehrerer Werte für den Azure-Ressourcentyp.
* Standort: Ermöglicht einem Benutzer die Auswahl eines oder mehrerer Werte für den Azure-Standort.

Auf diese Parameterwerte kann in anderen Teilen von Arbeitsmappen entweder über Bindungen oder Werterweiterungen verwiesen werden.

## <a name="creating-a-parameter"></a>Erstellen eines Parameters
1. Beginnen Sie mit einer leeren Arbeitsmappe im Bearbeitungsmodus.
2. Wählen Sie unter den Links in der Arbeitsmappe die Option _Parameter hinzufügen_ aus.
3. Klicken Sie auf die blaue Schaltfläche _Parameter hinzufügen_.
4. Geben Sie im daraufhin angezeigten Bereich für den neuen Parameter Folgendes ein:
    1. Parametername: `TimeRange` *(Beachten Sie, dass __Parameternamen__ **keine** Leerzeichen oder Sonderzeichen enthalten dürfen.)*
    2. Anzeigename: `Time Range` *(__Anzeigenamen__ können jedoch Leerzeichen, Sonderzeichen, Emojis usw. enthalten.)*
    2. Parametertyp: `Time range picker`
    3. Erforderlich: `checked`
    4. Verfügbare Zeitbereiche: „Letzte Stunde“, „Letzte 12 Stunden“, „Letzte 24 Stunden“, „Letzte 48 Stunden“, „Letzte 3 Tage“, „Letzte 7 Tage“ und „Auswahl benutzerdefinierter Zeitbereiche zulassen“
5. Wählen Sie auf der Symbolleiste die Option „Speichern“ aus, um den Parameter zu erstellen.

   ![Abbildung zum Erstellen eines Zeitbereichsparameters](./media/workbooks-parameters/time-settings.png)

So sieht die Arbeitsmappe im Lesemodus im Stil „Ovale Steuerelemente“ aus.

   ![Abbildung eines Zeitbereichsparameters im Lesemodus](./media/workbooks-parameters/parameters-time.png)

## <a name="referencing-a-parameter"></a>Verweisen auf einen Parameter
### <a name="via-bindings"></a>Über Bindungen
1. Fügen Sie der Arbeitsmappe ein Abfragesteuerelement hinzu, und wählen Sie eine Application Insights-Ressource aus.
2. Öffnen Sie das Dropdownmenü _Zeitbereich_, und wählen Sie unten im Abschnitt „Parameter“ die Option `Time Range` aus.
3. Dadurch wird der Zeitbereichsparameter an den Zeitbereich des Diagramms gebunden. Der Zeitbereich der Beispielabfrage ist nun auf „Letzte 24 Stunden“ festgelegt.
4. Führen Sie die Abfrage aus, um die Ergebnisse anzuzeigen.

    ![Abbildung eines Zeitbereichsparameters, auf den über Bindungen verwiesen wird](./media/workbooks-parameters/time-binding.png)

### <a name="in-kql"></a>In einer KQL
1. Fügen Sie der Arbeitsmappe ein Abfragesteuerelement hinzu, und wählen Sie eine Application Insights-Ressource aus.
2. Geben Sie in der KQL einen Zeitbereichsfilter mit dem folgenden Parameter an: `| where timestamp {TimeRange}`
3. Dadurch wird die Abfrageauswertungszeit auf `| where timestamp > ago(1d)` erweitert, den Zeitbereichswert des Parameters.
4. Führen Sie die Abfrage aus, um die Ergebnisse anzuzeigen.

    ![Abbildung eines Zeitbereichs, auf den in einer KQL verwiesen wird](./media/workbooks-parameters/time-in-code.png)

### <a name="in-text"></a>In Text 
1. Fügen Sie der Arbeitsmappe ein Textsteuerelement hinzu.
2. Geben Sie im Markdown `The chosen time range is {TimeRange:label}` ein.
3. Wählen Sie _Bearbeitung abgeschlossen_ aus.
4. Im Textsteuerelement wird Text angezeigt: _The chosen time range is Last 24 hours_ (Der ausgewählte Zeitbereich ist „Letzte 24 Stunden“)

## <a name="parameter-options"></a>Parameteroptionen:
Im Abschnitt _In Text_ wurde die Bezeichnung (`label`) des Parameters anstelle seines Werts verwendet. Parameter bieten je nach Typ verschiedene solcher Optionen. So sind beispielsweise bei der Zeitbereichsauswahl die Optionen für Wert, Bezeichnung, Abfrage, Start, Ende und Intervall zulässig.

Verwenden Sie den Abschnitt `Previews` im Bereich _Parameter bearbeiten_, um die Erweiterungsoptionen für den jeweiligen Parameter anzuzeigen:

![Abbildung der Optionen eines Zeitbereichsparameters](./media/workbooks-parameters/time-previews.png)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr](workbooks-visualizations.md) über die vielen umfassenden Visualisierungsoptionen für Arbeitsmappen.
* [Steuern](workbooks-access-control.md) Sie den Zugriff auf Ihre Arbeitsmappenressourcen, und geben Sie diese frei.
