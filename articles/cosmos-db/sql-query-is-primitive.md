---
title: IS_PRIMITIVE in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion IS_PRIMITIVE in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 960c6cbe6b60ad477f630b14ce0953601e71c34e
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349810"
---
# <a name="is_primitive-azure-cosmos-db"></a>IS_PRIMITIVE (Azure Cosmos DB)
 Gibt einen booleschen Wert zurück, der anzeigt, ob der angegebene Ausdruck ein Grundtyp ist (Zeichenfolge, boolesch, numerisch oder NULL).  
  
## <a name="syntax"></a>Syntax
  
```sql
IS_PRIMITIVE(<expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*expr*  
   Ist ein beliebiger Ausdruck.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen booleschen Ausdruck zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel werden JSON-Objekte des Typs „Boolean“, „Number“, „String“, „NULL“, „Object“, „Array“ und „undefined“ mithilfe der `IS_PRIMITIVE`-Funktion überprüft.  
  
```sql
SELECT   
           IS_PRIMITIVE(true) AS isPrim1,   
           IS_PRIMITIVE(1) AS isPrim2,  
           IS_PRIMITIVE("value") AS isPrim3,   
           IS_PRIMITIVE(null) AS isPrim4,  
           IS_PRIMITIVE({prop: "value"}) AS isPrim5,   
           IS_PRIMITIVE([1, 2, 3]) AS isPrim6,  
           IS_PRIMITIVE({prop: "value"}.prop2) AS isPrim7  
```  
  
 Hier ist das Resultset.  
  
```json
[{"isPrim1": true, "isPrim2": true, "isPrim3": true, "isPrim4": true, "isPrim5": false, "isPrim6": false, "isPrim7": false}]  
```  

## <a name="next-steps"></a>Nächste Schritte

- [Funktionen für die Typüberprüfung in Azure Cosmos DB](sql-query-type-checking-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
