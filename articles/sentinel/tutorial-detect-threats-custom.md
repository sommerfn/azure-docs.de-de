---
title: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von verdächtigen Bedrohungen mit Azure Sentinel | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie benutzerdefinierte Analyseregeln erstellen, um verdächtige Bedrohungen mit Azure Sentinel zu erkennen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 5d4c549eaded78c69d3e7fa7173b5ad9b1d82f2f
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333141"
---
# <a name="tutorial-create-custom-analytic-rules-to-detect-suspicious-threats"></a>Tutorial: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von verdächtigen Bedrohungen

Nachdem Sie  [Ihre Datenquellen mit Azure Sentinel verbunden](quickstart-onboard.md)  haben, können Sie benutzerdefinierte Regeln erstellen, mit denen Sie in Ihrer Umgebung nach bestimmten Kriterien suchen und bei einer Übereinstimmung Vorfälle generieren. Diese können Sie anschließend untersuchen. In diesem Tutorial erfahren Sie, wie Sie benutzerdefinierte Regeln zum Erkennen von Bedrohungen mit Azure Sentinel erstellen.

Dieses Tutorial unterstützt Sie bei der Gefahrenerkennung mit Azure Sentinel.
> [!div class="checklist"]
> * Erstellen von Analyseregeln
> * Automatisieren der Reaktionen auf Bedrohungen

## <a name="create-custom-analytic-rules"></a>Erstellen von benutzerdefinierten Analyseregeln

Für die Suche nach verdächtigen Typen von Bedrohungen und Anomalien in Ihrer Umgebung können Sie benutzerdefinierte Analyseregeln erstellen. Diese Regeln stellen sicher, dass Sie im Bedarfsfall sofort benachrichtigt werden, um die Bedrohungen einzugrenzen, zu überprüfen und zu beheben.

1. Wählen Sie im Azure-Portal unter „Azure Sentinel“ die Option **Analytics** aus.

1. Wählen Sie in der oberen Menüleiste die Option **+Erstellen** und anschließend **Geplante Abfrageregel** aus. Dadurch wird der **Assistent zum Erstellen von benutzerdefinierten Regeln** geöffnet.

    ![Erstellen einer geplanten Abfrage](media/tutorial-detect-threats-custom/create-scheduled-query.png)

1. Geben Sie auf der Registerkarte **Allgemein** einen aussagekräftigen Namen und eine Beschreibung ein. Legen Sie den **Schweregrad der Warnung** nach Bedarf fest. Sie können die Regel beim Erstellen auch gleich aktivieren. Dies führt dazu, dass sie sofort nach der Erstellung ausgeführt wird. Wenn Sie die Regel deaktiviert erstellen, wird sie der Registerkarte **Aktive Regeln** hinzugefügt. Dort können Sie sie jederzeit aktivieren.

    ![Erste Phase beim Erstellen einer benutzerdefinierten Analyseregel](media/tutorial-detect-threats-custom/general-tab.png)

1. Sie können eine Abfrage auf der Registerkarte **Einstellungen** direkt eingeben, oder Sie können sie in Log Analytics erstellen und anschließend in das Feld **Suchabfrage** einfügen. Wenn Sie Ihre Abfrage ändern und konfigurieren, simuliert Azure Sentinel die Abfrageergebnisse im Fenster **Ergebnisvorschau** rechts. Dadurch können Sie sofort sehen, wie viele Daten in einem bestimmten Zeitintervall für die erstellte Warnung generiert werden. Der Umfang der Daten hängt dabei von den Werten ab, die Sie für die Felder **Abfrage ausführen alle** und **Datensuche für letzte(n)** festlegen. Wenn Sie sehen, dass Ihre Warnungen im Schnitt zu häufig ausgelöst werden, können Sie für die Anzahl der Ergebnisse einen höheren Wert festlegen, der über der durchschnittlichen Baseline liegt.

   ![Erstellen einer Abfrage in Azure Sentinel](media/tutorial-detect-threats-custom/settings-tab.png)

   Mit der folgenden Beispielabfrage werden Sie gewarnt, wenn in Azure Activity eine ungewöhnliche Anzahl von Ressourcen erstellt wird.

    `AzureActivity
    \| where OperationName == "Create or Update Virtual Machine" or OperationName =="Create Deployment"
    \| where ActivityStatus == "Succeeded"
    \| make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller`

   > [!NOTE]
   > Die Abfrage sollte zwischen 1 und 1.000 Zeichen lang sein und darf weder „search \*“ noch „union \*“ enthalten.

    1. Legen Sie unter **Abfrageplanung** die folgenden Parameter fest:

        1.  Legen Sie bei **Abfrage ausführen alle** unter **Häufigkeit** fest, wie häufig die Abfrage ausgeführt werden soll: von alle fünf Minuten bis zu einmal am Tag.

        1.  Legen Sie bei **Daten abrufen von letztem/letzter** fest, über welchen Zeitraum die Abfrage für die entsprechende Datenmenge ausgeführt werden soll. Beispielsweise kann die Abfrage stündlich für die Daten der letzten 60 Minuten ausgeführt werden.

        1. Wenn eine Warnung bei Eintreten nur einmal angezeigt werden soll, können Sie die Option **Abfrage beenden, nachdem Warnung generiert wurde** festlegen. Dazu müssen Sie das Zeitfenster, in dem die Abfrage nicht ausgeführt werden soll, auf 24 Stunden festlegen.

    1. Legen Sie unter **Alert trigger** (Warnungstrigger) Triggerbedingungen fest. Ordnen Sie unter **Entitätszuordnung** die Spalten in Ihrer Abfrage den Entitätsfeldern zu, die von Azure Sentinel erkannt werden. Ordnen Sie jedem Entitätsfeld die entsprechende Spalte in der Abfrage zu, die Sie in Log Analytics erstellt haben. Jede Entität enthält mehrere Felder, wie etwa SID und GUID. Sie können jedes Feld der Entität für die Zuordnung verwenden, nicht nur die übergeordnete Entität.

1.  Wählen Sie auf der Registerkarte **Automate responses** (Reaktionen automatisieren) alle Playbooks aus, die automatisch ausgeführt werden sollen, wenn durch die benutzerdefinierte Regel eine Warnung generiert wird. Weitere Informationen zum Erstellen und Automatisieren von Playbooks finden Sie unter  [Reaktionen auf Bedrohungen](tutorial-respond-threats-playbook.md).

    ![Automatisieren der Reaktion auf Bedrohungen in Azure Sentinel](media/tutorial-detect-threats-custom/response-automation-custom.png)

1. Wählen Sie **Überprüfen** aus, um alle Einstellungen für die neue Warnungsregel zu überprüfen, und wählen Sie dann **Erstellen** aus, um die Warnungsregel zu initialisieren.

   ![Überprüfen der benutzerdefinierten Abfrage](media/tutorial-detect-threats-custom/review-tab.png)

1.  Nachdem die Warnung erstellt wurde, wird der Tabelle unter **Aktive Regeln** eine benutzerdefinierte Regel hinzugefügt. Über diese Liste können Sie die einzelnen Regeln aktivieren, deaktivieren oder löschen.

1.  Die Ergebnisse der von Ihnen erstellten Warnungsregeln können Sie auf der Seite **Vorfälle** anzeigen. Hier können Sie Bedrohungen eingrenzen,  [Vorfälle untersuchen](tutorial-investigate-cases.md) und die Bedrohungen beseitigen.


> [!NOTE]
> In Azure Sentinel generierte Warnungen sind über die  [Sicherheits-API von Microsoft Graph](https://aka.ms/securitygraphdocs) verfügbar. Weitere Informationen finden Sie in der Dokumentation zur  [Sicherheits-API von Microsoft Graph](https://aka.ms/graphsecurityreferencebetadocs).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Einführung erhalten, wie Bedrohungen mithilfe von Azure Sentinel erkannt werden.

Informationen zum Automatisieren der Reaktionen auf Bedrohungen finden Sie im Tutorial  [Einrichten automatisierter Reaktionen auf Bedrohungen in Azure Sentinel](tutorial-respond-threats-playbook.md).

