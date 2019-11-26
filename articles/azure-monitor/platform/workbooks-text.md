---
title: Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen und Textparametern | Microsoft-Dokumentation
description: Vereinfachen der komplexen Berichterstellung mit vordefinierten und benutzerdefiniert parametrisierten Arbeitsmappen. Erfahren Sie mehr über Textparameter in Arbeitsmappen.
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.service: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: ee3e24444b87c461841b591176774d4e945e4fcc
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164204"
---
# <a name="workbook-text-parameters"></a>Textparameter in Arbeitsmappen

Textfeldparameter bieten eine einfache Möglichkeit, Texteingaben von Arbeitsmappenbenutzern zu erfassen. Sie werden verwendet, wenn das Erfassen von Eingaben über ein Dropdown nicht praktikabel ist (z. B. einen beliebigen Schwellenwert oder generische Filter). Mit Arbeitsmappen können Autoren den Standardwert des Textfelds über eine Abfrage erhalten. Dies ermöglicht interessante Szenarien wie das Festlegen des Standardschwellenwerts basierend auf dem p95-Wert der Metrik.

Textfelder werden häufig als interne Variablen genutzt, die von anderen Steuerelementen der Arbeitsmappe verwendet werden. Dies erfolgt durch Verwendung einer Abfrage für Standardwerte, wobei das Eingabesteuerelement im Lesemodus als unsichtbar festgelegt wird. Ein Benutzer möchte beispielsweise einen Schwellenwert aus einer Formel (und nicht von einem Benutzer) beziehen und diesen Schwellenwert in nachfolgenden Abfragen verwenden.

## <a name="creating-a-text-parameter"></a>Erstellen eines Textparameters
1. Beginnen Sie mit einer leeren Arbeitsmappe im Bearbeitungsmodus.
2. Wählen Sie unter den Links in der Arbeitsmappe die Option _Parameter hinzufügen_ aus.
3. Klicken Sie auf die blaue Schaltfläche _Parameter hinzufügen_.
4. Geben Sie im daraufhin angezeigten Bereich für den neuen Parameter Folgendes ein:
    1. Parametername: `SlowRequestThreshold`
    2. Parametertyp: `Text`
    3. Erforderlich: `checked`
    4. Standardwert aus Abfrage abrufen: `unchecked`
5. Wählen Sie auf der Symbolleiste die Option „Speichern“ aus, um den Parameter zu erstellen.

    ![Abbildung zum Erstellen eines Textparameters](./media/workbooks-text/text-create.png)

So sieht die Arbeitsmappe im Lesemodus aus.

![Abbildung eines Textparameters im Lesemodus](./media/workbooks-text/text-readmode.png)

## <a name="referencing-a-text-parameter"></a>Verweisen auf einen Textparameter
1. Fügen Sie der Arbeitsmappe ein Abfragesteuerelement hin, indem Sie auf den blauen Link `Add query` klicken und eine Application Insights-Ressource auswählen.
2. Fügen Sie im KQL-Feld den folgenden Codeausschnitt hinzu:
    ```kusto
    requests
    | summarize AllRequests = count(), SlowRequests = countif(duration >= {SlowRequestThreshold}) by name
    | extend SlowRequestPercent = 100.0 * SlowRequests / AllRequests
    | order by SlowRequests desc
    ```
3. Wenn Sie den Textparameter mit dem Wert 500 zusammen mit dem Abfragesteuerelement verwenden, führen Sie effektiv die unten stehende Abfrage aus:
    ```kusto
    requests
    | summarize AllRequests = count(), SlowRequests = countif(duration >= 500) by name
    | extend SlowRequestPercent = 100.0 * SlowRequests / AllRequests
    | order by SlowRequests desc
    ```
4. Ausführen einer Abfrage, um die Ergebnisse anzuzeigen

    ![Abbildung eines Textparameters, auf den in der KQL verwiesen wird](./media/workbooks-text/text-reference.png)


## <a name="setting-default-values"></a>Festlegen von Standardwerte
1. Beginnen Sie mit einer leeren Arbeitsmappe im Bearbeitungsmodus.
2. Wählen Sie unter den Links in der Arbeitsmappe die Option _Parameter hinzufügen_ aus.
3. Klicken Sie auf die blaue Schaltfläche _Parameter hinzufügen_.
4. Geben Sie im daraufhin angezeigten Bereich für den neuen Parameter Folgendes ein:
    1. Parametername: `SlowRequestThreshold`
    2. Parametertyp: `Text`
    3. Erforderlich: `checked`
    4. Standardwert aus Abfrage abrufen: `checked`
5. Fügen Sie im KQL-Feld den folgenden Codeausschnitt hinzu:
    ```kusto
    requests
    | summarize round(percentile(duration, 95), 2)
    ```
    Mit dieser Abfrage wird der Standardwert des Textfelds auf die Dauer des 95. Perzentils für alle Anforderungen in der App festgelegt.
6. Ausführen einer Abfrage, um die Ergebnisse anzuzeigen
7. Wählen Sie auf der Symbolleiste die Option „Speichern“ aus, um den Parameter zu erstellen.

    ![Abbildung eines Textparameters mit Standardwert aus der KQL](./media/workbooks-text/text-default-value.png)

> [!NOTE]
> In diesem Beispiel werden Application Insights-Daten abgefragt. Der Ansatz kann jedoch für jede auf Protokollen basierende Datenquellen wie Log Analytics, Azure Resource Graph usw. verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr](workbooks-visualizations.md) über die vielen umfassenden Visualisierungsoptionen für Arbeitsmappen.
* [Steuern](workbooks-access-control.md) Sie den Zugriff auf Ihre Arbeitsmappenressourcen, und geben Sie diese frei.
