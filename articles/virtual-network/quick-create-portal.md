---
title: 'Erstellen eines virtuellen Netzwerks: Schnellstart – Azure-Portal'
titlesuffix: Azure Virtual Network
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal ein virtuelles Netzwerk erstellen. In einem virtuellen Netzwerk können Azure-Ressourcen wie virtuelle Computer sicher miteinander und mit dem Internet kommunizieren.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can securely communicate with each other and with the internet.
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 07/08/2019
ms.author: kumud
ms.openlocfilehash: d8e95f9c345a943eb458800b852640e3f1fde907
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488490"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Schnellstart: Erstellen eines virtuellen Netzwerks im Azure-Portal

Ein virtuelles Netzwerk ist der grundlegende Baustein für Ihr privates Netzwerk in Azure. Es ermöglicht Azure-Ressourcen, z. B. virtuellen Computern (Virtual Machines, VMs), die sichere Kommunikation miteinander und mit dem Internet. In dieser Schnellstartanleitung erfahren Sie, wie Sie im Azure-Portal ein virtuelles Netzwerk erstellen. Dann können Sie zwei VMs im virtuellen Netzwerk bereitstellen, eine sichere Kommunikation zwischen den beiden VMs ermöglichen und über das Internet eine Verbindung mit den VMs herstellen.


Wenn Sie kein Azure-Abonnement besitzen, können Sie jetzt ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Wählen Sie im Menü des Azure-Portals die Option **Ressource erstellen** aus.

2. Wählen Sie im Azure Marketplace die Option **Netzwerk** > **Virtuelles Netzwerk** aus.

3. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *myVirtualNetwork* ein. |
    | Adressraum | Geben Sie *10.1.0.0/16* ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie **Neue erstellen** aus, geben Sie *myResourceGroup* ein, und wählen Sie **OK** aus. |
    | Location | Wählen Sie **USA, Osten** aus.|
    | Subnetz – Name | Geben Sie *myVirtualSubnet* ein. |
    | Subnetzadressbereich | Geben Sie *10.1.0.0/24* ein. |

4. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie zwei virtuelle Computer im virtuellen Netzwerk:

### <a name="create-the-first-vm"></a>Erstellen des ersten virtuellen Computers

1. Wählen Sie im Menü des Azure-Portals die Option **Ressource erstellen** aus.

2. Wählen Sie im Azure Marketplace die Option **Compute** > **Windows Server 2019 Datacenter** aus.

3. Geben Sie in **Virtuellen Computer erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt. |
    | **INSTANZDETAILS** |  |
    | Name des virtuellen Computers | Geben Sie *myVm1* ein. |
    | Region | Wählen Sie **USA, Osten** aus. |
    | Verfügbarkeitsoptionen | Übernehmen Sie den Standardwert **Keine Infrastrukturredundanz erforderlich**. |
    | Image | Übernehmen Sie die Standardeinstellung **Windows Server 2019 Datacenter**. |
    | Size | Übernehmen Sie den Standardwert **Standard DS1 v2**. |
    | **ADMINISTRATORKONTO** |  |
    | Username | Geben Sie einen Benutzernamen Ihrer Wahl ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **REGELN FÜR EINGEHENDE PORTS** |  |
    | Öffentliche Eingangsports | Übernehmen Sie den Standardwert **Keine**. |
    | **SPAREN SIE GELD** |  |
    | Windows-Lizenz bereits vorhanden? | Übernehmen Sie den Standardwert **Nein**. |

4. Wählen Sie **Weiter: Datenträger** aus.

5. Übernehmen Sie in **Virtuellen Computer erstellen – Datenträger** die Standardwerte, und wählen Sie **Weiter: Netzwerk**.

6. Wählen Sie in **Virtuellen Computer erstellen – Netzwerk** diese Informationen aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Virtuelles Netzwerk | Übernehmen Sie den Standardwert **myVirtualNetwork**. |
    | Subnet | Übernehmen Sie den Standardwert **myVirtualSubnet (10.1.0.0/24)** . |
    | Öffentliche IP-Adresse | Übernehmen Sie den Standardwert **(neu) myVm-ip**. |
    | Öffentliche Eingangsports | Wählen Sie **Ausgewählte Ports zulassen** aus. |
    | Eingangsports auswählen | Wählen Sie **HTTP** und **RDP** aus.

7. Wählen Sie **Weiter: Verwaltung** aus.

8. Wählen Sie in **Virtuellen Computer erstellen – Verwaltung** für **Diagnosespeicherkonto** **Neu erstellen** aus.

9. Geben Sie in **Speicherkonto erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *myvmstorageaccount* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen.|
    | Kontoart | Übernehmen Sie den Standardwert **Storage (universell, Version 1)** . |
    | Leistung | Übernehmen Sie den Standardwert **Standard**. |
    | Replikation | Übernehmen Sie den Standardwert **Lokal redundanter Speicher (LRS)** . |

10. Klicken Sie auf **OK**.

11. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

12. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

### <a name="create-the-second-vm"></a>Erstellen des zweiten virtuellen Computers

1. Führen Sie die obigen Schritte 1 bis 9 aus.

    > [!NOTE]
    > Geben Sie in Schritt 2 für **Name des virtuellen Computers** *myVm2* ein.
    >
    > Achten Sie in Schritt 7 darauf, für **Diagnosespeicherkonto** **myvmstorageaccount** auszuwählen.

2. Klicken Sie auf **Überprüfen + erstellen**. Sie gelangen auf die Seite **Bewerten + erstellen**, und Azure überprüft Ihre Konfiguration.

3. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Nachdem Sie *myVm1* erstellt haben, stellen Sie eine Verbindung mit dem Internet her.

1. Geben Sie in der Suchleiste des Portals *myVm1* ein.

2. Wählen Sie die Schaltfläche **Verbinden** aus.

    ![Herstellen einer Verbindung mit einem virtuellen Computer](./media/quick-create-portal/connect-to-virtual-machine.png)

    Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

3. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

4. Öffnen Sie die heruntergeladene *RDP*-Datei.

    1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    2. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

        > [!NOTE]
        > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

5. Klicken Sie auf **OK**.

6. Während des Anmeldevorgangs wird vielleicht eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

7. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.

## <a name="communicate-between-vms"></a>Kommunikation zwischen VMs

1. Öffnen Sie auf dem Remote Desktop von *myVm1* PowerShell.

2. Geben Sie `ping myVm2` ein.

    Sie erhalten eine Meldung wie die folgende:

    ```powershell
    Pinging myVm2.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.clouda
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.1.0.5:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    Bei `ping` tritt ein Fehler auf, weil `ping` das Internet Control Message-Protokoll (ICMP) verwendet. Standardmäßig kann ICMP nicht durch die Windows-Firewall ausgeführt werden.

3. Damit *myVm2* in einem späteren Schritt ein Pingsignal an *myVm1* senden kann, geben Sie diesen Befehl ein:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Dieser Befehl lässt eingehenden ICMP-Datenverkehr durch die Windows-Firewall zu:

4. Schließen Sie die Remotedesktopverbindung mit *myVm1*.

5. Führen Sie die Schritte unter [Herstellen einer Verbindung mit einem virtuellen Computer über das Internet](#connect-to-a-vm-from-the-internet) erneut aus, stellen Sie jedoch die Verbindung mit *myVm2* her.

6. Geben Sie an einer Befehlszeile `ping myvm1` ein.

    Es wird etwa diese Meldung angezeigt:

    ```powershell
    Pinging myVm1.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.cloudapp.net [10.1.0.4] with 32 bytes of data:
    Reply from 10.1.0.4: bytes=32 time=1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.1.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 1ms, Average = 0ms
    ```

    Sie erhalten Antworten von *myVm1*, weil Sie in Schritt 3 ICMP-Datenverkehr durch die Windows-Firewall auf dem virtuellen Computer *myVm1* zugelassen haben.

7. Schließen Sie die Remotedesktopverbindung mit *myVm2*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie Ihre Arbeit mit dem virtuellen Netzwerk und den virtuellen Computern abgeschlossen haben, löschen Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen:

1. Geben Sie oben im Portal im Feld **Suchen** die Zeichenfolge *myResourceGroup* ein, und wählen Sie in den Suchergebnissen den Eintrag **myResourceGroup** aus.

2. Wählen Sie die Option **Ressourcengruppe löschen**.

3. Geben Sie *myResourceGroup* für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie ein virtuelles Standardnetzwerk und zwei virtuelle Computer erstellt. Sie haben über das Internet eine Verbindung mit einem der virtuellen Computer hergestellt und sicher zwischen den beiden virtuellen Computern kommuniziert. Weitere Informationen zu den Einstellungen des virtuellen Netzwerks finden Sie unter [Create, change, or delete a virtual network](manage-virtual-network.md) (Erstellen, Ändern oder Löschen eines virtuellen Netzwerks).

Azure lässt standardmäßig eine uneingeschränkte sichere Kommunikation zwischen virtuellen Computern zu. Im Gegensatz dazu sind nur eingehende Remotedesktopverbindungen mit Windows-VMs über das Internet möglich. Weitere Informationen zum Konfigurieren der verschiedenen Typen der VM-Netzwerkkommunikation finden Sie im Tutorial [Filtern von Netzwerkdatenverkehr mithilfe einer Netzwerksicherheitsgruppe über das Azure-Portal](tutorial-filter-network-traffic.md).
