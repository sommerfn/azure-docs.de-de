---
title: Azure Disk Encryption für Linux | Microsoft-Dokumentation
description: Stellen Sie Azure Disk Encryption mithilfe einer VM-Erweiterung auf einem virtuellen Linux-Computer bereit.
services: virtual-machines-linux
documentationcenter: ''
author: ejarvi
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/10/2019
ms.author: ejarvi
ms.openlocfilehash: 1801ca8bd59153de81ef680253da229e2bd4a338
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72597869"
---
# <a name="azure-disk-encryption-for-linux-microsoftazuresecurityazurediskencryptionforlinux"></a>Azure Disk Encryption für Linux (Microsoft.Azure.Security.AzureDiskEncryptionForLinux)

## <a name="overview"></a>Übersicht

Azure Disk Encryption nutzt das Subsystem dm-crypt in Linux zur Gewährleistung einer vollständigen Datenträgerverschlüsselung bei der [Auswahl von Azure Linux-Verteilungen](https://aka.ms/adelinux).  Diese Lösung ist in Azure Key Vault integriert, um die Verwaltung der Datenträger-Verschlüsselungsschlüssel zu erleichtern.

## <a name="prerequisites"></a>Voraussetzungen

Eine vollständige Liste mit den Voraussetzungen finden Sie unter [Azure Disk Encryption für Linux-VMs](../linux/disk-encryption-overview.md). Achten Sie besonders auf die folgenden Abschnitte:

- [Azure Disk Encryption für Linux-VMs](../linux/disk-encryption-overview.md#supported-vms-and-operating-systems)
- [Weitere VM-Anforderungen](../linux/disk-encryption-overview.md#additional-vm-requirements)
- [Netzwerkanforderungen](../linux/disk-encryption-overview.md#networking-requirements)

## <a name="extension-schemata"></a>Erweiterungsschemas

Es gibt zwei Schemas für Azure Disk Encryption: v1.1 – ein neueres, empfohlenes Schema, das keine Eigenschaften von Azure Active Directory (AAD) voraussetzt, und v0.1 – ein älteres Schema, für das AAD-Eigenschaften erforderlich sind. Sie müssen die Schemaversion verwenden, die Ihrer Erweiterung entspricht: Schema v1.1 für die AzureDiskEncryptionForLinux-Erweiterung, Version 1.1, und Schema v0.1 für die AzureDiskEncryptionForLinux-Erweiterung, Version 0.1.
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
          "DiskFormatQuery": "[diskFormatQuery]",
          "EncryptionOperation": "[encryptionOperation]",
          "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
          "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
          "KeyVaultURL": "[keyVaultURL]",
          "SequenceVersion": "sequenceVersion]",
          "VolumeType": "[volumeType]"
        },
        "type": "AzureDiskEncryptionForLinux",
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
      "AADClientSecret": "[aadClientSecret]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
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
      "AADClientCertificate": "[aadClientCertificate]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
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
| (optional) KeyVaultURL | url | string |
| Passphrase | password | string | 
| SequenceVersion | uniqueidentifier | string |
| VolumeType | Betriebssystem, Daten, alle | string |

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Ein Beispiel für die Bereitstellung von Vorlagen finden Sie unter [Aktivieren der Verschlüsselung auf einem laufenden virtuellen Linux-Computer](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

## <a name="azure-cli-deployment"></a>Bereitstellung mithilfe der Azure-Befehlszeilenschnittstelle

Anweisungen finden Sie in der aktuellen [Dokumentation der Azure-Befehlszeilenschnittstelle](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Informationen zur Problembehandlung finden Sie unter [Leitfaden zur Azure Disk Encryption-Problembehandlung](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/community/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu VM-Erweiterungen finden Sie unter [Erweiterungen und Features für virtuelle Computer für Linux](features-linux.md).