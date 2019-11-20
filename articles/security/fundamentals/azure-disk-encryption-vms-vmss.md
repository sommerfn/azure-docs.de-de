---
title: Azure Disk Encryption für virtuelle Computer und VM-Skalierungsgruppen
description: Dieser Artikel enthält eine Übersicht zu Azure Disk Encryption.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 10/15/2019
ms.custom: seodec18
ms.openlocfilehash: 239bfb4f5a0e9115c113bb2d5ddbc931f41c0f3d
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72599808"
---
# <a name="azure-disk-encryption-for-virtual-machines-and-virtual-machine-scale-sets"></a>Azure Disk Encryption für virtuelle Computer und VM-Skalierungsgruppen

Azure Disk Encryption kann sowohl für virtuelle Linux-Computer als auch für virtuelle Windows-Computer sowie für VM-Skalierungsgruppen verwendet werden. 

## <a name="linux-virtual-machines"></a>Virtuelle Linux-Computer

Die folgenden Artikel enthalten Informationen zur Verschlüsselung virtueller Linux-Computer:

### <a name="current-version-of-azure-disk-encryption"></a>Aktuelle Version von Azure Disk Encryption

- [Azure Disk Encryption für Linux-VMs](../../virtual-machines/linux/disk-encryption-overview.md)
- [Azure Disk Encryption-Szenarien auf virtuellen Linux-Computern](../../virtual-machines/linux/disk-encryption-linux.md)
- [Erstellen und Verschlüsseln einer Linux-VM mit der Azure-Befehlszeilenschnittstelle](../../virtual-machines/linux/disk-encryption-cli-quickstart.md)
- [Erstellen und Verschlüsseln eines virtuellen Linux-Computers mit Azure PowerShell](../../virtual-machines/linux/disk-encryption-powershell-quickstart.md)
- [Erstellen und Verschlüsseln eines virtuellen Linux-Computers mit dem Azure-Portal](../../virtual-machines/linux/disk-encryption-portal-quickstart.md)
- [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption](../../virtual-machines/linux/disk-encryption-key-vault.md)
- [Azure Disk Encryption-Beispielskripts](../../virtual-machines/linux/disk-encryption-sample-scripts.md)
- [Azure Disk Encryption-Problembehandlung](../../virtual-machines/linux/disk-encryption-troubleshooting.md)
- [Azure Disk Encryption für virtuelle IaaS-Computer – FAQ](../../virtual-machines/linux/disk-encryption-faq.md)

### <a name="azure-disk-encryption-with-azure-ad-previous-version"></a>Azure Disk Encryption mit Azure AD (vorherige Version)

- [Azure Disk Encryption mit Azure AD (vorherige Version)](../../virtual-machines/linux/disk-encryption-overview-aad.md)
- [Azure Disk Encryption-Szenarien auf virtuellen Linux-Computern](../../virtual-machines/linux/disk-encryption-linux.md)
- [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption mit Azure AD (vorheriges Release)](../../virtual-machines/linux/disk-encryption-key-vault-aad.md)

## <a name="windows-virtual-machines"></a>Virtuelle Windows-Computer

Die folgenden Artikel enthalten Informationen zur Verschlüsselung virtueller Windows-Computer:

### <a name="current-version-of-azure-disk-encryption"></a>Aktuelle Version von Azure Disk Encryption

- [Azure Disk Encryption für virtuelle Windows-Computer](../../virtual-machines/windows/disk-encryption-overview.md)
- [Azure Disk Encryption-Szenarien auf virtuellen Windows-Computern](../../virtual-machines/windows/disk-encryption-windows.md)
- [Erstellen und Verschlüsseln eines virtuellen Windows-Computers mit der Azure-Befehlszeilenschnittstelle](../../virtual-machines/windows/disk-encryption-cli-quickstart.md)
- [Erstellen und Verschlüsseln eines virtuellen Windows-Computers mit Azure PowerShell](../../virtual-machines/windows/disk-encryption-powershell-quickstart.md)
- [Erstellen und Verschlüsseln eines virtuellen Windows-Computers mit dem Azure-Portal](../../virtual-machines/windows/disk-encryption-portal-quickstart.md)
- [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption](../../virtual-machines/windows/disk-encryption-key-vault.md)
- [Azure Disk Encryption-Beispielskripts](../../virtual-machines/windows/disk-encryption-sample-scripts.md)
- [Azure Disk Encryption-Problembehandlung](../../virtual-machines/windows/disk-encryption-troubleshooting.md)
- [Azure Disk Encryption für virtuelle IaaS-Computer – FAQ](../../virtual-machines/windows/disk-encryption-faq.md)

### <a name="azure-disk-encryption-with-azure-ad-previous-version"></a>Azure Disk Encryption mit Azure AD (vorherige Version)

- [Azure Disk Encryption mit Azure AD (vorherige Version)](../../virtual-machines/windows/disk-encryption-overview-aad.md)
- [Azure Disk Encryption-Szenarien auf virtuellen Windows-Computern](../../virtual-machines/windows/disk-encryption-windows.md)
- [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption mit Azure AD (vorheriges Release)](../../virtual-machines/windows/disk-encryption-key-vault-aad.md)

## <a name="virtual-machine-scale-sets"></a>VM-Skalierungsgruppen

Die folgenden Artikel enthalten Informationen zur Verschlüsselung von VM-Skalierungsgruppen:

- [Azure Disk Encryption für VM-Skalierungsgruppen](../../virtual-machine-scale-sets/disk-encryption-overview.md) 
- [Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure CLI](../../virtual-machine-scale-sets/disk-encryption-cli.md) 
- [Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure PowerShell](../../virtual-machine-scale-sets/disk-encryption-powershell.md)
- [Verschlüsseln von VM-Skalierungsgruppen mit Azure Resource Manager](../../virtual-machine-scale-sets/disk-encryption-azure-resource-manager.md)
- [Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption](../../virtual-machine-scale-sets/disk-encryption-key-vault.md)
- [Verwenden von Azure Disk Encryption mit Erweiterungssequenzierung der VM-Skalierungsgruppe](../../virtual-machine-scale-sets/disk-encryption-extension-sequencing.md)

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über die Azure-Verschlüsselung](encryption-overview.md)
- [Datenverschlüsselung ruhender Daten](encryption-atrest.md)
- [Bewährte Methoden für Datensicherheit und Verschlüsselung](data-encryption-best-practices.md)
