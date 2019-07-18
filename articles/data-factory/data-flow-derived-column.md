---
title: 'Transformieren von abgeleiteten Spalten in Mapping Data Flow: Azure Data Factory | Microsoft-Dokumentation'
description: Informationen zum bedarfsorientierten Transformieren von Daten in Azure Data Factory mithilfe von Mapping Data Flow-Transformationen von abgeleiteten Spalten
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: 941c629fd8359edc7fc1cf364a6735314044d95e
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312190"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Transformieren von abgeleiteten Spalten in Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verwenden Sie die Transformation für abgeleitete Spalten, um in Ihrem Datenfluss neue Spalten zu generieren oder vorhandene Felder zu ändern.

## <a name="derived-column-settings"></a>Einstellungen für abgeleitete Spalten

Sie können eine bereits vorhandene Spalte löschen, indem Sie diese über die Dropdownliste mit den Spalten auswählen. Alternativ können Sie auch das Feld „Spaltenauswahl“ als Textfeld verwenden und den Namen der neuen Spalte eingeben. Den Ausdruck der abgeleiteten Spalte können Sie erstellen, indem Sie auf das Feld „Ausdruck eingeben“ klicken, um den [Data Flow-Ausdrucks-Generator](concepts-data-flow-expression-builder.md) zu öffnen.

![Einstellungen für abgeleitete Spalten](media/data-flow/dc1.png "Einstellungen für abgeleitete Spalten")

Sie können weitere abgeleitete Spalten hinzufügen, indem Sie mit der Maus auf eine Spalte zeigen und auf das Symbol „+“ klicken. Klicken Sie anschließend entweder auf „Spalte hinzufügen“ oder auf „Add column pattern“ (Spaltenmuster hinzufügen). Spaltenmuster können nützlich sein, wenn sich Ihre Spaltennamen von den Quellen unterscheiden. Weitere Informationen finden Sie unter [Column Patterns (Spaltenmuster)](concepts-data-flow-column-pattern.md).

![Auswahl neuer abgeleiteter Spalten](media/data-flow/columnpattern.png "New derived column selection")

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Mapping Data Flow expression language (Mapping Data Flow-Ausdruckssprache)](data-flow-expression-functions.md).
