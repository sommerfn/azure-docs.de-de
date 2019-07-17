---
title: Was ist die Freihanderkennung? – Freihanderkennungs-API
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie die Freihanderkennung in Anwendungen, Websites, Tools und andere Lösungen integrieren, um Freihandschriftdaten zu erkennen und als Eingabe zu nutzen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: overview
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 95121523c510e72894086740ad340bdfd33f3c32
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721402"
---
# <a name="what-is-the-ink-recognizer-api"></a>Was ist die Freihanderkennungs-API?


Der Cognitive Service Freihanderkennung bietet eine cloudbasierte-REST-API zum Analysieren und Erkennen von Freihandschriftinhalten. Im Gegensatz zu Diensten, die optische Zeichenerkennung (OCR) verwenden, erfordert die API Freihandschriftdaten als Eingabe. Freihandschriftdaten sind zeitlich geordnete Gruppen von 2D-Punkten (X,Y-Koordinaten), die die Bewegung von Eingabetools wie digitalen Stiften oder Fingern darstellen. Anschließend erkennt sie Formen und handschriftlichen Inhalt aus der Eingabe und gibt eine JSON-Antwort zurück, die alle erkannten Entitäten enthält.

![Ein Flussdiagramm, das das Senden einer Freihandschrifteingabe an die API beschreibt](media/ink-recognizer-pen-graph.png)

## <a name="features"></a>Features

Mit der Freihanderkennungs-API können Sie handschriftlichen Inhalt in Ihren Anwendungen leicht erkennen. 

|Feature  |BESCHREIBUNG  |
|---------|---------|
| Handschrifterkennung | Erkennen von handschriftlichem Inhalt in 63 [Kernsprachen und Gebietsschemas](language-support.md). | 
| Layouterkennung | Rufen Sie strukturelle Informationen über den Inhalt von Freihandschrifteingaben ab. Schlüsseln Sie den Inhalt in Schreibregionen, Absätze, Zeilen, Wörter, Listen mit Aufzählungszeichen auf. Ihre Anwendungen können dann mit den Layoutinformationen zusätzliche Funktionen wie automatische Listenformatierung und Formausrichtung erstellen. |
| Formerkennung | Erkennen der beim Erstellen von Notizen am häufigsten verwendeten [geometrischen Formen](concepts/send-ink-data.md#shapes-recognized-by-the-ink-recognizer-api). |
| Kombinierte Formen- und Texterkennung | Erkennen, welche Striche zu Formen und welche zu handgeschriebenem Inhalt gehören und deren separate Klassifizierung.|

## <a name="workflow"></a>Workflow

Die Freihanderkennungs-API ist ein RESTful-Webdienst und kann somit problemlos in jeder Programmiersprache aufgerufen werden, die HTTP-Anforderungen beherrscht und JSON analysieren kann.

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

Nach der Registrierung:

1. [Formatieren](concepts/send-ink-data.md#sending-ink-data) Sie Ihre Freihandschriftdaten in gültigem JSON-Code.
1. Senden Sie eine Anforderung mit Ihren Daten an die Freihanderkennungs-API.
1. Analysieren Sie die zurückgegebene JSON-Nachricht, um die API-Antwort zu verarbeiten.

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie einen Schnellstart in den folgenden Sprachen aus, um mit dem Senden von Aufrufen an die Freihanderkennungs-API zu beginnen.
* [C#](quickstarts/csharp.md)
* [Java](quickstarts/java.md)
* [JavaScript](quickstarts/csharp.md)

Um zu sehen, wie die Freihanderkennungs-API in einer Freihandschriftinhalte-App funktioniert, sehen Sie sich die folgenden Beispielanwendungen auf GitHub an:
* [C# und universelle Windows-Plattform (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# und Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript-Webbrowser-App](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Mobile Java- und Android-App](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Mobile Swift- und iOS-App](https://go.microsoft.com/fwlink/?linkid=2089805)
