---
title: Workflowautomatisierung (Vorschauversion) in Azure Security Center | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Workflows in Azure Security Center erstellen und automatisieren.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 74a33edb358f9b9bf9322c652c0696ef87182725
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73521364"
---
# <a name="workflow-automation-preview"></a>Workflowautomatisierung (Vorschauversion)

Jedes Sicherheitsprogramm umfasst mehrere Workflows für die Reaktion auf Vorfälle. Diese Prozesse können das Benachrichtigen relevanter Stakeholder, das Starten eines Change Management-Prozesses und das Anwenden spezifischer Korrekturschritte umfassen. Sicherheitsexperten empfehlen, möglichst viele Schritte dieser Verfahren zu automatisieren. Durch Automatisierung wird der Aufwand reduziert. Außerdem können Sie so die Sicherheit erhöhen, indem Sie sicherstellen, dass die Prozessschritte schnell, konsistent und gemäß Ihren vordefinierten Anforderungen ausgeführt werden.

In diesem Artikel wird die Funktion zur Workflowautomatisierung (Vorschauversion) von Azure Security Center beschrieben. Diese Previewfunktion kann Logic Apps bei Sicherheitswarnungen und Empfehlungen auslösen. Beispielsweise können Sie von Security Center E-Mail-Nachrichten an einen bestimmten Benutzer senden, wenn eine Warnung auftritt. Außerdem erfahren Sie, wie Sie Logik-Apps mithilfe von [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) erstellen.

> [!NOTE]
> Wenn Sie zuvor die Ansicht „Playbooks“ (Vorschauversion) auf der Seitenleiste verwendet haben, finden Sie dieselben Funktionen sowie die erweiterten Funktionen auf der neuen Seite „Workflowautomatisierung“ (Vorschauversion).


## <a name="requirements"></a>Requirements (Anforderungen)

* Damit Sie Azure Logic Apps-Workflows anwenden können, benötigen Sie die folgenden Logic Apps-Rollen/-Berechtigungen:

    * Die Berechtigungen der Rolle [Logik-App-Operator](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#logic-app-operator) oder der Lese-/Triggerzugriff von Logic Apps sind erforderlich. (Diese Rolle kann keine Logik-Apps erstellen oder bearbeiten, sondern nur vorhandene *ausführen*.)

    * Die Berechtigungen der Rolle [Logik-App-Mitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#logic-app-contributor) sind für die Erstellung und Änderung von Logik-Apps erforderlich.

* Wenn Sie Logik-App-Connectors verwenden möchten, benötigen Sie möglicherweise zusätzliche Anmeldeinformationen für die Anmeldung bei den jeweiligen Diensten (z. B. Ihren Instanzen von Outlook, Teams oder Slack).


## <a name="create-a-logic-app-and-define-when-it-should-automatically-run"></a>Erstellen einer Logik-App und Definieren des Zeitpunkts ihrer automatischen Ausführung 

1. Wählen Sie auf der Seitenleiste in Security Center **Workflowautomatisierung (Vorschauversion)** aus.

    [![Liste der Workflowautomatisierungen](media/workflow-automation/list-of-workflow-automations.png)](media/workflow-automation/list-of-workflow-automations.png#lightbox)

    Auf dieser Seite können Sie neue Automatisierungsregeln erstellen sowie vorhandene aktivieren, deaktivieren oder löschen.  
1. Um einen neuen Workflow zu definieren, klicken Sie auf **Workflowautomatisierung hinzufügen**. 

    Ein Bereich mit Optionen für die neue Automatisierung wird angezeigt. Darin können Sie Folgendes eingeben:
    1. Einen Namen und eine Beschreibung für die Automatisierung
    1. Die Trigger zum Auslösen dieses automatischen Workflows. Sie könnten beispielsweise Ihre Logik-App ausführen, wenn eine Sicherheitswarnung generiert wird, die „SQL“ enthält.
    1. Die Logik-App, die ausgeführt wird, wenn die Triggerbedingungen erfüllt sind 

        [![Liste der Workflowautomatisierungen](media/workflow-automation/add-workflow.png)](media/workflow-automation/add-workflow.png#lightbox)

1. Klicken Sie im Abschnitt „Aktionen“ auf **Erstellen Sie eine neue**, um mit der Erstellung der Logik-App zu beginnen.

    Sie werden zu Azure Logic Apps umgeleitet.

    [![Erstellen einer neuen Logik-App](media/workflow-automation/logic-apps-create-new.png)](media/workflow-automation/logic-apps-create-new.png#lightbox)

1. Geben Sie einen Namen, eine Ressourcengruppe und einen Speicherort ein, und klicken Sie auf **Erstellen**.

1. Sie können in Ihrer neuen Logik-App zwischen integrierten, vordefinierten Vorlagen der Kategorie „Sicherheit“ auswählen. Sie können aber auch einen benutzerdefinierten Ereignisflow definieren, der beim Auslösen dieses Prozesses gestartet wird.

    Im Logik-App-Designer werden die folgenden Trigger der Security Center-Connectors unterstützt:

    * **Beim Erstellen oder Auslösen einer Azure Security Center-Empfehlung (Vorschauversion)**
    * **Beim Erstellen oder Auslösen einer Azure Security Center-Warnung (Vorschauversion)**
    
    > [!NOTE]
    > Wenn Sie den älteren Trigger „Beim Auslösen einer Antwort auf eine Azure Security Center-Warnung“ verwenden, wird Ihre Logik-App nicht von der Funktion Workflowautomatisierung gestartet. Verwenden Sie stattdessen einen der oben genannten Trigger. 

    [![Beispiel-Logik-App](media/workflow-automation/sample-logic-app.png)](media/workflow-automation/sample-logic-app.png#lightbox)

1. Kehren Sie nach dem Definieren Ihrer Logik-App zum Bereich zur Definition der Workflowautomatisierung („Workflowautomatisierung hinzufügen“) zurück. Klicken Sie auf **Aktualisieren**, um sicherzustellen, dass Ihre neue Logik-App als Auswahl verfügbar ist.

    ![Aktualisieren](media/workflow-automation/refresh-the-list-of-logic-apps.png)

1. Wählen Sie Ihre Logik-App aus, und speichern Sie die Automatisierung.


## <a name="manually-trigger-a-logic-app"></a>Manuelles Auslösen einer Logik-App

Sie können Logik-Apps auch manuell ausführen, wenn Sie sich eine Sicherheitsempfehlung ansehen.

Um eine Logik-App manuell auszuführen, öffnen Sie eine Empfehlung, und klicken Sie auf „Logik-App auslösen (Vorschauversion)“:

[![Manuelles Auslösen einer Logik-App](media/workflow-automation/manually-trigger-logic-app.png)](media/workflow-automation/manually-trigger-logic-app.png#lightbox)


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Logik-Apps erstellen, diese manuell in Security Center ausführen und ihre Ausführung automatisieren. 

Weitere verwandte Informationen finden Sie in den folgenden Artikeln: 

- [Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md)
- [Sicherheitswarnungen in Azure Security Center](security-center-alerts-overview.md)
- [Informationen zu Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)
- [Logik-Apps-Connectors](https://docs.microsoft.com/connectors/)