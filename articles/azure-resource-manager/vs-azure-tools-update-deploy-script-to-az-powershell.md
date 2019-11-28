---
title: Aktualisieren des Vorlagenbereitstellungsskripts von Visual Studio für die Verwendung von Az PowerShell
description: Aktualisieren des Visual Studio-Vorlagenbereitstellungsskripts von AzureRM auf Az PowerShell
author: cweining
ms.topic: quickstart
ms.date: 09/27/2019
ms.author: cweining
ms.openlocfilehash: c34cde5593b48c301826be3dd2641dc2490ee88d
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2019
ms.locfileid: "74149070"
---
# <a name="updating-the-visual-studio-template-deployment-script-to-use-az-powershell"></a>Aktualisieren des Visual Studio-Vorlagenbereitstellungsskripts für die Verwendung von Az PowerShell

Visual Studio 16.4 unterstützt die Verwendung von Az PowerShell im Vorlagenbereitstellungsskript. Az PowerShell wird von Visual Studio nicht installiert, und das Bereitstellungsskript im Ressourcengruppenprojekt wird nicht automatisch aktualisiert. Um die neuere Az PowerShell-Version zu verwenden, müssen Sie diese vier Schritte ausführen:
1. Deinstallieren von AzureRM PowerShell
1. Installieren von Az PowerShell
1. Aktualisieren auf Visual Studio 16.4
1. Aktualisieren des Bereitstellungsskripts in Ihrem Projekt

## <a name="uninstall-azurerm-powershell"></a>Deinstallieren von AzureRM PowerShell
Befolgen Sie diese [Anweisungen](https://docs.microsoft.com/powershell/azure/uninstall-az-ps?view=azps-2.7.0#uninstall-the-azurerm-module) zum Deinstallieren von AzureRM PowerShell.

## <a name="install-az-powershell"></a>Installieren von Az PowerShell
Befolgen Sie diese [Anweisungen](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-2.7.0) zum Installieren von Az PowerShell.

## <a name="update-visual-studio-to-164"></a>Aktualisieren von Visual Studio auf Version 16.4
Führen Sie eine Aktualisierung auf Visual Studio 16.4 aus (wenn verfügbar). Stellen Sie während des Upgrades sicher, dass die Azure PowerShell-Komponente nicht aktiviert ist. Da Sie Az PowerShell aus dem Katalog installiert haben, benötigen Sie nicht die AzureRM-Version von PowerShell, die zusammen mit Visual Studio installiert wird.

Wenn Sie bereits ein Upgrade auf 16.4 ausgeführt haben und die Azure PowerShell-Komponente aktiviert war, können Sie sie deinstallieren. Führen Sie dazu den Visual Studio-Installer aus, und deaktivieren Sie die Azure PowerShell-Komponente in der Azure-Workload bzw. auf der Seite mit den einzelnen Komponenten.

## <a name="update-the-deployment-script-in-your-project"></a>Aktualisieren des Bereitstellungsskripts in Ihrem Projekt
Ersetzen Sie alle Vorkommen der Zeichenfolge „AzureRm“ im Bereitstellungsskript durch „Az“. Sie können die Überarbeitungen in diesem [Gist](https://gist.github.com/cweining/d2da2479418ea403499c4306dcf4f619) einsehen, um die Änderungen zu verfolgen. Weitere Informationen zum Aktualisieren von Skripts auf Az PowerShell finden Sie in dieser [Dokumentation](https://docs.microsoft.com/powershell/azure/migrate-from-azurerm-to-az?view=azps-2.5.0).


