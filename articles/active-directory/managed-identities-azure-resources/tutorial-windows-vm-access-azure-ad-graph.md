---
title: Verwenden einer systemseitig zugewiesenen verwalteten Identität einer Windows-VM für den Zugriff auf die Azure AD Graph-API
description: In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität einer Windows-VM verwenden, um auf die Azure AD Graph-API zuzugreifen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/20/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60938f26c27b9f94046b1be8e3d0cb6b247017c9
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997783"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-ad-graph-api"></a>Tutorial: Verwenden einer systemseitig zugewiesenen verwalteten Identität einer Windows-VM für den Zugriff auf die Azure AD Graph-API

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice.md)]

In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität für einen virtuellen Windows-Computer verwenden, um zum Abrufen ihrer Gruppenmitgliedschaften auf die Azure AD Graph-API zuzugreifen. Verwaltete Identitäten für Azure-Ressourcen werden von Azure automatisch verwaltet und ermöglichen Ihnen die Authentifizierung für Dienste, die die Azure AD-Authentifizierung unterstützen, ohne dass Sie Anmeldeinformationen in Ihren Code einfügen müssen.  In diesem Tutorial fragen Sie die Mitgliedschaft Ihrer VM-Identität in Azure AD-Gruppen ab. Gruppeninformationen werden beispielsweise häufig für Autorisierungsentscheidungen verwendet. Im Hintergrund wird die verwaltete Identität Ihrer VM durch einen **Dienstprinzipal** in Azure AD dargestellt. Fügen Sie den Dienstprinzipal, der die VM-Identität darstellt, vor dem Ausführen der Gruppenabfrage einer Gruppe in Azure AD hinzu. Hierzu können Sie Azure PowerShell, Azure AD PowerShell oder die Azure-Befehlszeilenschnittstelle (CLI) verwenden.

> [!div class="checklist"]
> * Stellen Sie eine Verbindung mit Azure AD her.
> * Fügen Sie Ihre VM-Identität einer Gruppe in Azure AD hinzu. 
> * Gewähren Sie der Identität der VM Zugriff auf Azure AD Graph. 
> * Rufen Sie mithilfe der VM-Identität ein Zugriffstoken ab, und rufen Sie damit Azure AD Graph auf.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- Um einer VM-Identität Zugriff auf Azure AD Graph gewähren zu können, muss Ihrem Konto die Rolle **Globaler Administrator** in Azure AD zugewiesen sein.
- Installieren Sie [die aktuelle Version von Azure AD PowerShell](/powershell/azure/active-directory/install-adv2), sofern noch nicht geschehen. 

## <a name="connect-to-azure-ad"></a>Stellen Sie eine Verbindung mit Azure AD her.

Sie müssen eine Verbindung mit Azure AD herstellen, um die VM einer Gruppe zuzuweisen und ihr die Berechtigung zum Abrufen der Gruppenmitgliedschaften zu erteilen. Sie können das Cmdlet „Connect-AzureAD“ direkt oder mit dem TenantId-Parameter verwenden, falls Sie über mehrere Mandanten verfügen.

```powershell
Connect-AzureAD
```
OR
```powershell
Connect-AzureAD -TenantId "Object Id of the tenant"
```

## <a name="add-your-vm-identity-to-a-group-in-azure-ad"></a>Fügen Sie Ihre VM-Identität einer Gruppe in Azure AD hinzu.

Wenn Sie die systemseitig zugewiesene verwaltete Identität auf der Windows-VM aktivieren, wird ein Dienstprinzipal in Azure AD erstellt.  Nun müssen Sie die VM einer Gruppe hinzufügen.  Im folgenden Beispiel wird eine neue Gruppe in Azure AD erstellt und der Dienstprinzipal Ihrer VM dieser Gruppe hinzugefügt:

```powershell
New-AzureADGroup -DisplayName "myGroup" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$AzureADGroup = Get-AzureADGroup -Filter "displayName eq 'myGroup'"
$ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq 'myVM'"
Add-AzureADGroupMember -ObjectId $AzureADGroup.ObjectID -RefObjectId $ManagedIdentitiesServicePrincipal.ObjectId
```
## <a name="grant-your-vm-access-to-the-azure-ad-graph-api"></a>Gewähren des Zugriffs auf die Azure AD Graph-API für Ihre VM

Mithilfe von verwalteten Identitäten für Azure-Ressourcen kann der Code Zugriffstoken zur Authentifizierung von Ressourcen abrufen, die die Azure AD-Authentifizierung unterstützen. Die Microsoft Azure AD Graph-API unterstützt die Azure AD-Authentifizierung. In diesem Schritt gewähren Sie dem Dienstprinzipal Ihrer VM-Identität Zugriff auf Azure AD Graph, damit die VM-Identität Gruppenmitgliedschaften abfragen kann. Dienstprinzipalen wird der Zugriff auf Microsoft Graph oder Azure AD Graph über **Anwendungsberechtigungen** gewährt. Der Anwendungsberechtigungstyp, den Sie erteilen müssen, hängt von der Entität ab, auf die Sie in Microsoft oder Azure AD Graph zugreifen möchten.

In diesem Tutorial ermöglichen Sie Ihrer VM-Identität das Abfragen von Gruppenmitgliedschaften mithilfe der Anwendungsberechtigung ```Directory.Read.All```. Um diese Berechtigung zu erteilen, benötigen Sie ein Benutzerkonto, dem in Azure AD die Rolle „Globaler Administrator“ zugewiesen ist. Normalerweise erteilen Sie eine Anwendungsberechtigung, indem Sie im Azure-Portal die Registrierung der Anwendung besuchen und die Berechtigung dort hinzufügen. Für verwaltete Identitäten für Azure-Ressourcen werden jedoch keine Anwendungsobjekte in Azure AD registriert, sondern nur Dienstprinzipale. Zum Registrieren der Anwendungsberechtigung verwenden Sie das Azure AD PowerShell-Befehlszeilentool. 

Azure AD Graph:
- appId des Dienstprinzipals (wird zum Erteilen der Anwendungsberechtigung verwendet): 00000002-0000-0000-c000-000000000000
- Ressourcen-ID (wird zum Anfordern des Zugriffstokens von verwalteten Identitäten für Azure-Ressourcen verwendet): https://graph.windows.net
- Referenz zu Berechtigungsbereichen: [Azure AD Graph-Berechtigungsreferenz](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

### <a name="grant-application-permissions-using-azure-ad-powershell"></a>Erteilen von Anwendungsberechtigungen mit Azure AD PowerShell

Sie benötigen Azure AD PowerShell, um diese Option verwenden zu können. Falls Sie das Tool nicht installiert haben, [laden Sie die aktuelle Version herunter](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2), bevor Sie fortfahren.

1. Öffnen Sie ein PowerShell-Fenster, und stellen Sie eine Verbindung mit Azure AD her:

   ```powershell
   Connect-AzureAD
   ```
   Verwenden Sie zum Herstellen einer Verbindung mit einer bestimmten Azure Active Directory-Instanz den Parameter _TenantId_ wie folgt:

   ```powershell
   Connect-AzureAD -TenantId "Object Id of the tenant"
   ```

   
2. Führen Sie die folgenden PowerShell-Befehle aus, um dem Dienstprinzipal, der Ihre VM-Identität darstellt, die Anwendungsberechtigung ``Directory.Read.All`` zuzuweisen.

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq 'myVM'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}
   New-AzureAdServiceAppRoleAssignment -ObjectId $ManagedIdentitiesServicePrincipal.ObjectId -PrincipalId $ManagedIdentitiesServicePrincipal.ObjectId -ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole.Id
   ``` 

   Die Ausgabe des letzten Befehls sollte wie folgt aussehen und die ID der Zuweisung zurückgeben:
        
   `ObjectId`:`gzR5KyLAiUOTiqFhNeWZWBtK7ZKqNJxAiWYXYVHlgMs`

   `ResourceDisplayName`:`Windows Azure Active Directory`

   `PrincipalDisplayName`:`myVM` 

   Wenn beim Aufruf von `New-AzureAdServiceAppRoleAssignment` der Fehler `bad request, one or more properties are invalid` auftritt, wurde die Anwendungsberechtigung dem Dienstprinzipal der VM-Identität möglicherweise bereits zugewiesen. Mit den folgenden PowerShell-Befehlen können Sie überprüfen, ob die Identität Ihrer VM bereits über die Anwendungsberechtigung für Azure AD Graph verfügt:

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq '<VM-NAME>'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}
   Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId | Where-Object {$_.Id -eq $AppRole.Id -and $_.PrincipalId -eq $ManagedIdentitiesServicePrincipal.ObjectId}
   ```

   Mit den folgenden PowerShell-Befehlen können Sie alle Anwendungsberechtigungen auflisten, die für Azure AD Graph erteilt wurden:

   ```powershell
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId
   ``` 

   Mit den folgenden PowerShell-Befehlen können Sie Anwendungsberechtigungen entfernen, die der Identität Ihrer VM für Azure AD Graph erteilt wurden:

   ```powershell
   $ManagedIdentitiesServicePrincipal = Get-AzureADServicePrincipal -Filter "displayName eq '<VM-NAME>'"
   $GraphAppId = "00000002-0000-0000-c000-000000000000"
   $GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
   $PermissionName = "Directory.Read.All"
   $AppRole = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName -and $_.AllowedMemberTypes -contains "Application"}   
   $ServiceAppRoleAssignment = Get-AzureADServiceAppRoleAssignment -ObjectId $GraphServicePrincipal.ObjectId | Where-Object {$_.Id -eq $AppRole.Id -and $_.PrincipalId -eq $ManagedIdentitiesServicePrincipal.ObjectId}
   Remove-AzureADServiceAppRoleAssignment -AppRoleAssignmentId $ServiceAppRoleAssignment.ObjectId -ObjectId $ManagedIdentitiesServicePrincipal.ObjectId
   ```
 
## <a name="get-an-access-token-using-the-vms-identity-to-call-azure-ad-graph"></a>Abrufen eines Zugriffstokens mithilfe der VM-Identität zum Aufrufen von Azure AD Graph 

Um die systemseitig zugewiesene verwaltete Identität der VM zur Authentifizierung bei Azure AD Graph zu verwenden, müssen Sie Anforderungen auf der VM ausführen.

1. Navigieren Sie im Portal zu **Virtuelle Computer**, wechseln Sie dann zu Ihrer Windows-VM, und klicken Sie auf dem Blatt **Übersicht** auf **Verbinden**.  
2. Geben Sie den **Benutzernamen** und das **Kennwort** ein, die Sie beim Erstellen der Windows-VM verwendet haben.
3. Sie haben nun eine Remotedesktopverbindung mit der VM erstellt. Öffnen Sie jetzt PowerShell in der Remotesitzung.  
4. Erstellen Sie mithilfe des PowerShell-Befehls „Invoke-WebRequest“ eine Anforderung an den lokalen Endpunkt für verwaltete Identitäten für Azure-Ressourcen, um ein Zugriffstoken für Azure AD Graph abzurufen.

   ```powershell
   $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://graph.windows.net' -Method GET -Headers @{Metadata="true"}
   ```

   Konvertieren Sie die Antwort aus einem JSON-Objekt in ein PowerShell-Objekt.

   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Extrahieren Sie das Zugriffstoken aus der Antwort.

   ```powershell
   $AccessToken = $content.access_token
   ```

5. Mit der Objekt-ID des Dienstprinzipals Ihrer VM-Identität (diesen Wert können Sie aus der in den vorherigen Schritten deklarierten Variablen abrufen: ``$ManagedIdentitiesServicePrincipal.ObjectId``) können Sie die Azure AD Graph-API zum Abrufen der Gruppenmitgliedschaften abfragen. Ersetzen Sie `<OBJECT ID>` durch die Objekt-ID aus dem vorherigen Schritt und <`ACCESS-TOKEN>` durch das zuvor abgerufene Zugriffstoken:

   ```powershell
   Invoke-WebRequest 'https://graph.windows.net/<Tenant ID>/servicePrincipals/<VM Object ID>/getMemberGroups?api-version=1.6' -Method POST -Body '{"securityEnabledOnly":"false"}' -Headers @{Authorization="Bearer $AccessToken"} -ContentType "application/json"
   ```
   
   Die Antwort enthält die `Object ID` der Gruppe, der Sie den Dienstprinzipal Ihrer VM zuvor hinzugefügt haben.

   Antwort:

   ```powershell   
   Content : {"odata.metadata":"https://graph.windows.net/<Tenant ID>/$metadata#Collection(Edm.String)","value":["<ObjectID of VM's group membership>"]}
   ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine systemseitig zugewiesene verwaltete Identität einer Windows-VM verwenden, um auf Azure AD Graph zuzugreifen.  Weitere Informationen zu Azure AD Graph finden Sie hier:

>[!div class="nextstepaction"]
>[Azure AD Graph](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api)
