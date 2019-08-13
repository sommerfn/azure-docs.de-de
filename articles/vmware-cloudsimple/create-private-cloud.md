---
title: Erstellen einer privaten Cloud für die Azure-VMware-Lösung von CloudSimple
description: Informationen zum Erstellen einer privaten CloudSimple-Cloud zum Erweitern von VMware-Workloads auf die Cloud mit operativer Flexibilität und Kontinuität
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 02a2bd311ea1e89a49eb12ef57a167a08eea5f98
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812251"
---
# <a name="create-a-cloudsimple-private-cloud"></a>Erstellen einer privaten CloudSimple-Cloud

Mithilfe einer privaten Cloud können Sie verschiedene häufige Anforderungen an die Netzwerkinfrastruktur angehen:

* **Wachstum:** Wenn Sie den Punkt erreichen, an dem Ihre Hardware für Ihre bereits bestehende Infrastruktur aktualisiert werden muss, ermöglicht eine private Cloud Erweiterungen ohne weitere Hardwareinvestitionen.

* **Schnelle Erweiterung:** Wenn temporärer oder ungeplanter Kapazitätsbedarf entsteht, können Sie mithilfe einer privaten Cloud ohne Verzögerung die zusätzlichen Kapazitäten bereitstellen.

* **Verbesserter Schutz:** Über eine private Cloud, die aus mindestens drei Knoten besteht, erhalten Sie automatische Redundanz und Schutz durch Hochverfügbarkeit.

* **Langfristige Infrastrukturanforderungen:** Wenn Ihre Rechenzentren ausgelastet sind oder Sie eine Neustrukturierung vornehmen möchten, um Ihre Kosten zu senken, bietet eine private Cloud Ihnen die Möglichkeit, einerseits Rechenzentren außer Betrieb zu nehmen und eine Migration zu einer cloudbasierten Lösung durchzuführen und andererseits die Kompatibilität mit den Vorgängen in Ihrem Unternehmen zu wahren.

Wenn Sie eine private Cloud erstellen, erhalten Sie einen einzelnen vSphere-Cluster und sämtliche VMs, die zur Verwaltung dienen und in diesem Cluster erstellt werden.

## <a name="before-you-begin"></a>Voraussetzungen

Bevor Sie eine private Cloud erstellen können, müssen Knoten bereitgestellt werden.  Weitere Informationen zum Bereitstellen von Knoten finden Sie im Artikel [Kaufen von Knoten für VMware-Lösung von CloudSimple – Azure](create-nodes.md).

Zuordnung eines CIDR-Bereichs für vSphere/vSAN-Subnetze für die private Cloud. Eine private Cloud wird als isolierte VMware-Stapelumgebung (ESXi-Hosts, vCenter, vSAN und NSX) erstellt, die von einem vCenter-Server verwaltet wird. Verwaltungskomponenten werden in dem Netzwerk bereitgestellt, das für vSphere/vSAN-Subnetze-CIDR ausgewählt ist. Der Netzwerk CIDR-Bereich wird während der Bereitstellung in unterschiedliche Subnetze aufgeteilt.  Der Adressraum des vSphere/vSAN-Subnetzadressraums muss eindeutig sein. Er darf sich nicht mit einem Netzwerk überschneiden, das mit der CloudSimple-Umgebung kommuniziert.  Zu den Netzwerken, die mit CloudSimple kommunizieren, gehören unter anderem lokale Netzwerke und virtuelle Azure-Netzwerke.  Weitere Informationen zu vSphere/vSAN-Subnetzen finden Sie unter [Übersicht über VLANs und Subnetze](cloudsimple-vlans-subnets.md).

* Minimales Präfix für vSphere/vSAN-Subnetze-CIDR-Bereich: /24 
* Maximales Präfix für vSphere/vSAN-Subnetze-CIDR-Bereich: /21

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="access-the-cloudsimple-portal"></a>Zugriff auf das CloudSimple-Portal

Greifen Sie auf das [CloudSimple-Portal](access-cloudsimple-portal.md) zu.

## <a name="create-a-new-private-cloud"></a>Erstellen einer neuen privaten Cloud

1. Klicken Sie auf der Seite **Ressourcen** auf **New Private Cloud** (Neue private Cloud).

    ![Erstellen einer privaten Cloud: erster Schritt](media/create-pc-button.png)

2. Wählen Sie den Ort aus, auf dem die Ressourcen der privaten Cloud gehostet werden sollen.

3. Wählen Sie den Knotentyp aus, den Sie für die private Cloud bereitgestellt haben (CS28 oder CS36). Die zweite Option umfasst die maximale Compute- und Arbeitsspeicherkapazität.

4. Wählen Sie die Anzahl der Knoten für die private Cloud aus. Sie können nicht mehr Knoten auswählen als Sie [bereitgestellt](create-nodes.md) haben.

    ![Erstellen einer privaten Cloud: Grundeinstellungen](media/create-private-cloud-basic-info.png)

5. Klicken Sie auf **Weiter: Erweiterte Optionen**.

6. Geben Sie den CIDR-Bereich für vSphere/vSAN-Subnetze ein. Stellen Sie sicher, dass sich der CIDR-Bereich nicht mit einem Ihrer lokalen Subnetze, mit anderen Azure-Subnetzen (virtuelle Netzwerke) oder dem Gatewaysubnetz überlappt.  Verwenden Sie keinen CIDR-Bereich, der auf virtuellen Azure-Netzwerken definiert ist.
    
    **Mögliche CIDR-Bereiche:** /24, /23, /22 oder /21. Der CIDR-Bereich /24 unterstützt bis zu neun Knoten, der Bereich /23 bis zu 41 Knoten und die Bereiche /22 und /21 bis zu 64 Knoten (die maximale Anzahl an Knoten in einer privaten Cloud).

    > [!CAUTION]
    > IP-Adressen im CIDR-Bereich vSphere/vSAN sind für die Verwendung durch die private Cloudinfrastruktur reserviert.  Verwenden Sie keine IP-Adresse dieses Bereichs auf einem virtuellen Computer.

7. Klicken Sie auf **Weiter: Überprüfen und erstellen**.

8. Überprüfen Sie die Einstellungen. Wenn Sie irgendeine der Einstellungen ändern müssen, klicken Sie auf **Zurück**.

9. Klicken Sie auf **Create**.

Die private Cloud wird bereitgestellt, sobald Sie auf „Erstellen“ klicken.  Den Fortschritt können Sie über auf der Seite [Aufgaben](https://docs.azure.cloudsimple.com/activity/#tasks) im CloudSimple-Portal überwachen.  Die Bereitstellung kann zwischen 30 Minuten und zwei Stunden dauern.  Sobald diese abgeschlossen ist, erhalten Sie eine E-Mail.

## <a name="next-steps"></a>Nächste Schritte

* [Expand private cloud (Erweitern einer privaten Cloud)](expand-private-cloud.md)