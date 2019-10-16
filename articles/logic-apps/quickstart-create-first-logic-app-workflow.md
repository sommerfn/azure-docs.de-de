---
title: 'Schnellstart: Erstellen und Automatisieren Ihres ersten Workflows mit Azure Logic Apps'
description: Erstellen Sie Ihre erste Logik-App für die Automatisierung von Aufgaben, Prozessen und Workflows mit Azure Logic Apps. Erstellen Sie Logik-Apps für Lösungen mit Systemintegration und Enterprise Application Integration (EAI) für Ihre Systeme und Clouddienste.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: quickstart
ms.custom: mvc
ms.date: 08/20/2019
ms.openlocfilehash: c85b3e3ced661eb36d9cb1eb0ae443a5ab21a913
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029436"
---
# <a name="quickstart-create-your-first-automated-workflow-with-azure-logic-apps---azure-portal"></a>Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal

Diese Schnellstartanleitung enthält eine Einführung in die Erstellung Ihres ersten automatisierten Workflows mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md). In diesem Artikel erstellen Sie eine Logik-App, die den RSS-Feed einer Website regelmäßig auf neue Elemente überprüft. Wenn neue Elemente vorhanden sind, sendet die Logik-App eine E-Mail für jedes Element. Am Ende entspricht Ihre Logik-App grob dem folgenden Workflow:

![Allgemeiner Logik-App-Beispielworkflow](./media/quickstart-create-first-logic-app-workflow/quickstart-workflow-overview.png)

Für diese Schnellstartanleitung benötigen Sie ein E-Mail-Konto bei einem von Logic Apps unterstützten E-Mail-Anbieter wie Office 365 Outlook, Outlook.com oder Gmail. Informationen zu Connectors für andere Anbieter finden Sie in [dieser Liste](https://docs.microsoft.com/connectors/). In dieser Logik-App verwenden wir ein Office 365 Outlook-Konto. Bei Verwendung eines anderen E-Mail-Kontos sind die Schritte im Großen und Ganzen identisch, aber die Benutzeroberfläche weicht ggf. etwas ab.

Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich darüber hinaus [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-your-logic-app"></a>Erstellen Ihrer Logik-App

1. Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Integration** > **Logik-App**.

   ![Erstellen von Logik-Apps: Azure-Portal](./media/quickstart-create-first-logic-app-workflow/create-new-logic-app.png)

1. Geben Sie unter **Logik-App erstellen** Details zur Logik-App wie hier gezeigt ein. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

   ![Angeben von Details für eine neue Logik-App](./media/quickstart-create-first-logic-app-workflow/create-logic-app-settings.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Name** | <*logic-app-name*> | Der Name Ihrer Logik-App, der nur Buchstaben, Ziffern, Bindestriche (`-`), Unterstriche (`_`), Klammern (`(`, `)`) und Punkte (`.`) enthalten darf. In diesem Beispiel wird „My-First-Logic-App“ verwendet. |
   | **Abonnement** | <*Name des Azure-Abonnements*> | Der Name Ihres Azure-Abonnements |
   | **Ressourcengruppe** | <*Name der Azure-Ressourcengruppe*> | Der Name der [Azure-Ressourcengruppe](../azure-resource-manager/resource-group-overview.md), die zum Organisieren verwandter Ressourcen verwendet wird. In diesem Beispiel wird „My-First-LA-RG“ verwendet. |
   | **Location** | <*Azure-Region*> | Die Region, in der die Informationen zu Ihrer Logik-App gespeichert werden sollen. In diesem Beispiel wird „USA, Westen“ verwendet. |
   | **Log Analytics** | Aus | Behalten Sie die Einstellung **Aus** für die Diagnoseprotokollierung bei. |
   ||||

1. Wählen Sie nach der Bereitstellung Ihrer App durch Azure für Ihre bereitgestellte Logik-App auf der Azure-Symbolleiste **Benachrichtigungen** > **Zu Ressource wechseln** aus.

   ![Navigieren zur neu erstellten Logik-App-Ressource](./media/quickstart-create-first-logic-app-workflow/go-to-new-logic-app-resource.png)

   Sie können zum Suchen und Auswählen Ihrer Logik-App auch den Namen in das Suchfeld eingeben.

   Der Logik-App-Designer wird geöffnet, und es wird eine Seite mit einem Einführungsvideo und häufig verwendeten Triggern angezeigt. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App**.

   ![Auswählen der leeren Vorlage für die Logik-App](./media/quickstart-create-first-logic-app-workflow/choose-logic-app-template.png)

Fügen Sie als Nächstes einen [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, der bei einem neuen RSS-Feedelement ausgelöst wird. Jede Logik-App muss mit einem Trigger beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt die Logic Apps-Engine eine Logik-App-Instanz, mit der Ihr Workflow gestartet und ausgeführt wird.

<a name="add-rss-trigger"></a>

## <a name="check-rss-feed-with-a-trigger"></a>Überprüfen des RSS-Feeds mit einem Trigger

1. Wählen Sie im Logik-App-Designer unterhalb des Suchfelds **Alle** aus.

1. Geben Sie im Suchfeld „rss“ ein. Wählen Sie in der Triggerliste den folgenden Trigger aus: **Beim Veröffentlichen eines Feedelements: RSS**

   ![Auswählen des Triggers „Beim Veröffentlichen eines Feedelements“](./media/quickstart-create-first-logic-app-workflow/add-rss-trigger-new-feed-item.png)

1. Geben Sie wie hier gezeigt und beschrieben die folgenden Informationen für Ihren Trigger ein:

   ![Einrichten eines Triggers mit RSS-Feed, Häufigkeit und Intervall](./media/quickstart-create-first-logic-app-workflow/add-rss-trigger-settings.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Die URL des RSS-Feeds** | `http://feeds.reuters.com/reuters/topNews` | Der Link für den RSS-Feed, den Sie überwachen möchten |
   | **Intervall** | 1 | Die Anzahl von Warteintervallen zwischen Überprüfungen |
   | **Frequency** | Minute | Die Zeiteinheit für die Intervalle zwischen Überprüfungen.  |
   ||||

   Das Intervall und die Häufigkeit definieren zusammen den Zeitplan für den Trigger Ihrer Logik-App. Diese Logik-App überprüft den Feed minütlich.

1. Klicken Sie auf die Titelleiste des Triggers, um die Triggerdetails vorerst auszublenden.

   ![Ausblenden der Details durch Reduzieren des Logik-App-Bereichs](./media/quickstart-create-first-logic-app-workflow/collapse-trigger-shape.png)

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus.

Ihre Logik-App befindet sich jetzt im Livemodus, aber es wird vorerst nur der RSS-Feed überprüft. Fügen Sie daher eine Aktion hinzu, die reagiert, wenn der Trigger ausgelöst wird.

## <a name="send-email-with-an-action"></a>Senden einer E-Mail mit einer Aktion

Fügen Sie nun eine [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, die eine E-Mail sendet, wenn im RSS-Feed ein neues Element erscheint.

1. Wählen Sie unterhalb des Triggers **Beim Veröffentlichen eines Feedelements** die Option **+ Neuer Schritt** aus.

   ![Auswählen von „Neuer Schritt“ unter „Trigger“](./media/quickstart-create-first-logic-app-workflow/add-new-step-under-trigger.png)

1. Wählen Sie unterhalb von **Aktion auswählen** und unterhalb des Suchfelds die Option **Alle** aus.

1. Geben Sie im Suchfeld „E-Mail senden“ ein. Wählen Sie aus der Liste der Aktionen die Aktion "E-Mail senden" für den gewünschten E-Mail-Anbieter aus.

   ![Auswählen der Aktion „E-Mail senden“ für Office 365 Outlook](./media/quickstart-create-first-logic-app-workflow/add-action-send-email.png)

   Um die Liste der Aktionen nach einer bestimmten App oder einem Dienst zu filtern, können Sie zuerst die App oder den Dienst auswählen:

   * Wählen Sie für Geschäfts-, Schul- oder Unikonten für Azure die Option „Office 365 Outlook“.
   * Wählen Sie für persönliche Microsoft-Konten die Option „Outlook.com“.

1. Falls Sie zur Eingabe von Anmeldeinformationen aufgefordert werden, melden Sie sich bei Ihrem E-Mail-Konto an, damit Logic Apps eine Verbindung mit Ihrem E-Mail-Konto herstellt.

1. Geben Sie in der Aktion **E-Mail senden** die Daten an, die die E-Mail enthalten soll.

   1. Geben Sie im Feld **An** die E-Mail-Adresse des Empfängers ein. Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben.

      Ignorieren Sie vorerst die angezeigte Liste **Dynamischen Inhalt hinzufügen**. Wenn Sie in einige Bearbeitungsfelder klicken, wird die Liste mit verfügbaren Parametern aus dem vorherigen Schritt angezeigt, die Sie als Eingaben in Ihren Workflow aufnehmen können.

   1. Geben Sie im Feld **Betreff** den folgenden Text mit einem nachstehenden Leerzeichen ein: ```New RSS item:```.

      ![Eingeben des E-Mail-Betreffs in der Betreffeigenschaft](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-subject.png)

   1. Wählen Sie in der Liste **Dynamischen Inhalt hinzufügen** die Option **Feedtitel** aus, um den Titel des RSS-Elements aufzunehmen.

      ![Auswählen der Eigenschaft „Feedtitel“ aus der Liste mit dynamischen Inhalten](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-subject-dynamic-content.png)

      Wenn der Vorgang abgeschlossen ist, sieht der Betreff der E-Mail wie in diesem Beispiel aus:

      ![Fertiges Beispiel für E-Mail-Betreff für hinzugefügten Feedtitel](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-feed-title.png)

      Wenn im Designer eine For each-Schleife angezeigt wird, haben Sie ein Token für ein Array ausgewählt, etwa das Token **categories-Item**. Für diese Art von Token fügt der Designer automatisch diese Schleife für die Aktion hinzu, die auf das Token verweist. Auf diese Weise führt Ihre Logik-App die gleiche Aktion für jedes Arrayelement durch. Wählen Sie zum Entfernen der Schleife auf der Titelleiste der Schleife die **Auslassungspunkte** ( **...** ) und dann **Löschen** aus.

   1. Geben Sie im Feld **Text** den folgenden Text ein, und wählen Sie die folgenden Token für den E-Mail-Text aus. Drücken Sie UMSCHALT+EINGABETASTE, um in einem Bearbeitungsfeld leere Zeilen hinzuzufügen.

      ![Auswählen von Eigenschaften für E-Mail-Textinhalt](./media/quickstart-create-first-logic-app-workflow/add-action-send-email-body.png)

      | Eigenschaft | BESCHREIBUNG |
      |----------|-------------|
      | **Feedtitel** | Der Titel des Elements |
      | **Feed veröffentlicht am** | Datum und Uhrzeit der Elementveröffentlichung |
      | **Link zum primären Feed** | Die URL für das Element |
      |||

1. Speichern Sie Ihre Logik-App.

Testen Sie als Nächstes Ihre Logik-App.

## <a name="run-your-logic-app"></a>Ausführen Ihrer Logik-App

Wählen Sie auf der Symbolleiste des Designers die Option **Ausführen** aus, um Ihre Logik-App manuell zu starten. Oder warten Sie, bis Ihre Logik-App den RSS-Feed gemäß dem festgelegten Zeitplan (minütlich) überprüft. Falls der RSS-Feed über neue Elemente verfügt, sendet Ihre Logik-App für jedes neue Element eine E-Mail. Andernfalls wartet Ihre Logik mit einer erneuten Prüfung bis zum nächsten Intervall. Überprüfen Sie Ihren Ordner mit den Junk-E-Mails, falls Sie keine E-Mails erhalten.

Im Anschluss sehen Sie eine Beispiel-E-Mail, die von dieser Logik-App gesendet wird.

![Beispiel-E-Mail bei Anzeige eines neuen RSS-Feedelements gesendet](./media/quickstart-create-first-logic-app-workflow/monitor-rss-feed-email.png)

Aus technischer Sicht passiert Folgendes: Wenn der Trigger den RSS-Feed prüft und neue Elemente findet, wird der Trigger ausgelöst, und die Logic Apps-Engine erstellt eine Instanz Ihres Logik-App-Workflows, mit der Aktionen im Workflow ausgeführt werden. Falls der Trigger keine neuen Elemente findet, wird er nicht ausgelöst und überspringt das Instanziieren des Workflows.

Herzlichen Glückwunsch! Sie haben Ihre erste Logik-App über das Azure-Portal erstellt und ausgeführt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie dieses Beispiel nicht mehr benötigen, löschen Sie die Ressourcengruppe mit Ihrer Logik-App und den dazugehörigen Ressourcen.

1. Wählen Sie im Azure-Hauptmenü **Ressourcengruppen** und dann die Ressourcengruppe Ihrer Logik-App aus. Wählen Sie im Bereich **Übersicht** die Option **Ressourcengruppe löschen** aus.

   ![Suchen, Auswählen und Löschen einer Ressourcengruppe](./media/quickstart-create-first-logic-app-workflow/delete-resource-group.png)

1. Geben Sie zur Bestätigung den Ressourcengruppennamen ein, und wählen Sie **Löschen** aus.

   ![Auswählen von „Löschen“ zum Bestätigen des Löschvorgangs](./media/quickstart-create-first-logic-app-workflow/delete-resource-group-2.png)

> [!NOTE]
> Wenn Sie eine Logik-App löschen, werden keine neuen Ausführungen instanziiert. Alle in Bearbeitung befindlichen und ausstehenden Ausführungen werden abgebrochen. Bei Tausenden von Ausführungen kann der Abbruch möglicherweise erhebliche Zeit in Anspruch nehmen.

## <a name="get-support"></a>Support

Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/home?forum=azurelogicapps).

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Ihre erste Logik-App erstellt, die gemäß einem festgelegten Zeitplan (minütlich) nach RSS-Updates sucht und eine Aktion ausführt (E-Mail senden), wenn ein Update vorhanden ist. Weitere Informationen finden Sie im folgenden Tutorial, in dem komplexere zeitplanbasierte Workflows erstellt werden:

> [!div class="nextstepaction"]
> [Überprüfen der Verkehrslage mit einer planerbasierten Logik-App](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
