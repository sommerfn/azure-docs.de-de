---
title: Behandeln von Problemen beim Anwendungszugriff auf einem virtuellen Computer in Azure | Microsoft-Dokumentation
description: Führen Sie diese ausführlichen Schritte zur Problembehandlung aus, um Probleme beim Herstellen einer Verbindung mit Anwendungen auf virtuellen Computern in Azure zu beheben.
services: virtual-machines
documentationcenter: ''
author: genlin
manager: gwallace
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: Anwendung kann nicht gestartet werden, Programm wird nicht geöffnet, Lauschport blockiert, Programm kann nicht gestartet werden, blockierter Lauschport
ms.assetid: b9ff7cd0-0c5d-4c3c-a6be-3ac47abf31ba
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: fd79e04cdd8f9d01131c016031d696c1583eb55d
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70080398"
---
# <a name="troubleshoot-application-connectivity-issues-on-virtual-machines-in-azure"></a>Beheben von Anwendungskonnektivitätsproblemen auf virtuellen Computern in Azure

Es gibt verschiedene Gründe, aus denen Sie eine Anwendung, die auf einem virtuellen Azure-Computer (VM) ausgeführt wird, nicht starten können bzw. keine Verbindung damit herstellen können. Zu den Gründen gehört, dass die Anwendung nicht ausgeführt wird oder nicht an den erwarteten Ports lauscht, dass der Lauschport blockiert ist oder dass Netzwerkeregeln Datenverkehr nicht ordnungsgemäß an die Anwendung übergeben. In diesem Artikel wird ein methodischer Ansatz zum Ermitteln und Beheben der Probleme beschrieben.

Wenn beim Herstellen einer Verbindung mit dem virtuellen Computer mit RDP oder SSH Probleme auftreten, finden Sie entsprechende Informationen in den folgenden Artikeln:

* [Behandeln von Problemen mit Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](troubleshoot-rdp-connection.md)
* [Behandeln von Problemen mit Secure Shell (SSH)-Verbindungen mit einem Linux-basierten virtuellen Azure-Computer](troubleshoot-ssh-connection.md)

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie Ihre Frage im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/)stellen, um dort Hilfe von Azure-Experten zu erhalten. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.

## <a name="quick-start-troubleshooting-steps"></a>Schritte zur Schnellstartproblembehandlung
Wenn beim Herstellen der Verbindung mit einer Anwendung Probleme auftreten, führen Sie die folgenden allgemeinen Schritte zur Problembehandlung aus. Versuchen Sie nach jedem Schritt erneut, eine Verbindung mit der Anwendung herzustellen:

* Starten Sie den virtuellen Computer neu.
* Erstellen Sie den Endpunkt, die Firewallregeln und die Regeln für die Netzwerksicherheitsgruppen (NSG) neu.
  * [Resource Manager-Modell: Verwalten von Netzwerksicherheitsgruppen](../../virtual-network/manage-network-security-group.md)
  * [Klassisches Modell: Verwalten der Endpunkte der Clouddienste](../../cloud-services/cloud-services-enable-communication-role-instances.md)
* Stellen Sie die Verbindung über einen anderen Ort her, z.B. über ein anderes virtuelles Azure-Netzwerk.
* Stellen Sie den virtuellen Computer erneut bereit.
  * [Erneutes Bereitstellen von virtuellen Windows-Computern](redeploy-to-new-node-windows.md)
  * [Erneutes Bereitstellen von virtuellen Linux-Computern](redeploy-to-new-node-linux.md)
* Erstellen Sie den virtuellen Computer neu.

Weitere Informationen finden Sie unter [Problembehandlung bei Endpunktverbindungen (RDP/SSH/HTTP-Fehler usw.)](https://social.msdn.microsoft.com/Forums/azure/en-US/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows).

## <a name="detailed-troubleshooting-overview"></a>Übersicht: Ausführliche Problembehandlung
Die Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird, konzentriert sich auf vier Hauptbereiche.

![Problembehandlung, wenn die Anwendung nicht gestartet werden kann](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1. Die Anwendung, die auf dem virtuellen Azure-Computer ausgeführt wird.
   * Wird die Anwendung selbst ordnungsgemäß ausgeführt?
2. Den virtuellen Azure-Computer.
   * Wird der virtuelle Computer selbst ordnungsgemäß ausgeführt, und reagiert er auf Anforderungen?
3. Azure-Netzwerkendpunkte.
   * Clouddienstendpunkte für virtuelle Computer im klassischen Bereitstellungsmodell
   * Netzwerksicherheitsgruppen und NAT-Eingangsregeln für virtuelle Computer im Resource Manager-Bereitstellungsmodell
   * Ist der Datenverkehrsfluss von den Benutzern zu dem virtuellen Computer bzw. der Anwendung über die erwarteten Ports möglich?
4. Ihre Internet-Edgegerät.
   * Verhindern Firewallregeln den ordnungsgemäßen Datenverkehrsfluss?

Bei Clientcomputern, die über eine Site-to-Site-VPN- oder ExpressRoute-Verbindung auf die Anwendung zugreifen, sind die Hauptbereiche, die zu Problemen führen können, die Anwendung und der virtuelle Azure-Computer.

Gehen Sie folgendermaßen vor, um die Quelle des Problems und dessen Korrektur zu bestimmen.

## <a name="step-1-access-application-from-target-vm"></a>Schritt 1: Zugreifen auf die Anwendung vom virtuellen Zielcomputer
Versuchen Sie, mithilfe des geeigneten Clientprogramms von der VM, auf der es läuft, auf die Anwendung zuzugreifen. Verwenden Sie den lokalen Hostnamen, die lokale IP-Adresse oder die Loopbackadresse (127.0.0.1).

![Anwendung direkt von der VM starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

Wenn es sich bei der Anwendung beispielsweise um einen Webserver handelt, öffnen Sie einen Browser auf der VM, und versuchen Sie, auf eine Webseite zuzugreifen, die auf der VM gehostet wird.

Wenn Sie auf die Anwendung zugreifen können, fahren Sie mit [Schritt 2](#step2)fort.

Wenn kein Zugriff auf die Anwendung möglich ist, überprüfen Sie die folgenden Einstellungen:

* Die Anwendung wird auf dem virtuellen Zielcomputer ausgeführt.
* Die Anwendung überwacht die erwarteten TCP- und UDP-Ports.

Verwenden Sie bei Windows- und Linux-basierten virtuellen Computern den Befehl **netstat -a** , um die aktiven Überwachungsports anzuzeigen. Überprüfen Sie die Ausgabe für die erwarteten Ports, die die Anwendung überwachen soll. Starten Sie die Anwendung neu, oder konfigurieren Sie sie so, dass die erwarteten Ports nach Bedarf verwendet werden, und versuchen Sie erneut, lokal auf die Anwendung zuzugreifen.

## <a id="step2"></a>Schritt 2: Zugreifen auf die Anwendung von einem anderen virtuellen Computer im gleichen virtuellen Netzwerk
Versuchen Sie, von einer anderen VM im gleichen virtuellen Netzwerk auf die Anwendung zuzugreifen. Verwenden Sie dazu den VM-Hostnamen oder die von Azure zugewiesene öffentliche, private oder Anbieter-IP-Adresse. Verwenden Sie für mithilfe des klassischen Bereitstellungsmodells erstellte virtuelle Computer nicht die öffentliche IP-Adresse des Clouddiensts.

![Anwendung von einer anderen VM starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

Wenn es sich bei der Anwendung beispielsweise um einen Webserver handelt, versuchen Sie, in einem Browser auf einer anderen VM im gleichen virtuellen Netzwerk auf eine Webseite zuzugreifen.

Wenn Sie auf die Anwendung zugreifen können, fahren Sie mit [Schritt 3](#step3)fort.

Wenn kein Zugriff auf die Anwendung möglich ist, überprüfen Sie die folgenden Einstellungen:

* Die Hostfirewall auf der Ziel-VM lässt die eingehende Anforderung und den ausgehenden Antwortdatenverkehr zu.
* Software zur Angriffserkennung oder Netzwerküberwachung, die auf der Ziel-VM ausgeführt wird, lässt den Datenverkehr zu.
* Die Endpunkte der Clouddienste oder die Netzwerksicherheitsgruppen lassen den Datenverkehr zu:
  * [Klassisches Modell: Verwalten der Endpunkte der Clouddienste](../../cloud-services/cloud-services-enable-communication-role-instances.md)
  * [Resource Manager-Modell: Verwalten von Netzwerksicherheitsgruppen](../../virtual-network/manage-network-security-group.md)
* Eine separate Komponente, die auf Ihrer VM auf dem Pfad zwischen der Test-VM und Ihrer VM ausgeführt wird, z.B. ein Load Balancer oder eine Firewall, lässt den Datenverkehr zu.

Verwenden Sie auf einem Windows-basierten virtuellen Computer die Windows-Firewall mit erweiterter Sicherheit, um zu bestimmen, ob die Firewallregeln den eingehenden und ausgehenden Datenverkehr der Anwendung ausschließen.

## <a id="step3"></a>Schritt 3: Zugreifen auf die Anwendung von einem Computer außerhalb des virtuellen Netzwerks
Versuchen Sie, von einem Computer, der sich nicht im gleichen Netzwerk wie der virtuelle Computer befindet, auf dem die Anwendung ausgeführt wird, auf die Anwendung zuzugreifen. Verwenden Sie ein anderes Netzwerk als der ursprüngliche Clientcomputer.

![Anwendung von einem Computer außerhalb des virtuellen Netzwerks starten](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

Wenn es sich bei der Anwendung beispielsweise um einen Webserver handelt, versuchen Sie, in einem Browser auf eine Webseite zuzugreifen, der auf einem Computer ausgeführt wird, der sich nicht im virtuellen Netzwerk befindet.

Wenn kein Zugriff auf die Anwendung möglich ist, überprüfen Sie die folgenden Einstellungen:

* Bei virtuellen Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden:
  
  * Stellen Sie sicher, dass die Endpunktkonfiguration für den virtuellen Computer eingehenden Datenverkehr zulässt, insbesondere das Protokoll (TCP oder UDP) und die öffentlichen und privaten Portnummern.
  * Stellen Sie sicher, dass die Zugriffssteuerungslisten (Access Control Lists, ACLs) auf dem Endpunkt aus dem Internet eingehenden Datenverkehr nicht verhindern.
  * Weitere Informationen finden Sie unter [Einrichten von Endpunkten für einen virtuellen Computer](../windows/classic/setup-endpoints.md).
* Bei virtuellen Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden:
  
  * Stellen Sie sicher, dass die Konfiguration der eingehenden NAT-Regel für den virtuellen Computer eingehenden Datenverkehr zulässt, insbesondere das Protokoll (TCP oder UDP) und die öffentlichen und privaten Portnummern.
  * Stellen Sie sicher, dass die Netzwerksicherheitsgruppen die eingehende Anforderung und den ausgehenden Antwortdatenverkehr zulassen.
  * Weitere Informationen finden Sie unter [Was ist eine Netzwerksicherheitsgruppe?](../../virtual-network/security-overview.md).

Wenn der virtuelle Computer oder der Endpunkt Mitglied einer Gruppe mit Lastenausgleich ist:

* Stellen Sie sicher, dass der Prüfpunkt-Protokoll (TCP oder UDP) und die Portnummer richtig sind.
* Wenn das Prüfpunkt-Protokoll und der Port vom Protokoll und Port der Gruppe mit Lastenausgleich abweichen:
  * Überprüfen Sie, ob die Anwendung auf das Testprotokoll (TCP oder UDP) und die Portnummer lauscht (verwenden Sie **netstat –a** auf dem virtuellen Zielcomputer).
  * Stellen Sie sicher, dass die Hostfirewall auf der Ziel-VM die eingehende Testanforderung und den ausgehenden Prüfpunkt-Antwortdatenverkehr zulässt.

Wenn Sie auf die Anwendung zugreifen können, stellen Sie sicher, dass Ihre Internet-Edgegeräte Folgendes zulassen:

* Den ausgehenden Anwendungsanforderungs-Datenverkehr vom Clientcomputer zum virtuellen Azure-Computer.
* Den eingehenden Anwendungs-Antwortdatenverkehr vom virtuellen Azure-Computer.

## <a name="step-4-if-you-cannot-access-the-application-use-ip-verify-to-check-the-settings"></a>Schritt 4: Wenn Sie auf die Anwendung nicht zugreifen können, überprüfen Sie die Einstellungen mithilfe der IP-Überprüfung. 

Weitere Informationen finden Sie unter [Übersicht über die Azure-Netzwerküberwachung](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen
[Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](troubleshoot-rdp-connection.md)

[Behandeln von Problemen mit Secure Shell (SSH)-Verbindungen mit einem Linux-basierten virtuellen Azure-Computer](troubleshoot-ssh-connection.md)


