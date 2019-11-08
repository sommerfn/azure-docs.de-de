---
title: Überwachen und Erfassen von Daten von Machine Learning-Webdienst-Endpunkten
titleSuffix: Azure Machine Learning
description: Überwachen Sie Webdienste, die mit Azure Machine Learning bereitgestellt werden, mithilfe von Azure Application Insights.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: lostmygithubaccount
ms.date: 10/11/2019
ms.custom: seoapril2019
ms.openlocfilehash: c02c502dc2ab85a6ae1c602c53723e9b5a758250
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73576736"
---
# <a name="monitor-and-collect-data-from-ml-web-service-endpoints"></a>Überwachen und Erfassen von Daten von ML-Webdienst-Endpunkten
[!INCLUDE [applies-to-skus](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

In diesem Artikel erfahren Sie, wie Sie durch die Aktivierung von Azure Application Insights Daten von Modellen erfassen, die in Webdienst-Endpunkten in Azure Kubernetes Service (AKS) oder Azure Container Instances (ACI) bereitgestellt wurden, und wie Sie diese Modelle überwachen. Sie können nicht nur die Eingabedaten und Antworten eines Endpunkts erfassen, sondern auch Folgendes überwachen:
* Anforderungsraten, Antwortzeiten und Fehlerraten
* Abhängigkeitsraten, Antwortzeiten und Fehlerraten
* Ausnahmen.

[Weitere Informationen zu Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) 


## <a name="prerequisites"></a>Voraussetzungen

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein kostenloses Konto erstellen, bevor Sie beginnen. Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning](https://aka.ms/AMLFree) noch heute aus.

* Ein Azure Machine Learning-Arbeitsbereich, ein lokales Verzeichnis mit Ihren Skripts und das Azure Machine Learning SDK für Python müssen installiert sein. Informationen zum Erfüllen dieser Voraussetzungen finden Sie unter [Konfigurieren einer Entwicklungsumgebung](how-to-configure-environment.md).
* Ein trainiertes Machine Learning-Modell, das in Azure Kubernetes Service (AKS) oder Azure Container Instance (ACI) bereitgestellt werden soll. Wenn Sie keines besitzen, sehen Sie sich das Tutorial zum [Trainieren eines Imageklassifizierungsmodells](tutorial-train-models-with-aml.md) an.

## <a name="web-service-input-and-response-data"></a>Eingabe- und -Antwortdaten eines Webdiensts

Die Eingabe und die Antwort für den Dienst werden entsprechend den Eingaben für das ML-Modell und seiner Vorhersage in den Azure Application Insights-Ablaufverfolgungen unter der Meldung `"model_data_collection"` protokolliert. Sie können Azure Application Insights für den Zugriff auf diese Daten direkt abfragen oder zur längeren Aufbewahrung oder weiteren Verarbeitung einen [fortlaufenden Export](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) in ein Speicherkonto einrichten. Modelldaten können dann im Azure ML-Dienst zum Einrichten von Bezeichnungen, erneuten Trainings, Erklärungen, Datenanalysen oder zu anderen Zwecken verwendet werden. 

## <a name="use-the-azure-portal-to-configure"></a>Verwenden des Azure-Portals für die Konfiguration

Sie können Azure Application Insights im Azure-Portal aktivieren und deaktivieren. 

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) Ihren Arbeitsbereich.

1. Wählen Sie auf der Registerkarte **Bereitstellungen** den Dienst aus, für den Sie Azure Application Insights aktivieren möchten.

   [![Liste der Dienste auf der Registerkarte „Bereitstellungen“](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Wählen Sie **Bearbeiten** aus.

   [![Schaltfläche „Bearbeiten“](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. Aktivieren Sie in **Erweiterte Einstellungen** das Kontrollkästchen **AppInsights-Diagnose aktivieren**.

   [![Aktiviertes Kontrollkästchen zum Aktivieren der Diagnose](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Wählen Sie unten auf dem Bildschirm **Aktualisieren** aus, um die Änderungen zu übernehmen. 

### <a name="disable"></a>Disable
1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) Ihren Arbeitsbereich.
1. Wählen Sie **Bereitstellungen**, den Dienst und dann **Bearbeiten** aus.

   [![Verwenden der Schaltfläche „Bearbeiten“](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. Deaktivieren Sie in **Erweiterte Einstellungen** das Kontrollkästchen **AppInsights-Diagnose aktivieren**. 

   [![Deaktiviertes Kontrollkästchen zum Aktivieren der Diagnose](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Wählen Sie unten auf dem Bildschirm **Aktualisieren** aus, um die Änderungen zu übernehmen. 
 
## <a name="use-python-sdk-to-configure"></a>Verwenden des Python SDK zum Konfigurieren 

### <a name="update-a-deployed-service"></a>Aktualisieren eines bereitgestellten Diensts
1. Suchen Sie den Dienst in Ihrem Arbeitsbereich. Der Wert für `ws` ist der Name des Arbeitsbereichs.

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Aktualisieren Sie Ihren Dienst, und aktivieren Sie Azure Application Insights. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Protokollieren von benutzerdefinierten Ablaufverfolgungen in Ihrem Dienst
Wenn Sie benutzerdefinierte Ablaufverfolgungen protokollieren möchten, führen Sie den im Dokument [Bereitstellung: wie und wo?](how-to-deploy-and-where.md) beschriebenen Standardbereitstellungsprozess für AKS oder ACI aus. Führen Sie dann die folgenden Schritte aus:

1. Aktualisieren Sie die Bewertungsdatei durch Hinzufügen von Druckanweisungen.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Aktualisieren Sie die Dienstkonfiguration.
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Erstellen Sie ein Image, und stellen Sie es in [AKS](how-to-deploy-to-aks.md) oder [ACI](how-to-deploy-to-aci.md) bereit.  

### <a name="disable-tracking-in-python"></a>Deaktivieren der Nachverfolgung in Python

Verwenden Sie den folgenden Code, um Azure Application Insights zu deaktivieren:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```

## <a name="evaluate-data"></a>Evaluieren von Daten
Die Daten Ihres Diensts werden in Ihrem Azure Application Insights-Konto in der Ressourcengruppe gespeichert, in der sich auch Azure Machine Learning befindet.
So zeigen Sie sie an:
1. Navigieren Sie in [Azure Machine Learning Studio](https://ml.azure.com) zu Ihrem Machine Learning Service-Arbeitsbereich, und klicken Sie auf den Link „Application Insights“.

    [![AppInsightsLoc](media/how-to-enable-app-insights/AppInsightsLoc.png)](./media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Wählen Sie die Registerkarte **Übersicht** aus, um einen Standardsatz von Metriken für Ihren Dienst anzuzeigen.

   [![Übersicht](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

1. Wählen Sie **Analytics** aus, um sich die Eingabe- und Ausgabenutzlasten Ihres Webdiensts anzusehen.
1. Wählen sie im Abschnitt „Schema“ die Option **Ablaufverfolgungen** aus, und filtern Sie nach Ablaufverfolgungen mit der Meldung `"model_data_collection"`. In den benutzerdefinierten Dimensionen können Sie die Eingaben, Vorhersagen und andere relevante Details sehen.

   [![Modelldaten](media/how-to-enable-app-insights/model-data-trace.png)](./media/how-to-enable-app-insights/model-data-trace.png#lightbox)


3. Um Ihre benutzerdefinierten Ablaufverfolgungen anzuzeigen, wählen Sie **Analyse** aus.
4. Wählen Sie im Abschnitt „Schema“ den Eintrag **Ablaufverfolgungen** aus. Wählen Sie dann **Ausführen** aus, um die Abfrage auszuführen. Die Daten sollten in einem Tabellenformat angezeigt werden und den benutzerdefinierten Aufrufen in Ihrer Bewertungsdatei zugeordnet sein. 

   [![Benutzerdefinierte Ablaufverfolgungen](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Weitere Informationen zu Azure Application Insights finden Sie unter [Was ist Application Insights?](../../azure-monitor/app/app-insights-overview.md).

## <a name="export-data-for-further-processing-and-longer-retention"></a>Exportieren von Daten zur weiteren Verarbeitung und längeren Aufbewahrung

Sie können den [fortlaufenden Export](https://docs.microsoft.com/azure/azure-monitor/app/export-telemetry) von Azure Application Insights verwenden, um Nachrichten an ein unterstütztes Speicherkonto zu senden, für das eine längere Aufbewahrungsdauer festgelegt werden kann. Die Meldungen vom Typ `"model_data_collection"` werden im JSON-Format gespeichert und können ganz einfach zum Extrahieren von Modelldaten analysiert werden. Die Daten können bei Bedarf mit Azure Data Factory, Azure ML-Pipelines oder anderen Datenverarbeitungstools transformiert werden. Nach der Transformation der Daten können Sie sie im Azure Machine Learning-Arbeitsbereich als Dataset registrieren. Informationen dazu finden Sie unter [Erstellen und Registrieren von Datasets](how-to-create-register-datasets.md).

   [![Fortlaufender Export](media/how-to-enable-app-insights/continuous-export-setup.png)](./media/how-to-enable-app-insights/continuous-export-setup.png)


## <a name="example-notebook"></a>Notebook mit Beispielen

Im Notebook [enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb) werden die in diesem Artikel behandelten Konzepte veranschaulicht. 
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Bereitstellen Ihrer Modelle in Webdienst-Endpunkten sowie zum Aktivieren von Azure Application Insights zum Nutzen der Datenerfassung und Endpunktüberwachung finden Sie unter [Bereitstellen eines Modells in einem Azure Kubernetes Service-Cluster](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-azure-kubernetes-service) bzw. [Bereitstellen eines Modells in Azure Container Instances](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-azure-container-instance).
* Unter [MLOps: Verwalten, Bereitstellen und Überwachen von Modellen mit Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/concept-model-management-and-deployment) erfahren Sie, wie Sie von Modellen in der Produktion erfasste Daten nutzen. Solche Daten können Sie dabei unterstützen, Ihren Machine Learning-Prozess kontinuierlich zu verbessern. 
