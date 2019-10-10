---
title: 'Tutorial: Erfassen von Ereignissen in einem Azure Digital Twins-Gebäudebereich | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie Benachrichtigungen von Ihren Gebäudebereichen empfangen, indem Sie Azure Digital Twins in Logic Apps integrieren.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 09/23/2019
ms.openlocfilehash: 26976956722d77e2dfb8c17734c207b2667c0126
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949179"
---
# <a name="tutorial-receive-notifications-from-your-azure-digital-twins-spaces-by-using-logic-apps"></a>Tutorial: Empfangen von Benachrichtigungen von den Azure Digital Twins-Gebäudebereichen mit Logic Apps

Nachdem Sie Ihre Azure Digital Twins-Instanz sowie Ihre Gebäudebereiche bereitgestellt und benutzerdefinierte Funktionen zum Überwachen bestimmter Bedingungen implementiert haben, können Sie Ihren Büroadministrator per E-Mail benachrichtigen, wenn die überwachten Bedingungen auftreten.

Im [ersten Tutorial](tutorial-facilities-setup.md) haben Sie den Raumgraphen eines imaginären Gebäudes konfiguriert. Ein Raum im Gebäude enthält Bewegungs-, Kohlendioxid- und Temperatursensoren. Im [zweiten Tutorial](tutorial-facilities-udf.md) haben Sie den Graphen und eine benutzerdefinierte Funktion bereitgestellt, um diese Sensorwerte zu überwachen und Benachrichtigungen auszulösen, wenn der Raum leer ist und die Temperatur- und Kohlendioxidwerte im zulässigen Bereich liegen. 

In diesem Tutorial erfahren Sie, wie Sie diese Benachrichtigungen in Azure Logic Apps integrieren, um E-Mails zu senden, wenn ein solcher Raum verfügbar ist. Mithilfe dieser Informationen kann ein Büroadministrator die Mitarbeiter beim Buchen des am besten geeigneten Besprechungsraums unterstützen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Integrieren von Ereignissen in Azure Event Grid
> * Senden von Benachrichtigungen über Ereignisse mit Logic Apps

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie das Azure Digital Twins-Setup [konfiguriert](tutorial-facilities-setup.md) and [bereitgestellt](tutorial-facilities-udf.md) haben. Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie fortfahren:

- Ein [Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine aktive Instanz von Azure Digital Twins.
- Die [C#-Beispiele für Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) (auf den Arbeitscomputer heruntergeladen und extrahiert).
- [.NET Core SDK Version 2.1.403 oder höher](https://www.microsoft.com/net/download) auf dem Entwicklungscomputer zum Ausführen des Beispiels. Führen Sie `dotnet --version` aus, um zu überprüfen, ob die richtige Version installiert ist.
- Ein Office 365-Konto zum Senden von Benachrichtigungs-E-Mails.

> [!TIP]
> Verwenden Sie bei der Bereitstellung einer neuen Instanz einen eindeutigen Namen für die Digital Twins-Instanz.

## <a name="integrate-events-with-event-grid"></a>Integrieren von Ereignissen in Event Grid

In diesem Abschnitt richten Sie [Event Grid](../event-grid/overview.md) ein, um Ereignisse von Ihrer Digital Twins-Instanz zu erfassen, die Sie dann an einen [Ereignishandler](../event-grid/event-handlers.md) wie Logic Apps umleiten.

### <a name="create-an-event-grid-topic"></a>Erstellen eines Event Grid-Themas

Ein [Event Grid-Thema](../event-grid/concepts.md#topics) stellt eine Schnittstelle zum Weiterleiten der Ereignisse bereit, die von der benutzerdefinierten Funktion generiert werden. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Bereich **Ressource erstellen**. 

1. Suchen Sie nach der Option **Event Grid-Thema**, und wählen Sie sie aus. Klicken Sie auf **Erstellen**.

1. Geben Sie einen **Namen** für Ihr Event Grid-Thema ein, und wählen Sie das **Abonnement** aus. Wählen Sie die **Ressourcengruppe**, die Sie für Ihre Digital Twins-Instanz verwendet oder erstellt haben, und den **Standort** aus. Klicken Sie auf **Erstellen**. 

    [![Erstellen eines Event Grid-Themas](./media/tutorial-facilities-events/create-event-grid-topic.png)](./media/tutorial-facilities-events/create-event-grid-topic.png#lightbox)

1. Navigieren Sie in Ihrer Ressourcengruppe zum Event Grid-Thema, wählen Sie **Übersicht** aus, und kopieren Sie den Wert für **Themenendpunkt** in eine temporäre Datei. Diese URL wird im nächsten Abschnitt benötigt. 

1. Wählen Sie **Zugriffsschlüssel**, und kopieren Sie **YOUR_KEY_1** und **YOUR_KEY_2** in eine temporäre Datei. Sie benötigen diese Werte im nächsten Abschnitt, um den Endpunkt zu erstellen.

    [![Event Grid-Schlüssel](./media/tutorial-facilities-events/event-grid-keys.png)](./media/tutorial-facilities-events/event-grid-keys.png#lightbox)

### <a name="create-an-endpoint-for-the-event-grid-topic"></a>Erstellen eines Endpunkts für das Event Grid-Thema

1. Vergewissern Sie sich im Befehlsfenster, dass Sie sich im Ordner **occupancy-quickstart\src** des Digital Twins-Beispiels befinden.

1. Öffnen Sie die Datei **actions\createEndpoints.yaml** im Visual Studio Code-Editor. Stellen Sie sicher, dass die Datei Folgendes enthält:

    ```yaml
    - type: EventGrid
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: <Primary connection string for your Event Grid>
      secondaryConnectionString: <Secondary connection string for your Event Grid>
      path: <Event Grid Topic Name without https:// and /api/events, e.g. eventgridname.region.eventgrid.azure.net>
    ```

1. Ersetzen Sie den Platzhalter `<Primary connection string for your Event Grid>` durch den Wert von **YOUR_KEY_1**.

1. Ersetzen Sie den Platzhalter `<Secondary connection string for your Event Grid>` durch den Wert von **YOUR_KEY_2**.

1. Ersetzen Sie den Platzhalter für **path** durch den Pfad des Event Grid-Themas. Diesen Pfad erhalten Sie, indem Sie **https://** und die nachgestellten Ressourcenpfade aus der **Themenendpunkt**-URL entfernen. Der Pfad sollte in etwa wie folgt aussehen: *IhrEventGridName.IhrStandort.eventgrid.azure.net*.

    > [!IMPORTANT]
    > Geben Sie alle Werte ohne Anführungszeichen ein. Stellen Sie sicher, dass nach den Doppelpunkten in der YAML-Datei mindestens ein Leerzeichen vorhanden ist. Sie können den Inhalt der YAML-Datei auch mit einer beliebigen YAML-Onlinevalidierung überprüfen, beispielsweise mit [diesem Tool](https://onlineyamltools.com/validate-yaml).

1. Speichern und schließen Sie die Datei. Führen Sie im Befehlsfenster den folgenden Befehl aus, und melden Sie sich an, wenn Sie dazu aufgefordert werden. 

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Dieser Befehl erstellt den Endpunkt für Event Grid. 

   [![Endpunkte für Event Grid](./media/tutorial-facilities-events/dotnet-create-endpoints.png)](./media/tutorial-facilities-events/dotnet-create-endpoints.png#lightbox)

## <a name="notify-events-with-logic-apps"></a>Senden von Benachrichtigungen über Ereignisse mit Logic Apps

Mit dem [Azure Logic Apps](../logic-apps/logic-apps-overview.md)-Dienst können Sie automatisierte Aufgaben für Ereignisse erstellen, die von anderen Diensten empfangen werden. In diesem Abschnitt richten Sie Logic Apps mithilfe eines [Event Grid-Themas](../event-grid/overview.md) zum Erstellen von E-Mail-Benachrichtigungen für Ereignisse ein, die von Ihren Raumsensoren weitergeleitet werden.

1. Wählen Sie auf der linken Seite im [Azure-Portal](https://portal.azure.com) die Option **Ressource erstellen** aus.

1. Suchen Sie nach einer neuen **Logik-App**-Ressource, und wählen Sie sie aus. Klicken Sie auf **Erstellen**.

1. Geben Sie einen **Namen** für die Logik-App-Ressource ein, und wählen Sie dann Ihr **Abonnement**, Ihre **Ressourcengruppe** sowie Ihren **Standort** aus. Klicken Sie auf **Erstellen**.

    [![Erstellen einer Logic Apps-Ressource](./media/tutorial-facilities-events/create-logic-app.png)](./media/tutorial-facilities-events/create-logic-app.png#lightbox)

1. Öffnen Sie die Logic Apps-Ressource nach ihrer Bereitstellung, und öffnen Sie dann den Bereich **Logik-App-Designer**. 

1. Wählen Sie den Trigger **Bei Auftreten eines Event Grid-Ressourcenereignisses** aus. Melden Sie sich mit Ihrem Azure-Konto bei Ihrem Mandanten an, wenn Sie dazu aufgefordert werden. Wählen Sie bei entsprechender Aufforderung für die Event Grid-Ressource **Zugriff zulassen** aus. Wählen Sie **Weiter**.

1. Führen Sie im Fenster **When a resource event occurs (Preview)** (Bei Auftreten eines Ressourcenereignisses (Vorschau)) die folgenden Schritte aus: 
   
   a. Wählen Sie das **Abonnement** aus, das Sie zum Erstellen des Event Grid-Themas verwendet haben.

   b. Wählen Sie **Microsoft.EventGrid.Topics** als **Ressourcentyp** aus.

   c. Wählen Sie Ihre Event Grid-Ressource im Dropdownfeld **Ressourcenname** aus.

   [![Bereich „Logik-App-Designer“](./media/tutorial-facilities-events/logic-app-resource-event.png)](./media/tutorial-facilities-events/logic-app-resource-event.png#lightbox)

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.

1. Führen Sie im Fenster **Aktion auswählen** die folgenden Schritte aus:

   a. Suchen Sie nach dem Ausdruck **json analysieren**, und wählen Sie die Aktion **JSON analysieren** aus.

   b. Wählen Sie im Feld **Inhalt** in der Liste **Dynamischer Inhalt** die Option **Text** aus.

   c. Wählen Sie **Beispielnutzlast zum Generieren eines Schemas verwenden** aus. Fügen Sie die folgende JSON-Nutzlast ein, und wählen Sie dann **Fertig** aus.

    ```JSON
    {
    "id": "32162f00-a8f1-4d37-aee2-9312aabba0fd",
    "subject": "UdfCustom",
    "data": {
      "TopologyObjectId": "20efd3a8-34cb-4d96-a502-e02bffdabb14",
      "ResourceType": "Space",
      "Payload": "\"Air quality is poor.\"",
      "CorrelationId": "32162f00-a8f1-4d37-aee2-9312aabba0fd"
    },
    "eventType": "UdfCustom",
    "eventTime": "0001-01-01T00:00:00Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/a382ee71-b48e-4382-b6be-eec7540cf271/resourceGroups/HOL/providers/Microsoft.EventGrid/topics/DigitalTwinEventGrid"
    }
    ```

    Diese Nutzlast enthält fiktive Werte. Logic Apps verwendet diese Beispielnutzlast zum Generieren eines *Schemas*.

    [![Logic Apps-Fenster „JSON analysieren“ für Event Grid](./media/tutorial-facilities-events/logic-app-parse-json.png)](./media/tutorial-facilities-events/logic-app-parse-json.png#lightbox)

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.

1. Führen Sie im Fenster **Aktion auswählen** die folgenden Schritte aus:

   a. Wählen Sie **Steuerelement > Bedingung** aus, oder suchen Sie in der Liste **Aktionen** nach **Bedingung**. 

   b. Wählen Sie im ersten Textfeld **Wert auswählen** die Option **eventType** in der Liste **Dynamischer Inhalt** für das Fenster **JSON analysieren** aus.

   c. Geben Sie `UdfCustom` ins zweite Textfeld **Wert auswählen** ein.

   [![Ausgewählte Bedingungen](./media/tutorial-facilities-events/logic-app-condition.png)](./media/tutorial-facilities-events/logic-app-condition.png#lightbox)

1. Führen Sie im Fenster **Bei TRUE** die folgenden Schritte aus:

   a. Wählen Sie **Aktion hinzufügen** und dann **Office 365 Outlook** aus.

   b. Wählen Sie in der Liste **Aktionen** die Option **E-Mail senden** aus. Wählen Sie **Anmelden**, und verwenden Sie die Anmeldeinformationen für Ihr E-Mail-Konto. Wählen Sie **Zugriff zulassen** aus, wenn Sie dazu aufgefordert werden.

   c. Geben Sie im Feld **An** Ihre E-Mail-ID ein, um Benachrichtigungen zu erhalten. Geben Sie im **Betreff** den Text **Digital Twins-Benachrichtigung für schlechte Luftqualität in Gebäudebereich** ein. Wählen Sie dann **TopologyObjectId** in der Liste **Dynamischer Inhalt** für **JSON analysieren** aus.

   d. Geben Sie im gleichen Fenster unter **Text** ungefähr folgenden Text ein: **In einem Raum wurde schlechte Luftqualität erkannt, und die Temperatur muss angepasst werden.** Sie können mit den Elementen in der Liste **Dynamischer Inhalt** genauere Details angeben.

   [![Logic Apps: Auswahl für „E-Mail senden“](./media/tutorial-facilities-events/logic-app-send-email.png)](./media/tutorial-facilities-events/logic-app-send-email.png#lightbox)

1. Wählen Sie oben im Bereich **Logik-App-Designer** die Option **Speichern** aus.

1. Stellen Sie sicher, dass Sie Sensordaten simulieren. Navigieren Sie dazu in einem Befehlsfenster zum Ordner **device-connectivity** des Digital Twins-Beispiels, und führen Sie den Befehl `dotnet run` aus.

Nach einigen Minuten sollten Sie die ersten E-Mail-Benachrichtigungen von der Logic Apps-Ressource erhalten. 

   [![E-Mail-Benachrichtigung](./media/tutorial-facilities-events/logic-app-notification.png)](./media/tutorial-facilities-events/logic-app-notification.png#lightbox)

Wenn Sie keine E-Mails mehr empfangen möchten, navigieren Sie im Portal zu Ihrer Logic Apps-Ressource, und wählen Sie den Bereich **Übersicht** aus. Wählen Sie **Deaktivieren** aus.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie sich nicht weiter mit Azure Digital Twins befassen möchten, können Sie die in diesem Tutorial erstellten Ressourcen löschen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü auf der linken Seite **Alle Ressourcen** und Ihre Digital Twins-Ressourcengruppe aus, und klicken Sie auf **Löschen**.

    > [!TIP]
    > Für den Fall, dass bei Ihnen Probleme beim Löschen der Digital Twins-Instanz aufgetreten sind, wurde ein Dienstupdate mit einer entsprechenden Korrektur bereitgestellt. Versuchen Sie erneut, die Instanz zu löschen.

2. Löschen Sie ggf. die Beispielanwendungen auf Ihrem Arbeitscomputer.

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Sensordaten visualisieren, Trends analysieren und Abweichungen erkennen:

> [!div class="nextstepaction"]
> [Tutorial: Visualisieren und Analysieren von Ereignissen in den Azure Digital Twins-Gebäudebereichen mit Azure Time Series Insights](tutorial-facilities-analyze.md)

Sie können sich auch ausführlicher über die Raumintelligenzgraphen und Objektmodelle in Azure Digital Twins informieren:

> [!div class="nextstepaction"]
> [Understanding Digital Twins object models and spatial intelligence graph](concepts-objectmodel-spatialgraph.md) (Grundlegendes zum Digital Twins-Objektmodell und zum Raumintelligenzgraphen)
