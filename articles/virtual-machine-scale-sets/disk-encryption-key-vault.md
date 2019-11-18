---
title: Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption
description: Dieser Artikel enthält Schritte zum Erstellen und Konfigurieren eines Schlüsseltresors für die Verwendung mit Azure Disk Encryption.
ms.service: virtual-machine-scale-sets
ms.topic: article
author: msmbaldwin
ms.author: mbaldwin
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: 0252fff84307577ab307e591948e16dc30c38f66
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72749997"
---
# <a name="creating-and-configuring-a-key-vault-for-azure-disk-encryption"></a>Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption

Azure Disk Encryption verwendet Azure Key Vault zum Steuern und Verwalten von Verschlüsselungsschlüsseln und Geheimnissen für die Datenträgerverschlüsselung.  Weitere Informationen zu Schlüsseltresoren finden Sie unter [Erste Schritte mit Azure Key Vault](../key-vault/key-vault-get-started.md) und [Schützen eines Schlüsseltresors](../key-vault/key-vault-secure-your-key-vault.md).

Das Erstellen und Konfigurieren eines Schlüsseltresors für Azure Disk Encryption umfasst drei Schritte:

1. Erstellen einer Ressourcengruppe (bei Bedarf)
2. Erstellen eines Schlüsseltresors 
3. Festlegen von erweiterten Zugriffsrichtlinien für den Schlüsseltresor

Diese Schritte werden in den folgenden Schnellstarts veranschaulicht:

Sie können auch einen Schlüssel für die Schlüsselverschlüsselung (Key Encryption Key, KEK) generieren oder importieren.

## <a name="install-tools-and-connect-to-azure"></a>Installieren von Tools und Herstellen einer Verbindung mit Azure

Die Schritte in diesem Artikel können mit der [Azure-Befehlszeilenschnittstelle](/cli/azure/), dem [Azure PowerShell-Az-Modul](/powershell/azure/overview) oder dem [Azure-Portal](https://portal.azure.com) abgeschlossen werden.

### <a name="connect-to-your-azure-account"></a>Herstellen einer Verbindung mit Ihrem Azure-Konto

Vor der Verwendung der Azure-Befehlszeilenschnittstelle oder von Azure PowerShell müssen Sie zunächst eine Verbindung mit Ihrem Azure-Abonnement herstellen. Dies erfolgt durch [Anmelden mit der Azure-Befehlszeilenschnittstelle](/cli/azure/authenticate-azure-cli?view=azure-cli-latest), [Anmelden mit Azure PowerShell](/powershell/azure/authenticate-azureps?view=azps-2.5.0) oder Bereitstellen der Anmeldeinformationen im Azure-Portal nach Aufforderung.

```azurecli-interactive
az login
```

```azurepowershell-interactive
Connect-AzAccount
```

[!INCLUDE [disk-encryption-key-vault](../../includes/disk-encryption-key-vault.md)]
 
## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure Disk Encryption](disk-encryption-overview.md)
- [Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure CLI](disk-encryption-cli.md)
- [Verschlüsseln von Betriebssystem- und angefügten Datenträgern in einer VM-Skalierungsgruppe mit Azure PowerShell](disk-encryption-powershell.md)
