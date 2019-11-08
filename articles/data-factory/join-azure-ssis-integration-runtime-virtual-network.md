---
title: Beitreten einer Azure-SSIS-Integrationslaufzeit zu einem virtuellen Netzwerk
description: Erfahren Sie, wie Sie eine Azure-SSIS Integration Runtime mit einem Azure Virtual Network verknüpfen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: d36900a1ce05eaf022637a6ef6b866fe0d190b17
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73672736"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Beitreten einer Azure-SSIS-Integrationslaufzeit zu einem virtuellen Netzwerk
Bei Verwendung von SQL Server Integration Services (SSIS) in Azure Data Factory sollten Sie in folgenden Szenarien die Azure-SSIS Integration Runtime (IR) mit einem Azure Virtual Network verknüpfen: 

- Sie möchten über SSIS-Pakete, die in der Azure-SSIS IR ausgeführt werden, eine Verbindung mit lokalen Datenspeichern herstellen, ohne eine selbstgehostete IR als Proxy zu konfigurieren oder zu verwalten. 

- Sie möchten über SSIS-Pakete, die in der Azure-SSIS IR ausgeführt werden, eine Verbindung mit Azure-Dienstressourcen herstellen, die mit VNET-Dienstendpunkten unterstützt werden.

- Sie hosten eine SSIS-Katalogdatenbank (SSISDB) in Azure SQL-Datenbank mit VNET-Dienstendpunkten oder einer verwalteten Instanz in einem virtuellen Netzwerk. 

Mit Data Factory können Sie die Azure-SSIS IR mit einem virtuellen Netzwerk verknüpfen, das über das klassische Bereitstellungsmodell oder das Azure Resource Manager-Bereitstellungsmodell erstellt wurde. 

> [!IMPORTANT]
> Das klassische virtuelle Netzwerk wird gerade als veraltet eingestuft. Daher sollten Sie stattdessen das virtuelle Azure Resource Manager-Netzwerk verwenden.  Wenn Sie das klassische virtuelle Netzwerk bereits verwenden, wechseln Sie so bald wie möglich zum virtuellen Azure Resource Manager-Netzwerk.

## <a name="access-to-on-premises-data-stores"></a>Zugriff auf lokale Datenspeicher
Wenn SSIS-Pakete auf lokale Datenspeicher zugreifen, können Sie die Azure-SSIS IR mit einem virtuellen Netzwerk verknüpfen, das mit dem lokalen Netzwerk verbunden ist. Sie können auch eine selbstgehostete IR als Proxy für die Azure-SSIS IR konfigurieren oder verwalten. Weitere Informationen finden Sie unter [Konfigurieren einer selbstgehosteten IR als Proxy für Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). 

Wenn Sie die Azure-SSIS IR mit einem virtuellen Netzwerk verknüpfen, sind einige wichtige Punkte zu beachten: 

- Wenn mit Ihrem lokalen Netzwerk kein virtuelles Netzwerk verbunden ist, erstellen Sie zunächst ein [virtuelles Azure Resource Manager-Netzwerk](../virtual-network/quick-create-portal.md#create-a-virtual-network), mit dem die Azure-SSIS IR verknüpft werden kann. Konfigurieren Sie dann eine Site-to-Site-[VPN-Gatewayverbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) oder eine [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md)-Verbindung von diesem virtuellen Netzwerk zu Ihrem lokalen Netzwerk. 

- Wenn bereits ein virtuelles Azure Resource Manager-Netzwerk am selben Standort wie die Azure-SSIS IR mit Ihrem lokalen Netzwerk verbunden ist, können Sie die IR mit diesem virtuellen Netzwerk verknüpfen. 

- Wenn bereits ein klassisches virtuelles Netzwerk an einem anderen Standort als die Azure-SSIS IR mit Ihrem lokalen Netzwerk verbunden ist, können Sie ein [virtuelles Azure Resource Manager-Netzwerk](../virtual-network/quick-create-portal.md#create-a-virtual-network) erstellen, mit dem die Azure-SSIS IR verknüpft werden kann. Konfigurieren Sie dann eine [Verbindung zwischen dem klassischen virtuellen Netzwerk und dem virtuellen Azure Resource Manager-Netzwerk](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md). 
 
- Wenn bereits ein virtuelles Azure Resource Manager-Netzwerk an einem anderen Standort als die Azure-SSIS IR mit Ihrem lokalen Netzwerk verbunden ist, können Sie zuerst ein [virtuelles Azure Resource Manager-Netzwerk](../virtual-network/quick-create-portal.md##create-a-virtual-network) erstellen, mit dem die Azure-SSIS IR verknüpft werden kann. Konfigurieren Sie dann eine Verbindung zwischen den virtuellen Azure Resource Manager-Netzwerken. 

## <a name="access-to-azure-services"></a>Zugriff auf Azure-Dienste
Wenn Ihre SSIS-Pakete auf Azure-Dienstressourcen zugreifen, die mit [VNET-Dienstendpunkten](../virtual-network/virtual-network-service-endpoints-overview.md) unterstützt werden, und Sie diese Ressourcen in der Azure-SSIS IR absichern möchten, können Sie Ihre Azure-SSIS IR dem mit VNET-Dienstendpunkten konfigurierten Subnetz des virtuellen Netzwerks hinzufügen. Fügen Sie in der Zwischenzeit den Azure-Dienstressourcen eine Regel für virtuelle Netzwerke hinzu, um den Zugriff aus demselben Subnetz zuzulassen.

## <a name="hosting-the-ssis-catalog-in-sql-database"></a>Hosten des SSIS-Katalogs in der SQL-Datenbank
Wenn Sie Ihren SSIS-Katalog in Azure SQL-Datenbank mit VNET-Dienstendpunkten hosten, stellen Sie sicher, dass Sie Azure-SSIS IR mit demselben virtuellen Netzwerk und Subnetz verknüpfen.

Um die Azure-SSIS IR mit demselben virtuellen Netzwerk wie die verwaltete Instanz zu verknüpfen, stellen Sie sicher, dass sich die Azure-SSIS IR in einem anderen Subnetz als die verwaltete Instanz befindet. Wenn Sie die Azure-SSIS IR mit einem anderen virtuellen Netzwerk als die verwaltete Instanz verknüpfen möchten, empfiehlt sich das Peering virtueller Netzwerke (das auf dieselbe Region beschränkt ist) oder eine Verbindung zwischen den virtuellen Netzwerken. Weitere Informationen finden Sie unter [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-connect-app.md).

In allen Fällen kann das virtuelle Netzwerk nur über das Azure Resource Manager-Bereitstellungsmodell bereitgestellt werden.

Die folgenden Abschnitte enthalten hierzu weitere Informationen. 

## <a name="virtual-network-configuration"></a>Konfiguration von virtuellen Netzwerken

Achten Sie bei der Einrichtung des virtuellen Netzwerks auf die Einhaltung der folgenden Anforderungen: 

-   Stellen Sie sicher, dass `Microsoft.Batch` ein registrierter Anbieter unter dem Abonnement Ihres Subnetzes des virtuellen Netzwerks ist, in dem die Azure-SSIS IR gehostet wird. Fügen Sie der Rolle „Mitwirkender für klassische virtuelle Computer“ für das virtuelle Netzwerk auch `MicrosoftAzureBatch` hinzu, wenn Sie ein klassisches virtuelles Netzwerk verwenden. 

-   Stellen Sie sicher, dass Sie über die erforderlichen Berechtigungen verfügen. Weitere Informationen finden Sie unter [Einrichten von Berechtigungen](#perms).

-   Wählen Sie das richtige Subnetz zum Hosten der Azure-SSIS Integration Runtime aus. Weitere Informationen finden Sie unter [Auswählen des Subnetzes](#subnet). 

-   Wenn Sie Ihren eigenen DNS (Domain Name System)-Server im virtuellen Netzwerk verwenden, finden Sie weitere Informationen unter [Einrichten des DNS-Servers](#dns_server). 

-   Wenn Sie eine Netzwerksicherheitsgruppe (NSG) im Subnetz verwenden, finden Sie Weitere Informationen unter [Einrichten einer NSG](#nsg). 

-   Wenn Sie Azure ExpressRoute oder eine benutzerdefinierte Route (User-Defined Route, UDR) verwenden, finden Sie weitere Informationen unter [Verwenden von Azure ExpressRoute oder einer UDR](#route). 

-   Stellen Sie sicher, dass durch die Ressourcengruppe des virtuellen Netzwerks bestimmte Azure-Netzwerkressourcen erstellt und gelöscht werden können. Weitere Informationen finden Sie unter [Einrichten der Ressourcengruppe](#resource-group). 

-   Wenn Sie die Azure-SSIS IR anpassen, wie im Thema zur [benutzerdefinierten Einrichtung der Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) beschrieben, erhalten die Azure-SSIS IR-Knoten private IP-Adressen aus einem vordefinierten Bereich von 172.16.0.0 bis 172.31.255.255. Daher sollten Sie sicherstellen, dass die Bereiche privater IP-Adressen Ihrer virtuellen oder lokalen Netzwerke nicht mit diesem Bereich kollidieren.

Dieses Diagramm veranschaulicht die für die Azure-SSIS IR erforderlichen Verbindungen:

![Azure-SSIS-Integrationslaufzeit](media/join-azure-ssis-integration-runtime-virtual-network/azure-ssis-ir.png)

### <a name="perms"></a> Einrichten von Berechtigungen

Der Benutzer, der die Azure-SSIS IR erstellt, muss über die folgenden Berechtigungen verfügen:

- Wenn Sie Ihre SSIS IR in ein virtuelles Azure Resource Manager-Netzwerk einbinden, haben Sie zwei Möglichkeiten:

  - Verwenden Sie die integrierte Rolle „Netzwerkmitwirkender“. Diese Rolle umfasst die Berechtigung _Microsoft.Network/\*_ , die jedoch einen deutlich größeren Umfang als erforderlich hat.

  - Erstellen Sie eine benutzerdefinierte Rolle, die nur die erforderliche Berechtigung _Microsoft.Network/virtualNetworks/\*/join/action_ aufweist. 

- Wenn Sie die SSIS IR mit einem klassischen virtuellen Netzwerk verknüpfen, wird empfohlen, die integrierte Rolle „Mitwirkender für klassische virtuelle Computer“ zu verwenden. Andernfalls müssen Sie eine benutzerdefinierte Rolle definieren, die die Berechtigung zum Einbinden in das virtuelle Netzwerk enthält.

### <a name="subnet"></a> Auswählen des Subnetzes

Hinweise zur Auswahl eines Subnetzes: 

-   Sie sollten nicht das GatewaySubnet zum Bereitstellen einer Azure-SSIS IR auswählen, da es Gateways für virtuelle Netzwerke vorbehalten ist. 

-   Stellen Sie sicher, dass das ausgewählte Subnetz ausreichend verfügbaren Adressraum für die Azure-SSIS IR aufweist. Sie benötigen mindestens doppelt so viele verfügbare IP-Adressen, wie IR-Knoten vorhanden sind. Einige IP-Adressen innerhalb jedes Subnetzes sind in Azure reserviert. Diese Adressen können nicht verwendet werden. Die erste und letzte IP-Adresse der Subnetze sind aus Gründen der Protokollkonformität reserviert. Darüber hinaus werden drei weitere Adressen für Azure-Dienste verwendet. Weitere Informationen finden Sie unter [Unterliegen die in den Subnetzen verwendeten IP-Adressen bestimmten Beschränkungen?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets) 

-   Verwenden Sie kein Subnetz, das ausschließlich von anderen Azure-Diensten genutzt wird (z. B. für verwaltete SQL-Datenbank-Instanzen, App Services usw.). 

### <a name="dns_server"></a> Einrichten des DNS-Servers 
Wenn Sie Ihren eigenen DNS-Server in einem virtuellen Netzwerk verwenden möchten, das mit der Azure-SSIS IR verknüpft ist, sollten Sie sicherstellen, dass globale Azure-Hostnamen aufgelöst werden können (z. B. der Azure Storage-Blobname `<your storage account>.blob.core.windows.net`). 

Die folgenden Schritte werden empfohlen: 

-   Konfigurieren Sie den benutzerdefinierten DNS für die Weiterleitung von Anforderungen an Azure DNS. Sie können nicht aufgelöste DNS-Einträge an die IP-Adresse der rekursiven Azure-Resolver (168.63.129.16) auf Ihrem eigenen DNS-Server weiterleiten. 

-   Richten Sie den benutzerdefinierten DNS als primären DNS-Server für das virtuelle Netzwerk ein. Richten Sie Azure DNS als sekundären DNS-Server ein. Registrieren Sie die IP-Adresse der rekursiven Azure-Resolver (168.63.129.16) als sekundären DNS-Server, falls Ihr eigener DNS-Server nicht verfügbar ist. 

Weitere Informationen finden Sie unter [Namensauflösung mithilfe eines eigenen DNS-Servers](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). 

### <a name="nsg"></a> Einrichten einer NSG
Wenn Sie eine NSG für das Subnetz implementieren müssen, das von der Azure-SSIS IR verwendet wird, lassen Sie eingehenden und ausgehenden Datenverkehr über die folgenden Ports zu: 

| Direction | Transportprotokoll | `Source` | Quellportbereich | Destination | Destination port range | Kommentare |
|---|---|---|---|---|---|---|
| Eingehend | TCP | BatchNodeManagement | * | VirtualNetwork | 29876, 29877 (wenn Sie die IR mit einem virtuellen Resource Manager-Netzwerk verknüpfen) <br/><br/>10100, 20100, 30100 (wenn Sie die IR mit einem klassischen virtuellen Netzwerk verknüpfen)| Der Data Factory-Dienst nutzt diese Ports für die Kommunikation mit den Knoten der Azure-SSIS IR im virtuellen Netzwerk. <br/><br/> Unabhängig davon, ob Sie eine NSG auf Subnetzebene erstellen, konfiguriert Data Factory immer eine NSG auf der Ebene der Netzwerkschnittstellenkarten (NICs), die an die virtuellen Computer angefügt sind, auf denen die Azure-SSIS IR gehostet wird. Nur eingehenden Datenverkehr von Data Factory-IP-Adressen für die angegebenen Ports wird durch diese NSG auf NIC-Ebene zugelassen. Auch wenn Sie diese Ports für den Internetdatenverkehr auf Subnetzebene öffnen, wird Datenverkehr von anderen IP-Adressen als Data Factory-IP-Adressen auf NIC-Ebene blockiert. |
| Ausgehend | TCP | VirtualNetwork | * | AzureCloud | 443 | Die Knoten Ihrer Azure-SSIS IR im virtuellen Netzwerk verwenden diesen Port für den Zugriff auf Azure-Dienste wie Azure Storage und Azure Event Hubs. |
| Ausgehend | TCP | VirtualNetwork | * | Internet | 80 | Die Knoten Ihrer Azure-SSIS IR im virtuellen Netzwerk verwenden diesen Port zum Herunterladen einer Zertifikatssperrliste aus dem Internet. |
| Ausgehend | TCP | VirtualNetwork | * | Sql | 1433, 11000–11999 | Für die Knoten Ihrer Azure-SSIS IR im virtuellen Netzwerk werden diese Ports verwendet, um auf die von Ihrem SQL-Datenbank-Server gehostete SSISDB zuzugreifen. Wenn die Verbindungsrichtlinie Ihres SQL-Datenbank-Servers anstatt auf **Redirect** auf **Proxy** festgelegt ist, wird nur Port 1433 benötigt. Diese Ausgangssicherheitsregel gilt nicht für eine SSISDB, die von der verwalteten Instanz im virtuellen Netzwerk gehostet wird. |
||||||||

### <a name="route"></a> Verwenden von Azure ExpressRoute oder einer UDR
Wenn Sie eine [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/)-Leitung mit Ihrer virtuellen Netzwerkinfrastruktur verbinden, um Ihr lokales Netzwerk auf Azure auszuweiten, wird bei einer allgemeinen Konfiguration die Tunnelerzwingung verwendet (Ankündigung einer BGP-Route – 0.0.0.0/0 – gegenüber dem virtuellen Netzwerk). Bei diesem Tunneln wird erzwungen, dass der ausgehende Internetdatenverkehr vom virtuellen Netzwerk zu Prüf- und Protokollierungszwecken an eine lokale Netzwerkappliance geroutet wird. 
 
Alternativ können Sie [UDRs](../virtual-network/virtual-networks-udr-overview.md) definieren, um zu erzwingen, dass ausgehender Internetdatenverkehr von dem Subnetz, von dem die Azure-SSIS IR gehostet wird, zu einem anderen Subnetz geroutet wird, von dem zu Prüf- oder Protokollierungszwecken eine virtuelle Netzwerkappliance (Network Virtual Appliance, NVA) als Firewall oder Azure Firewall gehostet wird. 

In beiden Fällen wird die für eingehende Verbindungen erforderliche Datenverkehrsroute von abhängigen Azure Data Factory-Diensten (insbesondere Azure Batch-Verwaltungsdiensten) zur Azure-SSIS IR im virtuellen Netzwerk unterbrochen. Um dies zu vermeiden, definieren Sie in dem Subnetz, in dem die Azure-SSIS IR enthalten ist, mindestens eine UDR. 

In einem Azure ExpressRoute-Szenario können Sie in dem Subnetz, in dem die Azure-SSIS IR gehostet wird, eine 0.0.0.0/0-Route anwenden, bei der der nächste Hoptyp **Internet** lautet. In einem NVA-Szenario können Sie den nächsten Hoptyp der vorhandenen 0.0.0.0/0-Route von **Virtuelles Gerät** in **Internet** ändern.

![Hinzufügen einer Route](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

Falls Sie weiterhin in der Lage sein möchten, den ausgehenden Internetdatenverkehr von diesem Subnetz zu überprüfen, können Sie spezifische UDRs definieren, um den Datenverkehr mit dem nächsten Hoptyp **Internet** ausschließlich zwischen Azure Batch-Verwaltungsdiensten und der Azure-SSIS IR zu routen.

Wenn sich die Azure-SSIS IR beispielsweise in `UK South` befindet, würden Sie über den [Downloadlink für IP-Adressbereiche und Diensttags](https://www.microsoft.com/en-us/download/details.aspx?id=56519) oder über die [API zur Ermittlung von Diensttags](https://aka.ms/discoveryapi) eine IP-Adressbereichsliste für das Diensttag `BatchNodeManagement.UKSouth` abrufen. Wenden Sie dann die folgenden UDRs zusammengehöriger IP-Adressbereichsrouten mit dem nächsten Hoptyp **Internet** an.

![Azure Batch-UDR-Einstellungen](media/join-azure-ssis-integration-runtime-virtual-network/azurebatch-udr-settings.png)

> [!NOTE]
> Bei diesem Ansatz fallen zusätzliche Wartungskosten an. Damit die Azure-SSIS IR funktionsfähig bleibt, sollten Sie regelmäßig den IP-Adressbereich überprüfen und der UDR neue IP-Adressbereiche hinzufügen. Es wird empfohlen, den IP-Adressbereich monatlich zu überprüfen. Nachdem die neue IP-Adresse im Diensttag angezeigt wird, dauert es einen weiteren Monat, bis sie wirksam wird. 

### <a name="resource-group"></a> Einrichten der Ressourcengruppe
Die Azure SSIS-IR muss bestimmte Netzwerkressourcen unter der gleichen Ressourcengruppe erstellen wie das virtuelle Netzwerk. Diese Ressourcen umfassen Folgendes:
   -   Ein Azure-Lastenausgleich mit dem Namen *\<GUID>-azurebatch-cloudserviceloadbalancer*.
   -   Eine öffentliche Azure-IP-Adresse mit dem Namen *\<GUID>-azurebatch-cloudservicepublicip*.
   -   Eine Netzwerksicherheitsgruppe mit dem Namen *\<GUID>-azurebatch-cloudservicenetworksecuritygroup*. 

Diese Ressourcen werden beim Start der IR erstellt und beim Beenden der IR gelöscht. Damit die IR ordnungsgemäß beendet werden kann, sollten Sie diese Netzwerkressourcen nicht in anderen Ressourcen wiederverwenden. 

Stellen Sie sicher, dass für die Ressourcengruppe oder das Abonnement, zu der bzw. dem das virtuelle Netzwerk gehört, keine Ressourcensperre besteht. Wenn Sie eine Schreibschutzsperre oder eine Löschsperre konfigurieren, kann beim Starten und Beenden der IR ein Fehler auftreten oder die IR nicht mehr reagieren. 

Stellen Sie sicher, dass Sie nicht über eine Azure-Richtlinie verfügen, die verhindert, dass die folgenden Ressourcen unter der Ressourcengruppe oder dem Abonnement erstellt werden, zu der bzw. dem das virtuelle Netzwerk gehört: 
   -   Microsoft.Network/LoadBalancers 
   -   Microsoft.Network/NetworkSecurityGroups 
   -   Microsoft.Network/PublicIPAddresses 

### <a name="faq"></a> Häufig gestellte Fragen (FAQ)

- Wie kann ich die öffentliche IP-Adresse schützen, die in der Azure-SSIS IR für eingehende Verbindungen verfügbar gemacht wird? Ist es möglich, die öffentliche IP-Adresse zu entfernen?
 
    Derzeit wird automatisch eine öffentliche IP-Adresse erstellt, wenn die Azure-SSIS IR mit dem virtuellen Netzwerk verknüpft wird. Wir verfügen über eine NSG auf NIC-Ebene, die ausschließlich eingehende Verbindungen von Azure Batch-Verwaltungsdiensten mit der Azure-SSIS IR zulässt. Sie können auch eine NSG auf Subnetzebene festlegen, um eingehende Verbindungen zu schützen.

    Wenn die öffentliche IP-Adresse nicht verfügbar gemacht werden soll, können Sie anstelle des virtuellen Netzwerks die [selbstgehostete IR als Proxy für die Azure-SSIS IR konfigurieren](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis), wenn dies auf Ihr Szenario zutrifft.
 
- Kann ich die statische IP-Adresse der Azure-SSIS IR der Liste zugelassener IP-Adressen hinzufügen, die die Firewall für die Datenquelle nutzt?
 
    - Wenn Sie eine lokale Datenquelle verwenden, können Sie den IP-Adressbereich dieses Subnetzes der Liste zugelassener IP-Adressen hinzufügen, nachdem Sie eine Verbindung zwischen dem virtuellen Netzwerk und Ihrem lokalen Netzwerk hergestellt und Ihre Azure-SSIS IR mit dem Subnetz des virtuellen Netzwerks verknüpft haben.
    - Wenn es sich bei der Datenquelle um einen Azure-Dienst handelt, der mit einem VNET-Dienstendpunkt unterstützt wird, können Sie in Ihrem virtuellen Netzwerk einen VNET-Dienstendpunkt konfigurieren und die Azure-SSIS IR mit dem Subnetz dieses virtuellen Netzwerks verknüpfen. Anschließend können Sie anstelle des IP-Bereichs die Regel für virtuelle Netzwerke der Azure-Dienste verwenden, um den Zugriff zuzulassen.
    - Wenn Sie eine andere Art von Clouddatenquelle verwenden, können Sie den ausgehenden Datenverkehr von der Azure-SSIS IR zum NVA oder zur Azure Firewall mithilfe einer UDR routen, indem Sie eine statische öffentliche IP-Adresse verwenden. Sie können die öffentliche IP-Adresse der NVA oder der Azure Firewall der Liste zugelassener IP-Adressen hinzufügen.
    - Wenn die oben genannten Antworten nicht auf Ihr Szenario zutreffen, können Sie den Zugriff auf die Datenquelle ermöglichen, indem Sie eine [selbstgehostete IR als Proxy für die Azure-SSIS IR konfigurieren](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). Anschließend können Sie die IP-Adresse des Computers, auf dem die selbstgehostete IR ausgeführt wird, der Liste zugelassener IP-Adressen hinzufügen, anstatt die Azure-SSIS IR mit dem virtuellen Netzwerk zu verknüpfen.

## <a name="azure-portal-data-factory-ui"></a>Azure-Portal (Data Factory-Benutzeroberfläche)
In diesem Abschnitt erfahren Sie, wie Sie mithilfe des Microsoft Azure-Portals und der Data Factory-Benutzeroberfläche eine vorhandene Azure-SSIS IR mit einem virtuellen Netzwerk verknüpfen (klassisch oder Azure Resource Manager). 

Bevor Sie die Azure-SSIS IR mit dem virtuellen Netzwerk verknüpfen, müssen Sie das virtuelle Netzwerk ordnungsgemäß konfigurieren. Führen Sie die Schritte in dem Abschnitt aus, der dem Typ Ihres virtuellen Netzwerks entspricht (klassisch oder Azure Resource Manager). Befolgen Sie dann die Schritte im dritten Abschnitt, um die Azure-SSIS IR mit dem virtuellen Netzwerk zu verknüpfen. 

### <a name="configure-an-azure-resource-manager-virtual-network"></a>Konfigurieren eines virtuellen Azure Resource Manager-Netzwerks

Konfigurieren Sie ein virtuelles Azure Resource Manager-Netzwerk mithilfe des Portals, bevor Sie eine Azure-SSIS IR mit dem Netzwerk verknüpfen.

1. Starten Sie Microsoft Edge oder Google Chrome. Derzeit wird die Data Factory-Benutzeroberfläche nur in diesen Webbrowsern unterstützt. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

1. Wählen Sie **Weitere Dienste**. Filtern Sie nach **Virtuelle Netzwerke**, und wählen Sie die Option aus. 

1. Filtern Sie nach Ihrem virtuellen Netzwerk, und wählen Sie es in der Liste aus. 

1. Wählen Sie auf der Seite **Virtuelles Netzwerk** die Option **Eigenschaften** aus. 

1. Wählen Sie für die **RESSOURCEN-ID** die Schaltfläche „Kopieren“ aus, um die Ressourcen-ID für das virtuelle Netzwerk in die Zwischenablage zu kopieren. Speichern Sie die ID aus der Zwischenablage in OneNote oder in einer Datei. 

1. Wählen Sie im linken Menü **Subnetze** aus. Stellen Sie sicher, dass die Anzahl der verfügbaren Adressen größer ist als die Anzahl der Knoten in Ihrer Azure-SSIS IR. 

1. Vergewissern Sie sich, dass dieser Azure Batch-Anbieter in dem Azure-Abonnement, das über das virtuelle Netzwerk verfügt, registriert ist. Alternativ können Sie auch den Azure Batch-Anbieter registrieren. Wenn in Ihrem Abonnement bereits ein Azure Batch-Konto enthalten ist, ist Ihr Abonnement für Azure Batch registriert. (Wenn Sie die Azure-SSIS Integration Runtime im Data Factory-Portal erstellen, wird der Azure Batch-Anbieter automatisch für Sie erstellt.) 

   a. Wählen Sie im Azure-Portal im linken Menü **Abonnements** aus. 

   b. Wählen Sie Ihr Abonnement aus. 

   c. Wählen Sie auf der linken Seite **Ressourcenanbieter** aus, und bestätigen Sie, dass **Microsoft.Batch** ein registrierter Anbieter ist. 

   ![Bestätigung des Status „Registriert“](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Wenn **Microsoft.Batch** nicht in der Liste angezeigt wird, erstellen Sie zum Registrieren [ein leeres Azure Batch-Konto](../batch/batch-account-create-portal.md) in Ihrem Abonnement. Sie können es später wieder löschen. 

### <a name="configure-a-classic-virtual-network"></a>Konfigurieren eines klassischen virtuellen Netzwerks
Konfigurieren Sie ein klassisches virtuelles Netzwerk mithilfe des Portals, bevor Sie eine Azure-SSIS IR mit dem Netzwerk verknüpfen. 

1. Starten Sie Microsoft Edge oder Google Chrome. Derzeit wird die Data Factory-Benutzeroberfläche nur in diesen Webbrowsern unterstützt. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

1. Wählen Sie **Weitere Dienste**. Filtern Sie nach **Virtuelle Netzwerke (klassisch)** , und wählen Sie die Option aus. 

1. Filtern Sie nach Ihrem virtuellen Netzwerk, und wählen Sie es in der Liste aus. 

1. Wählen Sie auf der Seite **Virtuelles Netzwerk (klassisch)** die Option **Eigenschaften** aus. 

   ![Ressourcen-ID des klassischen virtuellen Netzwerks](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

1. Wählen Sie für **RESSOURCEN-ID** die Schaltfläche „Kopieren“ aus, um die Ressourcen-ID für das klassische Netzwerk in die Zwischenablage zu kopieren. Speichern Sie die ID aus der Zwischenablage in OneNote oder in einer Datei. 

1. Wählen Sie im linken Menü **Subnetze** aus. Stellen Sie sicher, dass die Anzahl der verfügbaren Adressen größer ist als die Anzahl der Knoten in Ihrer Azure-SSIS IR. 

   ![Anzahl der verfügbaren Adressen im virtuellen Netzwerk](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

1. Verknüpfen Sie **MicrosoftAzureBatch** mit der Rolle **Mitwirkender für klassische virtuelle Computer** für das virtuelle Netzwerk. 

    a. Wählen Sie im linken Menü **Zugriffssteuerung (IAM)** aus, und wählen Sie die Registerkarte **Rollenzuweisungen** aus. 

    ![Schaltflächen „Zugriffssteuerung“ und „Hinzufügen“](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Wählen Sie **Rollenzuweisung hinzufügen** aus.

    c. Wählen Sie auf der Seite **Rollenzuweisung hinzufügen** für **Rolle** die Option **Mitwirkender für klassische virtuelle Computer** aus. Fügen Sie in das Feld **Auswählen** die Zeichenfolge **ddbf3205-c6bd-46ae-8127-60eb93363864** ein, und wählen Sie dann in der Liste der Suchergebnisse **Microsoft Azure Batch** aus. 

    ![Suchergebnisse auf der Seite „Rollenzuweisung hinzufügen“](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    d. Wählen Sie **Speichern** aus, um die Einstellungen zu speichern und die Seite zu schließen. 

    ![Speichern der Zugriffseinstellungen](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    e. Vergewissern Sie sich, dass **Microsoft Azure Batch** in der Liste der Mitwirkenden angezeigt wird. 

    ![Bestätigen des Azure Batch-Zugriffs](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

1. Vergewissern Sie sich, dass dieser Azure Batch-Anbieter in dem Azure-Abonnement, das über das virtuelle Netzwerk verfügt, registriert ist. Alternativ können Sie auch den Azure Batch-Anbieter registrieren. Wenn in Ihrem Abonnement bereits ein Azure Batch-Konto enthalten ist, ist Ihr Abonnement für Azure Batch registriert. (Wenn Sie die Azure-SSIS Integration Runtime im Data Factory-Portal erstellen, wird der Azure Batch-Anbieter automatisch für Sie erstellt.) 

   a. Wählen Sie im Azure-Portal im linken Menü **Abonnements** aus. 

   b. Wählen Sie Ihr Abonnement aus. 

   c. Wählen Sie auf der linken Seite **Ressourcenanbieter** aus, und bestätigen Sie, dass **Microsoft.Batch** ein registrierter Anbieter ist. 

   ![Bestätigung des Status „Registriert“](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Wenn **Microsoft.Batch** nicht in der Liste angezeigt wird, erstellen Sie zum Registrieren [ein leeres Azure Batch-Konto](../batch/batch-account-create-portal.md) in Ihrem Abonnement. Sie können es später wieder löschen. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Verknüpfen der Azure SSIS-IR mit einem virtuellen Netzwerk

Nachdem Sie Ihr virtuelles Azure Resource Manager-Netzwerk oder klassisches virtuelles Netzwerk konfiguriert haben, können Sie die Azure-SSIS IR mit dem virtuellen Netzwerk verknüpfen:

1. Starten Sie Microsoft Edge oder Google Chrome. Derzeit wird die Data Factory-Benutzeroberfläche nur in diesen Webbrowsern unterstützt. 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Menü **Data Factorys** aus. Wenn **Data Factorys** nicht im Menü angezeigt wird, wählen Sie **Weitere Dienste** und dann im Abschnitt **INTELLIGENCE + ANALYSE** die Option **Data Factorys** aus. 

   ![Liste von Data Factorys](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Wählen Sie Ihre Data Factory mit der Azure-SSIS IR in der Liste aus. Die Startseite für Ihre Data Factory wird angezeigt. Wählen Sie die Kachel **Verfassen und bereitstellen** aus. Die Data Factory-Benutzeroberfläche wird auf einer separaten Registerkarte angezeigt. 

   ![Data Factory-Startseite](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. Wechseln Sie in der Data Factory-Benutzeroberfläche zur Registerkarte **Bearbeiten**, klicken Sie auf **Verbindungen**, und wechseln Sie zur Registerkarte **Integration Runtimes**. 

   ![Registerkarte „Integration Runtimes“](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Wenn Ihre Azure-SSIS IR ausgeführt wird, wählen Sie in der Liste **Integration Runtimes** in der Spalte **Aktionen** für die Azure-SSIS IR die Schaltfläche **Beenden** aus. Sie können eine IR erst bearbeiten, wenn sie nicht länger ausgeführt wird. 

   ![Beenden der IR](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. Wählen Sie in der Liste **Integration Runtimes** in der Spalte **Aktionen** für die Azure-SSIS IR die Schaltfläche **Bearbeiten** aus. 

   ![Bearbeiten der Integration Runtime](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. Navigieren Sie im Bereich **Integration Runtime Setup** durch die Seiten **Allgemeine Einstellungen** und **SQL-Einstellungen**, indem Sie die Schaltfläche **Weiter** auswählen. 

1. Verfahren Sie auf der Seite **Erweiterte Einstellungen** wie folgt: 

   a. Aktivieren Sie das Kontrollkästchen neben **VNET auswählen**. 

   b. Wählen Sie für **Abonnement** Ihr Azure-Abonnement aus. Unter dem Abonnement können Sie ein vorhandenes virtuelles Netzwerk auswählen. 
  
   c. Wählen Sie unter **VNET-Name** Ihr virtuelles Netzwerk aus. 

   d. Wählen Sie unter **Subnetzname** Ihr Subnet in dem virtuellen Netzwerk aus. 

   e. Wenn Sie zusätzliche eine **selbstgehostete** IR als Proxy für die Azure-SSIS IR konfigurieren oder verwalten möchten, aktivieren Sie das Kontrollkästchen zum Einrichten einer selbstgehosteten IR. Weitere Informationen finden Sie unter [Konfigurieren einer selbstgehosteten IR als Proxy für Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis).

   f. Wählen Sie die Schaltfläche für die **VNET-Überprüfung** aus. Wenn die Überprüfung erfolgreich ist, wählen Sie die Schaltfläche **Weiter** aus. 

   ![Erweiterte Einstellungen für IR-Setup](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)

1. Überprüfen Sie auf der Seite **Zusammenfassung** alle Einstellungen für die Azure-SSIS IR. Wählen Sie dann die Schaltfläche **Aktualisieren** aus.

1. Starten Sie die Azure-SSIS IR, indem Sie in der Spalte **Aktionen** für die Azure-SSIS IR die Schaltfläche **Starten** auswählen. Es dauert etwa 20 bis 30 Minuten, um die Azure-SSIS IR zu starten, die mit einem virtuellen Netzwerk verknüpft wird. 

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="configure-a-virtual-network"></a>Konfigurieren eines virtuellen Netzwerks
Sie müssen das virtuelle Netzwerk konfigurieren, bevor Sie die Azure-SSIS IR damit verknüpfen können. Um für die Azure-SSIS IR, die mit dem Netzwerk verknüpft werden soll, automatisch Berechtigungen und Einstellungen für virtuelle Netzwerke zu konfigurieren, fügen Sie das folgende Skript hinzu:

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Erstellen einer Azure SSIS-IR und Verknüpfen dieser mit einem virtuellen Netzwerk
Sie können eine Azure SSIS-IR erstellen und gleichzeitig mit einem virtuellen Netzwerk verknüpfen. Das vollständige Skript und Anweisungen finden Sie unter [Erstellen einer Azure-SSIS IR](create-azure-ssis-integration-runtime.md#use-azure-powershell-to-create-an-integration-runtime).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Verknüpfen einer Azure SSIS-Integration Runtime mit einem virtuellen Netzwerk
Im Artikel [Erstellen einer Azure-SSIS Integration Runtime](create-azure-ssis-integration-runtime.md) wird veranschaulicht, wie im selben Skript eine Azure-SSIS IR erstellt und mit einem virtuellen Netzwerk verknüpft wird. Wenn Sie bereits über eine Azure-SSIS IR verfügen, führen Sie die folgenden Schritte aus, um sie mit dem virtuellen Netzwerk zu verknüpfen: 
1. Beenden Sie die Azure SSIS IR. 
1. Konfigurieren Sie die Azure SSIS-IR so, dass Sie mit dem virtuellen Netzwerk verknüpft wird. 
1. Starten Sie die Azure SSIS IR. 

### <a name="define-the-variables"></a>Definieren der Variablen
```powershell
$ResourceGroupName = "<your Azure resource group name>"
$DataFactoryName = "<your Data Factory name>" 
$AzureSSISName = "<your Azure-SSIS IR name>"
# Specify the information about your classic or Azure Resource Manager virtual network.
$VnetId = "<your Azure virtual network resource ID>"
$SubnetName = "<the name of subnet in your virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>Beenden der Azure SSIS IR
Sie müssen die Azure-SSIS IR beenden, bevor Sie sie mit einem virtuellen Netzwerk verknüpfen können. Dieser Befehl gibt alle Knoten frei und beendet die Abrechnung:

```powershell
Stop-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Konfigurieren der Einstellungen des virtuellen Netzwerks für das Verknüpfen der Azure SSIS-IR

Verwenden Sie das folgende Skript, um Einstellungen für das virtuelle Netzwerk zu konfigurieren, mit dem die Azure-SSIS IR verknüpft werden soll: 

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Konfigurieren der Azure SSIS IR
Um die Azure-SSIS IR für die Verknüpfung mit dem virtuellen Netzwerk zu konfigurieren, führen Sie den Befehl `Set-AzDataFactoryV2IntegrationRuntime` aus: 

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Starten der Azure SSIS IR
Führen Sie den folgenden Befehl aus, um die Azure-SSIS IR zu starten: 

```powershell
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Das Abschließen dieses Befehls dauert 20 bis 30 Minuten.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Azure-SSIS IR finden Sie in den folgenden Artikeln: 
- [Azure-SSIS IR](concepts-integration-runtime.md#azure-ssis-integration-runtime). Dieser Artikel enthält allgemeine konzeptionelle Informationen zu IRs, einschließlich der Azure-SSIS IR. 
- [Tutorial: Bereitstellen von SSIS-Paketen in Azure](tutorial-create-azure-ssis-runtime-portal.md). Dieses Tutorial enthält ausführliche Anweisungen zum Erstellen einer Azure-SSIS IR. Dabei wird Azure SQL-Datenbank zum Hosten des SSIS-Katalogs verwendet. 
- [Erstellen einer Azure-SSIS IR](create-azure-ssis-integration-runtime.md). Dieser Artikel baut auf dem Tutorial auf. Sie erfahren, wie Sie Azure SQL-Datenbank mit VNET-Dienstendpunkten oder mit einer verwalteten Instanz in einem virtuellen Netzwerk verwenden, um den SSIS-Katalog zu hosten. Außerdem wird veranschaulicht, wie Sie die Azure-SSIS IR mit einem virtuellen Netzwerk verknüpfen. 
- [Überwachen einer Azure-SSIS-Integrationslaufzeit](monitor-integration-runtime.md#azure-ssis-integration-runtime): In diesem Artikel erfahren Sie, wie Sie Informationen zur Azure-SSIS IR abrufen. Darüber hinaus enthält er Statusbeschreibungen für die zurückgegebenen Informationen. 
- [Verwalten einer Azure-SSIS-Integrationslaufzeit](manage-azure-ssis-integration-runtime.md): In diesem Artikel wird beschrieben, wie Sie die Azure-SSIS IR beenden, starten oder löschen. Außerdem wird gezeigt, wie Sie Ihre Azure SSIS-IR horizontal hochskalieren, indem Sie weitere Knoten hinzufügen.
