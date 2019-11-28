---
title: Verwenden von Anwendungen in Azure Blockchain Workbench
description: Tutorial zur Verwendung von Anwendungsverträgen in der Vorschauversion von Azure Blockchain Workbench
ms.date: 10/14/2019
ms.topic: tutorial
ms.reviewer: brendal
ms.openlocfilehash: 5761bf1294691c2d50e0e389fe69ec286df4a06c
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74324040"
---
# <a name="tutorial-using-applications-in-azure-blockchain-workbench"></a>Tutorial: Verwenden von Anwendungen in Azure Blockchain Workbench

Mithilfe von Blockchain Workbench können Sie Aktionen zu Verträgen erstellen und ausführen. Zudem können Sie Vertragsdetails wie Status und Transaktionsverlauf einsehen.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen eines neuen Vertrags
> * Ausführen einer Aktion für einen Vertrag

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Eine Blockchain Workbench-Bereitstellung. Weitere Informationen zu Details dieser Bereitstellung finden Sie unter [Azure Blockchain Workbench-Bereitstellung](deploy.md).
* Eine bereitgestellte Blockchainanwendung in Blockchain Workbench. Siehe [Erstellen einer Blockchainanwendung in Blockchain Workbench](create-app.md).

[Öffnen Sie die Blockchain Workbench](deploy.md#blockchain-workbench-web-url) in Ihrem Browser.

![Blockchain Workbench](./media/use/workbench.png)

Sie müssen sich als Mitglied der Blockchain Workbench anmelden. Wenn keine Anwendungen aufgelistet sind, sind Sie Mitglied der Blockchain Workbench, aber nicht Mitglied einer Anwendung. Der Administrator der Blockchain Workbench kann den Anwendungen Mitglieder zuordnen.

## <a name="create-new-contract"></a>Erstellen eines neuen Vertrags

Um einen neuen Vertrag zu erstellen, müssen Sie ein Mitglied sein, das als ein **Initiator** eines Vertrags angegeben wurde. Informationen zum Definieren der Anwendungsrollen und Initiatoren für den Vertrag finden Sie in der [Übersicht über Workflows in der Konfiguration](configuration.md#workflows). Informationen zum Zuweisen von Mitgliedern zu Anwendungsrollen finden Sie unter [Hinzufügen eines Mitglieds zu einer Anwendung](manage-users.md#add-member-to-application).

1. Wählen Sie im Abschnitt der Blockchain Workbench-Anwendung die Anwendungskachel aus, die den zu erstellenden Vertrag enthält. Eine Liste der aktiven Verträge wird angezeigt.

2. Um einen neuen Vertrag zu erstellen, wählen Sie **Neuer Vertrag**.

    ![Schaltfläche „Neuer Vertrag“](./media/use/contract-list.png)

3. Das Fenster **Neuer Vertrag** wird angezeigt. Geben Sie die Werte der Anfangsparameter an. Klicken Sie auf **Erstellen**.

    ![Bereich „Neuer Vertrag“](./media/use/new-contract.png)

    Der neu angelegte Vertrag wird in der Liste mit den anderen aktiven Verträgen angezeigt.

    ![Liste der aktiven Verträge](./media/use/active-contracts.png)

## <a name="take-action-on-contract"></a>Ausführen von Aktionen für den Vertrag

Abhängig vom Status des Vertrags können Mitglieder Aktionen für den Übergang in den nächsten Status des Vertrags ausführen. Aktionen sind als [Übergänge](configuration.md#transitions) innerhalb eines [Status](configuration.md#states) definiert. Mitglieder, die zu einer zulässigen Anwendungs- oder Instanzrolle für den Übergang gehören, können die Aktion ausführen. 

1. Wählen Sie im Abschnitt der Blockchain Workbench-Anwendung die Anwendungskachel, die den Vertrag enthält, um die Aktion auszuführen.
2. Wählen Sie den Vertrag in der Liste aus. Die Details zum Vertrag werden in verschiedenen Abschnitten angezeigt. 

    ![Vertragsdetails](./media/use/contract-details.png)

    | `Section`  | BESCHREIBUNG  |
    |---------|---------|
    | Status | Liste des aktuellen Fortschritts innerhalb der Vertragsstufen |
    | Details | Die aktuellen Werte des Vertrags |
    | Aktion | Details zur letzten Aktion |
    | Aktivität | Transaktionsverlauf des Vertrags |
    
3. Wählen Sie im Abschnitt **Aktion** die Option **Aktion ausführen**.

4. Die Details zum aktuellen Status des Vertrags werden in einem Bereich angezeigt. Wählen Sie im Dropdownfeld die gewünschte Aktion aus. 

    ![Aktion auswählen](./media/use/choose-action.png)

5. Wählen Sie **Aktion ausführen** aus, um die Aktion zu initiieren.
6. Wenn Parameter für die Aktion erforderlich sind, geben Sie die Werte für die Aktion an.

    ![Ausführen einer Aktion](./media/use/take-action.png)

7. Wählen Sie **Aktion ausführen** aus, um die Aktion auszuführen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Versionsverwaltung für die Azure Blockchain Workbench-Anwendung](version-app.md)
