---
title: Was ist die Personalisierung?
titleSuffix: Azure Cognitive Services
description: Die Personalisierung ist ein cloudbasierter API-Dienst, mit dem Sie die beste Benutzeroberfläche für Ihre Benutzer auswählen und dabei in Echtzeit von deren Verhalten lernen können.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 286a19207236392367b924bea7e26e90fd0db8d5
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253460"
---
# <a name="what-is-personalizer"></a>Was ist die Personalisierung?

Die Azure-Personalisierung ist ein cloudbasierter API-Dienst, mit dem Sie die beste Benutzeroberfläche für Ihre Benutzer auswählen und dabei in Echtzeit von deren Verhalten lernen können.

* Stellen Sie Informationen über Ihre Benutzer und Inhalt bereit, und empfangen Sie die Top-Aktion, um Sie Ihren Benutzern anzuzeigen. 
* Vor der Verwendung der Personalisierung müssen Sie keine Daten bereinigen oder bezeichnen.
* Senden Sie Feedback an die Personalisierung, wenn es Ihnen am besten passt. 
* Zeigen Sie Echtzeitanalysen an. 
* Verwenden Sie die Personalisierung als Teil eines größeren Data Science-Vorgangs zum Überprüfen vorhandener Experimente.

## <a name="how-does-personalizer-work"></a>Wie funktioniert die Personalisierung?

Die Personalisierung verwendet Machine Learning-Modelle, um zu ermitteln, welche Aktion in einem Kontext die höchste Rangfolge hat. Ihre Clientanwendung bietet eine Liste der möglichen Aktionen mit zugehörigen Informationen; außerdem Informationen über den Kontext, wozu Informationen über den Benutzer, das Gerät usw. zählen können. Die Personalisierung bestimmt die auszuführende Aktion. Sobald die Clientanwendung die ausgewählte Aktion verwendet, sendet sie in Form einer Belohnungsbewertung Feedback an die Personalisierung. Nach Eingang des Feedbacks aktualisiert die Personalisierung automatisch ihr eigenes, für zukünftige Rangfolgen verwendetes Modell.

## <a name="how-do-i-use-the-personalizer"></a>Wie verwende ich die Personalisierung?

![Verwendung der Personalisierung zur Auswahl des Videos, das einem Benutzer gezeigt wird](media/what-is-personalizer/personalizer-example-highlevel.png)

1. Wählen Sie eine zu personalisierende Benutzeroberfläche in Ihrer App aus.
1. Erstellen und konfigurieren sie den Personalisierungsdienst im Azure-Portal. Jede Instanz ist eine Personalisierungsschleife.
1. Verwenden Sie das SDK zum Aufrufen der Personalisierung mit Informationen (_Features_) zu Ihren Benutzern und dem Inhalt (_Aktionen_). Sie müssen vor der Verwendung der Personalisierung keine bereinigten, bezeichneten Daten bereitstellen. 
1. Zeigen Sie dem Benutzer in der Clientanwendung die von der Personalisierung ausgewählte Aktion an.
1. Verwenden Sie das SDK, um Feedback an die Personalisierung zu senden, die angibt, ob der Benutzer die von der Personalisierung vorgeschlagene Aktion ausgewählt hat. Dies ist eine _Belohnungsbewertung_, in der Regel zwischen -1 und 1.
1. Zeigen Sie die Analyse im Azure-Portal an, um auszuwerten, wie das System funktioniert und wie Ihre Daten die Personalisierung unterstützen.

## <a name="where-can-i-use-personalizer"></a>Wo kann ich die Personalisierung verwenden?

Beispielsweise kann die Clientanwendung die Personalisierung zu folgenden Zwecken hinzufügen:

* Personalisieren, welcher Artikel auf einer Nachrichtenwebsite hervorgehoben wird.    
* Optimieren einer Werbungsplatzierung auf einer Website.
* Anzeigen eines personalisierten „empfohlenen Artikels“ auf einer Einkaufswebsite.
* Empfehlen von Benutzeroberflächenelementen, z.B. von Filtern, die auf ein bestimmtes Foto angewendet werden.
* Auswählen der Antwort eines Chatbots, um eine Benutzerabsicht zu verdeutlichen oder eine Aktion vorzuschlagen.
* Priorisieren von Vorschlägen, was ein Benutzer als nächsten Schritt in einem Geschäftsprozess ausführen sollte.

## <a name="personalization-for-developers"></a>Personalisierung für Entwickler

Der Personalisierungsdienst verfügt über zwei APIs:

* Senden von Informationen (_Features_) zu Ihren Benutzern und dem Inhalt (_Aktionen_), der personalisiert werden soll. Die Personalisierung antwortet mit der Top-Aktion.
* Senden von Feedback an die Personalisierung, wie gut die Rangfolge funktioniert hat, in der Regel als Zahl zwischen 0 und 1 (im vorherigen Abschnitt -1 und 1). 

![Grundlegende Abfolge der Ereignisse für die Personalisierung](media/what-is-personalizer/personalization-intro.png)

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Erstellen einer Feedbackschleife in C#](csharp-quickstart-commandline-feedback-loop.md)
* [Verwenden der interaktiven Demo](https://personalizationdemo.azurewebsites.net/)
