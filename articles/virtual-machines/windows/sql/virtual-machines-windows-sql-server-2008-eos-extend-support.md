---
title: Verlängerung des Supports für SQL Server 2008 und SQL Server 2008 R2 mit Azure
description: Erfahren Sie, wie Sie den Support für SQL Server 2008 und SQL Server 2008 R2 verlängern, indem Sie Ihre SQL Server-Instanz zu Azure migrieren oder verlängerten Support erwerben, um lokale Instanzen zu behalten.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/08/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: b16fecd6db1f4bed319c832795a2252f8de96c6c
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607139"
---
# <a name="extend-support-for-sql-server-2008-and-sql-server-2008-r2-with-azure"></a>Verlängerung des Supports für SQL Server 2008 und SQL Server 2008 R2 mit Azure

Sowohl SQL Server 2008 als auch SQL Server 2008 R2 nähern sich dem [Ende ihres Supportlebenszyklus (EOS)](https://www.microsoft.com/sql-server/sql-server-2008). Da viele unserer Kunden beide Versionen noch immer verwenden, stellen wir verschiedene Optionen zur Verfügung, um weiterhin Support zu erhalten. Sie können Ihre lokalen SQL Server-Instanzen zu virtuellen Azure-Computern (VMs) migrieren, zu Azure SQL-Datenbank migrieren oder lokal bleiben und erweiterte Sicherheitsupdates erwerben.

Im Gegensatz zu einer verwalteten Instanz ist für die Migration zu einer Azure-VM keine erneute Zertifizierung Ihrer Anwendungen erforderlich. Außerdem erhalten Sie im Gegensatz zur fortgesetzten lokalen Bereitstellung mit der Migration zu einer Azure-VM kostenlose verlängerte Sicherheitspatches.

Im weiteren Verlauf dieses Artikels werden Überlegungen zur Migration Ihrer SQL Server-Instanz zu einer Azure-VM aufgeführt.

## <a name="provisioning"></a>Bereitstellung

Im Azure Marketplace steht ein `SQL Server 2008 R2 on Windows Server 2008 R2`-Image mit nutzungsbasierter Bezahlung zur Verfügung.

Kunden mit SQL Server 2008 müssen selbst eine Installation durchführen oder ein Upgrade auf SQL Server 2008 R2 ausführen. Ebenso müssen Kunden unter Windows Server 2008 die VM von einer benutzerdefinierten VHD bereitstellen oder ein Upgrade auf Windows Server 2008 R2 ausführen.

Auf über den Marketplace bereitgestellten Images ist die SQL-IaaS-Erweiterung bereits installiert. Die SQL-IaaS-Erweiterung ist eine Voraussetzung für die flexible Lizenzierung und automatisierte Patches. Kunden, die selbst installierte VMs bereitstellen, müssen die SQL-IaaS-Erweiterung manuell installieren. Die SQL-IaaS-Erweiterung wird unter Windows 2008 nicht unterstützt.

  > [!NOTE]
  > Die Blätter `Create` und `Manage` in SQL Server funktionieren zwar mit dem SQL Server 2008 R2-Image im Azure-Portal, die folgenden Features werden jedoch _nicht unterstützt_: Automatische Sicherungen, Azure Key Vault-Integration, R-Dienste und Speicherkonfiguration.

## <a name="licensing"></a>Lizenzierung
Bereitstellungen von SQL Server 2008 R2 mit nutzungsbasierter Bezahlung können in den [Azure-Hybridvorteil (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) konvertiert werden.

Um eine Software Assurance-basierte Lizenz in nutzungsbasierte Bezahlung zu konvertieren, sollten sich Kunden beim SQL-VM-[Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md) registrieren. Nach der Registrierung beim SQL-VM-Ressourcenanbieter ist der SQL-Lizenztyp zwischen AHB und nutzungsbasierter Bezahlung austauschbar.

Selbst installierte SQL Server 2008- oder SQL Server 2008 R2-Instanzen auf virtuellen Azure-Computern können beim SQL-Ressourcenanbieter registriert werden, wodurch ihr Lizenztyp auf die nutzungsbasierte Bezahlung umgestellt wird.

## <a name="migration"></a>Migration
Sie können SQL Server-EOS-Instanzen zu einer Azure-VM mit manuellen Sicherungs- und Wiederherstellungsmethoden migrieren. Dies ist die am häufigsten verwendete Migrationsmethode von einer lokalen Bereitstellung zu einer Azure-VM.

### <a name="azure-site-recovery"></a>Azure Site Recovery

Für Massenmigrationsvorgänge empfehlen wir den [Azure Site Recovery](/azure/site-recovery/site-recovery-overview)-Dienst. Mit Azure Site Recovery können Kunden den gesamten virtuellen Computer einschließlich SQL Server von einer lokalen Bereitstellung auf eine Azure-VM replizieren.

SQL Server erfordert App-konsistente Azure Site Recovery-Momentaufnahmen, um die Wiederherstellung zu gewährleisten, und Azure Site Recovery unterstützt App-konsistente Momentaufnahmen in einem Intervall von mindestens 1 Stunde. Das kleinste mögliche RPO für SQL Server mit Azure Site Recovery-Migrationsvorgängen beträgt 1 Stunde, und die RTO beträgt 2 Stunden plus SQL Server-Wiederherstellungszeit.

### <a name="database-migration-service"></a>Database Migration Service

[Database Migration Service](/azure/dms/dms-overview) ist eine Option für Kunden bei der Migration von einer lokalen Bereitstellung zu einer Azure-VM durch Upgrade von SQL Server auf SQL Server 2012 und höher.

## <a name="disaster-recovery"></a>Notfallwiederherstellung

Es gibt folgende Lösungen für die Notfallwiederherstellung von SQL Server am Ende des Supportlebenszyklus auf Azure-VMs:

- **SQL Server-Sicherungen:** Verwenden Sie Azure Backup, um Ihre SQL Server-Instanz am Ende des Supportlebenszyklus vor Ransomware, unbeabsichtigtem Löschen und Beschädigungen zu schützen. Für die Lösung ist derzeit eine Vorschauversion für SQL Server nach dem Ende des Supportlebenszyklus verfügbar. Diese unterstützt unter Windows 2008 R2 SP1 SQL Server 2008 und 2008 R2. Weitere Informationen finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#support-for-sql-server-2008-and-sql-server-2008-r2).
- **Protokollversand:** Sie können ein Protokollversandreplikat in einer anderen Zone oder Azure-Region mit fortlaufenden Wiederherstellungen erstellen, um das RTO zu verringern. Kunden müssen den Protokollversand manuell konfigurieren.
- **Azure Site Recovery**: Sie können Ihren virtuellen Computer mithilfe der Azure Site Recovery-Replikation zonen- und regionsübergreifend replizieren. SQL Server erfordert App-konsistente Momentaufnahmen, um die Wiederherstellung im Notfall zu gewährleisten. Azure Site Recovery bietet für die Notfallwiederherstellung von SQL Server am Ende des Supportlebenszyklus ein RPO von mindestens 1 Stunde und ein RTO von mindestens 2 Stunden + SQL Server-Wiederherstellungszeit.

## <a name="security-patching"></a>Sicherheitspatches
Erweiterte Sicherheitsupdates für SQL Server-VMs werden über die Microsoft Update-Kanäle übermittelt, sobald die SQL Server-VM beim SQL-[Ressourcenanbieter](virtual-machines-windows-sql-register-with-resource-provider.md) registriert wurde. Patches können manuell oder automatisch heruntergeladen werden.

**Automatisiertes Patchen** ist standardmäßig aktiviert. Beim automatisierten Patchen kann Azure automatisch Patches für SQL Server und das Betriebssystem anwenden. Sie können einen Wochentag, eine Uhrzeit und eine Dauer für das Wartungsfenster angeben, wenn die SQL-IaaS-Erweiterung installiert wurde. Azure führt das Patchen in diesem Wartungsfenster durch. Für die Zeitplanung des Wartungsfensters wird die Uhrzeit des VM-Gebietsschemas verwendet.  Weitere Informationen finden Sie unter [Automatisches Patchen für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-automated-patching.md).


## <a name="next-steps"></a>Nächste Schritte

Migrieren Ihrer SQL Server-VM zu Azure

* [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md)

Führen Sie erste Schritte mit SQL Server auf virtuellen Azure-Computern durch:

* [Bereitstellen eines virtuellen Windows-Computers mit SQL Server im Azure-Portal](quickstart-sql-vm-create-portal.md)

Erhalten Sie Antworten auf häufig gestellte Fragen zu virtuellen SQL-Computern:

* [SQL Server auf virtuellen Azure-Computern – FAQ](virtual-machines-windows-sql-server-iaas-faq.md)
