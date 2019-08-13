---
title: 'Erstellen einer Azure-VMware-Lösung von CloudSimple: – Dienst'
description: Beschreibt, wie der CloudSimple-Dienst im Azure-Portal erstellt wird
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6986e0a7e6eee6dbbd43c72a415b01df7da7da51
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812446"
---
# <a name="create-azure-vmware-solution-by-cloudsimple---service"></a>Erstellen einer Azure-VMware-Lösung von CloudSimple – Dienst

Als ersten Schritt für die Azure-VMware-Lösung von CloudSimple erstellen Sie den Dienst „Azure-VMware-Lösung von CloudSimple“ im Azure-Portal.

## <a name="before-you-begin"></a>Voraussetzungen

Ordnen Sie einen /28-CIDR-Block für das Gatewaysubnetz zu.  Ein Gatewaysubnetz ist für jeden CloudSimple-Dienst erforderlich und für die Region spezifisch, in der es erstellt wird. Das Gatewaysubnetz wird für Edge-Netzwerkdienste verwendet und erfordert einen CIDR-Block vom Typ „/28“. Der Adressraum des Gatewaysubnetzes muss eindeutig sein. Er darf sich nicht mit einem Netzwerk überschneiden, das mit der CloudSimple-Umgebung kommuniziert.  Zu den Netzwerken, die mit CloudSimple kommunizieren, gehören unter anderem lokale Netzwerke und virtuelle Azure-Netzwerke.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-the-service"></a>Erstellen des Diensts

1. Wählen Sie **Alle Dienste** aus.

2. Suchen Sie nach **CloudSimple Services**.

    ![Suchen nach dem CloudSimple-Dienst](media/create-cloudsimple-service-search.png)

3. Wählen Sie **CloudSimple Service** aus.

4. Klicken Sie auf **Hinzufügen**, um einen neuen Dienst zu erstellen.

    ![Hinzufügen des CloudSimple-Diensts](media/create-cloudsimple-service-add.png)

5. Wählen Sie das Abonnement aus, in dem Sie den CloudSimple-Dienst erstellen möchten.

6. Wählen Sie die Ressourcengruppe für den Dienst aus. Um eine neue Ressourcengruppe hinzuzufügen, klicken Sie auf **Neue erstellen**.

7. Geben Sie einen Namen ein, um den Dienst zu benennen.

8. Geben Sie das CIDR für das Dienstgateway ein. Geben Sie ein /28-Subnetz an, das sich mit keinem Ihrer vorhandenen Subnetze überschneidet.  Dazu gehören lokale Subnetze, Azure-Subnetze oder beliebige geplante CloudSimple-Subnetze. Sie können das CIDR nicht mehr ändern, nachdem der Dienst erstellt wurde.

    ![Erstellen des CloudSimple-Diensts](media/create-cloudsimple-service.png)

9. Klicken Sie auf **OK**.

Der Dienst wird erstellt und zur Liste der Dienste hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Erstellen einer privaten Cloud](https://docs.azure.cloudsimple.com/create-private-cloud/).
* Erfahren Sie mehr über [Konfigurieren einer Private Cloud-Umgebung](quickstart-create-private-cloud.md).
