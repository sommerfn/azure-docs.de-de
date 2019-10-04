---
title: Deep Learning- und KI-Frameworks
titleSuffix: Azure Data Science Virtual Machine
description: Verfügbare Deep Learning-Frameworks und -Tools in Azure Data Science Virtual Machine, einschließlich TensorFlow, PyTorch, Keras, Caffe, MXNet, Horovod, Theano, Chainer und mehr.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 08/11/2019
ms.openlocfilehash: d8d351659bcfdcdddcc09831b5f95f580bf38f1e
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71170865"
---
# <a name="deep-learning-and-ai-frameworks-for-azure-data-science-vm"></a>Deep Learning- und KI-Frameworks für Azure Data Science VM
[Data Science Virtual Machine](https://aka.ms/dsvm) (DSVM) unterstützt eine Reihe von Deep Learning-Frameworks zur Unterstützung bei der Erstellung von KI-Anwendungen mit Predictive Analytics und kognitiven Fähigkeiten wie Bild- und Sprachverständnis.

Über DSVM verfügbare Deep Learning-Frameworks sind beispielsweise:

+ TensorFlow
+ PyTorch
+ Keras
+ Caffe
+ Caffe2
+ Torch
+ MXNet Model Server
+ MXNet
+ Horovod
+ Theano
+ Chainer
+ Deep Water
+ NVIDIA DIGITS
+ nvidia-smi
+ TensorFlow Serving
+ TensorRT
+ Microsoft Cognitive Toolkit

|Deep Learning&nbsp;-Tools&nbsp; auf der &nbsp;DSVM|Windows|Linux|Hinweise &nbsp;zur Verwendung|
|---------|-------------------|------------------|-----|
|[TensorFlow](https://www.tensorflow.org/) | Ja (Windows 2016) | Ja |Installiert in Python 3.5 unter [Linux und Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) und Python 3.6 unter [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). Jupyter-Beispielnotebooks sind im Lieferumfang von DSVM enthalten.<br/><br/>**Informationen zur Ausführung**:<br/>* Terminal: Aktivieren Sie die richtige Umgebung, und führen Sie dann Python aus. <br/> * Jupyter: Stellen Sie eine Verbindung mit [Jupyter](provision-vm.md) oder [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine) her, und öffnen Sie dann das TensorFlow-Verzeichnis für Beispiele.  |
|[PyTorch](https://pytorch.org/)| Nein | Ja |Installiert in [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition). Jupyter-Beispielnotebooks sind im Lieferumfang enthalten, und die Beispiele befinden sich unter „/dsvm/samples/pytorch“.    <br/><br/>**Informationen zur Ausführung**:<br/>* Terminal: Aktivieren Sie die richtige Umgebung, und führen Sie dann Python aus.<br/>* [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine): Stellen Sie eine Verbindung her, und öffnen Sie dann das PyTorch-Verzeichnis für Beispiele.  |
|[Keras](https://keras.io/)| Ja | Ja |Die API wird in Python 3.5 unter [Linux und Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) und in Python 3.6 unter [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition) installiert. [Beispiele anzeigen](https://github.com/fchollet/keras/tree/master/examples).<br/><br/>**Informationen zur Ausführung**:<br/>* Terminal: Aktivieren Sie die richtige Umgebung, und führen Sie dann Python aus. <br/> * Jupyter: Laden Sie die Beispiele aus dem GitHub-Speicherort herunter, stellen Sie eine Verbindung mit [Jupyter](provision-vm.md) oder [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine) her, und öffnen Sie dann das Beispielverzeichnis. |
|[Caffe](https://github.com/caffe2/caffe2) | Nein |Ja (Ubuntu)|Caffe ist unter `/opt/caffe` installiert.   Beispiele finden Sie unter `/opt/caffe/examples`. <br/><br/>**Zur Ausführung** melden Sie sich mithilfe von X2Go bei Ihrem virtuellen Computer an. Starten Sie anschließend ein neues Terminal, und geben Sie Folgendes ein:<br/>`cd /opt/caffe/examples`<br/>`source activate root`<br/>`jupyter notebook`<br/><br/>Daraufhin öffnet sich ein neues Browserfenster mit Beispielnotebooks. Binärdateien werden in /opt/caffe/build/install/bin installiert.<br/><br/>Für die installierte Version von Caffe ist Python 2.7 erforderlich. Sie kann nicht mit der standardmäßig aktivierten Version 3.5 von Python verwendet werden. Um zu Python 2.7 zu wechseln, führen Sie `source activate root` aus, um in die Anaconda-Umgebung zu wechseln.|
|[Caffe2](https://github.com/caffe2/caffe2) | Nein |Ja (Ubuntu)|Caffe2 wird in der [Conda-Umgebung von Python 2.7 (root)](dsvm-languages.md#python-linux-and-windows-server-2012-edition) installiert. Die Quelle befindet sich in `/opt/caffe2`.<br/>In JupyterHub sind Beispielnotebooks enthalten.<br/><br/>**Informationen zur Ausführung**:<br/>* Terminal: * Aktivieren Sie die [Python-Umgebung (root)](dsvm-languages.md#python-linux-and-windows-server-2012-edition), starten Sie Python, und importieren Sie dann Caffe2. <br/> * JupyterHub: [Stellen Sie eine Verbindung mit JupyterHub her](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine), und navigieren Sie dann zum Caffe2-Verzeichnis. Dort finden Sie Beispielnotebooks. Für einige Notebooks muss im Python-Code der Caffe2-Stamm festgelegt werden. Geben Sie „/opt/caffe2“ ein. |
|[Torch](http://torch.ch/) | Nein |Ja (Ubuntu)|Torch ist unter `/dsvm/tools/torch` installiert. PyTorch ist in der Python 2.7-Umgebung (_root_) sowie in der Python 3.5-Umgebung (_py35_) installiert. Torch-Beispiele finden Sie unter `/dsvm/samples/torch` und PyTorch-Beispiele unter `/dsvm/samples/pytorch`. |
|[MXNet](https://mxnet.io/) | Ja (Windows 2016) | Ja|MXNet ist unter `C:\dsvm\tools\mxnet` (Windows) bzw. unter `/dsvm/tools/mxnet` (Linux) installiert. Python-Bindungen werden in Python 3.5 unter [Linux und Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) und Python 3.6 unter [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition) installiert. R-Bindungen werden ebenfalls unter Ubuntu installiert.<br/><br/>Jupyter-Beispielnotebooks sind enthalten. <br/><br/>**Informationen zur Ausführung**:<br/>* Terminal: Aktivieren Sie die richtige Umgebung, und führen Sie dann Python aus. <br/> * Jupyter: Stellen Sie eine Verbindung mit [Jupyter](provision-vm.md) oder [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine) her, und öffnen Sie dann das MXNet-Verzeichnis. Dort finden Sie Beispiele.|
|[MXNet Model Server](https://github.com/awslabs/mxnet-model-server) | Nein | Ja |Ein Server zum Erstellen von HTTP-Endpunkten für MXNet- und ONNX.Modelle. _Mxnet-model-server_ ist am Terminal verfügbar. Beispiele auf der [MXNet Model Server-Seite](https://github.com/awslabs/mxnet-model-server).|
|[Horovod](https://github.com/uber/horovod) | Nein | Ja (Ubuntu) |Verteiltes Deep Learning-Framework für TensorFlow. Horovod ist in Python 3.5 unter [Ubuntu](dsvm-languages.md#python-linux-and-windows-server-2012-edition) installiert.  [Beispiele anzeigen](https://github.com/uber/horovod/tree/master/examples).<br/><br/>**Zur Ausführung** aktivieren Sie die richtige Umgebung am Terminal, und führen Sie dann Python aus. |
|[Theano](https://github.com/Theano/Theano) | Nein | Ja (Ubuntu) |Theano ist in der Python 2.7-Umgebung (_root_) sowie in der Python 3.5-Umgebung (_py35_) installiert.<br/><br/>**Informationen zur Ausführung**: <br/>* Terminal: Aktivieren Sie die gewünschte Python-Version („root“ oder „py35“), führen Sie Python aus, und importieren Sie dann Theano.<br/>* Jupyter: Wählen Sie den Python 2.7- oder den Python 3.5-Kernel aus, und importieren Sie dann Theano.  <br/>Zur Umgehung eines aktuellen MKL-Fehlers müssen Sie zunächst die MKL-Threadingebene wie folgt festlegen:<br/><br/>`export MKL_THREADING_LAYER=GNU`|
|[Chainer](https://chainer.org/) |Nein | Ja |Chainer wird in [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) installiert. ChainerRL und ChainerCV sind ebenfalls installiert. <br/><br/>In JupyterHub sind Beispielnotebooks enthalten.<br/><br/>**Informationen zur Ausführung**: <br/>* Terminal: Aktivieren Sie die [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition)-Umgebung, führen Sie _Python_ aus, und importieren Sie dann Chainer. <br/> * JupyterHub: [Stellen Sie eine Verbindung mit JupyterHub her](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine), und navigieren Sie dann zum Chainer-Verzeichnis. Dort finden Sie Beispielnotebooks.|
|[NVidia Digits](https://github.com/NVIDIA/DIGITS) | Nein | Ja (Ubuntu) |Deep Learning-System von NVIDIA zum schnellen Trainieren von Deep Learning-Modellen. DIGITS ist unter `/dsvm/tools/DIGITS` installiert und als Dienst namens _digits_ verfügbar.  <br/><br/>**Informationen zur Ausführung**: <br/>Melden Sie sich mithilfe von X2Go bei dem virtuellen Computer an. Starten Sie den Dienst durch Ausführen von ```sudo systemctl start digits``` an einem Terminal. <br/><br/>Das Starten des Diensts dauert ca. eine Minute. Öffnen Sie einen Webbrowser, und navigieren Sie zu `http://localhost:5000`. Beachten Sie, dass DIGITS keine sichere Anmeldung bereitstellt und daher nicht außerhalb der VM verfügbar gemacht werden sollte.|
|[CUDA-, cuDNN-, NVIDIA-Treiber](https://developer.nvidia.com/cuda-toolkit) |Ja | Ja | |
|nvidia-smi|Ja | Ja |NVIDIA-Tool zum Abfragen der GPU-Aktivität. _nvidia-smi_ ist im Systempfad verfügbar. <br/><br/>Öffnen Sie eine Eingabeaufforderung (unter Windows) oder ein Terminal (unter Linux), und führen Sie dann _nvidia-smi_ aus.|
|[TensorFlow Serving](https://www.tensorflow.org/serving/) | Nein | Ja |Ein Server zum Rückschluss auf ein TensorFlow-Modell. tensorflow_model_server ist am Terminal verfügbar. Beispiele sind [online](https://www.tensorflow.org/serving/) verfügbar.|
|[TensorRT](https://developer.nvidia.com/tensorrt) |  Nein | Ja (Ubuntu) |Ein Deep Learning-Rückschlussserver von NVIDIA. TensorRT wird als _apt_-Paket installiert. Beispiele sind [online](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#samples) verfügbar.|
|[Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/)|Ja | Ja | Installiert in Python 3.5 unter [Linux und Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) und Python 3.6 unter [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). Jupyter-Beispielnotebooks sind im Lieferumfang von DSVM enthalten. <br/><br/>**Informationen zur Ausführung**: <br/>Terminal: Aktivieren Sie die richtige Umgebung, und führen Sie Python aus. <br/>Jupyter: Stellen Sie eine Verbindung mit [Jupyter](provision-vm.md) oder [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine) her, und öffnen Sie dann das CNTK-Verzeichnis für Beispiele. |
|Deep Water|Nein | Ja (Ubuntu) |Das Deep Learning-Framework für H2O wird in [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) installiert und ist auch in `/dsvm/tools/deep_water` verfügbar. In JupyterHub sind Beispielnotebooks enthalten. Deep Water erfordert CUDA 8 mit cuDNN 5.1. Standardmäßig befindet sich dies nicht unter dem Bibliothekspfad, da andere Deep Learning-Frameworks CUDA 9 und cuDNN 7 verwenden. Installieren Sie CUDA 8 und CuDNN 5.1 für Deep Water, um das Tool verwenden zu können:<br/><br/>```export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}```<br/>```export CUDA_ROOT=/usr/local/cuda-8.0```<br/><br/>So verwenden Sie Deep Water<br/>* Terminal: Aktivieren Sie die [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition)-Umgebung, und führen Sie dann _Python_ aus. <br/>* JupyterHub: [Stellen Sie eine Verbindung mit JupyterHub her](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine), und navigieren Sie dann zum deep_water-Verzeichnis. Dort finden Sie Beispielnotebooks.|
