---
title: Herstellen einer Verbindung mit Azure-VMs nach einem Failover aus der lokalen Umgebung in Azure mit Azure Site Recovery
description: Beschreibt das Herstellen einer Verbindung mit Azure-VMs nach einem Failover aus der lokalen Umgebung in Azure mit Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/13/2019
ms.author: mayg
ms.openlocfilehash: f535a681ac3508aafc2823bcc9b9ae7f22cc2d8e
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333046"
---
# <a name="connect-to-azure-vms-after-failover-from-on-premises"></a>Herstellen einer Verbindung mit Azure-VMs nach einem Failover aus der lokalen Umgebung 


In diesem Artikel wird beschrieben, wie Sie die Konnektivität so einrichten, dass Sie nach einem Failover erfolgreich eine Verbindung mit Azure-VMs herstellen können.

Wenn Sie Notfallwiederherstellung von lokalen virtuellen Computern (VMs) und physischen Servern in Azure einrichten, beginnt [Azure Site Recovery](site-recovery-overview.md) damit, Computer in Azure zu replizieren. Wenn es dann zu Ausfällen kommt, können Sie ein Failover von Ihrem lokalen Standort zu Azure durchführen. Wenn ein Failover auftritt, erstellt Site Recovery Azure-VMs mithilfe der replizierten lokalen Daten. Im Rahmen der Planung der Notfallwiederherstellung müssen Sie herausfinden, wie Sie nach einem Failover eine Verbindung mit Apps herstellen, die auf diesen Azure-VMs ausgeführt werden.

In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> * Vorbereiten von lokalen Computern vor dem Failover.
> * Vorbereiten virtueller Azure-Computer nach einem Failover. 
> * Beibehalten von IP-Adressen nach einem Failover.
> * Zuweisen neuer IP-Adressen zu Azure-VMs nach einem Failover.

## <a name="prepare-on-premises-machines"></a>Vorbereiten lokaler Computer

Bereiten Sie Ihre lokalen Computer vor dem Failover vor, um die Konnektivität mit virtuellen Azure-Computern sicherzustellen.

### <a name="prepare-windows-machines"></a>Vorbereiten von Windows-Computern

Führen Sie auf lokalen Windows-Computern die folgenden Schritte aus:

1. Konfigurieren Sie die Windows-Einstellungen. Hierzu gehören das Entfernen aller statischen permanenten Routen oder des WinHTTP-Proxys und das Festlegen der SAN-Richtlinie des Datenträgers auf **OnlineAll**. [Befolgen](../virtual-machines/windows/prepare-for-upload-vhd-image.md#set-windows-configurations-for-azure) Sie diese Anweisungen.

2. Stellen Sie sicher, dass [diese Dienste](../virtual-machines/windows/prepare-for-upload-vhd-image.md#check-the-windows-services) ausgeführt werden.

3. Aktivieren Sie den Remotedesktop (RDP), um Remoteverbindungen mit dem lokalen Computer zuzulassen. [Erfahren Sie](../virtual-machines/windows/prepare-for-upload-vhd-image.md#update-remote-desktop-registry-settings), wie RDP mit PowerShell aktiviert werden kann.

4. Wenn Sie nach einem Failover über das Internet auf eine Azure-VM zugreifen möchten, lassen Sie in der Windows-Firewall auf dem lokalen Computer TCP und UDP im öffentlichen Profil zu, und legen Sie RDP als zulässige App für alle Profile fest.

5. Wenn Sie nach einem Failover auf eine Azure-VM über ein Site-to-Site-VPN zugreifen möchten, lassen Sie in der Windows-Firewall auf dem lokalen Computer RDP für das Domänenprofil und private Profile zu. [Erfahren Sie](../virtual-machines/windows/prepare-for-upload-vhd-image.md#configure-windows-firewall-rules), wie RDP-Datenverkehr zugelassen wird.
6. Stellen Sie sicher, dass auf der lokalen VM keine ausstehenden Windows-Updates vorhanden sind, wenn Sie ein Failover auslösen. Sollte dies nämlich der Fall sein, können Updates nach dem Failover auf dem virtuellen Azure-Computer installiert werden, und Sie können sich erst dann bei der VM anmelden, wenn die Updates abgeschlossen sind.

### <a name="prepare-linux-machines"></a>Vorbereiten von Linux-Computern:

Führen Sie auf lokalen Linux-Computern die folgenden Schritte aus:

1. Stellen Sie sicher, dass der Secure Shell-Dienst so festgelegt ist, dass er beim Systemstart automatisch gestartet wird.
2. Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung zulassen.


## <a name="configure-azure-vms-after-failover"></a>Vorbereiten virtueller Azure-Computer nach dem Failover

Führen Sie nach dem Failover auf den Azure-VMs, die erstellt werden, die folgenden Schritte aus.

1. Um aus dem Internet auf die VM zugreifen zu können, müssen Sie ihr eine öffentliche IP-Adresse zuweisen. Sie können nicht die gleiche öffentliche IP-Adresse für den virtuellen Azure-Computer verwenden, die Sie für Ihren lokalen Computer verwendet haben. [Weitere Informationen](../virtual-network/virtual-network-public-ip-address.md)
2. Überprüfen Sie, ob die Regeln der Netzwerksicherheitsgruppen (NSG) auf dem virtuellen Computer eingehende Verbindungen am RDP- oder SSH-Port zulassen.
3. Überprüfen Sie die [Startdiagnose](../virtual-machines/troubleshooting/boot-diagnostics.md#enable-boot-diagnostics-on-existing-virtual-machine), um die VM anzuzeigen.


> [!NOTE]
> Der Azure Bastion-Dienst bietet privaten RDP- und SSH-Zugriff auf Azure-VMs. [Weitere Informationen](../bastion/bastion-overview.md) zu diesem Dienst.

## <a name="set-a-public-ip-address"></a>Festlegen einer öffentlichen IP-Adresse

Als Alternative zum manuellen Zuweisen einer öffentlichen IP-Adresse zu einem virtuellen Azure-Computer können Sie die Adresse während des Failovers mithilfe eines Skripts oder eines Azure Automation-Runbooks in einem [Wiederherstellungsplan](site-recovery-create-recovery-plans.md) von Site Recovery zuweisen, oder Sie können das Routing auf DNS-Ebene mithilfe von Azure Traffic Manager einrichten. [Weitere Informationen](concepts-public-ip-address-with-site-recovery.md) zum Einrichten einer öffentlichen IP-Adresse.


## <a name="assign-an-internal-address"></a>Zuweisen einer internen Adresse

Wenn Sie die interne IP-Adresse eines virtuellen Azure-Computers nach dem Failover festlegen möchten, stehen Ihnen mehrere Optionen zur Verfügung:

- **Behalten derselben IP-Adresse**: Sie können dieselbe IP-Adresse auf der Azure-VM verwenden, die dem lokalen Computer zugeordnet ist.
- **Verwenden einer anderen IP-Adresse**: Sie können eine andere IP-Adresse für den virtuellen Azure-Computer verwenden.


## <a name="retain-ip-addresses"></a>IP-Adressen beibehalten

Mit Site Recovery können Sie bei einem Failover in Azure die gleichen IP-Adressen beibehalten. Wenn Sie dieselbe IP-Adresse beibehalten, werden potenzielle Netzwerkprobleme nach einem Failover vermieden, aber dies führt zu einer gewissen Komplexität.

- Wenn der virtuelle Azure-Zielcomputer die gleiche IP-Adresse bzw. das gleiche Subnetz wie der lokale Standort verwendet, können Sie keine Verbindung zwischen den virtuellen Azure-Computern mithilfe einer Site-to-Site-VPN-Verbindung oder einer ExpressRoute-Verbindung herstellen. Subnetze müssen eindeutig sein.
- Sie benötigen nach dem Failover eine Verbindung zwischen dem lokalen Standort und Azure, damit Apps auf virtuellen Azure-Computern verfügbar sind. Azure unterstützt keine gestreckten VLANs. Wenn Sie also IP-Adressen beibehalten möchten, müssen Sie den IP-Adressraum in Azure übernehmen, indem Sie ein Failover für das gesamte Subnetz zusätzlich zum lokalen Computer durchführen.
- Bei einem Subnetzfailover wird sichergestellt, dass ein bestimmtes Subnetz nicht gleichzeitig lokal und in Azure verfügbar ist.

Zum Beibehalten von IP-Adressen sind die folgenden Schritte erforderlich:

- Legen Sie in den Compute- und Netzwerkeigenschaften replizierten Elements die Netzwerk- und IP-Adressierung für den virtuellen Azure-Zielcomputer fest, um die lokale Einstellung widerzuspiegeln.
- Subnetze müssen im Rahmen des Notfallwiederherstellungsprozesses verwaltet werden. Sie benötigen ein Azure-VNET, das mit dem lokalen Netzwerk übereinstimmt, und nach dem Failover müssen die Netzwerkrouten so geändert werden, dass sie die Verschiebung des Subnetzes in Azure und die neuen IP-Adressorte berücksichtigen.  

### <a name="failover-example"></a>Beispiel für ein Failover

Schauen wir uns ein Beispiel an.

- Das fiktive Unternehmen Woodgrove Bank hostet seine geschäftlichen Apps lokal und seine mobilen Apps in Azure.
- Es stellt eine Verbindung zwischen der lokalen Umgebung und Azure über ein Site-to-Site-VPN her. 
- Woodgrove verwendet Site Recovery, um die lokalen Computer in Azure zu replizieren.
- Die lokalen Apps von Woodgrove verwenden hartcodierte IP-Adressen, da die gleichen IP-Adressen in Azure beibehalten werden sollen.
- Lokal werden die Computer, die die Apps ausführen, in drei Subnetzen ausgeführt:
    - 192.168.1.0/24
    - 192.168.2.0/24
    - 192.168.3.0/24
- Die Apps, die in Azure ausgeführt werden, befinden sich im Azure-VNET **Azure Network** in zwei Subnetzen:
- 172.16.1.0/24
- 172.16.2.0/24.

Um die Adressen beizubehalten, geht das Unternehmen wie folgt vor.

1. Wenn die Replikation aktiviert wird, gibt Woodgrove an, dass die Computer in **Azure Network** repliziert werden sollen.
2. Woodgrove erstellt **Recovery Network** in Azure. Dieses VNET spiegelt das Subnetz 192.168.1.0/24 im lokalen Netzwerk wider.
3. Woodgrove richtet eine [VNET-zu-VNET-Verbindung](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) zwischen den beiden Netzwerken ein. 

    > [!NOTE]
    > Je nach Anwendungsanforderungen kann eine VNET-zu-VNET-Verbindung vor dem Failover als manueller Schritt/Skriptschritt/Azure Automation-Runbook in einem [Wiederherstellungsplan](site-recovery-create-recovery-plans.md) von Site Recovery oder nach Abschluss des Failovers eingerichtet werden.

4. Vor dem Failover hat Woodgrove für die Computereigenschaften in Site Recovery die IP-Zieladresse auf die Adresse des lokalen Computers festgelegt, wie im nächsten Verfahren beschrieben wird.
5. Nach dem Failover werden die Azure-VMS mit der gleichen IP-Adresse erstellt. Woodgrove stellt per VNET-Peering (mit aktivierter Transitkonnektivität) vom **Azure-Netzwerk** aus eine Verbindung mit dem **Netzwerk für die Wiederherstellung** (VNET) her.
6. Lokal muss Woodgrove Netzwerkänderungen vornehmen, einschließlich der Änderung von Routen, um widerzuspiegeln, dass 192.168.1.0/24 in Azure verschoben wurde.  

**Infrastruktur vor dem Failover**

![Vor dem Subnetzfailover](./media/site-recovery-network-design/network-design7.png)


**Infrastruktur nach einem Failover**

![Nach dem Subnetzfailover](./media/site-recovery-network-design/network-design9.png)


### <a name="set-target-network-settings"></a>Festlegen der Zielnetzwerkeinstellungen

Geben Sie vor dem Failover die Netzwerkeinstellungen und die IP-Adresse für den virtuellen Azure-Zielcomputer an.

1.  Wählen Sie unter „Recovery Services-Tresor > **Replizierte Elemente**“ den lokalen Computer aus.
2. Klicken Sie auf der Seite **Compute und Netzwerk** für den Computer auf **Bearbeiten**, um Netzwerk- und Adaptereinstellungen für den virtuellen Azure-Zielcomputer zu konfigurieren.
3. Wählen Sie unter **Netzwerkeigenschaften** das Zielnetzwerk aus, in dem sich der virtuelle Azure-Computer befindet, wenn er nach dem Failover erstellt wird.
4. Konfigurieren Sie unter **Netzwerkschnittstellen** die Netzwerkadapter im Zielnetzwerk. Standardmäßig zeigt Site Recovery alle erkannten NICs auf dem lokalen Computer an.
    - Unter **Schnittstellentyp des Zielnetzwerks** können Sie jede NIC als **Primär**, **Sekundär** oder **Nicht erstellen** festlegen, wenn Sie diese spezielle NIC nicht im Zielnetzwerk benötigen. Ein Netzwerkadapter muss für das Failover als primär festgelegt werden. Beachten Sie, dass sich das Ändern des Zielnetzwerks auf alle NICs für den virtuellen Azure-Computer auswirkt.
    - Klicken Sie auf den NIC-Namen, um das Subnetz anzugeben, in dem die Azure-VM bereitgestellt wird.
    - Überschreiben Sie **Dynamisch** mit der privaten IP-Adresse, die Sie dem virtuellen Azure-Zielcomputer zuweisen möchten. Wenn keine IP-Adresse angegeben wird, weist Site Recovery der NIC beim Failover die nächste verfügbare IP-Adresse im Subnetz zu.
    - [Erfahren Sie mehr](site-recovery-manage-network-interfaces-on-premises-to-azure.md) über das Verwalten von NICs für das lokale Failover in Azure.


## <a name="get-new-ip-addresses"></a>Abrufen neuer IP-Adressen

In diesem Szenario erhält die Azure-VM nach einem Failover eine neue IP-Adresse. Ein DNS-Update erfolgt zum Aktualisieren von Einträgen für Computer mit Failover, damit sie auf die IP-Adresse der Azure-VM verweisen.



## <a name="next-steps"></a>Nächste Schritte
[Erfahren Sie mehr](site-recovery-active-directory.md) über das Replizieren des lokalen Active Directory und DNS in Azure.


