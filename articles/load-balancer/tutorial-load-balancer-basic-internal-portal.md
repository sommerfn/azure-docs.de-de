---
title: 'Tutorial: Erstellen eines internen Lastenausgleichs – Azure-Portal'
titleSuffix: Azure Load Balancer
description: In diesem Tutorial wird gezeigt, wie Sie einen internen Load Balancer im Tarif „Basic“ über das Azure-Portal erstellen.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: As an IT administrator, I want to create a load balancer that load balances incoming internal traffic to virtual machines within a specific zone in a region.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2019
ms.author: allensu
ms.custom: seodec18
ms.openlocfilehash: 1b9d943f540a0132abc6a70eba888aa5f8f46093
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74225217"
---
# <a name="tutorial-balance-internal-traffic-load-with-a-basic-load-balancer-in-the-azure-portal"></a>Tutorial: Ausgleichen der internen Datenverkehrslast mithilfe eines Lastenausgleichs im Tarif „Basic“ über das Azure-Portal

Durch die Verteilung der eingehenden Anforderungen auf virtuelle Computer (VMs) ermöglicht ein Lastenausgleich ein höheres Maß an Verfügbarkeit und Skalierbarkeit. Sie können das Azure-Portal verwenden, um einen Load Balancer im Tarif „Basic“ zu erstellen und den internen Datenverkehr auf VMs zu verteilen. In diesem Tutorial wird veranschaulicht, wie Sie einen internen Load Balancer, Back-End-Server und Netzwerkressourcen im Tarif „Basic“ erstellen und konfigurieren.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

Falls Sie es vorziehen, können Sie diese Schritte auch mit der [Azure CLI](load-balancer-get-started-ilb-arm-cli.md) oder [Azure PowerShell](load-balancer-get-started-ilb-arm-ps.md) anstatt mit dem Portal ausführen.

Melden Sie sich zum Ausführen der Schritte dieses Tutorials am Azure-Portal unter [https://portal.azure.com](https://portal.azure.com) an.

## <a name="create-a-vnet-back-end-servers-and-a-test-vm"></a>Erstellen eines VNET, von Back-End-Servern und eines virtuellen Testcomputers

Erstellen Sie zunächst ein virtuelles Netzwerk (VNET). Erstellen Sie im VNET zwei VMs für den Back-End-Pool Ihres Load Balancers im Tarif „Basic“ und eine dritte VM zum Testen des Load Balancers. 

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Wählen Sie oben links im Portal **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk**.
   
1. Geben Sie im Bereich **Virtuelles Netzwerk erstellen** diese Werte ein (bzw. wählen Sie sie aus):
   
   - **Name**: Geben Sie *MyVNet* ein.
   - **Ressourcengruppe**: Wählen Sie **Neu erstellen** aus, geben Sie *MyResourceGroupLB* ein, und wählen Sie anschließend **OK** aus. 
   - **Subnetz** > **Name**: Geben Sie *MyBackendSubnet* ein.
   
1. Klicken Sie auf **Erstellen**.

   ![Erstellen eines virtuellen Netzwerks](./media/tutorial-load-balancer-basic-internal-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

1. Wählen Sie oben links im Portal die Option **Ressource erstellen** > **Compute** > **Windows Server 2016 Datacenter**. 
   
1. Geben Sie unter **Virtuellen Computer erstellen** auf der Registerkarte **Grundlagen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   - **Abonnement** > **Ressourcengruppe**: Öffnen Sie die Dropdownliste, und wählen Sie **MyResourceGroupLB** aus.
   - **Instanzendetails** > **Name des virtuellen Computers**: Geben Sie *MyVM1* ein.
   - **Instanzendetails** > **Verfügbarkeitsoptionen**: 
     1. Öffnen Sie die Dropdownliste, und wählen Sie die Option **Verfügbarkeitsgruppe** aus. 
     2. Wählen Sie **Neu erstellen**, geben Sie *MyAvailabilitySet* ein, und wählen Sie **OK**.
   
1. Wählen Sie die Registerkarte **Netzwerk** aus, oder wählen Sie **Weiter: Datenträger** und anschließend **Weiter: Netzwerk** aus. 
   
   Stellen Sie sicher, dass Folgendes ausgewählt ist:
   - **Virtuelles Netzwerk**: **MyVNet**
   - **Subnetz**: **MyBackendSubnet**
   
   Unter **Netzwerksicherheitsgruppe**:
   1. Wählen Sie **Advanced** (Erweitert). 
   1. Öffnen Sie die Dropdownliste **Netzwerksicherheitsgruppe konfigurieren**, und wählen Sie **Keine**. 
   
1. Wählen Sie die Registerkarte **Verwaltung** oder **Weiter** > **Verwaltung**. Legen Sie unter **Überwachung** die Option **Startdiagnose** auf **Aus** fest.
   
1. Klicken Sie auf **Überprüfen + erstellen**.
   
1. Überprüfen Sie die Einstellungen, und wählen Sie dann die Option **Erstellen**. 

1. Führen Sie die Schritte zum Erstellen einer zweiten VM mit dem Namen *MyVM2* aus, und verwenden Sie für alle anderen Einstellungen die Werte wie für MyVM1. 

1. Führen Sie die Schritte anschließend erneut aus, um eine dritte VM mit dem Namen *MyTestVM* zu erstellen. 

## <a name="create-a-basic-load-balancer"></a>Erstellen eines Load Balancers im Tarif „Basic“

Erstellen Sie einen internen Load Balancer im Tarif „Basic“, indem Sie das Portal verwenden. Der von Ihnen erstellte Name und die IP-Adresse werden automatisch als Front-End des Load Balancers konfiguriert.

1. Wählen Sie oben links im Portal **Ressource erstellen** > **Netzwerk** > **Load Balancer**.
   
2. Geben Sie auf der Seite **Lastenausgleich erstellen** auf der Registerkarte **Grundlagen** die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Überprüfen + erstellen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Subscription               | Wählen Sie Ihr Abonnement aus.    |    
    | Resource group         | Wählen Sie **Neu erstellen**, und geben Sie *MyResourceGroupLB* in das Textfeld ein.|
    | NAME                   | *myLoadBalancer*                                   |
    | Region         | Wählen Sie **Europa, Westen** aus.                                        |
    | type          | Wählen Sie **Intern** aus.                                        |
    | SKU           | Wählen Sie **Basic** aus.                          |
    | Virtuelles Netzwerk           | Wählen Sie *MyVNet* aus.                          |    
    | IP-Adresszuweisung              | Wählen Sie **Statisch** aus.   |
    | Private IP-Adresse|Geben Sie eine Adresse ein, die im Adressraum Ihres virtuellen Netzwerks und Subnetzes enthalten ist (beispielsweise *10.3.0.7*).  |

3. Klicken Sie auf der Registerkarte **Überprüfen + erstellen** auf **Erstellen**. 
   

## <a name="create-basic-load-balancer-resources"></a>Erstellen von Ressourcen für den Load Balancer im Tarif „Basic“

In diesem Abschnitt konfigurieren Sie Lastenausgleichseinstellungen für einen Back-End-Adresspool und einen Integritätstest. Außerdem geben Sie Lastenausgleichsregeln an.

### <a name="create-a-back-end-address-pool"></a>Erstellen eines Back-End-Adresspools

Zum Verteilen von Datenverkehr auf die VMs nutzt der Load Balancer einen Back-End-Adresspool. Der Back-End-Adresspool enthält die IP-Adressen der virtuellen Netzwerkschnittstellen (NICs), die mit dem Load Balancer verbunden sind. 

**Erstellen Sie wie folgt einen Back-End-Adresspool, der VM1 und VM2 enthält:**

1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die Option **MyLoadBalancer**.
   
1. Wählen Sie unter **Einstellungen** die Option **Back-End-Pools** und dann **Hinzufügen**.
   
1. Geben Sie auf der Seite **Back-End-Pool hinzufügen** die folgenden Werte ein (bzw. wählen Sie sie aus):
   
   - **Name**: Geben Sie *MyBackendPool* ein.
   - **Verknüpft mit**: Öffnen Sie die Dropdownliste, und wählen Sie die Option **Verfügbarkeitsgruppe** aus.
   - **Verfügbarkeitsgruppe**: Wählen Sie **MyAvailabilitySet** aus.
   
1. Wählen Sie **Zielnetzwerk-IP-Konfiguration hinzufügen**. 
   1. Fügen Sie **MyVM1** und **MyVM2** dem Back-End-Pool hinzu.
   2. Öffnen Sie nach dem Hinzufügen eines Computers jeweils die Dropdownliste, und wählen Sie die **Netzwerk-IP-Konfiguration** aus. 
   
   >[!NOTE]
   >Fügen Sie **MyTestVM** nicht dem Pool hinzu. 
   
1. Klicken Sie auf **OK**.
   
   ![Hinzufügen des Back-End-Adresspools](./media/tutorial-load-balancer-basic-internal-portal/3-load-balancer-backend-02.png)
   
1. Erweitern Sie auf der Seite **Back-End-Pools** den Eintrag **MyBackendPool**, und stellen Sie sicher, dass sowohl **VM1** als auch **VM2** aufgeführt ist.

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests

Damit der Load Balancer den VM-Status überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. 

**Erstellen Sie zur Überwachung der Integrität der virtuellen Computer wie folgt einen Integritätstest:**

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
   
   ![Hinzufügen eines Tests](./media/tutorial-load-balancer-basic-internal-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel

Mit einer Lastenausgleichsregel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt wird. Die Regel definiert die Front-End-IP-Konfiguration für eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs sowie die erforderlichen Quell- und Zielports. 

Mit der Lastenausgleichsregel **MyLoadBalancerRule** wird über Port 80 des Front-Ends **LoadBalancerFrontEnd** gelauscht. Die Regel sendet Netzwerkdatenverkehr an den Back-End-Adresspool **MyBackendPool** (ebenfalls unter Port 80). 

**Erstellen Sie die Lastenausgleichsregel wie folgt:**

1. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** und dann in der Ressourcenliste die Option **MyLoadBalancer**.
   
1. Wählen Sie unter **Einstellungen** die Option **Lastenausgleichsregeln** und dann **Hinzufügen**.
   
1. Geben Sie auf der Seite **Lastenausgleichsregel hinzufügen** die folgenden Werte ein (bzw. wählen Sie sie aus), falls sie noch nicht vorhanden sind:
   
   - **Name**: Geben Sie *MyLoadBalancerRule* ein.
   - **Front-End-IP-Adresse**: Geben Sie *LoadBalancerFrontEnd* ein (sofern noch nicht vorhanden).
   - **Protokoll**: Wählen Sie **TCP** aus.
   - **Port**: Geben Sie *80* ein.
   - **Back-End-Port**: Geben Sie *80* ein.
   - **Back-End-Pool**: Wählen Sie **MyBackendPool** aus.
   - **Integritätstest**: Wählen Sie **MyHealthProbe** aus. 
   
1. Klicken Sie auf **OK**.
   
   ![Hinzufügen einer Lastenausgleichsregel](./media/tutorial-load-balancer-basic-internal-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

Installieren Sie Internetinformationsdienste (Internet Information Services, IIS) auf den Back-End-Servern, und verwenden Sie dann MyTestVM, um den Load Balancer mit seiner privaten IP-Adresse zu testen. Jede Back-End-VM stellt eine andere Version der IIS-Standardwebseite bereit, damit Sie die Load Balancer-Verteilungsanforderungen zwischen den beiden VMs anzeigen können.

Ermitteln Sie im Portal auf der Seite **Übersicht** für **MyLoadBalancer** unter **Private IP-Adresse** die IP-Adresse. Zeigen Sie mit der Maus auf die Adresse, und wählen Sie das Symbol **Kopieren**, um sie zu kopieren. In diesem Beispiel lautet sie **10.3.0.7**. 

### <a name="connect-to-the-vms-with-rdp"></a>Herstellen einer Verbindung mit den virtuellen Computern per RDP

Erstellen Sie zunächst per Remotedesktop (RDP) eine Verbindung mit allen drei VMs. 

>[!NOTE]
>Standardmäßig ist der **RDP**-Port (Remotedesktop) für die VMs bereits geöffnet, um den Zugriff per Remotedesktop zu ermöglichen. 

**Stellen Sie die Remotedesktopverbindung (RDP) mit den VMs wie folgt her:**

1. Wählen Sie im Portal im Menü auf der linken Seite die Option **Alle Ressourcen**. Wählen Sie in der Ressourcenliste die einzelnen VMs in der Ressourcengruppe **MyResourceGroupLB** aus.
   
1. Wählen Sie auf der Seite **Übersicht** die Option **Verbinden** und dann **RDP-Datei herunterladen**. 
   
1. Öffnen Sie die heruntergeladene RDP-Datei, und wählen Sie **Verbinden**.
   
1. Wählen Sie auf dem Bildschirm „Windows-Sicherheit“ die Option **Weitere Optionen** und dann **Anderes Konto verwenden**. 
   
   Geben Sie Benutzername und Kennwort ein, und wählen Sie anschließend **OK** aus.
   
1. Wählen Sie für alle Eingabeaufforderungen zu Zertifikaten die Antwort **Ja**. 
   
   Der VM-Desktop wird in einem neuen Fenster geöffnet. 

### <a name="install-iis-and-replace-the-default-iis-page-on-the-back-end-vms"></a>Installieren von IIS und Ersetzen der IIS-Standardseite auf den Back-End-VMs

Verwenden Sie auf jedem Back-End-Server PowerShell, um IIS zu installieren und die IIS-Standardwebseite durch eine benutzerdefinierte Seite zu ersetzen.

>[!NOTE]
>Sie können auch den Assistenten zum **Hinzufügen von Rollen und Features** in **Server-Manager** verwenden, um IIS zu installieren. 

**Installieren Sie IIS, und aktualisieren Sie die Standardwebseite mit PowerShell, indem Sie wie folgt vorgehen:**

1. Starten Sie auf MyVM1 und MyVM2 **Windows PowerShell** über das Menü **Start**. 

2. Führen Sie die folgenden Befehle aus, um IIS zu installieren und die IIS-Standardwebseite zu ersetzen:
   
   ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```
1. Schließen Sie die RDP-Verbindungen mit MyVM1 und MyVM2, indem Sie **Trennen** wählen. Fahren Sie die VMs nicht herunter.

### <a name="test-the-load-balancer"></a>Testen des Lastenausgleichs

1. Öffnen Sie auf MyTestVM den **Internet Explorer**, und wählen Sie für alle Eingabeaufforderungen zur Konfiguration die Option **OK**. 
   
1. Fügen Sie die private IP-Adresse (*10.3.0.7*) des Load Balancers in die Adresszeile des Browsers ein (bzw. geben Sie sie ein). 
   
   Die benutzerdefinierte IIS-Webserver-Standardseite wird im Browser angezeigt. Die Nachricht lautet entweder **Hello World from MyVM1** oder **Hello World from MyVM2**.
   
1. Aktualisieren Sie den Browser, um zu verfolgen, wie der Load Balancer den Datenverkehr auf die VMs verteilt. Es kann sein, dass Sie zwischen den einzelnen Versuchen Ihren Browsercache löschen müssen.

   Die Seite **MyVM1** und die Seite **MyVM2** werden im Wechsel angezeigt, während der Load Balancer die Anforderungen auf die Back-End-VMs verteilt. 

   ![Neue IIS-Standardseite](./media/tutorial-load-balancer-basic-internal-portal/9-load-balancer-test.png) 
   
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Öffnen Sie die Ressourcengruppe **MyResourceGroupLB**, und wählen Sie die Option **Ressourcengruppe löschen**, um den Load Balancer und alle zugehörigen Ressourcen zu löschen, sofern Sie sie nicht mehr benötigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen internen Load Balancer im Tarif „Basic“ erstellt. Sie haben Netzwerkressourcen, Back-End-Server, einen Integritätstest und Regeln für den Load Balancer erstellt und konfiguriert. Sie haben IIS auf den Back-End-VMs installiert und eine Test-VM verwendet, um den Load Balancer im Browser zu testen. 

Informieren Sie sich als Nächstes darüber, wie Sie über mehrere Verfügbarkeitszonen hinweg einen Lastenausgleich für virtuelle Computer durchführen.

> [!div class="nextstepaction"]
> [Lastenausgleich für virtuelle Computer über Verfügbarkeitszonen hinweg](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
