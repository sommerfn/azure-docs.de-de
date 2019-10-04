---
title: Azure PowerShell-Skriptbeispiel – Erstellen einer Web-App und Bereitstellen von Code über GitHub | Microsoft-Dokumentation
description: Azure PowerShell-Skriptbeispiel – Erstellen einer Web-App und Bereitstellen von Code über GitHub
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: app-service-web
ms.workload: web
ms.topic: sample
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 9f6785b84d66ac2728c8fcd72b40eaf20b56372f
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70087795"
---
# <a name="create-a-web-app-and-deploy-code-from-github"></a>Erstellen einer Web-App und Bereitstellen von Code über GitHub

Dieses Beispielskript erstellt eine Web-App in App Service mit den zugehörigen Ressourcen und stellt dann Ihren Web-App-Code über ein öffentliches GitHub-Repository bereit (ohne Continuous Deployment). Informationen zur GitHub-Bereitstellung mit Continuous Deployment finden Sie unter [Erstellen einer Web-App mit Continuous Deployment über GitHub](powershell-continuous-deployment-github.md).

Installieren Sie bei Bedarf Azure PowerShell anhand der Anleitung im [Azure PowerShell-Handbuch](/powershell/azure/overview), und führen Sie dann `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen. Darüber hinaus benötigen Sie eine Verknüpfung mit dem GitHub-Repository, das den Code der Web-App enthält.

## <a name="sample-script"></a>Beispielskript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/deploy-github/deploy-github.ps1?highlight=1-2 "Create a web app and deploy code from GitHub")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach dem Ausführen des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen entfernt werden.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | Erstellt einen App Service-Plan. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Erstellt die Web-App. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Ändert eine Ressource in einer Ressourcengruppe. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche Azure PowerShell-Beispiele für Azure App Service-Web-Apps finden Sie unter [Azure PowerShell-Beispiele](../samples-powershell.md).
