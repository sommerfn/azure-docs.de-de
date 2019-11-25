---
title: Sprachassistenten – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Ein Überblick über die Features, Funktionen und Einschränkungen für Sprachassistenten, die das Speech Software Development Kit (SDK) verwenden.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: bd808c0c71e02483b4c4b06e612720c1802869a0
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73577977"
---
# <a name="about-voice-assistants"></a>Informationen zu Sprach-Assistenten

Sprachassistenten, die Azure Speech-Dienste verwenden, ermöglichen es Entwicklern, natürliche Konversationsschnittstellen für ihre Anwendungen und Umgebungen zu erstellen, die der menschlichen Sprache nahekommen.

Der Dienst für Sprachassistenten ermöglicht eine schnelle und zuverlässige Interaktion zwischen einem Gerät und einer Assistentenimplementierung. Hierbei kommt zur Aufgabenausführung entweder der Direct Line Speech-Kanal im Bot Framework oder die Lösung „Benutzerdefinierte Befehle (Vorschau)“ zum Einsatz.

Anwendungen stellen mit dem Speech Software Development Kit (SDK) eine Verbindung mit dem Sprachassistenten her.

   ![Konzeptionelle Darstellung des Orchestrierungsdienstflows für den Sprachassistenten](media/voice-assistants/overview.png "Flow für Sprachassistenten")

## <a name="choosing-an-assistant-solution"></a>Auswählen einer Assistenten-Lösung

Im ersten Schritt zur Erstellung eines Sprach-Assistenten wird festgelegt, welches Ziel erreicht werden soll. Die Speech-Dienste von Azure verfügen über mehrere Lösungen für die Entwicklung Ihrer Assistenten-Interaktionen, die sich gegenseitig ergänzen. Für den Einstieg ist die Wahl der richtigen Tools wichtig – unabhängig davon, ob Sie die Flexibilität und Vielseitigkeit des [Direct Line Speech](direct-line-speech.md)-Kanals des Bot Frameworks oder die Einfachheit von [Benutzerdefinierte Befehle (Vorschauversion)](custom-commands.md) für unkomplizierte Szenarien benötigen.

| Zielsetzung | Optionen | Beispiel |
|-------------------|------------------|----------------|
|Konversation mit offenem Ende mit Integration von stabilen Fähigkeiten und vollständiger Bereitstellungssteuerung | [Direct Line Speech](direct-line-speech.md)-Kanal des Bot Frameworks | <ul><li>„Ich möchte nach Stuttgart fahren“</li><li>„Welche Art von Pizza kann ich bestellen?“</li></ul>
|Konversation vom Typ „Befehl und Steuerung“ oder aufgabenorientierte Konversation mit vereinfachter Erstellung und einfachem Hosting | [Benutzerdefinierte Befehle (Vorschau)](custom-commands.md) | <ul><li>„Deckenlampe einschalten“</li><li>„Temperatur um fünf Grad erhöhen“</ul>

Wir empfehlen Ihnen die Verwendung von [Direct Line Speech](direct-line-speech.md) als Standardauswahl, falls Sie noch nicht genau wissen, wie Ihr Assistent eingesetzt werden soll. Hiermit erhalten Sie eine Integration mit umfassenden Tools und Erstellungshilfen, z. B. die [Lösung „Virtueller Assistent“ und die Unternehmensvorlage](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview) sowie den [QnA Maker-Dienst](https://docs.microsoft.com/azure/cognitive-services/QnAMaker/Overview/overview), um häufig verwendete Muster zu nutzen und zu erweitern und Ihre vorhandenen Wissensquellen zu verwenden.

Bei [Benutzerdefinierte Befehle (Vorschauversion)](custom-commands.md) handelt es sich um eine optimierte Erstellungs- und Hostingumgebung, die für Szenarien vom Typ „Befehl und Steuerung“ in natürlicher Sprache konzipiert wurde.
   ![Vergleich von Assistentenlösungen](media/voice-assistants/assistant-solution-comparison.png "CompVergleich von Assistentenlösungen")

## <a name="core-features"></a>Wichtige Funktionen

Sowohl bei Nutzung von [Direct Line Speech](direct-line-speech.md) als auch von [Benutzerdefinierte Befehle (Vorschauversion)](custom-commands.md) zum Erstellen Ihrer Assistenten-Interaktionen können Sie auf umfassende Anpassungsfunktionen zurückgreifen, mit denen Sie Ihren Assistenten an Ihre Wünsche in Bezug auf Marke, Produkt und Persönlichkeit anpassen können.

| Category | Features |
|----------|----------|
|[Benutzerdefiniertes Schlüsselwort](speech-devices-sdk-create-kws.md) | Benutzer können Konversationen mit Assistenten über ein benutzerdefiniertes Schlüsselwort wie z. B. „Hallo, Contoso“ starten. Eine App verwendet hierzu eine benutzerdefinierte Schlüsselwort-Engine im Speech SDK, um ein benutzerdefiniertes Schlüsselwort zu konfigurieren, [das Sie hier generieren können](speech-devices-sdk-create-kws.md). Sprachassistenten können eine dienstseitige Schlüsselwortüberprüfung nutzen, um die Genauigkeit der Schlüsselwortaktivierung zu verbessern (statt diesen Vorgang nur über das Gerät durchzuführen).
|[Spracherkennung](speech-to-text.md) | Sprachassistenten wandeln mithilfe der [Spracherkennung](speech-to-text.md) der Azure Speech-Dienste Audiodaten in Echtzeit in erkannten Text um. Dieser Text ist während der Transkription sowohl für Ihre Assistentenimplementierung als auch für Ihre Clientanwendung verfügbar.
|[Sprachsynthese](text-to-speech.md) | Textantworten von Ihrem Assistenten werden mithilfe der [Sprachsynthese](text-to-speech.md) der Azure Speech-Dienste synthetisiert. Diese Synthese wird dann Ihrer Clientanwendung als Audiostream zur Verfügung gestellt. Microsoft bietet die Möglichkeit, eine benutzerdefinierte, hochwertige neuronale TTS-Stimme zu erstellen, die Ihrer Marke eine Stimme verleiht. [Kontaktieren Sie uns](mailto:mstts@microsoft.com), wenn Sie mehr darüber erfahren möchten.

## <a name="getting-started-with-voice-assistants"></a>Erste Schritte mit Sprachassistenten

Wir bieten Schnellstarts an, die so konzipiert sind, dass Sie in weniger als 10 Minuten Code ausführen können. Diese Tabelle enthält eine Liste mit Schnellstarts für Sprachassistenten, die nach Sprache sortiert sind.

| Schnellstart | Plattform | API-Referenz |
|------------|----------|---------------|
| C#, UWP | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| Java | Windows, macOS, Linux | [Browse](https://aka.ms/csspeech/javaref) |
| Java | Android | [Browse](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Beispielcode

Beispielcode für die Erstellung eines Sprachassistenten ist auf GitHub verfügbar. Diese Beispiele decken die Clientanwendung für die Verbindung mit Ihrem Assistenten in mehreren gängigen Programmiersprachen ab.

* [Beispiele für Sprachassistenten (SDK)](https://aka.ms/csspeech/samples)
* [Tutorial: Sprachaktivierung für Ihren Assistenten mit dem Speech SDK, C#](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="tutorial"></a>Tutorial

In diesem Tutorial erfahren Sie, wie Sie Ihren [Assistenten mithilfe des Speech SDK und des Direct Line-Speech-Kanals für Sprache aktivieren](tutorial-voice-enable-your-bot-speech-sdk.md).

## <a name="customization"></a>Anpassung

Sprachassistenten, die mithilfe der Azure Speech-Dienste erstellt wurden, können die gesamte Bandbreite der Anpassungsoptionen nutzen, die für [Spracherkennung](speech-to-text.md), [Sprachsynthese](text-to-speech.md) und die [Auswahl benutzerdefinierter Stichwörter](speech-devices-sdk-create-kws.md) zur Verfügung stehen.

> [!NOTE]
> Die Anpassungsoptionen unterscheiden sich je nach Sprache/Gebietsschema. Weitere Informationen erhalten Sie unter [Unterstützte Sprachen](supported-languages.md).

## <a name="reference-docs"></a>Referenz

* [Speech SDK](speech-sdk-reference.md)
* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Nächste Schritte

* [Beziehen eines kostenlosen Abonnementschlüssels für die Spracherkennungsdienste](get-started.md)
* [Abrufen des Speech SDK](speech-sdk.md)
* [Weitere Informationen zu „Benutzerdefinierte Befehle (Vorschau)“](custom-commands.md)
* [Informationen zu Direct Line Speech](direct-line-speech.md)