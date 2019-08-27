---
title: Integrieren von Azure Active Directory in Azure Kubernetes Service
description: Erstellen von Azure Active Directory-fähigen AKS-Clustern (Azure Kubernetes Service)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 04/26/2019
ms.author: mlearned
ms.openlocfilehash: 5dabbb6458d0d0d4af51490bea0c3f38a7c5c41d
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69542891"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service"></a>Integrieren von Azure Active Directory in Azure Kubernetes Service

Azure Kubernetes Service (AKS) kann für die Verwendung von Azure Active Directory (Azure AD) für die Benutzerauthentifizierung konfiguriert werden. Bei dieser Konfiguration können Sie sich mit Ihrem Azure AD-Authentifizierungstoken bei einem AKS-Cluster anmelden.

Clusteradministratoren können die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Kubernetes auf der Grundlage einer Benutzeridentität oder Verzeichnisgruppenmitgliedschaft konfigurieren.

In diesem Artikel werden die folgenden Aufgaben erläutert:

- Bereitstellen der Voraussetzungen für AKS und Azure AD
- Bereitstellen eines Azure AD-fähigen Clusters
- Erstellen einer einfachen RBAC-Rolle im AKS-Cluster über das Azure-Portal

Sie können diese Schritte auch über die [Azure-Befehlszeilenschnittstelle][azure-ad-cli] ausführen.

> [!NOTE]
> Azure AD kann nur aktiviert werden, wenn Sie einen neuen RBAC-fähigen Cluster erstellen. Es ist nicht möglich, Azure AD für einen vorhandenen AKS-Cluster zu aktivieren.

## <a name="authentication-details"></a>Authentifizierungsdetails

Die Azure AD-Authentifizierung wird für AKS-Cluster bereitgestellt, die über OpenID Connect verfügen. OpenID Connect ist eine Identitätsebene, die auf dem OAuth 2.0-Protokoll aufbaut.

Weitere Informationen zu OpenID Connect finden Sie unter [Autorisieren des Zugriffs auf Webanwendungen mit OpenID Connect und Azure Active Directory][open-id-connect].

Innerhalb eines Kubernetes-Clusters wird für Authentifizierungstoken die Webhooktokenauthentifizierung verwendet. Die Webhooktokenauthentifizierung wird als Teil des AKS-Clusters konfiguriert und verwaltet.

Weitere Informationen zur Webhooktokenauthentifizierung finden Sie in der Kubernetes-Dokumentation im Abschnitt zur [Webhookauthentifizierung][kubernetes-webhook].

Zur Bereitstellung der Azure AD-Authentifizierung für einen AKS-Cluster werden zwei Azure AD-Anwendungen erstellt. Die erste Anwendung ist eine Serverkomponente, die Benutzerauthentifizierung bereitstellt. Die zweite Anwendung ist eine Clientkomponente, die verwendet wird, wenn Sie von der CLI zur Authentifizierung aufgefordert werden. Diese Clientanwendung verwendet die Serveranwendung für die eigentliche Authentifizierung der Anmeldeinformationen, die vom Client bereitgestellt werden.

> [!NOTE]
> Bei der Konfiguration von Azure AD für die AKS-Authentifizierung werden zwei Azure AD-Anwendungen konfiguriert. Die Schritte zum Delegieren von Berechtigungen für jede Anwendung müssen von einem Azure-Mandantenadministrator ausgeführt werden.

## <a name="create-the-server-application"></a>Erstellen der Serveranwendung

Die erste Azure AD-Anwendung wird verwendet, um eine Azure AD-Gruppenmitgliedschaft eines Benutzers zu erhalten. So erstellen Sie diese Anwendung im Azure-Portal:

1. Wählen Sie **Azure Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.

    a. Geben Sie der Anwendung einen Namen, z.B. *AKSAzureADServer*.

    b. Wählen Sie für **Unterstützte Kontotypen** die Option **Nur Konten in diesem Organisationsverzeichnis** aus.
    
    c. Wählen Sie als Typ für die Umleitungs-URI die Option **Web** aus, und geben Sie dann einen beliebigen URI-formatierten Wert ein, z.B. *https://aksazureadserver* .

    d. Wählen Sie anschließend **Registrieren** aus.

2. Wählen Sie **Manifest** aus, und ändern Sie dann den Wert von **GroupMembershipClaims:** in **All**. Wenn Sie mit den Änderungen fertig sind, wählen Sie **Speichern** aus.

    ![Aktualisieren der Gruppenmitgliedschaft auf „All“](media/aad-integration/edit-manifest.png)

3. Wählen Sie im linken Bereich der Azure AD-Anwendung die Option **Zertifikate und Geheimnisse** aus.

    a. Wählen Sie **+ Neuer geheimer Clientschlüssel** aus.

    b. Fügen Sie eine Schlüsselbeschreibung hinzu, z.B. *AKS Azure AD-Server*. Wählen Sie eine Ablaufzeit und dann **Hinzufügen** aus.

    c. Notieren Sie sich den Schlüsselwert, der nur dieses eine Mal angezeigt wird. Beim Bereitstellen eines Azure AD-fähigen AKS-Clusters wird dieser Wert als Serveranwendungsgeheimnis bezeichnet.

4. Wählen Sie im linken Bereich der Azure AD-Anwendung die Option **API-Berechtigungen** und dann **+ Berechtigung hinzufügen** aus.

    a. Wählen Sie unter **Microsoft-APIs** die Option **Microsoft Graph** aus.

    b. Wählen Sie **Delegierte Berechtigungen** aus, und aktivieren Sie dann das Kontrollkästchen neben **Verzeichnis > Directory.Read.All (Verzeichnisdaten lesen)** .

    c. Wenn keine standardmäßige delegierte Berechtigung für **Benutzer > User.Read (Anmelden und Benutzerprofil lesen)** vorhanden ist, aktivieren Sie das Kontrollkästchen daneben.

    d. Wählen Sie **Anwendungsberechtigungen** aus, und aktivieren Sie dann das Kontrollkästchen neben **Verzeichnis > Directory.Read.All (Verzeichnisdaten lesen)** .

    ![Festlegen von Graph-Berechtigungen](media/aad-integration/graph-permissions.png)

    e. Wählen Sie **Berechtigungen hinzufügen** aus, um die Änderungen zu speichern.

    f. Wählen Sie unter **Einwilligung erteilen** die Option **Administratoreinwilligung erteilen** aus. Diese Schaltfläche ist nur verfügbar, wenn das aktuelle Konto ein Mandantenadministrator ist.

    Wenn Berechtigungen erfolgreich gewährt wurden, wird die folgende Benachrichtigung im Portal angezeigt:

   ![Benachrichtigung über erfolgreich erteilte Berechtigungen](media/aad-integration/permissions-granted.png)

5. Wählen Sie im linken Bereich der Azure AD-Anwendung die Option **Eine API verfügbar machen**  und dann **+ Bereich hinzufügen** aus.
    
    a. Geben Sie einen Wert für **Bereichsname**, **Anzeigename der Administratoreinwilligung** und **Beschreibung der Administratoreinwilligung** (z.B. *AKSAzureADServer*) ein.

    b. Stellen Sie sicher, dass **Zustand** auf **Aktiviert** gesetzt ist.

    ![Verfügbarmachen der Server-App als eine API für die Verwendung mit anderen Diensten](media/aad-integration/expose-api.png)

    c. Wählen Sie **Bereich hinzufügen** aus.

6. Kehren Sie zur Seite **Übersicht** der Anwendung zurück, und notieren Sie sich die **Anwendungs-ID (Client)** . Beim Bereitstellen eines Azure AD-fähigen AKS-Clusters wird dieser Wert als Serveranwendungs-ID bezeichnet.

    ![Abrufen der Anwendungs-ID](media/aad-integration/application-id.png)

## <a name="create-the-client-application"></a>Erstellen der Clientanwendung

Die zweite Azure AD-Anwendung wird beim Anmelden mit der Kubernetes-CLI (kubectl) verwendet.

1. Wählen Sie **Azure Active Directory** > **App-Registrierungen** > **Neue Registrierung** aus.

    a. Geben Sie der Anwendung einen Namen, z.B. *AKSAzureADClient*.

    b. Wählen Sie für **Unterstützte Kontotypen** die Option **Nur Konten in diesem Organisationsverzeichnis** aus.

    c. Wählen Sie als Typ für die Umleitungs-URI die Option **Web** aus, und geben Sie dann einen beliebigen URI-formatierten Wert ein, z.B. *https://aksazureadclient* .

    d. Wählen Sie anschließend **Registrieren** aus.

2. Wählen Sie im linken Bereich der Azure AD-Anwendung die Option **API-Berechtigungen** und dann **+ Berechtigung hinzufügen** aus.

    a. Wählen Sie **Meine APIs** und dann die Azure AD-Serveranwendung aus, die Sie im vorherigen Schritt erstellt haben, z.B. *AKSAzureADServer*.

    b. Wählen Sie **Delegierte Berechtigungen** aus, und aktivieren Sie dann das Kontrollkästchen neben Ihrer Azure AD-Serveranwendung.

    ![Konfigurieren der Anwendungsberechtigungen](media/aad-integration/select-api.png)

    c. Wählen Sie **Berechtigungen hinzufügen** aus.

    d. Wählen Sie unter **Einwilligung erteilen** die Option **Administratoreinwilligung erteilen** aus. Diese Schaltfläche ist nur verfügbar, wenn das aktuelle Konto ein Mandantenadministrator ist. Wenn Berechtigungen gewährt wurden, wird die folgende Benachrichtigung im Portal angezeigt:

    ![Benachrichtigung über erfolgreich erteilte Berechtigungen](media/aad-integration/permissions-granted.png)

3. Wählen Sie im linken Bereich der Azure AD-Anwendung die Option **Authentifizierung** aus.

    - Wählen Sie unter **Standardclienttyp** die Option **Ja** aus, um **den Client als öffentlichen Client einzustufen**.

5. Notieren Sie sich die Anwendungs-ID im linken Bereich der Azure AD-Anwendung. Beim Bereitstellen eines Azure AD-fähigen AKS-Clusters wird dieser Wert als Clientanwendungs-ID bezeichnet.

   ![Abrufen der Anwendungs-ID](media/aad-integration/application-id-client.png)

## <a name="get-the-tenant-id"></a>Abrufen der Mandanten-ID

Als Nächstes rufen Sie die ID von Ihrem Azure-Mandanten ab. Dieser Wert wird beim Erstellen des AKS-Clusters verwendet.

Wählen Sie im Azure-Portal **Azure Active Directory** > **Eigenschaften** aus, und notieren Sie sich die **Verzeichnis-ID**. Beim Erstellen eines Azure AD-fähigen AKS-Clusters wird dieser Wert als Mandanten-ID bezeichnet.

![Abrufen der Azure-Mandanten-ID](media/aad-integration/tenant-id.png)

## <a name="deploy-the-aks-cluster"></a>Bereitstellen des AKS-Clusters

Verwenden Sie zum Erstellen einer Ressourcengruppe für den AKS-Cluster den Befehl [az group create][az-group-create].

```azurecli
az group create --name myResourceGroup --location eastus
```

Verwenden Sie den Befehl [az aks create][az-aks-create], um den AKS-Cluster bereitzustellen. Als Nächstes ersetzen Sie die Werte im folgenden Beispielbefehl. Verwenden Sie die Werte, die beim Erstellen der Azure AD-Anwendungen für Server-App-ID und -Geheimnis, Client-App-ID und Mandanten-ID gesammelt wurden.

```azurecli
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --generate-ssh-keys \
  --aad-server-app-id b1536b67-29ab-4b63-b60f-9444d0c15df1 \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 8aaf8bd5-1bdd-4822-99ad-02bfaa63eea7 \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

Die Erstellung eines AKS-Clusters dauert einige Minuten.

## <a name="create-an-rbac-binding"></a>Erstellen einer RBAC-Bindung

> [!NOTE]
> Beim Namen der Clusterrollenbindung wird Groß-/Kleinschreibung beachtet.

Bevor Sie ein Azure Active Directory-Konto mit einem AKS-Cluster verwenden, müssen Sie eine Rollenbindung oder eine Clusterrollenbindung erstellen. Rollen definieren die zu erteilenden Berechtigungen, und Bindungen wenden diese auf die gewünschten Benutzer an. Diese Zuweisungen können auf einen bestimmten Namespace oder im gesamten Cluster angewendet werden. Weitere Informationen finden Sie unter [Verwenden von RBAC-Autorisierung][rbac-authorization].

Verwenden Sie zunächst den Befehl [az aks get-credentials][az-aks-get-credentials] mit dem Argument `--admin`, um sich mit Administratorzugriff bei dem Cluster anzumelden.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Als Nächstes erstellen Sie eine Clusterrollenbindung (ClusterRoleBinding) für ein Azure AD-Konto, dem Sie Zugriff auf den AKS-Cluster gewähren möchten. Im folgenden Beispiel wird dem Konto Vollzugriff auf alle Namespaces im Cluster gewährt:

- Wenn sich der Benutzer, für den die RBAC-Bindung gewährt wird, im selben Azure AD-Mandanten befindet, weisen Sie Berechtigungen auf Grundlage des Benutzerprinzipalnamens (User Principal Name, UPN) zu. Fahren Sie mit dem Schritt zum Erstellen des YAML-Manifests für die Clusterrollenbindung fort.

- Befindet sich der Benutzer in einem anderen Azure AD-Mandanten, müssen Sie stattdessen die **objectId**-Eigenschaft abfragen und verwenden. Rufen Sie ggf. mithilfe des Befehls [az ad user show][az-ad-user-show] die Objekt-ID (objectId) des erforderlichen Benutzerkontos ab. Geben Sie den Benutzerprinzipalname (UPN) des erforderlichen Benutzerkontos an:

    ```azurecli-interactive
    az ad user show --upn-or-object-id user@contoso.com --query objectId -o tsv
    ```

Erstellen Sie eine Datei, z.B. *rbac-aad-user.yaml*, und fügen Sie dann den folgenden Inhalt ein. Ersetzen Sie in der letzten Zeile **userPrincipalName_or_objectId** durch den UPN oder die Objekt-ID. Die Auswahl hängt davon ab, ob sich der Benutzer im gleichen Azure AD-Mandanten befindet oder nicht.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: userPrincipalName_or_objectId
```

Wenden Sie die Bindung wie im folgenden Beispiel gezeigt mit dem Befehl [kubectl apply][kubectl-apply] an:

```console
kubectl apply -f rbac-aad-user.yaml
```

Für alle Mitglieder einer Azure AD-Gruppe kann auch eine Rollenbindung erstellt werden. Azure AD-Gruppen werden wie im folgenden Beispiel gezeigt mithilfe der Gruppenobjekt-ID angegeben.

Erstellen Sie eine Datei, z.B. *rbac-aad-group.yaml*, und fügen Sie dann den folgenden Inhalt ein. Aktualisieren Sie den Benutzernamen mit einem Namen aus Ihrem Azure AD-Mandanten:

 ```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
   kind: Group
   name: "894656e1-39f8-4bfe-b16a-510f61af6f41"
```

Wenden Sie die Bindung wie im folgenden Beispiel gezeigt mit dem Befehl [kubectl apply][kubectl-apply] an:

```console
kubectl apply -f rbac-aad-group.yaml
```

Weitere Informationen zum Schützen eines Kubernetes-Clusters mit RBAC finden Sie in der [Dokumentation zur Verwendung der RBAC-Autorisierung][rbac-authorization].

## <a name="access-the-cluster-with-azure-ad"></a>Zugreifen auf den Cluster mit Azure AD

Pullen Sie den Kontext für den Benutzer ohne Administratorrechte mit dem Befehl [az aks get-credentials][az-aks-get-credentials].

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Nach Ausführen des Befehls `kubectl` werden Sie aufgefordert, sich mithilfe von Azure zu authentifizieren. Befolgen Sie die Anweisungen auf dem Bildschirm zum Abschließen des Vorgangs wie im folgenden Beispiel gezeigt:

```console
$ kubectl get nodes

To sign in, use a web browser to open https://microsoft.com/devicelogin. Next, enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-79590246-0   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-1   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-2   Ready     agent     1h        v1.13.5
```

Nach Abschluss des Vorgangs wird das Authentifizierungstoken zwischengespeichert. Sie werden nur dann aufgefordert, sich erneut anzumelden, wenn das Token abläuft oder die Kubernetes-Konfigurationsdatei neu erstellt wird.

Wenn nach erfolgreicher Anmeldung eine Fehlermeldung bezüglich der Autorisierung angezeigt wird, überprüfen Sie Folgendes:

```console
error: You must be logged in to the server (Unauthorized)
```


- Sie haben die entsprechende Objekt-ID oder den UPN definiert, je nachdem, ob das Benutzerkonto im gleichen Azure AD-Mandanten vorhanden ist oder nicht.
- Der Benutzer ist kein Mitglied von mehr als 200 Gruppen.
- Das Geheimnis, das in der Anwendungsregistrierung für den Server definiert ist, entspricht dem mit `--aad-server-app-secret` konfigurierten Wert.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Verwendung von Azure AD-Benutzern und -Gruppen für die Zugriffskontrolle auf Cluster-Ressourcen finden Sie unter [Zugriffskontrolle auf Cluster-Ressourcen mit rollenbasierter Zugriffskontrolle und Azure AD-Identitäten in AKS][azure-ad-rbac].

Weitere Informationen zum Schutz von Kubernetes-Clustern finden Sie unter [Zugriffs- und Identitätsoptionen für AKS][rbac-authorization].

Weitere Informationen zur Identitäts- und Ressourcensteuerung finden Sie unter [Best Practices für Authentifizierung und Autorisierung in AKS][operator-best-practices-identity].

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v1-protocols-openid-connect-code.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[azure-ad-cli]: azure-ad-integration-cli.md
