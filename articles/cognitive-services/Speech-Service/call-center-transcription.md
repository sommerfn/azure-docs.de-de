---
title: Callcentertranskription – Speech Service
titleSuffix: Azure Cognitive Services
description: Ein häufiges Szenario für die Spracherkennung ist das Transkribieren großer Mengen von Telefoniedaten, die von verschiedenen Systemen stammen können, z.B. Systemen für interaktive Sprachantworten (Interactive Voice Response, IVR). Die Audiodaten können in Stereo oder Mono aufgenommen sein und als Rohdaten mit geringer bis keiner Nachbearbeitung des Signals vorliegen. Mithilfe von Speech-Diensten und dem Unified Speech-Modell kann ein Unternehmen qualitativ hochwertige Transkriptionen mit vielen Systemen für die Erfassung von Audiodaten erzielen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 4f5163ba448e4cc7e18b0e794a44003ce5ab1dce
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72516760"
---
# <a name="speech-services-for-telephony-data"></a>Speech-Dienste für Telefoniedaten

Telefoniedaten, die über Festnetze, Mobiltelefone und Funkgeräte erzeugt werden, sind in der Regel von geringer Qualität und schmalbandig im Bereich von 8 KHz, was Herausforderungen bei der Konvertierung von Sprache in Text mit sich bringt. Die neuesten Spracherkennungsmodelle von Azure Speech-Diensten zeichnen sich durch das Transkribieren dieser Telefoniedaten auch in solchen Fällen aus, in denen die Daten für den Menschen schwer verständlich sind. Diese Modelle werden mit großen Mengen an Telefoniedaten trainiert und weisen die beste Erkennungsgenauigkeit auf dem Markt selbst in lauten Umgebungen auf.

Ein häufiges Szenario für die Spracherkennung ist das Transkribieren großer Mengen von Telefoniedaten, die von verschiedenen Systemen stammen können, z.B. Systemen für interaktive Sprachantworten (Interactive Voice Response, IVR). Die von diesen Systemen bereitgestellten Audiodaten können in Stereo oder Mono aufgenommen sein und als Rohdaten mit geringer bis keiner Nachbearbeitung des Signals vorliegen. Mithilfe von Speech-Diensten und dem Unified Speech-Modell kann ein Unternehmen qualitativ hochwertige Transkriptionen erzielen, ganz gleich mit welchen Systemen für die Erfassung von Audiodaten gearbeitet wird.

Anhand von Telefoniedaten können Sie die Anforderungen Ihrer Kunden besser verstehen, neue Marketingmöglichkeiten ermitteln oder die Leistung von Mitarbeitern im Callcenter bewerten. Nachdem die Daten transkribiert wurden, kann ein Unternehmen die Ausgabe für eine verbesserte Telemetrie, die Identifizierung von Schlüsselausdrücken oder die Analyse der Stimmung von Kunden nutzen.

Die auf dieser Seite beschriebenen Technologien werden von Microsoft intern für verschiedene Dienste zur Verarbeitung von Supportanrufen sowohl in Echtzeit als auch im Batchmodus verwendet.

Nachfolgend sind einige der Technologien und zugehörigen Features der Azure Speech-Dienste erläutert.

> [!IMPORTANT]
> Das Unified-Modell der Speech-Dienste wird mit verschiedenen Daten trainiert und stellt eine einzelne Modelllösung für eine Reihe von Szenarien vom Diktieren bis zur Telefonieanalyse bereit.

## <a name="azure-technology-for-call-centers"></a>Azure-Technologie für Callcenter

Über den funktionalen Aspekt der Spracherkennungsdienste hinaus liegt ihr Hauptzweck – bei Einsatz im Callcenter – in der Verbesserung des Kundenerlebnisses. In dieser Hinsicht gibt es drei klare Bereiche:

* Analysen nach dem Anruf, d.h. Batchverarbeitung von Anrufaufzeichnungen
* Analyseverarbeitung des Audiosignals in Echtzeit, um verschiedene Erkenntnisse während des Anrufs zu gewinnen (wobei die Stimmungserkennung ein bedeutender Anwendungsfall ist)
* Virtuelle Assistenten (Bots), die entweder den Dialog zwischen dem Kunden und dem Bot führen, um das Problem des Kunden ohne Beteiligung eines Mitarbeiters zu lösen, oder die Anwendung von KI-Protokollen zur Unterstützung des Mitarbeiters

Die folgende Abbildung zeigt ein Diagramm der typischen Architektur zur Implementierung eines Batch-Szenarios. ![Architektur der Callcentertranskription](media/scenarios/call-center-transcription-architecture.png)

## <a name="speech-analytics-technology-components"></a>Technologiekomponenten für die Sprachanalyse

Unabhängig davon, ob es sich um eine Situation nach dem Anruf oder in Echtzeit handelt, bietet Azure eine Reihe ausgereifter und innovativer Technologien zur Verbesserung des Kundenerlebnisses.

### <a name="speech-to-text-stt"></a>Spracherkennung

Die [Spracherkennung](speech-to-text.md) ist das gefragteste Feature jeder Callcenterlösung. Da viele der nachgeschalteten Analyseprozesse auf transkribiertem Text basieren, ist die Wort-Fehler-Rate (Word Error Rate, WER) von größter Bedeutung. Eine besondere Herausforderung bei der Callcentertranskription sind die Geräusche, die im Callcenter vorherrschen (z.B. andere Mitarbeiter, die im Hintergrund sprechen), die große Bandbreite an Sprachregionen und Dialekten sowie die geringe Qualität des tatsächlichen Telefonsignals. WER steht in engem Zusammenhang damit, wie gut die Akustik- und Sprachmodelle für ein bestimmtes Gebietsschema trainiert sind. Daher ist es wichtig, dass Sie das Modell an Ihr Gebietsschema anpassen können. Unsere neuesten Unified-Modelle der Version 4.x stellen die Lösung im Hinblick auf Transkriptionsgenauigkeit und -latenz dar. Aufgrund des Trainings mit Zehntausenden von Stunden an Akustikdaten und Milliarden von lexikalischen Informationen sind Unified-Modelle die genauesten Modelle auf dem Markt für das Transkribieren von Callcenterdaten.

### <a name="sentiment"></a>Stimmung
Die Beurteilung, ob das Erlebnis für den Kunden positiv war, ist einer der wichtigsten Bereiche der Sprachanalyse in Bezug auf Callcenter. Die [Batch-Transkriptions-API](batch-transcription.md) bietet eine Stimmungsanalyse anhand von Äußerungen. Sie können die als Bestandteil eines Anruftranskripts erhaltenen Werte aggregieren, um die Stimmung beim Anruf sowohl für den Mitarbeiter als auch den Kunden zu ermitteln.

### <a name="silence-non-talk"></a>Stille (Sprechpause)
Es ist nicht ungewöhnlich, dass 35 % eines Supportanrufs aus sogenannten Sprechpausen bestehen. Hier einige Szenarien, in denen es zu Sprechpausen kommt: Mitarbeiter schlagen die früheren Interaktionen mit einem Kunden nach, Mitarbeiter verwenden Tools, die es ihnen ermöglichen, auf den Desktop des Kunden zuzugreifen und Funktionen auszuführen, Kunden befinden sich in der Warteschleife für eine Übertragung usw. Es ist äußerst wichtig zu beurteilen, wann bei einem Anruf Stille eintritt, da es eine Reihe bedeutender Empfindungen der Kunden gibt, die bei dieser Art von Szenarien und zum Zeitpunkt des Auftretens im Anruf erfolgen.

### <a name="translation"></a>Sprachübersetzung
Einige Unternehmen experimentieren mit der Bereitstellung übersetzter Transkripte von Supportanrufen in Fremdsprachen, damit Bereitstellungsmanager die Erfahrungen der Kunden weltweit verstehen können. Unsere [Übersetzungsfunktionen](translation.md) sind unübertroffen. Wir können Audio in Audio oder Audio in Text aus einer großen Anzahl von Gebietsschemas übersetzen.

### <a name="text-to-speech"></a>Text-zu-Sprache
Die [Sprachsynthese](text-to-speech.md) ist ein weiterer wichtiger Bereich bei der Implementierung von Bots, die mit den Kunden interagieren. Der typische Ablauf ist folgendermaßen: Der Kunde spricht, seine Stimme wird in Text transkribiert, der Text wird auf Absichten analysiert, eine Antwort wird basierend auf der erkannten Absicht synthetisiert und anschließend wird dem Kunden entweder ein Medienobjekt angezeigt oder es wird eine synthetisierte Sprachausgabe generiert. All dies muss natürlich schnell geschehen – daher ist die Latenz ein wichtiger Faktor für den Erfolg dieser Systeme.

Unsere End-to-End-Latenz angesicht der verschiedenen beteiligten Technologien wie [Spracherkennung](speech-to-text.md), [LUIS](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/), [Bot Framework](https://dev.botframework.com/) und [Sprachsynthese](text-to-speech.md) ist recht kurz.

Unsere neuen Stimmen sind außerdem nicht von menschlichen Stimmen zu unterscheiden. Sie können damit Ihrem Bot eine einzigartige Persönlichkeit verleihen.

### <a name="search"></a>Suchen,
Ein weiterer Schwerpunkt der Analyse ist das Erkennen von Interaktionen, bei denen ein bestimmtes Ereignis oder Erlebnis aufgetreten ist. Dazu werden in der Regel zwei Ansätze verwendet: entweder eine Ad-hoc-Suche, bei der der Benutzer einfach einen Ausdruck eingibt und das System antwortet, oder eine strukturiertere Abfrage, bei der ein Analytiker eine Reihe logischer Anweisungen erstellen kann, die ein Szenario in einem Anruf bezeichnen, und anschließend jeder Anruf anhand diese Reihe von Abfragen indiziert werden kann. Ein gutes Suchbeispiel ist die weitverbreitete Erklärung „Dieser Anruf wird zum Zweck der Qualitätssicherung aufgezeichnet… “, da viele Unternehmen sicherstellen möchten, dass die Mitarbeiter dies den Kunden mitteilen, bevor der Anruf tatsächlich aufgezeichnet wird. Die meisten Analysesysteme sind in der Lage, das durch Abfrage-/Suchalgorithmen ermittelte Verhalten als Trend anzugeben – diese Trendberichte sind schließlich eine der wichtigsten Funktionen eines Analysesystems. Mithilfe des [Cognitive Services-Verzeichnisses](https://azure.microsoft.com/services/cognitive-services/directory/search/) kann Ihre End-to-End-Lösung durch Indizierungs- und Suchfunktionen deutlich verbessert werden.

### <a name="key-phrase-extraction"></a>Schlüsselwortextraktion
Dieser Bereich ist eine der anspruchsvolleren Analyseanwendungen, die von KI und ML profitiert. Hier ist das Hauptszenario, die Absicht des Kunden zu ermitteln. Warum ruft der Kunde an? Welches Problem hat der Kunde? Warum hat der Kunde eine negative Erfahrung gemacht? Unser [Textanalysedienst](https://azure.microsoft.com/services/cognitive-services/text-analytics/) bietet eine Reihe von sofort einsetzbaren Analysefunktionen, mit denen Sie schnell ein Upgrade Ihrer End-to-End-Lösung durchführen können, um diese wichtigen Schlüsselwörter oder Ausdrücke zu extrahieren.

Nachfolgend werden die Batchverarbeitung und die Echtzeitpipelines für die Spracherkennung etwas ausführlicher erläutert.

## <a name="batch-transcription-of-call-center-data"></a>Batchtranskription von Callcenterdaten

Für die Transkription umfangreicher Audiodaten haben wir die [Batch-Transkriptions-API](batch-transcription.md) entwickelt. Die Batch-Transkriptions-API wurde entwickelt, um große Mengen an Audiodaten asynchron zu transkribieren. Im Hinblick auf die Transkription von Callcenterdaten stützt sich unsere Lösung auf folgende Säulen:

* **Genauigkeit**: Mit Unified-Modellen der vierten Generation bieten wir unübertroffene Transkriptionsqualität.
* **Latenz**: Uns ist bewusst, dass umfangreiche Transkriptionen schnell benötigt werden. Die über die [Batch-Transkriptions-API](batch-transcription.md) eingeleiteten Transkriptionsaufträge werden sofort in die Warteschlange gestellt. Sobald der Auftrag gestartet wird, erfolgt die Ausführung schneller als bei der Echtzeittranskription.
* **Sicherheit:** Wir wissen, dass Anrufe sensible Daten enthalten können. Sie können sich darauf verlassen, dass die Sicherheit bei uns höchste Priorität hat. Unser Dienst ist nach ISO, SOC, HIPAA und PCI zertifiziert.

Callcenter generieren täglich große Mengen an Audiodaten. Wenn Ihr Unternehmen Telefoniedaten an einem zentralen Ort (z.B. Azure Storage) speichert, können Sie mit der [Batch-Transkriptions-API](batch-transcription.md) Transkriptionen asynchron anfordern und empfangen.

Eine typische Lösung verwendet die folgenden Dienste:

* Azure Speech-Dienste werden zum Transkribieren von Sprache in Text verwendet. Um die Batch-Transkriptions-API zu nutzen, ist ein Standardabonnement (S0) für die Speech-Dienste erforderlich. Kostenlose Abonnements (F0) funktionieren nicht.
* [Azure Storage](https://azure.microsoft.com/services/storage/) wird verwendet, um Telefoniedaten und die von der Batch-Transkriptions-API zurückgegebenen Transkripte zu speichern. Dieses Speicherkonto sollte Benachrichtigungen verwenden, insbesondere wenn neue Dateien hinzugefügt werden. Mit diesen Benachrichtigungen wird der Transkriptionsprozess ausgelöst.
* [Azure Functions](https://docs.microsoft.com/azure/azure-functions/) wird verwendet, um den SAS-URI (Shared Access Signatures) für jede Aufzeichnung zu erstellen und die HTTP POST-Anforderung zum Starten einer Transkription auszulösen. Darüber hinaus werden mit Azure Functions Anforderungen zum Abrufen und Löschen von Transkriptionen mit der Batch-Transkriptions-API erstellt.
* [WebHooks](webhooks.md) werden verwendet, um nach Abschluss von Transkriptionen entsprechende Benachrichtigungen zu erhalten.

Die oben genannten Technologien werden von uns intern zur Unterstützung von Microsoft-Kundenanrufen im Batchmodus verwendet.
![Batch-Architektur](media/scenarios/call-center-batch-pipeline.png)

## <a name="real-time-transcription-for-call-center-data"></a>Echtzeittranskription für Callcenterdaten

Einige Unternehmen müssen Gespräche in Echtzeit transkribieren. Die Echtzeittranskription kann verwendet werden, um Schlüsselwörter zu identifizieren und die Suche nach gesprächsrelevanten Inhalten und Ressourcen auszulösen, um die Stimmung zu überwachen, die Barrierefreiheit zu verbessern oder Übersetzungen für Kunden und Mitarbeiter bereitzustellen, die keine Muttersprachler sind.

Für Szenarien, die eine Echtzeittranskription erfordern, wird die Verwendung des [Speech SDK](speech-sdk.md) empfohlen. Derzeit ist die Spracherkennung in [mehr als 20 Sprachen](language-support.md) verfügbar. Das SDK steht in C++, C#, Java, Python, Node.js, Objective-C und JavaScript bereit. Beispiele in den einzelnen Sprachen können Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk) finden. Aktuelle Neuigkeiten und Updates finden Sie unter [Versionshinweise](releasenotes.md).

Die oben genannten Technologien werden von uns intern zur Echtzeitanalyse von Microsoft-Kundenanrufen während des Gesprächs verwendet.

![Batch-Architektur](media/scenarios/call-center-reatime-pipeline.png)

## <a name="a-word-on-ivrs"></a>Kurzer Hinweis zu interaktiven Sprachantworten (IVR)

Speech-Dienste können entweder mit dem [Speech SDK](speech-sdk.md) oder der [REST-API](rest-apis.md) problemlos in jede Lösung integriert werden. Die Callcentertranskription kann jedoch zusätzliche Technologien erfordern. In der Regel ist eine Verbindung zwischen einem IVR-System und Azure erforderlich. Obwohl wir solche Komponenten nicht anbieten, möchten wir beschreiben, was eine Verbindung zu einem IVR beinhaltet.

Verschiedene IVR- oder Telefoniedienstprodukte (wie Genesys oder AudioCodes) bieten Integrationsfunktionen, die genutzt werden können, um die Durchleitung von eingehenden und ausgehenden Audiodaten an einen Azure-Dienst zu ermöglichen. Grundsätzlich kann ein benutzerdefinierter Azure-Dienst eine spezifische Schnittstelle bereitstellen, um Telefonanrufsitzungen zu definieren (z.B. Anrufstart oder Anrufende) und eine WebSocket-API zum Empfangen eines eingehenden Audiodatenstroms verfügbar zu machen, die mit den Speech-Diensten verwendet wird. Ausgehende Antworten, wie z.B. Unterhaltungstranskriptionen oder Verbindungen mit dem Bot Framework, können mit dem Sprachsynthesedienst von Microsoft synthetisiert und zur Wiedergabe an das IVR-System zurückgegeben werden.

Ein weiteres Szenario ist die direkte SIP-Integration. Ein Azure-Dienst stellt eine Verbindung mit einem SIP-Server her und erhält so einen eingehenden und einen ausgehenden Datenstrom, der für die Phasen der Spracherkennung und Sprachsynthese verwendet wird. Für die Verbindung mit einem SIP-Server gibt es kommerzielle Softwareangebote, wie z.B. Ozieki SDK, oder die [API für Anrufe und Onlinebesprechungen in Teams](/graph/api/resources/communications-api-overview) (derzeit in der Betaversion), die so konzipiert sind, dass sie diese Art von Szenario für Audioanrufe unterstützen.

## <a name="customize-existing-experiences"></a>Anpassen vorhandener Konzepte

Die Azure Speech-Dienste funktionieren bestens mit integrierten Modellen, können auf Wunsch aber auch speziell für Ihr Produkt oder Ihre Umgebung angepasst und optimiert werden. Die Anpassungsoptionen reichen von der Feinabstimmung des Akustikmodells bis hin zu individuellen Voicefonts für Ihre Marke. Nachdem Sie ein benutzerdefiniertes Modell erstellt haben, können Sie es mit allen Azure Speech-Diensten sowohl in Echtzeit als auch im Batchmodus verwenden.

| Speech-Dienst | Modell | BESCHREIBUNG |
|----------------|-------|-------------|
| Spracherkennung | [Akustikmodell](how-to-customize-acoustic-models.md) | Erstellen Sie ein benutzerdefiniertes Akustikmodell für Anwendungen, Tools oder Geräte, die in speziellen Umgebungen (etwa in einem Auto oder in einer Produktionshalle) verwendet werden, in denen jeweils besondere Aufzeichnungsverhältnisse herrschen. Beispiele wären etwa Sprecher mit Akzent, bestimmte Hintergrundgeräusche oder die Verwendung eines bestimmten Mikrofons für die Aufzeichnung. |
| | [Sprachmodell](how-to-customize-language-model.md) | Erstellen Sie ein benutzerdefiniertes Sprachmodell, um die Transkription spezifischen Vokabulars und spezifischer Grammatik (beispielsweise aus der Medizin- oder IT-Branche) zu verbessern. |
| | [Aussprachemodell](how-to-customize-pronunciation.md) | Durch ein benutzerdefiniertes Aussprachemodell können Sie die phonetische Form und Darstellung eines Worts oder Begriffs definieren. Dies ist für die Verarbeitung angepasster Benennungen wie Produktnamen oder Akronymen hilfreich. Erforderlich ist zu Beginn lediglich eine Aussprachedatei – eine einfache TXT-Datei. |
| Text-zu-Sprache | [Voicefont](how-to-customize-voice-font.md) | Mithilfe benutzerdefinierter Voicefonts können Sie eine wiedererkennbare, einzigartige Stimme für Ihre Marke erstellen. Für den Einstieg reicht bereits eine kleine Datenmenge aus. Je mehr Daten Sie bereitstellen, desto natürlicher und menschenähnlicher klingt Ihr Voicefont. |

## <a name="sample-code"></a>Beispielcode

Auf GitHub steht Beispielcode für jeden der Azure Speech-Dienste zur Verfügung. In den Beispielen werden gängige Szenarien wie etwa das Lesen von Audiodaten aus einer Datei oder einem Stream, die kontinuierliche und einmalige Erkennung oder die Verwendung benutzerdefinierter Modelle behandelt. Über die folgenden Links gelangen Sie zu den SDK- und REST-Beispielen:

* [Beispiele für Spracherkennung und Sprachübersetzung (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Beispiele für die Batch-Transkription (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [Beispiele für Text-zu-Sprache (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Referenz

* [Speech SDK](speech-sdk-reference.md)
* [Speech-Geräte-SDK](speech-devices-sdk.md)
* [REST-API: Spracherkennung](rest-speech-to-text.md)
* [REST-API: Sprachsynthese](rest-text-to-speech.md)
* [REST-API: Batchtranskription und Anpassung](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Beziehen eines kostenlosen Abonnementschlüssels für die Speech-Dienste](get-started.md)
