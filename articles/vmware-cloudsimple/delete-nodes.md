---
title: Löschen von Knoten für die VMware-Lösung von CloudSimple – Azure
description: Erfahren Sie, wie Sie mit der CloudSimple-Bereitstellung Knoten aus Ihrer VMware-Lösung löschen.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 122e0636f54e066ae86ed2d19cefe5863b026293
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69972832"
---
# <a name="delete-nodes-from-azure-vmware-solution-by-cloudsimple"></a>Löschen von Knoten aus der Azure VMware-Lösung von CloudSimple

CloudSimple-Knoten werden ab der Erstellung abgerechnet.  Die Knoten müssen gelöscht werden, um die Abrechnung zu beenden.  Sie löschen die nicht mehr verwendeten Knoten im Azure-Portal.

## <a name="before-you-begin"></a>Voraussetzungen

Ein Knoten kann nur unter folgenden Bedingungen gelöscht werden:

* Eine mit den Knoten erstellte private Cloud wird gelöscht.  Informationen zum Löschen einer privaten Cloud finden Sie unter [Löschen einer privaten Cloud für die Azure VMware-Lösung von CloudSimple](delete-private-cloud.md).
* Der Knoten wurde durch Verkleinern der privaten Cloud aus der privaten Cloud entfernt.  Informationen zum Verkleinern einer privaten Cloud finden Sie unter [Verkleinern einer privaten Cloud für die Azure VMware-Lösung von CloudSimple](shrink-private-cloud.md).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="delete-cloudsimple-node"></a>Löschen eines CloudSimple-Knotens

1. Wählen Sie **Alle Dienste** aus.

2. Suchen Sie nach **CloudSimple-Knoten**.

   ![Suchen nach CloudSimple-Knoten](media/create-cloudsimple-node-search.png)

3. Wählen Sie **CloudSimple-Knoten** aus.

4. Wählen Sie Knoten, die nicht zu einer privaten Cloud gehören, zum Löschen aus.  Die Spalte **PRIVATE CLOUD NAME** enthält den Namen der privaten Cloud, zu der ein Knoten gehört.  Wenn ein Knoten von keiner privaten Cloud verwendet wird, ist der Wert leer. 

    ![Auswählen von CloudSimple-Knoten](media/select-delete-cloudsimple-node.png)

> [!NOTE]
> Nur Knoten, die nicht Teil der privaten Cloud sind, können gelöscht werden.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [private Clouds](cloudsimple-private-cloud.md).
