---
title: Bereitstellen von Modellen in Azure Kubernetes Service
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Ihre Azure Machine Learning-Modelle mithilfe von Azure Kubernetes Service als Webdienst bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 11/06/2019
ms.openlocfilehash: 9055223d1e4ed056ad606533219925972b623f86
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73682116"
---
# <a name="deploy-a-model-to-an-azure-kubernetes-service-cluster"></a>Bereitstellen eines Modells in einem Azure Kubernetes Service-Cluster
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Erfahren Sie, wie Sie ein Modell mit Azure Machine Learning als Webdienst in Azure Kubernetes Service (AKS) bereitstellen. Azure Kubernetes Service ist gut für umfangreiche Produktionsbereitstellungen geeignet. Verwenden Sie Azure Kubernetes Service, wenn Sie eine oder mehrere der folgenden Funktionen benötigen:

- __Schnelle Antwortzeiten__
- __Autoskalierung__ des bereitgestellten Diensts
- Optionen für die __Hardwarebeschleunigung__, beispielsweise GPU und FPGA (Field-Programmable Gate Arrays)

> [!IMPORTANT]
> Das Azure Machine Learning SDK bietet keine Clusterskalierung. Weitere Informationen zum Skalieren der Knoten in einem AKS-Cluster finden Sie unter [Skalieren der Knotenanzahl in einem AKS-Cluster](../../aks/scale-cluster.md).

Bei der Bereitstellung in Azure Kubernetes Service führen Sie die Bereitstellung in einem AKS-Cluster durch, der __mit Ihrem Arbeitsbereich verbunden ist__. Es gibt zwei Möglichkeiten, einen AKS-Cluster mit Ihrem Arbeitsbereich zu verbinden:

* Erstellen Sie den AKS-Cluster mit dem Azure Machine Learning SDK, der Machine Learning CLI oder [Azure Machine Learning Studio](https://ml.azure.com). Bei diesem Vorgang wird der Cluster mit dem Arbeitsbereich verbunden.
* Fügen Sie einen vorhandenen AKS-Cluster an Ihren Azure Machine Learning-Arbeitsbereich an. Ein Cluster kann mit dem Azure Machine Learning SDK, der Machine Learning CLI oder Azure Machine Learning Studio angefügt werden.

> [!IMPORTANT]
> Die Erstellung bzw. der Anfügevorgang muss nur einmal durchgeführt werden. Nachdem ein AKS-Cluster mit dem Arbeitsbereich verbunden wurde, können sie ihn für Bereitstellungen einsetzen. Wenn Sie den AKS-Cluster nicht mehr benötigen, können Sie ihn trennen oder löschen. Nach dem Trennen oder Löschen ist eine Bereitstellung im Cluster nicht mehr möglich.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Machine Learning-Arbeitsbereich. Weitere Informationen finden Sie unter [Erstellen eines Azure Machine Learning-Arbeitsbereichs](how-to-manage-workspace.md).

- Ein Machine Learning-Modell, das in Ihrem Arbeitsbereich registriert ist. Wenn Sie über kein registriertes Modell verfügen, finden Sie hier weitere Informationen: [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).

- Die [Azure CLI-Erweiterung für Machine Learning Service](reference-azure-machine-learning-cli.md), das [Azure Machine Learning Python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) oder die [Visual Studio Code-Erweiterung für Azure Machine Learning](how-to-vscode-tools.md).

- Bei den in diesem Artikel verwendeten __Python__-Codeausschnitten wird davon ausgegangen, dass die folgenden Variablen festgelegt sind:

    * `ws`: Legen Sie diese Variable auf Ihren Arbeitsbereich fest.
    * `model`: Legen Sie diese Variable auf Ihr registriertes Modell fest.
    * `inference_config`: Legen Sie diese Variable auf die Rückschlusskonfiguration für das Modell fest.

    Weitere Informationen zum Festlegen dieser Variablen finden Sie unter [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).

- Bei den in diesem Artikel verwendeten __CLI__-Ausschnitten wird davon ausgegangen, dass Sie ein `inferenceconfig.json`-Dokument erstellt haben. Weitere Informationen zum Erstellen dieses Dokuments finden Sie unter [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).

## <a name="create-a-new-aks-cluster"></a>Erstellen eines neuen AKS-Clusters

**Geschätzter Zeitaufwand**: Ca. 20 Minuten.

Das Erstellen oder Anfügen eines AKS-Clusters ist ein für Ihren Arbeitsbereich einmaliger Vorgang. Sie können diesen Cluster für mehrere Bereitstellungen wiederverwenden. Wenn Sie den Cluster oder die Ressourcengruppe löschen, die ihn enthält, müssen Sie bei der nächsten Bereitstellung einen neuen Cluster erstellen. Sie können an Ihren Arbeitsbereich mehrere AKS-Cluster anfügen.

> [!TIP]
> Wenn Sie Ihren AKS-Cluster mithilfe einer Azure Virtual Network-Instanz schützen möchten, müssen Sie zuerst das virtuelle Netzwerk erstellen. Weitere Informationen finden Sie unter [Sichern von Azure ML-Experiment- und Rückschlussaufträgen in einem virtuellen Azure-Netzwerk](how-to-enable-virtual-network.md#aksvnet).

Wenn Sie einen AKS-Cluster nicht für die Produktion, sondern für die __Entwicklung__, zur __Validierung__ und für __Tests__ erstellen möchten, können Sie den __Clusterzweck__ auf __Dev/Test__ festlegen.

> [!WARNING]
> Wenn Sie `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` festlegen, ist der Cluster, der erstellt wird, nicht für Produktionsdatenverkehr geeignet und kann Rückschlusszeiten erhöhen. Entwicklungs- und Testcluster garantieren auch keine Fehlertoleranz. Wir empfehlen mindestens zwei virtuelle CPUs für Entwicklungs-/Testcluster.

Die folgenden Beispiele veranschaulichen, wie ein neuer AKS-Cluster mit SDK und CLI erstellt wird:

**Verwenden des SDK**

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

> [!IMPORTANT]
> Für [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py) müssen Sie, wenn Sie benutzerdefinierte Werte für `agent_count` und `vm_size` auswählen, und wenn `cluster_purpose` nicht gleich `DEV_TEST` ist, sicherstellen, dass `agent_count` multipliziert mit `vm_size` größer als oder gleich 12 virtuellen CPUs ist. Wenn Sie beispielsweise für `vm_size` „Standard_D3_v2“ verwenden, das 4 virtuelle CPUs hat, müssen Sie für `agent_count` einen Wert von mindestens 3 wählen.
>
> Das Azure Machine Learning-SDK bietet keine Unterstützung zur Skalierung eines AKS-Clusters. Die Knoten in Ihrem Cluster können Sie über die Benutzeroberfläche Ihres AKS-Clusters in Azure Machine Learning Studio skalieren. Sie können nur die Knotenanzahl ändern, nicht die VM-Größe des Clusters.

Weitere Informationen zu den in diesem Beispiel verwendeten Klassen, Methoden und Parametern finden Sie in den folgenden Referenzdokumenten:

* [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py)
* [AksCompute.provisioning_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [ComputeTarget.create](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#create-workspace--name--provisioning-configuration-)
* [ComputeTarget.wait_for_completion](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#wait-for-completion-show-output-false-)

**Verwenden der CLI**

```azurecli
az ml computetarget create aks -n myaks
```

Weitere Informationen finden Sie unter [az ml computetarget create ask](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-aks).

## <a name="attach-an-existing-aks-cluster"></a>Anfügen eines vorhandenen AKS-Clusters

**Geschätzter Zeitaufwand:** Ca. fünf Minuten.

Wenn Sie in Ihrem Azure-Abonnement bereits über einen AKS-Cluster verfügen und dieser eine Version unterhalb von 1.14.hat, können Sie diesen für die Bereitstellung Ihres Image verwenden.

> [!TIP]
> Der vorhandenen AKS-Cluster kann sich in einer anderen Azure-Region befinden als Ihr Azure Machine Learning-Arbeitsbereich.
>
> Wenn Sie Ihren AKS-Cluster mithilfe einer Azure Virtual Network-Instanz schützen möchten, müssen Sie zuerst das virtuelle Netzwerk erstellen. Weitere Informationen finden Sie unter [Sichern von Azure ML-Experiment- und Rückschlussaufträgen in einem virtuellen Azure-Netzwerk](how-to-enable-virtual-network.md#aksvnet).

Wenn Sie einen AKS-Cluster an einen Arbeitsbereich anfügen, können Sie durch Festlegen des Parameters `cluster_purpose` bestimmen, wie Sie den Cluster verwenden möchten.

Wenn Sie den Parameter `cluster_purpose` nicht bzw. `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD` festlegen, muss der Cluster über mindestens 12 virtuelle CPUs verfügen.

Wenn Sie `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` festlegen, muss der Cluster nicht über 12 virtuelle CPUs verfügen. Wir empfehlen mindestens zwei virtuelle CPUs für Entwicklung/Tests. Ein Cluster, der für Entwicklung/Tests konfiguriert ist, eignet sich jedoch nicht für Produktionsdatenverkehr und kann Rückschlusszeiten erhöhen. Entwicklungs- und Testcluster garantieren auch keine Fehlertoleranz.

> [!WARNING]
> Erstellen Sie nicht mehrere gleichzeitige Verknüpfungen für einen AKS-Cluster in Ihrem Arbeitsbereich. Verwenden Sie beispielsweise beim Verknüpfen eines AKS-Clusters mit einem Arbeitsbereich nicht zwei unterschiedliche Namen. Jede neue Verknüpfung führt zu einem Fehler der vorherigen vorhandenen Verknüpfungen.
>
> Falls Sie einen AKS-Cluster erneut verknüpfen möchten (etwa zum Ändern der SSL-Einstellung oder einer anderen Clusterkonfigurationseinstellung), müssen Sie zuerst die vorhandene Verknüpfung mithilfe von [AksCompute.detach()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#detach--) entfernen.

Weitere Informationen zum Erstellen eines AKS-Clusters mithilfe der Azure-CLI oder des Portals finden Sie in den folgenden Artikeln:

* [Erstellen eines AKS-Clusters (CLI)](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Schnellstart: Bereitstellen eines AKS-Clusters (Azure Kubernetes Service) über das Azure-Portal](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Das folgende Beispiel veranschaulicht das Anfügen eines vorhandenen AKS-Clusters an Ihren Arbeitsbereich:

**Verwenden des SDK**

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'myexistingcluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'myaks', attach_config)
```

Weitere Informationen zu den in diesem Beispiel verwendeten Klassen, Methoden und Parametern finden Sie in den folgenden Referenzdokumenten:

* [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py)
* [AksCompute.attach](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#attach-workspace--name--attach-configuration-)

**Verwenden der CLI**

Um einen vorhandenen Cluster mithilfe der CLI anzufügen, müssen Sie die Ressourcen-ID des vorhandenen Clusters abrufen. Verwenden Sie zum Abrufen dieses Werts den folgenden Befehl. Ersetzen Sie `myexistingcluster` durch den Namen Ihres AKS-Clusters. Ersetzen Sie `myresourcegroup` durch die Ressourcengruppe, die den Cluster enthält:

```azurecli
az aks show -n myexistingcluster -g myresourcegroup --query id
```

Dieser Befehl gibt einen Wert zurück, der in etwa wie folgt aussieht:

```text
/subscriptions/{GUID}/resourcegroups/{myresourcegroup}/providers/Microsoft.ContainerService/managedClusters/{myexistingcluster}
```

Um den vorhandenen Cluster an Ihren Arbeitsbereich anzufügen, verwenden Sie den folgenden Befehl. Ersetzen Sie `aksresourceid` durch den Wert, der über den vorherigen Befehl zurückgegeben wurde. Ersetzen Sie `myresourcegroup` durch die Ressourcengruppe, die Ihren Arbeitsbereich enthält. Ersetzen Sie `myworkspace` durch den Namen Ihres Arbeitsbereichs.

```azurecli
az ml computetarget attach aks -n myaks -i aksresourceid -g myresourcegroup -w myworkspace
```

Weitere Informationen finden Sie unter [az ml computetarget attach aks](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks).

## <a name="deploy-to-aks"></a>Bereitstellen für AKS

Um ein Modell für Azure Kubernetes Service bereitzustellen, erstellen Sie eine __Bereitstellungskonfiguration__, in der die benötigten Computeressourcen beschrieben werden. Dies sind beispielsweise die Anzahl von Kernen und die Arbeitsspeichergröße. Außerdem benötigen Sie eine __Rückschlusskonfiguration__, in der die zum Hosten des Modells und des Webdiensts erforderliche Umgebung beschrieben wird. Weitere Informationen zum Erstellen der Rückschlusskonfiguration finden Sie unter [Wie und wo Modelle bereitgestellt werden](how-to-deploy-and-where.md).

### <a name="using-the-sdk"></a>Verwenden des SDK

```python
from azureml.core.webservice import AksWebservice, Webservice
from azureml.core.model import Model

aks_target = AksCompute(ws,"myaks")
# If deploying to a cluster configured for dev/test, ensure that it was created with enough
# cores and memory to handle this deployment configuration. Note that memory is also used by
# things such as dependencies and AML components.
deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config, aks_target)
service.wait_for_deployment(show_output = True)
print(service.state)
print(service.get_logs())
```

Weitere Informationen zu den in diesem Beispiel verwendeten Klassen, Methoden und Parametern finden Sie in den folgenden Referenzdokumenten:

* [AksCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute?view=azure-ml-py)
* [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)
* [Model.deploy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-)
* [Webservice.wait_for_deployment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#wait-for-deployment-show-output-false-)

### <a name="using-the-cli"></a>Verwenden der CLI

Verwenden Sie für die Bereitstellung mit der CLI den folgenden Befehl. Ersetzen Sie `myaks` durch den Namen des AKS-Computeziels. Ersetzen Sie `mymodel:1` durch den Namen und die Version des registrierten Modells. Ersetzen Sie `myservice` durch den Namen, den dieser Dienst erhalten soll:

```azurecli-interactive
az ml model deploy -ct myaks -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aks-deploy-config.md)]

Weitere Informationen finden Sie in der [az ml model deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy)-Referenz. 

### <a name="using-vs-code"></a>Verwenden von VS Code

Informationen zur Verwendung von VS Code finden Sie im Artikel zum [Bereitstellen von AKS über die VS Code-Erweiterung](how-to-vscode-tools.md#deploy-and-manage-models).

> [!IMPORTANT] 
> Für die Bereitstellung über VS Code muss der AKS-Cluster im Vorfeld erstellt oder an Ihren Arbeitsbereich angefügt werden.

## <a name="deploy-models-to-aks-using-controlled-rollout-preview"></a>Bereitstellen von Modellen in AKS mithilfe eines kontrollierten Rollouts (Vorschau)
Sie können Modellversionen mithilfe von Endpunkten kontrolliert analysieren und höher stufen. Stellen Sie bis zu 6 Versionen hinter einem einzigen Endpunkt bereit, und konfigurieren Sie den Prozentsatz der Bewertung des Datenverkehrs für jede bereitgestellte Version. Sie können App-Erkenntnisse aktivieren, um Metriken zum Betrieb von Endpunkten und bereitgestellten Versionen anzuzeigen.

### <a name="create-an-endpoint"></a>Erstellen eines Endpunkts
Wenn Sie bereit sind, Ihre Modelle bereitzustellen, erstellen Sie einen Bewertungsendpunkt, und stellen Sie Ihre erste Version bereit. Der folgende Schritt zeigt, wie Sie den Endpunkt mithilfe des SDK erstellen und bereitstellen. Die erste Bereitstellung wird als Standardversion definiert, was bedeutet, dass ein nicht spezifiziertes Perzentil des Datenverkehrs in allen Versionen in die Standardversion eingehen.  

```python
import azureml.core,
from azureml.core.webservice import AksEndpoint
from azureml.core.compute import AksCompute
from azureml.core.compute import ComputeTarget
# select a created compute
compute = ComputeTarget(ws, 'myaks')
namespace_name= endpointnamespace 
# define the endpoint and version name
endpoint_name = "mynewendpoint",
version_name= "versiona",
# create the deployment config and define the scoring traffic percentile for the first deployment
endpoint_deployment_config = AksEndpoint.deploy_configuration(cpu_cores = 0.1, memory_gb = 0.2,
                                                              enable_app_insights = true, 
                                                              tags = {'sckitlearn':'demo'},
                                                              decription = testing versions,
                                                              version_name = version_name,
                                                              traffic_percentile = 20)
 # deploy the model and endpoint
 endpoint = Model.deploy(ws, endpoint_name, [model], inference_config, endpoint_deployment_config, compute)
 ```

### <a name="update-and-add-versions-to-an-endpoint"></a>Aktualisieren und Hinzufügen von Versionen zu einem Endpunkt

Fügen Sie Ihrem Endpunkt eine weitere Version hinzu, und konfigurieren Sie das bewertende Perzentil des Datenverkehrs, das in die Version eingeht. Es gibt zwei Arten von Versionen, eine Kontroll- und eine Behandlungsversion. Es kann mehrere Behandlungsversionen geben, um den Vergleich mit einer einzelnen Kontrollversion zu erleichtern. 

 ```python
from azureml.core.webservice import AksEndpoint

# add another model deployment to the same endpoint as above
version_name_add = "versionb" 
endpoint.create_version(version_name = version_name_add, 
                        inference_config=inference_config,
                        models=[model], 
                        tags = {'modelVersion':'b'}, 
                        description = "my second version", 
                        traffic_percentile = 10)
```

Aktualisieren Sie vorhandene Versionen, oder löschen Sie sie in einem Endpunkt. Sie können den Standardtyp, den Kontrolltyp und das Perzentil des Datenverkehrs der Version ändern. 
 
 ```python
from azureml.core.webservice import AksEndpoint

# update the version's scoring traffic percentage and if it is a default or control type 
endpoint.update_version(version_name=endpoint.versions["versionb"].name, 
                        description="my second version update", 
                        traffic_percentile=40,
                        is_default=True,
                        is_control_version_type=True)

# delete a version in an endpoint 
endpoint.delete_version(version_name="versionb")

```


## <a name="web-service-authentication"></a>Webdienstauthentifizierung

Bei der Bereitstellung in Azure Kubernetes Service ist die __schlüsselbasierte__ Authentifizierung standardmäßig aktiviert. Sie können auch die __tokenbasierte__ Authentifizierung aktivieren. Die tokenbasierte Authentifizierung erfordert, dass Clients ein Azure Active Directory-Konto verwenden, um ein Authentifizierungstoken anzufordern, mit dem Anforderungen an den bereitgestellten Dienst gesendet werden.

Um die Authentifizierung zu __deaktivieren__, legen Sie den Parameter `auth_enabled=False` beim Erstellen der Bereitstellungskonfiguration fest. Im folgenden Beispiel wird die Authentifizierung mithilfe des SDKs deaktiviert:

```python
deployment_config = AksWebservice.deploy_configuration(cpu_cores=1, memory_gb=1, auth_enabled=False)
```

Informationen zum Authentifizieren aus einer Clientanwendung finden Sie unter [Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells](how-to-consume-web-service.md).

### <a name="authentication-with-keys"></a>Authentifizierung mit Schlüsseln

Bei aktivierter Schlüsselauthentifizierung können Sie mithilfe der Methode `get_keys` einen primären und einen sekundären Authentifizierungsschlüssel abrufen:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Wenn Sie einen Schlüssel erneut generieren müssen, verwenden Sie [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py).

### <a name="authentication-with-tokens"></a>Authentifizierung mit Tokens

Um Tokenauthentifizierung zu aktivieren, legen Sie beim Erstellen oder Aktualisieren einer Bereitstellung den Parameter `token_auth_enabled=True` fest. Im folgenden Beispiel wird die Authentifizierung mithilfe des SDKs aktiviert:

```python
deployment_config = AksWebservice.deploy_configuration(cpu_cores=1, memory_gb=1, token_auth_enabled=True)
```

Bei aktivierter Tokenauthentifizierung können Sie mithilfe der Methode `get_token` ein JWT-Token und dessen Ablaufzeit abrufen:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Nach Ablauf der für `refresh_by` festgelegten Zeit müssen Sie ein neues Token anfordern.
>
> Microsoft empfiehlt dringend, den Azure Machine Learning-Arbeitsbereich in der gleichen Region zu erstellen wie den Azure Kubernetes Service-Cluster. Im Zuge der Tokenauthentifizierung richtet der Webdienst einen Aufruf an die Region, in der Ihr Azure Machine Learning-Arbeitsbereich erstellt wird. Ist die Region Ihres Arbeitsbereichs nicht verfügbar, können Sie kein Token für Ihren Webdienst abrufen (auch dann nicht, wenn sich Ihr Cluster in einer anderen Region befindet als Ihr Arbeitsbereich). Die tokenbasierte Authentifizierung ist dann erst wieder verfügbar, wenn die Region Ihres Arbeitsbereichs wieder verfügbar wird. Außerdem wirkt sich die Entfernung zwischen der Region Ihres Clusters und der Region Ihres Arbeitsbereichs direkt auf die Tokenabrufdauer aus.

## <a name="update-the-web-service"></a>Aktualisieren des Webdiensts

[!INCLUDE [aml-update-web-service](../../../includes/machine-learning-update-web-service.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Schützen von Experimenten und Rückschlüssen in einem virtuellen Netzwerk](how-to-enable-virtual-network.md)
* [Wie man ein Modell mit einem benutzerdefinierten Docker-Image bereitstellt](how-to-deploy-custom-docker-image.md)
* [Problembehandlung von Bereitstellungen von Azure Machine Learning Service mit AKS und ACI](how-to-troubleshoot-deployment.md)
* [Secure Azure Machine Learning web services with SSL (Sichere Azure Machine Learning-Webdienste mit SSL)](how-to-secure-web-service.md)
* [Consume a ML Model deployed as a web service (Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells)](how-to-consume-web-service.md).
* [Überwachen Ihrer Azure Machine Learning-Modelle mit Application Insights](how-to-enable-app-insights.md)
* [Sammeln von Daten für Modelle in der Produktion](how-to-enable-data-collection.md)
