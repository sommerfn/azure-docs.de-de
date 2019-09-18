---
title: Erstellen Ihres ersten automatisierten Machine Learning-Experiments
titleSuffix: Azure Machine Learning service
description: Erfahren Sie, wie Sie ein Klassifizierungsmodell mit automatisiertem Machine Learning auf der Angebotsseite (Vorschauversion) des Arbeitsbereichs von Azure Machine Learning trainieren und bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: tzvikei
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 09/09/2019
ms.openlocfilehash: 0dd4447736469644875dff914c6284b087be87d0
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910223"
---
# <a name="tutorial-create-your-first-classification-model-with-automated-machine-learning"></a>Tutorial: Erstellen Ihres ersten Klassifizierungsmodells mit automatisiertem maschinellem Lernen

In diesem Tutorial erfahren Sie, wie Sie Ihr erstes Experiment mit automatisiertem maschinellem Lernen über die Angebotsseite (Vorschauversion) des Arbeitsbereichs erstellen, ohne eine einzige Codezeile schreiben zu müssen. In diesem Beispiel wird ein Klassifizierungsmodell erstellt, mit dem vorhergesagt werden kann, ob ein Kunde Festgeld bei der Bank anlegt.

Mit automatisiertem maschinellen Lernen können Sie zeitintensive Aufgaben automatisieren. Beim automatisierten maschinellen Lernen werden viele Kombinationen von Algorithmen und Hyperparametern schnell durchlaufen, um basierend auf einer von Ihnen ausgewählten Erfolgsmetrik das beste Modell zu ermitteln.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Azure Machine Learning Service-Arbeitsbereichs
> * Ausführen eines automatisierten Machine Learning-Experiments
> * Anzeigen von Details zum Experiment
> * Bereitstellen des Modells.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://aka.ms/AMLFree) erstellen.

* Laden Sie die Datendatei [**bankmarketing_train.csv**](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) herunter. In der Spalte **y** ist angegeben, ob ein Kunde Festgeld angelegt hat. Sie wird später als Zielspalte für Vorhersagen in diesem Tutorial festgelegt. 

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

Ein Azure Machine Learning-Arbeitsbereich ist eine grundlegende Cloudressource zum Experimentieren, Trainieren und Bereitstellen von Machine Learning-Modellen. Er verknüpft Ihr Azure-Abonnement und Ihre Ressourcengruppe mit einem einfach nutzbaren Objekt im Dienst. 

Sie erstellen einen Arbeitsbereich über das Azure-Portal, einer webbasierten Konsole zum Verwalten Ihrer Azure-Ressourcen. 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> Notieren Sie sich Ihren **Arbeitsbereich** und Ihr **Abonnement**. Sie benötigen diese Informationen, um sicherzustellen, dass Sie Ihr Experiment an der richtigen Stelle erstellen. 

## <a name="create-and-run-the-experiment"></a>Erstellen und Ausführen des Experiments

Sie führen die folgende Experimenteinrichtung durch und führen Schritte auf der Angebotsseite des Arbeitsbereichs aus. Diese konsolidierte Oberfläche enthält Tools für maschinelles Lernen zur Durchführung von datenwissenschaftlichen Szenarien für Datenwissenschaftler aller Fachrichtungen.

1. Melden Sie sich auf der [Angebotsseite des Arbeitsbereichs](https://ml.azure.com/workspaceportal/) an.

1. Wählen Sie Ihr Abonnement und den erstellten Arbeitsbereich aus.

1. Wählen Sie **Erste Schritte** aus.

1.  Wählen Sie im linken Bereich im Abschnitt **Erstellung** die Option **Automatisiertes ML** aus.
Da dies Ihr erstes Experiment mit automatisiertem maschinellem Lernen ist, wird der Bildschirm **Erste Schritte** angezeigt.

    ![Azure Machine Learning Studio](media/tutorial-1st-experiment-automated-ml/get-started.png)

1. Wählen Sie **Create Experiment** (Experiment erstellen) aus. 

1. Geben Sie **my-1st-automl-experiment** als Experimentnamen ein.

1. Wählen Sie **Create a new compute** (Neue Computeressource erstellen) aus. 

    1. Konfigurieren Sie den Computekontext für dieses Experiment.
        
        Feld | Wert
        ----|---
        Computename |  Geben Sie einen eindeutigen Namen ein, der Ihren Computekontext identifiziert. In diesem Beispiel verwenden Sie **automl-compute**.
        Größe des virtuellen Computers| Wählen Sie die Größe für Ihren Computes aus. Verwenden Sie die Standardgröße, **Standard_DS12_V2**.
        Zusätzliche Einstellungen| *Min node* (Mindestanzahl von Knoten): 1. Zum Aktivieren der Datenprofilerstellung müssen Sie über mindestens einen Knoten verfügen. <br> *Max node* (Maximale Anzahl von Knoten): 6.
 
    1. Wählen Sie **Erstellen** aus, um Ihren neuen Compute zu erstellen. Dieser Vorgang nimmt einige Minuten in Anspruch. 

    1. Wählen Sie nach Abschluss der Erstellung in der Dropdownliste Ihren neuen Compute aus, und klicken Sie dann auf **Weiter**.

    >[!NOTE]
    >Für dieses Tutorial verwenden Sie das Standardspeicherkonto und den mit Ihrer neuen Computeressource erstellten Container. Dies wird automatisch im Formular eingetragen.

1. Wählen Sie **Aus lokaler Datei hochladen** aus. Hier erstellen Sie einen neuen Datensatz mit der Datei **bankmarketing_train.csv**, die Sie zuvor für dieses Tutorial heruntergeladen haben. 

    1. Wählen Sie **Durchsuchen** aus, und wählen Sie dann auf dem lokalen Computer die Datei **bankmarketing_train.csv** aus. 

    1. Weisen Sie Ihrem Dataset einen eindeutigen Namen zu, und geben Sie eine optionale Beschreibung ein. 

    1. Wählen Sie **Weiter** aus, um das Dataset in den Standardcontainer hochzuladen, der bei der Erstellung des Arbeitsbereichs automatisch eingerichtet wurde. Die öffentliche Vorschauversion unterstützt nur Uploads von lokalen Dateien. 

    1. Wenn der Upload abgeschlossen ist, wird das Formular **Einstellungen und Vorschau** basierend auf dem Dateityp intelligent ausgefüllt. Stellen Sie sicher, dass das Formular wie folgt ausgefüllt ist.
        
        Feld|Wert
        ---|---
        Dateiformat| Durch Trennzeichen getrennt
        Trennzeichen| Komma
        Codieren| UTF-8
        Spaltenüberschriften| Alle Dateien haben dieselben Header            Zeilen überspringen | Keine

        >[!NOTE]
        > Wenn eine der Einstellungen in diesem Formular aktualisiert wird, wird auch die Vorschau entsprechend aktualisiert.

        Klicken Sie auf **Weiter**.
    

    1. Das Formular **Schema** ermöglicht eine weitere Konfiguration der Daten für dieses Experiment. Wählen Sie für dieses Beispiel den Umschalter für das Feature **day_of_week** aus, um es für dieses Experiment nicht einzuschließen. Wählen Sie **Fertig** aus, um das Hochladen von Dateien und das Erstellen des Datasets für Ihr Experiment abzuschließen.

        ![Konfiguration der Registerkarte „Preview“ (Vorschau)](media/tutorial-1st-experiment-automated-ml/schema-tab-config.gif)

        
1. Wählen Sie **Classification** (Klassifizierung) als Vorhersageaufgabe aus.

1. Wählen Sie **y** als Zielspalte aus, in der Sie Vorhersagen ausführen möchten. Diese Spalte gibt an, ob der Kunde eine Termineinlage bei der Bank gezeichnet hat.

1. Erweitern Sie **Advanced Settings** (Erweiterte Einstellungen), und füllen Sie die Felder wie folgt aus.

    Erweiterte Einstellungen|Wert
    ------|------
    Primary metric (Primäre Metrik)| AUC_weighted 
    Exit Criteria (Beendigungskriterien)| Wenn eines dieser Kriterien erfüllt ist, wird der Trainingsauftrag vor dem vollständigen Abschluss beendet: <br> *Training job time (minutes)* (Dauer des Trainingsauftrags (Minuten)): 5  <br> *Max number of iterations* (Maximale Anzahl von Iterationen): 10 
    Preprocessing (Vorverarbeitung)| Aktivieren der Vorverarbeitung durch automatisiertes Machine Learning. Dies umfasst die automatische Datenbereinigung, die Vorbereitung und die Transformation, um synthetische Features zu generieren.
    Überprüfen| Wählen Sie die K-fache Kreuzvalidierung und **2** für die Anzahl der Kreuzvalidierungen aus. 
    Parallelität| Wählen Sie **5** für die maximale Anzahl gleichzeitiger Iterationen aus.

   >[!NOTE]
   > Für dieses Experiment legen Sie keinen Schwellenwert für Metriken oder maximale Kerne pro Iteration fest. Sie verhindern auch nicht, dass Algorithmen getestet werden.

1. Wählen Sie **Start** aus, um das Experiment auszuführen.

   Wenn das Experiment startet, wird ein leerer Bildschirm mit einer Statusmeldung oben angezeigt.

Der Vorbereitungsprozess des Experiments nimmt einige Minuten in Anspruch. Wenn dieser Prozess abgeschlossen wird, wechselt die Statusmeldung zu **Run is Running** (Ausführung läuft).

##  <a name="view-experiment-details"></a>Anzeigen von Details zum Experiment

Während der Experimentausführung aktualisiert der Bildschirm das **Iterationsdiagramm** und die **Iterationsliste** mit den verschiedenen Iterationen (Modellen), die ausgeführt werden. Die Liste der Iterationen ist nach Metrikbewertungen sortiert. Standardmäßig steht das Modell, das auf Grundlage unserer **AUC_weighted**-Metrik die höchste Bewertung erhält, ganz oben in der Liste.

>[!WARNING]
> Die Ausführung von Trainingsaufträgen dauert für jede Pipeline einige Minuten.

[![Dashboard mit den Ausführungsdetails](media/tutorial-1st-experiment-automated-ml/run-details.png)](media/tutorial-1st-experiment-automated-ml/run-details-expanded.png#lightbox)

## <a name="deploy-the-model"></a>Bereitstellen des Modells

Durch die Verwendung von automatisiertem maschinellem Lernen auf der Angebotsseite des Arbeitsbereichs können wir das beste Modell als Webdienst bereitstellen, um neue Daten vorherzusagen und potenzielle Chancen zu identifizieren. In diesem Experiment bedeutet Bereitstellung, dass das Finanzinstitut nun über eine iterative und skalierbare Lösung zur Identifizierung potenzieller Festgeldkunden verfügt.

Aus diesem Experimentkontext geht **VotingEnsemble** basierend auf der **AUC_weighted**-Metrik als bestes Modell hervor.  Wir stellen dieses Modell bereit. Die Bereitstellung dauert jedoch etwa 20 Minuten.

1. Wählen Sie auf der Seite **Run Detail** (Ausführungsdetail) oben rechts die Schaltfläche **Deploy Best Model** (Bestes Modell bereitstellen) aus.

1. Füllen Sie den Bereich **Deploy Best Model** (Bestes Modell bereitstellen) wie folgt aus:

    Feld| Wert
    ----|----
    „Deployment name“ (Bereitstellungsname)| my-automl-deploy
    „Deployment description“ (Bereitstellungsbeschreibung)| „Meine erste Bereitstellung eines automatisierten Machine Learning-Experiments“
    „Scoring script“ (Bewertungsskript)| Automatisch generiert
    Environment script (Umgebungsskript)| Automatisch generiert
    
1. Klicken Sie auf **Bereitstellen**.

    Wenn die Bereitstellung erfolgreich abgeschlossen wurde, wird eine entsprechende Meldung angezeigt.
    
Nun haben Sie einen einsatzfähigen Webdienst, mit dem Vorhersagen generiert werden können.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Bereitstellungsdateien sind größer als Daten- und Experimentdateien, sodass ihre Speicherung teurer ist. Löschen Sie nur die Bereitstellungsdateien, um die Kosten für Ihr Konto zu minimieren, oder wenn Sie den Arbeitsbereich und die Experimentdateien beibehalten möchten. Löschen Sie andernfalls die gesamte Ressourcengruppe, wenn Sie keine der Dateien verwenden möchten.  

### <a name="delete-the-deployment-instance"></a>Löschen der Bereitstellungsinstanz

Löschen Sie einfach die Bereitstellungsinstanz aus dem Azure-Portal, wenn Sie die Ressourcengruppe und den Arbeitsbereich für andere Tutorials und Untersuchungen beibehalten möchten. 

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com//). Navigieren Sie zu Ihrem Arbeitsbereich, und wählen Sie links unter dem Bereich **Ressourcen** die Option **Bereitstellungen** aus. 

1. Wählen Sie die zu löschende Bereitstellung aus, und klicken Sie auf **Delete** (Löschen). 

1. Wählen Sie **Proceed** (Fortfahren) aus.

### <a name="delete-the-resource-group"></a>Löschen der Ressourcengruppe

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial zu automatisierten Machine Learning haben Sie auf der Angebotsseite des Arbeitsbereichs ein Klassifizierungsmodell erstellt und bereitgestellt. Weitere Informationen und nächste Schritte finden Sie in diesen Artikeln:

> [!div class="nextstepaction"]
> [Verwenden eines Webdiensts](how-to-consume-web-service.md)

+ Weitere Informationen zur [Vorverarbeitung](how-to-create-portal-experiments.md#preprocess).
+ Weitere Informationen zur [Datenprofilerstellung](how-to-create-portal-experiments.md#profile).
+ Weitere Informationen zu [automatisiertem Machine Learning](concept-automated-ml.md).

>[!NOTE]
> Dieses Bank Marketing-Dataset wird unter der [Creative Commons (CCO: Public Domain)-Lizenz](https://creativecommons.org/publicdomain/zero/1.0/) zur Verfügung gestellt. Alle Rechte in den einzelnen Inhalten der Datenbank sind gemäß der [Database Contents License](https://creativecommons.org/publicdomain/zero/1.0/) lizenziert und auf [Kaggle](https://www.kaggle.com/janiobachmann/bank-marketing-dataset) verfügbar. Dieses Dataset war ursprünglich in der [UCI Machine Learning Database](https://archive.ics.uci.edu/ml/datasets/bank+marketing) verfügbar.<br><br>
> [Moro et al., 2014] S. Moro, P. Cortez und P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. (Ein datengesteuerter Ansatz zur Prognose des Erfolgs im Bank-Telemarketing.) Decision Support Systems, Elsevier, 62:22-31, Juni 2014.
