---
title: Zuordnen von Azure-Abonnements zu Ressourcenpools in Azure VMware Solution by CloudSimple
description: In diesem Artikel wird beschrieben, wie Sie einen Ressourcenpool in Azure VMware Solution by CloudSimple Ihrem Azure-Abonnement zuordnen können.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: efda996e03d46a2f97d19558f7c2930b623a639e
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332172"
---
# <a name="map-resource-pools-from-your-private-cloud-to-your-azure-subscription"></a>Zuordnen von Ressourcenpools aus Ihrer privaten Cloud zu Ihrem Azure-Abonnement

Durch die Zuordnung von Azure-Abonnements können Sie Ressourcenpools aus Ihrer privaten vCenter-Cloud Ihrem Azure-Abonnement zuordnen. Sie können nur das Abonnement zuordnen, in dem Sie den CloudSimple-Dienst erstellt haben.  Wenn Sie eine VMware-VM über das Azure-Portal erstellen, wird diese im zugeordneten Ressourcenpool bereitgestellt.  Im CloudSimple-Portal können Sie das Azure-Abonnement für Ihre privaten Clouds anzeigen und verwalten.

Ein Abonnement kann mehreren vCenter-Ressourcenpools einer privaten Cloud zugeordnet werden.  Sie müssen die Ressourcenpools aller privaten Clouds zuordnen.  Nur zugeordnete Ressourcenpools können für das Erstellen einer VMware-VM über das Azure-Portal verwendet werden.

> [!IMPORTANT]
> Durch das Zuordnen eines Ressourcenpools werden auch untergeordnete Ressourcenpools zugeordnet. Ein übergeordneter Ressourcenpool kann nicht zugeordnet werden, wenn untergeordnete Ressourcenpools bereits zugeordnet wurden.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass ein CloudSimple-Dienst und eine private Cloud in Ihrem Abonnement vorhanden sind.  Weitere Informationen zum Erstellen eines CloudSimple-Diensts finden Sie unter [Schnellstart: Erstellen des Diensts](quickstart-create-cloudsimple-service.md).  Eine Anleitung zur Erstellung einer privaten Cloud finden Sie unter [Schnellstart: Konfigurieren einer Private Cloud-Umgebung](quickstart-create-private-cloud.md).

Sie können den vCenter-Cluster (Stammressourcenpool) Ihrem Abonnement zuordnen.  Eine Anleitung zum Erstellen eines neuen Ressourcenpools finden Sie im Artikel [Erstellen eines Ressourcenpools](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.resmgmt.doc/GUID-0F6C6709-A5DA-4D38-BE08-6CB1002DD13D.html) auf der VMware-Dokumentationswebsite.

## <a name="default-resource-group"></a>Standardressourcengruppe

Wenn Sie eine neue CloudSimple-VM über das Azure-Portal erstellen, können Sie eine Ressourcengruppe auswählen.  Eine VM, die in einer privaten vCenter-Cloud in einem zugeordneten Ressourcenpool erstellt wurde, wird im Azure-Portal angezeigt.  Die ermittelten VMs werden in die Azure-Standardressourcengruppe eingeordnet.  Sie können den Namen der Standardressourcengruppe ändern.

## <a name="map-azure-subscription"></a>Zuordnen von Azure-Abonnements

1. Rufen Sie das [CloudSimple-Portal](access-cloudsimple-portal.md) auf.

2. Öffnen Sie die Seite **Ressourcen**, und wählen Sie die private Cloud aus, die zugeordnet werden soll.

3. Klicken Sie auf **Azure subscriptions mapping** (Zugeordnete Azure-Abonnements).

4. Klicken Sie auf **Bearbeiten**.

5. Sie können verfügbare Ressourcenpools zuordnen, indem Sie diese auf der linken Seite auswählen und auf den Pfeil klicken, der nach rechts zeigt.

6. Sie können Zuordnungen entfernen, indem Sie diese auf der rechten Seite auswählen und auf den Pfeil klicken, der nach links zeigt.

    ![Azure-Abonnements](media/resources-azure-mapping.png)

7. Klicken Sie auf **OK**.

## <a name="change-default-resource-group-name"></a>Ändern des Namens der Standardressourcengruppe

1. Rufen Sie das [CloudSimple-Portal](access-cloudsimple-portal.md) auf.

2. Öffnen Sie die Seite **Ressourcen**, und wählen Sie die private Cloud aus, die zugeordnet werden soll.

3. Klicken Sie auf **Azure subscriptions mapping** (Zugeordnete Azure-Abonnements).

4. Klicken Sie unter dem Namen der Azure-Ressourcengruppe auf **Bearbeiten**.

    ![Ressourcengruppennamen bearbeiten](media/resources-edit-resource-group-name.png)

5. Geben Sie einen neuen Namen für die Ressourcengruppe ein, und klicken Sie auf **Submit** (Übernehmen).

    ![Neuen Namen für Ressourcengruppe eingeben](media/resources-new-resource-group-name.png)

## <a name="next-steps"></a>Nächste Schritte

* [Nutzen von virtuellen VMware-Computern in Azure](quickstart-create-vmware-virtual-machine.md)
* Weitere Informationen zu [CloudSimple-VMs](cloudsimple-virtual-machines.md)