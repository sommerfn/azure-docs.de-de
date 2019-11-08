---
title: 'Trainieren und Bereitstellen eines Modells: Machine Learning in Azure IoT Edge | Microsoft-Dokumentation'
description: Trainieren Sie ein Machine Learning-Modell mit Azure Machine Learning, und verpacken Sie das Modell dann als Containerimage, das als Azure IoT Edge-Modul bereitgestellt werden kann.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6e1ee1fda658ef0884975e4055891f705c4f5058
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73493983"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Tutorial: Trainieren und Bereitstellen eines Azure Machine Learning-Modells

> [!NOTE]
> Dieser Artikel ist Teil einer Tutorialreihe zur Verwendung von Azure Machine Learning für IoT Edge. Falls Sie direkt zu diesem Artikel navigiert sind, ist es ratsam, mit dem [ersten Artikel](tutorial-machine-learning-edge-01-intro.md) der Reihe zu beginnen.

In diesem Artikel nutzen wir Azure Notebooks zunächst, um ein Machine Learning-Modell per Azure Machine Learning zu trainieren, und verpacken dieses Modell dann als Containerimage, das als Azure IoT Edge-Modul bereitgestellt werden kann. Von Azure Notebooks wird ein Azure Machine Learning-Arbeitsbereich genutzt. Dabei handelt es sich um eine grundlegende Komponente für das Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen.

Die Aktivitäten in diesem Teil des Tutorials sind auf zwei Notebooks aufgeteilt.

* **01-turbofan\_regression.ipynb:** In diesem Notebook werden die Schritte zum Trainieren und Veröffentlichen eines Modells mit Azure Machine Learning beschrieben. Hier ist eine grobe Aufstellung der Schritte angegeben:

  1. Herunterladen, Vorbereiten und Untersuchen der Trainingsdaten
  2. Verwenden des Dienstarbeitsbereichs zum Erstellen und Ausführen eines Machine Learning-Experiments
  3. Auswerten der Modellergebnisse aus dem Experiment
  4. Veröffentlichen des besten Modells im Dienstarbeitsbereich

* **02-turbofan\_deploy\_model.ipynb:** Bei diesem Notebook wird das im vorherigen Notebook erstellte Modell für die Erstellung eines Containerimages verwendet, das für die Bereitstellung auf einem Azure IoT Edge-Gerät bereit ist.

  1. Erstellen eines Bewertungsskripts für das Modell
  2. Erstellen und Veröffentlichen des Images
  3. Bereitstellen des Images als Webdienst in Azure Container Instances
  4. Verwenden des Webdiensts zum Überprüfen, ob das Modell und das Image wie erwartet funktionieren

Normalerweise werden die Schritte in diesem Artikel von Data Scientists ausgeführt.

## <a name="set-up-azure-notebooks"></a>Einrichten von Azure Notebooks

Wir verwenden Azure Notebooks, um die beiden Jupyter Notebooks und die unterstützenden Dateien zu hosten. Hier erstellen und konfigurieren wir ein Azure Notebooks-Projekt. Falls Sie Jupyter bzw. Azure Notebooks noch nicht verwendet haben, helfen Ihnen diese Dokumente mit Informationen zum Einstieg weiter:

* **Schnellstart:** [Erstellen und Freigeben eines Notebooks](../notebooks/quickstart-create-share-jupyter-notebook.md)
* **Tutorial:** [Erstellen und Ausführen einer Jupyter Notebook-Datei mit Python](../notebooks/tutorial-create-run-jupyter-notebook.md)

Wie auch bei virtuellen Entwicklungscomputern wird mit Azure Notebooks die Einheitlichkeit der Umgebung für die Übung sichergestellt.

> [!NOTE]
> Nach der Einrichtung kann von jedem Computer aus auf den Azure Notebooks-Dienst zugegriffen werden. Während der Einrichtung sollten Sie den virtuellen Entwicklungscomputer nutzen, auf dem alle benötigten Dateien vorhanden sind.

### <a name="create-an-azure-notebooks-account"></a>Erstellen eines Azure Notebooks-Kontos

Azure Notebooks-Konten sind unabhängig von Azure-Abonnements. Sie müssen ein Konto erstellen, um Azure Notebooks verwenden zu können.

1. Navigieren Sie zu [Azure Notebooks](https://notebooks.azure.com).

2. Klicken Sie oben rechts auf der Seite auf **Anmelden**.

3. Melden Sie sich entweder mit Ihrem Geschäfts-, Schul- oder Unikonto (Azure Active Directory) oder Ihrem persönlichen Konto (Microsoft-Konto) an.

4. Falls Sie Azure Notebooks noch nicht verwendet haben, werden Sie aufgefordert, den Zugriff für die Azure Notebooks-App zu gewähren.

5. Erstellen Sie eine Benutzer-ID für Azure Notebooks.

### <a name="upload-jupyter-notebooks-files"></a>Hochladen von Jupyter Notebooks-Dateien

In diesem Schritt erstellen wir ein neues Azure Notebooks-Projekt und laden Dateien in das Projekt hoch. Die hochgeladenen Dateien lauten:

* **01-turbofan\_regression.ipynb**: Jupyter Notebook-Datei, mit der der Prozess zum Herunterladen der von der Geräteumgebung generierten Daten aus dem Azure-Speicherkonto durchlaufen wird. Die Daten für das Trainieren des Klassifizierers werden untersucht und vorbereitet, das Modell wird trainiert, die Daten werden mit dem Testdataset aus der Datei „Test\_FD003.txt“ getestet, und abschließend wird das Klassifizierermodell im Machine Learning Service-Arbeitsbereich gespeichert.

* **02-turbofan\_deploy\_model.ipynb:** Jupyter Notebook zum Durchlaufen des Prozesses für die Verwendung des Klassifizierermodells, das im Machine Learning Service-Arbeitsbereich gespeichert ist, um ein Containerimage zu erzeugen. Nach der Erstellung des Images wird mit dem Notebook der Prozess zum Bereitstellen des Images als Webdienst durchlaufen, damit Sie überprüfen können, ob dies wie gewünscht funktioniert. Das überprüfte Image wird im Teil [Erstellen und Bereitstellen von benutzerdefinierten IoT Edge-Modulen](tutorial-machine-learning-edge-06-custom-modules.md) dieses Tutorials auf unserem IoT Edge-Gerät bereitgestellt.

* **Test\_FD003.txt:** Diese Datei enthält die Daten, die wir beim Überprüfen unseres trainierten Klassifizierers als unseren Testsatz verwenden. Der Einfachheit halber haben wir für dieses Beispiel die Testdaten so genutzt, wie sie ursprünglich bereitgestellt wurden.

* **RUL\_FD003.txt:** Diese Datei enthält die Restlebensdauer (RUL) für den letzten Zyklus jedes Geräts in der Datei „Test\_FD003.txt“. Die Dateien **readme.txt** und **Damage Propagation Modeling.pdf** unter „C:\\source\\IoTEdgeAndMlSample\\data\\Turbofan“ enthalten eine ausführliche Beschreibung der Daten.

* **Utils.py:** Enthält einen Satz mit Python-Hilfsfunktionen für die Arbeit mit Daten. Das erste Notebook enthält eine ausführliche Beschreibung der Funktionen.

* **README.md:** Infodatei, in der die Verwendung der Notebooks beschrieben ist.

Erstellen Sie ein neues Projekt, und laden Sie die Dateien in Ihr Notebook hoch.

1. Wählen Sie in der obersten Menüleiste die Option **Meine Projekte**.

1. Wählen Sie **+ Neues Projekt**. Geben Sie einen Namen und eine ID an. Es ist nicht erforderlich, dass das Projekt öffentlich ist oder über eine Infodatei verfügt.

1. Wählen Sie **Hochladen** und dann **Von Computer**.

1. Wählen Sie **Dateien auswählen**.

1. Navigieren Sie zu **C:\source\IoTEdgeAndMlSample\AzureNotebooks**. Wählen Sie alle Dateien aus, und klicken Sie auf **Öffnen**.

1. Wählen Sie **Hochladen**, um mit dem Upload zu beginnen, und wählen Sie anschließend **Fertig**, wenn der Prozess abgeschlossen ist.

## <a name="run-azure-notebooks"></a>Ausführen von Azure Notebooks

Nachdem das Projekt nun erstellt wurde, können Sie das Notebook **01-turbofan\_regression.ipynb** ausführen.

1. Wählen Sie auf der turbofan-Projektseite die Option **01-turbofan\_regression.ipynb**.

    ![Auswählen des ersten auszuführenden Notebooks](media/tutorial-machine-learning-edge-04-train-model/select-turbofan-regression-notebook.png)

2. Wählen Sie bei entsprechender Aufforderung im Dialogfeld den Python 3.6 Kernel und dann die Option **Set Kernel** (Kernel festlegen).

3. Klicken Sie oben rechts im Notebook auf das Widget **Not Trusted** (Nicht vertrauenswürdig), falls das Notebook als **Not Trusted** (Nicht vertrauenswürdig) aufgeführt ist. Wählen Sie **Trust** (Vertrauen), wenn das Dialogfeld angezeigt wird.

4. Befolgen Sie die Anleitung im Notebook.

    * Mit `Ctrl + Enter` wird eine Zelle ausgeführt.
    * Mit `Shift + Enter` wird eine Zelle ausgeführt und zur nächsten Zelle navigiert.
    * Wenn eine Zelle ausgeführt wird, wird dafür ein Sternchen zwischen eckigen Klammern angezeigt, z. B. **[\*]** . Wenn der Vorgang abgeschlossen ist, wird das Sternchen durch eine Zahl ersetzt und die relevante Ausgabe ggf. unten angezeigt. Da Zellen häufig auf den vorherigen Zellen aufbauen, kann nur jeweils eine Zelle ausgeführt werden.

5. Wechseln Sie zurück auf die Projektseite, nachdem Sie die Ausführung des Notebooks **01-turbofan\_regression.ipynb** abgeschlossen haben.

6. Öffnen Sie **02-turbofan\_deploy\_model.ipynb**, und wiederholen Sie die in diesem Abschnitt angegebenen Schritte, um das zweite Notebook auszuführen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben wir zwei Jupyter Notebooks verwendet, die in Azure Notebooks ausgeführt werden. Hierbei wurden die Daten von den turbofan-Geräten genutzt, um einen Klassifizierer für die Restlebensdauer (RUL) zu trainieren, den Klassifizierer als Modell zu speichern, ein Containerimage zu erstellen und das Image als Webdienst bereitzustellen und zu testen.

Fahren Sie mit dem nächsten Artikel fort, um ein IoT Edge-Gerät zu erstellen.

> [!div class="nextstepaction"]
> [Konfigurieren eines IoT Edge-Geräts](tutorial-machine-learning-edge-05-configure-edge-device.md)
