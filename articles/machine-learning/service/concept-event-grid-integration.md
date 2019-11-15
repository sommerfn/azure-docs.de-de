---
title: Nutzen von Azure Machine Learning-Ereignissen
titleSuffix: Azure Machine Learning
description: In diesem Artikel erfahren Sie, wie Sie Azure Event Grid verwenden können, um Ereignisse, die von Azure Machine Learning generiert wurden, zu abonnieren, zu verarbeiten und zu kündigen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: andyxu
author: gogowings
ms.date: 11/04/2019
ms.openlocfilehash: 888d294634078d2fe7737426f6cf025ae9948b0a
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73608526"
---
# <a name="consume-azure-machine-learning-events-preview"></a>Nutzen von Azure Machine Learning-Ereignissen (Vorschauversion)

Azure Machine Learning verwaltet den gesamten Lebenszyklus von Prozessen für maschinelles Lernen, wozu Modelltraining, Modellimplementierung und Überwachung gehören. Über Azure Machine Learning-Ereignisse können Anwendungen, indem für sie moderne serverlose Architekturen verwendet werden, während des Lebenszyklus für maschinelles Lernen auf Ereignisse reagieren, so z. B. das Abschließen von Trainingsausführungen, das Registrieren und Bereitstellen von Modellen und das Erkennen von Datendrift. 

Diese Ereignisse werden über [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) veröffentlicht. Über das Azure-Portal, PowerShell oder Azure CLI können Kunden problemlos Ereignisse abonnieren, [indem sie mindestens einen Ereignistyp sowie Filterbedingungen angeben](/azure/event-grid/event-filtering). Kunden können außerdem eine breite Palette von Ereignishandlern erstellen, etwa Azure Functions, Azure Logic Apps oder generische Webhooks. Azure Machine Learning stellt zusammen mit Azure Event Grid eine hochverfügbare, zuverlässige und fehlertolerante Ereignisbereitstellungsplattform bereit, mit der Sie ereignisgesteuerte Anwendungen erstellen können.

Informationen zum Verwenden von Azure Machine Learning mit Azure Event Grid finden Sie unter [Erstellen von ereignisgesteuerten Workflows für maschinelles Lernen (Vorschauversion)](how-to-use-event-grid.md).

## <a name="the-event-model"></a>Ereignismodell 

Azure Event Grid liest Ereignisse aus Quellen, z. B. Azure Machine Learning und andere Azure-Dienste. Diese Ereignisse werden dann an Ereignishandler wie Azure Event Hubs, Azure Functions, Logic Apps und weitere gesendet. In der folgenden ist Abbildung dargestellt, wie Event Grid Quellen und Handler verbindet, diese Darstellung ist jedoch keine vollständige Liste der unterstützten Integrationen.

![Azure Event Grid – funktionales Modell](./media/concept-event-grid-integration/azure-event-grid-functional-model.png)

Weitere Informationen zu Ereignisquellen und Ereignishandlern finden Sie unter [Was ist Event Grid?](/azure/event-grid/overview).

## <a name="azure-machine-learning-event-types"></a>Azure Machine Learning-Ereignistypen

Azure Machine Learning stellt Ereignisse an den verschiedenen Punkten eines Lebenszyklus für maschinelles Lernen bereit: 

| Ereignistypen | BESCHREIBUNG |
| ---------- | ----------- |
| `Microsoft.MachineLearningServices.RunCompleted` | Wird ausgelöst, wenn eine Ausführung eines Experiments für maschinelles Lernen abgeschlossen ist |
| `Microsoft.MachineLearningServices.ModelRegistered` | Wird ausgelöst, wenn ein Machine Learning-Modell im Arbeitsbereich registriert wird |
| `Microsoft.MachineLearningServices.ModelDeployed` | Wird ausgelöst, wenn eine Bereitstellung eines Rückschlussdiensts mit mindestens einem Modell abgeschlossen ist |
| `Microsoft.MachineLearningServices.DatasetDriftDetected` | Wird ausgelöst, wenn ein Datendrifterkennungsauftrag für zwei Datasets abgeschlossen ist |

## <a name="subscribe-to-machine-learning-events"></a>Abonnieren von Machine Learning-Ereignissen

Abonnements für Azure Machine Learning-Ereignisse werden durch rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) geschützt. Nur ein [Mitwirkender oder Besitzer](how-to-assign-roles.md#default-roles) eines Arbeitsbereichs kann Ereignisabonnements erstellen, aktualisieren und löschen.

Ereignisabonnements können anhand einer Vielzahl von Bedingungen gefiltert werden. Auf Ereignisabonnements können während der [Erstellung](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) des Ereignisabonnements oder [zu einem späteren Zeitpunkt](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) Filter angewendet werden. 

### <a name="filter-by-event-type"></a>Filtern nach Ereignistyp
In einem Ereignisabonnement können ein oder mehrere Azure Machine Learning-Ereignistypen angegeben sein.

### <a name="filter-by-event-subject"></a>Filtern nach Ereignisgegenstand
Azure Event Grid unterstützt Gegenstandsfilter auf Grundlage von __beginnt mit__- und __endet mit__-Übereinstimmungen, sodass Ereignisse mit einem übereinstimmenden Gegenstand an den Abonnenten übermittelt werden. Unterschiedliche Machine Learning-Ereignisse haben unterschiedliche Gegenstandsformate.

| Ereignistypen | Gegenstandsformat | Beispielgegenstand |
| ---------- | ----------- | ----------- |
| `Microsoft.MachineLearningServices.RunCompleted` | `experiments/{ExperimentId}/runs/{RunId}` | `experiments/b1d7966c-f73a-4c68-b846-992ace89551f/runs/my_exp1_1554835758_38dbaa94` |
| `Microsoft.MachineLearningServices.ModelRegistered` | `models/{modelName}:{modelVersion}` | `models/sklearn_regression_model:3` |
| `Microsoft.MachineLearningServices.ModelDeployed` | `endpoints/{serviceId}` | `endpoints/my_sklearn_aks` |
| `Microsoft.MachineLearningServices.DatasetDriftDetected` | `datadrift/{data.DataDriftId}/run/{data.RunId}` | `datadrift/4e694bf5-712e-4e40-b06a-d2a2755212d4/run/my_driftrun1_1550564444_fbbcdc0f` |

### <a name="advanced-filtering"></a>Erweiterte Filterung

Azure Event Grid unterstützt auch erweitertes Filtern anhand eines veröffentlichten Ereignisschemas. Ausführliche Informationen zum Azure Machine Learning-Ereignisschema finden Sie unter [Azure Event Grid-Ereignisschema für Azure Machine Learning](../../event-grid/event-schema-machine-learning.md).

Einige Beispiele für erweitertes Filtern, das Sie ausführen können, sind:

* Für das `Microsoft.MachineLearningServices.ModelRegistered`-Ereignis Filtern des Tagwerts eines Modells:

    ```
    --advanced-filter data.ModelTags.key1 StringIn ('value1')
    ```

Weitere Informationen zum Anwenden von Filtern finden Sie unter [Filtern von Ereignissen für Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

## <a name="consume-machine-learning-events"></a>Nutzen von Machine Learning-Ereignissen

Anwendungen, die Machine Learning-Ereignisse behandeln, sollten einigen empfohlene Vorgehensweisen entsprechen:

> [!div class="checklist"]
> * Da mehrere Abonnements zum Weiterleiten von Ereignissen an den gleichen Ereignishandler konfiguriert werden können, ist es wichtig, nicht davon auszugehen, dass Ereignisse aus einer bestimmten Quelle stammen, sondern das Thema der Nachricht zu überprüfen, um sicherzustellen, dass es aus dem Speicherkonto stammt, das Sie erwarten.
> * Überprüfen Sie auf ähnliche Weise, ob Sie auf die Verarbeitung des eventType vorbereitet sind, und gehen Sie nicht davon aus, dass alle Ereignisse, die Sie empfangen, den von Ihnen erwarteten Typen entsprechen.
> * Da Nachrichten in falscher Reihenfolge und mit Verzögerung eintreffen können, verwenden Sie die etag-Felder, um zu verstehen, ob Ihre Informationen zu Objekten weiterhin auf dem neuesten Stand ist.  Verwenden Sie auch die sequencer-Felder, um die Reihenfolge der Ereignisse für ein bestimmtes Objekt zu verstehen.
> * Ignorieren Sie Felder, die Sie nicht verstehen. So müssen Sie sich nicht mit neuen Features auseinandersetzen, die in der Zukunft hinzugefügt werden könnten.
> * Fehlgeschlagene oder abgebrochene Azure Machine Learning-Vorgänge lösen kein Ereignis aus. Wenn beispielsweise bei einer Modellimplementierung ein Fehler auftritt, wird Microsoft.MachineLearningServices.ModelDeployed nicht ausgelöst. Berücksichtigen Sie solche Fehlermodi, wenn Sie Ihre Anwendungen entwerfen. Sie können jederzeit über das Azure Machine Learning SDK, die CLI oder das Portal den Status eines Vorgangs überprüfen und die detaillierten Fehlerursachen ermitteln.

Mit Azure Event Grid können Kunden entkoppelte Meldungshandler erstellen, die durch Azure Machine Learning-Ereignisse ausgelöst werden können. Einige relevante Beispiele für Meldungshandler sind:
* Azure-Funktionen
* Azure Logic Apps
* Azure Event Hubs
* Azure Data Factory-Pipeline
* Generische Webhooks, die auf der Azure-Plattform oder an anderer Stelle gehostet werden

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Event Grid, und probieren Sie Azure Machine Learning-Ereignisse aus:

- [Einführung in Azure Event Grid](../../event-grid/overview.md)
- [Azure Event Grid-Ereignisschema für Azure Machine Learning](../../event-grid/event-schema-machine-learning.md)
- [Erstellen von ereignisgesteuerten Workflows für maschinelles Lernen (Vorschauversion)](how-to-use-event-grid.md)
