---
title: RAND in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion RAND in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 4a0672000e630c9e06df84d9c2da5cb8b988c05a
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349601"
---
# <a name="rand-azure-cosmos-db"></a>RAND (Azure Cosmos DB)
 Gibt einen nach dem Zufallsprinzip generierten numerischen Wert aus [0,1] zurück.
 
## <a name="syntax"></a>Syntax
  
```sql
RAND ()  
```  

## <a name="return-types"></a>Rückgabetypen

  Gibt einen numerischen Ausdruck zurück.

## <a name="remarks"></a>Anmerkungen

  `RAND` ist eine nichtdeterministische Funktion. Wiederkehrende Aufrufe von `RAND` geben nicht die gleichen Ergebnisse zurück.

## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel gibt einen nach dem Zufallsprinzip generierten numerischen Wert zurück.
  
```sql
SELECT RAND() AS rand 
```  
  
 Hier ist das Resultset.  
  
```json
[{"rand": 0.87860053195618093}]  
``` 

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
