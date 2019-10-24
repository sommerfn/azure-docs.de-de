---
title: Bekannte Probleme und Problembehandlung
titleSuffix: Azure Machine Learning
description: Hier finden Sie eine Liste mit bekannten Problemen, Problemumgehungen und Problembehandlungsinformationen für Azure Machine Learning.
services: machine-learning
author: j-martens
ms.author: jmartens
ms.reviewer: mldocs
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 08/09/2019
ms.custom: seodec18
ms.openlocfilehash: ee7bbff8ab501a1159030a8ee9c57f1c5a64ea22
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286546"
---
# <a name="known-issues-and-troubleshooting-azure-machine-learning"></a>Bekannte Probleme und Problembehandlung für Azure Machine Learning

Dieser Artikel hilft Ihnen, Fehler oder Ausfälle zu ermitteln und zu beheben, die beim Verwenden von Azure Machine Learning auftreten können.

## <a name="visual-interface-issues"></a>Probleme mit der grafischen Benutzeroberfläche

Probleme mit der grafischen Benutzeroberfläche für Machine Learning Service

### <a name="long-compute-preparation-time"></a>Lange Compute-Vorbereitungszeit

Das Erstellen eines neuen Computevorgangs oder das Beenden eines Computevorgangs nimmt einige Minuten oder sogar länger in Anspruch. Das Team arbeitet an der Optimierung.


### <a name="cannot-run-an-experiment-only-contains-dataset"></a>Ein Experiment, das nur ein Dataset enthält, kann nicht ausgeführt werden 

Möglicherweise möchten Sie ein Experiment ausführen, das nur ein Dataset enthält, um das Dataset zu visualisieren. Derzeit ist das Ausführen eines Experiments, das nur ein Dataset enthält, jedoch nicht zulässig. Wir arbeiten aktiv an der Behebung dieses Problems.
 
Als Zwischenlösung können Sie das Dataset mit einem beliebigen Datentransformationsmodul verbinden (Auswählen von Spalten im Dataset, Bearbeiten von Metadaten, Teilen von Daten usw.) und das Experiment ausführen. Anschließend können Sie das Dataset visualisieren. 

Die folgende Abbildung zeigt die Vorgehensweise: ![visulize-data](./media/resource-known-issues/aml-visualize-data.png)

## <a name="sdk-installation-issues"></a>SDK-Installationsprobleme

**Fehlermeldung: 'PyYAML' kann nicht deinstalliert werden.**

Azure Machine Learning SDK für Python: Bei PyYAML handelt es sich um ein mit distutils installiertes Projekt. Daher können wir bei einer teilweisen Deinstallation nicht genau bestimmen, welche Dateien zu ihm gehören. Verwenden Sie diesen Befehl, um die Installation des SDKs fortzusetzen und diesen Fehler zu ignorieren:

```Python
pip install --upgrade azureml-sdk[notebooks,automl] --ignore-installed PyYAML
```

**Fehlermeldung: `ERROR: No matching distribution found for azureml-dataprep-native`**

Die Python 3.7.4-Distribution von Anaconda weist einen Fehler auf, der die azureml-sdk-Installation unterbricht. Dieser Fehler wird in diesem [GitHub-Problem](https://github.com/ContinuumIO/anaconda-issues/issues/11195) behandelt. Zur Problembehandlung können Sie mithilfe des folgenden Befehls eine neue Conda-Umgebung erstellen:
```bash
conda create -n <env-name> python=3.7.3
```
Dadurch wird eine Conda-Umgebung mithilfe von Python 3.7.3 erstellt, bei der das Installationsproblem in 3.7.4 nicht auftritt.

## <a name="trouble-creating-azure-machine-learning-compute"></a>Problem beim Erstellen von Azure Machine Learning Compute

In seltenen Fällen kann es vorkommen, dass Benutzer, die vor dem allgemein verfügbaren Release ihren Azure Machine Learning-Arbeitsbereich über das Azure-Portal erstellt haben, Azure Machine Learning Compute nicht in diesem Arbeitsbereich erstellen können. In einem solchen Fall können Sie entweder eine Supportanfrage für den Dienst erstellen oder über das Portal oder das SDK einen neuen Arbeitsbereich erstellen und die Blockierung dadurch umgehend aufheben.

## <a name="image-building-failure"></a>Fehler bei der Image-Erstellung

Fehler bei der Image-Erstellung beim Bereitstellen des Webdiensts. Die Problemumgehung besteht darin „pynacl==1.2.1“ als pip-Anhängigkeit zur Conda-Datei für die Imagekonfiguration hinzuzufügen.

## <a name="deployment-failure"></a>Fehler bei der Bereitstellung

Wenn `['DaskOnBatch:context_managers.DaskOnBatch', 'setup.py']' died with <Signals.SIGKILL: 9>` angezeigt wird, ändern Sie die SKU für virtuelle Computer in Ihrer Bereitstellung in eine Variante mit mehr Arbeitsspeicher.

## <a name="fpgas"></a>FPGAs

Um Modelle auf FPGAs bereitzustellen, müssen Sie zuerst ein FPGA-Kontingent anfordern und dessen Genehmigung abwarten. Füllen Sie das Formular zur Kontingentanforderung aus, um Zugriff anzufordern: https://aka.ms/aml-real-time-ai

## <a name="automated-machine-learning"></a>Automatisiertes maschinelles Lernen

Das automatisierte maschinelle Lernen von Tensor Flow unterstützt derzeit nicht die Tensor Flow Version 1.13. Die Installation dieser Version führt dazu, dass Paketabhängigkeiten nicht mehr funktionieren. Wir arbeiten daran, dieses Problem in einer zukünftigen Version zu beheben. 

### <a name="experiment-charts"></a>Diagramm mit Experimenten

Seit dem 12. April werden Diagramme für die binäre Klassifizierung (Genauigkeit und Trefferquote, ROC, Gewinnkurve usw.), die in automatisierten ML-Experimentiterationen angezeigt werden, auf der Benutzeroberfläche nicht richtig gerendert. In Diagrammen werden derzeit invertierte Ergebnisse angezeigt, was dazu führt, dass Modelle mit besserer Leistung mit schlechteren Ergebnissen angezeigt werden. Derzeit wird an einer Lösung gearbeitet.

## <a name="databricks"></a>Databricks

Probleme mit Databricks und Azure Machine Learning:

### <a name="failure-when-installing-packages"></a>Fehler beim Installieren von Paketen

Bei der Installation des Azure Machine Learning SDK tritt in Azure Databricks ein Fehler auf, wenn mehrere Pakete installiert werden. Einige Pakete, z.B. `psutil`, können Konflikte verursachen. Um Fehler bei der Installation zu vermeiden, frieren Sie die Bibliotheksversion beim Installieren der Pakete ein. Dieses Problem hängt mit Databricks und nicht mit dem Azure Machine Learning SDK zusammen. Es kann auch mit anderen Bibliotheken auftreten. Beispiel:

```python
psutil cryptography==1.5 pyopenssl==16.0.0 ipython==2.2.0
```

Falls bei Python-Bibliotheken immer wieder Installationsprobleme auftreten, können Sie alternativ Initialisierungsskripts verwenden. Dieser Ansatz wird nicht offiziell unterstützt. Weitere Informationen finden Sie unter [Initialisierungsskripts im Clusterbereich](https://docs.azuredatabricks.net/user-guide/clusters/init-scripts.html#cluster-scoped-init-scripts).

### <a name="cancel-an-automated-machine-learning-run"></a>Abbrechen einer automatisierten Ausführung des maschinellen Lernens

Wenn Sie Funktionen für automatisiertes maschinelles Lernen in Azure Databricks verwenden um eine Ausführung abzubrechen und eine neue Experimentausführung zu starten, starten Sie Ihren Azure Databricks-Cluster neu.

### <a name="10-iterations-for-automated-machine-learning"></a>Mehr als 10 Iterationen von automatisiertem maschinellem Lernen

Sofern in den Einstellungen für automatisiertes maschinelles Lernen mehr als 10 Iterationen vorgesehen sind, legen Sie `show_output` auf `False` fest, wenn Sie die Ausführung übermitteln.

### <a name="widget-for-the-azure-machine-learning-sdkautomated-machine-learning"></a>Widget für das Azure Machine Learning SDK/automatisiertes maschinelles Lernen

Das Azure Machine Learning SDK-Widget wird in Databricks-Notebooks nicht unterstützt, da die Notebooks keine HTML-Widgets analysieren können. Sie können das Widget im Portal anzeigen, indem Sie diesen Python-Code in die Zelle Ihres Azure Databricks-Notebooks einfügen:

```
displayHTML("<a href={} target='_blank'>Azure Portal: {}</a>".format(local_run.get_portal_url(), local_run.id))
```

### <a name="import-error-no-module-named-pandascoreindexes"></a>Importfehler: Kein Modul mit dem Namen „pandas.core.indexes“

Wenn diese Fehlermeldung bei Verwendung von automatisiertem maschinellem Lernen angezeigt wird, gehen Sie folgendermaßen vor:

1. Führen Sie diesen Befehl aus, um zwei Pakete in Ihrem Azure Databricks-Cluster zu installieren: 

   ```
   scikit-learn==0.19.1
   pandas==0.22.0
   ```

1. Trennen Sie den Cluster auf, und fügen Sie ihn dann wieder an Ihr Notebook an. 

Wenn diese Schritte das Problem nicht beheben, versuchen Sie, den Cluster neu zu starten.

### <a name="failtosendfeather"></a>FailToSendFeather

Sollte beim Lesen von Daten aus einem Azure Databricks-Cluster ein Fehler vom Typ `FailToSendFeather` auftreten, haben Sie folgende Möglichkeiten:

* Upgraden Sie das Paket `azureml-sdk[automl_databricks]` auf die aktuelle Version.
* Fügen Sie mindestens die Version 1.1.8 von `azure-dataprep` hinzu.
* Fügen Sie mindestens die Version 0.11 von `pyarrow` hinzu.

## <a name="azure-portal"></a>Azure-Portal

Wenn Sie Ihren Arbeitsbereich direkt über einen Freigabelink aus dem SDK oder dem Portal anzeigen, können Sie die normale Übersichtsseite mit Abonnementinformationen in der Erweiterung nicht anzeigen. Außerdem können Sie nicht zu einem anderen Arbeitsbereich wechseln. Wenn Sie einen anderen Arbeitsbereich anzeigen möchten, müssen Sie direkt zum [Azure-Portal](https://portal.azure.com) navigieren und nach dem Namen des Arbeitsbereichs suchen.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

Manchmal kann es hilfreich sein, Diagnoseinformationen bereitstellen zu können, wenn Sie um Hilfe bitten. Um einige Protokolle anzuzeigen, besuchen Sie das [Azure-Portal](https://portal.azure.com), wechseln Sie zu Ihrem Arbeitsbereich, und wählen Sie **Arbeitsbereich > Experiment > Ausführen > Protokolle** aus.  Diese Informationen finden Sie auch im **Experimente**-Abschnitt auf der [Landing Page Ihres Arbeitsbereichs (Vorschau)](https://ml.azure.com).

> [!NOTE]
> Azure Machine Learning protokolliert während des Trainings auch Informationen aus zahlreichen Quellen, wie z. B. aus AutoML oder dem Docker-Container, in dem der Trainingsauftrag ausgeführt wird. Viele dieser Protokolle sind nicht dokumentiert. Wenn Sie Probleme haben und sich an den Microsoft-Support wenden, können diese Protokolle möglicherweise bei der Problembehandlung verwendet werden.

## <a name="activity-logs"></a>Aktivitätsprotokolle

Einige Aktionen innerhalb des Arbeitsbereichs Azure Machine Learning protokollieren keine Informationen im __Aktivitätsprotokoll__. Beispiele sind das Starten einer Trainingsausführung oder das Registrieren eines Modells.

Einige dieser Aktionen werden im Bereich __Activities__ (Aktivitäten) Ihres Arbeitsbereichs angezeigt, aber es wird nicht angegeben, wer die Aktivität initiiert hat.

## <a name="resource-quotas"></a>Ressourcenkontingente

Erfahren Sie mehr über die [Ressourcenkontingente](how-to-manage-quotas.md), die Ihnen im Rahmen von Azure Machine Learning begegnen können.

## <a name="authentication-errors"></a>Authentifizierungsfehler

Wenn Sie einen Verwaltungsvorgang innerhalb eines Remoteauftrags auf ein Computeziel anwenden, erhalten Sie eine der folgenden Fehlermeldungen:

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Sie erhalten beispielsweise eine Fehlermeldung, wenn Sie versuchen, ein Computeziel anhand einer ML-Pipeline zu erstellen oder anzuhängen, die zur Remoteausführung übermittelt wird.

## <a name="overloaded-azurefile-storage"></a>Überladener AzureFile-Speicher

Wenn Sie eine Fehlermeldung `Unable to upload project files to working directory in AzureFile because the storage is overloaded` erhalten, wenden Sie die folgenden Problemumgehungen an.

Falls Sie eine Dateifreigabe für andere Workloads (beispielsweise die Datenübertragung) verwenden, empfiehlt es sich, Blobs zu verwenden, damit die Dateifreigabe für die Übermittlung von Ausführungen frei ist. Alternativ kann die Workload auch auf zwei verschiedene Arbeitsbereiche aufgeteilt werden.

## <a name="webservices-in-azure-kubernetes-service-failures"></a>Webdienstefehler in Azure Kubernetes Service 

Viele Webdienstefehler in Azure Kubernetes Service können debuggt werden, indem mit `kubectl` eine Verbindung mit dem Cluster hergestellt wird. Sie können die `kubeconfig.json`-Datei für einen Azure Kubernetes Service-Cluster abrufen, indem Sie folgenden Befehl ausführen:

```bash
az aks get-credentials -g <rg> -n <aks cluster name>
```

## <a name="updating-azure-machine-learning-components-in-aks-cluster"></a>Aktualisieren von Azure Machine Learning-Komponenten in einem AKS-Cluster

Updates für Azure Machine Learning-Komponenten, die in einem Azure Kubernetes Service-Cluster installiert sind, müssen manuell angewendet werden. 

> [!WARNING]
> Überprüfen Sie die Version Ihres Azure Kubernetes Service-Clusters, bevor Sie die folgenden Aktionen ausführen. Wenn die Clusterversion größer oder gleich 1.14 ist, können Sie Ihren Cluster nicht erneut mit dem Azure Machine Learning Arbeitsbereich verbinden.

Sie können diese Updates anwenden, indem Sie den Cluster vom Azure Machine Learning-Arbeitsbereich trennen und ihn dann dem Arbeitsbereich erneut zuordnen. Ist SSL im Cluster aktiviert, müssen Sie das SSL-Zertifikat und den privaten Schlüssel bereitstellen, wenn Sie den Cluster erneut zuordnen. 

```python
compute_target = ComputeTarget(workspace=ws, name=clusterWorkspaceName)
compute_target.detach()
compute_target.wait_for_completion(show_output=True)

attach_config = AksCompute.attach_configuration(resource_group=resourceGroup, cluster_name=kubernetesClusterName)

## If SSL is enabled.
attach_config.enable_ssl(
    ssl_cert_pem_file="cert.pem",
    ssl_key_pem_file="key.pem",
    ssl_cname=sslCname)

attach_config.validate_configuration()

compute_target = ComputeTarget.attach(workspace=ws, name=args.clusterWorkspaceName, attach_configuration=attach_config)
compute_target.wait_for_completion(show_output=True)
```

Wenn Sie das SSL-Zertifikat und den privaten Schlüssel nicht mehr haben, oder wenn Sie ein von Azure Machine Learning generiertes Zertifikat verwenden, können Sie die Dateien vor dem Abtrennen des Clusters abrufen, indem Sie mit `kubectl` eine Verbindung mit dem Cluster herstellen und das Geheimnis `azuremlfessl` abrufen.

```bash
kubectl get secret/azuremlfessl -o yaml
```

>[!Note]
>Kubernetes speichert die Geheimnisse im Base64-codierten Format. Sie müssen die `cert.pem`- und die `key.pem`-Komponente jedes Geheimnisses aus dem Base64-Format decodieren, bevor Sie diese Komponenten für `attach_config.enable_ssl` bereitstellen. 

## <a name="recommendations-for-error-fix"></a>Empfehlungen zur Fehlerbehebung
Auf der Grundlage allgemeiner Beobachtungen finden Sie hier Empfehlungen für Azure ML zur Behebung einiger der häufigsten Fehler in Azure ML.

### <a name="moduleerrors-no-module-named"></a>ModuleErrors (Kein Modul benannt)
Wenn Sie beim Übermitteln von Experimenten in Azure ML auf „ModuleErrors“ treffen, bedeutet dies, dass das Trainingsskript die Installation eines Pakets erwartet, es aber nicht hinzugefügt wird. Nachdem Sie den Paketnamen angegeben haben, installiert Azure ML das Paket in der für Ihr Training verwendeten Umgebung. 

Wenn Sie [Estimators](https://docs.microsoft.com/en-us/azure/machine-learning/service/concept-azure-machine-learning-architecture#estimators) verwenden, um Experimente zu übermitteln, können Sie einen Paketnamen über den Parameter `pip_packages` oder `conda_packages` im Estimator auf der Grundlage angeben, aus welcher Quelle Sie das Paket installieren möchten. Sie können auch eine YML-Datei mit allen Ihren Abhängigkeiten mit `conda_dependencies_file` angeben oder alle Ihre pip-Anforderungen in einer TXT-Datei mit dem Parameter `pip_requirements_file` auflisten.

Azure ML bietet auch frameworkspezifische Estimators für Tensorflow, PyTorch, Chainer und SKLearn. Die Verwendung dieser Estimators stellt sicher, dass die Frameworkabhängigkeiten in Ihrem Namen in der für das Training verwendeten Umgebung installiert werden. Sie haben die Möglichkeit, zusätzliche Abhängigkeiten wie oben beschrieben anzugeben. 
 
 Die von Azure ML verwalteten Docker-Images und deren Inhalt werden unter [AzureML-Container](https://github.com/Azure/AzureML-Containers) angezeigt.
Frameworkspezifische Abhängigkeiten sind in der jeweiligen Dokumentation des Frameworks aufgeführt: [Chainer](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py#remarks), [PyTorch](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py#remarks), [TensorFlow](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py#remarks), [SKLearn](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py#remarks).

>[Hinweis!] Wenn Sie der Meinung sind, dass ein bestimmtes Paket häufig genug vorkommt, um in von Azure ML verwalteten Images und Umgebungen hinzugefügt zu werden, erstellen Sie ein GitHub-Problem unter [AzureML-Container](https://github.com/Azure/AzureML-Containers). 
 
 ### <a name="nameerror-name-not-defined-attributeerror-object-has-no-attribute"></a>NameError (Name nicht definiert), AttributeError (Objekt besitzt kein Attribut)
Diese Ausnahme sollte von Ihren Trainingsskripts stammen. Sie können sich die Protokolldateien des Azure-Portals ansehen, um weitere Informationen über den nicht definierten Namen oder den Attributfehler zu erhalten. Aus dem SDK können Sie `run.get_details()` verwenden, um die Fehlermeldung anzuzeigen. Dadurch werden auch alle für Ihre Ausführung generierten Protokolldateien aufgelistet. Werfen Sie unbedingt einen Blick auf das Trainingsskript, und beheben Sie den Fehler, bevor Sie den Vorgang wiederholen. 

### <a name="horovod-is-shutdown"></a>Horovod ist heruntergefahren
In den meisten Fällen bedeutet diese Ausnahme, dass es in einem der Prozesse, die horovod zum Herunterfahren veranlasst haben, eine zugrunde liegende Ausnahme aufgetreten ist. Jeder Rang im MPI-Auftrag erhält eine eigene dedizierte Protokolldatei in Azure ML. Diese Protokolle haben die Bezeichnung `70_driver_logs`. Im Falle von verteiltem Training werden die Protokollnamen durch das Suffix `_rank` ergänzt, um eine einfache Unterscheidung der Protokolle zu ermöglichen. Um den genauen Fehler zu finden, der das Herunterfahren von horovod verursacht hat, gehen Sie alle Protokolldateien durch, und suchen Sie am Ende der driver_log-Dateien nach `Traceback`. Eine dieser Dateien enthält die eigentliche zugrunde liegende Ausnahme. 
