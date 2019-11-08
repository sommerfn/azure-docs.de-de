---
title: Anzeigen von Bereitstellungen in Azure Monitor für Container (Vorschau) | Microsoft-Dokumentation
description: In diesem Artikel wird die Echtzeitansicht von Kubernetes-Bereitstellungen in Azure Monitor für Container ohne Verwendung von kubectl beschrieben.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: magoedte
ms.openlocfilehash: c7135aec98936b5247b39ae659e21735168bd289
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510759"
---
# <a name="how-to-view-deployments-preview-in-real-time"></a>Anzeigen von Bereitstellungen (Vorschau) in Echtzeit

Mit Azure Monitor für Container emuliert die Funktion „Bereitstellungen anzeigen“ (Vorschau) den direkten Zugriff auf Kubernetes-Bereitstellungsobjekte in Echtzeit, indem die Befehle `kubeclt get deployments` und `kubectl describe deployment {your deployment}` verfügbar gemacht werden. 

>[!NOTE]
>Als [private Cluster](https://azure.microsoft.com/updates/aks-private-cluster/) aktivierte AKS-Cluster werden bei dieser Funktion nicht unterstützt. Diese Funktion basiert auf dem direkten Zugriff auf die Kubernetes-API über einen Proxyserver im Browser. Durch die Aktivierung der Netzwerksicherheit zum Blockieren der Kubernetes-API über den Proxy wird der Datenverkehr blockiert. 

>[!NOTE]
>Diese Funktion steht in allen Azure-Regionen zur Verfügung, einschließlich Azure China. In Azure US Government ist sie derzeit nicht verfügbar.

Weitere Informationen finden Sie in der Kubernetes-Dokumentation zu [Bereitstellungen](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/). 

## <a name="how-it-works"></a>So funktioniert's

Informationen zum Direktzugriff auf die Kubernetes-API über die Funktion für Livedaten (Vorschau) sowie weitere Informationen zum Authentifizierungsmodell finden Sie [hier](https://kubernetes.io/docs/concepts/overview/kubernetes-api/). 

Mit der Funktion „Bereitstellungen“ (Vorschau) wird der Bereitstellungsendpunkt `/apis/apps/v1/deployments` einmalig (aktualisierbar) geladen. So können Sie eine bestimmte Bereitstellung auswählen und die Beschreibung für diese spezifische Bereitstellung auf dem Bereitstellungsendpunkt `/apis/apps/v1/namespaces/${nameSpace}/deployments/${deploymentName}` laden. 

Durch Auswählen von **Aktualisieren** links oben auf der Seite wird die Liste der Bereitstellungen aktualisiert. Dadurch wird die erneute Ausführung des Befehls `kubectl` simuliert. 

>[!IMPORTANT]
>Während der Ausführung dieser Funktion werden keine Daten dauerhaft gespeichert. Alle während der Sitzung erfassten Informationen werden gelöscht, wenn Sie den Browser schließen oder verlassen.  

>[!NOTE]
>Livedaten (Vorschau) können nicht über die Konsole auf einem Azure-Dashboard angeheftet werden.

## <a name="deployments-describe"></a>Beschreibung von Bereitstellungen

Führen Sie die folgenden Schritte aus, um Beschreibungsdetails zu einer Bereitstellung anzuzeigen. Dies entspricht `kubectl describe deployment`.

1. Navigieren Sie im Azure-Portal zur Ressourcengruppe des AKS-Clusters, und wählen Sie die AKS-Ressource aus.

2. Wählen Sie im Dashboard des AKS-Clusters links unter **Überwachung** die Option **Erkenntnisse** aus. 

3. Wählen Sie die Registerkarte **Bereitstellungen (Vorschau)** aus.

    ![Ansicht „Bereitstellungen“ im Azure-Portal](./media/container-insights-livedata-deployments/deployment-view.png)

In der Ansicht werden eine Liste aller ausgeführten Bereitstellungen sowie der Namespace und weitere ausführliche Informationen angezeigt, wobei die Ausführung des Befehls `kubectl get deployments –all-namespaces` emuliert wird. Sie können die Ergebnisse sortieren, indem Sie eine der Spalten auswählen. 

![Details im Eigenschaftenbereich für Bereitstellungen](./media/container-insights-livedata-deployments/deployment-properties-pane-details.png)

Wenn Sie in der Liste eine Bereitstellung auswählen, wird rechts auf der Seite automatisch ein Eigenschaftenbereich angezeigt. Dort sind Informationen zur ausgewählten Bereitstellung aufgeführt, die auch angezeigt werden, wenn Sie den Befehl `kubectl describe deployment {deploymentName}` ausführen. Möglicherweise haben Sie bemerkt, dass in den Beschreibungsinformationen einige Details fehlen. Insbesondere die **Vorlage** fehlt. Durch Auswählen der Registerkarte **Raw** können Sie zu den nicht analysierten Beschreibungsdetails navigieren.  

![Details unter „Raw“ im Eigenschaftenbereich für Bereitstellungen](./media/container-insights-livedata-deployments/deployment-properties-pane-raw.png)

Beim Überprüfen der Bereitstellungsdetails können Sie Containerprotokolle und -ereignisse in Echtzeit sehen. Wenn Sie **Livekonsole anzeigen** auswählen, wird der Konsolenbereich für Livedaten (Vorschau) unterhalb der Datentabelle für Bereitstellungen angezeigt. Hier können Sie Liveprotokolldaten in einem kontinuierlichen Stream sehen. Wenn in der Abrufstatusanzeige ein grünes Häkchen angezeigt wird, das sich ganz rechts im Bereich befindet, bedeutet das, dass Daten abgerufen werden können und in die Konsole gestreamt werden.

Sie können außerdem nach dem Namespace oder nach Ereignissen auf Clusterebene filtern. Weitere Informationen zum Anzeigen von Daten in Echtzeit in der Konsole finden Sie unter [Anzeigen von Livedaten (Vorschau) mit Azure Monitor für Container](container-insights-livedata-overview.md). 

![Livedaten in der Ansicht „Bereitstellungen“ in der Konsole](./media/container-insights-livedata-deployments/deployments-console-view-events.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Verwendung von Azure Monitor und zum Überwachen anderer Aspekte Ihres AKS-Clusters finden Sie unter [Anzeigen der Azure Kubernetes Service-Integrität](container-insights-analyze.md).

- Unter [Beispielabfragen für die Protokollsuche](container-insights-log-search.md#search-logs-to-analyze-data) finden Sie vordefinierte Abfragen und Beispiele zum Erstellen von Warnungen und Visualisierungen. Darüber hinaus können Sie eine weitergehende Analyse des Clusters durchführen.