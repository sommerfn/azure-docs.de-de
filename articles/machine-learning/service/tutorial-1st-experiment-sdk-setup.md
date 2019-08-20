---
title: 'Tutorial: Einrichten der Umgebung und des Arbeitsbereichs'
titleSuffix: Azure Machine Learning service
description: In dieser Tutorialreihe erfahren Sie Schritt für Schritt, wie Sie die ersten Schritte mit dem in Jupyter Notebook-Instanzen ausgeführten Azure Machine Learning Python SDK ausführen.  In Teil 1 wird die Erstellung einer Cloudumgebung für Notebook-Server sowie eines Arbeitsbereichs für das Verwalten Ihrer Experimente und Machine Learning-Modelle erläutert.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 07/20/2019
ms.openlocfilehash: 70fee90fb82618a409d2566a3235ad8ca42e1760
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934430"
---
# <a name="tutorial-set-up-environment-and-workspace"></a>Tutorial: Einrichten der Umgebung und des Arbeitsbereichs

In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie das in Jupyter-Notebooks ausgeführte Azure Machine Learning Python SDK einrichten. Dieses Tutorial ist der **erste Teil einer zweiteiligen Tutorialreihe** und behandelt die Einrichtung und Konfiguration der Python-Umgebung sowie die Erstellung eines Arbeitsbereichs zur Verwaltung Ihrer Experimente und Machine Learning-Modelle. Der [**zweite Teil**](tutorial-1st-experiment-sdk-train.md) baut auf diesem Tutorial auf und zeigt, wie Sie mehrere Machine Learning-Modelle trainieren und über das Azure-Portal sowie mithilfe des SDK verwalten.

In diesem Tutorial führen Sie Folgendes durch:

> [!div class="checklist"]
> * Erstellen eines Machine Learning-Arbeitsbereichs für das nächste Tutorial
> * Erstellen einer cloudbasierten Jupyter Notebook-VM, auf der das Azure Machine Learning Python SDK installiert und vorkonfiguriert ist

## <a name="prerequisites"></a>Voraussetzungen

Die einzige Voraussetzung für dieses Tutorial ist ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version des Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Ein Arbeitsbereich ist eine grundlegende Cloudressource zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen. Er verknüpft Ihr Azure-Abonnement und Ihre Ressourcengruppe mit einem einfach nutzbaren Objekt im SDK. Falls Sie bereits über einen Azure Machine Learning Service-Arbeitsbereich verfügen, fahren Sie direkt mit dem [nächsten Abschnitt](#azure) fort. Andernfalls erstellen Sie jetzt einen Arbeitsbereich.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="azure"></a>Erstellen eines cloudbasierten Notebook-Servers

In diesem Beispiel wird der cloudbasierte Notebook-Server in Ihrem Arbeitsbereich für eine vorkonfigurierte Umgebung ohne Installationsaufwand verwendet. Verwenden Sie [Ihre eigene Umgebung](how-to-configure-environment.md#local), wenn Sie Ihre Umgebung, Pakete und Abhängigkeiten lieber selbst gestalten möchten.

Erstellen Sie in Ihrem Arbeitsbereich eine Cloudressource, um erste Schritte mit Jupyter-Notebooks auszuführen. Bei dieser Ressource handelt es sich um einen cloudbasierten virtuellen Linux-Computer, der mit allem vorkonfiguriert ist, was Sie für die Ausführung von Azure Machine Learning Service benötigen.

1. Öffnen Sie Ihren Arbeitsbereich im [Azure-Portal](https://portal.azure.com/).  Wenn Sie sich nicht sicher sind, wie Sie Ihren Arbeitsbereich im Portal finden können, lesen Sie [Suchen nach Ihren Arbeitsbereich](how-to-manage-workspace.md#view).

1. Wählen Sie auf der Arbeitsbereichseite im Azure-Portal links **Notebook-VMs** aus.

1. Wählen Sie **+Neu** aus, um eine Notebook-VM zu erstellen.

     ![Auswählen der neuen VM](./media/tutorial-1st-experiment-sdk-setup/add-workstation.png)

1. Geben Sie einen Namen für Ihre VM an. Klicken Sie anschließend auf **Erstellen**.

    > [!NOTE]
    > Der Notebook-VM-Name muss zwischen 2 und 16 Zeichen lang sein. Gültige Zeichen sind Buchstaben, Ziffern und Bindestriche.  Der Name muss darüber hinaus im Azure-Abonnement eindeutig sein.

1. Warten Sie, bis sich der Status in **Wird ausgeführt** ändert.

### <a name="launch-jupyter-web-interface"></a>Starten der Jupyter-Webbenutzeroberfläche

Nachdem Ihre VM ausgeführt wird, verwenden Sie den Abschnitt **Notebook-VMs**, um die Jupyter-Webbenutzeroberfläche zu starten.

1. Wählen Sie **Jupyter** in der Spalte **URI** für Ihre VM aus.

    ![Starten des Jupyter Notebook-Servers](./media/tutorial-1st-experiment-sdk-setup/start-server.png)

    Der Link startet Ihren Notebook-Server und öffnet die Jupyter Notebook-Webseite im Browser auf einer neuen Registerkarte.  Dieser Link funktioniert nur für die Person, die die VM erstellt. Jeder Benutzer des Arbeitsbereichs muss seinen eigenen virtuellen Computer erstellen.

1. Auf der Jupyter Notebook-Webseite ist der oberste Ordnername Ihr Benutzername.  Wählen Sie diesen Ordner aus.

    > [!TIP]
    > Dieser Ordner befindet sich auf dem [Speichercontainer](concept-workspace.md#resources) in Ihrem Arbeitsbereich und nicht auf der Notebook-VM selbst.  Sie können die Notebook-VM löschen und dennoch Ihre gesamte Arbeit behalten.  Wenn Sie später eine neue Notebook-VM erstellen, wird der gleiche Ordner geladen. Wenn Sie Ihren Arbeitsbereich mit anderen teilen, werden Ihr Ordner und die Ordner der anderen Benutzer angezeigt.

1. Öffnen Sie das Unterverzeichnis `samples-*` und anschließend `tutorials/tutorial-1st-experiment-sdk-train.ipynb`, um den **zweiten Teil** des Tutorials auszuführen.

## <a name="end"></a> Bereinigen der Ressourcen

Überspringen Sie diesen Abschnitt, wenn Sie mit dem **zweiten Teil** des Tutorials fortfahren möchten.

### <a name="stop-the-notebook-vm"></a>Beenden der Notebook-VM

Sollten Sie einen cloudbasierten Notebook-Server verwendet haben, beenden Sie aus Kostengründen den virtuellen Computer, wenn Sie ihn nicht verwenden.

1. Wählen Sie in Ihrem Arbeitsbereich **Notebook-VMs** aus.

   ![Beenden des VM-Servers](./media/tutorial-1st-experiment-sdk-setup/stop-server.png)

1. Wählen Sie den virtuellen Computer in der Liste aus.

1. Wählen Sie **Stop** (Beenden) aus.

1. Wenn Sie bereit sind, den Server erneut zu verwenden, wählen Sie **Starten** aus.

### <a name="delete-everything"></a>Alles löschen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Sie können die Ressourcengruppe auch behalten und einen einzelnen Arbeitsbereich löschen. Zeigen Sie die Eigenschaften des Arbeitsbereichs an, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie folgende Aufgaben ausgeführt:

* Erstellen eines Azure Machine Learning Service-Arbeitsbereichs
* Erstellen und Konfigurieren eines cloudbasierten Notebook-Servers in Ihrem Arbeitsbereich

Im **zweiten Teil** dieses Tutorials wird ein einfaches Machine Learning-Modell trainiert.

> [!div class="nextstepaction"]
> [Tutorial: Trainieren Ihres ersten Modells](tutorial-1st-experiment-sdk-train.md)
