---
title: 'Tutorial: Erstellen der Infrastruktur für einen Service Fabric-Cluster auf virtuellen Azure-Computern – Azure Service Fabric | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie die Infrastruktur von virtuellen Azure-Computern zum Ausführen eines Service Fabric-Clusters einrichten.
services: service-fabric
documentationcenter: .net
author: jpconnock
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/22/2019
ms.author: jeconnoc
ms.custom: mvc
ms.openlocfilehash: b24b4d95827dbd398c0eba43dcbad9fbfeb51469
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74166279"
---
# <a name="tutorial-create-azure-vm-infrastructure-to-host-a-service-fabric-cluster"></a>Tutorial: Erstellen der Infrastruktur virtueller Azure-Computer zum Hosten eines Service Fabric-Clusters

Mit eigenständigen Service Fabric-Clustern können Sie Ihre eigene Umgebung wählen und einen Cluster im Rahmen des Service Fabric-Konzepts „Jedes Betriebssystem, jede Cloud“ erstellen. In dieser Tutorialreihe erstellen Sie einen eigenständigen auf virtuellen Azure-Computern gehosteten Cluster und installieren darin eine Anwendung.

Dieses Tutorial ist der erste Teil einer Serie. In diesem Artikel generieren Sie die Ressourcen für die virtuellen Azure-Computer, die zum Hosten Ihres eigenständigen Service Fabric-Clusters benötigt werden. In den Folgeartikeln installieren Sie die eigenständige Service Fabric-Suite sowie eine Beispielanwendung in Ihrem Cluster und bereinigen schließlich Ihren Cluster.

Im ersten Teil der Serie lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Gruppe von Azure-VM-Instanzen
> * Ändern der Sicherheitsgruppe
> * Anmelden bei einer der Instanzen
> * Vorbereiten der Instanz für Service Fabric

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein Azure-Abonnement, um dieses Tutorial durcharbeiten zu können.  Wenn Sie noch kein Konto haben, erstellen Sie eines im [Azure-Portal](https://portal.azure.com).

## <a name="create-azure-virtual-machine-instances"></a>Erstellen von Azure-VM-Instanzen

1. Melden Sie sich beim Azure-Portal an, und wählen Sie **Virtuelle Computer** (nicht „Virtuelle Computer (klassisch)“) aus.

   ![Azure-Portal – Virtuelle Computer][az-console]

2. Wählen Sie die Schaltfläche **Hinzufügen** aus, um das Formular **Virtuellen Computer erstellen** zu öffnen.

3. Wählen Sie auf der Registerkarte **Grundeinstellungen** das gewünschte Abonnement und die gewünschte Ressourcengruppe aus (es empfiehlt sich die Verwendung einer neuen Ressourcengruppe).

4. Ändern Sie den Typ **Image** in **Windows Server 2016 Datacenter**. 
 
5. Ändern Sie die **Größe** der Instanz in **Standard DS2 v2**. Legen Sie den **Benutzernamen** und das **Kennwort** eines Administrators fest, und notieren Sie sie.

6. Lassen Sie die **Regeln für eingehende Ports** zunächst blockiert. Diese werden im nächsten Abschnitt konfiguriert.

7. Erstellen Sie auf der Registerkarte **Netzwerk** ein neues **virtuelles Netzwerk**, und notieren Sie den Namen.

8. Legen Sie als Nächstes die **NIC-Netzwerksicherheitsgruppe** auf **Erweitert** fest. Erstellen Sie eine neue Sicherheitsgruppe, und notieren Sie den Namen. Erstellen Sie dann die folgenden Regeln, um TCP-Datenverkehr von allen Quellen zuzulassen:

   ![sf-inbound][sf-inbound]

   * Port `3389` für RDP und ICMP (grundlegende Konnektivität)
   * Ports `19000-19003` für Service Fabric
   * Ports `19080-19081` für Service Fabric
   * Port `8080` für Webbrowseranforderungen

   > [!TIP]
   > Damit Ihre virtuellen Computer in Service Fabric miteinander verbunden werden können, müssen die virtuellen Computer, die Ihre Infrastruktur hosten, die gleichen Anmeldeinformationen besitzen.  Konsistente Anmeldeinformationen lassen sich grundsätzlich auf zwei Arten erreichen: Sie können alle der gleichen Domäne hinzufügen oder auf jedem virtuellen Computer das gleiche Administratorkennwort festlegen. Glücklicherweise können alle virtuellen Computer im gleichen **virtuellen Netzwerk** in Azure mühelos verbunden werden, sodass sichergestellt ist, dass sich alle Instanzen im gleichen Netzwerk befinden können.

9. Fügen Sie eine weitere Regel hinzu. Legen Sie die Quelle auf **Diensttag** und das Quelldiensttag auf **VirtualNetwork** fest. In Service Fabric müssen für die Kommunikation innerhalb des Clusters die folgenden Ports geöffnet sein: 135, 137–139, 445, 20001–20031, 20606–20861.

   ![vnet-inbound][vnet-inbound]

10. Die übrigen Optionen sind mit ihrem Standardzustand zulässig. Sie können sie bei Bedarf überprüfen. Starten Sie dann den virtuellen Computer.

## <a name="creating-more-instances-for-your-service-fabric-cluster"></a>Erstellen weiterer Instanzen für den Service Fabric-Cluster

Starten Sie zwei weitere **virtuelle Computer**, und behalten Sie die gleichen im vorherigen Abschnitt beschriebenen Einstellungen bei. Verwenden Sie insbesondere den gleichen Benutzernamen und das gleiche Kennwort für den Administrator. Das **virtuelle Netzwerk** und die **NIC-Netzwerksicherheitsgruppe** müssen nicht neu erstellt werden. Wählen Sie die bereits erstellten im jeweiligen Dropdownmenü aus. Es kann einige Minuten dauern, bis die einzelnen Instanzen bereitgestellt werden.

## <a name="connect-to-your-instances"></a>Herstellen einer Verbindung mit den Instanzen

1. Wählen Sie eine der Instanzen im Abschnitt **Virtueller Computer** aus.

2. Notieren Sie auf der Registerkarte **Übersicht** die *private* IP-Adresse. Klicken Sie dann auf **Verbinden**.

3. Beachten Sie auf der Registerkarte **RDP**, dass die öffentliche IP-Adresse und der Port 3389, der zuvor speziell geöffnet wurde, verwendet werden. Laden Sie die RDP-Datei herunter.
 
4. Öffnen Sie die RDP-Datei, und geben Sie bei der entsprechenden Aufforderung den Benutzernamen und das Kennwort ein, die Sie beim Einrichten des virtuellen Computers festgelegt haben.

5. Nachdem die Verbindung mit einer Instanz hergestellt wurde, müssen Sie sich vergewissern, dass die Remoteregistrierung ausgeführt wird, und anschließend SMB aktivieren und die erforderlichen Ports für SMB und die Remoteregistrierung öffnen.

   SMB wird mit dem folgenden PowerShell-Befehl aktiviert:

   ```powershell
   netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes
   ```

6. Zum Öffnen der Firewallports wurde der folgende PowerShell-Befehl verwendet:

   ```powershell
   New-NetFirewallRule -DisplayName "Service Fabric Ports" -Direction Inbound -Action Allow -RemoteAddress LocalSubnet -Protocol TCP -LocalPort 135, 137-139, 445
   ```

7. Wiederholen Sie diesen Vorgang für die anderen Instanzen, und notieren Sie sich auch hier die privaten IP-Adressen.

## <a name="verify-your-settings"></a>Überprüfen Ihrer Einstellungen

1. Stellen Sie über RDP eine Verbindung mit einem der virtuellen Computer her, um die grundlegende Konnektivität zu überprüfen.

2. Öffnen Sie über diesen virtuellen Computer die **Eingabeaufforderung**. Verwenden Sie dann den Ping-Befehl, um eine Verbindung zwischen den virtuellen Computern herzustellen, und ersetzen Sie dabei die unten angegebene IP-Adresse durch eine der privaten IP-Adressen, die Sie zuvor notiert haben (nicht die IP des virtuellen Computers, mit dem die Verbindung bereits hergestellt ist).

   ```
   ping 172.31.20.163
   ```

   Wenn viermal eine Ausgabe wie `Reply from 172.31.20.163: bytes=32 time<1ms TTL=128` zurückgegeben wird, funktioniert die Verbindung zwischen den Instanzen.

3. Überprüfen Sie nun mithilfe des folgenden Befehls, ob Ihre SMB-Freigabe funktioniert:

   ```
   net use * \\172.31.20.163\c$
   ```

   Die zurückgegebene Ausgabe sollte in etwa wie folgt aussehen: `Drive Z: is now connected to \\172.31.20.163\c$.`.


   Damit sind die Instanzen ordnungsgemäß für Service Fabric vorbereitet.

## <a name="next-steps"></a>Nächste Schritte

Im ersten Teil der Reihe haben Sie gelernt, wie Sie drei Azure-VM-Instanzen starten und für die Service Fabric-Installation konfigurieren:

> [!div class="checklist"]
> * Erstellen einer Gruppe von Azure-VM-Instanzen
> * Ändern der Sicherheitsgruppe
> * Anmelden bei einer der Instanzen
> * Vorbereiten der Instanz für Service Fabric

Im zweiten Teil der Reihe erfahren Sie, wie Sie Service Fabric in Ihrem Cluster konfigurieren.

> [!div class="nextstepaction"]
> [Installieren und Erstellen eines Service Fabric-Clusters](service-fabric-tutorial-standalone-create-service-fabric-cluster.md)

<!-- IMAGES -->
[az-console]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/az-console.png
[sf-inbound]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/sf-inbound.png
[vnet-inbound]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/vnet-inbound.png
