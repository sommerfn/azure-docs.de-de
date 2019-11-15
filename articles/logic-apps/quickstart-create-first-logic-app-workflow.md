---
title: Erstellen Ihres ersten automatisierten Workflows – Azure Logic Apps
description: 'Schnellstart: Erstellen Ihres ersten automatisierten Workflows mithilfe von Azure Logic Apps für Systemintegrationslösungen und EAI-Lösungen (Enterprise Application Integration)'
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: quickstart
ms.custom: mvc
ms.date: 10/20/2019
ms.openlocfilehash: b6222747e352f446dab33314729f7f697e328909
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73824901"
---
# <a name="quickstart-create-your-first-workflow-by-using-azure-logic-apps---azure-portal"></a>Schnellstart: Erstellen Ihres ersten Workflows mithilfe von Azure Logic Apps – Azure-Portal

In dieser Schnellstartanleitung werden die grundlegenden allgemeinen Konzepte vorgestellt, auf denen das Erstellen Ihres ersten Workflows mithilfe von [Azure Logic Apps](../logic-apps/logic-apps-overview.md) beruht. Dazu gehören z.B. das Erstellen einer leeren Logik-App, das Hinzufügen eines Triggers und einer Aktion sowie das anschließende Testen Ihrer Logik-App. In diesem Schnellstart erstellen Sie eine Logik-App, die den RSS-Feed einer Website regelmäßig auf neue Elemente überprüft. Wenn neue Elemente vorhanden sind, sendet die Logik-App eine E-Mail für jedes Element. Am Ende entspricht Ihre Logik-App grob dem folgenden Workflow:

![Allgemeiner Logik-App-Beispielworkflow](./media/quickstart-create-first-logic-app-workflow/quickstart-workflow-overview.png)

Für dieses Szenario benötigen Sie ein E-Mail-Konto von einem von Azure Logic Apps unterstützten Dienst wie Office 365 Outlook, Outlook.com oder Gmail. Weitere unterstützte E-Mail-Dienste finden Sie im [Artikel zu den Connectors](https://docs.microsoft.com/connectors/). In diesem Beispiel wird Office 365 Outlook für die Logik-App verwendet. Wenn Sie einen anderen E-Mail-Dienst verwenden, sind die allgemeinen Schritte identisch, doch kann die Benutzeroberfläche geringfügig abweichen.

Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich darüber hinaus [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-your-logic-app"></a>Erstellen Ihrer Logik-App

1. Suchen Sie auf der Azure-Startseite im Suchfeld nach **Logic Apps**, und wählen Sie den entsprechenden Eintrag aus.

   ![Suchen und Auswählen von „Logic Apps“](./media/quickstart-create-first-logic-app-workflow/find-select-logic-apps.png)

1. Wählen Sie auf der Seite **Logic Apps** die Option **Hinzufügen** aus.

   ![Hinzufügen einer neuen Logik-App](./media/quickstart-create-first-logic-app-workflow/add-new-logic-app.png)

1. Geben Sie im Bereich **Logik-App** Details zu Ihrer Logik-App wie nachfolgend gezeigt ein. Wählen Sie **Erstellen** aus, wenn Sie fertig sind.

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

Fügen Sie als Nächstes einen [Trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, der bei einem neuen RSS-Feedelement ausgelöst wird. Jede Logik-App muss mit einem Trigger beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt die Azure Logic Apps-Engine eine Logik-App-Instanz, mit der Ihr Workflow gestartet und ausgeführt wird.

<a name="add-rss-trigger"></a>

## <a name="add-the-rss-trigger"></a>Hinzufügen des RSS-Triggers

1. Wählen Sie im **Logik-App-Designer** unterhalb des Suchfelds die Option **Alle** aus.

1. Geben Sie im Suchfeld `rss` ein, um den RSS-Connector zu suchen. Wählen Sie in der Triggerliste den Eintrag **Beim Veröffentlichen eines Feedelements** aus.

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

## <a name="add-the-send-email-action"></a>Hinzufügen der Aktion „E-Mail senden“

Fügen Sie nun eine [Aktion](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, die eine E-Mail sendet, wenn im RSS-Feed ein neues Element erscheint.

1. Wählen Sie unterhalb des Triggers **Beim Veröffentlichen eines Feedelements** die Option **+ Neuer Schritt** aus.

   ![Auswählen von „Neuer Schritt“ unter „Trigger“](./media/quickstart-create-first-logic-app-workflow/add-new-step-under-trigger.png)

1. Wählen Sie unterhalb von **Aktion auswählen** und unterhalb des Suchfelds die Option **Alle** aus.

1. Geben Sie im Suchfeld `send an email` ein, um Connectors zu suchen, die diese Aktion anbieten. Wählen Sie in der Liste der Aktionen die Aktion „E-Mail senden“ für den gewünschten E-Mail-Dienst aus. In diesem Beispiel wird der Office 365 Outlook-Connector verwendet, der über die Aktion **E-Mail senden** verfügt.

   ![Auswählen der Aktion „E-Mail senden“ für Office 365 Outlook](./media/quickstart-create-first-logic-app-workflow/add-action-send-email.png)

   Um die Liste der Aktionen nach einer bestimmten App oder einem Dienst zu filtern, können Sie zuerst die App oder den Dienst auswählen:

   * Wählen Sie für Geschäfts-, Schul- oder Unikonten für Azure die Option „Office 365 Outlook“.
   * Wählen Sie für persönliche Microsoft-Konten die Option „Outlook.com“.

1. Wenn der ausgewählte E-Mail-Connector Sie auffordert, Ihre Identität zu authentifizieren, führen Sie diesen Schritt jetzt aus, um eine Verbindung zwischen Ihrer Logik-App und Ihrem E-Mail-Dienst herzustellen.

   > [!NOTE]
   > In diesem speziellen Beispiel authentifizieren Sie Ihre Identität manuell. Connectors, die eine Authentifizierung erfordern, unterscheiden sich jedoch in Hinsicht auf die jeweils unterstützten Authentifizierungstypen. Außerdem stehen Optionen bereit, mit denen Sie die Handhabung der Authentifizierung einrichten können. Wenn Sie z.B. Azure Resource Manager-Vorlagen für die Bereitstellung verwenden, können Sie Eingaben, die Sie häufig oder einfach ändern möchten (z.B. Verbindungsinformationen), parametrisieren und sichern. Weitere Informationen finden Sie in den folgenden Themen:
   >
   > * [Vorlagenparameter für die Bereitstellung](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#template-parameters)
   > * [Autorisieren von OAuth-Verbindungen](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md#authorize-oauth-connections)
   > * [Authentifizieren des Zugriffs mit verwalteten Identitäten](../logic-apps/create-managed-service-identity.md)
   > * [Authentifizieren von Verbindungen für die Logik-App-Bereitstellung](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md#authenticate-connections)

1. Geben Sie in der Aktion **E-Mail senden** die Daten an, die die E-Mail enthalten soll.

   1. Geben Sie im Feld **An** die E-Mail-Adresse des Empfängers ein. Zu Testzwecken können Sie hier Ihre E-Mail-Adresse angeben.

      Ignorieren Sie vorerst die angezeigte Liste **Dynamischen Inhalt hinzufügen**. Wenn Sie in einige Bearbeitungsfelder klicken, wird die Liste mit verfügbaren Parametern aus dem vorherigen Schritt angezeigt, die Sie als Eingaben in Ihren Workflow aufnehmen können.

   1. Geben Sie im Feld **Betreff** den folgenden Text mit einem nachstehenden Leerzeichen ein: `New RSS item: `.

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

Aus technischer Sicht passiert Folgendes: Wenn der Trigger den RSS-Feed prüft und neue Elemente findet, wird der Trigger ausgelöst, und die Azure Logic Apps-Engine erstellt eine Instanz Ihres Logik-App-Workflows, mit der Aktionen im Workflow ausgeführt werden. Falls der Trigger keine neuen Elemente findet, wird er nicht ausgelöst und überspringt das Instanziieren des Workflows.

Herzlichen Glückwunsch! Sie haben Ihre erste Logik-App über das Azure-Portal erstellt und ausgeführt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie dieses Beispiel nicht mehr benötigen, löschen Sie die Ressourcengruppe mit Ihrer Logik-App und den dazugehörigen Ressourcen.

1. Wählen Sie im Azure-Hauptmenü **Ressourcengruppen** und dann die Ressourcengruppe Ihrer Logik-App aus. Wählen Sie im Bereich **Übersicht** die Option **Ressourcengruppe löschen** aus.

   ![Suchen, Auswählen und Löschen einer Ressourcengruppe](./media/quickstart-create-first-logic-app-workflow/delete-resource-group.png)

1. Wenn der Bestätigungsbereich angezeigt wird, geben Sie den Ressourcengruppennamen ein, und wählen Sie **Löschen** aus.

   ![Auswählen von „Löschen“ zum Bestätigen des Löschvorgangs](./media/quickstart-create-first-logic-app-workflow/delete-resource-group-2.png)

> [!NOTE]
> Wenn Sie eine Logik-App löschen, werden keine neuen Ausführungen instanziiert. Alle in Bearbeitung befindlichen und ausstehenden Ausführungen werden abgebrochen. Bei Tausenden von Ausführungen kann der Abbruch möglicherweise erhebliche Zeit in Anspruch nehmen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Ihre erste Logik-App erstellt, die gemäß einem festgelegten Zeitplan (minütlich) nach RSS-Updates sucht und eine Aktion ausführt (E-Mail senden), wenn ein Update vorhanden ist. Weitere Informationen finden Sie im folgenden Tutorial, in dem komplexere zeitplanbasierte Workflows erstellt werden:

> [!div class="nextstepaction"]
> [Überprüfen der Verkehrslage mit einer planerbasierten Logik-App](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
