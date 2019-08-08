---
title: Bereitstellen von Knoten für die VMware-Lösung von CloudSimple – Azure
description: Erfahren Sie, wie Sie mit CloudSimple-Bereitstellung Knoten zu Ihrer VMWare-Lösung hinzufügen können.
author: dikamath
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9eb02f04b5873e5906782a27ce833a724ceecfe3
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812380"
---
# <a name="provision-nodes-for-vmware-solution-by-cloudsimple---azure"></a>Bereitstellen von Knoten für die VMware-Lösung von CloudSimple – Azure

Stellen Sie Knoten im Azure-Portal bereit. Danach können Sie Kapazität mit nutzungsbasierter Bezahlung für die Umgebung Ihrer privaten CloudSimple-Cloud einrichten.

## <a name="add-a-provisioned-node-to-your-cloudsimple-private-cloud"></a>Hinzufügen eines bereitgestellten Knotens zu Ihrer privaten CloudSimple-Cloud

1. Wählen Sie **Alle Dienste** aus.
2. Suchen Sie nach **CloudSimple-Knoten**.

   ![Suchen nach CloudSimple-Knoten](media/create-cloudsimple-node-search.png)

3. Wählen Sie **CloudSimple-Knoten** aus.
4. Klicken Sie auf **Hinzufügen**, um Knoten zu erstellen.

    ![Hinzufügen von CloudSimple-Knoten](media/create-cloudsimple-node-add.png)

5. Wählen Sie das Abonnement aus, in dem Sie CloudSimple-Knoten bereitstellen möchten.
6. Wählen Sie die Ressourcengruppe für die Knoten aus. Um eine neue Ressourcengruppe hinzuzufügen, klicken Sie auf **Neue erstellen**.
7. Geben Sie das Präfix ein, um die Knoten zu kennzeichnen.
8. Wählen Sie den Standort für die Knotenressourcen aus.
9. Wählen Sie den dedizierten Standort (Dedicated location) aus, in dem die Knotenressourcen gehostet werden sollen.
10. Wählen Sie den Knotentyp aus. Sie können zwischen der [CS28- und CS36-Option](cloudsimple-node.md) wählen. Die zweite Option umfasst die maximale Compute- und Arbeitsspeicherkapazität.
11. Wählen Sie die Anzahl der bereitzustellenden Knoten aus.
12. Klicken Sie auf **Überprüfen + erstellen**.
13. Überprüfen Sie die Einstellungen. Wenn Sie irgendwelche Einstellungen ändern möchten, klicken Sie auf **Zurück**.
14. Klicken Sie auf **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer privaten Cloud](https://docs.azure.cloudsimple.com/create-private-cloud/)
