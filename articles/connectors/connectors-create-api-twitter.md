---
title: Herstellen einer Verbindung mit Twitter aus Azure Logic Apps | Microsoft-Dokumentation
description: Automatisieren Sie Aufgaben und Workflows, die Tweets überwachen und verwalten, und erhalten Sie Daten über Follower, Ihre verfolgten Benutzer, andere Benutzer, Timelines und mehr aus Ihrem Twitter-Konto, indem Sie Azure Logic Apps verwenden.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 8bce2183-544d-4668-a2dc-9a62c152d9fa
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 0fbd89202796cb4543dbecbeee605c9b87cc9d05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "62104985"
---
# <a name="monitor-and-manage-twitter-by-using-azure-logic-apps"></a>Überwachen und Verwalten von Twitter mithilfe von Azure Logic Apps

Mit Azure Logic Apps und dem Twitter-Connector können Sie automatisierte Aufgaben und Workflows erstellen, die Daten, die Ihnen in Twitter wichtig sind, wie z.B. Tweets, Follower, Benutzer und verfolgte Benutzer, Timelines und mehr, zusammen mit anderen Aktionen, überwachen und verwalten:

* Überwachen, Veröffentlichen und Suchen von Tweets.
* Abrufen von Daten, z.B. Follower, verfolgte Benutzer, Timelines und vieles mehr.

Sie können Trigger verwenden, die Antworten von Ihrem Twitter-Konto erhalten und die Ausgabe für andere Aktionen verfügbar machen. Sie können Aktionen verwenden, die Aufgaben mit Ihrem Twitter-Konto ausführen. Sie können die Ausgaben von Twitter-Aktionen auch in anderen Aktionen verwenden. Beispiel: Wenn ein neuer Tweet mit einem bestimmten Hashtag erscheint, können Sie Nachrichten mit dem Slack-Connector senden. Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich <a href="https://azure.microsoft.com/free/" target="_blank">für ein kostenloses Azure-Konto registrieren</a>. 

* Ihr Twitter-Konto und Benutzeranmeldeinformationen

   Ihre Anmeldeinformationen autorisieren Ihre Logik-App zur Erstellung einer Verbindung mit Ihrem Twitter-Konto sowie zum Zugriff auf das Konto.

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Die Logik-App, in der Sie auf Ihr Twitter-Konto zugreifen möchten. Wenn Sie mit einem Twitter-Trigger beginnen möchten, [erstellen Sie eine leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). Um eine Twitter-Aktion zu verwenden, starten Sie Ihre Logik-App mit einem anderen Trigger, z.B. dem **Wiederholungstrigger**.

## <a name="connect-to-twitter"></a>Verbinden mit Twitter

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie Ihre Logik-App im Logik-App-Designer, sofern sie nicht bereits geöffnet ist.

1. Auswählen eines Pfads: 

   * Geben Sie für leere Logik-Apps im Suchfeld die Zeichenfolge „twitter“ als Filter ein. 
   Wählen Sie in der Triggerliste den gewünschten Trigger aus. 

     Oder

   * Für vorhandene Logik-Apps: 
   
     * Wählen Sie im letzten Schritt zum Hinzufügen einer Aktion **Neuer Schritt** aus. 

       Oder

     * Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. 
     Wählen Sie das daraufhin angezeigte Pluszeichen ( **+** ) und dann **Aktion hinzufügen** aus.
     
       Geben Sie im Suchfeld den Begriff „twitter“ als Filter ein. 
       Wählen Sie in der Liste mit den Aktionen die gewünschte Aktion aus.

1. Wenn Sie zur Anmeldung bei Twitter aufgefordert werden, melden Sie sich an, um den Zugriff für Ihre Logik-App zu autorisieren.

1. Geben Sie die erforderlichen Details für Ihren ausgewählten Trigger oder Ihre ausgewählte Aktion an, und fahren Sie mit dem Erstellen Ihres Logik-App-Workflows fort.

## <a name="examples"></a>Beispiele

### <a name="twitter-trigger-when-a-new-tweet-is-posted"></a>Twitter-Trigger: Wenn ein neuer Tweet gepostet wird

Dieser Trigger startet den Workflow der Logik-App, wenn der Trigger einen neuen Tweet, z.B. mit dem Hashtag #Seattle, erkennt. Wenn diese Tweets dann gefunden werden, können Sie eine Datei mit dem Inhalt der Tweets zum Speicher hinzufügen, z.B. ein Dropbox-Konto, indem Sie den Dropbox-Connector verwenden. 

Optional können Sie eine Bedingung einfügen, dass berechtigte Tweets von Benutzern mit mindestens einer bestimmten Anzahl von Followern stammen müssen.

**Beispiel für Unternehmen**: Mit diesem Trigger können Sie Tweets über Ihr Unternehmen überwachen und den Inhalt der Tweets in eine SQL-Datenbank hochladen.

### <a name="twitter-action-post-a-tweet"></a>Twitter-Aktion: Tweet posten

Diese Aktion veröffentlicht einen Tweet, aber Sie können die Aktion so einrichten, dass der Tweet den Inhalt von Tweets enthält, die vom zuvor beschriebenen Trigger gefunden wurden. 

## <a name="connector-reference"></a>Connector-Referenz

Technische Details zu Triggern, Aktionen und Beschränkungen aus der OpenAPI-Beschreibung (ehemals Swagger) des Connectors finden Sie auf der [Referenzseite](/connectors/twitterconnector/) des Connectors.

## <a name="get-support"></a>Support

* Sollten Sie Fragen haben, besuchen Sie das [Azure Logic Apps-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
