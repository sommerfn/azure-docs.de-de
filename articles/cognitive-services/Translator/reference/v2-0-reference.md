---
title: Microsoft Translator-Text-API Version 2.0
titleSuffix: Azure Cognitive Services
description: Referenzdokumentation für die Textübersetzungs-API, Version 2.0
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/15/2018
ms.author: swmachan
ms.openlocfilehash: f111169558118a80602bcb2136bc63ce54c9e0d9
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72242500"
---
# <a name="translator-text-api-v20"></a>Microsoft Translator-Text-API Version 2.0

> [!IMPORTANT]
> Diese Version der Textübersetzungs-API ist als veraltet markiert. [Sehen Sie sich die Dokumentation für Version 3 der Textübersetzungs-API an](v3-0-reference.md).

Die Textübersetzungs-API, Version 2, kann nahtlos in Ihre Apps, Websites, Tools oder andere Lösungen integriert werden, damit Benutzererlebnisse in mehreren Sprachen zur Verfügung stehen. Sie können sie auf jeder Hardwareplattform und mit jedem Betriebssystem verwenden, um Sprachübersetzungen und andere sprachbezogene Vorgänge wie Textsprachenerkennung oder Sprachsynthese nach Industriestandards durchzuführen. Weitere Informationen finden Sie unter [Textübersetzungs-API](../translator-info-overview.md).

## <a name="getting-started"></a>Erste Schritte
Für den Zugriff auf die Textübersetzungs-API müssen Sie sich bei [Microsoft Azure registrieren](../translator-text-how-to-signup.md).

## <a name="authentication"></a>Authentication 
Alle Aufrufe der Textübersetzungs-API erfordern einen Abonnementschlüssel für die Authentifizierung. Die API unterstützt drei Authentifizierungsmethoden:

- Ein Zugriffstoken. Verwenden Sie den Abonnementschlüssel, um ein Zugriffstoken zu generieren. Senden Sie dazu eine POST-Anforderung an den Authentifizierungsdienst. Ausführliche Informationen dazu finden Sie in der Tokendienst-Dokumentation. Übergeben Sie das Zugriffstoken mithilfe des `Authorization`-Headers oder des `access_token`-Abfrageparameters an den Übersetzerdienst. Das Zugriffstoken ist 10 Minuten lang gültig. Rufen Sie alle 10 Minuten ein neues Zugriffstoken ab, und verwenden Sie innerhalb dieser 10 Minuten für wiederholte Anforderungen weiterhin dasselbe Zugriffstoken.
- Direkte Verwendung eines Abonnementschlüssels. Übergeben Sie Ihren Abonnementschlüssel als Wert im `Ocp-Apim-Subscription-Key`-Header, der in Ihrer Anforderung an die Textübersetzungs-API enthalten ist. Wenn Sie den Abonnementschlüssel direkt verwenden, müssen Sie nicht zum Erstellen eines Zugriffstokens den Tokenauthentifizierungsdienst aufrufen.
- Ein [Azure Cognitive Services-Abonnement für mehrere Dienste](https://azure.microsoft.com/pricing/details/cognitive-services/). Mit dieser Methode können Sie einen einzelnen geheimen Schlüssel verwenden, um Anforderungen für mehrere Dienste zu authentifizieren.
Wenn Sie einen geheimen Multi-Service-Schlüssel verwenden, müssen Sie der Anforderung zwei Authentifizierungsheader hinzufügen. Der erste Header übergibt den geheimen Schlüssel. Der zweite Header gibt die Region an, die Ihrem Abonnement zugeordnet ist:
   - `Ocp-Apim-Subscription-Key`
   - `Ocp-Apim-Subscription-Region`

Die Region ist für das Abonnement der Multi-Service-Text-API erforderlich. Die ausgewählte Region ist die einzige Region, die Sie bei Verwendung des Multi-Service-Abonnementschlüssels für die Textübersetzung verwenden können. Es muss die gleiche Region sein, die Sie ausgewählt haben, als Sie sich im Azure-Portal für das Multi-Service-Abonnement registriert haben.

Diese Regionen sind verfügbar: `australiaeast`, `brazilsouth`, `canadacentral`, `centralindia`, `centraluseuap`, `eastasia`, `eastus`, `eastus2`, `japaneast`, `northeurope`, `southcentralus`, `southeastasia`, `uksouth`, `westcentralus`, `westeurope`, `westus` und `westus2`.

Ihr Abonnementschlüssel und Zugriffstoken sind geheime Schlüssel, die nicht in der Ansicht dargestellt werden sollten.

## <a name="profanity-handling"></a>Behandlung von Obszönitäten
Im Quelltext vorhandene Obszönitäten werden vom Übersetzungsdienst normalerweise nicht entfernt. Der Grad an Obszönitäten und der Kontext, bei dem Wörter als obszön angesehen werden, unterscheiden sich je nach Kultur. Daher kann der Obszönitätsgrad in der Zielsprache unter Umständen erhöht oder reduziert werden.

Wenn Sie keine Obszönitäten in der Übersetzung wünschen, auch wenn diese im Quelltext vorhanden sind, können Sie die Obszönitäten-Filteroption für die Methoden verwenden, die diese unterstützen. Mit dieser Option können Sie auswählen, ob Obszönitäten gelöscht oder entsprechend gekennzeichnet werden sollen oder ob sie in der Zielsprache zugelassen werden sollen. Die akzeptierten Werte von `ProfanityAction` sind `NoAction` (Standard), `Marked` und `Deleted`.


|ProfanityAction    |Aktion |Beispielquelltext (Japanisch)  |Beispielübersetzung (Englisch)  |
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

Antwortinhaltstyp: application/xml

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG    |Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid  |(leer)    |Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `appid` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|text|(leer)   |Erforderlich. Eine Zeichenfolge, die den zu übersetzenden Text darstellt. Der Text darf nicht mehr als 10.000 Zeichen enthalten.|query|Zeichenfolge|
|from|(leer)   |Optional. Eine Zeichenfolge, die den Sprachcode der Sprache darstellt, aus der der Text übersetzt werden soll. Z.B. „en“ für Englisch|query|Zeichenfolge|
|zu|(leer) |Erforderlich. Eine Zeichenfolge, die den Sprachcode der Sprache darstellt, in die der Text übersetzt werden soll.|query|Zeichenfolge|
|contentType|(leer)    |Optional. Das Format des Texts, der übersetzt wird. Die unterstützten Formate sind `text/plain` (Standard) und `text/html`. HTML-Elemente müssen wohlgeformt und vollständig sein.|query|Zeichenfolge|
|category|(leer)   |Optional. Eine Zeichenfolge, die die Kategorie (Domäne) der Übersetzung enthält. Der Standardwert lautet `general`.|query|Zeichenfolge|
|Authorization|(leer)  |Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)  |Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|


### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="post-translatearray"></a>POST /TranslateArray

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Ruft Übersetzungen für mehrere Quelltexte ab.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

Der Anforderungstext hat das folgende Format:

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

Diese Elemente befinden sich in `TranslateArrayRequest`:


* `AppId`: Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `AppId` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.
* `From`: Optional. Eine Zeichenfolge, die den Sprachcode der Sprache darstellt, aus der der Text übersetzt werden soll. Wenn das Element leer gelassen wird, enthält die Antwort das Ergebnis der automatischen Spracherkennung.
* `Options`: Optional. Ein `Options`-Objekt, das die folgenden Werte enthält. Diese sind alle optional und entsprechen den Standardwerten für die häufigsten Einstellungen. Angegebene Element müssen in alphabetischer Reihenfolge aufgelistet werden.
    - `Category`: Eine Zeichenfolge, die die Kategorie (Domäne) der Übersetzung enthält. Der Standardwert lautet `general`.
    - `ContentType`: Das Format des Texts, der übersetzt wird. Die unterstützten Formate sind `text/plain` (Standard), `text/xml` und `text/html`. HTML-Elemente müssen wohlgeformt und vollständig sein.
    - `ProfanityAction`: Gibt an, wie Obszönitäten behandelt werden (wie oben beschrieben). Akzeptierte Werte sind `NoAction` (Standard), `Marked` und `Deleted`.
    - `State`: Der Benutzerstatus, mit dem die Anforderung und Antwort korreliert werden können. Die gleichen Inhalte werden in der Antwort zurückgegeben.
    - `Uri`: Filterergebnisse von diesem URI. Standard: `all`
    - `User`: Filterergebnisse von diesem Benutzer. Standard: `all`
* `Texts`: Erforderlich. Ein Array, das den Text für die Übersetzung enthält. Alle Zeichenfolgen müssen in der gleichen Sprache sein. Die Gesamtlänge aller Texte, die übersetzt werden sollen, darf 10.000 Zeichen nicht überschreiten. Die maximale Anzahl von Arrayelementen beträgt 2.000.
* `To`: Erforderlich. Eine Zeichenfolge, die den Sprachcode der Sprache darstellt, in die der Text übersetzt werden soll.

Sie können optionale Elemente auslassen. Elemente, die direkt untergeordnete Elemente von `TranslateArrayRequest` sind, müssen in alphabetischer Reihenfolge aufgeführt sein.

Die `TranslateArray`-Methode akzeptiert `application/xml` oder `text/xml` als `Content-Type`.

**Rückgabewert:** Ein `TranslateArrayResponse`-Array. Jede `TranslateArrayResponse` verfügt über diese Elemente:

* `Error`: Gibt einen Fehler an, wenn ein Fehler auftritt. Andernfalls ist das Element auf NULL festgelegt.
* `OriginalSentenceLengths`: Ein Array mit ganzen Zahlen, das die Länge jedes Satzes im Quelltext angibt. Die Länge des Arrays gibt die Anzahl der Sätze an.
* `TranslatedText`: Der übersetzte Text.
* `TranslatedSentenceLengths`: Ein Array mit ganzen Zahlen, das die Länge jedes Satzes im übersetzten Text angibt. Die Länge des Arrays gibt die Anzahl der Sätze an.
* `State`: Der Benutzerstatus, mit dem die Anforderung und Antwort korreliert werden können. Gibt den gleichen Inhalt wie in der Anforderung zurück.

Der Antworttext hat das folgende Format:

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
Eine erfolgreiche Antwort enthält ein Array von `TranslateArrayResponse`-Arrays im oben beschriebenen Format.

Zeichenfolge

Antwortinhaltstyp: application/xml

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|Authorization|(leer)  |Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode   |`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort. Häufige Fehler sind z.B. folgende: <ul><li>Ein Arrayelement kann nicht leer sein.</li><li>Ungültige Kategorie.</li><li>Ausgangssprache ist ungültig.</li><li>Zielsprache ist ungültig.</li><li>Die Anforderung enthält zu viele Elemente.</li><li>Die Ausgangssprache wird nicht unterstützt.</li><li>Die Zielsprache wird nicht unterstützt.</li><li>Die Übersetzungsanforderung verfügt über zu viele Daten.</li><li>HTML liegt nicht im richtigen Format vor.</li><li>Zu viele Zeichenfolgen wurde in der Übersetzungsanforderung übergeben.</li></ul>|
|401    |Ungültige Anmeldeinformationen.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Ruft die Anzeigenamen für die Sprachen ab, die als Parameter `languageCodes` übergeben werden. Die Anzeigenamen werden in die übergebene `locale`-Sprache übersetzt.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

Der Anforderungstext enthält ein Zeichenfolgenarray, das die Sprachcodes nach ISO 639-1 darstellt, für die die Anzeigenamen abgerufen werden. Hier sehen Sie ein Beispiel:

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

Antwortinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `appid` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|locale|(leer) |Erforderlich. Eine Zeichenfolge, die eines der folgenden Elemente darstellt, das zum Lokalisieren der Sprachnamen verwendet wird: <ul><li>Die Kombination eines aus zwei Kleinbuchstaben bestehenden ISO 639-Kulturcodes, der einer Sprache zugeordnet ist, und einem aus zwei Großbuchstaben bestehenden ISO 3166-Subkulturcode. <li>Ein eigenständiger, aus zwei Kleinbuchstaben bestehender ISO 639-Kulturcode.|query|Zeichenfolge|
|Authorization|(leer)  |Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)  |Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-getlanguagesfortranslate"></a>GET /GetLanguagesForTranslate

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Ruft eine Liste von Sprachcodes ab, die die Sprachen darstellen, die vom Übersetzungsdienst unterstützt werden.  `Translate` und `TranslateArray` können zwischen beliebigen zwei Sprachen aus der Liste übersetzen.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Rückgabewert:** Ein Zeichenfolgenarray, das die Sprachcodes enthält, die vom Übersetzungsdienst unterstützt werden.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Ein Zeichenfolgenarray, das die Sprachcodes enthält, die vom Übersetzungsdienst unterstützt werden.

Zeichenfolge

Antwortinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `appid` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|Authorization|(leer)  |Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503|Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-getlanguagesforspeak"></a>GET /GetLanguagesForSpeak

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Ruft die verfügbaren Sprachen für die Sprachsynthese ab.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Rückgabewert:** Ein Zeichenfolgenarray, das die Sprachcodes enthält, die vom Übersetzungsdienst für die Sprachsynthese unterstützt werden.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Ein Zeichenfolgenarray, das die Sprachcodes enthält, die vom Übersetzungsdienst für die Sprachsynthese unterstützt werden.

Zeichenfolge

Antwortinhaltstyp: application/xml

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `appid` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|Authorization|(leer)|Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|
 
### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400|Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401|Ungültige Anmeldeinformationen.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-speak"></a>GET /Speak

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Gibt einen WAV- oder MP3-Stream des übergebenen Texts zurück, der in der gewünschten Sprache gesprochen wird.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Rückgabewert:** Ein WAV- oder MP3-Stream des übergebenen Texts, der in der gewünschten Sprache gesprochen wird.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)

binary

Antwortinhaltstyp: application/xml

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `appid` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|text|(leer)   |Erforderlich. Eine Zeichenfolge, die eine oder mehrere Sätze, die für den Stream gesprochen werden sollen, in der angegebenen Sprache enthält. Der Text darf maximal 2.000 Zeichen lang sein.|query|Zeichenfolge|
|language|(leer)   |Erforderlich. Eine Zeichenfolge, die den unterstützten Sprachcode der Sprache darstellt, in der der Text gesprochen werden soll. Der Code muss einer der Codes sein, die von der Methode `GetLanguagesForSpeak` zurückgegeben werden.|query|Zeichenfolge|
|format|(leer)|Optional. Eine Zeichenfolge, die die Inhaltstyp-ID angibt. Derzeit sind `audio/wav` und `audio/mp3` verfügbar. Standardwert: `audio/wav`.|query|Zeichenfolge|
|options|(leer)    |Optional. Eine Zeichenfolge, die Eigenschaften der synthetisierten Spracheingabe angibt:<ul><li>`MaxQuality` und `MinSize` geben die Qualität des Audiosignals an. `MaxQuality` bietet die höchste Qualität. `MinSize` liefert die geringste Dateigröße. Der Standardwert ist `MinSize`.</li><li>`female` und `male` geben das gewünschte Geschlecht bei der Sprachwiedergabe an. Der Standardwert lautet `female`. Verwenden Sie den senkrechten Strich (<code>\|</code>), um mehrere Optionen einzuschließen. Beispiel: `MaxQuality|Male`.</li></li></ul>  |query|Zeichenfolge|
|Authorization|(leer)|Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)  |Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-detect"></a>GET /Detect

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Identifiziert die Sprache eines Textabschnitts.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Rückgabewert:** Eine Zeichenfolge, die einen aus zwei Zeichen bestehenden Sprachcode für den Text enthält.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)

Zeichenfolge

Antwortinhaltstyp: application/xml

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)  |Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `appid` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|text|(leer)|Erforderlich. Eine Zeichenfolge, die Text enthält, dessen Sprache identifiziert werden soll. Der Text darf maximal 10.000 Zeichen lang sein.|query|  Zeichenfolge|
|Authorization|(leer)|Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key  |(leer)    |Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400|Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|


## <a name="post-detectarray"></a>POST /DetectArray

### <a name="implementation-notes"></a>Hinweise zur Implementierung

Identifiziert die Sprachen in einem Zeichenfolgearray. Erkennt eigenständig die Sprache jedes einzelnen Arrayelements und gibt für jede Arrayzeile ein Ergebnis zurück.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

Der Anforderungstext hat das folgende Format:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

Der Text darf maximal 10.000 Zeichen lang sein.

**Rückgabewert:** Ein Zeichenfolgenarray, das einen aus zwei Zeichen bestehenden Sprachcode für jede Zeile des Eingabearrays enthält.

Der Antworttext hat das folgende Format:

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
`DetectArray` war erfolgreich. Gibt ein Zeichenfolgenarray zurück, das einen aus zwei Zeichen bestehenden Sprachcode für jede Zeile des Eingabearrays enthält.

Zeichenfolge

Antwortinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `appid` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|Authorization|(leer)|Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden.  Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-addtranslation"></a>GET /AddTranslation

### <a name="implementation-notes"></a>Hinweise zur Implementierung

> [!IMPORTANT]
> **Ablaufwarnung:** Diese Methode akzeptiert nach dem 31. Januar 2018 keine Übermittlungen neuer Sätze mehr. Es wird eine Fehlermeldung angezeigt. Sehen Sie sich die Ankündigung bzgl. der Änderungen am Framework für kollaborative Übersetzungen (Collaborative Translations Framework, CTF) an.

Es wird eine Übersetzung zum Translation Memory hinzugefügt.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)

Zeichenfolge

Anforderungsinhaltstyp: application: xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp   |
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `appid` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|originalText|(leer)|Erforderlich. Eine Zeichenfolge, die den zu übersetzenden Text enthält. Die Zeichenfolge darf maximal 1.000 Zeichen lang sein.|query|Zeichenfolge|
|translatedText|(leer) |Erforderlich. Eine Zeichenfolge, die den in die Zielsprache übersetzten Text enthält. Die Zeichenfolge darf maximal 2.000 Zeichen lang sein.|query|Zeichenfolge|
|from|(leer)   |Erforderlich. Eine Zeichenfolge, die den Sprachcode der Ausgangssprache des Texts darstellt. Beispiel: „en“ für Englisch und „de“ für Deutsch.|query|Zeichenfolge|
|zu|(leer)|Erforderlich. Eine Zeichenfolge, die den Sprachcode der Sprache darstellt, in die der Text übersetzt werden soll.|query|Zeichenfolge|
|rating|(leer) |Optional. Eine ganze Zahl, die die Qualitätsbewertung für die Zeichenfolge darstellt. Der Wert liegt zwischen -10 und 10. Der Standardwert ist 1.|query|integer|
|contentType|(leer)    |Optional. Das Format des Texts, der übersetzt wird. Die unterstützten Formate lauten `text/plain` und `text/html`. HTML-Elemente müssen wohlgeformt und vollständig sein.    |query|Zeichenfolge|
|category|(leer)|Optional. Eine Zeichenfolge, die die Kategorie (Domäne) der Übersetzung enthält. Der Standardwert lautet `general`.|query|Zeichenfolge|
|user|(leer)|Erforderlich. Eine Zeichenfolge, die zur Nachverfolgung des Erstellers der Einreichung verwendet wird.|query|Zeichenfolge|
|uri|(leer)|Optional. Eine Zeichenfolge, die den Inhaltsspeicherort der Übersetzung enthält.|query|Zeichenfolge|
|Authorization|(leer)|Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden.  Autorisierungstoken: `"Bearer" + " " + "access_token"`  |Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen.|
|410|`AddTranslation` wird nicht mehr unterstützt.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="post-addtranslationarray"></a>POST /AddTranslationArray

### <a name="implementation-notes"></a>Hinweise zur Implementierung

> [!IMPORTANT]
> **Ablaufwarnung:** Diese Methode akzeptiert nach dem 31. Januar 2018 keine Übermittlungen neuer Sätze mehr. Es wird eine Fehlermeldung angezeigt. Sehen Sie sich die Ankündigung bzgl. der Änderungen am Framework für kollaborative Übersetzungen (Collaborative Translations Framework, CTF) an.

Fügt dem Translation Memory ein Array von Übersetzungen hinzu. Diese Methode ist eine Arrayversion von `AddTranslation`.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`.

Der Anforderungstext hat das folgende Format:

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

Diese Elemente befinden sich in `AddtranslationsRequest`:

* `AppId`: Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `AppId` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.
* `From`: Erforderlich. Eine Zeichenfolge, die den Sprachcode der Quellsprache enthält. Sie muss in einer der Sprachen sein, die von der `GetLanguagesForTranslate`-Methode zurückgegeben wird.
* `To`: Erforderlich. Eine Zeichenfolge, die den Sprachcode der Zielsprache enthält. Sie muss in einer der Sprachen sein, die von der `GetLanguagesForTranslate`-Methode zurückgegeben wird.
* `Translations`: Erforderlich. Ein Array von Übersetzungen das dem Translation Memory hinzugefügt wird. Jede Übersetzung muss `OriginalText`, `TranslatedText` und `Rating` enthalten. Die maximale Größe von `OriginalText` und `TranslatedText` beträgt jeweils 1.000 Zeichen. Die Gesamtlänge aller `OriginalText`- und `TranslatedText`-Elemente darf 10.000 Zeichen nicht überschreiten. Die maximale Anzahl von Arrayelementen beträgt 100.
* `Options`: Erforderlich. Eine Reihe von Optionen, einschließlich `Category`, `ContentType`, `Uri` und `User`. `User` ist erforderlich. `Category`, `ContentType` und `Uri` sind optional. Angegebene Element müssen in alphabetischer Reihenfolge aufgelistet werden.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
`AddTranslationArray`-Methode erfolgreich. 

Nach dem 31. Januar 2018 werden keine Einreichungen von Sätzen mehr akzeptiert. Dieser Dienst reagiert dann mit dem Fehlercode 410.

Zeichenfolge

Antwortinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|Authorization|(leer)|Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden.  Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen.|
|410    |`AddTranslation` wird nicht mehr unterstützt.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503|Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="get-breaksentences"></a>GET /BreakSentences

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Unterteilt einen Textabschnitt in einzelne Sätze und gibt ein Array zurück, das die Längenwerte jedes Satzes enthält.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Rückgabewert:** Ein Array mit ganzen Zahlen, das die Länge der Sätze darstellt. Die Länge des Arrays stellt die Anzahl der Sätze dar. Die Werte stellen die Länge jedes Satzes dar.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Ein Array mit ganzen Zahlen, das die Länge der Sätze darstellt. Die Länge des Arrays stellt die Anzahl der Sätze dar. Die Werte stellen die Länge jedes Satzes dar.

integer

Antwortinhaltstyp: application/xml

### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)  |Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `appid` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query| Zeichenfolge|
|text|(leer)   |Erforderlich. Eine Zeichenfolge, die den Text darstellt, der in Sätze aufgeteilt werden soll. Die maximale Größe des Texts beträgt 10.000 Zeichen.|query|Zeichenfolge|
|language   |(leer)    |Erforderlich. Eine Zeichenfolge, die den Sprachcode des Eingabetexts darstellt.|query|Zeichenfolge|
|Authorization|(leer)|Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden. Autorisierungstoken: `"Bearer" + " " + "access_token"`   |Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)|Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400|Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401|Ungültige Anmeldeinformationen.|
|500|Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503|Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Ruft ein Array von Übersetzungen für ein gegebenes Sprachpaar aus dem Speicher und der MT-Engine ab. `GetTranslations` gibt im Gegensatz zu `Translate` alle verfügbaren Übersetzungen zurück.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`.

Der Anforderungstext enthält das optional `TranslationOptions`-Objekt, das folgendes Format aufweist:

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

Das `TranslateOptions`-Objekt enthält die Werte in der folgenden Liste. Diese sind alle optional und entsprechen den Standardwerten für die häufigsten Einstellungen. Angegebene Element müssen in alphabetischer Reihenfolge aufgelistet werden.

* `Category`: Eine Zeichenfolge, die die Kategorie (Domäne) der Übersetzung enthält. Der Standardwert lautet `general`.
* `ContentType`: Es wird nur die Option `text/plain` unterstützt, und dies ist auch die Standardeinstellung.
* `IncludeMultipleMTAlternatives`: Ein boolesches Flag, das bestimmt, ob mehrere Alternativen von der MT-Engine zurückgegeben werden sollen. Die gültigen Werte lauten `true` und `false` (mit Berücksichtigung der Groß-/Kleinschreibung). Der Standardwert ist `false`. Dieser gibt nur eine Alternative zurück. Durch das Festlegen des Flags auf `true` wird die Erstellung künstlicher Alternativen ermöglicht, die vollständig in das Framework für kollaborative Übersetzungen (Collaborative Translations Framework, CTF) integriert sind. Das Feature ermöglicht, dass Alternativen für Sätze zurückgegeben werden, die keine Übersetzungen im CTF aufweisen. Dies geschieht durch Hinzufügen künstlicher Alternativen aus der *n*-besten Liste des Decoders.
    - Bewertungen. Die Bewertungen werden wie folgt angewendet: 
         - Die beste automatische Übersetzung erhält die Bewertung 5.
       - Die Alternativen des CTF spiegeln die Autorität des Reviewers wider. Die Werte liegen im Bereich von -10 bis +10.
       - Die automatisch generierten (*n*-besten) Übersetzungsalternativen haben eine Bewertung von 0 und einen Übereinstimmungsgrad von 100.
    - Anzahl von Alternativen. Die Anzahl der zurückgegebenen Alternativen kann dem in `maxTranslations` angegebenen Wert entsprechen, jedoch auch niedriger sein.
    - Sprachenpaare. Diese Funktion ist für Übersetzungen zwischen einfachem und traditionellem Chinesisch (in beide Richtungen) nicht verfügbar. Sie ist für alle anderen von Microsoft Translator unterstützten Sprachenpaare verfügbar.
* `State`: Der Benutzerstatus, mit dem die Anforderung und Antwort korreliert werden können. Die gleichen Inhalte werden in der Antwort zurückgegeben.
* `Uri`: Filterergebnisse von diesem URI. Wenn kein Wert festgelegt ist, lautet der Standardwert `all`.
* `User`: Filterergebnisse von diesem Benutzer. Wenn kein Wert festgelegt ist, lautet der Standardwert `all`.

Anforderung `Content-Type` sollte `text/xml` sein.

**Rückgabewert:** Die Antwort hat das folgende Format:

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

Diese Antwort umfasst ein `GetTranslationsResponse` -Element, das die folgenden Werte enthält:

* `Translations`: Ein Array von gefundenen Übereinstimmungen, die in `TranslationMatch`-Objekten (im folgenden Abschnitt beschrieben) gespeichert sind. Die Übersetzungen können leichte Abweichungen vom Originaltext enthalten (Fuzzyübereinstimmung). Die Übersetzungen werden folgendermaßen sortiert: 100 %-Übereinstimmungen zuerst, darunter Fuzzyübereinstimmungen.
* `From`: Wenn die Methode keine `From`-Sprache angibt, stammt dieser Wert aus der automatischen Spracherkennung. Andernfalls wird die angegebene `From`-Sprache verwendet.
* `State`: Der Benutzerstatus, mit dem die Anforderung und Antwort korreliert werden können. Enthält den im `TranslateOptions`-Parameter angegebenen Wert.

`TranslationMatch`-Objekte bestehen aus Folgendem:

* `Error`: Der Fehlercode, wenn für eine bestimmte Eingabezeichenfolge ein Fehler auftritt. Andernfalls ist das Feld leer.
* `MatchDegree`: Gibt an, wie stark der eingegebene Text mit dem Originaltext übereinstimmt, der im Speicher gefunden wurde. Das System gleicht eingegebene Sätze (einschließlich ungenauer Übereinstimmungen) mit dem Speicher ab. Der zurückgegebene Wert liegt im Bereich von 0 bis 100, wobei 0 keine Ähnlichkeit und 100 eine genaue Übereinstimmung (mit gleicher Groß-/Kleinschreibung) angibt.
* `MatchedOriginalText`: Originaltext, der für dieses Ergebnis übereinstimmte. Dieser Wert wird nur zurückgegeben, wenn der verglichene Originaltext sich vom Eingabetext unterschieden hat. Er wird verwendet, um den Quelltext einer Fuzzyübereinstimmung zurückzugeben. Dieser Wert wird nicht für Microsoft Translator-Ergebnisse zurückgegeben.
* `Rating`: Gibt die Autorität der Person an, die die Entscheidung über die Qualität trifft. Ergebnisse der maschinellen Übersetzung haben die Bewertung 5. Anonym bereitgestellte Übersetzungen haben in der Regel eine Bewertung von 1 bis 4. Autoritativ bereitgestellte Übersetzungen haben in der Regel eine Bewertung von 6 bis 10.
* `Count`: Die Häufigkeit, mit der diese Übersetzung mit dieser Bewertung ausgewählt wurde. Für die automatisch übersetzte Antwort ist der Wert 0.
* `TranslatedText`: Der übersetzte Text.

### <a name="response-class-status-200"></a>Antwortklasse (Status 200)
Ein `GetTranslationsResponse`-Objekt im oben beschriebenen Format.

Zeichenfolge

Antwortinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|appid|(leer)|Erforderlich. Wenn der `Authorization`- oder `Ocp-Apim-Subscription-Key`-Header verwendet wird, lassen Sie das Feld `appid` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.|query|Zeichenfolge|
|text|(leer)|Erforderlich. Eine Zeichenfolge, die den zu übersetzenden Text darstellt. Die maximale Größe des Texts beträgt 10.000 Zeichen.|query|Zeichenfolge|
|from|(leer)|Erforderlich. Eine Zeichenfolge, die den Sprachcode der Sprache darstellt, aus der der Text übersetzt werden soll.|query|Zeichenfolge|
|zu |(leer)    |Erforderlich. Eine Zeichenfolge, die den Sprachcode der Sprache darstellt, in die der Text übersetzt werden soll.|query|Zeichenfolge|
|maxTranslations|(leer)|Erforderlich. Eine ganze Zahl, die die maximale Anzahl der Übersetzungen darstellt, die zurückgegeben werden sollen.|query|integer|
|Authorization| (leer)|Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden. Autorisierungstoken: `"Bearer" + " " + "access_token"`|Zeichenfolge|  Header|
|Ocp-Apim-Subscription-Key|(leer)  |Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im `X-MS-Trans-Info`-Antwortheader enthalten ist.|
|503|Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="post-gettranslationsarray"></a>POST /GetTranslationsArray

### <a name="implementation-notes"></a>Hinweise zur Implementierung
Ruft mehrere Übersetzungskandidaten für mehrere Quelltexte ab.

Der Anforderungs-URI ist `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`.

Der Anforderungstext hat das folgende Format:

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

`GetTranslationsArrayRequest` enthält die folgenden Elemente:

* `AppId`: Erforderlich. Wenn der `Authorization`-Header verwendet wird, lassen Sie das Feld `AppId` leer. Schließen Sie andernfalls eine Zeichenfolge ein, die `"Bearer" + " " + "access_token"` enthält.
* `From`: Erforderlich. Eine Zeichenfolge, die den Sprachcode der Sprache darstellt, aus der der Text übersetzt werden soll.
* `MaxTranslations`: Erforderlich. Eine ganze Zahl, die die maximale Anzahl der Übersetzungen darstellt, die zurückgegeben werden sollen.
* `Options`: Optional. Ein `Options`-Objekt, das die folgenden Werte enthält. Diese sind alle optional und entsprechen den Standardwerten für die häufigsten Einstellungen. Angegebene Element müssen in alphabetischer Reihenfolge aufgelistet werden.
    - `Category`: Eine Zeichenfolge, die die Kategorie (Domäne) der Übersetzung enthält. Der Standardwert lautet `general`.
    - `ContentType`: Es wird nur die Option `text/plain` unterstützt, und dies ist auch die Standardeinstellung.
    - `IncludeMultipleMTAlternatives`: Ein boolesches Flag, das bestimmt, ob mehrere Alternativen von der MT-Engine zurückgegeben werden sollen. Die gültigen Werte lauten `true` und `false` (mit Berücksichtigung der Groß-/Kleinschreibung). Der Standardwert ist `false`. Dieser gibt nur eine Alternative zurück. Durch das Festlegen des Flags auf `true` wird die Generierung künstlicher Alternativen in der Übersetzung ermöglicht, die vollständig in das Framework für kollaborative Übersetzungen (Collaborative Translations Framework, CTF) integriert sind. Das Feature ermöglicht, dass Alternativen für Sätze zurückgegeben werden, die keine Alternativen im CTF besitzen. Dies geschieht durch Hinzufügen künstlicher Alternativen aus der *n*-besten Liste des Decoders.
        - Bewertungen: Die Bewertungen werden wie folgt angewendet:
          - Die beste automatische Übersetzung erhält die Bewertung 5.
          - Die Alternativen des CTF spiegeln die Autorität des Reviewers wider. Die Werte liegen im Bereich von -10 bis +10.
          - Die automatisch generierten (*n*-besten) Übersetzungsalternativen haben eine Bewertung von 0 und einen Übereinstimmungsgrad von 100.
        - Anzahl von Alternativen. Die Anzahl der zurückgegebenen Alternativen kann dem in `maxTranslations` angegebenen Wert entsprechen, jedoch auch niedriger sein.
        - Sprachenpaare. Diese Funktion ist für Übersetzungen zwischen einfachem und traditionellem Chinesisch (in beide Richtungen) nicht verfügbar. Sie ist für alle anderen von Microsoft Translator unterstützten Sprachenpaare verfügbar.
* `State`: Der Benutzerstatus, mit dem die Anforderung und Antwort korreliert werden können. Die gleichen Inhalte werden in der Antwort zurückgegeben.
* `Uri`: Filterergebnisse von diesem URI. Wenn kein Wert festgelegt ist, lautet der Standardwert `all`.
* `User`: Filterergebnisse von diesem Benutzer. Wenn kein Wert festgelegt ist, lautet der Standardwert `all`.
* `Texts`: Erforderlich. Ein Array, das den Text für die Übersetzung enthält. Alle Zeichenfolgen müssen in der gleichen Sprache sein. Die Gesamtlänge aller Texte, die übersetzt werden sollen, darf 10.000 Zeichen nicht überschreiten. Die maximale Anzahl von Arrayelementen beträgt 10.
* `To`: Erforderlich. Eine Zeichenfolge, die den Sprachcode der Sprache darstellt, in die der Text übersetzt werden soll.

Sie können optionale Elemente auslassen. Elemente, die direkt untergeordnete Elemente von `GetTranslationsArrayRequest` sind, müssen in alphabetischer Reihenfolge aufgeführt sein.

Anforderung `Content-Type` sollte `text/xml` sein.

**Rückgabewert:** Die Antwort hat das folgende Format:

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

* `Translations`: Ein Array von gefundenen Übereinstimmungen, die in `TranslationMatch`-Objekten (im folgenden Abschnitt beschrieben) gespeichert sind. Die Übersetzungen können leichte Abweichungen vom Originaltext enthalten (Fuzzyübereinstimmung). Die Übersetzungen werden folgendermaßen sortiert: 100 %-Übereinstimmungen zuerst, darunter Fuzzyübereinstimmungen.
* `From`: Wenn die Methode keine `From`-Sprache angibt, stammt dieser Wert aus der automatischen Spracherkennung. Andernfalls wird die angegebene `From`-Sprache verwendet.
* `State`: Der Benutzerstatus, mit dem die Anforderung und Antwort korreliert werden können. Enthält den im `TranslateOptions`-Parameter angegebenen Wert.

Das `TranslationMatch`-Objekt enthält die folgenden Werte:
* `Error`: Der Fehlercode, wenn für eine bestimmte Eingabezeichenfolge ein Fehler auftritt. Andernfalls ist das Feld leer.
* `MatchDegree`: Gibt an, wie stark der eingegebene Text mit dem Originaltext übereinstimmt, der im Speicher gefunden wurde. Das System gleicht eingegebene Sätze (einschließlich ungenauer Übereinstimmungen) mit dem Speicher ab. Der zurückgegebene Wert liegt im Bereich von 0 bis 100, wobei 0 keine Ähnlichkeit und 100 eine genaue Übereinstimmung (mit gleicher Groß-/Kleinschreibung) angibt.
* `MatchedOriginalText`: Originaltext, der für dieses Ergebnis übereinstimmte. Dieser Wert wird nur zurückgegeben, wenn der verglichene Originaltext sich vom Eingabetext unterschieden hat. Er wird verwendet, um den Quelltext einer Fuzzyübereinstimmung zurückzugeben. Dieser Wert wird nicht für Microsoft Translator-Ergebnisse zurückgegeben.
* `Rating`: Gibt die Autorität der Person an, die die Entscheidung über die Qualität trifft. Ergebnisse der maschinellen Übersetzung haben die Bewertung 5. Anonym bereitgestellte Übersetzungen haben in der Regel eine Bewertung von 1 bis 4. Autoritativ bereitgestellte Übersetzungen haben in der Regel eine Bewertung von 6 bis 10.
* `Count`: Die Häufigkeit, mit der diese Übersetzung mit dieser Bewertung ausgewählt wurde. Für die automatisch übersetzte Antwort ist der Wert 0.
* `TranslatedText`: Der übersetzte Text.


### <a name="response-class-status-200"></a>Antwortklasse (Status 200)

Zeichenfolge

Antwortinhaltstyp: application/xml
 
### <a name="parameters"></a>Parameter

|Parameter|Wert|BESCHREIBUNG|Parametertyp|Datentyp|
|:--|:--|:--|:--|:--|
|Authorization  |(leer)    |Erforderlich, wenn das Feld `appid` und der `Ocp-Apim-Subscription-Key`-Header leer gelassen werden.  Autorisierungstoken: `"Bearer" + " " + "access_token"`|Header|Zeichenfolge|
|Ocp-Apim-Subscription-Key|(leer)  |Erforderlich, wenn das Feld `appid` und der `Authorization`-Header leer gelassen werden.|Header|Zeichenfolge|

### <a name="response-messages"></a>Antwortnachrichten

|HTTP-Statuscode|`Reason`|
|:--|:--|
|400    |Ungültige Anforderung. Überprüfen Sie die Eingabeparameter und die detaillierte Fehlerantwort.|
|401    |Ungültige Anmeldeinformationen.|
|500    |Serverfehler. Lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt. Teilen Sie uns das ungefähre Datum sowie die Uhrzeit der Anforderung zusammen mit der Anforderungs-ID mit, die im Antwortheader `X-MS-Trans-Info` enthalten ist.|
|503    |Service is temporarily unavailable. (Der Dienst ist vorübergehend nicht erreichbar.) Versuchen Sie es erneut, und lassen Sie es uns wissen, wenn der Fehler weiterhin auftritt.|

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Migrieren zu Version 3 der Textübersetzungs-API](../migrate-to-v3.md)


