---
title: Konfigurieren von Pre- und Post-Skripts für die Bereitstellung mit der Updateverwaltung in Azure
description: In diesem Artikel erfahren Sie, wie Sie Pre- und Post-Skripts für Updatebereitstellungen konfigurieren und verwalten.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 860eaf5d37b3d3064e3b10bd1dab02c04b95ab5b
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72755509"
---
# <a name="manage-pre-and-post-scripts"></a>Verwalten von Pre- und Post-Skripts

Pre- und Post-Skripts (vor bzw. nach der Bereitstellung auszuführende Skripts) ermöglichen es Ihnen, vor und nach einer Updatebereitstellung (Pre- bzw. Post-Aufgabe) PowerShell-Runbooks in Ihrem Azure Automation-Konto auszuführen. Pre- und Post-Skripts werden nicht lokal, sondern im Azure-Kontext ausgeführt. Pre-Skripts werden zu Beginn der Updatebereitstellung ausgeführt. Post-Skripts werden am Ende der Bereitstellung sowie nach allen konfigurierten Neustarts ausgeführt.

## <a name="runbook-requirements"></a>Anforderungen für Runbooks

Damit ein Runbook als Pre- oder Post-Skript verwendet werden kann, muss es in Ihr Automation-Konto importiert und veröffentlicht werden. Weitere Informationen zu diesem Prozess finden Sie unter [Veröffentlichen eines Runbooks](manage-runbooks.md#publish-a-runbook).

## <a name="using-a-pre-script-or-post-script"></a>Verwenden eines Pre- oder Post-Skripts

Wenn Sie ein Pre- oder Post-Skript in einer Updatebereitstellung verwenden möchten, müssen Sie zunächst eine Updatebereitstellung erstellen. Wählen Sie **Vor und nach dem Vorgang auszuführende Skripts** aus. Daraufhin wird die Seite **Vor und nach dem Vorgang auszuführende Skripts auswählen** angezeigt.

![Auswählen der Skripts](./media/pre-post-scripts/select-scripts.png)

Wählen Sie das Skript aus, das Sie verwenden möchten. In diesem Beispiel verwenden wir das Runbook **UpdateManagement-TurnOnVms**. Wenn Sie das Runbook auswählen, wird die Seite **Skript konfigurieren** geöffnet. Wählen Sie **Skript vor Vorgang** und anschließend **OK** aus.

Wiederholen Sie diese Schritte für das Skript **UpdateManagement-TurnOffVms**. Wählen Sie diesmal für **Skripttyp** jedoch **Skript nach Vorgang** aus.

Im Abschnitt **Ausgewählte Elemente** werden nun die beiden ausgewählten Skripts angezeigt. Eines ist ein Pre-Skript, das andere ein Post-Skript:

![Ausgewählte Elemente](./media/pre-post-scripts/selected-items.png)

Schließen Sie die Konfiguration der Updatebereitstellung ab.

Nach Abschluss der Updatebereitstellung können Sie unter **Updatebereitstellungen** die Ergebnisse anzeigen. Wie Sie sehen, wird der Status des Pre- und Post-Skripts angezeigt:

![Updateergebnisse](./media/pre-post-scripts/update-results.png)

Wenn Sie die Updatebereitstellungsausführung auswählen, werden weitere Details zu den Pre- und Post-Skripts angezeigt. Dort finden Sie auch einen Link zur Skriptquelle zum Zeitpunkt der Ausführung.

![Ergebnisse der Bereitstellungsausführung](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>Übergeben von Parametern

Beim Konfigurieren von Pre- und Post-Skripts können Sie genau wie beim Planen eines Runbooks Parameter übergeben. Parameter werden beim Erstellen der Updatebereitstellung definiert. Pre- und Post-Skripts unterstützen folgende Typen:

* [char]
* [byte]
* [int]
* [long]
* [decimal]
* [single]
* [double]
* [DateTime]
* [string]

Wenn Sie einen anderen Objekttyp benötigen, können Sie eine Typumwandlung in einen anderen Typ mit Ihrer eigenen Logik im Runbook durchführen.

Neben den Standardrunbookparametern ist noch ein weiterer Parameter verfügbar: **SoftwareUpdateConfigurationRunContext**.

Dieser Parameter ist eine JSON-Zeichenfolge, und wenn Sie ihn in Ihrem Pre- oder Post-Skript definieren, wird er automatisch von der Updatebereitstellung übergeben. Der Parameter enthält Informationen zur Updatebereitstellung, bei denen es sich um eine Teilmenge der von der [SoftwareUpdateconfigurations-API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration) zurückgegebenen Informationen handelt. 

Die folgende Tabelle enthält die in der Variablen bereitgestellten Eigenschaften:

### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext-Eigenschaften

|Eigenschaft  |BESCHREIBUNG  |
|---------|---------|
|SoftwareUpdateConfigurationName     | Der Name der Softwareupdatekonfiguration.        |
|SoftwareUpdateConfigurationRunId     | Die eindeutige ID für die Ausführung        |
|SoftwareUpdateConfigurationSettings     | Eine Sammlung von Eigenschaften im Zusammenhang mit der Softwareupdatekonfiguration.         |
|SoftwareUpdateConfigurationSettings.operatingSystem     | Die Zielbetriebssysteme für die Updatebereitstellung.         |
|SoftwareUpdateConfigurationSettings.duration     | Die maximale Dauer der Updatebereitstellungsausführung im Format `PT[n]H[n]M[n]S` gemäß ISO 8601 (das so genannte *Wartungsfenster*).          |
|SoftwareUpdateConfigurationSettings.Windows     | Eine Sammlung von Eigenschaften im Zusammenhang mit Windows-Computern.         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | Eine Liste mit KBs, die von der Bereitstellung ausgeschlossen werden.        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | Für die Updatebereitstellung ausgewählte Updateklassifizierungen.        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | Neustarteinstellungen für die Updatebereitstellung.        |
|azureVirtualMachines     | Eine Liste mit Ressourcen-IDs für die virtuellen Azure-Computer in der Updatebereitstellung.        |
|nonAzureComputerNames|Eine Liste mit den FQDNs der Azure-fremden Computer in der Updatebereitstellung.|

Das folgende Beispiel ist eine JSON-Zeichenfolge, die an den **SoftwareUpdateConfigurationRunContext**-Parameter übergeben wird:

```json
"SoftwareUpdateConfigurationRunContext":{
      "SoftwareUpdateConfigurationName":"sampleConfiguration",
      "SoftwareUpdateConfigurationRunId":"00000000-0000-0000-0000-000000000000",
      "SoftwareUpdateConfigurationSettings":{
         "operatingSystem":"Windows",
         "duration":"PT2H0M",
         "windows":{
            "excludedKbNumbers":[
               "168934",
               "168973"
            ],
            "includedUpdateClassifications":"Critical",
            "rebootSetting":"IfRequired"
         },
         "azureVirtualMachines":[
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-01",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-02",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-03"
         ],
         "nonAzureComputerNames":[
            "box1.contoso.com",
            "box2.contoso.com"
         ]
      }
   }
```

Ein vollständiges Beispiel mit allen Eigenschaften finden Sie unter: [Abrufen der Softwareupdatekonfiguration anhand des Namens](/rest/api/automation/softwareupdateconfigurations/getbyname#examples).

> [!NOTE]
> Das `SoftwareUpdateConfigurationRunContext`-Objekt kann doppelte Einträge für Computer enthalten. Dies kann dazu führen, dass Pre- und Post-Skripts mehrmals auf dem gleichen Computer ausgeführt werden. Verwenden Sie zur Umgehung dieses Verhaltens `Sort-Object -Unique`, um nur eindeutige VM-Namen in Ihrem Skript auszuwählen.


## <a name="stopping-a-deployment"></a>Beenden einer Bereitstellung

Wenn Sie eine Bereitstellung basierend auf einem vorbereitenden Skript beenden möchten, müssen Sie eine Ausnahme [auslösen](automation-runbook-execution.md#throw). Andernfalls werden die Bereitstellung und das Post-Skript trotzdem ausgeführt. Der folgende Codeausschnitt veranschaulicht das Auslösen einer Ausnahme:

```powershell
#In this case, we want to terminate the patch job if any run fails.
#This logic might not hold for all cases - you might want to allow success as long as at least 1 run succeeds
foreach($summary in $finalStatus)
{
    if ($summary.Type -eq "Error")
    {
        #We must throw in order to fail the patch deployment.
        throw $summary.Summary
    }
}
```

## <a name="samples"></a>Beispiele

Beispiele für Pre- und Post-Skripts können aus dem [Script Center-Katalog](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell) und aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages?q=Tags%3A%22UpdateManagement%22+Tags%3A%22Automation%22) heruntergeladen oder über das Azure-Portal importiert werden. Wählen Sie hierzu in Ihrem Automation-Konto unter **Prozessautomatisierung** die Option **Runbookkatalog** aus. Wählen Sie **Updateverwaltung** als Filter aus.

![Katalogliste](./media/pre-post-scripts/runbook-gallery.png)

Alternativ können Sie anhand der Namen in der folgenden Liste nach den Skripts suchen:

* Update Management – Turn On VMs (Updateverwaltung – VMs aktivieren)
* Update Management – Turn Off VMs (Updateverwaltung – VMs deaktivieren)
* Update Management – Run Script Locally (Updateverwaltung – Skript lokal ausführen)
* Update Management – Template for Pre/Post Scripts (Updateverwaltung – Vorlage für Pre-/Post-Skripts)
* Update Management – Run Script with Run Command (Updateverwaltung – Skript mit Ausführungsbefehl ausführen)

> [!IMPORTANT]
> Nachdem Sie die Runbooks importiert haben, müssen Sie sie veröffentlichen, um sie verwenden zu können. Suchen Sie hierzu in Ihrem Automation-Konto nach dem gewünschten Runbook, und wählen Sie **Bearbeiten** > **Veröffentlichen** aus.

Die Beispiele basieren alle auf der einfachen Vorlage, die im folgenden Beispiel definiert ist. Mit dieser Vorlage können Sie Ihr eigenes Runbook für die Verwendung mit Pre- und Post-Skripts erstellen. Die erforderliche Logik für die Authentifizierung bei Azure und die Behandlung des Parameters `SoftwareUpdateConfigurationRunContext` ist in der Vorlage enthalten.

```powershell
<#
.SYNOPSIS
 Barebones script for Update Management Pre/Post

.DESCRIPTION
  This script is intended to be run as a part of Update Management pre/post-scripts.
  It requires a RunAs account.

.PARAMETER SoftwareUpdateConfigurationRunContext
  This is a system variable which is automatically passed in by Update Management during a deployment.
#>

param(
    [string]$SoftwareUpdateConfigurationRunContext
)
#region BoilerplateAuthentication
#This requires a RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID
#endregion BoilerplateAuthentication

#If you wish to use the run context, it must be converted from JSON
$context = ConvertFrom-Json  $SoftwareUpdateConfigurationRunContext
#Access the properties of the SoftwareUpdateConfigurationRunContext
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines | Sort-Object -Unique
$runId = $context.SoftwareUpdateConfigurationRunId

Write-Output $context

#Example: How to create and write to a variable using the pre-script:
<#
#Create variable named after this run so it can be retrieved
New-AzureRmAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false
#Set value of variable
Set-AutomationVariable –Name $runId -Value $vmIds
#>

#Example: How to retrieve information from a variable set during the pre-script
<#
$variable = Get-AutomationVariable -Name $runId
#>
```

## <a name="interacting-with-machines"></a>Interaktion mit Computern

Pre- und Post-Aufgaben werden nicht direkt auf den Computern in Ihrer Bereitstellung, sondern als Runbook in Ihrem Automation-Konto ausgeführt. Pre- und Post-Aufgaben werden auch im Azure-Kontext ausgeführt und haben keinen Zugriff auf Azure-fremde Computer. In den folgenden Abschnitten erfahren Sie, wie Sie direkt mit den Computern interagieren. Dabei spielt es keine Rolle, ob es sich um virtuelle Azure-Computer oder um Azure-fremde Computer handelt.

### <a name="interacting-with-azure-machines"></a>Interaktion mit Azure-Computern

Pre- und Post-Aufgaben werden nicht nativ auf Ihren virtuellen Azure-Computern in Ihrer Bereitstellung, sondern als Runbooks ausgeführt. Für die Interaktion mit den Azure-VMs benötigen Sie die folgenden Elemente:

* Ein ausführendes Konto
* Ein Runbook, das Sie ausführen möchten

Um mit Azure-Computern interagieren zu können, sollten Sie das Cmdlet [Invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) für die Interaktion mit Ihren Azure-VMs verwenden. Ein Beispiel dafür finden Sie im Runbookbeispiel [Updateverwaltung: Ausführen eines Skripts mit dem Befehl „run“](https://gallery.technet.microsoft.com/Update-Management-Run-40f470dc).

### <a name="interacting-with-non-azure-machines"></a>Interagieren mit Azure-fremden Computern

Pre- und Post-Aufgaben werden im Azure-Kontext ausgeführt und haben keinen Zugriff auf Azure-fremde Computer. Für die Interaktion mit den Azure-fremden Computern benötigen Sie Folgendes:

* Ein ausführendes Konto
* Einen auf dem Computer installierten Hybrid Runbook Worker
* Ein Runbook, das Sie lokal ausführen möchten
* Ein übergeordnetes Runbook

Für die Interaktion mit Azure-fremden Computern wird ein übergeordnetes Runbook im Azure-Kontext ausgeführt. Dieses Runbook ruft ein untergeordnetes Runbook mit dem Cmdlet [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) auf. Sie müssen den `-RunOn`-Parameter und den Namen des Hybrid Runbook Workers, auf dem das Skript ausgeführt werden soll, angeben. Weitere Informationen finden Sie im Runbookbeispiel [Updateverwaltung: Lokales Ausführen eines Skripts](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44).

## <a name="abort-patch-deployment"></a>Abbrechen der Bereitstellung von Patches

Falls Ihr Pre-Skript einen Fehler zurückgibt, empfiehlt es sich ggf., die Bereitstellung abzubrechen. Dazu müssen Sie in Ihrem Skript für jegliche Logik, die einen Fehler zur Folge hätte, einen Fehler [auslösen](/powershell/module/microsoft.powershell.core/about/about_throw).

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.
    throw "There was an error, abort deployment"
}
```

## <a name="known-issues"></a>Bekannte Probleme

* Bei Verwendung von Pre- und Post-Skripts dürfen keine Objekte, Arrays oder booleschen Werte an Parameter übergeben werden. Andernfalls tritt ein Runbookfehler auf. Eine Liste mit allen unterstützten Typen finden Sie unter [Übergeben von Parametern](#passing-parameters).

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Tutorial erfahren Sie, wie Sie Updates für Ihre virtuellen Windows-Computer verwalten:

> [!div class="nextstepaction"]
> [Verwalten von Updates und Patches für Ihre virtuellen Azure Windows-Computer](automation-tutorial-update-management.md)

