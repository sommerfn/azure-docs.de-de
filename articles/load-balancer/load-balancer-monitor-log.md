---
title: Überwachen von Vorgängen, Ereignissen und Leistungsindikatoren für öffentliche Load Balancer vom Typ „Basic“
titlesuffix: Azure Load Balancer
description: Erfahren Sie, wie Sie die Protokollierung für Warnereignisse und Integritätsteststatus für öffentliche Load Balancer vom Typ „Basic“ aktivieren.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/27/2018
ms.author: allensu
ms.openlocfilehash: df35168d0fab0b01ff11c4105a1fcc5b16e21f30
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350721"
---
# <a name="azure-monitor-logs-for-public-basic-load-balancer"></a>Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“

>[!IMPORTANT] 
>Azure Load Balancer unterstützt zwei unterschiedliche Typen: Basic und Standard. In diesem Artikel wird der Load Balancer vom Typ „Basic“ beschrieben. Weitere Informationen zum Load Balancer vom Typ „Standard“ finden Sie unter [Übersicht: Azure Standard Load Balancer](load-balancer-standard-overview.md), der Telemetriedaten über mehrdimensionale Metriken in Azure Monitor verfügbar macht.

Sie können in Azure verschiedene Protokolltypen verwenden, um Load Balancer vom Typ „Basic“ zu verwalten und eventuelle Fehler zu beheben. Auf einige dieser Protokolle kann über das Portal zugegriffen werden. Protokolle können an einen Event Hub oder einen Log Analytics-Arbeitsbereich gestreamt werden. Alle Protokolle können aus Azure Blob Storage extrahiert und in anderen Tools wie Excel und PowerBI angezeigt werden.  In der unten stehenden Liste finden Sie weitere Informationen über die verschiedenen Typen von Protokollen.

* **Aktivitätsprotokolle**: Mithilfe von [Anzeigen von Aktivitätsprotokollen zur Überwachung von Aktionen in Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) können Sie alle Aktivitäten, die an Ihre Azure-Abonnements übermittelt werden, und ihren Status anzeigen. Aktivitätsprotokolle sind standardmäßig aktiviert und können im Azure-Portal angezeigt werden.
* **Warnereignisprotokolle**: Verwenden Sie dieses Protokoll, um Warnungen anzuzeigen, die vom Lastenausgleich ausgelöst wurden. Der Status des Lastenausgleichs wird alle fünf Minuten erfasst. Dieses Protokoll wird nur geschrieben, wenn ein Warnereignis für den Lastenausgleich ausgelöst wird.
* **Integritätstestprotokolle:** Verwenden Sie dieses Protokoll zum Anzeigen von Problemen, die vom Integritätstest erkannt wurden, z.B. die Anzahl der Instanzen in Ihrem Back-End-Pool, die aufgrund von Integritätstestfehlern keine Anforderungen vom Lastenausgleich empfangen. In dieses Protokoll wird geschrieben, wenn sich der Integritätsteststatus ändert.

> [!IMPORTANT]
> Azure Monitor-Protokolle funktioniert derzeit nur für Load Balancer vom Typ „Basic“. Protokolle sind nur für Ressourcen verfügbar, die im Ressourcen-Manager-Bereitstellungsmodell bereitgestellt werden. Sie können Protokolle nicht für Ressourcen im klassischen Bereitstellungsmodell verwenden. Weitere Informationen zu diesen Bereitstellungsmodellen finden Sie unter [Grundlegendes zur Bereitstellung über Resource Manager im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="enable-logging"></a>Aktivieren der Protokollierung

Die Aktivitätsprotokollierung ist automatisch für alle Resource Manager-Ressourcen aktiviert. Aktivieren Sie die Ereignis- und Integritätstestprotokollierung, um mit der Erfassung von Daten aus diesen Protokollen zu beginnen. Führen Sie die folgenden Schritte aus, um die Protokollierung zu aktivieren:

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wenn Sie noch nicht über einen Load Balancer verfügen, [erstellen Sie einen Load Balancer](https://docs.microsoft.com/azure/load-balancer/quickstart-create-basic-load-balancer-portal) , bevor Sie fortfahren.

1. Klicken Sie im Portal auf **Ressourcengruppen**.
2. Wählen Sie **\<Ressourcengruppenname>** am Standort Ihres Load Balancers aus.
3. Wählen Sie Ihren Load Balancer aus.
4. Wählen Sie **Überwachung** > **Diagnoseeinstellungen** aus.
5. Wählen Sie im Bereich **Diagnoseeinstellungen** unter **Diagnoseeinstellungen**, den Eintrag **Diagnoseeinstellung hinzufügen** aus.
6. Geben Sie im Bereich zum Erstellen von **Diagnoseeinstellungen** im Feld **Name** den Wert **myLBDiagnostics** ein.
7. Sie haben die Wahl unter drei Optionen für die **Diagnoseeinstellungen**.  Sie können eine, zwei oder alle drei auswählen und jede nach Ihren Anforderungen konfigurieren:
   * **In einem Speicherkonto archivieren**
   * **An einen Event Hub streamen**
   * **An Log Analytics senden**

    ### <a name="archive-to-a-storage-account"></a>In einem Speicherkonto archivieren
    Für diesen Vorgang benötigen Sie ein bereits erstelltes Speicherkonto.  Informationen zum Erstellen eines Speicherkontos finden Sie unter [Erstellen eines Speicherkontos](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal).

    1. Aktivieren Sie das Kontrollkästchen neben **In einem Speicherkonto archivieren**.
    2. Wählen Sie **Konfigurieren** aus, um den Bereich **Speicherkonto auswählen** zu öffnen.
    3. Wählen Sie im Pulldownfeld das **Abonnement** aus, in dem Ihr Speicherkonto erstellt wurde.
    4. Wählen Sie im Pulldownfeld den Namen Ihres Speicherkontos unter **Speicherkonto** aus. 
    5. Wählen Sie „OK“ aus.

    ### <a name="stream-to-an-event-hub"></a>An einen Event Hub streamen
    Für diesen Vorgang benötigen Sie einen bereits erstellten Event Hub.  Informationen zum Erstellen eines Event Hubs finden Sie unter [Schnellstart: Erstellen eines Event Hubs mithilfe des Azure-Portals](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

    1. Aktivieren Sie das Kontrollkästchen neben **An einen Event Hub streamen**
    2. Wählen Sie **Konfigurieren** aus, um den Bereich **Event Hub auswählen** zu öffnen.
    3. Wählen Sie im Pulldownfeld das **Abonnement** aus, unter dem Ihr Event Hub erstellt wurde.
    4. Wählen Sie im Pulldownfeld **Event Hub-Namespace auswählen** aus.
    5. Wählen Sie im Pulldownfeld **Event Hub-Richtlinienname auswählen** aus.
    6. Wählen Sie „OK“ aus.

    ### <a name="send-to-log-analytics"></a>An Log Analytics senden
    Für diesen Prozess benötigen Sie einen bereits erstellten und konfigurierten Log Analytics-Arbeitsbereich.  Eine Anleitung zum Erstellen eines Log Analytics-Arbeitsbereichs finden Sie unter [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

    1. Aktivieren Sie das Kontrollkästchen neben **An Log Analytics senden**.
    2. Wählen Sie im Pulldownfeld das **Abonnement** aus, in dem sich Ihr Log Analytics-Arbeitsbereich befindet.
    3. Wählen Sie im Pulldownfeld den **Log Analytics-Arbeitsbereich** aus.


8. Aktivieren Sie im Abschnitt **PROTOKOLL** im Bereich **Diagnoseeinstellungen** das Kontrollkästchen neben diesen Einträgen:
   * **LoadBalancerAlertEvent**
   * **LoadBalancerProbeHealthStatus**

9.  Aktivieren Sie im Abschnitt **METRIK** im Bereich **Diagnoseeinstellungen** das Kontrollkästchen neben diesem Eintrag:
   * **AllMetrics**.

11. Überprüfen Sie, ob alles richtig aussieht, und klicken Sie oben im Bereich zum Erstellen der **Diagnoseeinstellungen** auf **Speichern**.

## <a name="activity-log"></a>Aktivitätsprotokoll

Das Aktivitätsprotokoll wird standardmäßig generiert. Die Protokolle werden 90 Tage lang im Azure-Ereignisprotokollspeicher aufbewahrt. Mehr zu diesen Protokollen können Sie im Artikel [Anzeigen von Aktivitätsprotokollen zur Überwachung von Aktionen in Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit) erfahren.

## <a name="archive-to-storage-account-logs"></a>In Speicherkontoprotokollen archivieren

### <a name="alert-event-log"></a>Warnereignisprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es für das jeweilige Lastenausgleichsmodul aktiviert haben. Die Ereignisse werden im JSON-Format protokolliert und in dem Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Das folgende Beispiel stellt ein Ereignis  dar.

```json
{
    "time": "2016-01-26T10:37:46.6024215Z",
    "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
    "category": "LoadBalancerAlertEvent",
    "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
    "operationName": "LoadBalancerProbeHealthStatus",
    "properties": {
        "eventName": "Resource Limits Hit",
        "eventDescription": "Ports exhausted",
        "eventProperties": {
            "public ip address": "40.117.227.32"
        }
    }
}
```

Die JSON-Ausgabe zeigt die *eventname* -Eigenschaft, die den Grund beschreibt, aus dem der Lastenausgleich eine Warnung generiert hat. In diesem Fall wurde die Warnung generiert, weil Quell-IP-NAT-Grenzwerte (SNAT) eine TCP-Portauslastung verursacht haben.

### <a name="health-probe-log"></a>Integritätstestprotokoll

Dieses Protokoll wird nur generiert, wenn Sie es wie oben beschrieben für das jeweilige Lastenausgleichsmodul aktiviert haben. Die Daten werden im Speicherkonto gespeichert, das Sie beim Aktivieren der Protokollierung angegeben haben. Ein Container namens „insights-logs-loadbalancerprobehealthstatus“ wird erstellt, und die folgenden Daten werden protokolliert:

```json
{
    "records":[
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 1,
            "healthPercentage": 50.000000
        }
    },
    {
        "time": "2016-01-26T10:37:46.6024215Z",
        "systemId": "32077926-b9c4-42fb-94c1-762e528b5b27",
        "category": "LoadBalancerProbeHealthStatus",
        "resourceId": "/SUBSCRIPTIONS/XXXXXXXXXXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXX/RESOURCEGROUPS/RG7/PROVIDERS/MICROSOFT.NETWORK/LOADBALANCERS/WWEBLB",
        "operationName": "LoadBalancerProbeHealthStatus",
        "properties": {
            "publicIpAddress": "40.83.190.158",
            "port": "81",
            "totalDipCount": 2,
            "dipDownCount": 0,
            "healthPercentage": 100.000000
        }
    }]
}
```

Die JSON-Ausgabe zeigt im Eigenschaftenfeld die grundlegenden Informationen zum Status des Integritätstests. Die *dipDownCount* -Eigenschaft zeigt die Gesamtzahl von Instanzen im Back-End, die aufgrund fehlerhafter Testantworten keinen Netzwerkdatenverkehr empfangen.

### <a name="view-and-analyze-the-audit-log"></a>Anzeigen und Analysieren des Überwachungsprotokolls

Mit einer der folgenden Methoden können Sie die Überwachungsprotokolldaten anzeigen und analysieren:

* **Azure-Tools:** Rufen Sie Informationen aus den Überwachungsprotokollen über Azure PowerShell, die Azure-Befehlszeilenschnittstelle, die Azure REST-API oder über das Azure-Portal ab. Schrittweise Anleitungen für die einzelnen Methoden finden Sie im Artikel [Überwachen von Vorgängen mit dem Ressourcen-Manager](../azure-resource-manager/resource-group-audit.md) .
* **Power BI:** Falls Sie noch kein [Power BI](https:// .microsoft.com/pricing)-Konto besitzen, können Sie es kostenlos testen. Mithilfe des [Azure Audit Logs Content Pack for Power BI](https:// .microsoft.com/documentation/ -content-pack-azure-audit-logs) können Sie Ihre Daten mit vorkonfigurierten Dashboards analysieren oder Ansichten an Ihre Anforderungen anpassen.

### <a name="view-and-analyze-the-health-probe-and-event-log"></a>Anzeigen und Analysieren des Integritätstest- und Ereignisprotokolls

Stellen Sie eine Verbindung mit Ihrem Speicherkonto her, und rufen Sie die JSON-Protokolleinträge für das Integritätstest- und das Ereignisprotokoll ab. Sobald Sie die JSON-Dateien heruntergeladen haben, können Sie diese in das CSV-Format konvertieren oder in Excel, PowerBI oder einem anderen Datenvisualisierungstool anzeigen.

> [!TIP]
> Wenn Sie mit Visual Studio und den grundlegenden Konzepten zum Ändern der Werte für Konstanten und Variablen in C# vertraut sind, können Sie die [Protokollkonvertierungstools](https://github.com/Azure-Samples/networking-dotnet-log-converter) von GitHub verwenden.

## <a name="stream-to-an-event-hub"></a>An einen Event Hub streamen
Wenn Diagnoseinformationen an einen Event Hub gestreamt werden, kann er für die zentrale Protokollanalyse in einem SIEM-Tool eines Drittanbieters mit Azure Monitor-Integration verwendet werden. Weitere Informationen finden Sie unter [Streamen von Azure-Überwachungsdaten an einen Event Hub](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs#tools-with-azure-monitor-integration)

## <a name="send-to-log-analytics"></a>An Log Analytics senden
Ressourcen in Azure können ihre Diagnoseinformationen direkt an einen Log Analytics-Arbeitsbereich senden lassen, in dem zwecks Problembehandlung und Analyse komplexe Abfragen für die Informationen ausgeführt werden können.  Weitere Informationen finden Sie unter [Erfassen von Azure-Ressourcenprotokollen im Log Analytics-Arbeitsbereich in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-workspace)

## <a name="next-steps"></a>Nächste Schritte

[Grundlegendes zu Load Balancer-Tests](load-balancer-custom-probe-overview.md)
