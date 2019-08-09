---
title: Markupsprache für Sprachsynthese (Speech Synthesis Markup Language, SSML) – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Verwenden der Markupsprache für Sprachsynthese zum Steuern der Aussprache und des Satzrhythmus in Text-zu-Sprache
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 1bd1882218630aca0707a792d120045c06dea127
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552679"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Speech Synthesis Markup Language (SSML)

Speech Synthesis Markup Language (SSML) ist eine XML-basierte Markupsprache, die Entwicklern ermöglicht, anzugeben, wie der Eingabetext mithilfe des Sprachsynthesediensts in synthetisierte Sprache konvertiert werden soll. Verglichen mit Nur-Text ermöglicht SSML Entwicklern, die Tonhöhe, Aussprache, Sprechgeschwindigkeit, Lautstärke und mehr für die Ausgabe der Sprachsynthese zu optimieren. Die normale Interpunktion, z.B. das Pausieren nach einem Punkt, oder die Verwendung der korrekten Intonation, wenn ein Satz mit einem Fragezeichen endet, werden automatisch verarbeitet.

Die Speech Services-Implementierung von SSML basiert auf der [Markupsprache für Sprachsynthese, Version 1.0](https://www.w3.org/TR/speech-synthesis) des World Wide Web Consortiums.

> [!IMPORTANT]
> Chinesische, japanische und koreanische Zeichen zählen bei der Abrechnung jeweils als zwei Zeichen. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="standard-neural-and-custom-voices"></a>Standard-, neuronale und benutzerdefinierte Stimmen

Wählen Sie aus Standard- und neuronalen Stimmen, oder erstellen Sie Ihre eigene, benutzerdefinierte Stimme, die einzigartig für Ihr Produkt oder Ihre Marke ist. Mehr als 75 Standardstimmen sind in mehr als 45 Sprachen und Gebietsschemas verfügbar, und 5 neuronale Stimmen sind in 4 Sprachen und Gebietsschemas verfügbar. Eine vollständige Liste der unterstützten Sprachen, Gebietsschemas und Stimmen (neuronal und Standard) finden Sie unter [Sprachunterstützung](language-support.md).

Weitere Informationen zu Standard-, neuronalen und benutzerdefinierten Stimmen finden Sie unter [Text-to-speech overview](text-to-speech.md) (Übersicht über die Sprachsynthese).

## <a name="supported-ssml-elements"></a>Unterstützte SSML-Elemente

Jedes SSML-Dokument wird mit SSML-Elementen (oder Tags) erstellt. Diese Elemente werden zum Anpassen von Tonhöhe, Satzrhythmus, Lautstärke und mehr verwendet. In den folgenden Abschnitten wird erläutert, wie die einzelnen Elemente verwendet werden und wann ein Element erforderlich oder optional ist.  

> [!IMPORTANT]
> Vergessen Sie nicht die Eingabe von doppelten Anführungszeichen um Attributwerte. Die Standards für wohlgeformtes, gültiges XML erfordern es, dass Attributwerte in doppelten Anführungszeichen stehen. So ist `<prosody volume="90">` ein wohlgeformtes, gültiges Element, `<prosody volume=90>` aber nicht. SSML erkennt möglicherweise nicht Attributwerte, die nicht in Anführungszeichen stehen.

## <a name="create-an-ssml-document"></a>Erstellen eines SSML-Dokuments

`speak` ist das Stammelement und bei allen SSML-Dokumenten **erforderlich**. Das `speak`-Element enthält wichtige Informationen, z.B. die Version, Sprache und die Definition des Markupvokabulars.

**Syntax**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Attribute**

| Attribut | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| version | Gibt die Version der SSML-Spezifikation an, die zum Interpretieren des Dokumentmarkups verwendet wird. Die aktuelle Version ist 1.0. | Erforderlich |
| xml:lang | Gibt die Sprache des Stammdokuments an. Der Wert kann einen Kleinbuchstaben, einen Sprachcode aus zwei Buchstaben (z. B. **de**) oder den Sprachcode und Land/Region aus Großbuchstaben (z.B. **de-DE**) enthalten. | Erforderlich |
| xmlns | Gibt den URI zu dem Dokument an, in dem das Markupvokabular (die Elementtypen und Attributnamen) des SSML-Dokuments definiert werden. Der aktuelle URI ist https://www.w3.org/2001/10/synthesis. | Erforderlich |

## <a name="choose-a-voice-for-text-to-speech"></a>Auswählen einer Stimme für Sprachsynthese

Das `voice`-Element ist erforderlich. Hiermit wird die Stimme angeben, für die Sprachsynthese verwendet wird.

**Syntax**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Attribute**

| Attribut | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| name | Identifiziert die Stimme, die für die Ausgabe der Sprachsynthese verwendet wird. Eine vollständige Liste der unterstützten Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech). | Erforderlich |

**Beispiel**

> [!NOTE]
> In diesem Beispiel wird die Stimme `en-US-Jessa24kRUS` verwendet. Eine vollständige Liste der unterstützten Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech).

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Verwenden mehrerer Stimmen

Innerhalb des `speak`-Elements können Sie mehrere Stimmen für die Ausgabe der Sprachsynthese angeben. Diese Stimmen können in verschiedenen Sprachen sein. Der Text muss bei jeder Stimme von einem `voice`-Element umschlossen werden.

**Attribute**

| Attribut | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| name | Identifiziert die Stimme, die für die Ausgabe der Sprachsynthese verwendet wird. Eine vollständige Liste der unterstützten Stimmen finden Sie unter [Sprachunterstützung](language-support.md#text-to-speech). | Erforderlich |

**Beispiel**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Good morning!
    </voice>
    <voice  name="en-US-Guy24kRUS">
        Good morning to you too Jessa!
    </voice>
</speak>
```

## <a name="adjust-speaking-styles"></a>Anpassen von Sprechweisen

> [!IMPORTANT]
> Dieses Feature funktioniert nur bei neuronalen Stimmen.

Standardmäßig synthetisiert der Sprachanalysedienst Text mithilfe einer neutralen Sprechweise sowohl bei Standard- als auch neuronalen Stimmen. Bei neuronalen Stimmen können Sie die Sprechweise anpassen, um mit dem `<mstts:express-as>`-Element Fröhlichkeit, Einfühlungsvermögen oder Stimmung auszudrücken. Dies ist ein optionales Element und für Azure-Spracherkennungsdienste eindeutig.

Anpassungen der Sprechweise werden derzeit bei diesen neuronalen Stimmen unterstützt:
* `en-US-JessaNeural`
* `zh-CN-XiaoxiaoNeural`

Änderungen werden auf Satzebene angewendet, und die Sprechweise variiert je nach Stimme. Wenn keine Sprechweise unterstützt wird, gibt der Dienst Sprache in der neutralen Standardsprechweise zurück.

**Syntax**

```xml
<mstts:express-as type="string"></mstts:express-as>
```

**Attribute**

| Attribut | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| type | Gibt die Sprechweise an. Sprechweisen sind derzeit stimmenspezifisch. | Erforderlich, wenn die Sprechweise für eine neuronale Stimme angepasst wird. Bei Verwendung von `mstts:express-as` muss der Typ angegeben werden. Bei Angabe eines ungültigen Werts wird dieses Element ignoriert. |

Ermitteln Sie anhand dieser Tabelle, welche Sprechweisen für die einzelnen neuronalen Stimmen unterstützt werden.

| Sprache | type | BESCHREIBUNG |
|-------|------|-------------|
| `en-US-JessaNeural` | type=`cheerful` | Drückt eine positive und glückliche Emotion aus |
| | type=`empathy` | Drückt ein Gefühl von Anteilnahme und Verständnis aus |
| `zh-CN-XiaoxiaoNeural` | type=`newscast` | Drückt einen formalen Ton ähnlich wie bei Nachrichtensendungen aus |
| | type=`sentiment` | Vermittelt eine anrührende Botschaft oder Geschichte |

**Beispiel**

Dieser SSML-Codeausschnitt veranschaulicht, wie die Sprechweise mithilfe des `<mstts:express-as>`-Elements in `cheerful` (fröhlich) geändert wird.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-JessaNeural">
        <mstts:express-as type="cheerful">
            That'd be just amazing!
        </mstts:express-as>
    </voice>
</speak>
```

## <a name="add-or-remove-a-breakpause"></a>Hinzufügen oder Entfernen einer Unterbrechung/Pause

Verwenden Sie das `break`-Element zum Einfügen von Pausen (oder Unterbrechungen) zwischen Wörtern oder um Pausen zu verhindern, die vom Sprachsynthesedienst automatisch hinzugefügt werden.

> [!NOTE]
> Mithilfe dieses Elements können Sie das Standardverhalten von TTS (Text-To-Speech, Text-zu-Sprache, Sprachsynthese) bei einem Wort oder Ausdruck außer Kraft setzen, wenn die synthetisierte Sprache dafür unnatürlich klingt. Legen Sie `strength` auf `none` fest, um eine Unterbrechung des Satzrhythmus zu verhindern. Dieses Element wird automatisch vom Text-zu-Sprache-Dienst eingefügt.

**Syntax**

```xml
<break strength="string" />
<break time="string" />
```

**Attribute**

| Attribut | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| strength | Gibt die relative Dauer einer Pause mit einem der folgenden Werte an:<ul><li>none</li><li>x-weak</li><li>weak</li><li>medium (Standard)</li><li>strong</li><li>x-strong</li></ul> | Optional |
| time | Gibt die absolute Dauer einer Pause in Sekunden oder Millisekunden an. Beispiele für gültige Werte sind „2s“ und „500“ | Optional |

| Strength | BESCHREIBUNG |
|----------|-------------|
| „None“, oder wenn kein Wert angegeben | 0 ms |
| x-weak | 250 ms |
| weak | 500 ms |
| mittel | 750 ms |
| strong | 1\.000 ms |
| x-strong | 1\.250 ms |


**Beispiel**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Angeben von Absätzen und Sätzen

`p`- und `s`-Elemente werden verwendet, um Abschnitte bzw. Sätze zu bezeichnen. Wenn diese Elemente fehlen, ermittelt der Sprachsynthesedienst automatisch die Struktur des SSML-Dokuments.

Das `p`-Element kann Text und die folgenden Elemente enthalten: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `sub`, `mstts:express-as` und `s`.

Das `s`-Element kann Text und die folgenden Elemente enthalten: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `mstts:express-as` und `sub`.

**Syntax**

```XML
<p></p>
<s></s>
```

**Beispiel**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <p>
            <s>Introducing the sentence element.</s>
            <s>Used to mark individual sentences.</s>
        </p>
        <p>
            Another simple paragraph.
            Sentence structure in this paragraph is not explicitly marked.
        </p>
    </voice>
</speak>
```

## <a name="use-phonemes-to-improve-pronunciation"></a>Verwenden von Phonemen zur Verbesserung der Aussprache

Das `ph`-Element wird für die phonetische Aussprache in SSML-Dokumenten verwendet. Das `ph`-Element kann nur Text und keine anderen Elemente enthalten. Geben Sie immer lesbare Sprache als ein Fallback an.

Phonetische Alphabete bestehen aus Phonen (Lauten), die sich aus Buchstaben, Zahlen oder Zeichen (manchmal in Kombination) zusammensetzen. Jedes Phon beschreibt einen eindeutigen Sprachklang. Dies steht im Gegensatz zum lateinischen Alphabet, in dem jeder Buchstabe mehrere gesprochene Klänge darstellen kann. Überlegen Sie die unterschiedliche Aussprache des Buchstabens „C“ in den Städtenamen „Coburg“ und „Celle“ oder die unterschiedliche Aussprache der Buchstabenkombination „ch“ in den Wörtern „ich“ und „ach“.

**Syntax**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Attribute**

| Attribut | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| Alphabet | Gibt das phonetische Alphabet an, das verwendet werden soll, wenn die Aussprache der Zeichenfolge im `ph`-Attribut synthetisiert wird. Die Zeichenfolge, die das Alphabet angibt, muss in Kleinbuchstaben angegeben werden. Nachstehend sind die Alphabete aufgeführt, die Sie angeben können.<ul><li>ipa &ndash; International Phonetic Alphabet (Internationales phonetisches Alphabet)</li><li>sapi &ndash; Speech API Phone Set (Phongruppe für Spracheingabe-API)</li><li>ups &ndash; Universal Phone Set (Universelle Phongruppe)</li></ul>Das Alphabet gilt nur für das Phonem im Element. Weitere Informationen finden Sie unter [Phonetic Alphabet Reference (Referenz zum phonetischen Alphabet)](https://msdn.microsoft.com/library/hh362879(v=office.14).aspx). | Optional |
| ph | Eine Zeichenfolge mit Phonen, die die Aussprache des Worts im `phoneme`-Element angeben. Wenn die angegebene Zeichenfolge nicht erkannte Phone enthält, weist der Sprachsynthesedienst das gesamte SSML-Dokument zurück und erzeugt keine der im Dokument angegebenen Sprachausgaben. | Erforderlich, wenn Phoneme verwendet werden. |

**Beispiele**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <s>His name is Mike <phoneme alphabet="ups" ph="JH AU"> Zhou </phoneme></s>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
    </voice>
</speak>
```

## <a name="adjust-prosody"></a>Anpassen des Satzrhythmus

Das `prosody`-Element wird verwendet, um Änderungen an Tonhöhe, Kontur, Bereich, Geschwindigkeit, Dauer und Lautstärke für die Ausgabe der Sprachsynthese anzugeben. Das `prosody`-Element kann Text und die folgenden Elemente enthalten: `audio`, `break`, `p`, `phoneme`, `prosody`, `say-as`, `sub` und `s`.

Weil Attributwerte für den Satzrhythmus über einen breiten Bereich variieren können, interpretiert die Spracherkennung die zugewiesenen Werte als einen Vorschlag dazu, wie die tatsächlichen Satzrhythmuswerte für die ausgewählte Stimme lauten sollten. Der Sprachsynthesedienst beschränkt oder ersetzt nicht unterstützte Werte. Beispiele für nicht unterstützte Werte sind eine Tonhöhe von 1 MHz oder eine Lautstärke von 120.

**Syntax**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Attribute**

| Attribut | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| pitch | Gibt die Basistonhöhe für den Text an. Sie können die Tonhöhe ausdrücken als:<ul><li>Ein absoluter Wert, der ausgedrückt wird als eine Zahl, hinter der„Hz“ (Hertz) steht. Beispiel: „600Hz“.</li><li>Ein relativer Wert, der ausgedrückt wird als eine Zahl, vor der „+“ oder „–“ und hinter der „Hz“ oder „st“steht, das einen Betrag zur Änderung der Tonhöhe angibt. Beispiel: „+80Hz“ oder „–2st“. Das „st“ gibt an, dass die Änderungseinheit ein Halbton ist, bei dem es sich um die Hälfte eines Tons (ein halber Schritt) auf der diatonischen Standardtonleiter handelt.</li><li>Einen konstanten Wert:<ul><li>x-low</li><li>niedrig</li><li>mittel</li><li>high</li><li>x-high</li><li>default</li></ul></li></ul>. | Optional |
| contour | Die Kontur wird bei neuronalen Stimmen nicht unterstützt. Die Kontur stellt Änderungen in der Tonhöhe bei Sprachinhalten als ein Array von Zielen an den angegebenen Zeitpositionen in der Sprachausgabe dar. Jedes Ziel wird durch Gruppen von Parameterpaaren definiert. Beispiel: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>Der erste Wert in jeder Gruppe von Parametern gibt den Ort der Tonhöhenänderung als Prozentsatz der Textdauer an. Der zweite Wert gibt den Betrag an, um den die Tonhöhe erhöht oder verringert werden soll. Dazu wird ein relativer Wert oder ein Aufzählungswert für die Tonhöhe verwendet (siehe `pitch`). | Optional |
| range  | Ein Wert, der den Tonhöhenbereich für den Text darstellt. Sie können `range` mit denselben absoluten Werten, relativen Werten oder Aufzählungswerten ausdrücken, mit denen beschrieben `pitch` wird. | Optional |
| rate  | Gibt die Sprechgeschwindigkeit für den Text an. Sie können `rate` ausdrücken als:<ul><li>Ein relativer Wert, der ausgedrückt wird als eine Zahl, die als Multiplikator des Standards fungiert. So führt beispielsweise der Wert *1* zu keiner Änderung der Geschwindigkeit. Der Wert *,5* führt zu einer Halbierung der Geschwindigkeit. Der Wert *3* führt zu einer Verdreifachung der Geschwindigkeit.</li><li>Einen konstanten Wert:<ul><li>x-slow</li><li>langsam</li><li>mittel</li><li>fast</li><li>x-fast</li><li>default</li></ul></li></ul> | Optional |
| duration  | Die Zeitspanne in Sekunden oder Millisekunden, die vergehen sollte, während der Sprachsynthesedienst den Text liest. Beispiel: *2s* oder *1800ms*. | Optional |
| Volume  | Gibt die Lautstärke der Sprechstimme an. Sie können die Lautstärke ausdrücken als:<ul><li>Ein absoluter Wert, der ausgedrückt wird als eine Zahl im Bereich von 0,0 bis 100,0 – von *am leisesten* bis zu *am lautesten*. Beispiel: „75“. Der Standardwert ist „100,0“.</li><li>Ein relativer Wert, der ausgedrückt wird als eine Zahl, vor der ein „+“ oder „–“ steht und die einen Betrag zum Ändern der Lautstärke angibt. Beispiel: „+10“ oder „–5,5“.</li><li>Einen konstanten Wert:<ul><li>silent</li><li>x-soft</li><li>soft</li><li>mittel</li><li>loud</li><li>x-loud</li><li>default</li></ul></li></ul> | Optional |

### <a name="change-speaking-rate"></a>Ändern der Sprechgeschwindigkeit

Die Sprechgeschwindigkeit kann auf Standardstimmen auf Wort- oder Satzebene angewendet werden. Die Sprechgeschwindigkeit kann dagegen nur auf neuronale Stimmen auf Satzebene angewendet werden.

**Beispiel**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        <prosody rate="+30.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-volume"></a>Ändern der Lautstärke

Änderungen der Lautstärke können auf Standardstimmen auf Wort- oder Satzebene angewendet werden. Änderungen der Lautstärke können dagegen nur auf neuronale Stimmen auf Satzebene angewendet werden.

**Beispiel**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody volume="+20.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-pitch"></a>Ändern der Tonhöhe

Änderungen der Tonhöhe können auf Standardstimmen auf Wort- oder Satzebene angewendet werden. Änderungen der Tonhöhe können dagegen nur auf neuronale Stimmen auf Satzebene angewendet werden.

**Beispiel**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
        Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
    </voice>
</speak>
```

### <a name="change-pitch-contour"></a>Ändern der Tonhöhenkontur

> [!IMPORTANT]
> Änderungen der Tonhöhenkontur werden mit neuronalen Stimmen nicht unterstützt.

**Beispiel**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <prosody contour="(80%,+20%) (90%,+30%)" >
            Good morning.
        </prosody>
    </voice>
</speak>
```

## <a name="add-recorded-audio"></a>Hinzufügen von Audioaufzeichnungen

`audio` ist ein optionales Element, mit dem Sie MP3-Audioaufzeichnungen in ein SSML-Dokument einfügen können. Der Text des Elements „audio“ kann Nur-Text oder SSML-Markup enthalten, das verwendet wird, wenn die Audiodatei nicht verfügbar oder nicht abspielbar ist. Außerdem kann das Element `audio` Text und die folgenden Elemente enthalten: `audio`, `break`, `p`, `s`, `phoneme`, `prosody`, `say-as` und `sub`.

Alle Audiodaten, die im SSML-Dokument enthalten sind, müssen die folgenden Anforderungen erfüllen:

* Die MP3-Datei muss auf einem HTTPS-Endpunkt gehostet werden, der über das Internet zugänglich ist. HTTPS ist erforderlich, und die Domäne, die die MP3-Datei hostet, muss ein gültiges vertrauenswürdiges SSL-Zertifikat enthalten.
* Es muss es sich um eine gültige MP3-Datei (MPEG v2) handeln.
* Die Bitrate muss 48 KBit/s betragen.
* Die Abtastrate muss bei 16.000 Hz liegen.
* Die Gesamtzeit für alle Text- und Audiodateien in einer einzelnen Antwort darf nicht über 90 Sekunden liegen.
* Die MP3-Datei darf keine kundenspezifischen oder andere vertrauliche Informationen enthalten.

**Syntax**

```xml
<audio src="string"/></audio>
```

**Attribute**

| Attribut | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| src | Gibt den Speicherort bzw. die URL der Audiodatei an. | Erforderlich, wenn Sie das Element „audio“ im SSML-Dokument verwenden. |

**Beispiel**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <p>
        <audio src="https://contoso.com/opinionprompt.wav"/>
        Thanks for offering your opinion. Please begin speaking after the beep.
        <audio src="https://contoso.com/beep.wav">
        Could not play the beep, please voice your opinion now. </audio>
    </p>
</speak>
```

## <a name="add-background-audio"></a>Hinzufügen von Hintergrundaudioaufnahmen

Das Element `mstts:backgroundaudio` ermöglicht es Ihnen, Hintergrundaudioaufnahmen zu Ihren SSML-Dokumenten hinzuzufügen (oder eine Audiodatei mit Text-zu-Sprache zu vermischen). Mithilfe von `mstts:backgroundaudio` können Sie im Hintergrund eine Audiodatei in Dauerschleife abspielen, die am Anfang der Text-zu-Sprache-Aufnahme ein und am Ende wieder ausgeblendet wird.

Wenn die bereitgestellte Hintergrundaudiospur kürzer ist als die Text-zu-Sprache-Aufnahme oder das Ausblenden, wird diese wieder von vorne abgespielt. Wenn sie länger als die Text-zu-Sprache-Aufnahme ist, wird sie angehalten, sobald der Ausblendevorgang abgeschlossen ist.

Pro SSML-Dokument ist nur eine Hintergrundaudiodatei zulässig. Sie können jedoch `audio`-Tags in das Element `voice` integrieren, um dem SSML-Dokument zusätzliche Audioaufnahmen hinzuzufügen.

**Syntax**

```XML
<mstts:backgroundaudio src="string" volume="string" fadein="string" fadeout="string"/>
```

**Attribute**

| Attribut | BESCHREIBUNG | Erforderlich/optional |
|-----------|-------------|---------------------|
| src | Gibt den Speicherort bzw. die URL der Hintergrundaudiodatei an. | Erforderlich, wenn Sie eine Hintergrundaudioaufnahme in Ihrem SSML-Dokument verwenden. |
| Volume | Gibt die Lautstärke der Hintergrundaudiodatei an. **Akzeptierte Werte**: `0` bis `100` (einschließlich). Standardwert: `1`. | Optional |
| fadein | Gibt an, wie lange die Hintergrundaudiodatei eingeblendet wird. **Akzeptierte Werte**: `0` bis `10000` (einschließlich).  | Optional |
| fadeout | Gibt an, wie lange die Hintergrundaudiodatei ausgeblendet wird. **Akzeptierte Werte**: `0` bis `10000` (einschließlich).  | Optional |

**Beispiel**

```xml
<speak version="1.0" xml:lang="en-US" xmlns:mstts="http://www.w3.org/2001/mstts">
    <mstts:backgroundaudio src="https://contoso.com/sample.wav" volume="0.7" fadein="3000" fadeout="4000"/>
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)">
        The text provided in this document will be spoken over the background audio.
    </voice>
</speak>
```

## <a name="next-steps"></a>Nächste Schritte

* [Sprachunterstützung: Stimmen, Gebietsschemas, Sprachen](language-support.md)
