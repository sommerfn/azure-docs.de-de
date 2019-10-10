---
title: StringToArray in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie mehr über die SQL-Systemfunktion StringToArray in Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 2d1f90da50950ac6ff4f87ffe96ebad9f3d811cc
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349276"
---
# <a name="stringtoarray-azure-cosmos-db"></a>StringToArray (Azure Cosmos DB)
 Gibt den Ausdruck übersetzt in ein Array zurück. Wenn der Ausdruck nicht übersetzt werden kann, wird „undefined“ zurückgegeben.  
  
## <a name="syntax"></a>Syntax
  
```sql  
StringToArray(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumente
  
*str_expr*  
   Ist ein Zeichenfolgenausdruck, der als JSON-Arrayausdruck analysiert werden soll. 
  
## <a name="return-types"></a>Rückgabetypen
  
  Gibt einen Arrayausdruck oder „undefined“ zurück. 
  
## <a name="remarks"></a>Anmerkungen
  Geschachtelte Zeichenfolgenwerte müssen in doppelten Anführungszeichen angegeben werden, damit sie gültige JSON-Werte darstellen. Ausführliche Informationen zum JSON-Format finden Sie unter [json.org](https://json.org/).
  
## <a name="examples"></a>Beispiele
  
  Im folgenden Beispiel wird das typübergreifende Verhalten von `StringToArray` veranschaulicht. 
  
 Die folgenden Beispiele zeigen eine gültige Eingabe.

```sql
SELECT 
    StringToArray('[]') AS a1, 
    StringToArray("[1,2,3]") AS a2,
    StringToArray("[\"str\",2,3]") AS a3,
    StringToArray('[["5","6","7"],["8"],["9"]]') AS a4,
    StringToArray('[1,2,3, "[4,5,6]",[7,8]]') AS a5
```

Hier ist das Resultset.

```json
[{"a1": [], "a2": [1,2,3], "a3": ["str",2,3], "a4": [["5","6","7"],["8"],["9"]], "a5": [1,2,3,"[4,5,6]",[7,8]]}]
```

Das folgende Beispiel zeigt eine ungültige Eingabe. 
   
 Einfache Anführungszeichen innerhalb des Arrays sind kein gültiger JSON-Code.
Sie sind zwar innerhalb einer Abfrage gültig, werden jedoch nicht als gültige Arrays interpretiert. Zeichenfolgen innerhalb der Arrayzeichenfolge müssen mit Escapezeichen versehen werden: "[\\"\\"]". Alternativ kann die Arrayzeichenfolge in einfache Anführungszeichen eingeschlossen werden: '[""]'.

```sql
SELECT
    StringToArray("['5','6','7']")
```

Hier ist das Resultset.

```json
[{}]
```

Die folgenden Beispiele zeigen eine ungültige Eingabe.
   
 Der übergebene Ausdruck wird als JSON-Array analysiert. Folgendes wird nicht als Arraytyp ausgewertet und gibt daher „undefined“ zurück:
   
```sql
SELECT
    StringToArray("["),
    StringToArray("1"),
    StringToArray(NaN),
    StringToArray(false),
    StringToArray(undefined)
```

Hier ist das Resultset.

```json
[{}]
```

## <a name="next-steps"></a>Nächste Schritte

- [Zeichenfolgenfunktionen in Azure Cosmos DB](sql-query-string-functions.md)
- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
