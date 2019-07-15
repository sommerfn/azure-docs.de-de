---
title: Azure Kubernetes Services ausführen
titleSuffix: Text Analytics - Azure Cognitive Services
description: Stellen Sie die Textanalysecontainer mit dem Bild der Stimmungsanalyse bei den Azure Kubernetes Services bereit und testen Sie sie in einem Webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: a419ed3b9c0d2c4db9c552642dc5c662786f6730
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561249"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-kubernetes-services-aks"></a>Bereitstellen eines Containers für die Stimmungsanalyse für Azure Kubernetes Services (AKS)

Erfahren Sie, wie Sie den Container Cognitive Services [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) mit dem Image Sentiment Analysis für Azure Kubernetes Services (AKS) bereitstellen. Dieses Verfahren veranschaulicht die Erstellung einer Textanalyse-Ressource, die Erstellung eines zugehörigen Sentiment Analysis-Bildes und die Möglichkeit, diese Orchestrierung der beiden über einen Browser durchzuführen. Die Verwendung von Containern kann die Aufmerksamkeit der Entwickler von der Verwaltung der Infrastruktur auf die Anwendungsentwicklung lenken.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Verfahren müssen mehrere Tools lokal installiert und ausgeführt werden. Verwenden Sie nicht Azure Cloud Shell.

* Verwenden Sie ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
* Text-Editor, z.B.: [Visual Studio Code](https://code.visualstudio.com/download).
* Installieren Sie die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* Installieren Sie den [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
* Eine Azure-Ressource mit dem korrekten Tarif. Nicht alle Tarife können mit diesem Container verwendet werden:
    * Ressource **Textanalyse** nur mit F0- oder Standard-Tarif.
    * Ressource **Cognitive Services** mit dem S0-Tarif.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Kubernetes Services (AKS)](../../containers/includes/create-aks-resource.md)]

## <a name="deploy-text-analytics-container-to-an-aks-cluster"></a>Bereitstellen von Text Analytics-Container in einem AKS-Cluster

1. Öffnen Sie die Azure CLI und melden Sie sich bei Azure an

    ```azurecli
    az login
    ```

1. Melden Sie sich beim AKS-Cluster an (ersetzen Sie das `your-cluster-name` und `your-resource-group` durch die entsprechenden Werte)

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Nachdem dieser Befehl ausgeführt wurde, wird eine Meldung ähnlich der folgenden ausgegeben:

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Wenn Sie über mehrere Abonnements in Ihrem Azure-Konto verfügen und der `az aks get-credentials` Befehl mit einem Fehler zurückgegeben wird, ist ein häufiges Problem, dass Sie das falsche Abonnement verwenden. Stellen Sie einfach den Kontext Ihrer Azure CLI-Sitzung so ein, dass Sie das gleiche Abonnement verwenden, mit dem Sie die Ressourcen erstellt haben, und versuchen Sie es erneut.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. Öffnen Sie den Texteditor Ihrer Wahl (in diesem Beispiel mit __Visual Studio Code__):

    ```azurecli
    code .
    ```

1. Erstellen Sie im Texteditor eine neue Datei namens _sentiment.yaml_und fügen Sie die folgende YAML in diese ein. Achten Sie darauf, dass Sie das `billing/value` und `apikey/value` durch Ihr eigenes ersetzen.

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: sentiment
    spec:
      template:
        metadata:
          labels:
            app: sentiment-app
        spec:
          containers:
          - name: sentiment
            image: mcr.microsoft.com/azure-cognitive-services/sentiment
            ports:
            - containerPort: 5000
            env:
            - name: EULA
              value: "accept"
            - name: billing
              value: # < Your endpoint >
            - name: apikey
              value: # < Your API Key >
     
    --- 
    apiVersion: v1
    kind: Service
    metadata:
      name: sentiment
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: sentiment-app
    ```

1. Speichern Sie die Datei, und schließen Sie den Editor.
1. Führen Sie den Kubernetes-`apply`Befehl mit der _sentiment.yaml_ als Ziel aus:

    ```console
    kuberctl apply -f sentiment.yaml
    ```

    Nachdem der Befehl die Bereitstellungskonfiguration erfolgreich angewendet hat, erscheint eine Meldung ähnlich der folgenden Ausgabe:

    ```
    deployment.apps "sentiment" created
    service "sentiment" created
    ```
1. Verifizieren Sie, ob der POD bereitgestellt wurde:

    ```console
    kubectl get pods
    ```

    Dies gibt den Ausführungsstatus der POD aus:

    ```
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Verifizieren Sie, dass der Dienst verfügbar ist und holen Sie sich die IP-Adresse:

    ```console
    kubectl get services
    ```

    Dadurch wird der Betriebsstatus des _Sentiment_-Services in der POD ausgegeben:

    ```
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Nächste Schritte

* Verwenden weiterer [Cognitive Services-Container](../../cognitive-services-container-support.md)
* Verwenden Sie den [Text Analytics Connected Service](../vs-text-connected-service.md)
