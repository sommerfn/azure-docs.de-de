---
title: 'Azure Data Factory Mapping Data Flow: Zeilenänderungstransformation'
description: Hier erfahren Sie, wie Sie das Datenbankziel mithilfe der Zeilenänderungstransformation von Azure Data Factory Mapping Data Flow aktualisieren.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: 7a782c62165aa6f2641c2ebe8e4600198ec373c5
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73486204"
---
# <a name="azure-data-factory-alter-row-transformation"></a>Azure Data Factory: Zeilenänderungstransformation

Verwenden Sie die Zeilenänderungstransformation, um Einfüge-, Lösch-, Aktualisierungs- und Upsertrichtlinien für Zeilen festzulegen. Sie können 1: n-Bedingungen als Ausdrücke hinzufügen. Diese Bedingungen müssen in der Reihenfolge ihrer Priorität angegeben werden, da jede Zeile mit der Richtlinie gekennzeichnet wird, die dem ersten übereinstimmenden Ausdruck entspricht. Jede dieser Bedingungen kann dazu führen, dass für eine Zeile (oder für mehrere Zeilen) ein Einfüge-, Aktualisierungs-, Lösch- oder Upsertvorgang ausgeführt wird. Die Zeilenänderung kann sowohl DDL- als auch DML-Aktionen für Ihre Datenbank generieren.



![Zeilenänderungseinstellungen](media/data-flow/alter-row1.png "Zeilenänderungseinstellungen")

> [!NOTE]
> Zeilenänderungstransformationen können nur für Datenbanksenken oder Cosmos DB-Senken in Ihrem Datenfluss verwendet werden. Die Aktionen, die Sie Zeilen zuweisen (Einfügen, Aktualisieren, Löschen, Upsert), werden nicht während Debugsitzungen ausgeführt. Sie müssen einer Pipeline eine Aufgabe zum Ausführen des Datenflusses hinzufügen und das Pipelinedebugging oder Trigger verwenden, um die Zeilenänderungsrichtlinien auf Ihre Datenbanktabellen anzuwenden.

## <a name="indicate-a-default-row-policy"></a>Angeben einer Standardzeilenrichtlinie

Erstellen Sie eine Zeilenänderungstransformation, und geben Sie eine Zeilenrichtlinie mit der Bedingung `true()` an. Jede Zeile, die keinen der zuvor definierten Ausdrücke enthält, wird für die angegebene Zeilenrichtlinie gekennzeichnet. Standardmäßig wird jede Zeile, die keinen bedingten Ausdruck erfüllt, für `Insert` gekennzeichnet.

![Richtlinie für Zeilenänderung 1](media/data-flow/alter-row4.png "Richtlinie für Zeilenänderung 1")

> [!NOTE]
> Wenn Sie alle Zeilen mit einer Richtlinie kennzeichnen möchten, können Sie eine Bedingung für diese Richtlinie erstellen und `true()` als Bedingung angeben.

## <a name="view-policies"></a>Anzeigen von Richtlinien

Aktivieren Sie den Datenfluss-Debugmodus, um die Ergebnisse der Zeilenänderungsrichtlinien im Datenvorschaubereich anzuzeigen. Wenn Sie eine Zeilenänderung im Datenfluss-Debugmodus ausführen, werden keine DDL- oder DML-Aktionen für Ihr Ziel generiert. Damit diese Aktionen ausgeführt werden, muss der Datenfluss innerhalb einer Aufgabe zum Ausführen des Datenflusses in einer Pipeline ausgeführt werden.

![Richtlinien für Zeilenänderungen](media/data-flow/alter-row3.png "Richtlinien für Zeilenänderungen")

Dadurch können Sie den Zustand der einzelnen Zeilen auf der Grundlage Ihrer Bedingungen überprüfen und anzeigen. Die in Ihrem Datenfluss ausgeführten Einfüge-, Aktualisierungs-, Lösch- und Upsertaktionen werden jeweils durch ein Symbol dargestellt, um anzugeben, welche Aktion erfolgt, wenn der Datenfluss innerhalb einer Pipeline ausgeführt wird.

## <a name="sink-settings"></a>Senkeneinstellungen

Für Zeilenänderungen wird eine Datenbanksenke vorausgesetzt. In den Einstellungen für die Senke sollten Sie jede Aktion, die den Zeilenänderungsbedingungen entspricht, als zulässig festlegen.

![Senke für Zeilenänderung](media/data-flow/alter-row2.png "Senke für Zeilenänderung")

In einem ADF-Datenfluss mit Datenbanksenken werden Zeilen standardmäßig eingefügt. Wenn Sie auch Aktualisierungs-, Upsert- und Löschvorgänge zulassen möchten, müssen Sie in der Senke die entsprechenden Kontrollkästchen aktivieren.

> [!NOTE]
> Falls durch Ihre Einfüge-, Aktualisierungs- oder Upsertvorgänge das Schema der Zieltabelle in der Senke geändert wird, ist Ihr Datenfluss nicht erfolgreich. Wenn Sie das Zielschema in Ihrer Datenbank ändern möchten, müssen Sie in der Senke die Option für die Neuerstellung der Tabelle auswählen. Dadurch wird Ihre Tabelle verworfen und mit der neuen Schemadefinition neu erstellt.

## <a name="next-steps"></a>Nächste Schritte

Nach der Zeilenänderungstransformation können Sie [Ihre Daten in einen Zieldatenspeicher senken](data-flow-sink.md).
