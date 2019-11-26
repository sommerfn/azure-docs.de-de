---
title: 'Tutorial: Erstellen eines Azure DNS-Aliaseintrags zum Verweisen auf eine öffentliche Azure-IP-Adresse'
description: In diesem Tutorial wird gezeigt, wie Sie einen Azure DNS-Aliaseintrag konfigurieren, um auf eine öffentliche Azure-IP-Adresse zu verweisen.
services: dns
author: asudbring
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: allensu
ms.openlocfilehash: 81dfbe5f46116d263c4a04d6178437a2c8bc1185
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74072134"
---
# <a name="tutorial-configure-an-alias-record-to-refer-to-an-azure-public-ip-address"></a>Tutorial: Konfigurieren eines Aliaseintrags, um auf eine öffentliche Azure-IP-Adresse zu verweisen 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Netzwerkinfrastruktur
> * Erstellen eines virtuellen Webservercomputers
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
2. Klicken Sie links oben im Portal auf **Ressource erstellen**. Geben Sie *Ressourcengruppe* in das Suchfeld ein, und erstellen Sie eine Ressourcengruppe namens **RG-DNS-Alias-pip**.
3. Wählen Sie **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.
4. Erstellen Sie ein virtuelles Netzwerk namens **VNet-Servers**. Platzieren Sie es in der Ressourcengruppe **RG-DNS-Alias-pip**, und nennen Sie das Subnetz **SN-Web**.

## <a name="create-a-web-server-virtual-machine"></a>Erstellen eines virtuellen Webservercomputers
1. Wählen Sie **Ressource erstellen** > **Windows Server 2016-VM** aus.
2. Geben Sie als Name **Web-01** ein, und platzieren Sie den virtuellen Computer in der Ressourcengruppe **RG-DNS-Alias-TM**. Geben Sie einen Benutzernamen und ein Kennwort ein, und wählen Sie **OK** aus.
3. Wählen Sie unter **Größe** eine SKU mit 8 GB RAM aus.
4. Wählen Sie unter **Einstellungen** das virtuelle Netzwerk **VNet-Servers** und das Subnetz **SN-Web** aus. Wählen Sie für die öffentlichen Eingangsports **HTTP** > **HTTPS** > **RDP (3389)** und anschließend **OK** aus.
5. Klicken Sie auf der Seite **Zusammenfassung** auf **Erstellen**.

Der Vorgang dauert einige Minuten.

### <a name="install-iis"></a>Installieren von IIS

Installieren Sie IIS auf **Web-01**.

1. Stellen Sie eine Verbindung mit **Web-01** her, und melden Sie sich an.
2. Wählen Sie auf dem Dashboard **Server-Manager** die Option **Rollen und Features hinzufügen** aus.
3. Wählen Sie dreimal **Weiter** aus. Wählen Sie auf der Seite **Serverrollen** die Option **Webserver (IIS)** aus.
4. Wählen Sie **Features hinzufügen** und anschließend **Weiter** aus.
5. Wählen Sie viermal **Weiter** und anschließend **Installieren** aus. Der Vorgang dauert einige Minuten.
6. Warten Sie, bis die Installation abgeschlossen ist, und wählen Sie dann **Schließen** aus.
7. Öffnen Sie einen Webbrowser. Navigieren Sie zu **localhost**, und vergewissern Sie sich, dass die IIS-Standardwebseite angezeigt wird.

## <a name="create-an-alias-record"></a>Erstellen eines Aliaseintrags

Erstellen Sie einen Aliaseintrag, der auf die öffentliche IP-Adresse verweist.

1. Wählen Sie Ihre Azure DNS-Zone aus, um sie zu öffnen.
2. Wählen Sie **Ressourceneintragssatz** aus.
3. Wählen Sie im Textfeld **Name** den Namen **web01** aus.
4. Übernehmen Sie für **Typ** die Option **A-Eintrag**.
5. Aktivieren Sie das Kontrollkästchen **Alias Record Set** (Aliaseintragssatz).
6. Wählen Sie **Azure-Dienst auswählen** und anschließend die öffentliche IP-Adresse **Web-01-ip** aus.

## <a name="test-the-alias-record"></a>Testen des Aliaseintrags

1. Wählen Sie in der Ressourcengruppe **RG-DNS-Alias-pip** den virtuellen Computer **Web-01** aus. Notieren Sie sich die öffentliche IP-Adresse.
1. Navigieren Sie in einem Webbrowser zum vollqualifizierten Domänennamen für den virtuellen Computer „Web01-01“. Beispiel: **web01.contoso.com**. Daraufhin wird die IIS-Standardwebseite angezeigt.
2. Schließen Sie den Webbrowser.
3. Beenden Sie den virtuellen Computer **Web-01**, und starten Sie ihn neu.
4. Notieren Sie sich nach dem Neustart des virtuellen Computers die neue öffentliche IP-Adresse für den virtuellen Computer.
5. Öffnen Sie einen neuen Browser. Navigieren Sie erneut zum vollqualifizierten Domänennamen für den virtuellen Computer „Web01-01“. Beispiel: **web01.contoso.com**.

Dieser Vorgang ist erfolgreich, da Sie anstelle eines standardmäßigen A-Eintrags einen Aliaseintrag verwendet haben, um auf die öffentliche IP-Adressressource zu verweisen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die für dieses Tutorial erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe **RG-DNS-Alias-pip**.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Aliaseintrag erstellt, um auf eine öffentliche Azure-IP-Adresse zu verweisen. Weitere Informationen zu Azure DNS und Web-Apps finden Sie im Tutorial für Web-Apps.

> [!div class="nextstepaction"]
> [Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne](./dns-web-sites-custom-domain.md)
