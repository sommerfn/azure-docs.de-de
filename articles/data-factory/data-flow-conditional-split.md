---
title: 'Azure Data Factory-Zuordnungsdatenfluss: Transformation für bedingtes Teilen'
description: Azure Data Factory-Datenflusstransformation für bedingtes Teilen
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: d06b5b86737d0940930a3ccea3b6d65be0a802f9
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387901"
---
# <a name="mapping-data-flow-conditional-split-transformation"></a>Transformation für bedingtes Teilen für den Mappingdatenfluss



![Toolbox für bedingtes Teilen](media/data-flow/conditionalsplit2.png "Toolbox für bedingtes Teilen")

Die Transformation für bedingtes Teilen kann Datenzeilen je nach Inhalt der Daten an verschiedene Datenströme routen. Die Implementierung der Transformation für bedingtes Teilen ist mit einer CASE-Entscheidungsstruktur in einer Programmiersprache vergleichbar. Die Transformation wertet Ausdrücke aus und leitet dann die Datenzeile basierend auf den Ergebnissen an den angegebenen Datenstrom weiter. Diese Transformation stellt zudem eine Standardausgabe bereit, damit eine Zeile, die mit keinem Ausdruck übereinstimmt, an die Standardausgabe weitergeleitet wird.

![Bedingtes Teilen](media/data-flow/conditionalsplit1.png "Optionen für bedingtes Teilen")

## <a name="multiple-paths"></a>Mehrere Pfade

Wenn Sie weitere Bedingungen hinzufügen möchten, wählen Sie im unteren Konfigurationsbereich die Option „Datenstrom hinzufügen“ aus, und klicken Sie in das Ausdrucks-Generator-Textfeld, um Ihren Ausdruck zu erstellen.

![Bedingtes Teilen (mehrere Pfade)](media/data-flow/conditionalsplit3.png "Bedingtes Teilen (mehrere Pfade)")

## <a name="next-steps"></a>Nächste Schritte

Allgemeine Datenflusstransformationen, die mit bedingtem Teilen verwendet werden: [Join-Transformation](data-flow-join.md), [Lookup-Transformation](data-flow-lookup.md), [Select-Transformation](data-flow-select.md)
