---
title: PI in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion PI in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 27832008e8922e339a648985192a58b111555bc9
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349653"
---
# <a name="pi-azure-cosmos-db"></a>PI (Azure Cosmos DB)
 Gibt den konstanten Wert von PI zurück.  
  
## <a name="syntax"></a>Syntax
  
```sql
PI ()  
```  
   
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen numerischen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Das folgende Beispiel gibt den Wert von `PI` zurück.  
  
```sql
SELECT PI() AS pi 
```  
  
 Hier ist das Resultset.  
  
```json
[{"pi": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Mathematische Funktionen in Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
