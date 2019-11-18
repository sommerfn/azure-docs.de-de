---
title: Aufrufen, Auslösen oder Schachteln von Logik-Apps – Azure Logic Apps
description: Einrichten von HTTP-Endpunkten zum Aufrufen, Auslösen oder Schachteln von Logik-App-Workflows in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.workload: integration
author: ecfan
ms.author: klam
ms.reviewer: klam, jehollan, LADocs
ms.topic: article
ms.date: 11/04/2019
ms.openlocfilehash: 41173e088b000530030b24400640f8003f330db6
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73581049"
---
# <a name="call-trigger-or-nest-logic-apps-by-using-http-endpoints-in-azure-logic-apps"></a>Aufrufen, Auslösen oder Schachteln von Logik-Apps mithilfe von HTTP-Endpunkten in Azure Logic Apps

Damit Ihre Logik-App über eine URL aufgerufen werden und Ihre Logik-App eingehende Anforderungen aus anderen Diensten empfangen kann, können Sie einen synchronen HTTP-Endpunkt nativ als Trigger in dieser Logik-App verfügbar machen. Wenn Sie diese Funktion einrichten, können Sie Ihre Logik-App auch in anderen Logik-Apps schachteln, wodurch Sie ein Muster aus aufrufbaren Endpunkten erstellen können.

Zum Einrichten eines HTTP-Endpunkts können Sie jeden der folgenden Triggertypen verwenden, die es Logik-Apps ermöglichen, eingehende Anforderungen zu empfangen:

* [Anforderung](../connectors/connectors-native-reqres.md)
* [HTTP Webhook](../connectors/connectors-native-webhook.md)
* Verwaltete Connectortrigger vom Typ [ApiConnectionWebhook](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnectionwebhook-trigger), die eingehende HTTP-Anforderungen empfangen können

> [!NOTE]
> Die vorliegenden Beispiele verwenden einen Anforderungstrigger, Sie können aber auch jeden anderen auf HTTP-Anforderungen basierenden Trigger aus der Liste oben verwenden. Alle Prinzipien gelten gleichermaßen für diese anderen Triggertypen.

Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Die Logik-App, in der Sie den HTTP-Endpunkt als Trigger einrichten möchten. Sie können entweder mit einer leeren Logik-App beginnen oder eine vorhandene Logik-App verwenden, in der Sie den aktuellen Trigger ersetzen möchten. Dieses Beispiel beginnt mit einer leeren Logik-App.

## <a name="create-a-callable-endpoint"></a>Erstellen eines aufrufbaren Endpunkts

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Erstellen Sie im Logik-App-Designer eine leere Logik-App, und öffnen Sie sie.

   Dieses Beispiel verwendet einen Anforderungstrigger, Sie können aber auch jeden anderen Trigger verwenden, der eingehende HTTP-Anforderungen empfangen kann. Alle Prinzipien gelten gleichermaßen für diese Trigger. Weitere Informationen zum Anforderungstrigger finden Sie unter [Empfangen von und Antworten auf HTTPS-Aufrufe mittels Azure Logic Apps](../connectors/connectors-native-reqres.md).

1. Wählen Sie unter dem Suchfeld die Option **Integriert** aus. Geben Sie im Suchfeld den Begriff `request` als Filter ein. Wählen Sie in der Liste der Trigger die Option **Beim Empfang einer HTTP-Anforderung** aus.

   ![Suchen und Auswählen des Anforderungstriggers](./media/logic-apps-http-endpoint/find-and-select-request-trigger.png)

1. Optional können Sie im Feld **JSON-Schema für Anforderungstext** ein JSON-Schema eingeben, das die Nutzlast bzw. Daten beschreibt, die der Trigger empfangen soll.

   Der Designer verwendet dieses Schema, um Token zu generieren, die Triggerausgaben darstellen. Dann können Sie im gesamten Workflow Ihrer Logik-App ganz einfach auf diese Ausgaben verweisen. Informieren Sie sich über [Token, die aus JSON-Schemas generiert werden](#generated-tokens).

   Geben Sie für das vorliegende Beispiel das folgende Schema ein:

   ```json
      {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "streetNumber": {
                  "type": "string"
               },
               "streetName": {
                  "type": "string"
               },
               "town": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "string"
               }
            }
         }
      }
   }
    ```

   ![Bereitstellen eines JSON-Schemas für die Anforderungsaktion](./media/logic-apps-http-endpoint/manual-request-trigger-schema.png)

   Alternativ dazu können Sie ein JSON-Schema auch durch Bereitstellen einer Beispielnutzlast generieren:

   1. Wählen Sie im **Anforderungstrigger** die Option **Beispielnutzlast zum Generieren eines Schemas verwenden** aus.

   1. Geben Sie im Feld **Geben oder fügen Sie eine JSON-Beispielnutzlast ein** Ihre Beispielnutzlast ein, beispielsweise:

      ```json
      {
         "address": {
            "streetNumber": "00000",
            "streetName": "AnyStreet",
            "town": "AnyTown",
            "postalCode": "11111-1111"
        }
      }
      ```

   1. Wählen Sie abschließend **Fertig** aus.

      Im Feld **JSON-Schema für Anforderungstext** wird jetzt das generierte Schema angezeigt.

1. Speichern Sie Ihre Logik-App.

   Im Feld **HTTP POST für diese URL** wird die generierte Rückruf-URL angezeigt, mit der andere Dienste Ihre Logik-App aufrufen und auslösen können. Diese URL enthält einen Shared Access Signature-Schlüssel (SAS), der in den Abfrageparametern für die Authentifizierung verwendet wird. Beispiel:

   ![Generierte Rückruf-URL für den Endpunkt](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

   Sie können die HTTP-Endpunkt-URL auch aus dem Bereich **Übersicht** Ihrer Logik-App abrufen.

   1. Wählen Sie im Menü Ihrer Logik-App die Option **Übersicht** aus.

   1. Wählen Sie im Abschnitt **Zusammenfassung** die Option **Triggerverlauf anzeigen** aus.

      ![Abrufen der HTTP-Endpunkt-URL im Azure-Portal](./media/logic-apps-http-endpoint/find-manual-trigger-url.png)

   1. Kopieren Sie die URL unter **Rückruf-URL [POST]** :

      ![Kopieren der HTTP-Endpunkt-URL im Azure-Portal](./media/logic-apps-http-endpoint/copy-manual-trigger-callback-url.png)

      Außerdem können Sie die URL mit diesem Aufruf abrufen:

      ```http
      POST https://management.azure.com/{logic-app-resource-ID}/triggers/{endpoint-trigger-name}/listCallbackURL?api-version=2016-06-01
      ```

<a name="set-method"></a>

## <a name="set-expected-http-method"></a>Festlegen der erwarteten HTTP-Methode

Standardmäßig erwartet der Trigger eine HTTP POST-Anforderung. Sie können jedoch eine andere erwartete Methode angeben (aber nur eine).

1. Öffnen Sie im Anforderungstrigger die Liste **Neuen Parameter hinzufügen**, und wählen Sie **Methode** aus. Dadurch wird diese Eigenschaft zum Trigger hinzugefügt.

   ![Hinzufügen der Eigenschaft „Methode“ zum Trigger](./media/logic-apps-http-endpoint/select-add-new-parameter-for-method.png)

1. Wählen Sie aus der Liste **Methode** eine andere Methode aus, die der Trigger stattdessen erwarten soll. Alternativ dazu können Sie auch eine benutzerdefinierte Methode angeben.

   Wählen Sie beispielsweise die Methode **GET** aus, damit Sie die HTTP-Endpunkt-URL später testen können.

   ![Auswählen der zu verwendenden HTTP-Methode für Ihren Trigger](./media/logic-apps-http-endpoint/select-method-request-trigger.png)

## <a name="accept-parameters-in-endpoint-url"></a>Akzeptieren von Parametern in der Endpunkt-URL

Wenn Sie möchten, dass Ihre Endpunkt-URL Parameter akzeptiert, geben Sie den relativen Pfad in Ihrem Trigger an. Sie müssen zudem die [Methode explizit festlegen](#set-method), die von Ihrer HTTP-Anforderung erwartet wird.

1. Öffnen Sie im Anforderungstrigger die Liste **Neuen Parameter hinzufügen**, und wählen Sie **Relativer Pfad** aus. Dadurch wird diese Eigenschaft zum Trigger hinzugefügt.

   ![Hinzufügen der Eigenschaft „Relativer Pfad“ zum Trigger](./media/logic-apps-http-endpoint/select-add-new-parameter-for-relative-path.png)

1. Geben Sie in der Eigenschaft **Relativer Pfad** den relativen Pfad für den Parameter in Ihrem JSON-Schema an, der von Ihrer URL akzeptiert werden soll. Beispiel: `address/{postalCode}`.

   ![Angeben des relativen Pfads für den Parameter](./media/logic-apps-http-endpoint/relative-path-url-value.png)

1. Um den Parameter zu verwenden, suchen Sie eine **Antwort**-Aktion, und fügen Sie diese Ihrer Logik-App hinzu.

   1. Wählen Sie unter dem Anforderungstrigger **Neuer Schritt** > **Aktion hinzufügen** aus.

   1. Geben Sie unter **Aktion auswählen** im Suchfeld `response` als Filter ein.

   1. Wählen Sie in der Liste der Aktionen die Aktion **Antwort** aus.

1. Schließen Sie in die Eigenschaft **Text** der Antwortaktion das Token für den Parameter ein, den Sie im relativen Pfad des Triggers angegeben haben.

   Nehmen Sie beispielsweise an, dass die Antwortaktion `Postal Code: {postalCode}` zurückgeben soll.

   Geben Sie in der Eigenschaft **Text** die Zeichenfolge `Postal Code: ` mit einem nachfolgenden Leerzeichen ein. Wählen Sie aus der angezeigten Liste mit dynamischen Inhalten die Eigenschaft **postalCode** aus.

   ![Hinzufügen des angegebenen Parameters zum Antworttext](./media/logic-apps-http-endpoint/relative-url-with-parameter-token.png)

   Die Eigenschaft **Text** enthält jetzt den ausgewählten Parameter.

   ![Beispiel für Antworttext mit Parameter](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

1. Speichern Sie Ihre Logik-App.

    Ihre HTTP-Endpunkt-URL enthält nun den relativen Pfad, z.B.:

    ```http
    https://prod-25.westus.logic.azure.com/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke/address/postalCode?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}
    ```

1. Um den HTTP-Endpunkt zu testen, kopieren Sie die aktualisierte URL, und fügen Sie sie in einem anderen Browserfenster ein, aber ersetzen Sie `{postalCode}` mit `123456`, und drücken Sie die EINGABETASTE.

   Ihr Browser zeigt den Text `Postal Code: 123456` an.

## <a name="call-logic-app-through-http-endpoint"></a>Aufrufen einer Logik-App über einen HTTP-Endpunkt

Nach dem Erstellen des HTTP-Endpunkts können Sie die Logik-App auslösen, indem Sie eine HTTP-`POST`-Anforderung an die vollständige URL des Endpunkts senden. Logik-Apps bieten integrierte Unterstützung für Direktzugriffs-Endpunkte.

<a name="generated-tokens"></a>

## <a name="tokens-generated-from-schema"></a>Aus dem Schema generierte Token

Wenn Sie in Ihrem Anforderungstrigger ein JSON-Schema angeben, generiert der Logik-App-Designer Token für die Eigenschaften in diesem Schema. Diese Token können Sie dann zur Weitergabe von Daten über Ihren Logik-App-Workflow verwenden.

Wenn Sie Ihrem JSON-Schema beispielsweise weitere Eigenschaften wie `"suite"` hinzufügen, stehen Ihnen in den späteren Schritten für Ihre Logik-App Token für diese Eigenschaften zur Verfügung. So sieht das vollständige JSON-Schema aus:

```json
   {
   "type": "object",
   "properties": {
      "address": {
         "type": "object",
         "properties": {
            "streetNumber": {
               "type": "string"
            },
            "streetName": {
               "type": "string"
            },
            "suite": {
               "type": "string"
            },
            "town": {
               "type": "string"
            },
            "postalCode": {
               "type": "string"
            }
         }
      }
   }
}
```

## <a name="create-nested-logic-apps"></a>Erstellen von geschachtelten Logik-Apps

Sie können Workflows in Ihrer Logik-App schachteln, indem Sie andere Logik-Apps hinzufügen, die Anforderungen empfangen können. Führen Sie zum Einschließen dieser Logik-Apps die folgenden Schritte aus:

1. Wählen Sie in dem Schritt, in dem eine weitere Logik-App aufgerufen werden soll, die Option **Neuer Schritt** > **Aktion hinzufügen** aus.

1. Wählen Sie unter **Aktion auswählen** die Option **Integriert** aus. Geben Sie im Suchfeld den Begriff `logic apps` als Filter ein. Wählen Sie aus der Liste der Aktionen die Option **Logic Apps-Workflow auswählen** aus.

   ![Schachteln einer Logik-App in der aktuellen Logik-App](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

   Der Designer zeigt die geeigneten Logik-Apps an, die Sie auswählen können.

1. Wählen Sie die Logik-App aus, die aus Ihrer aktuellen Logik-App aufgerufen werden soll.

   ![Auswählen der Logik-App, die aus der aktuellen Logik-App aufgerufen werden soll](./media/logic-apps-http-endpoint/select-logic-app-to-nest.png)

## <a name="reference-content-from-an-incoming-request"></a>Verweisen auf Inhalt von einer eingehenden Anforderung aus

Wenn der Inhaltstyp der eingehenden Anforderung `application/json` lautet, können Sie auf die Eigenschaften in der eingehenden Anforderung verweisen. Andernfalls wird dieser Inhalt als einzelne binäre Einheit behandelt, die Sie an andere APIs übergeben können. Um innerhalb des Workflows Ihrer Logik-App auf diesen Inhalt zu verweisen, müssen Sie den Inhalt zuerst konvertieren.

Wenn Sie beispielsweise Inhalt vom Typ `application/xml` übergeben, können Sie mit dem [`@xpath()`-Ausdruck](../logic-apps/workflow-definition-language-functions-reference.md#xpath) eine XPath-Extrahierung durchführen oder mit dem [`@json()`-Ausdruck](../logic-apps/workflow-definition-language-functions-reference.md#json) XML in JSON konvertieren. Informieren Sie sich über das Arbeiten mit unterstützten [Inhaltstypen](../logic-apps/logic-apps-content-type.md).

Um die Ausgabe aus einer eingehenden Anforderung abzurufen, können Sie den [`@triggerOutputs`-Ausdruck](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs) verwenden. Nehmen Sie einmal an, Ihre Ausgabe sieht wie im folgenden Beispiel aus:

```json
{
   "headers": {
      "content-type" : "application/json"
   },
   "body": {
      "myProperty" : "property value"
   }
}
```

Um speziell auf die Eigenschaft `body` zuzugreifen, können Sie den [`@triggerBody()`-Ausdruck](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) als Verknüpfung verwenden.

## <a name="respond-to-requests"></a>Reagieren auf Anforderungen

Auf bestimmte Anforderungen, die Ihre Logik-App auslösen, möchten Sie möglicherweise mit Rückgabe von Inhalten an den Aufrufer reagieren. Um Statuscode, Header und Text für die Antwort zu erstellen, können Sie die Antwortaktion verwenden. Diese Aktion kann an einer beliebigen Stelle in der Logik-App auftreten, nicht nur am Ende des Workflows. Wenn Ihre Logik-App keine Antwortaktion enthält, antwortet der HTTP-Endpunkt *sofort* mit dem Status **202 – akzeptiert**.

Damit der ursprüngliche Aufrufer die Antwort erhält, müssen alle erforderlichen Schritte für die Antwort innerhalb des [Timeoutlimits der Anforderung](./logic-apps-limits-and-config.md) beendet sein, es sei denn, die ausgelöste Logik-App wurde als geschachtelte Logik-App aufgerufen. Wenn innerhalb dieses Limits keine Antwort erfolgt, kommt es bei der eingehenden Anforderung zu einem Timeout mit der Antwort **408 – Clienttimeout**.

Bei geschachtelten Logik-Apps wartet die übergeordnete Logik-App so lange auf eine Antwort, bis alle Schritte abgeschlossen sind, unabhängig davon, wie lange dies dauert.

### <a name="construct-the-response"></a>Erstellen der Antwort

Sie können mehrere Header und jeden Inhaltstyp in den Antworttext einbeziehen. Der Header der folgenden Antwort gibt beispielsweise an, dass der Inhaltstyp der Antwort `application/json` lautet und dass der Text Werte für die Eigenschaften `town` und `postalCode` enthält, basierend auf dem weiter oben in diesem Thema beschriebenen JSON-Schemas für den Anforderungstrigger.

![Bereitstellen des Antwortinhalts für die HTTP-Antwortaktion](./media/logic-apps-http-endpoint/content-for-response-action.png)

Antworten haben folgende Eigenschaften:

| Eigenschaft (Anzeige) | Eigenschaft (JSON) | BESCHREIBUNG |
|--------------------|-----------------|-------------|
| **Statuscode** | `statusCode` | Der HTTP-Statuscode, der in der Antwort auf die eingehende Anforderung verwendet werden soll. Dieser Code kann jeder gültige Statuscode sein, der mit 2xx, 4xx oder 5xx beginnt. Mit 3xx beginnende Statuscodes sind jedoch nicht zulässig. |
| **Header** | `headers` | Mindestens ein Header, der in die Antwort eingefügt werden soll. |
| **Text** | `body` | Ein Textobjekt, das eine Zeichenfolge, ein JSON-Objekt oder sogar binäre Inhalte enthalten kann und auf das in einem vorherigen Schritt verwiesen wird. |
||||

Um die JSON-Definition der Antwortaktion und die vollständige JSON-Definition Ihrer Logik-App anzuzeigen, wählen Sie in der Symbolleiste des Logik-App-Designers die Option **Codeansicht** aus.

``` json
"Response": {
   "type": "Response",
   "kind": "http",
   "inputs": {
      "body": {
         "postalCode": "@triggerBody()?['address']?['postalCode']",
         "town": "@triggerBody()?['address']?['town']"
      },
      "headers": {
         "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {}
}
```

## <a name="q--a"></a>Fragen und Antworten

#### <a name="q-what-about-url-security"></a>F: Wie sieht es mit der URL-Sicherheit aus?

**A:** Azure generiert über eine [Shared Access Signature (SAS)](https://docs.microsoft.com/rest/api/storageservices/delegate-access-with-shared-access-signature) auf sichere Weise Rückruf-URLs für Logik-Apps. Diese Signatur wird als Abfrageparameter übergeben und muss überprüft werden, bevor Ihre Logik-App ausgeführt werden kann. Azure generiert die Signatur durch eine eindeutige Kombination aus einem geheimen Schlüssel pro Logik-App, dem Namen des Triggers und dem ausgeführten Vorgang. Nur wenn ein Benutzer Zugriff auf den geheimen Logik-App-Schlüssel hat, kann er eine gültige Signatur generieren.

> [!IMPORTANT]
> Bei Produktions- und sicheren Systemen wird dringend davon abgeraten, Ihre Logik-App direkt aus dem Browser aufzurufen. Das hat folgende Gründe:
>
> * Der Schlüssel für den gemeinsamen Zugriff ist in der URL enthalten.
> * Sie können keine Richtlinien für sichere Inhalte verwalten, da Domänen von Azure Logic Apps-Benutzern gemeinsam verwendet werden.

#### <a name="q-can-i-configure-http-endpoints-further"></a>F: Kann ich weitere HTTP-Endpunkte konfigurieren?

**A:** Ja, HTTP-Endpunkte unterstützen eine erweiterte Konfiguration über [Azure API Management](../api-management/api-management-key-concepts.md). Dieser Dienst ermöglicht Ihnen auch die konsistente Verwaltung aller APIs einschließlich der Logik-Apps, das Einrichten benutzerdefinierter Domänennamen, Verwenden weiterer Authentifizierungsmethoden und vieles mehr, z.B.:

* [Ändern der Anforderungsmethode](../api-management/api-management-advanced-policies.md#SetRequestMethod)
* [Ändern der URL-Segmente der Anforderung](../api-management/api-management-transformation-policies.md#RewriteURL)
* Einrichten Ihrer API Management-Domänen im [Azure-Portal](https://portal.azure.com/)
* Einrichten der Richtlinie zum Überprüfen auf Standardauthentifizierung

## <a name="next-steps"></a>Nächste Schritte

* [Empfangen von und Antworten auf HTTPS-Aufrufe mittels Azure Logic Apps](../connectors/connectors-native-reqres.md)