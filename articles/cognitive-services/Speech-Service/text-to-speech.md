---
title: Text-zu-Sprache – Spracherkennungsdienst
titleSuffix: Azure Cognitive Services
description: Text-zu-Sprache aus dem Spracherkennungsdienst ist ein Feature, der es Ihren Anwendungen, Tools oder Geräten ermöglicht, Text in natürliche, menschenähnliche synthetisierte Sprache zu konvertieren. Wählen Sie aus Standard- und neuronalen Stimmen, oder erstellen Sie Ihre eigene, benutzerdefinierte Stimme, die einzigartig für Ihr Produkt oder Ihre Marke ist. Mehr als 75 Standardstimmen sind in mehr als 45 Sprachen und Gebietsschemas verfügbar, und 5 neuronale Stimmen sind in 4 Sprachen und Gebietsschemas verfügbar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: erhopf
ms.openlocfilehash: d12b952d298b41c8d06f0fcac141a45749de9051
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71799959"
---
# <a name="what-is-text-to-speech"></a>Was ist Text-zu-Sprache?

Text-zu-Sprache aus den Azure-Spracherkennungsdiensten ist ein Dienst, der es Ihren Anwendungen, Tools oder Geräten ermöglicht, Text in natürliche, menschenähnliche synthetisierte Sprache zu konvertieren. Wählen Sie aus Standard- und neuronalen Stimmen, oder erstellen Sie Ihre eigene, benutzerdefinierte Stimme, die einzigartig für Ihr Produkt oder Ihre Marke ist. Mehr als 75 Standardstimmen sind in mehr als 45 Sprachen und Gebietsschemas verfügbar, und 5 neuronale Stimmen sind in 4 Sprachen und Gebietsschemas verfügbar. Eine vollständige Liste finden Sie unter [Unterstützte Sprachen](language-support.md#text-to-speech).

Die Text-zu-Sprache-Technologie ermöglicht Inhaltserstellern, auf unterschiedliche Weise mit ihren Benutzern zu interagieren. Text-zu-Sprache kann die Barrierefreiheit verbessern, indem Benutzern eine Option für die akustische Interaktion mit dem Inhalt zur Verfügung gestellt wird. Unabhängig davon, ob der Benutzer eine Sehbehinderung oder eine Lernbehinderung hat oder während der Fahrt Navigationsinformationen benötigt, kann Text-zu-Sprache ein vorhandenes Erlebnis verbessern. Sprachsynthese ist auch ein wertvolles Add-On für Sprachbots und virtuelle Assistenten.


Mithilfe der Speech Synthesis Markup Language (SSML), einer XML-basierten Markupsprache, können die Sprachsynthese verwendende Entwickler angeben, wie der Eingabetext in synthetisierte Sprache konvertiert werden soll. Mit SSML können Sie Tonhöhe, Aussprache, Sprechgeschwindigkeit, Lautstärke und mehr anpassen. Weitere Informationen finden Sie unter [SSML](#speech-synthesis-markup-language-ssml).

### <a name="standard-voices"></a>Standardstimmen

Standardstimmen werden mithilfe von Techniken für statistische parametrische Sprachsynthese oder verketteter Sprachsynthese erstellt. Diese Stimmen sind sehr gut verständlich und klingen natürlich. Sie können für Ihre Anwendungen ganz einfach mehr als 45 Sprachen aktivieren und dabei eine Vielzahl von Stimmenoptionen nutzen. Diese Stimmen bieten eine sehr hohe Aussprachequalität, einschließlich der Unterstützung für Abkürzungen, Akronymerweiterungen, Interpretationen von Datum und Uhrzeit, Polyphone und mehr. Verwenden Sie Standardstimmen, um die Barrierefreiheit Ihrer Anwendungen und Dienste zu verbessern, indem Sie Benutzern die akustische Interaktion mit Ihren Inhalten ermöglichen.

### <a name="neural-voices"></a>Neuronale Stimmen

Neuronale Stimmen verwenden Deep Neural Networks, um die Beschränkungen herkömmlicher Sprachsynthesesysteme beim Abgleich der Muster für Betonung und Intonation bei gesprochener Sprache zu überwinden und die Spracheinheiten in eine Computerstimme zu synthetisieren. Die Standardsprachsynthese untergliedert den Satzrhythmus in separate linguistische Analyse- und akustische Vorhersageschritte, die von unabhängigen Modellen gesteuert werden, was zu gedämpfter Stimmensynthese führen kann. Unsere neuronalen Funktionen führen die Vorhersage der Satzgliederung und die Stimmensynthese gleichzeitig aus, sodass eine flüssigere und natürlicher klingende Sprache entsteht.

Gestalten Sie mit neuronalen Stimmen Interaktionen mit Chatbots und virtuellen Helfern noch natürlicher und einladender, wandeln Sie digitale Texte wie E-Books in Audiobooks um, und verpassen Sie Ihrem Navigationssystem im Auto ein Upgrade. Durch natürliche, menschenähnliche Intonation und klare Aussprache von Wörtern können neuronale Stimmen die Hörermüdung bei der Interaktion mit KI-Systemen erheblich verringern.

Neuronale Stimmen unterstützen verschiedene Stile – etwa neutral oder fröhlich. Die Stimme „Jessa“ (en-US) ist beispielsweise gut gelaunt und eignet sich somit optimal für eine herzliche, fröhliche Konversation. Sie können die [Speech Synthesis Markup Language](speech-synthesis-markup.md) verwenden, um Aspekte der Sprachausgabe wie Klangfarbe, Tonhöhe und Sprechgeschwindigkeit anzupassen. Eine vollständige Liste der verfügbaren Stimmen finden Sie unter [Unterstützte Sprachen](language-support.md#text-to-speech).

Weitere Informationen zu den Vorteilen neuronaler Stimmen finden Sie unter [Microsoft’s new neural text-to-speech service helps machines speak like people](https://azure.microsoft.com/blog/microsoft-s-new-neural-text-to-speech-service-helps-machines-speak-like-people/) (Der neue neuronale Text-zu-Sprache-Dienst von Microsoft lässt Computer wie Menschen sprechen).

### <a name="custom-voices"></a>Benutzerdefinierte Stimmen

Durch Stimmanpassungen können Sie eine wiedererkennbare, einzigartige Stimme für Ihre Marke erstellen. Zur Erstellung Ihres benutzerdefinierten Voicefonts laden Sie eine Tonstudioaufnahme und die zugehörigen Skripts als Trainingsdaten hoch. Der Dienst erstellt daraufhin ein individuelles Stimmenmodell, das auf Ihre Aufnahme abgestimmt ist. Diesen benutzerdefinierten Voicefont können Sie zum Synthetisieren von Sprache verwenden. Weitere Informationen finden Sie unter [Benutzerdefinierte Stimmen](how-to-customize-voice-font.md).

## <a name="speech-synthesis-markup-language-ssml"></a>Speech Synthesis Markup Language (SSML)

Speech Synthesis Markup Language (SSML) ist eine XML-basierte Markupsprache, die Entwicklern ermöglicht, anzugeben, wie der Eingabetext mithilfe des Sprachsynthesediensts in synthetisierte Sprache konvertiert werden soll. Verglichen mit Nur-Text ermöglicht SSML Entwicklern, die Tonhöhe, Aussprache, Sprechgeschwindigkeit, Lautstärke und mehr für die Ausgabe der Sprachsynthese zu optimieren. Die normale Interpunktion, z.B. das Pausieren nach einem Punkt, oder die Verwendung der korrekten Intonation, wenn ein Satz mit einem Fragezeichen endet, werden automatisch verarbeitet.

Alle an den Sprachsynthesedienst gesendeten Texteingaben müssen als SSML strukturiert werden. Weitere Informationen finden Sie unter [Speech Synthesis Markup Language (SSML)](speech-synthesis-markup.md).

### <a name="pricing-note"></a>Preishinweis

Bei der Nutzung des Sprachsynthesediensts wird Ihnen einschließlich Interpunktion jedes Zeichen in Rechnung gestellt, das in Sprache umgewandelt wird. Während das SSML-Dokument selbst nicht abrechenbar ist, werden optionale Elemente wie Phoneme und Tonhöhe, mit denen eingestellt wird, wie der Text in Sprache umgewandelt wird, als abrechenbare Zeichen gezählt. Folgendes wird abgerechnet:

* Text, der dem Sprachsynthesedienst im SSML-Text der Anforderung übergeben wird
* Alle Markups im Textfeld des Anforderungstexts im SSML-Format, mit Ausnahme von `<speak>`- und `<voice>`-Tags
* Buchstaben, Satzzeichen, Leerzeichen, Tabulatoren, Markups und alle sonstigen Leerzeichen
* Jeder in Unicode definierte Codepunkt

Ausführliche Informationen finden Sie unter [Cognitive Services-Preise – Sprachdienste](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

> [!IMPORTANT]
> Chinesische, japanische und koreanische Zeichen zählen bei der Abrechnung jeweils als zwei Zeichen.

## <a name="core-features"></a>Wichtige Funktionen

Die folgende Tabelle enthält die wichtigen Features für Text-zu-Sprache:

| Anwendungsfall | SDK | REST |
|----------|-----|------|
| Konvertieren von Text in Sprache. | Ja | Ja |
| Hochladen von Datasets für die Stimmanpassung. | Nein | Ja\* |
| Erstellen und Verwalten von Voicefont-Modellen. | Nein | Ja\* |
| Erstellen und Verwalten von Voicefont-Bereitstellungen. | Nein | Ja\* |
| Erstellen und Verwalten von Voicefont-Tests. | Nein | Ja\* |
| Verwalten von Abonnements. | Nein | Ja\* |

\* *Diese Dienste sind über den Endpunkt cris.ai verfügbar. Weitere Informationen finden Sie in der [Swagger-Referenz](https://westus.cris.ai/swagger/ui/index). Diese benutzerdefinierten Stimmtrainings- und Management-APIs implementieren eine Drosselung, die die Anforderungen auf 25 pro 5 Sekunden begrenzt, während die Sprachsynthese-API selbst eine Drosselung implementiert, die 200 Anforderungen pro Sekunde als Höchstwert zulässt. Wenn Drosselung auftritt, werden Sie über Nachrichtenheader benachrichtigt.*

## <a name="get-started-with-text-to-speech"></a>Erste Schritte mit der Sprachsynthese

Wir bieten Schnellstarts an, die so konzipiert sind, dass Sie in weniger als 10 Minuten Code ausführen können. In dieser Tabelle werden Schnellstarts für Text-zu-Sprache nach Sprache aufgelistet.

### <a name="sdk-quickstarts"></a>SDK-Schnellstarts

| Schnellstart (SDK) | Plattform | API-Referenz |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-text-to-speech-dotnetcore.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](quickstart-text-to-speech-dotnet-windows.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](quickstart-text-to-speech-csharp-uwp.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, Unity](quickstart-text-to-speech-csharp-unity.md) | Windows, Android | [Browse](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-text-to-speech-cpp-windows.md) | Windows | [Browse](https://aka.ms/csspeech/cppref) |
| [C++](quickstart-text-to-speech-cpp-linux.md) | Linux | [Browse](https://aka.ms/csspeech/cppref) |
| [Java](quickstart-text-to-speech-java-jre.md) | Windows, Linux, macOS | [Browse](https://aka.ms/csspeech/javaref) |
| [Java](quickstart-text-to-speech-java-android.md) | Android | [Browse](https://aka.ms/csspeech/javaref) |
| [Objective-C](quickstart-text-to-speech-objectivec-macos.md) | macOS | [Browse](https://aka.ms/csspeech/objectivecref) |
| [Objective-C](quickstart-text-to-speech-objectivec-ios.md) | iOS | [Browse](https://aka.ms/csspeech/objectivecref) |
| [Swift](quickstart-text-to-speech-swift-macos.md) | macOS | [Browse](https://aka.ms/csspeech/objectivecref) |
| [Swift](quickstart-text-to-speech-swift-ios.md) | iOS | [Browse](https://aka.ms/csspeech/objectivecref) |
| [Python](quickstart-text-to-speech-python.md) | Window, Linux, macOS | [Browse](https://aka.ms/csspeech/pythonref) |

### <a name="rest-quickstarts"></a>REST-Schnellstarts

| Schnellstart (REST) | Plattform | API-Referenz |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Window, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Window, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Beispielcode

Beispielcode für Text-zu-Sprache finden Sie auf GitHub. Diese Beispiele umfassen die Text-zu-Sprache-Konvertierung in den gängigsten Programmiersprachen.

* [Beispiele für Text-zu-Sprache (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Beispiele für Text-zu-Sprache (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Referenz

* [Speech SDK](speech-sdk-reference.md)
* [Speech-Geräte-SDK](speech-devices-sdk.md)
* [REST-API: Spracherkennung](rest-speech-to-text.md)
* [REST-API: Sprachsynthese](rest-text-to-speech.md)
* [REST-API: Batchtranskription und Anpassung](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen eines kostenlosen Speech Services-Abonnements](get-started.md)
* [Erstellen benutzerdefinierter Voicefonts](how-to-customize-voice-font.md)
