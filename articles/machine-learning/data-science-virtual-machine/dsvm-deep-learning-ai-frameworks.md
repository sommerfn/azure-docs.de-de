---
title: Deep Learning- und KI-Frameworks
titleSuffix: Azure Data Science Virtual Machine
description: Verfügbare Deep Learning-Frameworks und -Tools unter Azure Data Science Virtual Machine
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: conceptual
ms.date: 10/1/2019
ms.openlocfilehash: fd38bf1f7741c4d610ef43a12d90533d4ac7b703
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71802413"
---
# <a name="deep-learning-and-ai-frameworks-for-the-azure-data-science-vm"></a>Deep Learning- und KI-Frameworks für Azure Data Science VM
Die in DSVM verfügbaren Deep Learning-Frameworks sind nachstehend aufgelistet.

## <a name="caffehttpsgithubcombvlccaffe"></a>[Caffe](https://github.com/BVLC/caffe)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | |
| Unterstützte DSVM-Editionen      | Linux (Ubuntu)     |
| Konfiguration/Installation auf der DSVM  | Caffe ist unter `/opt/caffe` installiert.   Beispiele finden Sie unter `/opt/caffe/examples`.|
| Zur Ausführung      | Melden Sie sich mithilfe von X2Go auf Ihrer VM an, starten Sie ein neues Terminal, und geben Sie Folgendes ein:<br/>`cd /opt/caffe/examples`<br/>`source activate root`<br/>`jupyter notebook`<br/><br/>Daraufhin öffnet sich ein neues Browserfenster mit Beispielnotebooks. Binärdateien werden in /opt/caffe/build/install/bin installiert.<br/><br/>Für die installierte Version von Caffe ist Python 2.7 erforderlich. Sie kann nicht mit der standardmäßig aktivierten Version 3.5 von Python verwendet werden. Um zu Python 2.7 zu wechseln, führen Sie `source activate root` aus, um in die Anaconda-Umgebung zu wechseln.|    

## <a name="caffe2httpsgithubcomcaffe2caffe2"></a>[Caffe2](https://github.com/caffe2/caffe2)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | |
| Unterstützte DSVM-Editionen      | Linux (Ubuntu)     |
| Konfiguration/Installation auf der DSVM  | Caffe2 wird in der Conda-Umgebung von Python 2.7 (root) installiert. |
| Zur Ausführung      | Terminal: Starten Sie Python, und importieren Sie Caffe2. <br/> * JupyterHub: [Stellen Sie eine Verbindung mit JupyterHub her](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine), und navigieren Sie dann zum Caffe2-Verzeichnis. Dort finden Sie Beispielnotebooks. Für einige Notebooks muss im Python-Code der Caffe2-Stamm festgelegt werden. Geben Sie „/opt/caffe2“ ein. |

## <a name="chainerhttpschainerorg"></a>[Chainer](https://chainer.org/)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | 5,2 |
| Unterstützte DSVM-Editionen      | Linux (Ubuntu)     |
| Konfiguration/Installation auf der DSVM  | Chainer wird in Python 3.5 installiert. |
| Zur Ausführung      | Terminal: Aktivieren Sie die Python 3.5-Umgebung, und führen Sie `python` und dann `import chainer` aus. <br/> * JupyterHub: [Stellen Sie eine Verbindung mit JupyterHub her](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine), und navigieren Sie dann zum Chainer-Verzeichnis. Dort finden Sie Beispielnotebooks.| 

## <a name="cuda-cudnn-nvidia-driverhttpsdevelopernvidiacomcuda-toolkit"></a>[CUDA-, cuDNN-, NVIDIA-Treiber](https://developer.nvidia.com/cuda-toolkit)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | 10.0.130|
| Unterstützte DSVM-Editionen      | Windows und Linux   |
| Konfiguration/Installation auf der DSVM  |_nvidia-smi_ steht unter dem Systempfad zur Verfügung.  |
| Zur Ausführung      | Öffnen Sie eine Eingabeaufforderung (unter Windows) oder ein Terminal (unter Linux), und führen Sie dann _nvidia-smi_ aus. |


## <a name="horovodhttpsgithubcomuberhorovod"></a>[Horovod](https://github.com/uber/horovod)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | 0.16.1|
| Unterstützte DSVM-Editionen      | Linux (Ubuntu)   |
| Konfiguration/Installation auf der DSVM  | Horovod wird in Python 3.5 installiert. |
| Zur Ausführung      | Aktivieren Sie die richtige Umgebung am Terminal, und führen Sie dann Python aus. |

## <a name="kerashttpskerasio"></a>[Keras](https://keras.io/)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | 2.2.4 |
| Unterstützte DSVM-Editionen      | Windows und Linux   |
| Konfiguration/Installation auf der DSVM  | Keras wird unter Windows in Python 3.6 und unter Linux in Python 3.5 installiert. |
| Zur Ausführung      | Aktivieren Sie die richtige Umgebung am Terminal, und führen Sie dann Python aus. |

## <a name="microsoft-cognitive-toolkit-cntkhttpsdocsmicrosoftcomcognitive-toolkit"></a>[Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | 2.5.1 |
| Unterstützte DSVM-Editionen      | Windows und Linux   |
| Konfiguration/Installation auf der DSVM  | CNTK wird unter [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition) in Python 3.6 und unter [Linux](./dsvm-languages.md#python-linux-edition) in Python 3.5 installiert. |
| Zur Ausführung      | Terminal: Aktivieren Sie die richtige Umgebung, und führen Sie Python aus. <br/>Jupyter: Stellen Sie eine Verbindung mit [Jupyter](provision-vm.md) oder [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine) her, und öffnen Sie dann das CNTK-Verzeichnis für Beispiele. |

## <a name="pytorchhttpspytorchorg"></a>[PyTorch](https://pytorch.org/)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | 1.2.0 |
| Unterstützte DSVM-Editionen      | Linux |
| Konfiguration/Installation auf der DSVM  | Installiert in [Python 3.5](dsvm-languages.md#python-linux-edition). Jupyter-Beispielnotebooks sind im Lieferumfang enthalten, und die Beispiele befinden sich unter „/dsvm/samples/pytorch“. |
| Zur Ausführung      | Terminal: Aktivieren Sie die richtige Umgebung, und führen Sie dann Python aus.<br/>* [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine): Stellen Sie eine Verbindung her, und öffnen Sie dann das PyTorch-Verzeichnis für Beispiele.  |

## <a name="tensorflowhttpswwwtensorfloworg"></a>[TensorFlow](https://www.tensorflow.org/)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | 1.13 |
| Unterstützte DSVM-Editionen      | Windows, Linux |
| Konfiguration/Installation auf der DSVM  | Installiert in Python 3.5 unter [Linux](dsvm-languages.md#python-linux-edition) und Python 3.6 unter [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition) |
| Zur Ausführung      | Terminal: Aktivieren Sie die richtige Umgebung, und führen Sie dann Python aus. <br/> * Jupyter: Stellen Sie eine Verbindung mit [Jupyter](provision-vm.md) oder [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine) her, und öffnen Sie dann das TensorFlow-Verzeichnis für Beispiele.   |

## <a name="tensorflow-servinghttpswwwtensorfloworgserving"></a>[TensorFlow Serving](https://www.tensorflow.org/serving/)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | 1.12 |
| Unterstützte DSVM-Editionen      | Linux |
| Konfiguration/Installation auf der DSVM  | tensorflow_model_server ist am Terminal verfügbar. |
| Zur Ausführung      |  Beispiele sind [online](https://www.tensorflow.org/serving/) verfügbar.   |


## <a name="theanohttpsgithubcomtheanotheano"></a>[Theano](https://github.com/Theano/Theano)

|    |           |
| ------------- | ------------- |
| Unterstützte Versionen | 1.0.3 |
| Unterstützte DSVM-Editionen      | Linux |
| Konfiguration/Installation auf der DSVM  |Theano ist in der Python 2.7-Umgebung (_root_) sowie in der Python 3.5-Umgebung (_py35_) installiert. |
| Zur Ausführung      |  Terminal: Aktivieren Sie die gewünschte Python-Version („root“ oder „py35“), führen Sie Python aus, und importieren Sie dann Theano.<br/>* Jupyter: Wählen Sie den Python 2.7- oder den Python 3.5-Kernel aus, und importieren Sie dann Theano.  <br/>Zur Umgehung eines aktuellen MKL-Fehlers müssen Sie zunächst die MKL-Threadingebene wie folgt festlegen:<br/><br/>`export MKL_THREADING_LAYER=GNU`  |