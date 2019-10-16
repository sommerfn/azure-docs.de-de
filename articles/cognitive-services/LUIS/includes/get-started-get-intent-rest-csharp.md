---
title: Abrufen der Absicht mit einem REST-Aufruf in C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: e6ae9590cee3a2ddc3b8e121161fcf84815da28a
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71838552"
---
## <a name="prerequisites"></a>Voraussetzungen

* [Visual Studio Community 2017 Edition](https://visualstudio.microsoft.com/vs/community/)
* Programmiersprache C# (in VS Community 2017 enthalten)
* ID der öffentlichen App: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Abrufen des LUIS-Schlüssels

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-programmatically"></a>Programmgesteuertes Abrufen der Absicht

Verwenden Sie C# zum Abfragen der GET-[API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) des Vorhersageendpunkts, um die gleichen Ergebnisse wie im vorherigen Abschnitt im Browserfenster zu erhalten. 

1. Erstellen Sie eine neue Konsolenanwendung in Visual Studio. 

    ![Erstellen einer neuen Konsolenanwendung in Visual Studio](../media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. Klicken Sie im Visual Studio-Projekt im Projektmappen-Explorer auf **Verweis hinzufügen**, und wählen Sie dann auf der Registerkarte „Assemblys“ den Eintrag **System.Web** aus.

    ![Wählen Sie „Verweis hinzufügen“ und dann „System.Web“ auf der Registerkarte „Assemblys“ aus.](../media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Überschreiben Sie „Program.cs“ mit dem folgenden Code:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. Ersetzen Sie den Wert von `YOUR_KEY` durch Ihren LUIS-Schlüssel.

5. Erstellen Sie die Konsolenanwendung, und führen Sie sie aus. Es wird das gleiche JSON-Ergebnis wie zuvor im Browserfenster angezeigt.

    ![Konsolenfenster mit JSON-Ergebnis aus LUIS](../media/luis-get-started-cs-get-intent/console-turn-on.png)



## <a name="luis-keys"></a>LUIS-Schlüssel

[!INCLUDE [Use authoring key for endpoint](../../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Schließen Sie nach Beendigung dieser Schnellstartanleitung das Visual Studio-Projekt, und entfernen Sie das Projektverzeichnis aus dem Dateisystem. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen von Äußerungen und Trainieren mit C#](../luis-get-started-cs-add-utterance.md)