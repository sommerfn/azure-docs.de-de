---
title: Hinzufügen von switch-Anweisungen zu Workflows – Azure Logic Apps | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie switch-Anweisungen erstellen, die Workflowaktionen auf der Grundlage bestimmter Werte in Azure Logic Apps steuern.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/08/2018
ms.openlocfilehash: 2a3f8ee5cba3110d392555fad78c1cb2513b5d4e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60683135"
---
# <a name="create-switch-statements-that-run-workflow-actions-based-on-specific-values-in-azure-logic-apps"></a>Erstellen von switch-Anweisungen, die Workflowaktionen auf der Grundlage bestimmter Werte in Azure Logic Apps ausführen

Fügen Sie eine Anweisung vom Typ *switch* hinzu, wenn Sie bestimmte Aktionen auf der Grundlage der Werte von Objekten, Ausdrücken oder Token ausführen möchten. Diese Struktur wertet das Objekt, den Ausdruck oder das Token aus, wählt den entsprechenden Fall für das Ergebnis und bestimmte Aktionen für genau diesen Fall aus. Beim Ausführen der Switch-Anweisung sollte nur ein Fall mit dem Ergebnis übereinstimmen.

Angenommen, Sie möchten, dass eine Logik-App verschiedene Schritte basierend auf einer Option ausführt, die in einer E-Mail ausgewählt ist. In diesem Beispiel überprüft die Logik-App den RSS-Feed einer Website auf neuen Inhalt. Wenn ein neues Element im RSS-Feed angezeigt wird, sendet die Logik-App eine E-Mail an eine genehmigende Person. Je nachdem, ob die genehmigende Person „Genehmigen“ oder „Ablehnen“ auswählt, führt die Logik-App unterschiedliche Schritte aus.

> [!TIP]
> Wie alle Programmiersprachen unterstützt die switch-Anweisung nur Gleichheitsoperatoren. Wenn Sie andere relationale Operatoren verwenden müssen – z.B. „größer als“ – verwenden Sie eine [Bedingungsanweisung](../logic-apps/logic-apps-control-flow-conditional-statement.md).
> Um deterministisches Ausführungsverhalten zu gewährleisten, müssen Fälle einen eindeutigen und statischen Wert anstelle von dynamischen Token oder Ausdrücken enthalten.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Um dem Beispiel in diesem Artikel zu folgen, [erstellen Sie diese Beispiel-Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) mit einem Outlook.com- oder Office 365 Outlook-Konto.

  1. Beim Hinzufügen der Aktion zum Senden von E-Mail suchen Sie stattdessen diese Aktion, und wählen Sie sie aus: **Genehmigungs-E-Mail senden**.

     ![Auswählen von „Genehmigungs-E-Mail senden“](./media/logic-apps-control-flow-switch-statement/send-approval-email-action.png)

  1. Füllen Sie die Pflichtfelder wie „E-Mail-Adresse“ für die Person aus, die die Genehmigungs-E-Mail erhält. 
  Geben Sie unter **Benutzeroptionen** „Genehmigen, Ablehnen“ ein.

     ![Eingeben von E-Mail-Details](./media/logic-apps-control-flow-switch-statement/send-approval-email-details.png)

## <a name="add-switch-statement"></a>Hinzufügen einer switch-Anweisung

1. Fügen Sie für dieses Beispiel am Ende Ihres Beispielworkflows eine switch-Anweisung hinzu. Wählen Sie nach dem letzten Schritt **Neuer Schritt** aus.

   Wenn Sie eine switch-Anweisung zwischen Schritten hinzufügen möchten, bewegen Sie den Zeiger auf den Pfeil an der Stelle, an der Sie die switch-Anweisung hinzufügen möchten. Wählen Sie das daraufhin angezeigte **Pluszeichen** ( **+** ) aus, und wählen Sie dann **Aktion hinzufügen** aus.

1. Geben Sie im Suchfeld den Begriff „switch“ als Filter ein. Wählen Sie diese Aktion aus: **Switch – steuern**.

   ![switch hinzufügen](./media/logic-apps-control-flow-switch-statement/add-switch-statement.png)

   Eine switch-Anweisung wird mit einem Fall und einem Standardfall angezeigt. 
   Eine switch-Anweisung erfordert normalerweise neben dem Standardfall mindestens einen weiteren Fall. 

   ![Leere switch-Standardanweisung](./media/logic-apps-control-flow-switch-statement/empty-switch.png)

1. Klicken Sie in das Feld **On**, so dass die dynamische Inhaltsliste angezeigt wird. Wählen Sie in dieser Aktivierungsliste das Feld **SelectedOption** aus, dessen Ausgabe die auszuführende Aktion bestimmt. 

   ![„SelectedOption“ auswählen](./media/logic-apps-control-flow-switch-statement/select-selected-option.png)

1. Zum Verarbeiten der Fälle, bei denen die genehmigende Person `Approve` oder `Reject` auswählt, fügen Sie einen weiteren Fall zwischen **Fall** und **Standard** hinzu. 

   ![Weiteren Fall hinzufügen](./media/logic-apps-control-flow-switch-statement/switch-plus.png)

1. Fügen Sie den entsprechenden Fällen diese Aktionen hinzu:

   | Fallnr. | **SelectedOption** | Aktion |
   |--------|--------------------|--------|
   | Fall 1 | **Genehmigen** | Fügen Sie die Outlook-Aktion **E-Mail senden** für das Senden von Details zum RSS-Element nur hinzu, wenn die genehmigende Person **Genehmigen** ausgewählt hat. |
   | Fall 2 | **Ablehnen** | Fügen Sie die Outlook-Aktion **E-Mail senden** hinzu, um andere genehmigende Personen zu darüber zu benachrichtigen, dass das RSS-Element abgelehnt wurde. |
   | Standard | Keine | Keine Aktion erforderlich. In diesem Beispiel ist der **Standardfall** leer, da **SelectedOption** nur zwei Optionen aufweist. |
   |||

   ![Fertig gestellte switch-Anweisung](./media/logic-apps-control-flow-switch-statement/finished-switch.png)

1. Speichern Sie Ihre Logik-App. 

   Zum manuellen Testen dieses Beispiels wählen Sie **Ausführen**, bis die Logik-App ein neues RSS-Element findet und eine Genehmigungs-E-Mail sendet. 
   Wählen Sie **Genehmigen** aus, um die Ergebnisse zu beobachten.

## <a name="json-definition"></a>JSON-Definition

Da Sie nun eine Logik-App mit einer switch-Anweisung erstellt haben, sehen wir uns die allgemeine Codedefinition an, die der switch-Anweisung zugrunde liegt.

``` json
"Switch": {
   "type": "Switch",
   "expression": "@body('Send_approval_email')?['SelectedOption']",
   "cases": {
      "Case": {
         "actions": {
           "Send_an_email": {}
         },
         "case" : "Approve"
      },
      "Case_2": {
         "actions": {
           "Send_an_email_2": {}
         },
         "case": "Reject"
      }
   },
   "default": {
      "actions": {}
   },
   "runAfter": {
      "Send_approval_email": [
         "Succeeded"
      ]
   }
}
```

| Bezeichnung | BESCHREIBUNG |
|-------|-------------|
| `"Switch"`         | Name der switch-Anweisung, den Sie zur besseren Lesbarkeit umbenennen können |
| `"type": "Switch"` | Gibt an, dass die Aktion eine switch-Anweisung ist |
| `"expression"`     | Gibt in diesem Beispiel die ausgewählte Option der genehmigenden Person an, für die weiter unten in der Definition für jeden Fall eine Auswertung erfolgt |
| `"cases"` | Definiert eine beliebige Anzahl von Fällen. `"Case_*"` der jeweilige Standardname für jeden Fall, den Sie zur besseren Lesbarkeit ändern können |
| `"case"` | Gibt den Wert des Falls an, der ein konstanter und eindeutiger Wert sein muss, den die switch-Anweisung für den Vergleich verwendet. Wenn keine Fälle dem Ergebnis des switch-Ausdrucks entsprechen, werden die Aktionen im Abschnitt `"default"` ausgeführt. | 
| | | 

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Featurevorschläge übermitteln oder darüber abstimmen möchten, besuchen Sie die [Website für Azure Logic Apps-Benutzerfeedback](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* [Ausführen von Schritten basierend auf einer Bedingung (Bedingungsanweisungen)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Ausführen und Wiederholen von Schritten (Schleifen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Ausführen oder Zusammenführen paralleler Schritte (Verzweigungen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Ausführen von Schritten basierend auf gruppierten Aktionsstatus (Bereiche)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
