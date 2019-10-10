---
title: StringToBoolean in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion StringToBoolean in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8719857dca16585a045f8174dbac8df455f38f38
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349260"
---
# <a name="stringtoboolean-azure-cosmos-db"></a>StringToBoolean (Azure Cosmos DB)
 Gibt den Ausdruck übersetzt in einen booleschen Wert zurück. Wenn der Ausdruck nicht übersetzt werden kann, wird „undefined“ zurückgegeben.  
  
## <a name="syntax"></a>Syntax
  
```sql
StringToBoolean(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck, der als boolescher Ausdruck analysiert werden soll.  
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen booleschen Ausdruck oder „undefined“ zurück.  
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird das typübergreifende Verhalten von `StringToBoolean` veranschaulicht. 
 
 Die folgenden Beispiele zeigen eine gültige Eingabe.

Leerzeichen sind ausschließlich vor oder nach „true“/„false“ zulässig.

```sql
SELECT 
    StringToBoolean("true") AS b1, 
    StringToBoolean("    false") AS b2,
    StringToBoolean("false    ") AS b3
```  
  
 Hier ist das Resultset.  
  
```json
[{"b1": true, "b2": false, "b3": false}]
```  

Die folgenden Beispiele zeigen eine ungültige Eingabe.

 Bei booleschen Werten muss die Groß-/Kleinschreibung beachtet werden. „true“ und „false“ dürfen daher nur Kleinbuchstaben enthalten.

```sql
SELECT 
    StringToBoolean("TRUE"),
    StringToBoolean("False")
```  

Hier ist das Resultset.  
  
```json
[{}]
``` 

Der übergebene Ausdruck wird als boolescher Ausdruck analysiert. Die folgenden Eingaben werden nicht als boolescher Typ ausgewertet und geben daher „undefined“ zurück:

```sql
SELECT 
    StringToBoolean("null"),
    StringToBoolean(undefined),
    StringToBoolean(NaN), 
    StringToBoolean(false), 
    StringToBoolean(true)
```  

Hier ist das Resultset.  
  
```json
[{}]
```  

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
