---
title: 'Tutorial: Vornehmen eines Lastausgleichs für Internetdatenverkehr virtueller Computer – Azure-Portal'
titleSuffix: Azure Load Balancer
description: In diesem Tutorial wird gezeigt, wie Sie über das Azure-Portal eine Load Balancer Standard-Instanz erstellen und verwalten.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create and Standard Load Balancer so that I can load balance internet traffic to VMs and add and remove VMs from the load-balanced set.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/11/2019
ms.author: allensu
ms.custom: seodec18
ms.openlocfilehash: 4d4703ccb4ee96eb69a780f91eae1eb6da9e1578
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225180"
---
# <a name="tutorial-load-balance-internet-traffic-to-vms-using-the-azure-portal"></a>Tutorial: Vornehmen eines Lastausgleichs für den Internetdatenverkehr virtueller Computer mit dem Azure-Portal

Durch die Verteilung der eingehenden Anforderungen auf mehrere virtuelle Computer bietet ein Lastenausgleich ein höheres Maß an Verfügbarkeit und Skalierbarkeit. In diesem Tutorial lernen Sie die verschiedenen Komponenten von Azure Load Balancer Standard kennen, mit denen Internetdatenverkehr an virtuelle Computer verteilt und Hochverfügbarkeit bereitgestellt wird. Folgendes wird vermittelt:


> [!div class="checklist"]
> * Erstellen einer Azure Load Balancer-Instanz
> * Erstellen von Load Balancer-Ressourcen
> * Erstellen von virtuellen Computern und Installieren des IIS-Servers
> * Betrachten des Load Balancers in Aktion
> * Hinzufügen und Entfernen von virtuellen Computern zu bzw. aus eine Load Balancer

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="create-a-standard-load-balancer"></a>Einrichten eines Load Balancers im Tarif „Standard“

In diesem Abschnitt erstellen Sie eine Load Balancer Standard-Instanz für den Lastenausgleich virtueller Computer. Ein Load Balancer im Standard-Tarif unterstützt nur eine öffentliche Standard-IP-Adresse. Wenn Sie einen Load Balancer im Standard-Tarif (Load Balancer Standard) erstellen, müssen Sie dafür auch eine neue öffentliche Standard-IP-Adresse erstellen, die als Front-End konfiguriert ist. Dieses hat standardmäßig den Namen *LoadBalancerFrontend*. 

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Load Balancer**.
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

3. Klicken Sie auf der Registerkarte **Überprüfen + erstellen** auf **Erstellen**.

   ![Einrichten eines Load Balancers im Tarif „Standard“](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Erstellen von Load Balancer-Ressourcen

In diesem Abschnitt konfigurieren Sie Load Balancer-Einstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem legen Sie eine Lastenausgleichsregel fest.

### <a name="create-a-backend-address-pool"></a>Erstellen eines Back-End-Adresspools

Ein Back-End-Adresspool enthält die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind, um Datenverkehr an die virtuellen Computer verteilen zu können. Erstellen Sie den Back-End-Adresspool *myBackendPool*, um virtuelle Computer für den Lastenausgleich von Internetdatenverkehr einzubeziehen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** aus, und klicken Sie anschließend in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Back-End-Pools** und anschließend auf **Hinzufügen**.
3. Geben Sie auf der Seite **Back-End-Pool hinzufügen** die Zeichenfolge *myBackendPool* als Name für Ihren Back-End-Pool ein, und wählen Sie anschließend **Hinzufügen** aus.

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Damit der Load Balancer den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Erstellen Sie zur Überwachung der Integrität der virtuellen Computer einen Integritätstest namens *myHealthProbe*.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** aus, und klicken Sie anschließend in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Integritätstests** und anschließend auf **Hinzufügen**.
3. Verwenden Sie folgende Werte, um den Integritätstest zu erstellen:
     
    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *myHealthProbe* ein. |
    | Protocol | Wählen Sie **HTTP** aus. |
    | Port | Geben Sie *80* ein.|
    | Intervall | Geben Sie für das **Intervall** den Wert *15* (Sekunden zwischen Testversuchen) ein. |
    | Fehlerhafter Schwellenwert | Wählen Sie *2* als Wert für den **Fehlerschwellenwert** bzw. als Anzahl aufeinander folgender Testfehler aus, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird.|
    
4. Klicken Sie auf **OK**.

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport. Erstellen Sie eine Load Balancer-Regel namens *myLoadBalancerRuleWeb*, die am Port 80 des Front-Ends *FrontendLoadBalancer* lauscht und den Netzwerkdatenverkehr nach erfolgtem Lastenausgleich an den Back-End-Adresspool *myBackEndPool* sendet, wobei ebenfalls der Port 80 verwendet wird.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** aus, und klicken Sie anschließend in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Lastenausgleichsregeln** und anschließend auf **Hinzufügen**.
3. Konfigurieren Sie die Lastenausgleichsregel mit folgenden Werten:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *myHTTPRule* ein. |
    | Protocol | Wählen Sie **TCP** aus. |
    | Port | Geben Sie *80* ein.|
    | Back-End-Port | Geben Sie *80* ein. |
    | Back-End-Pool | Wählen Sie *myBackendPool* aus.|
    | Integritätstest | Wählen Sie *myHealthProbe* aus. |
    
4. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **OK**.

## <a name="create-backend-servers"></a>Erstellen von Back-End-Servern

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk sowie drei virtuelle Computer für den Back-End-Pool des Load Balancers und installieren anschließend IIS auf den virtuellen Computern, um den Load Balancer zu testen.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.
2. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *myVNet* ein. |
    | Adressraum | Geben Sie *10.1.0.0/16* ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie die vorhandene Ressource *myResourceGroupSLB* aus. |
    | Location | Wählen Sie **Europa, Westen** aus.|
    | Subnetzname | Geben Sie *myBackendSubnet* ein. |
    | Subnetzadressbereich | Geben Sie *10.1.0.0/24* ein. |
    
3. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Load Balancer Standard unterstützt nur virtuelle Computer mit Standard-IP-Adressen im Back-End-Pool. In diesem Abschnitt werden drei virtuelle Computer (*myVM1*, *myVM2* und *myVM3*) mit einer öffentlichen Standard-IP-Adresse in drei verschiedenen Zonen (*Zone 1*, *Zone 2* und *Zone 3*) erstellt, die dem Back-End-Pool der zuvor erstellten Load Balancer Standard-Instanz hinzugefügt werden.

1. Wählen Sie oben links im Portal die Option **Ressource erstellen** > **Compute** > **Windows Server 2016 Datacenter**. 
   
1. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   - **Abonnement** > **Ressourcengruppe**: Wählen Sie **myResourceGroupSLB** aus.
   - **Instanzendetails** > **Name des virtuellen Computers**: Geben Sie *myVM1* ein.
   - **Instanzendetails** > **Region**: Wählen Sie **Europa, Westen** aus.
   - **Instanzendetails** > **Verfügbarkeitsoptionen**: Wählen Sie **Verfügbarkeitszonen** aus. 
   - **Instanzendetails** > **Verfügbarkeitszone**: Wählen Sie **1** aus.
  
1. Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus. 
   
   - Stellen Sie sicher, dass Folgendes ausgewählt ist:
       - **Virtuelles Netzwerk:** **myVnet**
       - **Subnetz**: **myBackendSubnet**
       - **Öffentliche IP-Adresse**: Wählen Sie **Neu erstellen** und anschließend im Fenster **Öffentliche IP-Adresse erstellen** unter **SKU** die Option **Standard** und unter **Verfügbarkeitszone** die Option **Zonenredundant** aus.
      
   - Wählen Sie zum Erstellen einer neuen Netzwerksicherheitsgruppe (NSG) – einer Art Firewall – unter **Netzwerksicherheitsgruppe** die Option **Erweitert**. 
       1. Wählen Sie im Feld **Netzwerksicherheitsgruppe konfigurieren** die Option **Neu erstellen**. 
       1. Geben Sie *myNetworkSecurityGroup* ein, und wählen Sie **OK** aus.

   - Führen Sie die folgenden Schritte aus, um den virtuellen Computer dem Back-End-Pool des Load Balancers hinzuzufügen:
        - Wählen Sie unter **Lastenausgleich** für **Diese VM hinter einer vorhandenen Lastenausgleichslösung platzieren?** die Option **Ja** aus.
        - Wählen Sie in den **Lastenausgleichseinstellungen** unter **Optionen für den Lastenausgleich** die Option **Azure Load Balancer** aus.
        - Wählen Sie unter **Load Balancer auswählen** die Option *myLoadBalancer* aus. 
1. Wählen Sie die Registerkarte **Verwaltung** oder **Weiter** > **Verwaltung**. Legen Sie unter **Überwachung** die Option **Startdiagnose** auf **Aus** fest. 
1. Klicken Sie auf **Überprüfen + erstellen**.   
1. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**.
1. Erstellen Sie mithilfe der entsprechenden Schritte zwei weitere virtuelle Computer (*myVM2* und *myVM3*) mit einer öffentlichen IP-Adresse für eine Standard-SKU in der **Verfügbarkeitszone** **2** bzw. **3**. (Verwenden Sie ansonsten die gleichen Einstellungen wie für *myVM1*.)  

### <a name="create-network-security-group-rule"></a>Erstellen einer Netzwerksicherheitsgruppen-Regel

In diesem Abschnitt erstellen Sie eine Netzwerksicherheitsgruppen-Regel, um eingehende Verbindungen über HTTP zuzulassen.

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** aus, und klicken Sie anschließend in der Ressourcenliste auf **myNetworkSecurityGroup** (in der Ressourcengruppe **myResourceGroupSLB**).
2. Klicken Sie unter **Einstellungen** auf **Eingangssicherheitsregeln** und anschließend auf **Hinzufügen**.
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

### <a name="install-iis-on-vms"></a>Installieren von IIS auf VMs

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** aus, und klicken Sie anschließend in der Ressourcenliste auf **myVM1** (in der Ressourcengruppe *myResourceGroupSLB*).
2. Klicken Sie auf der Seite **Übersicht** auf **Verbinden**, um eine RDP-Verbindung mit dem virtuellen Computer herzustellen.
3. Wählen Sie im Fenster **Herstellen der Verbindung mit dem virtuellen Computer** die Option **RDP-Datei herunterladen** aus, und öffnen Sie dann die heruntergeladene RDP-Datei.
4. Klicken Sie im Fenster **Remotedesktopverbindung** auf **Verbinden**.
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
1. Ermitteln Sie auf dem Bildschirm **Übersicht** die öffentliche IP-Adresse für den Load Balancer. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** aus, und klicken Sie anschließend auf **myPublicIP**.

2. Kopieren Sie die öffentliche IP-Adresse, und fügen Sie sie in die Adressleiste des Browsers ein. Die Standardseite des IIS-Webservers wird im Browser angezeigt.

      ![IIS-Webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Sie können eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Load Balancer den Datenverkehr auf die drei virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird.

## <a name="remove-or-add-vms-from-the-backend-pool"></a>Entfernen oder Hinzufügen von virtuellen Computern aus dem Back-End-Pool
Für die virtuellen Computer, auf denen Ihre App ausgeführt wird, sind unter Umständen gelegentlich Wartungsarbeiten erforderlich (etwa die Installation von Betriebssystemupdates). Zur Bewältigung eines höheren Datenverkehrsaufkommens für Ihre App müssen gegebenenfalls weitere virtuelle Computer hinzugefügt werden. In diesem Abschnitt erfahren Sie, wie Sie einen virtuellen Computer (*myVM1*) aus dem Load Balancer entfernen oder dem Load Balancer einen virtuellen Computer hinzufügen.

### <a name="remove-vm-from-a-backend-pool"></a>Entfernen eines virtuellen Computers aus einem Back-End-Pool
Gehen Sie wie folgt vor, um *myVM1* aus dem Back-End-Pool zu entfernen:

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** aus, und klicken Sie anschließend in der Ressourcenliste auf **myLoadBalancer**.
2. Klicken Sie unter **Einstellungen** auf **Back-End-Pools** und dann in der Liste des Back-End-Pools auf **myBackendPool**.
3. Wählen Sie auf der Seite **myBackendPool** am Ende der Zeile für *myVM1* das Löschsymbol aus, und klicken Sie anschließend auf **Speichern**, um *VM1* zu entfernen.

Wenn *myVM1* nicht mehr im Back-End-Adresspool enthalten ist, können Sie alle Wartungsaufgaben auf *myVM1* durchführen, z.B. die Installation von Softwareupdates. In Abwesenheit von *VM1* wird die Last nun auf *myVM2* und *myVM3* verteilt. 

### <a name="add-vm-to-a-backend-pool"></a>Hinzufügen eines virtuellen Computers zu einem Back-End-Pool
Gehen Sie wie folgt vor, um *myVM1* wieder dem Back-End-Pool hinzuzufügen:

1. Wählen Sie im linken Menü **Alle Dienste** > **Alle Ressourcen** und anschließend in der Ressourcenliste die Option **myVM1** aus.
2. Wählen Sie auf der Seite **VM1** unter **Einstellungen** die Option **Netzwerk** aus.
3. Wählen Sie auf der Seite **Netzwerk** die Registerkarte **Lastenausgleich** und anschließend die Option **Lastenausgleich hinzufügen** aus.
4. Gehen Sie auf der Seite **Lastenausgleich hinzufügen** wie folgt vor:
   1. Wählen Sie unter **Optionen für den Lastenausgleich** die Option **Azure Load Balancer** aus.
   2. Wählen Sie unter **Load Balancer auswählen** die Option *myLoadBalancer* aus.
   3. Wählen Sie unter **Back-End-Pool auswählen** die Option *myBackendPool* aus. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, den Load Balancer und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe *myResouceGroupSLB* aus, die den Load Balancer enthält, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Load Balancer Standard-Instanz erstellt, virtuelle Computer hinzugefügt, die Datenverkehrsregel für den Load Balancer sowie einen Integritätstest konfiguriert und den Load Balancer getestet. Außerdem haben Sie eine VM aus der Gruppe mit Lastenausgleich entfernt und sie wieder dem Back-End-Adresspool hinzugefügt. Weitere Informationen zu Azure Load Balancer finden Sie in den Tutorials zu Azure Load Balancer.

> [!div class="nextstepaction"]
> [Azure Load Balancer-Tutorials](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
