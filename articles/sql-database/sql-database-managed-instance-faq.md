---
title: Häufig gestellte Fragen zu verwalteten Instanzen
description: Häufig gestellte Fragen (FAQ) zur verwalteten SQL-Datenbank-Instanz
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 07/16/2019
ms.openlocfilehash: 34ffcd63879920c3c744deb4cf0b28684081dc25
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825730"
---
# <a name="sql-database-managed-instance-frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ) zur verwalteten SQL-Datenbank-Instanz

Dieser Artikel enthält viele der am häufigsten gestellten Fragen zur [verwalteten SQL-Datenbank-Instanz](sql-database-managed-instance.md).

## <a name="where-can-i-find-a-list-of-features-supported-on-managed-instance"></a>Wo finde ich eine Liste der Funktionen, die in einer verwalteten Instanz unterstützt werden?

Eine Liste der in einer verwalteten Instanz unterstützten Funktionen finden Sie unter [Azure SQL-Datenbank und SQL Server](sql-database-features.md).

Informationen zu den Unterschieden in der Syntax und dem Verhalten zwischen einer verwalteten Azure SQL-Datenbank-Instanz und einem lokalen SQL Server finden Sie unter [T-SQL-Unterschiede zu SQL Server](sql-database-managed-instance-transact-sql-information.md).


## <a name="where-can-i-find-technical-characteristics-and-resource-limits-for-managed-instance"></a>Wo finde ich technische Merkmale und Ressourceneinschränkungen für die verwaltete Instanz?

Informationen zu den verfügbaren Merkmalen der Hardwaregenerationen finden Sie unter [technische Unterschiede zwischen Hardwaregenerationen](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).
Informationen zu den verfügbaren Dienstebenen und ihren Merkmalen finden Sie unter [technische Unterschiede zwischen Dienstebenen](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

## <a name="where-can-i-find-known-issues-and-bugs"></a>Wo finde ich die bekannten Probleme und Fehler?

Programmfehler und bekannte Probleme werden unter [Bekannte Probleme](sql-database-managed-instance-transact-sql-information.md#Issues) erörtert.

## <a name="where-can-i-find-latest-features-and-the-features-in-public-preview"></a>Wo finde ich die neuesten Features sowie die Features in der Public Preview?

Neue Features und Previewfunktionen finden Sie in den [Versionshinweisen](/azure/sql-database/sql-database-release-notes?tabs=managed-instance).

## <a name="how-much-time-takes-to-create-or-update-instance-or-to-restore-a-database"></a>Wie lange dauert es, eine Instanz zu erstellen oder zu aktualisieren oder eine Datenbank wiederherzustellen?

Die erwartete Zeit zum Erstellen einer neuen verwalteten Instanz oder zum Ändern der Dienstebene (virtuelle Kerne, Speicher) hängt von mehreren Faktoren ab. Sehen Sie sich die [Verwaltungsvorgänge](/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations) an. 

## <a name="can-a-managed-instance-have-the-same-name-as-on-premises-sql-server"></a>Kann eine verwaltete Instanz den gleichen Namen wie ein lokaler SQL Server haben?

Die Namen verwalteter Instanzen müssen mit *database.windows.net* enden. So verwenden Sie statt der Standardzone eine andere DNS-Zone, z. B. **mi-another-name**.contoso.com: 
- Verwenden Sie „CliConfig“ zum Definieren eines Alias. Weil das Tool nur ein Wrapper für Registrierungseinstellungen ist, könnte es auch mithilfe einer Gruppenrichtlinie oder eines Skripts ausgeführt werden.
- Verwenden Sie *CNAME* mit der Option *TrustServerCertificate=true*.

## <a name="how-can-i-move-database-from-managed-instance-back-to-sql-server-or-azure-sql-database"></a>Wie kann ich eine Datenbank aus einer verwalteten Instanz zurück zu SQL Server oder Azure SQL-Datenbank verschieben?

Sie können [die Datenbank in eine BACPAC-Datei exportieren](sql-database-export.md) und dann [die BACPAC-Datei importieren]( sql-database-import.md). Dieser Ansatz wird empfohlen, wenn Ihre Datenbank kleiner als 100 GB ist.

Transaktionsreplikation kann verwendet werden, wenn alle Tabellen in der Datenbank Primärschlüssel aufweisen.

Native `COPY_ONLY`-Sicherungen einer verwalteten Instanz können nicht in SQL Server wiederhergestellt werden, da eine verwaltete Instanz eine höhere Datenbankversion als SQL Server aufweist.

## <a name="how-can-i-migrate-my-instance-database-to-a-single-azure-sql-database"></a>Wie kann ich meine Instanzdatenbank zu einer einzelnen Azure SQL-Datenbank migrieren?

Eine Option ist das [Exportieren der Datenbank in eine BACPAC-Datei](sql-database-export.md) und das anschließende [Importieren der BACPAC-Datei](sql-database-import.md). 

Dieser Ansatz wird empfohlen, wenn die Datenbank kleiner als 100 GB ist. Transaktionsreplikation kann verwendet werden, wenn alle Tabellen in der Datenbank Primärschlüssel aufweisen.

## <a name="how-do-i-choose-between-gen-4-and-gen-5-hardware-generation-for-managed-instance"></a>Wie wähle ich zwischen der Hardwaregeneration Gen 4 und Gen 5 für die verwaltete Instanz?

Das hängt von der Workload ab, da einige Hardwaregenerationen für bestimmte Arten von Workloads besser als andere geeignet sind. Der Themenbereich Leistung ist komplex, jedoch weisen die Hardwaregenerationen die folgenden Unterschiede auf, die sich auf die Workloadleistung auswirken:
- Gen 4 bietet eine bessere Computeunterstützung, da sie auf physischen Prozessoren basiert, während Gen 5 auf vCore-Prozessoren basiert. Das kann für rechenintensive Workloads von Vorteil sein.
- Gen 5 unterstützt den beschleunigten Netzwerkbetrieb. Dies führt zu einer besseren E/A-Bandbreite für den Remotespeicher. Das kann für E/A-intensive Workloads auf universellen Dienstebenen vorteilhaft sein. In Gen 5 werden schnellere lokale SSD-Datenträger als in Gen 4 verwendet. Das kann für E/A-intensive Workloads auf unternehmenskritischen Dienstebenen vorteilhaft sein.

Es wird dringend empfohlen, vor der Liveschaltung die Leistung der tatsächlichen für die Produktion vorgesehenen Workloads zu testen, um zu bestimmen, welche Hardwaregeneration in einem bestimmten Fall besser geeignet ist.

## <a name="can-i-switch-my-managed-instance-hardware-generation-between-gen-4-and-gen-5-online"></a>Kann ich für meine verwaltete Instanz online zwischen der Hardwaregeneration Gen 4 und Gen 5 wechseln? 

Der automatische Onlinewechsel zwischen Hardwaregenerationen ist möglich, wenn beide Hardwaregenerationen in der Region verfügbar sind, in der Ihre verwaltete Instanz bereitgestellt wurde. In diesem Fall können Sie das [Skript aus dem Blogbeitrag](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Change-hardware-generation-on-Managed-Instance/ba-p/699824) verwenden, in dem erläutert wird, wie Sie zwischen Hardwaregenerationen wechseln.

Dabei handelt es sich um einen Vorgang mit langer Ausführungszeit, da eine neue verwaltete Instanz im Hintergrund bereitgestellt wird und Datenbanken automatisch zwischen der alten und neuen Instanz mit einem schnellen Failover am Ende des Prozesses übertragen werden. 

Wenn nicht beide Hardwaregenerationen in der gleichen Region unterstützt werden, kann die Hardwaregeneration nur manuell gewechselt werden. Dazu müssen Sie eine neue Instanz in der Region bereitstellen, in der die gewünschte Hardwaregeneration verfügbar ist, sowie die Daten zwischen der alten und neuen Instanz manuell sichern und wiederherstellen.


## <a name="how-do-i-tune-performance-of-my-managed-instance"></a>Wie optimiere ich die Leistung meiner verwalteten Instanz? 

Da eine universelle verwaltete Instanz Remotespeicher verwendet, wirkt sich die Größe der Daten-und Protokolldateien auf die Leistung aus. Weitere Informationen finden Sie unter [Auswirkung der Protokolldateigröße auf die Leistung einer universell verwalteten Instanz](https://medium.com/azure-sqldb-managed-instance/impact-of-log-file-size-on-general-purpose-managed-instance-performance-21ad170c823e).

Für E/A-intensive Workloads empfiehlt sich möglicherweise die Verwendung von Gen 5-Hardware und für rechenintensive Workloads die Verwendung von Gen 4. Weitere Informationen finden Sie unter [Gewusst wie: Auswählen zwischen Gen 4 und Gen5](#how-do-i-choose-between-gen-4-and-gen-5-hardware-generation-for-managed-instance).

Wenn Ihre Workload aus vielen kleinen Transaktionen besteht, sollten Sie den Verbindungstyp von Proxymodus in Umleitungsmodus ändern.

## <a name="what-is-the-maximum-storage-size-for-managed-instance"></a>Was ist die maximale Speichergröße für die verwaltete Instanz? 

Die Speichergröße für verwaltete Instanzen hängt von der ausgewählten Dienstebene („Universell“ oder „Unternehmenskritisch“) ab. Informationen zu den Speichereinschränkungen dieser Dienstebenen finden Sie unter [Merkmale der Dienstebene](sql-database-service-tiers-general-purpose-business-critical.md).

## <a name="is-the-backup-storage-deducted-from-my-managed-instance-storage"></a>Wird der Sicherungsspeicher vom Speicher meiner verwalteten Instanz abgezogen? 

Nein, der Sicherungsspeicher wird nicht vom Speicher Ihrer verwalteten Instanz abgezogen. Der Sicherungsspeicher ist unabhängig vom Instanzspeicherplatz, und seine Größe ist nicht begrenzt. Der Sicherungsspeicher wird durch den Zeitraum zum Beibehalten der Sicherung Ihrer Instanzdatenbanken begrenzt, der auf 7 bis 35 Tage festgelegt werden kann. Weitere Informationen finden Sie unter [Automatisierte Sicherungen](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
  
## <a name="how-can-i-set-inbound-nsg-rules-on-management-ports"></a>Wie kann ich NSG-Regeln für eingehenden Datenverkehr an Verwaltungsports festlegen?

Die integrierte Firewall-Funktion konfiguriert die Windows-Firewall auf allen virtuellen Computern im Cluster, um eingehende Verbindungen nur von IP-Adressbereichen zuzulassen, die Computern mit Microsoft-Verwaltung/-Bereitstellung und sicheren Administratorarbeitsstationen zugeordnet sind. So werden Eindringversuche über die Vermittlungsschicht effektiv verhindert.

Ports und ihre Verwendung:

Port 9000 und 9003 werden für die Service Fabric-Infrastruktur verwendet. Die primäre Aufgabe von Service Fabric ist es, den virtuellen Cluster fehlerfrei zu halten und den Zielzustand hinsichtlich der Anzahl der Komponentenreplikate zu bewahren.

Die Ports 1438, 1440 und 1452 werden vom Knoten-Agent verwendet. Der Knoten-Agent ist eine Anwendung, die im Cluster ausgeführt wird und von der Steuerungsebene zum Ausführen von Verwaltungsbefehlen verwendet wird.

Die Kommunikation wird nicht nur von der integrierten Firewall in der Vermittlungsschicht, sondern auch durch Zertifikate geschützt.
  
Weitere Informationen zu diesem Thema und zum Überprüfen der integrierten Firewall finden Sie unter [Ermitteln einer Firewall, die in eine verwaltete Azure SQL-Datenbank-Instanz integriert ist](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).


## <a name="how-can-i-mitigate-networking-risks"></a>Wie kann ich Netzwerkrisiken verringern? 

Kunden wird empfohlen, zum Verringern von Netzwerkrisiken eine Reihe von Sicherheitseinstellungen und -kontrollen anzuwenden:

- Aktivieren Sie [Transparent Data Encryption (TDE)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql) für alle Datenbanken.
- Deaktivieren Sie die Common Language Runtime (CLR). Dies wird auch für lokale Umgebungen empfohlen.
- Verwenden Sie nur Authentifizierung über Azure Active Directory (AAD).
- Greifen Sie auf die Instanz mit einem DBA-Konto mit geringen Berechtigungen zu.
- Konfigurieren Sie den Zugriff auf die JiT-Jumpbox für das Systemadministratorkonto.
- Aktivieren Sie [SQL-Überwachung](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine), und integrieren Sie sie in Warnungsmechanismen.
- Aktivieren Sie die [Bedrohungserkennung](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) aus der [Advanced Data Security (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security)-Sammlung.


## <a name="where-can-i-find-use-cases-and-resulting-cost-savings-with-managed-instance"></a>Wo finde ich Anwendungsfälle und entsprechende Kosteneinsparungen für verwaltete Instanzen?

Fallstudien zu verwalteten Instanzen:

- [Komatsu](https://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [KMD](https://customers.microsoft.com/en-ca/story/kmd-professional-services-azure-sql-database)
- [PowerDETAILS](https://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](https://customers.microsoft.com/story/allscripts-partner-professional-services-azure)   
Es gibt auch eine Forrester-Studie, die ein besseres Verständnis der Vorteile, Kosten und Risiken im Zusammenhang mit der Bereitstellung einer verwalteten Azure SQL-Datenbank-Instanz vermittelt: [Der Total Economic Impact der verwalteten Microsoft Azure SQL-Datenbank-Instanz](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance).


## <a name="can-i-do-dns-refresh"></a>Kann ich eine DNS-Aktualisierung ausführen? 
  
Derzeit bieten wir keine Funktion zum Aktualisieren der DNS-Serverkonfiguration für die verwaltete Instanz.

Die DNS-Konfiguration wird letztendlich aktualisiert:

- Wenn die DHCP-Lease abläuft.
- Bei einem Plattformupgrade.

Als Problemumgehung können Sie ein Downgrade der verwalteten Instanz auf 4 virtuelle Kerne durchführen und danach ein Upgrade der Instanz durchführen. Dies hat die Nebenwirkung, dass die DNS-Konfiguration aktualisiert wird.


## <a name="can-a-managed-instance-have-a-static-ip-address"></a>Kann eine verwaltete Instanz eine statische IP-Adresse aufweisen?

In seltenen, jedoch erforderlichen Situationen müssen Sie eine Onlinemigration einer verwalteten Instanz zu einem neuen virtuellen Cluster durchführen. Gegebenenfalls ist diese Migration aufgrund von Änderungen in unserem Technologiestapel erforderlich, mit denen die Sicherheit und Zuverlässigkeit des Diensts verbessert werden sollen. Die Migration zu einem neuen virtuellen Cluster führt zum Andern der IP-Adresse, die dem Hostnamen der verwalteten Instanz zugeordnet ist. Der Dienst für die verwaltete Instanz beansprucht keine Unterstützung statischer IP-Adressen und behält sich das Recht vor, die IP-Adresse im Rahmen regulärer Wartungszyklen zu ändern.

Aus diesem Grund wird dringend empfohlen, keine statische IP-Adresse zu verwenden, da dies zu unnötigen Ausfallzeiten führen kann.

## <a name="can-i-move-a-managed-instance-or-its-vnet-to-another-resource-group"></a>Kann ich eine verwaltete Instanz oder das zugehörige VNet in eine andere Ressourcengruppe verschieben?

Nein, dies ist die aktuelle Plattformeinschränkung. Nachdem eine verwaltete Instanz erstellt wurde, wird das Verschieben dieser Instanz oder des VNets in eine andere Ressourcengruppe oder ein anderes Abonnement nicht unterstützt.

## <a name="can-i-change-the-time-zone-for-an-existing-managed-instance"></a>Kann ich die Zeitzone für eine vorhandene verwaltete Instanz ändern?

Die Zeitzonenkonfiguration kann festgelegt werden, wenn eine verwaltete Instanz zum ersten Mal bereitgestellt wird. Das Ändern der Zeitzone der vorhandenen verwalteten Instanz wird nicht unterstützt. Weitere Informationen finden Sie unter [Zeitzonenbeschränkungen](sql-database-managed-instance-timezone.md#limitations).

Als Problemumgehung können Sie eine neue verwaltete Instanz mit der richtigen Zeitzone erstellen und dann entweder eine Sicherung und Wiederherstellung oder eine [instanzübergreifende Point-in-Time-Wiederherstellung](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/) durchführen (von uns empfohlen).


## <a name="how-do-i-resolve-performance-issues-with-my-managed-instance"></a>Wie löse ich Leistungsprobleme bei meiner verwalteten Instanz?

Als Ausgangspunkt für einen Leistungsvergleich zwischen der verwalteten Instanz und SQL Server empfiehlt sich der Artikel [Best practices for performance comparison between Azure SQL Managed Instance and SQL Server](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210) (Bewährte Methoden für den Leistungsvergleich zwischen der verwalteten Azure-SQL-Instanz und SQL Server, in englischer Sprache).

Das Laden von Daten erfolgt aufgrund des obligatorischen vollständigen Wiederherstellungsmodells und der [Einschränkungen](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics) für den Schreibdurchsatz des Transaktionsprotokolls in einer verwalteten Instanz häufig langsamer als in SQL Server. Dies kann manchmal umgangen werden, indem vorübergehende Daten in tempdb statt einer Benutzerdatenbank gespeichert oder gruppierte Columnstore-Tabellen oder speicheroptimierte Tabellen verwendet werden.


## <a name="can-i-restore-my-encrypted-database-to-managed-instance"></a>Kann ich meine verschlüsselte Datenbank in einer verwalteten Instanz wiederherstellen?

Ja. Sie müssen die Datenbank nicht entschlüsseln, um sie in einer verwalteten Instanz wiederherstellen zu können. Sie müssen ein Zertifikat/einen als Verschlüsselungsschlüssel-Schutzvorrichtung verwendeten Schlüssel im Quellsystem für die verwaltete Instanz bereitstellen, um Daten aus der verschlüsselten Sicherungsdatei lesen zu können. Dies kann auf zwei Arten erreicht werden:

- *Laden Sie die Zertifikatschutzvorrichtung in die verwaltete Instanz hoch*. Das kann nur mithilfe von PowerShell geschehen. Im [Beispielskript](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate-tde-certificate) wird der gesamte Prozess beschrieben.
- *Laden Sie eine asymmetrische Schlüsselschutzvorrichtung in Azure Key Vault (AKV) hoch, und verweisen Sie in der verwalteten Instanz darauf*. Dieser Ansatz ähnelt dem TDE-Anwendungsfall Bring-Your-Own-Key (BYOK), in dem ebenfalls die AKV-Integration zum Speichern des Verschlüsselungsschlüssels verwendet wird. Wenn Sie den Schlüssel nicht als Schutzvorrichtung für den Verschlüsselungsschlüssel verwenden, sondern nur für die verwaltete Instanz zum Wiederherstellen verschlüsselter Datenbanken zur Verfügung stellen möchten, befolgen Sie die Anweisungen zum [Einrichten von BYOK-TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption), und aktivieren Sie nicht das Kontrollkästchen *Legen Sie den ausgewählten Schlüssel als TDE-Standardschutzvorrichtung fest*.

Nachdem Sie die Verschlüsselungsschutzvorrichtung für die verwaltete Instanz verfügbar gemacht haben, können Sie mit dem Standardverfahren für die Datenbankwiederherstellung fortfahren.

## <a name="how-can-i-migrate-from-azure-sql-database-single-or-elastic-pool-to-managed-instance"></a>Wie kann ich eine Azure SQL-Einzeldatenbank oder einen Azure SQL-Pool für elastische Datenbanken zu einer verwalteten Instanz migrieren? 

Die verwaltete Instanz bietet die gleichen Leistungsstufen pro Compute- und Speichergröße wie andere Bereitstellungsoptionen von Azure SQL-Datenbank. Wenn Sie Daten in einer einzelnen Instanz konsolidieren möchten oder einfach eine Funktion ausschließlich in der verwalteten Instanz unterstützt werden soll, können Sie die Daten mithilfe der Export-/Import-Funktion (BACPAC) migrieren.
