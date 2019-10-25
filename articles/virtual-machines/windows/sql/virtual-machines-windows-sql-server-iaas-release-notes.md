---
title: Dokumentationsänderungen für SQL Server auf virtuellen Azure-Computern | Microsoft-Dokumentation
description: Erfahren Sie mehr über die neuen Funktionen und Verbesserungen für SQL Server auf Azure-VMs.
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/01/2019
ms.openlocfilehash: f5f8985a0b9a97c559016add2567a936220aa910
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72300101"
---
# <a name="documentation-changes-for-sql-server-on-azure-virtual-machines"></a>Dokumentationsänderungen für SQL Server auf virtuellen Azure-Computern

In Azure können Sie einen virtuellen Computer (VM) mit einem integrierten SQL Server-Image bereitstellen. In diesem Artikel werden die Dokumentationsänderungen zusammengefasst, die mit neuen Funktionen und Verbesserungen in den neuesten Releases von [SQL Server in Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/) in Zusammenhang stehen. 


## <a name="october-2019"></a>Oktober 2019

| Änderungen | Details |
| --- | --- |
| **Leistungsoptimierte Speicherkonfiguration** | Sie ist nun möglich, Ihre [Speicherkonfiguration vollständig anzupassen](virtual-machines-windows-sql-server-storage-configuration.md#new-vms), wenn Sie eine neue SQL Server-VM erstellen. |
| **Premium-Dateifreigabe für FCI** | Sie können nun eine Failoverclusterinstanz mit einer [Premium-Dateifreigabe](virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-storage.md) anstelle der ursprünglichen Methode für [direkte Speicherplätze](virtual-machines-windows-portal-sql-create-failover-cluster.md) erstellen. 
| &nbsp; | &nbsp; |

## <a name="august-2019"></a>August 2019

| Änderungen | Details |
| --- | --- |
| **Dedizierter Azure-Host** | Sie können Ihre SQL Server-VM auf einem [dedizierten Azure-Host](virtual-machines-windows-sql-dedicated-host.md) ausführen. |
| &nbsp; | &nbsp; |


## <a name="july-2019"></a>Juli 2019


| Änderungen | Details |
| --- | --- |
| **Verschieben einer SQL-VM in eine andere Region** | Verwenden Sie Azure Site Recovery, um [Ihre SQL Server-VM aus einer Region in eine andere zu migrieren](virtual-machines-windows-sql-move-different-region.md). |
| &nbsp; | &nbsp; |

## <a name="june-2019"></a>Juni 2019


| Änderungen | Details |
| --- | --- |
| **Neue SQL-IaaS-Installationsmodi** | Die SQL Server-IaaS-Erweiterung kann nun im [Lightweight-Modus](virtual-machines-windows-sql-server-agent-extension.md) installiert werden, um den SQL Server-Dienst nicht neu starten zu müssen.  |
| **Änderung der SQL Server-Edition** | Sie können nun die [Editionseigenschaft](virtual-machines-windows-sql-change-edition.md) für Ihren virtuellen SQL Server-Computer ändern. |
| **Änderungen am SQL-VM-Ressourcenanbieter** | Sie können [Ihren virtuellen SQL Server-Computer beim SQL-VM-Ressourcenanbieter registrieren](virtual-machines-windows-sql-register-with-resource-provider.md) und dabei die neuen SQL-IaaS-Modi verwenden. Diese Möglichkeit umfasst auch [Windows 2008](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008-or-2008-r2-on-windows-server-2008-vms)-Images.|
| **Bring-Your-Own-License-Images mit dem Azure-Hybridvorteil** | Bei über den Azure Marketplace bereitgestellten Bring-Your-Own-License-Images kann der [Lizenztyp nun in nutzungsbasierte Bezahlung](virtual-machines-windows-sql-ahb.md#remarks) geändert werden.| 
| &nbsp; | &nbsp; |


## <a name="may-2019"></a>Mai 2019

| Änderungen | Details |
| --- | --- |
| **Neue SQL Server-VM-Verwaltung im Azure-Portal** | Es gibt jetzt eine Methode, mit der Sie Ihre SQL Server-VM im Azure-Portal verwalten können. Weitere Informationen finden Sie unter [Verwalten von SQL Server-VMs über das Azure-Portal](virtual-machines-windows-sql-manage-portal.md).  | 
| &nbsp; | &nbsp; |



## <a name="april-2019"></a>April 2019

| Änderungen | Details |
| --- | --- |
| **Erweiterte Unterstützung für SQL Server 2008/2008 R2** | [Erweiterung der Unterstützung](virtual-machines-windows-sql-server-2008-eos-extend-support.md) für SQL Server 2008 und SQL Server 2008 R2 durch *unverändertes* Migrieren zu einer Azure-VM. | 
| &nbsp; | &nbsp; |


## <a name="march-2019"></a>März 2019

| Änderungen | Details |
| --- | --- |
| **Unterstützung von benutzerdefinierten Images** | Sie können jetzt die [SQL Server-IaaS-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md#installation) für benutzerdefinierte Betriebssystem- und SQL-Images installieren, die die eingeschränkte Funktionalität der [flexiblen Lizenzierung](virtual-machines-windows-sql-ahb.md) bietet. Geben Sie beim Registrieren eines benutzerdefinierten SQL Server-VM-Images beim Ressourcenanbieter als Lizenztyp „AHUB“ an. Andernfalls tritt bei der Registrierung ein Fehler auf. | 
| **Unterstützung für benannte Instanzen** | Sie können jetzt die [SQL Server-IaaS-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md#installation) mit einer benannten Instanz verwenden, wenn die Standardinstanz ordnungsgemäß deinstalliert wurde. | 
| **Portalerweiterung** | Die Azure-Portal-Benutzeroberfläche zur Bereitstellung einer SQL Server-VM wurde überarbeitet, um die Benutzerfreundlichkeit zu verbessern. Weitere Informationen finden Sie in Kurzform im [Schnellstart](quickstart-sql-vm-create-portal.md) und ausführlicher in der [Schrittanleitung](virtual-machines-windows-portal-sql-server-provision.md) zum Bereitstellen einer SQL Server-VM.|
| &nbsp; | &nbsp; |


## <a name="february-2019"></a>Februar 2019

| Änderungen | Details |
| --- | --- |
| **Verbesserung beim Portal** | Es ist jetzt möglich, mithilfe des [Azure-Portals](virtual-machines-windows-sql-ahb.md#change-the-license-for-vms-already-registered-with-the-resource-provider) das Lizenzierungsmodell für einen virtuellen SQL Server-Computer von nutzungsbasierter Bezahlung in Bring-Your-Own-License zu ändern.|
|**Vereinfachung der Bereitstellung von Verfügbarkeitsgruppen mit der Azure SQL Server-VM-CLI** | Es ist jetzt einfacher denn je, eine Verfügbarkeitsgruppe auf einer SQL Server-VM in Azure bereitzustellen. Sie können mit der [Azure-Befehlszeilenschnittstelle](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) den Windows-Failovercluster, den internen Lastenausgleich und die Verfügbarkeitsgruppenlistener über die Befehlszeile erstellen. Weitere Informationen finden Sie unter [Verwenden der Azure SQL Server-VM-CLI zum Konfigurieren von Always On-Verfügbarkeitsgruppen für SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |

## <a name="2018"></a>2018


### <a name="december-2018"></a>Dezember 2018

| Änderungen | Details |
| --- | --- |
| **Neuer Ressourcenanbieter für SQL Server-Cluster** | Ein neuer Ressourcenanbieter (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) definiert die Metadaten des Windows-Failoverclusters. Ein Einbinden einer SQL Server-VM in die *SqlVirtualMachineGroups* startet den Windows Server-Failovercluster-Dienst (WSFC) und verknüpft den virtuellen Computer mit dem Cluster.  |
|**Automatisiertes Einrichten einer Verfügbarkeitsgruppenbereitstellung mit Azure-Schnellstartvorlagen** |Es ist jetzt möglich, mit zwei Azure-Schnellstartvorlagen den Windows-Failovercluster zu erstellen, SQL Server-VMs in diesen einzubinden, den Listener zu erstellen und den internen Lastenausgleich zu konfigurieren. Weitere Informationen finden Sie unter [Verwenden von Azure-Schnellstartvorlagen zum Konfigurieren von Always On-Verfügbarkeitsgruppen für SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Automatische Registrierung beim SQL-VM-Ressourcenanbieter** | SQL Server-VMs, die nach diesem Monat bereitgestellt werden, werden automatisch beim neuen SQL Server-Ressourcenanbieter registriert. SQL Server-VMs, die vor diesem Monat bereitgestellt wurden, müssen weiterhin manuell registriert werden. Weitere Informationen finden Sie unter [Registrieren von virtuellen SQL Server-Computern in Azure mit dem SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md).|
| &nbsp; | &nbsp; |


### <a name="november-2018"></a>November 2018

| Änderungen | Details |
| --- | --- |
| **Neuer SQL-VM-Ressourcenanbieter** |  Ein neuer Ressourcenanbieter (Microsoft.SqlVirtualMachine) bietet eine bessere Verwaltung Ihrer SQL Server-VMs. Weitere Informationen zum Registrieren Ihrer VMs finden Sie unter [Registrieren von virtuellen SQL Server-Computern in Azure mit dem SQL-VM-Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md). |
|**Wechsel des Lizenzierungsmodells** | Sie können nun mit der Azure-Befehlszeilenschnittstelle oder PowerShell für Ihre SQL Server-VM zwischen dem nutzungsbasierten Modell und dem Bring-Your-Own-License-Modell wechseln. Weitere Informationen finden Sie unter [Ändern des Lizenzierungsmodells für einen virtuellen SQL Server-Computer in Azure](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Zusätzliche Ressourcen

**Virtuelle Windows-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Bereitstellen eines virtuellen Windows-Computers mit SQL Server](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrieren einer Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md)
* [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md)
* [Bewährte Methoden für die Leistung von SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-performance.md)
* [Anwendungsmuster und Entwicklungsstrategien für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Virtuelle Linux-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Linux-Computer](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Bereitstellen eines virtuellen Linux-Computers mit SQL Server](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Häufig gestellte Fragen (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [SQL Server unter Linux – Dokumentation](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
