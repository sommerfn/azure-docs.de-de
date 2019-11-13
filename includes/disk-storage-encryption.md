---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 10/24/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0659e57f5a5b223c199becf492b27c7a70cbdc63
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612068"
---
Verwaltete Azure-Datenträger verschlüsseln Daten standardmäßig automatisch, wenn die Daten in der Cloud gespeichert werden. Die serverseitige Verschlüsselung schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation. Daten in verwalteten Azure-Datenträgern werden transparent mit 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) – einer der stärksten verfügbaren Blockverschlüsselungen – FIPS 140-2-konform ver- und entschlüsselt.   

Die Verschlüsselung wirkt sich nicht auf die Leistung verwalteter Datenträger aus. Für die Verschlüsselung werden keine zusätzlichen Kosten in Rechnung gestellt.

Weitere Informationen zu den kryptografischen Modulen, die verwalteten Azure-Datenträgern zugrunde liegen, finden Sie unter [Kryptografie-API: Die nächste Generation](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="about-encryption-key-management"></a>Informationen zur Verwaltung von Verschlüsselungsschlüsseln

Sie können von der Plattform verwaltete Schlüssel für die Verschlüsselung der verwalteten Datenträger verwenden oder die Verschlüsselung mit eigenen Schlüsseln verwalten (öffentliche Vorschauversion). Wenn Sie die Verschlüsselung mit eigenen Schlüsseln verwalten möchten, können Sie einen *vom Kunden verwalteten Schlüssel* angeben, der zum Verschlüsseln und Entschlüsseln aller Daten in verwalteten Datenträgern verwendet werden soll. 

In den folgenden Abschnitten werden die einzelnen Optionen für die Schlüsselverwaltung ausführlicher beschrieben.

## <a name="platform-managed-keys"></a>Von der Plattform verwaltete Schlüssel

Verwaltete Datenträger verwenden standardmäßig von der Plattform verwaltete Verschlüsselungsschlüssel. Ab dem 10. Juni 2017 werden alle neuen verwalteten Datenträger, Momentaufnahmen, Images und neuen Daten, die auf vorhandene verwaltete Datenträger geschrieben wurden, automatisch mit von der Plattform verwalteten Schlüsseln verschlüsselt. 

## <a name="customer-managed-keys-public-preview"></a>Vom Kunden verwaltete Schlüssel (öffentliche Vorschauversion)

Sie können die Verschlüsselung auf der Ebene verwalteter Datenträger mit eigenen Schlüsseln verwalten. Die serverseitige Verschlüsselung für verwaltete Datenträger mit vom Kunden verwalteten Schlüsseln bietet eine integrierte Benutzerfunktionalität mit Azure Key Vault. Sie können entweder [ihre RSA-Schlüssel](../articles/key-vault/key-vault-hsm-protected-keys.md) in den Schlüsseltresor importieren oder neue RSA-Schlüssel in Azure Key Vault generieren. Die Verschlüsselung und Entschlüsselung von verwalteten Azure-Datenträgern erfolgt durch [Umschlagverschlüsselung](../articles/storage/common/storage-client-side-encryption.md#encryption-via-the-envelope-technique) vollständig transparent. Daten werden mithilfe eines [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)-256 basierten Datenverschlüsselungsschlüssels (DEK) verschlüsselt, der wiederum mit Ihren Schlüsseln geschützt wird. Sie müssen den Zugriff auf verwaltete Datenträger in Ihrem Schlüsseltresor gewähren, damit Sie Ihre Schlüssel zum Verschlüsseln und Entschlüsseln des DEK verwenden können. Dies ermöglicht eine umfassende Kontrolle über Ihre Daten und Schlüssel. Sie können Ihre Schlüssel jederzeit deaktivieren oder den Zugriff auf verwaltete Datenträger widerrufen. Sie können auch die Verwendung von Verschlüsselungsschlüsseln mithilfe der Azure Key Vault-Überwachung überwachen, um sicherzustellen, dass nur verwaltete Datenträger oder andere vertrauenswürdige Azure-Dienste auf Ihre Schlüssel zugreifen.

Das folgende Diagramm zeigt, wie Azure Active Directory und Azure Key Vault von verwalteten Datenträgern verwendet werden, um Anforderungen mit dem vom Kunden verwalteten Schlüssel zu stellen:

![Workflow für verwaltete Datenträger mit vom Kunden verwalteten Schlüsseln](media/disk-storage-encryption/customer-managed-keys-sse-managed-disks-workflow.png)


In der folgenden Liste werden die nummerierten Schritte im Diagramm erläutert:

1. Ein Azure Key Vault-Administrator erstellt Schlüsseltresorressourcen.
1. Der Schlüsseltresoradministrator importiert die jeweiligen RSA-Schlüssel in Key Vault oder generiert neue RSA-Schlüssel in Key Vault.
1. Dieser Administrator erstellt eine Instanz der Datenträgerverschlüsselungssatz-Ressource und gibt eine Azure Key Vault-ID sowie eine Schlüssel-URL an. Der Datenträgerverschlüsselungssatz ist eine neue Ressource, die zur Vereinfachung der Schlüsselverwaltung für verwaltete Datenträger eingeführt wurde. 
1. Beim Erstellen eines Datenträgerverschlüsselungssatzes wird eine [vom System zugewiesene verwaltete Identität](../articles/active-directory/managed-identities-azure-resources/overview.md) in Azure Active Directory (AD) erstellt und mit dem Datenträgerverschlüsselungssatz verknüpft. 
1. Der Azure Key Vault-Administrator erteilt dann der verwalteten Identität Berechtigungen zum Durchführen von Vorgängen im Schlüsseltresor.
1. Ein VM-Benutzer erstellt Datenträger, indem er sie dem Datenträgerverschlüsselungssatz zuordnet. Der VM-Benutzer kann auch die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln für vorhandene Ressourcen aktivieren, indem er diese dem Datenträgerverschlüsselungssatz zuordnet. 
1. Verwaltete Datenträger verwenden die verwaltete Identität, um Anforderungen an den Azure Key Vault zu senden.
1. Zum Lesen oder Schreiben von Daten senden verwaltete Datenträger Anforderungen an Azure Key Vault, um den Datenverschlüsselungsschlüssel zu verschlüsseln (wrap) und zu entschlüsseln (unwrap), um die Daten zu verschlüsseln und zu entschlüsseln. 

Informationen zum Widerrufen von Kunden verwalteter Schlüsseln finden Sie in den Artikeln zu [Azure Key Vault-PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) und zur [Azure Key Vault-CLI](https://docs.microsoft.com/cli/azure/keyvault). Durch das Widerrufen des Zugriffs wird der Zugriff auf alle Daten im Speicherkonto blockiert, da Azure Storage keinen Zugriff mehr auf den Verschlüsselungsschlüssel hat.

### <a name="supported-scenarios-and-restrictions"></a>Unterstützte Szenarien und Einschränkungen

Während der Vorschauphase werden nur die folgenden Szenarien unterstützt:

- Erstellen eines virtuellen Computers (VM) aus einem Azure Marketplace-Image und Verschlüsseln des Betriebssystemdatenträgers mit serverseitiger Verschlüsselung unter Verwendung vom Kunden verwalteter Schlüssel.
- Erstellen eines benutzerdefinierten Image, das mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wird.
- Erstellen einer VM aus einem benutzerdefiniertes Image und Verschlüsseln des Betriebssystemdatenträgers mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln.
- Erstellen von Datenträgern, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt werden.
- Erstellen von Momentaufnahmen, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt werden.
- Erstellen von VM-Skalierungsgruppen, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt werden.

Für die Vorschauversion gelten folgende Einschränkungen:

- Nur in der Region „USA, Westen-Mitte“ verfügbar.
- Datenträger, die aus benutzerdefinierten Images erstellt werden, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, müssen mit denselben vom Kunden verwalteten Schlüsseln verschlüsselt werden und sich im selben Abonnement befinden.
- Momentaufnahmen, die von Datenträgern erstellt werden, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, müssen mit denselben vom Kunden verwalteten Schlüsseln verschlüsselt werden.
- Benutzerdefinierte Images, die mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wurden, können nicht im Katalog mit freigegebenen Images verwendet werden.
- Der Schlüsseltresor muss sich in demselben Abonnement und derselben Region wie die vom Kunden verwalteten Schlüssel befinden.
- Mit vom Kunden verwalteten Schlüsseln verschlüsselte Datenträger, Momentaufnahmen und Images können nicht in ein anderes Abonnement verschoben werden.

### <a name="setting-up-your-azure-key-vault"></a>Einrichten v on Azure Key Vault

1.  Erstellen Sie eine Azure Key Vault-Instanz und den Verschlüsselungsschlüssel.

    Beim Erstellen der Key Vault-Instanz müssen Sie vorläufiges Löschen und den Schutz vor endgültigem Löschen aktivieren. Durch vorläufiges Löschen wird sichergestellt, dass der Schlüsseltresor einen gelöschten Schlüssel für einen bestimmten Aufbewahrungszeitraum (standardmäßig 90 Tage) speichert. Der Schutz vor endgültigem Löschen stellt sicher, dass ein gelöschter Schlüssel erst nach Ablauf der Aufbewahrungsdauer dauerhaft gelöscht werden kann. Diese Einstellungen schützen Sie vor dem Verlust von Daten durch versehentliches Löschen. Diese Einstellungen sind obligatorisch, wenn ein Schlüsseltresor für die Verschlüsselung verwalteter Datenträger verwendet wird.

    ```powershell
    $keyVault = New-AzKeyVault -Name myKeyVaultName ` 
    -ResourceGroupName myRGName ` 
    -Location westcentralus ` 
    -EnableSoftDelete ` 
    -EnablePurgeProtection 
     
    $key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName ` 
    -Name myKeyName ` 
    -Destination Software `  
    ```

1.  Erstellen Sie eine DiskEncryptionSet-Instanz. 
    
    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName myRGName ` 
      -TemplateUri "https://raw.githubusercontent.com/ramankumarlive/manageddiskscmkpreview/master/CreateDiskEncryptionSet.json" ` 
      -diskEncryptionSetName "myDiskEncryptionSet1" ` 
      -keyVaultId "/subscriptions/mySubscriptionId/resourceGroups/myRGName/providers/Microsoft.KeyVault/vaults/myKeyVaultName" ` 
      -keyVaultKeyUrl "https://myKeyVaultName.vault.azure.net/keys/myKeyName/403445136dee4a57af7068cab08f7d42" ` 
      -region "WestCentralUS"
    ```

1.  Gewähren Sie der DiskEncryptionSet-Ressource Zugriff auf den Schlüsseltresor.

    ```powershell
    $identity = Get-AzADServicePrincipal -DisplayName myDiskEncryptionSet1  
     
    Set-AzKeyVaultAccessPolicy ` 
        -VaultName $keyVault.VaultName ` 
        -ObjectId $identity.Id ` 
        -PermissionsToKeys wrapkey,unwrapkey,get 
     
    New-AzRoleAssignment ` 
        -ObjectId $identity.Id ` 
        -RoleDefinitionName "Reader" ` 
        -ResourceName $keyVault.VaultName ` 
        -ResourceType "Microsoft.KeyVault/vaults" ` 
        -ResourceGroupName myRGName `  
    ```

### <a name="create-a-vm-using-a-marketplace-image-encrypting-the-os-and-data-disks-with-customer-managed-keys-via-a-resource-manager-template"></a>Erstellen einer VM mit einem Marketplace-Image, Verschlüsseln des Betriebssystems und der Datenträger mit vom Kunden verwalteten Schlüsseln über eine Resource Manager-Vorlage

```
$password=ConvertTo-SecureString -String "myVMPassword" `
  -AsPlainText -Force
New-AzResourceGroupDeployment -ResourceGroupName CMKTesting `
  -TemplateUri "https://raw.githubusercontent.com/ramankumarlive/manageddiskscmkpreview/master/CreateVMWithDisksEncryptedWithCMK.json" `
  -virtualMachineName "myVMName" `
  -adminPassword $password `
  -vmSize "Standard_DS3_V2" `
  -diskEncryptionSetId "/subscriptions/mySubscriptionId/resourceGroups/myRGName/providers/Microsoft.Compute/diskEncryptionSets/myDiskEncryptionSet1" `
  -region "westcentralus" 
```

### <a name="create-an-empty-disk-encrypted-using-server-side-encryption-with-customer-managed-keys-and-attach-it-to-a-vm"></a>Erstellen eines leeren Datenträgers, der mit serverseitiger Verschlüsselung und vom Kunden verwalteten Schlüsseln verschlüsselt wird, und Anfügen an eine VM

```PowerShell
$vmName = "yourVMName"
$rgName = "yourRGName"
$diskName = "yourDiskName"
$diskSKU = "Premium_LRS"
$diskSizeinGiB = "30"
$diskEncryptionSetId = "/subscriptions/<subscriptionID>/resourceGroups/yourRGName/providers/Microsoft.Compute/diskEncryptionSets/<yourDiskEncryptionSetName>"
$region = "westcentralus"
$diskLUN = 1

New-AzResourceGroupDeployment -ResourceGroupName $rgName `
  -TemplateUri "https://raw.githubusercontent.com/ramankumarlive/manageddiskscmkpreview/master/CreateEmptyDataDiskEncryptedWithSSECMK.json" `
  -diskName $diskName `
  -diskSkuName $diskSKU `
  -dataDiskSizeInGb $diskSizeinGiB `
  -diskEncryptionSetId $diskEncryptionSetId `
  -region $region 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName
$vm = Add-AzVMDataDisk -VM $vm -Name $diskName -CreateOption Attach -ManagedDiskId $disk.Id -Lun 1
Update-AzVM -ResourceGroupName $rgName -VM $vm
```


> [!IMPORTANT]
> Von Kunden verwaltete Schlüssel basieren auf verwalteten Identitäten für Azure-Ressourcen, einem Feature von Azure Active Directory (Azure AD). Wenn Sie vom Kunden verwaltete Schlüssel konfigurieren, wird Ihren Ressourcen im Hintergrund automatisch eine verwaltete Identität zugewiesen. Wenn Sie anschließend das Abonnement, die Ressourcengruppe oder den verwalteten Datenträger von einem Azure AD-Verzeichnis in ein anderes Verzeichnis verschieben, wird die den verwalteten Datenträgern zugeordnete verwaltete Identität nicht an den neuen Mandanten übertragen, sodass vom Kunden verwaltete Schlüssel möglicherweise nicht mehr funktionieren. Weitere Informationen finden Sie unter [Übertragen eines Abonnements zwischen Azure AD-Verzeichnissen](../articles/active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>Vergleich: Serverseitige Verschlüsselung und Azure-Datenträgerverschlüsselung

[Azure Disk Encryption](../articles/security/fundamentals/azure-disk-encryption-vms-vmss.md) nutzt das [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview)-Feature von Windows und das [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt)-Feature von Linux zum Verschlüsseln verwalteter Datenträger mit vom Kunden verwalteten Schlüsseln innerhalb der Gast-VM.  Die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln bewirkt eine ADE-Verbesserung, indem Sie beliebige Betriebssystemtypen und Images für Ihre VMs verwenden können, indem Daten im Speicherdienst verschlüsselt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Was ist der Azure-Schlüsseltresor?](../articles/key-vault/key-vault-overview.md)
