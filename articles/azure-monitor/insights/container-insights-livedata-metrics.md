---
title: Anzeigen von Metriken in Echtzeit mit Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird die Echtzeitansicht von Metriken ohne Verwendung von kubectl mit Azure Monitor für Container beschrieben.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/15/2019
ms.author: magoedte
ms.openlocfilehash: 4608058d347888ab77bd4303600f00b270d6fed5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510827"
---
# <a name="how-to-view-metrics-in-real-time"></a>Anzeigen von Metriken in Echtzeit

Mit der Funktion für Livedaten (Vorschau) in Azure Monitor für Container können Sie Metriken zum Knoten- und Podstatus in einem Cluster in Echtzeit visualisieren. Hierbei wird der direkte Zugriff auf die Befehle `kubectl top nodes`, `kubectl get pods –all-namespaces` und `kubectl get nodes` emuliert, um die Daten abzurufen, zu analysieren und in Leistungsdiagrammen zu visualisieren, die in diesen Einblicken enthalten sind. 

Dieser Artikel bietet eine ausführliche Übersicht und hilft Ihnen, die Verwendung dieser Funktion zu verstehen.  

>[!NOTE]
>Als [private Cluster](https://azure.microsoft.com/updates/aks-private-cluster/) aktivierte AKS-Cluster werden mit dieser Funktion nicht unterstützt. Diese Funktion basiert auf dem direkten Zugriff auf die Kubernetes-API über einen Proxyserver im Browser. Durch die Aktivierung der Netzwerksicherheit zum Blockieren der Kubernetes-API über den Proxy wird der Datenverkehr blockiert. 

>[!NOTE]
>Diese Funktion steht in allen Azure-Regionen zur Verfügung, einschließlich Azure China. In Azure US Government ist sie derzeit nicht verfügbar.

Hilfe bei der Einrichtung oder Problembehandlung der Funktion für Livedaten (Vorschau) finden Sie in unserem [Einrichtungsleitfaden](container-insights-livedata-setup.md).

## <a name="how-it-works"></a>So funktioniert's 

Informationen zum Direktzugriff auf die Kubernetes-API über die Funktion für Livedaten (Vorschau) sowie weitere Informationen zum Authentifizierungsmodell finden Sie [hier](https://kubernetes.io/docs/concepts/overview/kubernetes-api/). 

Diese Funktion führt standardmäßig alle fünf Sekunden einen Abrufvorgang für die Metrikendpunkte (einschließlich `/api/v1/nodes`, `/apis/metrics.k8s.io/v1beta1/nodes`und `/api/v1/pods`) aus. Diese Daten werden in Ihrem Browser zwischengespeichert und in den vier Leistungsdiagrammen dargestellt, die in Azure Monitor für Container bei Auswahl von **Live schalten (Vorschau)** auf der Registerkarte **Cluster** enthalten sind. Jeder nachfolgende Abruf wird in einem rollierenden Fünf-Minuten-Visualisierungsfenster dargestellt. 

![Option „Live schalten“ in der Ansicht „Cluster“](./media/container-insights-livedata-metrics/cluster-view-go-live-example-01.png)

Das Abrufintervall wird mithilfe des Dropdownmenüs **Intervall festlegen** konfiguriert, in dem Sie den Abruf neuer Daten auf alle 1, 5, 15 oder 30 Sekunden festlegen können. 

![Dropdownmenü für Abrufintervall unter „Live schalten“](./media/container-insights-livedata-metrics/cluster-view-polling-interval-dropdown.ping.png)

>[!IMPORTANT]
>Es wird empfohlen, das Abrufintervall bei der Problembehandlung für kurze Zeit auf eine Sekunde festzulegen. Diese Anforderungen können sich auf die Verfügbarkeit und Drosselung der Kubernetes-API in Ihrem Cluster auswirken. Anschließend legen Sie wieder ein längeres Abrufintervall fest. 

>[!IMPORTANT]
>Während der Ausführung dieser Funktion werden keine Daten dauerhaft gespeichert. Alle während dieser Sitzung erfassten Informationen werden sofort gelöscht, wenn Sie den Browser schließen oder die Funktion verlassen. Daten stehen nur innerhalb des Fünf-Minuten-Zeitfensters zur Visualisierung bereit. Alle Metriken, die älter als fünf Minuten sind, werden ebenfalls dauerhaft gelöscht.

Diese Diagramme können nicht an das letzte Azure-Dashboard angeheftet werden, das Sie im Livemodus angezeigt haben.

## <a name="metrics-captured"></a>Erfasste Metriken

### <a name="node-cpu-utilization---node-memory-utilization-"></a>Knoten-CPU-Auslastung in % / Knotenspeicherauslastung in % 

Diese beiden Leistungsdiagramme bilden eine Entsprechung des Aufrufs von `kubectl top nodes` ab. Dabei werden die Ergebnisse der Spalten **CPU%** und **MEMORY%** im jeweiligen Diagramm erfasst. 

![Beispielergebnisse für „kubectl top nodes“](./media/container-insights-livedata-metrics/kubectl-top-nodes-example.png)

![Diagramm zum Prozentsatz der Knoten-CPU-Auslastung](./media/container-insights-livedata-metrics/cluster-view-node-cpu-util.png)

![Diagramm zum Prozentsatz der Knotenspeicherauslastung](./media/container-insights-livedata-metrics/cluster-view-node-memory-util.png)

Die Perzentilberechnungen funktionieren bei größeren Clustern, um Ausreißerknoten im Cluster zu erkennen. Hiermit kann beispielsweise zum Zweck des zentralen Herunterskalierens festgestellt werden, ob Knoten nicht genügend ausgelastet sind. Mithilfe der **Min**-Aggregation können Sie erkennen, welche Knoten eine geringe Auslastung im Cluster aufweisen. Zur weiteren Untersuchung wählen Sie die Registerkarte **Knoten** aus und sortieren das Raster nach CPU- oder Arbeitsspeicherauslastung.

Dies hilft ebenfalls zu verstehen, welche Knoten nahe den Grenzwerten liegen und ob eine horizontale Skalierung erforderlich ist. Mithilfe der **Max**- und **P95**-Aggregationen können Sie feststellen, ob im Cluster Knoten mit hoher Ressourcenauslastung vorhanden sind. Zur weiteren Untersuchung wechseln Sie wieder zur Registerkarte **Knoten**.

### <a name="node-count"></a>Knotenanzahl

Dieses Leistungsdiagramm bildet eine Entsprechung des Aufrufs von `kubectl get nodes` ab. Dabei wird die Spalte **STATUS** einem Diagramm gruppiert nach Statustypen zugeordnet.

![Beispielergebnisse für „kubectl get nodes“](./media/container-insights-livedata-metrics/kubectl-get-nodes-example.png)

![Diagramm zur Knotenanzahl](./media/container-insights-livedata-metrics/cluster-view-node-count-01.png)

Knoten werden entweder im Zustand **Bereit** oder **Nicht bereit** angezeigt. Sie werden gezählt (und es wird eine Gesamtzahl erstellt), und die Ergebnisse dieser beiden Aggregationen werden in einem Diagramm dargestellt.
Hiermit kann beispielsweise festgestellt werden, ob Ihre Knoten fehlerhafte Zustände aufweisen. Mithilfe der **Nicht bereit**-Aggregation können Sie schnell die Anzahl der Knoten im Cluster sehen, die sich derzeit im Zustand **Nicht bereit** befinden.

### <a name="active-pod-count"></a>Anzahl aktiver Pods

Dieses Leistungsdiagramm bildet eine Entsprechung des Aufrufs von `kubectl get pods –all-namespaces` ab. Dabei wird die Spalte **STATUS** dem Diagramm gruppiert nach Statustypen zugeordnet.

![Beispielergebnisse für „kubectl get pods“](./media/container-insights-livedata-metrics/kubectl-get-pods-example.png)

![Diagramm zur Knotenpodanzahl](./media/container-insights-livedata-metrics/cluster-view-node-pod-count.png)

>[!NOTE]
>Statusnamen, die von `kubectl` interpretiert werden, stimmen im Diagramm möglicherweise nicht exakt überein. 

## <a name="next-steps"></a>Nächste Schritte

Unter [Beispielabfragen für die Protokollsuche](container-insights-log-search.md#search-logs-to-analyze-data) finden Sie vordefinierte Abfragen und Beispiele zum Erstellen von Warnungen und Visualisierungen. Darüber hinaus können Sie eine weitergehende Analyse des Clusters durchführen.