---
title: 'Tutorial: Erstellen einer Azure Time Series Insights-Umgebung | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie eine Time Series Insights-Umgebung erstellen, die mit Daten von simulierten Geräten aufgefüllt wird.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 10/16/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 518847db727c9d8c527d272f9122ef9850ca9135
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72553049"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Tutorial: Erstellen einer Azure Time Series Insights-Umgebung

In diesem Tutorial wird der Prozess zum Erstellen einer Azure Time Series Insights-Umgebung beschrieben, die mit Daten von simulierten Geräten gefüllt wird. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen Sie eine Time Series Insights-Umgebung.
> * Erstellen einer Lösung für die Gerätesimulation mit einem IoT Hub
> * Herstellen einer Verbindung der Time Series Insights-Umgebung mit dem IoT-Hub
> * Ausführen einer Gerätesimulation zum Streamen von Daten in die Time Series Insights-Umgebung
> * Überprüfen der simulierten Telemetriedaten

> [!IMPORTANT]
> Registrieren Sie sich für ein [kostenloses Azure-Abonnement](https://azure.microsoft.com/free/), falls Sie noch keins besitzen.

## <a name="prerequisites"></a>Voraussetzungen

* Ihr Azure-Anmeldekonto muss außerdem Mitglied der Rolle **Besitzer** des Abonnements sein. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Azure-Ressourcen mit RBAC und dem Azure-Portal](../role-based-access-control/role-assignments-portal.md).

## <a name="review-video"></a>Video

### <a name="learn-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-and-get-started-with-time-series-insights-br"></a>Hier wird gezeigt, wie Sie einen Azure IoT Solution Accelerator verwenden, um Daten zu generieren und die ersten Schritte mit Time Series Insights auszuführen. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="overview"></a>Übersicht

Die Time Series Insights-Umgebung ist der Ort, an dem Gerätedaten erfasst und gespeichert werden. Nach dem Speichern können der [Azure Time Series Insights-Explorer](time-series-quickstart.md) und die [Time Series Insights-Abfrage-API](/rest/api/time-series-insights/ga-query-api) verwendet werden, um die Daten abzufragen und zu analysieren.

Azure IoT Hub ist die Ereignisquelle, die von allen (simulierten oder physischen) Geräten in diesem Tutorial zum sicheren Herstellen einer Verbindung und zum Übertragen von Daten in Ihre Azure-Cloud verwendet wird.

In diesem Tutorial wird auch ein [IoT Solution Accelerator](https://www.azureiotsolutions.com) genutzt, um Beispieltelemetriedaten zu generieren und an IoT Hub zu streamen.

>[!TIP]
> Mit [IoT Solution Accelerators](https://www.azureiotsolutions.com) werden für Unternehmen konzipierte vorkonfigurierte Lösungen bereitgestellt, mit denen Sie die Entwicklung benutzerdefinierter IoT-Lösungen beschleunigen können.

## <a name="create-a-device-simulation"></a>Erstellen einer Gerätesimulation

Erstellen Sie zunächst die Lösung für die Gerätesimulation, mit der Testdaten zum Auffüllen Ihrer Time Series Insights-Umgebung generiert werden.

1. Rufen Sie in einem separaten Fenster oder auf einer separaten Registerkarte [azureiotsolutions.com](https://www.azureiotsolutions.com) auf. Melden Sie sich mit demselben Azure-Abonnementkonto an, und wählen Sie den Accelerator **Gerätesimulation** aus. Wählen Sie **Jetzt testen** aus.

   [![Ausführen des Accelerators „Gerätesimulation“](media/tutorial-create-populate-tsi-environment/sa-main.png)](media/tutorial-create-populate-tsi-environment/sa-main.png#lightbox)

1. Geben Sie auf der Lösungsseite **Gerätesimulation erstellen** die erforderlichen Parameter ein:

   Parameter|BESCHREIBUNG
   ---|---
   **Bereitstellungsname** | Mit diesem eindeutigen Wert wird eine neue Ressourcengruppe erstellt. Die aufgelisteten Azure-Ressourcen werden erstellt und der Ressourcengruppe zugewiesen.
   **Azure-Abonnement** | Geben Sie dasselbe Abonnement an, das Sie auch bei der Erstellung Ihrer Time Series Insights-Umgebung im vorherigen Abschnitt verwendet haben.
   **Bereitstellungsoptionen** | Wählen Sie **Provision new IoT Hub** (Neuen IoT Hub bereitstellen) aus, um einen für dieses Tutorial spezifischen neuen IoT-Hub zu erstellen.
   **Azure-Standort** | Geben Sie dieselbe Region an, die Sie auch bei der Erstellung Ihrer Time Series Insights-Umgebung im vorherigen Abschnitt verwendet haben.

   Wählen Sie nach Abschluss des Vorgangs die Option **Lösung erstellen** aus, um die Azure-Ressourcen der Lösung bereitzustellen. Dieser Vorgang kann bis zu 20 Minuten dauern.

   [![Bereitstellen der Gerätesimulationslösung](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png#lightbox)

1. Nachdem die Bereitstellung abgeschlossen ist, ändert sich der Text über Ihrer neuen Lösung von **Wird bereitgestellt...** in **Bereit**.

   >[!IMPORTANT]
   > Wählen Sie noch nicht **Starten** aus. Lassen Sie diese Webseite geöffnet, da Sie später hierhin zurückkehren.

   [![Bereitstellung der Gerätesimulationslösung abgeschlossen](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png#lightbox)

1. Anschließend können Sie die neu erstellten Ressourcen im Azure-Portal überprüfen. Auf der Seite **Ressourcengruppen** wurde eine neue Ressourcengruppe mit dem **Lösungsnamen** erstellt, den Sie im letzten Schritt angegeben haben. Notieren Sie sich die Ressourcen, die für die Gerätesimulation erstellt wurden.

   [![Ressourcen für die Gerätesimulation](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="create-an-environment"></a>Erstellen einer Umgebung

Erstellen Sie zweitens in Ihrem Azure-Abonnement eine Time Series Insights-Umgebung.

1. Melden Sie sich unter Verwendung Ihres Azure-Abonnementkontos beim [Azure-Portal](https://portal.azure.com) an. 
1. Wählen Sie oben links die Option **+ Ressource erstellen**. 
1. Wählen Sie die Kategorie **Internet der Dinge (IoT)** und dann **Time Series Insights** aus. 

   [![Auswählen der Time Series Insights-Umgebungsressource](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

1. Geben Sie auf der Seite **Time Series Insights-Umgebung** die erforderlichen Parameter ein:

   Parameter|BESCHREIBUNG
   ---|---
   **Umgebungsname** | Wählen Sie einen eindeutigen Namen für die Time Series Insights-Umgebung aus. Die Namen werden vom Time Series Insights-Explorer und von [Abfrage-APIs](https://docs.microsoft.com/rest/api/time-series-insights/ga-query) verwendet.
   **Abonnement** | Abonnements sind Container für Azure-Ressourcen. Wählen Sie ein Abonnement aus, um die Time Series Insights-Umgebung zu erstellen.
   **Ressourcengruppe** | Eine Ressourcengruppe ist ein Container für Azure-Ressourcen. Wählen Sie eine vorhandene Ressourcengruppe für die Time Series Insights-Umgebungsressource aus, oder erstellen Sie eine neue.
   **Location** | Wählen Sie eine Rechenzentrumsregion für Ihre Time Series Insights-Umgebung aus. Erstellen Sie die Time Series Insights-Umgebung in derselben Region wie andere IoT-Ressourcen, um zusätzliche Wartezeit zu vermeiden.
   **Tier** | Wählen Sie den erforderlichen Durchsatz aus. Wählen Sie **S1** aus.
   **Capacity** | Die Kapazität ist der Multiplikator, der auf die Eingangsrate und Speicherkapazität der ausgewählten SKU angewendet wird. Sie können die Kapazität nach der Erstellung ändern. Wählen Sie als Kapazität **1**.

   Wenn Sie fertig sind, wählen Sie **Weiter: Ereignisquelle** aus, um mit dem nächsten Schritt fortzufahren.

   [![Erstellen einer Time Series Insights-Umgebungsressource](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png#lightbox)

1. Verbinden Sie nun die Time Series Insights-Umgebung mit dem vom Solution Accelerator erstellten IoT-Hub. Legen Sie **Hub auswählen** auf `Select existing` fest. Wählen Sie dann den IoT-Hub, der vom Solution Accelerator erstellt wurde, als Sie **IoT Hub-Name** festgelegt haben.

   [![Herstellen einer Verbindung der Time Series Insights-Umgebung mit dem erstellten IoT-Hub](media/tutorial-create-populate-tsi-environment/ap-create-resource-iot-hub.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-iot-hub.png#lightbox)

   Wählen Sie zum Schluss **Überprüfen + erstellen** aus.

1. Im Bereich **Benachrichtigungen** können Sie den Abschluss der Bereitstellung überwachen. 

   [![Bereitstellung der Time Series Insights-Umgebung erfolgreich abgeschlossen](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="run-device-simulation"></a>Ausführen der Gerätesimulation

Füllen Sie nach Abschluss der Bereitstellung und anfänglichen Konfiguration die Time Series Insights-Umgebung mit Beispieldaten von durch den [Solution Accelerator erzeugten simulierten Geräten](#create-a-device-simulation) auf.

Zusammen mit dem IoT-Hub wurde eine Azure App Service-Webanwendung generiert, um Telemetriedaten für simulierte Geräte zu erstellen und zu übertragen.

1. Wechseln Sie zurück zum [Solution Accelerators-Dashboard](https://www.azureiotsolutions.com/Accelerators#dashboard). Melden Sie sich an, falls dies erforderlich ist, indem Sie dasselbe Azure-Konto wie im gesamten bisherigen Tutorial verwenden. Wählen Sie Ihre „Gerätelösung“ aus, und **wechseln Sie anschließend zu Ihrem Solution Accelerator**, um die bereitgestellte Lösung zu starten.

     [![Solution Accelerators-Dashboard](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png#lightbox)

1. Die Web-App für die Gerätesimulation fordert Sie zunächst auf, der Webanwendung die Berechtigung „Sie anmelden und Ihr Profil lesen“ zu erteilen. Mit dieser Berechtigung kann die Anwendung die Benutzerprofilinformationen abrufen, die zur Unterstützung der Funktionsweise einer Anwendung erforderlich sind:

     [![Gerätesimulation – Zustimmung für Webanwendung](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png#lightbox)

1. Geben Sie die erforderlichen Parameter ein, nachdem die Seite mit dem **Simulationssetup** geladen wurde.

   Parameter|BESCHREIBUNG
   ---|---
   **IoT-Zielhub** | Wählen Sie **Use pre-provisioned IoT Hub** (Vorab bereitgestellten IoT Hub verwenden) aus.
   **Gerätemodell** | Wählen Sie **Chiller** aus.
   **Anzahl von Geräten**  | Geben Sie unter **Menge** die Zahl `1000` ein.
   **Telemetriefrequenz** | Geben Sie `10` Sekunden ein.
   **Simulationsdauer** | Wählen Sie **End in:** (Endet in:) aus, und geben Sie einen Wert von `5` Minuten ein.

   Wählen Sie abschließend **Simulation starten** aus. Die Simulation wird fünf Minuten lang ausgeführt. Sie generiert alle zehn Sekunden Daten aus 1.000 simulierten Geräten. 

   [![Setup der Gerätesimulation](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png#lightbox)

1. Während der Ausführung der Simulation sehen Sie, dass die Felder **Total messages** (Gesamte Nachrichten) und **Messages per second** (Nachrichten pro Sekunde) etwa alle zehn Sekunden aktualisiert werden. Die Simulation wird nach ca. fünf Minuten beendet, und das **Simulationssetup** wird wieder angezeigt.

   [![Aktive Gerätesimulation](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png#lightbox)

## <a name="verify-the-telemetry-data"></a>Überprüfen der Telemetriedaten

In diesem letzten Abschnitt überprüfen Sie, ob die Telemetriedaten in der Time Series Insights-Umgebung generiert und gespeichert wurden. Für die Überprüfung der Daten verwenden Sie den Time Series Insights-Explorer, mit dem Telemetriedaten abgefragt und analysiert werden können.

1. Kehren Sie zur Seite **Übersicht** der Ressourcengruppe in der Time Series Insights-Umgebung zurück. Wählen Sie die Time Series Insights-Umgebung aus.

   [![Time Series Insights-Umgebung: Ressourcengruppe und Umgebung](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

1. Wählen Sie in der Time Series Insights-Umgebung auf der Seite **Übersicht** die **Time Series Insights-Explorer-URL** aus, um den Time Series Insights-Explorer zu öffnen.

   [![Time Series Insights-Explorer](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

1. Der Time Series Insights-Explorer wird über Ihr Azure-Portalkonto geladen und authentifiziert. Beim ersten Anzeigen sehen Sie im Diagrammbereich, dass die Time Series Insights-Umgebung mit simulierten Telemetriedaten aufgefüllt wurde. Verwenden Sie oben links die Dropdownliste, um den Zeitbereich einzugrenzen. Geben Sie einen Zeitbereich ein, der groß genug ist, um die Dauer der Gerätesimulation abzudecken. Wählen Sie dann das Lupensymbol für die Suche aus.

   [![Time Series Insights-Explorer: Zeitbereichsfilter](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

1. Durch das Eingrenzen des Zeitbereichs kann das Diagramm auf die einzelnen Datenübertragungsspitzen (Bursts), auf den IoT-Hub und die Time Series Insights-Umgebung vergrößert werden. Beachten Sie auch oben rechts den Text **Das Streaming wurde abgeschlossen**, unter dem die Gesamtzahl von gefundenen Ereignissen angezeigt wird. Sie können auch den Schieberegler für die **Intervallgröße** verschieben, um die Granularität im Diagramm zu steuern:

   [![Time Series Insights-Explorer: Nach Zeitbereich gefilterte Ansicht](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

1. Darüber hinaus können Sie auch mit der linken Maustaste auf eine Region klicken, um nach einem Bereich zu filtern. Klicken Sie dann mit der rechten Maustaste, und verwenden Sie die Option **Ereignisse untersuchen**, um Ereignisdetails in der tabellarischen Ansicht **Ereignisse** anzuzeigen:

   [![Time Series Insights-Explorer: Nach Zeitbereich gefilterte Ansicht mit Ereignissen](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Tutorial werden mehrere ausgeführte Azure-Dienste erstellt, um die Time Series Insights-Umgebung und die Gerätesimulationslösung zu unterstützen. Navigieren Sie zurück zum Azure-Portal, um sie zu entfernen.

Gehen Sie im Azure-Portal im Menü auf der linken Seite wie folgt vor:

1. Wählen Sie das Symbol **Ressourcengruppen** aus. Wählen Sie dann die Ressourcengruppe aus, die Sie für die Time Series Insights-Umgebung erstellt haben. Wählen Sie am oberen Rand der Seite **Ressourcengruppe löschen** aus, geben Sie den Namen der Ressourcengruppe ein, und wählen Sie dann **Löschen** aus.

1. Wählen Sie das Symbol **Ressourcengruppen** aus. Wählen Sie dann die Ressourcengruppe aus, die mit dem Solution Accelerator für die Gerätesimulation erstellt wurde. Wählen Sie am oberen Rand der Seite **Ressourcengruppe löschen** aus, geben Sie den Namen der Ressourcengruppe ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen Sie eine Time Series Insights-Umgebung.
> * Erstellen einer Lösung für die Gerätesimulation mit einem IoT Hub
> * Herstellen einer Verbindung der Time Series Insights-Umgebung mit dem IoT-Hub
> * Ausführen einer Gerätesimulation zum Streamen von Daten in die Time Series Insights-Umgebung
> * Überprüfen der simulierten Telemetriedaten

Sie haben erfahren, wie Sie Ihre eigene Time Series Insights-Umgebung erstellen. Lernen Sie nun, wie Sie eine Webanwendung entwickeln, die Daten aus einer Time Series Insights-Umgebung nutzt:

> [!div class="nextstepaction"]
> [SDK-Visualisierungsbeispiele für gehostete Clients](https://tsiclientsample.azurewebsites.net/)
