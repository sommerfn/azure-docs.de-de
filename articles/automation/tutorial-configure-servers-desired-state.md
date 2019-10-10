---
title: Konfigurieren von Servern auf einen gewünschten Status und Verwalten der Abweichung mit Azure Automation
description: 'Tutorial: Verwalten von Serverkonfigurationen mit der Azure Automation-Zustandskonfiguration'
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
manager: carmonm
ms.topic: conceptual
ms.date: 08/08/2018
ms.openlocfilehash: 09ba4bc9e5ac496a7d1d65ff145d56818e53116e
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243337"
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Konfigurieren von Servern mit einem gewünschten Status und Verwalten der Abweichung mit Azure Automation

Mit der Azure Automation-Zustandskonfiguration können Sie Konfigurationen für Ihre Server festlegen und sicherstellen, dass sich diese Server im Verlauf der Zeit im angegebenen Zustand befinden.

> [!div class="checklist"]
> - Integrieren eines virtuellen Computers in die Azure Automation DSC-Verwaltung
> - Hochladen einer Konfiguration in Azure Automation
> - Kompilieren einer Konfiguration in eine Knotenkonfiguration
> - Zuweisen einer Knotenkonfiguration zu einem verwalteten Knoten
> - Überprüfen des Konformitätsstatus eines verwalteten Knotens

## <a name="prerequisites"></a>Voraussetzungen

Um dieses Lernprogramm abzuschließen, benötigen Sie:

- Ein Azure Automation-Konto. Informationen zum Erstellen eines ausführenden Azure Automation-Kontos finden Sie unter [Azure Run As Account](automation-sec-configure-azure-runas-account.md)(Ausführendes Azure-Konto).
- Eine Azure Resource Manager-VM (nicht klassisch) unter Windows Server 2008 R2 oder höher. Eine Anleitung zum Erstellen einer VM finden Sie unter [Erstellen Ihres ersten virtuellen Windows-Computers im Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
- Azure PowerShell-Modul, Version 3.6 oder höher. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/azurerm/install-azurerm-ps) Informationen dazu.
- Erfahrung mit DSC (Desired State Configuration, Konfiguration des gewünschten Zustands). Weitere Informationen zu DSC finden Sie unter [Windows PowerShell DSC – Übersicht](/powershell/scripting/dsc/overview/overviews).

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Connect-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Connect-AzureRmAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Erstellen und Hochladen einer Konfiguration in Azure Automation

Für dieses Tutorial verwenden wir eine einfache DSC-Konfiguration, die sicherstellt, dass IIS auf der VM installiert ist.

Weitere Informationen zu DSC-Konfigurationen finden Sie unter [DSC-Konfigurationen](/powershell/scripting/dsc/configurations/configurations).

Geben Sie in einem Texteditor Folgendes ein, und speichern Sie die Datei lokal als `TestConfig.ps1`.

```powershell
configuration TestConfig {
   Node WebServer {
      WindowsFeature IIS {
         Ensure               = 'Present'
         Name                 = 'Web-Server'
         IncludeAllSubFeature = $true
      }
   }
}
```

> [!NOTE]
> In komplexeren Szenarien, in denen mehrere Module importiert werden müssen, die DSC-Ressourcen bereitstellen, müssen Sie sicherstellen, dass jedes Modul über eine eindeutige Zeile vom Typ `Import-DscResource` in Ihrer Konfiguration verfügt.

Rufen Sie das Cmdlet `Import-AzureRmAutomationDscConfiguration` auf, um die Konfiguration in Ihr Automation-Konto hochzuladen:

```powershell
 Import-AzureRmAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Kompilieren einer Konfiguration in eine Knotenkonfiguration

Eine DSC-Konfiguration muss in eine Knotenkonfiguration kompiliert werden, bevor sie einem Knoten zugewiesen werden kann.

Informationen zum Kompilieren von Konfigurationen finden Sie unter [DSC-Konfigurationen](/powershell/scripting/dsc/configurations/configurations).

Rufen Sie das Cmdlet `Start-AzureRmAutomationDscCompilationJob` auf, um die `TestConfig`-Konfiguration in eine Knotenkonfiguration zu kompilieren:

```powershell
Start-AzureRmAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

Dies erstellt eine Konfiguration mit dem Namen `TestConfig.WebServer` in Ihrem Automation-Konto.

## <a name="register-a-vm-to-be-managed-by-state-configuration"></a>Registrieren eines virtuellen Computers für die Verwaltung durch die Zustandskonfiguration

Mit Azure Automation State Configuration können Sie virtuelle Azure-Computer (mit dem klassischen Modell oder dem Resource Manager-Modell), lokale virtuelle Computer, Linux-Computer, virtuelle AWS-Computer und lokale physische Computer verwalten. In diesem Thema wird das Registrieren von ausschließlich Azure Resource Manager-VMs behandelt. Informationen zum Registrieren anderer Computertypen finden Sie unter [Integrieren von Computern für die Verwaltung durch die Azure Automation-Zustandskonfiguration](automation-dsc-onboarding.md).

Rufen Sie das Cmdlet `Register-AzureRmAutomationDscNode` auf, um Ihren virtuellen Computer bei der Azure Automation-Zustandskonfiguration zu registrieren.

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm'
```

Dadurch wird der angegebene virtuelle Computer als verwalteter Knoten für die Zustandskonfiguration registriert.

### <a name="specify-configuration-mode-settings"></a>Angeben der Konfigurationsmoduseinstellungen

Wenn Sie eine VM als verwalteten Knoten registrieren, können Sie auch Eigenschaften der Konfiguration angeben. Sie können z.B. festlegen, dass der Zustand der VM nur einmalig angewendet werden soll (DSC versucht nicht, die Konfiguration nach der ersten Prüfung zu übernehmen), indem Sie `ApplyOnly` als Wert der **ConfigurationMode**-Eigenschaft angeben:

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationMode 'ApplyOnly'
```

Sie können auch angeben, wie oft DSC den Konfigurationszustand überprüft, indem Sie die **ConfigurationModeFrequencyMins**-Eigenschaft verwenden:

```powershell
# Run a DSC check every 60 minutes
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationModeFrequencyMins 60
```

Weitere Informationen zum Festlegen von Konfigurationseigenschaften für einen verwalteten Knoten finden Sie unter [Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode).

Weitere Informationen zu DSC-Konfigurationseinstellungen finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](/powershell/scripting/dsc/managing-nodes/metaConfig).

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Zuweisen einer Knotenkonfiguration zu einem verwalteten Knoten

Nun können wir die kompilierte Knotenkonfiguration der zu konfigurierenden VM zuordnen.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -NodeId $node.Id
```

Damit wird dem registrierten DSC-Knoten `DscVm` die benannte Knotenkonfiguration `TestConfig.WebServer` zugeordnet.
Standardmäßig wird der DSC-Knoten alle 30 Minuten auf Konformität mit der Knotenkonfiguration geprüft.
Informationen zum Ändern des Intervalls für die Konformitätsprüfung finden Sie unter [Konfigurieren des lokalen Konfigurations-Managers](/powershell/scripting/dsc/managing-nodes/metaConfig).

## <a name="working-with-partial-configurations"></a>Arbeiten mit Teilkonfigurationen

Azure Automation DSC unterstützt die Verwendung von [Teilkonfigurationen](/powershell/dsc/pull-server/partialconfigs).
In diesem Szenario ist DSC so konfiguriert, dass mehrere Konfigurationen unabhängig voneinander verwaltet werden können, wobei jede Konfiguration von Azure Automation abgerufen wird.
Einem Knoten kann aber nur eine Konfiguration mittels Automation-Konto zugewiesen werden.
Dies bedeutet, dass Sie bei Verwendung von zwei Konfigurationen für einen Knoten zwei Automation-Kontos benötigen.

Ausführliche Informationen zum Registrieren einer Teilkonfiguration aus dem Pulldienst finden Sie in der Dokumentation zu [Teilkonfigurationen](https://docs.microsoft.com/powershell/dsc/pull-server/partialconfigs#partial-configurations-in-pull-mode).

Weitere Informationen dazu, wie Teams zusammenarbeiten können, um Server gemeinsam mithilfe von Konfiguration als Code zu verwalten, finden Sie unter [Grundlegendes zu DSCs Rolle in einer CI/CD-Pipeline](/powershell/dsc/overview/authoringadvanced).

## <a name="check-the-compliance-status-of-a-managed-node"></a>Überprüfen des Konformitätsstatus eines verwalteten Knotens

Sie können Berichte zum Konformitätsstatus eines verwalteten Knotens abrufen, indem Sie das Cmdlet `Get-AzureRmAutomationDscNodeReport` aufrufen:

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzureRmAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeId $node.Id

# Display the most recent report
$reports[0]
```

## <a name="removing-nodes-from-service"></a>Entfernen von Knoten aus dem Dienst

Wenn Sie Azure Automation State Configuration einen Knoten hinzufügen, werden die Einstellungen im lokalen Konfigurations-Manager so festgelegt, dass eine Registrierung beim Dienst erfolgt und Konfigurationen sowie erforderliche Module zum Konfigurieren des Computers abgerufen werden.
Wenn Sie den Knoten aus dem Dienst entfernen möchten, können Sie dazu entweder das Azure-Portal oder die Az-Cmdlets verwenden.

> [!NOTE]
> Durch das Aufheben der Registrierung eines Knotens beim Dienst werden die Einstellungen des lokalen Konfigurations-Managers lediglich so festgelegt, dass der Knoten keine Verbindung mehr mit dem Dienst herstellt.
> Dies hat keine Auswirkung auf die Konfiguration, die derzeit auf den Knoten angewendet wird.
> Zum Entfernen der aktuellen Konfiguration verwenden Sie [PowerShell](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/remove-dscconfigurationdocument?view=powershell-5.1), oder löschen Sie die lokale Konfigurationsdatei (dies ist für Linux-Knoten die einzige Option).

### <a name="azure-portal"></a>Azure-Portal

Klicken Sie in Azure Automation im Inhaltsverzeichnis auf **Zustandskonfiguration (DSC)** .
Klicken Sie dann auf **Knoten**, um die Liste der Knoten anzuzeigen, die beim Dienst registriert sind.
Klicken Sie auf den Namen des Knotens, den Sie entfernen möchten.
Klicken Sie in der daraufhin geöffneten Knotenansicht auf **Registrierung aufheben**.

### <a name="powershell"></a>PowerShell

Zum Aufheben der Registrierung eines Knotens beim Azure Automation State Configuration-Dienst mithilfe von PowerShell befolgen Sie die Anweisungen in der Dokumentation für das Cmdlet [Unregister-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-2.0.0).

## <a name="next-steps"></a>Nächste Schritte

- Eine Einführung finden Sie unter [Erste Schritte mit Azure Automation State Configuration](automation-dsc-getting-started.md).
- Weitere Informationen zum Integrieren von Knoten finden Sie unter [Onboarding von Computern zur Verwaltung durch Azure Automation DSC](automation-dsc-onboarding.md).
- Wie Sie DSC-Konfigurationen kompilieren und anschließend Zielknoten zuweisen, erfahren Sie unter [Kompilieren von DSC-Konfigurationen in Azure Automation DSC](automation-dsc-compile.md).
- Eine PowerShell-Cmdlet-Referenz ist unter [Azure Automation State Configuration-Cmdlets](/powershell/module/azurerm.automation/#automation) verfügbar.
- Eine Preisübersicht finden Sie unter [Automation – Preise](https://azure.microsoft.com/pricing/details/automation/).
- Ein Verwendungsbeispiel für Azure Automation State Configuration in einer Continuous Deployment-Pipeline finden Sie unter [Continuous Deployment mit Azure Automation State Configuration und Chocolatey](automation-dsc-cd-chocolatey.md).
