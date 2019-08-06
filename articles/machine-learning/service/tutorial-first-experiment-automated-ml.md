---
title: Erstellen Ihres ersten automatisierten Machine Learning-Experiments
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie ein Klassifizierungsmodell mit automatisiertem Machine Learning im Azure-Portal trainieren und bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tsikiksr
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 07/23/2019
ms.openlocfilehash: 7c7d90d4ca1625edecc9d84e1ff7beec50032884
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68445080"
---
# <a name="tutorial-train-and-deploy-a-classification-model-with-automated-machine-learning-in-the-azure-portal-preview"></a>Tutorial: Trainieren und Bereitstellen eines Klassifizierungsmodells mit automatisiertem Machine Learning im Azure-Portal (Vorschauversion)

In diesem Tutorial erfahren Sie, wie Sie Ihr erstes automatisiertes Machine Learning-Experiment im Azure-Portal erstellen. In diesem Beispiel wird ein Klassifizierungsmodell erstellt, mit dem vorhergesagt werden kann, ob ein Client eine Termineinlage bei der Bank zeichnet. 

Mit Verwendung der automatisierten Machine Learning-Funktionen des Diensts und des Azure-Portals starten Sie den automatisierten Machine Learning-Prozess, und die Wahl des Algorithmus sowie die Optimierung der Hyperparameter werden Ihnen abgenommen. Beim automatisierten Machine Learning werden viele Kombinationen von Algorithmen und Hyperparametern durchlaufen, bis das beste Modell für Ihr Kriterium gefunden wird, ohne dass Sie eine einzige Codezeile schreiben müssen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren eines Azure Machine Learning Service-Arbeitsbereichs
> * Erstellen Sie ein Experiment.
> * Trainieren Sie automatisch ein Klassifizierungsmodell.
> * Zeigen Sie Details der Trainingsausführung an.
> * Bereitstellen des Modells.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie noch heute die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

* Die **bankmarketing_train.csv**-Datendatei. Sie können sie [hier](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv)herunterladen.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="create-an-experiment"></a>Erstellen eines Experiments

1. Navigieren Sie zum linken Bereich des Arbeitsbereichs. Wählen Sie **Automated Machine Learning** (Automatisiertes Machine Learning) im Abschnitt **Authoring (Preview)** (Erstellen (Vorschau)) aus.

    ![Navigationsbereich im Azure-Portal](media/tutorial-1st-experiment-automated-ml/nav-pane.png)

    Da dies das erste Experiment mit automatisiertem Machine Learning ist, sehen Sie den Bildschirm **Welcome to Automated Machine Learning** (Willkommen beim automatisierten Machine Learning). 

1.  Wählen Sie **Create Experiment** (Experiment erstellen) aus. Geben Sie dann **my-1st-automl-experiment** als Experimentnamen ein.

1. Wählen Sie **Create a new compute** (Neuen Compute erstellen) aus, und konfigurieren Sie Ihren Computekontext für dieses Experiment.

    Feld| Wert
    ---|---
    Computename| Geben Sie einen eindeutigen Namen ein, der Ihren Computekontext identifiziert. In diesem Beispiel verwenden wir **automl-compute**.
    Größe des virtuellen Computers| Wählen Sie die Größe für Ihren Computes aus. Wir verwenden **Standard_DS12_v2**.
    Zusätzliche Einstellungen| *Min node* (Mindestanzahl von Knoten): 1. Zum Aktivieren der Datenprofilerstellung müssen Sie über mindestens einen Knoten verfügen. <br> *Max node* (Maximale Anzahl von Knoten): 6. 

    Wählen Sie zum Starten der Erstellung Ihres neuen Computes **Create** (Erstellen) aus. Dies kann einen Moment dauern. 

    Wählen Sie nach Abschluss der Erstellung in der Dropdownliste Ihren neuen Compute aus, und klicken Sie dann auf **Weiter**.

1. Für dieses Tutorial verwenden wir das Standardspeicherkonto und den mit Ihrem neuen Compute erstellten Container. Dies wird automatisch im Formular eingetragen.

1. Wählen Sie **Upload** (Hochladen) aus, um die Datei **bankmarketing_train.csv** auf dem lokalen Computer auszuwählen und in den Standardcontainer hochzuladen. Die öffentliche Vorschau unterstützt nur lokale Dateiuploads und Azure Blob Storage-Konten. Wenn der Upload beendet ist, wählen Sie die Datei in der Liste aus. 

    [![Auswählen einer Datendatei](media/tutorial-1st-experiment-automated-ml/select-data-file.png)](media/tutorial-1st-experiment-automated-ml/select-data-file-expanded.png#lightbox)

1. Auf der Registerkarte **Preview** (Vorschau) können wir unsere Daten für dieses Experiment weiter konfigurieren.

    Geben Sie auf der Registerkarte „Preview“ (Vorschau) an, dass die Daten Header enthalten. Der Dienst schließt standardmäßig alle Features (Spalten) für das Training ein. Scrollen Sie für dieses Beispiel nach rechts, und ignorieren Sie (**Ignore**) das **day_of_week**-Feature (Wochentag).

    ![Konfiguration der Registerkarte „Preview“ (Vorschau)](media/tutorial-1st-experiment-automated-ml/preview-tab-config.gif)


    >[!NOTE]
    > Die Datenprofilerstellung ist bei Computes mit mindestens 0 Knoten nicht verfügbar.

1. Wählen Sie **Classification** (Klassifizierung) als Vorhersageaufgabe aus.

1. Wählen Sie **y** als Zielspalte aus, die Spalte, für die wir Vorhersagen ausführen möchten. Diese Spalte gibt an, ob der Client eine Termineinlage bei der Bank gezeichnet hat.

1. Erweitern Sie **Advanced Settings** (Erweiterte Einstellungen), und füllen Sie die Felder wie folgt aus.

    Erweiterte Einstellungen|Wert
    ------|------
    Primary metric (Primäre Metrik)| AUC_weighted 
    Exit Criteria (Beendigungskriterien)| Wenn eines dieser Kriterien erfüllt ist, wird der Trainingsauftrag vor dem vollständigen Abschluss beendet. <br> *Training job time (minutes)* (Dauer des Trainingsauftrags (Minuten)): 5  <br> *Max number of iterations* (Maximale Anzahl von Iterationen): 10 
    Preprocessing (Vorverarbeitung)| Aktivieren der Vorverarbeitung durch automatisiertes Machine Learning. Dies umfasst die automatische Datenbereinigung, die Vorbereitung und die Transformation, um synthetische Features zu generieren.
    Überprüfen| Wählen Sie die K-fache Kreuzvalidierung und 2 für die Anzahl der Kreuzvalidierungen aus. 
    Parallelität| Wählen Sie 5 für die maximale Anzahl gleichzeitiger Iterationen aus.

   >[!NOTE]
   > Für dieses Experiment legen wir keinen Schwellenwert für Metriken oder maximale Iterationen fest und verhindern nicht, dass die Algorithmen getestet werden.

1. Klicken Sie auf **Start**, um das Experiment auszuführen.

   Nach dem Start des Experiments sehen Sie einen leeren **Run Detail**-Bildschirm (Ausführungsdetail) mit dem folgenden Status am oberen Rand. Der das Experiment vorbereitende Prozess nimmt einige Minuten in Anspruch. Wenn die Vorbereitung abgeschlossen ist, wechselt die Statusmeldung zu **Run is Running** (Ausführung läuft).

      ![Vorbereitung der Ausführung](media/tutorial-1st-experiment-automated-ml/run-preparing.png)

##  <a name="view-experiment-details"></a>Anzeigen von Details zum Experiment

Während der Experimentausführung aktualisiert der Bildschirm **Run Detail** (Ausführungsdetail) Iterationsdiagramm und Liste mit den verschiedenen Iterationen (Modellen), die ausgeführt werden. Die Iterationsliste ist nach der Metrikbewertung sortiert. Standardmäßig steht das Modell mit der auf unserer **AUC_weighted**-Metrik basierenden höchsten Bewertung am Anfang der Liste.

>[!TIP]
> Die Ausführung von Trainingsaufträgen kann für jede Pipeline einige Minuten beanspruchen.

[![Dashboard mit den Ausführungsdetails](media/tutorial-1st-experiment-automated-ml/run-details.png)](media/tutorial-1st-experiment-automated-ml/run-details-expanded.png#lightbox)

## <a name="deploy-model"></a>Bereitstellen des Modells

Aus diesem Experiment geht **VotingEnsemble** basierend auf der **AUC_weighted**-Metrik als bestes Modell hervor. Mit automatisiertem Machine Learning im Azure-Portal können wir dieses Modell als Webdienst bereitstellen, um mit nur einem Mausklick eine Vorhersage für neue Daten zu erhalten. 

1. Wählen Sie auf der Seite **Run Detail** (Ausführungsdetail) die Schaltfläche **Deploy Best Model** (Bestes Modell bereitstellen) aus.

1. Füllen Sie den Bereich **Deploy Best Model** (Bestes Modell bereitstellen) wie folgt aus:

    Feld| Wert
    ----|----
    „Deployment name“ (Bereitstellungsname)| my-automl-deploy
    „Deployment description“ (Bereitstellungsbeschreibung)| „Meine erste Bereitstellung eines automatisierten Machine Learning-Experiments“
    „Scoring script“ (Bewertungsskript)| Automatisch generiert
    Environment script (Umgebungsskript)| Automatisch generiert
    
1. Klicken Sie auf **Bereitstellen**. Die Bereitstellung kann bis zu 20 Minuten dauern.

    Wenn die Bereitstellung erfolgreich abgeschlossen wurde, wird die folgende Meldung angezeigt.

    ![Bereitstellung abgeschlossen](media/tutorial-1st-experiment-automated-ml/deploy-complete-status.png)
    
    Das ist alles! Sie haben einen einsatzfähigen Webdienst, mit dem Vorhersagen generiert werden können.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Bereitstellungsdateien sind größer als Daten- und Experimentdateien, sodass ihre Speicherung teurer ist. Löschen Sie einfach die Bereitstellungsdateien, um die Kosten für Ihr Konto zu minimieren, und wenn Sie den Arbeitsbereich und die Experimentdateien beibehalten möchten. Löschen Sie andernfalls die gesamte Ressourcengruppe, wenn Sie keine der Dateien verwenden möchten.  

### <a name="delete-deployment-instance"></a>Löschen der Bereitstellungsinstanz

Löschen Sie einfach die Bereitstellungsinstanz aus dem Azure-Portal, wenn Sie die Ressourcengruppe und den Arbeitsbereich für andere Tutorials und Untersuchungen beibehalten möchten. 

1. Navigieren Sie auf der linken Seite zum Bereich **Assets** (Ressourcen), und wählen Sie **Deployments** (Bereitstellungen) aus. 

1. Wählen Sie die zu löschende Bereitstellung aus, und klicken Sie auf **Delete** (Löschen). 

1. Wählen Sie **Proceed** (Fortfahren) aus.

### <a name="delete-resource-group"></a>Ressourcengruppe löschen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial zu automatisierten Machine Learning haben Sie im Azure-Portal ein Klassifizierungsmodell erstellt und bereitgestellt. Weitere Informationen und nächste Schritte finden Sie in diesen Artikeln:

+ Informationen zum [Nutzen eines Webdiensts](how-to-consume-web-service.md).
+ Weitere Informationen zur [Vorverarbeitung](how-to-create-portal-experiments.md#preprocess).
+ Weitere Informationen zur [Datenprofilerstellung](how-to-create-portal-experiments.md#profile).
+ Weitere Informationen zu [automatisiertem Machine Learning](concept-automated-ml.md).

>[!NOTE]
> Dieses Bank Marketing-Dataset wird unter der [Creative Commons (CCO: Public Domain)-Lizenz](https://creativecommons.org/publicdomain/zero/1.0/) zur Verfügung gestellt. Alle Rechte in den einzelnen Inhalten der Datenbank sind gemäß der [Database Contents License](https://creativecommons.org/publicdomain/zero/1.0/) lizenziert und auf [Kaggle](https://www.kaggle.com/janiobachmann/bank-marketing-dataset) verfügbar. Dieses Dataset ist ursprünglich in der [UCI Machine Learning Database](https://archive.ics.uci.edu/ml/datasets/bank+marketing) verfügbar.<br><br>
>  Bitte zitieren Sie die folgenden Werke: <br> [Moro et al., 2014] S. Moro, P. Cortez und P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. (Ein datengesteuerter Ansatz zur Prognose des Erfolgs im Bank-Telemarketing.) Decision Support Systems, Elsevier, 62:22-31, Juni 2014.