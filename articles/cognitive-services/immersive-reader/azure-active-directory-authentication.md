---
title: Azure Active Directory-Authentifizierung (Azure AD)
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird beschrieben, wie Sie eine neue Plastischer Reader-Ressource mit einer benutzerdefinierten Unterdomäne erstellen und dann Azure AD in Ihrem Azure-Mandanten konfigurieren.
services: cognitive-services
author: rwaller
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: rwaller
ms.openlocfilehash: d51c27b90113679c1547f2d030459a03cc22c80c
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72299806"
---
# <a name="use-azure-active-directory-azure-ad-authentication-with-the-immersive-reader-service"></a>Verwenden der Azure Active Directory-Authentifizierung (Azure AD) mit dem Dienst Plastischer Reader

In den folgenden Abschnitten verwenden Sie entweder die Azure Cloud Shell-Umgebung oder Azure PowerShell, um eine neue Plastischer Reader-Ressource mit einer benutzerdefinierten Unterdomäne zu erstellen und dann Azure AD in Ihrem Azure-Mandanten zu konfigurieren. Nachdem Sie diese anfängliche Konfiguration abgeschlossen haben, rufen Sie Azure AD auf, um ein Zugriffstoken zu erhalten. Dies ähnelt der Vorgehensweise bei Verwendung des SDK für den plastischen Reader. Falls Sie einmal nicht mehr weiterwissen, nutzen Sie die Links in jedem Abschnitt mit allen verfügbaren Optionen für jeden Azure PowerShell-Befehl.

## <a name="create-an-immersive-reader-resource-with-a-custom-subdomain"></a>Erstellen einer Plastischer Reader-Ressource mit einer benutzerdefinierten Unterdomäne

1. Öffnen Sie als Erstes die [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). [Wählen Sie dann ein Abonnement aus](https://docs.microsoft.com/powershell/module/servicemanagement/azure/select-azuresubscription?view=azuresmps-4.0.0#description):

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName <YOUR_SUBSCRIPTION>
   ```

2. Als Nächstes [erstellen Sie eine Plastischer Reader-Ressource](https://docs.microsoft.com/powershell/module/az.cognitiveservices/new-azcognitiveservicesaccount?view=azps-1.8.0) mit einer benutzerdefinierten Unterdomäne.

   >[!NOTE]
   > Der Name der Unterdomäne wird im SDK für den plastischen Reader verwendet, wenn der Reader mit der launchAsync-Funktion gestartet wird.

   -SkuName kann F0 (Free-Tarif) oder S0 sein (Standard-Tarif, auch kostenlos während Public Preview). Der S0-Tarif hat ein höheres Limit für die Aufrufrate und kein monatliches Kontingent für die Anzahl der Aufrufe.

   -Location kann einer der folgenden Werte sein: `eastus`, `westus`, `australiaeast`, `centralindia`, `japaneast`, `northeurope`, `westeurope`

   -CustomSubdomainName muss global eindeutig sein und darf einige Zeichen nicht enthalten, wie z.B. „.“, „!“ oder „,“.


   ```azurepowershell-interactive
   $resource = New-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <RESOURCE_NAME> -Type ImmersiveReader -SkuName S0 -Location <REGION> -CustomSubdomainName <UNIQUE_SUBDOMAIN>

   // Display the Resource info
   $resource
   ```

   Wenn die Erstellung erfolgreich verläuft, zeigt der **Endpunkt** der Ressource den Unterdomänennamen an, der für Ihre Ressource eindeutig ist.

   Hier erfassen wir das neu erstellte Ressourcenobjekt in einer **$resource**-Variablen, da diese später beim Gewähren des Zugriffs auf diese Ressource verwendet wird.


   >[!NOTE]
   > Wenn Sie im Azure-Portal eine Ressource erstellen, wird die Ressource „Name“ als benutzerdefinierte Unterdomäne verwendet. Sie können den Namen der Unterdomäne im Portal überprüfen, indem Sie zur Seite „Übersicht“ der Ressource wechseln und die Unterdomäne im dort aufgeführten Endpunkt suchen, z.B. `https://[SUBDOMAIN].cognitiveservices.azure.com/`. Sie können dies auch später überprüfen, wenn Sie die Unterdomäne für die Integration mit dem SDK benötigen.

   Wenn die Ressource im Portal erstellt wurde, können Sie jetzt auch [eine vorhandene Ressource abrufen](https://docs.microsoft.com/powershell/module/az.cognitiveservices/get-azcognitiveservicesaccount?view=azps-1.8.0).

   ```azurepowershell-interactive
   $resource = Get-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <RESOURCE_NAME>

   // Display the Resource info
   $resource
   ```

## <a name="assign-a-role-to-a-service-principal"></a>Zuweisen einer Rolle zu einem Dienstprinzipal

Nachdem Sie nun über eine Unterdomäne verfügen, die Ihrer Ressource zugeordnet ist, müssen Sie einem Dienstprinzipal eine Rolle zuweisen.

1. Zunächst [erstellen Sie eine Azure AD-Anwendung](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADApplication?view=azps-1.8.0).

   >[!NOTE]
   > Das auch als „geheimer Clientschlüssel“ bezeichnete Kennwort wird beim Abrufen von Authentifizierungstoken verwendet.

   ```azurepowershell-interactive
   $password = "<YOUR_PASSWORD>"
   $secureStringPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
   $aadApp = New-AzADApplication -DisplayName ImmersiveReaderAAD -IdentifierUris http://ImmersiveReaderAAD -Password $secureStringPassword

   // Display the Azure AD app info
   $aadApp
   ```

   Hier erfassen wir das neu erstellte Azure AD-App-Objekt in einer **$aadApp**-Variablen, die im nächsten Schritt verwendet werden soll.

2. Als Nächstes müssen Sie für die AAD-Anwendung [einen Dienstprinzipal erstellen](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-1.8.0).

   ```azurepowershell-interactive
   $principal = New-AzADServicePrincipal -ApplicationId $aadApp.ApplicationId

   // Display the service principal info
   $principal
   ```

   Hier erfassen wir das neu erstellte Dienstprinzipalobjekt in einer **$principal**-Variablen, die im nächsten Schritt verwendet werden soll.


3. Der letzte Schritt besteht darin, dem Dienstprinzipal (im Bereich der Ressource) [die Rolle „Cognitive Services-Benutzer“ zuzuweisen](https://docs.microsoft.com/powershell/module/az.Resources/New-azRoleAssignment?view=azps-1.8.0). Durch Zuweisen einer Rolle gewähren Sie dem Dienstprinzipal Zugriff auf diese Ressource. Sie können einem Dienstprinzipal Zugriff auf mehrere Ressourcen in Ihrem Abonnement gewähren.

   ```azurepowershell-interactive
   New-AzRoleAssignment -ObjectId $principal.Id -Scope $resource.Id -RoleDefinitionName "Cognitive Services User"
   ```

   >[!NOTE]
   > Dieser Schritt muss für jede von Ihnen erstellte Ressource des plastischen Readers ausgeführt werden. Wenn Sie z.B. mehrere Ressourcen für verschiedene globale Regionen erstellen, müssen Sie diesen Schritt für jede dieser regionalen Ressourcen ausführen, damit die Azure AD-Authentifizierung für alle diese Ressourcen funktioniert. Wenn Sie eine neue Ressource zu einem späteren Zeitpunkt erstellen, müssen Sie diesen Rollenzuweisungsschritt auch für diese neue Ressource ausführen.


## <a name="obtain-an-azure-ad-token"></a>Abrufen eines Azure AD-Tokens

In diesem Beispiel wird Ihr Kennwort verwendet, um den Dienstprinzipal zum Abrufen eines Azure AD-Tokens zu authentifizieren.

1. Rufen Sie Ihre **TenantId** ab:
   ```azurepowershell-interactive
   $context = Get-AzContext
   $context.Tenant.Id
   ```

2. Rufen Sie ein Token ab:
   ```azurepowershell-interactive
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

* Sehen Sie sich das [Node.js-Tutorial](./tutorial-nodejs.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Node.js bietet.
* Schauen Sie sich das [Python-Tutorial](./tutorial-python.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Python bietet.
* Schauen Sie sich das [Swift-Tutorial](./tutorial-ios-picture-immersive-reader.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Swift bietet.
* Machen Sie sich mit dem [SDK für Plastischer Reader](https://github.com/microsoft/immersive-reader-sdk) und der [zugehörigen Referenz](./reference.md) vertraut.