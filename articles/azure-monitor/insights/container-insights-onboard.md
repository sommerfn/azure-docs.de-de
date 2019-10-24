---
title: Aktivieren von Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Azure Monitor für Container aktivieren und konfigurieren, damit Sie erfahren, wie gut die Leistung Ihrer Container ist und welche leistungsbezogenen Probleme erkannt wurden.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 07/12/2019
ms.openlocfilehash: 44cdc2d6b93ac9a62f96875ca6c679fbb97d85a9
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555401"
---
# <a name="how-to-enable-azure-monitor-for-containers"></a>Aktivieren von Azure Monitor für Container

In diesem Artikel finden Sie eine Übersicht der verfügbaren Optionen für die Einrichtung von Azure Monitor für Container zum Überwachen der Leistung von Workloads, die in Kubernetes-Umgebungen bereitgestellt und von [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/) gehostet werden.

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

* Prometheus-Metriken werden standardmäßig nicht gesammelt. Bevor Sie [den Agent konfigurieren](container-insights-agent-config.md), um sie zu erfassen, sollten Sie sich intensiv mit der [Prometheus-Dokumentation](https://prometheus.io/) befassen, um zu verstehen, was Sie definieren können.

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

## <a name="next-steps"></a>Nächste Schritte

* Bei aktivierter Überwachung zum Erfassen der Integritätsmetriken für AKS-Clusterknoten wie auch für Pods stehen diese Integritätsmetriken im Azure-Portal zur Verfügung. Informationen zum Erlernen der Verwendung von Azure Monitor für Container finden Sie unter [Anzeigen der Integrität von Azure Kubernetes Service](container-insights-analyze.md).
