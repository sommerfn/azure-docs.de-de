---
title: include file
description: include file
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/21/2019
ms.openlocfilehash: 2ec45b67367198c14fc9d03cdb659a51aed8a504
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841481"
---
1. Melden Sie sich mit den Anmeldeinformationen, die Sie für das Azure-Abonnement verwenden, beim [Azure-Portal](https://portal.azure.com/) an. 

   ![Azure-Portal](./media/aml-create-in-portal/portal-dashboard-05-2019.png)

1. Wählen Sie links oben im Portal **Ressource erstellen** aus.

   ![Erstellen einer Ressource im Azure-Portal](./media/aml-create-in-portal/portal-create-a-resource-07-2019.png)

1. Wählen Sie mithilfe der Suchleiste **Machine Learning-Dienstarbeitsbereich** aus.

   ![Suchen nach einem Arbeitsbereich](./media/aml-create-in-portal/allservices-search.png)

1. Wählen Sie im Bereich **ML-Dienstarbeitsbereich** die Option **Erstellen** aus, um zu beginnen.

    ![Schaltfläche „Erstellen“](./media/aml-create-in-portal/portal-create-button.png)

1. Konfigurieren Sie Ihren Arbeitsbereich im Bereich **ML-Dienstarbeitsbereich**.

    ![Arbeitsbereich erstellen](./media/aml-create-in-portal/workspace-create-main-tab.png)

   Feld|BESCHREIBUNG
   ---|---
   Arbeitsbereichname |Geben Sie einen eindeutigen Namen ein, der Ihren Arbeitsbereich identifiziert. In diesem Beispiel verwenden wir **docs-ws**. Namen müssen in der Ressourcengruppe eindeutig sein. Verwenden Sie einen Namen, der leicht zu merken ist und sich von den von anderen Benutzern erstellten Arbeitsbereichen unterscheidet.  
   Subscription |Wählen Sie das gewünschte Azure-Abonnement aus.
   Resource group | Verwenden Sie eine vorhandene Ressourcengruppe in Ihrem Abonnement, oder geben Sie einen Namen ein, um eine neue Ressourcengruppe zu erstellen. Eine Ressourcengruppe enthält verwandte Ressourcen für eine Azure-Lösung. In diesem Beispiel verwenden wir **docs-aml**. 
   Location | Wählen Sie den Standort aus, der Ihren Benutzern und den Datenressourcen am nächsten ist. Dort wird der Arbeitsbereich erstellt.

1. Überprüfen Sie Ihre Arbeitsbereichskonfiguration, und wählen Sie **Erstellen** aus. Es kann einige Augenblicke dauern, bis der Arbeitsbereich erstellt wurde.

1. Wenn der Vorgang abgeschlossen ist, wird eine Erfolgsmeldung zur Bereitstellung angezeigt. Diese finden Sie auch im Abschnitt „Benachrichtigungen“. Um den neuen Arbeitsbereich anzuzeigen, wählen Sie **Zu Ressource wechseln** aus.

   ![Status der Arbeitsbereichserstellung](./media/aml-create-in-portal/notifications.png)
