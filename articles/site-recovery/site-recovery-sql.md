---
title: Einrichten der Notfallwiederherstellung für SQL Server mit SQL Server und Azure Site Recovery | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Notfallwiederherstellung für SQL Server mithilfe von SQL Server und Azure Site Recovery einrichten.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/30/2019
ms.author: sutalasi
ms.openlocfilehash: 1c44b10b54a5f58dff1aecf36c3633cc8ffbd8f0
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491778"
---
# <a name="set-up-disaster-recovery-for-sql-server"></a>Einrichten der Notfallwiederherstellung für SQL Server

In diesem Artikel erfahren Sie, wie Sie das SQL Server-Back-End einer Anwendung mit einer Kombination aus SQL Server-Technologien für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR) und [Azure Site Recovery](site-recovery-overview.md) schützen.

Informieren Sie sich zunächst unbedingt über die SQL Server-Funktionen zur Notfallwiederherstellung, einschließlich Failoverclustering, Always On-Verfügbarkeitsgruppen, Datenbankspiegelung, Protokollversand, aktive Georeplikation und Autofailover-Gruppen.

## <a name="dr-recommendation-for-integration-of-sql-server-bcdr-technologies-with-site-recovery"></a>DR-Empfehlungen für die Integration von SQL Server-BCDR-Technologien in Site Recovery

Die Auswahl einer BCDR-Technologie für die Wiederherstellung von SQL-Servern sollte auf Ihren RTO- und RPO-Bedürfnissen basieren. Weitere Informationen dazu finden Sie in der folgenden Tabelle. Sobald Sie sich entschieden haben, können Sie Site Recovery mit dem Failovervorgang der Technologie integrieren, um die Wiederherstellung Ihrer gesamten Anwendung zu orchestrieren.

**Bereitstellungstyp** | **BCDR Technology** | **Erwartete RTO für SQL** | **Erwartete RPO für SQL** |
--- | --- | --- | ---
SQL Server auf Azure-IaaS-VM oder auf lokalem System| **[AlwaysOn-Verfügbarkeitsgruppe](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-2017)** | Entspricht der Zeit, die es dauert, das sekundäre Replikat zum primären zu machen | Die Replikation ist asynchron zum sekundären Replikat, da etwas Datenverlust auftreten kann.
SQL Server auf Azure-IaaS-VM oder auf lokalem System| **[Failoverclustering (Always On-FCI)](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server?view=sql-server-2017)** | Entspricht der Zeit, die das Failover zwischen Knoten benötigt | Es verwendet den freigegebenen Speicher, deswegen ist dieselbe Ansicht für Speicherinstanzen bei Failover verfügbar.
SQL Server auf Azure-IaaS-VM oder auf lokalem System| **[Datenbankspiegelung (Hochleistungsmodus)](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server?view=sql-server-2017)** | Entspricht der Zeit, die es bedarf, den Dienst zu erzwingen, der den Spiegelserver als betriebsbereiten Standbyserver nutzt. | Die Replikation ist asynchron. Bei der Spiegeldatenbank kann im Vergleich zur Prinzipaldatenbank zu etwas Verzögerung auftreten. Der Abstand ist normalerweise gering, doch er kann sich erweitern, wenn die Systeme des Prinzipal- oder Spiegelservers stark ausgelastet sind.<br></br>Der Protokollversand kann die Datenbankspiegelung unterstützen und ist eine beliebte Alternative zu einer asynchronen Datenbankspiegelung.
SQL als PaaS auf Azure<br></br>(Elastische Pools, SQL-Datenbank-Server) | **Aktive Georeplikation** | 30 Sekunden, sobald sie ausgelöst wird<br></br>Wenn das Failover auf eine sekundäre Datenbank aktiviert ist, werden alle anderen sekundären Datenbanken automatisch mit der neuen primären Datenbank verknüpft. | RPO von 5 Sekunden<br></br>Aktive Georeplikation nutzt die Always On-Technologie von SQL Server, um Transaktionen mit ausgeführtem Commit in der primären Datenbank asynchron mit Momentaufnahmeisolation in eine sekundäre Datenbank zu replizieren. <br></br>Die sekundären Daten haben niemals partielle Transaktionen.
SQL als PaaS ist mit aktiver Georeplikation auf Azure konfiguriert<br></br>(Verwaltete SQL-Datenbank-Instanz, elastische Pools, SQL-Datenbankserver) | **Autofailover-Gruppen** | RTO von 1 Stunde | RPO von 5 Sekunden<br></br>Gruppen für automatisches Failover stellen die Gruppensemantik über der aktiven Georeplikation bereit, es wird aber der gleiche asynchrone Replikationsmechanismus verwendet.
SQL Server auf Azure-IaaS-VM oder auf lokalem System| **Replikation mit Azure Site Recovery** | In der Regel weniger als 15 Minuten. [Erfahren Sie mehr](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/) über RTO SLA von Azure Site Recovery. | 1 Stunde für die Anwendungskonsistenz und 5 Minuten für die Absturzkonsistenz. 

> [!NOTE]
> Wenn Sie SQL-Workloads mit Azure Site Recovery schützen, sollten Sie einige Erwägungen in Betracht ziehen:
> * Azure Site Recovery ist anwendungsunabhängig. Insofern können Sie eine beliebige Version eines SQL-Servers, die auf einem unterstützten Betriebssystem ausgeführt wird, mit Azure Site Recovery schützen. [Weitere Informationen](vmware-physical-azure-support-matrix.md#replicated-machines)
> * Sie können Site Recovery für eine Bereitstellung unter Azure, Hyper-V, VMware oder in einer physische Infrastruktur verwenden. Halten Sie sich an die [Anweisungen](site-recovery-sql.md#how-to-protect-a-sql-server-cluster-standard-editionsql-server-2008-r2) am Ende des Dokuments, um Ihren SQL-Servercluster mit Azure Site Recovery zu schützen.
> * Stellen Sie sicher, dass die Datenänderungsrate (geschriebene Bytes pro Sekunde) auf dem Computer innerhalb der [Site Recovery-Grenzwerte](vmware-physical-azure-support-matrix.md#churn-limits) liegen. Bei Windows-Computern finden Sie dies in der Registerkarte „Leistung“ im Task-Manager. Beachten Sie die Schreibgeschwindigkeit für jeden Datenträger.
> * Azure Site Recovery unterstützt die Replikation von Failoverclusterinstanzen auf Storage Spaces Direct. [Weitere Informationen](azure-to-azure-how-to-enable-replication-s2d-vms.md)
 

## <a name="disaster-recovery-of-application"></a>Notfallwiederherstellung der Anwendung

**Azure Site Recovery orchestriert den Testfailover und den Failover Ihrer gesamten Anwendung mithilfe von Wiederherstellungsplänen.** 

Es gibt einige Voraussetzungen, um sicherzustellen, dass der Wiederherstellungsplan vollständig nach Ihren Bedürfnissen angepasst ist. Für jede Bereitstellung eines SQL Servers ist normalerweise ein Active Directory erforderlich. Und Sie benötigen eine Verbindung für Ihre Logikschicht.

### <a name="step-1-set-up-active-directory"></a>Schritt 1: Einrichten von Active Directory

Richten Sie für den ordnungsgemäßen Betrieb von SQL Server Active Directory am sekundären Wiederherstellungsstandort ein.

* **Kleine Unternehmen**: Wenn nur eine geringe Anzahl von Anwendungen und ein einzelner Domänencontroller für den lokalen Standort vorhanden sind und Sie ein Failover für den gesamten Standort durchführen möchten, empfiehlt es sich, den Domänencontroller mithilfe der Site Recovery-Replikation im sekundären Datencenter oder in Azure zu replizieren.
* **Mittlere bis große Unternehmen**: Wenn Sie über eine hohe Anzahl von Anwendung verfügen, eine Active Directory-Gesamtstruktur verwenden und ein anwendungs- oder workloadspezifisches Failover durchführen möchten, empfiehlt sich die Einrichtung eines zusätzlichen Domänencontrollers im sekundären Datencenter oder in Azure. Wenn Sie für die Wiederherstellung an einem Remotestandort Always On-Verfügbarkeitsgruppen verwenden, empfiehlt es sich, einen weiteren zusätzlichen Domänencontroller für den sekundären Standort oder für Azure einzurichten, der für die wiederhergestellte SQL Server-Instanz verwendet werden kann.

Bei den Anweisungen in diesem Artikel wird davon ausgegangen, dass am sekundären Standort ein Domänencontroller verfügbar ist. [weiteren Informationen](site-recovery-active-directory.md) zum Schützen von Active Directory mit Site Recovery.

### <a name="step-2-ensure-connectivity-with-other-application-tiers-and-web-tier"></a>Schritt 2: Stellen Sie die Verbindung mit anderen Logikschichten und Webschichten sicher

Stellen Sie sicher, dass Sie eine Verbindung mit der Anwendung und der Webebene haben, sobald die Datenbankebene in der Azure-Zielregion läuft. Sie sollten die notwendigen schritte vorab durchführen, damit Sie die Konnektivität mit einem Testfailover überprüfen können.

Hier sind einige Beispiele für ein besseres Verständnis der Konnektivitätserwägungen beim Entwickeln von Anwendungen:
* [Entwickeln Sie eine Anwendung für die Cloud-Notfallwiederherstellung](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* [Notfallwiederherstellungsstrategien für elastische Pools](../sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)

### <a name="step-3-integrate-with-always-on-active-geo-replication-or-auto-failover-groups-for-application-failover"></a>Schritt 3: Integration mit Always On, aktiver Georeplikation oder Autofailover-Gruppen für Anwendungsfailover

Die BCDR-Technologien Always On, aktive Georeplikation und Autofailover-Gruppen haben sekundäre Replikate eines SQL-Servers in der Azure-Zielregion. Deswegen muss der erste Schritt bei Ihrem Anwendungsfailover die Bestimmung dieses Replikats als primäres sein (wir gehen davon aus, dass Sie an sekundärer Stelle bereits einen Domänencontroller haben). Dieser Schritt mag nicht notwendig sein, wenn Sie sich für ein Autofailover entscheiden. Erst nachdem der Datenbankfailover vorbei ist, sollten Sie es für Ihre Web- oder Logikschichten ausführen.

> [!NOTE] 
> Wenn Sie die SQL-Computer mit Azure Site Recovery geschützt haben, dann müssen Sie nur eine Wiederherstellungsgruppe mit diesen Computern erstellen und ihre Failover in den Wiederherstellungsplan einfügen.

[Erstellen Sie einen Wiederherstellungsplan](site-recovery-create-recovery-plans.md) mit Logik- und Webschichten auf virtuellen Computern. Folgen Sie diesen Schritten, um der Datenbankebene ein Failover hinzuzufügen:

1. Importieren Sie Skripts in Ihr Azure Automation-Konto. Hier sind die Skripts zum Durchführen eines Failovers für die SQL-Verfügbarkeitsgruppe auf einem [mit Resource Manager erstellten virtuellen Computer](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) und einem [klassischen virtuellen Computer](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![In Azure bereitstellen](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. Fügen Sie ASR-SQL-FailoverAG als vorausgehende Aktion der ersten Gruppe des Wiederherstellungsplans hinzu.

1. Befolgen Sie die im Skript verfügbaren Anweisungen, um eine Automation-Variable zu erstellen, um den Namen der Verfügbarkeitsgruppen bereitzustellen.

### <a name="step-4-conduct-a-test-failover"></a>Schritt 4: Führen Sie ein Testfailover aus

Einige BCDR-Technologien wie SQL Always On unterstützten Testfailover nicht nativ. Deswegen empfehlen wir den folgenden Ansatz  **nur, wenn Sie ihn in solche Technologien integrieren möchten**:

1. Richten Sie [Azure Backup](../backup/backup-azure-arm-vms.md) auf dem virtuellen Computer ein, auf dem das Replikat der Verfügbarkeitsgruppe in Azure gehostet wird.

1. Vor dem Auslösen des Testfailovers des Wiederherstellungsplans stellen Sie den virtuellen Computer aus der im vorherigen Schritt erstellten Sicherung wieder her.

    ![Wiederherstellen von Daten aus der Azure-Sicherung](./media/site-recovery-sql/restore-from-backup.png)

1. [Erzwingen Sie ein Quorum](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) auf dem virtuellen Computer, der aus einer Sicherung wiederhergestellt wurde.

1. Aktualisieren Sie die IP-Adresse der Listener in eine IP-Adresse, die im Testfailovernetzwerk verfügbar ist.

    ![Aktualisieren Sie die Listener-IP-Adresse](./media/site-recovery-sql/update-listener-ip.png)

1. Schalten Sie die Listener online.

    ![Onlineschalten des Listeners](./media/site-recovery-sql/bring-listener-online.png)

1. Erstellen Sie einen Lastenausgleich mit einer IP-Adresse, die im Front-End-IP-Pool entsprechend jedem Verfügbarkeitsgruppenlistener erstellt wurde und der über einen virtuellen SQL-Computer im Back-End-Pool verfügt.

     ![Erstellen eines Lastenausgleichs: Front-End-IP-Pool](./media/site-recovery-sql/create-load-balancer1.png)

    ![Erstellen eines Lastenausgleichs: Back-End-Pool](./media/site-recovery-sql/create-load-balancer2.png)

1. Fügen Sie in diesem Wiederherstellungsplan in denfolgenden Wiederherstellungsgruppen zuerst Ihrer Logikschicht einen Failover hinzu, dann der Webschicht. 
1. Führen Sie einen Testfailover des Wiederherstellungsplans durch, um das End-to-End-Failover der Anwendung zu prüfen.

## <a name="steps-to-do-a-failover"></a>Schritte zum Failover

Sobald Sie das Skript in Schritt 3 im Wiederherstellungsplan hinzugefügt haben und den es durch ein Testfailover mit dem speziellen Ansatz aus Schritt 4 validiert haben, können Sie das Failover des Wiederherstellungsplans aus Schritt 3 durchführen.

Bitte beachten Sie, dass die Failoverschritte für Anwendungen und Webschichten bei beiden Testfailovern und Failover-Wiederherstellungsplänen identisch sein sollten.

## <a name="how-to-protect-a-sql-server-cluster-standard-editionsql-server-2008-r2"></a>So schützen Sie einen SQL Server-Cluster (Standard Edition/SQL Server 2008 R2)

Bei einem Cluster mit SQL Server Standard Edition oder SQL Server 2008 R2 empfiehlt es sich, SQL Server mit der Site Recovery-Replikation zu schützen.

### <a name="azure-to-azure-and-on-premises-to-azure"></a>Azure zu Azure und lokale Bereitstellung auf Azure

Site Recovery unterstützt bei der Replikation in einer Azure-Region keine Gastcluster. Für die Standard-Edition von SQL Server steht zudem keine kostengünstige Notfallwiederherstellungslösung zur Verfügung. In diesem Szenario empfiehlt es sich, den SQL Server-Cluster mit einer eigenständigen SQL Server-Instanz am primären Standort zu schützen und die Wiederherstellung am sekundären Standort vorzunehmen.

1. Konfigurieren Sie eine zusätzliche eigenständige SQL Server-Instanz in der primären Azure-Region oder an einem lokalen Standort.
1. Konfigurieren Sie diese Instanz so, dass sie als Spiegelung für die zu schützenden Datenbanken fungiert. Konfigurieren Sie die Spiegelung im Modus für hohe Sicherheit.
1. Konfigurieren Sie Site Recovery am primären Standort ([Azure](azure-to-azure-tutorial-enable-replication.md), [Hyper-V](site-recovery-hyper-v-site-to-azure.md) oder [VMware VMs/physische Server)](site-recovery-vmware-to-azure-classic.md).
1. Verwenden Sie die Site Recovery-Replikation, um die neue SQL Server-Instanz zum sekundären Standort zu replizieren. Da es sich hierbei um eine Spiegelkopie mit hoher Sicherheit handelt, wird sie mit dem primären Cluster synchronisiert. Bei der Replikation wird allerdings die Site Recovery-Replikation verwendet.


![Standardcluster](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Überlegungen zum Failback

Bei SQL Server Standard-Clustern ist für das Failback nach einem nicht geplanten Failover eine SQL-Serversicherung und -wiederherstellung der Spiegelinstanz im ursprünglichen Cluster mit Wiederherstellung der Spiegelung erforderlich.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="how-does-sql-get-licensed-when-protected-with-azure-site-recovery"></a>Wie erhält SQL eine Lizenz, wenn es durch Azure Site Recovery geschützt ist?
Die Azure Site Recovery-Replikation für SQL Server ist für alle Azure Site Recovery-Szenarios im Software Assurance-Vorteil für die Notfallwiederherstellung enthalten (Azure-Notfallwiederherstellung von lokalen Standorten oder regionsübergreifende Azure IaaS-Notfallwiederherstellung). [hier](https://azure.microsoft.com/pricing/details/site-recovery/)

### <a name="will-azure-site-recovery-support-my-sql-version"></a>Unterstützt Azure Site Recovery meine SQL-Version?
Azure Site Recovery ist anwendungsunabhängig. Insofern können Sie eine beliebige Version eines SQL-Servers, die auf einem unterstützten Betriebssystem ausgeführt wird, mit Azure Site Recovery schützen. [Weitere Informationen](vmware-physical-azure-support-matrix.md#replicated-machines)

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über die [Site Recovery-Architektur](site-recovery-components.md).
* Erfahren Sie mehr über SQL Servers in Azure und [hochverfügbare Lösungen](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions) für die Wiederherstellung in einer sekundären Azure-Region.
* Erfahren Sie mehr über SQL Database in Azure und [Geschäftskontinuität](../sql-database/sql-database-business-continuity.md) und [Hochverfügbarkeit](../sql-database/sql-database-high-availability.md) für die Wiederherstellung in sekundären Azure-Regionen.
* [Erfahren Sie mehr](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#hybrid-it-disaster-recovery-solutions) über SQL-Servercomputer an lokalen Standorten und die Hochverfügbarkeitsoptionen für die Wiederherstellung in Azure Virtual Machines.
