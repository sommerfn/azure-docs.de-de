---
title: Funktionsweise der Personalisierung – Personalisierung
titleSuffix: Azure Cognitive Services
description: Personalisierung verwendet maschinelles Lernen, um zu ermitteln, welche Aktion in einem Kontext verwendet werden soll. Jede Lernschleife verfügt über ein Modell, das ausschließlich mit Daten trainiert wird, die Sie über Rangfolge- und Relevanzaufrufe an es gesendet haben. Jede Lernschleife ist vollkommen unabhängig von den anderen.
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: diberry
ms.openlocfilehash: 902bf84ebf090cf9f0f886ad1e774ff7bdfeca93
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490746"
---
# <a name="how-personalizer-works"></a>Funktionsweise der Personalisierung

Personalisierung verwendet maschinelles Lernen, um zu ermitteln, welche Aktion in einem Kontext verwendet werden soll. Jede Lernschleife verfügt über ein Modell, das ausschließlich mit Daten trainiert wird, die Sie über **Rangfolge**- und **Relevanz**aufrufe an es gesendet haben. Jede Lernschleife ist vollkommen unabhängig von den anderen. Erstellen Sie für jeden Teil oder jedes Verhalten Ihrer Anwendung, den/das Sie anpassen möchten, eine Lernschleife.

Für jede Schleife **rufen Sie die Rangfolge-API** basierend auf dem aktuellen Kontext auf mit:

* Liste der möglichen Aktionen: Inhaltselemente, aus denen die Top-Aktion ausgewählt werden soll.
* Liste der [Kontextmerkmale](concepts-features.md): kontextbezogen relevante Daten wie Benutzer, Inhalt und Kontext.

Die **Rangfolge**-API entscheidet über die Verwendung einer der beiden folgenden Optionen:

* _Exploit_ (Nutzen): Das aktuelle Modell entscheidet über die beste Aktion auf Grundlage zurückliegender Daten.
* _Explore_ (Erkunden): Eine andere Aktion anstelle der Top-Aktion auswählen.

Die **Relevanz**-API:

* Sammelt Daten zum Trainieren des Modells durch Aufzeichnung der Merkmale und Relevanzbewertungen jedes Rangfolgeaufrufs.
* Verwendet diese Daten zum Aktualisieren des Modells, basierend auf der in der _Lernrichtlinie_ angegebenen Konfiguration.

## <a name="architecture"></a>Architecture

Die folgende Abbildung zeigt den Fluss der Rangfolge- und Relevanzaufrufe durch die Architektur:

![Alternativer Text](./media/how-personalizer-works/personalization-how-it-works.png "Funktionsweise der Personalisierung")

1. Personalisierung verwendet ein internes KI-Modell, um den Rang der Aktion zu bestimmen.
1. Der Dienst entscheidet, ob das aktuelle Modell genutzt oder neue Auswahlmöglichkeiten für das Modell erkundet werden sollen.  
1. Das Rangfolgeergebnis wird an EventHub gesendet.
1. Wenn die Personalisierung die Relevanz empfängt, wird diese an EventHub gesendet. 
1. Rang und Relevanz sind korreliert.
1. Das KI-Modell wird auf Grundlage der Korrelationsergebnisse aktualisiert.
1. Die Rückschluss-Engine wird mit dem neuen Modell aktualisiert. 

## <a name="research-behind-personalizer"></a>Die Forschung hinter der Personalisierung

Personalisierung basiert auf innovativer Wissenschaft und Forschung im Bereich des [vertiefenden Lernens](concepts-reinforcement-learning.md), einschließlich Artikeln, Forschungsaktivitäten und Bereichen der laufenden Forschung bei Microsoft Research.

## <a name="terminology"></a>Begriff

* **Lernschleife**: Sie können eine Lernschleife für jeden Teil Ihrer Anwendung erstellen, der von der Personalisierung profitieren kann. Wenn Sie mehr als eine zu personalisierende Erfahrung haben, erstellen Sie für jede eine Schleife. 

* **Aktionen**: Aktionen sind die Inhaltselemente, z. B. Produkte oder Werbeaktionen, unter denen ausgewählt wird. Die Personalisierung wählt über die Rangfolge-API die Top-Aktion aus, die Ihren Benutzern angezeigt werden soll, bekannt als die _Relevanzaktion_. Jede Aktion kann Merkmale besitzen, die zusammen mit der Rangfolgeanforderung übermittelt werden.

* **Context**: Um eine genauere Rangfolge bereitzustellen, geben Sie Informationen zu Ihrem Kontext an, beispielsweise:
    * Ihre Benutzer.
    * Das Gerät, das sie verwenden. 
    * Die aktuelle Zeit.
    * Andere Daten über die aktuelle Situation.
    * Vergangene Daten über den Benutzer oder Kontext.

    Ihre spezifische Anwendung kann abweichende Kontextinformationen haben. 

* **[Merkmale](concepts-features.md)** : Eine Einheit von Informationen über ein Inhaltselement oder einen Benutzerkontext.

* **Relevanz**: Ein Maß dafür, wie der Benutzer auf die von der Rangfolge-API zurückgegebene Aktion reagiert hat, als eine Bewertung zwischen 0 und 1. Der Wert von 0 bis 1 wird von Ihrer Geschäftslogik festgelegt, basierend darauf, wie die Auswahl dabei geholfen hat, Ihre geschäftlichen Ziele der Personalisierung zu erreichen. 

* **Exploration** (Erkunden): Der Personalisierungsdienst erkundet, wenn er, anstatt die beste Aktion zurückzugeben, eine andere Aktion für den Benutzer auswählt. Der Personalisierungsdienst kann durch Erkunden Verschiebungen und Stagnation vermeiden sowie sich an laufendes Benutzerverhalten anpassen. 

* **Experimentdauer**: Die Zeitspanne, die der Personalisierungsdienst auf eine Relevanz wartet, beginnend ab dem Zeitpunkt, zu dem der Rangfolgeaufruf für dieses Ereignis erfolgte.

* **Inaktive Ereignisse**: Ein inaktives Ereignis ist eines, für das Sie die Rangfolge aufgerufen haben, sich aber nicht sicher sind, ob der Benutzer jemals das Ergebnis sehen wird, weil dies Entscheidungen der Clientanwendung unterliegt. Inaktive Ereignisse ermöglichen es Ihnen, Personalisierungsergebnisse zu erstellen und zu speichern und dann zu entscheiden, sie später zu verwerfen, ohne dass dies das Machine Learning-Modell beeinträchtigt.

* **Modell**: Ein Personalisierungsmodell erfasst alle Daten, die über Benutzerverhalten gelernt wurden, erhält Trainingsdaten aus der Kombination der Argumente, die Sie an die Rangfolge- und Relevanzaufrufe senden, und besitzt ein Trainingsverhalten, das von der Lernrichtlinie bestimmt wird. 

* **Lernrichtlinie**: Die Art und Weise, wie die Personalisierung ein Modell bei jedem Ereignis trainiert, wird durch einige Metaparameter bestimmt, die die Funktionsweise der Machine Learning-Algorithmen beeinflussen. Neue Personalisierungsschleifen beginnen mit einer Standardlernrichtlinie, mit der sich eine moderate Leistung erzielen lässt. Im Zuge von [Auswertungen](concepts-offline-evaluation.md) kann die Personalisierung neue Lernrichtlinien erstellen, die speziell für die Anwendungsfälle Ihrer Schleife optimiert sind. Mit Richtlinien, die im Zuge der Auswertung generiert und für die einzelnen spezifischen Schleifen optimiert wurden, verbessert sich die Leistung der Personalisierung erheblich.

## <a name="example-use-cases-for-personalizer"></a>Beispielanwendungsfall für die Personalisierung

* Absichtsklärung und -unterscheidung: Verhelfen Sie Ihren Benutzern zu einer besseren Erfahrung, wenn ihre Absicht nicht eindeutig ist, indem Sie eine Option anbieten, die für jeden Benutzer personalisiert ist.
* Standardvorschläge für Menüs und Optionen: Lassen Sie den Bot als ersten Schritt das wahrscheinlichste Element auf personalisierte Weise vorschlagen, anstatt ein unpersönliches Menü oder eine Liste von Alternativen zu präsentieren.
* Bot-Merkmale und Ton: Erwägen Sie für Bots, die Ton, Ausführlichkeit und Schreibstil variieren können, diese Eigenschaften auf personalisierte Weise zu variieren.
* Benachrichtigungs- und Warnungsinhalt: Entscheiden Sie, welchen Text Sie für Warnmeldungen verwenden möchten, um die Benutzer stärker einzubeziehen.
* Zeitliche Steuerung von Benachrichtigungen und Warnungen: Nutzen Sie personalisierte Informationen dazu, wann Benachrichtigungen am besten an Benutzer gesendet werden, um sie stärker einzubeziehen.

## <a name="how-to-use-personalizer-in-a-web-application"></a>Verwenden von Personalisierung in einer Webanwendung

Das Hinzufügen einer Schleife zu einer Webanwendung umfasst folgende Aktionen:

* Bestimmen Sie, welche Erfahrung Sie personalisieren möchten, welche Aktionen und Merkmale Sie haben, welche Kontextmerkmale verwendet werden sollen, und welche Belohnung Sie festlegen werden.
* Fügen Sie einen Verweis auf das Personalisierung-SDK in Ihrer Anwendung hinzu.
* Rufen Sie die Rang-API auf, wenn Sie bereit sind, zu personalisieren.
* Speichern Sie die Ereignis-ID. Sie senden später eine Belohnung mit der Relevanz-API.
1. Rufen Sie die Aktivierung für das Ereignis auf, sobald Sie sind Sie sicher, dass der Benutzer Ihre personalisierte Seite gesehen hat.
1. Warten Sie auf die Auswahl des Benutzers aus nach Rang sortiertem Inhalt. 
1. Rufen Sie Relevanz-API auf, um anzugeben, wie gut die Ausgabe der Rang-API funktioniert hat.

## <a name="how-to-use-personalizer-with-a-chat-bot"></a>Verwenden von Personalisierung mit einem Chatbot

In diesem Beispiel sehen Sie, wie mit Personalisierung ein Standardvorschlag gemacht wird, anstatt den Benutzer jedes Mal durch eine Reihe von Menüs oder Optionen zu schicken.

* Rufen Sie den [Code](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/tree/master/samples/ChatbotExample) für dieses Beispiel ab.
* Richten Sie Ihre Bot-Lösung ein. Achten Sie darauf, Ihre LUIS-Anwendung zu veröffentlichen. 
* Verwalten Sie Aufrufe der Rang- und Relevanz-API für Bots.
    * Fügen Sie Code zum Verwalten der LUIS-Absichtsverarbeitung hinzu. Wenn **Keine** als am höchsten eingestufte Absicht zurückgegeben wird oder die Punktzahl der am höchsten eingestufte Absicht unter dem Schwellenwert Ihrer Geschäftslogik liegt, senden Sie die Absichtsliste an Personalisierung, um die Absichten zu bewerten.
    * Zeigen Sie dem Benutzer die Absichtsliste als auswählbare Links an, wobei die erste Absicht die am höchsten eingestufte Absicht aus der Antwort der Rang-API ist.
    * Erfassen Sie die Auswahl des Benutzers, und senden Sie diese im Aufruf der Relevanz-API. 

### <a name="recommended-bot-patterns"></a>Empfohlene Bot-Muster

* Rufen Sie die Rang-API der Personalisierung immer auf, wenn eine Unterscheidung erforderlich ist, anstatt Ergebnisse für jeden Benutzer zwischenzuspeichern. Das Ergebnis der Vereindeutigung einer Absicht kann sich im Laufe der Zeit für eine Person ändern, und wenn die Rang-API Abweichungen untersuchen kann, wird das allgemeine Lernen beschleunigt.
* Wählen Sie eine Interaktion, die bei vielen Benutzern üblich ist, damit Sie genügend Daten zur Personalisierung haben. Beispielsweise können einleitende Fragen besser geeignet sein, als kleinere Klarstellungen tief im Konversationsgraphen, zu denen u. U. nur wenige Benutzer gelangen.
* Verwenden Sie Rang-API-Aufrufe, um Konversationen vom Typ „erster Vorschlag ist richtig“ zu ermöglichen, bei denen der Benutzer gefragt wird „Möchten Sie X?“ oder „Meinten Sie X?“ und dann einfach bestätigen kann, anstatt dem Benutzer ein Menü mit Optionen zur Auswahl zu geben. Beispiel: Benutzer: „Ich möchte einen Kaffee bestellen.“ Bot: „Möchten Sie einen doppelten Espresso?“ Auf diese Weise ist auch das Relevanzsignal stark, da direkt auf den einen Vorschlag Bezug genommen wird.

## <a name="how-to-use-personalizer-with-a-recommendation-solution"></a>Verwenden der Personalisierung mit einer Empfehlungslösung

Verwenden Sie Ihr Empfehlungsmodul, um einen umfangreichen Katalog auf einige wenige Elemente zu herunterzufiltern, die dann als 30 mögliche Aktionen an die Rang-API gesendet werden können.

Sie können Empfehlungsmodule mit der Personalisierung verwenden:

* Richten Sie die [Empfehlungslösung](https://github.com/Microsoft/Recommenders/) ein. 
* Rufen Sie beim Anzeigen einer Seite das Empfehlungsmodell auf, um eine kurze Liste mit Empfehlungen zu erhalten.
* Rufen Sie die Personalisierung auf, um eine Rangfolge für die Ausgabe der Empfehlungslösung festzulegen.
* Senden Sie mit dem Relevanz-API-Aufruf Feedback zu Ihrer Benutzeraktion.


## <a name="pitfalls-to-avoid"></a>Vermeidbare Fehler

* Verwenden Sie die Personalisierung nicht, wenn das personalisierte Verhalten nicht für alle Benutzer erkannt werden kann, sondern für bestimmte Benutzer gespeichert bleiben sollte oder aus einer benutzerspezifischen Liste von Alternativen stammt. Beispiel: Das Verwenden der Personalisierung für Vorschläge zu einer ersten Pizzabestellung aus einer Liste von 20 möglichen Menüelementen ist nützlich. Im Gegensatz dazu kann der Kontakt aus der Kontaktliste der Benutzer, der angerufen werden kann, wenn Hilfe bei der Kinderbetreuung benötigt wird (wie z. B. „Oma“), nicht für Ihre gesamte Benutzerbasis personalisiert werden.


## <a name="adding-content-safeguards-to-your-application"></a>Hinzufügen von Sicherheitsmechanismen für Inhalt zu Ihrer Anwendung

Wenn Ihre Anwendung starke Abweichungen bei den Inhalten zulässt, die den Benutzern angezeigt werden, und einige dieser Inhalte für einige Benutzer unsicher oder ungeeignet sind, sollten Sie im Voraus planen, um sicherzustellen, dass die richtigen Sicherheitsvorkehrungen getroffen werden, um zu verhindern, dass Ihre Benutzer unzulässige Inhalte sehen. Das beste Muster zum Implementieren von Sicherheitsmechanismen ist:
    * Rufen Sie die Aktionen ab, denen ein Rang zugewiesen werden soll.
    * Filtern Sie Aktionen aus, die nicht für die Zielgruppe geeignet sind.
    * Weisen Sie nur den geeigneten Aktionen einen Rang zu.
    * Zeigen Sie dem Benutzer die am besten bewertete Aktion an.

In einigen Architekturen ist die obige Sequenz u. U. schwer zu implementieren. In diesem Fall gibt es einen alternativen Ansatz für die Implementierung von Sicherheitsmaßnahmen nach Rangzuweisung, es muss jedoch eine Vorkehrung getroffen werden, damit Aktionen, die nicht unter die Sicherheitsmaßnahmen fallen, nicht zum Trainieren des Personalisierungsmodells verwendet werden.

* Rufen Sie die Aktionen, denen ein Rang zugewiesen werden soll, mit deaktivierter Lernfunktion ab.
* Weisen Sie den Aktionen Ränge zu.
* Überprüfen Sie, ob die erste Aktion gültig ist.
    * Wenn die erste Aktion gültig ist, aktivieren Sie die Lernfunktion für diesen Rang, und zeigen Sie sie dann dem Benutzer an.
    * Wenn die erste Aktion nicht gültig ist, aktivieren Sie das Lernen für diese Rangfolge nicht, und entscheiden Sie anhand Ihrer eigenen Logik oder alternativer Ansätze, was Sie dem Benutzer zeigen möchten. Auch wenn Sie die Option mit dem zweitbesten Rang verwenden, aktivieren Sie das Lernen für diese Rangfolge nicht.

## <a name="verifying-adequate-effectiveness-of-personalizer"></a>Überprüfung der angemessenen Wirksamkeit der Personalisierung

Sie können die Wirksamkeit der Personalisierung in regelmäßigen Abständen überwachen, indem Sie [Offlineauswertungen](how-to-offline-evaluation.md) durchführen.

## <a name="next-steps"></a>Nächste Schritte

Versteshen, [wo Sie Personalisierung verwenden können](where-can-you-use-personalizer.md).
Führen Sie [Offlineauswertungen](how-to-offline-evaluation.md) durch.
