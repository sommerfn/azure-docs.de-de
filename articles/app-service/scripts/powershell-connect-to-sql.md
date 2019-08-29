---
title: 'Azure PowerShell-Skriptbeispiel: Herstellen einer Verbindung zwischen einer App und einer SQL-Datenbank | Microsoft-Dokumentation'
description: 'Azure PowerShell-Skriptbeispiel: Herstellen einer Verbindung zwischen einer App Service-App und einer SQL-Datenbank'
services: app-service\web
documentationcenter: ''
author: syntaxc4
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 055440a9-fff1-49b2-b964-9c95b364e533
ms.service: app-service
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 799fb67b20c45653ec741a92ec7a75aff2f8c509
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70087825"
---
# <a name="connect-an-app-service-app-to-a-sql-database"></a>Herstellen einer Verbindung zwischen einer App Service-App und einer SQL-Datenbank

In diesem Szenario erfahren Sie, wie Sie eine Azure SQL-Datenbank und eine App Service-App erstellen. Anschließend verknüpfen Sie die SQL-Datenbank mithilfe von App-Einstellungen mit der App.

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview), und führen Sie dann `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen.

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect an app to a SQL database")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die App Service-App und alle zugehörigen Ressourcen entfernt werden.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | Erstellt einen App Service-Plan. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Erstellt eine App Service-App. |
| [New-AzSQLServer](/powershell/module/az.sql/new-azsqlserver) | Erstellt einen SQL-Datenbank-Server. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Erstellt eine Firewallregel für einen SQL-Datenbank-Server. |
| [New-AzSQLDatabase](/powershell/module/az.sql/new-azsqldatabase) | Erstellt eine Datenbank oder eine elastische Datenbank. |
| [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) | Ändert die Konfiguration einer App Service-App. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service finden Sie unter [PowerShell samples for Azure App Service](../samples-powershell.md) (PowerShell-Beispiele für Azure App Service).
