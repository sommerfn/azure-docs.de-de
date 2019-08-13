---
title: Azure-VMware-Lösung von CloudSimple – Schnellstart – Erstellen des Diensts
description: Erfahren Sie, wie der CloudSimple-Dienst erstellt wird und wie Knoten bereitgestellt und reserviert werden.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 13b07b3b50bdb03373275ca9594baa6357e9f66f
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812294"
---
# <a name="quickstart---create-service"></a>Schnellstart: Erstellen des Diensts

Erstellen Sie zunächst die Azure-VMware-Lösung von CloudSimple im Azure-Portal ein.

## <a name="vmware-solution-by-cloudsimple---service-overview"></a>VMware-Lösung von CloudSimple: Übersicht über den Dienst

Der CloudSimple-Dienst ermöglicht es Ihnen, die Azure-VMware-Lösung von CloudSimple zu nutzen.  Nach Erstellung des Diensts können Sie Knoten bereitstellen, Knoten reservieren und private Clouds erstellen.  Sie fügen den CloudSimple-Dienst in jeder Azure-Region hinzu, in der der CloudSimple-Dienst verfügbar ist.  Der Dienst definiert das Umkreisnetzwerk von Azure-VMware-Lösung von CloudSimple.  Dieses Umkreisnetzwerk wird für Dienste verwendet, zu denen VPN, ExpressRoute und Internetkonnektivität mit Ihren privaten Clouds gehören.

Um den CloudSimple-Dienst hinzuzufügen, müssen Sie ein Gatewaysubnetz erstellen. Das Gatewaysubnetz wird verwendet, wenn das Edge-Netzwerk erstellt wird, und das Gatewaysubnetz erfordert einen /28-CIDR-Block. Der Gatewaysubnetz-Adressraum muss eindeutig sein. Er darf nicht mit einem Ihrer lokalen Netzwerkadressräume oder mit einem Adressraum des virtuellen Azure-Netzwerk überlappen.

## <a name="before-you-begin"></a>Voraussetzungen

Ordnen Sie einen /28-CIDR-Block für das Gatewaysubnetz zu.  Ein Gatewaysubnetz ist für jeden CloudSimple-Dienst erforderlich und für die Region spezifisch, in der es erstellt wird. Das Gatewaysubnetz wird für Edge-Netzwerkdienste verwendet und erfordert einen CIDR-Block vom Typ „/28“. Der Adressraum des Gatewaysubnetzes muss eindeutig sein. Er darf sich nicht mit einem Netzwerk überschneiden, das mit der CloudSimple-Umgebung kommuniziert.  Zu den Netzwerken, die mit CloudSimple kommunizieren, gehören unter anderem lokale Netzwerke und virtuelle Azure-Netzwerke. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-the-service"></a>Erstellen des Diensts

1. Wählen Sie **Alle Dienste** aus.
2. Suchen Sie nach **CloudSimple Service**.

    ![Suchen nach dem CloudSimple-Dienst](media/create-cloudsimple-service-search.png)

3. Wählen Sie **CloudSimple Service** aus.
4. Klicken Sie auf **Hinzufügen**, um einen neuen Dienst zu erstellen.

    ![Hinzufügen des CloudSimple-Diensts](media/create-cloudsimple-service-add.png)

5. Wählen Sie das Abonnement aus, in dem Sie den CloudSimple-Dienst erstellen möchten.
6. Wählen Sie die Ressourcengruppe für den Dienst aus. Um eine neue Ressourcengruppe hinzuzufügen, klicken Sie auf **Neue erstellen**.
7. Geben Sie einen Namen ein, um den Dienst zu benennen.
8. Geben Sie das CIDR für das Dienstgateway ein. Geben Sie ein /28-Subnetz an, das mit keinem Ihrer lokalen Subnetze, Azure-Subnetze oder geplanten CloudSimple-Subnetze überlappt. Sie können das CIDR nicht mehr ändern, nachdem der Dienst erstellt wurde.

    ![Erstellen des CloudSimple-Diensts](media/create-cloudsimple-service.png)

9. Klicken Sie auf **OK**.

Der Dienst wird erstellt und zur Liste der Dienste hinzugefügt.

## <a name="provision-nodes"></a>Bereitstellen von Knoten

Um für die Umgebung einer privaten CloudSimple-Cloud Kapazität mit nutzungsbasierter Bezahlung einzurichten, müssen Sie zuerst Knoten im Azure-Portal bereitstellen.

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

* [Erstellen einer privaten Cloud und Konfigurieren der Umgebung](quickstart-create-private-cloud.md)
* Weitere Informationen über den [CloudSimple-Dienst](https://docs.azure.cloudsimple.com/cloudsimple-service)
