---
title: Azure Disk Encryption für Windows | Microsoft-Dokumentation
description: Stellen Sie Azure Disk Encryption mithilfe einer VM-Erweiterung auf einem virtuellen Windows-Computer bereit.
services: virtual-machines-windows
documentationcenter: ''
author: ejarvi
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: ejarvi
ms.openlocfilehash: 11394f692765cc1df5db0eb5c0dd06425026505d
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70092646"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Azure Disk Encryption für Windows (Microsoft.Azure.Security.AzureDiskEncryption)

## <a name="overview"></a>Übersicht

Azure Disk Encryption nutzt BitLocker, um auf virtuellen Azure-Computern unter Windows eine vollständige Datenträgerverschlüsselung bereitzustellen.  Diese Lösung ist in Azure Key Vault integriert, um die Verschlüsselungsschlüssel und Geheimnisse für die Datenträgerverschlüsselung in Ihrem Key Vault-Abonnement zu verwalten. 

## <a name="prerequisites"></a>Voraussetzungen

Eine vollständige Liste der Voraussetzungen finden Sie unter [Voraussetzungen für Azure Disk Encryption](
../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="operating-system"></a>Betriebssystem

Eine Liste der derzeit unterstützten Windows-Versionen finden Sie unter [Voraussetzungen für Azure Disk Encryption](../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="internet-connectivity"></a>Internetkonnektivität

Für Azure Disk Encryption ist eine Internetverbindung für den Zugriff auf Active Directory, Key Vault, Storage und Paketverwaltungs-Endpunkte erforderlich.  Weitere Informationen zu Netzwerksicherheitseinstellungen finden Sie unter [Voraussetzungen für Azure Disk Encryption](
../../security/azure-security-disk-encryption-prerequisites.md).

## <a name="extension-schemata"></a>Erweiterungsschemas

Es gibt zwei Schemas für Azure Disk Encryption: v1.1 – ein neueres, empfohlenes Schema, das keine Eigenschaften von Azure Active Directory (AAD) voraussetzt, und v0.1 – ein älteres Schema, für das AAD-Eigenschaften erforderlich sind. Sie müssen die Schemaversion verwenden, die Ihrer Erweiterung entspricht: Schema v1.1 für die AzureDiskEncryption-Erweiterung, Version 1.1, und Schema v0.1 für die AzureDiskEncryption-Erweiterung, Version 0.1.

### <a name="schema-v11-no-aad-recommended"></a>Schema v1.1: Kein AAD (empfohlen)

Das v1.1-Schema wird empfohlen und erfordert keine Azure Active Directory-Eigenschaften.

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```


### <a name="schema-v01-with-aad"></a>Schema v0.1: mit AAD 

Das 0.1-Schema erfordert `aadClientID` und entweder `aadClientSecret` oder `AADClientCertificate`.

Verwenden von `aadClientSecret`:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```

Verwenden von `AADClientCertificate`:

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientCertificate": "[aadClientCertificate]"
    },    
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KekVaultResourceId": "[keyVaultResourceID]",
      "KeyVaultURL": "[keyVaultURL]",
      "KeyVaultResourceId": "[keyVaultResourceID]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
  "type": "AzureDiskEncryption",
  "typeHandlerVersion": "[extensionVersion]"
  }
}
```


### <a name="property-values"></a>Eigenschaftswerte

| NAME | Wert/Beispiel | Datentyp |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| publisher | Microsoft.Azure.Security | string |
| type | AzureDiskEncryptionForLinux | string |
| typeHandlerVersion | 0.1, 1.1 | int |
| (0.1-Schema) AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| (0.1-Schema) AADClientSecret | password | string |
| (0.1-Schema) AADClientCertificate | thumbprint | string |
| DiskFormatQuery | {"dev_path":"","name":"","file_system":""} | JSON-Wörterbuch |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | string | 
| KeyEncryptionAlgorithm | 'RSA-OAEP', 'RSA-OAEP-256', 'RSA1_5' | string |
| KeyEncryptionKeyURL | url | string |
| KeyVaultURL | url | string |
| (optional) Passphrase | password | string | 
| SequenceVersion | uniqueidentifier | string |
| VolumeType | Betriebssystem, Daten, alle | string |

## <a name="template-deployment"></a>Bereitstellung von Vorlagen
Ein Beispiel für eine Vorlagenbereitstellung finden Sie unter [Create a new encrypted Windows VM from gallery image](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image) (Erstellen einer neuen verschlüsselten Windows-VM aus einem Katalogimage).

## <a name="azure-cli-deployment"></a>Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle

Anweisungen finden Sie in der aktuellen [Dokumentation der Azure-Befehlszeilenschnittstelle](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Lesen Sie den [Leitfaden zur Azure Disk Encryption-Problembehandlung](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/community/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Erweiterungen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Windows](features-windows.md).
