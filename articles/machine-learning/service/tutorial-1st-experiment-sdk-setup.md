---
title: 'Tutorial: Erstellen Ihres ersten ML-Experiments: Einrichtung'
titleSuffix: Azure Machine Learning
description: In dieser Tutorialreihe erfahren Sie Schritt für Schritt, wie Sie die ersten Schritte mit dem in Jupyter Notebook-Instanzen ausgeführten Azure Machine Learning Python SDK ausführen.  In Teil 1 wird die Erstellung einer Cloudumgebung für Notebook-Server sowie eines Arbeitsbereichs für das Verwalten Ihrer Experimente und Machine Learning-Modelle erläutert.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 09/25/2019
ms.openlocfilehash: 3bbda22689bb330acc836173162a64b840f1bbd8
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71828034"
---
# <a name="tutorial-get-started-creating-your-first-ml-experiment-with-the-python-sdk"></a>Tutorial: Erste Schritte beim Erstellen Ihres ersten ML-Experiments mit dem Python SDK

In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie das in Jupyter-Notebooks ausgeführte Azure Machine Learning Python SDK einrichten. Dieses Tutorial ist der **erste Teil einer zweiteiligen Tutorialreihe** und behandelt die Einrichtung und Konfiguration der Python-Umgebung sowie die Erstellung eines Arbeitsbereichs zur Verwaltung Ihrer Experimente und Machine Learning-Modelle. Der [**zweite Teil**](tutorial-1st-experiment-sdk-train.md) baut auf diesem Tutorial auf und zeigt, wie Sie mehrere Machine Learning-Modelle trainieren und über das Azure-Portal sowie mithilfe des SDK verwalten.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen eines [Azure Machine Learning-Arbeitsbereichs](concept-workspace.md) für das nächste Tutorial
> * Klonen Sie das Tutorial-Notebook in Ihrem Ordner im Arbeitsbereich.
> * Erstellen einer cloudbasierten Jupyter Notebook-VM, auf der das Azure Machine Learning Python SDK installiert und vorkonfiguriert ist

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Ein Azure Machine Learning-Arbeitsbereich ist eine grundlegende Cloudressource zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen. Er verknüpft Ihr Azure-Abonnement und Ihre Ressourcengruppe mit einem einfach nutzbaren Objekt im Dienst. 

Sie erstellen einen Arbeitsbereich über das Azure-Portal, einer webbasierten Konsole zum Verwalten Ihrer Azure-Ressourcen. 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> Notieren Sie sich Ihren **Arbeitsbereich** und Ihr **Abonnement**. Sie benötigen diese Informationen, um sicherzustellen, dass Sie Ihr Experiment an der richtigen Stelle erstellen. 


## <a name="azure"></a>Klonen eines Notebook-Ordners

In diesem Beispiel wird der cloudbasierte Notebook-Server in Ihrem Arbeitsbereich für eine vorkonfigurierte Umgebung ohne Installationsaufwand verwendet. Verwenden Sie [Ihre eigene Umgebung](how-to-configure-environment.md#local), wenn Sie Ihre Umgebung, Pakete und Abhängigkeiten lieber selbst gestalten möchten.

Sie führen die folgende Experimenteinrichtung durch und führen Schritte auf der Landing Page des Arbeitsbereichs (Vorschauversion) aus. Diese konsolidierte Oberfläche enthält Tools für maschinelles Lernen zur Durchführung von Data Science-Szenarien für Data Science-Fachleute aller Kenntnisstufen.

1. Melden Sie sich auf der [Angebotsseite des Arbeitsbereichs](https://ml.azure.com/) an.

1. Wählen Sie Ihr Abonnement und den erstellten Arbeitsbereich aus.

1. Wählen Sie links **Notebooks and Files** (Notebooks und Dateien) aus.

1. Öffnen Sie den Ordner **Beispiele**.

1. Wählen Sie rechts vom Ordner **Tutorials** die Auslassungspunkte ( **„...“** ) und anschließend **Klonen** aus.

    ![Klonen des Ordners](media/tutorial-1st-experiment-sdk-setup/clone-tutorials.png)

1. Für jeden Benutzer, der auf den Arbeitsbereich zugreift, wird ein Ordner angezeigt.  Wählen Sie Ihren Ordner aus, um den Ordner **Tutorials** dort zu klonen.

## <a name="a-nameopenselect-a-vm-to-run-the-notebook"></a><a name="open">Auswählen eines virtuellen Computers zum Ausführen des Notebooks

1. Öffnen Sie unter **Benutzerdateien** Ihren Ordner und anschließend den geklonten Ordner **Tutorials**.

    ![Öffnen des Ordners „Tutorials“](media/tutorial-1st-experiment-sdk-setup/expand-user-folder.png)

    > [!IMPORTANT]
    > Im Ordner **Beispiele** können Notebooks angezeigt, aber nicht ausgeführt werden.  Öffnen Sie zum Ausführen eines Notebooks die geklonte Version des Notebooks unbedingt im Abschnitt **Benutzerdateien**.
    
1. Wählen Sie die Datei **tutorial-1st-experiment-sdk-train.ipynb** im Ordner **Tutorials** aus.

1. Wählen Sie auf der oberen Leiste eine Notebook-VM aus, die zum Ausführen des Notebooks verwendet werden soll. Diese VMs werden mit allen Komponenten vorkonfiguriert, die Sie zum Ausführen von Azure Machine Learning benötigen. Sie können einen virtuellen Computer auswählen, der von einem beliebigen Benutzer Ihres Arbeitsbereichs erstellt wurde. 

1. Werden keine virtuellen Computer gefunden, wählen Sie **+ Neue VM** aus, um den virtuellen Computer zu erstellen.

    ![Erstellen einer VM](media/tutorial-1st-experiment-sdk-setup/no-vm.png)

    1. Geben Sie beim Erstellen eines virtuellen Computers einen Namen an.  Der Name muss zwischen 2 und 16 Zeichen lang sein. Gültige Zeichen sind Buchstaben, Ziffern und Bindestriche (-), und der Name muss in Ihrem Azure-Abonnement eindeutig sein.

    1. Klicken Sie anschließend auf **Erstellen**. Das Einrichten Ihres virtuellen Computers dauert ungefähr fünf Minuten.

1. Sobald der virtuelle Computer verfügbar ist, wird er auf der oberen Symbolleiste angezeigt.  Sie können das Notebook jetzt entweder über **Alle ausführen** auf der Symbolleiste oder unter Verwendung von **UMSCHALT+EINGABE** in den Codezellen des Notebooks ausführen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie folgende Aufgaben ausgeführt:

* Erstellen eines Azure Machine Learning-Arbeitsbereichs.
* Erstellen und Konfigurieren eines cloudbasierten Notebook-Servers in Ihrem Arbeitsbereich

In **Teil 2** des Tutorials führen Sie den Code in `tutorial-1st-experiment-sdk-train.ipynb` aus, um ein Machine Learning-Modell zu trainieren. 

> [!div class="nextstepaction"]
> [Tutorial: Trainieren Ihres ersten Modells](tutorial-1st-experiment-sdk-train.md)

> [!IMPORTANT]
> Falls Sie weder mit Teil 2 dieses Tutorials noch mit einem anderen Tutorial fortfahren möchten, empfiehlt es sich aus Kostengründen, den [virtuellen Computer für den cloudbasierten Notebook-Server zu beenden](tutorial-1st-experiment-sdk-train.md#clean-up-resources), wenn Sie ihn gerade nicht verwenden.


