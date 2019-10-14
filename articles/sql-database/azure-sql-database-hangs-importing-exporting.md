---
title: Der Import-/Exportdienst von Azure SQL Datenbank benötigt viel Zeit, um eine Datenbank zu importieren oder zu exportieren | Microsoft-Dokumentation
description: Der Import-/Exportdienst von Azure SQL Datenbank benötigt viel Zeit, um eine Datenbank zu importieren oder zu exportieren
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
author: v-miegge
ms.author: ramakoni
ms.reviewer: ''
ms.date: 09/27/2019
ms.openlocfilehash: ed5f3d1cd505270eb91c9cfbd6fb5c38b908f33d
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71975369"
---
# <a name="azure-sql-database-importexport-service-takes-a-long-time-to-import-or-export-a-database"></a>Der Import-/Exportdienst von Azure SQL Datenbank benötigt viel Zeit, um eine Datenbank zu importieren oder zu exportieren

Wenn Sie den Import-/Exportdienst von Azure SQL-Datenbank verwenden, bemerken Sie möglicherweise, dass der Vorgang manchmal lange dauert. Dieser Artikel bietet zusätzliche Informationen zu den möglichen Ursachen für die Verzögerungen und alternative Methoden, die Sie verwenden können, um diese Probleme zu umgehen.

## <a name="azure-sql-database-importexport-service"></a>Import-/Exportdienst von Azure SQL-Datenbank

Der Import-/Exportdienst von Azure SQL-Datenbank ist ein REST-basierter Webdienst, der in jedem Microsoft Azure-Rechenzentrum ausgeführt wird. Es handelt sich um den Dienst, der aufgerufen wird, wenn Sie entweder die Optionen [Datenbank importieren](https://docs.microsoft.com/azure/sql-database/sql-database-import#import-from-a-bacpac-file-in-the-azure-portal) oder [Exportieren](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-the-azure-portal) verwenden, um Ihre SQL-Datenbank im Microsoft Azure-Portal zu verschieben. Der Dienst bietet einen kostenlosen Warteschlangendienst für Anforderungen und einen kostenlosen Computedienst, um Importe und Exporte aus einer Microsoft Azure SQL-Datenbank in Microsoft Azure Blob Storage (Binary Large Object) durchzuführen.

Die Import- und Exportvorgänge sind keine herkömmliche physische Datenbanksicherung, sondern eine logische Sicherung der Datenbank, die ein spezielles BACPAC-Format verwendet. Mit diesem logischen BACPAC-Format können Sie vermeiden, ein physisches Format zu verwenden, das sich zwischen den Versionen von SQL Server und SQL-Datenbank unterscheiden kann. Daher können Sie mithilfe dieses Formats die Datenbank auf sichere Weise in einer SQL-Datenbank und in einer SQL Server-Datenbank wiederherstellen.

## <a name="what-causes-the-process-to-take-a-long-time"></a>Ursache für die lange Ausführungszeit des Prozesses

Der Import-/Exportdienst von Azure SQL-Datenbank bietet eine begrenzte Anzahl von virtuellen Computern (VMs) pro Region, um die Import- und Exportvorgänge zu verarbeiten. Die Compute-VM wird pro Region gehostet, um sicherzustellen, dass regionsübergreifende Bandbreitenverzögerungen und -gebühren beim Import oder Export vermieden werden. Wenn daher zu viele Anforderungen gleichzeitig in derselben Region ausgegeben werden, kommt es zu erheblichen Verzögerungen bei der Verarbeitung der Vorgänge. Die für das Ausführen von Anforderungen erforderliche Zeit kann von einigen Sekunden bis hin zu vielen Stunden variieren.

> [!NOTE]
> Wenn eine Anforderung nicht innerhalb von vier Tagen verarbeitet wird, bricht der Dienst die Anforderung automatisch ab.

## <a name="recommended-solutions"></a>Empfohlene Lösungen

Wenn Ihre Datenbankexporte nur für die Wiederherstellung nach versehentlichem Löschen von Daten verwendet werden, bieten alle Editionen von Azure SQL Server-Datenbank Self-Service-Wiederherstellungsfunktionen aus vom System generierten Sicherungen. Wenn Sie diese Exporte aber aus anderen Gründen benötigen und eine konsistente oder besser vorhersagbare Import- oder Exportleistung benötigen, sollten Sie die folgenden Optionen in Betracht ziehen:

* [Exportieren in eine BACPAC-Datei mit dem Hilfsprogramm SQLPackage](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-the-sqlpackage-utility)
* [Exportieren in eine BACPAC-Datei mit SQL Server Management Studio (SSMS)](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-sql-server-management-studio-ssms)
* Direktes Ausführen des BACPAC-Imports oder -Exports direkt in Ihrem Code mithilfe der DacFx-API (Microsoft® SQL Server® Data-Tier Application Framework). Weitere Informationen finden Sie unter:
  * [Exportieren einer Datenebenenanwendung](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
  * [Microsoft.SqlServer.Dac-Namespace](https://docs.microsoft.com/dotnet/api/microsoft.sqlserver.dac)
  * [DACFx herunterladen](https://www.microsoft.com/download/details.aspx?id=55713)

## <a name="considerations-when-exporting-or-importing-an-azure-sql-database"></a>Überlegungen beim Exportieren oder Importieren einer Azure SQL-Datenbank

* Für alle in diesem Artikel erläuterten Methoden wird das DTU-Kontingent aufgebraucht, was eine Drosselung durch den Azure SQLDB-Dienst zur Folge hat. Sie können [die DTU-Statistiken für die Datenbank im Azure-Portal anzeigen](https://docs.microsoft.com/azure/sql-database/sql-database-monitor-tune-overview#monitor-database-performance). Wenn die Datenbank an Ressourcenlimits stößt, [führen Sie ein Upgrade der Dienstebene aus](https://docs.microsoft.com/azure/sql-database/sql-database-scale-resources), um weitere Ressourcen hinzuzufügen.
* Clientanwendungen (z.B. das sqlpackage-Hilfsprogramm oder Ihre benutzerdefinierte DAC-Anwendung) sollten idealerweise von einem virtuellen Computer (VM) in derselben Region wie die SQL-Datenbank ausgeführt werden. Andernfalls können aufgrund von Netzwerklatenz Leistungsprobleme auftreten.
* Das Exportieren von großen Tabellen ohne gruppierte Indizes kann sehr langsam sein oder sogar zu einem Fehler führen. Dies liegt daran, dass die Tabelle nicht parallel aufgeteilt und exportiert werden kann und in einer einzelnen Transaktion exportiert werden muss. Dies verursacht die Verlangsamung und potenzielle Fehler beim Export, insbesondere bei großen Tabellen. 


## <a name="related-documents"></a>Verwandte Dokumente

[Überlegungen beim Exportieren einer Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-export#considerations-when-exporting-an-azure-sql-database)
