---
title: 'Schnellstart: Erstellen einer privaten Azure DNS-Zone im Azure-Portal'
description: In diesem Schnellstart erstellen und testen Sie eine private DNS-Zone und einen Eintrag in Azure DNS. Dies ist eine ausführliche Anleitung zum Erstellen und Verwalten Ihrer ersten privaten DNS-Zone und Ihres ersten DNS-Eintrags im Azure-Portal.
services: dns
author: vhorne
ms.service: dns
ms.topic: quickstart
ms.date: 10/11/2019
ms.author: victorh
ms.openlocfilehash: 352a63e611e3823e03bedb01a9c1a5071d628c4f
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73163823"
---
# <a name="quickstart-create-an-azure-private-dns-zone-using-the-azure-portal"></a>Schnellstart: Erstellen einer privaten Azure DNS-Zone im Azure-Portal

In diesem Schnellstart wird Schritt für Schritt gezeigt, wie Sie im Azure-Portal Ihre erste private DNS-Zone und Ihren ersten DNS-Eintrag erstellen.

Eine DNS-Zone wird zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet. Wenn Sie eine Domäne in Azure DNS hosten möchten, müssen Sie eine DNS-Zone für diesen Domänennamen erstellen. Jeder DNS-Eintrag für Ihre Domäne wird dann in dieser DNS-Zone erstellt. Um eine private DNS-Zone in Ihrem virtuellen Netzwerk zu veröffentlichen, geben Sie die Liste mit den virtuellen Netzwerken an, für die das Auflösen von Einträgen in der Zone zulässig ist.  Diese werden als *verknüpfte* virtuelle Netzwerke bezeichnet. Wenn die automatische Registrierung aktiviert ist, aktualisiert Azure DNS auch die Zoneneinträge, sobald ein virtueller Computer erstellt, seine IP-Adresse geändert oder der virtuelle Computer gelöscht wird.

In dieser Schnellstartanleitung wird Folgendes vermittelt:

> [!div class="checklist"]
> * Erstellen einer privaten DNS-Zone
> * Erstellen eines virtuellen Netzwerks
> * Verknüpfen des virtuellen Networks
> * Erstellen von virtuellen Testcomputern
> * Erstellen eines zusätzlichen DNS-Eintrags
> * Testen der privaten Zone

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Sie können für diesen Schnellstart auch [Azure PowerShell](private-dns-getstarted-powershell.md) oder die [Azure-Befehlszeilenschnittstelle](private-dns-getstarted-cli.md) (Azure CLI) verwenden.

## <a name="create-a-private-dns-zone"></a>Erstellen einer privaten DNS-Zone

Im folgenden Beispiel wird in der Ressourcengruppe **MyAzureResourceGroup** eine DNS-Zone mit dem Namen **contoso.com** erstellt.

Eine DNS-Zone enthält die DNS-Einträge für eine Domäne. Wenn Sie eine Domäne in Azure DNS hosten möchten, erstellen Sie eine DNS-Zone für diesen Domänennamen.

![Suche von privaten DNS-Zonen](media/private-dns-portal/search-private-dns.png)

1. Geben Sie in der Suchleiste des Portals **private DNS-Zonen** in das Suchfeld ein, und drücken Sie die **EINGABETASTE**.
1. Wählen Sie **Private DNS-Zone** aus.
2. Wählen Sie **Create private dns zone** (Private DNS-Zone erstellen) aus.

1. Geben Sie auf der Seite **Create Private DNS zone** (Private DNS-Zone erstellen) die folgenden Werte ein (bzw. wählen Sie sie aus):

   - **Ressourcengruppe**: Wählen Sie **Neue erstellen** aus, geben Sie *MyAzureResourceGroup* ein, und wählen Sie **OK** aus. Der Name der Ressourcengruppe muss innerhalb des Azure-Abonnements eindeutig sein. 
   -  **Name**: Geben Sie *private.contoso.com* für dieses Beispiel ein.
1. Wählen Sie als **Ressourcengruppenstandort** die Option **USA, Westen-Mitte** aus.

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Klicken Sie auf **Erstellen**.

Die Erstellung der Zone kann einige Minuten dauern.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Wählen Sie links oben auf der Portalseite die Option **Ressource erstellen**, anschließend **Netzwerk** und dann **Virtuelles Netzwerk** aus.
2. Geben Sie in **Name** den Namen **myAzureVNet** ein.
3. Wählen Sie für **Ressourcengruppe** den Wert **MyAzureResourceGroup** aus.
4. Wählen Sie **USA, Westen-Mitte** als **Standort** aus.
5. Übernehmen Sie die anderen Standardwerte, und wählen Sie **Erstellen** aus.

## <a name="link-the-virtual-network"></a>Verknüpfen des virtuellen Networks

Um die private DNS-Zone mit einem virtuellen Netzwerk zu verknüpfen, erstellen Sie eine virtuelle Netzwerkverknüpfung.

![Hinzufügen einer virtuellen Netzwerkverknüpfung](media/private-dns-portal/dns-add-virtual-network-link.png)

1. Öffnen Sie die Ressourcengruppe **MyAzureResourceGroup**, und wählen Sie die private Zone **private.contoso.com** aus.
2. Wählen Sie im linken Bereich die Option **Virtual network links** (Virtuelle Netzwerkverknüpfungen) aus.
3. Wählen Sie **Hinzufügen**.
4. Geben Sie **myLink** für den **Linknamen** ein.
5. Wählen Sie unter **Virtuelles Netzwerk** die Option **myAzureVNet** aus.
6. Aktivieren Sie das Kontrollkästchen **Enable auto registration** (Automatische Registrierung aktivieren).
7. Klicken Sie auf **OK**.

## <a name="create-the-test-virtual-machines"></a>Erstellen der virtuellen Testcomputer

Erstellen Sie nun zwei virtuelle Computer, um die private DNS-Zone zu testen:

1. Wählen Sie links oben im Portal die Option **Ressource erstellen** und dann **Windows Server 2016 Datacenter** aus.
1. Wählen Sie **MyAzureResourceGroup** als Ressourcengruppe aus.
1. Geben Sie **myVM01** als Namen des virtuellen Computers ein.
1. Wählen Sie **USA, Westen-Mitte** für die **Region** aus.
1. Geben Sie **azureadmin** als Namen des Administratorbenutzers ein.
2. Geben Sie **Azure12345678** als Kennwort ein, und bestätigen Sie das Kennwort.

5. Wählen Sie für **Öffentliche Eingangsports** die Option **Ausgewählte Ports zulassen** und dann für **Eingangsports auswählen** die Option **RDP (3389)** aus.
10. Übernehmen Sie die anderen Standardwerte auf der Seite, und klicken Sie auf **Weiter: Datenträger >** .
11. Übernehmen Sie die Standardwerte auf der Seite **Datenträger**, und klicken Sie dann auf **Weiter: Netzwerk >** .
1. Stellen Sie sicher, dass **myAzureVNet** als virtuelles Netzwerk ausgewählt ist.
1. Übernehmen Sie die anderen Standardwerte auf der Seite, und klicken Sie auf **Weiter: Verwaltung >** .
2. Wählen Sie unter **Startdiagnose** die Option **Aus** aus, übernehmen Sie die anderen Standardwerte, und wählen Sie dann **Überprüfen + erstellen** aus.
1. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Erstellen**.

Wiederholen Sie diese Schritte, um einen weiteren virtuellen Computer mit dem Namen **myVM02** zu erstellen.

Die Erstellung der beiden virtuellen Computer dauert einige Minuten.

## <a name="create-an-additional-dns-record"></a>Erstellen eines zusätzlichen DNS-Eintrags

 Im folgenden Beispiel wird ein Eintrag mit dem relativen Namen **db** in der DNS-Zone **private.contoso.com** in der Ressourcengruppe **MyAzureResourceGroup** erstellt. Der vollqualifizierte Name des Ressourceneintragssatzes lautet **db.private.contoso.com**. Der Eintragstyp lautet „A“ mit der IP-Adresse von **myVM01**.

1. Öffnen Sie die Ressourcengruppe **MyAzureResourceGroup**, und wählen Sie die private Zone **private.contoso.com** aus.
2. Wählen Sie **+ Ressourceneintragssatz**.
3. Geben Sie für **Name** den Wert **db** ein.
4. Geben Sie unter **IP-Adresse** die IP-Adresse ein, die für **myVM01** angezeigt wird. Diese sollte beim Start des virtuellen Computers automatisch registriert worden sein.
5. Klicken Sie auf **OK**.

## <a name="test-the-private-zone"></a>Testen der privaten Zone

Nun können Sie die Namensauflösung für die private Zone **private.contoso.com** testen.

### <a name="configure-vms-to-allow-inbound-icmp"></a>Konfigurieren von virtuellen Computern zum Zulassen eingehender ICMP

Sie können den Ping-Befehl zum Testen der Namensauflösung verwenden. Konfigurieren Sie daher die Firewall auf beiden virtuellen Computern, um eingehende ICMP-Pakete zuzulassen.

1. Stellen Sie eine Verbindung mit „myVM01“ her, und öffnen Sie ein Windows PowerShell-Fenster mit Administratorrechten.
2. Führen Sie den folgenden Befehl aus:

   ```powershell
   New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
   ```

Wiederholen Sie den Schritt für „myVM02“.

### <a name="ping-the-vms-by-name"></a>Pingen der virtuellen Computer anhand des Namens

1. Führen Sie an der Windows PowerShell-Eingabeaufforderung von „myVM02“ einen Ping-Befehl für „myVM01“ aus. Verwenden Sie dazu den automatisch registrierten Hostnamen:
   ```
   ping myVM01.private.contoso.com
   ```
   Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:
   ```
   PS C:\> ping myvm01.private.contoso.com

   Pinging myvm01.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```
2. Pingen Sie jetzt den Namen **db**, den Sie zuvor erstellt haben:
   ```
   ping db.private.contoso.com
   ```
   Eine ähnliche Ausgabe wie die folgende sollte angezeigt werden:
   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Löschen aller Ressourcen

Löschen Sie die Ressourcengruppe **MyAzureResourceGroup**, um die in diesem Schnellstart erstellten Ressourcen zu löschen, wenn Sie sie nicht mehr benötigen.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Azure DNS Private Zones-Szenarien](private-dns-scenarios.md)

