---
title: include file
description: include file
services: machine-learning
author: peterclu
ms.service: machine-learning
ms.author: peterlu
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 10/09/2019
ms.openlocfilehash: 3f969fb346ce341e694458dcc38bf193f012226b
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792653"
---
Ein Pipeline wird auf einem Computeziel ausgeführt. Dabei handelt es sich um eine an Ihren Arbeitsbereich angefügte Computeressource. Wenn Sie ein Computeziel erstellen, können Sie es für künftige Ausführungen wiederverwenden.

1. Wählen Sie im oberen Bereich der Canvas die Option **Ausführen** aus, um die Pipeline auszuführen.

1. Wenn der Bereich **Einstellungen** angezeigt wird, wählen Sie **Computeziel auswählen** aus.

    Falls Sie bereits über ein verfügbares Computeziel verfügen, können Sie es auswählen, um diese Pipeline auszuführen.

    > [!NOTE]
    > Auf der grafischen Benutzeroberfläche können nur Experimente für Machine Learning Compute-Ziele ausgeführt werden. Andere Computeziele werden nicht angezeigt.

1. Geben Sie einen Namen für die Computeressource an.

1. Wählen Sie **Speichern** aus.

    ![Computeziele einrichten](./media/aml-ui-create-training-compute/set-compute.png)

1. Klicken Sie auf **Run** (Ausführen).

1. Wählen Sie im Dialogfeld **Pipelineausführung einrichten** für **Experiment** die Option **+ Neues Experiment** aus.

    > [!NOTE]
    > Experimente fassen ähnliche Pipelineausführungen in einer Gruppe zusammen. Wenn Sie eine Pipeline mehrmals ausführen, können Sie für weitere Ausführungen das gleiche Experiment auswählen.

    * Geben Sie unter **Name des Experiments** einen aussagekräftigen Experimentnamen ein.

    * Wählen Sie **Ausführen** aus.
    
    Ausführungsstatus und Details können rechts oben auf der Canvas angezeigt werden.

    > [!NOTE]
    > Die Erstellung einer Computeressource dauert etwa fünf Minuten. Nach der Erstellung der Ressource können Sie sie wiederverwenden und diese Wartezeit in künftigen Ausführungen vermeiden.
    >
    > Eine im Leerlauf befindliche Computeressource wird automatisch auf 0 Knoten skaliert, um Kosten zu sparen.  Wenn Sie sie nach einer Verzögerung erneut verwenden, müssen Sie unter Umständen etwa fünf Minuten warten, bis sie wieder hochskaliert wurde.
