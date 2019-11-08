---
title: Verwenden von Azure Virtual WAN zum Erstellen von ExpressRoute-Verbindungen mit Azure und lokalen Umgebungen | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie Azure Virtual WAN verwenden, um ExpressRoute-Verbindungen mit Azure und lokalen Umgebungen zu erstellen.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my corporate on-premises network(s) to my VNets using Virtual WAN and ExpressRoute.
ms.openlocfilehash: 8ad86280eab3041667bf9d1713ae2b4bc82a4c9e
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491534"
---
# <a name="tutorial-create-an-expressroute-association-using-azure-virtual-wan"></a>Tutorial: Erstellen einer ExpressRoute-Zuordnung mithilfe von Azure Virtual WAN

In diesem Tutorial wird veranschaulicht, wie Sie mit Virtual WAN über eine ExpressRoute-Leitung eine Verbindung mit Ihren Ressourcen in Azure herstellen. Weitere Informationen zu Virtual WAN und zugehörigen Ressourcen finden Sie in der [Übersicht über Virtual WAN](virtual-wan-about.md).

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen WAN
> * Erstellen eines Hubs und eines Gateways
> * Verbinden eines VNET mit einem Hub
> * Verbinden einer Leitung mit einem Hubgateway
> * Testen der Konnektivität
> * Ändern einer Gatewaygröße
> * Ankündigen einer Standardroute

## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass die folgenden Voraussetzungen erfüllt sind bzw. Folgendes vorhanden ist:

* Sie verfügen über ein virtuelles Netzwerk, mit dem Sie eine Verbindung herstellen möchten. Stellen Sie sicher, dass sich kein Subnetz Ihres lokalen Netzwerks mit den virtuellen Netzwerken überlappt, mit denen Sie eine Verbindung herstellen möchten. Informationen zum Erstellen eines virtuellen Netzwerks im Azure-Portal finden Sie im [Schnellstart](../virtual-network/quick-create-portal.md).

* Ihr virtuelles Netzwerk verfügt nicht über Gateways für virtuelle Netzwerke. Falls Ihr virtuelles Netzwerk über ein Gateway verfügt (entweder VPN oder ExpressRoute), müssen Sie alle Gateways entfernen. Für diese Konfiguration ist es erforderlich, dass virtuelle Netzwerke stattdessen mit dem Gateway des Virtual WAN-Hubs verbunden werden.

* Beschaffen Sie sich einen IP-Adressbereich für Ihre Hubregion. Der Hub ist ein virtuelles Netzwerk, das von Virtual WAN erstellt und verwendet wird. Der von Ihnen für den Hub angegebene Adressbereich darf sich nicht mit einem Ihrer vorhandenen virtuellen Netzwerke überlappen, mit denen Sie eine Verbindung herstellen. Außerdem ist keine Überlappung mit Ihren Adressbereichen möglich, mit denen Sie lokal eine Verbindung herstellen. Falls Sie nicht mit den IP-Adressbereichen in Ihrer lokalen Netzwerkkonfiguration vertraut sind, sollten Sie sich an eine Person wenden, die Ihnen diese Informationen zur Verfügung stellen kann.

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.

## <a name="openvwan"></a>Erstellen eines Virtual WAN

Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.

1. Navigieren Sie zur Seite „Virtual WAN“. Klicken Sie im Portal auf **Ressource erstellen**. Geben Sie **Virtual WAN** in das Suchfeld ein, und drücken Sie die EINGABETASTE.
2. Wählen Sie in den Ergebnissen **Virtual WAN** aus. Klicken Sie auf der Seite „Virtual WAN“ auf **Erstellen**, um die Seite „WAN erstellen“ zu öffnen.
3. Füllen Sie auf der Seite **WAN erstellen** auf der Registerkarte **Grundlagen** die folgenden Felder aus:

   ![Erstellen des WAN](./media/virtual-wan-expressroute-portal/createwan.png)

   * **Abonnement**: Wählen Sie das Abonnement aus, das Sie verwenden möchten.
   * **Ressourcengruppe**: Erstellen Sie eine neue oder verwenden Sie eine vorhandene Ressourcengruppe.
   * **Ressourcengruppenstandort**: Wählen Sie in der Dropdownliste einen Ressourcengruppenstandort aus. Ein WAN ist eine globale Ressource, die nicht in einer bestimmten Region angeordnet ist. Sie müssen aber eine Region auswählen, damit Sie die von Ihnen erstellte WAN-Ressource leichter verwalten und finden können.
   * **Name**: Geben Sie den für Ihr WAN gewünschten Namen ein.
   * **Typ**: Wählen Sie **Standard** aus. Mit der SKU „Basic“ können Sie kein ExpressRoute-Gateway erstellen.
4. Klicken Sie nach dem Ausfüllen der Felder auf **Überprüfen + erstellen**.
5. Klicken Sie nach bestandenen Überprüfung auf **Erstellen**, um das virtuelle WAN zu erstellen.

## <a name="hub"></a>Erstellen eines virtuellen Hubs und eines Gateways

Ein virtueller Hub ist ein virtuelles Netzwerk, das von Virtual WAN erstellt und genutzt wird. Es kann verschiedene Gateways wie VPN und ExpressRoute enthalten. In diesem Abschnitt erstellen Sie für Ihren virtuellen Hub ein ExpressRoute-Gateway. Sie können das Gateway entweder beim [Erstellen eines neuen virtuellen Hubs](#newhub) erstellen oder es in einem [bestehenden Hub](#existinghub) erstellen, indem Sie es bearbeiten. 

ExpressRoute-Gateways werden in Einheiten von 2 Gbit/s bereitgestellt. 1 Skalierungseinheit = 2 Gbit/s mit Unterstützung von bis zu 10 Skalierungseinheiten = 20 Gbit/s. Die vollständige Erstellung eines virtuellen Hubs und eines Gateways dauert ca. 30 Minuten.

### <a name="newhub"></a>Erstellen eines neuen virtuellen Hubs und eines Gateways

Erstellen Sie einen neuen virtuellen Hub. Nachdem ein Hub erstellt wurde, werden Ihnen für den Hub auch dann Kosten berechnet, wenn Sie keine Websites zuordnen.

[!INCLUDE [Create a hub](../../includes/virtual-wan-tutorial-er-hub-include.md)]

### <a name="existinghub"></a>Erstellen eines Gateways in einem bestehenden Hub

Sie können auch ein Gateway in einem bestehenden Hub erstellen, indem Sie es bearbeiten.

1. Navigieren Sie zum virtuellen Hub, den Sie bearbeiten möchten, und wählen Sie ihn aus.
2. Aktivieren Sie auf der Seite **Virtuellen Hub bearbeiten** das Kontrollkästchen **ExpressRoute-Gateway einschließen**.
3. Klicken Sie zum Bestätigen der Änderungen auf **Bestätigen**. Die vollständige Erstellung eines Hubs samt Ressourcen dauert ca. 30 Minuten.

   ![Bestehender Hub](./media/virtual-wan-expressroute-portal/edithub.png "Bearbeiten eines Hubs")

### <a name="to-view-a-gateway"></a>Anzeigen eines Gateways

Nachdem Sie ein ExpressRoute-Gateway erstellt haben, können Sie dessen Details anzeigen. Navigieren Sie zum Hub, wählen Sie **ExpressRoute** aus, und zeigen Sie das Gateway an.

![Gateway anzeigen](./media/virtual-wan-expressroute-portal/viewgw.png "Gateway anzeigen")

## <a name="connectvnet"></a>Verbinden Ihres VNET mit dem Hub

In diesem Abschnitt erstellen Sie die Peeringverbindung zwischen Ihrem Hub und einem VNET. Wiederholen Sie diese Schritte für jedes VNET, mit dem Sie eine Verbindung herstellen möchten.

1. Klicken Sie auf der Seite für Ihr virtuelles WAN auf **Virtual network connection** (VNET-Verbindung).
2. Klicken Sie auf der Seite für die VNET-Verbindung auf **+Add connection** (+Verbindung hinzufügen).
3. Füllen Sie auf der Seite **Add connection** (Verbindung hinzufügen) die folgenden Felder aus:

    * **Verbindungsname**: Dies ist der Name Ihrer Verbindung.
    * **Hubs**: Wählen Sie den Hub aus, den Sie dieser Verbindung zuordnen möchten.
    * **Abonnement**: Überprüfen Sie das Abonnement.
    * **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk aus, das Sie mit diesem Hub verbinden möchten. Für das virtuelle Netzwerk darf nicht bereits ein Gateway für virtuelle Netzwerke vorhanden sein (weder VPN noch ExpressRoute).

## <a name="connectcircuit"></a>Verbinden Ihrer Leitung mit dem Hubgateway

Nachdem das Gateway erstellt wurde, können Sie es mit einer [ExpressRoute-Leitung](../expressroute/expressroute-howto-circuit-portal-resource-manager.md) verbinden. Beachten Sie, dass ExpressRoute Premium-Leitungen, die sich an von ExpressRoute Global Reach unterstützten Standorten befinden, eine Verbindung mit einem Virtual WAN ExpressRoute-Gateway herstellen können.

### <a name="to-connect-the-circuit-to-the-hub-gateway"></a>Verbinden der Leitung mit dem Hubgateway

Wechseln Sie im Portal zur Seite **Virtueller Hub -> Konnektivität -> ExpressRoute**. Wenn Sie in Ihrem Abonnement Zugriff auf eine ExpressRoute-Leitung haben, sehen Sie die zu verwendende Leitung in der Liste der Leitungen. Wenn Sie keine Leitungen sehen, aber über einen Autorisierungsschlüssel und einen Peerleitungs-URI verfügen, können Sie eine Leitung einlösen und verbinden. Siehe [Herstellen einer Verbindung durch Einlösen eines Autorisierungsschlüssels](#authkey).

1. Wählen Sie die Leitung aus.
2. Wählen Sie **Leitung(en) verbinden** aus.

   ![Leitung(en) verbinden](./media/virtual-wan-expressroute-portal/cktconnect.png "Leitungen verbinden")

### <a name="authkey"></a>Herstellen einer Verbindung durch Einlösen eines Autorisierungsschlüssels

Verwenden Sie den Autorisierungsschlüssel und den Leitungs-URI, die Ihnen zur Verfügung gestellt wurden, um eine Verbindung herzustellen.

1. Klicken Sie auf der Seite „ExpressRoute“ auf **+Autorisierungsschlüssel einlösen**.

   ![Einlösen](./media/virtual-wan-expressroute-portal/redeem.png "einlösen")
2. Füllen Sie auf der Seite „Autorisierungsschlüssel einlösen“ die Werte aus.

   ![Einlösen von Schlüsselwerten](./media/virtual-wan-expressroute-portal/redeemkey2.png "Schlüsselwerte einlösen")
3. Klicken Sie auf **Hinzufügen**, um den Schlüssel hinzuzufügen.
4. Zeigen Sie die Leitung an. Bei einer eingelösten Leitung wird nur der Name (ohne Typ, Anbieter und andere Informationen) angezeigt, da sie sich in einem anderen Abonnement als dem des Benutzers befindet.

## <a name="to-test-connectivity"></a>Testen der Konnektivität

Nachdem die Leitungsverbindung hergestellt wurde, wird als Hubverbindungsstatus „dieser Hub“ angezeigt, was bedeutet, dass die Verbindung mit dem ExpressRoute-Gateway des Hubs hergestellt wird. Warten Sie ca. 5 Minuten, bevor Sie die Konnektivität auf einem Client hinter Ihrer ExpressRoute-Leitung testen, z. B. einer VM im VNET, das Sie zuvor erstellt haben.

Wenn Sie Standorte haben, die mit einem Virtual WAN VPN-Gateway im gleichen Hub wie das ExpressRoute-Gateway verbunden sind, können Sie eine bidirektionale Verbindung zwischen VPN- und ExpressRoute-Endpunkten herstellen. Dynamisches Routing (BGP) wird unterstützt. Die ASN der Gateways im Hub ist zur Zeit unveränderlich.

## <a name="to-change-the-size-of-a-gateway"></a>Ändern der Größe des Gateways

Wenn Sie die Größe Ihres ExpressRoute-Gateways ändern möchten, wechseln Sie zum ExpressRoute-Gateway innerhalb des Hubs, und wählen Sie die Skalierungseinheiten im Dropdownmenü aus. Speichern Sie die Änderung. Das Aktualisieren des Hubgateways dauert ca. 30 Minuten.

![Ändern einer Gatewaygröße](./media/virtual-wan-expressroute-portal/changescale.png "Gatewaygröße ändern")

## <a name="to-advertise-default-route-00000-to-endpoints"></a>Ankündigen der Standardroute 0.0.0.0/0 für Endpunkte

Wenn Sie möchten, dass der virtuelle Azure-Hub die Standardroute 0.0.0.0.0.0/0 Ihren ExpressRoute-Endpunkten ankündigt, müssen Sie das Weitergeben der Standardroute aktivieren.

1. Wählen Sie Ihre **Leitung ->…-> Verbindung bearbeiten** aus.

   ![Bearbeiten der Verbindung](./media/virtual-wan-expressroute-portal/defaultroute1.png "Bearbeiten der Verbindung")

2. Wählen Sie **Aktivieren** aus, um die Standardroute weiterzugeben.

   ![Standardroute weitergeben](./media/virtual-wan-expressroute-portal/defaultroute2.png "Standardroute weitergeben")

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Virtual WAN finden Sie auf der Seite mit der [Übersicht über Virtual WAN](virtual-wan-about.md).