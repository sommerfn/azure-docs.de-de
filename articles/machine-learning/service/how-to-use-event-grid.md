---
title: Erstellen von ereignisgesteuerten Workflows für maschinelles Lernen
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie Event Grid mit Azure Machine Learning verwenden, um ereignisgesteuerte Lösungen zu ermöglichen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shipatel
author: shivp950
ms.reviewer: larryfr
ms.date: 11/04/2019
ms.openlocfilehash: b10b848918fda7a1f271a4e617b1706971f103d7
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73622338"
---
# <a name="create-event-driven-machine-learning-workflows-preview"></a>Erstellen von ereignisgesteuerten Workflows für maschinelles Lernen (Vorschauversion)

[Azure Event Grid](https://docs.microsoft.com/azure/event-grid/) unterstützt Azure Machine Learning-Dienstereignisse. Beispielsweise können Sie Ereignisse aus einem Ausführungsabschluss, einer Modellregistrierung, einer Modellimplementierung und einer Datendrifterkennung verwenden, die für einen Arbeitsbereich gelten.

Weitere Informationen finden Sie unter [Nutzen von Azure Machine Learning-Ereignissen](concept-event-grid-integration.md) und [Azure Event Grid-Ereignisschema für Azure Machine Learning](/azure/event-grid/event-schema-machine-learning).

Verwenden Sie Event Grid, um übliche Szenarien wie die folgenden zu ermöglichen:

* Auslösen von Pipelines für erneutes Trainieren
* Streaming von Ereignissen aus Azure Machine Learning an verschiedene Endpunkte

## <a name="prerequisites"></a>Voraussetzungen

* Mitwirkender- oder Besitzerzugriff auf den Azure Machine Learning-Arbeitsbereich, für den Sie Ereignisse erstellen.
* Wählen Sie einen Ereignishandlerendpunkt aus, z. B. einen Webhook oder einen Event Hub. Weitere Informationen finden Sie unter [Ereignishandler](https://docs.microsoft.com/azure/event-grid/event-handlers). 

## <a name="register-resource-providers"></a>Registrieren von Ressourcenanbietern

Wenn Sie Azure Event Grid oder Machine Learning vor dem 01. November 2019 verwendet haben, müssen Sie möglicherweise die Ressourcenanbieter erneut registrieren, damit Sie die Schritte in diesem Dokument ausführen können. Führen Sie die folgenden Schritte aus, um die Anbieter erneut zu registrieren:

1. Wechseln Sie zum Azure-Portal, und wählen Sie __Abonnements__ aus. Wählen Sie das Abonnement aus, mit dem Sie arbeiten möchten.
1. Wählen Sie __Ressourcenanbieter__ aus, und suchen Sie dann nach __EventGrid__.
1. Wählen Sie den Eintrag __Microsoft.EventGrid__ aus, und wählen Sie dann __Erneut registrieren__ aus.

    ![re-register-resource-provider](media/how-to-use-event-grid/re-register-resource-provider.png)

1. Suchen Sie nach __MachineLearningServices__, wählen Sie den Eintrag aus, und wählen Sie dann __Erneut registrieren__ aus.

> [!TIP]
> Wenn Sie nicht die Berechtigungen haben, diese Schritte auszuführen, bitten Sie Ihren Abonnementadministrator, sie auszuführen.

## <a name="configure-machine-learning-events-using-the-azure-portal"></a>Konfigurieren von Ereignissen für maschinelles Lernen mit dem Azure-Portal

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zu Ihrem Azure Machine Learning-Arbeitsbereich.

1. Wählen Sie in der linken Leiste die Option __Ereignisse__ aus, und wählen Sie dann **Ereignisabonnements** aus. 

    ![select-events-in-workspace.png](media/how-to-use-event-grid/select-event.png)

1. Wählen Sie den Typ des zu nutzenden Ereignisses aus. Im folgenden Screenshot sind beispielsweise __Modell registriert__, __Model bereitgestellt__, __Ausführung abgeschlossen__ und __Datasetdrift erkannt__ aktiviert:

    ![add-event-type](media/how-to-use-event-grid/add-event-type.png)

1. Wählen Sie den Endpunkt aus, für den das Ereignis veröffentlicht werden soll. Im folgenden Screenshot ist __Event Hub__ der ausgewählte Endpunkt:

    ![select-event-handler](media/how-to-use-event-grid/select-event-handler.png)

Nachdem Sie Ihre Auswahl bestätigt haben, klicken Sie auf __Erstellen__. Nach der Konfiguration werden diese Ereignisse per Pushvorgang an ihren Endpunkt übermittelt.

## <a name="set-up-azure-event-grid-using-cli"></a>Einrichten von Azure Event Grid über CLI

Sie können entweder die neueste [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) installieren oder die Azure Cloud Shell verwenden, die als Bestandteil Ihres Azure-Abonnements bereitgestellt wird.

Um die Event Grid-Erweiterung zu installieren, verwenden Sie den folgenden Befehl in der CLI (Command-Line Interface, Befehlszeilenschnittstelle):

```azurecli-interactive
az add extension --name eventgrid
```

Im folgenden Beispiel wird veranschaulicht, wie Sie ein Azure-Abonnement auswählen und dann ein neues Ereignisabonnement für Azure Machine Learning erstellen:

```azurecli-interactive
# Select the Azure subscription that contains the workspace
az account set --subscription "<name or ID of the subscription>"

# Subscribe to the machine learning workspace.
az eventgrid event-subscription create \
  --name {eventGridFilterName} \
  --source-resource-id "/subscriptions/{subId}/resourceGroups/{rgName}/ \providers/Microsoft.MachineLearningServices/workspaces/{wsName}" \
  --endpoint {event handler endpoint} \
  --included-event-types Microsoft.MachineLearningServices.ModelRegistered \
  --subject-begins-with "models/mymodelname"
```

## <a name="sample-scenarios"></a>Beispielszenarien

### <a name="use-a-logic-app-to-send-email-alerts"></a>Verwenden einer Logik-App, um E-Mail-Warnungen zu senden

Nutzen Sie [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/), um E-Mails für Ihre Ereignisse zu konfigurieren. Passen Sie diese mit Bedingungen an, und geben Sie Empfänger an, um die Zusammenarbeit und Beachtung zwischen Teams zu ermöglichen, die zusammenarbeiten.

1. Wechseln Sie im Azure-Portal zu Ihrem Azure Machine Learning-Arbeitsbereich, und wählen Sie in der linken Leiste die Registerkarte „Ereignisse“ aus. Wählen Sie dort die Option __Logik-Apps__ aus. 

    ![select-logic-ap](media/how-to-use-event-grid/select-logic-ap.png)

1. Melden Sie sich bei der Benutzeroberfläche für Logik-Apps an, und wählen Sie „MachineLearningServices“ als Thementyp aus. 

    ![select-topic-type](media/how-to-use-event-grid/select-topic-type.png)

1. Wählen Sie die Ereignisse aus, für die eine Benachrichtigung erfolgen soll. Im folgenden Screenshot ist beispielsweise __RunCompleted__ ausgewählt.

    ![select-event-runcomplete](media/how-to-use-event-grid/select-event-runcomplete.png)

1. Sie können auch Filter hinzufügen, damit die Logik-App nur für eine Teilmenge von Ereignistypen ausgelöst wird. Im folgenden Screenshot wird ein __Präfixfilter__ von __/datadriftID/runs/__ verwendet.

    ![filter-events](media/how-to-use-event-grid/filtering-events.png)

1. Fügen Sie nun einen Schritt hinzu, um dieses Ereignis zu nutzen und nach E-Mails zu suchen. Es gibt mehrere unterschiedliche E-Mail-Konten, die Sie zum Empfangen von Ereignissen verwenden können. Außerdem können Sie Bedingungen dazu festlegen, wann eine E-Mail-Benachrichtigung gesendet werden soll.

    ![select-email-action](media/how-to-use-event-grid/select-email-action.png)

1. Wählen Sie __E-Mail senden__ aus, und geben Sie Werte für die Parameter ein. In den Betreff können Sie den __Ereignistyp__ und das __Thema__ einfügen, um das Filtern von Ereignissen zu vereinfachen. Sie können auch in den Nachrichtentext einen Link zur Arbeitsbereichsseite für Ausführungen einfügen. 

    ![configure-email-body](media/how-to-use-event-grid/configure-email-body.png)

1. Um diese Aktion zu speichern, wählen Sie **Speichern unter** in der linken Ecke der Seite aus. Bestätigen Sie in der rechten Leiste, die angezeigt wird, die Erstellung dieser Aktion.

    ![confirm-logic-app-create](media/how-to-use-event-grid/confirm-logic-app-create.png)


### <a name="use-a-logic-app-to-trigger-retraining-workflows-when-data-drift-occurs"></a>Verwenden einer Logik-App, um ein erneutes Trainieren von Workflows auszulösen, wenn Datendrift auftritt

Modelle werden mit der Zeit zu veralteten Modellen, sodass sie in dem Kontext, in dem sie ausgeführt werden, nicht länger nützlich sind. Eine Möglichkeit zum Feststellen, ob es an der Zeit ist, das Modell erneut zu trainieren, ist das Erkennen von Datendrift. 

In diesem Beispiel wird gezeigt, wie Event Grid mit einer Azure-Logik-App verwendet werden kann, um ein erneute Trainieren auszulösen. Im Beispiel wird eine Azure Data Factory-Pipeline ausgelöst, wenn Datendrift zwischen dem Trainings- und dem Nutzungsdataset eines Modells auftritt.

Führen Sie zur Vorbereitung zunächst die folgenden Aktionen aus:

* Einrichten einer Datasetüberwachung zum [Erkennen von Datendrift]( https://aka.ms/datadrift) in einem Arbeitsbereich
* Erstellen einer veröffentlichten [Azure Data Factory-Pipeline](https://docs.microsoft.com/azure/data-factory/)

In diesem Beispiel wird eine einfache Data Factory-Pipeline verwendet, um Dateien in einen Blobspeicher zu kopieren und eine veröffentlichte Machine Learning-Pipeline auszuführen. Weitere Informationen zu diesem Szenario finden Sie unter [Ausführen von Azure Machine Learning Service-Pipelines in Azure Data Factory-Pipelines](https://docs.microsoft.com/azure/data-factory/transform-data-machine-learning-service).

![adf-mlpipeline-stage](media/how-to-use-event-grid/adf-mlpipeline-stage.png)

1. Beginnen Sie mit dem Erstellen der Logik-App. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), suchen Sie nach „Logik-Apps“, und wählen Sie „Erstellen“ aus.

    ![search-logic-app](media/how-to-use-event-grid/search-for-logic-app.png)

1. Geben Sie die erforderlichen Informationen ein. Um die Umgebung zu vereinfachen, verwenden Sie dasselbe Abonnement und dieselbe Ressourcengruppe wie Ihre Azure Data Factory-Pipeline und Ihr Azure Machine Learning-Arbeitsbereich.

    ![set-up-logic-app-for-adf](media/how-to-use-event-grid/set-up-logic-app-for-adf.png)

1. Nachdem Sie die Logik-App erstellt haben, wählen Sie __Bei Auftreten eines Event Grid-Ressourcenereignisses__ aus. 

    ![select-event-grid-trigger](media/how-to-use-event-grid/select-event-grid-trigger.png)

1. Melden Sie sich an, und geben Sie die Details für das Ereignis ein. Legen Sie __Ressourcenname__ auf den Namen des Arbeitsbereichs fest. Legen Sie __Ereignistyp__ auf __DatasetDriftDetected__ fest.

    ![login-and-add-event](media/how-to-use-event-grid/login-and-add-event.png)

1. Fügen Sie einen neuen Schritt hinzu, und suchen Sie nach __Azure Data Factory__. Wählen Sie __Pipelineausführung erstellen__ aus. 

    ![create-adfpipeline-run](media/how-to-use-event-grid/create-adfpipeline-run.png)

1. Melden Sie sich an, und geben die veröffentlichte Azure Data Factory-Pipeline an, die ausgeführt werden soll.

    ![specify-adf-pipeline](media/how-to-use-event-grid/specify-adf-pipeline.png)

1. Speichern und erstellen Sie die Logik-App über die **Speichern**-Schaltfläche oben links auf der Seite. Um Ihre App anzuzeigen, wechseln Sie in Ihrem Arbeitsbereich in das [Azure-Portal](https://portal.azure.com), und klicken Sie auf **Ereignisse**.

    ![show-logic-app-webhook](media/how-to-use-event-grid/show-logic-app-webhook.png)

Ab jetzt wird die Data Factory-Pipeline ausgelöst, wenn eine Datendrift auftritt. Die Details zu Ihrer Datendriftausführung und Machine Learning-Pipeline werden im [neuen Arbeitsbereichsportal](https://ml.azure.com) angezeigt. 

![view-in-workspace](media/how-to-use-event-grid/view-in-workspace.png)


### <a name="use-azure-functions-to-deploy-a-model-based-on-tags"></a>Verwenden von Azure Functions, um ein Modell auf Basis von Tags bereitzustellen

Ein Azure Machine Learning-Modellobjekt enthält Parameter, über die Sie Bereitstellungen abwickeln können, so z. B. Modellname, Version, Tag und Eigenschaft. Das Modellregistrierungsereignis kann einen Endpunkt auslösen, und Sie können eine Azure-Funktion verwenden, um ein Modell entsprechend den Werten dieser Parameter bereitzustellen.

Ein Beispiel finden Sie im Repository unter [https://github.com/Azure-Samples/MachineLearningSamples-NoCodeDeploymentTriggeredByEventGrid](https://github.com/Azure-Samples/MachineLearningSamples-NoCodeDeploymentTriggeredByEventGrid). Führen Sie die Schritte aus, die in der **README**-Datei angegeben sind.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu verfügbaren Ereignissen finden Sie unter [Azure Event Grid-Ereignisschema für Azure Machine Learning](/azure/event-grid/event-schema-machine-learning).