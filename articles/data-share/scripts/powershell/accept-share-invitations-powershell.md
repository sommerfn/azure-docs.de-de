---
title: 'PowerShell-Skript: Akzeptieren einer Einladung für Azure Data Share | Microsoft-Dokumentation'
description: Dieses PowerShell-Skript akzeptiert Einladungen von einer vorhandenen Datenfreigabe.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 246e3550650dfd458b4aeecda3b5b7733e49f017
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70307334"
---
# <a name="use-powershell-to-accept-a-data-share-invitation"></a>Verwenden von PowerShell zum Akzeptieren einer Einladung von einer Datenfreigabe

Dieses PowerShell-Skript akzeptiert Einladungen, die an einen Consumer gesendet werden.

## <a name="sample-script"></a>Beispielskript
```powershell
#List invitations sent to a consumer
Get-AzDataShareInvitation

# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$invitationId = "<Invitation id>"

#Accept a specific invitation by creating a share subscription
New-AzDataShareSubscription -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -Name $dataShareName -InvitationId $invitationId

```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [Get-AzDataShareInvitation](/powershell/module/az.datashare/get-azdatashareinvitation?view=azps-2.6.0) | Abrufen und Auflisten gesendeter Einladungen zu Datenfreigaben |
| [New-AzDataShareSubscription](/powershell/module/az.datashare/get-azdatasharesubscription?view=azps-2.6.0) | Erstellen eines Abonnements für eine Datenfreigabe |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).

