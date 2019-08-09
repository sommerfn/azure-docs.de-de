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
ms.openlocfilehash: 11e8b26c81d3a8f672abbeafc153df73d063cb6e
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68610661"
---
# <a name="deploy-a-machine-learning-model-to-azure-app-service-preview"></a>Bereitstellen eines Machine Learning-Modells in Azure App Service (Vorschauversion)

Hier erfahren Sie, wie Sie ein Modell in Azure Machine Learning Service als Web-App in Azure App Service bereitstellen.

> [!IMPORTANT]
> Azure Machine Learning Service und Azure App Service sind zwar allgemein verfügbar, die Funktion zum Bereitstellen eines Modells über Machine Learning Service in App Service befindet sich jedoch in der Vorschauphase.

Mit Azure Machine Learning Service können Sie Docker-Images auf der Grundlage trainierter Machine Learning-Modelle erstellen. Dieses Image enthält einen Webdienst, der Daten empfängt und an das Modell übermittelt und anschließend die Antwort zurückgibt. Azure App Service kann zum Bereitstellen des Images verwendet werden und bietet die folgenden Features:

* [SSL-Unterstützung](/azure/app-service/app-service-web-ssl-cert-load) für die sichere Kommunikation zwischen Clients und dem Dienst
* [Erweiterung](/azure/azure-monitor/platform/autoscale-get-started?toc=%2fazure%2fapp-service%2ftoc.json) auf mehrere Instanzen ohne erneute Bereitstellung
* [Erweiterte Authentifizierung](/azure/app-service/configure-authentication-provider-aad) für höhere Sicherheit

Weitere Informationen zu den Features von Azure App Service finden Sie unter [App Service: Übersicht](/azure/app-service/overview).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Machine Learning-Dienstbereich. Weitere Informationen finden Sie unter [Erstellen eines Workspace](setup-create-workspace.md)-Artikels.
* Ein in Ihrem Arbeitsbereich registriertes trainiertes Machine Learning-Modell. Falls Sie kein Modell besitzen, können Sie anhand der Informationen unter [Tutorial: Trainieren von Bildklassifikationsmodellen mit MNIST-Daten und Scikit-learn mithilfe von Azure Machine Learning](tutorial-train-models-with-aml.md) ein Modell trainieren und registrieren.
* Ein auf der Grundlage des Modells erstelltes Docker-Image. Wenn Sie über kein Image verfügen, erstellen Sie eins anhand der Informationen unter [Tutorial: Bereitstellen eines Bildklassifizierungsmodells in Azure Container Instances](tutorial-deploy-models-with-aml.md) erstellen.

## <a name="deploy-image-as-a-web-app"></a>Bereitstellen eines Images als Web-App

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Azure Machine Learning-Arbeitsbereich aus. Greifen Sie im Abschnitt __Übersicht__ über den Link __Registrierung__ auf die Azure Container Registry-Instanz für den Arbeitsbereich zu.

    ![Screenshot der Übersicht über den Arbeitsbereich](media/how-to-deploy-app-service/workspace-overview.png)

2. Wählen Sie in Azure Container Registry __Repositorys__ und anschließend den __Namen des Images__ aus, das Sie bereitstellen möchten. Wählen Sie für die bereitzustellende Version den Eintrag __...__ und anschließend __In Web-App bereitstellen__ aus.

    ![Screenshot der Bereitstellung von ACR in einer Web-App](media/how-to-deploy-app-service/deploy-to-web-app.png)

3. Geben Sie zum Erstellen der Web-App einen Websitenamen, ein Abonnement und eine Ressourcengruppe an, und wählen Sie den App Service-Plan und den Standort aus. Wählen Sie abschließend __Erstellen__.

    ![Screenshot des Dialogfelds für eine neue Web-App](media/how-to-deploy-app-service/web-app-for-containers.png)

## <a name="use-the-web-app"></a>Verwenden der Web-App

Wählen Sie im [Azure-Portal](https://portal.azure.com) die im vorherigen Schritt erstellte Web-App aus. Kopieren Sie im Abschnitt __Übersicht__ die __URL__. Dieser Wert ist die __Basis-URL__ des Diensts.

![Screenshot der Übersicht für die Web-App](media/how-to-deploy-app-service/web-app-overview.png)

Der Webdienst, der Anforderungen an das Modell übergibt, befindet sich unter `{baseurl}/score`. Beispiel: `https://mywebapp.azurewebsites.net/score`. Der folgende Python-Code veranschaulicht, wie Sie Daten an die URL übermitteln und die Antwort anzeigen:

```python
import requests
import json

scoring_uri = "https://mywebapp.azurewebsites.net/score"

headers = {'Content-Type':'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

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