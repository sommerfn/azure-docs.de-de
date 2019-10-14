---
title: Integrieren einer App in ein Azure Virtual Network – Azure App Service
description: Sie erfahren, wie Sie eine Verbindung zwischen einer App in Azure App Service und einem neuen oder vorhandenen Azure Virtual Network herstellen.
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/21/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: a6d0cba41e694e154da32a878cb4c076aae13e65
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72034726"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integrieren Ihrer App in ein Azure Virtual Network
In diesem Dokument wird die Azure App Service-Funktion für die Integration in ein virtuelles Netzwerk beschrieben, und Sie erfahren, wie Sie die Funktion mit Apps in [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) einrichten. Mit [Azure Virtual Networks][VNETOverview] (VNETs) können Sie viele Ihrer Azure-Ressourcen in einem Netzwerk platzieren, das nicht über das Internet geroutet werden kann.  

Es gibt zwei Varianten der Nutzung von Azure App Service. 

1. Mehrinstanzenfähige Systeme, die den gesamten Bereich der Tarife unterstützen, mit Ausnahme von Isoliert
2. Die App Service-Umgebung (App Service Environment, ASE), die in Ihrem VNET bereitgestellt wird und Apps im Tarif „App Service (isoliert)“ unterstützt

In diesem Dokument werden die beiden Funktionen für die VNET-Integration erläutert, die in der mehrinstanzenfähigen App Service-Lösung verwendet werden können. In der [App Service-Umgebung][ASEintro] befindet sich Ihre App bereits in einem VNET und benötigt das VNET-Integrationsfeature nicht, um Ressourcen in demselben VNET zu erreichen. Ausführliche Informationen zu allen Netzwerkfunktionen von App Service finden Sie unter [App Service networking features](networking-features.md) (App Service-Netzwerkfunktionen).

Es gibt zwei Versionen der Funktion für die VNET-Integration:

1. Eine Version ermöglicht die Integration in VNETs in der gleichen Region. Diese Version der Funktion erfordert ein Subnetz in einem VNET in der gleichen Region. Diese Funktion ist noch in der Vorschau, wird aber für Windows-App-Produktionsworkloads unterstützt, wobei einige Einschränkungen unten aufgeführt sind.
2. Die andere Version ermöglicht die Integration in VNETs in anderen Regionen oder in klassische VNETs. Diese Version erfordert die Bereitstellung eines virtuellen Netzwerkgateways in Ihrem VNET. Dies ist das Feature, das auf einer Point-to-Site-VPN-Verbindung basiert und nur für Windows-Apps unterstützt wird.

Für eine App kann jeweils nur eine Version der Funktion für die VNET-Integration verwendet werden. Daher stellt sich die Frage, welche Version für Sie die richtige Lösung ist. Beide Versionen können für viele Zwecke verwendet werden. Es gibt jedoch einige wesentliche Unterschiede:

| Problem  | Lösung | 
|----------|----------|
| Sie möchten eine RFC 1918-Adresse (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) in der gleichen Region erreichen. | Regionale VNET-Integration |
| Sie möchten Ressourcen in einem klassischen VNET oder einem VNET in einer anderen Region erreichen. | VNET-Integration, die ein Gateway erfordert |
| Sie möchten RFC 1918-Endpunkte über ExpressRoute erreichen. | Regionale VNET-Integration |
| Sie möchten Ressourcen über Dienstendpunkte erreichen. | Regionale VNET-Integration |

RFC 1918-fremde Adressen können mit keiner Version der Funktion über ExpressRoute erreicht werden. Dazu müssen Sie gegenwärtig eine ASE verwenden.

Bei der regionalen VNET-Integration wird Ihr VNET weder mit dem lokalen Netzwerk verbunden noch werden Dienstendpunkte konfiguriert. Dafür ist eine separate Netzwerkkonfiguration erforderlich. Die regionale VNET-Integration ermöglicht es Ihrer App lediglich, Aufrufe für diese Verbindungstypen auszuführen.

Unabhängig von der verwendeten Version erhält Ihre Web-App durch die VNET-Integration Zugriff auf Ressourcen in Ihrem virtuellen Netzwerk, es wird aber kein eingehender privater Zugriff auf Ihre Web-App aus dem virtuellen Netzwerk gewährt. Privater Websitezugriff bezieht sich darauf, den Zugriff auf Ihre App nur über ein privates Netzwerk zuzulassen, z.B. aus einem virtuellen Azure-Netzwerk heraus. Die VNET-Integration dient nur zum Ausführen ausgehender Aufrufe von Ihrer App in Ihr VNET. 

Für die Funktion für die VNET-Integration gilt Folgendes:

* erfordert einen der Tarife Tarif Standard, Premium oder PremiumV2 
* TCP und UDP wird unterstützt
* funktioniert mit App Service-Apps und Funktions-Apps

Einige Dinge werden von der VNET-Integration nicht unterstützt, z.B.:

* Bereitstellung eines Laufwerks
* AD-Integration 
* NetBios

## <a name="regional-vnet-integration"></a>Regionale VNET-Integration 

> [!NOTE]
> Peering ist für Linux-basierten App Service noch nicht verfügbar.
>

Wenn Sie die VNET-Integration mit VNETs verwenden, die sich in der gleichen Region befinden wie Ihre App, ist ein delegiertes Subnetz mit mindestens 32 Adressen erforderlich. Das Subnetz kann nicht für andere Zwecke verwendet werden. Ausgehende Aufrufe von Ihrer App erfolgen von den Adressen im delegierten Subnetz. Bei dieser Version der VNET-Integration werden die Aufrufe von Adressen in Ihrem VNET ausgeführt. Die Verwendung von Adressen in Ihrem VNET ermöglicht Ihrer App Folgendes:

* Aufrufe an per Dienstendpunkt geschützte Dienste
* Zugriff auf Ressourcen über ExpressRoute-Verbindungen
* Zugriff auf Ressourcen im VNET, mit dem Sie verbunden sind
* Zugriff auf Ressourcen über Verbindungen mit Peering, einschließlich ExpressRoute-Verbindungen

Diese Funktion ist zurzeit als Vorschauversion verfügbar, wird aber mit folgenden Einschränkungen für Windows-App-Produktionsworkloads unterstützt:

* Sie können nur Adressen im RFC 1918-Bereich erreichen. d. h. Adressen in den Adressblöcken 10.0.0.0/8, 172.16.0.0/12 und 192.168.0.0/16.
* Sie können nicht über Verbindungen mit globalem Peering auf Ressourcen zugreifen.
* Sie können keine Routen für den Datenverkehr festlegen, der von Ihrer App in Ihr VNET gesendet wird.
* Die Funktion ist nur bei neueren App Service-Skalierungseinheiten verfügbar, die App Service-Pläne mit dem Tarif „PremiumV2“ unterstützen.
* Das Integrationssubnetz kann nur von einem App Service-Plan verwendet werden.
* Die Funktion kann nicht für Apps im Plan „App Service (isoliert)“ in einer App Service-Umgebung verwendet werden.
* Für das Feature ist ein nicht genutztes Subnetz erforderlich, also vom Typ /27 mit 32 Adressen oder höher in Ihrem Resource Manager-VNET.
* Die App und das VNET müssen sich in der gleichen Region befinden.
* Ein VNET mit einer integrierten App kann nicht gelöscht werden. Sie müssen zuerst die Integration entfernen. 
* Sie können nur eine regionale VNET-Integration pro App Service-Plan verwenden. Mehrere Apps im gleichen App Service-Plan können das gleiche VNET verwenden. 
* Sie können das Abonnement einer App oder eines App Service-Plans nicht ändern, solange eine App vorhanden ist, die eine regionale VNet-Integration verwendet.

Eine Adresse wird für jede Instanz des App Service-Plans verwendet. Wenn Sie Ihre App auf fünf Instanzen skaliert haben, werden fünf Adressen genutzt. Da die Subnetzgröße nach der Zuweisung nicht geändert werden kann, müssen Sie ein Subnetz verwenden, das für die Aufnahme jedweder Skalierung Ihrer App groß genug ist. Die empfohlene Größe ist A /26 mit 64 Adressen. A /27 mit 32 Adressen würde 20 Instanzen eines Premium-App Service-Plans aufnehmen, wenn Sie die Größe des App Service-Plans nicht geändert haben. Wenn Sie einen App Service-Plan zentral hoch- oder herunterskalieren, benötigen Sie für einen kurzen Zeitraum doppelt so viele Adressen. 

Wenn Sie möchten, dass Ihre Apps in einem anderen App Service-Plan ein VNET erreichen, das bereits mit Apps in einem anderen App Service-Plan verbunden ist, müssen Sie ein anderes Subnetz auswählen. Es darf nicht das Subnetz sein, das von der bereits vorhandenen VNET-Integration verwendet wird.  

Diese Funktion befindet sich auch für Linux in der Vorschauphase. So verwenden Sie die Funktion für die VNET-Integration mit einem Resource Manager-VNET in der gleichen Region:

1. Wechseln Sie zur Netzwerkbenutzeroberfläche im Portal. Wenn Ihre App die neue Funktion unterstützt, ist eine Option „VNET hinzufügen (Vorschauversion)“ verfügbar.  

   ![Auswählen der VNET-Integration][6]

1. Wählen Sie **VNET hinzufügen (Vorschau)** aus.  

1. Wählen Sie das Resource Manager-VNET aus, das Sie integrieren möchten, und erstellen Sie dann ein neues Subnetz, oder wählen Sie ein vorhandenes leeres Subnetz aus. Die Integration dauert weniger als eine Minute. Während der Integration wird Ihre App neu gestartet.  Nach Abschluss der Integration werden Details für das VNET, mit dem Sie integriert sind, angezeigt, und ein Banner oben informiert Sie, dass sich das Feature in der Vorschau befindet.

   ![Auswählen von VNET und Subnetz][7]

Nachdem Ihre App in Ihr VNET integriert wurde, verwendet sie den DNS-Server, mit dem Ihr VNET konfiguriert ist. 

Eine regionale VNET-Integration erfordert, dass Ihr Integrationssubnetz an Microsoft.Web delegiert wird.  Das Subnetz wird von der Benutzeroberfläche der VNet-Integration automatisch an Microsoft.Web delegiert. Wenn Ihr Konto nicht über ausreichende Netzwerkberechtigungen verfügt, um dies festzulegen, benötigen Sie eine Person, die in Ihrem Integrationssubnetz Attribute festlegen kann, um das Subnetz zu delegieren. Wenn Sie das Subnetz für die Integration manuell delegieren möchten, wechseln Sie zur Benutzeroberfläche des Azure Virtual Network-Subnetzes, und legen Sie Delegierung für Microsoft.Web fest.

Um Ihre App vom VNET zu trennen, wählen Sie **Verbindung trennen** aus. Hiermit wird Ihre Web-App neu gestartet. 


#### <a name="web-app-for-containers"></a>Web-App für Container

Wenn Sie App Service unter Linux mit den integrierten Images verwenden, funktioniert die regionale VNET-Integration ohne zusätzliche Änderungen. Wenn Sie Web-App für Container verwenden, müssen Sie Ihr Docker-Image ändern, um die VNET-Integration zu verwenden. Verwenden Sie in Ihrem Docker-Image die Umgebungsvariable PORT als Hauptlauschport des Webservers, anstatt eine hartcodierte Portnummer zu verwenden. Die Umgebungsvariable PORT wird von der App Service-Plattform automatisch beim Start des Containers festgelegt. Wenn Sie SSH verwenden, muss der SSH-Daemon so konfiguriert sein, dass er auf den Port mit der Nummer lauscht, die in der SSH_PORT-Umgebungsvariablen angegeben ist, wenn regionale VNET-Integration verwendet wird.

### <a name="service-endpoints"></a>Dienstendpunkte

Mit dem neuen VNET-Integrationsfeature können Sie Dienstendpunkte verwenden.  Um Dienstendpunkte mit Ihrer App zu verwenden, stellen Sie über die neue VNET-Integration eine Verbindung mit einem ausgewählten VNET her, und konfigurieren Sie dann Dienstendpunkte in dem für die Integration verwendeten Subnetz. 


### <a name="how-vnet-integration-works"></a>Funktionsweise der VNET-Integration

Apps in App Service werden auf Workerrollen gehostet. Der Tarif „Basic“ und höhere Tarife sind dedizierte Hostingpläne, bei denen keine anderen Kundenworkloads auf den gleichen Workern ausgeführt werden. Die VNET-Integration beruht auf der Einbindung virtueller Schnittstellen mit Adressen im delegierten Subnetz. Da sich die Von-Adresse in Ihrem VNET befindet, hat sie ähnlich wie eine VM im VNET Zugriff auf die meisten Ressourcen, die im oder über das VNET verfügbar sind. Die Netzwerkimplementierung unterscheidet sich von der Ausführung einer VM in Ihrem VNET. Einige Netzwerkfeatures sind daher bei der Verwendung dieser Funktion noch nicht verfügbar.

![VNET-Integration](media/web-sites-integrate-with-vnet/vnet-integration.png)

Wenn die VNET-Integration aktiviert ist, sendet Ihre App ausgehende Aufrufe an das Internet weiterhin über die üblichen Kanäle. Ihre App verwendet nach wie vor die ausgehenden Adressen, die im Portal für die App-Eigenschaften aufgeführt sind. Was sich für Ihre App ändert, sind Aufrufe an per Dienstendpunkt geschützte Dienste oder RFC 1918-Adressen. Diese Aufrufe werden in Ihr VNET geleitet. 

Die Funktion unterstützt nur eine virtuelle Schnittstelle pro Worker,  Eine virtuelle Schnittstelle pro Worker bedeutet eine regionale VNET-Integration pro App Service Plan. Alle Apps im gleichen App Service-Plan können die gleiche VNET-Integration verwenden. Wenn eine App jedoch eine Verbindung mit einem zusätzlichen VNET herstellen muss, müssen Sie jedoch einen weiteren App Service-Plan erstellen. Die verwendete virtuelle Schnittstelle ist keine Ressource, auf die Kunden direkten Zugriff haben.

Aufgrund der Funktionsweise dieser Technologie wird der Datenverkehr in Verbindung mit der VNET-Integration nicht in Network Watcher- oder NSG-Flussprotokollen (Netzwerksicherheitsgruppe) aufgeführt.  

## <a name="gateway-required-vnet-integration"></a>VNET-Integration, die ein Gateway erfordert 

Die Funktion für die von einem Gateway abhängige VNET-Integration:

* Kann zum Herstellen einer Verbindung mit VNETs in einer beliebigen Region verwendet werden (sowohl Resource Manager-VNETs als auch klassische VNETs)
* Eine App kann nur jeweils mit einem VNET eine Verbindung herstellen
* Bis zu fünf VNETs können in einen App Service-Plan integriert werden 
* Ermöglicht die Verwendung des gleichen VNET durch mehrere Apps, ohne dass sich dies auf die für einen App Service-Plan zulässige Gesamtanzahl von VNETs auswirkt.  (wenn Sie sechs Apps haben, die das gleiche VNET im gleichen App Service-Plan nutzen, zählt dies als ein VNET) 
* Erfordert ein virtuelles Netzwerkgateway, das mit Point-to-Site-VPN konfiguriert ist
* Unterstützt dank der SLA auf dem Gateway eine SLA mit einer Verfügbarkeit von 99,9%

Folgendes wird von dieser Funktion nicht unterstützt:
* Verwenden mit Linux-Apps
* Zugriff auf Ressourcen über ExpressRoute 
* Zugriff auf Ressourcen über Dienstendpunkte 

### <a name="getting-started"></a>Erste Schritte

Beachten Sie Folgendes, bevor Sie Ihre Web-App mit einem virtuellen Netzwerk verbinden:

* Ein virtuelles Zielnetzwerk muss über ein Point-to-Site-VPN mit einem dynamischen Routinggateway aktiviert werden, bevor es mit einer App verbunden werden kann. 
* Das VNET muss sich im gleichen Abonnement befinden wie Ihr App Service-Plan (ASP).
* Die Apps, die in ein VNET integriert sind, nutzen das DNS, das für das VNET angegeben ist.

### <a name="set-up-a-gateway-in-your-vnet"></a>Einrichten eines Gateways in Ihrem VNET ###

Wenn Sie bereits über ein Gateway verfügen, das mit Point-to-Site-Adressen konfiguriert ist, können Sie mit dem Konfigurieren der VNET-Integration mit Ihrer App fortfahren.  
So erstellen Sie ein Gateway

1. [Erstellen Sie ein Gatewaysubnetz][creategatewaysubnet] in Ihrem VNET.  

1. [Erstellen Sie das VPN-Gateway][creategateway]. Wählen Sie einen routenbasierten VPN-Typ aus.

1. [Legen Sie die Point-to-Site-Adressen fest][setp2saddresses]. Wenn sich das Gateway nicht in der Basic-SKU befindet, muss IKEV2 in der Point-to-Site-Konfiguration deaktiviert und SSTP ausgewählt werden. Der Adressraum muss in den RFC 1918-Adressblöcken 10.0.0.0/8, 172.16.0.0/12 und 192.168.0.0/16 liegen.

Wenn Sie das Gateway nur für die Verwendung mit der App Service-VNET-Integration erstellen, müssen Sie kein Zertifikat hochladen. Das Erstellen des Gateways kann 30 Minuten dauern. Sie können Ihre App erst mit Ihrem VNET integrieren, nachdem das Gateway bereitgestellt wurde. 

### <a name="configure-vnet-integration-with-your-app"></a>Konfigurieren der VNET-Integration mit Ihrer App 

So aktivieren Sie die VNET-Integration in Ihrer App 

1. Wechseln Sie im Azure-Portal zu Ihrer App, öffnen Sie die App-Einstellungen, und wählen Sie „Netzwerke > VNET-Integration“ aus. Bei beiden Versionen der VNET-Integration muss Ihr ASP mindestens in einer Standard-SKU enthalten sein, um die Funktion verwenden zu können. 
 ![Benutzeroberfläche der VNET-Integration][1]

1. Wählen Sie **VNET hinzufügen** aus. 
 ![Hinzufügen der VNET-Integration][2]

1. Wählen Sie Ihr VNET aus. 
  ![Auswählen Ihres VNET][8]
  
Nach diesem letzten Schritt wird Ihre App neu gestartet.  

### <a name="how-the-gateway-required-vnet-integration-feature-works"></a>Funktionsweise der VNET-Integration, die ein Gateway erfordert

Die Funktion für die von einem Gateway abhängige VNET-Integration basiert auf Point-to-Site-VPN-Technologie. Die Point-to-Site-Technologie beschränkt den Netzwerkzugriff auf den virtuellen Computer, auf dem die App gehostet wird. Apps sind darauf beschränkt, nur Datenverkehr an das Internet über Hybrid Connections oder VNET-Integration zu senden. 

![Funktionsweise der VNET-Integration][3]

## <a name="managing-vnet-integration"></a>Verwalten der VNET-Integration
Die Möglichkeit, eine Verbindung mit einem VNET herzustellen und zu trennen, ist auf der App-Ebene angesiedelt. Vorgänge, die sich auf die VNET-Integration über mehrere Apps auswirken können, werden auf Ebene des App Service-Plans ausgeführt. Sie können im Portal unter „App > Netzwerk > VNET-Integration“ Details zu Ihrem VNET abrufen. Ähnliche Informationen sind auf ASP-Ebene im Portal unter „App Service-Plan > Netzwerk > VNET-Integration“ verfügbar. Dort sehen Sie auch, welche Apps im App Service-Plan eine bestimmte Integration verwenden.

 ![Details zum VNET][4]

Die in der Benutzeroberfläche der VNET-Integration verfügbaren Informationen sind in den App- und ASP-Portalen identisch.

* VNET-Name: Link zur Benutzeroberfläche des virtuellen Netzwerks
* Standort: gibt den Standort Ihres VNET an. Die Integration mit einem VNET an einem anderen Standort kann Latenzprobleme für Ihre App verursachen. 
* Zertifikatstatus: gibt an, ob Ihre Zertifikate zwischen Ihrem App Service-Plan und dem VNET synchron sind.
* Gatewaystatus: Wenn Sie die von einem Gateway abhängige VNET-Integration verwenden, wird der Gatewaystatus angezeigt.
* VNET-Adressraum: zeigt den IP-Adressraum für Ihr VNET an. 
* Punkt-zu-Standort-Adressraum: zeigt den Point-to-Site-IP-Adressraum für Ihr VNET an. Wenn Sie bei Verwendung der von einem Gateway abhängigen Funktion Aufrufe an Ihr VNET ausführen, ist die VON-Adresse Ihrer App eine dieser Adressen. 
* Standort-zu-Standort-Adressraum: Sie können Site-to-Site-VPNs verwenden, um Ihr VNET mit lokalen Ressourcen oder den anderen VNETs zu verbinden. Die mit dieser VPN-Verbindung definierten IP-Adressbereiche werden hier dargestellt.
* DNS-Server: zeigt die mit Ihrem VNET konfigurierten DNS-Server.
* IP addresses routed to the VNet (An das VNET weitergeleitete IP-Adressen): zeigt die Adressblöcke, die zur Weiterleitung in Ihr VNET geroutet werden 

Der einzige Vorgang, den Sie in der App-Ansicht Ihrer VNET-Integration durchführen können, ist das Trennen Ihrer App von dem VNET, mit dem derzeit eine Verbindung besteht. Um Ihre App von einem VNET zu trennen, wählen Sie **Verbindung trennen** aus. Wenn Sie die Verbindung mit einem VNET trennen, wird Ihre App neu gestartet. Das Trennen der Verbindung führt nicht zu Änderungen in Ihrem VNET. Das Subnetz oder Gateway wird nicht entfernt. Wenn Sie Ihr VNET löschen möchten, müssen Sie zuerst Ihre App vom VNET trennen und die darin enthaltenen Ressourcen löschen (z. B. Gateways). 

Um die Benutzeroberfläche der ASP-VNET-Integration zu erreichen, öffnen Sie die ASP-Benutzeroberfläche, und wählen Sie **Netzwerk** aus.  Wählen Sie unter „VNET-Integration“ die Option **Zum Konfigurieren hier klicken** aus, um die Netzwerkfeaturestatus-Benutzeroberfläche zu öffnen.

![Informationen zur ASP-VNET-Integration][5]

Die Benutzeroberfläche der ASP-VNET-Integration zeigt alle VNETs an, die von den Apps in Ihrem ASP verwendet werden. Klicken Sie auf das gewünschte VNET, um Informationen dazu anzuzeigen. Sie können hier zwei Aktionen durchführen.

* **Netzwerk synchronisieren**. Der Vorgang zum Synchronisieren des Netzwerks ist nur für die von einem Gateway abhängige VNET-Integration vorgesehen. Durch die Netzwerksynchronisierung wird sichergestellt, dass Ihre Zertifikate und Netzwerkinformationen synchronisiert sind. Wenn Sie das DNS des VNET hinzufügen oder ändern, müssen Sie eine **Netzwerksynchronisierung** durchführen. Bei diesem Vorgang werden alle Apps, die dieses VNET verwenden, neu gestartet.
* **Routen hinzufügen:** Durch das Hinzufügen von Routen wird ausgehender Datenverkehr in Ihr VNET geleitet.

**Routing:** Die in Ihrem VNET definierten Routen werden zum Leiten des Datenverkehrs aus der App in Ihr VNET verwendet. Wenn Sie weiteren ausgehenden Datenverkehr in das VNET senden möchten, können Sie diese Adressblöcke hier hinzufügen. Diese Funktion kann nur mit der VNET-Integration verwendet werden, die ein Gateway erfordert.

**Zertifikate:** Wenn die von einem Gateway abhängige VNET-Integration aktiviert ist, müssen Zertifikate ausgetauscht werden, um die Sicherheit der Verbindung zu gewährleisten. Zusammen mit den Zertifikaten werden die DNS-Konfiguration, Routen und anderen Elemente, mit denen das Netzwerk beschrieben wird, ausgetauscht.
Wenn Zertifikate oder Netzwerkinformationen geändert werden, müssen Sie auf „Netzwerk synchronisieren“ klicken. Wenn Sie auf „Netzwerk synchronisieren“ klicken, bewirkt dies einen kurzen Ausfall der Verbindung zwischen der App und dem VNET. Die App wird nicht neu gestartet, aber der Konnektivitätsverlust kann dazu führen, dass Ihre Website nicht richtig funktioniert. 

## <a name="accessing-on-premises-resources"></a>Zugriff auf lokale Ressourcen
Apps können durch Integration mit VNETs, die Site-to-Site-Verbindungen aufweisen, auf lokale Ressourcen zugreifen. Wenn Sie die von einem Gateway abhängige VNET-Integration verwenden, müssen Sie Ihre lokalen VPN-Gatewayrouten mit Ihren Point-to-Site-Adressblöcken aktualisieren. Verwenden Sie beim ersten Einrichten des Site-to-Site-VPN die Skripts für die Konfiguration. Die Routen sollten damit richtig eingerichtet werden. Wenn Sie die Point-to-Site-Adressen nach dem Erstellen des Site-to-Site-VPN hinzufügen, müssen Sie die Routen manuell aktualisieren. Die Details zur Vorgehensweise variieren je nach Gateway und sind hier nicht beschrieben. Das Border Gateway Protocol (BGP) kann nicht für eine Site-to-Site-VPN-Verbindung konfiguriert werden.

Für die regionale VNET-Integration ist keine zusätzliche Konfiguration erforderlich, um Ihr VNET und das lokale Netzwerk zu erreichen. Sie müssen Ihr VNET lediglich über ExpressRoute oder ein Site-to-Site-VPN mit dem lokalen Netzwerk verbinden. 

> [!NOTE]
> Bei der von einem Gateway abhängigen VNET-Integration wird eine App nicht in ein VNET integriert, das ein ExpressRoute-Gateway enthält. Selbst wenn das ExpressRoute-Gateway im [Koexistenzmodus][VPNERCoex] konfiguriert ist, funktioniert die VNET-Integration nicht. Wenn Sie über eine ExpressRoute-Verbindung auf Ressourcen zugreifen müssen, können Sie die regionale VNET-Integration oder eine in Ihrem VNET ausgeführte [App Service-Umgebung][ASE] verwenden. 
> 
> 

## <a name="peering"></a>Peering
Wenn Sie das Peering mit der regionalen VNET-Integration verwenden, ist keine zusätzliche Konfiguration erforderlich. 

Bei der von einem Gateway abhängigen VNET-Integration müssen Sie für das Peering einige zusätzliche Elemente konfigurieren. So konfigurieren Sie Peering für Ihre App

1. Fügen Sie eine Peeringverbindung mit dem VNET hinzu, mit dem Ihre App eine Verbindung herstellt. Aktivieren Sie beim Hinzufügen der Peeringverbindung **Zugriff auf virtuelles Netzwerk zulassen** sowie **Weitergeleiteten Datenverkehr zulassen** und **Gatewaytransit zulassen**.
1. Fügen Sie in dem VNET, das mit dem VNET, mit dem Sie verbunden sind, mittels Peering verknüpft ist, eine Peeringverbindung hinzu. Aktivieren Sie beim Hinzufügen der Peeringverbindung im Ziel-VNET **Zugriff auf virtuelles Netzwerk zulassen**, und aktivieren Sie **Weitergeleiteten Datenverkehr zulassen** und **Allow remote gateways** (Remotegateways zulassen).
1. Wechseln Sie im Portal zu „App Service-Plan > Netzwerk > VNet Integration UI“ (Benutzeroberfläche der VNET-Integration).  Wählen Sie das VNET aus, mit dem Ihre App eine Verbindung herstellt. Fügen Sie den Adressbereich des VNET, das mit dem VNET, mit dem Ihre App verbunden ist, mittels Peering verknüpft ist, im Abschnitt „Routing“ hinzu.  


## <a name="pricing-details"></a>Preisübersicht
Bei der Funktion für die regionale VNET-Integration fallen neben den Gebühren für den App Service-Tarif keine zusätzlichen Gebühren an.

Bei der VNET-Integration, die ein Gateway erfordert, fallen drei Gebühren an:

* Gebühren für den App Service-Tarif: Ihre Apps müssen in einem App Service-Plan mit dem Tarif „Standard“, „Premium“ oder „PremiumV2“ enthalten sein. Weitere Informationen zu diesen Kosten finden Sie hier: [App Service – Preise][ASPricing] 
* Datenübertragungskosten: Für ausgehende Daten fallen keine Gebühren an, auch wenn sich das VNET im selben Rechenzentrum befindet. Diese Kosten werden unter [Datenübertragung – Preisübersicht][DataPricing] beschrieben. 
* Kosten für VPN Gateway: Für das VNET-Gateway, das für das Point-to-Site-VPN erforderlich ist, fallen Kosten an. Die Details finden Sie auf der Seite [VPN Gateway – Preise][VNETPricing].


## <a name="troubleshooting"></a>Problembehandlung
Die Funktion ist zwar einfach einzurichten, aber dies bedeutet nicht, dass für Ihre Benutzeroberfläche keinerlei Probleme auftreten. Falls beim Zugreifen auf den gewünschten Endpunkt Probleme auftreten, können Sie einige Hilfsprogramme verwenden, um die Verbindung über die App-Konsole zu testen. Sie können zwei Konsolen verwenden. Eine ist die Kudu-Konsole, und die andere ist die Konsole im Azure-Portal. Greifen Sie in der App auf „Tools -> Kudu“ zu, um zur Kudu-Konsole zu gelangen. Sie können auch die Kudo-Konsole unter „[sitename].scmn.azurewebsites.net“ erreichen. Wechseln Sie nach dem Laden der Website zur Konsolenregisterkarte „Debuggen“. Um auf die über das Azure-Portal gehostete Konsole zuzugreifen, greifen Sie in der App auf „Tools“ -> „Konsole“ zu. 

#### <a name="tools"></a>Tools
Die Tools **ping**, **nslookup** und **tracert** funktionieren aufgrund von Sicherheitseinschränkungen nicht über die Konsole. Es wurden zwei separate Tools hinzugefügt, um diese Lücke zu füllen. Zum Testen der DNS-Funktionalität haben wir ein Tool mit dem Namen „nameresolver.exe“ hinzugefügt. Die Syntax ist:

    nameresolver.exe hostname [optional: DNS Server]

Sie können **nameresolver** verwenden, um die Hostnamen zu überprüfen, von denen Ihre App abhängig ist. So können Sie testen, ob für das DNS etwas falsch konfiguriert ist oder ob ggf. kein Zugriff auf Ihren DNS-Server besteht. Den von Ihrer App verwendeten DNS-Server können Sie in der Konsole in den Umgebungsvariablen WEBSITE_DNS_SERVER und WEBSITE_DNS_ALT_SERVER einsehen.

Mit dem nächsten Tool können Sie die TCP-Verbindung mit einer Host/Port-Kombination testen. Dieses Tool hat den Namen **tcpping** und die folgende Syntax:

    tcpping.exe hostname [optional: port]

Das **tcpping**-Hilfsprogramm teilt Ihnen mit, ob Sie einen bestimmten Host und Port erreichen können. Es kann nur unter folgenden Bedingungen erfolgreich ausgeführt werden: Eine Anwendung lauscht auf der Host- und Portkombination, und von Ihrer App aus ist Netzwerkzugriff auf den angegebenen Host und Port möglich.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Debuggen des Zugriffs auf im VNET gehostete Ressourcen
Es kann verschiedene Ursachen haben, warum Ihre App einen bestimmten Host und Port nicht erreichen kann. In den meisten Fällen liegt eine der drei folgenden Ursachen vor:

* **Eine Firewall.** Falls eine Firewall den Zugriff verhindert, wird das TCP-Timeout ausgelöst. Das TCP-Timeout entspricht hier 21 Sekunden. Überprüfen Sie die Verbindung mithilfe des **tcpping**-Tools. TCP-Timeouts können zwar auch zahlreiche andere Ursachen haben, es empfiehlt sich allerdings, bei der Firewall zu beginnen. 
* **Kein DNS-Zugriff.** Das DNS-Timeout beträgt drei Sekunden pro DNS-Server. Wenn Sie zwei DNS-Server besitzen, beträgt das Timeout 6 Sekunden. Überprüfen Sie mithilfe des nameresolver-Tools, ob DNS funktioniert. Zur Erinnerung: Das nslookup-Tool kann nicht verwendet werden, da es nicht das DNS verwendet, mit dem Ihr VNET konfiguriert ist. Dieses Problem kann darauf zurückzuführen sein, dass eine Firewall oder Netzwerksicherheitsgruppe den Zugriff auf das DNS blockiert.

Sollte das Problem weiterhin bestehen, überprüfen Sie zunächst Folgendes: 

**Regionale VNET-Integration**
* Ist Ihr Ziel eine RFC 1918-Adresse?
* Blockiert eine Netzwerksicherheitsgruppe ausgehenden Datenverkehr aus Ihrem Integrationssubnetz?
* Bei einer Verbindung über ExpressRoute oder ein VPN: Ist Ihr lokales Gateway zum Zurückleiten von Datenverkehr an Azure konfiguriert? Wenn Sie die Endpunkte in Ihrem VNET erreichen können, aber nicht lokal, ist es sinnvoll, dies zu überprüfen.

**VNET-Integration, die ein Gateway erfordert**
* Liegt der Point-to-Site-Adressbereich in den RFC 1918-Bereichen (10.0.0.0-10.255.255.255/172.16.0.0-172.31.255.255/192.168.0.0-192.168.255.255)?
* Wird im Portal angezeigt, dass das Gateway ausgeführt wird? Fahren Sie das Gateway hoch, wenn es heruntergefahren ist.
* Werden Zertifikate als synchronisiert angezeigt, oder vermuten Sie, dass die Netzwerkkonfiguration geändert wurde?  Wenn Ihre Zertifikate nicht synchronisiert sind oder Sie vermuten, dass an Ihrer VNET-Konfiguration eine Änderung vorgenommen wurde, die nicht mit Ihren ASPs synchronisiert wurde, klicken Sie auf „Netzwerk synchronisieren“.
* Bei einer Verbindung über ExpressRoute oder ein VPN: Ist Ihr lokales Gateway zum Zurückleiten von Datenverkehr an Azure konfiguriert? Wenn Sie die Endpunkte in Ihrem VNET erreichen können, aber nicht lokal, ist es sinnvoll, dies zu überprüfen.

Das Debuggen von Netzwerkproblemen ist eine Herausforderung, da Sie nicht sehen können, was den Zugriff auf eine bestimmte Host:Port-Kombination blockiert. Mögliche Ursachen sind:

* Aktivierte Firewall auf Ihrem Host, die den Zugriff auf den Anwendungsport über Ihren Punkt-zu-Standort-IP-Bereich verhindert Gegebenenfalls erforderlicher öffentlicher Zugriff für die Durchquerung von Subnetzen
* Zielhost ist ausgefallen
* Anwendung ist nicht verfügbar
* Falsche IP-Adresse oder falscher Hostname
* Ihre Anwendung lauscht über andere Ports als von Ihnen erwartet. Sie können Ihre Prozess-ID auf den lauschenden Port festlegen, indem Sie auf dem Endpunkthost „netstat -aon“ verwenden. 
* Netzwerksicherheitsgruppen sind so konfiguriert, dass der Zugriff auf Ihren Anwendungshost und -port aus Ihrem Punkt-zu-Standort-IP-Bereich verhindert wird

Denken Sie daran, dass Sie nicht wissen, welche Adresse Ihre App tatsächlich verwendet. Es kann sich um eine beliebige Adresse im Integrationssubnetz oder Point-to-Site-Adressbereich handeln. Sie müssen daher den Zugriff vom gesamten Adressbereich zulassen. 

Weitere Debugschritte:

* Stellen Sie eine Verbindung mit einer VM im VNET her, und versuchen Sie, die Ressource host:port von dort aus zu erreichen. Um den TCP-Zugriff zu testen, verwenden Sie den PowerShell-Befehl **test-netconnection**. Die Syntax ist:

      test-netconnection hostname [optional: -Port]

* Rufen Sie eine Anwendung auf einer VM auf, und testen Sie den Zugriff auf den jeweiligen Host und Port über die Konsole der App mit **tcpping**.

#### <a name="on-premises-resources"></a>Lokale Ressourcen ####

Wenn Ihre App eine Ressource lokal nicht erreichen kann, überprüfen Sie, ob Sie die Ressource über Ihr VNET erreichen können. Verwenden Sie den PowerShell-Befehl **test-netconnection**, um den TCP-Zugriff zu überprüfen. Wenn Ihre VM die lokale Ressource nicht erreichen kann, ist Ihre VPN- oder ExpressRoute-Verbindung möglicherweise nicht richtig konfiguriert.

Wenn die im VNET gehostete VM auf ein lokales System zugreifen kann, die App jedoch nicht, trifft wahrscheinlich einer der folgenden Gründe zu:

* Die Routen sind in Ihrem lokalen Gateway nicht mit Ihren Subnetz- oder Point-to-Site-Adressbereichen konfiguriert.
* Die Netzwerksicherheitsgruppen blockieren den Zugriff auf den Point-to-Site-IP-Bereich.
* Die lokalen Firewalls blockieren den Datenverkehr des Point-to-Site-IP-Bereichs.
* Sie versuchen, eine RFC 1918-fremde Adresse mit der Funktion für die regionale VNET-Integration zu erreichen.


## <a name="powershell-automation"></a>PowerShell-Automatisierung

Sie können App Service mithilfe von PowerShell in ein virtuelles Azure-Netzwerk integrieren. Ein ausführungsbereites Skript finden Sie unter [Verbinden einer App in Azure App Service mit einem virtuellen Azure-Netzwerk](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-details.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-aspdetails.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-newvnet.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-newvnetdetails.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-selectvnet.png

<!--Links-->
[VNETOverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ASEintro]: environment/intro.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
