---
title: 'Behandeln von Inhaltstypen: Azure Logic Apps'
description: Erfahren Sie, wie Logic Apps Inhaltstypen zur Entwurfszeit und Laufzeit behandelt.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 07/20/2018
ms.openlocfilehash: 97897da13c70c29834b1fc276829b316416efd8d
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67868914"
---
# <a name="handle-content-types-in-azure-logic-apps"></a>Behandeln von Inhaltstypen in Azure Logic Apps

Verschiedene Inhaltstypen können durch eine Logik-App fließen, z.B. JSON, XML, Flatfiles und binäre Daten. Logic Apps unterstützt zwar alle Inhaltstypen, doch einige verfügen über native Unterstützung und benötigen keine Umformung oder Umwandlung in Ihren Logik-Apps. Bei anderen Typen ist möglicherweise eine Umformung oder Umwandlung notwendig. In diesem Artikel wird beschrieben, wie Logic Apps Inhaltstypen behandelt, und wie Sie diese Typen bei Bedarf ordnungsgemäß umformen oder umwandeln können.

Um die geeignete Methode zur Behandlung von Inhaltstypen zu bestimmen, bezieht sich Logic Apps auf den `Content-Type`-Headerwert in HTTP-Aufrufen, z.B.:

* [application/json](#application-json) (nativer Typ)
* [text/plain](#text-plain) (nativer Typ)
* [application/xml und application/octet-stream](#application-xml-octet-stream)
* [Andere Inhaltstypen](#other-content-types)

<a name="application-json"></a>

## <a name="applicationjson"></a>Anwendung/json

Logic Apps speichert und behandelt jede Anforderung mit dem Inhaltstyp *application/json* als JavaScript Notation-Objekt (JSON). Standardmäßig können Sie JSON-Inhalt ohne Umwandlung analysieren. Um eine Anforderung mit einem Header des Inhaltstyps „application/json“ zu analysieren, können Sie einen Ausdruck verwenden. Dieses Beispiel gibt den Wert `dog` aus dem `animal-type`-Array ohne Umwandlung zurück: 
 
`@body('myAction')['animal-type'][0]` 
  
  ```json
  {
    "client": {
       "name": "Fido",
       "animal-type": [ "dog", "cat", "rabbit", "snake" ]
    }
  }
  ```

Wenn Sie mit JSON-Daten arbeiten, die keinen Header angeben, können Sie diese Daten manuell mithilfe der [json()-Funktion](../logic-apps/workflow-definition-language-functions-reference.md#json) in JSON umwandeln, z.B.: 
  
`@json(triggerBody())['animal-type']`

### <a name="create-tokens-for-json-properties"></a>Erstellen von Token für JSON-Eigenschaften

Logic Apps ermöglicht Ihnen, benutzerfreundliche Token zu generieren, die die Eigenschaften im JSON-Inhalt darstellen, sodass Sie auf diese Eigenschaften verweisen und sie leichter in Ihrem Logik-App-Workflow verwenden können.

* **Anforderungstrigger**

  Wenn Sie diesen Trigger im Logik-App-Designer verwenden, können Sie ein JSON-Schema angeben, das die Nutzlast beschreibt, die Sie zu empfangen erwarten. 
  Der Designer analysiert JSON-Inhalte mithilfe dieses Schemas und generiert benutzerfreundliche Token, die die Eigenschaften in Ihrem JSON-Inhalt darstellen. 
  Sie können dann in Ihrem Logik-App-Workflow ganz einfach auf diese Eigenschaften verweisen und sie verwenden. 
  
  Wenn Sie nicht über ein Schema verfügen, können Sie das Schema generieren. 
  
  1. Wählen Sie im Anforderungstrigger **Beispielnutzlast zum Generieren eines Schemas verwenden** aus.  
  
  2. Geben Sie unter **Geben oder fügen Sie eine JSON-Beispielnutzlast ein** eine Beispielnutzlast ein, und wählen Sie dann **Fertig**. Beispiel: 

     ![Angeben einer JSON-Beispielnutzlast](./media/logic-apps-content-type/request-trigger.png)

     Das generierte Schema wird jetzt in Ihrem Trigger angezeigt.

     ![Angeben einer JSON-Beispielnutzlast](./media/logic-apps-content-type/generated-schema.png)

     Hier ist die zugrunde liegende Definition für Ihren Anforderungstrigger im Codeansicht-Editor:

     ```json
     "triggers": { 
        "manual": {
           "type": "Request",
           "kind": "Http",
           "inputs": { 
              "schema": {
                 "type": "object",
                 "properties": {
                    "client": {
                       "type": "object",
                       "properties": {
                          "animal-type": {
                             "type": "array",
                             "items": {
                                "type": "string"
                             },
                          },
                          "name": {
                             "type": "string"
                          }
                       }
                    }
                 }
              }
           }
        }
     }
     ```

  3. Stellen Sie in der Anforderung sicher, dass Sie einen `Content-Type`-Header einbeziehen, und legen Sie als Headerwert `application/json` fest.

* **JSON-Analyseaktion**

  Wenn Sie diese Aktion im Logik-App-Designer verwenden, können Sie die JSON-Ausgabe analysieren und benutzerfreundliche Token generieren, die die Eigenschaften in Ihrem JSON-Inhalt darstellen. 
  Sie können dann in Ihrem Logik-App-Workflow ganz einfach auf diese Eigenschaften verweisen und sie verwenden. Ähnlich wie beim Anforderungstrigger können Sie ein JSON-Schema eingeben oder generieren, das den JSON-Inhalt beschreibt, den Sie analysieren möchten. 
  Auf diese Weise können Sie leichter Daten von Azure Service Bus, Azure Cosmos DB usw. nutzen.

  ![JSON-Analyse](./media/logic-apps-content-type/parse-json.png)

<a name="text-plain"></a>

## <a name="textplain"></a>text/plain

Wenn Ihre Logik-App HTTP-Nachrichten empfängt, für deren `Content-Type`-Header `text/plain` festgelegt ist, speichert Ihre Logik-App diese Nachrichten unformatiert. Wenn Sie diese Nachrichten ohne Umwandlung in nachfolgende Aktionen einbeziehen, werden Anforderungen ausgegeben, in denen der `Content-Type`-Header auf `text/plain` festgelegt ist. 

Wenn Sie z.B. mit einer Flatfile-Datei arbeiten, erhalten Sie möglicherweise eine HTTP-Anforderung, in der für den `Content-Type`-Header der `text/plain`-Inhaltstyp festgelegt ist:

`Date,Name,Address`</br>
`Oct-1,Frank,123 Ave`

Wenn Sie diese Anforderung anschließend in einer späteren Aktion als Text für eine andere Anforderung senden, z.B. `@body('flatfile')`, dann verfügt diese zweite Anforderung auch über einen `Content-Type`-Header, für den `text/plain` festgelegt ist. Wenn Sie mit Nur-Text-Daten arbeiten, jedoch keinen Header angegeben haben, können Sie diese Daten mithilfe der [string()-Funktion](../logic-apps/workflow-definition-language-functions-reference.md#string) wie in diesem Ausdruck manuell in Text umwandeln: 

`@string(triggerBody())`

<a name="application-xml-octet-stream"></a>

## <a name="applicationxml-and-applicationoctet-stream"></a>application/xml und application/octet-stream

Logic Apps behält den `Content-Type` immer in einer empfangenen HTTP-Anforderung oder Antwort bei. Wenn also Ihre Logik-App Inhalt empfängt, bei dem `Content-Type` auf `application/octet-stream` festgelegt ist, und Sie diesen Inhalt ohne Umwandlung in eine nachfolgende Aktion einschließen, ist in der ausgehenden Anforderung auch für `Content-Type` der Wert `application/octet-stream` festgelegt. So kann Logic Apps garantieren, dass keine Daten auf dem Weg durch den Workflow verlorengehen. Der Aktionszustand, d.h. Eingaben und Ausgaben, wird jedoch in einem JSON-Objekt gespeichert, während er den Workflow durchläuft. 

## <a name="converter-functions"></a>Konvertierungsfunktionen

Zum Erhalten einiger Datentypen konvertiert Logic Apps den Inhalt in eine Base64-codierte Binärzeichenfolge mit entsprechenden Metadaten, in der `$content`-Nutzlast und `$content-type` beibehalten werden, wobei eine automatische Konvertierung erfolgt. 

In dieser Liste wird beschrieben, wie Logic Apps Inhalt konvertiert, wenn Sie diese [Funktionen](../logic-apps/workflow-definition-language-functions-reference.md) verwenden:

* `json()`: Konvertiert Daten in `application/json`.
* `xml()`: Konvertiert Daten in `application/xml`.
* `binary()`: Konvertiert Daten in `application/octet-stream`.
* `string()`: Konvertiert Daten in `text/plain`.
* `base64()`: Konvertiert Inhalt in eine Base64-codierte Zeichenfolge
* `base64toString()`: Konvertiert eine Base64-codierte Zeichenfolge in `text/plain`
* `base64toBinary()`: Konvertiert eine Base64-codierte Zeichenfolge in `application/octet-stream`
* `dataUri()`: Konvertiert eine Zeichenfolge in einen Daten-URI
* `dataUriToBinary()`: Konvertiert einen Daten-URI in eine binäre Zeichenfolge
* `dataUriToString()`: Konvertiert einen Daten-URI in eine Zeichenfolge

Wenn Sie z.B. eine HTTP-Anforderung erhalten, in der für `Content-Type` der Wert `application/xml` festgelegt ist, z.B. diesen Inhalt:

```html
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Sie können diesen Inhalt mithilfe des `@xml(triggerBody())`-Ausdrucks mit der `xml()`- und `triggerBody()`-Funktion umwandeln und diesen Inhalt später verwenden. Alternativ können Sie den `@xpath(xml(triggerBody()), '/CustomerName')`-Ausdruck mit der `xpath()`- und `xml()`-Funktion verwenden. 

## <a name="other-content-types"></a>Andere Inhaltstypen

Logic Apps arbeitet mit anderen Inhaltstypen und unterstützt sie, aber es kann erforderlich sein, dass Sie den Nachrichtentext manuell durch Decodieren der `$content`-Variablen erhalten.

Nehmen Sie beispielsweise an, dass Ihre Logik-App durch eine Anforderung mit dem `application/x-www-url-formencoded`-Inhaltstyp ausgelöst wird. Um alle Daten beizubehalten, hat die `$content`-Variable im Anforderungstext eine Nutzlast, die als Base64-Zeichenfolge codiert ist:

`CustomerName=Frank&Address=123+Avenue`

Da die Eingabe der Anforderung nicht im Nur-Text- oder JSON-Format erfolgt ist, wird die Anforderung wie folgt in der Aktion gespeichert:

```json
"body": {
   "$content-type": "application/x-www-url-formencoded",
   "$content": "AAB1241BACDFA=="
}
```

Logic Apps bietet native Funktionen für die Behandlung von Formulardaten, z.B.: 

* [triggerFormDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue)
* [triggerFormDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues)
* [formDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) 
* [formDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues)

Alternativ können Sie mithilfe eines Ausdrucks wie in diesem Beispiel manuell auf die Daten zugreifen:

`@string(body('formdataAction'))` 

Wenn die ausgehende Anforderung denselben Inhaltstypheader `application/x-www-url-formencoded` haben soll, können Sie die Anforderung mithilfe eines Ausdrucks wie `@body('formdataAction')` ohne Umwandlung dem Aktionstext hinzufügen. Diese Methode funktioniert jedoch nur, wenn der Text der einzige Parameter der `body`-Eingabe ist. Wenn Sie versuchen, den `@body('formdataAction')`-Ausdruck in einer `application/json`-Anforderung zu verwenden, erhalten Sie eine Laufzeitfehlermeldung, da der Text codiert gesendet wird.
