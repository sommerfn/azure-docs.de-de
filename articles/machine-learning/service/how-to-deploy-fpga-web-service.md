---
title: Was ist FPGA – wie funktioniert die Bereitstellung?
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie ein Webdienst mit einem Modell bereitgestellt wird, das auf einem FPGA mit Azure Machine Learning für Rückschlüsse mit extrem geringen Latenzzeiten ausgeführt wird.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: jordane
author: jpe316
ms.date: 10/25/2019
ms.custom: seodec18
ms.openlocfilehash: 5e8dc6181660f0c1545df0688e2749f8f0187027
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73496892"
---
# <a name="what-are-field-programmable-gate-arrays-fpga-and-how-to-deploy"></a>Was sind Field Programmable Gate Arrays (FPGA) und wie werden sie bereitgestellt?
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

Dieser Artikel enthält eine Einführung in Field Programmable Gate Arrays (FPGAs) und erläutert, wie Sie Ihre Modelle mit Azure Machine Learning für ein Azure-FPGA bereitstellen. 

FPGAs enthalten ein Array von programmierbaren Logikblöcken sowie eine Hierarchie von neu konfigurierbaren Interconnects. Durch die Interconnects können diese Blöcke nach der Fertigung auf verschiedene Weise konfiguriert werden. Im Vergleich zu anderen Chips bieten FPGAs eine Kombination aus Programmierbarkeit und Leistung.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGAs im Vergleich zu CPU, GPU und ASIC

Der folgende Abbildung und die Tabelle zeigen FPGAs im Vergleich zu anderen Prozessoren.

![Diagramm zu Azure Machine Learning: FPGA-Vergleich](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

|Prozessor||BESCHREIBUNG|
|---|:-------:|------|
|Anwendungsspezifische integrierte Schaltkreise|ASICs|Benutzerdefinierte Schaltkreise, z.B. TPUs (TensorFlow-Prozessoren) von Google, bieten die höchste Effizienz. Sie können nicht neu konfiguriert werden, wenn sich Ihre Anforderungen ändern.|
|Field-Programmable Gate Arrays|FPGAs|FPGAs, wie sie in Azure verfügbar sind, stellen eine fast so gute Leistung wie ASICs bereit. Sie sind auch flexibel und können im Lauf der Zeit erneut konfiguriert werden, um neue Programmlogik zu implementieren.|
|Grafikprozessoren|GPUs|Eine beliebte Wahl für KI-Berechnungen. GPUs bieten Funktionen zur Parallelverarbeitung und sind beim Grafikrendering schneller als CPUs.|
|Zentralprozessoren|CPUs|CPUs sind Allzweckprozessoren, deren Leistung für die Grafik- und Videoverarbeitung nicht optimal ist.|

FPGAs in Azure basieren auf den FPGA-Geräten von Intel, die Data Scientists und Entwickler verwenden, um KI-Echtzeitberechnungen zu beschleunigen. Diese FPGA-fähige Architektur bietet Leistung, Flexibilität und Skalierbarkeit und ist in Azure verfügbar.

FPGAs ermöglichen es, eine geringe Latenzzeit für Echtzeit-Rückschlussanforderungen (oder Modellbewertungsanforderungen) zu erreichen. Asynchrone Anforderungen (Batchverarbeitung) sind nicht erforderlich. Die Batchverarbeitung kann zu Wartezeiten führen, weil mehr Daten verarbeitet werden müssen. Bei Implementierungen von neuronalen Prozessoren ist keine Batchverarbeitung erforderlich. Daher kann die Wartezeit im Vergleich zu CPU- und GPU-Prozessoren um ein Vielfaches geringer ausfallen.

### <a name="reconfigurable-power"></a>Neu konfigurierbare Leistung
Sie können FPGAs für verschiedene Arten von Machine Learning-Modellen neu konfigurieren. Durch diese Flexibilität ist es einfacher, die Anwendungen auf der Grundlage der optimalen numerischen Genauigkeit und des verwendeten Speichermodells zu beschleunigen. Da FPGAs neu konfigurierbar sind, können Sie mit den Anforderungen der sich schnell ändernden KI-Algorithmen Schritt halten.

## <a name="whats-supported-on-azure"></a>In Azure unterstützte Funktionen
Microsoft Azure ist die weltweit größte Cloudinvestition in FPGAs. Mit dieser FPGA-fähigen Hardwarearchitektur werden trainierte neuronale Netze schnell und mit geringerer Wartezeit ausgeführt. Azure kann vortrainierte DNNs (Deep Neural Networks) FPGA-übergreifend parallelisieren, um Ihren Dienst horizontal hochzuskalieren. Die DNNs können als Deep Featurizer zum Transferlernen vortrainiert oder durch aktualisierte Gewichtungen optimiert werden.

FPGAs in Azure unterstützen:

+ Szenarien zur Bildklassifizierung und -erkennung
+ TensorFlow-Bereitstellung
+ Intel FPGA-Hardware 

Diese DNN-Modelle sind derzeit verfügbar:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16
  - SSD-VGG

FPGAs sind in diesen Azure-Regionen verfügbar:
  - East US
  - Asien, Südosten
  - Europa, Westen
  - USA, Westen 2

> [!IMPORTANT]
> Um die Latenz und den Durchsatz zu optimieren, sollte sich Ihr Client, der Daten an das FPGA-Modell sendet, in einer der oben genannten Regionen befinden (in derjenigen Region, in der Sie das Modell bereitgestellt haben).

Die **PBS-Familie virtueller Azure-Computer** enthält Intel Arria 10 FPGAs. In der Azure-Kontingentzuteilung wird dafür „Standard PBS Family vCPUs“ angezeigt. Der virtuelle PB6-Computer verfügt über sechs vCPUs und ein FPGA. Er wird von Azure ML automatisch im Rahmen der Bereitstellung eines Modells in einem FPGA bereitgestellt. Er wird nur mit Azure ML verwendet und kann keine beliebigen Bitströme ausführen. Sie können beispielsweise keine Bitströme auf dem FPGA für Verschlüsselung, Codierung usw. einspielen.

### <a name="scenarios-and-applications"></a>Szenarien und Anwendungen

Azure-FPGAs sind in Azure Machine Learning integriert. Microsoft verwendet FPGAs für die DNN-Auswertung, die Rangfolge bei der Bing-Suche und die Beschleunigung von Software Defined Networking (SDN), um die Latenz zu verringern und gleichzeitig CPUs für andere Aufgaben freizugeben.

In den folgenden Szenarien werden FPGAs verwendet:
+ [Automatisiertes optisches Untersuchungssystem](https://blogs.microsoft.com/ai/build-2018-project-brainwave/)

+ [Landnutzungskartierung](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/)



## <a name="example-deploy-models-on-fpgas"></a>Beispiel: Bereitstellen von Modellen auf FPGAs 

Sie können ein Modell als Webdienst auf FPGAs mit hardwarebeschleunigten Azure Machine Learning-Modellen bereitstellen. Verwenden von FPGAs bietet Rückschlüsse mit extrem geringen Latenzzeiten, sogar mit einer einzigen Batchgröße. Rückschlüsse oder Modellbewertungen stellen die Phase dar, in der das bereitgestellte Modell für die Vorhersage verwendet wird (meist für Produktionsdaten).


### <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement.  Wenn Sie keins besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

- FPGA-Kontingent. Überprüfen Sie mithilfe der Azure CLI, ob Sie über ein Kontingent verfügen:

    ```shell
    az vm list-usage --location "eastus" -o table --query "[?localName=='Standard PBS Family vCPUs']"
    ```

    > [!TIP]
    > Die anderen möglichen Standorte sind ``southeastasia``, ``westeurope`` und ``westus2``.

    Der Befehl gibt einen Text ähnlich dem folgenden zurück:

    ```text
    CurrentValue    Limit    LocalName
    --------------  -------  -------------------------
    0               6        Standard PBS Family vCPUs
    ```

    Stellen Sie sicher, dass unter __CurrentValue__ mindestens 6 vCPUs vorhanden sind.

    Wenn Sie kein Kontingent besitzen, senden Sie das Anforderungsformular auf [https://aka.ms/accelerateAI](https://aka.ms/accelerateAI).

- Ein Azure Machine Learning-Arbeitsbereich und das Azure Machine Learning SDK für Python müssen installiert sein. Weitere Informationen finden Sie unter [Erstellen eines Arbeitsbereichs](how-to-manage-workspace.md).
 
- Python SDK für hardwarebeschleunigte Modelle:

    ```shell
    pip install --upgrade azureml-accel-models
    ```


## <a name="1-create-and-containerize-models"></a>1. Erstellen und Containerisieren von Modellen

Dieses Dokument beschreibt, wie ein TensorFlow-Diagramm erstellt wird, um das Eingabebild vorzuverarbeiten, es mit ResNet 50 auf einem FPGA als Feature zu erklären und die Features dann über einen anhand eines ImageNet-Datasets trainierten Klassifizierer auszuführen.

Befolgen Sie die Anweisungen für folgende Punkte:

* Definieren des TensorFlow-Modells
* Konvertieren des Modells
* Bereitstellen des Modells
* Nutzen des bereitgestellten Modells
* Löschen bereitgestellter Dienste

Verwenden Sie das [Azure Machine Learning SDK für Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), um eine Dienstdefinition zu erstellen. Eine Dienstdefinition ist eine Datei, die eine Pipeline mit auf TensorFlow basierenden Diagrammen (Eingabe, Featurizer und Klassifizierung) beschreibt. Durch den Bereitstellungsbefehl werden die Definition und die Diagramme automatisch in einer ZIP-Datei komprimiert, die dann in Azure Blob Storage hochgeladen wird. Das DNN wurde bereits zur Ausführung im FPGA bereitgestellt.

### <a name="load-azure-ml-workspace"></a>Laden des Azure ML-Arbeitsbereichs

Laden Sie Ihren Azure ML-Arbeitsbereich.

```python
import os
import tensorflow as tf

from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep='\n')
```

### <a name="preprocess-image"></a>Vorverarbeiten des Image

Die Eingabe an den Webdienst ist ein JPEG-Bild.  Der erste Schritt besteht im Decodieren und Vorverarbeiten des JPEG-Bilds.  JPEG-Bilder werden als Zeichenfolgen behandelt, und das Ergebnis sind Tensor-Elemente, die als Eingaben für das ResNet 50-Modell verwendet werden.

```python
# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.accel.models.utils as utils
tf.reset_default_graph()

in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="load-featurizer"></a>Laden des Featurizer

Initialisieren Sie das Modell und laden Sie einen TensorFlow-Checkpoint der quantisierten Version von ResNet50 herunter, der als Featurizer verwendet werden soll.  Sie können „QuantizedResnet50“ im Codeausschnitt unten ersetzen, indem Sie andere Deep Neural Networks importieren:

- QuantizedResnet152
- QuantizedVgg16
- Densenet121

```python
from azureml.accel.models import QuantizedResnet50
save_path = os.path.expanduser('~/models')
model_graph = QuantizedResnet50(save_path, is_frozen=True)
feature_tensor = model_graph.import_graph_def(image_tensors)
print(model_graph.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Hinzufügen des Klassifizierers

Dieser Klassifizierer wurde anhand eines ImageNet-Datasets trainiert.  Beispiele für Übertragungslernen und das Training Ihrer individuellen Gewichtungen finden Sie in der Sammlung der [Beispielnotebooks](https://aka.ms/aml-notebooks).

```python
classifier_output = model_graph.get_default_classifier(feature_tensor)
print(classifier_output)
```

### <a name="save-the-model"></a>Speichern des Modells

Nachdem nun der Präprozessor, der ResNet 50-Featurizer und der Klassifizierer geladen wurden, speichern Sie das Diagramm und die zugehörigen Variablen als Modell.

```python
model_name = "resnet50"
model_save_path = os.path.join(save_path, model_name)
print("Saving model in {}".format(model_save_path))

with tf.Session() as sess:
    model_graph.restore_weights(sess)
    tf.saved_model.simple_save(sess, model_save_path,
                               inputs={'images': in_images},
                               outputs={'output_alias': classifier_output})
```

### <a name="save-input-and-output-tensors"></a>Speichern von Eingabe- und Ausgabetensoren
Die Eingabe- und Ausgabetensoren, die bei der Vorverarbeitung und der Klassifizierung erstellt wurden, sind für Modellkonvertierung und -rückschluss erforderlich.

```python
input_tensors = in_images.name
output_tensors = classifier_output.name

print(input_tensors)
print(output_tensors)
```

> [!IMPORTANT]
> Speichern Sie die Eingabe- und Ausgabetensoren. Sie benötigen sie für Modellkonvertierungs- und Modellrückschlussanforderungen.

Die verfügbaren Modelle und die entsprechenden standardmäßigen Klassifizierungsausgabetensoren sind nachfolgend aufgeführt. Diese verwenden Sie für Rückschlüsse, wenn Sie die Standardklassifizierung verwendet haben.

+ Resnet50, QuantizedResnet50
  ```python
  output_tensors = "classifier_1/resnet_v1_50/predictions/Softmax:0"
  ```
+ Resnet152, QuantizedResnet152
  ```python
  output_tensors = "classifier/resnet_v1_152/predictions/Softmax:0"
  ```
+ Densenet121, QuantizedDensenet121
  ```python
  output_tensors = "classifier/densenet121/predictions/Softmax:0"
  ```
+ Vgg16, QuantizedVgg16
  ```python
  output_tensors = "classifier/vgg_16/fc8/squeezed:0"
  ```
+ SsdVgg, QuantizedSsdVgg
  ```python
  output_tensors = ['ssd_300_vgg/block4_box/Reshape_1:0', 'ssd_300_vgg/block7_box/Reshape_1:0', 'ssd_300_vgg/block8_box/Reshape_1:0', 'ssd_300_vgg/block9_box/Reshape_1:0', 'ssd_300_vgg/block10_box/Reshape_1:0', 'ssd_300_vgg/block11_box/Reshape_1:0', 'ssd_300_vgg/block4_box/Reshape:0', 'ssd_300_vgg/block7_box/Reshape:0', 'ssd_300_vgg/block8_box/Reshape:0', 'ssd_300_vgg/block9_box/Reshape:0', 'ssd_300_vgg/block10_box/Reshape:0', 'ssd_300_vgg/block11_box/Reshape:0']
  ```

### <a name="register-model"></a>Registrieren des Modells

[Registrieren](./concept-model-management-and-deployment.md) Sie das Modell über das SDK anhand der ZIP-Datei in Azure Blob Storage. Das Hinzufügen von Tags und anderen Metadaten zum Modell ermöglicht Ihnen das Nachverfolgen Ihrer trainierten Modelle.

```python
from azureml.core.model import Model

registered_model = Model.register(workspace=ws,
                                  model_path=model_save_path,
                                  model_name=model_name)

print("Successfully registered: ", registered_model.name,
      registered_model.description, registered_model.version, sep='\t')
```

Wenn Sie ein Modell bereits registriert haben und es laden möchten, können Sie es abrufen.

```python
from azureml.core.model import Model
model_name = "resnet50"
# By default, the latest version is retrieved. You can specify the version, i.e. version=1
registered_model = Model(ws, name="resnet50")
print(registered_model.name, registered_model.description,
      registered_model.version, sep='\t')
```

### <a name="convert-model"></a>Konvertieren des Modells

Konvertieren Sie das TensorFlow-Diagramm in das [ONNX](https://onnx.ai/)-Format (Open Neural Network Exchange).  Sie müssen die Namen der Tensor-Ein- und -Ausgabeelemente angeben. Diese Namen werden von Ihrem Client verwendet, wenn Sie den Webdienst nutzen.

```python
from azureml.accel import AccelOnnxConverter

convert_request = AccelOnnxConverter.convert_tf_model(
    ws, registered_model, input_tensors, output_tensors)

# If it fails, you can run wait_for_completion again with show_output=True.
convert_request.wait_for_completion(show_output=False)

# If the above call succeeded, get the converted model
converted_model = convert_request.result
print("\nSuccessfully converted: ", converted_model.name, converted_model.url, converted_model.version,
      converted_model.id, converted_model.created_time, '\n')
```

### <a name="create-docker-image"></a>Erstellen des Docker-Images

Das konvertierte Modell und alle Abhängigkeiten werden einem Docker-Image hinzugefügt.  Dieses Docker-Image kann dann bereitgestellt und instanziiert werden.  Zu den unterstützten Bereitstellungszielen zählen AKS in der Cloud oder ein Edge-Gerät (etwa [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview)).  Sie können auch Tags und Beschreibungen für das registrierte Docker-Image hinzufügen.

```python
from azureml.core.image import Image
from azureml.accel import AccelContainerImage

image_config = AccelContainerImage.image_configuration()
# Image name must be lowercase
image_name = "{}-image".format(model_name)

image = Image.create(name=image_name,
                     models=[converted_model],
                     image_config=image_config,
                     workspace=ws)
image.wait_for_creation(show_output=False)
```

Listen Sie die Images nach Tag auf, und rufen Sie die ausführlichen Protokolle für Debugzwecke ab.

```python
for i in Image.list(workspace=ws):
    print('{}(v.{} [{}]) stored at {} with build log {}'.format(
        i.name, i.version, i.creation_state, i.image_location, i.image_build_log_uri))
```

## <a name="2-deploy-to-cloud-or-edge"></a>2. Bereitstellen in der Cloud oder in Edge

### <a name="deploy-to-the-cloud"></a>Bereitstellen in der Cloud

Um Ihr Modell als hochgradig skalierbaren Produktionswebdienst bereitzustellen, verwenden Sie Azure Kubernetes Service (AKS). Sie können mit dem Azure Machine Learning SDK, der CLI oder [Azure Machine Learning Studio](https://ml.azure.com) einen neuen erstellen.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Specify the Standard_PB6s Azure VM and location. Values for location may be "eastus", "southeastasia", "westeurope", or "westus2”. If no value is specified, the default is "eastus".
prov_config = AksCompute.provisioning_configuration(vm_size = "Standard_PB6s",
                                                    agent_count = 1,
                                                    location = "eastus")

aks_name = 'my-aks-cluster'
# Create the cluster
aks_target = ComputeTarget.create(workspace=ws,
                                  name=aks_name,
                                  provisioning_configuration=prov_config)
```

Die AKS-Bereitstellung kann etwa 15 Minuten dauern.  Überprüfen Sie, ob die Bereitstellung erfolgreich war.

```python
aks_target.wait_for_completion(show_output=True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

Stellen Sie den Container im AKS-Cluster bereit.
```python
from azureml.core.webservice import Webservice, AksWebservice

# For this deployment, set the web service configuration without enabling auto-scaling or authentication for testing
aks_config = AksWebservice.deploy_configuration(autoscale_enabled=False,
                                                num_replicas=1,
                                                auth_enabled=False)

aks_service_name = 'my-aks-service'

aks_service = Webservice.deploy_from_image(workspace=ws,
                                           name=aks_service_name,
                                           image=image,
                                           deployment_config=aks_config,
                                           deployment_target=aks_target)
aks_service.wait_for_deployment(show_output=True)
```

#### <a name="test-the-cloud-service"></a>Testen des Clouddiensts
Das Docker-Image unterstützt gRPC und die TensorFlow Serving-API „predict“.  Verwenden Sie den Beispielclient zum Aufrufen des Docker-Images, um Vorhersagen aus dem Modell zu erhalten.  Clientbeispielcode ist verfügbar:
- [Python](https://github.com/Azure/aml-real-time-ai/blob/master/pythonlib/amlrealtimeai/client.py)
- [C#](https://github.com/Azure/aml-real-time-ai/blob/master/sample-clients/csharp)

Wenn Sie TensorFlow Serving verwenden möchten, können Sie [einen Beispielclient herunterladen](https://www.tensorflow.org/serving/setup).

```python
# Using the grpc client in Azure ML Accelerated Models SDK package
from azureml.accel import PredictionClient

address = aks_service.scoring_uri
ssl_enabled = address.startswith("https")
address = address[address.find('/')+2:].strip('/')
port = 443 if ssl_enabled else 80

# Initialize AzureML Accelerated Models client
client = PredictionClient(address=address,
                          port=port,
                          use_ssl=ssl_enabled,
                          service_name=aks_service.name)
```

Da diese Klassifizierung mit dem [ImageNet](http://www.image-net.org/)-Dataset trainiert wurde, ordnen Sie die Klassen den für Menschen lesbaren Bezeichnungen zu.

```python
import requests
classes_entries = requests.get(
    "https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()

# Score image with input and output tensor names
results = client.score_file(path="./snowleopardgaze.jpg",
                            input_name=input_tensors,
                            outputs=output_tensors)

# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
```

### <a name="clean-up-the-service"></a>Bereinigen des Diensts
Löschen Sie Ihren Webdienst, das Image und das Modell (muss in dieser Reihenfolge ausgeführt werden, da Abhängigkeiten vorhanden sind).

```python
aks_service.delete()
aks_target.delete()
image.delete()
registered_model.delete()
converted_model.delete()
```

### <a name="deploy-to-a-local-edge-server"></a>Bereitstellung auf einem lokalen Edge-Server

Alle [Azure Data Box-Edge-Geräte](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview
) enthalten ein FPGA für die Ausführung des Modells.  Es kann nur jeweils ein Modell gleichzeitig auf dem FPGA ausgeführt werden.  Um ein anderes Modell auszuführen, stellen Sie einfach einen neuen Container bereit. Anleitungen und Beispielcode finden Sie in [diesem Azure-Beispiel](https://github.com/Azure-Samples/aml-hardware-accelerated-models).

## <a name="secure-fpga-web-services"></a>Sichere FPGA-Webdienste

Zum Absichern von FPGA-Webdiensten lesen Sie das Dokument [Absichern von Webdiensten](how-to-secure-web-service.md).

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich diese Notebooks, Videos und Blogs an:

+ Einige [Beispielnotebooks](https://aka.ms/aml-accel-models-notebooks).

+ [Hyperscalehardware: ML at scale on top of Azure + FPGA (ML mit Skalierung über Azure und FPGA): Build 2018 (Video)](https://channel9.msdn.com/events/Build/2018/BRK3202)

+ [Einblicke in die FPGA-basierte konfigurierbare Cloud von Microsoft (Video)](https://channel9.msdn.com/Events/Build/2017/B8063)

+ [Project Brainwave für KI in Echtzeit: Projektstartseite](https://www.microsoft.com/research/project/project-brainwave/)
