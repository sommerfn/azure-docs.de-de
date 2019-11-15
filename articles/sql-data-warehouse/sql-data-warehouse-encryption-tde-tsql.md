---
title: Transparent Data Encryption (T-SQL)
description: Transparent Data Encryption (TDE) in SQL Data Warehouse (T-SQL)
services: sql-data-warehouse
author: julieMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: security
ms.date: 04/30/2019
ms.author: jrasnick
ms.reviewer: rortloff
ms.custom: seo-lt-2019
ms.openlocfilehash: 4e7f4f31cd8b899e3fcf79568ea62830313936b9
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73822603"
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Erste Schritte mit Transparent Data Encryption (TDE)
> [!div class="op_single_selector"]
> * [Sicherheitsübersicht](sql-data-warehouse-overview-manage-security.md)
> * [Authentifizierung](sql-data-warehouse-authentication.md)
> * [Verschlüsselung (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Verschlüsselung (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>Erforderliche Berechtigungen
Sie müssen Administrator oder ein Mitglied der Rolle „dbmanager“ sein, um Transparent Data Encryption (TDE) zu aktivieren.

## <a name="enabling-encryption"></a>Aktivieren der Verschlüsselung
Führen Sie die folgenden Schritte aus, um TDE für ein SQL Data Warehouse zu aktivieren:

1. Stellen Sie eine Verbindung mit der Datenbank *master* auf dem Server her, auf dem die Datenbank gehostet wird. Verwenden Sie eine Anmeldung, bei der es sich um einen Administrator oder ein Mitglied der Rolle **dbmanager** in der Datenbank „master“ handelt.
2. Führen Sie die folgende Anweisung aus, um die Datenbank zu verschlüsseln.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung
Führen Sie die folgenden Schritte aus, um TDE für ein SQL Data Warehouse zu deaktivieren:

1. Stellen Sie eine Verbindung mit der Datenbank *master* her. Verwenden Sie eine Anmeldung, bei der es sich um einen Administrator oder ein Mitglied der Rolle **dbmanager** in der Datenbank „master“ handelt.
2. Führen Sie die folgende Anweisung aus, um die Datenbank zu verschlüsseln.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Ein angehaltenes SQL Data Warehouse muss vor dem Ändern der TDE-Einstellungen fortgesetzt werden.
> 
> 

## <a name="verifying-encryption"></a>Überprüfen der Verschlüsselung
Führen Sie die folgenden Schritte aus, um den Verschlüsselungsstatus für ein SQL Data Warehouse zu überprüfen:

1. Stellen Sie eine Verbindung mit der Datenbank *master* oder einer Instanzdatenbank her. Verwenden Sie eine Anmeldung, bei der es sich um einen Administrator oder ein Mitglied der Rolle **dbmanager** in der Datenbank „master“ handelt.
2. Führen Sie die folgende Anweisung aus, um die Datenbank zu verschlüsseln.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Das Ergebnis ```1``` steht für eine verschlüsselte Datenbank, und mit ```0``` wird eine nicht verschlüsselte Datenbank angegeben.

## <a name="encryption-dmvs"></a>Verschlüsselung-DMVs
* [sys.databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: https://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
