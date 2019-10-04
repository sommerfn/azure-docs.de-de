---
title: Untersuchen von Vorfällen mit Azure Sentinel Preview | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Vorfälle mit Azure Sentinel untersuchen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a493cd67-dc70-4163-81b8-04a9bc0232ac
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: bad3fddd6caf7e6eb455e59280f181c787b95a4e
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780399"
---
# <a name="tutorial-investigate-incidents-with-azure-sentinel-preview"></a>Tutorial: Untersuchen von Vorfällen mit Azure Sentinel Preview

> [!IMPORTANT]
> Azure Sentinel ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Dieses Tutorial unterstützt Sie bei der Gefahrenerkennung mit Azure Sentinel.

Nachdem Sie [Ihre Datenquellen mit Azure Sentinel verbunden](quickstart-onboard.md) haben, möchten Sie gewarnt werden, wenn etwas Verdächtiges geschieht. Damit Sie dies tun können, ermöglicht Azure Sentinel es Ihnen, erweiterte Warnungsregeln zu erstellen, die Vorfälle generieren, die Sie zuweisen und verwenden können, um Anomalien und Bedrohungen für Ihre Umgebung gründlich untersuchen zu können. 

> [!div class="checklist"]
> * Erstellen von Vorfällen
> * Untersuchen von Vorfällen
> * Reagieren auf Bedrohungen

## <a name="investigate-incidents"></a>Untersuchen von Vorfällen

Ein Vorfall kann mehrere Warnungen enthalten. Es ist eine Aggregation aller relevanten Beweise für eine bestimmte Untersuchung. Ein Vorfall wird auf Grundlage von Warnungen erstellt, die Sie auf der Seite **Analyse** angegeben haben. Die Eigenschaften, die zu den Warnungen gehören, z. B. Schweregrad und Status, werden auf Vorfallsebene festgelegt. Nachdem Azure Sentinel die Informationen dazu hat, welche Bedrohungsarten Sie suchen und wie Sie diese finden, können Sie erkannte Bedrohungen überwachen, indem Sie Vorfälle untersuchen. 

1. Wählen Sie **Vorfälle** aus. Auf der Seite **Vorfälle** werden Sie darüber informiert, wie viele Vorfälle vorhanden sind, wie viele offen sind, für wie viele Sie **In progress** (In Arbeit) festgelegt haben, und wie viele geschlossen wurden. Für jeden Vorfall wird angezeigt, wann er aufgetreten ist, sowie der Vorfallsstatus. Beurteilen Sie mithilfe des Schweregrads, welche Fälle zuerst bearbeitet werden. Klicken Sie auf der Seite **Vorfälle** auf die Registerkarte **Warnungen**, um alle zu einem Vorfall gehörenden Warnungen anzeigen zu lassen. Entitäten, die Sie zuvor als Teil des Vorfalls zugeordnet haben, können Sie sich in der Registerkarte **Entitäten** ansehen.  Sie können die Vorfälle nach Bedarf filtern, z. B. nach Status oder Schweregrad. Wenn Sie sich die Registerkarte **Vorfälle** ansehen, werden Ihnen offene Vorfälle angezeigt, die Warnungen enthalten, die von Ihren auf der Seite **Analyse** festgelegten Erkennungsregeln ausgelöst wurden. Im oberen Bereich werden Ihre aktiven Vorfälle angezeigt, neue Vorfälle sowie Vorfälle, die sich in Arbeit befinden. Sie können auch eine Übersicht über alle Vorfälle nach Schweregrad geordnet anzeigen lassen.

   ![Warnungsdashboard](./media/tutorial-investigate-cases/cases.png)

2. Klicken Sie auf einen bestimmten Vorfall, um mit der Untersuchung zu beginnen. Rechts sehen Sie detaillierte Informationen zum Vorfall, einschließlich seines Schweregrads und einer Zusammenfassung der Anzahl an Entitäten, die auf Grundlage Ihrer Zuordnung im Vorfall involviert sind. Jeder Vorfall hat eine eindeutige ID. Der Schweregrad eines Vorfalls wird gemäß der zum Vorfall gehörenden Warnungen mit dem höchsten Schweregrad bestimmt.  

1. Wenn Sie sich mehr Details zu den Warnungen und Entitäten des Vorfalls ansehen möchten, klicken Sie auf der Vorfallsseite auf **View full details** (Vollständige Details anzeigen), und prüfen Sie die relevanten Registerkarten, in denen die Vorfallsinformationen zusammengefasst sind.  Die vollständige Vorfallsansicht fasst alle Beweise in der Warnung zusammen, die zugeordneten Warnungen und Entitäten.

1. Prüfen Sie in der Registerkarte **Warnungen** die Warnung selbst, z. B. wann sie ausgelöst wurde, und um wie viel sie den von Ihnen festgelegten Schwellenwert überschreitet. Sie können sich alle relevanten Informationen zur Warnung ansehen: die Abfrage, die die Warnung ausgelöst hat, die Anzahl der Ergebnisse, die pro Abfrage zurückgegeben wurde, und die Möglichkeit, Playbooks für die Warnungen auszuführen. Wenn Sie noch detaillierte Informationen zum Vorfall erhalten möchten, klicken Sie auf die Zahl der Treffer. Dadurch wird die Abfrage geöffnet, die die Ergebnisse generiert hat, und die Ergebnisse, die die Warnung in Log Analytics ausgelöst haben.

3. In der Registerkarte **Entitäten** können Sie sich alle Entitäten ansehen, die Sie als Teil der Warnungsregeldefinition zugeordnet haben. 

4. Wenn Sie einen Vorfall aktiv untersuchen, empfiehlt es sich, den Vorfallsstatus auf **In Arbeit** festzulegen, bis Sie ihn schließen. Sie können den Vorfall auch schließen. Hierbei zeigt der Status **closed resolved** (geschlossen, aufgelöst) an, dass ein Vorfall bearbeitet wurde, während der Status **closed dismissed** (geschlossen, verworfen) anzeigt, dass Vorfälle nicht bearbeitet werden müssen. Sie müssen Erklärungen abgeben, warum genau Sie einen Vorfall schließen.

5. Vorfälle können einem bestimmten Benutzer zugewiesen werden. Jedem Vorfall können Sie einen Besitzer zuweisen, indem Sie das Feld **Besitzer** festlegen. Alle Vorfälle sind zu Beginn nicht zugewiesen. Sie können die Vorfälle nach Ihrem Namen filtern, um alle Vorfälle anzeigen zu lassen, deren Besitzer Sie sind. 

5. Klicken Sie auf **Untersuchen**, um die Untersuchungszuordnung und das Ausmaß der Sicherheitsverletzung mit Schritten zur Abwehr anzuzeigen. 



## <a name="respond-to-threats"></a>Reagieren auf Bedrohungen

In Azure Sentinel stehen Ihnen zwei Hauptoptionen zur Verfügung, um mithilfe von Playbooks auf Bedrohungen zu reagieren. Sie können festlegen, dass ein Playbook automatisch ausgeführt wird, wenn eine Warnung ausgelöst wird, oder Sie können ein Playbook manuell als Reaktion auf eine Warnung ausführen.

- Bei der Konfiguration des Playbooks können Sie festlegen, dass es automatisch ausgeführt wird, wenn eine Warnung ausgelöst wird. 

- Innerhalb der Warnung können Sie ein Playbook manuell ausführen, indem Sie auf **Playbooks anzeigen** klicken und dann ein Playbook auswählen, das ausgeführt werden soll.




## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine Einführung erhalten, wie Vorfälle mithilfe von Azure Sentinel untersucht werden. Fahren Sie mit dem [Tutorial: Einrichten automatisierter Reaktionen auf Bedrohungen in der Vorschauversion von Azure Sentinel](tutorial-respond-threats-playbook.md) fort.
> [!div class="nextstepaction"]
> [Reagieren Sie auf Bedrohungen](tutorial-respond-threats-playbook.md), um Ihre Maßnahmen zur Reaktion auf Bedrohungen zu automatisieren.

