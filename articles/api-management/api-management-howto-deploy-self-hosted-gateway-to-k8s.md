---
title: Bereitstellen eines selbstgehosteten Azure API Management-Gateways für Kubernetes | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein selbstgehostetes Azure API Management-Gateway für Kubernetes bereitstellen.
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: 109316acb73d3c5f00186298c1f8840c516e5790
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510591"
---
# <a name="deploy-a-self-hosted-azure-api-management-gateway-to-kubernetes"></a>Bereitstellen eines selbstgehosteten Azure API Management-Gateways für Kubernetes

Dieser Artikel beschreibt die Schritte für die Bereitstellung eines selbstgehosteten Azure API Management-Gateways in einer Kubernetes-Umgebung.

> [!NOTE]
> Das Feature „selbstgehostetes Gateway“ ist als Vorschauversion verfügbar. Während der Vorschauphase ist das selbstgehostete Gateway nur in den Tarifen „Developer“ und „Premium“ ohne zusätzliche Kosten verfügbar. Der Tarif „Developer“ ist auf eine Bereitstellung mit einem einzelnen selbstgehosteten Gateway beschränkt.


## <a name="prerequisites"></a>Voraussetzungen

- Absolvieren Sie die folgende Schnellstartanleitung: [Erstellen einer neuen Azure API Management-Dienstinstanz](get-started-create-service-instance.md)
- Erstellen Sie einen Kubernetes-Cluster. [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/) ist eine gute Option für Entwicklungs- und Evaluierungszwecke. Für Produktionsworkloads können Sie [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/) oder einen Kubernetes-Cluster in einer fremden Cloud verwenden.
- [Stellen Sie eine Gatewayressource in Ihrer API Management-Instanz bereit](api-management-howto-provision-self-hosted-gateway.md).

## <a name="deploy-the-gateway-to-kubernetes"></a>Bereitstellen des Gateways für Kubernetes

1. Wählen Sie **Gateways** unter **Einstellungen** aus.
2. Wählen Sie die Gatewayressource aus, die Sie bereitstellen möchten.
3. Wählen Sie **Bereitstellung** aus.
4. Beachten Sie, dass ein neues Token im Textfeld **Token** automatisch für Sie mit den Standardwerten **Ablauf** und **Geheimer Schlüssel** generiert wurde. Passen Sie einen Wert oder beide Werte an, und wählen Sie **Generieren** aus, um ein neues Token zu erstellen.
5. Stellen Sie sicher, dass **Kubernetes** unter **Bereitstellungsskripts** ausgewählt ist.
6. Wählen Sie den **<Gatewayname>.yml**-Dateilink neben **Bereitstellung** aus, um die Datei herunterzuladen.
7. Passen Sie die Portzuordnungen und den Containernamen in der YML-Datei nach Bedarf an.
8. Wählen Sie das Symbol **Kopieren** rechts neben dem Textfeld **Bereitstellen** aus, um den `kubectl`-Befehl in der Zwischenablage zu speichern. 
9. Fügen Sie den Befehl in das Terminalfenster (oder Befehlsfenster) ein. Beachten Sie, dass der Befehl erwartet, dass die heruntergeladene Umgebungsdatei im aktuellen Verzeichnis vorhanden ist.
```console
    kubectl apply -f <gateway-name>.yaml
```
10. Führen Sie den Befehl aus. Der Befehl weist Ihren Kubernetes-Cluster an, den Container mithilfe des aus der Microsoft Container Registry heruntergeladenen Images des selbstgehosteten Gateways auszuführen, und den Container zu konfigurieren, um die Ports HTTP (8080) und HTTPS (443) verfügbar zu machen.
11. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob der Gatewaypod ausgeführt wird. Beachten Sie, dass Ihr Podname anders ist. 
```console
kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
local-gateway-55f774f844-bv9wt    1/1     Running   0          1m
```
12. Führen Sie den folgenden Befehl aus, um zu überprüfen, ob der Gatewaydienst ausgeführt wird. Beachten Sie, dass Ihr Dienstname anders ist. 
```console
kubectl get services
NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
localgateway     NodePort    10.110.230.87   <none>        80:32504/TCP,443:30043/TCP   1m
```
13. Navigieren Sie zurück zum Azure-Portal, und vergewissern Sie sich, dass der soeben bereitgestellte Gatewayknoten einen fehlerfreien Status meldet.

![Gatewaystatus](media/api-management-howto-deploy-self-hosted-gateway-to-k8s/status.png)

> [!TIP]
> Verwenden Sie den Befehl <code>kubectl logs <gateway-pod-name></code>, um eine Momentaufnahme des Protokolls des selbstgehosteten Gateways anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum selbstgehosteten Gateway finden Sie unter [Azure API Management: Übersicht über das selbstgehostete Gateway](self-hosted-gateway-overview.md).
* Erfahren Sie mehr über [Azure Kubernetes Service](https://docs.microsoft.com/azure/aks/intro-kubernetes).


