---
title: Senden einer E-Mail aus einem Azure Automation-Runbook
description: Hier erfahren Sie, wie Sie mithilfe von SendGrid eine E-Mail aus einem Runbook senden.
services: automation
ms.service: automation
ms.subservice: process-automation
author: bobbytreed
ms.author: robreed
ms.date: 07/15/2019
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: ce05aadb53cc3ad24ed65ea139594010e1aef047
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68235091"
---
# <a name="tutorial-send-an-email-from-an-azure-automation-runbook"></a>Tutorial: Senden einer E-Mail aus einem Azure Automation-Runbook

Sie können unter Verwendung von PowerShell und [SendGrid](https://sendgrid.com/solutions) eine E-Mail aus einem Runbook senden. In diesem Tutorial erfahren Sie, wie Sie ein wiederverwendbares Runbook erstellen, das mithilfe eines in [Azure KeyVault](/azure/key-vault/) gespeicherten API-Schlüssels eine E-Mail sendet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Erstellen einer Azure Key Vault-Instanz
> * Speichern Ihres SendGrid-API-Schlüssels in Key Vault
> * Erstellen eines Runbooks, das Ihren API-Schlüssel abruft und eine E-Mail sendet

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorials müssen folgende Voraussetzungen erfüllt sein:

* Azure-Abonnement: Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Erstellen eines SendGrid-Kontos](/azure/sendgrid-dotnet-how-to-send-email#create-a-sendgrid-account)
* [Automation-Konto](automation-offering-get-started.md) mit **Az**-Modulen und [ausführender Verbindung](automation-create-runas-account.md) zum Speichern und Ausführen des Runbooks

## <a name="create-an-azure-keyvault"></a>Erstellen einer Azure Key Vault-Instanz

Sie können eine Azure Key Vault-Instanz mit dem folgenden PowerShell-Skript erstellen. Ersetzen Sie die Variablenwerte durch die Werte für Ihre Umgebung. Verwenden Sie die eingebettete Azure Cloud Shell-Instanz, indem Sie die Schaltfläche <kbd>Ausprobieren</kbd> in der oberen rechten Ecke des Codeblocks nutzen. Sie können den Code auch lokal kopieren und ausführen, wenn das [Azure PowerShell-Modul](/powershell/azure/install-az-ps) auf dem lokalen Computer installiert ist.

> [!NOTE]
> Führen Sie zum Abrufen Ihres API-Schlüssels die Schritte unter [Senden von E-Mails in Azure mit SendGrid](/azure/sendgrid-dotnet-how-to-send-email#to-find-your-sendgrid-api-key) aus.

```azurepowershell-interactive
$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
# If you omit the SubscriptionId parameter, the default subscription is selected.
Connect-AzAccount -SubscriptionId $SubscriptionId

# Use Get-AzLocation to see your available locations.
$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "<Enter a universally unique vault name>"
$SendGridAPIKey = "<SendGrid API key>"
$AutomationAccountName = "testaa"

# Create new Resource Group, or omit this step if you already have a resource group.
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName -Location $region
$resourceId = $newKeyVault.ResourceId

# Convert the SendGrid API key into a SecureString
$Secret = ConvertTo-SecureString -String $SendGridAPIKey -AsPlainText -Force
Set-AzKeyVaultSecret -VaultName $VaultName -Name 'SendGridAPIKey' -SecretValue $Secret

# Grant access to the KeyVault to the Automation RunAs account.
$connection = Get-AzAutomationConnection -ResourceGroupName $KeyVaultResourceGroupName -AutomationAccountName $AutomationAccountName -Name AzureRunAsConnection
$appID = $connection.FieldDefinitionValues.ApplicationId
Set-AzKeyVaultAccessPolicy -VaultName $VaultName -ServicePrincipalName $appID -PermissionsToSecrets Set, Get
```

Weitere Möglichkeiten zum Erstellen einer Azure Key Vault-Instanz und zum Speichern eines Geheimnisses finden Sie in der [Dokumentation zu Key Vault](/azure/key-vault/).

## <a name="import-required-modules-to-your-automation-account"></a>Importieren erforderlicher Module in Ihr Automation-Konto

Für die Verwendung von Azure Key Vault innerhalb eines Runbooks benötigt Ihr Automation-Konto die folgenden Module:

* [Az.Profile](https://www.powershellgallery.com/packages/Az.Profile)
* [Az.KeyVault](https://www.powershellgallery.com/packages/Az.KeyVault)

Klicken Sie auf der Registerkarte „Azure Automation“ unter „Installationsoptionen“ auf <kbd>Deploy to Azure Automation</kbd> (In Azure Automation bereitstellen). Das Azure-Portal wird geöffnet. Wählen Sie auf der Seite „Importieren“ Ihr Automation-Konto aus, und klicken Sie auf <kbd>OK</kbd>.

Weitere Methoden zum Hinzufügen der erforderlichen Module finden Sie unter [Verwenden von Modulen in Azure Automation](/azure/automation/shared-resources/modules#import-modules).

## <a name="create-the-runbook-to-send-an-email"></a>Erstellen des Runbooks zum Senden einer E-Mail

Wenn Sie eine Key Vault-Instanz erstellt und Ihren SendGrid-API-Schlüssel gespeichert haben, können Sie das Runbook erstellen, das den API-Schlüssel abruft und eine E-Mail sendet.

Dieses Runbook verwendet das [ausführende Konto](automation-create-runas-account.md) „AzureRunAsConnection“ für die Authentifizierung bei Azure, um das Geheimnis aus Azure Key Vault abzurufen.

Verwenden Sie dieses Beispiel, um ein Runbook mit dem Namen **Send-GridMailMessage** zu erstellen. Sie können das PowerShell-Skript ändern und für verschiedene Szenarien wiederverwenden.

1. Wechseln Sie zu Ihrem Azure Automation-Konto.
2. Wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** aus.
3. Wählen Sie oben in der Liste mit den Runbooks die Option **+ Runbook erstellen** aus.
4. Geben Sie auf der Seite **Runbook hinzufügen** den Namen **Send-GridMailMessage** für das Runbook ein. Wählen Sie als Runbooktyp **PowerShell** aus. Wählen Sie anschließend **Erstellen**.
   ![Erstellen eines Runbooks](./media/automation-send-email/automation-send-email-runbook.png)
5. Das Runbook wird erstellt, und die Seite **PowerShell-Runbook bearbeiten** wird geöffnet.
   ![Bearbeiten des Runbooks](./media/automation-send-email/automation-send-email-edit.png)
6. Kopieren Sie das folgende PowerShell-Beispiel auf die Seite **Bearbeiten**. Stellen Sie sicher, dass `$VaultName` der Name ist, den Sie beim Erstellen der Key Vault-Instanz angegeben haben.

    ```powershell-interactive
    Param(
      [Parameter(Mandatory=$True)]
      [String] $destEmailAddress,
      [Parameter(Mandatory=$True)]
      [String] $fromEmailAddress,
      [Parameter(Mandatory=$True)]
      [String] $subject,
      [Parameter(Mandatory=$True)]
      [String] $content
    )

    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Out-Null
    $VaultName = "<Enter your vault name>"
    $SENDGRID_API_KEY = (Get-AzKeyVaultSecret -VaultName $VaultName -Name "SendGridAPIKey").SecretValueText
    $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
    $headers.Add("Authorization", "Bearer " + $SENDGRID_API_KEY)
    $headers.Add("Content-Type", "application/json")

    $body = @{
    personalizations = @(
        @{
            to = @(
                    @{
                        email = $destEmailAddress
                    }
            )
        }
    )
    from = @{
        email = $fromEmailAddress
    }
    subject = $subject
    content = @(
        @{
            type = "text/plain"
            value = $content
        }
    )
    }

    $bodyJson = $body | ConvertTo-Json -Depth 4

    $response = Invoke-RestMethod -Uri https://api.sendgrid.com/v3/mail/send -Method Post -Headers $headers -Body $bodyJson
    ```

7. Wählen Sie die Option **Veröffentlichen**, um das Runbook zu speichern und zu veröffentlichen.

Führen Sie die Schritte unter [Verwalten von Runbooks in Azure Automation](manage-runbooks.md#test-a-runbook) oder [Starten eines Runbooks in Azure Automation](start-runbooks.md) aus, um zu überprüfen, ob das Runbook ausgeführt wird.
Wird die Test-E-Mail zuerst nicht angezeigt, überprüfen Sie den **Junk-** und **Spam**-Ordner.

## <a name="clean-up"></a>Bereinigen

Löschen Sie das Runbook, falls es nicht mehr benötigt wird. Wählen Sie das Runbook hierzu in der Runbookliste aus, und klicken Sie auf **Löschen**.

Löschen Sie den Schlüsseltresor mithilfe des Cmdlets [Remove-AzureRMKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault?view=azurermps).

```azurepowershell-interactive
$VaultName = "<your KeyVault name>"
$ResourceGroupName = "<your ResourceGroup name>"
Remove-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $ResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Problemen beim Erstellen oder Starten des Runbooks finden Sie unter [Beheben von Fehlern bei Runbooks](./troubleshoot/runbooks.md).
* Informationen zum Aktualisieren von Modulen in Ihrem Automation-Konto finden Sie unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md).
* Informationen zum Überwachen der Runbookausführung finden Sie unter [Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Azure Monitor-Protokolle](automation-manage-send-joblogs-log-analytics.md).
* Informationen zum Auslösen eines Runbooks mithilfe einer Warnung finden Sie unter [Verwenden einer Warnung zum Auslösen eines Azure Automation-Runbooks](automation-create-alert-triggered-runbook.md).
