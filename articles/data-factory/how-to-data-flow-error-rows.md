---
title: Beheben von fehlerhaften Zeilen mit Zuordnungsdatenflüssen in Azure Data Factory
description: Erfahren Sie, wie Sie SQL-Kürzungsfehler in Azure Data Factory mithilfe von Zuordnungsdatenflüssen beheben.
services: data-factory
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/28/2019
ms.author: makromer
ms.openlocfilehash: 3fe3403ad06d82ba5ccd33d2718bf0e5eff64490
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73164628"
---
# <a name="handle-sql-truncation-error-rows-in-data-factory-mapping-data-flows"></a>Beheben von SQL-Kürzungsfehlern in Zeilen in Data Factory-Zuordnungsdatenflüssen

Als gängiges Szenario in Data Factory bei der Verwendung von Zuordnungsdatenflüssen werden die transformierten Daten in eine Azure SQL-Datenbank geschrieben. In diesem Szenario muss eine mögliche Spaltenkürzung als häufige Fehlerbedingung verhindert werden. Führen Sie die nachfolgenden Schritte aus, um die Protokollierung von Spalten zu ermöglichen, die nicht in eine Zielzeichenfolgenspalte passen, sodass der Datenfluss in diesen Szenarien fortgesetzt werden kann.

## <a name="scenario"></a>Szenario

1. Verwendet wird eine Azure SQL-Datenbank-Zieltabelle, die eine ```nvarchar(5)```-Spalte mit dem Namen „name“ aufweist.

2. Innerhalb des Datenflusses sollen Filmtitel aus der Senke zur Zielspalte „name“ zugeordnet werden.

    ![Datenfluss für Filme 1](media/data-flow/error4.png)
    
3. Filmtitel passen jedoch nicht ganz in eine Senkenspalte, die nur 5 Zeichen umfassen kann. Beim Ausführen dieses Datenflusses erhalten Sie eine Fehlermeldung ähnlich der folgenden: ```"Job failed due to reason: DF-SYS-01 at Sink 'WriteToDatabase': java.sql.BatchUpdateException: String or binary data would be truncated. java.sql.BatchUpdateException: String or binary data would be truncated."```

## <a name="how-to-design-around-this-condition"></a>Umgehen dieser Fehlerbedingung

1. In diesem Szenario ist die maximale Länge der Spalte „name“ auf fünf Zeichen beschränkt. Daher fügen wir eine Transformation für bedingtes Teilen hinzu, sodass Zeilen mit Titeln („title“) mit mehr als fünf Zeichen protokolliert werden können und gleichzeitig die restlichen Zeilen, die in diesen Bereich passen, in die Datenbank geschrieben werden können.

    ![Bedingtes Teilen](media/data-flow/error1.png)

2. Mit dieser Transformation für bedingtes Teilen wird die maximale Länge von „title“ auf fünf Zeichen festgelegt. Jede Zeile, die kleiner oder gleich fünf Zeichen ist, wird in den Datenstrom ```GoodRows``` geleitet. Jede Zeile mit mehr als fünf Zeilen wird in den Datenstrom ```BadRows``` geleitet.

3. Nun müssen die fehlerhaften Zeilen protokolliert werden. Fügen Sie dem Datenstrom ```BadRows``` eine Senkentransformation für die Protokollierung hinzu. Hier werden alle Felder automatisch zugeordnet, damit der gesamte Transaktionsdatensatz protokolliert wird. Dies ist die Ausgabe einer CSV-Datei mit Texttrennzeichen in einer einzelnen Datei in Blob Storage. Die Protokolldatei erhält den Namen „badrows.csv“.

    ![Datei „badrows.csv“ mit fehlerhaften Zeilen](media/data-flow/error3.png)
    
4. Der vollständige Datenfluss ist nachfolgend dargestellt. Nun können fehlerhafte Zeilen abgespaltet werden, um SQL-Kürzungsfehler zu vermeiden, und diese Einträge können in einer Protokolldatei eingefügt werden. Fehlerfreie Zeilen können dagegen weiterhin in die Zieldatenbank geschrieben werden.

    ![Vollständiger Datenfluss](media/data-flow/error2.png)

## <a name="next-steps"></a>Nächste Schritte

* Erstellen Sie die restliche Datenflusslogik mithilfe von Mapping Data Flow-[Transformationen](concepts-data-flow-overview.md).
