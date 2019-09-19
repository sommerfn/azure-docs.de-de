---
title: 'Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe einer Node-Web-App | Microsoft-Dokumentation'
description: In dieser Schnellstartanleitung legen Sie mithilfe einer Node-Web-App ein Geheimnis in Azure Key Vault fest und rufen es ab.
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.topic: quickstart
ms.date: 09/03/2010
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: 02b9c439a932a4b35700871e68bdad7f03451110
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2019
ms.locfileid: "71003501"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-by-using-a-node-web-app"></a>Schnellstart: Festlegen und Abrufen eines Geheimnisses aus Azure Key Vault mithilfe einer Node-Web-App 

In dieser Schnellstartanleitung erfahren Sie, wie Sie ein Geheimnis in Azure Key Vault speichern und es über eine Web-App abrufen. Die Verwendung von Key Vault trägt zum Schutz der Informationen bei. Um den Wert des Geheimnisses anzuzeigen, müssen Sie diese Schnellstartanleitung in Azure befolgen. In dieser Schnellstartanleitung werden Node.js und verwaltete Identitäten für Azure-Ressourcen verwendet. Folgendes wird vermittelt:

* Erstellen eines Schlüsseltresors
* Speichern eines Geheimnisses im Schlüsseltresor
* Abrufen eines Geheimnisses aus dem Schlüsseltresor
* Erstellen einer Azure-Webanwendung
* Aktivieren einer [verwalteten Identität](../active-directory/managed-service-identity/overview.md) für die Web-App
* Erteilen der erforderlichen Berechtigungen zum Lesen von Daten aus dem Schlüsseltresor für die Webanwendung

Stellen Sie vor dem Fortfahren sicher, dass Sie mit den [grundlegenden Konzepten von Key Vault](basic-concepts.md) vertraut sind.

> [!NOTE]
> Key Vault ist ein zentrales Repository zum programmgesteuerten Speichern von Geheimnissen. Dafür müssen sich Anwendungen und Benutzer jedoch zuerst bei Key Vault authentifizieren, d.h. sie müssen ein Geheimnis angeben. Aus Sicherheitsgründen muss das erste Geheimnis regelmäßig rotiert werden. 
>
> Dank der [verwalteten Dienstidentitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) erhalten in Azure ausgeführte Anwendungen eine Identität, die automatisch von Azure verwaltet wird. Das macht die *Einführung von Geheimnissen weniger problematisch*, da Benutzer und Anwendungen die empfohlene Vorgehensweise verwenden können und sich nicht um die Rotation des ersten Geheimnisses kümmern müssen.

## <a name="prerequisites"></a>Voraussetzungen

* [Node.js](https://nodejs.org/en/)
* [Git-Client](https://www.git-scm.com/)
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 oder höher. Für diese Schnellstartanleitung ist es erforderlich, dass Sie die Azure CLI lokal ausführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zur Installation und Aktualisierung der CLI finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0](https://review.docs.microsoft.com/en-us/cli/azure/install-azure-cli?branch=master&view=azure-cli-latest).
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Geben Sie Folgendes ein, um sich mithilfe der Azure-Befehlszeilenschnittstelle bei Azure anzumelden:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Wählen Sie einen Namen für die Ressourcengruppe aus, und fügen Sie den Platzhalter ein.
Im folgenden Beispiel wird eine Ressourcengruppe am Standort „USA, Osten“ erstellt.

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "East US"
```

Die eben erstellte Ressourcengruppe wird im gesamten Artikel verwendet.

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Als Nächstes erstellen Sie einen Schlüsseltresor, indem Sie die im vorherigen Schritt erstellte Ressourcengruppe verwenden. In diesem Artikel wird „ContosoKeyVault“ als Name verwendet, aber Sie müssen einen eindeutigen Namen nutzen. Geben Sie die folgenden Informationen ein:

* Name des Schlüsseltresors
* Ressourcengruppenname Der Name muss eine Zeichenfolge aus 3 bis 24 Zeichen sein und darf nur die Zeichen 0–9, a–z, A–Z und Bindestrich (-) enthalten.
* Standort: **USA, Osten**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "East US"
```

An diesem Punkt ist nur Ihr Azure-Konto zum Ausführen von Vorgängen für den neuen Tresor autorisiert.

## <a name="add-a-secret-to-the-key-vault"></a>Hinzufügen eines Geheimnisses zum Schlüsseltresor

Wir fügen ein Geheimnis hinzu, um die Vorgehensweise zu veranschaulichen. Sie können eine SQL-Verbindungszeichenfolge oder beliebige andere Informationen speichern, die sicher aufbewahrt, aber für Ihre Anwendung verfügbar gemacht werden müssen. In diesem Tutorial heißt das Kennwort **AppSecret**, und darin wird der Wert **MySecret** gespeichert.

Geben Sie die folgenden Befehle ein, um im Schlüsseltresor ein Geheimnis namens **AppSecret** zu erstellen. Dieses Geheimnis speichert den Wert **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Geben Sie Folgendes ein, um den Wert im Geheimnis als Nur-Text anzuzeigen:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Mit diesem Befehl werden die Geheimnisinformationen einschließlich des URI angezeigt. Nach der Ausführung dieser Schritte sollten Sie einen URI zu einem Geheimnis in einem Schlüsseltresor besitzen. Notieren Sie sich diese Informationen. Sie benötigen sie in einem späteren Schritt.

## <a name="clone-the-repo"></a>Klonen des Repositorys

Klonen Sie das Repository, um eine lokale Kopie zu erstellen, in der Sie die Quelle bearbeiten können. Führen Sie den folgenden Befehl aus:

```
git clone https://github.com/Azure-Samples/key-vault-node-quickstart.git
```

## <a name="install-dependencies"></a>Installieren von Abhängigkeiten

Führen Sie die folgenden Befehle zum Installieren von Abhängigkeiten aus:

```
cd key-vault-node-quickstart
npm install
```

In diesem Projekt werden zwei Node-Module verwendet: [ms-rest-azure](https://www.npmjs.com/package/ms-rest-azure) und [azure-keyvault](https://www.npmjs.com/package/azure-keyvault).

## <a name="publish-the-web-app-to-azure"></a>Veröffentlichen der Web-App in Azure

Erstellen Sie einen [Azure App Service](https://azure.microsoft.com/services/app-service/)-Plan. In diesem Plan können Sie mehrere Web-Apps speichern.

    ```
    az appservice plan create --name myAppServicePlan --resource-group myResourceGroup
    ```
Erstellen Sie als Nächstes eine Web-App. Ersetzen Sie im folgenden Beispiel `<app_name>` durch einen global eindeutigen App-Namen (gültige Zeichen sind „a - z“, „0 - 9“ und „-“). Die Runtime ist auf NODE|6.9 festgelegt. Führen Sie `az webapp list-runtimes` aus, um alle unterstützten Runtimes anzuzeigen.

    ```
    # Bash
    az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9" --deployment-local-git
    ```
Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

    ```
    {
      "availabilityState": "Normal",
      "clientAffinityEnabled": true,
      "clientCertEnabled": false,
      "cloningInfo": null,
      "containerSize": 0,
      "dailyMemoryTimeQuota": 0,
      "defaultHostName": "<app_name>.azurewebsites.net",
      "enabled": true,
      "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git"
      < JSON data removed for brevity. >
    }
    ```
Navigieren Sie zu Ihrer neu erstellten Web-App. Ihnen sollte nun eine funktionsfähige Web-App angezeigt werden. Ersetzen Sie `<app_name>` durch einen eindeutigen App-Namen.

    ```
    http://<app name>.azurewebsites.net
    ```
Mit dem obigen Befehl wird auch eine Git-fähige App erstellt, mit der Sie die Bereitstellung in Azure über Ihr lokales Git-Repository durchführen können. Das lokale Git-Repository ist mit dieser URL konfiguriert: `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`.

Nach Abschluss des vorherigen Befehls können Sie Ihrem lokalen Git-Repository eine Azure-Remoteinstanz hinzufügen. Ersetzen Sie `<url>` durch die URL des Git-Repositorys.

    ```
    git remote add azure <url>
    ```

## <a name="enable-a-managed-identity-for-the-web-app"></a>Aktivieren einer verwalteten Identität für die Web-App

Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen und anderen Schlüsseln und Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. Mit den Informationen unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md) kann dieses Problem leichter gelöst werden, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory (Azure AD) bereitgestellt wird. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung einschließlich von Key Vault unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

Führen Sie den Befehl „assign-identity“ aus, um die Identität für diese Anwendung zu erstellen:

```azurecli
az webapp identity assign --name <app_name> --resource-group "<YourResourceGroupName>"
```

Dieser Befehl entspricht dem Aufrufen des Portals und dem Festlegen der Einstellung **Identität/Vom System zugewiesen** auf **Ein** in den Webanwendungseigenschaften.

### <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Zuweisen von Berechtigungen zu Ihrer Anwendung für das Lesen von Geheimnissen aus Key Vault

Notieren Sie sich die Ausgabe des vorherigen Befehls. Sie sollte das folgende Format aufweisen:
        
        {
          "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
          "type": "SystemAssigned"
        }
        
Führen Sie anschließend den folgenden Befehl aus, indem Sie den Namen Ihres Schlüsseltresors und den Wert von **principalId** verwenden:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get set
```

## <a name="deploy-the-node-app-to-azure-and-retrieve-the-secret-value"></a>Bereitstellen der Node-App in Azure und Abrufen des Geheimniswerts

Führen Sie den folgenden Befehl aus, um die App in Azure bereitzustellen:

```
git push azure master
```

Wenn Sie anschließend zu `https://<app_name>.azurewebsites.net` navigieren, können Sie den geheimen Wert sehen. Stellen Sie sicher, dass Sie den Namen `<YourKeyVaultName>` durch Ihren Tresornamen ersetzt haben.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Key Vault-Instanz erstellt und ein Geheimnis darin gespeichert. Weitere Informationen zu Key Vault und zur Integration in Ihre Anwendungen finden Sie in den folgenden Artikeln:

- [Was ist der Azure-Schlüsseltresor?](key-vault-overview.md)
- [Entwicklerhandbuch zu Azure-Schlüsseltresor](key-vault-developers-guide.md)
- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](about-keys-secrets-and-certificates.md)
- [Bewährte Methoden zum Verwenden von Key Vault](key-vault-best-practices.md)