---
title: Azure PowerShell-Skriptbeispiel – Erstellen eines Log Analytics-Arbeitsbereichs | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Erstellen eines Log Analytics-Arbeitsbereichs
ms.service: azure-monitor
ms.subservice: logs
ms.topic: sample
author: MGoedtel
ms.author: magoedte
ms.date: 09/07/2017
ms.openlocfilehash: 6aad6c6f0656bd317a808de13e340d5774cea49b
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72931913"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>Erstellen eines Log Analytics-Arbeitsbereichs mit PowerShell

Mithilfe dieses Skripts verfügen Sie rasch über einen einsatzbereiten Azure Log Analytics-Arbeitsbereich, den Sie benötigen, wenn Sie damit beginnen möchten, Daten zu erfassen, zu analysieren und zu bearbeiten.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/log-analytics/log-analytics-create-new-resource/log-analytics-create-new-resource.ps1 "Create new Log Analytics workspace")]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript wird mithilfe der folgenden Befehle ein neuer Log Analytics-Arbeitsbereich in Ihrem Abonnement erstellt. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Get-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/get-azoperationalinsightsworkspace) | Ruft Informationen zu einem vorhandenen Arbeitsbereich ab. |
| [New-AzOperationalInsightsWorkspace](/powershell/module/az.operationalinsights/new-azoperationalinsightsworkspace) | Erstellt einen Arbeitsbereich in der angegebenen Ressourcengruppe und am angegebenen Ort. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

