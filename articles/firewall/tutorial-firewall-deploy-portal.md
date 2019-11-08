---
title: 'Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal'
description: In diesem Tutorial erfahren Sie, wie Sie Azure Firewall über das Azure-Portal bereitstellen und konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 10/28/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 124a87728a8d201c329b15d94ae7e61a225646ab
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73468453"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal

Die Steuerung des ausgehenden Netzwerkzugriffs ist ein wichtiger Teil eines umfassenden Netzwerksicherheitsplans. Vielleicht möchten Sie beispielsweise den Zugriff auf Websites einschränken. Mitunter kann es auch empfehlenswert sein, die verfügbaren Ports einzuschränken.

Eine Möglichkeit zur Steuerung des ausgehenden Netzwerkzugriffs aus einem Subnetz ist Azure Firewall. Mit Azure Firewall können Sie Folgendes konfigurieren:

* Anwendungsregeln, die vollqualifizierte Domänennamen (Fully Qualified Domain Names, FQDNs) definieren, auf die von einem Subnetz aus zugegriffen werden kann.
* Netzwerkregeln, die die Quelladresse, das Protokoll, den Zielport und die Zieladresse definieren.

Die konfigurierten Firewallregeln werden auf den Netzwerkdatenverkehr angewendet, wenn Sie Ihren Netzwerkdatenverkehr an die Firewall als Subnetz-Standardgateway weiterleiten.

In diesem Tutorial erstellen Sie der Einfachheit halber ein einzelnes vereinfachtes VNET mit drei Subnetzen. Für Produktionsbereitstellungen wird ein [Hub-Spoke-Modell](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) empfohlen, bei dem sich die Firewall in einem eigenen VNET befindet. Die Workloadserver befinden sich in per Peering verknüpften VNETs in derselben Region mit einem oder mehreren Subnetzen.

* **AzureFirewallSubnet:** Das Subnetz mit der Firewall.
* **Workload-SN:** Das Subnetz mit dem Workloadserver. Der Netzwerkdatenverkehr dieses Subnetzes durchläuft die Firewall.
* **Jump-SN:** Das Subnetz mit dem Sprungserver. Der Sprungserver besitzt eine öffentliche IP-Adresse, mit der Sie eine Remotedesktopverbindung herstellen können. Von dort aus können Sie dann über einen weiteren Remotedesktop eine Verbindung mit dem Workloadserver herstellen.

![Netzwerkinfrastruktur des Tutorials](media/tutorial-firewall-rules-portal/Tutorial_network.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten einer Netzwerkumgebung zu Testzwecken
> * Bereitstellen einer Firewall
> * Erstellen einer Standardroute
> * Konfigurieren einer Anwendungsregel zum Zulassen des Zugriffs auf www.google.com
> * Konfigurieren einer Netzwerkregel, um den Zugriff auf externe DNS-Server zuzulassen
> * Testen der Firewall

Sie können dieses Tutorial auch mit [Azure PowerShell](deploy-ps.md) durcharbeiten.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="set-up-the-network"></a>Einrichten des Netzwerks

Erstellen Sie zunächst eine Ressourcengruppe für die Ressourcen, die zum Bereitstellen der Firewall benötigt werden. Erstellen Sie dann ein VNet, Subnetze und Testserver.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Die Ressourcengruppe enthält alle Ressourcen für das Tutorial.

1. Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.
2. Wählen Sie im Menü des Azure-Portals die Option **Ressourcengruppen** aus, oder suchen Sie auf einer beliebigen Seite nach *Ressourcengruppen*, und wählen Sie diese Option anschließend aus. Wählen Sie anschließend **Hinzufügen**.
3. Geben Sie unter **Ressourcengruppenname** die Zeichenfolge *Test-FW-RG* ein.
4. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
5. Wählen Sie unter **Ressourcengruppenstandort** einen Standort aus. Alle weiteren Ressourcen, die Sie erstellen, müssen sich am gleichen Standort befinden.
6. Klicken Sie auf **Erstellen**.

### <a name="create-a-vnet"></a>Erstellen eines VNET

Dieses VNET soll drei Subnetze enthalten.

> [!NOTE]
> Die Größe des Subnetzes „AzureFirewallSubnet“ beträgt /26. Weitere Informationen zur Subnetzgröße finden Sie unter [Azure Firewall – Häufig gestellte Fragen](firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size).

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.
1. Wählen Sie **Netzwerk** > **Virtuelles Netzwerk** aus.
1. Geben Sie unter **Name** die Zeichenfolge **Test-FW-VN** ein.
1. Geben Sie unter **Adressraum** die Zeichenfolge **10.0.0.0/16** ein.
1. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
1. Wählen Sie für **Ressourcengruppe** die Gruppe **Test-FW-RG** aus.
1. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
1. Geben Sie unter **Subnetz** als **Name** die Zeichenfolge **AzureFirewallSubnet** ein. Die Firewall befindet sich diesem Subnetz, und der Subnetzname **muss** „AzureFirewallSubnet“ lauten.
1. Geben Sie unter **Adressbereich** die Zeichenfolge **10.0.1.0/26** ein.
1. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und wählen Sie dann **Erstellen** aus.

### <a name="create-additional-subnets"></a>Erstellen zusätzlicher Subnetze

Erstellen Sie als Nächstes Subnetze für den Sprungserver sowie ein Subnetz für die Workloadserver.

1. Wählen Sie im Menü des Azure-Portals die Option **Ressourcengruppen** aus, oder suchen Sie auf einer beliebigen Seite nach *Ressourcengruppen*, und wählen Sie diese Option anschließend aus. Wählen Sie anschließend **Test-FW-RG** aus.
2. Wählen Sie das virtuelle Netzwerk **Test-FW-VN** aus.
3. Wählen Sie **Subnetze** >  **+ Subnetz** aus.
4. Geben Sie unter **Name** die Zeichenfolge **Workload-SN** ein.
5. Geben Sie unter **Adressbereich** die Zeichenfolge **10.0.2.0/24** ein.
6. Klicken Sie auf **OK**.

Erstellen Sie ein weiteres Subnetz namens **Jump-SN** mit dem Adressbereich **10.0.3.0/24**.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie nun die virtuellen Sprung- und Workloadcomputer, und platzieren Sie sie in den entsprechenden Subnetzen.

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.
2. Wählen Sie **Compute**, und wählen Sie dann in der Liste der ausgewählten Elemente die Option **Windows Server 2016 Datacenter**.
3. Geben Sie die folgenden Werte für den virtuellen Computer ein:

   |Einstellung  |Wert  |
   |---------|---------|
   |Resource group     |**Test-FW-RG**|
   |Name des virtuellen Computers     |**Srv-Jump**|
   |Region     |Wie zuvor|
   |Benutzername des Administrators     |**azureuser**|
   |Kennwort     |**Azure123456!**|

4. Wählen Sie unter **Regeln für eingehende Ports** für **Öffentliche Eingangsports** die Option **Ausgewählte Ports zulassen** aus.
5. Wählen Sie unter **Eingangsports auswählen** die Option **RDP (3389)** aus.

6. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie auf **Weiter: Datenträger**.
7. Übernehmen Sie die Standardeinstellungen für Datenträger, und wählen Sie **Weiter: Netzwerk** aus.
8. Stellen Sie sicher, dass als virtuelles Netzwerk **Test-FW-VN** und als Subnetz **Jump-SN** ausgewählt ist.
9. Übernehmen Sie für **Öffentliche IP** den Standardnamen der neuen öffentlichen IP-Adresse (Srv-Jump-Ip).
11. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und wählen Sie **Weiter: Verwaltung** aus.
12. Wählen Sie **Aus** aus, um die Startdiagnose zu deaktivieren. Übernehmen Sie für die anderen Einstellungen die Standardwerte, und klicken Sie dann auf **Bewerten + erstellen**.
13. Überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und wählen Sie dann **Erstellen** aus.

Konfigurieren Sie anhand der Angaben in der folgenden Tabelle eine weitere VM mit dem Namen **Srv-Work**. Die restliche Konfiguration ist mit der Konfiguration des virtuellen Computers „Srv-Jump“ identisch.

|Einstellung  |Wert  |
|---------|---------|
|Subnet|**Workload-SN**|
|Öffentliche IP-Adresse|**Keine**|
|Öffentliche Eingangsports|**Keine**|

## <a name="deploy-the-firewall"></a>Bereitstellen der Firewall

Stellen Sie die Firewall im VNET bereit.

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** die Option **Ressource erstellen** aus.
2. Geben Sie **Firewall** in das Suchfeld ein, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie **Firewall** aus, und klicken Sie anschließend auf **Erstellen**.
4. Konfigurieren Sie die Firewall auf der Seite **Firewall erstellen** anhand der folgenden Tabelle:

   |Einstellung  |Wert  |
   |---------|---------|
   |Subscription     |\<Ihr Abonnement\>|
   |Resource group     |**Test-FW-RG** |
   |NAME     |**Test-FW01**|
   |Location     |Wählen Sie den gleichen Standort aus wie zuvor.|
   |Virtuelles Netzwerk auswählen     |**Vorhandene verwenden**: **Test-FW-VN**|
   |Öffentliche IP-Adresse     |**Neu erstellen**. Die öffentliche IP-Adresse muss vom Standard-SKU-Typ sein.|

5. Klicken Sie auf **Überprüfen + erstellen**.
6. Überprüfen Sie die Zusammenfassung, und wählen Sie dann **Erstellen** aus, um die Firewall zu erstellen.

   Die Bereitstellung dauert einige Minuten.
7. Navigieren Sie nach Abschluss der Bereitstellung zur Ressourcengruppe **Test-FW-RG**, und wählen Sie die Firewall **Test-FW01** aus.
8. Notieren Sie sich die private IP-Adresse. Diese wird später für die Erstellung der Standardroute benötigt.

## <a name="create-a-default-route"></a>Erstellen einer Standardroute

Konfigurieren Sie die ausgehende Standardroute für das Subnetz **Workload-SN** so, dass sie die Firewall durchläuft.

1. Wählen Sie im Menü des Azure-Portals die Option **Alle Dienste** aus, oder suchen Sie auf einer beliebigen Seite nach *Alle Dienste*, und wählen Sie diese Option anschließend aus.
2. Wählen Sie unter **Netzwerk** die Option **Routingtabellen** aus.
3. Wählen Sie **Hinzufügen**.
4. Geben Sie unter **Name** die Zeichenfolge **Firewall-route** ein.
5. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
6. Wählen Sie für **Ressourcengruppe** die Gruppe **Test-FW-RG** aus.
7. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
8. Klicken Sie auf **Erstellen**.
9. Klicken Sie auf **Aktualisieren**, und wählen Sie anschließend die Routingtabelle **Firewall-route** aus.
10. Wählen Sie **Subnetze** und dann **Zuordnen** aus.
11. Wählen Sie **Virtuelles Netzwerk** > **Test-FW-VN** aus.
12. Wählen Sie unter **Subnetz** die Option **Workload-SN** aus. Stellen Sie sicher, dass Sie nur das Subnetz **Workload-SN** für diese Route auswählen. Andernfalls funktioniert die Firewall nicht korrekt.

13. Klicken Sie auf **OK**.
14. Wählen Sie **Routen** und dann **Hinzufügen** aus.
15. Geben Sie für **Routenname** den Namen **fw-dg** ein.
16. Geben Sie unter **Adresspräfix** die Zeichenfolge **0.0.0.0/0** ein.
17. Wählen Sie unter **Typ des nächsten Hops** die Option **Virtuelles Gerät** aus.

    Azure Firewall ist eigentlich ein verwalteter Dienst, in dieser Situation kann aber „Virtuelles Gerät“ verwendet werden.
18. Geben Sie unter **Adresse des nächsten Hops** die private IP-Adresse für die Firewall ein, die Sie sich zuvor notiert haben.
19. Klicken Sie auf **OK**.

## <a name="configure-an-application-rule"></a>Konfigurieren einer Anwendungsregel

Hierbei handelt es sich um die Anwendungsregel, die ausgehenden Zugriff auf [www.google.com]\(www.google.com) zulässt.

1. Öffnen Sie **Test-FW-RG**, und wählen Sie die Firewall **Test-FW01** aus.
2. Wählen Sie auf der Seite **Test-FW01** unter **Einstellungen** die Option **Regeln** aus.
3. Klicken Sie auf die Registerkarte **Anwendungsregelsammlung**.
4. Wählen Sie **Anwendungsregelsammlung hinzufügen** aus.
5. Geben Sie unter **Name** die Zeichenfolge **App-Coll01** ein.
6. Geben Sie für **Priorität** den Wert **200** ein.
7. Wählen Sie für **Aktion** die Option **Zulassen** aus.
8. Geben Sie unter **Regeln** > **Ziel-FQDNs** für **Name** die Zeichenfolge **Allow-Google** ein.
9. Geben Sie unter **Quelladressen** die Zeichenfolge **10.0.2.0/24** ein.
10. Geben Sie unter **Protokoll:Port** die Zeichenfolge **http, https** ein.
11. Geben Sie unter **Ziel-FQDNs** die Adresse **www.google.com** ein.
12. Wählen Sie **Hinzufügen**.

Azure Firewall enthält eine integrierte Regelsammlung für Infrastruktur-FQDNs, die standardmäßig zulässig sind. Diese FQDNs sind plattformspezifisch und können nicht für andere Zwecke verwendet werden. Weitere Informationen finden Sie unter [Infrastruktur-FQDNs](infrastructure-fqdns.md).

## <a name="configure-a-network-rule"></a>Konfigurieren einer Netzwerkregel

Hierbei handelt es sich um die Netzwerkregel, die ausgehenden Zugriff auf zwei IP-Adressen am Port 53 (DNS) zulässt.

1. Klicken Sie auf die Registerkarte **Netzwerkregelsammlung**.
2. Wählen Sie **Netzwerkregelsammlung hinzufügen** aus.
3. Geben Sie unter **Name** die Zeichenfolge **Net-Coll01** ein.
4. Geben Sie für **Priorität** den Wert **200** ein.
5. Wählen Sie für **Aktion** die Option **Zulassen** aus.
6. Geben Sie unter **Regeln** für **Name** die Zeichenfolge **Allow-DNS** ein.
7. Wählen Sie für **Protokoll** die Option **UDP** aus.
8. Geben Sie unter **Quelladressen** die Zeichenfolge **10.0.2.0/24** ein.
9. Geben Sie für die Zieladresse die Zeichenfolge **209.244.0.3,209.244.0.4** ein.

   Dies sind öffentliche DNS-Server, die von CenturyLink betrieben werden.
1. Geben Sie unter **Zielports** den Wert **53** ein.
2. Wählen Sie **Hinzufügen**.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>Ändern der primären und sekundären DNS-Adresse für die Netzwerkschnittstelle **Srv-Work**

In diesem Tutorial konfigurieren Sie zu Testzwecken die primäre und sekundäre DNS-Adresse des Servers. Hierbei handelt es sich nicht um eine generelle Azure Firewall-Anforderung.

1. Wählen Sie im Menü des Azure-Portals die Option **Ressourcengruppen** aus, oder suchen Sie auf einer beliebigen Seite nach *Ressourcengruppen*, und wählen Sie diese Option anschließend aus. Wählen Sie die Ressourcengruppe **Test-FW-RG** aus.
2. Wählen Sie die Netzwerkschnittstelle für die VM **Srv-Work** aus.
3. Wählen Sie unter **Einstellungen** die Option **DNS-Server** aus.
4. Wählen Sie unter **DNS-Server** die Option **Benutzerdefiniert** aus.
5. Geben Sie **209.244.0.3** in das Textfeld **DNS-Server hinzufügen** und **209.244.0.4** in das nächste Textfeld ein.
6. Wählen Sie **Speichern** aus.
7. Starten Sie den virtuellen Computer **Srv-Work** neu.

## <a name="test-the-firewall"></a>Testen der Firewall

Testen Sie nun die Firewall, um sicherzustellen, dass sie wie erwartet funktioniert.

1. Überprüfen Sie im Azure-Portal die Netzwerkeinstellungen für den virtuellen Computer **Srv-Work**, und notieren Sie sich die private IP-Adresse.
2. Verbinden Sie einen Remotedesktop mit der VM **Srv-Jump**, und melden Sie sich an. Öffnen Sie auf der VM eine Remotedesktopverbindung mit der privaten IP-Adresse von **Srv-Work**.
3. Navigieren Sie in Internet Explorer zu https://www.google.com.
4. Klicken Sie in den Sicherheitswarnungen von Internet Explorer auf **OK** > **Schließen**.

   Die Google-Startseite sollte nun angezeigt werden.

5. Navigieren Sie zu https://www.microsoft.com.

   Sie sollten durch die Firewall blockiert werden.

Damit haben Sie sich vergewissert, dass die Firewallregeln funktionieren:

* Sie können zum einzigen zulässigen FQDN navigieren, aber nicht zu anderen.
* Sie können DNS-Namen mithilfe des konfigurierten externen DNS-Servers auflösen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die Firewallressourcen für das nächste Tutorial behalten oder die Ressourcengruppe **Test-FW-RG** löschen, wenn Sie sie nicht mehr benötigen. Dadurch werden alle firewallbezogenen Ressourcen gelöscht.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Überwachen von Azure Firewall-Protokollen](./tutorial-diagnostics.md)
