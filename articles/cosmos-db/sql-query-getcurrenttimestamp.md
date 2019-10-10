---
title: GetCurrentTimestamp in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion GetCurrentTimestamp in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: b816c63da56025fe6e9cbaece2cde5dcd01585d4
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71351014"
---
# <a name="getcurrenttimestamp-azure-cosmos-db"></a>GetCurrentTimestamp (Azure Cosmos DB)
 Diese Funktion gibt die Anzahl der Millisekunden zurück, die seit dem Donnerstag am 1. Januar 1970 um 00:00:00 Uhr vergangen sind. 
  
## <a name="syntax"></a>Syntax
  
```sql
GetCurrentTimestamp ()  
```  
  
## <a name="return-types"></a>Rückgabetypen
  
  Ein numerischer Wert, der der aktuellen Anzahl der Millisekunden entspricht, die seit der Unix-Epoche vergangen sind, d. h. die Anzahl der Millisekunden, die seit dem Donnerstag am 1. Januar 1970 um 00:00:00 Uhr vergangen sind, wird zurückgegeben.

## <a name="remarks"></a>Anmerkungen

  GetCurrentTimestamp() ist eine nichtdeterministische Funktion.
  
  Das zurückgegebene Ergebnis entspricht der koordinierten Weltzeit (UTC).

## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird gezeigt, wie der aktuelle Zeitstempel mithilfe der integrierten GetCurrentTimestamp()-Funktion abgerufen wird.
  
```sql
SELECT GetCurrentTimestamp() AS currentUtcTimestamp
```  
  
 Hier ist ein Beispielresultset:
  
```json
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a name="next-steps"></a>Nächste Schritte

- [Datums- und Uhrzeitfunktionen in Azure Cosmos DB](sql-query-date-time-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
