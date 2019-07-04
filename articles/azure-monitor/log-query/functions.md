---
title: Funktionen in Azure Monitor-Protokollabfragen | Microsoft-Dokumentation
description: In diesem Artikel wird die Verwendung von Funktionen beschrieben, um in Azure Monitor aus einer Abfrage eine andere Protokollabfrage aufzurufen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: bwren
ms.openlocfilehash: 4b3116230a085bfbb9a6139fbada4179d802bf5e
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296065"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Verwenden von Funktionen in Azure Monitor-Protokollabfragen

Um eine Protokollabfrage mit einer anderen Abfrage zu verwenden, können Sie diese als Funktion speichern. Dadurch können Sie komplexe Abfragen vereinfachen, indem Sie sie in Einzelteile zerlegen und gemeinsamen Code bei mehreren Abfragen wiederverwenden.

## <a name="create-a-function"></a>Erstellen einer Funktion

Erstellen Sie eine Funktion mit Log Analytics, indem Sie im Azure-Portal auf **Speichern** klicken und dann die Informationen in der folgenden Tabelle angeben.

| Einstellung | BESCHREIBUNG |
|:---|:---|
| NAME           | Anzeigename für die Abfrage im **Abfrage-Explorer**. |
| Speichern unter        | Funktion |
| Funktionsalias | Kurzname zur Verwendung der Funktion in anderen Abfragen. Darf keine Leerzeichen enthalten und muss eindeutig sein. |
| Category (Kategorie)       | Eine Kategorie zum Organisieren gespeicherter Abfragen und Funktionen im **Abfrage-Explorer**. |

> [!NOTE]
> Eine Funktion in Azure Monitor kann keine andere Funktion enthalten.

> [!NOTE]
> Funktionen können in Azure Monitor-Protokollabfragen, aktuell jedoch nicht in Application Insights-Abfragen, gespeichert werden.



## <a name="use-a-function"></a>Verwenden einer Funktion
Sie verwenden eine Funktion, indem Sie den Alias in eine andere Abfrage einbeziehen. Sie kann wie jede andere Tabelle verwendet werden.

## <a name="example"></a>Beispiel
Die folgende Beispielabfrage gibt sämtliche fehlenden Sicherheitsupdates zurück, die am Vortag gemeldet wurden. Speichern Sie diese Abfrage als Funktion mit dem Alias _security_updates_last_day_. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

Erstellen Sie eine andere Abfrage, und verweisen Sie auf die Funktion _security_updates_last_day_, um nach SQL-bezogenen erforderlichen Sicherheitsupdates zu suchen.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Schreiben von Azure Monitor-Protokollabfragen finden Sie in folgenden weiteren Lektionen:

- [Zeichenfolgenvorgänge](string-operations.md)
- [Datums- und Uhrzeitvorgänge](datetime-operations.md)
- [Aggregationsfunktionen](aggregations.md)
- [Erweiterte Aggregationen](advanced-aggregations.md)
- [JSON und Datenstrukturen](json-data-structures.md)
- [Verknüpfungen](joins.md)
- [Diagramme](charts.md)
