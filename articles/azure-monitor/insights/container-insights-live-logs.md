---
title: Anzeigen von Azure Monitor für Containerprotokolle in Echtzeit | Microsoft-Dokumentation
description: In diesem Artikel wird die Echtzeitansicht von Containerprotokollen (stdout/stderr) und Ereignissen ohne Verwendung von kubectl mit Azure Monitor für Container beschrieben.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 07/12/2019
ms.openlocfilehash: 25cfe10ec192f874d050bca22ce1b85c2d1afbb4
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554087"
---
# <a name="how-to-view-logs-and-events-in-real-time-preview"></a>Anzeigen von Protokollen und Ereignissen in Echtzeit (Vorschauversion)
Azure Monitor für Container enthält ein Feature, das sich derzeit in der Vorschauversion befindet. Dieses Feature bietet eine Liveansicht Ihrer Azure Kubernetes Service-Containerprotokolle (stdout/stderr; AKS) und Ereignisse, ohne dass Sie kubectl-Befehle ausführen müssen. Wenn Sie eine Option auswählen, wird unter der Leistungsdatentabelle in der Ansicht für **Knoten**, **Controller** und **Container** ein neuer Bereich angezeigt. Dort werden von der Container-Engine generierte Liveprotokolle und -ereignisse angezeigt, die weitere Unterstützung bei der Behandlung von Problemen in Echtzeit bieten.

>[!NOTE]
>Diese Funktion steht in allen Azure-Regionen zur Verfügung, einschließlich Azure China. In Azure US Government ist sie derzeit nicht verfügbar.

>[!NOTE]
>Damit diese Funktion verwendet werden kann, ist Zugriff auf die Clusterressource vom Typ **Benutzerrolle für Azure Kubernetes Service-Cluster** erforderlich. [Weitere Informationen zur Benutzerrolle für Azure Kubernetes Service-Cluster](https://docs.microsoft.com/en-us/azure/aks/control-kubeconfig-access#available-cluster-roles-permissions).

Liveprotokolle unterstützen drei Methoden zum Steuern des Zugriffs auf die Protokolle:

1. AKS ohne aktivierte Kubernetes RBAC-Autorisierung
2. Mit Kubernetes RBAC-Autorisierung aktivierter AKS
3. Mit auf SAML basiertem SSO in Azure Active Directory (AD) aktivierter AKS

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Kubernetes-Cluster ohne aktiviertes RBAC
 
Wenn Sie über einen Kubernetes-Cluster verfügen, der nicht mit der Kubernetes RBAC-Autorisierung konfiguriert oder mit einmaligem Anmelden von Azure AD integriert ist, brauchen Sie diese Schritte nicht auszuführen. Da die Kubernetes-Autorisierung die Kube-API verwendet, sind schreibgeschützte Berechtigungen erforderlich.

## <a name="kubernetes-rbac-authorization"></a>Kubernetes RBAC-Autorisierung
Wenn die Kubernetes RBAC-Autorisierung aktiviert ist, müssen Sie die Clusterrollenbindung anwenden. Die folgenden Beispielschritte zeigen, wie Sie über diese yaml-Konfigurationsvorlage die Clusterrollenbindung konfigurieren. 

1. Kopieren Sie die yaml-Datei, fügen Sie sie anschließend ein, und speichern Sie sie als LogReaderRBAC.yaml.  

    ```
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRole 
    metadata: 
       name: containerHealth-log-reader 
    rules: 
       - apiGroups: [""] 
         resources: ["pods/log", "events"] 
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

2. Wenn Sie sie zum ersten Mal konfigurieren, wenden Sie die Clusterrollenbindung an, indem Sie den folgenden Befehl ausführen: `kubectl create -f LogReaderRBAC.yaml`. Führen Sie zum Aktualisieren Ihrer Konfiguration den folgenden Befehl aus, wenn Sie noch vor der Einführung von Liveereignisprotokollen die Unterstützung für die Vorschauversion von Liveprotokollen aktiviert haben: `kubectl apply -f LogReaderRBAC.yaml`.

## <a name="configure-aks-with-azure-active-directory"></a>Konfigurieren von AKS mit Azure Active Directory

Azure Kubernetes Service (AKS) kann für die Verwendung von Azure Active Directory (AD) für die Benutzerauthentifizierung konfiguriert werden. Informationen zur erstmaligen Konfiguration finden Sie unter [Integrieren von Azure Active Directory in Azure Kubernetes Service](../../aks/azure-ad-integration.md). Machen Sie beim Ausführen der Schritte zum Erstellen der [Clientanwendung](../../aks/azure-ad-integration.md#create-the-client-application) folgende Angaben:

-  **Umleitungs-URI**: Es müssen zwei **Web**anwendungstypen erstellt werden. Der erste Basis-URL-Wert sollte `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` lauten und der zweite Basis-URL-Wert `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`.
- Wählen Sie nach dem Registrieren der Anwendung auf der Seite **Übersicht** im linken Bereich **Authentifizierung** aus. Führen Sie auf der Seite **Authentifizierung** unter **Erweiterte Einstellungen** implizit **Zugriffstoken** und **ID-Token** auf, und speichern Sie dann Ihre Änderungen.

>[!NOTE]
>Wenn Sie diese Funktion in der Azure China-Region verwenden, sollte der erste Basis-URL-Wert `https://afd.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` und der zweite Basis-URL-Wert `https://monitoring.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` lauten.

>[!NOTE]
>Die Konfiguration der Authentifizierung mit Azure Active Directory für einmaliges Anmelden kann nur während der anfänglichen Bereitstellung eines neuen AKS-Clusters durchgeführt werden. Sie können einmaliges Anmelden nicht für einen bereits bereitgestellten AKS-Cluster konfigurieren.
  
>[!IMPORTANT]
>Wenn Sie Azure AD mit dem aktualisierten URI erneut für die Benutzerauthentifizierung konfiguriert haben, löschen Sie den Cache Ihres Browsers, um sicherzustellen, dass das aktualisierte Authentifizierungstoken heruntergeladen und angewendet wird.   

## <a name="view-live-logs-and-events"></a>Anzeigen von Liveprotokollen und- ereignissen

Sie können Echtzeitprotokollereignisse anzeigen, da diese von der Container-Engine der **Knoten-** , **Controller-** und **Containeransicht** generiert werden. Wählen Sie im Bereich „Eigenschaften“ die Option **Livedaten anzeigen (Vorschauversion)** aus. Nun wird unter der Leistungsdatentabelle ein Bereich angezeigt, in dem Sie Protokolle und Ereignisse in einem fortlaufenden Datenstrom anzeigen können.

![Option zum Anzeigen von Liveprotokollen im Bereich „Knoteneigenschaften“](./media/container-insights-live-logs/node-properties-live-logs-01.png)  

Protokoll- und Ereignisnachrichten sind je nach Auswahl des Ressourcentyps in der Ansicht beschränkt.

| Sicht | Ressourcentyp | Protokoll oder Ereignis | Angezeigte Daten |
|------|---------------|--------------|----------------|
| Nodes | Knoten | Ereignis | Wenn ein Knoten ausgewählt wird, werden Ereignisse nicht gefiltert und zeigen Kubernetes-Ereignisse für den gesamten Cluster an. Der Titel des Bereichs zeigt den Namen des Clusters an. |
| Nodes | Pod | Ereignis | Wenn ein Pod ausgewählt wird, werden Ereignisse in seinen Namespace gefiltert. Der Titel des Bereichs zeigt den Namespace des Pods an. | 
| Controllers | Pod | Ereignis | Wenn ein Pod ausgewählt wird, werden Ereignisse in seinen Namespace gefiltert. Der Titel des Bereichs zeigt den Namespace des Pods an. |
| Controllers | Controller | Ereignis | Wenn ein Controller ausgewählt ist, werden Ereignisse in seinen Namespace gefiltert. Der Titel des Bereichs zeigt den Namespace des Controllers an. |
| Knoten/Controller/Container | Container | Protokolle | Der Titel des Bereichs zeigt den Namen des Pods an, mit dem der Container gruppiert ist. |

Wenn der AKS-Cluster mit SSO über AAD konfiguriert ist, werden Sie bei der ersten Verwendung während dieser Browsersitzung zur Authentifizierung aufgefordert. Wählen Sie Ihr Konto aus und führen Sie die Authentifizierung mit Azure durch.  

Nach erfolgreicher Authentifizierung wird der Liveprotokollbereich im unteren Abschnitt des mittleren Bereichs angezeigt. Wenn die Abrufstatusanzeige ein grünes Häkchen anzeigt, das sich ganz rechts im Bereich befindet, bedeutet dies, dass Daten abgerufen werden können.
    
  ![Abgerufene Daten des Liveprotokollbereichs](./media/container-insights-live-logs/live-logs-pane-01.png)  

In der Suchleiste können Sie nach Schlüsselwort filtern, um den Text im Protokoll oder Ereignis hervorzuheben. In der Suchleiste ganz rechts wird dann angezeigt, wie viele Ergebnisse dem Filter entsprechen.

  ![Beispiel für den Filter des Liveprotokollbereichs](./media/container-insights-live-logs/live-logs-pane-filter-example-01.png)

Beim Anzeigen von Ereignissen können Sie außerdem mit dem **Filter** rechts neben der Suchleiste die Ergebnisse einschränken. Je nachdem, welche Ressource Sie ausgewählt haben, stehen im Filterfeld Pods, Namespaces oder Cluster zur Auswahl.  

Klicken Sie auf die Option **Scrollen**, um den automatischen Bildlauf zu unterbrechen und das Verhalten des Bereichs zu steuern sowie manuell durch die gelesenen neuen Daten zu scrollen. Klicken Sie einfach erneut auf die Option **Scrollen**, um den automatischen Bildlauf wieder zu aktivieren. Sie können den Abruf von Protokolldaten oder Ereignissen auch anhalten, indem Sie auf die Option **Anhalten** klicken. Wenn Sie bereit sind, den Vorgang fortzusetzen, klicken Sie einfach auf **Wiedergeben**.  

![Anhalten der Liveansicht im Liveprotokollbereich](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

Sie können zu Azure Monitor-Protokollen wechseln, um historische Containerprotokolle anzuzeigen. Wählen Sie hierfür **Containerprotokolle anzeigen** in der Dropdownliste **In Analytics anzeigen** aus.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Verwendung von Azure Monitor und zum Überwachen anderer Aspekte Ihres AKS-Clusters finden Sie unter [Anzeigen der Azure Kubernetes Service-Integrität](container-insights-analyze.md).

- Sehen Sie sich die [Beispiele zu Protokollabfragen](container-insights-log-search.md#search-logs-to-analyze-data) an, die auch vordefinierte Abfragen enthalten. Mit diesen Materialien können Sie Auswertungen von bzw. Anpassungen für Warnungen, Visualisierungen und Analysen von Clustern vornehmen.
