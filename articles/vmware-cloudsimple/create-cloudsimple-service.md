---
title: 'Azure-VMware-Lösung von CloudSimple: Erstellen eines CloudSimple-Diensts'
description: Beschreibt, wie der CloudSimple-Dienst im Azure-Portal erstellt wird
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0f589adf5cbcd04b9642dfe1362fd13d5be1f9aa
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69640632"
---
# <a name="create-the-azure-vmware-solution-by-cloudsimple-service"></a>Erstellen der Azure-VMware-Lösung des CloudSimple-Diensts

Als ersten Schritt für die Azure-VMware-Lösung von CloudSimple erstellen Sie den Dienst „Azure-VMware-Lösung von CloudSimple“ im Azure-Portal.

> [!IMPORTANT]
> Vor dem Erstellen des CloudSimple-Diensts müssen Sie den Microsoft.VMwareCloudSimple-Ressourcenanbieter in Ihrem Azure-Abonnement registrieren. Führen Sie dazu die Schritte in [Aktivieren des Microsoft.VMwareCloudSimple-Ressourcenanbieters in Ihrem Azure-Abonnement](enable-cloudsimple-service.md) aus.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an.

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
8. Geben Sie das CIDR für das Dienstgateway ein. Geben Sie ein /28-Subnetz an, das mit keinem Ihrer lokalen Subnetze, Azure-Subnetze oder geplanten CloudSimple-Subnetze überlappt. Sie können das CIDR nicht mehr ändern, nachdem der Dienst erstellt wurde.

    ![Erstellen des CloudSimple-Diensts](media/create-cloudsimple-service.png)
9. Klicken Sie auf **OK**.

Der Dienst wird erstellt und zur Liste der Dienste hinzugefügt.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Erstellen einer privaten Cloud](create-private-cloud.md).
* Erfahren Sie mehr über das [Konfigurieren einer privaten Cloudumgebung](quickstart-create-private-cloud.md).
