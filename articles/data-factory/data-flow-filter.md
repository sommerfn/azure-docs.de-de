---
title: 'Azure Data Factory-Zuordnungsdatenfluss: Filtertransformation'
description: 'Azure Data Factory-Zuordnungsdatenfluss: Filtertransformation'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 2afe079c346a15ec212664ce022ac5e2926b12d4
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387805"
---
# <a name="azure-data-factory-filter-transformation"></a>Azure Data Factory: Filtertransformation



Die Filtertransformation ermöglicht die Zeilenfilterung. Erstellen Sie einen Ausdruck, der die Filterbedingung definiert. Klicken Sie in das Textfeld, um den Ausdrucks-Generator zu starten. Erstellen Sie im Ausdrucks-Generator einen Filterausdruck, um zu steuern, welche Zeilen aus dem aktuellen Datenstrom zur nächsten Transformation durchlaufen (filtern) dürfen. Stellen Sie sich die Filtertransformation als die WHERE-Klausel einer SQL-Anweisung vor.

## <a name="filter-on-loan_status-column"></a>Filtern nach der Spalte „loan_status“:

```
in([‘Default’, ‘Charged Off’, ‘Fully Paid’], loan_status).
```

Filtern Sie in der Filmedemo nach der Spalte „year“:

```
year > 1980
```

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie eine Spaltenfiltertransformation aus, die [Auswahltransformation](data-flow-select.md)
