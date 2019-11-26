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
ms.date: 10/04/2019
ms.openlocfilehash: b44d624db419919823ec85bcb599007fb75c92b9
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73929684"
---
1. Melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Abonnement beim [Azure-Portal](https://portal.azure.com/) an. 

1. Wählen Sie im Azure-Portal oben links die Option **+ Ressource erstellen** aus.

      ![Neue Ressource erstellen](media/aml-create-in-portal/create-workspace.gif)

1. Suchen Sie mithilfe der Suchleiste **Machine Learning**.

1. Wählen Sie **Machine Learning** aus.

1. Wählen Sie im Bereich **Machine Learning** die Option **Erstellen** aus, um zu beginnen.

1. Geben Sie die folgenden Informationen an, um den neuen Arbeitsbereich zu konfigurieren:

   Feld|BESCHREIBUNG 
   ---|---
   Arbeitsbereichname |Geben Sie einen eindeutigen Namen ein, der Ihren Arbeitsbereich identifiziert. In diesem Beispiel verwenden wir **docs-ws**. Namen müssen in der Ressourcengruppe eindeutig sein. Verwenden Sie einen Namen, der leicht zu merken ist und sich von den von anderen Benutzern erstellten Arbeitsbereichen unterscheidet. 
   Subscription |Wählen Sie das gewünschte Azure-Abonnement aus.
   Resource group | Verwenden Sie eine vorhandene Ressourcengruppe in Ihrem Abonnement, oder geben Sie einen Namen ein, um eine neue Ressourcengruppe zu erstellen. Eine Ressourcengruppe enthält verwandte Ressourcen für eine Azure-Lösung. In diesem Beispiel verwenden wir **docs-aml**. 
   Location | Wählen Sie den Standort aus, der Ihren Benutzern und den Datenressourcen am nächsten ist, um Ihren Arbeitsbereich zu erstellen.
   Arbeitsbereichs-Edition | Wählen Sie **Enterprise** aus. In diesem Tutorial muss die Enterprise-Edition verwendet werden. Die Enterprise-Edition befindet sich in der Vorschauphase und verursacht derzeit keine zusätzlichen Kosten. 

1. Wählen Sie **Erstellen** aus, nachdem die Konfiguration des Arbeitsbereichs abgeschlossen ist. 

   > [!WARNING] 
   > Die Erstellung des Arbeitsbereichs in der Cloud kann einige Minuten dauern.

   Wenn der Vorgang abgeschlossen ist, wird eine Erfolgsmeldung zur Bereitstellung angezeigt. 
 
 1. Um den neuen Arbeitsbereich anzuzeigen, wählen Sie **Zu Ressource wechseln** aus.

