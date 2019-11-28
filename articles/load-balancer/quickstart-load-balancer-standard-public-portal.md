---
title: 'Schnellstart: Erstellen einer Load Balancer Standard-Instanz – Azure-Portal'
titleSuffix: Azure Load Balancer
description: In diesem Schnellstart wird gezeigt, wie Sie über das Azure-Portal eine Load Balancer Standard-Instanz erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create a Standard Load Balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/11/2019
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: d15223dfe6d9ce710f2a3d402a49203ef169132e
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225207"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Schnellstart: Erstellen eines Load Balancers im Tarif „Standard“ für den Lastenausgleich virtueller Computer über das Azure-Portal

Durch die Verteilung der eingehenden Anforderungen auf mehrere virtuelle Computer bietet ein Lastenausgleich ein höheres Maß an Verfügbarkeit und Skalierbarkeit. Sie können das Azure-Portal verwenden, um einen Load Balancer für den Lastenausgleich virtueller Computer (Virtual Machines, VMs) zu erstellen. In dieser Schnellstartanleitung erfahren Sie, wie Sie für den Lastenausgleich virtueller Computer einen Load Balancer im Tarif „Standard“ verwenden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-standard-load-balancer"></a>Einrichten eines Load Balancers im Tarif „Standard“

In diesem Abschnitt erstellen Sie eine Load Balancer Standard-Instanz für den Lastenausgleich virtueller Computer. Ein Load Balancer im Standard-Tarif unterstützt nur eine öffentliche Standard-IP-Adresse. Wenn Sie einen Load Balancer im Standard-Tarif erstellen, müssen Sie für diesen auch eine neue öffentliche Standard-IP-Adresse erstellen, die als Front-End konfiguriert ist. Dieses hat standardmäßig den Namen *LoadBalancerFrontend*. 

1. Wählens Sie links oben auf dem Bildschirm die Optionen **Ressource erstellen** > **Netzwerk** > **Lastenausgleich** aus.
2. Geben Sie auf der Seite **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Überprüfen + erstellen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Resource group         | Wählen Sie **Neu erstellen** aus, und geben Sie *myResourceGroupSLB* in das Textfeld ein.|
    | NAME                   | *myLoadBalancer*                                   |
    | Region         | Wählen Sie **Europa, Westen** aus.                                        |
    | type          | Wählen Sie **Öffentlich** aus.                                        |
    | SKU           | Wählen Sie **Standard** aus.                          |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. |
    | Name der öffentlichen IP-Adresse              | Geben Sie *myPublicIP* in das Textfeld ein.   |
    |Verfügbarkeitszone| Wählen Sie **Zonenredundant** aus.    |
3. Wählen Sie auf der Registerkarte **Bewerten + erstellen** die Option **Erstellen** aus.   

    ![Einrichten eines Load Balancers im Tarif „Standard“](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Erstellen von Load Balancer-Ressourcen

In diesem Abschnitt konfigurieren Sie Load Balancer-Einstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem legen Sie eine Lastenausgleichsregel fest.

### <a name="create-a-backend-address-pool"></a>Erstellen eines Back-End-Adresspools

Ein Back-End-Adresspool enthält die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind, um Datenverkehr an die virtuellen Computer verteilen zu können. Erstellen Sie den Back-End-Adresspool *myBackendPool*, um virtuelle Computer für den Lastenausgleich von Internetdatenverkehr einzubeziehen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.
2. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** und dann **Hinzufügen** aus.
3. Geben Sie auf der Seite **Back-End-Pool hinzufügen** die Zeichenfolge *myBackendPool* als Name für Ihren Back-End-Pool ein, und wählen Sie anschließend **Hinzufügen** aus.

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Damit der Load Balancer den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Erstellen Sie zur Überwachung der Integrität der virtuellen Computer einen Integritätstest namens *myHealthProbe*.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.
2. Wählen Sie unter **Einstellungen** die Option **Integritätstests** und dann **Hinzufügen** aus.
    
    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *myHealthProbe* ein. |
    | Protocol | Wählen Sie **HTTP** aus. |
    | Port | Geben Sie *80* ein.|
    | Intervall | Geben Sie für das **Intervall** den Wert *15* (Sekunden zwischen Testversuchen) ein. |
    | Fehlerhafter Schwellenwert | Wählen Sie **2** als Wert für den **Fehlerschwellenwert** bzw. als Anzahl aufeinander folgender Testfehler aus, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird.|
    | | |
4. Klicken Sie auf **OK**.

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel
Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport. Erstellen Sie eine Load Balancer-Regel namens *myLoadBalancerRuleWeb*, die am Port 80 des Front-Ends *FrontendLoadBalancer* lauscht und den Netzwerkdatenverkehr nach erfolgtem Lastenausgleich an den Back-End-Adresspool *myBackEndPool* sendet, wobei ebenfalls der Port 80 verwendet wird. 

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myLoadBalancer** aus.
2. Wählen Sie unter **Einstellungen** die Option **Lastenausgleichsregeln** und dann **Hinzufügen** aus.
3. Konfigurieren Sie die Lastenausgleichsregel mit folgenden Werten:
    
    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *myHTTPRule* ein. |
    | Protocol | Wählen Sie **TCP** aus. |
    | Port | Geben Sie *80* ein.|
    | Back-End-Port | Geben Sie *80* ein. |
    | Back-End-Pool | Wählen Sie *myBackendPool* aus.|
    | Integritätstest | Wählen Sie *myHealthProbe* aus. |
4. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie dann **OK** aus.


## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk sowie drei virtuelle Computer für den Back-End-Pool des Load Balancers und installieren anschließend IIS auf den virtuellen Computern, um den Load Balancer zu testen.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.

1. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *myVNet* ein. |
    | Adressraum | Geben Sie *10.1.0.0/16* ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie die vorhandene Ressource *myResourceGroupSLB* aus. |
    | Location | Wählen Sie **Europa, Westen** aus.|
    | Subnetzname | Geben Sie *myBackendSubnet* ein. |
    | Subnetzadressbereich | Geben Sie *10.1.0.0/24* ein. |
1. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern
Load Balancer Standard unterstützt nur virtuelle Computer mit Standard-IP-Adressen im Back-End-Pool. In diesem Abschnitt werden drei virtuelle Computer (*myVM1*, *myVM2* und *myVM3*) mit einer öffentlichen Standard-IP-Adresse in drei verschiedenen Zonen (*Zone 1*, *Zone 2* und *Zone 3*) erstellt, die später dem Back-End-Pool der zuvor erstellten Load Balancer Standard-Instanz hinzugefügt werden.

1. Wählen Sie oben links im Portal die Option **Ressource erstellen** > **Compute** > **Windows Server 2019 Datacenter**. 
   
1. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   - **Abonnement** > **Ressourcengruppe**: Wählen Sie **myResourceGroupSLB** aus.
   - **Instanzendetails** > **Name des virtuellen Computers**: Geben Sie *myVM1* ein.
   - **Instanzendetails** > **Region**: Wählen Sie **Europa, Westen** aus.
   - **Instanzendetails** > **Verfügbarkeitsoptionen**: Wählen Sie **Verfügbarkeitszonen** aus. 
   - **Instanzendetails** > **Verfügbarkeitszone**: Wählen Sie **1** aus.
   - **Administratorkonto**> Geben Sie Informationen für **Benutzernamen**, **Kennwort** und **Kennwort bestätigen** ein.
   - Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus.
  
1. Stellen Sie auf der Registerkarte **Netzwerk** sicher, dass Folgendes ausgewählt ist:
   - **Virtuelles Netzwerk:** *myVnet*
   - **Subnetz:** *myBackendSubnet*
   - **Öffentliche IP-Adresse:** Wählen Sie **Neu erstellen** und anschließend im Fenster **Öffentliche IP-Adresse erstellen** unter **SKU** die Option **Standard** und unter **Verfügbarkeitszone** die Option **Zonenredundant** und dann **OK** aus.
   - Wählen Sie zum Erstellen einer neuen Netzwerksicherheitsgruppe (NSG) – einer Art Firewall – unter **Netzwerksicherheitsgruppe** die Option **Erweitert**. 
       1. Wählen Sie im Feld **Netzwerksicherheitsgruppe konfigurieren** die Option **Neu erstellen**. 
       1. Geben Sie *myNetworkSecurityGroup* ein, und wählen Sie **OK** aus.
   - Führen Sie die folgenden Schritte aus, um den virtuellen Computer dem Back-End-Pool des Load Balancers hinzuzufügen:
        - Wählen Sie unter **Lastenausgleich** für **Diese VM hinter einer vorhandenen Lastenausgleichslösung platzieren?** die Option **Ja** aus.
        - Wählen Sie in den **Lastenausgleichseinstellungen** unter **Optionen für den Lastenausgleich** die Option **Azure Load Balancer** aus.
        - Wählen Sie unter **Load Balancer auswählen** die Option *myLoadBalancer* aus.
        - Wählen Sie die Registerkarte **Verwaltung** oder **Weiter** > **Verwaltung**.
2. Legen Sie auf der Registerkarte **Verwaltung** unter **Überwachung** die Option **Startdiagnose** auf **Aus** fest. 
1. Klicken Sie auf **Überprüfen + erstellen**.   
1. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.
1. Führen Sie die Schritte 2 bis 6 aus, um zwei zusätzliche VMs mit folgenden Werten zu erstellen. Verwenden Sie für alle übrigen Einstellungen die Werte von *myVM1*:

    | Einstellung | VM 2| VM 3|
    | ------- | ----- |---|
    | NAME |  *myVM2* |*myVM3*|
    | Verfügbarkeitszone | 2 |3|
    |Öffentliche IP-Adresse| **Standard**-SKU:|**Standard**-SKU:|
    | Öffentliche IP-Adresse – Verfügbarkeitszone| **Zonenredundant** |**Zonenredundant**|
    | Netzwerksicherheitsgruppe | Wählen Sie die vorhandene *myNetworkSecurity-Gruppe* aus.| Wählen Sie die vorhandene *myNetworkSecurity-Gruppe* aus.|

 ### <a name="create-nsg-rule"></a>Erstellen der NSG-Regel

In diesem Abschnitt erstellen Sie eine Netzwerksicherheitsgruppen-Regel, um eingehende Verbindungen über HTTP zuzulassen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myNetworkSecurityGroup** (in der Ressourcengruppe **myResourceGroupSLB**) aus.
2. Wählen Sie unter **Einstellungen** die Option **Eingangssicherheitsregeln** und dann **Hinzufügen**.
3. Geben Sie für die Eingangssicherheitsregel *myHTTPRule* folgende Werte ein, um eine eingehende HTTP-Verbindung über den Port 80 zuzulassen:
    - *Service Tag* für **Quelle**
    - *Internet* für **Quelldiensttag**
    - *80* für **Zielportbereiche**
    - *TCP* für **Protokoll**
    - *Zulassen* für **Aktion**
    - *100* für **Priorität**
    - *myHTTPRule* als Name
    - *Allow HTTP* als Beschreibung
4. Wählen Sie **Hinzufügen**.
 
### <a name="install-iis"></a>Installieren von IIS

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste den Eintrag **myVM1** (in der Ressourcengruppe *myResourceGroupSLB*) aus.
2. Wählen Sie auf der Seite **Übersicht** die Option **Verbinden**, um eine RDP-Verbindung mit dem virtuellen Computer herzustellen.
5. Melden Sie sich mit den Anmeldeinformationen, die Sie während der Erstellung dieses virtuellen Computers bereitgestellt haben, beim virtuellen Computer an. Dadurch wird eine Remotedesktopsitzung mit dem virtuellen Computer *myVM1* gestartet.
6. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme**>**Windows PowerShell**.
7. Führen Sie im PowerShell-Fenster die folgenden Befehle aus, um den IIS-Server zu installieren, die Standarddatei „iisstart.htm“ zu entfernen und dann eine neue Datei „iisstart.htm“ hinzuzufügen, die den Namen des virtuellen Computers enthält:

   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
6. Schließen Sie die RDP-Sitzung mit *myVM1*.
7. Wiederholen Sie die Schritte 1 bis 6, um IIS und die aktualisierte Datei „iisstart.htm“ auf *myVM2* und *myVM3* zu installieren.

## <a name="test-the-load-balancer"></a>Testen des Load Balancers
1. Ermitteln Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für den Load Balancer. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend **myPublicIP** aus.

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

   ![IIS-Webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Um den verteilten Load Balancer-Datenverkehr auf allen drei virtuellen Computern anzuzeigen, können Sie die Standardseite des IIS-Webservers einzelner virtueller Computer anpassen und dann die Aktualisierung Ihres Webbrowsers auf dem Clientcomputer durchsetzen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Load Balancer und alle zugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe (*myResourceGroupSLB*) aus, die den Load Balancer enthält, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine Load Balancer Standard-Instanz erstellt, virtuelle Computer angefügt, die Datenverkehrsregel für den Load Balancer sowie einen Integritätstest konfiguriert und den Load Balancer getestet. Weitere Informationen zu Azure Load Balancer finden Sie in den Tutorials zu Azure Load Balancer.

> [!div class="nextstepaction"]
> [Azure Load Balancer-Tutorials](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
