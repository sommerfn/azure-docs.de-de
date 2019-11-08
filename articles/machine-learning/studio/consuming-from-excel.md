---
title: Nutzen des Webdiensts in Excel
titleSuffix: ML Studio (classic) Azure
description: Mit Azure Machine Learning Studio (klassisch) können Webdienste auf einfache Weise direkt von Excel aus aufgerufen werden, ohne einen Code schreiben zu müssen.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/01/2018
ms.openlocfilehash: aa1e457cf401e74ffb8a7a02b5eba5f4c4f0c334
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73621848"
---
# <a name="consuming-an-azure-machine-learning-studio-classic-web-service-from-excel"></a>Nutzen eines (klassischen) Azure Machine Learning Studio-Webdiensts aus Excel

 Mit Azure Machine Learning Studio (klassisch) können Webdienste auf einfache Weise direkt von Excel aus aufgerufen werden, ohne einen Code schreiben zu müssen.

Wenn Sie Excel 2013 (oder höher) oder Excel Online verwenden, empfehlen wir die Verwendung des [Excel-Add-Ins](excel-add-in-for-web-services.md).



## <a name="steps"></a>Schritte
Veröffentlichen eines Webdiensts. [Tutorial 3: Bereitstellen eines Kreditrisikomodells](tutorial-part3-credit-risk-deploy.md) erläutert diesen Vorgang. Die Excel-Arbeitsmappenfunktion wird derzeit nur für Antwort-/Anfrage-Dienste unterstützt, die eine einzelne Ausgabe aufweisen (d. h. eine einzelne Bewertungsbezeichnung). 

Wenn Sie einen Webdienst haben, klicken Sie auf den Bereich **WEB SERVICES** der linken Seite in Studio und wählen Sie den aus Excel zu verwendenden Webdienst aus.

**Klassischer Webdienst**

1. Auf der Registerkarte **DASHBOARD** für den Webdienst befindet sich die Zeile **REQUEST/RESPONSE** für den Antwort-/Anfrage-Dienst. Wenn dieser Dienst eine einzelne Ausgabe hat, sollte sich in dieser Zeile der Link **Download Excel Workbook** befinden.

    ![Herunterladen der Excel-Arbeitsmappe im Webdienstportal von Studio (klassisch)](./media/consuming-from-excel/excellink.png)
2. Klicken Sie auf **Download Excel Workbook**.

**Neuer Webdienst**

1. Wählen Sie im Azure Machine Learning Web Service-Portal **Consume**aus.
2. Klicken Sie auf der Seite „Consume“ im Abschnitt **Web service consumption options** auf das Excel-Symbol.

**Verwenden der Arbeitsmappe**

1. Öffnen Sie die Arbeitsmappe.
2. Es wird eine Sicherheitswarnung angezeigt. Klicken Sie auf die Schaltfläche **Bearbeitung aktivieren**.

    ![Aktivieren des Bearbeitens zum Entfernen der Sicherheitswarnung bezüglich geschützter Ansicht](./media/consuming-from-excel/enableeditting.png)
3. Es wird eine Sicherheitswarnung angezeigt. Klicken Sie auf die Schaltfläche **Inhalt aktivieren** zum Ausführen von Makros im Arbeitsblatt.

    ![Aktivieren des Inhalts zum Verwerfen der Sicherheitswarnung, die Makros deaktiviert](./media/consuming-from-excel/enablecontent.png)
4. Sobald Makros aktiviert sind, wird eine Tabelle generiert. Spalten in Blau sind als Eingabe für den RRS-Webdienst oder als **PARAMETER**erforderlich. Beachten Sie die Ausgaben des RRS-Diensts **PREDICTED VALUES** in Grün. Wenn alle Spalten für eine bestimmte Zeile gefüllt wurden, ruft die Arbeitsmappe automatisch die Bewertungs-API auf und zeigt die bewerteten Ergebnisse an.

    ![Tabelle für Parametereingaben und die resultierenden vorhergesagten Werte](./media/consuming-from-excel/sampletable.png)
5. Um mehr als eine Zeile zu bewerten, geben Sie in der zweiten Zeile Daten ein. Daraufhin werden die Vorhersagewerte erzeugt. Sie können auch gleichzeitig mehrere Zeilen einfügen.

Sie können beliebige Excel-Funktionen (Diagramme, Power Map, bedingte Formatierungen usw.) mit den Vorhersagewerten verwenden, um die Daten zu visualisieren.

## <a name="sharing-your-workbook"></a>Freigeben Ihrer Arbeitsmappe
Damit die Makros funktionieren, muss der API-Schlüssel Teil des Arbeitsblatts sein. Das bedeutet, dass Sie die Arbeitsmappe nur für Entitäten und Personen freigeben sollten, denen Sie vertrauen.

## <a name="automatic-updates"></a>Automatische Aktualisierungen
RRS-Aufrufe werden in diesen beiden Situationen ausgeführt:

1. Beim ersten Mal, wenn in einer Zeile in jedem **PARAMETER**
2. Jedes Mal, wenn einer der **PARAMETER** in einer Zeile, in der alle **PARAMETER** vorhanden sind, geändert wird.