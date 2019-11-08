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
ms.openlocfilehash: bcc4af837cc28a18345e49cd39ddf88d24e44dd8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488913"
---
1. Wählen Sie **VPN-Standorte verbinden** aus, um die Seite **Standorte verbinden** zu öffnen.

    ![Verbindung](./media/virtual-wan-tutorial-connect-vpn-site-include/connect.png "Verbinden")

   Füllen Sie die folgenden Felder aus:

   * Geben Sie einen vorinstallierten Schlüssel ein. Wenn Sie keinen Schlüssel eingeben, wird von Azure automatisch ein Schlüssel generiert.
   * Wählen Sie das Protokoll und die IPsec-Einstellungen aus. Informieren Sie sich über die Details zu IPSec (Standard/Benutzerdefiniert). (Link zur Seite)
   * Wählen Sie die entsprechende Option für **Standardroute weitergeben** aus. Mit der Option **Aktivieren** kann der virtuelle Hub eine bekannte Standardroute für diese Verbindung weitergeben. Dieses Flag ermöglicht die Weitergabe der Standardroute an eine Verbindung, wenn diese dem Virtual WAN-Hub bereits bekannt ist, weil darin eine Firewall bereitgestellt wurde, oder wenn für eine andere verbundene Site die Tunnelerzwingung aktiviert ist. Der Ursprung der Standardroute liegt nicht im Virtual WAN-Hub.

2. Wählen Sie **Verbinden**aus.
3. Nach wenigen Minuten werden für die Site die Verbindung und der Konnektivitätsstatus angezeigt.

   ![status](./media/virtual-wan-tutorial-connect-vpn-site-include/status.png "status")

   **Verbindungsstatus:** Dies ist der Status der Azure-Ressource für die Verbindung, mit der der VPN-Standort mit dem VPN-Gateway des Azure-Hubs verbunden wird. Nachdem dieser Vorgang auf der Steuerungsebene erfolgreich abgeschlossen wurde, wird mit dem Herstellen der Konnektivität für das Azure-VPN-Gateway und das lokale VPN-Gerät fortgefahren.

   **Konnektivitätsstatus:** Dies ist der tatsächliche Status der Konnektivität (Datenpfad) zwischen dem VPN-Gateway von Azure im Hub und dem VPN-Standort. Der Status kann wie folgt lauten:

    * **Unbekannt:** Dieser Status tritt normalerweise auf, wenn die Back-End-Systeme aktiv sind und der Übergang in einen anderen Status erfolgt.
    * **Verbindung wird hergestellt**: Das Azure-VPN-Gateway versucht, den tatsächlichen lokalen VPN-Standort zu erreichen.
    * **Verbunden**: Die Konnektivität zwischen dem Azure-VPN-Gateway und dem lokalen VPN-Standort wird hergestellt.
    * **Getrennt**: Dieser Status tritt auf, wenn die Verbindung aus irgendeinem Grund (lokal oder in Azure) getrennt wurde.
4. An einem Hub-VPN-Standort können Sie zusätzlich Folgendes durchführen: 

   * Bearbeiten oder Löschen der VPN-Verbindung
   * Löschen des Standorts im Azure-Portal
   * Laden Sie eine branchspezifische Konfiguration herunter, um Details zur Azure-Seite zu erhalten, indem Sie das Kontextmenü (…) neben dem Standort öffnen. Wählen Sie im obersten Menü die Option **VPN-Konfiguration herunterladen** aus, wenn Sie die Konfiguration für alle verbundenen Standorte Ihres Hubs herunterladen möchten.