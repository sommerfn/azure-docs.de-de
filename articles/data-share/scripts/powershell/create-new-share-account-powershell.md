---
title: 'PowerShell-Skript: Erstellen eines neuen Azure Data Share-Kontos | Microsoft-Dokumentation'
description: Dieses PowerShell-Skript erstellt ein neues Data Share-Konto.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: c3852dd5f1d3d3df8a982716ce5dab9426782869
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70307272"
---
# <a name="use-powershell-to-create-a-data-share-account-in-azure"></a>Verwenden von PowerShell zum Erstellen eines Data Share-Kontos in Azure

Dieses PowerShell-Skript erstellt ein neues Data Share-Konto. 

## <a name="sample-script"></a>Beispielskript

```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name"
$location = "<Location>"
$dataShareAccountName = "<Data share account name>"

# Create a new Azure Data Share account
New-AzDataShareAccount -ResourceGroupName $resourceGroupName -Name $dataShareAccountName -Location $location
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [New-AzDataShareAccount](/powershell/module/az.datashare/new-azdatashareaccount?view=azps-2.6.0) | Erstellt ein Data Share-Konto |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
