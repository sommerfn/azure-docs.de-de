---
title: Hinzufügen bedingter Anweisungen zu Workflows – Azure Logic Apps | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Bedingungen erstellen, die Aktionen in Workflows in Azure Logic Apps steuern.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/09/2018
ms.openlocfilehash: 9ee484971e217b0ca4dd7ad855e9e6dc3313e5d4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60684757"
---
# <a name="create-conditional-statements-that-control-workflow-actions-in-azure-logic-apps"></a>Erstellen bedingter Anweisungen zum Steuern von Workflowaktionen in Azure Logic Apps

Fügen Sie eine *bedingte Anweisung* hinzu, falls bestimmte Aktionen in Ihrer Logik-App erst nach dem Übergeben einer bestimmten Bedingung ausgeführt werden sollen. Diese Kontrollstruktur vergleicht die Daten in Ihrem Workflow mit bestimmten Werten oder Feldern. Sie können verschiedene Aktionen angeben, die abhängig davon ausgeführt werden, ob die Daten die Bedingung erfüllen. Sie können Bedingungen ineinander schachteln.

Angenommen Sie verfügen über eine Logik-App, die zu viele E-Mails sendet, wenn neue Elemente auf dem RSS-Feed einer Website verfügbar gemacht werden. Sie können Bedingungsanweisung hinzufügen, um nur dann eine E-Mail zu senden, wenn das neue Element eine bestimmte Zeichenfolge enthält. 

> [!TIP]
> Zum Ausführen verschiedener Schritte basierend auf anderen bestimmten Werten verwenden Sie stattdessen eine [*switch-Anweisung*](../logic-apps/logic-apps-control-flow-switch-statement.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Um dem Beispiel in diesem Artikel zu folgen, [erstellen Sie diese Beispiel-Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md) mit einem Outlook.com- oder Office 365 Outlook-Konto.

## <a name="add-condition"></a>Bedingung hinzufügen

1. Öffnen Sie Ihre Logik-App im <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> im Logik-App-Designer.

1. Fügen Sie an der gewünschten Position eine Bedingung hinzu. 

   Um eine Bedingung zwischen Schritten hinzuzufügen, bewegen Sie den Zeiger auf den Pfeil an der Position, an der Sie die Bedingung hinzufügen möchten. Wählen Sie das daraufhin angezeigte **Pluszeichen** ( **+** ) aus, und wählen Sie dann **Aktion hinzufügen** aus. Beispiel:

   ![Hinzufügen einer Aktion zwischen Schritten](./media/logic-apps-control-flow-conditional-statement/add-action.png)

   Wenn Sie eine Bedingung am Ende Ihres Workflows hinzufügen möchten, wählen Sie in Ihrer Logik-App unten die Option **+ Neuer Schritt** > **Aktion hinzufügen** aus.

1. Geben Sie im Suchfeld den Begriff „Bedingung“ als Filter ein. Wählen Sie diese Aktion aus: **Bedingung – steuern**

   ![Bedingung hinzufügen](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

1. Erstellen Sie im Feld **Bedingung** Ihre Bedingung. 

   1. Geben Sie im linken Feld die Daten oder das Feld an, die verglichen werden sollen.

      Wenn Sie in das linke Feld klicken, wird die Liste mit dynamischen Inhalten angezeigt, sodass Sie Ausgaben aus vorherigen Schritten in der Logik-App auswählen können. 
      Wählen Sie für dieses Beispiel die RSS-Feedzusammenfassung aus.

      ![Erstellen der Bedingung](./media/logic-apps-control-flow-conditional-statement/edit-condition.png)

   1. Wählen Sie im mittleren Feld den auszuführenden Vorgang aus. 
   Wählen Sie für dieses Beispiel die Option **enthält** aus. 

   1. Geben Sie im rechten Feld einen Wert oder ein Feld als Suchkriterium an. 
   Geben Sie für dieses Beispiel diese Zeichenfolge an: **Microsoft**

   Hier ist die vollständige Bedingung:

   ![Vollständige Bedingung](./media/logic-apps-control-flow-conditional-statement/edit-condition-2.png)

   Um Ihrer Bedingung eine weitere Zeile hinzuzufügen, wählen Sie **Hinzufügen** > **Zeile hinzufügen** aus. 
   Um eine Gruppe mit Unterbedingungen hinzuzufügen, wählen Sie **Hinzufügen** > **Gruppe hinzufügen** aus. 
   Um vorhandene Zeilen zu gruppieren, aktivieren Sie die Kontrollkästchen für die betreffenden Zeilen, wählen Sie für eine beliebige Zeile die Schaltfläche mit den Auslassungspunkten (...) aus, und wählen Sie dann **Gruppe erstellen** aus.

1. Fügen Sie unter **Bei TRUE** und **Bei FALSE** die auszuführenden Schritte basierend darauf hinzu, ob die Bedingung erfüllt ist. Beispiel:

   ![Bedingung mit „Bei TRUE“- und „Bei FALSE“-Pfaden](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Sie können vorhandene Aktionen in die **Bei TRUE**- und **Bei FALSE**-Pfade ziehen.

1. Speichern Sie Ihre Logik-App.

Diese Logik-App sendet jetzt nur eine E-Mail, wenn die neuen Elemente im RSS-Feed Ihre Bedingung erfüllen.

## <a name="json-definition"></a>JSON-Definition

So sieht die allgemeine Codedefinition hinter einer bedingten Anweisung aus:

``` json
"actions": {
  "Condition": {
    "type": "If",
    "actions": {
      "Send_an_email": {
        "inputs": {},
        "runAfter": {}
    },
    "expression": {
      "and": [ 
        { 
          "contains": [ 
            "@triggerBody()?['summary']", 
            "Microsoft"
          ]
        } 
      ]
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Featurevorschläge übermitteln oder darüber abstimmen möchten, besuchen Sie die [Website für Azure Logic Apps-Benutzerfeedback](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* [Ausführen von Schritten basierend auf verschiedenen Werten (switch-Anweisungen)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Ausführen und Wiederholen von Schritten (Schleifen)](../logic-apps/logic-apps-control-flow-loops.md)
* [Ausführen oder Zusammenführen paralleler Schritte (Verzweigungen)](../logic-apps/logic-apps-control-flow-branches.md)
* [Ausführen von Schritten basierend auf gruppierten Aktionsstatus (Bereiche)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)
