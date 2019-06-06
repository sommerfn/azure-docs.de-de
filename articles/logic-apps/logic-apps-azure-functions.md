---
title: Hinzufügen und Ausführen von Code in Azure Logic Apps mit Azure Functions
description: Hinzufügen und Ausführen von Code in Azure Logic Apps mit Azure Functions
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.topic: article
ms.date: 08/20/2018
ms.reviewer: klam, LADocs
ms.openlocfilehash: 9b304f2d4d2e498701be5977decf202cb0fa995b
ms.sourcegitcommit: d73c46af1465c7fd879b5a97ddc45c38ec3f5c0d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65922070"
---
# <a name="add-and-run-code-by-using-azure-functions-in-azure-logic-apps"></a>Hinzufügen und Ausführen von Code mithilfe von Azure Functions in Azure Logic Apps

Wenn Sie Code ausführen möchten, der in Ihren Logik-Apps für einen bestimmten Auftrag verantwortlich ist, können Sie mit [Azure Functions](../azure-functions/functions-overview.md) eigene Funktionen erstellen. Mit diesem Dienst können Sie Node.js-, C#- and F#-Code erstellen, sodass Sie keine vollständige App oder die Infrastruktur zur Ausführung Ihres Codes erstellen müssen. Sie können [Logik-Apps auch aus Azure-Funktionen aufrufen](#call-logic-app).
Azure Functions ermöglicht serverloses Computing in der Cloud und ist nützlich, um Aufgaben auszuführen, z.B. diese Beispiele:

* Erweitern Sie das Verhalten Ihrer Logik-App um Funktionen in Node.js oder C#.
* Führen Sie in Ihrem Logik-App-Workflow Berechnungen durch.
* Wenden Sie in Ihren Logik-Apps die erweiterte Formatierung oder Computefelder an.

Informationen zum Ausführen von Codeausschnitten ohne Erstellen von Azure-Funktionen finden Sie unter [Hinzufügen und Ausführen von Inlinecode](../logic-apps/logic-apps-add-run-inline-code.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Eine Azure-Funktionen-App, bei der es sich um einen Container für Azure-Funktionen handelt, und Ihre Azure-Funktion. Wenn noch keine Funktionen-App vorhanden ist, [erstellen Sie zuerst Ihre Funktionen-App](../azure-functions/functions-create-first-azure-function.md). Sie können Ihre Funktion dann entweder [separat außerhalb Ihrer Logik-App](#create-function-external) oder [aus Ihrer Logik-App](#create-function-designer) im Logik-App-Designer erstellen.

  Sowohl für vorhandene als auch für neue Funktionen-Apps und Funktionen gelten die gleichen Anforderungen in Bezug auf die Arbeit mit Logik-Apps:

  * Ihre Funktionen-App muss dasselbe Azure-Abonnement wie Ihre Logik-App aufweisen.

  * Ihre Funktion verwendet einen HTTP-Trigger, z.B. die Funktionsvorlage **HTTP-Trigger** für **JavaScript** oder **C#** . 

    Die HTTP-Trigger-Vorlage kann Inhalte mit dem Typ `application/json` von Ihrer Logik-App akzeptieren. 
    Wenn Sie Ihrer Logik-App eine Azure-Funktion hinzufügen, zeigt der Logik-App-Designer benutzerdefinierte Funktionen, die im Rahmen Ihres Azure-Abonnements aus dieser Vorlage erstellt wurden. 

  * Ihre Funktion verwendet keine benutzerdefinierten Routen – es sei denn, dass Sie eine [OpenAPI-Definition](../azure-functions/functions-openapi-definition.md) festgelegt haben, die früher als [Swagger-Datei](https://swagger.io/) bezeichnet wurde. 
  
  * Wenn Sie für Ihre Funktion eine OpenAPI-Definition festgelegt haben, erhalten Sie im Logik-App-Designer eine umfangreichere Benutzeroberfläche für die Arbeit mit Funktionsparametern. Bevor Ihre Logik-App Funktionen mit OpenAPI-Definitionen finden und darauf zugreifen kann, [müssen Sie Ihre Funktionen-App mit diesen Schritten einrichten](#function-swagger).

* Die Logik-App, in der Sie die Funktion hinzufügen möchten, einschließlich eines [Triggers](../logic-apps/logic-apps-overview.md#logic-app-concepts) als erstem Schritt in Ihrer Logik-App 

  Bevor Sie Aktionen zum Ausführen von Funktionen hinzufügen können, muss Ihre Logik-App über einen Trigger gestartet werden.

  Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

> [!NOTE]
> Logic Apps-Integration in Functions funktioniert nicht, wenn Slots (Vorschau) aktiviert sind.

<a name="create-function-external"></a>

## <a name="create-functions-outside-logic-apps"></a>Erstellen von Funktionen außerhalb von Logik-Apps

Erstellen Sie im [Azure-Portal](https://portal.azure.com) Ihre Azure-Funktionen-App, die über dasselbe Azure-Abonnement wie Ihre Logik-App verfügen muss, und erstellen Sie dann Ihre Azure-Funktion.
Falls Sie mit dem Erstellen von Azure-Funktionen noch nicht vertraut sind, können Sie sich darüber informieren, wie Sie [Ihre erste Funktion im Azure-Portal erstellen](../azure-functions/functions-create-first-azure-function.md). Beachten Sie aber diese Anforderungen zum Erstellen von Funktionen, die Sie aus Logik-Apps aufrufen können:

* Stellen Sie sicher, dass Sie die Funktionsvorlage **HTTP-Trigger** für **JavaScript** oder **C#** auswählen.

  ![HTTP-Trigger – JavaScript oder C#](./media/logic-apps-azure-functions/http-trigger-function.png)

<a name="function-swagger"></a>

* (Optional) Wenn Sie [eine API-Definition generieren](../azure-functions/functions-openapi-definition.md) (früher als [Swagger-Datei](https://swagger.io/) bezeichnet), können Sie für Ihre Funktion eine umfassendere Benutzeroberfläche nutzen, wenn Sie im Logik-App-Designer mit Funktionsparametern arbeiten. Wenn Sie Ihre Funktionen-App so einrichten möchten, dass Ihre Logik-App Funktionen, die über Swagger-Beschreibungen verfügen, finden und nutzen kann, führen Sie die folgenden Schritte aus:

  1. Stellen Sie sicher, dass Ihre Funktionen-App aktiv ausgeführt wird.

  2. Richten Sie in Ihrer Funktionen-App die [Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) mit den folgenden Schritten ein, damit alle Ursprünge zulässig sind:

     1. Wählen Sie über die Liste mit den **Funktionen-Apps** Ihre Funktionen-App und dann **Plattformfeatures** > **CORS** aus.

        ![Auswählen von Funktionen-App > Plattformfeatures > CORS](./media/logic-apps-azure-functions/function-platform-features-cors.png)

     2. Fügen Sie unter **CORS** das Platzhalterzeichen `*` hinzu, aber entfernen Sie alle anderen Ursprünge in der Liste, und wählen Sie **Speichern**.

        ![Legen Sie für "CORS* das Platzhalterzeichen "*" fest.](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

### <a name="access-property-values-inside-http-requests"></a>Zugreifen auf Eigenschaftswerte in HTTP-Anforderungen

Webhook-Funktionen können HTTP-Anforderungen als Eingaben akzeptieren und diese Anforderungen an andere Funktionen übergeben. Obwohl Logic Apps beispielsweise über [Funktionen zum Konvertieren von DateTime-Werten](../logic-apps/workflow-definition-language-functions-reference.md) verfügt, wird in diesem einfachen Beispiel für eine JavaScript-Funktion Folgendes veranschaulicht: Zugreifen auf eine Eigenschaft in einem Anforderungsobjekt, das an die Funktion übergeben wird, und Durchführen von Vorgängen für diesen Eigenschaftswert. Für den Zugriff auf Eigenschaften in Objekten wird in diesem Beispiel der Operator [Punkt (.)](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Property_accessors) verwendet: 

```javascript
function convertToDateString(request, response){
   var data = request.body;
   response = {
      body: data.date.ToDateString();
   }
}
```

In dieser Funktion läuft Folgendes ab:

1. Die Funktion erstellt die Variable `data` und weist das `body`-Objekt im `request`-Objekt dieser Variablen zu. In der Funktion wird der Operator „Punkt“ (.) eingesetzt, um auf das `body`-Objekt im `request`-Objekt zu verweisen: 

   ```javascript
   var data = request.body;
   ```

2. Die Funktion kann nun über die Variable `data` auf die `date`-Eigenschaft zugreifen und diesen Eigenschaftswert aus dem DateTime-Typ in den DateString-Typ konvertieren, indem die Funktion `ToDateString()` aufgerufen wird. Die Funktion gibt auch das Ergebnis über die `body`-Eigenschaft in der Antwort der Funktion zurück: 

   ```javascript
   body: data.date.ToDateString();
   ```

Nachdem Sie nun Ihre Azure-Funktion erstellt haben, können Sie die Schritte zum [Hinzufügen von Funktionen zu Logik-Apps](#add-function-logic-app) ausführen.

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Erstellen von Funktionen innerhalb von Logik-Apps

Bevor Sie eine Azure-Funktion erstellen können, indem Sie in Ihrer Logik-App im Logik-App-Designer beginnen, müssen Sie zuerst über eine Azure-Funktionen-App verfügen. Hierbei handelt es sich um einen Container für Ihre Funktionen. Erstellen Sie zuerst diese Funktionen-App, falls sie noch nicht vorhanden ist. Weitere Informationen finden Sie unter [Erstellen Ihrer ersten Funktion im Azure-Portal](../azure-functions/functions-create-first-azure-function.md). 

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer. 

2. Führen Sie zum Erstellen und Hinzufügen Ihrer Funktion den Schritt aus, der für Ihr Szenario zutrifft:

   * Wählen Sie unter dem letzten Schritt im Workflow Ihrer Logik-App **Neuer Schritt** aus.

   * Bewegen Sie Ihren Mauszeiger zwischen vorhandenen Schritten im Workflow Ihrer Logik-App auf den Pfeil, wählen Sie das Pluszeichen (+) und dann **Aktion hinzufügen** aus.

3. Geben Sie im Suchfeld „azure functions“ als Filter ein.
Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **Azure-Funktion auswählen – Azure Functions** 

   ![Suchen nach „Azure Functions“](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. Wählen Sie in der Liste mit den Funktionen-Apps Ihre Funktionen-App aus. Wählen diese Aktion aus, nachdem die Liste mit den Aktionen geöffnet wurde: **Azure Functions – Neue Funktion erstellen**

   ![Auswählen Ihrer Funktionen-App](./media/logic-apps-azure-functions/select-function-app-create-function.png)

5. Definieren Sie im Editor für Funktionsdefinitionen Ihre Funktion:

   1. Geben Sie im Feld **Funktionsname** einen Namen für Ihre Funktion an. 

   2. Fügen Sie im Feld **Code** der Funktionsvorlage Ihren Code hinzu. Binden Sie auch die Antwort und Nutzlast ein, die für Ihre Logik-App zurückgegeben werden soll, nachdem die Ausführung Ihrer Funktion abgeschlossen ist. 

      ![Definieren Ihrer Funktion](./media/logic-apps-azure-functions/function-definition.png)

      Im Code der Vorlage bezieht sich das *`context`-Objekt* auf die Nachricht, die Ihre Logik-App in einem späteren Schritt über das Feld **Anforderungstext** sendet. 
      Verwenden Sie für den Zugriff auf die Eigenschaften des `context`-Objekts aus Ihrer Funktion heraus die folgende Syntax: 

      `context.body.<property-name>`

      Um beispielsweise auf die `content`-Eigenschaft im `context`-Objekt zu verweisen, verwenden Sie diese Syntax: 

      `context.body.content`

      Der Vorlagencode enthält auch eine `input`-Variable, in der der Wert aus dem Parameter `data` gespeichert wird, damit Ihre Funktion Vorgänge an diesem Wert durchführen kann. 
      Innerhalb der JavaScript-Funktionen ist die `data`-Variable außerdem eine Kurzform für `context.body`.

      > [!NOTE]
      > Die `body`-Eigenschaft gilt hier für das `context`-Objekt und ist mit dem **Body**-Token aus der Ausgabe einer Aktion nicht identisch, das Sie möglicherweise ebenfalls an Ihre Funktion übergeben. 
 
   3. Wenn Sie fertig sind, wählen Sie **Erstellen** aus.

6. Geben Sie im Feld **Anforderungstext** die Eingabe für Ihre Funktion ein, die als JSON-Objekt (JavaScript Object Notation) formatiert werden muss. 

   Diese Eingabe ist das *Kontextobjekt* oder die Nachricht, das bzw. die von Ihrer Logik-App an Ihre Funktion gesendet wird. Wenn Sie in das Feld **Anforderungstext** klicken, wird die Liste mit dem dynamischen Inhalt angezeigt, und Sie können Token für Ausgaben aus vorherigen Schritten auswählen. In diesem Beispiel wird angegeben, dass die Kontextnutzlast die Eigenschaft `content` enthält, die den Wert des Tokens **Von** aus dem E-Mail-Trigger aufweist:

   ![Beispiel „Anforderungstext“ –Nutzlast des Kontextobjekts](./media/logic-apps-azure-functions/function-request-body-example.png)

   Hier wird das Kontextobjekt nicht in eine Zeichenfolge umgewandelt, sodass der Inhalt des Objekts der JSON-Nutzlast direkt hinzugefügt wird. Wenn das Kontextobjekt aber kein JSON-Token ist, das eine Zeichenfolge, ein JSON-Objekt oder ein JSON-Array übergibt, wird eine Fehlermeldung angezeigt. Wurde in diesem Beispiel stattdessen das Token **Empfangszeit** verwendet, können Sie das Kontextobjekt in eine Zeichenfolge umwandeln, indem Sie doppelte Anführungszeichen hinzufügen:  

   ![Umwandeln eines Objekts in eine Zeichenfolge](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

7. Wählen Sie **Erweiterte Optionen anzeigen**, um andere Details anzugeben, z.B. die zu verwendende Methode, Anforderungsheader oder Abfrageparameter.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Hinzufügen von vorhandenen Funktionen zu Logik-Apps

Zum Aufrufen von vorhandenen Azure-Funktionen aus Ihren Logik-Apps können Sie Azure-Funktionen wie alle anderen Aktionen im Logik-App-Designer hinzufügen. 

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer. 

2. Wählen Sie im Schritt zum Hinzufügen der Funktion **Neuer Schritt** > **Aktion hinzufügen**. 

3. Geben Sie im Suchfeld „azure functions“ als Filter ein.
Wählen Sie in der Liste mit den Aktionen diese Aktion aus: **Azure-Funktion auswählen – Azure Functions** 

   ![Suchen nach „Azure Functions“](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. Wählen Sie in der Liste mit den Funktionen-Apps Ihre Funktionen-App aus. Wählen Sie Ihre Funktion aus, wenn die Liste mit den Funktionen angezeigt wird. 

   ![Auswählen Ihrer Funktionen-App und Azure-Funktion](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   Für Funktionen, die über API-Definitionen (Swagger-Beschreibungen) verfügen und [so eingerichtet sind, dass Ihre Logik-App diese Funktionen finden und darauf zugreifen kann](#function-swagger), können Sie **Swagger-Aktionen** auswählen:

   ![Auswählen von Funktionen-App, „Swagger-Aktionen“ und Azure-Funktion](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

5. Geben Sie im Feld **Anforderungstext** die Eingabe für Ihre Funktion ein, die als JSON-Objekt (JavaScript Object Notation) formatiert werden muss. 

   Diese Eingabe ist das *Kontextobjekt* oder die Nachricht, das bzw. die von Ihrer Logik-App an Ihre Funktion gesendet wird. Wenn Sie in das Feld **Anforderungstext** klicken, wird die Liste mit dem dynamischen Inhalt angezeigt, und Sie können Token für Ausgaben aus vorherigen Schritten auswählen. In diesem Beispiel wird angegeben, dass die Kontextnutzlast die Eigenschaft `content` enthält, die den Wert des Tokens **Von** aus dem E-Mail-Trigger aufweist:

   ![Beispiel „Anforderungstext“ –Nutzlast des Kontextobjekts](./media/logic-apps-azure-functions/function-request-body-example.png)

   Hier wird das Kontextobjekt nicht in eine Zeichenfolge umgewandelt, sodass der Inhalt des Objekts der JSON-Nutzlast direkt hinzugefügt wird. Wenn das Kontextobjekt aber kein JSON-Token ist, das eine Zeichenfolge, ein JSON-Objekt oder ein JSON-Array übergibt, wird eine Fehlermeldung angezeigt. Wurde in diesem Beispiel stattdessen das Token **Empfangszeit** verwendet, können Sie das Kontextobjekt in eine Zeichenfolge umwandeln, indem Sie doppelte Anführungszeichen hinzufügen: 

   ![Umwandeln eines Objekts in eine Zeichenfolge](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

6. Wählen Sie **Erweiterte Optionen anzeigen**, um andere Details anzugeben, z.B. die zu verwendende Methode, Anforderungsheader oder Abfrageparameter.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-functions"></a>Aufrufen von Logik-Apps aus Funktionen

Wenn Sie eine Logik-App aus einer Azure-Funktion auslösen möchten, muss diese App mit einem Trigger starten, der einen aufrufbaren Endpunkt bereitstellt. So können Sie beispielsweise die Logik-App mit dem Trigger **HTTP**, **Anforderung**, **Azure-Warteschlangen** oder **Event Grid** starten. Senden Sie in Ihrer Funktion eine HTTP POST-Anforderung an die URL des Triggers, und binden Sie die Nutzlast ein, die von dieser Logik-App verarbeitet werden soll. Weitere Informationen hierzu finden Sie unter [Aufrufen, Auslösen oder Schachteln von Logik-Apps](../logic-apps/logic-apps-http-endpoint.md). 

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Logic Apps-Connectors](../connectors/apis-list.md).
