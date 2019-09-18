---
title: 'Tutorial: Bereitstellen eines Machine Learning-Modells mithilfe der grafischen Benutzeroberfläche'
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie mithilfe der grafischen Benutzeroberfläche von Azure Machine Learning Service eine Vorhersageanalyselösung (Predictive Analytics-Lösung) erstellen. Sie erhalten Informationen zum Trainieren, Bewerten und Bereitstellen eines Machine Learning-Modells mithilfe von Drag & Drop-Modulen. Dieses Tutorial ist der zweite Teil einer zweiteiligen Reihe über das Prognostizieren von Automobilpreisen mithilfe der linearen Regression.
author: peterclu
ms.author: peterlu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 07/11/2019
ms.openlocfilehash: 64062aeab9a807d2aee7f4bca05d4019a3d9b736
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70858636"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-visual-interface"></a>Tutorial: Bereitstellen eines Machine Learning-Modells mithilfe der grafischen Benutzeroberfläche

Um anderen Personen die Möglichkeit zu bieten, das in [Teil 1 des Tutorials](ui-tutorial-automobile-price-train-score.md) entwickelte Vorhersagemodell zu verwenden, können Sie es als Azure-Webdienst bereitstellen. Bis jetzt haben Sie mit dem Trainieren des Modells experimentiert. Jetzt ist es an der Zeit, neue Vorhersagen basierend auf Benutzereingaben zu generieren. In diesem Teil des Tutorials führen Sie die folgenden Schritte aus:

> [!div class="checklist"]
> * Vorbereiten eines Modells für die Bereitstellung
> * Bereitstellen eines Webdiensts
> * Testen eines Webdiensts
> * Verwalten eines Webdiensts
> * Nutzen des Webdiensts

## <a name="prerequisites"></a>Voraussetzungen

Führen Sie [Teil 1 des Tutorials](ui-tutorial-automobile-price-train-score.md) aus, um zu erfahren, wie Sie ein Machine Learning-Modell auf der grafischen Benutzeroberfläche trainieren und bewerten.

## <a name="prepare-for-deployment"></a>Vorbereiten der Bereitstellung

Bevor Sie Ihr Experiment als Webdienst bereitstellen, müssen Sie zuerst Ihr *Trainingsexperiment* in ein *Vorhersageexperiment* konvertieren.

1. Wählen Sie unten im Experimentbereich die Option zum **Erstellen eines Vorhersageexperiments**\*.

    ![Animierte GIF der Darstellung der automatischen Konvertierung eines Trainingsexperiments in ein Vorhersageexperiment](./media/ui-tutorial-automobile-price-deploy/deploy-web-service.gif)

    Wenn Sie **Vorhersageexperiment erstellen** auswählen, geschehen mehrere Dinge:
    
    * Das trainierte Modell wird als Modul **Trainiertes Modell** in der Modulpalette gespeichert. Sie finden das Modell unter **Trained Models** (Trainierte Modelle).
    * Module, die zum Training verwendet wurden, werden entfernt. Das gilt insbesondere für:
      * Train Model (Modell trainieren)
      * Split Data (Daten aufteilen)
      * Bewerten eines Modells
    * Das gespeicherte trainierte Modell wird wieder dem Experiment hinzugefügt.
    * Die Module **Web service input** und **Web service output** werden hinzugefügt. Diese Module identifizieren, wo die Daten des Benutzers in das Modell eingehen und wo die Daten zurückgegeben werden.

    Das **Trainingsexperiment** ist weiterhin auf den neuen Registerkarten gespeichert, die oben im Experimentbereich befinden.

1. **Ausführen** aus.

1. Wählen Sie die Ausgabe des Moduls **Modell bewerten** aus, und wählen Sie dann **Ergebnisse anzeigen** aus, um sicherzustellen, dass das Modell noch funktioniert. Daraufhin werden die ursprünglichen Daten zusammen mit dem prognostizierten Preis („Scored Labels“) angezeigt.

Ihr Experiment sollte jetzt wie folgt aussehen:  

![Screenshot der erwarteten Konfiguration des Experiments nach dem Vorbereiten der Bereitstellung](./media/ui-tutorial-automobile-price-deploy/predictive-graph.png)

## <a name="deploy-the-web-service"></a>Bereitstellen des Webdiensts

1. Wählen Sie unten im Experimentbereich die Option **Deploy Web Service** (Webdienst bereitstellen) aus.

1. Wählen Sie das **Computeziel** für die Ausführung Ihres Webdiensts aus.

    Derzeit unterstützt die grafische Benutzeroberfläche nur die Bereitstellung in Azure Kubernetes Service-Computezielen (AKS). Sie können im Machine Learning Service-Arbeitsbereich eines der verfügbaren AKS-Computeziele auswählen oder eine neue AKS-Umgebung konfigurieren, indem Sie im daraufhin angezeigten Dialogfeld die entsprechenden Schritte ausführen.

    ![Screenshot einer möglichen Konfiguration für ein neues Computeziel](./media/ui-tutorial-automobile-price-deploy/deploy-compute.png)

1. Wählen Sie **Deploy Web Service** (Webdienst bereitstellen) aus. Wenn die Bereitstellung abgeschlossen ist, wird die folgende Benachrichtigung angezeigt. Die Bereitstellung kann mehrere Minuten dauern.

    ![Screenshot der Bestätigungsmeldung einer erfolgreichen Bereitstellung.](./media/ui-tutorial-automobile-price-deploy/deploy-succeed.png)

## <a name="test-the-web-service"></a>Testen des Webdiensts

Sie können Ihre Webdienste der grafischen Benutzeroberfläche testen und verwalten, indem Sie zur Registerkarte **Webdienste** navigieren.

1. Wechseln Sie zum Abschnitt „Web Services“ (Webdienste). Es wird der von Ihnen bereitgestellte Webdienst mit dem Namen **Tutorial – Predict Automobile Price[Predictive Exp]** (Tutorial – Automobilpreisvorhersage[Vorhersageexperiment]) angezeigt.

     ![Screenshot der Registerkarte „Web Services“, in dem der zuletzt erstellte Webdienst hervorgehoben ist](./media/ui-tutorial-automobile-price-deploy/web-services.png)

1. Wählen Sie den Namen des Webdiensts aus, um weitere Details anzuzeigen.

1. Klicken Sie auf **Test**.

    [![Screenshot der Testseite für den Webdienst](./media/ui-tutorial-automobile-price-deploy/web-service-test.png)](./media/ui-tutorial-automobile-price-deploy/web-service-test.png#lightbox)

1. Geben Sie Testdaten ein, oder verwenden Sie die automatisch ausgefüllten Beispieldaten, und wählen Sie die Option **Test** aus.

    Die Testanforderung wird an den Webdienst gesendet, und die Ergebnisse werden auf der Seite angezeigt. Für die Eingabedaten wird zwar ein Preiswert generiert, dieser wird jedoch nicht zum Generieren des Vorhersagewerts verwendet.

## <a name="consume-the-web-service"></a>Nutzen des Webdiensts

Benutzer können jetzt API-Anforderungen an Ihren Azure-Webdienst senden und Ergebnisse empfangen, um den Preis ihrer neuen Autos vorherzusagen.

**Anforderung/Antwort** – Der Benutzer sendet eine oder mehrere Zeilen von Automobildaten mithilfe eines Hypertext Transfer-Protokolls (HTTP) an den Dienst. Der Dienst antwortet mit einem oder mehreren Ergebnissätzen.

REST-Beispielaufrufe finden Sie auf der Seite der Webdienstdetails auf der Registerkarte **Consume** (Nutzung).

   ![Screenshot eines REST-Beispielaufrufs, der Benutzern auf der Registerkarte „Consume“ angezeigt wird](./media/ui-tutorial-automobile-price-deploy/web-service-consume.png)

Navigieren Sie zur Registerkarte **API Doc** (API-Dok), um weitere API-Details anzuzeigen.

## <a name="manage-models-and-deployments"></a>Verwalten von Modellen und Bereitstellungen

Die von Ihnen auf der grafischen Benutzeroberfläche erstellten Modelle und Webdienstbereitstellungen können auch im Azure Machine Learning Service-Arbeitsbereich verwaltet werden.

1. Öffnen Sie Ihren Arbeitsbereich im [Azure-Portal](https://portal.azure.com/).  

1. Wählen Sie in Ihrem Arbeitsbereich die Option **Models** (Modelle) aus. Wählen Sie dann das Experiment aus, das Sie erstellt haben.

    ![Screenshot, der zeigt, wie Sie im Azure-Portal zu Experimenten navigieren](./media/ui-tutorial-automobile-price-deploy/portal-models.png)

    Auf dieser Seite werden weitere Details zum Modell angezeigt.

1. Wählen Sie **Deployments** (Bereitstellungen) aus. Auf dieser Registerkarte werden alle Webdienste aufgeführt, die das Modell verwenden. Wählen Sie den Namen des Webdiensts aus. Daraufhin wird die Seite der Webdienstdetails angezeigt. Auf dieser Seite finden Sie ausführlichere Informationen zum Webdienst.

    [![Screenshot eines detaillierten Ausführungsberichts](./media/ui-tutorial-automobile-price-deploy/deployment-details.png)](./media/ui-tutorial-automobile-price-deploy/deployment-details.png#lightbox)

Diese Modelle und Bereitstellungen finden Sie auch auf der [Landing Page des Arbeitsbereichs (Vorschau)](https://ml.azure.com) in den Abschnitten **Modelle** und **Endpunkte**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die wichtigsten Schritte zum Erstellen, Bereitstellen und Verwenden eines Machine Learning-Modells auf der grafischen Benutzeroberfläche gelernt. Wenn Sie mehr darüber erfahren möchten, wie Sie die grafische Benutzeroberfläche verwenden können, um andere Arten von Problemen zu lösen, sehen Sie sich die Beispielexperimente an.

> [!div class="nextstepaction"]
> [Beispiel für die Kreditrisikoklassifizierung](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
