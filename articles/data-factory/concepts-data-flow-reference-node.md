---
title: Datenzuordnungsflow in Azure Data Factory – Verweisknoten
description: Im Data Factory-Datenfluss wird ein Verweisknoten für Joins, Suchvorgänge und Unions hinzugefügt.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 4ed17114cc0ce586c68c5b3e087acffdb82ea96c
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72030492"
---
# <a name="mapping-data-flow-reference-node"></a>Mapping Data Flow: Verweisknoten



![Verweisknoten](media/data-flow/referencenode.png "Verweisknoten")

Ein Verweisknoten wird in der Canvas automatisch hinzugefügt, um anzugeben, dass der Knoten an Verweise auf einen anderen Knoten in der Canvas angefügt ist. Sie können sich einen Verweisknoten als Zeiger oder Verweis auf eine andere Datenflusstransformation vorstellen.

Beispiel:  Wenn Sie mehrere Datenströme verknüpfen oder vereinigen, wird in der Datenfluss-Canvas möglicherweise ein Verweisknoten hinzugefügt, der den Namen und die Einstellungen des nicht primären eingehenden Datenstroms darstellt.

Der Verweisknoten kann nicht verschoben oder gelöscht werden. Sie können jedoch in den Knoten klicken, um die ursprünglichen Transformationseinstellungen zu ändern.

Die Benutzeroberflächenregeln, die angewandt werden, wenn der Verweisknoten im Datenfluss hinzugefügt wird, basieren auf dem verfügbaren Platz und vertikalen Abstand zwischen den Zeilen.
