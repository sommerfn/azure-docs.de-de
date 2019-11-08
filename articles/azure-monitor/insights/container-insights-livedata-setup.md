---
title: Einrichten von Livedaten (Vorschau) mit Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Echtzeitansicht von Containerprotokollen (stdout/stderr) und Ereignissen ohne Verwendung von kubectl mit Azure Monitor für Container einrichten.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2019
ms.author: magoedte
ms.openlocfilehash: 596c5ad378d471c6c98616a48f44e96c365ee0bb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510755"
---
# <a name="how-to-setup-the-live-data-preview-feature"></a>Einrichten der Funktion für Livedaten (Vorschau)

Zum Anzeigen von Livedaten (Vorschau) von AKS-Clustern (Azure Kubernetes Service) mit Azure Monitor für Container müssen Sie die Authentifizierung mit Berechtigung für den Zugriff auf Ihre Kubernetes-Daten konfigurieren. Diese Sicherheitskonfiguration ermöglicht den Echtzeitzugriff auf Ihre Daten über die Kubernetes-API direkt im Azure-Portal.

Diese Funktion unterstützt drei verschiedene Methoden, um den Zugriff auf die Protokolle, Ereignisse und Metriken zu steuern:

- AKS ohne aktivierte Kubernetes RBAC-Autorisierung
- Mit Kubernetes RBAC-Autorisierung aktivierter AKS
- Mit auf SAML basiertem SSO in Azure Active Directory (AD) aktivierter AKS

Für diese Anweisungen ist sowohl Administratorzugriff auf den Kubernetes-Cluster als auch Administratorzugriff auf Azure AD (wenn Azure Active Directory (AD) für die Benutzerauthentifizierung konfiguriert wird) erforderlich.  

In diesem Artikel wird erläutert, wie Sie die Authentifizierung konfigurieren, um den Zugriff auf die Funktion für Livedaten (Vorschau) vom Cluster zu steuern:

- AKS-Cluster mit aktivierter rollenbasierter Zugriffssteuerung (Role-Based Access Control, RBAC)
- AKS-Cluster mit integriertem Azure Active Directory 

>[!NOTE]
>Als [private Cluster](https://azure.microsoft.com/updates/aks-private-cluster/) aktivierte AKS-Cluster werden mit dieser Funktion nicht unterstützt. Diese Funktion basiert auf dem direkten Zugriff auf die Kubernetes-API über einen Proxyserver im Browser. Durch die Aktivierung der Netzwerksicherheit zum Blockieren der Kubernetes-API über den Proxy wird der Datenverkehr blockiert. 

>[!NOTE]
>Diese Funktion steht in allen Azure-Regionen zur Verfügung, einschließlich Azure China. In Azure US Government ist sie derzeit nicht verfügbar.

## <a name="authentication-model"></a>Authentifizierungsmodell

Die Funktion für Livedaten (Vorschau) verwendet die Kubernetes-API, die dem Befehlszeilentool `kubectl` entspricht. Die Kubernetes-API-Endpunkte verwenden ein selbstsigniertes Zertifikat, das von Ihrem Browser nicht überprüft werden kann. Diese Funktion verwendet einen internen Proxy, um das Zertifikat mit dem AKS-Dienst zu überprüfen und einen vertrauenswürdigen Datenverkehr sicherzustellen.

Im Azure-Portal werden Sie aufgefordert, Ihre Anmeldeinformationen für einen Azure Active Directory-Cluster zu überprüfen, und während der Clustererstellung (und in diesem Artikel auch bei der Neukonfiguration) zur Einrichtung der Clientregistrierung umgeleitet. Dieses Verhalten ähnelt dem für `kubectl` erforderlichen Authentifizierungsprozess. 

>[!NOTE]
>Die Autorisierung für Ihren Cluster wird von Kubernetes und dem Sicherheitsmodell verwaltet, mit dem es konfiguriert ist. Benutzer, die auf diese Funktion zugreifen, benötigen die Berechtigung zum Herunterladen der Kubernetes-Konfiguration (*kubeconfig*), ähnlich der Ausführung von `az aks get-credentials -n {your cluster name} -g {your resource group}`. Diese Konfigurationsdatei enthält das Autorisierungs- und Authentifizierungstoken für die **Benutzerrolle für Azure Kubernetes Service-Cluster** sowohl bei AKS-Clustern mit als auch ohne aktivierte RBAC-Autorisierung. Sie enthält Informationen über Azure AD sowie Details zur Clientregistrierung, wenn AKS mit auf SAML basiertem SSO in Azure Active Directory (AD) aktiviert ist.

>[!IMPORTANT]
>Benutzer dieser Funktion müssen für den Cluster über die [Benutzerrolle für Azure Kubernetes Service-Cluster](../../azure/role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-user-role permissions) verfügen, um `kubeconfig` herunterladen und diese Funktion verwenden zu können. Benutzer müssen **nicht** über den Zugriff für Mitwirkende auf den Cluster verfügen, um diese Funktion zu nutzen. 

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Kubernetes-Cluster ohne aktiviertes RBAC

Wenn Sie über einen Kubernetes-Cluster verfügen, der nicht mit der Kubernetes RBAC-Autorisierung konfiguriert oder mit einmaligem Anmelden von Azure AD integriert ist, brauchen Sie diese Schritte nicht auszuführen. Der Grund dafür ist, dass Sie in einer Nicht-RBAC-Konfiguration standardmäßig über Administratorberechtigungen verfügen.

## <a name="configure-kubernetes-rbac-authentication"></a>Konfigurieren der Kubernetes RBAC-Authentifizierung

Wenn Sie die Kubernetes RBAC-Autorisierung aktivieren, werden zwei Benutzer, **clusterUser** und **clusterAdmin**, für den Zugriff auf die Kubernetes-API verwendet. Dies ähnelt dem Ausführen von `az aks get-credentials -n {cluster_name} -g {rg_name}` ohne die Verwaltungsoption. Das bedeutet, dass **clusterUser** Zugriff auf die Endpunkte in der Kubernetes-API erteilt werden muss.

Die folgenden Beispielschritte zeigen, wie Sie über diese yaml-Konfigurationsvorlage die Clusterrollenbindung konfigurieren.

1. Kopieren Sie die yaml-Datei, fügen Sie sie anschließend ein, und speichern Sie sie als LogReaderRBAC.yaml.  

    ```
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRole 
    metadata: 
       name: containerHealth-log-reader 
    rules: 
        - apiGroups: ["", "metrics.k8s.io", "extensions", "apps"] 
          resources: 
             - "pods/log" 
             - "events" 
             - "nodes" 
             - "pods" 
             - "deployments" 
             - "replicasets" 
          verbs: ["get", "list"] 
    --- 
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRoleBinding 
    metadata: 
       name: containerHealth-read-logs-global 
    roleRef: 
       kind: ClusterRole 
       name: containerHealth-log-reader 
       apiGroup: rbac.authorization.k8s.io 
    subjects: 
    - kind: User 
      name: clusterUser 
      apiGroup: rbac.authorization.k8s.io 
    ```

2. Führen Sie zum Aktualisieren Ihrer Konfiguration den Befehl `kubectl apply -f LogReaderRBAC.yaml` aus.

>[!NOTE] 
> Wenn Sie eine frühere Version der Datei `LogReaderRBAC.yaml` auf Ihren Cluster angewendet haben, aktualisieren Sie sie. Dazu kopieren Sie den in Schritt 1 oben gezeigten neuen Code, fügen ihn ein, und führen dann den in Schritt 2 gezeigten Befehl aus, um den Code auf den Cluster anzuwenden.

## <a name="configure-ad-integrated-authentication"></a>Konfigurieren der integrierten AD-Authentifizierung 

Ein AKS-Cluster, der zur Verwendung von Azure Active Directory (AD) für die Benutzerauthentifizierung konfiguriert ist, verwendet die Anmeldeinformationen der Person, die auf diese Funktion zugreift. Bei dieser Konfiguration können Sie sich mit Ihrem Azure AD-Authentifizierungstoken bei einem AKS-Cluster anmelden.

Die Azure AD-Clientregistrierung muss neu konfiguriert werden, damit das Azure-Portal die Autorisierungsseiten als vertrauenswürdige Umleitungs-URL umleiten kann. Benutzern aus Azure AD wird dann der direkte Zugriff auf dieselben Kubernetes-API-Endpunkte über **ClusterRoles** und **ClusterRoleBindings** gewährt. 

Weitere Informationen zur Einrichtung für erweiterte Sicherheit in Kubernetes finden Sie in der [Kubernetes-Dokumentation](https://kubernetes.io/docs/reference/access-authn-authz/rbac/). 

>[!NOTE]
>Wenn Sie einen neuen RBAC-fähigen Cluster erstellen, lesen Sie [Integrieren von Azure Active Directory in Azure Kubernetes Service](../../aks/azure-ad-integration.md), und folgen Sie den Schritten zum Konfigurieren der Azure AD-Authentifizierung. Während der Schritte zum Erstellen der Clientanwendung werden in einem Hinweis in diesem Abschnitt zwei Umleitungs-URLs hervorgehoben, die Sie für Azure Monitor für Container erstellen müssen. 

### <a name="client-registration-reconfiguration"></a>Neukonfiguration der Clientregistrierung

1. Suchen Sie die Clientregistrierung für Ihren Kubernetes-Cluster in Azure AD unter **Azure Active Directory > App-Registrierungen** im Azure-Portal.

2. Wählen Sie im linken Bereich die Option **Authentifizierung** aus. 

3. Fügen Sie dieser Liste zwei Umleitungs-URLs als **Web**-Anwendungstypen hinzu. Der erste Basis-URL-Wert sollte `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` lauten und der zweite Basis-URL-Wert `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`.

    >[!NOTE]
    >Wenn Sie diese Funktion in Azure China verwenden, sollte der erste Basis-URL-Wert `https://afd.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` und der zweite Basis-URL-Wert `https://monitoring.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` lauten. 
    
4. Nachdem Sie die Umleitungs-URLs registriert haben, wählen Sie unter **Erweiterte Einstellungen** die Optionen **Zugriffstoken** und **ID-Token** aus, und speichern Sie die Änderungen.

>[!NOTE]
>Die Konfiguration der Authentifizierung mit Azure Active Directory für einmaliges Anmelden kann nur während der anfänglichen Bereitstellung eines neuen AKS-Clusters durchgeführt werden. Sie können einmaliges Anmelden nicht für einen bereits bereitgestellten AKS-Cluster konfigurieren.
  
>[!IMPORTANT]
>Wenn Sie Azure AD mit dem aktualisierten URI erneut für die Benutzerauthentifizierung konfiguriert haben, löschen Sie den Cache Ihres Browsers, um sicherzustellen, dass das aktualisierte Authentifizierungstoken heruntergeladen und angewendet wird.

## <a name="grant-permission"></a>Erteilen der Berechtigung

Jedem Azure AD-Konto muss die Berechtigung für die entsprechenden APIs in Kubernetes erteilt werden, um auf die Funktion für Livedaten (Vorschau) zugreifen zu können. Die entsprechenden Schritte für das Azure Active Directory-Konto ähneln den Schritten, die im Abschnitt [Kubernetes RBAC-Authentifizierung](#configure-kubernetes-rbac-authentication) beschrieben sind. Bevor Sie die YAML-Konfigurationsvorlage auf Ihren Cluster anwenden, ersetzen Sie **clusterUser** unter **ClusterRoleBinding** durch den gewünschten Benutzer. 

>[!IMPORTANT]
>Wenn sich der Benutzer, für den die RBAC-Bindung gewährt wird, im selben Azure AD-Mandanten befindet, weisen Sie Berechtigungen entsprechend „userPrincipalName“ zu. Befindet sich der Benutzer in einem anderen Azure AD-Mandanten, müssen Sie die Eigenschaft „objectId“ abfragen und verwenden.

Weitere Unterstützung beim Konfigurieren von **ClusterRoleBinding** für Ihren AKS-Cluster finden Sie unter [Erstellen einer RBAC-Bindung](../../aks/azure-ad-integration-cli.md#create-rbac-binding).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Authentifizierung eingerichtet haben, können Sie [Metriken](container-insights-livedata-metrics.md), [Bereitstellungen](container-insights-livedata-deployments.md) sowie [Ereignisse und Protokolle](container-insights-livedata-overview.md) von Ihrem Cluster in Echtzeit anzeigen.