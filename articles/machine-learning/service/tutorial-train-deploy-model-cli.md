---
title: Trainieren und Bereitstellen von Modellen über die Befehlszeilenschnittstelle
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie die Machine Learning-Erweiterung für die Azure-Befehlszeilenschnittstelle verwenden können, um ein Modell über die Befehlszeile zu trainieren, zu registrieren und bereitzustellen.
ms.author: larryfr
author: Blackmist
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 09/12/2019
ms.openlocfilehash: 1854599956755716955a6e691c3266ac54ddafd9
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73581551"
---
# <a name="tutorial-train-and-deploy-a-model-from-the-cli"></a>Tutorial: Trainieren und Bereitstellen eines Modells über die Befehlszeilenschnittstelle
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Tutorial verwenden Sie die Machine Learning-Erweiterung für die Azure-Befehlszeilenschnittstelle, um ein Modell zu trainieren, zu registrieren und bereitzustellen.

Die Python-Trainingsskripts in diesem Tutorial verwenden [scikit-learn](https://scikit-learn.org/), um ein Basismodell zu trainieren. Der Schwerpunkt dieses Tutorials liegt nicht auf den Skripts oder dem Modell, sondern auf der Verwendung der Befehlszeilenschnittstelle für die Arbeit mit Azure Machine Learning.

Erfahren Sie, wie Sie die folgenden Maßnahmen durchführen:

> [!div class="checklist"]
> * Installieren der Erweiterung für maschinelles Lernen
> * Erstellen eines Azure Machine Learning-Arbeitsbereichs
> * Erstellen der zum Trainieren des Modells verwendeten Computeressource
> * Starten einer Trainingsausführung
> * Registrieren und Herunterladen eines Modells
> * Bereitstellen des Modells als Webdienst
> * Bewerten von Daten mithilfe des Webdiensts

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Um die CLI-Befehle in diesem Dokument aus Ihrer **lokalen Umgebung** zu verwenden, benötigen Sie die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

    Wenn Sie die [Azure Cloud Shell](https://azure.microsoft.com//features/cloud-shell/) verwenden, befindet sich die CLI in der Cloud, und der Zugriff erfolgt über den Browser.

## <a name="download-the-example-project"></a>Herunterladen des Beispielprojekts

Laden Sie für dieses Tutorial das Projekt [https://github.com/microsoft/MLOps](https://github.com/microsoft/MLOps) herunter. Die Dateien in den Verzeichnissen `model-training` und `model-deployment` werden von den Schritten in diesem Tutorial verwendet.

Um eine lokale Kopie der Dateien zu erhalten, laden Sie entweder ein [ZIP-Archiv herunter](https://github.com/microsoft/MLOps/archive/master.zip), oder verwenden Sie den folgenden Git-Befehl, um das Repository zu klonen:

```azurecli-interactive
git clone https://github.com/microsoft/MLOps.git
```

### <a name="training-files"></a>Trainingsdateien

Das Verzeichnis `model-training` enthält die folgenden Dateien, die für das Training eines Modells verwendet werden:

* `.azureml\sklearn.runconfig`: Eine __Laufzeitkonfigurationsdatei__. Diese Datei definiert die Laufzeitumgebung, die zum Trainieren des Modells erforderlich ist.
* `train-sklearn.py`: Das Trainingsskript. Mit dieser Datei wird das Modell trainiert.
* `mylib.py`: Ein Hilfsmodul, das von `train-sklearn.py` verwendet wird.
* `training-env.yml`: Definiert die Softwareabhängigkeiten, die für die Ausführung des Trainingsskripts erforderlich sind.

Das Trainingsskript verwendet den mit scikit-learn bereitgestellten Diabetes-Datensatz, um ein Modell zu trainieren.

### <a name="deployment-files"></a>Bereitstellungsdateien

Das Verzeichnis `model-deployment` enthält die folgenden Dateien, mit denen das trainierte Modell als Webdienst bereitgestellt wird:

* `aciDeploymentConfig.yml`: Eine Datei für die __Bereitstellungskonfiguration__. Diese Datei definiert die für das Modell erforderliche Hostingumgebung.
* `inferenceConfig.yml`: Eine Datei für die Rückschlusskonfiguration. Diese Datei definiert die Softwareumgebung, die vom Dienst verwendet wird, um Daten mit dem Modell zu bewerten.
* `score.py`: Ein Python-Skript, das eingehende Daten akzeptiert, sie mit dem Modell bewertet und dann eine Antwort zurückgibt.
* `scoring-env.yml`: Die Conda-Abhängigkeiten, die für die Ausführung des Modells und des `score.py`-Skripts erforderlich sind.

## <a name="connect-to-your-azure-subscription"></a>Verbinden mit Ihrem Azure-Abonnement

Ihnen stehen mehrere Möglichkeiten zur Verfügung, sich über die CLI bei Ihrem Azure-Abonnement zu authentifizieren. Die grundlegendste ist die interaktive Authentifizierung mithilfe eines Browsers. Öffnen Sie zur interaktiven Authentifizierung eine Befehlszeile oder ein Terminal, und verwenden Sie den folgenden Befehl:

```azurecli-interactive
az login
```

Die CLI öffnet Ihren Standardbrowser, sofern sie dazu in der Lage ist, und lädt eine Anmeldeseite. Andernfalls müssen Sie einen Browser öffnen und die Anweisungen in der Befehlszeile befolgen. Die Anweisungen umfassen das Navigieren zu [https://aka.ms/devicelogin](https://aka.ms/devicelogin) und Eingeben eines Autorisierungscodes.

## <a name="install-the-machine-learning-extension"></a>Installieren der Erweiterung für maschinelles Lernen

Um die Erweiterung für maschinelles Lernen zu installieren, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az extension add -n azure-cli-ml
```

Wenn Sie die Meldung erhalten, dass die Erweiterung bereits installiert ist, verwenden Sie den folgenden Befehl, um auf die neueste Version zu aktualisieren:

```azurecli-interactive
az extension update -n azure-cli-ml
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist ein grundlegender Container mit Ressourcen auf der Azure-Plattform. Wenn Sie mit Azure Machine Learning arbeiten, enthält die Ressourcengruppe Ihren Azure Machine Learning-Arbeitsbereich. Sie enthält auch andere Azure-Dienste, die vom Arbeitsbereich verwendet werden. Wenn Sie Ihr Modell z. B. mit einer cloudbasierten Computeressource trainieren, wird diese Ressource in der Ressourcengruppe erstellt.

Um __eine neue Ressourcengruppe zu erstellen__, verwenden Sie den folgenden Befehl. Ersetzen Sie `<resource-group-name>` durch den Namen, der für diese Ressourcengruppe verwendet werden soll. Ersetzen Sie `<location>` durch die Azure-Region, die für diese Ressourcengruppe verwendet werden soll:

> [!TIP]
> Wählen Sie eine Region aus, in der Azure Machine Learning verfügbar ist. Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=machine-learning-service).

```azurecli-interactive
az group create --name <resource-group-name> --location <location>
```

Die Antwort dieses Befehls ähnelt dem folgenden JSON-Code:

```json
{
  "id": "/subscriptions/<subscription-GUID>/resourceGroups/<resourcegroupname>",
  "location": "<location>",
  "managedBy": null,
  "name": "<resource-group-name>",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

Weitere Informationen zum Arbeiten mit Ressourcengruppen finden Sie unter [az group](https://docs.microsoft.com//cli/azure/group?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Verwenden Sie den folgenden Befehl, um einen neuen Arbeitsbereich zu erstellen. Ersetzen Sie `<workspace-name>` durch den Namen, den Sie für diesen Arbeitsbereich verwenden möchten. Ersetzen Sie `<resource-group-name>` durch den Namen der Ressourcengruppe:

```azurecli-interactive
az ml workspace create -w <workspace-name> -g <resource-group-name>
```

Die Ausgabe dieses Befehls ähnelt dem folgenden JSON-Code:

```json
{
  "applicationInsights": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<application-insight-name>",
  "containerRegistry": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.containerregistry/registries/<acr-name>",
  "creationTime": "2019-08-30T20:24:19.6984254+00:00",
  "description": "",
  "friendlyName": "<workspace-name>",
  "id": "/subscriptions/<service-GUID>/resourceGroups/<resource-group-name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>",
  "identityPrincipalId": "<GUID>",
  "identityTenantId": "<GUID>",
  "identityType": "SystemAssigned",
  "keyVault": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.keyvault/vaults/<key-vault-name>",
  "location": "<location>",
  "name": "<workspace-name>",
  "resourceGroup": "<resource-group-name>",
  "storageAccount": "/subscriptions/<service-GUID>/resourcegroups/<resource-group-name>/providers/microsoft.storage/storageaccounts/<storage-account-name>",
  "type": "Microsoft.MachineLearningServices/workspaces",
  "workspaceid": "<GUID>"
}
```

## <a name="connect-local-project-to-workspace"></a>Verbinden eines lokalen Projekts mit dem Arbeitsbereich

Verwenden Sie über ein Terminal oder eine Eingabeaufforderung die folgenden Befehle, um Verzeichnisse in das Verzeichnis `mlops` zu ändern und sich dann mit Ihrem Arbeitsbereich zu verbinden:

```azurecli-interactive
cd ~/mlops
az ml folder attach -w <workspace-name> -g <resource-group-name>
```

Die Ausgabe dieses Befehls ähnelt dem folgenden JSON-Code:

```json
{
  "Experiment name": "model-training",
  "Project path": "/Code/MLOps/model-training",
  "Resource group": "<resource-group-name>",
  "Subscription id": "<subscription-id>",
  "Workspace name": "<workspace-name>"
}
```

Dieser Befehl erstellt eine `.azureml/config.json`-Datei, die Informationen enthält, die für die Verbindung mit Ihrem Arbeitsbereich erforderlich sind. Der Rest der in diesem Tutorial verwendeten `az ml`-Befehle verwendet diese Datei, sodass Sie den Arbeitsbereich und die Ressourcengruppe nicht zu allen Befehlen hinzufügen müssen.

## <a name="create-the-compute-target-for-training"></a>Erstellen des Computeziels für das Training

In diesem Beispiel wird eine Azure Machine Learning-Notebook-VM verwendet, um das Modell zu trainieren. Verwenden Sie den folgenden Befehl, um eine neue Notebook-VM zu erstellen:

```azurecli-interactive
az ml computetarget create amlcompute -n cpu --max-nodes 4 --vm-size Standard_D2_V2
```

Die Ausgabe dieses Befehls ähnelt dem folgenden JSON-Code:

```json
{
  "location": "<location>",
  "name": "cpu",
  "provisioningErrors": null,
  "provisioningState": "Succeeded"
}
```

Dieser Befehl erstellt ein neues Computeziel namens `cpu` mit maximal vier Knoten. Die gewählte VM-Größe stellt einer VM eine GPU-Ressource zur Verfügung. Informationen zur VM-Größe finden Sie unter [VM-Typen und -Größen].

> [!IMPORTANT]
> Der Name des Computeziels (in diesem Fall `cpu`) ist wichtig, da die im nächsten Abschnitt verwendete `.azureml/sklearn.runconfig`-Datei auf ihn verweist.

## <a name="submit-the-training-run"></a>Übermitteln der Trainingsausführung

Um eine Trainingsausführung auf dem `cpu`-Computeziel zu starten, ändern Sie die Verzeichnisse in das Verzeichnis `model-training`, und verwenden Sie dann den folgenden Befehl:

```azurecli-interactive
cd ~/mlops/model-training
az ml run submit-script -e myexperiment -c sklearn -d training-env.yml -t runoutput.json train-sklearn.py
```

Dieser Befehl gibt einen Namen für das Experiment an (`myexperiment`). Das Experiment speichert Informationen zu dieser Ausführung im Arbeitsbereich.

Der Parameter `-c sklearn` gibt die Datei `.azureml/sklearn.runconfig` an. Wie bereits erwähnt, enthält diese Datei Informationen zur Konfiguration der von der Trainingsausführung verwendeten Umgebung. Wenn Sie diese Datei untersuchen, werden Sie feststellen, dass sie auf das zuvor erstellte `cpu`-Computeziel verweist. Sie listet auch die Anzahl der Knoten auf, die beim Training verwendet werden sollen (`"nodeCount": "4"`), und enthält einen Abschnitt `"condaDependenciees"`, der die Python-Pakete auflistet, die für die Ausführung des Trainingsskripts erforderlich sind.

Weitere Informationen zu Laufzeitkonfigurationsdateien finden Sie unter [Einrichten von und Verwenden von Computezielen für das Modelltraining](how-to-set-up-training-targets.md#create-run-configuration-and-submit-run-using-azure-machine-learning-cli). In dieser [JSON-Datei](https://github.com/microsoft/MLOps/blob/b4bdcf8c369d188e83f40be8b748b49821f71cf2/infra-as-code/runconfigschema.json) finden Sie das vollständige Schema einer Laufzeitkonfiguration.

Der Parameter `-t` speichert einen Verweis auf diese Ausführung in einer JSON-Datei und wird in den nächsten Schritten zur Registrierung und zum Herunterladen des Modells verwendet.

Während der Trainingsausführung werden Informationen aus der Trainingssitzung auf die Remotecomputeressource gestreamt. Ein Teil der Informationen ähnelt dem folgenden Text:

```text
alpha is 0.80, and mse is 3340.02
alpha is 0.85, and mse is 3349.36
alpha is 0.90, and mse is 3359.09
alpha is 0.95, and mse is 3369.13


The experiment completed successfully. Finalizing run...
Cleaning up all outstanding Run operations, waiting 300.0 seconds
```

Dieser Text wird aus dem Trainingsskript (`train-sklearn.py`) protokolliert und zeigt zwei der Leistungsmetriken für dieses Modell an. In diesem Fall möchten wir das Modell mit dem höchsten Alphawert verwenden. Die Leistungsmetriken sind spezifisch für das Modell, das Sie trainieren. Andere Modelle weisen andere Leistungsmetriken auf.

Wenn Sie die `train-sklearn.py` untersuchen, werden Sie feststellen, dass sie auch den Alphawert verwendet, wenn sie die trainierten Modelle in einer Datei speichert. In diesem Fall trainiert sie mehrere Modelle. Dasjenige mit dem höchsten Alphawert sollte das beste Modell sein. Unter Betrachtung der obigen Ausgabe und des Codes wurde das Modell mit einem Alphawert von 0,95 als `./outputs/ridge_0.95.pkl` gespeichert.

Das Modell wurde im Verzeichnis `./outputs` auf dem Computeziel gespeichert, auf dem es trainiert wurde. In diesem Fall ist dies die Azure Machine Learning-Notebook-VM in der Azure-Cloud. Der Trainingsprozess lädt automatisch die Inhalte des Verzeichnisses `./outputs` aus dem Computeziel, in dem das Training erfolgt, in Ihren Azure Machine Learning-Arbeitsbereich hoch. Es wird als Teil des Experiments gespeichert (`myexperiment` in diesem Beispiel).

## <a name="register-the-model"></a>Registrieren des Modells

Um das Modell direkt aus der gespeicherten Version in Ihrem Experiment zu registrieren, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az ml model register -n mymodel -f runoutput.json --asset-path "outputs/ridge_0.95.pkl" -t registeredmodel.json
```

Dieser Befehl registriert die von der Trainingsausführung erstellte Datei `outputs/ridge_0.95.pkl` als neue Modellregistrierung namens `mymodel`. Der `--assets-path` verweist auf einen Pfad in einem Experiment. In diesem Fall werden die Experiment- und Ausführungsinformationen aus der Datei `runoutput.json` geladen, die durch den Trainingsbefehl erstellt wurde. Die `-t registeredmodel.json` erstellt eine JSON-Datei, die auf das mit diesem Befehl erstellte neue registrierte Modell verweist und von anderen CLI-Befehlen verwendet wird, die mit registrierten Modellen arbeiten.

Die Ausgabe dieses Befehls ähnelt dem folgenden JSON-Code:

```json
{
  "createdTime": "2019-09-19T15:25:32.411572+00:00",
  "description": "",
  "experimentName": "myexperiment",
  "framework": "Custom",
  "frameworkVersion": null,
  "id": "mymodel:1",
  "name": "mymodel",
  "properties": "",
  "runId": "myexperiment_1568906070_5874522d",
  "tags": "",
  "version": 1
}
```

### <a name="model-versioning"></a>Versionsverwaltung der Modelle

Beachten Sie die Versionsnummer, die für das Modell zurückgegeben wird. Die Version wird jedes Mal inkrementiert, wenn Sie ein neues Modell mit diesem Namen registrieren. Sie können z. B. das Modell herunterladen und über eine lokale Datei registrieren, indem Sie die folgenden Befehle ausführen:

```azurecli-interactive
az ml model download -i "mymodel:1" -t .
az ml model register -n mymodel -p "ridge_0.95.pkl"
```

Der erste Befehl lädt das registrierte Modell in das aktuelle Verzeichnis herunter. Der Dateiname lautet `ridge_0.95.pkl`, d. h. es handelt sich um die Datei, auf die bei der Registrierung des Modells verwiesen wurde. Der zweite Befehl registriert das lokale Modell (`-p "ridge_0.95.pkl"`) mit demselben Namen wie die vorherige Registrierung (`mymodel`). Dieses Mal listen die zurückgegebenen JSON-Daten die Version als 2 auf.

## <a name="deploy-the-model"></a>Bereitstellen des Modells

Ändern Sie zum Bereitstellen eines Modells die Verzeichnisse in das Verzeichnis `model-deployment`, und verwenden Sie dann den folgenden Befehl:

```azurecli-interactive
cd ~/mlops/model-deployment
az ml model deploy -n myservice -m "mymodel:1" --ic inferenceConfig.yml --dc aciDeploymentConfig.yml
```

Sie erhalten möglicherweise die Meldung „Fehler beim Erstellen des Docker-Clients“. Sie können diese Meldung ignorieren. Die Befehlszeilenschnittstelle kann einen Webdienst in einem lokalen Docker-Container bereitstellen und auf Docker prüfen. In diesem Fall verwenden wir keine lokale Bereitstellung.

Dieser Befehl stellt einen neuen Dienst namens `myservice` bereit, der die Version 1 des Modells verwendet, das Sie zuvor registriert haben.

Die Datei `inferenceConfig.yml` liefert Informationen darüber, wie Sie Rückschlüsse ziehen können, z. B. über das Eingabeskript (`score.py`) und über Softwareabhängigkeiten. Weitere Informationen zur Struktur dieser Datei finden Sie unter [Rückschlusskonfigurationsschema](reference-azure-machine-learning-cli.md#inference-configuration-schema). Weitere Informationen zu Eingabeskripts finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md#prepare-to-deploy).

Die `aciDeploymentConfig.yml` beschreibt die zum Hosten des Dienstes verwendete Bereitstellungsumgebung. Die Bereitstellungskonfiguration hängt vom Computetyp ab, den Sie für die Bereitstellung verwenden. In diesem Fall wird eine Azure Container Instance verwendet. Weitere Informationen finden Sie unter [Bereitstellungskonfigurationsschema](reference-azure-machine-learning-cli.md#deployment-configuration-schema).

Es wird einige Minuten dauern, bis der Bereitstellungsprozess abgeschlossen ist.

> [!TIP]
> In diesem Beispiel wird Azure Container Instances verwendet. Bereitstellungen für Azure Container Instances erstellen automatisch die erforderliche Azure Container Instances-Ressource. Wenn Sie stattdessen für den Azure Kubernetes Service bereitstellen möchten, müssen Sie vorab einen AKS-Cluster erstellen und ihn als Teil des `az ml model deploy`-Befehls angeben. Ein Beispiel für die Bereitstellung in AKS finden Sie unter [Bereitstellen eines Modells in einem Azure Kubernetes Service-Cluster](how-to-deploy-azure-kubernetes-service.md).

Nach einigen Minuten werden Informationen ähnlich dem folgenden JSON-Code zurückgegeben:

```json
ACI service creation operation finished, operation "Succeeded"
{
  "computeType": "ACI",
  {...ommitted for space...}
  "runtimeType": null,
  "scoringUri": "http://6c061467-4e44-4f05-9db5-9f9a22ef7a5d.eastus2.azurecontainer.io/score",
  "state": "Healthy",
  "tags": "",
  "updatedAt": "2019-09-19T18:22:32.227401+00:00"
}
```

### <a name="the-scoring-uri"></a>Bewertungs-URI

Der von der Bereitstellung zurückgegebene `scoringUri` ist der REST-Endpunkt für ein als Webdienst bereitgestelltes Modell. Sie können diesen URI auch mit dem folgenden Befehl abrufen:

```azurecli-interactive
az ml service show -n myservice
```

Dieser Befehl gibt dasselbe JSON-Dokument zurück, einschließlich des `scoringUri`.

Der REST-Endpunkt kann verwendet werden, um Daten an den Dienst zu senden. Informationen zum Erstellen einer Clientanwendung, die Daten an den Dienst sendet, finden Sie unter [Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells](how-to-consume-web-service.md).

### <a name="send-data-to-the-service"></a>Senden von Daten an den Dienst

Obwohl Sie eine Clientanwendung erstellen können, um den Endpunkt aufzurufen, bietet die Machine Learning-CLI ein Hilfsprogramm, das als Testclient fungieren kann. Verwenden Sie den folgenden Befehl, um Testdaten an den Dienst zu senden:

```azurecli-interactive
az ml service run -n myservice -d '{"data":[[1,2,3,4,5,6,7,8,9,10]]}'
```

Die Antwort des Befehls ähnelt `[4684.920839774082]`.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

> [!IMPORTANT]
> Die von Ihnen erstellten Ressourcen können ggf. auch in anderen Azure Machine Learning-Tutorials und -Anleitungen verwendet werden.

### <a name="delete-deployed-service"></a>Löschen bereitgestellter Dienste

Wenn Sie planen, den Azure Machine Learning-Arbeitsbereich weiterhin zu nutzen, aber den bereitgestellten Dienst zur Senkung der Kosten entfernen möchten, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az ml service delete -n myservice
```

Dieser Befehl gibt ein JSON-Dokument zurück, das den Namen des gelöschten Dienstes enthält. Es kann einige Minuten dauern, bis der Dienst gelöscht wird.

### <a name="delete-the-training-compute"></a>Löschen des Computeziels für das Training

Wenn Sie planen, den Azure Machine Learning-Arbeitsbereich weiterhin zu nutzen, aber das für das Training erstellte `cpu`-Computeziel entfernen möchten, verwenden Sie den folgenden Befehl:

```azurecli-interactive
az ml computetarget delete -n cpu
```

Dieser Befehl gibt ein JSON-Dokument zurück, das die ID des gelöschten Computeziels enthält. Es kann einige Minuten dauern, bis das Computeziel gelöscht wird.

### <a name="delete-everything"></a>Alles löschen

Wenn Sie die erstellten Ressourcen nicht mehr benötigen, löschen Sie sie, damit Ihnen keine Zusatzkosten entstehen.

Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle in diesem Dokument erstellten Azure-Ressourcen zu löschen. Ersetzen Sie `<resource-group-name>` durch den Namen der Ressourcengruppe, die Sie zuvor erstellt haben:

```azurecli-interactive
az group delete -g <resource-group-name> -y
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Azure Machine Learning-Tutorial haben Sie die Machine Learning-CLI für die folgenden Aufgaben verwendet:

> [!div class="checklist"]
> * Installieren der Erweiterung für maschinelles Lernen
> * Erstellen eines Azure Machine Learning-Arbeitsbereichs
> * Erstellen der zum Trainieren des Modells verwendeten Computeressource
> * Starten einer Trainingsausführung
> * Registrieren und Herunterladen eines Modells
> * Bereitstellen des Modells als Webdienst
> * Bewerten von Daten mithilfe des Webdiensts

Weitere Informationen zum Verwenden der Befehlszeilenschnittstelle (CLI) finden Sie unter [Verwenden der CLI-Erweiterung für Azure Machine Learning](reference-azure-machine-learning-cli.md).
