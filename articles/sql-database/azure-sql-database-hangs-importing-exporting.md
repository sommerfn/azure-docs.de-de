---
title: Der Import/Export-Dienst dauert sehr lange.
description: Der Import-/Exportdienst von Azure SQL Datenbank benötigt viel Zeit, um eine Datenbank zu importieren oder zu exportieren
ms.custom: seo-lt-2019
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
author: v-miegge
ms.author: ramakoni
ms.reviewer: ''
ms.date: 09/27/2019
ms.openlocfilehash: 29edd7da42a5eb4586cd52cc0f913a56e199a5ba
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73808279"
---
# <a name="azure-sql-database-importexport-service-takes-a-long-time-to-import-or-export-a-database"></a>Der Import-/Exportdienst von Azure SQL Datenbank benötigt viel Zeit, um eine Datenbank zu importieren oder zu exportieren

Wenn Sie den Import-/Exportdienst Azure SQL-Datenbank verwenden, kann der Vorgang länger dauern als erwartet. In diesem Artikel werden die möglichen Gründe für diese Verzögerung sowie alternative Methoden zur Umgehung dieses Problems beschrieben.

## <a name="azure-sql-database-importexport-service"></a>Import-/Exportdienst von Azure SQL-Datenbank

Der Import-/Exportdienst von Azure SQL-Datenbank ist ein REST-basierter Webdienst, der in jedem Azure-Rechenzentrum ausgeführt wird. Dieser Dienst wird aufgerufen, wenn Sie im Azure-Portal die Option [Datenbank importieren](https://docs.microsoft.com/azure/sql-database/sql-database-import#import-from-a-bacpac-file-in-the-azure-portal) oder [Exportieren](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-the-azure-portal) verwenden, um Ihre SQL-Datenbank zu verschieben. Der Dienst bietet kostenlose Anforderungswarteschlangen und Computedienste für Importe und Exporte zwischen einer Azure SQL-Datenbank und Azure Blob Storage.

Bei den Import- und Exportvorgängen handelt es sich nicht um eine herkömmliche physische Datenbanksicherung, sondern um eine logische Sicherung der Datenbank in einem speziellen BACPAC-Format. Das BACPAC-Format ermöglicht die Vermeidung eines physischen Formats, das sich möglicherweise zwischen den Versionen von Microsoft SQL Server und Azure  SQL-Datenbank unterscheidet. Daher können Sie die Datenbank mithilfe dieses Formats auf sichere Weise in einer SQL Server-Datenbank und in einer SQL-Datenbank wiederherstellen.

## <a name="what-causes-delays-in-the-process"></a>Wodurch wird der Prozess verzögert?

Der Import-/Exportdienst von Azure SQL-Datenbank stellt für die Verarbeitung von Import- und Exportvorgängen eine begrenzte Anzahl von Compute-VMs (virtual machines, virtuelle Computer) pro Region bereit. Die Compute-VMs werden pro Region gehostet, um regionsübergreifende Bandbreitenverzögerungen und -gebühren bei Import- oder Exportvorgängen zu vermeiden. Eine zu hohe Anzahl gleichzeitiger Anforderungen in einer Region kann zu erheblichen Verzögerungen bei der Vorgangsverarbeitung führen. Die für die Anforderungsausführung benötigte Zeit kann wenige Sekunden, aber auch mehrere Stunden betragen.

> [!NOTE]
> Wenn eine Anforderung nicht innerhalb von vier Tagen verarbeitet wird, bricht der Dienst die Anforderung automatisch ab.

## <a name="recommended-solutions"></a>Empfohlene Lösungen

Wenn Ihre Datenbankexporte nur für die Wiederherstellung nach versehentlichem Löschen von Daten verwendet werden, bieten alle Editionen von Azure SQL-Datenbank eine Self-Service-Wiederherstellungsfunktion auf der Grundlage von systemgenerierten Sicherungen. Falls diese Exporte aber aus anderen Gründen erforderlich sind und Sie eine konsistente oder besser planbare Import-/Exportleistung benötigen, ziehen Sie die folgenden Optionen in Betracht:

* [Exportieren in eine BACPAC-Datei mit dem Hilfsprogramm „SQLPackage“](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-the-sqlpackage-utility)
* [Exportieren in eine BACPAC-Datei mit SQL Server Management Studio (SSMS)](https://docs.microsoft.com/azure/sql-database/sql-database-export#export-to-a-bacpac-file-using-sql-server-management-studio-ssms)
* Direktes Ausführen des BACPAC-Imports oder -Exports in Ihrem Code mithilfe der Microsoft SQL Server Data-Tier Application Framework-API (DACFx). Weitere Informationen finden Sie hier:
  * [Exportieren einer Datenebenenanwendung](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application)
  * [Microsoft.SqlServer.Dac-Namespace](https://docs.microsoft.com/dotnet/api/microsoft.sqlserver.dac)
  * [DACFx herunterladen](https://www.microsoft.com/download/details.aspx?id=55713)

## <a name="things-to-consider-when-you-export-or-import-an-azure-sql-database"></a>Wichtige Punkte beim Exportieren oder Importieren einer Azure SQL-Datenbank

* Bei allen in diesem Artikel behandelten Methoden wird das DTU-Kontingent (Database Transaction Unit, Datenbanktransaktionseinheit) verbraucht, was eine Drosselung durch den Azure SQL-Datenbank-Dienst zur Folge hat. Sie können [die DTU-Statistiken für die Datenbank im Azure-Portal anzeigen](https://docs.microsoft.com/azure/sql-database/sql-database-monitor-tune-overview#monitor-database-performance). Wenn die Datenbank ihre Ressourcenlimits erreicht hat, [führen Sie ein Upgrade der Dienstebene aus](https://docs.microsoft.com/azure/sql-database/sql-database-scale-resources), um weitere Ressourcen hinzuzufügen.
* Clientanwendungen (wie etwa das Hilfsprogramm „SqlPackage“ oder Ihre benutzerdefinierte DAC-Anwendung) sollten im Idealfall auf einem virtuellen Computer in der gleichen Region ausgeführt werden, in der sich auch Ihre SQL-Datenbank befindet. Andernfalls treten ggf. durch Netzwerklatenz bedingte Leistungsproblemen auf.
* Das Exportieren großer Tabellen ohne gruppierte Indizes kann sehr langsam sein oder sogar Fehler zur Folge haben. Dieses Verhalten tritt auf, da die Tabelle nicht parallel aufgeteilt und exportiert werden kann. Stattdessen muss sie in einer einzelnen Transaktion exportiert werden, was besonders bei großen Tabellen die Leistung beeinträchtigt und unter Umständen zu einem Exportfehler führt.


## <a name="related-documents"></a>Verwandte Dokumente

[Überlegungen beim Exportieren einer Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-export#considerations-when-exporting-an-azure-sql-database)
