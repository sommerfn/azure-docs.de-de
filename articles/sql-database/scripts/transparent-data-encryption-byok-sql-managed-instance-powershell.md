---
title: 'PowerShell: Aktivieren von BYOK-TDE: verwaltete Azure SQL-Datenbank-Instanz '
description: Erfahren Sie, wie eine verwaltete Azure SQL-Datenbank-Instanz mithilfe von PowerShell für die Verwendung von Transparent Data Encryption (TDE) zur Verschlüsselung ruhender Daten in einem BYOK-Szenario konfiguriert wird.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
ms.date: 11/05/2019
ms.openlocfilehash: ddffda5229c9c0d33c563e3ae7b4a884f0f92dff
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73691394"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>Verwalten von Transparent Data Encryption in einer verwalteten Instanz mithilfe eines eigenen Azure Key Vault-Schlüssels

In diesem PowerShell-Skriptbeispiel wird Transparent Data Encryption (TDE) mit einem kundenseitig verwalteten Schlüssel für eine verwaltete Azure SQL-Datenbank-Instanz unter Verwendung eines Schlüssels aus Azure Key Vault konfiguriert. Dieses Szenario wird häufig als „Bring Your Own Key“ (BYOK) für TDE bezeichnet. Weitere Informationen zu TDE mit einem kundenseitig verwalteten Schlüssel finden Sie unter [Azure SQL Transparent Data Encryption mithilfe von Schlüsseln, die vom Kunden in Azure Key Vault verwaltet werden: Bring Your Own Key-Unterstützung](../transparent-data-encryption-byok-azure-sql.md).

## <a name="prerequisites"></a>Voraussetzungen

- Eine vorhandene verwaltete Instanz. Siehe [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz mithilfe von PowerShell](sql-database-create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Sowohl die lokale Verwendung von PowerShell als auch die Verwendung von Azure Cloud Shell erfordert Azure PowerShell 2.3.2 oder eine höhere Version. Wenn Sie ein Upgrade durchführen müssen, lesen Sie [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps), oder führen Sie das folgende Beispielskript zum Installieren des Moduls für den aktuellen Benutzer aus:

`Install-Module -Name Az -AllowClobber -Scope CurrentUser`

Wenn Sie PowerShell lokal ausführen, müssen Sie auch `Connect-AzAccount` ausführen, um eine Verbindung mit Azure herzustellen.

## <a name="sample-scripts"></a>Beispielskripts

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie in den [Azure PowerShell samples for Azure SQL Database](../sql-database-powershell-samples.md) (Azure PowerShell-Beispiele für Azure SQL-Datenbank).
