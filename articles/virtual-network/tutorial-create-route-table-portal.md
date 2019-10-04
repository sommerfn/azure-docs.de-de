---
title: 'Weiterleiten von Netzwerkdatenverkehr: Tutorial – Azure-Portal'
titlesuffix: Azure Virtual Network
description: In diesem Tutorial erfahren Sie, wie Sie unter Verwendung des Azure-Portals Netzwerkdatenverkehr mit einer Routingtabelle weiterleiten.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/12/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 153c692a8fb0fa538ec49c6eafa11815dd794b5d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2019
ms.locfileid: "64681542"
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Tutorial: Weiterleiten von Netzwerkdatenverkehr mithilfe des Azure-Portals

Azure leitet den Netzwerkdatenverkehr standardmäßig durch alle Subnetze des virtuellen Netzwerks. Sie können eigene Routen erstellen, um das Azure-Standardrouting außer Kraft zu setzen. Die Möglichkeit zum Erstellen von benutzerdefinierten Routen ist beispielsweise hilfreich, wenn Sie über ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) Datenverkehr zwischen Subnetzen weiterleiten möchten. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Routingtabelle
> * Erstellen einer Route
> * Erstellen eines virtuellen Netzwerks mit mehreren Subnetzen
> * Zuordnen einer Routingtabelle zu einem Subnetz
> * Erstellen eines virtuellen Netzwerkgeräts, das Datenverkehr weiterleitet
> * Bereitstellen von VMs in unterschiedlichen Subnetzen
> * Weiterleiten von Datenverkehr aus einem Subnetz zu einem anderen über ein virtuelles Netzwerkgerät

Dieser Artikel kann auch mit der [Azure CLI](tutorial-create-route-table-cli.md) oder mit [Azure PowerShell](tutorial-create-route-table-powershell.md) durchgearbeitet werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-route-table"></a>Erstellen einer Routingtabelle

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Routingtabelle** aus.

1. Geben Sie in **Routingtabelle erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *myRouteTablePublic* ein. |
    | Abonnement | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie **Neue erstellen** aus, geben Sie *myResourceGroup* ein, und wählen Sie *OK* aus. |
    | Location | Behalten Sie den Standardwert von **USA, Osten** bei.
    | BGP-Routenverteilung | Behalten Sie den Standardwert von **Aktiviert** bei. |

1. Klicken Sie auf **Erstellen**.

## <a name="create-a-route"></a>Erstellen einer Route

1. Geben Sie in der Suchleiste des Portals *myRouteTablePublic* ein.

1. Wenn **myRouteTablePublic** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.

1. Wählen Sie in **myRouteTablePublic** unter **Einstellungen**  die Optionen **Routen** > **+ Hinzufügen**.

    ![Hinzufügen einer Route](./media/tutorial-create-route-table-portal/add-route.png)

1. Geben Sie in **Route hinzufügen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Routenname | Geben Sie *ToPrivateSubnet* ein. |
    | Adresspräfix | Geben Sie *10.0.1.0/24* ein. |
    | Typ des nächsten Hops | Wählen Sie **Virtuelles Gerät** aus. |
    | Adresse des nächsten Hops | Geben Sie *10.0.2.4* ein. |

1. Klicken Sie auf **OK**.

## <a name="associate-a-route-table-to-a-subnet"></a>Zuordnen einer Routingtabelle zu einem Subnetz

Bevor Sie eine Routingtabelle einem Subnetz zuordnen können, müssen Sie ein virtuelles Netzwerk und ein Subnetz erstellen.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.

1. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *myVirtualNetwork* ein. |
    | Adressraum | Geben Sie *10.0.0.0/16* ein. |
    | Abonnement | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie ***Vorhandene auswählen*** > **MyResourceGroup**. |
    | Location | Behalten Sie den Standardwert von **USA, Osten** bei. |
    | Subnetzname | Geben Sie *Öffentlich* ein. |
    | Subnetzadressbereich | Geben Sie *10.0.0.0/24* ein. |

1. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.

### <a name="add-subnets-to-the-virtual-network"></a>Subnetze zum virtuellen Netzwerk hinzufügen

1. Geben Sie in der Suchleiste des Portals *myVirtualNetwork* ein.

1. Wenn **myVirtualNetwork** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.

1. Wählen Sie in **myVirtualNetwork** unter **Einstellungen** die Optionen **Subnetze** > **+ Subnetz**.

    ![Hinzufügen des Subnetzes](./media/tutorial-create-route-table-portal/add-subnet.png)

1. Geben Sie unter **Subnetz hinzufügen** diese Informationen ein:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *Privat* ein. |
    | Adressraum | Geben Sie *10.0.1.0/24* ein. |

1. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **OK**.

1. Wählen Sie erneut **+ Subnetz** aus. Geben Sie diese Informationen ein:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *DMZ* ein. |
    | Adressraum | Geben Sie *10.0.2.0/24* ein. |

1. Übernehmen Sie wie beim letzten Mal die übrigen Standardeinstellungen, und wählen Sie **OK**.

    Azure zeigt die drei Subnetze an: **Öffentlich**, **Privat** und **DMZ**.

### <a name="associate-myroutetablepublic-to-your-public-subnet"></a>Zuordnen von „myRouteTablePublic“ zu Ihrem öffentlichen Subnetz

1. Wählen Sie **Öffentlich** aus.

1. Wählen Sie in **Öffentlich** die Option **Routingtabelle** > **MyRouteTablePublic** > **Speichern**.

    ![Zuordnen einer Routingtabelle](./media/tutorial-create-route-table-portal/associate-route-table.png)

## <a name="create-an-nva"></a>Erstellen eines virtuellen Netzwerkgeräts

Virtuelle Netzwerkgeräte sind VMs, die bei Netzwerkfunktionen wie Routing und Firewalloptimierung helfen. Sie können ein anderes Betriebssystem auswählen, wenn Sie möchten. In diesem Tutorial wird vorausgesetzt, dass Sie **Windows Server 2016 Datacenter** verwenden.

1. Wählen Sie oben links auf dem Bildschirm die Option **Ressource erstellen** > **Compute** > **Windows Server 2016 Datacenter**.

1. Geben Sie in **Virtuellen Computer erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Abonnement | Wählen Sie Ihr Abonnement aus. |
    | Ressourcengruppe | Wählen Sie **myResourceGroup** aus. |
    | **INSTANZDETAILS** |  |
    | Name des virtuellen Computers | Geben Sie *MyVmNva* ein. |
    | Region | Wählen Sie **USA, Osten** aus. |
    | Verfügbarkeitsoptionen | Übernehmen Sie den Standardwert **Keine Infrastrukturredundanz erforderlich**. |
    | Image | Übernehmen Sie den Standardwert **Microsoft Windows Server 2016 Datacenter**. |
    | Größe | Übernehmen Sie den Standardwert **Standard DS1 v2**. |
    | **ADMINISTRATORKONTO** |  |
    | Username | Geben Sie den gewünschten Benutzernamen ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **REGELN FÜR EINGEHENDE PORTS** |  |
    | Öffentliche Eingangsports | Übernehmen Sie den Standardwert **Keine**.
    | **SPAREN SIE GELD** |  |
    | Windows-Lizenz bereits vorhanden? | Übernehmen Sie den Standardwert **Nein**. |

1. Wählen Sie **Weiter: Datenträger**.

1. Wählen Sie in **Virtuellen Computer erstellen – Datenträger** die Einstellungen entsprechend Ihren Anforderungen.

1. Wählen Sie **Weiter: Netzwerk** aus.

1. Wählen Sie in **Virtuellen Computer erstellen – Netzwerk** diese Informationen aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Virtuelles Netzwerk | Übernehmen Sie den Standardwert **myVirtualNetwork**. |
    | Subnetz | Wählen Sie **DMZ (10.0.2.0/24)** aus. |
    | Öffentliche IP-Adresse | Wählen Sie **Keine**. Sie benötigen keine öffentliche IP-Adresse. Der virtuelle Computer wird nicht über das Internet verbunden.|

1. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Weiter: Verwaltung** aus.

1. Wählen Sie in **Virtuellen Computer erstellen – Verwaltung** für **Diagnosespeicherkonto** **Neu erstellen** aus.

1. Geben Sie in **Speicherkonto erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *mynvastorageaccount* ein. |
    | Kontoart | Übernehmen Sie den Standardwert **Storage (universell, Version 1)**. |
    | Leistung | Übernehmen Sie den Standardwert **Standard**. |
    | Replikation | Übernehmen Sie den Standardwert **Lokal redundanter Speicher (LRS)**.

1. Klicken Sie auf **OK**.

1. Klicken Sie auf **Überprüfen + erstellen**. Sie gelangen auf die Seite **Bewerten + erstellen**, und Azure überprüft Ihre Konfiguration.

1. Wenn **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

    Die Erstellung des virtuellen Computers dauert einige Minuten. Fahren Sie erst fort, wenn Azure die VM erstellt hat. Auf der Seite **Ihre Bereitstellung läuft** werden Bereitstellungsdetails angezeigt.

1. Wenn Ihr virtueller Computer bereit ist, wählen Sie **Zu Ressource wechseln** aus.

## <a name="turn-on-ip-forwarding"></a>Aktivieren der IP-Weiterleitung

Aktivieren Sie die IP-Weiterleitung für *myVmNva*. Wenn Azure Netzwerkdatenverkehr an *myVmNva* sendet, und wenn der Verkehr für eine andere IP-Adresse bestimmt ist, sendet die IP-Weiterleitung den Datenverkehr an den richtigen Ort.

1. Wählen Sie für **myVmNva** unter **Einstellungen** die Option **Netzwerk**.

1. Wählen Sie **myvmnva123** aus. Dies ist die Netzwerkschnittstelle, die Azure für Ihren virtuellen Computer erstellt hat. Sie enthält eine Zahlenzeichenfolge, damit sie für Sie eindeutig ist.

    ![VM-Netzwerk](./media/tutorial-create-route-table-portal/virtual-machine-networking.png)

1. Wählen Sie unter **Einstellungen** die Option **IP-Konfigurationen**.

1. Wählen Sie auf **myvmnva123 – IP-Konfigurationen** für **IP-Weiterleitung** die Option **Aktiviert** und anschließend **Speichern**.

    ![Aktivieren der IP-Weiterleitung](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png)

## <a name="create-public-and-private-virtual-machines"></a>Erstellen von öffentlichen und privaten virtuellen Computern

Erstellen Sie einen öffentlichen und einem privaten virtuellen Computer im virtuellen Netzwerk. Später werden Sie sie verwenden, um anzuzeigen, dass Azure den *öffentlichen* Subnetzverkehr durch das virtuelle Netzwerkgerät zum *privaten* Subnetz leitet.

Führen Sie die Schritte 1 bis 12 unter [Erstellen eines virtuellen Netzwerkgeräts](#create-an-nva) aus. Verwenden Sie den Großteil der Einstellungen. Diese Werte müssen anders sein:

| Einstellung | Wert |
| ------- | ----- |
| **PUBLIC VM** | |
| BASICS |  |
| Name des virtuellen Computers | Geben Sie *myVmPublic* ein. |
| NETWORKING | |
| Subnetz | Wählen Sie **Öffentlich (10.0.0.0/24)**. |
| Öffentliche IP-Adresse | Übernehmen Sie die Standardeinstellung. |
| Öffentliche Eingangsports | Wählen Sie **Ausgewählte Ports zulassen** aus. |
| Eingangsports auswählen | Wählen Sie **HTTP** und **RDP** aus. |
| MANAGEMENT | |
| Diagnosespeicherkonto | Übernehmen Sie den Standardwert **mynvastorageaccount**. |
| **PRIVATE VM** | |
| BASICS |  |
| Name des virtuellen Computers | Geben Sie *myVmPrivate* ein. |
| NETWORKING | |
| Subnetz | Wählen Sie **Privat (10.0.1.0/24)** aus. |
| Öffentliche IP-Adresse | Übernehmen Sie die Standardeinstellung. |
| Öffentliche Eingangsports | Wählen Sie **Ausgewählte Ports zulassen** aus. |
| Eingangsports auswählen | Wählen Sie **HTTP** und **RDP** aus. |
| MANAGEMENT | |
| Diagnosespeicherkonto | Übernehmen Sie den Standardwert **mynvastorageaccount**. |

Sie können die VM *myVmPrivate* erstellen, während Azure die VM *myVmPublic* erstellt. Fahren Sie erst mit den verbleibenden Schritten fort, wenn Azure die Erstellung beider VMs abgeschlossen hat.

## <a name="route-traffic-through-an-nva"></a>Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät

### <a name="sign-in-to-myvmprivate-over-remote-desktop"></a>Melden bei „myVmPrivate“ per Remotedesktop

1. Geben Sie in der Suchleiste des Portals *myVmPrivate* ein.

1. Wenn die VM **myVmPrivate** in den Suchergebnissen angezeigt wird, wählen Sie sie aus.

1. Wählen Sie **Verbinden**, um eine Remotedesktopverbindung zur *myVmPrivate* VM herzustellen.

1. Wählen Sie unter **Mit virtuellem Computer verbinden** die Option **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die heruntergeladene *RDP*-Datei.

    1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    1. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des privaten virtuellen Computers festgelegt haben.

    1. Sie müssen möglicherweise **Weitere Optionen** > **verwenden ein anderes Konto**, um die Private-VM-Anmeldeinformationen zu verwenden.

1. Klicken Sie auf **OK**.

    Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt.

1. Wählen Sie **Ja** für die Verbindung mit dem virtuellen Computer.

### <a name="enable-icmp-through-the-windows-firewall"></a>Aktivieren Sie ICMP über die Windows-Firewall.

In einem späteren Schritt wird das Routing mithilfe des Traceroute-Tools getestet. Traceroute verwendet das Internet Control Message-Protokoll (ICMP). Dieses Protokoll wird durch die Windows-Firewall standardmäßig blockiert. Aktivieren Sie ICMP über die Windows-Firewall.

1. Öffnen Sie auf dem Remotedesktop von *myVmPrivate* PowerShell.

1. Geben Sie diesen Befehl ein:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Sie verwenden Traceroute, um das Routing in diesem Tutorial zu testen. Für Produktionsumgebungen empfehlen wir nicht, ICMP über die Windows-Firewall zuzulassen.

### <a name="turn-on-ip-forwarding-within-myvmnva"></a>Aktivieren der IP-Weiterleitung in myVmNva

Sie haben [die IP-Weiterleitung](#turn-on-ip-forwarding) für die Netzwerkschnittstelle des virtuellen Computers mit Azure aktiviert. Das Betriebssystem des virtuellen Computers muss Netzwerkdatenverkehr ebenfalls weiterleiten. Mit diesen Befehlen aktivieren Sie die IP-Weiterleitung für das Betriebssystem des virtuellen Computers *myVmNva*.

1. Öffnen Sie über eine Eingabeaufforderung auf dem virtuellen Computer *myVmPrivate* eine Remotedesktopverbindung mit dem virtuellen Computer *myVmNva*:

    ```cmd
    mstsc /v:myvmnva
    ```

1. Geben Sie über PowerShell auf der *myVmNva* diesen Befehl ein, um die IP-Weiterleitung zu aktivieren:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```

1. Starten die den virtuellen Computer *myVmNva* neu. Wählen Sie in der Taskleiste **Starttaste** > **Netzschaltersymbol**, **Andere (Geplant)** > **Fortsetzen**.

    Dadurch wird auch die Remotedesktopsitzung getrennt.

1. Nachdem der virtuelle Computer *myVmNva* neu gestartet wurde, erstellen Sie eine Remotedesktopsitzung für den virtuellen Computer *myVmPublic*. Öffnen Sie, solange Sie noch mit der *myVmPrivate* verbunden sind, eine Eingabeaufforderung und führen Sie diesen Befehl aus:

    ```cmd
    mstsc /v:myVmPublic
    ```
1. Öffnen Sie auf dem Remotedesktop von *myVmPublic* PowerShell.

1. Aktivieren Sie ICMP für die Windows-Firewall, indem Sie den folgenden Befehl eingeben:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

## <a name="test-the-routing-of-network-traffic"></a>Testen des Routings von Netzwerkdatenverkehr

Zunächst testen wir das Routing des Netzwerkdatenverkehrs von *myVmPublic* zu *myVmPrivate*.

1. Geben Sie von PowerShell auf *myVmPublic* diesen Befehl ein:

    ```powershell
    tracert myVmPrivate
    ```

    Die Antwort ähnelt diesem Beispiel:

    ```powershell
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:

    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4

    Trace complete.
    ```

    Sie sehen, dass der erste Hop zu 10.0.2.4 erfolgt. Dies ist die private IP-Adresse des Netzwerkgeräts. Der zweite Hop ist die private IP-Adresse der VM *myVmPrivate*: 10.0.1.4. Zuvor haben Sie die Route der Routingtabelle *myRouteTablePublic* hinzugefügt und dem Subnetz *Öffentlich* zugeordnet. Infolgedessen sendete Azure den Netzwerkdatenverkehr über das Netzwerkgerät und nicht direkt an das Subnetz *Privat*.

1. Schließen Sie die Remotedesktopsitzung mit der VM *myVmPublic*, sodass Sie nur noch mit der VM *myVmPrivate* verbunden sind.

1. Geben Sie über eine Eingabeaufforderung auf der VM *myVmPrivate* den folgenden Befehl ein:

    ```cmd
    tracert myVmPublic
    ```

    Damit testen wir das Routing des Netzwerkdatenverkehrs *myVmPrivate* zu *myVmPublic*. Die Antwort ähnelt diesem Beispiel:

    ```cmd
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:

    1     1 ms     1 ms     1 ms  10.0.0.4

    Trace complete.
    ```

    Wie Sie sehen, dass Azure Netzwerkdatenverkehr von der VM *myVmPrivate* zur VM *myVmPublic* weitergeleitet. Azure-Routen leiten standardmäßig Datenverkehr direkt durch Subnetze weiter.

1. Schließen Sie die Remotedesktopsitzung mit der VM *myVmPrivate*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit allen ihren Ressourcen, wenn Sie sie nicht mehr benötigen:

1. Geben Sie in die Suchleiste im Portal *myResourceGroup* ein.

1. Wenn **myResourceGroup** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie für **Geben Sie den Ressourcengruppennamen ein:** den Namen *myResourceGroup* ein, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Routingtabelle erstellt und einem Subnetz zugeordnet. Sie haben ein einfaches virtuelles Netzwerkgerät erstellt, das Datenverkehr von einem öffentlichen Subnetz an ein privates Subnetz weiterleitet. Jetzt, da Sie wissen, wie das geht, können Sie verschiedene vorkonfigurierte Netzwerkgeräte aus dem [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking) bereitstellen. Sie führen viele Netzwerkfunktionen, die Sie hilfreich finden werden. Weitere Informationen zum Routing finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](virtual-networks-udr-overview.md) und [Erstellen, Ändern oder Löschen einer Routingtabelle](manage-route-table.md).

Sie können zwar viele Azure-Ressourcen in einem virtuellen Netzwerk bereitstellen, für einige kann Azure jedoch keine Ressourcen für einige PaaS-Dienste in einem virtuellen Netzwerk bereitstellen. Es ist möglich, den Zugriff auf die Ressourcen einiger Azure PaaS-Dienste zu beschränken. Die Beschränkung darf jedoch nur Netzwerkdatenverkehr von einem Subnetz eines virtuellen Netzwerks sein. Im nächsten Tutorial erfahren Sie, wie Sie den Netzwerkzugriff auf Azure-PaaS-Ressourcen einschränken.

> [!div class="nextstepaction"]
> [Beschränken des Netzwerkzugriffs auf PaaS-Ressourcen](tutorial-restrict-network-access-to-resources.md)
