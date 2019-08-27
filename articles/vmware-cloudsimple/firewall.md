---
title: 'Azure VMware-Lösung von CloudSimple: Einrichten von Firewalltabellen und -regeln'
description: Beschreibt, wie Firewalltabellen und -regeln für die private Cloud eingerichtet werden, um den Datenverkehr für Subnetze und VLANs einzuschränken.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c56d469360814cb663b4c5c11689961225eebb2d
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69544037"
---
# <a name="set-up-firewall-tables-and-rules-for-private-clouds"></a>Einrichten von Firewalltabellen und -regeln für private Clouds

Mit Firewalltabellen und den zugehörigen Regeln können Sie Einschränkungen für Datenverkehr festlegen, die auf bestimmte Subnetze und VLANs angewendet werden sollen.

* Ein Subnetz kann einer Firewalltabelle zugeordnet werden.
* Eine Firewalltabelle kann mehreren Subnetzen zugeordnet werden.

## <a name="add-a-new-firewall-table"></a>Hinzufügen einer neuen Firewalltabelle

1. [Greifen Sie auf das CloudSimple Portal zu](monitor-activity.md), und wählen Sie **Network** (Netzwerk) im seitlichen Menü aus.
2. Wählen Sie **Firewall Tables** (Firewalltabellen) aus.
3. Wählen Sie **Create Firewall Tables** (Firewalltabellen erstellen) aus.

    ![Seite „VLAN/Subnetz“](media/firewall-tables-page.png)

4. Geben Sie einen Namen für die Tabelle ein.
5. Eine Standardregel für die Tabelle wird aufgeführt. Klicken Sie auf **Create New Rule** (Neue Regel erstellen), um eine zusätzliche Regel zu erstellen. Details dazu finden Sie im folgenden Verfahren.
6. Klicken Sie zum Speichern der Firewalltabelle auf **Done** (Fertig).

## <a name="firewall-rules"></a>Firewallregeln

Firewallregeln legen fest, wie die Firewall bestimmte Arten von Datenverkehr behandelt. Auf der Registerkarte **Rules** (Regeln) für eine ausgewählte Firewalltabelle werden alle zugehörigen Regeln aufgelistet.

![Firewallregeltabelle](media/firewall-rules-tab.png)

## <a name="create-a-firewall-rule"></a>Erstellen einer Firewallregel

1. Zeigen Sie die Einstellungen zum Erstellen einer Firewallregel auf eine der folgenden Weisen an:
    * Klicken Sie beim Erstellen einer Firewalltabelle auf **Add Rule** (Regel hinzufügen).
    * Wählen Sie auf der Seite **Network (Netzwerk) > Firewall Tables (Firewalltabellen)** eine bestimmte Firewalltabelle aus, und klicken Sie dann auf **Create new firewall rule** (Neue Firewallregel erstellen).
2. Richten Sie die Regel wie folgt ein:
    * **Name**: Geben Sie der Regel einen Namen.
    * **Priorität**. Weisen Sie der Regel eine Priorität zu. Regeln mit niedrigeren Werten werden zuerst ausgeführt.
    * **Traffic Type (Datenverkehrstyp)** . Wählen Sie aus, ob die Regel für die private Cloud-, das Internet-oder VPN-Datenverkehr (zustandslos) bzw. für eine öffentliche IP-Adresse (zustandsbehaftet) gilt.
    * **Protokoll**. Wählen Sie das Protokoll aus, das von der Regel abgedeckt wird (TCP, UDP oder ein beliebiges anderes Protokoll).
    * **Direction (Richtung)** . Wählen Sie aus, ob die Regel für ein- oder ausgehenden Datenverkehr gilt. Sie müssen separate Regeln für eingehenden und ausgehenden Datenverkehr definieren.
    * **Action (Aktion)** . Wählen Sie die Aktion aus, die ausgeführt werden soll, wenn die Regel übereinstimmt (zulassen oder verweigern).
    * **Source** (Quelle): Geben Sie die von der Regel abgedeckten Quellen an (CIDR-Block, intern oder beliebige Quelle).
    * **Source port range (Quellportbereich)** . Geben Sie den Bereich der Ports an, die der Regel unterliegen.
    * **Direction (Richtung)** . Wählen Sie „inbound“ (eingehend) oder „outbound (ausgehend) aus.
    * **Destination (Ziel)** . Geben Sie die von der Regel abgedeckten Ziele an (CIDR-Block, intern oder beliebige Quelle).
    * **Source port range (Quellportbereich)** . Geben Sie den Bereich der Ports an, die der Regel unterliegen.

    ![Firewall table add rule (Firewalltabelle, Regel hinzufügen)](media/firewall-rule-create.png)

3. Klicken Sie auf **Done** (Fertig), um die Regel zu speichern und sie der Liste der Regeln für die Firewalltabelle hinzuzufügen.

## <a name="attach-vlanssubnets"></a>Attach VLANs/Subnets (VLANs/Subnetze anzufügen)

Nachdem Sie eine Firewalltabelle definiert haben, können Sie die Subnetze angeben, die den Regeln in der Tabelle unterliegen.

1. Wählen Sie auf der Seite **Network (Netzwerk)**  > **Firewall Tables (Firewalltabellen)** eine Firewalltabelle aus.
2. Öffnen Sie die Registerkarte **Attached VLANs/Subnet** (Angefügte(s) VLANs/Subnetz).
3. Klicken Sie auf **Attach to a VLAN/Subnet** (An ein VLAN/Subnetz anfügen).
4. Wählen Sie die private Cloud und das VLAN aus. Der zugehörige Subnetzname und der CIDR-Block werden angezeigt.
5. Klicken Sie auf **Submit**.
