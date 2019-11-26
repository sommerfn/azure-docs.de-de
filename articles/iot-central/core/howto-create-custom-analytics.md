---
title: Erweitern von Azure IoT Central mit benutzerdefinierten Analysen | Microsoft-Dokumentation
description: Als Lösungsentwickler konfigurieren Sie eine IoT Central-Anwendung zur Ausführung von benutzerdefinierten Analysen und Visualisierungen. Diese Lösung verwendet Azure Databricks.
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 618216208b61051d5446f96fb5b28a451b188c35
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72942691"
---
# <a name="extend-azure-iot-central-with-custom-analytics-using-azure-databricks"></a>Erweitern von Azure IoT Central mit benutzerdefinierten Analysen mithilfe von Azure Databricks

Diese Schrittanleitung zeigt, wie Sie als Lösungsentwickler Ihre IoT Central-Anwendung um benutzerdefinierte Analysen und Visualisierungen erweitern können. Im Beispiel wird ein [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/)-Arbeitsbereich verwendet, um den IoT Central-Telemetriedatenstrom zu analysieren und Visualisierungen wie [Boxplots](https://wikipedia.org/wiki/Box_plot) zu generieren.

Diese Schrittanleitung zeigt, wie Sie IoT Central mithilfe der integrierten [Analysetools](howto-create-analytics.md) über die bisherige Funktionalität hinaus erweitern.

In dieser Schrittanleitung erfahren Sie Folgendes:

* Streamen von Telemetriedaten aus einer IoT Central-Anwendung mithilfe des *kontinuierlichen Datenexports*
* Erstellen Sie eine Azure Databricks-Umgebung zum Analysieren und Darstellen von Gerätetelemetrie.

## <a name="prerequisites"></a>Voraussetzungen

Um die in dieser Anleitung aufgeführten Schritte ausführen zu können, benötigen Sie ein aktives Azure-Abonnement.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

### <a name="iot-central-application"></a>IoT Central-Anwendung

Erstellen Sie über die Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) eine IoT Central-Anwendung mit folgenden Einstellungen:

| Einstellung | Wert |
| ------- | ----- |
| Zahlungsplan | Nutzungsbasierte Bezahlung |
| Anwendungsvorlage | Beispiel „Contoso“ |
| Anwendungsname | Standardwert übernehmen oder eigenen Namen angeben |
| URL | Standardwert übernehmen oder eigenes eindeutiges URL-Präfix angeben |
| Verzeichnis | Ihr Azure Active Directory-Mandant |
| Azure-Abonnement | Ihr Azure-Abonnement |
| Region | East US |

Bei den Beispielen und Screenshots in diesem Artikel wird die Region **USA, Osten** verwendet. Wählen Sie einen Standort in Ihrer Nähe, und stellen Sie sicher, dass Sie alle Ressourcen in derselben Region erstellen.

### <a name="resource-group"></a>Resource group

Verwenden Sie das [Azure-Portal zum Erstellen einer Ressourcengruppe](https://portal.azure.com/#create/Microsoft.ResourceGroup) namens **IoTCentralAnalysis**. Sie soll die anderen von Ihnen erstellten Ressourcen enthalten. Erstellen Sie Ihre Azure-Ressourcen am gleichen Standort wie Ihre IoT Central-Anwendung.

### <a name="event-hubs-namespace"></a>Event Hubs-Namespace

Verwenden Sie das [Azure-Portal zum Erstellen eines Event Hubs-Namespace](https://portal.azure.com/#create/Microsoft.EventHub) mit den folgenden Einstellungen:

| Einstellung | Wert |
| ------- | ----- |
| NAME    | Namen Ihres Namespace auswählen |
| Tarif | Basic |
| Subscription | Ihr Abonnement |
| Resource group | IoTCentralAnalysis |
| Location | East US |
| Durchsatzeinheiten | 1 |

### <a name="azure-databricks-workspace"></a>Azure Databricks-Arbeitsbereich

Verwenden Sie das [Azure-Portal zum Erstellen eines Azure Databricks-Diensts](https://portal.azure.com/#create/Microsoft.Databricks) mit den folgenden Einstellungen:

| Einstellung | Wert |
| ------- | ----- |
| Arbeitsbereichname    | Wählen Sie den Namen Ihres Arbeitsbereichs |
| Subscription | Ihr Abonnement |
| Resource group | IoTCentralAnalysis |
| Location | East US |
| Preisstufe | Standard |

Wenn Sie die erforderlichen Ressourcen erstellt haben, sieht Ihre Ressourcengruppe **IoTCentralAnalysis** wie im folgenden Screenshot aus:

![Ressourcengruppe „IoTCentralAnalysis“](media/howto-create-custom-analytics/resource-group.png)

## <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

Sie können eine IoT Central-Anwendung konfigurieren, um Telemetriedaten kontinuierlich an einen Event Hub zu exportieren. In diesem Abschnitt erstellen Sie einen Event Hub, um Telemetriedaten aus Ihrer IoT Central-Anwendung zu empfangen. Der Event Hub übermittelt die Telemetriedaten zur Verarbeitung an Ihren Stream Analytics-Auftrag.

1. Navigieren Sie im Azure-Portal zu Ihrem Event Hubs-Namespace, und klicken Sie auf **+ Event Hub**.
1. Nennen Sie den Event Hub **centralexport**, und klicken Sie auf **Erstellen**.
1. Wählen Sie in der Liste der Event Hubs in Ihrem Namespace **centralexport** aus. Wählen Sie dann **SAS-Richtlinien** aus.
1. Wählen Sie **+ Hinzufügen**. Erstellen Sie eine Richtlinie namens **Lauschen** mit dem Anspruch **Lauschen**.
1. Wenn die Richtlinie fertig ist, wählen Sie sie in der Liste aus, und kopieren Sie den Wert von **Verbindungszeichenfolge – Primärschlüssel**.
1. Notieren Sie sich diese Verbindungszeichenfolge. Sie werden Sie später verwenden, wenn Sie Ihr Databricks-Notebook zum Lesen aus dem Event Hub konfigurieren.

Ihr Event Hubs-Namespace sieht wie im folgenden Screenshot aus:

![Event Hubs-Namespace](media/howto-create-custom-analytics/event-hubs-namespace.png)

## <a name="configure-export-in-iot-central"></a>Konfigurieren des Exports nach IoT Central

Navigieren Sie auf der Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) zu der IoT Central-Anwendung, die Sie aus der Contoso-Vorlage erstellt haben. In diesem Abschnitt konfigurieren Sie die Anwendung so, dass die Telemetriedaten aus den simulierten Geräten an Ihren Event Hub gestreamt werden. So konfigurieren Sie den Export:

1. Navigieren Sie zur Seite **Kontinuierlicher Datenexport**, klicken Sie auf **+ Neu**, und wählen Sie **Azure Event Hubs** aus.
1. Verwenden Sie die folgenden Einstellungen, um den Export zu konfigurieren, und klicken Sie dann auf **Speichern**:

    | Einstellung | Wert |
    | ------- | ----- |
    | Anzeigename | Exportieren nach Event Hubs |
    | Enabled | Andererseits |
    | Event Hubs-Namespace | Der Name Ihres Event Hubs-Namespace |
    | Event Hub | centralexport |
    | Messungen | Andererseits |
    | Geräte | Aus |
    | Gerätevorlagen | Aus |

![Konfiguration des kontinuierlichen Datenexports](media/howto-create-custom-analytics/cde-configuration.png)

Warten Sie, bis der Exportstatus **Wird ausgeführt** lautet, bevor Sie fortfahren.

## <a name="configure-databricks-workspace"></a>Konfigurieren des Databricks-Arbeitsbereichs

Navigieren Sie im Azure-Portal zu Ihrem Azure Databricks-Dienst, und wählen Sie **Arbeitsbereich starten** aus. In Ihrem Browser wird eine neue Registerkarte geöffnet, die Sie an Ihrem Arbeitsbereich anmeldet.

### <a name="create-a-cluster"></a>Erstellen eines Clusters

Wählen Sie auf der Seite **Azure Databricks** unter der Liste der allgemeinen Aufgaben **Neuer Cluster** aus.

Verwenden Sie die Informationen in der folgenden Tabelle zum Erstellen Ihres Clusters:

| Einstellung | Wert |
| ------- | ----- |
| Clustername | centralanalysis |
| Clustermodus | Standard |
| Databricks-Runtimeversion | 5.3 (Scala 2.11, Spark 2.4.0) |
| Python-Version | 3 |
| Automatische Skalierung aktivieren | Nein |
| Terminate after __ minutes of inactivity (Nach __ Minuten Inaktivität beenden) | 30 |
| Workertyp | Standard_DS3_v2 |
| Worker | 1 |
| Treibertyp | Entspricht dem Worker |

Das Erstellen eines Clusters kann mehrere Minuten dauern. Warten Sie, bis die Clustererstellung abgeschlossen ist, bevor Sie den Vorgang fortsetzen.

### <a name="install-libraries"></a>Installieren von Bibliotheken

Warten Sie auf der Seite **Cluster**, bis der Clusterstatus **Wird ausgeführt** lautet.

Die folgenden Schritte zeigen, wie Sie die für Ihr Beispiel benötigte Bibliothek in den Cluster importieren:

1. Warten Sie auf der Seite **Cluster**, bis der Status des interaktiven Clusters **centralanalysis** gleich **Wird ausgeführt** lautet.

1. Wählen Sie den Cluster und dann die Registerkarte **Bibliotheken** aus.

1. Wählen Sie auf der Registerkarte **Bibliotheken** die Option **Neue installieren** aus.

1. Wählen Sie auf der Seite **Bibliothek installieren** als Bibliotheksquelle **Maven** aus.

1. Geben Sie im Textfeld **Koordinaten** den folgenden Wert ein: `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.10`

1. Wählen Sie **Installieren**, um die Bibliothek auf dem Cluster zu installieren.

1. Der Bibliotheksstatus lautet jetzt **Installiert**:

    ![Bibliothek installiert](media/howto-create-custom-analytics/cluster-libraries.png)

### <a name="import-a-databricks-notebook"></a>Importieren eines Databricks-Notebooks

Mit den folgenden Schritten importieren Sie ein Databricks-Notebook, das den Python-Code zum Analysieren und Visualisieren Ihrer IoT Central-Telemetrie enthält:

1. Navigieren Sie in Ihrer Databricks-Umgebung zur Seite **Arbeitsbereich**. Wählen Sie das Dropdownfeld neben Ihrem Kontonamen und dann **Importieren** aus.

1. Wählen Sie, dass Sie aus einer URL importieren möchten, und geben Sie folgende Adresse ein: [https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true](https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true)

1. Wählen Sie zum Importieren des Notebooks **Importieren** aus.

1. Wählen Sie den **Arbeitsbereich** zum Anzeigen des importierten Notebooks aus:

    ![Importiertes Notebook](media/howto-create-custom-analytics/import-notebook.png)

1. Bearbeiten Sie den Code in der ersten Python-Zelle, um die zuvor gespeicherte Event Hubs-Verbindungszeichenfolge hinzuzufügen:

    ```python
    from pyspark.sql.functions import *
    from pyspark.sql.types import *

    ###### Event Hub Connection strings ######
    telementryEventHubConfig = {
      'eventhubs.connectionString' : '{your Event Hubs connection string}'
    }
    ```

## <a name="run-analysis"></a>Ausführen der Analyse

Zum Ausführen der Analyse müssen Sie das Notebook an den Cluster anfügen:

1. Wählen Sie **Getrennt** und dann den Cluster **centralanalysis** aus.
1. Wenn der Cluster nicht ausgeführt wird, starten Sie ihn.
1. Wählen Sie zum Starten des Notebooks die Schaltfläche „Ausführen“ aus.

Möglicherweise wird in der letzten Zelle ein Fehler angezeigt. Wenn dies zutrifft, überprüfen Sie, ob die vorherigen Zellen ausgeführt werden, warten Sie eine Minute, damit einige Daten in den Speicher geschrieben werden, und führen Sie dann die letzte Zelle erneut aus.

### <a name="view-smoothed-data"></a>Anzeigen von geglätteten Daten

Scrollen Sie im Notebook nach unten bis Zelle 14, um einen Plot vom gleitenden Durchschnitt der Luftfeuchtigkeit nach Gerätetyp anzuzeigen. Dieser Plot wird beim Empfang der Streaming-Telemetrie fortlaufend aktualisiert:

![Geglätteter Telemetrieplot](media/howto-create-custom-analytics/telemetry-plot.png)

Sie können die Größe des Diagramms im Notebook ändern.

### <a name="view-box-plots"></a>Anzeigen von Boxplots

Scrollen Sie im Notebook nach unten bis Zelle 20, um die [Boxplots](https://en.wikipedia.org/wiki/Box_plot) anzuzeigen. Weil die Boxplots auf statischen Daten basieren, müssen Sie für deren Aktualisierung die Zelle erneut ausführen:

![Boxplots](media/howto-create-custom-analytics/box-plots.png)

Sie können die Größe der Plots im Notebook ändern.

## <a name="tidy-up"></a>Aufräumen

Um nach dieser Vorgehensweise aufzuräumen und unnötige Kosten zu vermeiden, löschen Sie die Ressourcengruppe **IoTCentralAnalysis** im Azure-Portal.

Sie können die IoT Central-Anwendung in der Anwendung selbst auf der Seite **Verwaltung** löschen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schrittanleitung wurde Folgendes vermittelt:

* Streamen von Telemetriedaten aus einer IoT Central-Anwendung mithilfe des *kontinuierlichen Datenexports*
* Erstellen einer Azure Databricks-Umgebung zum Analysieren und Darstellen von Telemetriedaten

Sie wissen nun, wie Sie benutzerdefinierte Analysen erstellen. Als Nächstes sollten Sie den Artikel [Visualisieren und Analysieren der Azure IoT Central-Daten in einem Power BI-Dashboard](howto-connect-powerbi.md) lesen.
