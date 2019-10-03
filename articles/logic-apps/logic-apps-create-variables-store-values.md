---
title: Erstellen und Verwalten von Variablen zum Speichern von Werten – Azure Logic Apps
description: So speichern und verwalten Sie Werte, indem Sie Variablen in Azure Logic Apps verwenden.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 0f7947e4a96a49e3a7a3b0059a1b20b21ac8cbd1
ms.sourcegitcommit: a19bee057c57cd2c2cd23126ac862bd8f89f50f5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71180789"
---
# <a name="store-and-manage-values-by-using-variables-in-azure-logic-apps"></a>Speichern und Verwalten von Werten mittels Variablen in Azure Logic Apps

In diesem Artikel erfahren Sie, wie Sie Variablen erstellen und damit arbeiten, die Sie zum Speichern von Werten in Ihrer Logik-App verwenden. Beispielsweise können Sie mithilfe von Variablen nachverfolgen, wie oft eine Schleife ausgeführt wird. Um ein Array zu durchlaufen oder ein Array auf ein bestimmtes Element zu überprüfen, können Sie eine Variable verwenden, um auf die Indexnummern für die einzelnen Arrayelemente zu verweisen.

Variablen können für Datentypen wie „integer“, „float“, „boolean“, „string“, „array“ und „object“ erstellt werden. Nachdem Sie eine Variable erstellt haben, können Sie weitere Aufgaben ausführen, wie z.B.:

* Abrufen des Werts einer Variablen oder Verweisen auf den Wert
* Erhöhen oder Verringern der Variable um einen konstanten Wert (*increment* und *decrement*)
* Zuweisen eines anderen Werts zu der Variablen
* Einfügen oder *Anfügen* des Variablenwerts als Letztes in einer Zeichenfolge oder einem Array

Vorhandene Variabeln sind nur innerhalb der Logic App-Instanz, die sie erstellt, global. Außerdem bleiben sie über alle Schleifeniterationen innerhalb einer Logic App-Instanz bestehen. Wenn Sie auf eine Variable verweisen, verwenden Sie den Namen der Variablen als Token und nicht den Namen der Aktion, was die übliche Vorgehensweise wäre, um auf die Ausgaben einer Aktion zu verweisen.

> [!IMPORTANT]
> Standardmäßig werden Zyklen in einer „ForEach“-Schleife parallel ausgeführt. Wenn Sie Variablen in Schleifen verwenden, führen Sie die Schleife [sequenziell](../logic-apps/logic-apps-control-flow-loops.md#sequential-foreach-loop) aus, damit Variablen vorhersagbare Ergebnisse zurückgeben.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Die Logik-App, für die Sie die Variable erstellen möchten.

  Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md) und [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

* Ein [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts), der als erster Schritt in Ihrer Logik-App verwendet wird.

  Bevor Sie Aktionen zum Erstellen und Arbeiten mit Variablen hinzufügen können, muss Ihre Logik-App mit einem Trigger beginnen.

<a name="create-variable"></a>

## <a name="initialize-variable"></a>Initialisieren einer Variablen

Sie können eine Variable erstellen und ihren Datentyp und Anfangswert deklarieren – alles innerhalb einer Aktion in Ihrer Logik-App. Variablen können nur auf globaler Ebene deklariert werden, nicht innerhalb von Bereichen, Bedingungen und Schleifen.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) oder in Visual Studio Ihre Logik-App im Logik-App-Designer.

   In diesem Beispiel werden das Azure-Portal und eine Logik-App mit einem vorhandenen Trigger verwendet.

1. Führen Sie in Ihrer Logik-App unter dem Schritt, in dem Sie eine Variable hinzufügen möchten, einen dieser Schritte aus: 

   * Zum Hinzufügen einer Aktion unter dem letzten Schritt wählen Sie **Neuer Schritt** aus.

     ![Hinzufügen einer Aktion](./media/logic-apps-create-variables-store-values/add-action.png)

   * Zum Hinzufügen einer Aktion zwischen Schritten bewegen Sie den Mauszeiger über den Verbindungspfeil, sodass das Pluszeichen ( **+** ) angezeigt wird. Wählen Sie das Pluszeichen und dann die Option **Aktion hinzufügen** aus.

1. Geben Sie unter **Aktion auswählen** im Suchfeld `variables` als Filter ein. Wählen Sie in der Liste der Aktionen **Variable initialisieren** aus.

   ![Aktion select](./media/logic-apps-create-variables-store-values/select-initialize-variable-action.png)

1. Geben Sie diese Informationen zu Ihrer Variablen wie unten beschrieben an:

   | Eigenschaft | Erforderlich | Value |  BESCHREIBUNG |
   |----------|----------|-------|--------------|
   | **Name** | Ja | <*Variablenname*> | Der Name für die zu erhöhende Variable |
   | **Typ** | Ja | <*Variablentyp*> | Der Datentyp für die Variable |
   | **Wert** | Nein | <*Anfangswert*> | Der Anfangswert für die Variable <p><p>**Tipp**: Obwohl es sich um eine optionale Einstellung handelt, ist es eine bewährte Methode, diesen Wert einzustellen, damit Sie immer den Anfangswert für Ihre Variable kennen. |
   |||||

   Beispiel:

   ![Initialisieren einer Variablen](./media/logic-apps-create-variables-store-values/initialize-variable.png)

1. Jetzt können Sie fortfahren und die gewünschten Aktionen hinzufügen. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus.

Wenn Sie vom Designer in den Code-Editor wechseln, wird die Aktion **Variable initialisieren** innerhalb Ihrer Logik-App-Definition angezeigt, die im JavaScript Object Notation (JSON)-Format vorliegt:

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   }
},
```

> [!NOTE]
> Obwohl die Aktion **Variable initialisieren** einen `variables`-Abschnitt aufweist, der als Array strukturiert ist, kann die Aktion jeweils nur eine Variable erstellen. Jede neue Variable erfordert eine einzelne Aktion **Variable initialisieren**.

Dies sind weitere Beispiele für Variablentypen:

*String-Variable*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myStringVariable",
               "type": "String",
               "value": "lorem ipsum"
          } ]
      },
      "runAfter": {}
   }
},
```

*Boolean-Variable*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myBooleanVariable",
               "type": "Boolean",
               "value": false
          } ]
      },
      "runAfter": {}
   }
},
```

*Array mit Integern*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": [1, 2, 3]
          } ]
      },
      "runAfter": {}
   }
},
```

*Array mit Zeichenfolgen*

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "myArrayVariable",
               "type": "Array",
               "value": ["red", "orange", "yellow"]
          } ]
      },
      "runAfter": {}
   }
},
```

<a name="get-value"></a>

## <a name="get-the-variables-value"></a>Abrufen des Variablenwerts

Um den Inhalt einer Variablen abzurufen oder darauf zu verweisen, können Sie im Logik-App-Designer und im Code-Editor auch die [variables()-Funktion](../logic-apps/workflow-definition-language-functions-reference.md#variables) verwenden. Zum Verweisen auf eine Variable verwenden Sie den Namen der Variablen als Token und nicht den Namen der Aktion, was die übliche Vorgehensweise wäre, um auf die Ausgaben einer Aktion zu verweisen.

Dieser Ausdruck ruft zum Beispiel die Elemente aus der Arrayvariablen ab, die [zuvor in diesem Artikel](#append-value) mit der Funktion `variables()` erstellt wurde. Die Funktion `string()` gibt den Inhalt der Variablen im Zeichenfolgenformat zurück: `"1, 2, 3, red"`

```json
@{string(variables('myArrayVariable'))}
```

<a name="increment-value"></a>

## <a name="increment-variable"></a>Erhöhen eines Variablenwerts 

Um eine Variable um einen konstanten Wert zu erhöhen oder zu *inkrementieren*, fügen Sie die Aktion **Variable schrittweise erhöhen** zu Ihrer Logik-App hinzu. Diese Aktion funktioniert nur bei den Variablen „integer“ und „float“.

1. Wählen Sie im Logik-App-Designer unter dem Schritt, in dem Sie eine vorhandene Variable erhöhen möchten, **Neuer Schritt** aus. 

   Zum Beispiel verfügt diese Logik-App bereits über einen Trigger und eine Aktion, die eine Variable erstellt hat. Fügen Sie deshalb eine neue Aktion unter diesen Schritten hinzu:

   ![Hinzufügen einer Aktion](./media/logic-apps-create-variables-store-values/add-increment-variable-action.png)

   Zum Hinzufügen einer Aktion zwischen vorhandenen Schritten bewegen Sie den Mauszeiger über den Verbindungspfeil, sodass das Pluszeichen (+) angezeigt wird. Wählen Sie das Pluszeichen und dann die Option **Aktion hinzufügen** aus.

1. Geben Sie im Suchfeld den Begriff „Variable schrittweise erhöhen“ als Filter ein. Wählen Sie in der Liste die Aktion **Variable schrittweise erhöhen** aus.

   ![Auswählen der Aktion „Variable schrittweise erhöhen“](./media/logic-apps-create-variables-store-values/select-increment-variable-action.png)

1. Geben Sie diese Informationen zum schrittweisen Erhöhen Ihrer Variable an:

   | Eigenschaft | Erforderlich | Value |  BESCHREIBUNG |
   |----------|----------|-------|--------------|
   | **Name** | Ja | <*Variablenname*> | Der Name für die zu erhöhende Variable |
   | **Wert** | Nein | <*Inkrementwert*> | Der zum Erhöhen der Variablen verwendete Wert. Der Standardwert ist eins. <p><p>**Tipp**: Obwohl es sich um eine optionale Einstellung handelt, ist es eine bewährte Methode, diesen Wert einzustellen, damit Sie immer den spezifischen Wert für die schrittweise Erhöhung Ihrer Variablen kennen. |
   ||||

   Beispiel:

   ![Beispiel für Inkrementwert](./media/logic-apps-create-variables-store-values/increment-variable-action-information.png)

1. Wenn Sie fertig sind, wählen Sie auf der Symbolleiste des Designers die Option **Speichern** aus.

Wenn Sie vom Designer in den Code-Editor wechseln, wird die Aktion **Variable schrittweise erhöhen** innerhalb Ihrer Logik-App-Definition im JSON-Format angezeigt:

```json
"actions": {
   "Increment_variable": {
      "type": "IncrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```

## <a name="example-create-loop-counter"></a>Beispiel: Erstellen eines Schleifenzählers

Variablen werden häufig verwendet, um die Anzahl der Durchläufe einer Schleife zu zählen. Dieses Beispiel zeigt, wie Sie Variablen für diese Aufgabe erstellen und verwenden, indem Sie eine Schleife erstellen, die die Anlagen in einer E-Mail zählt.

1. Erstellen Sie im Azure-Portal eine leere Logik-App. Fügen Sie einen Trigger hinzu, der nach neuen E-Mails und Anlagen sucht.

   In diesem Beispiel wird der Office 365 Outlook-Trigger für **Wenn eine neue E-Mail empfangen wird** verwendet. Sie können diesen Trigger so einstellen, dass er nur dann ausgelöst wird, wenn die E-Mail Anlagen enthält. Es ist auch möglich, einen beliebigen Connector zu verwenden, der nach neuen E-Mails mit Anlagen sucht, wie z. B. der Outlook.com-Connector.

1. Wählen Sie in dem Trigger, um nach Anlagen zu suchen und diese an den Workflow Ihrer Logik-App zu übergeben, für diese Eigenschaften **Ja** aus:

   * **Mit Anlage**
   * **Anlagen einschließen**

   ![Suche nach und Anfügen von Anlagen](./media/logic-apps-create-variables-store-values/check-include-attachments.png)

1. Fügen Sie die [Aktion **Variable initialisieren**](#create-variable) hinzu. Erstellen Sie eine integer-Variable namens `Count` mit einem Anfangswert von Null.

   ![Hinzufügen der Aktion „Variable initialisieren“](./media/logic-apps-create-variables-store-values/initialize-variable.png)

1. Zum Durchlaufen der einzelnen Anlagen fügen Sie eine *For Each*-Schleife hinzu.

   1. Wählen Sie unter der Aktion **Variable initialisieren** die Option **Neuer Schritt** aus.

   1. Wählen Sie unter **Aktion auswählen** die Option **Integriert** aus. Geben Sie in das Suchfeld `for each` als Suchfilter ein, und wählen Sie **For Each** aus.

      ![Hinzufügen einer „For Each“-Schleife](./media/logic-apps-create-variables-store-values/add-loop.png)

1. Klicken Sie in der Schleife in das Feld **Ausgabe von vorherigen Schritten auswählen**. In der dann angezeigten Liste „Dynamischer Inhalt“ wählen Sie **Anlagen** aus.

   ![Auswählen von „Anlagen“](./media/logic-apps-create-variables-store-values/select-attachments.png)

   Die Eigenschaft **Anlagen** übergibt ein Array mit den E-Mail-Anhängen aus der Ausgabe des Triggers an Ihre Schleife.

1. Wählen Sie in der **For Each**-Schleife den Befehl **Aktion hinzufügen** aus.

   ![Auswählen von „Aktion hinzufügen“](./media/logic-apps-create-variables-store-values/add-action-2.png)

1. Geben Sie im Suchfeld den Begriff „Variable schrittweise erhöhen“ als Filter ein. Wählen Sie in der Liste der Aktionen **Variable schrittweise erhöhen** aus.

   > [!NOTE]
   > Stellen Sie sicher, dass die Aktion **Variable schrittweise erhöhen** innerhalb der Schleife angezeigt wird. Wenn die Aktion außerhalb der Schleife erscheint, ziehen Sie sie in die Schleife.

1. Wählen Sie in der Aktion **Variable schrittweise erhöhen** in der Liste **Name** die Variable **Anzahl** aus.

   ![Auswählen der Variablen „Anzahl“](./media/logic-apps-create-variables-store-values/add-increment-variable-example.png)

1. Fügen Sie unter der Schleife eine beliebige Aktion hinzu, die Ihnen die Anzahl der Anlagen sendet. Nehmen Sie in Ihre Aktion den Wert aus der Variablen **Anzahl** auf, wie z.B.:

   ![Hinzufügen einer Aktion, die Ergebnisse sendet](./media/logic-apps-create-variables-store-values/send-email-results.png)

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

### <a name="test-your-logic-app"></a>Testen Ihrer Logik-App

1. Wenn Ihre Logik-App nicht aktiviert ist, wählen Sie im Menü der Logik-App die Option **Überblick** aus. Wählen Sie auf der Symbolleiste **Aktivieren** aus.

1. Wählen Sie auf der Symbolleiste des Logik-App-Designers **Ausführen** aus. Dadurch wird die Logik-App manuell gestartet.

1. Senden Sie eine E-Mail mit einer oder mehreren Anlagen an das in diesem Beispiel verwendete E-Mail-Konto.

   Dies löst den Trigger der Logik-App aus, der eine Instanz für den Workflow Ihrer Logik-App erstellt und ausführt. Als Ergebnis sendet Ihnen die Logik-App eine Nachricht oder E-Mail, die die Anzahl der Anlagen in der von Ihnen gesendeten E-Mail enthält.

Wenn Sie vom Designer in den Code-Editor wechseln, erscheint die **For Each**-Schleife auf diese Weise zusammen mit der Aktion **Variable schrittweise erhöhen** innerhalb Ihrer Logik-App-Definition, die im JSON-Format vorliegt.

```json
"actions": {
   "For_each": {
      "type": "Foreach",
      "actions": {
         "Increment_variable": {
           "type": "IncrementVariable",
            "inputs": {
               "name": "Count",
               "value": 1
            },
            "runAfter": {}
         }
      },
      "foreach": "@triggerBody()?['Attachments']",
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="decrement-value"></a>

## <a name="decrement-variable"></a>Verringern eines Variablenwerts

Um eine Variable um einen konstanten Wert zu verringern oder zu *dekrementieren*, folgen Sie den Schritten zum [Erhöhen eines Variablenwerts](#increment-value), wobei Sie in diesem Fall stattdessen die Aktion **Variablenwert verringern** suchen und auswählen. Diese Aktion funktioniert nur bei den Variablen „integer“ und „float“.

Für die Aktion **Variablenwert verringern** gibt es folgende Eigenschaften:

| Eigenschaft | Erforderlich | Value |  BESCHREIBUNG |
|----------|----------|-------|--------------|
| **Name** | Ja | <*Variablenname*> | Der Name für die zu verringernde Variable | 
| **Wert** | Nein | <*Inkrementwert*> | Der zum Verringern der Variablen verwendete Wert. Der Standardwert ist eins. <p><p>**Tipp**: Obwohl es sich um eine optionale Einstellung handelt, ist es eine bewährte Methode, diesen Wert einzustellen, damit Sie immer den spezifischen Wert für die schrittweise Verringerung Ihrer Variablen kennen. |
||||| 

Wenn Sie vom Designer in den Code-Editor wechseln, wird die Aktion **Variablenwert verringern** innerhalb Ihrer Logik-App-Definition im JSON-Format angezeigt.

```json
"actions": {
   "Decrement_variable": {
      "type": "DecrementVariable",
      "inputs": {
         "name": "Count",
         "value": 1
      },
      "runAfter": {}
   }
},
```

<a name="assign-value"></a>

## <a name="set-variable"></a>Festlegen der Variablen

Um einer vorhandenen Variablen einen anderen Wert zuzuweisen, folgen Sie den Schritten zum [Erhöhen eines Variablenwerts](#increment-value) mit Ausnahme von diesen Schritten:

1. Suchen und wählen Sie in diesem Fall stattdessen die Aktion **Variable festlegen**.

1. Geben Sie den zuzuweisenden Variablennamen und Wert an. Sowohl der neue Wert als auch die Variable müssen den gleichen Datentyp aufweisen. Der Wert wird benötigt, da für diese Aktion kein Standardwert vorhanden ist.

Für die Aktion **Variable festlegen** gibt es folgende Eigenschaften:

| Eigenschaft | Erforderlich | Value |  BESCHREIBUNG |
|----------|----------|-------|--------------|
| **Name** | Ja | <*Variablenname*> | Der Name für die zu ändernde Variable |
| **Wert** | Ja | <*Neuer-Wert*> | Der Wert, der der Variable zugewiesen werden soll. Beide müssen den gleichen Datentyp aufweisen. |
||||| 

> [!NOTE]
> Wenn Sie keine Variablen inkrementieren oder dekrementieren, *kann* das Ändern von Variablen innerhalb von Schleifen zu unerwarteten Ergebnissen führen, da Schleifen standardmäßig parallel oder gleichzeitig ausgeführt werden. Versuchen Sie in diesen Fällen, Ihre Schleife so einzustellen, dass sie sequenziell ausgeführt wird. Wenn Sie beispielsweise auf den Variablenwert innerhalb der Schleife verweisen wollen und am Anfang und am Ende der Schleifeninstanz den gleichen Wert erwarten, folgen Sie diesen Schritten, um die Ausführung der Schleife zu ändern: 
>
> 1. Wählen Sie in der oberen rechten Ecke Ihrer Schleife die Ellipsenschaltfläche ( **...** ) und dann **Einstellungen** aus.
> 
> 2. Ändern Sie unter **Steuerung der Parallelität** die Einstellung **Standard außer Kraft setzen** auf **Ein**.
>
> 3. Ziehen Sie den Schieberegler **Parallelitätsgrad** auf **1**.

Wenn Sie vom Designer in den Code-View-Editor wechseln, erscheint die Aktion **Variable festlegen** innerhalb Ihrer Logik-App-Definition, die im JSON-Format vorliegt. Dieses Beispiel ändert den aktuellen Wert der Variablen`Count` in einen anderen Wert.

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
               "name": "Count",
               "type": "Integer",
               "value": 0
          } ]
      },
      "runAfter": {}
   },
   "Set_variable": {
      "type": "SetVariable",
      "inputs": {
         "name": "Count",
         "value": 100
      },
      "runAfter": {
         "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

<a name="append-value"></a>

## <a name="append-to-variable"></a>Anfügen an Variable

Bei Variablen, die Strings oder Arrays speichern, können Sie den Wert einer Variablen als letztes Element in diesen Zeichenfolgen oder Arrays *anfügen* oder anhängen. Führen Sie die Schritte zum [Erhöhen eines Variablenwerts](#increment-value) aus, mit dem Unterschied, dass Sie stattdessen folgende Schritte ausführen: 

1. Suchen und wählen Sie eine dieser Aktionen basierend darauf, ob Ihre Variable eine Zeichenfolge oder ein Array ist: 

   * **An Zeichenfolgenvariable anfügen**
   * **An Arrayvariable anfügen** 

1. Geben Sie den Wert an, der als letztes Element in der Zeichenfolge oder im Array angefügt werden soll. Dieser Wert ist erforderlich.

Für die Aktionen **An ... anfügen** gibt es folgende Eigenschaften:

| Eigenschaft | Erforderlich | Value |  BESCHREIBUNG |
|----------|----------|-------|--------------|
| **Name** | Ja | <*Variablenname*> | Der Name für die zu ändernde Variable |
| **Wert** | Ja | <*Anzufügender-Wert*> | Der anzufügende Wert, der von einem beliebigen Typ sein kann. |
|||||

Wenn Sie vom Designer in den Code-View-Editor wechseln, erscheint die Aktion **An Arrayvariable anfügen** innerhalb Ihrer Logik-App-Definition, die im JSON-Format vorliegt. In diesem Beispiel wird eine Arrayvariable erstellt und ein weiterer Wert als letztes Element im Array hinzugefügt. Das Ergebnis ist eine aktualisierte Variable, die dieses Array enthält: `[1,2,3,"red"]`

```json
"actions": {
   "Initialize_variable": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ {
            "name": "myArrayVariable",
            "type": "Array",
            "value": [1, 2, 3]
         } ]
      },
      "runAfter": {}
   },
   "Append_to_array_variable": {
      "type": "AppendToArrayVariable",
      "inputs": {
         "name": "myArrayVariable",
         "value": "red"
      },
      "runAfter": {
        "Initialize_variable": [ "Succeeded" ]
      }
   }
},
```

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Logic Apps-Connectors](../connectors/apis-list.md).
