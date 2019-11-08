---
title: Ausführen von Batchvorhersagen mit dem Azure Machine Learning-Designer (Vorschau)
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie ein Modell trainieren und eine Batchvorhersagenpipeline mithilfe des Designers einrichten. Stellen Sie die Pipeline als parametrisierten Webdienst bereit, der über eine beliebige HTTP-Bibliothek ausgelöst werden kann.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.reviewer: trbye
ms.author: trbye
author: trevorbye
ms.date: 11/04/2019
ms.custom: Ignite2019
ms.openlocfilehash: c496e57ea5f4b5b7fcda4f9c43c511488ef3c246
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73509434"
---
# <a name="run-batch-predictions-using-azure-machine-learning-designer"></a>Ausführen von Batchvorhersagen mit dem Azure Machine Learning-Designer
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In dieser Anleitung erfahren Sie, wie Sie mit dem Designer ein Modell trainieren und eine Batchvorhersagenpipeline sowie einen Webdienst einrichten. Die Batchvorhersage ermöglicht die kontinuierliche und bedarfsgesteuerte Bewertung von trainierten Modellen für große Datasets. Sie kann optional als Webdienst konfiguriert werden, der über eine beliebige HTTP-Bibliothek ausgelöst werden kann. 

Informationen zum Einrichten von Batchbewertungsdiensten mit dem SDK finden Sie in der begleitenden [Anleitung](how-to-run-batch-predictions.md).

In dieser Anleitung lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines einfachen ML-Experiments in einer Pipeline
> * Erstellen einer parametrisierten Batchrückschlusspipeline
> * Verwalten und Ausführen von Pipelines (manuell oder über einen REST-Endpunkt)

## <a name="prerequisites"></a>Voraussetzungen

1. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](https://aka.ms/AMLFree) aus.

1. Erstellen Sie einen [Arbeitsbereich](tutorial-1st-experiment-sdk-setup.md).

1. Melden Sie sich beim [Azure Machine Learning-Studio](https://ml.azure.com/) an.

In dieser Anleitung wird davon ausgegangen, dass Sie mit den Grundlagen der Erstellung einer einfachen Pipeline im Designer vertraut sind. Eine Einführung in den Designer erhalten Sie im [Tutorial](tutorial-designer-automobile-price-train-score.md). 

## <a name="create-a-pipeline"></a>Erstellen einer Pipeline

Zum Erstellen einer Batchrückschlusspipeline benötigen Sie zunächst ein Machine Learning-Experiment. Zum Erstellen einer Pipeline navigieren Sie in Ihrem Arbeitsbereich zur Registerkarte **Designer** und erstellen eine neue Pipeline, indem Sie die Option **Easy-to-use prebuilt modules** (Benutzerfreundliche vorgefertigte Module) auswählen.

![Designerstartseite](media/how-to-run-batch-predictions-ui/ui-batch-scoring-1.png)

Im Folgenden finden Sie ein einfaches Machine Learning-Modell zu Demonstrationszwecken. Bei den Daten handelt es sich um ein registriertes Dataset, das aus den Azure Open Datasets-Diabetesdaten erstellt wurde. Informationen zum Registrieren von Datasets aus Azure Open Datasets finden Sie im [Anleitungsabschnitt](how-to-create-register-datasets.md#create-datasets-with-azure-open-datasets). Die Daten werden in Trainings-und Validierungssätze aufgeteilt, und ein verstärkter Entscheidungsbaum wird trainiert und bewertet. Zum Erstellen einer Rückschlusspipeline muss die Pipeline mindestens einmal ausgeführt werden. Klicken Sie auf die Schaltfläche **Ausführen**, um die Pipeline auszuführen.

![Erstellen eines einfachen Experiments](media/how-to-run-batch-predictions-ui/ui-batch-scoring-2.png)

## <a name="create-a-batch-inference-pipeline"></a>Erstellen einer Batchrückschlusspipeline

Nach dem Ausführen der Pipeline finden Sie neben **Ausführen** und **Veröffentlichen** eine weitere Option namens **Rückschlusspipeline erstellen** zur Verfügung. Klicken Sie auf die Dropdownliste, und wählen Sie **Batchrückschlusspipeline** aus.

![Erstellen einer Batchrückschlusspipeline](media/how-to-run-batch-predictions-ui/ui-batch-scoring-5.png)

Das Ergebnis ist eine standardmäßige Batchrückschlusspipeline. Dies umfasst einen Knoten für die ursprüngliche Experimenteinrichtung der Pipeline, einen Knoten für Rohdaten zur Bewertung und einen Knoten zum Vergleich der Rohdaten mit der ursprünglichen Pipeline.

![Standardmäßige Batchrückschlusspipeline](media/how-to-run-batch-predictions-ui/ui-batch-scoring-6.png)

Sie können weitere Knoten hinzufügen, um das Verhalten des Batchrückschlussprozesses zu ändern. In diesem Beispiel fügen Sie vor der Bewertung einen Knoten für die Stichprobenentnahme aus den Eingabedaten hinzu. Erstellen Sie einen Knoten **Partition und Beispiel**, und platzieren Sie diesen zwischen den Rohdaten- und Bewertungsknoten. Klicken Sie anschließend auf den Knoten **Partition und Beispiel**, um Zugriff auf die Einstellungen und Parameter zu erhalten.

![Neuer Knoten](media/how-to-run-batch-predictions-ui/ui-batch-scoring-7.png)

Mit dem Parameter *Rate of sampling* (Stichprobenrate) wird gesteuert, von welchem Prozentsatz des ursprünglichen Datasets eine zufällige Stichprobe entnommen werden soll. Dieser Parameter muss regelmäßig angepasst werden, damit Sie ihn als Pipelineparameter verwenden können. Pipelineparameter können zur Laufzeit geändert und in einem Payload-Objekt angegeben werden, wenn die Pipeline von einem REST-Endpunkt aus erneut gestartet wird. 

Um dieses Feld als Pipelineparameter zu aktivieren, klicken Sie auf die Ellipse oberhalb des Felds, und klicken Sie dann auf **Zu Pipelineparameter hinzufügen**. 

![Beispieleinstellungen](media/how-to-run-batch-predictions-ui/ui-batch-scoring-8.png)

Legen Sie als nächstes einen Namen und Standardwert für den Parameter fest. Mit dem Namen wird der Parameter identifiziert und in einem REST-Aufruf angegeben.

![Pipelineparameter](media/how-to-run-batch-predictions-ui/ui-batch-scoring-9.png)

## <a name="deploy-batch-inferencing-pipeline"></a>Bereitstellen der Batchrückschlusspipeline

Sie können die Pipeline nun bereitstellen. Klicken Sie auf die Schaltfläche **Bereitstellen**, und öffnen Sie so die Schnittstelle zum Einrichten eines Endpunkts. Klicken Sie auf die Dropdownliste, und wählen Sie **Neuer PipelineEndpoint** aus.

![Bereitstellen der Pipeline](media/how-to-run-batch-predictions-ui/ui-batch-scoring-10.png)

Geben Sie einen Namen und optional eine Beschreibung für den Endpunkt ein. Im unteren Bereich sehen Sie den `sample-rate`-Parameter, den Sie mit dem Standardwert 0,8 konfiguriert haben. Klicken Sie abschließend auf **Bereitstellen**.

![Einrichten des Endpunkts](media/how-to-run-batch-predictions-ui/ui-batch-scoring-11.png)

## <a name="manage-endpoints"></a>Verwalten von Endpunkten 

Wechseln Sie nach Abschluss der Bereitstellung zur Registerkarte **Endpunkte**, und klicken Sie dann auf den Namen des soeben erstellten Endpunkts.

![Endpunktlink](media/how-to-run-batch-predictions-ui/ui-batch-scoring-12.png)

Auf diesem Bildschirm werden alle veröffentlichten Pipelines unter dem jeweiligen Endpunkt angezeigt. Klicken Sie auf die Rückschlusspipeline.

![Rückschlusspipeline](media/how-to-run-batch-predictions-ui/ui-batch-scoring-13.png)

Auf der Seite mit den Pipelinedetails finden Sie ausführliche Informationen zum Ausführungsverlauf und zur Verbindungszeichenfolge für die Pipeline. Klicken Sie auf die Schaltfläche **Ausführen**, um eine manuelle Ausführung der Pipeline zu erstellen.

![Pipelinedetails](media/how-to-run-batch-predictions-ui/ui-batch-scoring-14.png)

In der Ausführungseinrichtung können Sie eine Beschreibung für die Ausführung angeben und den Wert für alle Pipelineparameter ändern. Führen Sie die Rückschlusspipeline mit einer Abtastrate von 0,9 erneut aus. Klicken Sie auf **Ausführen**, um die Pipeline auszuführen.

![Ausführen der Pipeline](media/how-to-run-batch-predictions-ui/ui-batch-scoring-15.png)

Die Registerkarte **Nutzen** enthält den REST-Endpunkt zum erneuten Ausführen der Pipeline. Sie benötigen einen OAuth 2.0-Authentifizierungsheader vom Typ Bearer, um einen REST-Aufruf auszuführen. Weitere Informationen zum Einrichten der Authentifizierung für Ihren Arbeitsbereich und zum Erstellen eines parametrisierten REST-Aufrufes finden Sie im folgenden [Tutorialabschnitt](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint).

## <a name="next-steps"></a>Nächste Schritte

Zum Trainieren und Bereitstellen eines Regressionsmodells befolgen Sie die Schritte im Designer-[Tutorial](tutorial-designer-automobile-price-train-score.md).