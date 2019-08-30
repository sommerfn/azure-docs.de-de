---
title: Bereitstellen von ML-Modellen in Azure App Service (Vorschauversion)
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie mithilfe von Azure Machine Learning Service ein Modell in einer Web-App in Azure App Service bereitstellen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/01/2019
ms.openlocfilehash: ada2a19de12c2f3f6b23fcc3d759afb0c747d37d
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69897402"
---
# <a name="deploy-a-machine-learning-model-to-azure-app-service-preview"></a>Bereitstellen eines Machine Learning-Modells in Azure App Service (Vorschauversion)

Hier erfahren Sie, wie Sie ein Modell in Azure Machine Learning Service als Web-App in Azure App Service bereitstellen.

> [!IMPORTANT]
> Azure Machine Learning Service und Azure App Service sind zwar allgemein verfügbar, die Funktion zum Bereitstellen eines Modells über Machine Learning Service in App Service befindet sich jedoch in der Vorschauphase.

Mit Azure Machine Learning Service können Sie Docker-Images auf der Grundlage trainierter Machine Learning-Modelle erstellen. Dieses Image enthält einen Webdienst, der Daten empfängt und an das Modell übermittelt und anschließend die Antwort zurückgibt. Azure App Service kann zum Bereitstellen des Images verwendet werden und bietet die folgenden Features:

* Erweiterte [Authentifizierung](/azure/app-service/configure-authentication-provider-aad) für höhere Sicherheit. Zu den Authentifizierungsmethoden zählen Azure Active Directory und mehrstufige Authentifizierung.
* [Autoskalierung](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json) ohne erneute Bereitstellung
* [SSL-Unterstützung](/azure/app-service/app-service-web-ssl-cert-load) für die sichere Kommunikation zwischen Clients und dem Dienst

Weitere Informationen zu den Features von Azure App Service finden Sie unter [App Service: Übersicht](/azure/app-service/overview).

> [!IMPORTANT]
> Wenn die mit dem bereitgestellten Modell verwendeten Bewertungsdaten oder die Ergebnisse der Bewertung protokolliert werden sollen, sollten Sie die Bereitstellung stattdessen in Azure Kubernetes Service durchführen. Weitere Informationen finden Sie unter [Sammeln von Daten zu Ihren Produktionsmodellen](how-to-enable-data-collection.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Dienstbereich. Weitere Informationen finden Sie unter [Erstellen eines Workspace](how-to-manage-workspace.md)-Artikels.
* Ein in Ihrem Arbeitsbereich registriertes trainiertes Machine Learning-Modell. Falls Sie kein Modell besitzen, können Sie anhand der Informationen unter [Tutorial: Trainieren von Bildklassifikationsmodellen mit MNIST-Daten und Scikit-learn mithilfe von Azure Machine Learning](tutorial-train-models-with-aml.md) ein Modell trainieren und registrieren.

    > [!IMPORTANT]
    > Bei den in diesem Artikel verwendeten Codeausschnitten wird davon ausgegangen, dass Sie die folgenden Variablen festgelegt haben:
    >
    > * `ws`: Ihr Azure Machine Learning-Arbeitsbereich
    > * `model`: das registrierte Modell, das bereitgestellt wird
    > * `inference_config`: die Rückschlusskonfiguration für das Modell
    >
    > Weitere Informationen zum Festlegen dieser Variablen finden Sie unter [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](how-to-deploy-and-where.md).

## <a name="prepare-for-deployment"></a>Vorbereiten der Bereitstellung

Vor der Bereitstellung müssen Sie die Elemente definieren, die zum Ausführen des Modells als Webdienst erforderlich sind. Im Folgenden werden die allgemeinen für eine Bereitstellung erforderlichen Elemente beschrieben:

* Ein __Eingabeskript__. Dieses Skript akzeptiert Anforderungen, bewertet die Anforderung mithilfe des Modells und gibt die Ergebnisse zurück.

    > [!IMPORTANT]
    > Das Eingabeskript ist spezifisch für Ihr Modell. Es muss das Format der Daten der eingehenden Anforderung, das Format der im Modell erwarteten Daten und das Format der an Clients zurückgegebenen Daten erkennen.
    >
    > Wenn die Anforderungsdaten in einem Format vorliegen, das in Ihrem Modell nicht verwendet werden kann, können sie im Skript in ein akzeptables Format umgewandelt werden. Außerdem kann die Antwort umgewandelt werden, bevor sie an den Client zurückgegeben wird.

    > [!IMPORTANT]
    > Das Azure Machine Learning SDK bietet keine Möglichkeit des Zugriffs auf Ihren Datenspeicher oder Ihre Datasets durch den Webdienst. Wenn das bereitgestellte Modell auf Daten zugreifen muss, die außerhalb der Bereitstellung gespeichert sind – wie z.B. in einem Azure Storage-Konto –, müssen Sie mit dem entsprechenden SDK eine benutzerdefinierte Codelösung entwickeln. Ein Beispiel hierfür ist das [Azure Storage SDK für Python](https://github.com/Azure/azure-storage-python).
    >
    > Eine andere Alternative, die in Ihrem Szenario funktionieren könnte, ist die [Batchvorhersage](how-to-run-batch-predictions.md), die beim Erstellen von Bewertungen Zugriff auf Datenspeicher bietet.

    Weitere Informationen zu Eingabeskripts finden Sie unter [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](how-to-deploy-and-where.md).

* **Abhängigkeiten**, z. B. Hilfsprogramme oder Python/Conda-Pakete, die zum Ausführen des Eingabeskripts oder Modells erforderlich sind

Diese Entitäten werden in einer __Rückschlusskonfiguration__ gekapselt. Die Rückschlusskonfiguration verweist auf das Eingabeskript und andere Abhängigkeiten.

> [!IMPORTANT]
> Beim Erstellen einer Rückschlusskonfiguration zur Verwendung mit Azure App Service müssen Sie ein [Environment](https://docs.microsoft.com//python/api/azureml-core/azureml.core.environment%28class%29?view=azure-ml-py)-Objekt verwenden. Im folgenden Beispiel wird veranschaulicht, wie ein Environment-Objekt erstellt und mit einer Rückschlusskonfiguration verwendet wird:
>
> ```python
> from azureml.core import Environment
> from azureml.core.environment import CondaDependencies
>
> # Create an environment and add conda dependencies to it
> myenv = Environment(name="myenv")
> # Enable Docker based environment
> myenv.docker.enabled = True
> # Build conda dependencies
> myenv.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
> ```

Weitere Informationen zu Umgebungen finden Sie unter [Erstellen und Verwalten von Umgebungen zum Training und zur Bereitstellung](how-to-use-environments.md).

Weitere Informationen zur Rückschlusskonfiguration finden Sie unter [Bereitstellen von Modellen mit dem Azure Machine Learning-Dienst](how-to-deploy-and-where.md).

> [!IMPORTANT]
> Bei der Bereitstellung in Azure App Service müssen Sie keine __Bereitstellungskonfiguration__ erstellen.

## <a name="create-the-image"></a>Erstellen des Images

Verwenden Sie [Model.package](https://docs.microsoft.com//python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#package-workspace--models--inference-config--generate-dockerfile-false-), um das Docker-Image zu erstellen, das in Azure App Service bereitgestellt wird. Der folgende Codeausschnitt veranschaulicht, wie ein neues Image aus dem Modell und der Rückschlusskonfiguration erstellt wird:

```python
package = Model.package(ws, [model], inference_config)
package.wait_for_creation(show_output=True)
```

Bei `show_output=True` wird die Ausgabe des Docker-Buildprozesses angezeigt. Nach Abschluss des Prozesses wurde das Image in der Azure Container Registry-Instanz für Ihren Arbeitsbereich erstellt.

## <a name="deploy-image-as-a-web-app"></a>Bereitstellen eines Images als Web-App

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Azure Machine Learning-Arbeitsbereich aus. Greifen Sie im Abschnitt __Übersicht__ über den Link __Registrierung__ auf die Azure Container Registry-Instanz für den Arbeitsbereich zu.

    [![Screenshot der Übersicht über den Arbeitsbereich](media/how-to-deploy-app-service/workspace-overview.png)](media/how-to-deploy-app-service/workspace-overview-expanded.png)

2. Wählen Sie in Azure Container Registry __Repositorys__ und anschließend den __Namen des Images__ aus, das Sie bereitstellen möchten. Wählen Sie für die bereitzustellende Version den Eintrag __...__ und anschließend __In Web-App bereitstellen__ aus.

    [![Screenshot der Bereitstellung von ACR in einer Web-App](media/how-to-deploy-app-service/deploy-to-web-app.png)](media/how-to-deploy-app-service/deploy-to-web-app-expanded.png)

3. Geben Sie zum Erstellen der Web-App einen Websitenamen, ein Abonnement und eine Ressourcengruppe an, und wählen Sie den App Service-Plan und den Standort aus. Wählen Sie abschließend __Erstellen__.

    ![Screenshot des Dialogfelds für eine neue Web-App](media/how-to-deploy-app-service/web-app-for-containers.png)

## <a name="use-the-web-app"></a>Verwenden der Web-App

Wählen Sie im [Azure-Portal](https://portal.azure.com) die im vorherigen Schritt erstellte Web-App aus. Kopieren Sie im Abschnitt __Übersicht__ die __URL__. Dieser Wert ist die __Basis-URL__ des Diensts.

[![Screenshot der Übersicht über die Web-App](media/how-to-deploy-app-service/web-app-overview.png)](media/how-to-deploy-app-service/web-app-overview-expanded.png)

Der Webdienst, der Anforderungen an das Modell übergibt, befindet sich unter `{baseurl}/score`. Beispiel: `https://mywebapp.azurewebsites.net/score`. Der folgende Python-Code veranschaulicht, wie Sie Daten an die URL übermitteln und die Antwort anzeigen:

```python
import requests
import json

scoring_uri = "https://mywebapp.azurewebsites.net/score"

headers = {'Content-Type':'application/json'}

print(headers)
    
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10],
    [10,9,8,7,6,5,4,3,2,1]
]})

response = requests.post(scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Konfigurieren Ihrer Web-App finden Sie in der Dokumentation zu [App Service für Linux](/azure/app-service/containers/).
* Weitere Informationen zur Skalierung finden Sie unter [Erste Schritte mit der automatischen Skalierung in Azure](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json).
* Weitere Informationen zur SSL-Unterstützung finden Sie unter [Verwenden eines SSL-Zertifikats in Ihrem Anwendungscode in Azure App Service](/azure/app-service/app-service-web-ssl-cert-load).
* Weitere Informationen zur Authentifizierung finden Sie unter [Konfigurieren Ihrer App Service-App zur Verwendung der Azure Active Directory-Anmeldung](/azure/app-service/configure-authentication-provider-aad).
* [Consume a ML Model deployed as a web service (Nutzen eines als Webdienst bereitgestellten Azure Machine Learning-Modells)](how-to-consume-web-service.md).