---
title: Bereichsbezogene Synchronisierung für Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die bereichsbezogene Synchronisierung von Azure AD für eine verwaltete Azure Active Directory Domain Services-Domäne konfigurieren
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: article
ms.date: 09/06/2019
ms.author: iainfou
ms.openlocfilehash: 5fe19d3800883782187ae15c0a6fc0cd9709f0e9
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70842669"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-azure-active-directory-domain-services"></a>Konfigurieren der bereichsbezogenen Synchronisierung von Azure AD für Azure Active Directory Domain Services

Zum Bereitstellen von Authentifizierungsdiensten synchronisiert Azure Active Directory Domain Services (Azure AD DS) Benutzer und Gruppen von Azure AD. In einer Hybridumgebung können Benutzer und Gruppen aus einer lokalen AD DS-Umgebung (Active Directory Domain Services) zuerst mithilfe von Azure AD Connect mit Azure AD und anschließend mit Azure AD DS synchronisiert werden. Standardmäßig werden alle Benutzer und Gruppen aus einem Azure AD-Verzeichnis mit einer verwalteten Azure AD DS-Domäne synchronisiert. Bei speziellen Anforderungen können Sie stattdessen festlegen, dass nur eine definierte Gruppe von Benutzern synchronisiert werden soll.

In diesem Artikel erfahren Sie, wie Sie eine verwaltete Azure AD DS-Domäne mit einer bereichsbezogenen Synchronisierung erstellen, und anschließend die Gruppe der Benutzer für diesen Bereich ändern oder deaktivieren.

## <a name="scoped-synchronization-overview"></a>Übersicht über die bereichsbezogene Synchronisierung

Standardmäßig werden alle Benutzer und Gruppen aus einem Azure AD-Verzeichnis mit einer verwalteten Azure AD DS-Domäne synchronisiert. Wenn nur wenige Benutzer auf die verwaltete Domäne zugreifen müssen, können Sie nur diese Benutzerkonten synchronisieren. Diese bereichsbezogene Synchronisierung ist gruppenbasiert. Wenn Sie eine gruppenbasierte bereichsbezogene Synchronisierung konfigurieren, werden nur die Benutzerkonten, die zu den von Ihnen angegebenen Gruppen gehören, mit der verwalteten Azure AD DS-Domäne synchronisiert.

Aus der folgenden Tabelle geht hervor, wie Sie die bereichsbezogene Synchronisierung verwenden können:

| Aktueller Status | Gewünschter Status | Erforderliche Konfiguration |
| --- | --- | --- |
| Eine vorhandene verwaltete Domäne ist zum Synchronisieren aller Benutzerkonten und Gruppen konfiguriert. | Sie möchten nur Benutzerkonten synchronisieren, die zu bestimmten Gruppen gehören. | Sie können von der Synchronisierung aller Benutzer nicht zur bereichsbezogenen Synchronisierung wechseln. [Löschen Sie die vorhandene verwaltete Domäne](delete-aadds.md), und führen Sie dann die in diesem Artikel beschriebenen Schritte aus, um eine verwaltete Azure AD DS-Domäne mit konfigurierter bereichsbezogener Synchronisierung neu zu erstellen. |
| Keine vorhandene verwaltete Domäne. | Sie möchten eine neue verwaltete Domäne erstellen und nur Benutzerkonten synchronisieren, die zu bestimmten Gruppen gehören. | Führen Sie die in diesem Artikel beschriebenen Schritte aus, um eine neue verwaltete Azure AD DS-Domäne mit konfigurierter bereichsbezogener Synchronisierung zu erstellen. |
| Eine vorhandene verwaltete Domäne ist so konfiguriert, dass nur die Konten synchronisiert werden, die zu bestimmten Gruppen gehören. | Sie möchten die Liste der Gruppen ändern, deren Benutzer mit der verwalteten Azure AD DS-Domäne synchronisiert werden sollen. | Führen Sie die in diesem Artikel beschriebenen Schritte aus, um die bereichsbezogene Synchronisierung zu ändern. |

Zum Konfigurieren der Einstellungen für die bereichsbezogene Synchronisierung verwenden Sie das Azure-Portal oder PowerShell:

| Aktion | | |
|--|--|--|
| Erstellen einer verwalteten Azure AD DS-Domäne und Konfigurieren der bereichsbezogenen Synchronisierung | [Azure-Portal](#enable-scoped-synchronization-using-the-azure-portal) | [PowerShell](#enable-scoped-synchronization-using-powershell) |
| Ändern der bereichsbezogenen Synchronisierung | [Azure-Portal](#modify-scoped-synchronization-using-the-azure-portal) | [PowerShell](#modify-scoped-synchronization-using-powershell) |
| Deaktivieren der bereichsbezogenen Synchronisierung | [Azure-Portal](#disable-scoped-synchronization-using-the-azure-portal) | [PowerShell](#disable-scoped-synchronization-using-powershell) |

> [!WARNING]
> Wenn Sie den Bereich der Synchronisierung ändern, werden zwangsläufig alle Daten von der verwalteten Azure AD DS-Domäne neu synchronisiert.
> 
>  * Wenn Sie den Synchronisierungsbereich für eine verwaltete Azure AD DS-Domäne ändern, erfolgt eine vollständige Neusynchronisierung.
>  * Objekte, die in der verwalteten Azure AD DS-Domäne nicht mehr erforderlich sind, werden gelöscht. Neue Objekte werden in der verwalteten Domäne erstellt.
>  * Diese Neusynchronisierung dauert möglicherweise etwas länger. Die Synchronisierungszeit hängt von der Anzahl der Objekte (Benutzer, Gruppen und Gruppenmitgliedschaften) in der verwalteten Azure AD DS-Domäne und im Azure AD-Verzeichnis ab. Bei großen Verzeichnissen mit Hunderten oder Tausenden von Objekten kann die Neusynchronisierung einige Tage dauern.

## <a name="enable-scoped-synchronization-using-the-azure-portal"></a>Aktivieren der bereichsbezogenen Synchronisierung im Azure-Portal

1. Führen Sie die Anweisungen im [Tutorial zum Erstellen und Konfigurieren einer Azure AD DS-Instanz](tutorial-create-instance.md) aus. Sorgen Sie dafür, dass Sie bis auf die Angabe des Synchronisierungsbereichs alle Voraussetzungen erfüllt und die Bereitstellungsschritte ausgeführt haben.
1. Wählen Sie beim Synchronisierungsschritt **Bereichsbezogen** aus, und legen Sie dann die Azure AD-Gruppen fest, die mit der Azure AD DS-Instanz synchronisiert werden sollen.

Es kann bis zu einer Stunde dauern, bis die Bereitstellung der verwalteten Azure AD DS-Domäne abgeschlossen ist. Während dieser Bereitstellungsphase wird im Azure-Portal auf der Seite **Übersicht** für Ihre verwaltete Azure AD DS-Domäne der aktuelle Status angezeigt.

Wenn im Azure-Portal angezeigt wird, dass die verwaltete Azure AD DS-Domäne die Bereitstellung abgeschlossen hat, müssen Sie die folgenden Aufgaben ausführen:

* Aktualisieren Sie DNS-Einstellungen für das virtuelle Netzwerk, damit virtuelle Computer die verwaltete Domäne für den Domänenbeitritt oder die Domänenauthentifizierung finden können.
    * Wählen Sie zum Konfigurieren von DNS Ihre verwaltete Azure AD DS-Domäne im Portal aus. Im Fenster **Übersicht** werden Sie aufgefordert, diese DNS-Einstellungen automatisch zu konfigurieren.
* [Aktivieren Sie die Kennwortsynchronisierung für Azure AD Domain Services](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds), damit sich Endbenutzer mit ihren Unternehmensanmeldeinformationen bei der verwalteten Domäne anmelden können.

## <a name="modify-scoped-synchronization-using-the-azure-portal"></a>Ändern der bereichsbezogenen Synchronisierung im Azure-Portal

Führen Sie die folgenden Schritte aus, um die Liste der Gruppen zu ändern, deren Benutzer mit der verwalteten Azure AD DS-Domäne synchronisiert werden sollen:

1. Wählen Sie im Azure-Portal Ihre Azure AD DS-Instanz aus, z.B. *contoso.com*.
1. Wählen Sie im Menü auf der linken Seite **Synchronisierung** aus.
1. Um eine Gruppe hinzuzufügen, wählen Sie im oberen Bereich **+ Gruppen auswählen** aus, und wählen Sie dann die hinzuzufügenden Gruppen aus.
1. Um eine Gruppe aus dem Synchronisierungsbereich zu entfernen, wählen Sie diese in der Liste der derzeit synchronisierten Gruppen aus, und wählen Sie dann **Gruppen entfernen** aus.
1. Wenn alle Änderungen vorgenommen wurden, wählen Sie **Synchronisierungsbereich speichern** aus.

Wenn Sie den Bereich der Synchronisierung ändern, werden zwangsläufig alle Daten von der verwalteten Azure AD DS-Domäne neu synchronisiert. Objekte, die in der verwalteten Azure AD DS-Domäne nicht mehr benötigt werden, werden gelöscht, und die erneute Synchronisierung kann einige Zeit in Anspruch nehmen.

## <a name="disable-scoped-synchronization-using-the-azure-portal"></a>Deaktivieren der bereichsbezogenen Synchronisierung im Azure-Portal

Führen Sie die folgenden Schritte aus, um die gruppenbasierte bereichsbezogene Synchronisierung für eine verwaltete Azure AD DS-Domäne zu deaktivieren:

1. Wählen Sie im Azure-Portal Ihre Azure AD DS-Instanz aus, z.B. *contoso.com*.
1. Wählen Sie im Menü auf der linken Seite **Synchronisierung** aus.
1. Legen Sie den Synchronisierungsbereich von **Bereichsbezogen** auf **Alle** fest, und wählen Sie dann **Synchronisierungsbereich speichern** aus.

Wenn Sie den Bereich der Synchronisierung ändern, werden zwangsläufig alle Daten von der verwalteten Azure AD DS-Domäne neu synchronisiert. Objekte, die in der verwalteten Azure AD DS-Domäne nicht mehr benötigt werden, werden gelöscht, und die erneute Synchronisierung kann einige Zeit in Anspruch nehmen.

## <a name="powershell-script-for-scoped-synchronization"></a>PowerShell-Skript für die bereichsbezogene Synchronisierung

Zum Konfigurieren der bereichsbezogenen Synchronisierung mithilfe von PowerShell müssen Sie zuerst das folgende Skript in einer Datei namens `Select-GroupsToSync.ps1` speichern. Mit diesem Skript wird Azure AD DS für die Synchronisierung ausgewählter Gruppen von Azure AD konfiguriert. Alle Benutzerkonten, die zu den angegebenen Gruppen gehören, werden mit der verwalteten Azure AD DS-Domäne synchronisiert.

Dieses Skript wird in den weiteren Schritten in diesem Artikel verwendet.

```powershell
param (
    [Parameter(Position = 0)]
    [String[]]$groupsToAdd
)

Connect-AzureAD
$sp = Get-AzureADServicePrincipal -Filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
$role = $sp.AppRoles | where-object -FilterScript {$_.DisplayName -eq "User"}

Write-Output "`n****************************************************************************"

Write-Output "Total group-assignments need to be added: $($groupsToAdd.Count)"
$newGroupIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($groupName in $groupsToAdd)
{
    try
    {
        $group = Get-AzureADGroup -Filter "DisplayName eq '$groupName'"
        $newGroupIds.Add($group.ObjectId)

        Write-Output "Group-Name: $groupName, Id: $($group.ObjectId)"
    }
    catch
    {
        Write-Error "Failed to find group: $groupName. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

$currentAssignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId
Write-Output "Total current group-assignments: $($currentAssignments.Count), SP-ObjectId: $($sp.ObjectId)"

$currAssignedObjectIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($assignment in $currentAssignments)
{
    Write-Output "Assignment-ObjectId: $($assignment.PrincipalId)"

    if ($newGroupIds.Contains($assignment.PrincipalId) -eq $false)
    {
        Write-Output "This assignment is not needed anymore. Removing it! Assignment-ObjectId: $($assignment.PrincipalId)"
        Remove-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -AppRoleAssignmentId $assignment.ObjectId
    }
    else
    {
        $currAssignedObjectIds.Add($assignment.PrincipalId)
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

foreach ($id in $newGroupIds)
{
    try
    {
        if ($currAssignedObjectIds.Contains($id) -eq $false)
        {
            Write-Output "Adding new group-assignment. Role-Id: $($role.Id), Group-Object-Id: $id, ResourceId: $($sp.ObjectId)"
            New-AzureADGroupAppRoleAssignment -Id $role.Id -ObjectId $id -PrincipalId $id -ResourceId $sp.ObjectId
        }
        else
        {
            Write-Output "Group-ObjectId: $id is already assigned."
        }
    }
    catch
    {
        Write-Error "Exception occurred assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```

## <a name="enable-scoped-synchronization-using-powershell"></a>Aktivieren der bereichsbezogenen Synchronisierung mit PowerShell

Verwenden Sie PowerShell, um diese Schritte auszuführen. Lesen Sie die Anweisungen zum [Aktivieren von Azure Active Directory Domain Services mithilfe von PowerShell](powershell-create-instance.md). Einige Schritte in diesem Artikel wurden leicht geändert, um die bereichsbezogene Synchronisierung zu konfigurieren.

1. Führen Sie die folgenden Aufgaben aus dem Artikel aus, um Azure AD DS mithilfe von PowerShell zu aktivieren. Halten Sie bei dem Schritt an, mit dem die eigentliche verwaltete Domäne erstellt wird. Die bereichsbezogene Synchronisierung wird beim Erstellen der verwalteten Azure AD DS-Domäne konfiguriert.

   * [Installieren Sie die erforderlichen PowerShell-Module](powershell-create-instance.md#prerequisites).
   * [Erstellen Sie den erforderlichen Dienstprinzipal und die Azure AD-Gruppe für den Administratorzugriff](powershell-create-instance.md#create-required-azure-ad-resources).
   * [Erstellen Sie unterstützende Azure-Ressourcen wie ein virtuelles Netzwerk und Subnetze](powershell-create-instance.md#create-supporting-azure-resources).

1. Legen Sie die Gruppen und die darin enthaltenen Benutzer fest, die von Azure AD synchronisiert werden sollen. Erstellen Sie eine Liste mit den Anzeigenamen der Gruppen, die mit Azure AD DS synchronisiert werden sollen.

1. Führen Sie [das Skript aus dem vorherigen Abschnitt](#powershell-script-for-scoped-synchronization) aus, und verwenden Sie den Parameter *-groupsToAdd*, um die Liste der zu synchronisierenden Gruppen zu übergeben.

   > [!WARNING]
   > Sie müssen die Gruppe *AAD DC-Administratoren* in die Liste der Gruppen für die bereichsbezogene Synchronisierung aufnehmen. Wenn Sie diese Gruppe nicht einbeziehen, ist die verwaltete Azure AD DS-Domäne nicht verwendbar.

   ```powershell
   .\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName2")
   ```

1. Erstellen Sie jetzt die verwaltete Azure AD DS-Domäne, und aktivieren Sie die gruppenbasierte bereichsbezogene Synchronisierung. Nehmen Sie *"filteredSync" = "Enabled"* in den Parameter *-Properties* auf.

    Legen Sie Ihre Azure-Abonnement-ID fest, und geben Sie dann einen Namen für die verwaltete Domäne an, z.B. *contoso.com*. Die ID Ihres Abonnements können Sie mithilfe des Cmdlets [Get-AzSubscription][Get-AzSubscription] abrufen. Legen Sie den Namen der Ressourcengruppe, den Namen des virtuellen Netzwerks und die Region auf die Werte fest, die in den vorherigen Schritten zum Erstellen der unterstützenden Azure-Ressourcen verwendet wurden:

   ```powershell
   $AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
   $ManagedDomainName = "contoso.com"
   $ResourceGroupName = "myResourceGroup"
   $VnetName = "myVnet"
   $AzureLocation = "westus"

   # Enable Azure AD Domain Services for the directory.
   New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
   -Location $AzureLocation `
   -Properties @{"DomainName"=$ManagedDomainName; "filteredSync" = "Enabled"; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
   -Force -Verbose
   ```

Es dauert einige Minuten, bis die Ressource erstellt und die Steuerung an die PowerShell-Eingabeaufforderung zurückgegeben wird. Im Hintergrund wird weiterhin die verwaltete Azure AD DS-Domäne bereitgestellt, und es kann bis zu einer Stunde dauern, bis die Bereitstellung beendet ist. Während dieser Bereitstellungsphase wird im Azure-Portal auf der Seite **Übersicht** für Ihre verwaltete Azure AD DS-Domäne der aktuelle Status angezeigt.

Wenn im Azure-Portal angezeigt wird, dass die verwaltete Azure AD DS-Domäne die Bereitstellung abgeschlossen hat, müssen Sie die folgenden Aufgaben ausführen:

* Aktualisieren Sie DNS-Einstellungen für das virtuelle Netzwerk, damit virtuelle Computer die verwaltete Domäne für den Domänenbeitritt oder die Domänenauthentifizierung finden können.
    * Wählen Sie zum Konfigurieren von DNS Ihre verwaltete Azure AD DS-Domäne im Portal aus. Im Fenster **Übersicht** werden Sie aufgefordert, diese DNS-Einstellungen automatisch zu konfigurieren.
* [Aktivieren Sie die Kennwortsynchronisierung für Azure AD Domain Services](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds), damit sich Endbenutzer mit ihren Unternehmensanmeldeinformationen bei der verwalteten Domäne anmelden können.

## <a name="modify-scoped-synchronization-using-powershell"></a>Ändern der bereichsbezogenen Synchronisierung mit PowerShell

Um die Liste der Gruppen zu ändern, deren Benutzer mit der verwalteten Azure AD DS-Domäne synchronisiert werden sollen, führen Sie erneut das [PowerShell-Skript](#powershell-script-for-scoped-synchronization) aus, und geben Sie die neue Liste der Gruppen an. Im folgenden Beispiel enthält die Liste der zu synchronisierenden Gruppen nicht mehr *GroupName2*, dafür aber *GroupName3*.

> [!WARNING]
> Sie müssen die Gruppe *AAD DC-Administratoren* in die Liste der Gruppen für die bereichsbezogene Synchronisierung aufnehmen. Wenn Sie diese Gruppe nicht einbeziehen, ist die verwaltete Azure AD DS-Domäne nicht verwendbar.

```powershell
.\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName3")
```

Wenn Sie den Bereich der Synchronisierung ändern, werden zwangsläufig alle Daten von der verwalteten Azure AD DS-Domäne neu synchronisiert. Objekte, die in der verwalteten Azure AD DS-Domäne nicht mehr benötigt werden, werden gelöscht, und die erneute Synchronisierung kann einige Zeit in Anspruch nehmen.

## <a name="disable-scoped-synchronization-using-powershell"></a>Deaktivieren der bereichsbezogenen Synchronisierung mit PowerShell

Legen Sie zum Deaktivieren der gruppenbasierten bereichsbezogenen Synchronisierung für eine verwaltete Azure AD DS-Domäne *"filteredsync" = "Disabled"* für die Azure AD DS-Ressource fest, und aktualisieren Sie dann die verwaltete Domäne. Nach Abschluss des Vorgangs ist für alle Benutzer und Gruppen die Synchronisierung von Azure AD festgelegt.

```powershell
// Retrieve the Azure AD DS resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

// Update the Azure AD DS resource
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

Wenn Sie den Bereich der Synchronisierung ändern, werden zwangsläufig alle Daten von der verwalteten Azure AD DS-Domäne neu synchronisiert. Objekte, die in der verwalteten Azure AD DS-Domäne nicht mehr benötigt werden, werden gelöscht, und die erneute Synchronisierung kann einige Zeit in Anspruch nehmen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Synchronisierungsvorgang finden Sie unter [Grundlegendes zur Synchronisierung in Azure AD Domain Services.](synchronization.md)

<!-- EXTERNAL LINKS -->
[Get-AzSubscription]: /powershell/module/Az.Accounts/Get-AzSubscription
