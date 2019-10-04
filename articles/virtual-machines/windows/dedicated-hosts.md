---
title: Übersicht über dedizierte Azure-Hosts für virtuelle Computer | Microsoft-Dokumentation
description: Erfahren Sie mehr darüber, wie dedizierte Azure-Hosts für die Bereitstellung virtueller Computer verwendet werden können.
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/25/2019
ms.author: cynthn
ms.openlocfilehash: 011bfeb337f3c04b2d9041abedac50affe1f86b0
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68977260"
---
# <a name="preview-azure-dedicated-hosts"></a>Vorschau: Dedizierte Azure-Hosts

Azure Dedicated Host ist ein Dienst zum Bereitstellen physischer Server, die virtuelle Computer hosten können und ausschließlich für ein Azure-Abonnement reserviert sind. Dedizierte Hosts sind die in unseren Rechenzentren verwendeten physischen Server, die als Ressource bereitgestellt werden. Sie können dedizierte Hosts in einer Region, einer Verfügbarkeitszone und einer Fehlerdomäne bereitstellen. Anschließend können Sie VMs direkt in Ihren bereitgestellten Hosts platzieren, und zwar in einer beliebigen Konfiguration, die Ihren Anforderungen am besten entspricht.

[!INCLUDE [virtual-machines-common-dedicated-hosts-preview](../../../includes/virtual-machines-common-dedicated-hosts-preview.md)]

[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]

## <a name="next-steps"></a>Nächste Schritte

- Sie können einen dedizierten Host über [Azure PowerShell](dedicated-hosts-powershell.md), das [Portal](dedicated-hosts-portal.md) und die [Azure-Befehlszeilenschnittstelle](../linux/dedicated-hosts-cli.md) bereitstellen.

- [Hier](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) finden Sie eine Beispielvorlage, die sowohl Zonen als auch Fehlerdomänen für maximale Resilienz in einer Region verwendet.
