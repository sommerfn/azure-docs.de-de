---
title: Machine Learning-CLI-Erweiterung
titleSuffix: Azure Machine Learning
description: Hier erhalten Sie Informationen zur Azure Machine Learning-CLI-Erweiterung für die Azure CLI. Die Azure CLI ist ein plattformübergreifendes Befehlszeilenhilfsprogramm, das das Arbeiten mit Ressourcen in der Azure-Cloud ermöglicht. Die Machine Learning-Erweiterung ermöglicht Ihnen das Arbeiten mit Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: jordane
author: jpe316
ms.date: 10/22/2019
ms.custom: seodec18
ms.openlocfilehash: dcbe535bb92f7755980929956093b0f0121ef41b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73476450"
---
# <a name="use-the-cli-extension-for-azure-machine-learning"></a>Verwenden der CLI-Erweiterung für Azure Machine Learning
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Der Azure Machine Learning-CLI ist eine Erweiterung der [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest), eine plattformübergreifende Befehlszeilenschnittstelle für die Azure-Plattform. Diese Erweiterung unterstützt Befehle für die Arbeit mit Azure Machine Learning. Sie ermöglicht Ihnen die Automatisierung Ihrer Machine Learning-Aktivitäten. Die folgende Liste enthält einige Beispielaktionen, die Sie mit der CLI-Erweiterung ausführen können:

+ Ausführen von Experimenten zum Erstellen von Machine Learning-Modellen

+ Registrieren von Machine Learning-Modellen zur Verwendung durch Kunden

+ Paketieren, Bereitstellen und Nachverfolgen des Lebenszyklus Ihrer Machine Learning-Modelle

Die CLI ist kein Ersatz für das Azure Machine Learning SDK. Sie stellt ein ergänzendes Tool dar, das für die Verarbeitung hochgradig parametrisierter Aufgaben optimiert ist.

## <a name="prerequisites"></a>Voraussetzungen

* Für die Verwendung der CLI benötigen Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Die [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)

## <a name="full-reference-docs"></a>Vollständige Referenzdokumentation

Hier finden Sie die [vollständige Referenzdokumentation für die Erweiterung „azure-cli-ml“ der Azure CLI](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/?view=azure-cli-latest).

## <a name="install-the-extension"></a>Installieren der Erweiterung

Um die Machine Learning-CLI-Erweiterung zu installieren, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!TIP]
> Beispieldateien, die Sie mit den [hier](https://aka.ms/azml-deploy-cloud) aufgeführten Befehlen verwenden können.

Klicken Sie bei Aufforderung auf `y`, um die Erweiterung zu installieren.

Um sicherzustellen, dass die Erweiterung installiert wurde, verwenden Sie den folgenden Befehl zum Anzeigen einer Liste von ML-spezifischen Unterbefehlen:

```azurecli-interactive
az ml -h
```

## <a name="update-the-extension"></a>Aktualisieren der Erweiterung

Um die Machine Learning-CLI-Erweiterung zu aktualisieren, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az extension update -n azure-cli-ml
```


## <a name="remove-the-extension"></a>Entfernen der Erweiterung

Verwenden Sie zum Entfernen der CLI-Erweiterung den folgenden Befehl:

```azurecli-interactive
az extension remove -n azure-cli-ml
```

## <a name="resource-management"></a>Ressourcenverwaltung

Die folgenden Befehle veranschaulichen, wie Sie mit der CLI Ressourcen verwalten, die von Azure Machine Learning verwendet werden.

+ Erstellen Sie eine Ressourcengruppe, wenn noch keine vorhanden ist:

    ```azurecli-interactive
    az group create -n myresourcegroup -l westus2
    ```

+ Erstellen Sie einen Azure Machine Learning-Arbeitsbereich:

    ```azurecli-interactive
    az ml workspace create -w myworkspace -g myresourcegroup
    ```

    Weitere Informationen finden Sie unter [az ml workspace create](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext-azure-cli-ml-az-ml-workspace-create).

+ Fügen Sie eine Arbeitsbereichskonfiguration einem Ordner hinzu, um CLI-Kontextfähigkeit zu aktivieren.

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    Dieser Befehl erstellt ein `.azureml`-Unterverzeichnis, das RUNCONFIG-Beispieldateien und Conda-Umgebungsdateien enthält. Es enthält auch eine `config.json`-Datei, die für die Kommunikation mit dem Azure Machine Learning-Arbeitsbereich verwendet wird.

    Weitere Informationen finden Sie unter [az ml folder attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

+ Fügen Sie einen Azure-Blobcontainer als Datenspeicher an.

    ```azurecli-interactive
    az ml datastore attach-blob  -n datastorename -a accountname -c containername
    ```

    Weitere Informationen finden Sie unter [az ml datastore attach-blob](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-attach-blob).

+ Laden Sie Dateien in einen Datenspeicher hoch.

    ```azurecli-interactive
    az ml datastore upload  -n datastorename -p sourcepath
    ```

    Weitere Informationen finden Sie unter [az ml datastore upload](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/datastore?view=azure-cli-latest#ext-azure-cli-ml-az-ml-datastore-upload).

+ Fügen Sie einen AKS-Cluster als Computeziel an.

    ```azurecli-interactive
    az ml computetarget attach aks -n myaks -i myaksresourceid -g myresourcegroup -w myworkspace
    ```

    Weitere Informationen finden Sie unter [az ml computetarget attach aks](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks).

+ Erstellen Sie ein neues AMLcompute-Ziel.

    ```azurecli-interactive
    az ml computetarget create amlcompute -n cpu --min-nodes 1 --max-nodes 1 -s STANDARD_D3_V2
    ```

    Weitere Informationen finden Sie unter [az ml computetarget create amlcompute](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-amlcompute).

## <a id="experiments"></a>Ausführen von Experimenten

* Starten Sie eine Ausführung Ihres Experiments. Wenn Sie diesen Befehl verwenden, geben Sie den Namen der Runconfig-Datei (den Text vor „\*.runconfig“, wenn Sie auf Ihr Dateisystem schauen) für den -c-Parameter an.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > Der Befehl `az ml folder attach` erstellt ein `.azureml`-Unterverzeichnis, das zwei RUNCONFIG-Beispieldateien enthält. 
    >
    > Wenn Sie ein Python-Skript haben, das programmgesteuert ein Laufzeitkonfigurationsobjekt erstellt, können Sie es mit [RunConfig.save()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) als RUNCPNFIG-Datei speichern.
    >
    > Das vollständige RUNCONFIG-Schema finden Sie in dieser [JSON-Datei](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json).

    Weitere Informationen finden Sie unter [az ml run submit-script](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

* Zeigen Sie eine Liste der Experimente an:

    ```azurecli-interactive
    az ml experiment list
    ```

    Weitere Informationen finden Sie unter [az ml experiment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

## <a name="environment-management"></a>Umgebungsverwaltung

Anhand der folgenden Befehle wird veranschaulicht, wie Sie Azure Machine Learning-[Umgebungen](how-to-configure-environment.md) für Ihren Arbeitsbereich erstellen, registrieren und auflisten:

+ Erstellen Sie die Gerüstdateien für eine Umgebung:

    ```azurecli-interactive
    az ml environment scaffold -n myenv -d myenvdirectory
    ```

    Weitere Informationen finden Sie unter [az ml environment scaffold](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-scaffold).

+ Registrieren Sie eine Umgebung:

    ```azurecli-interactive
    az ml environment register -d myenvdirectory
    ```

    Weitere Informationen finden Sie unter [az ml environment register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-register).

+ Listen Sie die registrierten Umgebungen auf:

    ```azurecli-interactive
    az ml environment list
    ```

    Weitere Informationen finden Sie unter [az ml environment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-list).

+ Laden Sie eine registrierte Umgebung herunter:

    ```azurecli-interactive
    az ml environment download -n myenv -d downloaddirectory
    ```

    Weitere Informationen finden Sie unter [az ml environment download](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/environment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-environment-download).

## <a name="ml-pipeline-management"></a>Verwaltung von ML-Pipelines

Die folgenden Befehle veranschaulichen, wie mit Machine Learning-Pipelines gearbeitet wird.

+ Erstellen Sie eine Machine Learning-Pipeline:

    ```azurecli-interactive
    az ml pipeline create -n mypipeline -y mypipeline.yml
    ```

    Weitere Informationen finden Sie unter [az ml pipeline create](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/pipeline?view=azure-cli-latest#ext-azure-cli-ml-az-ml-pipeline-create).

    Weitere Informationen zur Pipeline-YAML-Datei finden Sie unter [Definieren von Machine Learning-Pipelines in YAML](reference-pipeline-yaml.md).

+ Führen Sie eine Pipeline aus:

    ```azurecli-interactive
    az ml run submit-pipeline -n myexperiment -y mypipeline.yml
    ```

    Weitere Informationen finden Sie unter [az ml run submit-pipeline](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-pipeline).

    Weitere Informationen zur Pipeline-YAML-Datei finden Sie unter [Definieren von Machine Learning-Pipelines in YAML](reference-pipeline-yaml.md).

+ Planen Sie eine Pipeline:

    ```azurecli-interactive
    az ml pipeline create-schedule -n myschedule -e myexpereiment -i mypipelineid -y myschedule.yml
    ```

    Weitere Informationen finden Sie unter [az ml pipeline create-schedule](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/pipeline?view=azure-cli-latest#ext-azure-cli-ml-az-ml-pipeline-create-schedule).

    Weitere Informationen zur YAML-Datei für einen Pipelinezeitplan finden Sie unter [Definieren von Machine Learning-Pipelines in YAML](reference-pipeline-yaml.md#schedules).

## <a name="model-registration-profiling-deployment"></a>Registrierung von Modellen, Profilerstellung und Bereitstellung

Die folgenden Befehle veranschaulichen, wie ein trainiertes Modell registriert und dann als Produktionsdienst bereitgestellt wird:

+ Hiermit registrieren Sie ein Modell bei Azure Machine Learning:

    ```azurecli-interactive
    az ml model register -n mymodel -p sklearn_regression_model.pkl
    ```

    Weitere Informationen finden Sie unter [az ml model register](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register).

+ **OPTIONAL** Erstellen Sie ein Profil für Ihr Modell, um optimale CPU- und Arbeitsspeicherwerte für die Bereitstellung zu erzielen.
    ```azurecli-interactive
    az ml model profile -n myprofile -m mymodel:1 --ic inferenceconfig.json -d "{\"data\": [[1,2,3,4,5,6,7,8,9,10],[10,9,8,7,6,5,4,3,2,1]]}" -t myprofileresult.json
    ```

    Weitere Informationen finden Sie unter [az ml model profile](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-profile).

+ Bereitstellen Ihrer Modelldatei für AKS
    ```azurecli-interactive
    az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json --dc deploymentconfig.json --ct akscomputetarget
    ```
    
    Weitere Informationen zum Rückschlusskonfigurationsdatei-Schema finden Sie unter [Inference configuration schema (Rückschlusskonfigurationsschema)](#inferenceconfig).
    
    Weitere Informationen zum Bereitstellungskonfigurationsdatei-Schema finden Sie unter [Deployment configuration schema (Bereitstellungskonfigurationsschema)](#deploymentconfig).

    Weitere Informationen finden Sie unter [az ml model deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy).

<a id="inferenceconfig"></a>

## <a name="inference-configuration-schema"></a>Rückschlusskonfigurationsschema

[!INCLUDE [inferenceconfig](../../../includes/machine-learning-service-inference-config.md)]

<a id="deploymentconfig"></a>

## <a name="deployment-configuration-schema"></a>Bereitstellungskonfigurationsschema

### <a name="local-deployment-configuration-schema"></a>Konfigurationsschema zur lokalen Bereitstellung

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-local-deploy-config.md)]

### <a name="azure-container-instance-deployment-configuration-schema"></a>Konfigurationsschema zur Bereitstellung einer Azure-Containerinstanz 

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aci-deploy-config.md)]

### <a name="azure-kubernetes-service-deployment-configuration-schema"></a>Konfigurationsschema zur Bereitstellung von Azure Kubernetes Service

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aks-deploy-config.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Befehlsreferenz für die Machine Learning-CLI-Erweiterung](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml?view=azure-cli-latest)

* [Trainieren und Bereitstellen von Machine Learning-Modellen mit Azure Pipelines](/azure/devops/pipelines/targets/azure-machine-learning)
