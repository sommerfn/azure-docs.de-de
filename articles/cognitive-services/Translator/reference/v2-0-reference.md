---
title: Textübersetzungs-API Version 2.0
titleSuffix: Azure Cognitive Services
description: Referenzdokumentation für die Textübersetzungs-API Version 2.0
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/15/2018
ms.author: swmachan
ms.openlocfilehash: 88503c73e2ca9cf04e64ca3a47793e9b10ca325a
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357833"
---
# <a name="translator-text-api-v20"></a>Microsoft Translator-Text-API Version 2.0

> [!IMPORTANT]
> Diese Version der Textübersetzungs-API ist als veraltet markiert. [Sehen Sie sich die Dokumentation für Version 3 der Textübersetzungs-API an](v3-0-reference.md).

Die Textübersetzungs-API, Version 2, kann nahtlos in Ihre Anwendungen, Websites, Tools oder andere Lösungen integriert werden, damit Benutzererlebnisse in mehreren Sprachen zur Verfügung stehen. Durch Nutzung von Industriestandards kann die API auf jeder Hardwareplattform und mit jedem Betriebssystem verwendet werden, um Sprachübersetzungen und andere sprachbezogene Vorgänge wie Textsprachenerkennung oder Sprachsynthese durchzuführen. Hier finden Sie weitere Informationen zur Microsoft-Textübersetzungs-API.

## <a name="getting-started"></a>Erste Schritte
Für den Zugriff auf die Textübersetzungs-API müssen Sie sich bei [Microsoft Azure registrieren](../translator-text-how-to-signup.md).

## <a name="authorization"></a>Authorization
Alle Aufrufe an die Textübersetzungs-API erfordern einen Abonnementschlüssel für die Authentifizierung. Die API unterstützt drei Authentifizierungsmodi:

- Ein Zugriffstoken. Verwenden Sie den unter **Schritt 9** angegebenen Abonnementschlüssel, um ein Zugriffstoken zu generieren. Stellen Sie dazu eine POST-Anforderung an den Authentifizierungsdienst. Ausführliche Informationen dazu finden Sie in der Tokendienst-Dokumentation. Übergeben Sie das Zugriffstoken mithilfe des Autorisierungsheaders oder des Abfrageparameters `access_token` an den Übersetzerdienst. Das Zugriffstoken ist 10 Minuten lang gültig. Rufen Sie alle 10 Minuten ein neues Zugriffstoken ab, und verwenden Sie innerhalb dieser 10 Minuten für wiederholte Anforderungen weiterhin dasselbe Zugriffstoken.
- Direkte Verwendung eines Abonnementschlüssels. Übergeben Sie Ihren Abonnementschlüssel als Wert im `Ocp-Apim-Subscription-Key`-Header, der in Ihrer Anforderung an die Übersetzungs-API enthalten ist. In diesem Modus müssen Sie den Authentifizierungstokendienst nicht aufrufen, um ein Zugriffstoken zu generieren.
- Ein [Cognitive Services-Abonnement für mehrere Dienste](https://azure.microsoft.com/pricing/details/cognitive-services/). In diesem Modus können Sie einen einzelnen geheimen Schlüssel verwenden, um Anforderungen für mehrere Dienste zu authentifizieren. <br/>
Wenn Sie einen geheimen Multi-Service-Schlüssel verwenden, müssen Sie Ihrer Anforderung zwei Authentifizierungsheader hinzufügen. Der erste Header übergibt den geheimen Schlüssel. Der zweite Header gibt die Region an, die Ihrem Abonnement zugeordnet ist:
   - `Ocp-Apim-Subscription-Key`
   - `Ocp-Apim-Subscription-Region`

Die Region ist für das Abonnement der Multi-Service-Text-API erforderlich. Die von Ihnen ausgewählte Region ist die einzige Region, die Sie für die Textübersetzung verwenden können, wenn Sie den Multi-Service-Abonnementschlüssel verwenden, und muss sich um die gleiche Region handeln, die Sie bei der Registrierung für Ihr Multi-Service-Abonnement über das Azure-Portal ausgewählt haben.

Diese Regionen sind verfügbar: `australiaeast`, `brazilsouth`, `canadacentral`, `centralindia`, `centraluseuap`, `eastasia`, `eastus`, `eastus2`, `japaneast`, `northeurope`, `southcentralus`, `southeastasia`, `uksouth`, `westcentralus`, `westeurope`, `westus` und `westus2`.

Prüfen Sie, dass Ihr Abonnementschlüssel und das Zugriffstoken Geheimnisse sind, die nicht in der Ansicht dargestellt werden sollten.

## <a name="profanity-handling"></a>Behandlung von Obszönitäten
In der Regel behält der Übersetzungsdienst Obszönitäten, die im Quelltext vorhanden sind, in der Übersetzung bei. Der Grad der Obszönität sowie der Kontext, der Wörter obszön machen, unterscheiden sich von Kultur zu Kultur, und so kann der Grad der Obszönität in der Zielsprache verstärkt oder auch verringert werden.

Wenn Sie keine Obszönitäten in der Übersetzung wünschen, auch wenn diese im Quelltext vorhanden sind, können Sie den die Obszönitäten-Filteroption für die Methoden verwenden, die diese unterstützen. Mit dieser Option können Sie auswählen, ob Obszönitäten gelöscht oder entsprechend gekennzeichnet werden sollen oder ob keine Aktion durchgeführt werden soll. Die akzeptierten Werte von `ProfanityAction` sind `NoAction` (Standard), Markiert und `Deleted`.


|ProfanityAction    |Aktion |Beispielquelle (Japanisch)  |Beispielübersetzung (Deutsch)  |
|:--|:--|:--|:--|
|NoAction   |Standard. Entspricht dem Fall, in dem die Option nicht festgelegt wird. Die Obszönitäten werden von der Ausgangs- in die Zielsprache übergeben.        |彼はジャッカスです。     |Er ist ein Trottel.   |
|Marked     |Obszöne Wörter werden von den XML-Tags \<profanity> und \</profanity> umschlossen.       |彼はジャッカスです。 |Er ist ein \<profanity>Trottel\</profanity>.  |
|Deleted    |Obszöne Begriffe werden aus der Ausgabe entfernt, und es wird kein Ersatzbegriff gestellt.     |彼はジャッカスです。 |Er ist ein.   |

    
## <a name="excluding-content-from-translation"></a>Ausschließen von Inhalt aus der Übersetzung
Wenn Sie Inhalte mit Tags wie HTML (`contentType=text/html`) übersetzen, kann es nützlich sein, bestimmte Inhalte aus der Übersetzung auszuschließen. Sie können das Attribut `class=notranslate` verwenden, um Inhalt anzugeben, der in der Originalsprache übernommen werden soll. Im folgenden Beispiel wird der Inhalt im ersten `div`-Element nicht übersetzt, während der Inhalt im zweiten `div`-Element übersetzt wird.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>GET /Translate

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Eine Textzeichenfolge wird von einer Sprache in eine andere übersetzt.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/Translate`.

**Rückgabewert:** Eine Zeichenfolge, die den übersetzten Text darstellt.

Wenn Sie zuvor `AddTranslation` oder `AddTranslationArray` zur Eingabe einer Übersetzung mit der Bewertung von 5 oder höher für den gleichen Quelltext verwendet haben, gibt `Translate` nur die erste Wahl zurück, die für Ihr System zur Verfügung steht. „Der gleiche Quelltext“ bedeutet genau dasselbe Ergebnis (hundertprozentige Übereinstimmung) mit Ausnahme von Groß- und Kleinschreibung, Leerzeichen, Tagwerten und Zeichensetzung am Ende des Satzes. Wenn keine Bewertung von 5 oder höher gespeichert wird, ist das zurückgegebene Ergebnis die automatische Übersetzung durch Microsoft Translator.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)

Zeichenfolge

Anforderungsinhaltstyp: application/xml 

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG    |Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid  |(leer)    |Erforderlich. Wenn der Header „Authorization“ oder „Ocp-Apim-Subscription-Key“ verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die „Bearer“ + „ „ (Leerzeichen) + „access_token“ enthält.|query|Zeichenfolge|
|text|(leer)   |Erforderlich. Eine Zeichenfolge, die den Text darstellt, der übersetzt werden soll. Die Textgröße darf 10.000 Zeichen nicht überschreiten.|query|Zeichenfolge|
|from|(leer)   |Optional. Eine Zeichenfolge, die den Sprachcode des Übersetzungstexts darstellt. Z.B. „en“ für Englisch|query|Zeichenfolge|
|zu|(leer) |Erforderlich. Eine Zeichenfolge, die den Code der Sprache darstellt, in die der Text übersetzt werden soll.|query|Zeichenfolge|
|contentType|(leer)    |Optional. Das Format des Texts, der übersetzt wird. Die unterstützten Formate sind text/plain (Standard) und text/html. Jede HTML muss ein wohlgeformtes vollständiges Element sein.|query|Zeichenfolge|
|category|(leer)   |Optional. Eine Zeichenfolge, die die Kategorie (Domäne) der Übersetzung enthält. Der Standardwert lautet „general“.|query|Zeichenfolge|
|Authorization|(leer)  |Erforderlich, falls das Feld „appid“ oder der Header „Ocp-Apim-Subscription-Key“ nicht angegeben ist. Autorisierungstoken:  „Bearer“ + „ „ + „access_token“.|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)  |Erforderlich, falls das Feld „appid“ oder der Header „Authorization“ nicht angegeben ist.|Header|Zeichenfolge|


### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader „X-MS-Trans-Info“ enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="post-translatearray"></a>POST /TranslateArray

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Verwenden Sie die `TranslateArray`-Methode, um Übersetzungen für mehrere Quelltexte abzurufen.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

Das Format des Anforderungstexts sollte wie Folgt aussehen:

```
<TranslateArrayRequest>
  <AppId />
  <From>language-code</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" />
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</TranslateArrayRequest>
```

Elemente innerhalb des `TranslateArrayRequest` sind:


* `appid`: Erforderlich. Wenn der Header `Authorization` und `Ocp-Apim-Subscription-Key` verwendet wird, lassen Sie das Feld „appid“ leer, und fügen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.
* `from`: Optional. Eine Zeichenfolge, die den Code der Sprache darstellt, aus der der Text übersetzt werden soll. Wenn das Element leer ist, enthält die Antwort das Ergebnis der automatischen Erkennung.
* `options`: Optional. Ein `Options`-Objekt, das die unten aufgeführten Werte enthält. Diese sind alle optional und sind Standardwerte für die häufigsten Einstellungen. Angegebene Element müssen in alphabetischer Reihenfolge aufgelistet werden.
    - `Category`: Eine Zeichenfolge, die die Kategorie (Domäne) der Übersetzung enthält. Der Standardwert lautet `general`.
    - `ContentType`: Das Format des Texts, der übersetzt wird. Die unterstützten Formate sind `text/plain` (Standard), `text/xml` und `text/html`. Jede HTML muss ein wohlgeformtes vollständiges Element sein.
    - `ProfanityAction`: Gibt an, wie Obszönitäten behandelt werden (wie oben beschrieben). Akzeptierte Werte von `ProfanityAction` sind `NoAction` (Standard), `Marked` und `Deleted`.
    - `State`: Benutzerstatus, mit dem Anforderung und Antwort korreliert werden können. Die gleichen Inhalte werden in der Antwort zurückgegeben.
    - `Uri`: Filterergebnisse von diesem URI. Standard: `all`
    - `User`: Filterergebnisse von diesem Benutzer. Standard: `all`
* `texts`: Erforderlich. Eine Zeichenfolge, die Texte für die Übersetzung enthält. Alle Zeichenfolgen müssen in der gleichen Sprache sein. Der Gesamtwert aller Texte, die übersetzt werden sollen, darf 10.000 Zeichen nicht übersteigen. Die maximale Anzahl von Arrayelementen beträgt 2.000.
* `to`: Erforderlich. Eine Zeichenfolge, die den Code der Sprache darstellt, in die der Text übersetzt werden soll.

Optionale Elemente können ausgelassen werden. Elemente, die direkte untergeordnete Elemente von TranslateArrayRequest sind, müssen in alphabetischer Reihenfolge aufgeführt sein.

Die TranslateArray-Methode akzeptiert `application/xml` oder `text/xml` für `Content-Type`.

**Rückgabewert:** Ein `TranslateArrayResponse`-Array. Jede `TranslateArrayResponse` verfügt über folgende Elemente:

* `Error`: Gibt einen Fehler an, wenn ein Fehler aufgetreten ist. Andernfalls ist das Element auf NULL festgelegt.
* `OriginalSentenceLengths`: Ein Array von Integerwerten, das die Länge jedes Satzes im ursprünglichen Ausgangstext angibt. Die Länge des Arrays gibt die Anzahl der Sätze an.
* `TranslatedText`: Der übersetzte Text.
* `TranslatedSentenceLengths`: Ein Array von Integerwerten, das die Länge jedes Satzes im übersetzten Text angibt. Die Länge des Arrays gibt die Anzahl der Sätze an.
* `State`: Benutzerstatus, mit dem Anforderung und Antwort korreliert werden können. Gibt den gleichen Inhalt wie in der Anforderung zurück.

Das Format des Antworttexts sieht wie folgt aus.

```
<ArrayOfTranslateArrayResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <TranslateArrayResponse>
    <From>language-code</From>
    <OriginalTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </OriginalTextSentenceLengths>
    <State/>
    <TranslatedText>string-value</TranslatedText>
    <TranslatedTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </TranslatedTextSentenceLengths>
  </TranslateArrayResponse>
</ArrayOfTranslateArrayResponse>
```

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Eine erfolgreiche Antwort enthält ein Array von `TranslateArrayResponse` im oben beschriebenen Format.

Zeichenfolge

Anforderungsinhaltstyp: application/xml

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|Authorization|(leer) |Erforderlich, falls das Feld „appid“ oder der Header „Ocp-Apim-Subscription-Key“ nicht angegeben ist. Autorisierungstoken:  „Bearer“ + „ „ + „access_token“.|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, falls das Feld „appid“ oder der Header „Authorization“ nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode   |`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort. Häufige Fehler sind z.B. folgende: <ul><li>Ein Arrayelement kann nicht leer sein</li><li>Ungültige Kategorie</li><li>Ausgangssprache ist ungültig</li><li>Zielsprache ist ungültig</li><li>Die Anforderung enthält zu viele Elemente</li><li>Die Ausgangssprache (From) wird nicht unterstützt</li><li>Die Zielsprache wird nicht unterstützt</li><li>Die Übersetzungsanforderung verfügt über zu viele Daten</li><li>HTML liegt nicht im richtigen Format vor</li><li>Zu viele Zeichenfolgen wurde in der Übersetzungsanforderung übergeben</li></ul>|
|401    |Ungültige Anmeldeinformationen|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader „X-MS-Trans-Info“ enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Ruft die Anzeigenamen für die Sprachen ab, die als Parameter `languageCodes` übergeben werden. Die Anzeigenamen werden mit der übergebenen Gebietsschemasprache lokalisiert.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

Der Anforderungstext enthält ein Zeichenfolgenarray, das die Sprachencodes der ISO 639-1 darstellt, für die die Anzeigenamen abgerufen werden. Beispiel:

```
<ArrayOfstring xmlns:i="https://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Rückgabewert:** Ein Zeichenfolgenarray, das Sprachnamen enthält, die vom Übersetzungsdienst unterstützt werden und in die angeforderte Sprache übersetzt werden.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Ein Zeichenfolgenarray, das Sprachnamen enthält, die vom Übersetzungsdienst unterstützt werden und in die angeforderte Sprache übersetzt werden.

Zeichenfolge

Anforderungsinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der Header `Authorization` oder `Ocp-Apim-Subscription-Key` verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|locale|(leer) |Erforderlich. Eine Zeichenfolge, die eine Kombination aus einem Kulturcode aus zwei Kleinbuchstaben (laut ISO 639) darstellt, die mit einer Sprache und einem Subkulturcode aus zwei Großbuchstaben (laut ISO 3166) in Verbindung steht, um die Sprachnamen oder einen eigenständigen ISO 639-Kulturcode in Kleinbuchstaben zu lokalisieren.|query|Zeichenfolge|
|Authorization|(leer)  |Erforderlich, falls das Feld „appid“ oder der Header `Ocp-Apim-Subscription-Key` nicht angegeben ist. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)  |Erforderlich, falls das Feld „appid“ oder der Header `Authorization` nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader „X-MS-Trans-Info“ enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-getlanguagesfortranslate"></a>GET /GetLanguagesForTranslate

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Rufen Sie eine Liste von Sprachcodes ab, die die Sprachen darstellen, die vom Übersetzungsdienst unterstützt werden.  `Translate` und `TranslateArray` können zwischen beliebigen zwei Sprachen aus der Liste übersetzen.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Rückgabewert:** Ein Zeichenfolgenarray, das die Sprachcodes enthält, die vom Übersetzungsdienst unterstützt werden.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Ein Zeichenfolgenarray, das die Sprachcodes enthält, die vom Übersetzungsdienst unterstützt werden.

Zeichenfolge

Anforderungsinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der Header `Authorization` oder `Ocp-Apim-Subscription-Key` verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|Authorization|(leer)  |Erforderlich, falls das Feld `appid` oder der Header `Ocp-Apim-Subscription-Key` nicht angegeben ist. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, falls das Feld `appid` oder der Header `Authorization` nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader „X-MS-Trans-Info“ enthalten ist.|
|503|Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-getlanguagesforspeak"></a>GET /GetLanguagesForSpeak

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Ruft die verfügbaren Sprachen für die Sprachsynthese ab.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Rückgabewert:** Ein Zeichenfolgenarray, das die Sprachcodes enthält, die vom Übersetzungsdienst für die Sprachsynthese unterstützt werden.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Ein Zeichenfolgenarray, das die Sprachcodes enthält, die vom Übersetzungsdienst für die Sprachsynthese unterstützt werden.

Zeichenfolge

Anforderungsinhaltstyp: application/xml

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der Header `Authorization` oder `Ocp-Apim-Subscription-Key` verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|Authorization|(leer)|Erforderlich, falls das Feld `appid` oder der Header `Ocp-Apim-Subscription-Key` nicht angegeben ist. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, falls das Feld `appid` oder der Header `Authorization` nicht angegeben ist.|Header|Zeichenfolge|
 
### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400|Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401|Ungültige Anmeldeinformationen|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader `X-MS-Trans-Info` enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-speak"></a>GET /Speak

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Gibt einen Wave- oder MP3-Stream des übergebenen Texts zurück, der in der gewünschten Sprache gesprochen wird.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Rückgabewert:** Eine Wave- oder MP3-Stream des übergebenen Texts, der in der gewünschten Sprache gesprochen wird.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)

binary

Anforderungsinhaltstyp: application/xml 

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der Header `Authorization` oder `Ocp-Apim-Subscription-Key` verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|text|(leer)   |Erforderlich. Eine Zeichenfolge, die einen Satz oder mehrere Sätze in der angegebenen Sprache enthält, in der der Wave-Stream wiedergegeben werden soll. Die Größe des gesprochenen Textes darf 2000 Zeichen nicht überschreiten.|query|Zeichenfolge|
|language|(leer)   |Erforderlich. Eine Zeichenfolge, die den Code der unterstützten Sprache darstellt, in die der Text gesprochen werden soll. Der Code muss in der Liste der Codes aufgeführt sein, die von der Methode `GetLanguagesForSpeak` zurückgegeben werden.|query|Zeichenfolge|
|format|(leer)|Optional. Eine Zeichenfolge, die die Inhaltstyp-ID zurückgibt. Derzeit sind `audio/wav` und `audio/mp3` verfügbar. Standardwert: `audio/wav`.|query|Zeichenfolge|
|options|(leer)    |<ul><li>Optional. Eine Zeichenfolge, die Eigenschaften der synthetisierten Spracheingabe angibt.<li>`MaxQuality` und `MinSize` sind verfügbar, um die Qualität der Audiosignale anzugeben. Mit `MaxQuality` erhalten Sie eine hochqualitative Stimmwiedergabe, und mit `MinSize` erhalten Sie eine Stimmwiedergabe von niedrigerer Qualität. Der Standardwert ist `MinSize`.</li><li>`female` und `male` sind verfügbar, um das gewünschte Geschlecht bei der Sprachwiedergabe zu bestimmen. Der Standardwert ist `female`. Verwenden Sie den senkrechten Strich <code>\|</code>, um mehrere Optionen einzuschließen. Beispiel: `MaxQuality|Male`.</li></li></ul> |query|Zeichenfolge|
|Authorization|(leer)|Erforderlich, falls das Feld `appid` oder der Header `Ocp-Apim-Subscription-Key` nicht angegeben ist. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)  |Erforderlich, falls das Feld `appid` oder der Header `Authorization` nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader `X-MS-Trans-Info` enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-detect"></a>GET /Detect

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Verwenden Sie die `Detect`-Methode, um die Sprache einer ausgewählten Textpassage zu identifizieren.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Rückgabewert:** Eine Zeichenfolge, die einen aus zwei Zeichen bestehenden Sprachcode für den angegebenen Text enthält. .

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)

Zeichenfolge

Anforderungsinhaltstyp: application/xml

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)  |Erforderlich. Wenn der Header `Authorization` oder `Ocp-Apim-Subscription-Key` verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|text|(leer)|Erforderlich. Eine Zeichenfolge, die Text enthält, dessen Sprache identifiziert werden soll. Die Textgröße darf 10.000 Zeichen nicht überschreiten.|query| Zeichenfolge|
|Authorization|(leer)|Erforderlich, falls das Feld `appid` oder der Header `Ocp-Apim-Subscription-Key` nicht angegeben ist. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key  |(leer)    |Erforderlich, falls das Feld `appid` oder der Header `Authorization` nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400|Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader `X-MS-Trans-Info` enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|


## <a name="post-detectarray"></a>POST /DetectArray

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Verwenden Sie die `DetectArray`-Methode, um die Sprache eines Zeichenfolgenarrays zu identifizieren. Es werden unabhängige Erkennungsvorgänge für jedes Arrayelement durchgeführt, und es wird ein Ergebnis für jede Arrayzeile zurückgegeben.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

Das Format des Anforderungstexts sollte wie Folgt aussehen.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

Die Textgröße darf 10.000 Zeichen nicht überschreiten.

**Rückgabewert:** Ein Zeichenfolgenarray, das aus zwei Zeichen bestehende Sprachcodes für jede Zeile des Eingabearrays enthält.

Das Format des Antworttexts sieht wie folgt aus.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
DetectArray war erfolgreich. Es wird ein Zeichenfolgenarray zurückgegeben, das Sprachcodes mit zwei Zeichen für jede Zeile des Eingabearrays enthält.

Zeichenfolge

Anforderungsinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der Header `Authorization` oder `Ocp-Apim-Subscription-Key` verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|Authorization|(leer)|Erforderlich, falls das Feld `appid` oder der Header `Ocp-Apim-Subscription-Key` nicht angegeben ist. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, falls das Feld `appid` oder der Header „Authorization“ nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader „X-MS-Trans-Info“ enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-addtranslation"></a>GET /AddTranslation

### <a name="implementation-notes"></a>Hinweise zur Implementierung

> [!IMPORTANT]
> **ABLAUFWARNUNG:** Nach dem 31. Januar 2018 akzeptiert diese Methode keine neuen Einreichungen von Sätzen, und Sie erhalten eine Fehlermeldung. Sehen Sie sich diese Ankündigung bzgl. der Änderungen an den Funktionen der kollaborativen Übersetzung an.

Es wird eine Übersetzung zum Translation Memory hinzugefügt.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)

Zeichenfolge

Anforderungsinhaltstyp: application: xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp   |
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der Header `Authorization` oder `Ocp-Apim-Subscription-Key` verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|originalText|(leer)|Erforderlich. Eine Zeichenfolge, die Text enthält, aus dem übersetzt wird. Die Zeichenfolge besitzt eine maximale Länge von 1000 Zeichen.|query|Zeichenfolge|
|translatedText|(leer) |Erforderlich. Eine Zeichenfolge, die übersetzten Text in der Zielsprache enthält. Die Zeichenfolge besitzt eine maximale Länge von 2000 Zeichen.|query|Zeichenfolge|
|from|(leer)   |Erforderlich. Eine Zeichenfolge, die den Sprachcode des Übersetzungstexts darstellt. en = englisch, de = deutsch etc...|query|Zeichenfolge|
|zu|(leer)|Erforderlich. Eine Zeichenfolge, die den Code der Sprache darstellt, in die der Text übersetzt werden soll.|query|Zeichenfolge|
|rating|(leer) |Optional. Ein Integer, der die Qualitätsbewertung für diese Zeichenfolge darstellt. Ein Wert zwischen –10 and 10. Der Standardwert lautet 1.|query|integer|
|contentType|(leer)    |Optional. Das Format des Texts, der übersetzt wird. Die unterstützten Formate sind „text/plain“ und „text/html“. Jede HTML muss ein wohlgeformtes vollständiges Element sein.   |query|Zeichenfolge|
|category|(leer)|Optional. Eine Zeichenfolge, die die Kategorie (Domäne) der Übersetzung enthält. Der Standardwert lautet „general“.|query|Zeichenfolge|
|user|(leer)|Erforderlich. Eine Zeichenfolge, die zur Nachverfolgung des Erstellers der Einreichung verwendet wird.|query|Zeichenfolge|
|uri|(leer)|Optional. Eine Zeichenfolge, die den Inhaltsspeicherort dieser Übersetzung enthält.|query|Zeichenfolge|
|Authorization|(leer)|Erforderlich, falls das Feld „appid“ oder der Header `Ocp-Apim-Subscription-Key` nicht angegeben ist. Autorisierungstoken: `"Bearer" + " " + "access_token"`    |Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, falls das Feld `appid` oder der Header `Authorization` nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen|
|410|AddTranslation wird nicht mehr unterstützt.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader „X-MS-Trans-Info“ enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="post-addtranslationarray"></a>POST /AddTranslationArray

### <a name="implementation-notes"></a>Hinweise zur Implementierung

> [!IMPORTANT]
> **ABLAUFWARNUNG:** Nach dem 31. Januar 2018 akzeptiert diese Methode keine neuen Einreichungen von Sätzen, und Sie erhalten eine Fehlermeldung. Sehen Sie sich diese Ankündigung bzgl. der Änderungen an den Funktionen der kollaborativen Übersetzung an.

Es wird ein Array von Übersetzungen hinzugefügt, für die Translation Memory hinzugefügt werden soll. Diese ist eine Arrayversion von `AddTranslation`.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`.

Das Format des Anforderungstexts sieht wie folgt aus.

```
<AddtranslationsRequest>
  <AppId></AppId>
  <From>A string containing the language code of the source language</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
  </Options>
  <To>A string containing the language code of the target language</To>
  <Translations>
    <Translation xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
      <OriginalText>string-value</OriginalText>
      <Rating>int-value</Rating>
      <Sequence>int-value</Sequence>
      <TranslatedText>string-value</TranslatedText>
    </Translation>
  </Translations>
</AddtranslationsRequest>
```

Elemente innerhalb des AddtranslationsRequest-Elements sind:

* `AppId`: Erforderlich. Wenn der Header `Authorization` und `Ocp-Apim-Subscription-Key` verwendet wird, lassen Sie das Feld „appid“ leer, und fügen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.
* `From`: Erforderlich. Eine Zeichenfolge, die den Sprachcode der Quellsprache enthält. Sie muss in einer der Sprachen sein, die von der `GetLanguagesForTranslate`-Methode zurückgegeben wird.
* `To`: Erforderlich. Eine Zeichenfolge, die den Sprachcode der Zielsprache enthält. Sie muss in einer der Sprachen sein, die von der `GetLanguagesForTranslate`-Methode zurückgegeben wird.
* `Translations`: Erforderlich. Ein Array von Übersetzungen das dem Translation Memory hinzugefügt wird. Jede Übersetzung muss Folgendes enthalten: originalText, translatedText und Bewertung. Die Größe der Werte originalText und translatedText ist auf 1000 Zeichen beschränkt. Die Gesamtzeichenanzahl aller Quelltexte (originalText) und Zieltexte (translatedText) darf 10.000 Zeichen nicht überschreiten. Die maximale Anzahl von Arrayelementen beträgt 100.
* `Options`: Erforderlich. Eine Reihe von Optionen, einschließlich Category (Kategorie), ContentType, Uri und User. Der Benutzer (User) ist erforderlich. Category, ContentType und Uri sind optional. Angegebene Element müssen in alphabetischer Reihenfolge aufgelistet werden.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Die Methode AddTranslationArray war erfolgreich. Nach dem 31. Januar 2018 werden keine Einreichungen von Sätzen mehr akzeptiert. Dieser Dienst reagiert dann mit dem Fehlercode 410.

Zeichenfolge

Anforderungsinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|Authorization|(leer)|Erforderlich, falls das Feld „appid“ oder der Header „Ocp-Apim-Subscription-Key“ nicht angegeben ist. Autorisierungstoken:  „Bearer“ + „ „ + „access_token“.|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, falls das Feld „appid“ oder der Header „Authorization“ nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen|
|410    |AddTranslation wird nicht mehr unterstützt.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader `X-MS-Trans-Info` enthalten ist.|
|503|Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-breaksentences"></a>GET /BreakSentences

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Unterteilt eine Textpassage in einzelne Sätze und gibt ein Array zurück, das die Längenwerte in jedem Satz enthält.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Rückgabewert:** Ein Integerarray stellt die Länge der Sätze dar. Die Länge des Arrays stellt die Anzahl von Sätzen dar, und die Werte stehen jeweils für die Länge der einzelnen Sätze.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Ein Integerarray stellt die Länge der Sätze dar. Die Länge des Arrays stellt die Anzahl von Sätzen dar, und die Werte stehen jeweils für die Länge der einzelnen Sätze.

integer

Anforderungsinhaltstyp: application/xml 

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)  |Erforderlich. Wenn der Header „Authorization“ oder „Ocp-Apim-Subscription-Key“ verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die „Bearer“ + „ „ (Leerzeichen) + „access_token“ enthält.|query| Zeichenfolge|
|text|(leer)   |Erforderlich. Eine Zeichenfolge, die den Text darstellt, der in Sätze aufgeteilt werden soll. Die Textgröße darf 10.000 Zeichen nicht überschreiten.|query|Zeichenfolge|
|language   |(leer)    |Erforderlich. Eine Zeichenfolge, die den Sprachcode des Eingabetexts darstellt.|query|Zeichenfolge|
|Authorization|(leer)|Erforderlich, falls das Feld „appid“ oder der Header „Ocp-Apim-Subscription-Key“ nicht angegeben ist. Autorisierungstoken:  „Bearer“ + „ „ + „access_token“.    |Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, falls das Feld „appid“ oder der Header „Authorization“ nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400|Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401|Ungültige Anmeldeinformationen|
|500|Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader „X-MS-Trans-Info“ enthalten ist.|
|503|Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Ruft ein Array von Übersetzungen für ein gegebenes Sprachpaar aus dem Speicher und der MT-Engine ab. GetTranslations unterscheidet sich vom Übersetzungsdienst, da alle verfügbaren Übersetzungen zurückgegeben werden.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`.

Der Anforderungstext enthält das optional TranslationOptions-Objekt, das folgendes Format aufweist.

```
<TranslateOptions xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
  <Category>string-value</Category>
  <ContentType>text/plain</ContentType>
  <ReservedFlags></ReservedFlags>
  <State>int-value</State>
  <Uri>string-value</Uri>
  <User>string-value</User>
</TranslateOptions>
```

Das `TranslateOptions`-Objekt enthält die unten aufgeführten Werte. Diese sind alle optional und sind Standardwerte für die häufigsten Einstellungen. Angegebene Element müssen in alphabetischer Reihenfolge aufgelistet werden.

* `Category`: Eine Zeichenfolge, die die Kategorie (Domäne) der Übersetzung enthält. Der Standardwert lautet „general“.
* `ContentType`: „text/plain“ ist die einzige unterstützte bzw. die Standardversion.
* `IncludeMultipleMTAlternatives`: boolesches Flag zur Bestimmung, ob mehr als eine Alternative von der MT-Engine zurückgegeben werden soll. Gültige Werte sind „true“ und „false“ (mit Berücksichtigung der Groß- und Kleinschreibung). Der Standardwert ist „false“ und enthält nur eine Alternative. Wenn Sie das Flag auf „true“ festlegen, ist die Erstellung künstlicher Alternativen in der Übersetzung möglich, die vollständig in das Framework für kollaborative Übersetzungen (Collaborative Translations Framework, CTF) integriert sind. Das Feature lässt zu, dass Alternativen für Sätze zurückgegeben werden dürfen, die keine Alternativen im CTF besitzen. Dies geschieht durch Hinzufügen künstlicher Alternativen aus der N-besten Liste des Decoders.
    - Bewertungen: Die Bewertungen werden wie folgt angewendet: 1) Die beste automatische Übersetzung hat eine Bewertung von 5. 2) Die Alternativen von CTF spiegeln die Autorität des Reviewers von –10 bis +10 wieder. 3) Die automatisch generierten (N-beste) Übersetzungsalternativen verfügen über eine Bewertung von 0 und einen Übereinstimmungsgrad von 100.
    - Anzahl der Alternativen: Die Anzahl zurückgegebener Alternativen geht bis zu „maxTranslations“, es kann aber auch weniger sein.
    - Sprachpaare: Diese Funktion ist für Übersetzungen zwischen einfachem und traditionellem Chinesisch in beiden Richtungen nicht verfügbar. Sie ist für alle anderen von Microsoft Translator unterstützten Sprachenpaare verfügbar.
* `State`: Benutzerstatus, mit dem Anforderung und Antwort korreliert werden können. Die gleichen Inhalte werden in der Antwort zurückgegeben.
* `Uri`: Filterergebnisse von diesem URI. Wenn kein Wert festgelegt ist, lautet der Standardwert „alle“.
* `User`: Filterergebnisse von diesem Benutzer. Wenn kein Wert festgelegt ist, lautet der Standardwert „alle“.

Anforderung `Content-Type` sollte `text/xml` sein.

**Rückgabewert:** Das Format der Antwort sieht folgendermaßen aus.

```
<GetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <From>Two character language code</From>
  <State/>
  <Translations>
    <TranslationMatch>
      <Count>int-value</Count>
      <MatchDegree>int-value</MatchDegree>
      <MatchedOriginalText/>
      <Rating>int value</Rating>
      <TranslatedText>string-value</TranslatedText>
    </TranslationMatch>
  </Translations>
</GetTranslationsResponse>
```

Dies schließt ein `GetTranslationsResponse`-Element ein, das folgende Werte enthält:

* `Translations`: Ein Array gefundener Übereinstimmungen, gespeichert in TranslationMatch-Objekten (siehe unten). Die Übersetzungen können leichte Abweichungen des Originaltexts enthalten (Fuzzyübereinstimmung). Die Übersetzungen werden folgendermaßen sortiert: 100 %-Übereinstimmungen zuerst, darunter Fuzzyübereinstimmungen.
* `From`: Wenn die Methode keine Ausgangssprache angegeben hat, ist dies das Ergebnis der automatischen Spracherkennung. Andernfalls wird die Ausgangssprache angegebenen.
* `State`: Benutzerstatus, mit dem Anforderung und Antwort korreliert werden können. Enthält den gleichen Wert, der auch im TranslateOptions-Parameter angegeben ist.

Das TranslationMatch-Objekt enthält Folgendes:

* `Error`: Wenn ein Fehler für eine bestimmte Eingabezeichenfolge aufgetreten ist, wird der Fehlercode gespeichert. Andernfalls bleibt das Feld leer.
* `MatchDegree`: Das System gleicht eingegebene Sätze (einschließlich ungenauer Übereinstimmungen) mit dem Speicher ab.  MatchDegree gibt an, wie eng der Eingabetext mit dem Originaltext, der im Speicher gefunden wurde, übereinstimmt. Der Wert gibt eine Spanne von 0 bis 100 zurück, wobei 0 keine Ähnlichkeit aufweist und 100 eine Übereinstimmung mit der Schreibweise ist.
MatchedOriginalText: Originaltext, der für dieses Ergebnis übereinstimmte. Er wird nur zurückgegeben, wenn der übereinstimmende Originaltext sich vom Eingabetext unterschieden hat. Wird verwendet, um den Quelltext einer Fuzzyübereinstimmung zurückzugeben. Wir nicht für Microsoft Translator-Ergebnisse zurückgegeben.
* `Rating`: Gibt die Autorität der Person an, die die Entscheidung über die Qualität trifft. Ergebnisse der maschinellen Übersetzung haben eine Bewertung von 5. Anonym bereitgestellte Übersetzungen haben generelle einen Bewertungsrahmen von 1 bis 4, während autoritativ bereitgestellte Übersetzungen einen Bewertungsrahmen von 6 bis 10 besitzen.
* `Count`: Die Häufigkeit, mit der diese Übersetzung mit dieser Bewertung ausgewählt wurde. Für die automatisch übersetzte Antwort ist der Wert 0.
* `TranslatedText`: Der übersetzte Text.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Ein `GetTranslationsResponse`-Objekt im oben beschriebenen Format.

Zeichenfolge

Anforderungsinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der Header `Authorization` oder `Ocp-Apim-Subscription-Key` verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|text|(leer)|Erforderlich. Eine Zeichenfolge, die den Text darstellt, der übersetzt werden soll. Die Textgröße darf 10.000 Zeichen nicht überschreiten.|query|Zeichenfolge|
|from|(leer)|Erforderlich. Eine Zeichenfolge, die den Sprachcode des Übersetzungstexts darstellt.|query|Zeichenfolge|
|zu |(leer)    |Erforderlich. Eine Zeichenfolge, die den Code der Sprache darstellt, in die der Text übersetzt werden soll.|query|Zeichenfolge|
|maxTranslations|(leer)|Erforderlich. Ein Integer, der die Höchstanzahl der Übersetzungen darstellt, die zurückgegeben werden sollen.|query|integer|
|Authorization| (leer)|Erforderlich, falls das Feld `appid` oder der Header `Ocp-Apim-Subscription-Key` nicht angegeben ist. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Zeichenfolge| Header|
|Ocp-Apim-Subscription-Key|(leer)  |Erforderlich, falls das Feld `appid` oder der Header `Authorization` nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader `X-MS-Trans-Info` enthalten ist.|
|503|Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="post-gettranslationsarray"></a>POST /GetTranslationsArray

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Verwenden Sie die `GetTranslationsArray`-Methode, um mehrere Übersetzungskandidaten für mehrere Quelltexte abzurufen.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`.

Das Format des Anforderungstexts sieht wie folgt aus.

```
<GetTranslationsArrayRequest>
  <AppId></AppId>
  <From>language-code</From>
  <MaxTranslations>int-value</MaxTranslations>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"/>
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</GetTranslationsArrayRequest>
```

`GetTranslationsArrayRequest` enthält folgende Elemente:

* `AppId`: Erforderlich. Wenn Autorisierungsheader verwendet wird, lassen Sie das Feld „appid“ leer, oder schließen Sie eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.
* `From`: Erforderlich. Eine Zeichenfolge, die den Sprachcode des Übersetzungstexts darstellt.
* `MaxTranslations`: Erforderlich. Ein Integer, der die Höchstanzahl der Übersetzungen darstellt, die zurückgegeben werden sollen.
* `Options`: Optional. Ein Options-Objekt, das die unten aufgeführten Werte enthält. Diese sind alle optional und sind Standardwerte für die häufigsten Einstellungen. Angegebene Element müssen in alphabetischer Reihenfolge aufgelistet werden.
    - Category` (Kategorie): Eine Zeichenfolge, die die Kategorie (Domäne) der Übersetzung enthält. Der Standardwert lautet „general“.
    - `ContentType`: „text/plain“ ist die einzige unterstützte bzw. die Standardversion.
    - `IncludeMultipleMTAlternatives`: boolesches Flag zur Bestimmung, ob mehr als eine Alternative von der MT-Engine zurückgegeben werden soll. Gültige Werte sind „true“ und „false“ (mit Berücksichtigung der Groß- und Kleinschreibung). Der Standardwert ist „false“ und enthält nur eine Alternative. Wenn Sie das Flag auf „true“ festlegen, ist die Erstellung künstlicher Alternativen in der Übersetzung möglich, die vollständig in das Framework für kollaborative Übersetzungen (Collaborative Translations Framework, CTF) integriert sind. Das Feature lässt zu, dass Alternativen für Sätze zurückgegeben werden dürfen, die keine Alternativen im CTF besitzen. Dies geschieht durch Hinzufügen künstlicher Alternativen aus der N-besten Liste des Decoders.
        - Bewertungen: Die Bewertungen werden wie folgt angewendet: 1) Die beste automatische Übersetzung hat eine Bewertung von 5. 2) Die Alternativen von CTF spiegeln die Autorität des Reviewers von –10 bis +10 wieder. 3) Die automatisch generierten (N-beste) Übersetzungsalternativen verfügen über eine Bewertung von 0 und einen Übereinstimmungsgrad von 100.
        - Anzahl der Alternativen: Die Anzahl zurückgegebener Alternativen geht bis zu „maxTranslations“, es kann aber auch weniger sein.
        - Sprachpaare: Diese Funktion ist für Übersetzungen zwischen einfachem und traditionellem Chinesisch in beiden Richtungen nicht verfügbar. Sie ist für alle anderen von Microsoft Translator unterstützten Sprachenpaare verfügbar.
* `State`: Benutzerstatus, mit dem Anforderung und Antwort korreliert werden können. Die gleichen Inhalte werden in der Antwort zurückgegeben.
* `Uri`: Filterergebnisse von diesem URI. Wenn kein Wert festgelegt ist, lautet der Standardwert „alle“.
* `User`: Filterergebnisse von diesem Benutzer. Wenn kein Wert festgelegt ist, lautet der Standardwert „alle“.
* `Texts`: Erforderlich. Eine Zeichenfolge, die Texte für die Übersetzung enthält. Alle Zeichenfolgen müssen in der gleichen Sprache sein. Der Gesamtwert aller Texte, die übersetzt werden sollen, darf 10.000 Zeichen nicht übersteigen. Die maximale Anzahl von Arrayelementen beträgt 10.
* `To`: Erforderlich. Eine Zeichenfolge, die den Code der Sprache darstellt, in die der Text übersetzt werden soll.

Optionale Elemente können ausgelassen werden. Elemente, die direkte untergeordnete Elemente von `GetTranslationsArrayRequest` sind, müssen in alphabetischer Reihenfolge aufgeführt sein.

Anforderung `Content-Type` sollte `text/xml` sein.

**Rückgabewert:** Das Format der Antwort sieht folgendermaßen aus.

```
<ArrayOfGetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <GetTranslationsResponse>
    <From>language-code</From>
    <State/>
    <Translations>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText>string-value</MatchedOriginalText>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText/>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
    </Translations>
  </GetTranslationsResponse>
</ArrayOfGetTranslationsResponse>
```

Jedes `GetTranslationsResponse`-Element enthält die folgenden Werte:

* `Translations`: Ein Array gefundener Übereinstimmungen, gespeichert in `TranslationMatch`-Objekten (siehe unten). Die Übersetzungen können leichte Abweichungen des Originaltexts enthalten (Fuzzyübereinstimmung). Die Übersetzungen werden folgendermaßen sortiert: 100 %-Übereinstimmungen zuerst, darunter Fuzzyübereinstimmungen.
* `From`: Wenn die Methode keine `From`-Sprache angegeben hat, ist dies das Ergebnis der automatischen Spracherkennung. Andernfalls wird die Ausgangssprache angegebenen.
* `State`: Benutzerstatus, mit dem Anforderung und Antwort korreliert werden können. Enthält den gleichen Wert, der auch im `TranslateOptions`-Parameter angegeben ist.

`TranslationMatch`-Objekte bestehen aus Folgendem:
* `Error`: Wenn ein Fehler für eine bestimmte Eingabezeichenfolge aufgetreten ist, wird der Fehlercode gespeichert. Andernfalls bleibt das Feld leer.
* `MatchDegree`: Das System gleicht eingegebene Sätze (einschließlich ungenauer Übereinstimmungen) mit dem Speicher ab.  `MatchDegree` gibt an, wie nah der eingegebene Text mit dem Originaltext übereinstimmt, der im Speicher gefunden wurde. Der Wert gibt eine Spanne von 0 bis 100 zurück, wobei 0 keine Ähnlichkeit aufweist und 100 eine Übereinstimmung mit der Schreibweise ist.
* `MatchedOriginalText`: Originaltext, der für dieses Ergebnis übereinstimmte. Er wird nur zurückgegeben, wenn der übereinstimmende Originaltext sich vom Eingabetext unterschieden hat. Wird verwendet, um den Quelltext einer Fuzzyübereinstimmung zurückzugeben. Wir nicht für Microsoft Translator-Ergebnisse zurückgegeben.
* `Rating`: Gibt die Autorität der Person an, die die Entscheidung über die Qualität trifft. Ergebnisse der maschinellen Übersetzung haben eine Bewertung von 5. Anonym bereitgestellte Übersetzungen haben generelle einen Bewertungsrahmen von 1 bis 4, während autoritativ bereitgestellte Übersetzungen einen Bewertungsrahmen von 6 bis 10 besitzen.
* `Count`: Die Häufigkeit, mit der diese Übersetzung mit dieser Bewertung ausgewählt wurde. Für die automatisch übersetzte Antwort ist der Wert 0.
* `TranslatedText`: Der übersetzte Text.


### <a name="response-class-status-200"></a>Antwortklasse (Status 200)

Zeichenfolge

Anforderungsinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|Authorization  |(leer)    |Erforderlich, falls das Feld `appid` oder der Header `Ocp-Apim-Subscription-Key` nicht angegeben ist. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)  |Erforderlich, falls das Feld `appid` oder der Header `Authorization` nicht angegeben ist.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader `X-MS-Trans-Info` enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Referenz für Version 3 der Textübersetzungs-API](../migrate-to-v3.md)










