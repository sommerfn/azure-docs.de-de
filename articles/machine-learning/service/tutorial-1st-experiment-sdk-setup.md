---
title: 'Tutorial: Erstellen Ihres ersten ML-Experiments: Einrichtung'
titleSuffix: Azure Machine Learning service
description: In dieser Tutorialreihe erfahren Sie Schritt für Schritt, wie Sie die ersten Schritte mit dem in Jupyter Notebook-Instanzen ausgeführten Azure Machine Learning Python SDK ausführen.  In Teil 1 wird die Erstellung einer Cloudumgebung für Notebook-Server sowie eines Arbeitsbereichs für das Verwalten Ihrer Experimente und Machine Learning-Modelle erläutert.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 08/28/2019
ms.openlocfilehash: f1003324e9f4b3762b5d8eca703af4a1fbd4613a
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70308819"
---
# <a name="tutorial-get-started-creating-your-first-ml-experiment-with-the-python-sdk"></a>Tutorial: Erste Schritte beim Erstellen Ihres ersten ML-Experiments mit dem Python SDK

In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie das in Jupyter-Notebooks ausgeführte Azure Machine Learning Python SDK einrichten. Dieses Tutorial ist der **erste Teil einer zweiteiligen Tutorialreihe** und behandelt die Einrichtung und Konfiguration der Python-Umgebung sowie die Erstellung eines Arbeitsbereichs zur Verwaltung Ihrer Experimente und Machine Learning-Modelle. Der [**zweite Teil**](tutorial-1st-experiment-sdk-train.md) baut auf diesem Tutorial auf und zeigt, wie Sie mehrere Machine Learning-Modelle trainieren und über das Azure-Portal sowie mithilfe des SDK verwalten.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen eines [Azure Machine Learning-Arbeitsbereichs](concept-workspace.md) für das nächste Tutorial
> * Erstellen einer cloudbasierten Jupyter Notebook-VM, auf der das Azure Machine Learning Python SDK installiert und vorkonfiguriert ist

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Ein Azure Machine Learning-Arbeitsbereich ist eine grundlegende Cloudressource zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen. Er verknüpft Ihr Azure-Abonnement und Ihre Ressourcengruppe mit einem einfach nutzbaren Objekt im SDK. Falls Sie bereits über einen Azure Machine Learning Service-Arbeitsbereich verfügen, fahren Sie direkt mit dem [nächsten Abschnitt](#azure) fort. Andernfalls erstellen Sie jetzt einen Arbeitsbereich.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="azure"></a>Erstellen eines cloudbasierten Notebook-Servers

In diesem Beispiel wird der cloudbasierte Notebook-Server in Ihrem Arbeitsbereich für eine vorkonfigurierte Umgebung ohne Installationsaufwand verwendet. Verwenden Sie [Ihre eigene Umgebung](how-to-configure-environment.md#local), wenn Sie Ihre Umgebung, Pakete und Abhängigkeiten lieber selbst gestalten möchten.

Erstellen Sie in Ihrem Arbeitsbereich eine Cloudressource, um erste Schritte mit Jupyter-Notebooks auszuführen. Bei dieser Ressource handelt es sich um einen cloudbasierten virtuellen Linux-Computer, der mit allem vorkonfiguriert ist, was Sie für die Ausführung von Azure Machine Learning Service benötigen.

1. Öffnen Sie Ihren Arbeitsbereich im [Azure-Portal](https://portal.azure.com/).  Wenn Sie sich nicht sicher sind, wie Sie Ihren Arbeitsbereich im Portal finden können, lesen Sie [Suchen nach Ihren Arbeitsbereich](how-to-manage-workspace.md#view).

1. Wählen Sie auf der Arbeitsbereichseite im Azure-Portal links **Notebook-VMs** aus.

1. Wählen Sie **+Neu** aus, um eine Notebook-VM zu erstellen.

     ![Auswählen der neuen VM](./media/tutorial-1st-experiment-sdk-setup/add-workstation.png)

1. Geben Sie einen Namen für Ihre VM an. 
   + Der Notebook-VM-Name muss zwischen 2 und 16 Zeichen lang sein. Gültige Zeichen sind Buchstaben, Ziffern und Bindestriche.  
   + Der Name muss darüber hinaus im Azure-Abonnement eindeutig sein.

1. Klicken Sie anschließend auf **Erstellen**. Die Einrichtung Ihres virtuellen Computers kann einen Moment dauern.

1. Warten Sie, bis sich der Status in **Wird ausgeführt** ändert.
   Nachdem Ihre VM ausgeführt wird, verwenden Sie den Abschnitt **Notebook-VMs**, um die Jupyter-Webbenutzeroberfläche zu starten.

1. Wählen Sie **Jupyter** in der Spalte **URI** für Ihre VM aus.

    ![Starten des Jupyter Notebook-Servers](./media/tutorial-1st-experiment-sdk-setup/start-server.png)

   Der Link startet Ihren Notebook-Server und öffnet die Jupyter Notebook-Webseite im Browser auf einer neuen Registerkarte.  Dieser Link funktioniert nur für die Person, die die VM erstellt. Jeder Benutzer des Arbeitsbereichs muss seinen eigenen virtuellen Computer erstellen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie folgende Aufgaben ausgeführt:

* Erstellen eines Azure Machine Learning Service-Arbeitsbereichs
* Erstellen und Konfigurieren eines cloudbasierten Notebook-Servers in Ihrem Arbeitsbereich

In **Teil 2** des Tutorials führen Sie den Code in `tutorial-1st-experiment-sdk-train.ipynb` aus, um ein Machine Learning-Modell zu trainieren. 

> [!div class="nextstepaction"]
> [Tutorial: Trainieren Ihres ersten Modells](tutorial-1st-experiment-sdk-train.md)

> [!IMPORTANT]
> Falls Sie weder mit Teil 2 dieses Tutorials noch mit einem anderen Tutorial fortfahren möchten, empfiehlt es sich aus Kostengründen, den [virtuellen Computer für den cloudbasierten Notebook-Server zu beenden](tutorial-1st-experiment-sdk-train.md#clean-up-resources), wenn Sie ihn gerade nicht verwenden.
