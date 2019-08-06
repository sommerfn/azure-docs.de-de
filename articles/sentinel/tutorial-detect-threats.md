---
title: Untersuchen von Warnungen mit Azure Sentinel Preview | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Warnungen mit Azure Sentinel untersucht werden.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: b5fbc5ac-68b2-4024-9c1b-bd3cc41a66d0
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2019
ms.author: rkarlin
ms.openlocfilehash: ad9c752898733286701db2d0f0b1fc40029b7521
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2019
ms.locfileid: "68370715"
---
# <a name="tutorial-detect-threats-with-azure-sentinel-preview"></a>Tutorial: Erkennen von Bedrohungen mit Azure Sentinel Preview

> [!IMPORTANT]
> Azure Sentinel befindet sich momentan in der Public Preview.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Nachdem Sie [Ihre Datenquellen mit Azure Sentinel verbunden](quickstart-onboard.md) haben, möchten Sie gewarnt werden, wenn etwas Verdächtiges geschieht. Damit Sie dies tun können, ermöglicht Azure Sentinel es Ihnen, erweiterte Warnungsregeln zu erstellen, die Fälle generieren, die Sie zuweisen und verwenden können, um Anomalien und Bedrohungen für Ihre Umgebung gründlich untersuchen zu können. 

Dieses Tutorial unterstützt Sie bei der Gefahrenerkennung mit Azure Sentinel.
> [!div class="checklist"]
> * Erstellen von Erkennungsregeln
> * Automatisieren der Reaktionen auf Bedrohungen

## <a name="create-detection-rules"></a>Erstellen von Erkennungsregeln

Wenn Sie Fälle untersuchen möchten, müssen Sie zunächst Erkennungsregeln erstellen. 

> [!NOTE]
> In Azure Sentinel generierte Warnungen stehen über [Microsoft Graph Security](https://aka.ms/securitygraphdocs) zur Verfügung. Weitere Informationen und eine Übersicht der Integrationspartner finden Sie in der Dokumentation zu [Microsoft Graph Security-Warnungen](https://aka.ms/graphsecurityreferencebetadocs).

Erkennungsregeln basieren auf der Art der Bedrohungen und Anomalien, die in Ihrer Umgebung verdächtig sein könnten und über die Sie umgehend informiert werden möchten, sodass sichergestellt ist, dass sie erkannt, untersucht und beseitigt werden. 

1. Wählen Sie im Azure-Portal unter „Azure Sentinel“ die Option **Analytics** aus.

   ![Analytics](./media/tutorial-detect-threats/alert-rules.png)

2. Klicken Sie in der Menüleiste auf **+Hinzufügen**.  

   ![Erstellen einer Warnungsregel](./media/tutorial-detect-threats/create-alert-rule.png)

3. Geben Sie unter **Warnungsregel erstellen** einen aussagekräftigen Namen ein, und legen Sie ggf. für den **Schweregrad** einen Wert fest. 

4. Erstellen Sie die Abfrage in Log Analytics, und fügen Sie sie anschließend in das Feld **Set alert rule** (Warnungsregel festlegen) ein. Mit der folgenden Beispielabfrage werden Sie gewarnt, wenn in Azure Activity eine ungewöhnliche Anzahl von Ressourcen erstellt wird.

        AzureActivity
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"
        | where ActivityStatus == "Succeeded"
        | make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller

   > [!NOTE]
   > Die Abfragelänge sollte zwischen 1 und 10.000 Zeichen sein und kann weder „search *“ noch „union *“ enthalten.


5. Verwenden Sie im Abschnitt **Entitätszuordnung** die Felder unter **Entitätstyp**, um Entitätsfeldern, die von Azure Sentinel erkannt werden, die Spalten in Ihrer Abfrage zuzuordnen. Ordnen Sie jedem Entitätsfeld die entsprechende Spalte in der Abfrage zu, die Sie in Log Analytics erstellt haben. Wählen Sie unter **Eigenschaft** den entsprechenden Spaltennamen aus. Jede Entität enthält mehrere Felder, wie etwa SID, GUID usw. Sie können jedes Feld der Entität für die Zuordnung verwenden, unabhängig von dessen Position in der Feldhierarchie. Es können also auch untergeordnete Felder zugeordnet werden.

6. Legen Sie unter **Alert trigger** (Warnungstrigger) Triggerbedingungen fest. Damit werden Bedingungen definiert, gemäß denen die Warnung ausgelöst wird. 

7. Legen Sie unter **Häufigkeit** fest, wie häufig die Abfrage ausgeführt wird: häufig (alle fünf Minuten) oder selten (einmal am Tag). 

8. Geben Sie den **Zeitraum** an, um das Zeitfenster für eine Datenmenge festzulegen, für die die Abfrage ausgeführt wird. Beispielsweise kann die Abfrage jede Stunde für 60 Minuten Daten ausgeführt werden.

9. Sie können auch die **Unterdrückung** festlegen. Eine Unterdrückung ist hilfreich, wenn Sie möchten, dass für einen Vorfall Warnungen nicht mehrfach ausgelöst werden. So können Sie festlegen, dass während eines bestimmten Zeitraums keine Warnungen ausgelöst werden. Damit werden mehrfache Warnungen für einen Vorfall verhindert und weitere Warnungen für eine bestimmte Zeitspanne unterdrückt. Wenn beispielsweise für **Alert scheduling** **Frequency** (Warnungsplanung: Häufigkeit) 60 Minuten und für **Alert scheduling Period** (Warnungsplanung: Zeitraum) zwei Stunden festgelegt wurden und die Abfrageergebnisse den festgelegten Schwellenwert überschritten haben, wird eine Warnung zweimal ausgelöst: einmal, wenn erkannt wird, dass die 60 Minuten überschritten wurden, und erneut, wenn sie sich in den ersten Minuten der 2-stündigen Datenerfassung befindet. Für den Fall, dass eine Warnung ausgelöst wird, wird empfohlen, für die Unterdrückung die Zeitdauer festzulegen, die für den Warnungszeitraum festgelegt wurde. In unserem Beispiel sollten Sie für die Unterdrückung 60 Minuten festlegen, sodass Warnungen nur für Ereignisse ausgelöst werden, die in der letzten Stunde auftreten.

8. Nachdem Sie die Abfrage in das Feld **Set alert rule** (Warnungsregel festlegen) eingefügt haben, wird unter **Logic alert simulation** (Logik für Simulation der Warnung) sofort eine Simulation der Warnung angezeigt, sodass Sie sehen können, wie viele Daten über ein bestimmtes Zeitintervall für die erstellte Warnung generiert werden. Das hängt davon ab, welche Werte Sie für die **Häufigkeit** und den **Schwellenwert** festgelegt haben. Wenn Sie sehen, dass Ihre Warnung im Schnitt zu häufig ausgelöst wird, sollten Sie für die Anzahl der Ergebnisse einen höheren Wert festlegen, sodass er über Ihrer durchschnittlichen Baseline liegt.

9. Klicken Sie auf **Erstellen**, um Ihre Warnungsregel zu initialisieren. Nach dem Erstellen der Warnung wird ein Fall erstellt, der die Warnung enthält. Auf der Registerkarte **Security Analytics** (Sicherheitsanalyse) werden die definierten Erkennungsregeln in Zeilen angezeigt. Hier wird auch die Anzahl der Treffer für jede Regel angezeigt (d. h. Situationen, in denen die Warnungen ausgelöst wurden). Über diese Liste können Sie die einzelnen Regeln aktivieren, deaktivieren oder löschen. Sie können auch mit der rechten Maustaste auf die Auslassungspunkte (...) am Ende der Zeile für die einzelnen Warnungen klicken, um eine Regel zu bearbeiten, zu deaktivieren, zu klonen, zu löschen oder um Treffer anzuzeigen. Bei der Seite **Analyse** handelt es sich um einen Katalog mit allen aktiven Warnungsregeln, darunter auch Vorlagen, die Sie aktivieren können, und Warnungsregeln, die Sie auf Basis von Vorlagen erstellt haben.

1. Das Ergebnis der Warnungsregeln können auf der Seite **Fälle** angezeigt werden. Hier können Sie Bedrohungen eingrenzen, [Fälle untersuchen](tutorial-investigate-cases.md) und Bedrohungen beseitigen.



## <a name="automate-threat-responses"></a>Automatisieren der Reaktionen auf Bedrohungen

SIEM/SOC-Teams können gelegentlich von Sicherheitswarnungen geradezu überschwemmt werden. Dabei kann das Warnungsaufkommen so hoch sein, dass die zur Verfügung stehenden Sicherheitsadministratoren überlastet sind. Dies führt oftmals dazu, dass zahlreiche Warnungen nicht untersucht werden können und Organisationen anfällig für nicht erkannte Angriffe werden. 

Viele – wenn nicht sogar der Großteil – dieser Warnungen entsprechen wiederkehrenden Mustern, die mithilfe spezifischer, definierter Wartungsaktionen verarbeitet werden können. Azure Sentinel ermöglicht Ihnen in Playbooks bereits das Definieren von Wartungsaktionen. Darüber hinaus ist es möglich, Echtzeitautomatisierung als Teil der Playbookdefinition festzulegen, um Ihnen die vollständige Automatisierung einer definierten Reaktion auf bestimmte Sicherheitswarnungen zu ermöglichen. Mithilfe der Echtzeitautomatisierung können zuständige Teams die Arbeitsauslastung erheblich reduzieren, indem sie die Routinereaktionen auf wiederkehrende Warnungstypen vollständig automatisieren, sodass Sie sich auf eindeutige Warnungen, die Analyse von Mustern, die Ermittlung von Bedrohungen und mehr konzentrieren können.

So automatisieren Sie Reaktionen:

1. Wählen Sie die Warnung aus, für die Sie die Reaktion automatisieren möchten.
1. Wählen Sie im Navigationsmenü des Azure Sentinel-Arbeitsbereichs die Option **Analytics** aus.
1. Wählen Sie die zu automatisierende Warnung aus. 
1. Wählen Sie auf der Seite **Warnungsregel bearbeiten** unter **Real-time automation** (Echtzeitautomatisierung) für **Triggered playbooks** (Ausgelöste Playbooks) das Playbook aus, das bei einer Übereinstimmung mit dieser Warnungsregel ausgeführt werden soll.
1. Wählen Sie **Speichern** aus.

   ![Echtzeitautomatisierung](./media/tutorial-detect-threats/rt-configuration.png)


Sie können die Behebung für eine Warnung auch manuell durchführen, indem Sie innerhalb der Warnung ein Playbook ausführen. Klicken Sie hierfür auf **Playbooks anzeigen**, und wählen Sie dann ein Playbook aus, das ausgeführt werden soll. Informationen zur Erstellung eines neuen bzw. Bearbeitung eines vorhandenen Playbooks finden Sie unter [Tutorial: Einrichten automatisierter Reaktionen auf Bedrohungen in der Vorschauversion von Azure Sentinel](tutorial-respond-threats-playbook.md).



## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine Einführung erhalten, wie Bedrohungen mithilfe von Azure Sentinel erkannt werden. 

Informationen zum Automatisieren Ihrer Reaktionen auf Bedrohungen finden Sie unter [Einrichten automatisierter Reaktionen auf Bedrohungen mithilfe automatisierter Playbooks](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Reagieren Sie auf Bedrohungen](tutorial-respond-threats-playbook.md), um Ihre Maßnahmen zur Reaktion auf Bedrohungen zu automatisieren.

