---
title: 'Tutorial: Analysieren von Ereignissen aus dem Azure Digital Twins-Setup'
description: In diesem Tutorial erfahren Sie, wie Sie Ereignisse in Ihren Azure Digital Twins-Gebäudebereichen mit Azure Time Series Insights visualisieren und analysieren.
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.custom: seodec18
ms.service: digital-twins
ms.topic: tutorial
ms.date: 11/12/2019
ms.openlocfilehash: 3df0fa448e320cba6dd3aaba1bb1be09c1a8b49b
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74107682"
---
# <a name="tutorial-visualize-and-analyze-events-from-azure-digital-twins-by-using-time-series-insights"></a>Tutorial: Visualisieren und Analysieren von Ereignissen über Azure Digital Twins mit Time Series Insights

Nachdem Sie Ihre Azure Digital Twins-Instanz sowie Ihre Gebäudebereiche bereitgestellt und die benutzerdefinierte Funktion zum Überwachen bestimmter Bedingungen implementiert haben, können Sie die Ereignisse und Daten aus den Gebäudebereichen analysieren, um Trends und Abweichungen zu erkennen.

Im [ersten Tutorial](tutorial-facilities-setup.md) haben Sie den Raumgraphen eines imaginären Gebäudes mit einem Raum mit Bewegungs-, Kohlendioxid- und Temperatursensoren konfiguriert. Im [zweiten Tutorial](tutorial-facilities-udf.md) haben Sie Ihren Graphen und eine benutzerdefinierte Funktion bereitgestellt. Die Funktion überwacht die Sensorwerte und löst Benachrichtigungen aus, wenn die richtigen Bedingungen erfüllt sind, d.h. wenn der Raum leer ist und die Temperatur- und Kohlendioxidwerte normal sind.

In diesem Tutorial erfahren Sie, wie Sie die Benachrichtigungen und Daten aus Ihrem Azure Digital Twins-Setup in Azure Time Series Insights integrieren. Sie können die Sensorwerte dann im Zeitverlauf visualisieren. Sie können nach Trends suchen, beispielsweise welcher Raum am häufigsten genutzt wird und zu welcher Tageszeit der Raum am häufigsten belegt ist. Sie können auch Abweichungen erkennen, beispielsweise in welchen Räumen es wärmer und die Luftqualität schlecht ist oder ob ein Bereich im Gebäude durchgängig hohe Temperaturwerte meldet, die auf eine defekte Klimaanlage hinweisen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Streamen von Daten mithilfe von Azure Event Hubs
> * Analysieren mit Time Series Insights

## <a name="prerequisites"></a>Voraussetzungen

In diesem Tutorial wird vorausgesetzt, dass Sie das Azure Digital Twins-Setup [konfiguriert](tutorial-facilities-setup.md) and [bereitgestellt](tutorial-facilities-udf.md) haben. Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie fortfahren:

- Ein [Azure-Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Eine aktive Instanz von Azure Digital Twins.
- Die [C#-Beispiele für Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp) (auf den Arbeitscomputer heruntergeladen und extrahiert).
- [.NET Core SDK Version 2.1.403 oder höher](https://www.microsoft.com/net/download) auf dem Entwicklungscomputer zum Ausführen des Beispiels. Führen Sie `dotnet --version` aus, um zu überprüfen, ob die richtige Version installiert ist.

> [!TIP]
> Verwenden Sie bei der Bereitstellung einer neuen Instanz einen eindeutigen Namen für die Digital Twins-Instanz.

## <a name="stream-data-by-using-event-hubs"></a>Streamen von Daten mithilfe von Event Hubs

Mit dem [Event Hubs](../event-hubs/event-hubs-about.md)-Dienst können Sie eine Pipeline zum Streamen Ihrer Daten erstellen. In diesem Abschnitt wird erläutert, wie Sie den Event Hub als Konnektor zwischen Ihren Azure Digital Twins- und Time Series Insights-Instanzen erstellen.

### <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im linken Bereich **Ressource erstellen**.

1. Suchen Sie nach der Option **Event Hubs**, und wählen Sie sie aus. Klicken Sie auf **Erstellen**.

1. Geben Sie einen **Namen** für den Event Hubs-Namespace ein. Wählen Sie unter **Tarif** die Option **Standard** sowie Ihr **Abonnement**, die **Ressourcengruppe**, die Sie für Ihre Digital Twins-Instanz verwendet haben, sowie den **Standort** aus. Klicken Sie auf **Erstellen**.

1. Wählen Sie in der Event Hubs-Namespacebereitstellung erst den Bereich **Übersicht** und dann **Zu Ressource wechseln** aus.

    [![Event Hubs-Namespace nach der Bereitstellung](./media/tutorial-facilities-analyze/open-event-hub-ns.png)](./media/tutorial-facilities-analyze/open-event-hub-ns.png#lightbox)

1. Wählen Sie oben im Bereich **Übersicht** des Event Hubs-Namespace die Schaltfläche **Event Hub** aus.
    [![Schaltfläche „Event Hub“](./media/tutorial-facilities-analyze/create-event-hub.png)](./media/tutorial-facilities-analyze/create-event-hub.png#lightbox)

1. Geben Sie einen **Namen** für den Event Hub ein, und wählen Sie **Erstellen** aus.

   Nach der Bereitstellung wird der Event Hub im Bereich **Event Hubs** des Event Hubs-Namespace mit dem Status **Aktiv** angezeigt. Wählen Sie diesen Event Hub aus, um den Bereich **Übersicht** zu öffnen.

1. Wählen Sie im oberen Bereich die Schaltfläche **Consumergruppe**, und geben Sie einen Namen für die Consumergruppe ein, beispielsweise **tsievents**. Klicken Sie auf **Erstellen**.

    [![Event Hub-Consumergruppe](./media/tutorial-facilities-analyze/event-hub-consumer-group.png)](./media/tutorial-facilities-analyze/event-hub-consumer-group.png#lightbox)

   Nach der Erstellung wird die Consumergruppe in der Liste unten im Bereich **Übersicht** des Event Hubs angezeigt.

1. Öffnen Sie den Bereich **Freigegebene Zugriffsrichtlinien** für Ihren Event Hub, und wählen Sie die Schaltfläche **Hinzufügen** aus. Geben Sie als Richtlinienname **ManageSend** ein, vergewissern Sie sich, dass alle Kontrollkästchen aktiviert sind, und wählen Sie **Erstellen** aus.

    [![Event Hub-Verbindungszeichenfolgen](./media/tutorial-facilities-analyze/event-hub-connection-strings.png)](./media/tutorial-facilities-analyze/event-hub-connection-strings.png#lightbox)

1. Öffnen Sie die soeben erstellte Richtlinie „ManageSend“, und kopieren Sie die Werte für **Verbindungszeichenfolge – Primärschlüssel** und **Verbindungszeichenfolge – Sekundärschlüssel** in eine temporäre Datei. Sie benötigen diese Werte im nächsten Abschnitt, um einen Endpunkt für den Event Hub zu erstellen.

### <a name="create-an-endpoint-for-the-event-hub"></a>Erstellen eines Endpunkts für den Event Hub

1. Vergewissern Sie sich im Befehlsfenster, dass Sie sich im Ordner **occupancy-quickstart\src** des Azure Digital Twins-Beispiels befinden.

1. Öffnen Sie die Datei **actions\createEndpoints.yaml** im Editor. Ersetzen Sie den Inhalt durch den folgenden Code:

    ```yaml
    - type: EventHub
      eventTypes:
      - SensorChange
      - SpaceChange
      - TopologyOperation
      - UdfCustom
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hub
    - type: EventHub
      eventTypes:
      - DeviceMessage
      connectionString: Primary_connection_string_for_your_event_hub
      secondaryConnectionString: Secondary_connection_string_for_your_event_hub
      path: Name_of_your_Event_Hub
    ```

1. Ersetzen Sie die Platzhalter `Primary_connection_string_for_your_event_hub` durch den Wert von **Verbindungszeichenfolge – Primärschlüssel** für den Event Hub. Stellen Sie sicher, dass die Verbindungszeichenfolge das folgende Format aufweist:

   ```plaintext
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey1GUID;EntityPath=nameOfYourEventHub
   ```

1. Ersetzen Sie die Platzhalter `Secondary_connection_string_for_your_event_hub` durch den Wert von **Verbindungszeichenfolge – Sekundärschlüssel** für den Event Hub. Stellen Sie sicher, dass die Verbindungszeichenfolge das folgende Format aufweist: 

   ```plaintext
   Endpoint=sb://nameOfYourEventHubNamespace.servicebus.windows.net/;SharedAccessKeyName=ManageSend;SharedAccessKey=yourShareAccessKey2GUID;EntityPath=nameOfYourEventHub
   ```

1. Ersetzen Sie die Platzhalter `Name_of_your_Event_Hub` durch den Namen Ihres Event Hubs.

    > [!IMPORTANT]
    > Geben Sie alle Werte ohne Anführungszeichen ein. Stellen Sie sicher, dass nach den Doppelpunkten in der YAML-Datei mindestens ein Leerzeichen vorhanden ist. Sie können den Inhalt der YAML-Datei auch mit einer beliebigen YAML-Onlinevalidierung überprüfen, beispielsweise mit [diesem Tool](https://onlineyamltools.com/validate-yaml).

1. Speichern und schließen Sie die Datei. Führen Sie im Befehlsfenster den folgenden Befehl aus, und melden Sie sich mit Ihrem Azure-Konto an, wenn Sie dazu aufgefordert werden.

    ```cmd/sh
    dotnet run CreateEndpoints
    ```

   Dieser Befehl erstellt zwei Endpunkte für Ihren Event Hub.

   [![Endpunkte für Event Hubs](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png)](./media/tutorial-facilities-analyze/dotnet-create-endpoints.png#lightbox)

## <a name="analyze-with-time-series-insights"></a>Analysieren mit Time Series Insights

1. Wählen Sie auf der linken Seite im [Azure-Portal](https://portal.azure.com) die Option **Ressource erstellen** aus. 

1. Suchen Sie nach einer allgemein verfügbaren **Time Series Insights**-Ressource, und wählen Sie sie aus. Klicken Sie auf **Erstellen**.

1. Geben Sie einen **Namen** für Ihre Time Series Insights-Instanz ein, und wählen Sie dann Ihr **Abonnement** aus. Wählen Sie die **Ressourcengruppe**, die Sie für Ihre Digital Twins-Instanz verwendet haben, und den **Standort** aus. Klicken Sie auf **Weiter: Ereignisquelle** oder die Registerkarte **Ereignisquelle**.

    [![Auswahl zum Erstellen einer Time Series Insights-Instanz](./media/tutorial-facilities-analyze/create-tsi.png)](./media/tutorial-facilities-analyze/create-tsi.png#lightbox)

1. Geben Sie auf der Registerkarte **Ereignisquelle** einen **Namen** ein. Wählen Sie **Ereignishub** als **Quelltyp** aus, und stellen Sie sicher, dass die anderen Werte ordnungsgemäß ausgewählt sind. Wählen Sie unter **Event Hub-Richtlinienname** den Namen **ManageSend** und anschließend die im vorherigen Abschnitt erstellte Consumergruppe für **Event Hub-Consumergruppe** aus. Klicken Sie auf **Überprüfen + erstellen**.

    [![Auswahl zum Erstellen einer Ereignisquelle](./media/tutorial-facilities-analyze/tsi-event-source.png)](./media/tutorial-facilities-analyze/tsi-event-source.png#lightbox)

1. Überprüfen Sie im Bereich **Überprüfen + erstellen** die von Ihnen eingegebenen Informationen, und klicken Sie auf **Erstellen**.

1. Wählen Sie im Bereitstellungsbereich die Time Series Insights-Ressource aus, die Sie gerade erstellt haben. Der Bereich **Übersicht** für Ihre Time Series Insights-Umgebung wird geöffnet.

1. Klicken Sie oben auf die Schaltfläche **Zur Umgebung wechseln**. Falls eine Datenzugriffswarnung angezeigt wird, öffnen Sie den Bereich **Datenzugriffsrichtlinien** für Ihre Time Series Insights-Instanz, und wählen Sie **Hinzufügen**, die Rolle **Mitwirkender** und anschließend den gewünschten Benutzer aus.

1. Die Schaltfläche **Zur Umgebung wechseln** öffnet den [Time Series Insights-Explorer](../time-series-insights/time-series-insights-explorer.md). Wenn keine Ereignisse angezeigt werden, simulieren Sie Geräteereignisse, indem Sie zum Projekt **device-connectivity** des Digital Twins-Beispiels navigieren und `dotnet run` ausführen.

1. Nachdem einige simulierte Ereignisse generiert wurden, kehren Sie zum Time Series Insights-Explorer zurück, und wählen Sie oben die Schaltfläche „Aktualisieren“. Daraufhin sollten Analysediagramme für Ihre simulierten Sensordaten erstellt werden. 

    [![Diagramm im Time Series Insights-Explorer](./media/tutorial-facilities-analyze/tsi-explorer.png)](./media/tutorial-facilities-analyze/tsi-explorer.png#lightbox)

1. Im Time Series Insights-Explorer können Sie dann Diagramme und Wärmebilder für verschiedene Ereignisse und Daten von Ihren Räumen, Sensoren und anderen Ressourcen generieren. Klicken Sie auf der linken Seite auf die Dropdownfelder **MEASURE** und **TEILEN NACH**, um eigene Visualisierungen zu erstellen. 

   Wählen Sie beispielsweise im Feld **MEASURE** die Option **Ereignisse** und im Feld **TEILEN NACH** die Option **DigitalTwins-SensorHardwareId** aus, um ein Wärmebild für jeden Sensor zu generieren. Das Wärmebild sieht etwa wie in der folgenden Abbildung aus:

   [![Wärmebild im Time Series Insights-Explorer](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png)](./media/tutorial-facilities-analyze/tsi-explorer-heatmap.png#lightbox)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie sich nicht weiter mit Azure Digital Twins befassen möchten, können Sie die in diesem Tutorial erstellten Ressourcen löschen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü auf der linken Seite **Alle Ressourcen** und Ihre Digital Twins-Ressourcengruppe aus, und klicken Sie dann auf **Löschen**.

    > [!TIP]
    > Für den Fall, dass bei Ihnen Probleme beim Löschen der Digital Twins-Instanz aufgetreten sind, wurde ein Dienstupdate mit einer entsprechenden Korrektur bereitgestellt. Versuchen Sie erneut, die Instanz zu löschen.

2. Löschen Sie ggf. die Beispielanwendungen auf Ihrem Arbeitscomputer.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie mehr über Raumintelligenzgraphen und Objektmodelle in Azure Digital Twins.

> [!div class="nextstepaction"]
> [Understanding Digital Twins object models and spatial intelligence graph](concepts-objectmodel-spatialgraph.md) (Grundlegendes zum Digital Twins-Objektmodell und zum Raumintelligenzgraphen)
