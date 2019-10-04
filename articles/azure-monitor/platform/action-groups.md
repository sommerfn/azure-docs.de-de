---
title: Erstellen und Verwalten von Aktionsgruppen im Azure-Portal
description: Erfahren Sie, wie Sie Aktionsgruppen im Azure-Portal erstellen und verwalten.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 8/19/2019
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: a0b0df9110f062b5f9c23840cb21308b634c9c81
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69898153"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Erstellen und Verwalten von Aktionsgruppen im Azure-Portal
Eine Aktionsgruppe ist eine Sammlung von Benachrichtigungseinstellungen, die vom Besitzer eines Azure-Abonnements definiert wurden. Azure Monitor- und Service Health-Warnungen verwenden Aktionsgruppen, um Benutzer zu benachrichtigen, dass eine Warnung ausgelöst wurde. Verschiedene Warnungen können je nach den Bedürfnissen des Benutzers die gleiche Aktionsgruppe oder verschiedene Aktionsgruppen verwenden. Sie können in einem Abonnement bis zu 2.000 Aktionsgruppen konfigurieren.

Sie konfigurieren eine Aktion, um eine Person per E-Mail oder SMS zu benachrichtigen. Die Person erhält eine Bestätigung mit dem Hinweis, dass sie der Aktionsgruppe hinzugefügt wurde.

In diesem Artikel wird beschrieben, wie Sie Aktionsgruppen im Azure-Portal erstellen und verwalten.

Jede Aktion besteht aus den folgenden Eigenschaften:

* **Name:** Ein eindeutiger Bezeichner innerhalb der Aktionsgruppe.  
* **Aktionstyp:** Die ausgeführte Aktion. Beispiele sind das Senden eines Sprachanrufs, SMS, E-Mail oder das Auslösen verschiedener Arten von automatisierten Aktionen. Siehe „Typen“ weiter unten in diesem Artikel.
* **Details**: Die entsprechenden Details. Diese können je nach *Aktionstyp* variieren.

Weitere Informationen zum Verwenden von Azure Resource Manager-Vorlagen zur Konfigurierung von Aktionsgruppen finden Sie unter [Aktionsgruppen-Resource Manager-Vorlagen](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Erstellen einer Aktionsgruppe mit dem Azure-Portal

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Überwachen** aus. Im Bereich **Überwachen** sind alle Ihre Überwachungseinstellungen und -daten an einem zentralen Ort zusammengefasst.

    ![Der Dienst „Überwachen“](./media/action-groups/home-monitor.png)
    
1. Wählen Sie **Warnungen** und dann **Aktionen verwalten** aus.

    ![Verwalten der Schaltfläche „Aktionen“](./media/action-groups/manage-action-groups.png)
    
1. Wählen Sie **Aktionsgruppe hinzufügen**, und füllen Sie die Felder aus.

    ![Der Befehl „Aktionsgruppe hinzufügen“](./media/action-groups/add-action-group.png)
    
1. Geben Sie jeweils einen Namen in die Felder **Aktionsgruppenname** und **Kurzname** ein. Der Kurzname wird anstelle eines vollständigen Aktionsgruppennamens verwendet, wenn Benachrichtigungen mithilfe dieser Gruppe gesendet werden.

      ![Das Dialogfeld „Aktionsgruppe hinzufügen“](./media/action-groups/action-group-define.png)

1. In das Feld **Abonnement** wird automatisch Ihr aktuelles Abonnement eingetragen. In diesem Abonnement wird die Aktionsgruppe gespeichert.

1. Wählen Sie die **Ressourcengruppe**, in der die Aktionsgruppe gespeichert wird.

1. Definieren Sie eine Liste mit Aktionen. Geben Sie für jede Aktion Folgendes an:

    1. **Name**: Geben Sie einen eindeutigen Bezeichner für diese Aktion ein.

    1. **Aktionstyp**: Wählen Sie E-Mail/SMS/Push/Sprachanruf, Logik-App, Webhook, ITSM oder Automation-Runbook aus.

    1. **Details**: Geben Sie je nach Aktionstyp eine Telefonnummer, eine E-Mail-Adresse, einen Webhook-URI, eine Azure-App, eine ITSM-Verbindung oder ein Automation-Runbook ein. Legen Sie für die ITSM-Aktion darüber hinaus **Arbeitselement** und andere Felder fest, die Ihr ITSM-Tool benötigt.
    
    1. **Allgemeines Warnungsschema**: Sie können auch das [allgemeine Warnungsschema](https://aka.ms/commonAlertSchemaDocs) verwenden, das den Vorteil einer einzelnen erweiterbaren und einheitlichen Warnungsnutzlast für alle Benachrichtigungsdienste in Azure Monitor bietet.

1. Wählen Sie **OK**, um die Aktionsgruppe zu erstellen.

## <a name="manage-your-action-groups"></a>Verwalten von Aktionsgruppen

Nachdem Sie eine Aktionsgruppe erstellt haben, wird diese im Abschnitt **Aktionsgruppen** des Bereichs **Überwachen** angezeigt. Wählen Sie die Aktionsgruppe, die Sie verwalten möchten, um Folgendes zu tun:

* Fügen Sie Aktionen hinzu, bearbeiten oder entfernen Sie diese.
* Löschen der Aktionsgruppe.

## <a name="action-specific-information"></a>Aktionsspezifische Informationen

> [!NOTE]
> Informationen zu den numerischen Grenzwerten für die unten angegebenen Elemente finden Sie unter [Grenzwerte für Monitor](https://docs.microsoft.com/azure/azure-subscription-service-limits#azure-monitor-limits).  

### <a name="automation-runbook"></a>Automation Runbook
Informationen zu den Einschränkungen für Runbook-Nutzlasten finden Sie unter [Azure-Abonnementdienstgrenzen](../../azure-subscription-service-limits.md).

Es kann sein, dass Sie in einer Aktionsgruppe über eine begrenzte Anzahl von Runbook-Aktionen verfügen. 

### <a name="azure-app-push-notifications"></a>Pushbenachrichtigungen der Azure-App
Es kann sein, dass Sie in einer Aktionsgruppe über eine begrenzte Anzahl von Azure-App-Aktionen verfügen.

### <a name="email"></a>Email
E-Mails werden von den folgenden E-Mail-Adressen gesendet. Achten Sie darauf, dass Ihre E-Mail-Filterung ordnungsgemäß konfiguriert ist.
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

Es kann sein, dass Sie in einer Aktionsgruppe über eine begrenzte Anzahl von E-Mail-Aktionen verfügen. Weitere Informationen finden Sie im Artikel [Ratenlimits für Sprache, SMS-Nachrichten, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhookbeiträge](./../../azure-monitor/platform/alerts-rate-limiting.md).

### <a name="email-azure-resource-manager-role"></a>E-Mail an Azure Resource Manager-Rolle
Senden Sie eine E-Mail an die Mitglieder dieser Rolle im Abonnement.

Es kann sein, dass Sie in einer Aktionsgruppe über eine begrenzte Anzahl von E-Mail-Aktionen verfügen. Weitere Informationen finden Sie im Artikel [Ratenlimits für Sprache, SMS-Nachrichten, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhookbeiträge](./../../azure-monitor/platform/alerts-rate-limiting.md).

### <a name="function"></a>Funktion
Die für Funktions-Apps als Aktionen konfigurierten Funktionsschlüssel werden über die Functions-API gelesen, für die derzeit Funktions-Apps der Version 2 erforderlich sind, um die App-Einstellung „AzureWebJobsSecretStorageType“ auf „files“ festzulegen. Weitere Informationen finden Sie unter [Changes to Key Management in Functions V2]( https://aka.ms/funcsecrets) (Änderungen der Schlüsselverwaltung in Functions V2).

Es kann sein, dass Sie in einer Aktionsgruppe über eine begrenzte Anzahl von Functions-Aktionen verfügen.

### <a name="itsm"></a>ITSM
Für eine ITSM-Aktion muss eine ITSM-Verbindung hergestellt werden. Informieren Sie sich, wie Sie [eine ITSM-Verbindung erstellen](../../azure-monitor/platform/itsmc-overview.md).

Es kann sein, dass Sie in einer Aktionsgruppe über eine begrenzte Anzahl von ITSM-Aktionen verfügen. 

### <a name="logic-app"></a>Logik-App
Es kann sein, dass Sie in einer Aktionsgruppe über eine begrenzte Anzahl von Logik-App-Aktionen verfügen.

### <a name="secure-webhook"></a>Sicherer Webhook
**Die Funktion „sicherer Webhook“ befindet sich zurzeit in der Vorschauphase.**

Mithilfe der Aktion „Aktionsgruppenwebhook“ können Sie Azure Active Directory nutzen, um die Verbindung zwischen Ihrer Aktionsgruppe und Ihrer geschützten Web-API (Webhookendpunkt) zu sichern. Der gesamte Workflow für das Nutzen dieser Funktionalität wird unten beschrieben. Eine Übersicht über Azure AD-Anwendungen und -Dienstprinzipale finden Sie unter [Microsoft Identity Platform (v2.0): Übersicht](https://docs.microsoft.com/azure/active-directory/develop/v2-overview).

1. Erstellen Sie eine Azure AD-Anwendung für ihre geschützte Web-API. Siehe https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview.
    - Konfigurieren Sie Ihre geschützte API so, dass Sie von einer Daemon-App aufgerufen wird.
    
1. Aktivieren Sie Aktionsgruppen, um Ihre Azure AD-Anwendung zu verwenden.

    > [!NOTE]
    > Sie müssen Mitglied der [Rolle „Azure AD-Anwendungsadministrator“](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#available-roles) sein, um dieses Skript ausführen zu können.
    
    - Ändern Sie den Connect-AzureAD-Befehl des PowerShell-Skripts so, dass Ihre Azure AD-Mandanten-ID verwendet wird.
    - Ändern Sie die $myAzureADApplicationObjectId-Variable des PowerShell-Skripts so, dass die Objekt-ID Ihrer Azure AD-Anwendung verwendet wird.
    - Führen Sie das geänderte Skript aus.
    
1. Konfigurieren Sie die Aktion für den sicheren Aktionsgruppen-Webhook.
    - Kopieren Sie den Wert von $MyApp.ObjectID aus dem Skript, und geben Sie ihn in das Feld Anwendungsobjekt-ID in der Webhookaktionsdefinition ein.
    
    ![Sichere Webhookaktion](./media/action-groups/action-groups-secure-webhook.png)

#### <a name="secure-webhook-powershell-script"></a>PowerShell-Skript für sicheren Webhook

```PowerShell
Connect-AzureAD -TenantId "<provide your Azure AD tenant ID here>"
    
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object Id of your Azure AD Application>"
    
# This is the Action Groups Azure AD AppId
$actionGroupsAppId = "461e8683-5575-4561-ac7f-899cc907d62a"
    
# This is the name of the new role we will add to your Azure AD Application
$actionGroupRoleName = "ActionGroupsSecureWebhook"
    
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}
    
# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles
$actionGroupsSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $actionGroupsAppId + "'")

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match "ActionGroupsSecureWebhook")
{
    Write-Host "The Action Groups role is already defined.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $actionGroupRoleName -Description "This is a role for Action Groups to join"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}
    
# Create the service principal if it doesn't exist
if ($actionGroupsSP -match "AzNS AAD Webhook")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the Action Groups Azure AD Application and add it to the role
    $actionGroupsSP = New-AzureADServicePrincipal -AppId $actionGroupsAppId
}
    
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $actionGroupsSP.ObjectId -PrincipalId $actionGroupsSP.ObjectId
    
Write-Host "My Azure AD Application ($myApp.ObjectId): " + $myApp.ObjectId
Write-Host "My Azure AD Application's Roles"
Write-Host $myApp.AppRoles
```

### <a name="sms"></a>sms
Weitere wichtige Informationen finden Sie in den Artikeln zu [Ratenlimits](./../../azure-monitor/platform/alerts-rate-limiting.md) und zum [Verhalten von SMS-Benachrichtigungen](../../azure-monitor/platform/alerts-sms-behavior.md).

Es kann sein, dass Sie in einer Aktionsgruppe über eine begrenzte Anzahl von SMS-Aktionen verfügen.  

### <a name="voice"></a>Sprache
Weitere Informationen finden Sie im Artikel [Ratenlimits für Sprache, SMS-Nachrichten, E-Mail-Nachrichten, Azure App-Pushbenachrichtigungen und Webhookbeiträge](./../../azure-monitor/platform/alerts-rate-limiting.md).

Es kann sein, dass Sie in einer Aktionsgruppe über eine begrenzte Anzahl von Sprachaktionen verfügen.

### <a name="webhook"></a>Webhook
Webhooks werden mit den folgenden Regeln abgerufen. Es wird maximal zweimal versucht, den Webhookaufruf durchzuführen, wenn der HTTP-Statuscode 408, 429, 503 oder 504 zurückgegeben wird oder der HTTP-Endpunkt nicht reagiert. Der erste Wiederholungsversuch erfolgt nach 10 Sekunden. Der zweite Wiederholungsversuch erfolgt nach 100 Sekunden. Nach zwei Ausfällen wird der Endpunkt 30 Minuten lang von keiner Aktionsgruppe aufgerufen. 

Quell-IP-Adressbereiche
 - 13.72.19.232
 - 13.106.57.181
 - 13.106.54.3
 - 13.106.54.19
 - 13.106.38.142
 - 13.106.38.148
 - 13.106.57.196
 - 13.106.57.197
 - 52.244.68.117
 - 52.244.65.137
 - 52.183.31.0
 - 52.184.145.166
 - 51.4.138.199
 - 51.5.148.86
 - 51.5.149.19

Um Updates über Änderungen an diesen IP-Adressen zu erhalten, empfehlen wir Ihnen, eine Service Health-Warnung zu konfigurieren, die eine Überwachung auf Informationsbenachrichtigungen des Aktionsgruppendiensts ausführt.

Es kann sein, dass Sie in einer Aktionsgruppe über eine begrenzte Anzahl von Webhookaktionen verfügen.



## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [SMS-Warnungsverhalten in Aktionsgruppen](../../azure-monitor/platform/alerts-sms-behavior.md).  
* Erweitern Sie Ihr [Verständnis des Webhookschemas für Aktivitätsprotokollwarnungen](../../azure-monitor/platform/activity-log-alerts-webhook.md).  
* Erfahren Sie mehr über den [ITSM-Connector](../../azure-monitor/platform/itsmc-overview.md).
* Weitere Informationen zu [Ratenlimits](../../azure-monitor/platform/alerts-rate-limiting.md) für Warnungen.
* Verschaffen Sie sich eine [Übersicht über Aktivitätsprotokollwarnungen](../../azure-monitor/platform/alerts-overview.md), und erfahren Sie, wie Sie Warnungen empfangen können.  
* Erfahren Sie, wie Sie [Warnungen konfigurieren, wenn eine Dienstintegritätsbenachrichtigung gesendet wird](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).
