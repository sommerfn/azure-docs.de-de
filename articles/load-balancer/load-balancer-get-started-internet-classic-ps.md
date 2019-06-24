---
title: Erstellen eines Load Balancers mit Internetzugriff – Azure PowerShell (klassisch)
titlesuffix: Azure Load Balancer
description: Erfahren Sie, wie Sie einen Load Balancer mit Internetzugriff im klassischen Modus mithilfe von PowerShell erstellen.
services: load-balancer
documentationcenter: na
author: genlin
manager: cshepard
tags: azure-service-management
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: genli
ms.openlocfilehash: 54ef8782620a387d60454023d0e446279e467a99
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "64730330"
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Erste Schritte zum Erstellen eines Load Balancers (klassisch) mit Internetzugriff in PowerShell

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure-Befehlszeilenschnittstelle](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Bevor Sie mit Azure-Ressourcen arbeiten, müssen Sie sich darüber im Klaren sein, dass Azure derzeit über zwei Bereitstellungsmodelle verfügt: Azure Resource Manager und klassische Bereitstellung. Stellen Sie sicher, dass Sie die [Bereitstellungsmodelle und -tools](../azure-classic-rm.md) verstanden haben, bevor Sie mit Azure-Ressouren arbeiten. Zum Anzeigen der Dokumentation für verschiedene Tools klicken Sie auf die Registerkarten oben in diesem Artikel. Dieser Artikel gilt für das klassische Bereitstellungsmodell. Informationen zum Erstellen eines Load Balancers mit Internetzugriff unter Verwendung des Azure-Ressourcen-Managers finden Sie [hier](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-load-balancer-using-powershell"></a>Einrichten des Load Balancers mit PowerShell

Führen Sie die folgenden Schritte aus, um einen Lastenausgleich mit PowerShell einzurichten:

1. Wenn Sie Azure PowerShell noch nie verwendet haben, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview) , und befolgen Sie die komplette Anleitung, um sich bei Azure anzumelden und Ihr Abonnement auszuwählen.
2. Nach dem Erstellen eines virtuellen Computers können Sie PowerShell-Cmdlets verwenden, um einem virtuellen Computer in demselben Clouddienst einen Load Balancer hinzuzufügen.

Im folgenden Beispiel fügen Sie dem Clouddienst „mytestcloud“ (oder „myctestcloud.cloudapp.net“) eine Load Balancer-Gruppe namens „webfarm“ hinzu. Dabei werden die Endpunkte für den Load Balancer den virtuellen Computern „web1“ und „web2“ hinzugefügt. Der Load Balancer empfängt Netzwerkdatenverkehr an Port 80 und führt für die durch den lokalen Endpunkt (in diesem Fall Port 80) festgelegten virtuellen Computer per TCP einen Lastenausgleich durch.

### <a name="step-1"></a>Schritt 1

Erstellen eines Endpunkts mit Lastenausgleich für den ersten virtuellen Computer „web1“

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

### <a name="step-2"></a>Schritt 2

Erstellen eines weiteren Endpunkts für den zweiten virtuellen Computer „web2“ unter Verwendung desselben Namens für die Gruppe mit Lastenausgleich

```powershell
Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM
```

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Entfernen eines virtuellen Computers aus einem Load Balancer

Sie können mithilfe von „Remove-AzureEndpoint“ den Endpunkt eines virtuellen Computers aus dem Load Balancer entfernen.

```powershell
Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin | Update-AzureVM
```

## <a name="next-steps"></a>Nächste Schritte

Sie können auch einen [internen Lastenausgleich erstellen](load-balancer-get-started-ilb-classic-ps.md) und die Art des [Verteilungsmodus](load-balancer-distribution-mode.md) für ein bestimmtes Datenverkehrsverhalten im Netzwerk konfigurieren.

Wenn es für Ihre Anwendung erforderlich ist, dass Verbindungen mit Servern hinter einem Lastenausgleich aufrechterhalten werden, informieren Sie sich über [TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md). Es ist hilfreich, sich über das Verhalten von Leerlaufverbindungen zu informieren, wenn Sie Azure Load Balancer verwenden.
