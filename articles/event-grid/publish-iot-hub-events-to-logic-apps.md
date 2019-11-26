---
title: 'Tutorial: Verwenden von IoT Hub-Ereignissen zum Auslösen von Azure Logic Apps'
description: 'Tutorial: Erstellen Sie mit dem Ereignisroutingdienst von Azure Event Grid automatisierte Prozesse zum Durchführen von Aktionen in Azure Logic Apps anhand von IoT Hub-Ereignissen.'
services: iot-hub
documentationcenter: ''
author: kgremban
manager: philmea
editor: ''
ms.service: iot-hub
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2019
ms.author: kgremban
ms.openlocfilehash: e003cb650b0589ab43c984850838c56cbbf1ff2f
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74106779"
---
# <a name="tutorial-send-email-notifications-about-azure-iot-hub-events-using-logic-apps"></a>Tutorial: Senden von E-Mail-Benachrichtigungen zu Azure IoT Hub-Ereignissen mit Logic Apps

Azure Event Grid ermöglicht es Ihnen, auf Ereignisse in IoT Hub zu reagieren, indem Sie Aktionen in Ihren nachgeschalteten Geschäftsanwendungen auslösen.

Dieser Artikel führt Sie durch eine Beispielkonfiguration, die IoT Hub und Event Grid verwendet. Am Ende verfügen Sie über eine Azure-Logik-App, die zum Senden einer E-Mail-Benachrichtigung bei jedem Hinzufügen eines Geräts zu Ihrem IoT Hub eingerichtet ist. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein E-Mail-Konto von einem beliebigen von Azure Logic Apps unterstützten E-Mail-Anbieter, wie Office 365 Outlook, Outlook.com oder Gmail. Mit diesem E-Mail-Konto werden die Ereignisbenachrichtigungen gesendet. Eine vollständige Liste der unterstützten Logic App-Connectors finden Sie in der [Übersicht über Connectors](https://docs.microsoft.com/connectors/).
* Ein aktives Azure-Konto. Wenn Sie noch nicht über eines verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/pricing/free-trial/).
* Eine IoT Hub-Instanz in Azure. Wenn Sie noch keinen erstellt haben, finden Sie unter [Erste Schritte mit IoT Hub](../iot-hub/iot-hub-csharp-csharp-getstarted.md) eine exemplarische Vorgehensweise. 

## <a name="create-a-logic-app"></a>Erstellen einer Logik-App

Erstellen Sie zuerst eine Logik-App, und fügen Sie einen Ereignisrastertrigger hinzu, der die Ressourcengruppe für Ihren virtuellen Computer überwacht. 

### <a name="create-a-logic-app-resource"></a>Erstellen einer Logik-App-Ressource

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Ressource erstellen** > **Integration** > **Logik-App** aus.

   ![Erstellen einer Logik-App](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

2. Geben Sie der Logik-App einen Namen, der in Ihrem Abonnement eindeutig ist, und wählen Sie dann dasselbe Abonnement, dieselbe Ressourcengruppe und denselben Standort wie für Ihren IoT Hub aus. 
3. Klicken Sie auf **Erstellen**.

4. Nachdem die Ressource erstellt wurde, navigieren Sie zu Ihrer Logik-App. 

5. Der Designer für Logik-Apps zeigt Vorlagen für allgemeine Muster an, die Ihnen einen schnelleren Einstieg ermöglichen. Wählen Sie im Logik-App-Designer unter **Vorlagen** die Option **Leere Logik-App** aus, um Ihre Logik-App neu zu erstellen.

### <a name="select-a-trigger"></a>Auswählen eines Triggers

Ein Trigger ist ein bestimmtes Ereignis, durch das Ihre Logik-App gestartet wird. In diesem Tutorial empfängt der Trigger, der den Workflow einleitet, eine Anforderung über HTTP.  

1. Geben Sie auf der Suchleiste für Connectors und Trigger die Zeichenfolge **HTTP** ein.
2. Wählen Sie **Anforderung – Beim Empfang einer HTTP-Anforderung** als Trigger aus. 

   ![Auswählen des HTTP-Anforderungstriggers](./media/publish-iot-hub-events-to-logic-apps/http-request-trigger.png)

3. Wählen Sie **Beispielnutzlast zum Generieren eines Schemas verwenden** aus. 

   ![Auswählen des HTTP-Anforderungstriggers](./media/publish-iot-hub-events-to-logic-apps/sample-payload.png)

4. Fügen Sie den folgenden Beispiel-JSON-Code in das Textfeld ein, und wählen Sie dann **Fertig** aus:

   ```json
   [{
     "id": "56afc886-767b-d359-d59e-0da7877166b2",
     "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
     "subject": "devices/LogicAppTestDevice",
     "eventType": "Microsoft.Devices.DeviceCreated",
     "eventTime": "2018-01-02T19:17:44.4383997Z",
     "data": {
       "twin": {
         "deviceId": "LogicAppTestDevice",
         "etag": "AAAAAAAAAAE=",
         "deviceEtag": "null",
         "status": "enabled",
         "statusUpdateTime": "0001-01-01T00:00:00",
         "connectionState": "Disconnected",
         "lastActivityTime": "0001-01-01T00:00:00",
         "cloudToDeviceMessageCount": 0,
         "authenticationType": "sas",
         "x509Thumbprint": {
           "primaryThumbprint": null,
           "secondaryThumbprint": null
         },
         "version": 2,
         "properties": {
           "desired": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           },
           "reported": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           }
         }
       },
       "hubName": "egtesthub1",
       "deviceId": "LogicAppTestDevice"
     },
     "dataVersion": "1",
     "metadataVersion": "1"
   }]
   ```

5. Möglicherweise erhalten Sie die Popupbenachrichtigung **Denken Sie daran, in Ihre Anforderung einen Content-Type-Header einzuschließen, der auf „application/json“ festgelegt ist**. Sie können diese Empfehlung problemlos ignorieren und mit dem nächsten Abschnitt fortfahren. 

### <a name="create-an-action"></a>Erstellen einer App

Aktionen sind alle Schritte, die auftreten, nachdem der Trigger den Logik-App-Workflow gestartet hat. Im Rahmen dieses Tutorials besteht die Aktion im Senden einer E-Mail-Benachrichtigung von Ihrem E-Mail-Anbieter. 

1. Wählen Sie **Neuer Schritt** aus. Daraufhin wird ein Fenster zum **Auswählen einer Aktion** geöffnet.

2. Suchen Sie nach **E-Mail**.

3. Suchen Sie abhängig von Ihrem E-Mail-Anbieter nach dem entsprechenden Connector, und wählen Sie diesen aus. In diesem Tutorial wird **Office 365 Outlook** verwendet. Die Schritte für andere E-Mail-Anbieter sind ähnlich. 

   ![Auswählen des E-Mail-Anbieter-Connectors](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

4. Wählen Sie die Aktion **E-Mail senden** aus. 

5. Melden Sie sich nach Aufforderung bei Ihrem E-Mail-Konto an. 

6. Erstellen Sie Ihre E-Mail-Vorlage. 
   * **An**: Geben Sie die E-Mail-Adresse an, die die E-Mail-Benachrichtigungen empfangen soll. Verwenden Sie für dieses Tutorial ein E-Mail-Konto, auf das Sie zu Testzwecken zugreifen können. 
   * **Betreff** und **Text**: Schreiben Sie den Text für Ihre E-Mail. Wählen Sie im Auswahltool JSON-Eigenschaften aus, um dynamische Inhalte basierend auf Ereignisdaten einzuschließen.  

   Ihre E-Mail-Vorlage könnte folgendem Beispiel entsprechen:

   ![Ausfüllen der E-Mail-Informationen](./media/publish-iot-hub-events-to-logic-apps/email-content.png)

7. Speichern Sie Ihre Logik-App. 

### <a name="copy-the-http-url"></a>Kopieren der HTTP-URL

Bevor Sie den Logik-App-Designer verlassen, kopieren Sie die URL, auf die Ihre Logik-App auf einen Trigger lauscht. Verwenden Sie diese URL zum Konfigurieren von Event Grid. 

1. Erweitern Sie das Triggerkonfigurationsfeld **Beim Empfang einer HTTP-Anforderung**, indem Sie darauf klicken. 
2. Kopieren Sie den Wert **HTTP POST-URL** durch Auswählen der Kopierschaltfläche daneben. 

   ![Kopieren der HTTP POST-URL](./media/publish-iot-hub-events-to-logic-apps/copy-url.png)

3. Speichern Sie diese URL, damit Sie sie im nächsten Abschnitt verwenden können. 

## <a name="configure-subscription-for-iot-hub-events"></a>Konfigurieren des Abonnements für IoT Hub-Ereignisse

In diesem Abschnitt konfigurieren Sie Ihren IoT Hub zum Veröffentlichen von Ereignissen, sobald diese auftreten. 

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub. 
2. Wählen Sie **Ereignisse** aus.

   ![Öffnen der Details zu Event Grid](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. Klicken Sie auf **Ereignisabonnement**. 

   ![Erstellen eines neuen Ereignisabonnements](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. Erstellen Sie das Ereignisabonnement mit den folgenden Werten: 
   * **Ereignistyp**: Deaktivieren Sie die Option „Alle Ereignistypen abonnieren“, und klicken Sie im Menü auf **Gerät erstellt**.
   * **Endpunktdetails**: Wählen Sie als Endpunkttyp die Option **Webhook** aus, klicken Sie auf „Endpunkt auswählen“, fügen Sie die URL ein, die Sie aus Ihrer Logik-App kopiert haben, und bestätigen Sie die Auswahl.

     ![Auswählen der Endpunkt-URL](./media/publish-iot-hub-events-to-logic-apps/endpoint-url.png)

   * **Details zum Ereignisabonnement**: Geben Sie einen aussagekräftigen Namen ein, und wählen Sie **Event Grid-Schema** aus.

   Nach Abschluss sollte das Formular wie im folgenden Beispiel aussehen: 

    ![Beispielformular für ein Ereignisabonnement](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. Sie können hier das Ereignisabonnement speichern und Benachrichtigungen für alle Geräte empfangen, die in Ihren IoT Hub erstellt werden. Im Rahmen dieses Tutorials möchten wir jedoch die optionalen Felder zum Filtern nach bestimmten Geräten verwenden. Wählen Sie **Zusätzliche Features** am oberen Rand des Formulars aus. 

6. Erstellen Sie die folgenden Filter:

   * **Betreff beginnt mit**: Geben Sie `devices/Building1_` ein, um nach Geräteereignissen in Gebäude 1 zu filtern.
   * **Betreff endet auf**: Geben Sie `_Temperature` an, um nach Geräteereignissen zu filtern, die im Zusammenhang mit der Temperatur stehen.

5. Wählen Sie **Erstellen** aus, um das Ereignisabonnement zu speichern.

## <a name="create-a-new-device"></a>Erstellen eines neuen Geräts

Testen Sie Ihre Logik-App, indem Sie ein neues Gerät erstellen, um eine E-Mail-Benachrichtigung für das Ereignis auszulösen. 

1. Wählen Sie von Ihrem IoT Hub aus **IoT-Geräte** aus. 
2. Wählen Sie **Hinzufügen**.
3. Geben Sie für **Geräte-ID** die Zeichenfolge `Building1_Floor1_Room1_Temperature` ein.
4. Wählen Sie **Speichern** aus. 
5. Sie können mehrere Geräte mit unterschiedlichen Geräte-IDs hinzufügen, um die Ereignisabonnementfilter zu testen. Versuchen Sie es mit diesen Beispielen: 
   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

Nachdem Sie Ihrem IoT Hub einige Geräte hinzugefügt haben, überprüfen Sie Ihre E-Mail, um herauszufinden, welche die Logik-App ausgelöst haben. 

## <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI

Anstelle des Azure-Portals können Sie die IoT Hub-Schritte auch mithilfe der Azure-Befehlszeilenschnittstelle erreichen. Einzelheiten dazu finden Sie in den Azure CLI-Seiten zum [Erstellen eines Ereignisabonnements](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) und [Erstellen eines IoT-Geräts](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Tutorial werden Ressourcen verwendet, für die Gebühren in Ihrem Azure-Abonnement anfallen. Wenn Sie mit dem Ausprobieren des Tutorials und Testen Ihrer Ergebnisse fertig sind, deaktivieren oder löschen Sie die Ressourcen, die nicht beibehalten werden sollen. 

Wenn Sie die Arbeit an Ihrer Logik-App nicht verlieren möchten, deaktivieren Sie sie, anstatt sie zu löschen. 

1. Navigieren Sie zu Ihrer Logik-App.
2. Wählen Sie auf dem Blatt **Übersicht** eine der Optionen **Löschen** oder **Deaktivieren** aus. 

Jedes Abonnement kann über einen kostenlosen IoT Hub verfügen. Wenn Sie für dieses Tutorial einen kostenlosen Hub erstellt haben, müssen Sie ihn nicht löschen, um Gebühren zu vermeiden.

1. Navigieren Sie zu Ihrem IoT Hub. 
2. Wählen Sie auf dem Blatt **Übersicht** die Option **Löschen** aus. 

Auch wenn Sie Ihren IoT Hub behalten, sollten Sie das Ereignisabonnement, das Sie erstellt haben, löschen. 

1. Wählen Sie in Ihren IoT Hub die Option **Event Grid** aus.
2. Wählen Sie das Ereignisabonnement aus, das Sie entfernen möchten. 
3. Klicken Sie auf **Löschen**. 

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Reagieren auf Ereignisse in IoT Hub mithilfe von Event Grid zum Auslösen von Aktionen](../iot-hub/iot-hub-event-grid.md).
* [Erfahren Sie, wie Sie Ereignisse im Zusammenhang mit der Herstellung und Trennung von Geräteverbindungen sortieren.](../iot-hub/iot-hub-how-to-order-connection-state-events.md)
* Erfahren Sie mehr über die Möglichkeiten mit [Event Grid](overview.md).


