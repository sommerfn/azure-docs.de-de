---
title: Azure Active Directory-Authentifizierung (Azure AD)
titleSuffix: Azure Cognitive Services
description: Referenz zum Plastischer Reader-SDK
services: cognitive-services
author: rwaller
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 07/22/2019
ms.author: rwaller
ms.openlocfilehash: de6a29e1c4c102aa7d4038185c1635544ba9dfe2
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688789"
---
# <a name="use-azure-active-directory-azure-ad-authentication-with-the-immersive-reader-service"></a>Verwenden der Azure Active Directory-Authentifizierung (Azure AD) mit dem Dienst Plastischer Reader

In den folgenden Abschnitten verwenden Sie entweder die Azure Cloud Shell-Umgebung oder die Azure CLI, um eine neue Plastischer Reader-Ressource mit einer benutzerdefinierten Unterdomäne zu erstellen und dann Azure AD in Ihrem Azure-Mandanten zu konfigurieren. Nachdem Sie diese anfängliche Konfiguration abgeschlossen haben, rufen Sie Azure AD auf, um ein Zugriffstoken zu erhalten. Dies ähnelt der Vorgehensweise bei Verwendung des SDK für den plastischen Reader. Falls Sie einmal nicht mehr weiterwissen, nutzen Sie die Links in jedem Abschnitt mit allen verfügbaren Optionen für jeden Azure CLI-Befehl.

## <a name="create-an-immersive-reader-resource-with-a-custom-subdomain"></a>Erstellen einer Plastischer Reader-Ressource mit einer benutzerdefinierten Unterdomäne

1. Öffnen Sie als Erstes die [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). [Wählen Sie dann ein Abonnement aus](https://docs.microsoft.com/powershell/module/servicemanagement/azure/select-azuresubscription?view=azuresmps-4.0.0#description):

   ```azurecli-interactive
   Select-AzSubscription -SubscriptionName <YOUR_SUBSCRIPTION>
   ```

2. Als Nächstes [erstellen Sie eine Plastischer Reader-Ressource](https://docs.microsoft.com/powershell/module/az.cognitiveservices/new-azcognitiveservicesaccount?view=azps-1.8.0) mit einer benutzerdefinierten Unterdomäne.

   >[!NOTE]
   > Der Name der Unterdomäne wird im SDK für den plastischen Reader verwendet, wenn der Reader mit der launchAsync-Funktion gestartet wird.

   -SkuName kann F0 (Free-Tarif) oder S0 sein (Standard-Tarif, auch kostenlos während Public Preview). Der S0-Tarif hat ein höheres Limit für die Aufrufrate und kein monatliches Kontingent für die Anzahl der Aufrufe.

   -Location kann einer der folgenden Werte sein: `eastus`, `westus`, `australiaeast`, `centralindia`, `japaneast`, `northeurope`, `westeurope`

   -CustomSubdomainName muss global eindeutig sein und darf einige Zeichen nicht enthalten, wie z.B. „.“, „!“ oder „,“.


   ```azurecli-interactive
   $resource = New-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <RESOURCE_NAME> -Type ImmersiveReader -SkuName S0 -Location <REGION> -CustomSubdomainName <UNIQUE_SUBDOMAIN>

   // Display the Resource info
   $resource
   ```

   Wenn die Erstellung erfolgreich verläuft, zeigt der **Endpunkt** der Ressource den Unterdomänennamen an, der für Ihre Ressource eindeutig ist.

   Hier erfassen wir das neu erstellte Ressourcenobjekt in einer **$resource**-Variablen, da diese später beim Gewähren des Zugriffs auf diese Ressource verwendet wird.


   >[!NOTE]
   > Wenn Sie im Azure-Portal eine Ressource erstellen, wird die Ressource „Name“ als benutzerdefinierte Unterdomäne verwendet. Sie können den Namen der Unterdomäne im Portal überprüfen, indem Sie zur Seite „Übersicht“ der Ressource wechseln und die Unterdomäne im dort aufgeführten Endpunkt suchen, z.B. `https://[SUBDOMAIN].cognitiveservices.azure.com/`. Sie können dies auch später überprüfen, wenn Sie die Unterdomäne für die Integration mit dem SDK benötigen.

   Wenn die Ressource im Portal erstellt wurde, können Sie jetzt auch [eine vorhandene Ressource abrufen](https://docs.microsoft.com/powershell/module/az.cognitiveservices/get-azcognitiveservicesaccount?view=azps-1.8.0).

   ```azurecli-interactive
   $resource = Get-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <RESOURCE_NAME>

   // Display the Resource info
   $resource
   ```

## <a name="assign-a-role-to-a-service-principal"></a>Zuweisen einer Rolle zu einem Dienstprinzipal

Nachdem Sie nun über eine Unterdomäne verfügen, die Ihrer Ressource zugeordnet ist, müssen Sie einem Dienstprinzipal eine Rolle zuweisen.

1. Zunächst [erstellen Sie eine Azure AD-Anwendung](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADApplication?view=azps-1.8.0).

   >[!NOTE]
   > Das auch als „geheimer Clientschlüssel“ bezeichnete Kennwort wird beim Abrufen von Authentifizierungstoken verwendet.

   ```azurecli-interactive
   $password = "<YOUR_PASSWORD>"
   $secureStringPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
   $aadApp = New-AzADApplication -DisplayName ImmersiveReaderAAD -IdentifierUris http://ImmersiveReaderAAD -Password $secureStringPassword

   // Display the Azure AD app info
   $aadApp
   ```

   Hier erfassen wir das neu erstellte Azure AD-App-Objekt in einer **$aadApp**-Variablen, die im nächsten Schritt verwendet werden soll.

2. Als Nächstes müssen Sie für die AAD-Anwendung [einen Dienstprinzipal erstellen](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-1.8.0).

   ```azurecli-interactive
   $principal = New-AzADServicePrincipal -ApplicationId $aadApp.ApplicationId

   // Display the service principal info
   $principal
   ```

   Hier erfassen wir das neu erstellte Dienstprinzipalobjekt in einer **$principal**-Variablen, die im nächsten Schritt verwendet werden soll.


3. Der letzte Schritt besteht darin, dem Dienstprinzipal (im Bereich der Ressource) [die Rolle „Cognitive Services-Benutzer“ zuzuweisen](https://docs.microsoft.com/powershell/module/az.Resources/New-azRoleAssignment?view=azps-1.8.0). Durch Zuweisen einer Rolle gewähren Sie dem Dienstprinzipal Zugriff auf diese Ressource. Sie können einem Dienstprinzipal Zugriff auf mehrere Ressourcen in Ihrem Abonnement gewähren.

   ```azurecli-interactive
   New-AzRoleAssignment -ObjectId $principal.Id -Scope $resource.Id -RoleDefinitionName "Cognitive Services User"
   ```

   >[!NOTE]
   > Dieser Schritt muss für jede von Ihnen erstellte Ressource des plastischen Readers ausgeführt werden. Wenn Sie z.B. mehrere Ressourcen für verschiedene globale Regionen erstellen, müssen Sie diesen Schritt für jede dieser regionalen Ressourcen ausführen, damit die Azure AD-Authentifizierung für alle diese Ressourcen funktioniert. Wenn Sie eine neue Ressource zu einem späteren Zeitpunkt erstellen, müssen Sie diesen Rollenzuweisungsschritt auch für diese neue Ressource ausführen.


## <a name="obtain-an-azure-ad-token"></a>Abrufen eines Azure AD-Tokens

In diesem Beispiel wird Ihr Kennwort verwendet, um den Dienstprinzipal zum Abrufen eines Azure AD-Tokens zu authentifizieren.

1. Rufen Sie Ihre **TenantId** ab:
   ```azurecli-interactive
   $context = Get-AzContext
   $context.Tenant.Id
   ```

2. Rufen Sie ein Token ab:
   ```azurecli-interactive
   $authority = "https://login.windows.net/" + $context.Tenant.Id
   $resource = "https://cognitiveservices.azure.com/"
   $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
   $clientCredential = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential" -ArgumentList $aadApp.ApplicationId, $password
   $token = $authContext.AcquireTokenAsync($resource, $clientCredential).Result
   $token
   ```

   >[!NOTE]
   > Das SDK für den plastischen Reader verwendet die AccessToken-Eigenschaft des Tokens, z.B. $token.AccessToken. Weitere Informationen finden Sie in der SDK-[Referenz](reference.md) und in den [Codebeispielen](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples).

Alternativ dazu kann der Dienstprinzipal auch mit einem Zertifikat authentifiziert werden. Außer einem Dienstprinzipal werden auch Benutzerprinzipale unterstützt, indem Berechtigungen durch eine andere Azure AD-Anwendung delegiert werden. In diesem Fall verwenden Benutzer keine Kennwörter oder Zertifikate, sondern werden beim Abrufen des Tokens zur zweistufigen Authentifizierung aufgefordert.

## <a name="next-steps"></a>Nächste Schritte

* Schauen Sie sich das [Tutorial](./tutorial.md) an, um zu sehen, welche weiteren Möglichkeiten Sie mit dem SDK für den plastischen Reader haben.
* Schauen Sie sich das [SDK für den plastischen Reader](https://github.com/microsoft/immersive-reader-sdk) und die [Referenz für das SDK für den plastischen Reader](./reference.md) an.