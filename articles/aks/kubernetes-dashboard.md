---
title: Verwalten eines Azure Kubernetes Service-Clusters mit dem Webdashboard
description: Erfahren Sie mehr über die Verwendung des integrierten Kubernetes-Dashboards mit Webbenutzeroberfläche zur Verwaltung eines Azure Kubernetes Service-Clusters (AKS).
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: mlearned
ms.openlocfilehash: 5aa8268fee7d43ad13ea8710760ba493683f502e
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126885"
---
# <a name="access-the-kubernetes-web-dashboard-in-azure-kubernetes-service-aks"></a>Zugreifen auf das Kubernetes-Webdashboard in Azure Kubernetes Service (AKS)

Kubernetes enthält ein Webdashboard, das für einfache Verwaltungsvorgänge verwendet werden kann. In diesem Dashboard können Sie den Integritätsstatus und grundlegende Metriken für Ihre Anwendungen anzeigen, Dienste erstellen und bereitstellen und vorhandene Anwendungen bearbeiten. In diesem Artikel wird erläutert, wie Sie mithilfe von Azure CLI auf das Kubernetes-Dashboard zugreifen können. Anschließend werden einige grundlegende Vorgänge im Dashboard vorgestellt.

Weitere Informationen zum Kubernetes-Dashboard finden Sie unter [Web UI (Dashboard)][kubernetes-dashboard] (Webbenutzeroberfläche (Dashboard)).

## <a name="before-you-begin"></a>Voraussetzungen

Bei den Schritten in diesem Dokument wird davon ausgegangen, dass Sie einen AKS-Cluster erstellt und eine `kubectl`-Verbindung mit dem Cluster hergestellt haben. Wenn Sie einen AKS-Cluster erstellen müssen, finden Sie weitere Informationen in der [AKS-Schnellstartanleitung][aks-quickstart].

Außerdem muss die Version 2.0.46 oder höher der Azure-Befehlszeilenschnittstelle installiert und konfiguriert sein. Führen Sie  `az --version`  aus, um die Version zu ermitteln. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie weitere Informationen unter  [Installieren der Azure CLI][install-azure-cli].

## <a name="start-the-kubernetes-dashboard"></a>Starten des Kubernetes-Dashboards

Verwenden Sie den Befehl [az aks browse][az-aks-browse], um das Kubernetes-Dashboard zu starten. Im folgenden Beispiel wird das Dashboard für den Cluster namens *myAKSCluster* in der Ressourcengruppe namens *myResourceGroup* geöffnet:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Der Befehl erstellt einen Proxy zwischen Ihrem Entwicklungssystem und der Kubernetes-API und öffnet einen Webbrowser mit dem Kubernetes-Dashboard. Wenn ein Webbrowser das Kubernetes-Dashboard nicht öffnet, kopieren Sie die in der Azure-Befehlszeilenschnittstelle angegebene URL-Adresse (in der Regel `http://127.0.0.1:8001`).

![Anmeldeseite des Kubernetes-Webdashboards](./media/kubernetes-dashboard/dashboard-login.png)

Sie haben folgende Möglichkeiten, sich beim Dashboard Ihres Clusters anzumelden:

* Über eine [kubeconfig-Datei][kubeconfig-file]. Eine kubeconfig-Datei können Sie mit dem Befehl [az aks get-credentials][az-aks-get-credentials] generieren.
* Über ein Token, z. B. ein [Dienstkontotoken][aks-service-accounts] oder ein Benutzertoken. In [AAD-fähigen Clustern][aad-cluster] handelt es sich dabei um ein AAD-Token. Sie können `kubectl config view` verwenden, um die Token in der kubeconfig-Datei aufzulisten. Weitere Informationen zum Erstellen eines AAD-Tokens zur Verwendung mit einem AKS-Cluster finden Sie unter [Integrieren von Azure Active Directory in Azure Kubernetes Service mit der Azure CLI][aad-cluster].
* Über das Standarddienstkonto des Dashboards, das verwendet wird, wenn Sie auf *Überspringen* klicken.

> [!WARNING]
> Machen Sie das Kubernetes-Dashboard unabhängig von der verwendeten Authentifizierungsmethode niemals öffentlich verfügbar.
> 
> Wenn Sie die Authentifizierung für das Kubernetes-Dashboard einrichten, empfiehlt es sich, ein Token statt des Standarddienstkontos des Dashboards zu verwenden. Ein Token ermöglicht jedem Benutzer, seine eigenen Berechtigungen zu verwenden. Wenn Sie das Standarddienstkonto des Dashboards verwenden, kann ein Benutzer unter Umständen seine eigenen Berechtigungen umgehen und stattdessen das Dienstkonto verwenden.
> 
> Wenn Sie das Standarddienstkonto des Dashboards verwenden und in Ihrem AKS-Cluster die rollenbasierte Zugriffssteuerung verwendet wird, muss ein *ClusterRoleBinding*-Element erstellt werden, bevor Sie ordnungsgemäß auf das Dashboard zugreifen können. Das Kubernetes-Dashboard wird standardmäßig mit minimalem Lesezugriff bereitgestellt und zeigt RBAC-Zugriffsfehler an. Ein Clusteradministrator kann zusätzlichen Zugriff auf das Dienstkonto *kubernetes-dashboard* gewähren, dies kann jedoch ein Vektor für Berechtigungsausweitung sein. Sie können auch Azure Active Directory-Authentifizierung integrieren, um differenziertere Zugriffsmöglichkeiten zu bieten.
>
> Verwenden Sie wie im folgenden Beispiel dargestellt den Befehl [kubectl create clusterrolebinding][kubectl-create-clusterrolebinding], um eine Bindung zu erstellen. **Diese Beispielbindung wendet keine zusätzlichen Authentifizierungskomponenten an und kann daher zu unsicherer Verwendung führen.**
>
> ```console
> kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
> ```
> 
> Sie können nun in Ihrem RBAC-fähigen Cluster auf das Kubernetes-Dashboard zugreifen. Verwenden Sie wie im vorherigen Schritt beschrieben den Befehl [az aks browse][az-aks-browse], um das Kubernetes-Dashboard zu starten.
>
> Wenn in Ihrem Cluster keine rollenbasierte Zugriffssteuerung verwendet wird, wird die Erstellung eines *ClusterRoleBinding*-Elements nicht empfohlen.
> 
> Weitere Informationen zur Verwendung der unterschiedlichen Authentifizierungsmethoden finden Sie im Wiki des Kubernetes-Dashboards unter [Zugriffssteuerung][dashboard-authentication].

Nachdem Sie eine Methode für die Anmeldung ausgewählt haben, wird das Kubernetes-Dashboard angezeigt. Wenn Sie ein *Token* oder *Überspringen* verwenden, werden im Kubernetes-Dashboard die Berechtigungen des aktuell angemeldeten Benutzers für den Zugriff auf den Cluster verwendet.

![Die Übersichtsseite des Kubernetes-Webdashboards](./media/kubernetes-dashboard/dashboard-overview.png)

## <a name="create-an-application"></a>Erstellen einer Anwendung

Um zu demonstrieren, wie das Kubernetes-Dashboard die Komplexität von Verwaltungsaufgaben verringern kann, erstellen wir nun eine Anwendung. Sie können eine Anwendung über das Kubernetes-Dashboard erstellen, indem Sie Texteingaben oder eine YAML-Datei bereitstellen oder einen grafischen Assistenten verwenden.

Um eine Anwendung zu erstellen, führen Sie die folgenden Schritte aus:

1. Wählen Sie die Schaltfläche **Erstellen** in der rechten oberen Ecke aus.
1. Um den grafischen Assistenten zu verwenden, wählen Sie **App erstellen** aus.
1. Geben Sie einen Namen für die Bereitstellung an, z.B. *nginx*.
1. Geben Sie den Namen des zu verwendenden Containerimages ein, z.B. *nginx:1.15.5*.
1. Um Port 80 für Webdatenverkehr verfügbar zu machen, erstellen Sie einen Kubernetes-Dienst. Wählen Sie unter **Dienst** die Option **Extern** aus, und geben Sie für den Port und den Zielport jeweils **80** ein.
1. Wenn Sie fertig sind, wählen Sie **Bereitstellen** aus, um die App zu erstellen.

![Bereitstellen einer App im Kubernetes-Webdashboard](./media/kubernetes-dashboard/create-app.png)

Es dauert einen Moment, bis eine externe öffentliche IP-Adresse dem Kubernetes-Dienst zugewiesen wird. Wählen Sie auf der linken Seite unter **Discovery and Load Balancing** (Ermittlung und den Lastenausgleich) die Option **Dienste** aus. Der Dienst Ihrer Anwendung wird aufgeführt, einschließlich der *externen Endpunkte*, wie im folgenden Beispiel gezeigt:

![Anzeigen der Liste der Dienste und Endpunkte](./media/kubernetes-dashboard/view-services.png)

Wählen Sie die Endpunktadresse aus, um ein Webbrowserfenster auf der NGINX-Standardseite zu öffnen:

![Anzeigen der NGINX-Standardseite der bereitgestellten Anwendung](./media/kubernetes-dashboard/default-nginx.png)

## <a name="view-pod-information"></a>Anzeigen von Podinformationen

Das Kubernetes-Dashboard kann grundlegende Überwachungsmetriken und Informationen zur Problembehandlung wie z.B. Protokolle bereitstellen.

Um weitere Informationen zu Ihren Anwendungspods anzuzeigen, wählen Sie im linken Menü **Pods** aus. Die Liste der verfügbaren Pods wird angezeigt. Wählen Sie Ihren *nginx*-Pod aus, um die Informationen anzuzeigen, etwa den Ressourcenverbrauch:

![Anzeigen von Podinformationen](./media/kubernetes-dashboard/view-pod-info.png)

## <a name="edit-the-application"></a>Bearbeiten der Anwendung

Über das Kubernetes-Dashboard können Sie nicht nur Anwendungen erstellen und anzeigen, sondern auch Anwendungsbereitstellungen bearbeiten und aktualisieren. Um zusätzliche Redundanz für die Anwendung bereitzustellen, erhöhen Sie die Anzahl der NGINX-Replikate.

So bearbeiten Sie eine Bereitstellung

1. Wählen Sie im linken Menü **Bereitstellungen** und dann Ihre *nginx*-Bereitstellung aus.
1. Wählen Sie auf der Navigationsleiste rechts oben **Bearbeiten** aus.
1. Suchen Sie den Wert `spec.replica` in der Nähe von Zeile 20. Um die Anzahl der Replikate für die Anwendung zu erhöhen, ändern Sie diesen Wert von *1* in *3*.
1. Klicken Sie abschließend auf **Aktualisieren**.

![Bearbeiten der Bereitstellung zum Aktualisieren der Anzahl von Replikaten](./media/kubernetes-dashboard/edit-deployment.png)

Es dauert einige Zeit, bis die neuen Pods in einer Replikatgruppe erstellt werden. Wählen Sie im linken Menü **Replikatsätze** und dann Ihre *nginx*-Replikatgruppe aus. Die Liste der Pods entspricht nun der aktualisierten Replikatanzahl, wie in der folgenden Beispielausgabe gezeigt:

![Anzeigen von Informationen zur Replikatgruppe](./media/kubernetes-dashboard/view-replica-set.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Kubernetes-Dashboard finden Sie unter [Kubernetes-Dashboard mit Webbenutzeroberfläche][kubernetes-dashboard].

<!-- LINKS - external -->
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubeconfig-file]: https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

<!-- LINKS - internal -->
[aad-cluster]: ./azure-ad-integration-cli.md
[aks-quickstart]: ./kubernetes-walkthrough.md
[aks-service-accounts]: ./concepts-identity.md#kubernetes-service-accounts
[az-account-get-access-token]: /cli/azure/account?view=azure-cli-latest#az-account-get-access-token
[az-aks-browse]: /cli/azure/aks#az-aks-browse
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[install-azure-cli]: /cli/azure/install-azure-cli
