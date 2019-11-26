---
title: 'Tutorial: Konfigurieren der Portweiterleitung: Azure-Portal'
titleSuffix: Azure Load Balancer
description: In diesem Tutorial wird gezeigt, wie Sie die Portweiterleitung per Azure Load Balancer konfigurieren, um in einem virtuellen Azure-Netzwerk eine Verbindung mit VMs zu erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: As an IT administrator, I want to configure port forwarding in Azure Load Balancer to remotely connect to VMs in an Azure virtual network.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: allensu
ms.custom: seodec18
ms.openlocfilehash: 6dda01543a6a7f447adefcc6cc3cfa3ea5da5492
ms.sourcegitcommit: b1a8f3ab79c605684336c6e9a45ef2334200844b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74048848"
---
# <a name="tutorial-configure-port-forwarding-in-azure-load-balancer-using-the-portal"></a>Tutorial: Konfigurieren der Portweiterleitung im Azure Load Balancer mit dem Portal

Mit der Portweiterleitung können Sie die Verbindung mit virtuellen Computern (VMs) in einem virtuellen Azure-Netzwerk mithilfe einer öffentlichen IP-Adresse und Portnummer von Azure Load Balancer herstellen. 

In diesem Tutorial richten Sie die Portweiterleitung auf einer Azure Load Balancer-Instanz ein. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Erstellen eines öffentlichen Load Balancers im Tarif „Standard“ für den Netzwerkdatenverkehr über virtuelle Computer. 
> * Erstellen eines virtuellen Netzwerks und virtueller Computer mit einer Netzwerksicherheitsgruppen-Regel (NSG). 
> * Hinzufügen der VMs zum Back-End-Adresspool des Lastenausgleichs.
> * Erstellen eines Integritätstests und von Datenverkehrsregeln für den Lastenausgleich.
> * Erstellen von NAT-Eingangsregeln für die Portweiterleitung für den Lastenausgleich.
> * Installieren und Konfigurieren von IIS auf den virtuellen Computern zum Anzeigen von Lastenausgleich und Portweiterleitung in Aktion.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

Melden Sie sich für alle Schritte in diesem Tutorial im Azure-Portal unter [https://portal.azure.com](https://portal.azure.com) an.

## <a name="create-a-standard-load-balancer"></a>Erstellen eines Load Balancers im Tarif „Standard“

Erstellen Sie zunächst einen öffentlichen Load Balancer im Tarif „Standard“, der den Datenverkehr über virtuelle Computer ausgleichen kann. Ein Load Balancer im Tarif „Standard“ unterstützt nur eine öffentliche Standard-IP-Adresse. Wenn Sie einen Load Balancer im Tarif „Standard“ erstellen, müssen Sie dafür auch eine neue öffentliche Standard-IP-Adresse erstellen, die als Lastenausgleichs-Front-End konfiguriert und standardmäßig als **LoadBalancerFrontend** benannt ist. 

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Load Balancer**.
2. Geben Sie auf der Seite **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Überprüfen + erstellen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Resource group         | Wählen Sie **Neu erstellen**, und geben Sie *MyResourceGroupLB* in das Textfeld ein.|
    | NAME                   | *myLoadBalancer*                                   |
    | Region         | Wählen Sie **Europa, Westen** aus.                                        |
    | type          | Wählen Sie **Öffentlich** aus.                                        |
    | SKU           | Wählen Sie **Standard** aus.                          |
    | Öffentliche IP-Adresse | Wählen Sie **Neu erstellen**. |
    | Name der öffentlichen IP-Adresse              | Geben Sie *myPublicIP* in das Textfeld ein.   |
    |Verfügbarkeitszone| Wählen Sie **Zonenredundant** aus.    |
     
    >[!NOTE]
     >Stellen Sie sicher, dass Sie Ihren Load Balancer und alle seine Ressourcen an einem Standort erstellen, der Verfügbarkeitszonen unterstützt. Weitere Informationen siehe [Regionen, die Verfügbarkeitszonen unterstützen](../availability-zones/az-overview.md#services-support-by-region). 

3. Klicken Sie auf der Registerkarte **Überprüfen + erstellen** auf **Erstellen**.  
  
## <a name="create-and-configure-back-end-servers"></a>Erstellen und Konfigurieren von Back-End-Servern

Erstellen Sie ein virtuelles Netzwerk mit zwei virtuellen Computern, und fügen Sie die VMs dem Back-End-Pool Ihres Load Balancers hinzu. 

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Wählen Sie oben links im Portal **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk**.
   
1. Geben Sie im Bereich **Virtuelles Netzwerk erstellen** diese Werte ein (bzw. wählen Sie sie aus):
   
   - **Name**: Geben Sie *MyVNet* ein.
   - **Ressourcengruppe**: Öffnen Sie die Dropdownliste **Vorhandene auswählen**, und wählen Sie **MyResourceGroupLB** aus. 
   - **Subnetz** > **Name**: Geben Sie *MyBackendSubnet* ein.
   
1. Klicken Sie auf **Erstellen**.

   ![Erstellen eines virtuellen Netzwerks](./media/tutorial-load-balancer-port-forwarding-portal/2-load-balancer-virtual-network.png)

### <a name="create-vms-and-add-them-to-the-load-balancer-back-end-pool"></a>Erstellen von VMs und Hinzufügen zum Back-End-Pool des Lastenausgleichs

1. Wählen Sie oben links im Portal die Option **Ressource erstellen** > **Compute** > **Windows Server 2016 Datacenter**. 
   
1. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   - **Abonnement** > **Ressourcengruppe**: Öffnen Sie die Dropdownliste, und wählen Sie **MyResourceGroupLB** aus.
   - **Name des virtuellen Computers**: Geben Sie *MyVM1* ein.
   - **Region**: Wählen Sie **Europa, Westen** aus. 
   - **Benutzername**: Geben Sie *azureuser* ein.
   - **Kennwort**: Geben Sie *Azure1234567* ein. 
     Geben Sie das Kennwort im Feld **Kennwort bestätigen** noch einmal ein.
   
1. Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus. 
   
   Stellen Sie sicher, dass Folgendes ausgewählt ist:
   - **Virtuelles Netzwerk**: **MyVNet**
   - **Subnetz**: **MyBackendSubnet**
   
1. Wählen Sie auf der Seite **Öffentliche IP-Adresse erstellen** unter **Öffentliche IP-Adresse** **Neue erstellen**, **Standard** aus, und wählen Sie dann **OK** aus. 
   
1. Wählen Sie zum Erstellen einer neuen Netzwerksicherheitsgruppe (NSG) – einer Art Firewall – unter **Netzwerksicherheitsgruppe** die Option **Erweitert** aus. 
   1. Wählen Sie im Feld **Netzwerksicherheitsgruppe konfigurieren** die Option **Neu erstellen**. 
   1. Geben Sie *MyNetworkSecurityGroup* ein, und wählen Sie **OK**. 
   
   >[!NOTE]
   >Beachten Sie, dass die NSG bereits standardmäßig über eine Eingangsregel zum Öffnen von Port 3389 – dem Remote Desktop Port (RDP) – verfügt.
   
1. Fügen Sie den virtuellen Computer einem Lastenausgleichs-Back-End-Pool hinzu, den Sie erstellen:
   
   1. Wählen Sie unter **LASTENAUSGLEICH** > **Diese VM hinter einer vorhandenen Lastenausgleichslösung platzieren?** **Ja** aus. 
   1. Öffnen Sie die Dropdownliste für **Optionen für den Lastenausgleich**, und wählen Sie **Azure Load Balancer** aus. 
   1. Öffnen Sie die Dropdownliste für **Load Balancer auswählen**, und wählen Sie **MyLoadBalancer** aus. 
   1. Wählen Sie unter **Back-End-Pool auswählen** **Neuen erstellen** aus, geben Sie dann *MyBackendPool* ein, und wählen Sie **Erstellen** aus. 
   
   ![Erstellen eines virtuellen Netzwerks](./media/tutorial-load-balancer-port-forwarding-portal/create-vm-networking.png)
   
1. Wählen Sie die Registerkarte **Verwaltung** oder **Weiter** > **Verwaltung**. Legen Sie unter **Überwachung** die Option **Startdiagnose** auf **Aus** fest.
   
1. Klicken Sie auf **Überprüfen + erstellen**.
   
1. Überprüfen Sie die Einstellungen, und wählen Sie bei erfolgreicher Überprüfung **Erstellen** aus. 

1. Führen Sie die Schritte zum Erstellen einer zweiten VM mit dem Namen *MyVM2* aus, und verwenden Sie für alle anderen Einstellungen die Werte wie für MyVM1. 
   
   Öffnen Sie die Dropdownliste für **Netzwerksicherheitsgruppe** nach der Auswahl von **Erweitert**, und wählen Sie die **MyNetworkSecurityGroup** aus, die Sie bereits erstellt haben. 
   
   Stellen Sie sicher, dass unter **Back-End-Pool auswählen** **MyBackendPool** ausgewählt ist. 

### <a name="create-an-nsg-rule-for-the-vms"></a>Erstellen einer NSG-Regel für die VMs

Erstellen Sie eine Netzwerksicherheitsgruppen-Regel (NSGS) für die virtuellen Computer, um eingehende Internetverbindungen (HTTP) zuzulassen.

>[!NOTE]
>Standardmäßig verfügt die NSG bereits über eine Regel zum Öffnen von Port 3389 – dem Remote Desktop Port (RDP).

1. Wählen Sie im linken Menü die Option **Alle Ressourcen**. Wählen Sie in der Ressourcenliste in der Ressourcengruppe **MyResourceGroupLB** die Option **MyNetworkSecurityGroup**.
   
1. Wählen Sie unter **Einstellungen** die Option **Eingangssicherheitsregeln** und dann **Hinzufügen**.
   
1. Geben Sie im Dialogfeld **Eingangssicherheitsregel hinzufügen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   
   - **Quelle**: Wählen Sie **Diensttag** aus.  
   - **Quelldiensttag**: Wählen Sie **Internet** aus. 
   - **Zielportbereiche**: Geben Sie *80* ein.
   - **Protokoll**: Wählen Sie **TCP** aus. 
   - **Aktion**: Wählen Sie **Zulassen** aus.  
   - **Priorität**: Geben Sie *100* ein. 
   - **Name**: Geben Sie *MyHTTPRule* ein. 
   - **Beschreibung:** Geben Sie *Allow HTTP* ein. 
   
1. Wählen Sie **Hinzufügen**. 
   
   ![Erstellen einer NSG-Regel](./media/tutorial-load-balancer-port-forwarding-portal/8-load-balancer-nsg-rules.png)
   
## <a name="create-load-balancer-resources"></a>Erstellen von Load Balancer-Ressourcen

In diesem Abschnitt überprüfen Sie den Load Balancer-Back-End-Pool und konfigurieren einen Integritätstest sowie Datenverkehrsregeln für den Lastenausgleich.

### <a name="view-the-back-end-address-pool"></a>Anzeigen des Back-End-Adresspools

Zum Verteilen von Datenverkehr auf die virtuellen Computer verwendet der Load Balancer einen Back-End-Adresspool, der die IP-Adressen der virtuellen Netzwerkschnittstellen (NICs) enthält, die mit dem Load Balancer verbunden sind. 

Sie haben Ihren Load Balancer-Back-End-Pool erstellt und virtuelle Computer hinzugefügt, als Sie die virtuellen Computer erstellt haben. Sie können auch auf der Load Balancer-Seite **Back-End-Pools** Back-End-Pools erstellen und virtuelle Computer hinzufügen oder entfernen. 

1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die Option **MyLoadBalancer**.
   
1. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools**.
   
1. Erweitern Sie auf der Seite **Back-End-Pools** den Eintrag **MyBackendPool**, und stellen Sie sicher, dass sowohl **VM1** als auch **VM2** aufgeführt ist.

1. Wählen Sie **MyBackendPool** aus. 
   
   Auf der Seite **MyBackendPool** können Sie unter **VIRTUELLER COMPUTER** und **IP-ADRESSE** dem Pool verfügbare VMs hinzufügen oder davon entfernen.

Sie können neue Back-End-Pools durch Auswahl von **Hinzufügen** auf der Seite **Back-End-Pools** erstellen.

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Damit der Load Balancer den VM-Status überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. 

1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die Option **MyLoadBalancer**.
   
1. Wählen Sie unter **Einstellungen** die Option **Integritätstests** und dann **Hinzufügen**.
   
1. Geben Sie auf der Seite **Integritätstest hinzufügen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   
   - **Name**: Geben Sie *MyHealthProbe* ein.
   - **Protokoll**: Öffnen Sie die Dropdownliste, und wählen Sie **HTTP** aus. 
   - **Port**: Geben Sie *80* ein. 
   - **Pfad**: Übernehmen Sie */* als Standard-URI. Sie können diesen Wert durch einen beliebigen anderen URI ersetzen. 
   - **Intervall**: Geben Sie *15* ein. Das Intervall ist die Anzahl von Sekunden zwischen Testversuchen.
   - **Fehlerschwellenwert**: Geben Sie *2* ein. Dieser Wert gibt die Anzahl aufeinander folgender Testfehler an, die auftreten müssen, damit ein virtueller Computer als fehlerhaft eingestuft wird.
   
1. Klicken Sie auf **OK**.
   
   ![Hinzufügen eines Tests](./media/tutorial-load-balancer-port-forwarding-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Mit einer Lastenausgleichsregel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt wird. Die Regel definiert die Front-End-IP-Konfiguration für eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs sowie die erforderlichen Quell- und Zielports. 

Mit der Lastenausgleichsregel **MyLoadBalancerRule** wird über Port 80 des Front-Ends **LoadBalancerFrontEnd** gelauscht. Die Regel sendet Netzwerkdatenverkehr an den Back-End-Adresspool **MyBackendPool** (ebenfalls unter Port 80). 

1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die Option **MyLoadBalancer**.
   
1. Wählen Sie unter **Einstellungen** die Option **Lastenausgleichsregeln** und dann **Hinzufügen**.
   
1. Geben Sie auf der Seite **Lastenausgleichsregel hinzufügen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   
   - **Name**: Geben Sie *MyLoadBalancerRule* ein.
   - **Protokoll**: Wählen Sie **TCP** aus.
   - **Port**: Geben Sie *80* ein.
   - **Back-End-Port**: Geben Sie *80* ein.
   - **Back-End-Pool**: Wählen Sie **MyBackendPool** aus.
   - **Integritätstest**: Wählen Sie **MyHealthProbe** aus. 
   
1. Klicken Sie auf **OK**.
   
   ![Hinzufügen einer Lastenausgleichsregel](./media/tutorial-load-balancer-port-forwarding-portal/5-load-balancing-rules.png)

## <a name="create-an-inbound-nat-port-forwarding-rule"></a>Erstellen von NAT-Eingangsregeln für die Portweiterleitung

Erstellen Sie eine Netzwerkadressübersetzung-Eingangsregel (NAT) für den Lastenausgleich zum Weiterleiten von Datenverkehr von einem bestimmten Port der Front-End-IP-Adresse zu einem bestimmten Port einer Back-End-VM.

1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die Option **MyLoadBalancer** aus.
   
1. Wählen Sie unter **Einstellungen** die Option **NAT-Eingangsregeln** und dann **Hinzufügen** aus. 
   
1. Geben Sie auf der Seite **NAT-Regel für eingehenden Datenverkehr hinzufügen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   
   - **Name**: Geben Sie *MyNATRuleVM1* ein.
   - **Port**: Geben Sie *4221* ein.
   - **Virtueller Zielcomputer**: Wählen Sie **MyVM1** aus der Dropdownliste aus.
   - **Netzwerk-IP-Konfiguration**: Wählen Sie **ipconfig1** aus der Dropdownliste aus.
   - **Portzuordnung**: Wählen Sie **Benutzerdefiniert** aus.
   - **Zielport**: Geben Sie *3389* ein.
   
1. Klicken Sie auf **OK**.
   
1. Wiederholen Sie die Schritte zum Hinzufügen einer NAT-Eingangsregel mit dem Namen *MyNATRuleVM2*mit **Port**: *4222* und **Virtueller Zielcomputer**: **MyVM2**.

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

In diesem Abschnitt installieren Sie Internetinformationsdienste (Internet Information Services, IIS) auf den Back-End-Servern und passen die Standardwebseite an, um den Computernamen anzuzeigen. Dann testen Sie mit der öffentlichen IP-Adresse den Lastenausgleich. 

Jede Back-End-VM stellt eine andere Version der IIS-Standardwebseite bereit, damit Sie die Load Balancer-Verteilungsanforderungen zwischen den beiden VMs anzeigen können.

### <a name="connect-to-the-vms-with-rdp"></a>Herstellen einer Verbindung mit den virtuellen Computern per RDP

Stellen Sie über Remotedesktop (RDP) mit jedem virtuellen Computer eine Verbindung her. 

1. Wählen Sie im Portal im Menü auf der linken Seite die Option **Alle Ressourcen**. Wählen Sie in der Ressourcenliste die einzelnen VMs in der Ressourcengruppe **MyResourceGroupLB** aus.
   
1. Wählen Sie auf der Seite **Übersicht** die Option **Verbinden** und dann **RDP-Datei herunterladen**. 
   
1. Öffnen Sie die heruntergeladene RDP-Datei, und wählen Sie **Verbinden**.
   
1. Wählen Sie auf dem Bildschirm „Windows-Sicherheit“ die Option **Weitere Optionen** und dann **Anderes Konto verwenden**. 
   
   Geben Sie den Benutzernamen *azureuser* und das Kennwort *Azure1234567* ein, und wählen Sie **OK**.
   
1. Wählen Sie für alle Eingabeaufforderungen zu Zertifikaten die Antwort **Ja**. 
   
   Der VM-Desktop wird in einem neuen Fenster geöffnet. 

### <a name="install-iis-and-replace-the-default-iis-web-page"></a>Installieren von IIS und Ersetzen der IIS-Standardwebseite 

Verwenden Sie PowerShell, um IIS zu installieren und die IIS-Standardwebseite durch eine Seite zu ersetzen, die den Namen der VM anzeigt.

1. Starten Sie auf MyVM1 und MyVM2 **Windows PowerShell** über das Menü **Start**. 

2. Führen Sie die folgenden Befehle aus, um IIS zu installieren und die IIS-Standardwebseite zu ersetzen:
   
   ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    
   ```
   
1. Schließen Sie die RDP-Verbindungen mit MyVM1 und MyVM2, indem Sie **Trennen** wählen. Fahren Sie die VMs nicht herunter.

### <a name="test-load-balancing"></a>Testen des Lastenausgleichs

1. Kopieren Sie im Portal auf der Seite **Übersicht** für **MyLoadBalancer** unter **Öffentliche IP-Adresse** die öffentliche IP-Adresse. Zeigen Sie mit der Maus auf die Adresse, und wählen Sie das Symbol **Kopieren**, um sie zu kopieren. In diesem Beispiel lautet sie **40.67.218.235**. 
   
1. Fügen Sie die private IP-Adresse (*40.67.218.235*) des Load Balancers in die Adresszeile des Internetbrowsers ein (bzw. geben Sie sie ein). 
   
   Die benutzerdefinierte IIS-Webserver-Standardseite wird im Browser angezeigt. Die Nachricht lautet entweder **Hello World from MyVM1** oder **Hello World from MyVM2**.
   
   ![Neue IIS-Standardseite](./media/tutorial-load-balancer-port-forwarding-portal/9-load-balancer-test.png) 
   
1. Aktualisieren Sie den Browser, um zu verfolgen, wie der Load Balancer den Datenverkehr auf die VMs verteilt. Die Seite **MyVM1** und die Seite **MyVM2** werden im Wechsel angezeigt, während der Load Balancer die Anforderungen auf die Back-End-VMs verteilt.
   
   >[!NOTE]
   >Sie müssen vielleicht zwischen Wiederholungsversuchen Ihren Browsercache leeren oder ein neues Browserfenster öffnen.

## <a name="test-port-forwarding"></a>Testen der Portweiterleitung

Mit Portweiterleitung können Sie mit der IP-Adresse des Load Balancers und dem in der NAT-Regel definierten Front-End-Portwert eine Remotedesktopverbindung mit einer Back-End-VM herstellen. 

1. Kopieren Sie im Portal auf der Seite **Übersicht** für **MyLoadBalancer** die öffentliche IP-Adresse. Zeigen Sie mit der Maus auf die Adresse, und wählen Sie das Symbol **Kopieren**, um sie zu kopieren. In diesem Beispiel lautet sie **40.67.218.235**. 
   
1. Öffnen Sie eine Eingabeaufforderung, und erstellen Sie mit dem folgenden Befehl mithilfe der öffentlichen IP-Adresse des Lastenausgleichs und des Front-End-Ports, den Sie in der NAT-Regel der VM definiert haben, eine Remotedesktopsitzung mit MyVM2. 
   
   ```
   mstsc /v:40.67.218.235:4222
   ```
  
1. Öffnen Sie die heruntergeladene RDP-Datei, und wählen Sie **Verbinden** aus.
   
1. Wählen Sie auf dem Bildschirm „Windows-Sicherheit“ die Option **Weitere Optionen** und dann **Anderes Konto verwenden**. 
   
   Geben Sie den Benutzernamen *azureuser* und das Kennwort *Azure1234567* ein, und wählen Sie **OK**.
   
1. Wählen Sie für alle Eingabeaufforderungen zu Zertifikaten die Antwort **Ja**. 
   
   Der MyVM2-Desktop wird in einem neuen Fenster geöffnet. 

Die RDP-Verbindung ist erfolgreich, weil die NAT-Eingangsregel **MyNATRuleVM2** den Datenverkehr über den Front-End-Port 4222 des Load Balancers an den MyVM2-Port 3389 (den RDP-Port) leitet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Öffnen Sie die Ressourcengruppe **MyResourceGroupLB**, und wählen Sie die Option **Ressourcengruppe löschen**, um den Load Balancer und alle zugehörigen Ressourcen zu löschen, sofern Sie sie nicht mehr benötigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen öffentlichen Load Balancer im Tarif „Standard“ erstellt. Sie haben Netzwerkressourcen, Back-End-Server, einen Integritätstest und Regeln für den Load Balancer erstellt und konfiguriert. Sie haben IIS auf den Back-End-VMs installiert und die öffentliche IP-Adresse des Load Balancers verwendet, um den Load Balancer zu testen. Sie haben die Portweiterleitung von einem festgelegten Port des Load Balancers zu einem Port auf einer Back-End-VM getestet. 

Weitere Informationen zu Azure Load Balancer finden Sie in den weiteren Tutorials zum Lastenausgleich.

> [!div class="nextstepaction"]
> [Azure Load Balancer-Tutorials](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
