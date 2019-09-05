---
title: SQL Server auf virtuellen Windows-Computern in Azure – FAQ | Microsoft-Dokumentation
description: Dieser Artikel enthält Antworten auf häufig gestellte Fragen zur Ausführung von SQL Server auf Azure-VMs.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: felixwu
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/05/2019
ms.author: mathoma
ms.openlocfilehash: eeda78c69c21fafcbe64071422bf7d73a4737249
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2019
ms.locfileid: "70208321"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-virtual-machines-in-azure"></a>Häufig gestellte Fragen zu SQL Server auf virtuellen Windows-Computern in Azure

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

Dieser Artikel bietet Antworten auf einige der häufigsten Fragen zur Ausführung von [SQL Server auf virtuellen Windows-Computern in Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> In diesem Artikel werden spezifische Probleme mit SQL Server auf virtuellen Windows-Computern behandelt. Wenn Sie SQL Server auf virtuellen Linux-Computern ausführen, finden Sie weitere Informationen unter [Linux – häufig gestellte Fragen](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Images

1. **Welche Images im SQL Server Virtual Machine-Katalog sind verfügbar?** 

   Azure verwaltet Images virtueller Computer für alle unterstützten Hauptversionen von SQL Server in allen Editionen für Windows und Linux. Weitere Informationen finden Sie in der vollständigen Liste der [Windows-VM-Images](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) und [Linux-VM-Images](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create).

1. **Werden vorhandene Images im SQL Server Virtual Machine-Katalog aktualisiert?**

   SQL Server-Images im Katalog der virtuellen Computer werden alle zwei Monate mit den aktuellen Windows- und Linux-Updates aktualisiert. Für Windows-Images umfasst dies alle Updates, die in Windows Update als „wichtig“ gekennzeichnet sind, einschließlich wichtiger SQL Server-Sicherheitsupdates und Service Packs. Für Linux-Images umfasst dies die neuesten Systemupdates. Kumulative Updates für SQL Server werden für Linux und Windows unterschiedlich gehandhabt. Für Linux sind kumulative Updates für SQL Server ebenfalls in der Aktualisierung enthalten. Virtuelle Windows-Computer werden derzeit jedoch nicht mit kumulativen Updates für SQL Server oder Windows Server aktualisiert.

1. **Können Images von virtuellen SQL Server-Computern aus dem Katalog entfernt werden?**

   Ja. Azure behält nur ein Image pro Hauptversion und Edition bei. Wenn beispielsweise ein neues Service Pack für SQL Server veröffentlicht wird, fügt Azure dem Katalog ein neues Image für dieses Service Pack hinzu. Das SQL Server-Image für das vorherige Service Pack wird umgehend aus dem Azure-Portal entfernt. Es bleibt jedoch für drei weitere Monate zur Bereitstellung über PowerShell verfügbar. Nach drei Monaten ist das jeweils vorhergehende Service Pack-Image nicht mehr verfügbar. Diese Entfernungsrichtlinie gilt auch, wenn eine SQL Server-Version nach Ende ihres Lebenszyklus nicht mehr unterstützt wird.


1. **Ist es möglich, ein älteres Image von SQL Server bereitzustellen, das im Azure-Portal nicht angezeigt wird?**

   Ja, durch Verwenden von PowerShell. Weitere Informationen zum Bereitstellen von SQL Server-VMs über PowerShell finden Sie unter [Bereitstellen von SQL Server-VMs mit Azure PowerShell](virtual-machines-windows-ps-sql-create.md).

1. **Kann ich ein generalisiertes Azure SQL Server-Marketplace-Image meiner SQL Server-VM erstellen und zum Bereitstellen von VMs verwenden?**

   Ja, Sie müssen allerdings [alle SQL Server-VMs beim SQL Server-VM-Ressourcenanbieter registrieren](virtual-machines-windows-sql-register-with-resource-provider.md), um Ihre SQL Server-VM im Portal verwalten und Features wie automatisches Patchen und automatische Sicherungen nutzen zu können. Beim Registrieren beim Ressourcenanbieter müssen Sie außerdem den Lizenztyp jeder SQL Server-VM angeben. 

1. **Kann ich eine SQL Server VM mithilfe meiner eigenen VHD bereitstellen?**

   Ja, Sie müssen allerdings [alle SQL Server-VMs beim SQL Server-VM-Ressourcenanbieter registrieren](virtual-machines-windows-sql-register-with-resource-provider.md), um Ihre SQL Server-VM im Portal verwalten und Features wie automatisches Patchen und automatische Sicherungen nutzen zu können.

1. **Ist es möglich, Konfigurationen einzurichten, die nicht im Katalog der virtuellen Computer gezeigt werden (z.B. Windows 2008 R2 + SQL Server 2012) ?**

   Nein. Für Katalogimages von virtuellen Computern, die SQL Server enthalten, müssen Sie eines der bereitgestellten Images entweder über das Azure-Portal oder über [PowerShell](virtual-machines-windows-ps-sql-create.md) auswählen. Sie haben jedoch die Möglichkeit, einen virtuellen Windows-Computer bereitzustellen und SQL Server darauf selbst zu installieren. Sie müssen dann allerdings [die SQL Server-VM beim SQL Server-VM-Ressourcenanbieter registrieren](virtual-machines-windows-sql-register-with-resource-provider.md), um sie im Portal verwalten und Features wie automatisches Patchen und automatische Sicherungen nutzen zu können. 


## <a name="creation"></a>Erstellung

1. **Wie erstelle ich einen virtuellen Azure-Computer mit SQL Server?**

   Die einfachste Methode besteht im Erstellen eines virtuellen Computers, der SQL Server enthält. Ein Tutorial für die Registrierung bei Azure und das Erstellen eines virtuellen SQL Server-Computers im Portal finden Sie unter [Bereitstellen eines virtuellen SQL Server-Computers im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md). Sie können sich für ein VM-Image mit sekundengenau abgerechneter SQL Server-Lizenzierung entscheiden oder ein Image verwenden, das die Verwendung Ihrer eigenen SQL Server-Lizenz ermöglicht. Sie können SQL Server auch manuell auf einem virtuellen Computer mit einer frei lizenzierten Edition (Developer oder Express) oder einer wiederverwendeten lokalen Lizenz installieren. Denken Sie daran, [die SQL Server-VM beim SQL Server-VM-Ressourcenanbieter zu registrieren](virtual-machines-windows-sql-register-with-resource-provider.md), um sie im Portal verwalten und Features wie automatisches Patchen und automatische Sicherungen nutzen zu können. Wenn Sie Ihre eigene Lizenz verwenden, benötigen Sie die [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/). Weitere Informationen finden Sie unter [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Preisinformationen für virtuelle Azure-Computer unter SQL Server).

1. **Wie kann ich meine lokale SQL Server-Datenbank zur Cloud migrieren?**

   Erstellen Sie zuerst einen virtuellen Azure-Computer mit einer SQL Server-Instanz. Migrieren Sie dann Ihre lokalen Datenbanken zu dieser Instanz. Datenmigrationsstrategien finden Sie unter [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md).

## <a name="licensing"></a>Lizenzierung

1. **Wie kann ich meine lizenzierte Kopie von SQL Server auf einer Azure-VM installieren?**

   Dazu stehen drei Methoden zur Auswahl. Wenn Sie ein EA-Kunde (Enterprise Agreement) sind, können Sie eines der [VM-Images, die Lizenzen unterstützen](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), bereitstellen. Dies wird auch als Bring-Your-Own-License (BYOL) bezeichnet. Wenn Sie über [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default) verfügen, können Sie den [Azure-Hybridvorteil](virtual-machines-windows-sql-ahb.md) für ein vorhandenes Image mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYG) aktivieren. Sie können auch die SQL Server-Installationsmedien auf eine Windows Server-VM kopieren und dann SQL Server auf dem virtuellen Computer installieren. Registrieren Sie Ihre SQL Server-VM unbedingt beim [Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md), um Features wie Portalverwaltung, automatisierte Sicherung und automatisiertes Patchen nutzen zu können. 

1. **Muss ich Lizenzgebühren für SQL Server auf einem virtuellen Azure-Computer bezahlen, wenn dieser nur für Standby/Failover verwendet wird?**

   Um eine kostenlose passive Lizenz für eine sekundäre Standbyverfügbarkeitsgruppe oder eine Failoverclusterinstanz zu erhalten, müssen Sie sämtliche der folgenden Kriterien erfüllen, die in der [PDF-Datei der Lizenzanleitung](https://download.microsoft.com/download/7/8/C/78CDF005-97C1-4129-926B-CE4A6FE92CF5/SQL_Server_2017_Licensing_guide.pdf) beschrieben werden:

   1. Sie verfügen über [Lizenzmobilität](https://www.microsoft.com/licensing/licensing-programs/software-assurance-license-mobility?activetab=software-assurance-license-mobility-pivot:primaryr2) durch [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3). 
   1. Die passive SQL Server-Instanz leistet keine Bereitstellung von SQL Server-Daten an Clients oder Ausführung aktiver SQL Server-Workloads. Sie wird nur für die Synchronisierung mit dem primären Server verwendet und verwaltet andernfalls die passive Datenbank in einem betriebsbereiten Standbyzustand. Wenn sie Daten bereitstellt, z. B. Berichte an Clients, die aktive SQL Server-Workloads ausführen, oder „Arbeit“ ausführen (z. B. zusätzliche Sicherungen vom sekundären Server), muss es sich um eine bezahlte lizenzierte SQL Server-Instanz handeln. 
   1. Die aktive SQL Server-Lizenz wird von Software Assurance abgedeckt und lässt **eine** passive sekundäre SQL Server-Instanz mit maximal der gleichen Computegröße wie der lizenzierte aktive Server zu. 
   1. Die sekundäre SQL Server-VM verwendet eines der [Lizenzmodelle](virtual-machines-windows-sql-ahb.md) Bring-Your-Own-License (BYOL) oder Azure-Hybridvorteil (AHB). 

1. **Kann ich einen virtuellen Computer so ändern, dass meine eigene SQL Server-Lizenz verwendet wird, wenn er mithilfe eines der Katalogimages mit nutzungsbasierter Bezahlung erstellt wurde?**

   Ja. Sie können ein Image mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYG) problemlos auf Bring-Your-Own-License (BYOL) umstellen, indem Sie den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/faq/) aktivieren.  Weitere Informationen finden Sie unter [Ändern des Lizenzierungsmodells für eine SQL Server-VM](virtual-machines-windows-sql-ahb.md). Zurzeit ist diese Funktion nur für Public Cloud-Kunden verfügbar.

1. **Sind für den Wechsel des Lizenzierungsmodells Ausfallzeiten für SQL Server erforderlich?**

   Nein. Das [Ändern das Lizenzierungsmodells](virtual-machines-windows-sql-ahb.md) erfordert keine Ausfallzeiten für SQL Server, da die Änderung sofort wirksam werden und kein Neustart des virtuellen Computers erforderlich ist. Um jedoch Ihre SQL Server-VM beim SQL-VM-Ressourcenanbieter zu registrieren, ist die [SQL-IaaS-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) eine Voraussetzung, und durch die Installation der SQL-IaaS-Erweiterung im _Vollmodus_ wird der SQL Server-Dienst neu gestartet. Wenn die SQL-IaaS-Erweiterung installiert werden muss, installieren Sie sie entweder im _Lightweight_-Modus für eingeschränkte Funktionalität, oder installieren Sie sie während eines Wartungsfensters im _Vollmodus_. Die im _Lightweight_-Modus installierte SQL-IaaS-Erweiterung kann jederzeit auf den _Vollmodus_ umgestellt werden. Dazu ist jedoch ein Neustart des SQL Server-Diensts erforderlich. 

1. **Kann ich über das Azure-Portal mehrere Instanzen auf derselben VM verwalten?**

   Nein. Die Portalverwaltung ist ein vom SQL Server-VM-Ressourcenanbieter bereitgestelltes Feature, das auf die SQL Server-IaaS-Agent-Erweiterung angewiesen ist. Daher gelten für den Ressourcenanbieter die gleichen Einschränkungen wie für die Erweiterung. Das Portal kann nur eine Standardinstanz oder eine benannte Instanz verwalten, sofern diese ordnungsgemäß konfiguriert wurde. Weitere Informationen zu diesen Einschränkungen finden Sie unter [SQL Server-IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md). 

1. **Können CSP-Abonnements den Azure-Hybridvorteil aktivieren?**

   Ja, der Azure-Hybridvorteil ist für CSP-Abonnements verfügbar. CSP-Kunden sollten zunächst ein Image mit nutzungsbasierter Bezahlung bereitstellen und dann [das Lizenzierungsmodell ändern](virtual-machines-windows-sql-ahb.md) in BYOL (Bring-Your-Own-License).

1. **Fallen für das Registrieren meines virtuellen Computers beim neuen SQL Server-VM-Ressourcenanbieter zusätzliche Kosten an?**

   Nein. Der SQL Server-VM-Ressourcenanbieter stellt lediglich erweiterte Verwaltungsfunktionen für SQL Server auf virtuellen Azure-Computern ohne zusätzliche Gebühren bereit. 

1. **Ist der SQL Server-VM-Ressourcenanbieter für alle Kunden verfügbar?**
 
   Ja, sofern die SQL Server-VM mit dem Resource Manager-Modell in der Public Cloud bereitgestellt wurde und nicht mit dem klassischen Modell. Alle anderen Kunden können sich für den neuen SQL Server-VM-Ressourcenanbieter registrieren. Jedoch können nur Kunden mit [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3)-Vorteil ihre eigene Lizenz nutzen, indem sie den [Azure-Hybridvorteil (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) auf einer SQL Server-VM aktivieren. 

1. **Was geschieht mit der Ressource des Ressourcenanbieters (_Microsoft.SqlVirtualMachine_), wenn die VM-Ressource verschoben oder gelöscht wird?** 

   Wenn die Ressource Microsoft.Compute/VirtualMachine gelöscht oder verschoben wird, erhält die zugeordnete Ressource Microsoft.SqlVirtualMachine eine Benachrichtigung zum asynchronen Replizieren des Vorgangs.

1. **Was geschieht mit dem virtuellen Computer, wenn die Ressource des Ressourcenanbieters (_Microsoft.SqlVirtualMachine_) gelöscht wird?**

    Die Ressource Microsoft.Compute/VirtualMachine ist nicht betroffen, wenn die Ressource Microsoft.SqlVirtualMachine gelöscht wird. Die Lizenzierungsänderungen werden jedoch wieder auf die ursprüngliche Imagequelle zurückgesetzt. 

1. **Können selbst bereitgestellte SQL Server-VMs beim SQL Server-VM-Ressourcenanbieter registriert werden?**

    Ja. Wenn Sie SQL Server über eigene Medien bereitgestellt und die SQL-IaaS-Erweiterung installiert haben, können Sie die SQL Server-VM beim Ressourcenanbieter registrieren, um in den Genuss der von der SQL-IaaS-Erweiterung gebotenen Vorteile bei der Verwaltung zu kommen. Sie können eine selbst bereitgestellte SQL Server-VM jedoch nicht auf die nutzungsbasierte Bezahlung umstellen.

1. **Ist es möglich, das Lizenzierungsmodell auf einem virtuellen SQL Server-Computer zu wechseln, der mit dem klassischem Modell bereitgestellt wird?**

   Nein. Eine Änderung des Lizenzierungsmodells wird auf einer klassischen VM nicht unterstützt. Sie können Ihre VM zum Azure Resource Manager-Modell migrieren und beim SQL Server-VM-Ressourcenanbieter registrieren. Nach der Registrierung beim SQL Server-VM-Ressourcenanbieter kann das Lizenzierungsmodell für den virtuellen Computer geändert werden. 
   


## <a name="administration"></a>Verwaltung

1. **Kann ich eine zweite Instanz von SQL Server auf dem gleichen virtuellen Computer installieren? Kann ich die installierten Funktionen der Standardinstanz ändern?**

   Ja. Die SQL Server-Installationsmedien befinden sich in einem Ordner auf dem Laufwerk **C** . Führen Sie **Setup.exe** von diesem Speicherort aus, um neue SQL Server-Instanzen hinzuzufügen oder andere installierte Features von SQL Server auf dem Computer zu ändern. Beachten Sie, dass einige Funktionen wie automatisierte Sicherung, automatisiertes Patchen und Azure Key Vault-Integration nur für die Standardinstanz oder eine ordnungsgemäß konfigurierte benannte Instanz (siehe Frage 3) ausgeführt werden können. 

1. **Kann ich die Standardinstanz von SQL Server deinstallieren?**

   Ja. Dabei sind jedoch ein paar Punkte zu beachten: Erstens kann die SQL Server zugeordnete Abrechnung abhängig vom Lizenzmodell für den virtuellen Computer weiterhin stattfinden. Zweitens und wie bereits in der vorhergehenden Antwort erläutert bauen diese Funktionen auf der [SQL Server-IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) auf. Wenn Sie die Standardinstanz deinstallieren, ohne auch die IaaS-Erweiterung zu entfernen, sucht die Erweiterung weiterhin nach der Standardinstanz und generiert möglicherweise Fehler im Ereignisprotokoll. Diese Fehler stammen aus den folgenden beiden Quellen: **Microsoft SQL Server-Verwaltung von Anmeldeinformationen** und **Microsoft SQL Server-IaaS-Agent**. Einer der Fehler kann dem Folgenden ähneln:

      Netzwerkbezogener oder instanzspezifischer Fehler beim Herstellen einer Verbindung mit SQL Server. Der Server wurde nicht gefunden, oder auf ihn kann nicht zugegriffen werden.

   Wenn Sie sich entscheiden, die Standardinstanz zu deinstallieren, deinstallieren Sie auch die [SQL Server-IaaS-Agenterweiterung](virtual-machines-windows-sql-server-agent-extension.md). 

1. **Kann ich eine benannte Instanz von SQL Server mit der IaaS-Erweiterung verwenden?**
   
   Ja, wenn die benannte Instanz die einzige SQL Server-Instanz ist und die ursprüngliche Standardinstanz [ordnungsgemäß deinstalliert](virtual-machines-windows-sql-server-agent-extension.md#install-on-a-vm-with-a-single-named-sql-server-instance) wurde. Wenn es keine Standardinstanz gibt und zugleich mehrere benannte Instanzen auf einer einzelnen SQL Server-VM vorhanden sind, kann die SQL Server-IaaS-Agent-Erweiterung nicht installiert werden. 

1. **Kann ich SQL Server vollständig von einer SQL Server-VM entfernen?**

   Ja, die Abrechnung basiert jedoch weiterhin auf Ihrer SQL Server-VM, wie in den [Preisinformationen für virtuelle Azure-Computer mit SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md) beschrieben. Wenn Sie SQL Server nicht mehr benötigen, können Sie einen neuen virtuellen Computer bereitstellen und die Daten und Anwendungen zu diesem neuen virtuellen Computer migrieren. Anschließend können Sie den virtuellen SQL Server-Computer entfernen.
   
## <a name="updating-and-patching"></a>Aktualisieren und Patchen

1. **Wie wechsle ich auf einem virtuellen Azure-Computer zu einer anderen Version/Edition von SQL Server?**

   Kunden können ihre Version/Edition von SQL Server mithilfe eines Setupmediums ändern, das die gewünschte Version oder Edition von SQL Server enthält. Verwenden Sie nach der Editionsänderung das Azure-Portal, um die Editionseigenschaft des virtuellen Computers anzupassen, sodass sie die Abrechnung für den virtuellen Computer korrekt widerspiegelt. Weitere Informationen finden Sie unter [How to perform an in-place upgrade of SQL Server edition on Azure VM](virtual-machines-windows-sql-change-edition.md) (Ausführen eines direkten Upgrades der SQL Server-Edition auf einem virtuellen Azure-Computer). Es besteht kein Unterschied bei der Abrechnung für verschiedene Versionen von SQL Server. Nach dem Ändern der SQL Server-Version ist keine weitere Aktion erforderlich.

1. **Wo kann ich das Setupmedium zum Ändern der Edition oder Version von SQL Server erhalten?**

  Kunden, die über [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) verfügen, können Ihre Installationsmedien vom [Volume Licensing Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) erhalten. Kunden ohne Software Assurance können die Setupmedien aus einem SQL Server-VM-Image mit der gewünschten Edition im Marketplace verwenden.

1. **Wie werden Updates und Servicepacks auf eine SQL Server-VM angewendet?**

   Virtuelle Computer bieten Ihnen die Kontrolle über die Hostcomputer, einschließlich des Zeitpunkts, zu dem Updates angewendet werden sollen, und der Art und Weise. Für das Betriebssystem können Sie manuell Windows-Updates anwenden oder einen Planungsdienst namens [Automatisiertes Patchen](virtual-machines-windows-sql-automated-patching.md) aktivieren. Automatisiertes Patchen installiert alle wichtigen Updates, einschließlich SQL Server-Updates in dieser Kategorie. Andere optionale Updates für SQL Server müssen manuell installiert werden.

1. **Kann ich meine SQL Server 2008/2008 R2-Instanz aktualisieren, nachdem sie beim SQL Server-VM-Ressourcenanbieter registriert wurde?**

   Ja. Sie können ein beliebiges Setupmedium für ein Upgrade der Version und Edition von SQL Server verwenden. Anschließend können Sie den [SQL-IaaS-Erweiterungsmodus](virtual-machines-windows-sql-register-with-resource-provider.md#change-management-modes) von _NoAgent_ auf _Vollständig_ upgraden. Dadurch erhalten Sie Zugriff auf alle Vorteile der SQL-IaaS-Erweiterung, z. B. die Verwaltung im Portal, automatisierte Sicherungen und automatisiertes Patchen. 

## <a name="general"></a>Allgemein

1. **Werden SQL Server-Failoverclusterinstanzen (FCI) auf Azure-VMs unterstützt?**

   Ja. Sie können [einen Windows-Failovercluster unter Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) erstellen und für die Speicherung des Clusters das Feature „Direkte Speicherplätze“ (S2D) verwenden. Alternativ können Sie, wie unter [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions) beschrieben, Cluster- oder Speicherlösungen von Drittanbietern nutzen.

   > [!IMPORTANT]
   > Derzeit wird die _vollständige_ [SQL Server-IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) nicht für SQL Server-Failoverclusterinstanzen (FCI) in Azure unterstützt. Es wird empfohlen, die Erweiterung für den Modus _Vollständig_ von virtuellen Computern in den FCI zu deinstallieren und die Erweiterung stattdessen im _Lightweight_-Modus zu installieren. Diese Erweiterung unterstützt Features wie automatisierte Sicherung und automatisiertes Patchen sowie einige Portalfeatures für SQL Server. Diese Features sind nach der Deinstallation des _vollständigen_ Agents nicht mehr für SQL Server-VMs einsetzbar.

1. **Was ist der Unterschied zwischen SQL Server-VMs und dem SQL-Datenbank-Dienst?**

   Im Prinzip unterscheidet sich die Ausführung von SQL Server auf virtuellen Azure-Computern nicht von der Ausführung in einem Remoterechenzentrum. Im Gegensatz dazu bietet [SQL-Datenbank](../../../sql-database/sql-database-technical-overview.md) Database-as-a-Service. Mit SQL-Datenbank haben Sie keinen Zugriff auf die Computer, die Ihre Datenbanken hosten. Einen vollständigen Vergleich finden Sie unter [Wählen Sie eine SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Wie installiere ich SQL Data-Tools auf meiner Azure-VM?**

    Laden Sie die SQL Data-Tools von [Microsoft SQL Server Data Tools – Business Intelligence für Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313) herunter, und installieren Sie sie.

1. **Werden verteilte Transaktionen mit MSDTC auf SQL Server-VMs unterstützt?**
   
    Ja. Lokaler DTC wird für SQL Server 2016 SP2 und höher unterstützt. Allerdings müssen Anwendungen getestet werden, wenn Always On-Verfügbarkeitsgruppen zum Einsatz kommen, da bei den während eines Failovers ausgeführten Transaktionen ein Fehler auftritt und die Transaktionen wiederholt werden müssen. Gruppierter DTC ist ab Windows Server 2019 verfügbar. 

## <a name="resources"></a>Ressourcen

**Virtuelle Windows-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Windows-Computer](virtual-machines-windows-sql-server-iaas-overview.md)
* [Bereitstellen eines virtuellen Windows-Computers mit SQL Server](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrieren einer Datenbank zu SQL Server auf einer Azure-VM](virtual-machines-windows-migrate-sql.md)
* [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md)
* [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md)
* [Anwendungsmuster und Entwicklungsstrategien für SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Virtuelle Linux-Computer:**

* [Übersicht über SQL Server auf einem virtuellen Linux-Computer](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Bereitstellen eines virtuellen Linux-Computers mit SQL Server](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Häufig gestellte Fragen (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [SQL Server unter Linux – Dokumentation](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
