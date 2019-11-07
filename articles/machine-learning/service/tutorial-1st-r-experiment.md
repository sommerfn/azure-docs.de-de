---
title: 'Tutorial: Ihr erstes ML-Experiment mit R'
titleSuffix: Azure Machine Learning
description: In diesem Tutorial werden die grundlegenden Entwurfsmuster von Azure Machine Learning beschrieben. Sie trainieren ein logistisches Regressionsmodell, indem Sie die R-Pakete „azuremlsdk“ und „caret“ verwenden, um die Wahrscheinlichkeit einer tödlichen Verletzung bei einem Autounfall vorherzusagen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: sgilley
author: revodavid
ms.author: davidsmi
ms.date: 11/04/2019
ms.openlocfilehash: bcd1fff61e1612cc3361548527e5ed13affa3ba5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73509274"
---
# <a name="tutorial-train-and-deploy-your-first-model-in-r-with-azure-machine-learning"></a>Tutorial: Trainieren und Bereitstellen Ihres ersten Modells in R mit Azure Machine Learning
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Tutorial werden die grundlegenden Entwurfsmuster von Azure Machine Learning beschrieben.  Sie trainieren ein **caret**-Modell und stellen es bereit, um die Wahrscheinlichkeit eines Todesfalls bei einem Autounfall vorherzusagen. Nach Abschluss dieses Tutorials verfügen Sie über die praktischen Kenntnisse für das R SDK, die Sie für komplexere Experimente und Workflows benötigen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Herstellen einer Verbindung mit Ihrem Arbeitsbereich
> * Laden von Daten und Vorbereiten des Trainings
> * Hochladen von Daten in den Datenspeicher, damit diese für das Remotetraining zur Verfügung stehen
> * Erstellen einer Computeressource
> * Trainieren eines caret-Modells zum Vorhersagen der Wahrscheinlichkeit eines Todesfalls
> * Bereitstellen eines Vorhersageendpunkts
> * Testen des Modells über R

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

## <a name="prerequisites"></a>Voraussetzungen

1. Befolgen Sie die [Installationsanleitung](https://azure.github.io/azureml-sdk-for-r/articles/installation.html), um Folgendes durchzuführen:
    + Installieren von Anaconda
    + Installieren von `azuremlsdk`
    + Installieren des Azure Machine Learning SDK für Python

1. Laden Sie die drei Tutorialdateien von [GitHub](https://github.com/Azure/azureml-sdk-for-r/tree/master/vignettes/train-and-deploy-with-caret) herunter.  Speichern Sie sie im Verzeichnis **tutorials**.

2. Erstellen Sie einen Azure Machine Learning-Arbeitsbereich, und laden Sie die zugehörige Konfigurationsdatei herunter, indem Sie die unten angegebenen Schritte ausführen.


### <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Ein Azure Machine Learning-Arbeitsbereich ist eine grundlegende Cloudressource zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen. Er verknüpft Ihr Azure-Abonnement und Ihre Ressourcengruppe mit einem einfach nutzbaren Objekt im SDK. Falls Sie bereits über einen Azure Machine Learning-Arbeitsbereich verfügen, können Sie direkt mit dem [nächsten Abschnitt](#config) fortfahren. Andernfalls erstellen Sie jetzt einen Arbeitsbereich.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

### <a name="config"></a> Herunterladen der Datei „config.json“

1. Wählen Sie oben auf der Seite des Arbeitsbereichs die Option **„config.json“ herunterladen**.

    ![Herunterladen der Datei „config.json“](media/tutorial-1st-r-experiment/download-config.png)  

1. Fügen Sie diese Datei dem Verzeichnis **tutorials** hinzu.

Sie können das Tutorial jetzt ausführen.

Wir empfehlen Ihnen, RStudio zum Ausführen dieses Tutorials zu verwenden. Wählen Sie in RStudio die Option „File“ > „New Project“ > „Existing Directory“ („Datei“ > „Neues Projekt“ > „Vorhandenes Verzeichnis“) und dann das oben erstellte Verzeichnis **tutorials** aus.

> [!Important]
> Der restliche Inhalt dieses Artikels entspricht dem Inhalt der Datei **train-and-deploy-to-aci.Rmd**.  
> Falls Sie mit RMarkdown vertraut sind, können Sie den Code aus dieser Datei verwenden.  Alternativ können Sie die Codeausschnitte aus der Datei oder diesem Artikel kopieren und in ein R-Skript oder die Befehlszeile einfügen.


## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung
Das Setup für Ihre Entwicklungsarbeit in diesem Tutorial umfasst die folgenden Aktionen:

* Installieren erforderlicher Pakete
* Herstellen einer Verbindung mit einem Arbeitsbereich, damit lokale Computer mit Remoteressourcen kommunizieren können
* Erstellen eines Experiments zum Nachverfolgen Ihrer Ausführungen
* Erstellen eines Remotecomputeziels für das Training

### <a name="install-required-packages"></a>Installieren erforderlicher Pakete
In diesem Tutorial wird davon ausgegangen, dass Sie das Azure ML SDK bereits installiert haben. Importieren Sie das Paket **azuremlsdk**.

```R
library(azuremlsdk)
```

Im Tutorial werden Daten aus dem [**DAAG**-Paket](https://cran.r-project.org/package=DAAG) verwendet. Installieren Sie das Paket, falls es noch nicht vorhanden ist.

```R
install.packages("DAAG")
```

Die Trainings- und Bewertungsskripts (`accidents.R` und `accident_predict.R`) verfügen über einige zusätzliche Abhängigkeiten. Wenn Sie die lokale Ausführung dieser Skripts planen, sollten Sie sicherstellen, dass Sie auch über diese erforderlichen Pakete verfügen.

### <a name="load-your-workspace"></a>Laden Ihres Arbeitsbereichs
Instanziieren Sie ein Arbeitsbereichsobjekt aus Ihrem vorhandenen Arbeitsbereich. Mit dem folgenden Code werden die Arbeitsbereichsdetails aus der Datei **config.json** geladen. Sie können einen Arbeitsbereich auch mit [`get_workspace()`](https://azure.github.io/azureml-sdk-for-r/reference/get_workspace.html) abrufen.

```R
ws <- load_workspace_from_config()
```

### <a name="create-an-experiment"></a>Erstellen eines Experiments
Mit einem Azure ML-Experiment wird eine Gruppierung von Ausführungen nachverfolgt. Hierfür wird normalerweise dasselbe Trainingsskript verwendet. Erstellen Sie ein Experiment, um die Ausführungen zum Trainieren des caret-Modells mit den Unfalldaten nachzuverfolgen.

```R
experiment_name <- "accident-logreg"
exp <- experiment(ws, experiment_name)
```

### <a name="create-a-compute-target"></a>Erstellen eines Computeziels
Unter Verwendung von Azure Machine Learning Compute (AmlCompute), einem verwalteten Dienst, können Datenanalysten Machine Learning-Modelle in Clustern von virtuellen Azure-Computern trainieren. Beispiele hierfür sind unter anderem VMs mit GPU-Unterstützung. In diesem Tutorial erstellen Sie den Cluster „AmlCompute“ mit einem einzelnen Knoten als Trainingsumgebung. Mit dem folgenden Code wird der Computecluster für Sie erstellt, sofern er in Ihrem Arbeitsbereich noch nicht vorhanden ist.

Unter Umständen müssen Sie einige Minuten warten, bis der Computecluster bereitgestellt wurde, sofern dies noch nicht erfolgt ist.

```R
cluster_name <- "rcluster"
compute_target <- get_compute(ws, cluster_name = cluster_name)
if (is.null(compute_target)) {
  vm_size <- "STANDARD_D2_V2" 
  compute_target <- create_aml_compute(workspace = ws,
                                       cluster_name = cluster_name,
                                       vm_size = vm_size,
                                       max_nodes = 1)
}

wait_for_provisioning_completion(compute_target)
```

## <a name="prepare-data-for-training"></a>Aufbereiten von Daten für das Training
Im Tutorial werden Daten aus dem Paket **DAAG** verwendet. Dieses Dataset enthält Daten von mehr als 25.000 Autounfällen in den USA. Darin sind Variablen enthalten, die Sie zum Vorhersagen der Wahrscheinlichkeit eines Todesfalls verwenden können. Importieren Sie die Daten zuerst in R, und transformieren Sie sie für die Analyse in den neuen Datenrahmen `accidents`. Exportieren Sie ihn anschließend in eine `Rdata`-Datei.

```R
library(DAAG)
data(nassCDS)

accidents <- na.omit(nassCDS[,c("dead","dvcat","seatbelt","frontal","sex","ageOFocc","yearVeh","airbag","occRole")])
accidents$frontal <- factor(accidents$frontal, labels=c("notfrontal","frontal"))
accidents$occRole <- factor(accidents$occRole)

saveRDS(accidents, file="accidents.Rd")
```

### <a name="upload-data-to-the-datastore"></a>Hochladen von Daten in den Datenspeicher
Laden Sie Daten in die Cloud hoch, damit über Ihre Umgebung für das Remotetraining darauf zugegriffen werden kann. Jeder Azure ML-Arbeitsbereich verfügt über einen Standarddatenspeicher, in dem die Verbindungsinformationen in dem Azure-Blobcontainer gespeichert werden, der unter dem an den Arbeitsbereich angefügten Speicherkonto bereitgestellt wird. Mit dem folgenden Code werden die oben erstellten Unfalldaten in diesen Datenspeicher hochgeladen.

```R
ds <- get_default_datastore(ws)

target_path <- "accidentdata"
upload_files_to_datastore(ds,
                          list("./accidents.Rd"),
                          target_path = target_path,
                          overwrite = TRUE)
```


## <a name="train-a-model"></a>Trainieren eines Modells

Passen Sie für dieses Tutorial ein logistisches Regressionsmodell an Ihre hochgeladenen Daten an, indem Sie Ihren Remotecomputecluster verwenden. Zum Übermitteln eines Auftrags ist Folgendes erforderlich:

* Vorbereiten des Trainingsskripts
* Erstellen eines Estimators
* Übermitteln des Auftrags

### <a name="prepare-the-training-script"></a>Vorbereiten des Trainingsskripts
Ein Trainingsskript mit dem Namen `accidents.R` wurde für Sie in demselben Verzeichnis wie dieses Tutorial bereitgestellt. Beachten Sie die folgenden Details **im Trainingsskript**, die angegeben wurden, um den Azure ML-Dienst für das Training zu nutzen:

* Für das Trainingsskript wird das Argument `-d` genutzt, um das Verzeichnis mit den Trainingsdaten zu ermitteln. Wenn Sie Ihren Auftrag definieren und später dann senden, verweisen Sie auf den Datenspeicher für dieses Argument. Azure ML stellt den Speicherordner im Remotecluster für den Trainingsauftrag bereit.
* Mit dem Trainingsskript wird die endgültige Genauigkeit als Metrik für die Ausführungsaufzeichnung in Azure ML mit `log_metric_to_run()` protokolliert. Das Azure ML SDK verfügt über Protokollierungs-APIs zum Protokollieren verschiedener Metriken bei Trainingsausführungen. Diese Metriken werden aufgezeichnet und in der Ausführungsaufzeichnung des Experiments dauerhaft gespeichert. Auf die Metriken kann dann jederzeit zugegriffen werden, oder sie können in [Azure Machine Learning Studio](http://ml.azure.com) auf der Seite mit den Ausführungsdetails angezeigt werden. In der [Referenz](https://azure.github.io/azureml-sdk-for-r/reference/index.html#section-training-experimentation) finden Sie alle Protokollierungsmethoden (`log_*()`).
* Das Trainingsskript speichert Ihr Modell in einem Verzeichnis namens **outputs**. Der Ordner `./outputs` erhält von Azure ML eine Sonderbehandlung. Während des Trainings werden in `./outputs` geschriebene Dateien von Azure ML automatisch in Ihre Ausführungsaufzeichnung hochgeladen und dauerhaft als Artefakte gespeichert. Indem Sie das trainierte Modell in `./outputs` speichern, können Sie auch nach Abschluss der Ausführung auf Ihr Modell zugreifen und es abrufen. Der Zugriff auf Ihre Umgebung für das Remotetraining ist dann nicht mehr möglich.

### <a name="create-an-estimator"></a>Erstellen eines Estimators

Mit einem Azure ML-Estimator werden die Informationen für die Ausführungskonfiguration gekapselt, die zum Ausführen eines Trainingsskripts auf dem Computeziel benötigt werden. Azure ML-Ausführungen werden als Containeraufträge auf dem angegebenen Computeziel ausgeführt. Standardmäßig enthält das für Ihren Trainingsauftrag erstellte Docker-Image R, das Azure ML SDK und eine Reihe von häufig verwendeten R-Paketen. Hier ist die vollständige Liste mit den Standardpaketen angegeben.

Definieren Sie Folgendes, um den Estimator zu erstellen:

* Das Verzeichnis mit Ihren Skripts, die für das Training benötigt werden (`source_directory`). Alle Dateien in diesem Verzeichnis werden zur Ausführung auf den bzw. die Clusterknoten hochgeladen. Das Verzeichnis muss Ihr Trainingsskript und alle zusätzlich erforderlichen Skripts enthalten.
* Das Trainingsskript, das ausgeführt wird (`entry_script`).
* Das Computeziel (`compute_target`), in diesem Fall der weiter oben erstellte Cluster „AmlCompute“.
* Die für das Trainingsskript erforderlichen Parameter (`script_params`). Azure ML führt Ihr Trainingsskript als Befehlszeilenskript mit `Rscript` aus. In diesem Tutorial geben Sie ein Argument für das Skript an. Dies ist der Bereitstellungspunkt für das Datenverzeichnis, auf den Sie mit `ds$path(target_path)` zugreifen können.
* Alle Umgebungsabhängigkeiten, die für das Training erforderlich sind. Das für das Training erstellte Docker-Standardimage enthält bereits die drei Pakete (`caret`, `e1071` und `optparse`), die im Trainingsskript benötigt werden.  Daher müssen Sie keine zusätzlichen Informationen angeben. Falls Sie nicht standardmäßig enthaltene R-Pakete verwenden, können Sie den Parameter `cran_packages` des Estimators nutzen, um weitere CRAN-Pakete hinzuzufügen. In der Referenz zu [`estimator()`](https://azure.github.io/azureml-sdk-for-r/reference/estimator.html) sind alle konfigurierbaren Optionen angegeben.

```R
est <- estimator(source_directory = ".",
                 entry_script = "accidents.R",
                 script_params = list("--data_folder" = ds$path(target_path)),
                 compute_target = compute_target
                 )
```

### <a name="submit-the-job-on-the-remote-cluster"></a>Übermitteln des Auftrags für den Remotecluster

Übermitteln Sie Ihren Auftrag abschließend für die Ausführung in Ihrem Cluster. Mit `submit_experiment()` wird ein Run-Objekt zurückgegeben, das Sie dann für die Kommunikation mit der Ausführung verwenden. Insgesamt dauert die erste Ausführung **ungefähr 10 Minuten**. Bei späteren Ausführungen wird aber dasselbe Docker-Image wiederverwendet, solange sich die Skriptabhängigkeiten nicht ändern.  In diesem Fall wird das Image zwischengespeichert, und der Container weist eine deutlich kürzere Startdauer auf.

```R
run <- submit_experiment(exp, est)
```

Sie können die Details der Ausführung im RStudio-Viewer anzeigen. Wenn Sie auf den angegebenen Link „Webansicht“ klicken, gelangen Sie zu Azure Machine Learning Studio. In dieser Anwendung können Sie die Ausführung auf der Benutzeroberfläche überwachen.

```R
view_run_details(run)
```

Das Modelltraining wird im Hintergrund durchgeführt. Warten Sie, bis das Training des Modells abgeschlossen ist, bevor Sie weiteren Code ausführen.

```R
wait_for_run_completion(run, show_output = TRUE)
```

Sie – und Ihre Kolleginnen und Kollegen mit Zugriff auf den Arbeitsbereich – können mehrere Experimente parallel übermitteln, und Azure ML übernimmt das Planen der Aufgaben im Computecluster. Sie können den Cluster sogar so konfigurieren, dass automatisch zentral auf mehrere Knoten hochskaliert und diese Skalierung dann rückgängig gemacht wird, wenn in der Warteschlange keine Computeaufgaben mehr vorhanden sind. Diese Konfiguration ist ein kostengünstiger Ansatz, um Teams die gemeinsame Nutzung von Computeressourcen zu ermöglichen.

## <a name="retrieve-training-results"></a>Abrufen der Trainingsergebnisse
Nachdem das Training für Ihr Modell abgeschlossen wurde, können Sie auf die Artefakte Ihres Auftrags zugreifen, die in der Ausführungsaufzeichnung gespeichert wurden, z. B. die protokollierten Metriken und das fertige trainierte Modell.

### <a name="get-the-logged-metrics"></a>Abrufen der protokollierten Metriken
Im Trainingsskript `accidents.R` haben Sie eine Metrik Ihres Modells protokolliert: die Genauigkeit der Vorhersagen in den Trainingsdaten. Sie können Metriken in [Studio](https://ml.azure.com) anzeigen oder wie folgt als R-Liste für die lokale Sitzung extrahieren:

```R
metrics <- get_run_metrics(run)
metrics
```

Wenn Sie mehrere Experimente ausgeführt haben (z. B. mit verschiedenen Variablen, Algorithmen oder Hyperparametern), können Sie die Metriken jeder Ausführung verwenden, um die Modelle zu vergleichen und das für die Produktion bestimmte Modell auszuwählen.

### <a name="get-the-trained-model"></a>Abrufen des trainierten Modells
Sie können das trainierte Modell abrufen und die Ergebnisse in Ihrer lokalen R-Sitzung anzeigen. Mit dem folgenden Code wird der Inhalt des Verzeichnisses `./outputs` heruntergeladen, in dem die Modelldatei enthalten ist.

```R
download_files_from_run(run, prefix="outputs/")
accident_model <- readRDS("outputs/model.rds")
summary(accident_model)
```

Es sind einige Faktoren zu erkennen, die zu einer Erhöhung der geschätzten Wahrscheinlichkeit eines tödlichen Unfalls beitragen:

* Höhere Aufprallgeschwindigkeit 
* Männlicher Fahrer
* Älterer Fahrzeuginsasse
* Beifahrer

Für folgende Faktoren ist die Wahrscheinlichkeit eines tödlichen Unfalls geringer:

* Vorhandensein von Airbags
* Vorhandensein von Sicherheitsgurten
* Frontaler Aufprall 

Das Herstellungsjahr des Fahrzeugs hat keine größeren Auswirkungen.

Sie können dieses Modell verwenden, um neue Vorhersagen zu treffen:

```R
newdata <- data.frame( # valid values shown below
 dvcat="10-24",        # "1-9km/h" "10-24"   "25-39"   "40-54"   "55+"  
 seatbelt="none",      # "none"   "belted"  
 frontal="frontal",    # "notfrontal" "frontal"
 sex="f",              # "f" "m"
 ageOFocc=16,          # age in years, 16-97
 yearVeh=2002,         # year of vehicle, 1955-2003
 airbag="none",        # "none"   "airbag"   
 occRole="pass"        # "driver" "pass"
 )

## predicted probability of death for these variables, as a percentage
as.numeric(predict(accident_model,newdata, type="response")*100)
```

## <a name="deploy-as-a-web-service"></a>Bereitstellen als Webdienst

Mit Ihrem Modell können Sie die Gefahr einer tödlichen Verletzung aufgrund einer Kollision vorhersagen. Verwenden Sie Azure ML, um Ihr Modell als Vorhersagedienst bereitzustellen. In diesem Tutorial stellen Sie den Webdienst in [Azure Container Instances](https://docs.microsoft.com/en-us/azure/container-instances/) (ACI) bereit.

### <a name="register-the-model"></a>Registrieren des Modells

Registrieren Sie zuerst das Modell, das Sie in Ihren Arbeitsbereich heruntergeladen haben, mit [`register_model()`](https://azure.github.io/azureml-sdk-for-r/reference/register_model.html). Bei einem registrierten Modell kann es sich um eine beliebige Auflistung von Dateien handeln, aber in diesem Fall ist das R-Modellobjekt ausreichend. Azure ML verwendet das registrierte Modell für die Bereitstellung.

```R
model <- register_model(ws, 
                        model_path = "outputs/model.rds", 
                        model_name = "accidents_model",
                        description = "Predict probablity of auto accident")
```

### <a name="define-the-inference-dependencies"></a>Definieren der Rückschlussabhängigkeiten
Zum Erstellen eines Webdiensts für Ihr Modell müssen Sie zuerst ein Bewertungsskript (`entry_script`) erstellen. Hierbei handelt es sich um ein R-Skript, für das Variablenwerte (im JSON-Format) als Eingabe verwendet werden und eine Vorhersage für Ihr Modell ausgegeben wird. Verwenden Sie für dieses Tutorial die angegebene Bewertungsdatei `accident_predict.R`. Das Bewertungsskript muss eine `init()`-Methode enthalten, mit der Ihr Modell geladen und eine Funktion zurückgegeben wird, für die das Modell zum Treffen einer Vorhersage basierend auf den Eingabedaten verwendet wird. Weitere Informationen finden Sie in der [Dokumentation](https://azure.github.io/azureml-sdk-for-r/reference/inference_config.html#details).

Definieren Sie als Nächstes eine Azure ML-**Umgebung** für die Paketabhängigkeiten Ihres Skripts. Mit einer Umgebung geben Sie R-Pakete an (per CRAN oder auf andere Weise), die für die Ausführung Ihres Skripts benötigt werden. Sie können auch die Werte von Umgebungsvariablen angeben, auf die Ihr Skript verweisen kann, um sein Verhalten zu ändern. Standardmäßig wird von Azure ML das gleiche Docker-Standardimage erstellt, das mit dem Estimator für das Training verwendet wird. Erstellen Sie eine Umgebung ohne spezielle Attribute, da für das Tutorial keine besonderen Anforderungen gelten.

```R
r_env <- r_environment(name = "basic_env")
```

Geben Sie den Parameter `custom_docker_image` an, wenn Sie stattdessen Ihr eigenes Docker-Image für die Bereitstellung verwenden möchten. Alle konfigurierbaren Optionen zum Definieren einer Umgebung finden Sie in der Referenz zu [`r_environment()`](https://azure.github.io/azureml-sdk-for-r/reference/r_environment.html).

Nun ist alles bereit, um eine **Rückschlusskonfiguration** zum Einkapseln Ihres Bewertungsskripts und der Umgebungsabhängigkeiten zu erstellen.

```R
inference_config <- inference_config(
  entry_script = "accident_predict.R",
  environment = r_env)
```

### <a name="deploy-to-aci"></a>Bereitstellen für ACI
In diesem Tutorial stellen Sie Ihren Dienst für ACI bereit. Mit diesem Code wird ein einzelner Container bereitgestellt, um auf eingehende Anforderungen zu reagieren. Dies ist für Tests und geringere Auslastungen geeignet. Weitere konfigurierbare Optionen finden Sie unter [`aci_webservice_deployment_config()`](https://azure.github.io/azureml-sdk-for-r/reference/aci_webservice_deployment_config.html). (Für Produktionsbereitstellungen können Sie auch eine [Bereitstellung für Azure Kubernetes Service](https://azure.github.io/azureml-sdk-for-r/articles/deploy-to-aks/deploy-to-aks.html) durchführen.)

``` R
aci_config <- aci_webservice_deployment_config(cpu_cores = 1, memory_gb = 0.5)
```

Sie stellen Ihr Modell jetzt als Webdienst bereit. Die Bereitstellung **kann mehrere Minuten dauern**. 

```R
aci_service <- deploy_model(ws, 
                            'accident-pred', 
                            list(model), 
                            inference_config, 
                            aci_config)

wait_for_deployment(aci_service, show_output = TRUE)
```

## <a name="test-the-deployed-service"></a>Testen des bereitgestellten Diensts

Nachdem Ihr Modell als Dienst bereitgestellt wurde, können Sie den Dienst mit R testen, indem Sie [`invoke_webservice()`](https://azure.github.io/azureml-sdk-for-r/reference/invoke_webservice.html) verwenden.  Stellen Sie einen neuen Satz mit Daten für die Vorhersage bereit, konvertieren Sie sie in das JSON-Format, und senden Sie sie an den Dienst.

```R
library(jsonlite)

newdata <- data.frame( # valid values shown below
 dvcat="10-24",        # "1-9km/h" "10-24"   "25-39"   "40-54"   "55+"  
 seatbelt="none",      # "none"   "belted"  
 frontal="frontal",    # "notfrontal" "frontal"
 sex="f",              # "f" "m"
 ageOFocc=22,          # age in years, 16-97
 yearVeh=2002,         # year of vehicle, 1955-2003
 airbag="none",        # "none"   "airbag"   
 occRole="pass"        # "driver" "pass"
 )

prob <- invoke_webservice(aci_service, toJSON(newdata))
prob
```

Sie können auch den HTTP-Endpunkt des Webdiensts abrufen, der REST-Clientaufrufe akzeptiert. Sie können diesen Endpunkt für jeden freigeben, der den Webdienst testen oder in eine Anwendung integrieren möchte.

```R
aci_service$scoring_uri
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcen, wenn Sie sie nicht mehr benötigen. Löschen Sie keine Ressourcen, die Sie noch verwenden möchten. 

Löschen des Webdiensts:
```{r delete_service, eval=FALSE}
delete_webservice(aci_service)
```

Löschen des registrierten Modells:
```{r delete_model, eval=FALSE}
delete_model(model)
```

Löschen des Computeclusters:
```{r delete_compute, eval=FALSE}
delete_compute(compute)
```

### <a name="delete-everything"></a>Alles löschen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Sie können die Ressourcengruppe auch behalten und einen einzelnen Arbeitsbereich löschen. Zeigen Sie die Eigenschaften des Arbeitsbereichs an, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun Ihr erstes Azure Machine Learning-Experiment in R abgeschlossen haben, können Sie sich über das [Azure Machine Learning SDK für R](https://azure.github.io/azureml-sdk-for-r/index.html) informieren.

