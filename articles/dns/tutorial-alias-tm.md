---
title: 'Tutorial: Erstellen eines Aliaseintrags zur Unterstützung von Apex-Domänennamen: Traffic Manager'
titleSuffix: Azure DNS
description: In diesem Tutorial erfahren Sie, wie Sie einen Azure DNS-Alias-Ressourceneintrag für die Verwendung des Apex-Domänennamens mit Traffic Manager konfigurieren.
services: dns
author: asudbring
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: allensu
ms.openlocfilehash: 3834b782be054611de67b782b7fcd0c46cbf3a19
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74082251"
---
# <a name="tutorial-configure-an-alias-record-to-support-apex-domain-names-with-traffic-manager"></a>Tutorial: Konfigurieren eines Alias-Ressourceneintrags zur Unterstützung von Apex-Domänennamen mit Traffic Manager 

Sie können einen Aliaseintrag für den Domänennamen-Apex erstellen, um auf ein Azure Traffic Manager-Profil zu verweisen. Ein Beispiel wäre etwa „contoso.com“. Statt einen Umleitungsdienst zu verwenden, können Sie Azure DNS so konfigurieren, dass direkt von Ihrer Zone aus auf ein Traffic Manager-Profil verwiesen wird. 


In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Hostcomputers und einer Netzwerkinfrastruktur
> * Erstellen eines Traffic Manager-Profils
> * Erstellen eines Aliaseintrags
> * Testen des Aliaseintrags


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
Sie müssen über einen Domänennamen verfügen, den Sie zum Testen in Azure DNS hosten können. Sie müssen uneingeschränkte Kontrolle über diese Domäne haben. Das bedeutet, Sie müssen unter anderem die Namenservereinträge für die Domäne festlegen können.

Eine Anleitung zum Hosten Ihrer Domäne in Azure DNS finden Sie unter [Tutorial: Hosten Ihrer Domäne in Azure DNS](dns-delegate-domain-azure-dns.md).

Die für dieses Tutorial verwendete Beispieldomäne lautet „contoso.com“. Verwenden Sie aber ruhig Ihren eigenen Domänennamen.

## <a name="create-the-network-infrastructure"></a>Erstellen der Netzwerkinfrastruktur
Erstellen Sie zuerst ein virtuelles Netzwerk und ein Subnetz, um die Webserver darin zu platzieren.
1. Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.
2. Klicken Sie links oben im Portal auf **Ressource erstellen**. Geben Sie *Ressourcengruppe* in das Suchfeld ein, und erstellen Sie eine Ressourcengruppe namens **RG-DNS-Alias-TM**.
3. Wählen Sie **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.
4. Erstellen Sie ein virtuelles Netzwerk namens **VNet-Servers**. Platzieren Sie es in der Ressourcengruppe **RG-DNS-Alias-TM**, und nennen Sie das Subnetz **SN-Web**.

## <a name="create-two-web-server-virtual-machines"></a>Erstellen von zwei virtuellen Webservercomputern
1. Wählen Sie **Ressource erstellen** > **Windows Server 2016-VM** aus.
2. Geben Sie als Name **Web-01** ein, und platzieren Sie den virtuellen Computer in der Ressourcengruppe **RG-DNS-Alias-TM**. Geben Sie einen Benutzernamen und ein Kennwort ein, und wählen Sie **OK** aus.
3. Wählen Sie unter **Größe** eine SKU mit 8 GB RAM aus.
4. Wählen Sie unter **Einstellungen** das virtuelle Netzwerk **VNet-Servers** und das Subnetz **SN-Web** aus.
5. Wählen Sie **Öffentliche IP-Adresse**. Wählen Sie unter **Zuweisung** die Option **Statisch** und anschließend **OK** aus.
6. Wählen Sie für die öffentlichen Eingangsports **HTTP** > **HTTPS** > **RDP (3389)** und anschließend **OK** aus.
7. Klicken Sie auf der Seite **Zusammenfassung** auf **Erstellen**. Der Vorgang dauert einige Minuten.

Wiederholen Sie dieses Verfahren, um einen weiteren virtuellen Computer mit dem Namen **Web-02** zu erstellen.

### <a name="add-a-dns-label"></a>Hinzufügen einer DNS-Bezeichnung
Für die Verwendung der öffentlichen IP-Adressen mit Traffic Manager ist eine DNS-Bezeichnung erforderlich.
1. Wählen Sie in der Ressourcengruppe **RG-DNS-Alias-TM** die öffentliche IP-Adresse **Web-01-ip** aus.
2. Wählen Sie unter **Einstellungen** die Option **Konfiguration**.
3. Geben Sie im Textfeld „DNS-Namensbezeichnung“ die Zeichenfolge **web01pip** ein.
4. Wählen Sie **Speichern** aus.

Wiederholen Sie dieses Verfahren für die öffentliche IP-Adresse **Web-02-ip**, und verwenden Sie dabei **web02pip** als DNS-Namensbezeichnung.

### <a name="install-iis"></a>Installieren von IIS

Installieren Sie IIS auf **Web-01** und **Web-02**.

1. Stellen Sie eine Verbindung mit **Web-01** her, und melden Sie sich an.
2. Wählen Sie auf dem Dashboard **Server-Manager** die Option **Rollen und Features hinzufügen** aus.
3. Wählen Sie dreimal **Weiter** aus. Wählen Sie auf der Seite **Serverrollen** die Option **Webserver (IIS)** aus.
4. Wählen Sie **Features hinzufügen** und anschließend **Weiter** aus.
5. Wählen Sie viermal **Weiter** aus. Wählen Sie dann **Installieren** aus. Der Vorgang dauert einige Minuten.
6. Warten Sie, bis die Installation abgeschlossen ist, und wählen Sie dann **Schließen** aus.
7. Öffnen Sie einen Webbrowser. Navigieren Sie zu **localhost**, und vergewissern Sie sich, dass die IIS-Standardwebseite angezeigt wird.

Wiederholen Sie dieses Verfahren, um IIS auf **Web-02** zu installieren.


## <a name="create-a-traffic-manager-profile"></a>Erstellen eines Traffic Manager-Profils

1. Öffnen Sie die Ressourcengruppe **RG-DNS-Alias-TM**, und wählen Sie die öffentliche IP-Adresse **Web-01-ip** aus. Notieren Sie sich die IP-Adresse für später. Wiederholen Sie diesen Schritt für die öffentliche IP-Adresse **Web-02-ip**.
1. Wählen Sie **Ressource erstellen** > **Netzwerk** > **Traffic Manager-Profil** aus.
2. Geben Sie als Name **TM-alias-test** ein. Platzieren Sie es in der Ressourcengruppe **RG-DNS-Alias-TM**.
3. Klicken Sie auf **Erstellen**.
4. Wählen Sie nach Abschluss der Bereitstellung die Option **Zu Ressource wechseln** aus.
5. Wählen Sie auf der Seite „Traffic Manager-Profil“ unter **Einstellungen** die Option **Endpunkte** aus.
6. Wählen Sie **Hinzufügen**.
7. Wählen Sie für **Typ** die Option **Externer Endpunkt** aus, und geben Sie für **Name** den Namen **EP-Web01** ein.
8. Geben Sie im Textfeld **Vollqualifizierter Domänenname (FQDN) oder IP-Adresse** die IP-Adresse für **Web-01-ip** ein, die Sie sich zuvor notiert haben.
9. Wählen Sie den gleichen **Standort** wie für die anderen Ressourcen aus, und wählen Sie dann **OK** aus.

Wiederholen Sie dieses Verfahren, um den Endpunkt **Web-02** unter Verwendung der IP-Adresse hinzuzufügen, die Sie sich zuvor für **Web-02-ip** notiert haben.

## <a name="create-an-alias-record"></a>Erstellen eines Aliaseintrags

Erstellen Sie einen Alias-Ressourceneintrag, der auf das Traffic Manager-Profil verweist.

1. Wählen Sie Ihre Azure DNS-Zone aus, um sie zu öffnen.
2. Wählen Sie **Datensatzgruppe** aus.
3. Lassen Sie das Textfeld **Name** leer, um den Domänennamen-Apex darzustellen. Ein Beispiel wäre etwa „contoso.com“.
4. Übernehmen Sie für **Typ** die Option **A-Eintrag**.
5. Aktivieren Sie das Kontrollkästchen **Alias Record Set** (Aliaseintragssatz).
6. Wählen Sie **Azure-Dienst auswählen** und anschließend das Traffic Manager-Profil **TM-alias-test** aus.

## <a name="test-the-alias-record"></a>Testen des Aliaseintrags

1. Navigieren Sie in einem Webbrowser zum Domänennamen-Apex. Ein Beispiel wäre etwa „contoso.com“. Die IIS-Standardwebseite wird angezeigt. Schließen Sie den Webbrowser.
2. Fahren Sie den virtuellen Computer **Web-01** herunter. Warten Sie einige Minuten, bis er vollständig heruntergefahren ist.
3. Öffnen Sie einen neuen Webbrowser, und navigieren Sie erneut zum Domänennamen-Apex.
4. Daraufhin wird erneut die IIS-Standardseite angezeigt, da Traffic Manager den Datenverkehr an **Web-02** umgeleitet hat.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die für dieses Tutorial erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe **RG-DNS-Alias-TM**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Aliaseintrag erstellt, um unter Verwendung Ihres Apex-Domänennamens auf ein Traffic Manager-Profil zu verweisen. Weitere Informationen zu Azure DNS und Web-Apps finden Sie im Tutorial für Web-Apps.

> [!div class="nextstepaction"]
> [Hosten von Web-Apps mit Lastenausgleich im Zonen-Apex](./dns-alias-appservice.md)
