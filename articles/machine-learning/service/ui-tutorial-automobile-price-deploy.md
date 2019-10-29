---
title: 'Tutorial: Bereitstellen eines Machine Learning-Modells mithilfe der grafischen Benutzeroberfläche'
titleSuffix: Azure Machine Learning
description: Hier erfahren Sie, wie Sie mithilfe der grafischen Benutzeroberfläche von Azure Machine Learning eine Predictive Analytics-Lösung erstellen. Sie erhalten Informationen zum Trainieren, Bewerten und Bereitstellen eines Machine Learning-Modells mithilfe von Drag & Drop-Modulen.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 10/22/2019
ms.openlocfilehash: 6f8717f70a2cb03a7fd683cfe61f1198461f4305
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792676"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-visual-interface"></a>Tutorial: Bereitstellen eines Machine Learning-Modells mithilfe der grafischen Benutzeroberfläche

Um anderen die Verwendung des im [ersten Teil des Tutorials](ui-tutorial-automobile-price-train-score.md) entwickelten Vorhersagemodells zu ermöglichen, können Sie es als Echtzeitendpunkt bereitstellen. Im ersten Teil haben Sie Ihr Modell trainiert. Jetzt ist es an der Zeit, neue Vorhersagen basierend auf Benutzereingaben zu generieren. In diesem Teil des Tutorials führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Bereitstellen eines Echtzeitendpunkts
> * Erstellen eines Rückschlussclusters
> * Testen eines Echtzeitendpunkts

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie [Teil 1 des Tutorials](ui-tutorial-automobile-price-train-score.md) aus, um zu erfahren, wie Sie ein Machine Learning-Modell auf der grafischen Benutzeroberfläche trainieren und bewerten.

## <a name="deploy-a-real-time-endpoint"></a>Bereitstellen eines Echtzeitendpunkts

Für die Bereitstellung Ihrer Pipeline ist Folgendes erforderlich:

1. Konvertieren Sie die Trainingspipeline in eine Echtzeit-Rückschlusspipeline. Dadurch werden Trainingsmodule entfernt sowie Ein- und Ausgaben für Rückschlussanforderungen hinzugefügt.
1. Stellen Sie die Rückschlusspipeline bereit.

### <a name="create-a-real-time-inference-pipeline"></a>Erstellen einer Echtzeit-Rückschlusspipeline

1. Wählen Sie im oberen Bereich der Pipelinecanvas **Rückschlusspipeline erstellen** > **Echtzeit-Rückschlusspipeline** aus.

    Wenn Sie **Rückschlusspipeline erstellen** auswählen, passiert Folgendes:
    
    * Das trainierte Modell wird als Modul **Dataset** in der Modulpalette gespeichert. Sie finden es unter **My Datasets** (Meine Datasets).
    * Zum Trainieren verwendete Module wie **Train Model** (Modell trainieren) und **Split Data** (Daten aufteilen) werden entfernt.
    * Das gespeicherte trainierte Modell wird wieder der Pipeline hinzugefügt.
    * Die Module **Web Service Input** (Webdiensteingabe) und **Web Service Output** (Webdienstausgabe) werden hinzugefügt. Diese Module identifizieren, wo Benutzerdaten in das Modell eingehen und wo sie zurückgegeben werden.

    > [!Note]
    > Die **Trainingspipeline** wird am oberen Rand der Pipelinecanvas unter der neuen Registerkarte gespeichert. Darüber hinaus steht sie auf der grafischen Benutzeroberfläche als veröffentlichte Pipeline zur Verfügung.
    >

    Ihre Pipeline sollte nun wie folgt aussehen:  

   ![Screenshot der erwarteten Pipelinekonfiguration nach Vorbereitung der Bereitstellung](./media/ui-tutorial-automobile-price-deploy/predictive-graph.png)

1. Wählen Sie **Ausführen** aus, und verwenden Sie das gleiche Computeziel und Experiment wie im ersten Teil.

1. Wählen Sie das Modul **Score Model** (Modell bewerten) aus.

1. Wählen Sie im Eigenschaftenbereich **Ausgaben** > **Visualisieren** aus, um sich zu vergewissern, dass das Modell weiterhin funktioniert. Daraufhin werden die ursprünglichen Daten zusammen mit dem prognostizierten Preis („Scored Labels“) angezeigt.

1. Klicken Sie auf **Bereitstellen**.

### <a name="create-an-inferencing-cluster"></a>Erstellen eines Rückschlussclusters

Im angezeigten Dialogfeld können Sie vorhandene AKS-Cluster (Azure Kubernetes Service) in Ihrem Arbeitsbereich auswählen, um Ihr Modell bereitzustellen. Sollten Sie über keinen AKS-Cluster verfügen, gehen Sie wie folgt vor, um einen zu erstellen:

1. Wählen Sie im Dialogfeld die Option **Compute** aus, um zur Seite **Compute** zu gelangen.

1. Wählen Sie auf dem Navigationsmenüband **Rückschlusscluster** >  **+ Neu** aus.

    ![Screenshot: Navigation zum Bereich für den neuen Rückschlusscluster](./media/ui-tutorial-automobile-price-deploy/new-inference-cluster.png)

1. Konfigurieren Sie im Bereich des Rückschlussclusters einen neuen Kubernetes-Dienst.

1. Geben Sie unter **Computename** den Namen „aks-compute“ ein.
    
1. Wählen Sie unter **Region** eine verfügbare Region in der Nähe aus.

1. Klicken Sie auf **Erstellen**.

    > [!Note]
    > Die Erstellung eines neuen AKS-Diensts dauert etwa 15 Minuten. Der Bereitstellungsstatus kann auf der Seite **Rückschlusscluster** überprüft werden.
    >

### <a name="deploy-the-real-time-endpoint"></a>Bereitstellen des Echtzeitendpunkts

Kehren Sie nach Abschluss der Bereitstellung des AKS-Diensts zur Echtzeit-Rückschlusspipeline zurück, um deren Bereitstellung abzuschließen.

1. Wählen Sie über der Canvas die Option **Bereitstellen** aus.

1. Wählen Sie **Neuen Echtzeitendpunkt bereitstellen** aus. 

1. Wählen Sie den erstellten AKS-Cluster aus.

1. Klicken Sie auf **Bereitstellen**.

    ![Screenshot: Einrichtung eines neuen Echtzeit-Endpunkts](./media/ui-tutorial-automobile-price-deploy/setup-endpoint.png)

    Nach Abschluss der Bereitstellung wird über der Canvas eine Erfolgsbenachrichtigung angezeigt. Der Bereitstellungsvorgang kann einige Minuten dauern.

## <a name="test-the-real-time-endpoint"></a>Testen des Echtzeitendpunkts

Sie können den Echtzeitendpunkt testen, indem Sie auf der linken Seite im Navigationsbereich des Arbeitsbereichs zur Seite **Endpunkte** navigieren.

1. Wählen Sie auf der Seite **Endpunkte** den bereitgestellten Endpunkt aus.

    ![Screenshot: Registerkarte „Echtzeitendpunkte“ mit Hervorhebung des kürzlich erstellten Endpunkts](./media/ui-tutorial-automobile-price-deploy/web-services.png)

1. Klicken Sie auf **Test**.

1. Geben Sie Testdaten ein, oder verwenden Sie die automatisch ausgefüllten Beispieldaten, und wählen Sie die Option **Test** aus.

    Die Testanforderung wird an den Endpunkt übermittelt, und die Ergebnisse werden auf der Seite angezeigt. Für die Eingabedaten wird zwar ein Preiswert generiert, dieser wird jedoch nicht zum Generieren des Vorhersagewerts verwendet.

    ![Screenshot: Test des Echtzeitendpunkts mit Hervorhebung des ausgewerteten Bezeichners für den Preis](./media/ui-tutorial-automobile-price-deploy/test-endpoint.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die wichtigsten Schritte zum Erstellen, Bereitstellen und Verwenden eines Machine Learning-Modells auf der grafischen Benutzeroberfläche gelernt. Wenn Sie mehr darüber erfahren möchten, wie Sie die grafische Benutzeroberfläche verwenden können, um andere Arten von Problemen zu lösen, sehen Sie sich die Beispielpipelines an.

> [!div class="nextstepaction"]
> [Beispiel für die Kreditrisikoklassifizierung](how-to-ui-sample-classification-predict-credit-risk-cost-sensitive.md)
