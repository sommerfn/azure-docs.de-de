---
title: Transparent Data Encryption
description: Eine Übersicht über transparente Datenverschlüsselung für SQL-Datenbank und Data Warehouse Das Dokument behandelt die Vorteile dieser Verschlüsselung sowie die Konfigurationsoptionen. Diese umfassen die von einem Dienst verwaltete transparente Datenverschlüsselung und Bring Your Own Key.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
ms.date: 11/01/2019
ms.openlocfilehash: b6af171eafbaf1f4d31bad649fcb0c69d8bdc24d
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73821703"
---
# <a name="transparent-data-encryption-for-sql-database-and-data-warehouse"></a>Transparente Datenverschlüsselung für SQL-Datenbank und Data Warehouse

Transparente Datenverschlüsselung (Transparent Data Encryption, TDE) trägt zum Schutz von Azure SQL-Datenbank, einer verwalteten Azure SQL-Instanz und Azure Data Warehouse vor der Bedrohung durch schädliche Offlineaktivitäten durch die Verschlüsselung von ruhenden Daten bei. TDE ver- und entschlüsselt die Datenbank, die zugehörigen Sicherungen und die Transaktionsprotokolldateien im Ruhezustand in Echtzeit, ohne dass Änderungen an der Anwendung erforderlich sind. TDE ist standardmäßig für alle neu bereitgestellten Azure SQL-Datenbanken aktiviert. TDE kann nicht zum Verschlüsseln der **master**-Datenbank in SQL-Datenbank verwendet werden.  Die **master**-Datenbank enthält Objekte, die zum Ausführen der TDE-Vorgänge für die Benutzerdatenbanken erforderlich sind.

TDE muss für ältere Datenbanken von Azure SQL-Datenbank, eine verwaltete Azure SQL-Instanz oder Azure SQL Data Warehouse manuell aktiviert werden.
Datenbanken verwalteter Instanzen, die durch Wiederherstellen erstellt wurden, erben den Verschlüsselungsstatus der Quelldatenbank.

Die transparente Datenverschlüsselung verschlüsselt den Speicher einer gesamten Datenbank mithilfe eines symmetrischen Schlüssels, der als Datenbankverschlüsselungsschlüssel bezeichnet wird. Dieser Verschlüsselungsschlüssel für die Datenbank wird durch die Schutzvorrichtung der transparenten Datenverschlüsselung geschützt. Bei dieser Schutzvorrichtung handelt es sich entweder um ein von einem Dienst verwaltetes Zertifikat (von einem Dienst verwaltete transparente Datenverschlüsselung) oder um einen asymmetrischen Schlüssel, der in Azure Key Vault gespeichert ist (Bring Your Own Key). Die Schutzvorrichtung der transparenten Datenverschlüsselung ist für Azure SQL-Datenbank und Data Warehouse auf Serverebene und für eine verwaltete Azure SQL-Instanz auf Instanzebene festgelegt. In diesem Dokument bezieht sich der Begriff *Server* sowohl auf den Server als auch die Instanz (sofern nicht anders angegeben).

Beim Datenbankstart wird der Verschlüsselungsschlüssel der verschlüsselten Datenbank entschlüsselt und dann für die Entschlüsselung und erneute Verschlüsselung der Datenbankdateien im Prozess der SQL Server-Datenbank-Engine verwendet. Die transparente Datenverschlüsselung führt die E/A-Verschlüsselung und -Entschlüsselung der Daten auf Seitenebene in Echtzeit durch. Jede Seite wird entschlüsselt, wenn sie in den Speicher gelesen wird, und dann verschlüsselt, bevor sie auf den Datenträger geschrieben wird. Eine allgemeine Beschreibung der transparenten Datenverschlüsselung finden Sie unter [Transparente Datenverschlüsselung](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).

SQL Server kann bei der Ausführung auf einem virtuellen Azure-Computer auch einen asymmetrischen Schlüssel aus Key Vault verwenden. Die Konfigurationsschritte unterscheiden sich von der Verwendung eines asymmetrischen Schlüssels in SQL-Datenbank und einer verwalteten SQL-Instanz. Weitere Informationen finden Sie unter [Erweiterbare Schlüsselverwaltung mit Azure Key Vault (SQL Server)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).

## <a name="service-managed-transparent-data-encryption"></a>Von einem Dienst verwaltete transparente Datenverschlüsselung

In Azure ist bei der Standardeinstellung für die transparente Datenverschlüsselung der Verschlüsselungsschlüssel der Datenbank durch ein integriertes Serverzertifikat geschützt. Das integrierte Serverzertifikat ist für jeden Server eindeutig, und der verwendete Verschlüsselungsalgorithmus ist AES 256. Wenn sich eine Datenbank in einer Georeplikationsbeziehung befindet, werden die primäre und die sekundäre Geodatenbank vom übergeordneten Serverschlüssel der primären Datenbank geschützt. Sind zwei Datenbanken mit dem gleichen Server verbunden, verwenden sie auch das gleiche integrierte Zertifikat.  Microsoft rotiert diese Zertifikate gemäß der internen Sicherheitsrichtlinie automatisch, und der Stammschlüssel wird von einem Microsoft-internen Geheimnisspeicher geschützt.  Kunden können die Compliance von SQL-Datenbank mit internen Sicherheitsrichtlinien in Überwachungsberichten unabhängiger Drittanbieter überprüfen, die in [Microsoft Trust Center](https://servicetrust.microsoft.com/) zur Verfügung stehen.

Microsoft verschiebt und verwaltet auch die Schlüssel nahtlos, die für die Georeplikation und Wiederherstellung benötigt werden.

> [!IMPORTANT]
> Alle neu erstellten SQL-Datenbanken und Datenbanken verwalteter Instanzen werden standardmäßig mithilfe der von einem Dienst verwalteten transparenten Datenverschlüsselung verschlüsselt. Vorhandene SQL-Datenbanken, die vor Mai 2017 erstellt wurden, und SQL-Datenbanken, die durch Wiederherstellung, Georeplikation und Datenbankkopie erstellt wurden, sind standardmäßig nicht verschlüsselt. Vorhandene Datenbanken verwalteter Instanzen, die vor Februar 2019 erstellt wurden, sind standardmäßig nicht verschlüsselt. Datenbanken verwalteter Instanzen, die durch Wiederherstellen erstellt wurden, erben den Verschlüsselungsstatus von der Quelle.

## <a name="customer-managed-transparent-data-encryption---bring-your-own-key"></a>Vom Kunden verwaltete Transparent Data Encryption – BYOK (Bring Your Own Key)

[TDE mit vom Kunden verwalteten Schlüsseln in Azure Key Vault](transparent-data-encryption-byok-azure-sql.md) erlaubt die Verschlüsselung des Datenbank-Verschlüsselungsschlüssels (Database Encryption Key, DEK) mit einem vom Kunden verwalteten asymmetrischen Schlüssel, der als TDE-Schutzvorrichtung bezeichnet wird.  Dies wird in der Regel auch als „Bring Your Own Key-Unterstützung“ (BYOK) für Transparent Data Encryption bezeichnet. Im BYOK-Szenario wird die TDE-Schutzvorrichtung in einer Instanz von [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault) (dem cloudbasierten, externen Schlüsselverwaltungssystem von Azure) gespeichert, die dem Kunden gehört und von ihm verwaltet wird. Die TDE-Schutzvorrichtung kann [vom Schlüsseltresor generiert oder von einem lokalen HSM-Gerät auf den Schlüsseltresor übertragen werden](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys). Der auf der Startseite einer Datenbank gespeicherte TDE-DEK wird durch die TDE-Schutzvorrichtung, die in Azure Key Vault gespeichert ist und den Schlüsseltresor niemals verlässt, verschlüsselt und entschlüsselt.  SQL-Datenbank müssen die Berechtigungen für den Schlüsseltresor des Kunden gewährt werden, damit der DEK ver- und entschlüsselt werden kann. Wenn die Berechtigungen des logischen SQL-Servers für den Schlüsseltresor widerrufen werden, ist kein Zugriff mehr auf die Datenbank möglich, und alle Daten bleiben verschlüsselt. Bei Azure SQL-Datenbank ist die TDE-Schutzvorrichtung auf der logischen SQL Server-Ebene festgelegt und wird von allen Datenbanken geerbt, die diesem Server zugeordnet sind. Bei der [verwalteten Azure SQL-Instanz](https://docs.microsoft.com/azure/sql-database/sql-database-howto-managed-instance) ist die TDE-Schutzvorrichtung auf Instanzebene festgelegt und wird von allen *verschlüsselten* Datenbanken für diese Instanz geerbt. In diesem Dokument bezieht sich der Begriff *Server* sowohl auf den Server als auch die Instanz (sofern nicht anders angegeben).

Mit TDE und Azure Key Vault-Integration können Benutzer Aufgaben der Schlüsselverwaltung steuern, z.B. Schlüsselrotationen, Schlüsseltresorberechtigungen und Schlüsselsicherungen. Außerdem können sie die Überwachung und Berichterstellung für alle TDE-Schutzvorrichtungen aktivieren, die Azure Key Vault-Funktionalität nutzen. Key Vault bietet eine zentrale Schlüsselverwaltung, verwendet streng überwachte Hardwaresicherheitsmodule (HSMs) und ermöglicht die Aufgabentrennung zwischen dem Verwalten von Schlüsseln und Daten, um Sicherheitsrichtlinien einzuhalten.
Weitere Informationen zur transparenten Datenverschlüsselung (TDE) mit Azure Key Vault-Integration (Bring Your Own Key-Unterstützung) für Azure SQL-Datenbank, verwaltete SQL-Instanz und Data Warehouse finden Sie unter [Azure SQL – Transparent Data Encryption: Bring Your Own Key-Unterstützung](transparent-data-encryption-byok-azure-sql.md).

Weitere Informationen zur Verwendung der transparenten Datenverschlüsselung mit Azure Key Vault-Integration (Bring Your Own Key-Unterstützung) finden Sie in der Anleitung [Aktivieren der transparenten Datenverschlüsselung mithilfe Ihres eigenen Schlüssels aus Key Vault mit PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Verschieben einer durch die transparente Datenverschlüsselung gesicherte Datenbank

Für Vorgänge in Azure müssen Datenbanken nicht entschlüsselt werden. Die Einstellungen der transparenten Datenverschlüsselung für die Quelldatenbank oder die primäre Datenbank werden transparent an das Ziel vererbt. Dies gilt für folgende Vorgänge:

- Geografische Wiederherstellung
- Self-Service-Point-in-Time-Wiederherstellung
- Wiederherstellung einer gelöschten Datenbank
- Aktive Georeplikation
- Erstellung einer Datenbankkopie
- Wiederherstellen einer Sicherungsdatei in der verwalteten Azure SQL-Instanz

> [!IMPORTANT]
> Die Verwendung einer manuellen Kopiesicherung einer Datenbank, die über einen vom Dienst verwalteten TDE-Vorgang verschlüsselt wurde, ist auf einer verwalteten Azure SQL-Instanz nicht zulässig, da nicht auf das für die Verschlüsselung genutzte Zertifikat zugegriffen werden kann. Verwenden Sie die Point-in-Time-Wiederherstellung, um diese Art von Datenbank auf eine andere verwaltete Instanz zu verschieben.

Wenn Sie eine Datenbank exportieren, die mit der transparenten Datenverschlüsselung gesichert ist, werden die exportierten Inhalte der Datenbank nicht verschlüsselt. Diese exportierten Inhalte werden in unverschlüsselten BACPAC-Dateien gespeichert. Achten Sie darauf, die BACPAC-Dateien entsprechend zu schützen, und aktivieren Sie die transparente Datenverschlüsselung, nachdem der Import der neuen Datenbank abgeschlossen ist.

Wenn die BACPAC-Datei z.B. von einer lokalen Instanz von SQL Server exportiert wird, erfolgt keine automatische Verschlüsselung des importierten Inhalts der neuen Datenbank. Wenn die BACPAC-Datei auf eine lokale Instanz von SQL Server exportiert wird, erfolgt ebenfalls keine automatische Verschlüsselung der neuen Datenbank.

Eine Ausnahme besteht im Exportieren nach und aus SQL-Datenbank. Die transparente Datenverschlüsselung ist in der neuen Datenbank aktiviert, die BACPAC-Datei ist jedoch nicht verschlüsselt.


## <a name="manage-transparent-data-encryption"></a>Verwalten von Transparent Data Encryption
# <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)
Verwalten Sie Transparent Data Encryption im Azure-Portal.

Sie müssen eine Verbindung als Azure-Besitzer, -Mitwirkender oder SQL-Sicherheitsmanager herstellen, um die transparente Datenverschlüsselung über das Azure-Portal zu konfigurieren.

Die transparente Datenverschlüsselung wird auf Datenbankebene aktiviert und deaktiviert. Rufen Sie das [Azure-Portal](https://portal.azure.com) auf, und melden Sie sich mit Ihrem Azure-Administrator- oder Mitwirkendenkonto an, um die transparente Datenverschlüsselung für eine Datenbank zu aktivieren. Suchen Sie die Einstellungen für die transparente Datenverschlüsselung in Ihrer Benutzerdatenbank. Standardmäßig wird die von einem Dienst verwaltete transparente Datenverschlüsselung verwendet. Für den Server, der die Datenbank enthält, wird automatisch ein Zertifikat für die transparente Datenverschlüsselung generiert. Bei einer verwalteten Azure SQL-Instanz verwenden Sie T-SQL, um die transparente Datenverschlüsselung für eine Datenbank zu aktivieren oder deaktivieren.

![Von einem Dienst verwaltete transparente Datenverschlüsselung](./media/transparent-data-encryption-azure-sql/service-managed-tde.png)  

Der Hauptschlüssel für die transparente Datenverschlüsselung, der auch als Schutzvorrichtung für die transparente Datenverschlüsselung bezeichnet wird, wird auf Serverebene festgelegt. Öffnen Sie die Einstellungen für die transparente Datenverschlüsselung auf Ihrem Server, um diese mit Bring Your Own Key-Unterstützung zu verwenden und Ihre Datenbank mit einem Schlüssel von Key Vault zu schützen.

![Transparente Datenverschlüsselung mit Bring Your Own Key-Unterstützung](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)
Verwalten Sie Transparent Data Encryption mithilfe von PowerShell.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Das PowerShell Azure Resource Manager-Modul wird von Azure SQL-Datenbank weiterhin unterstützt, aber alle zukünftigen Entwicklungen erfolgen für das Az.Sql-Modul. Informationen zu diesen Cmdlets finden Sie unter [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Die Argumente für die Befehle im Az- und den AzureRm-Modulen sind im Wesentlichen identisch.

Sie müssen eine Verbindung als Azure-Besitzer, -Mitwirkender oder SQL-Sicherheitsmanager herstellen, um die transparente Datenverschlüsselung über PowerShell zu konfigurieren.

### <a name="cmdlets-for-azure-sql-database-and-data-warehouse"></a>Cmdlets für Azure SQL-Datenbank und Data Warehouse

Verwenden Sie die folgenden Cmdlets für Azure SQL-Datenbank und Data Warehouse:

| Cmdlet | BESCHREIBUNG |
| --- | --- |
| [Set-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) |Aktiviert oder deaktiviert die transparente Datenverschlüsselung für eine Datenbank|
| [Get-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) |Ruft den Status der transparenten Datenverschlüsselung für eine Datenbank ab |
| [Get-AzSqlDatabaseTransparentDataEncryptionActivity](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) |Überprüft den Verschlüsselungsfortschritt für eine Datenbank |
| [Add-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/add-azsqlserverkeyvaultkey) |Fügt einer SQL Server-Instanz einen Key Vault-Schlüssel hinzu |
| [Get-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserverkeyvaultkey) |Ruft die Key Vault-Schlüssel für einen Azure SQL-Datenbank-Server ab  |
| [Set-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) |Legt die Schutzvorrichtung der transparenten Datenverschlüsselung für eine SQL Server-Instanz fest |
| [Get-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) |Ruft die Schutzvorrichtung für die transparente Datenverschlüsselung ab |
| [Remove-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) |Entfernt einen Key Vault-Schlüssel aus einer SQL Server-Instanz |
|  | |

> [!IMPORTANT]
> Verwenden Sie bei einer verwalteten Azure SQL-Instanz den T-SQL-Befehl [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database), um die transparente Datenverschlüsselung auf Datenbankebene zu aktivieren oder deaktivieren, und prüfen Sie das [PowerShell-Beispielskript](transparent-data-encryption-byok-azure-sql-configure.md), um die transparente Datenverschlüsselung auf Instanzebene zu verwalten.

# <a name="transact-sqltabazure-transactsql"></a>[Transact-SQL](#tab/azure-TransactSQL)
Verwalten Sie Transparent Data Encryption mithilfe von Transact-SQL.

Stellen Sie mit einem Konto, das ein Administratorkonto oder ein Mitglied der Rolle **dbmanager** in der Masterdatenbank ist, eine Verbindung mit der Datenbank her.

| Get-Help | BESCHREIBUNG |
| --- | --- |
| [ALTER DATABASE (Azure SQL-Datenbank)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) | Mit SET ENCRYPTION ON/OFF wird eine Datenbank verschlüsselt oder entschlüsselt. |
| [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Gibt Informationen über den Verschlüsselungsstatus einer Datenbank und die ihr zugeordneten Verschlüsselungsschlüssel zurück. |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Gibt Informationen über den Verschlüsselungsstatus jedes Data Warehouse-Knotens und die ihm zugeordneten Verschlüsselungsschlüssel der Datenbank zurück. |
|  | |

Sie können die Schutzvorrichtung der transparenten Datenverschlüsselung mithilfe von Transact-SQL nicht durch einen Schlüssel von Key Vault ersetzen. Verwenden Sie PowerShell oder das Azure-Portal.

# <a name="rest-apitabazure-restapi"></a>[REST-API](#tab/azure-RESTAPI)
Verwalten Sie Transparent Data Encryption mithilfe der REST-API.

Sie müssen eine Verbindung als Azure-Besitzer, -Mitwirkender oder SQL-Sicherheitsmanager herstellen, um die transparente Datenverschlüsselung über die REST-API zu konfigurieren.
Verwenden Sie den folgenden Satz von Befehlen für Azure SQL-Datenbank und Data Warehouse:

| Get-Help | BESCHREIBUNG |
| --- | --- |
|[Server erstellen oder aktualisieren](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Fügt einer SQL Server-Instanz, die für den Zugriff auf Key Vault verwendet wird, Azure Active Directory-Identität hinzu.|
|[Serverschlüssel erstellen oder aktualisieren](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|Fügt einer SQL Server-Instanz einen Key Vault-Schlüssel hinzu|
|[Serverschlüssel entfernen](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|Entfernt einen Key Vault-Schlüssel aus einer SQL Server-Instanz|
|[Serverschlüssel abrufen](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|Ruft einen bestimmten Key Vault-Schlüssel aus einer SQL Server-Instanz ab|
|[Serverschlüssel nach Server auflisten](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|Ruft die Key Vault-Schlüssel für eine SQL Server-Instanz ab |
|[Verschlüsselungsschutzvorrichtung erstellen oder aktualisieren](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|Legt die Schutzvorrichtung der transparenten Datenverschlüsselung für eine SQL Server-Instanz fest|
|[Verschlüsselungsschutzvorrichtung abrufen](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|Ruft Schutzvorrichtung der transparenten Datenverschlüsselung für eine SQL Server-Instanz ab|
|[Verschlüsselungsschutzvorrichtungen nach Server auflisten](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|Ruft die Schutzvorrichtungen der transparenten Datenverschlüsselung für eine SQL Server-Instanz ab |
|[Transparent Data Encryption-Konfiguration erstellen oder aktualisieren](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|Aktiviert oder deaktiviert die transparente Datenverschlüsselung für eine Datenbank|
|[Transparent Data Encryption-Konfiguration abrufen](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|Ruft die Konfiguration der transparenten Datenverschlüsselung für eine Datenbank ab|
|[Ergebnisse der Transparent Data Encryption-Konfiguration auflisten](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|Ruft das Verschlüsselungsergebnis für eine Datenbank ab|

## <a name="next-steps"></a>Nächste Schritte

- Eine allgemeine Beschreibung der transparenten Datenverschlüsselung finden Sie unter [Transparente Datenverschlüsselung](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).
- Weitere Informationen zur transparenten Datenverschlüsselung mit Bring Your Own Key-Unterstützung für Azure SQL-Datenbank, verwaltete Azure SQL-Instanz und Data Warehouse finden Sie unter [Transparente Datenverschlüsselung mit Bring Your Own Key-Unterstützung](transparent-data-encryption-byok-azure-sql.md).
- Weitere Informationen zur Verwendung der transparenten Datenverschlüsselung mit Bring Your Own Key-Unterstützung finden Sie in der Anleitung [Aktivieren der transparenten Datenverschlüsselung mithilfe Ihres eigenen Schlüssels aus Key Vault mit PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).
- Weitere Informationen zu Key Vault finden Sie in der [Dokumentation zu Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
