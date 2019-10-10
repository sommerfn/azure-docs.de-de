---
title: 'Transformieren von abgeleiteten Spalten in Mapping Data Flow: Azure Data Factory | Microsoft-Dokumentation'
description: Informationen zum bedarfsorientierten Transformieren von Daten in Azure Data Factory mithilfe von Mapping Data Flow-Transformationen von abgeleiteten Spalten
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: aacd6f1799f1813e168bd04e78f18cf60ad5243f
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72026851"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Transformieren von abgeleiteten Spalten in Mapping Data Flow

Verwenden Sie die Transformation für abgeleitete Spalten, um in Ihrem Datenfluss neue Spalten zu generieren oder vorhandene Felder zu ändern.

## <a name="derived-column-settings"></a>Einstellungen für abgeleitete Spalten

Sie können eine bereits vorhandene Spalte löschen, indem Sie diese über die Dropdownliste mit den Spalten auswählen. Alternativ können Sie auch das Feld „Spaltenauswahl“ als Textfeld verwenden und den Namen der neuen Spalte eingeben. Den Ausdruck der abgeleiteten Spalte können Sie erstellen, indem Sie auf das Feld „Ausdruck eingeben“ klicken, um den [Data Flow-Ausdrucks-Generator](concepts-data-flow-expression-builder.md) zu öffnen.

![Einstellungen für abgeleitete Spalten](media/data-flow/dc1.png "Einstellungen für abgeleitete Spalten")

Sie können weitere abgeleitete Spalten hinzufügen, indem Sie mit der Maus auf eine Spalte zeigen und auf das Symbol „+“ klicken. Klicken Sie anschließend entweder auf „Spalte hinzufügen“ oder auf „Add column pattern“ (Spaltenmuster hinzufügen). Spaltenmuster können nützlich sein, wenn sich Ihre Spaltennamen von den Quellen unterscheiden. Weitere Informationen finden Sie unter [Column Patterns (Spaltenmuster)](concepts-data-flow-column-pattern.md).

![Auswahl neuer abgeleiteter Spalten](media/data-flow/columnpattern.png "New derived column selection")

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Mapping Data Flow expression language (Mapping Data Flow-Ausdruckssprache)](data-flow-expression-functions.md).
