---
title: Konfigurieren von Kubernetes-Hybridclustern mit Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Azure Monitor für Container zum Überwachen von Kubernetes-Clustern konfigurieren können, die in Azure Stack oder einer anderen Umgebung gehostet werden.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 10/15/2019
ms.openlocfilehash: d25b9b3bb155dced973d415b396ebfaa4403b011
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510831"
---
# <a name="configure-hybrid-kubernetes-clusters-with-azure-monitor-for-containers"></a>Konfigurieren von Kubernetes-Hybridclustern mit Azure Monitor für Container

Azure Monitor für Container bietet umfassende Überwachungsfunktionen für die in Azure gehosteten Azure Kubernetes Service- (AKS) und AKS-Engine-Cluster. In diesem Artikel wird beschrieben, wie Sie die Überwachung von Kubernetes-Clustern, die außerhalb von Azure gehostet werden, aktivieren und eine ähnliche Überwachung erreichen können.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie zunächst sicher, dass Sie über Folgendes verfügen:

* Einen Log Analytics-Arbeitsbereich

    Azure Monitor für Container unterstützt einen Log Analytics-Arbeitsbereich in den unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor) aufgeführten Regionen in Azure. Ihren eigenen Arbeitsbereich können Sie über [Azure Resource Manager](../platform/template-workspace-configuration.md), über [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json) oder im [Azure-Portal](../learn/quick-create-workspace.md) erstellen.

    >[!NOTE]
    >Die Aktivierung der Überwachung mehrerer Cluster mit dem gleichen Clusternamen im gleichen Log Analytics-Arbeitsbereich ist nicht möglich. Clusternamen müssen eindeutig sein.
    >

* Sie müssen der **Rolle „Log Analytics-Mitwirkender“ angehören**, um die Containerüberwachung aktivieren zu können. Weitere Informationen zum Steuern des Zugriffs auf einen Log Analytics-Arbeitsbereich finden Sie unter [Verwalten des Zugriffs auf Arbeitsbereich und Logdaten](../platform/manage-access.md).

* [HELM-Client](https://helm.sh/docs/using_helm/) zum Integrieren des Azure Monitor für Containercharts für den angegebenen Kubernetes-Cluster

* Für die Kommunikation zwischen der Containerversion des Log Analytics-Agents für Linux und Azure Monitor sind die folgenden Proxy- und Firewallkonfigurationsinformationen erforderlich:

    |Agent-Ressource|Ports |
    |------|---------|   
    |*.ods.opinsights.azure.com |Port 443 |  
    |*.oms.opinsights.azure.com |Port 443 |  
    |*.blob.core.windows.net |Port 443 |  
    |*.dc.services.visualstudio.com |Port 443 | 

* Für den Container-Agent muss `cAdvisor port: 10255` auf allen Knoten im Cluster geöffnet werden, damit Leistungsmetriken erfasst werden können.

* Für den Container-Agent müssen die folgenden Umgebungsvariablen im Container angegeben werden, damit die Kommunikation mit dem Kubernetes-API-Dienst innerhalb des Clusters zum Erfassen von Bestandsdaten möglich ist: `KUBERNETES_SERVICE_HOST` und `KUBERNETES_PORT_443_TCP_PORT`. 

>[!IMPORTANT]
>Die für die Überwachung von Kubernetes-Hybridclustern unterstützte Agent-Mindestversion ist ciprod10182019 oder höher. 

## <a name="supported-configurations"></a>Unterstützte Konfigurationen

Folgende Konfigurationen werden offiziell für Azure Monitor für Container unterstützt.

- Umgebungen: Kubernetes lokal, AKS-Engine in Azure und Azure Stack. Weitere Informationen finden Sie unter [AKS-Engine in Azure Stack](https://docs.microsoft.com/azure-stack/user/azure-stack-kubernetes-aks-engine-overview?view=azs-1908).
- Die Versionen von Kubernetes und die Supportrichtlinie entsprechen den unterstützten Versionen von [AKS](../../aks/supported-kubernetes-versions.md). 
- Containerruntime: Docker und Moby
- Linux-Betriebssystemrelease für Master- und Workerknoten: Ubuntu (18.04 LTS und 16.04 LTS)
- Unterstützte Zugriffssteuerung: Kubernetes mit und ohne RBAC

## <a name="enable-monitoring"></a>Aktivieren der Überwachung

Die Aktivierung von Azure Monitor für Container für den Kubernetes-Hybridcluster umfasst die folgenden Schritte in der angegebenen Reihenfolge.

1. Konfigurieren Ihres Log Analytics-Arbeitsbereichs mit der Container Insights-Lösung

2. Aktivieren des HELM-Charts von Azure Monitor für Container über den Log Analytics-Arbeitsbereich

### <a name="how-to-add-the-azure-monitor-containers-solution"></a>Hinzufügen von Azure Monitor für Container

Sie können die Lösung mit der bereitgestellten Azure Resource Manager-Vorlage mithilfe des Azure PowerShell-Cmdlets `New-AzResourceGroupDeployment` oder mit der Azure-Befehlszeilenschnittstelle bereitstellen.

Wenn Sie mit der Bereitstellung von Ressourcen mithilfe einer Vorlage nicht vertraut sind, finden Sie weitere Informationen unter:

* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md)

* [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Wenn Sie die Azure CLI verwenden möchten, müssen Sie sie zuerst installieren und lokal verwenden. Sie benötigen Azure CLI 2.0.59 oder höher. Um Ihre Version zu ermitteln, führen Sie `az --version` aus. Informationen zur Installation und zum Upgrade von Azure CLI finden Sie unter [Installieren von Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 

Diese Methode umfasst zwei JSON-Vorlagen. Eine Vorlage gibt die Konfiguration zur Aktivierung der Überwachung an, während die andere Vorlage die zu konfigurierenden Parameterwerte enthält, mit denen Folgendes festgelegt wird:

- **workspaceResourceId:** die vollständige Ressourcen-ID Ihres Log Analytics Arbeitsbereichs
- **workspaceRegion:** die Region, in der der Arbeitsbereich erstellt wird. Beim Anzeigen über das Azure-Portal wird sie in den Arbeitsbereichseigenschaften auch als **Standort** bezeichnet.

Führen Sie die folgenden Schritte aus, um zunächst die für den Wert des `workspaceResourceId`-Parameters erforderliche vollständige Ressourcen-ID des Log Analytics-Arbeitsbereichs in der Datei **containerSolutionParams.json** zu ermitteln. Führen Sie anschließend das PowerShell-Cmdlet oder den Azure CLI-Befehl aus, um die Lösung hinzuzufügen.

1. Listen Sie mit dem folgenden Befehl alle Abonnements auf, auf die Sie Zugriff haben:

    ```azurecli
    az account list --all -o table
    ```

    Die Ausgabe ähnelt der folgenden:

    ```azurecli
    Name                                  CloudName    SubscriptionId                        State    IsDefault
    ------------------------------------  -----------  ------------------------------------  -------  -----------
    Microsoft Azure                       AzureCloud   68627f8c-91fO-4905-z48q-b032a81f8vy0  Enabled  True
    ```

    Kopieren Sie den Wert für **SubscriptionId**.

2. Wechseln Sie mit dem folgenden Befehl zu dem Abonnement, das den Log Analytics-Arbeitsbereich hostet:

    ```azurecli
    az account set -s <subscriptionId of the workspace>
    ```

3. Im folgenden Beispiel wird die Liste mit den Arbeitsbereichen Ihrer Abonnements im JSON-Standardformat angezeigt. 

    ```
    az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
    ```

    Suchen Sie in der Ausgabe nach dem Namen des Arbeitsbereichs, und kopieren Sie dann die vollständige Ressourcen-ID dieses Log Analytics-Arbeitsbereichs unter dem Feld **ID**.

4. Kopieren Sie die folgende JSON-Syntax, und fügen Sie sie in Ihre Datei ein:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceResourceId": {
            "type": "string",
            "metadata": {
                "description": "Azure Monitor Log Analytics Workspace Resource ID"
            }
        },
        "workspaceRegion": {
            "type": "string",
            "metadata": {
                "description": "Azure Monitor Log Analytics Workspace region"
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "name": "[Concat('ContainerInsights', '-',  uniqueString(parameters('workspaceResourceId')))]",
            "apiVersion": "2017-05-10",
            "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
            "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "apiVersion": "2015-11-01-preview",
                            "type": "Microsoft.OperationsManagement/solutions",
                            "location": "[parameters('workspaceRegion')]",
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "properties": {
                                "workspaceResourceId": "[parameters('workspaceResourceId')]"
                            },
                            "plan": {
                                "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                                "product": "[Concat('OMSGallery/', 'ContainerInsights')]",
                                "promotionCode": "",
                                "publisher": "Microsoft"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
         }
      ]
   }
    ```

5. Speichern Sie diese Datei als „containerSolution.json“ in einem lokalen Ordner.

6. Fügen Sie die folgende JSON-Syntax in Ihre Datei ein:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceResourceId": {
          "value": "<workspaceResourceId>"
      },
      "workspaceRegion": {
        "value": "<workspaceRegion>"
      }
     }
    }
    ```

7. Ändern Sie nach dem Ausführen des Azure CLI-Befehls [az monitor log-analytics workspace show](https://docs.microsoft.com/cli/azure/monitor/log-analytics/workspace?view=azure-cli-latest#az-monitor-log-analytics-workspace-list) die Werte für **workspaceResourceId** in den Wert, den Sie in Schritt 3 kopiert haben, und kopieren Sie für **workspaceRegion** den Wert **Region**.

8. Speichern Sie diese Datei als „containerSolutionParams.json“ in einem lokalen Ordner.

9. Nun können Sie die Vorlage bereitstellen. 

   * Verwenden Sie zum Bereitstellen mit Azure PowerShell die folgenden Befehle in dem Ordner mit der Vorlage:

       ```powershell
       # configure and login to the cloud of log analytics workspace.Specify the corresponding cloud environment of your workspace to below command.
       Connect-AzureRmAccount -Environment <AzureCloud | AzureChinaCloud | AzureUSGovernment>
       ```

       ```powershell
       # set the context of the subscription of Log Analytics workspace
       Set-AzureRmContext -SubscriptionId <subscription Id of log analytics workspace>
       ```
       
       ```powershell
       # execute deployment command to add container insights solution to the specified Log Analytics workspace
       New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <resource group of log analytics workspace> -TemplateFile .\containerSolution.json -TemplateParameterFile .\containerSolutionParams.json
       ```
       
       Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

       ```powershell
       provisioningState       : Succeeded
       ```

   * Führen Sie zum Bereitstellen mit Azure CLI die folgenden Befehle aus:
    
       ```azurecli
       az login
       az account set --name <AzureCloud | AzureChinaCloud | AzureUSGovernment>
       az login
       az account set --subscription "Subscription Name"
       # execute deployment command to add container insights solution to the specified Log Analytics workspace
       az group deployment create --resource-group <resource group of log analytics workspace> --template-file ./containerSolution.json --parameters @./containerSolutionParams.json
       ```

       Die Änderung der Konfiguration kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt:

       ```azurecli
       provisioningState       : Succeeded
       ```
     
       Nach dem Aktivieren der Überwachung kann es ca. 15 Minuten dauern, bis Integritätsmetriken für den Cluster angezeigt werden. 

## <a name="install-the-chart"></a>Installieren des Charts

Gehen Sie zum Aktivieren des HELM-Charts folgendermaßen vor:

1. Führen Sie folgenden Befehl aus, um das Repository der Azure-Charts in die lokale Liste einzufügen:

    ```
    helm repo add incubator https://kubernetes-charts-incubator.storage.googleapis.com/
    ````

2. Installieren Sie das Chart, indem Sie den folgenden Befehl ausführen:

    ```
    $ helm install --name myrelease-1 \
    --set omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<my_prod_cluster> incubator/azuremonitor-containers
    ```

    Wenn sich der Log Analytics-Arbeitsbereich in einer Azure-Region in China befindet, führen Sie den folgenden Befehl aus:

    ```
    $ helm install --name myrelease-1 \
     --set omsagent.domain=opinsights.azure.cn,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers 
    ```

    Wenn sich der Log Analytics-Arbeitsbereich in einer Azure-Region von „US Government“ befindet, führen Sie den folgenden Befehl aus:

    ```
    $ helm install --name myrelease-1 \
    --set omsagent.domain=opinsights.azure.us,omsagent.secret.wsid=<your_workspace_id>,omsagent.secret.key=<your_workspace_key>,omsagent.env.clusterName=<your_cluster_name> incubator/azuremonitor-containers
    ```

## <a name="configure-agent-data-collection"></a>Konfigurieren der Datensammlung des Agents

Ab Chartversion 1.0.0 werden die Einstellungen für die Datensammlung des Agents über die ConfigMap gesteuert. Die Dokumentation zu den Einstellungen für die Datensammlung des Agents finden Sie [hier](container-insights-agent-config.md). 

Nachdem Sie das Chart erfolgreich bereitgestellt haben, können Sie im Azure-Portal die Daten für den Kubernetes-Hybridcluster in Azure Monitor für Container überprüfen.  

>[!NOTE]
>Die Latenz bei der Erfassung liegt bei rund fünf bis zehn Minuten, vom Agent bis zum Commit im Azure Log Analytics-Arbeitsbereich. Als Status des Clusters wird der Wert **Keine Daten** oder **Unbekannt** angegeben, bis alle erforderlichen Überwachungsdaten in Azure Monitor verfügbar sind. 

## <a name="next-steps"></a>Nächste Schritte

Nach erfolgter Aktivierung der Überwachung zum Erfassen der Integrität und Ressourcennutzung des Kubernetes-Hybridclusters und der darin ausgeführten Workloads finden Sie [hier](container-insights-analyze.md) Informationen zur Verwendung von Azure Monitor für Container.
