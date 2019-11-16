---
title: Aktivieren von Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Azure Monitor für Container aktivieren und konfigurieren, damit Sie erfahren, wie gut die Leistung Ihrer Container ist und welche leistungsbezogenen Probleme erkannt wurden.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 11/11/2019
ms.openlocfilehash: f2a33f96f77678e02c5b72c36563781e7d7ac334
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73928276"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Aktivieren von Azure Monitor für Container

In diesem Artikel finden Sie eine Übersicht der verfügbaren Optionen für die Einrichtung von Azure Monitor für Container zum Überwachen der Leistung von Workloads, die in Kubernetes-Umgebungen bereitgestellt und von [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/), der AKS-Engine in [Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908) oder lokal bereitgestelltem Kubernetes gehostet werden.

Azure Monitor für Container kann für neue oder mindestens eine vorhandene Bereitstellung von AKS mit den folgenden unterstützten Methoden aktiviert werden:

* Über das Azure-Portal, Azure PowerShell oder mit der Azure-Befehlszeilenschnittstelle
* Verwenden von [Terraform und AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie über Folgendes verfügen:

* **Einen Log Analytics-Arbeitsbereich.**

    Azure Monitor für Container unterstützt einen Log Analytics-Arbeitsbereich in den unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor) aufgeführten Regionen in Azure.

    Sie können einen Arbeitsbereich beim Aktivieren der Überwachung des neuen AKS-Clusters erstellen oder beim Onboarding einen Standardarbeitsbereich in der Standardressourcengruppe des AKS-Clusterabonnements erstellen lassen. Wenn Sie ihn selbst erstellen möchten, können Sie dies über den [Azure Resource Manager](../platform/template-workspace-configuration.md), [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) oder das [Azure-Portal](../learn/quick-create-workspace.md) erledigen. Eine Liste der unterstützten Zuordnungspaare, die für den Standardarbeitsbereich verwendet werden, finden Sie unter [Von Azure Monitor für Container unterstützte Regionszuordnungen](container-insights-region-mapping.md).

* Sie müssen der **Rolle „Log Analytics-Mitwirkender“ angehören**, um die Containerüberwachung aktivieren zu können. Weitere Informationen zur Zugriffssteuerung auf einen Log Analytics-Arbeitsbereich finden Sie unter [Verwalten von Arbeitsbereichen](../platform/manage-access.md).

* Sie sind ein Mitglied der **[Besitzer](../../role-based-access-control/built-in-roles.md#owner)** -Rolle für die AKS-Clusterressource.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

* Prometheus-Metriken werden standardmäßig nicht gesammelt. Bevor Sie [den Agent konfigurieren](container-insights-prometheus-integration.md), um sie zu erfassen, sollten Sie sich intensiv mit der [Prometheus-Dokumentation](https://prometheus.io/) befassen, um zu verstehen, was Sie definieren können.

## <a name="supported-configurations"></a>Unterstützte Konfigurationen

Folgende Konfigurationen werden offiziell für Azure Monitor für Container unterstützt.

- Umgebungen: Kubernetes lokal, AKS-Engine in Azure und Azure Stack. Weitere Informationen finden Sie unter [AKS-Engine in Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908).
- Die Versionen von Kubernetes und die Supportrichtlinie entsprechen den unterstützten Versionen von [AKS](../../aks/supported-kubernetes-versions.md). 

## <a name="network-firewall-requirements"></a>Netzwerkfirewallanforderungen

In der folgenden Tabelle sind die Proxy- und Firewall-Konfigurationsinformationen aufgelistet, die für den Container-Agent zur Kommunikation mit Azure Monitor für Container benötigt werden. Der gesamte Netzwerkdatenverkehr vom Agent ist ausgehender Datenverkehr an Azure Monitor.

|Agent-Ressource|Ports |
|--------------|------|
| *.ods.opinsights.azure.com | 443 |  
| *.oms.opinsights.azure.com | 443 | 
| *.blob.core.windows.net | 443 |
| dc.services.visualstudio.com | 443 |
| *.microsoftonline.com | 443 |
| *.monitoring.azure.com | 443 |
| login.microsoftonline.com | 443 |

In der folgenden Tabelle sind die Proxy- und Firewall-Konfigurationsinformationen für Azure China aufgelistet.

|Agent-Ressource|Ports |BESCHREIBUNG | 
|--------------|------|-------------|
| *.ods.opinsights.azure.cn | 443 | Datenerfassung |
| *.oms.opinsights.azure.cn | 443 | OMS-Onboarding |
| *.blob.core.windows.net | 443 | Wird zum Überwachen der ausgehenden Konnektivität verwendet. |
| microsoft.com | 80 | Wird für Netzwerkkonnektivität verwendet. Dies ist nur erforderlich, wenn es sich bei der Version des Agent-Images um ciprod09262019 oder eine frühere Version handelt. |
| dc.services.visualstudio.com | 443 | Für Agent-Telemetrie mithilfe von Application Insights in der öffentlichen Azure-Cloud. |

In der folgenden Tabelle sind die Proxy- und Firewall-Konfigurationsinformationen für Azure US Government aufgelistet.

|Agent-Ressource|Ports |BESCHREIBUNG | 
|--------------|------|-------------|
| *.ods.opinsights.azure.us | 443 | Datenerfassung |
| *.oms.opinsights.azure.us | 443 | OMS-Onboarding |
| *.blob.core.windows.net | 443 | Wird zum Überwachen der ausgehenden Konnektivität verwendet. |
| microsoft.com | 80 | Wird für Netzwerkkonnektivität verwendet. Dies ist nur erforderlich, wenn es sich bei der Version des Agent-Images um ciprod09262019 oder eine frühere Version handelt. |
| dc.services.visualstudio.com | 443 | Für Agent-Telemetrie mithilfe von Application Insights in der öffentlichen Azure-Cloud. |

## <a name="components"></a>Komponenten

Ihre Möglichkeit zum Überwachen der Leistung hängt von einem containerbasierten Log Analytics-Agent für Linux ab, der speziell für Azure Monitor für Container entwickelt wurde. Dieser spezialisierte Agent sammelt Leistungs- und Ereignisdaten von allen Knoten im Cluster, und der Agent wird während der Bereitstellung automatisch bereitgestellt und mit dem angegebenen Log Analytics-Arbeitsbereich registriert. Dabei wird die Agent-Version „microsoft/oms:ciprod04202018“ oder eine höhere Version bereitgestellt (dargestellt im Format *mmttjjjj*).

>[!NOTE]
>In der Vorschauversion der Windows Server-Unterstützung für AKS ist für einen AKS-Cluster mit Windows Server-Knoten kein Agent installiert, um Daten zu erfassen und an Azure Monitor weiterzuleiten. Stattdessen erfasst ein Linux-Knoten, der als Teil der Standardbereitstellung automatisch im Cluster bereitgestellt wird, die Daten, und leitet diese für alle Windows-Knoten im Cluster an Azure Monitor weiter.  
>

Wenn eine neue Version des Agents veröffentlicht wird, wird er in Ihren Managed Kubernetes-Clustern, die unter Azure Kubernetes Service (AKS) gehostet werden, automatisch aktualisiert. Informationen zu den freigegebenen Versionen finden Sie unter [Agent-Versionsankündigungen](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).

>[!NOTE]
>Wenn Sie bereits einen AKS-Cluster bereitgestellt haben, können Sie die Überwachung mithilfe der Azure-Befehlszeilenschnittstelle oder einer bereitgestellten Azure Resource Manager-Vorlage aktivieren (siehe weiter unten in diesem Artikel). Sie können mit `kubectl` kein Upgrade für den Agent durchführen oder diesen löschen, bereitstellen oder erneut bereitstellen.
>Die Vorlage muss in derselben Ressourcengruppe wie der Cluster bereitgestellt werden.

Indem Sie eine der Methoden verwenden, die in der folgenden Tabelle beschrieben sind, aktivieren Sie Azure Monitor für Container.

| Bereitstellungszustand | Methode | BESCHREIBUNG |
|------------------|--------|-------------|
| Neuer AKS-Cluster | [Erstellen eines Clusters mithilfe der Azure CLI](../../aks/kubernetes-walkthrough.md#create-aks-cluster)| Sie können die Überwachung eines neuen AKS-Clusters aktivieren, den Sie mit der Azure CLI erstellen. |
| | [Erstellen eines Clusters mithilfe von Terraform](container-insights-enable-new-cluster.md#enable-using-terraform)| Sie können die Überwachung eines neuen AKS-Clusters aktivieren, den Sie mithilfe des Open-Source-Tools Terraform erstellen. |
| Vorhandener AKS-Cluster | [Aktivieren mithilfe der Azure CLI](container-insights-enable-existing-clusters.md#enable-using-azure-cli) | Sie können die Überwachung eines bereits bereitgestellten AKS-Clusters mithilfe der Azure-Befehlszeilenschnittstelle (CLI) aktivieren. |
| |[Aktivieren mithilfe von Terraform](container-insights-enable-existing-clusters.md#enable-using-terraform) | Sie können die Überwachung eines bereits bereitgestellten AKS-Clusters mithilfe des Open-Source-Tools Terraform aktivieren. |
| | [Aktivieren über Azure Monitor](container-insights-enable-existing-clusters.md#enable-from-azure-monitor-in-the-portal)| Sie können die Überwachung eines oder mehrerer bereits bereitgestellten AKS-Cluster über die AKS-Multi-Cluster-Seite in Azure Monitor aktivieren. |
| | [Aktivieren aus einem AKS-Cluster](container-insights-enable-existing-clusters.md#enable-directly-from-aks-cluster-in-the-portal)| Sie können die Überwachung direkt aus einem AKS-Cluster im Azure-Portal aktivieren. |
| | [Aktivieren mithilfe einer Azure Resource Manager-Vorlage](container-insights-enable-existing-clusters.md#enable-using-an-azure-resource-manager-template)| Sie können die Überwachung eines AKS-Clusters mit einer vorkonfigurierten Azure Resource Manager-Vorlage aktivieren. |
| | [Aktivieren für Kubernetes-Hybridcluster](container-insights-hybrid-setup.md) | Sie können die Überwachung einer AKS-Engine, die in Azure Stack gehostet wird, oder für lokal gehostetes Kubernetes aktivieren. |

## <a name="next-steps"></a>Nächste Schritte

* Bei aktivierter Überwachung zum Erfassen der Integritätsmetriken für AKS-Clusterknoten wie auch für Pods stehen diese Integritätsmetriken im Azure-Portal zur Verfügung. Informationen zum Erlernen der Verwendung von Azure Monitor für Container finden Sie unter [Anzeigen der Integrität von Azure Kubernetes Service](container-insights-analyze.md).
