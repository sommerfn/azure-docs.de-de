---
title: SQL Server-VM auf einem dedizierten Azure-Host
description: Erfahren Sie mehr darüber, wie eine SQL Server-VM auf einem dedizierten Azure-Host ausgeführt wird.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/12/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 8b29bbce1511b022def522d46c74b99967a76ea3
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71204481"
---
# <a name="sql-server-vm-on-an-azure-dedicated-host"></a>SQL Server-VM auf einem dedizierten Azure-Host 

In diesem Artikel werden die Besonderheiten erläutert, die es für ein Verwenden einer SQL Server-VM mit einem [dedizierten Azure-Host](/azure/virtual-machines/windows/dedicated-hosts) gibt. Weitere Informationen zu dedizierten Azure-Hosts bzw. zum Dienst Azure Dedicated Host finden Sie im Blogbeitrag [Einführung von Azure Dedicated Host](https://azure.microsoft.com/blog/introducing-azure-dedicated-host/). 

## <a name="overview"></a>Übersicht
[Azure Dedicated Host](/azure/virtual-machines/windows/dedicated-hosts) ist ein Dienst zum Bereitstellen physischer Server, die virtuelle Computer hosten können und ausschließlich für ein Azure-Abonnement reserviert sind. Dedizierte Hosts sind die in Microsofts Rechenzentren verwendeten physischen Server, die als Ressource bereitgestellt werden. Sie können dedizierte Hosts in einer Region, einer Verfügbarkeitszone und einer Fehlerdomäne bereitstellen. Anschließend können Sie VMs direkt in Ihren bereitgestellten Hosts platzieren, und zwar in einer beliebigen Konfiguration, die Ihren Anforderungen am besten entspricht.


[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-common-dedicated-hosts-preview.md)]


## <a name="licensing"></a>Lizenzierung

Sie können zwischen zwei verschiedenen Lizenzierungsoptionen wählen, wenn Sie Ihre SQL Server-VM einem dedizierten Azure-Host hinzufügen. 

  - **SQL VM licensing** (Lizenzierung virtueller SQL-Computer): Dies ist die vorhandene Lizenzierungsoption, bei der Sie jede SQL Server-VM-Lizenz einzeln bezahlen. 
  - **Dedicated host licensing** (Lizenzierung eines dedizierten Hosts): Das neue Lizenzierungsmodell, das für Azure Dedicated Host verfügbar ist und in dem SQL Server-Lizenzen gebündelt sind und auf Hostebene bezahlt werden. 


Optionen auf Hostebene für das Verwenden vorhandener SQL Server-Lizenzen: 
  - SQL Server Enterprise Edition-Azure-Hybridvorteil
    - Verfügbar für Kunden mit SA oder Abonnement.
    - Lizenzieren Sie alle verfügbaren physischen Kerne, und profitieren Sie von unbegrenzter Virtualisierung (bis zur maximalen Anzahl von vCPUs, die vom Host unterstützt wird).
        - Weitere Informationen zum Anwenden des Azure-Hybridvorteils auf einen dedizierten Azure-Host finden Sie in [Häufig gestellte Fragen zum Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/faq/). 
  - SQL Server-Lizenzen, die vor dem 1. Oktober erworben wurden
      - SQL Server Enterprise Edition hat beide Lizenzoptionen: Hostebene und VM-bezogen. 
      - Für SQL Server Standard Edition ist nur die Lizenzoption „VM-bezogen“ verfügbar. 
          - Ausführliche Informationen finden Sie unter [Produktbestimmungen von Microsoft](https://www.microsoft.com/licensing/product-licensing/products). 
  - Wenn keine dedizierte Hostebenenoption für SQL Server ausgewählt ist, kann SQL Server-Azure-Hybridvorteil auf der Ebene einzelner virtueller Computer ausgewählt werden, genau wie bei mehrinstanzenfähigen virtuellen Computern.



## <a name="provisioning"></a>Bereitstellung  
Das Bereitstellen einer SQL Server-VM auf dem dedizierten Host unterscheidet sich nicht von der Vorgehensweise für andere virtuelle Azure-Computer. Sie können dazu [Azure PowerShell](../dedicated-hosts-powershell.md), das [Azure-Portal](../dedicated-hosts-portal.md) und die [Azure-Befehlszeilenschnittstelle](../../linux/dedicated-hosts-cli.md) verwenden.

Das Hinzufügen einer vorhandenen SQL Server-VM zu dem dedizierten Host erfordert Ausfallzeit, wirkt sich jedoch nicht auf Daten aus und bewirkt keinen Datenverlust. Trotzdem sollten vor dem Verschieben alle Datenbanken, einschließlich der Systemdatenbanken, gesichert werden.

## <a name="virtualization"></a>Virtualisierung 

Einer der Vorteile eines dedizierten Hosts ist die unbegrenzte Virtualisierung. Beispielsweise können Sie über Lizenzen für 64 virtuelle Kerne verfügen, können den Host aber so konfigurieren, dass er 128 virtuelle Kerne hat. Sie erhalten also die doppelte Anzahl von virtuellen Kernen, zahlen aber nur für die Hälfte der SQL Server-Lizenzen. 

Da dieser Host Ihr Host ist, können Sie die Virtualisierung mit einem Verhältnis von 1:2 festlegen. 

## <a name="faq"></a>Häufig gestellte Fragen

**F: Wie funktioniert der Azure-Hybridvorteil für Windows Server- bzw. SQL Server-Lizenzen mit Azure Dedicated Host?**

A: Kunden können den Wert Ihrer vorhandenen Windows Server- und SQL Server Lizenzen mit Software Assurance oder qualifizierenden Abonnementlizenzen verwenden, um einen verringerten Preis für Azure Dedicated Host mit Azure-Hybridvorteil zu zahlen. Windows Server Datacenter und SQL Server Enterprise Edition-Kunden erhalten eine unbegrenzte Virtualisierung (Bereitstellen so vieler virtueller Windows Server-Computer wie auf dem Host möglich, abhängig von der physischen Kapazität des zugrunde liegenden Servers), wenn sie den gesamten Host lizenzieren und Azure-Hybridvorteil verwenden.  Für alle Windows Server- und SQL Server-Workloads in Azure Dedicated Host können zudem erweiterte Sicherheitsupdates für Windows Server und SQL Server 2008/R2 ohne zusätzliche Kosten in Anspruch genommen werden. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln: 

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-iaas-faq.md)
* [Preisinformationen für SQL Server auf virtuellen Windows-Computern](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server auf Windows-VMs – Versionshinweise](virtual-machines-windows-sql-server-iaas-release-notes.md)


