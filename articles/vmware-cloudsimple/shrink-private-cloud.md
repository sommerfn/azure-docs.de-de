---
title: Shrink Azure VMware Solution von CloudSimple Private Cloud
description: Beschreibt, wie Sie eine CloudSimple Private Cloud verkleinern.
author: sharaths-cs
ms.author: b-shsury
ms.date: 07/01/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 67bb7e0cd8a800c2c2ba87c768739fd573ef6888
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812211"
---
# <a name="shrink-a-cloudsimple-private-cloud"></a>Verkleinern einer CloudSimple Private Cloud

CloudSimple bietet die Flexibilität, eine Private Cloud dynamisch zu verkleinern.  Eine Private Cloud besteht aus einem oder mehreren vSphere-Clustern. Jeder Cluster kann 3 bis 16 Knoten enthalten. Wenn Sie eine Private Cloud verkleinern, entfernen Sie einen Knoten aus dem bestehenden Cluster oder löschen einen ganzen Cluster. 

## <a name="before-you-begin"></a>Voraussetzungen

Folgende Bedingungen müssen für das Schrumpfen einer Private Cloud erfüllt sein.  Management Cluster (erster Cluster), die beim Erstellen einer Private Cloud erstellt wurden, können nicht gelöscht werden.

* Ein vSphere-Cluster muss aus drei Knoten bestehen.  Ein Cluster mit nur drei Knoten kann nicht verkleinert werden.
* Der gesamte Speicherverbrauch sollte die Gesamtkapazität nach dem Schrumpfen des Clusters nicht überschreiten. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="shrink-a-private-cloud"></a>Verkleinern einer privaten Cloud 

1. [Zugriff auf das CloudSimple-Portal](access-cloudsimple-portal.md).

2. Öffnen der **Ressourcen** Seite.

3. Klicken Sie auf die Private Cloud, die Sie verkleinern möchten

4. Klicken Sie auf der Übersichtsseite auf **Verkleinern**.

    ![Verkleinern einer privaten Cloud](media/shrink-private-cloud.png)

5. Wählen Sie den Cluster aus, den Sie verkleinern oder löschen möchten. 

    ![Private Cloud verkleinern - Cluster auswählen](media/shrink-private-cloud-select-cluster.png)

6. Wählen Sie **Einen Knoten entfernen** oder **Gesamten Cluster löschen**. 

7. Verifizieren der Clusterkapazität

8. Klicken Sie auf **Senden**, um die Private Cloud zu verkleinern.

Verkleinern der privaten Cloud wird gestartet.  Sie können den Status der Installation unter Aufgaben verfolgen.  Der Verkleinerungsprozesses kann je nach den Daten, die mit vSAN resynchronisiert werden müssen, einige Stunden dauern.

## <a name="next-steps"></a>Nächste Schritte

* [Nutzen von virtuellen VMware-Computern in Azure](quickstart-create-vmware-virtual-machine.md)
* Weitere Informationen zu [privaten Clouds](cloudsimple-private-cloud.md)