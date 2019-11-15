---
title: Azure Machine Learning in Visual Studio Code
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Azure Machine Learning für Visual Studio Code installieren und mit Azure Machine Learning ein Experiment erstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jimgries
author: greazer
ms.date: 09/20/2019
ms.custom: seodec18
ms.openlocfilehash: dd85f3a495b90b3a1dc9d3f021d3600496792759
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73824354"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Erste Schritte mit Azure Machine Learning für Visual Studio Code

In diesem Artikel erfahren Sie, wie Sie die Erweiterung **Azure Machine Learning für Visual Studio Code** verwenden, um Machine Learning-Modelle zu trainieren und bereitzustellen.

[Azure Machine Learning](overview-what-is-azure-ml.md) rationalisiert die Erstellung, das Training und die Bereitstellung von Machine Learning-Modellen.
+ Für Schulungen bietet der Dienst Unterstützung für die lokale oder die Remoteausführung von Experimenten. Für jedes Experiment können Sie benutzerdefinierte Metriken mehrerer Ausführungen protokollieren, um eine Feinabstimmung der Hyperparameter vorzunehmen.
+ Sie können mit Azure Machine Learning auch auf einfache Weise Machine Learning-Modelle für Ihre Test- und Produktionsanforderungen bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

+ Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) aus.

+ Installieren Sie [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview), einen Code-Editor mit geringen Anforderungen, der unter Windows, Mac und Linux ausgeführt werden kann.

+ [Installieren Sie Python 3.5 oder höher](https://www.anaconda.com/download/).


## <a name="install-the-extension"></a>Installieren der Erweiterung

Beim Installieren der Azure Machine Learning-Erweiterung werden automatisch zwei weitere Erweiterungen installiert. Dabei handelt es sich um die [Azure-Kontoerweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) und die [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-Python.Python)-Erweiterung. Weitere Informationen zum Bearbeiten, Ausführen und Debuggen von Python-Code mit der Python-Erweiterung finden Sie im [Python-Hallo Welt-Tutorial](https://code.visualstudio.com/docs/Python/Python-tutorial).

Installieren der Azure Machine Learning-Erweiterung:

1. Öffnen Sie Visual Studio Code.

1. Wechseln Sie zur Registerkarte „Erweiterungen“, und suchen Sie nach „Azure Machine Learning“.

1. Wählen Sie auf der Registerkarte der Erweiterung die Option **Installieren**.

1. Eine Begrüßungsregisterkarte für die Erweiterung wird in Visual Studio Code geöffnet, und das Azure-Symbol (im folgenden Screenshot rot hervorgehoben) wird der Aktivitätsleiste hinzugefügt.

   ![Azure-Symbol auf der Aktivitätsleiste von Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. Wählen Sie im Dialogfeld die Option **Anmelden** aus, und befolgen Sie die Aufforderung auf dem Bildschirm für die Authentifizierung bei Azure.

   Die Azure-Kontoerweiterung, die zusammen mit der Azure Machine Learning-Erweiterung für Visual Studio Code installiert wurde, hilft Ihnen bei der Authentifizierung bei Ihrem Azure-Konto. Eine Liste mit Befehlen finden Sie auf der Seite für die [Azure-Kontoerweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!TIP]
> Sie können das Installationsprogramm für die Erweiterung auch direkt bei [Azure Machine Learning für Visual Studio Code](https://aka.ms/vscodetoolsforai) (Vorschauversion) herunterladen.

## <a name="quickstart-with-azure-machine-learning"></a>Schnellstart mit Azure Machine Learning
Es gibt mehrere Möglichkeiten zum Ausführen von Trainingsskripts mit Azure Machine Learning. Wenn Sie gerade erst beginnen, sehen Sie sich zunächst an, wie Sie schnell ein Trainingsskript für die Ausführung in Azure übermitteln.

Wenn Sie bereits mit den Azure Machine Learning-Konzepten vertraut sind und weitere Details dazu erhalten möchten, wie Sie sie mit der Erweiterung verwalten und anwenden, finden Sie weitere Informationen weiter unten unter [Ausführliche Informationen zu Azure Machine Learning mit VS Code](./how-to-vscode-tools.md#azure-machine-learning-in-depth-with-vs-code).

## <a name="run-an-existing-python-training-script-in-azure"></a>Ausführen eines vorhandenen Python-Trainingsskripts in Azure
Wenn Sie bereits über ein Trainingsskript verfügen, bietet die Azure Machine Learning-Erweiterung für VS Code nicht nur eine hervorragende Oberfläche für Bearbeitung, Debugging und Quellverwaltung, sondern sie erleichtert auch das Ausführen und Speichern von Metriken für Ihr Skript in Azure.

Lassen Sie uns anfangen. Sie können Ihr eigenes Trainingsskript verwenden, sofern dieses bereit ist, oder das Beispiel [vscode-tools-for-ai repo](https://github.com/microsoft/vscode-tools-for-ai) klonen. Dies ist das öffentliche Repository zum Übermitteln von Problemen mit dieser Erweiterung. Es enthält auch einen kleinen Beispielordner **mnist**, den wir für dieses Beispiel verwenden.

1. Öffnen Sie den Ordner **mnist** in VS Code.

1. Erstellen Sie eine neue Python-Umgebung mithilfe Ihres bevorzugten Pakets für virtuelle Umgebungen oder mithilfe von Anaconda, und installieren Sie die Pakete TensorFlow und NumPy.

1. Wählen Sie in der linken unteren Ecke der VS Code-Statusleiste die neu erstellte Umgebung als Python-Interpreter aus.

1. Öffnen Sie **train.py**, und führen Sie die Datei aus, indem Sie den Debugger öffnen und auf die Schaltfläche „Ausführen“ klicken (oder einfach F5 drücken).

   [![Ausführen des MNIST-Trainings](./media/vscode-tools-for-ai/run-mnist.gif)](./media/vscode-tools-for-ai/run-mnist.gif#lightbox)

Wenn alles ordnungsgemäß installiert ist, wird das Skript ausgeführt, und ein TensorFlow-Modell wird im Ausgabeordner erstellt.

[![Anzeigen des TensorFlow-Modells](./media/vscode-tools-for-ai/show-tensorflow-model.gif)](./media/vscode-tools-for-ai/show-tensorflow-model.gif#lightbox)

Nachdem Sie nun wissen, dass Ihr Skript ordnungsgemäß ausgeführt wird, führen Sie es in Azure aus.

Dies kann problemlos ohne weitere Änderungen an **train.py** erfolgen. Sie können jedoch mit wenigen einfachen Änderungen Azure Machine Learning zum automatischen Verfolgen wichtiger Metriken Ihrer Wahl zu jedem Trainingslauf verwenden.

### <a name="make-azure-aware-of-your-run-metrics"></a>Angeben von Ausführungsmetriken für Azure
So ändern Sie das Projekt, damit Azure wichtige Informationen in Ihren Ausführungen erkennen kann

1. Erstellen Sie eine Datei mit dem Namen **amlrun.py** im selben Ordner wie **train.py**.

    ```Python
    import azureml
    from azureml.core import Run

    # access the Azure ML run
    # init run param to check if running within AML
    def get_AMLRun():
        try:
            run = Run.get_submitted_run()
            return run
        except Exception as e:
            print("Caught = {}".format(e.message))
            return None
    ```

2. Importieren Sie die Datei „amlrun“ in **train.py**.

    ```Python
    ...
    from utils import prepare_data
    from amlrun import get_AMLRun
    ...
    ```
3. Initialisieren Sie das Ausführungsobjekt in **train.py**.

    ```Python
    ...
    init = tf.global_variables_initializer()
    saver = tf.train.Saver()
    run = get_AMLRun()
    ...
    ```
4. Protokollieren Sie Metriken in Azure mit der run.log()-Funktion:

    ```Python
    ...
            acc_val = acc_op.eval(feed_dict = {X: X_test, y: y_test})

            # log accuracies to AML logger if using AML
            if run != None:
                run.log('Validation Accuracy', np.float(acc_val))
                run.log('Training Accuracy', np.float(acc_train))

            print(epoch, '-- Training accuracy:', acc_train, '\b Validation
    ...
    ```
### <a name="run-the-script-in-azure"></a>Führen Sie das Skript in Azure aus.
Das ist alles! Verwenden Sie jetzt einfach die Erweiterung, um Ihr Skript in der Cloud auszuführen. Beachten Sie, dass im folgenden Video zur exemplarischen Vorgehensweise der zeitliche Aufwand für das Erstellen eines neuen Azure ML-Arbeitsbereichs und neuer Computeressourcen sowie für das Ausführen des Trainingsskripts verkürzt dargestellt wird.

   [![Starten eines Azure ML-Experiments](./media/vscode-tools-for-ai/start-golden-path.gif)](./media/vscode-tools-for-ai/start-golden-path.gif#lightbox)

Nachdem Sie auf die Schaltfläche „Run Experiment“ (Experiment ausführen) geklickt haben, beantworten Sie die Eingabeaufforderungen wie folgt:

1. Wählen Sie Ihr Azure-Abonnement aus.
1. Wählen Sie aus, einen *neuen* Azure ML-Arbeitsbereich zu erstellen.
1. Treffen Sie eine Auswahl in einem Satz vorkonfigurierter Vorlagen, um die Python-Umgebung für die Ausführungen zu initialisieren. Die Vorlagen bilden einen Ausgangspunkt und enthalten Einstellungen für Folgendes:
    1. **PyTorch**, **TensorFlow** oder **Scikit-learn**
    1. **Einzelnes** oder **verteiltes** Computetraining
    1. **Allgemein** für benutzerdefinierte Umgebungen
1. Stellen Sie sicher, dass die Liste der pip- und conda-Pakete für Ihr Skript vollständig ist, indem Sie Pakete hinzufügen, die in der Vorlage nicht enthalten sind.
1. Überprüfen Sie die Standardnamen und Spezifikationen für den Testlauf, und klicken Sie in der JSON-Datei auf den Link **Submit Experiment** (Experiment übermitteln). Die JSON-Datei wird nicht gespeichert, da Sie nur vorhanden ist, damit Sie die Experimenteinstellungen vor der Übermittlung überprüfen und ändern können.
1. Warten Sie einfach, während die Erweiterung alles für Sie einrichtet und Ihr Skript ausführt.

    [![Trainieren in der Cloud](./media/vscode-tools-for-ai/run-golden-path.gif)](./media/vscode-tools-for-ai/run-golden-path.gif#lightbox)

Nach wenigen Sekunden werden Sie benachrichtigt, dass das Experiment an Azure übermittelt wurde. Nun können Sie den Status in Azure Machine Learning Studio anzeigen, indem Sie in der VS Code-Benachrichtigung auf den Link **View Experiment Run** (Experimentausführung anzeigen) klicken, oder indem Sie in VS Code auf der Registerkarte „Azure“ auf die Schaltfläche „Aktualisieren“ klicken.

Derzeit wird das Anzeigen von Ausführungsmetriken nur in Studio unterstützt. Mit dem oben genannten Link **View Experiment Run** (Experimentausführung anzeigen) gelangen Sie zur Ausführung, wo die von Ihnen protokollierten Metriken angezeigt werden.
[![Experimentausführung im Portal](./media/vscode-tools-for-ai/experiment-run-on-portal.PNG)](./media/vscode-tools-for-ai/experiment-run-on-portal.PNG#lightbox)

## <a name="azure-machine-learning-in-depth-with-vs-code"></a>Ausführliche Informationen zu Azure Machine Learning mit VS Code

In der obigen exemplarischen Vorgehensweise haben wir ein Experiment über den einfachsten Pfad übermittelt. Wie Ihnen vielleicht aufgefallen ist, minimiert die Erweiterung die Schritte, die Sie zum Ausführen eines Experiments ausführen müssen. In diesem Abschnitt wird beschrieben, wie Sie alle Azure Machine Learning-Konzepte einzeln verwalten und so maximale Kontrolle erlangen.

Bevor Sie mit dem Trainieren und Bereitstellen von Machine Learning-Modellen in Visual Studio Code beginnen, müssen Sie in der Cloud einen [Azure Machine Learning-Arbeitsbereich](concept-workspace.md) erstellen. Dieser Arbeitsbereich enthält Ihre Modelle und Ressourcen.

### <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das Azure-Symbol aus. Die Seitenleiste für Azure Machine Learning wird angezeigt.

    [![Erstellen eines Arbeitsbereichs](./media/vscode-tools-for-ai/create-workspace.gif)](./media/vscode-tools-for-ai/create-workspace.gif#lightbox)


1. Klicken Sie mit der rechten Maustaste auf Ihr Azure-Abonnement, und wählen Sie **Arbeitsbereich erstellen** aus. Standardmäßig wird ein Name mit dem Datum und der Uhrzeit der Erstellung generiert. Ändern Sie den Namen in **TeamWorkspace**, und drücken Sie die EINGABETASTE.

1. Wählen Sie in der Liste eine Ressourcengruppe aus, wenn Sie wissen, welche ausgewählt werden soll, oder erstellen Sie eine neue. Wenn Sie eine neue erstellen, wählen Sie einen Standort aus, der dem Standort, an dem Sie Ihr Modell bereitstellen möchten, am nächsten gelegen ist. In diesem Fall haben wir **USA, Westen 2** ausgewählt.

1. Nachdem Sie die EINGABETASTE drücken, erhält Azure Machine Learning die Anforderung zum Erstellen des Arbeitsbereichs. Sie werden im Visual Studio Code-Benachrichtigungsbereich über den Vorgang benachrichtigt.

1. Erweitern Sie den Knoten „Abonnement“, um den neu erstellten Arbeitsbereich zu suchen.

### <a name="create-an-experiment"></a>Erstellen eines Experiments
In Ihrem Arbeitsbereich können Experimente erstellt werden, um einzelne Modelltrainingsausführungen zu verfolgen und zu analysieren. Ausführungen können in der Azure-Cloud oder auf Ihrem lokalen Computer erfolgen.

1. Erweitern Sie den Arbeitsbereich **TeamWorkspace**. Klicken Sie mit der rechten Maustaste auf den Knoten **Experimente**, und wählen Sie dann im Kontextmenü **Create Experiment** (Experiment erstellen) aus.

1. Geben Sie an der Eingabeaufforderung einen Namen für das Experiment ein. In den Beispielscreenshots hat das Experiment den Namen **MNIST**.

1. Wählen Sie die EINGABETASTE, um das neue Experiment zu erstellen. Das neue Experiment wird in der Struktur als untergeordnetes Element des Knotens **Experimente** angezeigt.

1. In einem Arbeitsbereich können Sie mit der rechten Maustaste auf ein Experiment klicken, um es als das **aktive** Experiment festzulegen. Im **aktiven** Experiment ist dieses Experiment in der Cloud dem Ordner zuordnet, den Sie derzeit in Visual Studio Code geöffnet haben. Dieser Ordner sollte Ihre lokalen Python-Skripts enthalten. Wenn Sie ein aktives Experiment festlegen, werden Schlüsselmetriken für alle Trainingsausführungen im Experiment gespeichert, unabhängig davon, wo sie ausgeführt werden.

    [![Erstellen eines Experiments](./media/vscode-tools-for-ai/create-experiment.gif)](./media/vscode-tools-for-ai/create-experiment.gif#lightbox)


### <a name="create-and-manage-compute-targets"></a>Erstellen und Verwalten von Computezielen

Mit Azure Machine Learning für Visual Studio Code können Sie Ihre Daten vorbereiten, Modelle trainieren und sie sowohl lokal als auch an Remotecomputezielen bereitstellen.

Die Erweiterung unterstützt mehrere Remotecomputeziele für Azure Machine Learning. Weitere Informationen finden Sie in der vollständigen Liste der unterstützten [Computeziele für Azure Machine Learning](how-to-set-up-training-targets.md).

### <a name="create-compute-targets-for-azure-machine-learning-in-visual-studio-code"></a>Erstellen von Computezielen für Azure Machine Learning in Visual Studio Code

Erstellen eines Computeziels:

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das Azure-Symbol aus. Die Seitenleiste für Azure Machine Learning wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Azure Machine Learning-Arbeitsbereich.

1. Klicken Sie unter dem Arbeitsbereichsknoten mit der rechten Maustaste auf den Knoten **Compute**, und wählen Sie **Create Compute** (Compute erstellen) aus.

1. Wählen Sie den Computezieltyp in der Liste aus.

1. Wählen Sie an der Eingabeaufforderung der Befehlspalette eine VM-Größe aus. Sie können die Computemodelle mit Text filtern, z. B. „gpu“.

1. Geben Sie an der Eingabeaufforderung der Befehlspalette einen Namen für das Computeziel ein.

1. Nachdem Sie den Namen eingegeben haben, wird die Computeressource mithilfe von Standardparametern erstellt. Um die Parameter zu ändern, klicken Sie mit der rechten Maustaste auf die neue Computeressource, und wählen Sie **Edit Compute** (Compute bearbeiten) aus.

1. Nehmen Sie im angezeigten JSON-Code die gewünschten Änderungen vor, und klicken Sie dann auf die CodeLens „Speichern und fortfahren“. (Sie können auf der Tastatur **STRG-UMSCHALT-P** drücken, um die Befehlspalette aufzurufen, und dann den Befehl **Azure ML: Speichern und fortfahren** ausführen.)

Hier ist ein Beispiel für das Erstellen und Bearbeiten einer Azure Machine Learning-Computeressource (AMLCompute):

[![Erstellen eines AML-Computeziels in Visual Studio Code](./media/vscode-tools-for-ai/create-remote-compute.gif)](./media/vscode-tools-for-ai/create-remote-compute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>Die Laufzeitkonfigurationsdatei

Um ein Azure Machine Learning-Experiment auf einem Computeziel auszuführen, muss dieses entsprechend konfiguriert werden. Eine Ausführungskonfigurationsdatei ist der Mechanismus, mit dem diese Umgebung angegeben wird.

Im Folgenden finden Sie ein Beispiel dafür, wie Sie eine Ausführungskonfiguration für die oben erstellte Ressource AMLCompute erstellen.

[![Erstellen einer Ausführungskonfiguration für ein Computeziel](./media/vscode-tools-for-ai/create-runconfig.gif)](./media/vscode-tools-for-ai/create-runconfig.gif#lightbox)

Auch zum Ausführen von Azure ML-Experimenten auf Ihrem lokalen Computer ist eine Ausführungskonfigurationsdatei erforderlich. Beim Erstellen einer lokalen Ausführungskonfiguration wird in der verwendeten Python-Umgebung standardmäßig der Pfad zu dem Interpreter verwendet, den Sie in VS Code festgelegt haben.

### <a name="train-and-tune-models"></a>Trainieren und Optimieren von Modellen

Bei der Azure ML-Erweiterung für VS Code gibt es mehrere Möglichkeiten, ein Trainingsskript in einem Experiment auszuführen.

1. Klicken Sie mit der rechten Maustaste auf das Trainingsskript, und wählen Sie **Azure ML: Run as Experiment in Azure** (Azure ML: Als Experiment in Azure ausführen) aus.
1. Klicken Sie auf das Symbolleistensymbol „Run Experiment“ (Experiment ausführen).
1. Klicken Sie mit der rechten Maustaste auf einen Ausführungskonfigurationsknoten.
1. Führen Sie über die VS Code-Befehlspalette den Befehl **Azure ML: Run Experiment** (Azure ML: Experiment ausführen) aus.

So führen Sie ein Azure Machine Learning-Experiment aus

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das Azure-Symbol aus.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Azure Machine Learning-Arbeitsbereich.

1. Erweitern Sie unter dem Knoten „Arbeitsbereich“ den Knoten **Experimente**, und klicken Sie mit der rechten Maustaste auf das Experiment, das Sie ausführen möchten.

1. Wählen Sie **Run Experiment** (Experiment ausführen) aus.

1. Wählen Sie den Namen der Python-Datei aus, die Sie zum Trainieren Ihres Modells ausführen möchten, und drücken Sie die EINGABETASTE, um die Ausführung zu übermitteln. Hinweis: Die ausgewählte Datei muss sich in dem Ordner befinden, den Sie zurzeit in VS Code geöffnet haben.

1. Nach dem Übermitteln der Ausführung wird unter dem ausgewählten Experiment ein **Ausführungsknoten** angezeigt. Über diesen Knoten können Sie den Status Ihrer Ausführungen überwachen. Hinweis: Möglicherweise ist es erforderlich, das Fenster regelmäßig zu aktualisieren, um den aktuellen Status anzuzeigen.

Hier sehen Sie ein Beispiel für das Ausführen eines Experiments auf dem zuvor erstellen Computeziel:

[![Lokales Ausführen eines Experiments](./media/vscode-tools-for-ai/run-experiment.gif)](./media/vscode-tools-for-ai/run-experiment.gif#lightbox)

### <a name="deploy-and-manage-models"></a>Erstellen und Verwalten von Modellen
In Azure Machine Learning können Sie Ihre Machine Learning-Modelle in der Cloud und am Edge bereitstellen und verwalten.

#### <a name="register-your-model-to-azure-machine-learning-from-visual-studio-code"></a>Registrieren Ihres Modells bei Azure Machine Learning in Visual Studio Code

Nachdem Sie Ihr Modell nun trainiert haben, können Sie es in Ihrem Arbeitsbereich registrieren. Registrierte Modelle lassen sich nachverfolgen und bereitstellen.

Registrieren des Modells:

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das Azure-Symbol aus. Die Seitenleiste für Azure Machine Learning wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und den Azure Machine Learning-Arbeitsbereich.

1. Klicken Sie unter dem Knoten „Arbeitsbereich“ mit der rechten Maustaste auf **Modelle**, und wählen Sie **Register Model** (Modell registrieren) aus.

1. Geben Sie in der Befehlspalette einen Modellnamen in das Feld ein.

1. Wählen Sie in der Liste aus, ob Sie eine **Modelldatei** (für einzelne Modelle) oder einen **Modellordner** (für Modelle mit mehreren Dateien, wie etwa TensorFlow) hochladen möchten.

1. Wählen Sie Ihren Ordner oder die Datei aus.

1. Wenn Sie mit der Konfiguration Ihrer Modelleigenschaften fertig sind, wählen Sie im Fenster unten rechts die Option **Senden**.

Hier sehen Sie ein Beispiel für die Registrierung Ihres Modells bei Azure Machine Learning:

[![Registrieren eines Modells bei AML](./media/vscode-tools-for-ai/register-model.gif)](./media/vscode-tools-for-ai/register-model.gif#lightbox)


#### <a name="deploy-your-service-from-visual-studio-code"></a>Bereitstellen des Diensts über Visual Studio Code

In Visual Studio Code können Sie Ihren Webdienst an diesen Orten bereitstellen:
+ Azure Container Instances (ACI) für Tests
+ Azure Kubernetes Service (AKS) für die Produktion

Sie müssen zum Testen nicht vorab einen ACI-Container erstellen, da ACI-Container nach Bedarf erstellt werden. AKS-Cluster müssen allerdings im Voraus konfiguriert werden. Weitere Informationen finden Sie unter [Bereitstellen von Modellen mit Azure Machine Learning](how-to-deploy-and-where.md).

Bereitstellen eines Webdiensts:

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das Azure-Symbol aus. Die Seitenleiste für Azure Machine Learning wird angezeigt.

1. Erweitern Sie in der Strukturansicht Ihr Azure-Abonnement und Ihren Azure Machine Learning-Arbeitsbereich.

1. Erweitern Sie unter dem Arbeitsbereich-Knoten den Knoten **Modelle**.

1. Klicken Sie mit der rechten Maustaste auf das Modell, das Sie bereitstellen möchten, und wählen Sie im Kontextmenü die Option **Deploy Service from Registered Model** (Dienst aus registriertem Modell bereitstellen) aus.

1. Wählen Sie in der Befehlspalette das Computeziel aus, an dem die Bereitstellung erfolgen soll.

1. Geben Sie in der Befehlspalette einen Namen für diesen Dienst in das Feld ein.

1. Drücken Sie in der Befehlspalette die EINGABETASTE auf Ihrer Tastatur, um nach der Skriptdatei zu suchen und sie auszuwählen.

1. Drücken Sie in der Befehlspalette die EINGABETASTE auf Ihrer Tastatur, um nach der Conda-Abhängigkeitsdatei zu suchen und sie auszuwählen.

1. Wenn Sie mit der Konfiguration Ihrer Diensteigenschaften fertig sind, wählen Sie im Fenster unten rechts die Option **Senden**, um die Bereitstellung durchzuführen. In der Diensteigenschaftendatei können Sie eine lokale Docker-Datei oder eine Datei vom Typ „schema.json“ angeben.

Der Webdienst ist nun bereitgestellt.

Hier sehen Sie ein Beispiel für die Bereitstellung eines Webdiensts:

[![Bereitstellen eines Webdiensts](./media/vscode-tools-for-ai/create-image.gif)](./media/vscode-tools-for-ai/create-image.gif#lightbox)

### <a name="experiment-with-additional-features"></a>Experimentieren mit weiteren Features

Sie können über die Befehlspalette auf viele Azure Machine Learning-Features in Visual Studio Code zugreifen. Um die Befehlspalette aufzurufen, drücken Sie STRG+UMSCHALT+P. Von hier aus können Sie nach zusätzlichen Azure ML-Features der Erweiterung suchen.

[![Tastenkombinationen für Azure Machine Learning für Visual Studio Code](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Nächste Schritte

* Eine exemplarische Vorgehensweise zum Trainieren mit Azure Machine Learning außerhalb von Visual Studio Code finden Sie in [Tutorial: Trainieren von Modellen mit Azure Machine Learning](tutorial-train-models-with-aml.md).
* Eine exemplarische Vorgehensweise zum lokalen Bearbeiten, Ausführen und Debuggen von Code finden Sie im [Python-Hello World-Tutorial](https://code.visualstudio.com/docs/Python/Python-tutorial).
