---
title: 'Schnellstart: Verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation'
description: Hier werden die ersten Schritte mit verwalteten Azure SQL-Datenbank-Instanzen beschrieben.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlr
ms.date: 07/11/2019
ms.openlocfilehash: 65b6b503d107b36813d1716348ce5f11fa840cc0
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71937212"
---
# <a name="getting-started-with-azure-sql-database-managed-instance"></a>Erste Schritte mit verwalteten Azure SQL-Datenbank-Instanzen

Bei der Bereitstellungsoption [Verwaltete Instanz](sql-database-managed-instance-index.yml) wird eine Datenbank mit nahezu uneingeschränkter Kompatibilität mit der aktuellen lokalen SQL Server-Datenbank-Engine (Enterprise Edition) erstellt. Darüber hinaus bietet sie eine native Implementierung eines [virtuellen Netzwerks (VNET)](../virtual-network/virtual-networks-overview.md) zur Behebung allgemeiner Sicherheitsrisiken sowie ein vorteilhaftes [Geschäftsmodell](https://azure.microsoft.com/pricing/details/sql-database/) für Kunden mit lokaler SQL Server-Instanz. In diesem Artikel erfahren Sie, wie Sie schnell eine verwaltete Instanz konfigurieren und erstellen und Ihre Datenbanken migrieren.

## <a name="quickstart-overview"></a>Schnellstartübersicht

In den folgenden Schnellstarts erfahren Sie, wie Sie schnell eine verwaltete Instanz erstellen, eine VM oder eine Point-to-Site-VPN-Verbindung für Clientanwendungen konfigurieren und eine Datenbank mithilfe einer `.bak`-Datei in Ihrer neuen verwalteten Instanz wiederherstellen.

### <a name="configure-environment"></a>Konfigurieren der Umgebung

Zunächst müssen Sie Ihre verwaltete Instanz mit der Netzwerkumgebung erstellen, in der sie angeordnet werden soll. Außerdem müssen Sie die Verbindung vom Computer bzw. virtuellen Computer aktivieren, auf dem Sie Abfragen für die verwaltete Instanz ausführen. Sie können die folgenden Leitfäden verwenden:

- [Erstellen einer verwalteten Instanz im Azure-Portal](sql-database-managed-instance-get-started.md). Im Azure-Portal können Sie die erforderlichen Parameter (Benutzername/Kennwort, Anzahl von Kernen und maximale Speichermenge) konfigurieren und automatisch die Azure-Netzwerkumgebung erstellen, ohne sich mit Netzwerkdetails oder Infrastrukturanforderungen befassen zu müssen. Sie müssen lediglich sicherstellen, dass Sie über einen [Abonnementtyp](sql-database-managed-instance-resource-limits.md#supported-subscription-types) verfügen, für den derzeit die Erstellung einer verwalteten Instanz zulässig ist. Wenn Sie ein eigenes Netzwerk verwenden oder das Netzwerk anpassen möchten, helfen Ihnen die Informationen unter [Konfigurieren eines vorhandenen virtuellen Netzwerks für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-configure-vnet-subnet.md) oder [Erstellen eines virtuellen Netzwerks für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-create-vnet-subnet.md) weiter.
- Eine verwaltete Instanz wird in einem eigenen VNET ohne öffentlichen Endpunkt erstellt. Für den Clientanwendungszugriff können Sie wie in einem der folgenden Schnellstartanleitungen beschrieben **eine VM in demselben VNET (in einem anderen Subnetz)** oder **eine Point-to-Site-VPN-Verbindung mit dem VNET auf Ihrem Clientcomputer erstellen**:
  - Aktivieren Sie den [öffentlichen Endpunkt](sql-database-managed-instance-public-endpoint-configure.md) auf Ihrer verwalteten Instanz, um direkt aus Ihrer Umgebung auf Ihre Daten zuzugreifen.
  - Für die Konnektivität von Clientanwendungen (einschließlich SQL Server Management Studio) erstellen Sie eine [Azure-VM im VNET der verwalteten Instanz](sql-database-managed-instance-configure-vm.md).
  - Richten Sie auf dem Clientcomputer, auf dem Sie SQL Server Management Studio und andere Anwendungen mit Clientkonnektivität ausführen, eine [Point-to-Site-VPN-Verbindung mit Ihrer verwalteten Instanz](sql-database-managed-instance-configure-p2s.md) ein. Dies ist eine der zwei Optionen für die Konnektivität mit Ihrer verwalteten Instanz und dem zugehörigen VNET.

  > [!NOTE]
  > Sie können auch ExpressRoute oder eine Site-to-Site-Verbindung über Ihr lokales Netzwerk verwenden. Diese Methoden werden in den Schnellstarts jedoch nicht behandelt.

Als Alternative zur manuellen Erstellung der verwalteten Instanz können Sie mithilfe von [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md), [PowerShell mit einer Resource Manager-Vorlage](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md) oder der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) (Azure CLI) ein Skript erstellen und diesen Prozess automatisieren.

### <a name="migrate-your-databases"></a>Migrieren Ihrer Datenbanken

Nachdem Sie eine verwaltete Instanz erstellt und den Zugriff konfiguriert haben, können Sie mit der Migration Ihrer Datenbanken von lokalen SQL Server-Instanzen oder Azure-VMs beginnen. Die Migration schlägt fehl, wenn die zu migrierende Quelldatenbank nicht unterstützte Features enthält. Um Fehler zu vermeiden und die Kompatibilität zu überprüfen, können Sie den [Datenmigrations-Assistenten (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) installieren. Dieser analysiert Ihre Datenbanken in SQL Server und ermittelt sämtliche Probleme, die die Migration zu einer verwalteten Instanz verhindern können (beispielsweise das Vorhandensein von [FileStream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) oder mehreren Protokolldateien). Nachdem Sie diese Probleme behoben haben, sind Ihre Datenbanken bereit für die Migration zur verwalteten Instanz. Der [Assistent für Datenbankexperimente](https://blogs.msdn.microsoft.com/datamigration/2018/08/06/release-database-experimentation-assistant-dea-v2-6/) ist ein nützliches Tool, das Ihre Workload in SQL Server aufzeichnen und in der verwalteten Instanz wiedergeben kann, um zu ermitteln, ob nach der Migration zu einer verwalteten Instanz Leistungsprobleme zu erwarten sind.

Sobald Sie sicher sind, dass Ihre Datenbank zu einer verwalteten Instanz migriert werden kann, können Sie die nativen Wiederherstellungsfunktionen von SQL Server verwenden, um eine Datenbank aus einer `.bak`-Datei in einer verwalteten Instanz wiederherzustellen. Sie können diese Methode verwenden, um Datenbanken von der SQL Server-Datenbank-Engine, die lokal installiert ist, oder von virtuellen Azure-Computern zu migrieren. Einen Schnellstart finden Sie unter [Wiederherstellen einer Datenbank aus einer Sicherung in einer verwalteten Instanz](sql-database-managed-instance-get-started-restore.md). In diesem Schnellstart stellen Sie eine Datenbank mit dem Transact-SQL-Befehl `RESTORE` aus einer in Azure Blob Storage gespeicherten `.bak`-Datei wieder her.

> [!TIP]
> Informationen zum Erstellen einer Sicherung Ihrer Datenbank in Azure Blob Storage mit dem Transact-SQL-Befehl `BACKUP` finden Sie unter [SQL Server-Sicherung über URLs](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

In diesen Schnellstarts erfahren Sie, wie Sie schnell eine Datenbanksicherung erstellen, konfigurieren und in einer verwalteten Instanz wiederherstellen. In einigen Szenarien müssen Sie die Bereitstellung von verwalteten Instanzen und der erforderlichen Netzwerkumgebung anpassen oder automatisieren. Diese Szenarien werden im Anschluss beschrieben.

## <a name="customize-network-environment"></a>Anpassen der Netzwerkumgebung

Wenn die Instanz über das [Azure-Portal](sql-database-managed-instance-get-started.md) erstellt wird, kann das VNET/Subnetz zwar automatisch konfiguriert werden, manchmal empfiehlt es sich jedoch, es vor der Erstellung verwalteter Instanzen zu erstellen, da Sie in diesem Fall die Parameter des VNETs/Subnetzes konfigurieren können. Die einfachste Möglichkeit zum Erstellen und Konfigurieren der Netzwerkumgebung ist die Verwendung der [Azure-Vorlage für die Ressourcenbereitstellung](sql-database-managed-instance-create-vnet-subnet.md). Diese erstellt und konfiguriert Ihr Netzwerk und das Subnetz, in dem die Instanz platziert wird. Sie müssen lediglich auf die Bereitstellungsschaltfläche von Azure Resource Manager klicken und das Formular mit Parametern ausfüllen.

Alternativ können Sie die Erstellung des Netzwerks auch mit diesem [PowerShell-Skript](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) automatisieren.

Wenn Sie bereits über ein VNET und ein Subnetz verfügen, in dem Sie Ihre verwaltete Instanz bereitstellen möchten, müssen diese die [Netzwerkanforderungen](sql-database-managed-instance-connectivity-architecture.md#network-requirements) erfüllen. Verwenden Sie dieses [PowerShell-Skript, um zu überprüfen, ob Ihr Subnetz korrekt konfiguriert ist](sql-database-managed-instance-configure-vnet-subnet.md). Das Skript überprüft Ihr Netzwerk, meldet Probleme, informiert Sie über erforderliche Änderungen und bietet anschließend an, die nötigen Änderungen an Ihrem VNET/Subnetz vorzunehmen. Führen Sie dieses Skript aus, wenn Sie Ihr VNET/Subnetz nicht manuell konfigurieren möchten. Sie können es auch nach jeder größeren Neukonfigurationen Ihrer Netzwerkinfrastruktur ausführen. Wenn Sie Ihr eigenes Netzwerk erstellen und konfigurieren möchten, lesen Sie [Verbindungsarchitektur](sql-database-managed-instance-connectivity-architecture.md) und [The ultimate guide for creating and configuring Azure SQL Managed Instance environment](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01) (Ultimativer Leitfaden für das Erstellen und Konfigurieren einer Umgebung für die verwaltete Azure SQL-Instanz).

## <a name="migrate-to-a-managed-instance"></a>Migrieren zu einer verwalteten Instanz

In den Artikeln in diesen Schnellstarts erfahren Sie, wie Sie schnell eine verwaltete Instanz einrichten und Ihre Datenbanken mithilfe der nativen `RESTORE`-Funktion migrieren. Dies ist ein guter Ausgangspunkt, wenn Sie schnelle Proof of Concepts durchführen und sicherstellen möchten, dass Ihre Lösung auf der verwalteten Instanz ausgeführt werden kann. 

Um jedoch Ihre Produktionsdatenbank oder sogar Entwicklungs-/Testdatenbanken zu migrieren, die Sie für Leistungstests verwenden möchten, müssen Sie den Einsatz einiger zusätzlicher Verfahren in Erwägung ziehen, wie zum Beispiel:
- Leistungstests: Sie sollten die Baselineleistung auf Ihrer SQL Server-Quellinstanz messen und mit der Leistung auf der verwalteten Zielinstanz vergleichen, zu der Sie die Datenbank migriert haben. Erfahren Sie mehr über die [bewährten Methoden für den Leistungsvergleich](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210).
- Onlinemigration: Bei Verwendung der in diesem Artikel beschriebenen nativen `RESTORE`-Funktion müssen Sie jedoch warten, bis die Datenbanken wiederhergestellt wurden (und in Azure Blob Storage kopiert wurden, sofern sie dort noch nicht gespeichert sind). Dies führt insbesondere bei größeren Datenbanken zu einer Downtime Ihrer Anwendung. Verwenden Sie für Ihre Produktionsdatenbank [Data Migration Service (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json), um die Datenbank mit minimaler Downtime zu migrieren. DMS pusht die in Ihrer Quelldatenbank vorgenommenen Änderungen inkrementell in die Datenbank der verwalteten Instanz, die wiederhergestellt wird, und minimiert so die Downtime. Dadurch können Sie Ihre Anwendung schnell und mit minimaler Downtime von der Quell- auf die Zieldatenbank umstellen.

Erfahren Sie mehr über den [empfohlenen Migrationsprozess](sql-database-managed-instance-migrate.md).

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich mit der [allgemeinen Liste mit unterstützten Features in verwalteten Instanzen](sql-database-features.md) sowie mit [Details und bekannten Problemen](sql-database-managed-instance-transact-sql-information.md) vertraut.
- Informieren Sie sich über [technische Eigenschaften der verwalteten Instanz](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).
- Weiterführende Schrittanleitungen finden Sie unter [Verwenden einer verwalteten Instanz in Azure SQL-Datenbank](sql-database-howto-managed-instance.md).
- [Identifizieren der richtigen Azure SQL Datenbank-SKU/Verwaltete-Instanz-SKU für Ihre lokale Datenbank](/sql/dma/dma-sku-recommend-sql-db/).
