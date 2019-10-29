---
title: Verwenden von Azure Key Vault in einer Resource Manager-Vorlage
description: Hier erfahren Sie, wie Sie bei der Resource Manager-Vorlagenbereitstellung sichere Parameterwerte mithilfe von Azure Key Vault übergeben.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 05/23/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 5dfc186e5d047de76e16ec145f5f0afe94b8d2f4
ms.sourcegitcommit: 6eecb9a71f8d69851bc962e2751971fccf29557f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72533598"
---
# <a name="tutorial-integrate-azure-key-vault-in-your-resource-manager-template-deployment"></a>Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung

Erfahren Sie, wie Sie bei der Bereitstellung von Azure Resource Manager Geheimnisse aus einer Azure Key Vault-Instanz abrufen und als Parameter übergeben. Der Parameterwert wird nie verfügbar gemacht, da lediglich die Key Vault-ID als Verweis verwendet wird. Weitere Informationen finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](./resource-manager-keyvault-parameter.md).

Im Tutorial [Festlegen der Reihenfolge für die Ressourcenbereitstellung](./resource-manager-tutorial-create-templates-with-dependent-resources.md) erstellen Sie einen virtuellen Computer (VM). Sie müssen den Administratorbenutzernamen und das Kennwort für den virtuellen Computer angeben. Anstatt das Kennwort einzugeben, können Sie es in Azure Key Vault vorab speichern und die Vorlage anschließend anpassen, um das Kennwort während der Bereitstellung aus dem Schlüsseltresor abzurufen.

![Abbildung: Integration einer Resource Manager-Vorlage in einem Schlüsseltresor](./media/resource-manager-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Vorbereiten eines Schlüsseltresors
> * Öffnen einer Schnellstartvorlage
> * Bearbeiten der Parameterdatei
> * Bereitstellen der Vorlage
> * Überprüfen der Bereitstellung
> * Bereinigen von Ressourcen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der Erweiterung [Azure Resource Manager-Tools](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Verwenden Sie aus Sicherheitsgründen ein generiertes Kennwort für das Administratorkonto des virtuellen Computers. Hier sehen Sie ein Beispiel für die Kennwortgenerierung:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Vergewissern Sie sich, dass das generierte Kennwort die Kennwortanforderungen für den virtuellen Computer erfüllt. Jeder Azure-Dienst hat bestimmte Kennwortanforderungen. Informationen zu den Kennwortanforderungen für virtuelle Computer finden Sie unter [Welche Anforderungen an das Kennwort gelten beim Erstellen eines virtuellen Computers?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="prepare-a-key-vault"></a>Vorbereiten eines Schlüsseltresors

In diesem Abschnitt erstellen Sie einen Schlüsseltresor und fügen ihm ein Geheimnis hinzu, damit Sie dieses beim Bereitstellen der Vorlage abrufen können. Es gibt zahlreiche Möglichkeiten zum Erstellen eines Schlüsseltresors. In diesem Tutorial verwenden Sie Azure PowerShell, um eine [Resource Manager-Vorlage](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json) bereitstellen. Mit dieser Vorlage werden die folgenden Aktionen ausgeführt:

* Erstellen eines Schlüsseltresors mit aktivierter `enabledForTemplateDeployment`-Eigenschaft. Diese Eigenschaft muss auf *true* festgelegt sein, damit der Vorlagenbereitstellungsprozess auf die im Schlüsseltresor definierten Geheimnisse zugreifen kann.
* Hinzufügen eines Geheimnisses zum Schlüsseltresor. Das Geheimnis enthält das Administratorkennwort für den virtuellen Computer.

> [!NOTE]
> Wenn Sie als Benutzer, der die Vorlage für virtuelle Maschinen bereitstellt, nicht Besitzer oder ein Mitwirkender für den Schlüsseltresor sind, muss der Besitzer oder ein Mitwirkender Ihnen Zugriff auf die Berechtigung *Microsoft.KeyVault/vaults/deploy/action* für den Schlüsseltresor gewähren. Weitere Informationen finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](./resource-manager-keyvault-parameter.md).

Wählen Sie zum Ausführen des folgenden Azure PowerShell-Skripts die Option **Testen Sie es.** aus, um Azure Cloud Shell zu öffnen. Klicken Sie zum Einfügen des Skripts mit der rechten Maustaste auf den Shellbereich, und wählen Sie **Einfügen** aus.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$upn = Read-Host -Prompt "Enter your user principal name (email address) used to sign in to Azure"
$secretValue = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$keyVaultName = $projectName
$adUserId = (Get-AzADUser -UserPrincipalName $upn).Id
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -keyVaultName $keyVaultName -adUserId $adUserId -secretValue $secretValue
```

> [!IMPORTANT]
> * Der Ressourcengruppenname entspricht dem Projektnamen mit dem Zusatz **rg**. Um das [Bereinigen der in diesem Tutorial erstellten Ressourcen](#clean-up-resources) zu vereinfachen, verwenden Sie beim [Bereitstellen der nächsten Vorlage](#deploy-the-template) den gleichen Projekt- und Ressourcengruppennamen.
> * Der Standardname für das Geheimnis lautet **vmAdminPassword**. Er ist in der Vorlage hartcodiert.
> * Damit die Vorlage das Geheimnis abrufen kann, müssen Sie die Zugriffsrichtlinie „Zugriff auf Azure Resource Manager für Vorlagenbereitstellung aktivieren“ für den Schlüsseltresor aktivieren. Diese Richtlinie ist in der Vorlage aktiviert. Weitere Informationen zu dieser Zugriffsrichtlinie finden Sie unter [Bereitstellen von Schlüsseltresoren und Geheimnissen](./resource-manager-keyvault-parameter.md#deploy-key-vaults-and-secrets).

Die Vorlage enthält einen Ausgabewert mit dem Namen *keyVaultId*. Notieren Sie sich den ID-Wert zur späteren Verwendung, wenn Sie den virtuellen Computer bereitstellen. Die Ressourcen-ID hat das folgende Format:

```json
/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
```

Beim Kopieren und Einfügen der ID wird diese möglicherweise auf mehrere Zeilen aufgeteilt. Führen Sie die Zeilen zusammen, und löschen Sie die zusätzlichen Leerzeichen.

Führen Sie zum Überprüfen der Bereitstellung den folgenden PowerShell-Befehl im gleichen Shellbereich aus, um das Geheimnis als Klartext abzurufen. Der Befehl funktioniert nur in der gleichen Shellsitzung, da er die Variable *$keyVaultName* verwendet, die im vorherigen PowerShell-Skript definiert wurde.

```azurepowershell
(Get-AzKeyVaultSecret -vaultName $keyVaultName  -name "vmAdminPassword").SecretValueText
```

Damit haben Sie einen Schlüsseltresor und ein Geheimnis vorbereitet. In den folgenden Abschnitten wird beschrieben, wie Sie eine vorhandene Vorlage anpassen, um das Geheimnis während der Bereitstellung abzurufen.

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

Azure-Schnellstartvorlagen ist ein Repository für Resource Manager-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in diesem Tutorial verwendete Vorlage heißt [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Bereitstellen eines einfachen virtuellen Windows-Computers).

1. Wählen Sie in Visual Studio Code **Datei** > **Datei öffnen** aus.

1. Fügen Sie im Feld **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Wählen Sie **Öffnen** aus, um die Datei zu öffnen. Das Szenario entspricht dem verwendeten Szenario unter [Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen](./resource-manager-tutorial-create-templates-with-dependent-resources.md) verwendet haben.
   Die Vorlage definiert fünf Ressourcen:

   * `Microsoft.Storage/storageAccounts`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Informationen finden Sie in der [Vorlagenreferenz](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

   Bevor Sie die Vorlage anpassen, sollten Sie sich zunächst grundlegend damit vertraut machen.

1. Wählen Sie **Datei** > **Speichern unter** aus. Speichern Sie dann eine Kopie der Datei unter dem Namen *azuredeploy.json* auf dem lokalen Computer.

1. Wiederholen Sie die Schritte 1 bis 3, um die folgende URL zu öffnen, und speichern Sie die Datei anschließend als *azuredeploy.parameters.json*.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Bearbeiten der Parameterdatei

Die Vorlagendatei muss nicht geändert werden.

1. Öffnen Sie *azuredeploy.parameters.json* in Visual Studio Code, falls die Datei noch nicht geöffnet ist.
1. Ändern Sie den `adminPassword`-Parameter wie folgt:

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```

    > [!IMPORTANT]
    > Ersetzen Sie den Wert für **id** durch die Ressourcen-ID des zuvor erstellten Schlüsseltresors.

    ![Integrieren von Key Vault und Resource Manager-Vorlage: VM-Bereitstellung – Parameterdatei](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)

1. Aktualisieren Sie die folgenden Werte:

    * **adminUsername:** Der Name des Administratorkontos für den virtuellen Computer.
    * **dnsLabelPrefix:** Der Name des dnsLabelPrefix-Werts.

    Beispiele für Namen finden Sie in der vorherigen Abbildung.

1. Speichern Sie die Änderungen.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Befolgen Sie die Anweisungen unter [Bereitstellen der Vorlage](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). Laden Sie *azuredeploy.json* und *azuredeploy.parameters.json* in Cloud Shell hoch, und stellen Sie anschließend die Vorlage mithilfe des folgenden PowerShell-Skripts bereit:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$location = Read-Host -Prompt "Enter the same location that is used for creating the key vault (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "$HOME/azuredeploy.json" `
    -TemplateParameterFile "$HOME/azuredeploy.parameters.json"
```

Verwenden Sie beim Bereitstellen der Vorlage die gleiche Ressourcengruppe wie für den Schlüsseltresor. Auf diese Weise lassen sich die Ressourcen einfacher bereinigen, da Sie statt zwei nur eine Ressourcengruppe löschen müssen.

## <a name="validate-the-deployment"></a>Überprüfen der Bereitstellung

Testen Sie nach erfolgreicher Bereitstellung des virtuellen Computers die Anmeldeinformationen unter Verwendung des im Schlüsseltresor gespeicherten Kennworts.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

1. Wählen Sie **Ressourcengruppen** >  **\<*NameIhrerRessourcengruppe*>**  > **simpleWinVM** aus.
1. Wählen Sie oben die Option **Connectors** aus.
1. Wählen Sie **RDP-Datei herunterladen** aus, und melden Sie sich gemäß den Anweisungen unter Verwendung des im Schlüsseltresor gespeicherten Kennworts bei dem virtuellen Computer an.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein Geheimnis aus Ihrem Azure-Schlüsseltresor abgerufen. Anschließend haben Sie das Geheimnis bei der Vorlagenbereitstellung verwendet. Informationen zum Erstellen verknüpfter Vorlagen finden Sie unter:

> [!div class="nextstepaction"]
> [Erstellen verknüpfter Vorlagen](./resource-manager-tutorial-create-linked-templates.md)
