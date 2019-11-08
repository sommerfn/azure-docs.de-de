---
title: include file
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4bcee1097010bb8746b11185a470ca2584485c3f
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488922"
---
1. Klicken Sie auf der Portalseite für Ihr virtuelles WAN im Abschnitt **Konnektivität** auf **VPN-Standorte**, um die Seite mit VPN-Standorten zu öffnen.
2. Klicken Sie auf der Seite **VPN sites** (VPN-Standorte) auf **+Create site** (+ Standort erstellen).

   ![Grundlagen](./media/virtual-wan-tutorial-site-include/basics.png "Grundlagen")
3. Füllen Sie auf der Seite **VPN-Standort erstellen** auf der Registerkarte **Grundlagen** die folgenden Felder aus:

    * **Region**: Zuvor als Standort bezeichnet. Dies ist der Standort, an dem Sie diese Standortressource erstellen möchten.
    * **Name**: Der Name, den Sie Ihrer lokalen Site geben möchten.
    * **Gerätehersteller**: Der Name des VPN-Geräteherstellers (z. B. Citrix, Cisco, Barracuda). Diese Vorgehensweise kann dem Azure-Team helfen, Ihre Umgebung besser zu verstehen, um in Zukunft weitere Optimierungsmöglichkeiten hinzuzufügen oder Sie bei der Problembehandlung zu unterstützen.
    * **Border Gateway Protocol**: Durch eine Aktivierung werden alle Verbindungen vom Standort für BGP aktiviert. Sie richten die BGP-Informationen für jede Verbindung vom VPN-Standort im Abschnitt für Verbindungen ein. Die Vorgehensweise zum Konfigurieren von BGP für eine Virtual WAN-Instanz ist dieselbe wie beim Konfigurieren von BGP für ein VPN mit einem Azure-Gateway für virtuelle Netzwerke. Ihre lokale BGP-Peeradresse darf nicht der öffentlichen IP-Adresse Ihres VPN-Geräts oder dem VNET-Adressraum des VPN-Standorts entsprechen. Verwenden Sie als BGP-Peer-IP-Adresse eine andere IP-Adresse für das VPN-Gerät. Dabei kann es sich um eine Adresse handeln, die der Loopback-Schnittstelle des Geräts zugewiesen ist. Es darf jedoch keine APIPA-Adresse (169.254.x.x) sein. Geben Sie diese Adresse im entsprechenden VPN-Standort an, der den Standort darstellt. Informationen zu den Voraussetzungen für BGP finden Sie unter [Übersicht über BGP mit Azure VPN Gateway](../articles/vpn-gateway/vpn-gateway-bgp-overview.md). Sie können eine VPN-Verbindung jederzeit bearbeiten, um die zugehörigen BGP-Parameter zu aktualisieren (Peering-IP-Adresse für die Verbindung und Adressraumnummer), nachdem die BGP-Einstellung für den VPN-Standort aktiviert wurde.
    * **Privater Adressraum**: Dies ist der IP-Adressraum Ihres lokalen Standorts. Der Datenverkehr, der für diesen Adressraum bestimmt ist, wird an Ihre lokale Site geleitet. Dies ist erforderlich, wenn BGP nicht für den Standort aktiviert ist.
    * **Hubs**: Der Hub, mit dem Sie Ihren Standort verbinden möchten. Ein Standort kann nur mit Hubs verbunden werden, die über ein VPN-Gateway verfügen. Wenn ein Hub nicht angezeigt wird, erstellen Sie zunächst ein VPN-Gateway in diesem Hub.
4. Wählen Sie **Links** aus, um Informationen über die physischen Verbindungen an der Zweigstelle hinzuzufügen. Falls Sie über ein CPE-Gerät eines Virtual WAN-Partners verfügen, können Sie sich beim Partner erkundigen, ob diese Informationen im Rahmen des im Partnersystem eingerichteten Uploads von Zweigstelleninformationen an Azure übermittelt werden.

   ![Links](./media/virtual-wan-tutorial-site-include/links.png "Links")

    * **Linkname**: Ein Name, den Sie für die physische Verbindung am VPN-Standort angegeben möchten. Beispiel: meinLink1.
    * **Anbietername**: Der Name des Anbieters der physischen Verbindung am VPN-Standort. Beispiel: ATT, Verizon.
    * **Geschwindigkeit**: Die Geschwindigkeit des VPN-Geräts am Zweigstellenstandort. Beispiel: Der Wert 50 entspricht einer Geschwindigkeit von 50 MBit/s für das VPN-Gerät am Zweigstellenstandort.
    * **IP-Adresse**: Die öffentliche IP-Adresse des lokalen Geräts, dass diese Verbindung verwendet. Optional können Sie die private IP-Adresse Ihres lokalen VPN-Geräts hinter ExpressRoute angeben.
5. Mithilfe des Kontrollkästchens können Sie Verbindungen löschen oder zusätzliche Verbindungen hinzufügen. Pro VPN-Standort werden vier Verbindungen unterstützt. Wenn Sie beispielsweise vier Internetdienstanbieter am Zweigstellenstandort einsetzen, können Sie vier Verbindungen erstellen. Erstellen Sie eine Verbindung pro Internetdienstanbieter, und geben Sie für jede Verbindung Informationen an.
6. Nachdem Sie die Felder ausgefüllt haben, klicken Sie auf **Überprüfen + erstellen**, um die Eingaben zu überprüfen und den Standort zu erstellen.
7. Zeigen Sie den Status auf der Seite „VPN-Standorte“ an. Der Standort wechselt in den Zustand **Verbindung erforderlich**, weil der Standort noch nicht mit dem Hub verbunden wurde.