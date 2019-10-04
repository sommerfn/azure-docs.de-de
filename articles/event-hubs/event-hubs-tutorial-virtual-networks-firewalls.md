---
title: Tutorial – Aktivieren der Integration virtueller Netzwerke und von Firewalls für Event Hubs | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie Event Hubs in virtuelle Netzwerke und Firewalls integrieren, um den sicheren Zugriff zu ermöglichen.
services: event-hubs
author: axisc
manager: darosa
ms.author: aschhab
ms.date: 11/28/2018
ms.topic: tutorial
ms.service: event-hubs
ms.custom: mvc
ms.openlocfilehash: 0f7c7e348c154aab1deb10273346a5395599b745
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605863"
---
# <a name="tutorial-enable-virtual-networks-integration-and-firewalls-on-event-hubs-namespace"></a>Tutorial: Aktivieren der Integration virtueller Netzwerke und von Firewalls für den Event Hubs-Namespace

Mit [Dienstendpunkten von virtuellen Netzwerken (VNETs)](../virtual-network/virtual-network-service-endpoints-overview.md) werden der Bereich privater Adressen Ihres virtuellen Netzwerks und die Identität Ihres VNET über eine direkte Verbindung auf die Azure-Dienste erweitert. Endpunkte ermöglichen es Ihnen, Ihre kritischen Ressourcen von Azure-Diensten auf Ihre virtuellen Netzwerke zu beschränken und so zu schützen. Der Datenverkehr aus Ihrem VNET an den Azure-Dienst verbleibt immer im Backbone-Netzwerk von Microsoft Azure.

Mit Firewalls können Sie den Zugriff auf den Event Hubs-Namespace von bestimmten IP-Adressen (oder IP-Adressbereichen) beschränken.

In diesem Tutorial wird veranschaulicht, wie Sie VNET-Dienstendpunkte integrieren und Firewalls (IP-Filterung) mit Ihrem vorhandenen Azure Event Hubs-Namespace einrichten, indem Sie das Portal verwenden.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Integrieren von VNET-Dienstendpunkten in Ihren Event Hubs-Namespace
> * Einrichten der Firewall (IP-Filterung) mit Ihrem Event Hubs-Namespace

>[!WARNING]
> Die Implementierung der VNET-Integration kann verhindern, dass andere Azure-Dienste mit Event Hubs interagieren.
>
> Erstanbieterintegrationen werden nicht unterstützt, wenn virtuelle Netzwerke aktiviert sind.
> Häufige Azure-Szenarien, die mit virtuellen Netzwerken nicht funktionieren:
> * Azure-Diagnose und -Protokollierung
> * Azure Stream Analytics
> * Event Grid-Integration
> * Web-Apps und Functions müssen in einem virtuellen Netzwerk angeordnet sein
> * IoT Hub-Routen
> * IoT Device Explorer


> [!IMPORTANT]
> Virtuelle Netzwerke werden in den Tarifen **Standard** und **Dediziert** von Event Hubs unterstützt. Sie werden nicht im Basic-Tarif unterstützt.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][] erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Da wir einen vorhandenen Event Hubs-Namespace nutzen, sollten Sie sicherstellen, dass bei Ihnen hierfür ein Event Hubs-Namespace verfügbar ist. Falls nicht, hilft Ihnen [dieses Tutorial](./event-hubs-create.md) weiter.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Beginnen Sie im [Azure-Portal][Azure portal], und melden Sie sich mit Ihrem Azure-Abonnement an.

## <a name="select-event-hubs-namespace"></a>Auswählen eines Event Hubs-Namespace

Für dieses Tutorial haben wir einen Event Hubs-Namespace erstellt, zu dem wir navigieren.

## <a name="navigate-to-firewalls-and-virtual-networks-experience"></a>Navigieren zur Benutzeroberfläche für Firewalls und virtuelle Netzwerke

Verwenden Sie das Navigationsmenü im linken Bereich des Portals, um die Option **Firewalls und virtuelle Netzwerke** auswählen zu können.

  ![Navigieren zum Menü](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-landing-page.png)

  Wenn Sie diese Seite zum ersten Mal besuchen, muss das Optionsfeld **Alle Netzwerke** ausgewählt sein. Dies bewirkt, dass für den Event Hubs-Namespace alle eingehenden Verbindungen zugelassen werden.

## <a name="add-virtual-network-service-endpoint"></a>Hinzufügen eines VNET-Dienstendpunkts

Zum Beschränken des Zugriffs müssen Sie den Dienstendpunkt des virtuellen Netzwerks (VNET-Dienstendpunkt) für diesen Event Hubs-Namespace integrieren.

1. Klicken Sie oben auf der Seite auf das Optionsfeld **Ausgewählte Netzwerke**, um den Rest der Seite mit Menüoptionen zu aktivieren.
  ![Ausgewählte Netzwerke](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-selecting-selected-networks.png)
2. Wählen Sie im Abschnitt „Virtuelles Netzwerk“ der Seite die Option ***+ Vorhandenes virtuelles Netzwerk hinzufügen***. Hiermit wird der Bereich angezeigt, in dem Sie ein bereits erstelltes virtuelles Netzwerk auswählen können.
  ![Hinzufügen eines vorhandenen virtuellen Netzwerks](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane.png)
3. Wählen Sie in der Liste das virtuelle Netzwerk und dann das Subnetz aus.
   ![Auswählen des Subnetzes](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane-with-subnet-query.png)
4. Sie müssen den Dienstendpunkt aktivieren, bevor Sie das virtuelle Netzwerk der Liste hinzufügen. Wenn der Dienstendpunkt nicht aktiviert ist, erhalten Sie im Portal eine entsprechende Aufforderung.
  ![Auswählen des Subnetzes und Aktivieren des Endpunkts](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane-after-enabling.png)
    > [!NOTE]
    > Falls Sie den Dienstendpunkt nicht aktivieren können, können Sie den fehlenden VNET-Dienstendpunkt ignorieren und die ARM-Vorlage verwenden. Diese Funktionalität ist nicht im Portal verfügbar.

5. Nach der Aktivierung des Dienstendpunkts im ausgewählten Subnetz können Sie ihn als Nächstes der Liste mit den zulässigen virtuellen Netzwerken hinzufügen.
  ![Hinzufügen des Subnetzes nach dem Aktivieren des Endpunkts](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-vnet-from-portal-slide-in-pane-after-adding.png)

6. Wählen Sie im oberen Menüband die Schaltfläche **Speichern**, um die Konfiguration des virtuellen Netzwerks für den Dienst zu speichern. Warten Sie einige Minuten, bis die Bestätigung unter den Portalbenachrichtigungen angezeigt wird.

## <a name="add-firewall-for-specified-ip"></a>Hinzufügen der Firewall für die angegebene IP-Adresse

Wir können den Zugriff auf den Event Hubs-Namespace für einen begrenzten Bereich von IP-Adressen oder eine bestimmte IP-Adresse beschränken, indem wir Firewallregeln verwenden.

1. Klicken Sie oben auf der Seite auf das Optionsfeld **Ausgewählte Netzwerke**, um den Rest der Seite mit Menüoptionen zu aktivieren.
  ![Ausgewählte Netzwerke](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-selecting-selected-networks.png)
2. Im Abschnitt **Firewall** unter dem Raster ***Adressbereich*** können Sie spezifische IP-Adressen oder IP-Adressbereiche hinzufügen.
  ![Hinzufügen von IP-Adressen](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-firewall.png)
3. Wählen Sie nach dem Hinzufügen der IP-Adressen (bzw. IP-Adressbereiche) im oberen Menüband die Option **Speichern**, um sicherzustellen, dass die Konfiguration auf Dienstseite gespeichert wird. Warten Sie einige Minuten, bis die Bestätigung unter den Portalbenachrichtigungen angezeigt wird.
  ![Hinzufügen von IP-Adressen und Auswählen von „Speichern“](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-firewall-hitting-save.png)

## <a name="adding-your-current-ip-address-to-the-firewall-rules"></a>Hinzufügen Ihrer aktuellen IP-Adresse zu den Firewallregeln

1. Sie haben auch die Möglichkeit, Ihre aktuelle IP-Adresse schnell hinzuzufügen. Aktivieren Sie hierzu das Kontrollkästchen ***Add your client IP address (YOUR CURRENT IP ADDRESS)*** (Client-IP-Adresse hinzufügen (IHRE AKTUELLE IP-ADRESSE)) direkt oberhalb des Rasters ***Adressbereich***.
  ![Hinzufügen der aktuellen IP-Adresse](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-current-ip-hitting-save.png)
2. Wählen Sie nach dem Hinzufügen Ihrer aktuellen IP-Adresse zu den Firewallregeln im oberen Menüband die Option **Speichern**, um sicherzustellen, dass die Konfiguration auf Dienstseite gespeichert wird. Warten Sie einige Minuten, bis die Bestätigung unter den Portalbenachrichtigungen angezeigt wird.
  ![Hinzufügen der aktuellen IP-Adresse und Auswählen von „Speichern“](./media/event-hubs-tutorial-vnet-and-firewalls/vnet-firewall-adding-current-ip-hitting-save-after-saving.png)

## <a name="conclusion"></a>Zusammenfassung

In diesem Tutorial haben Sie VNET-Endpunkte und Firewallregeln in einen vorhandenen Event Hubs-Namespace integriert. Sie haben Folgendes gelernt:
> [!div class="checklist"]
> * Integrieren von VNET-Dienstendpunkten in Ihren Event Hubs-Namespace
> * Einrichten der Firewall (IP-Filterung) mit Ihrem Event Hubs-Namespace


[Azure portal]: https://portal.azure.com/
