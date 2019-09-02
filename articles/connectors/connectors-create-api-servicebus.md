---
title: Senden und Empfangen von Nachrichten mit Azure Service Bus – Azure Logic Apps | Microsoft-Dokumentation
description: Einrichten des Messaging in der Unternehmenscloud mit Azure Service Bus in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 944bac44c1fc6504dfe1a93df5760ccf4ee46fa0
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69982205"
---
# <a name="exchange-messages-in-the-cloud-with-azure-service-bus-and-azure-logic-apps"></a>Austauschen von Nachrichten in der Cloud mit Azure Service Bus und Azure Logic Apps

Mit Azure Logic Apps und dem Azure Service Bus-Connector können Sie automatisierte Aufgaben und Workflows, die Daten wie Verkäufe, Bestellungen, Journale und Warenbestandsbewegungen in Anwendungen für Ihre Organisation übertragen, erstellen. Der Connector überwacht, sendet und verwaltet nicht nur Nachrichten, sondern führt auch Aktionen mit Warteschlangen, Sitzungen, Themen, Abonnements usw. aus, zum Beispiel:

* Überwachen des Eingangs (automatisch abschließen) oder Empfangs (Peek-Lock) von Nachrichten in Warteschlangen, Themen und Abonnements 
* Senden von Nachrichten
* Erstellen und Löschen von Themenabonnements
* Verwalten von Nachrichten in Warteschlangen und Themenabonnements, z.B. abrufen, verzögert abrufen, abschließen, verzögern, verwerfen und als unzustellbar kennzeichnen.
* Erneuern von Sperren für Nachrichten und Sitzungen in Warteschlangen und Themenabonnements
* Schließen von Sitzungen in Warteschlangen und Themen

Sie können Trigger verwenden, die Antworten von Service Bus erhalten und die Ausgabe für andere Aktionen in Ihren Logik-Apps verfügbar machen. Sie können die Ausgaben von Service Bus-Aktionen auch in anderen Aktionen verwenden. Wenn Sie noch nicht mit Service Bus und Logik-Apps vertraut sind, lesen Sie [Was ist Azure Service Bus?](../service-bus-messaging/service-bus-messaging-overview.md) und [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Ein Service Bus-Namespace und eine Messagingentität, z.B. eine Warteschlange. Wenn Sie nicht über diese Elemente verfügen, informieren Sie sich, wie Sie [Ihren Service Bus-Namespace und eine Warteschlange erstellen](../service-bus-messaging/service-bus-create-namespace-portal.md). 

  Diese Elemente müssen im gleichen Azure-Abonnement wie Ihre Logik-Apps, die diese Elemente verwenden, vorhanden sein.

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Die Logik-App, in der Sie Service Bus verwenden möchten. Ihre Logik-App muss demselben Azure-Abonnement wie Ihre Service Bus-Instanz angehören. Wenn Sie mit einem Service Bus-Trigger beginnen möchten, [erstellen Sie eine leere Logik-App](../logic-apps/quickstart-create-first-logic-app-workflow.md). Um eine Service Bus-Aktion zu verwenden, starten Sie Ihre Logik-App mit einem anderen Trigger, z.B. dem **Wiederholungstrigger**.

<a name="permissions-connection-string"></a>

## <a name="check-permissions"></a>Überprüfen der Berechtigungen

Vergewissern Sie sich, dass Ihre Logik-App über Berechtigungen für den Zugriff auf Ihren Service Bus-Namespace verfügt. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

2. Navigieren Sie zu Ihrem Service Bus-*Namespace*. Wählen Sie auf der Seite „Namespace“ unter **Einstellungen** die Option **Freigegebene Zugriffsrichtlinien** aus. Stellen Sie im Bereich **Ansprüche** sicher, dass Sie die **Verwaltungsberechtigungen** für diesen Namespace besitzen.

   ![Verwalten von Berechtigungen für Ihren Service Bus-Namespace](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

3. Rufen Sie die Verbindungszeichenfolge für Ihren Service Bus-Namespace ab. Sie benötigen diese Zeichenfolge, wenn Sie die Verbindungsinformationen in Ihrer Logik-App eingeben.

   1. Wählen Sie **RootManageSharedAccessKey**. 
   
   1. Wählen Sie neben der primären Verbindungszeichenfolge die Schaltfläche „Kopieren“ aus. Speichern Sie die Verbindungszeichenfolge für die spätere Verwendung.

      ![Kopieren der Verbindungszeichenfolge für Service Bus-Namespaces](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Um zu überprüfen, ob Ihre Verbindungszeichenfolge mit Ihrem Service Bus-Namespace oder einer Messagingentität (z.B. einer Warteschlange) verknüpft ist, suchen Sie die Verbindungszeichenfolge für den Parameter `EntityPath` . Wenn Sie diesen Parameter gefunden haben, gilt die Verbindungszeichenfolge für eine bestimmte Entität und stellt nicht die richtige Zeichenfolge für die Verwendung mit Ihrer Logik-App dar.

## <a name="add-trigger-or-action"></a>Hinzufügen von Triggern oder Aktionen

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie Ihre Logik-App im Logik-App-Designer, sofern sie nicht bereits geöffnet ist.

1. Um einer leeren Logik-App einen *Trigger* hinzuzufügen, geben Sie in das Suchfeld „Azure Service Bus“ als Filter ein. Wählen Sie in der Triggerliste den gewünschten Trigger aus. 

   Um z.B. Ihre Logik-App auszulösen, wenn ein neues Element an eine Service Bus-Warteschlange gesendet wird, wählen Sie diesen Trigger aus: **Bei Empfang einer Nachricht in einer Warteschlange (automatisch abschließen)**

   ![Service Bus-Trigger auswählen](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   > [!NOTE]
   > Einige Trigger können eine oder mehrere Nachrichten zurückgeben, z.B. der Trigger **Bei Empfang mindestens einer Nachricht in der Warteschlange (autom. abschließen)** . Wird ein solcher Trigger ausgelöst, gibt er mindestens eine und maximal so viele Nachrichten zurück, wie diese in seiner Eigenschaft **Maximale Nachrichtenanzahl** angegeben ist.

   *Alle Service Bus-Trigger sind Trigger mit langem Abruf*. Das bedeutet, dass ein Trigger beim Auslösen alle Nachrichten verarbeitet und dann 30 Sekunden lang auf weitere Nachrichten wartet, die in der Warteschlange oder im Themenabonnement eingehen. 
   Gehen innerhalb von 30 Sekunden keine Nachrichten ein, wird die Triggerausführung übersprungen. 
   Andernfalls fährt der Trigger mit dem Lesen von Nachrichten fort, bis die Warteschlange oder das Themenabonnement leer ist. Der nächste Triggerabruf basiert auf dem in den Triggereigenschaften angegebenen Wiederholungsintervall.

1. Führen Sie zum Hinzufügen einer *Aktion* zu einer vorhandenen Logik-App die folgenden Schritte aus: 

   1. Wählen Sie im letzten Schritt zum Hinzufügen einer Aktion **Neuer Schritt** aus. 

      Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. 
      Wählen Sie das daraufhin angezeigte Pluszeichen ( **+** ) und dann **Aktion hinzufügen** aus.

   1. Geben Sie im Suchfeld den Begriff „Azure Service Bus“ als Filter ein. 
   Wählen Sie in der Liste mit den Aktionen die gewünschte Aktion aus. 
 
      Wählen Sie beispielsweise diese Aktion aus: **Nachricht senden**

      ![Auswählen einer Service Bus-Aktion](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png) 

1. Wenn Sie Ihre Logik-App zum ersten Mal mit Ihrem Service Bus-Namespace verbinden, fordert der Logik-App-Designer nun die Verbindungsinformationen an. 

   1. Geben Sie einen Namen für Ihre Verbindung ein, und wählen Sie den Service Bus-Namespace aus.

      ![Erstellen einer Service Bus-Verbindung, Teil 1](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-1.png)

      Wenn Sie die Verbindungszeichenfolge stattdessen manuell eingeben möchten, wählen Sie **Verbindungsinformationen manuell eingeben** aus. 
      Wenn Sie die Verbindungszeichenfolge nicht kennen, lesen Sie [Suchen der Verbindungszeichenfolge](#permissions-connection-string).

   1. Wählen Sie jetzt Ihre Service Bus-Richtlinie und dann **Erstellen** aus.

      ![Service Bus-Verbindung herstellen, Teil 2](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-2.png)

1. Wählen Sie in diesem Beispiel die gewünschte Messagingentität aus, z.B. eine Warteschlange oder ein Thema. In diesem Beispiel wählen Sie Ihre Service Bus-Warteschlange aus. 
   
   ![Auswählen der Service Bus-Warteschlange](./media/connectors-create-api-azure-service-bus/service-bus-select-queue.png)

1. Geben Sie die erforderlichen Informationen zu Ihrem Trigger oder Ihrer Aktion ein. Führen Sie in diesem Beispiel die erforderlichen Schritte für Ihren Trigger oder Ihre Aktion aus: 

   * **Für den Beispieltrigger**: Legen Sie das Abrufintervall und die Häufigkeit für die Überprüfung der Warteschlange fest.

     ![Einrichten des Abrufintervalls](./media/connectors-create-api-azure-service-bus/service-bus-trigger-details.png)

     Wenn Sie fertig sind, fahren Sie mit dem Erstellen Ihres Logik-App-Workflows fort, indem Sie die gewünschten Aktionen hinzufügen. Sie können beispielsweise eine Aktion hinzufügen, die eine E-Mail sendet, wenn eine neue Nachricht eingeht.
     Wenn Ihr Trigger die Warteschlange überprüft und eine neue Nachricht findet, führt Ihre Logik-App die von Ihnen ausgewählten Aktionen für die gefundene Nachricht aus.

   * **Für die Beispielaktion**: Geben Sie den Nachrichteninhalt und alle weiteren Details ein. 

     ![Angeben von Nachrichteninhalt und weiteren Informationen](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)

     Wenn Sie fertig sind, fahren Sie mit dem Erstellen Ihres Logik-App-Workflows fort, indem Sie weitere gewünschte Aktionen hinzufügen. Sie können beispielsweise eine Aktion hinzufügen, die eine E-Mail sendet, um das Senden Ihrer Nachricht zu bestätigen.

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern**.

## <a name="connector-reference"></a>Connector-Referenz

Der Service Bus-Connector kann bis zu 1.500 eindeutige Sitzungen gleichzeitig von einem Service Bus in den Connectorcache speichern. Wenn die Anzahl der Sitzungen dieses Limit überschreitet, werden alte Sitzungen aus dem Cache entfernt. Weitere technische Details zu Triggern, Aktionen und Beschränkungen aus der OpenAPI-Beschreibung (ehemals Swagger) des Connectors finden Sie auf der [Referenzseite des Connectors](/connectors/servicebus/).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
