---
title: 'Azure Data Factory Mapping Data Flow: Exists-Transformation'
description: Prüfen, ob Zeilen vorhanden sind, mit Data Factory Mapping Data Flow-Instanzen mit Exists-Transformation
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 8b488a079b2da1bcf0dd064025ed251a1dc25213
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029390"
---
# <a name="mapping-data-flow-exists-transformation"></a>Mapping Data Flow: Exists-Transformation



Die Exists-Transformation ist eine Zeilenfilterungstransformation, die das Durchlaufen von Zeilen in Ihren Daten beendet oder zulässt. Die Exists-Transform ist mit ```SQL WHERE EXISTS``` und ```SQL WHERE NOT EXISTS``` vergleichbar. Nach einer Exists-Transformation enthalten die resultierenden Zeilen aus Ihrem Datenstrom entweder alle Zeilen, in denen die Spaltenwerte aus Quelle 1 in Quelle 2 vorhanden oder in Quelle 2 nicht vorhanden sind.

![Exists-Einstellungen](media/data-flow/exists.png "ist vorhanden 1")

Wählen Sie die zweite Quelle für Exists aus, damit Datenfluss die Werte aus Datenstrom 1 mit Datenstrom 2 vergleichen kann.

Wählen Sie die Spalte aus Quelle 1 und aus Quelle 2 aus, deren Werte Sie auf das Vorhandensein oder Nichtvorhandensein überprüfen möchten.

## <a name="multiple-exists-conditions"></a>Mehrere Exists-Bedingungen

Neben jeder Zeile in Ihren Spaltenbedingungen für Exists wird ein Pluszeichen (+) angezeigt, wenn Sie auf die jeweilige Zeile zeigen. Dadurch können Sie mehrere Zeilen für Exists-Bedingungen hinzufügen. Jede weitere Bedingung ist ein „Und“.

## <a name="custom-expression"></a>Benutzerdefinierter Ausdruck

![Benutzerdefinierte Exists-Einstellungen](media/data-flow/exists1.png "„Exists“ benutzerdefiniert")

Klicken Sie auf „Benutzerdefinierter Ausdruck“, um stattdessen einen Freiformausdruck als Ihre „Exists“- oder „Not-exists“-Bedingung zu erstellen. Wenn Sie dieses Kontrollkästchen aktivieren, können Sie einen eigenen Ausdruck als Bedingung eingeben.

## <a name="next-steps"></a>Nächste Schritte

Ähnliche Transformationen sind [Lookup](data-flow-lookup.md) und [Join](data-flow-join.md).
