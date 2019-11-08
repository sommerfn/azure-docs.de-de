---
title: Bereitstellen eines virtuellen Computers aus dem Azure Marketplace
description: Es wird beschrieben, wie Sie einen virtuellen Computer über einen vorkonfigurierten virtuellen Computer vom Azure Marketplace bereitstellen.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 11/29/2018
ms.author: pabutler
ms.openlocfilehash: 2888d7643fd4f624634dc2ec520bec6e753382f1
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73816823"
---
# <a name="deploy-a-virtual-machine-from-the-azure-marketplace"></a>Bereitstellen eines virtuellen Computers über den Azure Marketplace

In diesem Artikel wird beschrieben, wie Sie einen vorkonfigurierten virtuellen Computer (VM) über einen Azure Marketplace bereitstellen, indem Sie das beigefügte Azure PowerShell-Skript verwenden.  Mit diesem Skript werden auch die WinRM-HTTP- und HTTPS-Endpunkte auf der VM verfügbar gemacht.  Für dieses Skript ist es erforderlich, dass Sie bereits ein Zertifikat in Azure Key Vault hochgeladen haben, wie unter [Erstellen von Zertifikaten für Azure Key Vault](./cpp-create-key-vault-cert.md) beschrieben. 

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

## <a name="vm-deployment-template"></a>VM-Bereitstellungsvorlage

Die Azure-VM-Bereitstellungsvorlage für den Schnellstart ist als Onlinedatei unter [azuredeploy.json](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json) verfügbar.  Sie enthält die folgenden Parameter:

|  **Parameter**        |   **Beschreibung**                                 |
|  -------------        |   ---------------                                 |
| newStorageAccountName | Name des Speicherkontos                       |
| dnsNameForPublicIP    | DNS-Name für die öffentliche IP-Adresse. Muss aus Kleinbuchstaben bestehen.    |
| adminUserName         | Benutzername des Administrators                          |
| adminPassword         | Kennwort des Administrators                          |
| imagePublisher        | Imageherausgeber                                   |
| imageOffer            | Imageangebot                                       |
| imageSKU              | Image-SKU                                         |
| vmSize                | Größe des virtuellen Computers                                    |
| vmName                | Name des virtuellen Computers                                    |
| vaultName             | Der Name des Schlüsseltresors                             |
| vaultResourceGroup    | Die Ressourcengruppe des Schlüsseltresors                   |
| certificateUrl        | URL für das Zertifikat, einschließlich der Version in KeyVault, z.B. `https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7` |
|  |  |


## <a name="deployment-script"></a>Bereitstellungsskript

Bearbeiten Sie das folgende Azure PowerShell-Skript, und führen Sie es aus, um die angegebene Azure Marketplace-VM bereitzustellen.

```powershell

New-AzResourceGroupDeployment -Name "dplvm$postfix" -ResourceGroupName "$rgName" -TemplateUri "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json" -newStorageAccountName "test$postfix" -dnsNameForPublicIP $vmName -adminUserName "isv" -adminPassword $pwd -vmSize "Standard_A2" -vmName $vmName -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id 

```


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie eine vorkonfigurierte VM bereitgestellt haben, können Sie die darauf enthaltenen Lösungen und Dienste konfigurieren und darauf zugreifen oder die VM für weitere Entwicklungsschritte nutzen. 
