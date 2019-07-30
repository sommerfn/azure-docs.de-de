---
title: Erweitern von HDInsight mit Virtual Network – Azure
description: Erfahren Sie, wie Sie HDInsight mithilfe von Azure Virtual Network mit anderen Cloud-Ressourcen oder Ressourcen in Ihrem Rechenzentrum verbinden.
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: dab4262e5412c8ef3cd1e0d2ef8203d7f289693f
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839185"
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>Erweitern von Azure HDInsight per Azure Virtual Network

Es wird beschrieben, wie Sie HDInsight mit einem [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) verwenden. Die Nutzung eines Azure Virtual Network ermöglicht die folgenden Szenarien:

* Herstellen einer Verbindung mit HDInsight direkt über ein lokales Netzwerk

* Herstellen einer Verbindung für HDInsight mit Datenspeichern in einem Azure Virtual Network

* Direktes Zuweisen von [Apache Hadoop](https://hadoop.apache.org/)-Diensten, die nicht öffentlich über das Internet verfügbar sind. Zum Beispiel [Apache Kafka](https://kafka.apache.org/)-APIs oder die [Apache HBase](https://hbase.apache.org/)-Java-API.

> [!IMPORTANT]  
> Wenn Sie einen HDInsight-Cluster in einem VNET erstellen, werden mehrere Netzwerkressourcen erstellt, z. B. NICs und Lastenausgleichsmodule. Löschen Sie diese Netzwerkressourcen **nicht**, da sie benötigt werden, damit Ihr Cluster mit dem VNET ordnungsgemäß funktioniert.
>
> Ab dem 28. Februar 2019 werden diese Netzwerkressourcen (wie z. B. Netzwerkadapter, Lastenausgleichsmodule usw.) für in einem virtuellen Netzwerk NEU erstellte Cluster in der gleichen HDInsight-Clusterressourcengruppe bereitgestellt. Zuvor wurden diese Ressourcen in der Ressourcengruppe des virtuellen Netzwerks bereitgestellt. Für derzeit ausgeführte Cluster und Cluster, die ohne virtuelles Netzwerk erstellt wurden, ändert sich nichts.

## <a name="prerequisites-for-code-samples-and-examples"></a>Voraussetzungen für die Codebeispiele und Beispiele

* Grundlagen von TCP/IP-Netzwerken. Falls Sie nicht mit TCP/IP-Netzwerken vertraut sind, sollten Sie sich an eine Person wenden, die über die entsprechenden Kenntnisse verfügt, bevor Sie Änderungen an Produktionsnetzwerken vornehmen.
* Wenn Sie PowerShell verwenden, benötigen Sie das [AZ-Modul](https://docs.microsoft.com/powershell/azure/overview).
* Wenn Sie die Azure-Befehlszeilenschnittstelle verwenden möchten, diese aber noch nicht installiert haben, lesen Sie [Installieren der Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

> [!IMPORTANT]  
> Eine ausführliche Anleitung zum Herstellen einer Verbindung von HDInsight mit Ihrem lokalen Netzwerk mithilfe eines virtuellen Azure-Netzwerks finden Sie im Dokument [Verbinden von HDInsight mit Ihrem lokalen Netzwerk](connect-on-premises-network.md).

## <a name="planning"></a>Planung

Sie müssen die folgenden Fragen beantworten, wenn Sie die Installation von HDInsight in einem virtuellen Netzwerk planen:

* Müssen Sie HDInsight in einem vorhandenen virtuellen Netzwerk installieren? Oder erstellen Sie ein neues Netzwerk?

    Falls Sie ein vorhandenes virtuelles Netzwerk verwenden, müssen Sie unter Umständen die Netzwerkkonfiguration ändern, bevor Sie HDInsight installieren können. Weitere Informationen finden Sie im Abschnitt [Hinzufügen von HDInsight zu einem vorhandenen virtuellen Netzwerk](#existingvnet).

* Möchten Sie das virtuelle Netzwerk mit HDInsight einem anderen virtuellen Netzwerk oder Ihrem lokalen Netzwerk hinzufügen?

    Um Ressourcen auf einfache Weise netzwerkübergreifend verwenden zu können, müssen Sie ggf. ein benutzerdefiniertes DNS erstellen und die DNS-Weiterleitung konfigurieren. Weitere Informationen finden Sie im Abschnitt [Verbinden von mehreren Netzwerken](#multinet).

* Möchten Sie ein- oder ausgehenden Datenverkehr für HDInsight beschränken oder umleiten?

    HDInsight muss über eine uneingeschränkte Kommunikation mit bestimmten IP-Adressen im Azure-Rechenzentrum verfügen. Es gibt auch mehrere Ports, für die die Clientkommunikation durch Firewalls zugelassen sein muss. Weitere Informationen finden Sie im Abschnitt [Steuern des Netzwerkdatenverkehrs](#networktraffic).

## <a id="existingvnet"></a>Hinzufügen von HDInsight zu einem vorhandenen virtuellen Netzwerk

Führen Sie die Schritte in diesem Abschnitt aus, um zu erfahren, wie Sie einen neuen HDInsight-Cluster einem vorhandenen Azure Virtual Network hinzufügen.

> [!NOTE]  
> Ein vorhandener HDInsight-Cluster kann in einem virtuellen Netzwerk nicht hinzugefügt werden.

1. Verwenden Sie das klassische oder das Resource Manager-Bereitstellungsmodell für das virtuelle Netzwerk?

    Für HDInsight 3.4 und höher ist ein virtuelles Resource Manager-Netzwerk erforderlich. In früheren Versionen von HDInsight war ein klassisches virtuelles Netzwerk erforderlich.

    Falls Ihr vorhandenes Netzwerk ein klassisches virtuelles Netzwerk ist, müssen Sie ein virtuelles Resource Manager-Netzwerk erstellen und die beiden Netzwerke dann verbinden. [Herstellen einer Verbindung zwischen klassischen VNets und neuen VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md)

    Nach dem Verknüpfen kann die Installation von HDInsight im Resource Manager-Netzwerk mit Ressourcen im klassischen Netzwerk interagieren.

2. Verwenden Sie Netzwerksicherheitsgruppen, benutzerdefinierte Routen oder virtuelle Network Appliances, um Datenverkehr einzuschränken, der in das bzw. aus dem virtuellen Netzwerk fließt?

    Als verwalteter Dienst ist für HDInsight der uneingeschränkte Zugriff auf mehrere IP-Adressen im Azure-Rechenzentrum erforderlich. Aktualisieren Sie alle vorhandenen Netzwerksicherheitsgruppen oder benutzerdefinierten Routen, um die Kommunikation mit diesen IP-Adressen zuzulassen.
    
    HDInsight hostet mehrere Dienste, für die viele verschiedene Ports verwendet werden. Verhindern Sie, dass der Datenverkehr für diesen Port blockiert wird. Eine Liste mit Ports, die für Firewalls von virtuellen Geräten zugelassen werden müssen, finden Sie im Abschnitt „Sicherheit“.
    
    Verwenden Sie die folgenden Azure PowerShell- oder Azure CLI-Befehle, um Ihre vorhandene Sicherheitskonfiguration zu ermitteln:

    * Netzwerksicherheitsgruppen

        Ersetzen Sie `RESOURCEGROUP` durch den Namen der Ressourcengruppe, die das virtuelle Netzwerk enthält, und geben Sie dann diesen Befehl ein:
    
        ```powershell
        Get-AzNetworkSecurityGroup -ResourceGroupName  "RESOURCEGROUP"
        ```
    
        ```azurecli
        az network nsg list --resource-group RESOURCEGROUP
        ```

        Weitere Informationen finden Sie im Dokument zur [Problembehandlung bei Netzwerksicherheitsgruppen](../virtual-network/diagnose-network-traffic-filter-problem.md).

        > [!IMPORTANT]  
        > Netzwerksicherheitsgruppen-Regeln werden der Reihenfolge nach basierend auf der Regelpriorität angewendet. Die erste Regel, die mit dem Datenverkehrsmuster übereinstimmt, wird angewendet, und auf diesen Datenverkehr werden dann keine anderen Regeln angewendet. Sortieren Sie die Regeln von „weniger strikt“ bis „strikt“. Weitere Informationen finden Sie im Dokument [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).

    * Benutzerdefinierte Routen

        Ersetzen Sie `RESOURCEGROUP` durch den Namen der Ressourcengruppe, die das virtuelle Netzwerk enthält, und geben Sie dann diesen Befehl ein:

        ```powershell
        Get-AzRouteTable -ResourceGroupName "RESOURCEGROUP"
        ```

        ```azurecli
        az network route-table list --resource-group RESOURCEGROUP
        ```

        Weitere Informationen finden Sie im Dokument [Problembehandlung bei Routen](../virtual-network/diagnose-network-routing-problem.md).

3. Erstellen Sie einen HDInsight-Cluster, und wählen Sie während der Konfiguration die virtuelle Azure-Netzwerk-Instanz aus. Den Vorgang zur Clustererstellung können Sie anhand der Schritte in den folgenden Dokumenten nachvollziehen:

    * [Erstellen von Linux-basierten Clustern in HDInsight mithilfe des Azure-Portals](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [Erstellen von Linux-basierten Clustern in HDInsight mit Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [Erstellen von HDInsight-Clustern mit der klassischen Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [Erstellen von Hadoop-Clustern in HDInsight mit Azure Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

   > [!IMPORTANT]  
   > Das Hinzufügen von HDInsight zu einem virtuellen Netzwerk ist ein optionaler Konfigurationsschritt. Achten Sie darauf, beim Konfigurieren des Clusters das virtuelle Netzwerk auszuwählen.

## <a id="multinet"></a>Verbinden von mehreren Netzwerken

Die größte Herausforderung bei der Konfiguration von mehreren Netzwerken ist die Namensauflösung zwischen den Netzwerken.

Azure ermöglicht die Namensauflösung für Azure-Dienste, die in einem virtuellen Netzwerk installiert sind. Diese integrierte Namensauflösung erlaubt HDInsight das Herstellen einer Verbindung mit den folgenden Ressourcen, indem ein vollqualifizierter Domänenname (FQDN) verwendet wird:

* Alle Ressourcen, die im Internet verfügbar sind. Beispiel: „microsoft.com“, „windowsupdate.com“.

* Alle Ressourcen, die sich in demselben Azure Virtual Network befinden, indem der __interne DNS-Name__ der Ressource verwendet wird. Beim Verwenden der Standardnamensauflösung sind dies Beispiele für interne DNS-Namen, die HDInsight-Workerknoten zugewiesen werden:

  * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
  * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Beide Knoten können direkt miteinander und mit anderen Knoten in HDInsight kommunizieren, indem interne DNS-Namen verwendet werden.

Bei der Standardnamensauflösung ist es für HDInsight __nicht__ zulässig, die Namen von Ressourcen in Netzwerken aufzulösen, die mit dem virtuellen Netzwerk verknüpft sind. Beispielsweise ist es eine gängige Vorgehensweise, Ihr lokales Netzwerk mit dem virtuellen Netzwerk zu verknüpfen. Allein mit der Standardnamensauflösung kann HDInsight nicht anhand des Namens auf Ressourcen im lokalen Netzwerk zugreifen. Das Gegenteil gilt ebenfalls: Ressourcen in Ihrem lokalen Netzwerk können anhand des Namens nicht auf Ressourcen im virtuellen Netzwerk zugreifen.

> [!WARNING]  
> Sie müssen den benutzerdefinierten DNS-Server erstellen und das virtuelle Netzwerk für seine Verwendung konfigurieren, bevor Sie den HDInsight-Cluster erstellen.

Die Ausführung der folgenden Aktionen ist erforderlich, um die Namensauflösung zwischen dem virtuellen Netzwerk und Ressourcen in verknüpften Netzwerken zu aktivieren:

1. Erstellen eines benutzerdefinierten DNS-Servers im Azure Virtual Network, in dem Sie die Installation von HDInsight planen

2. Konfigurieren des virtuellen Netzwerks zur Verwendung des benutzerdefinierten DNS-Servers

3. Suchen nach dem von Azure zugewiesenen DNS-Suffix für Ihr virtuelles Netzwerk. Dieser Wert ähnelt diesem Wert: `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. Informationen zum Ermitteln des DNS-Suffix finden Sie im Abschnitt [Beispiel: Benutzerdefiniertes DNS](#example-dns).

4. Konfigurieren der Weiterleitung zwischen den DNS-Servern. Die Konfiguration richtet sich nach dem Typ des Remotenetzwerks.

   * Wenn das Remotenetzwerk ein lokales Netzwerk ist, können Sie das DNS wie folgt konfigurieren:
        
     * __Benutzerdefiniertes DNS__ (im virtuellen Netzwerk):

         * Leiten Sie Anforderungen für das DNS-Suffix des virtuellen Netzwerks an den rekursiven Azure-Resolver (168.63.129.16) weiter. Azure verarbeitet Anforderungen von Ressourcen im virtuellen Netzwerk.

         * Leiten Sie alle anderen Anforderungen an den lokalen DNS-Server weiter. Das lokale DNS verarbeitet alle anderen Anforderungen der Namensauflösung. Dies gilt auch für Internetressourcen, z.B. „Microsoft.com“.

     * __Lokales DNS:__ Leiten Sie Anforderungen für das DNS-Suffix des virtuellen Netzwerks an den benutzerdefinierten DNS-Server weiter. Der benutzerdefinierte DNS-Server leitet die Daten dann an den rekursiven Azure-Resolver weiter.

       Bei dieser Konfiguration werden Anforderungen von vollqualifizierten Domänennamen, die das DNS-Suffix des virtuellen Netzwerks enthalten, an den benutzerdefinierten DNS-Server weiter. Alle anderen Anforderungen (auch für öffentliche Internetadressen) werden vom lokalen DNS-Server verarbeitet.

   * Konfigurieren Sie das DNS wie folgt, wenn das Remotenetzwerk ein anderes Azure Virtual Network ist:

     * __Benutzerdefiniertes DNS__ (in jedem virtuellen Netzwerk):

         * Anforderungen für das DNS-Suffix der virtuellen Netzwerke werden an die benutzerdefinierten DNS-Server weitergeleitet. Das DNS in jedem virtuellen Netzwerk ist für das Auflösen von Ressourcen im jeweiligen Netzwerk verantwortlich.

         * Leiten Sie alle anderen Anforderungen an den rekursiven Azure-Resolver weiter. Der rekursive Resolver ist für das Auflösen von lokalen Ressourcen und Internetressourcen verantwortlich.

       Der DNS-Server für jedes Netzwerk leitet Anforderungen jeweils basierend auf dem DNS-Suffix an das andere Netzwerk weiter. Andere Anforderungen werden mit dem rekursiven Azure-Resolver aufgelöst.

     Ein Beispiel für die einzelnen Konfigurationen finden Sie im Abschnitt [Beispiel: Benutzerdefiniertes DNS](#example-dns).

Weitere Informationen finden Sie im Dokument [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="directly-connect-to-apache-hadoop-services"></a>Herstellen einer direkten Verbindung mit Apache Hadoop-Diensten

Sie können unter `https://CLUSTERNAME.azurehdinsight.net` eine Verbindung mit dem Cluster herstellen. Diese Adresse verwendet eine öffentliche IP-Adresse, die möglicherweise nicht erreichbar sind, wenn Sie NSGs verwendet haben, um eingehenden Datenverkehr aus dem Internet einzuschränken. Darüber hinaus können Sie bei der Bereitstellung des Clusters in einem VNET mithilfe des privaten Endpunkts `https://CLUSTERNAME-int.azurehdinsight.net` darauf zugreifen. Dieser Endpunkt wird in eine private IP-Adresse innerhalb des VNET für den Clusterzugriff aufgelöst.

Verwenden Sie die folgenden Schritte, um über das virtuelle Netzwerk eine Verbindung mit Apache Ambari und anderen Webseiten herzustellen:

1. Verwenden Sie eine der folgenden Methoden, um die internen vollqualifizierten Domänennamen (FQDNs) der HDInsight-Clusterknoten zu ermitteln:

    Ersetzen Sie `RESOURCEGROUP` durch den Namen der Ressourcengruppe, die das virtuelle Netzwerk enthält, und geben Sie dann diesen Befehl ein:

    ```powershell
    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP" | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Suchen Sie in der Liste mit den zurückgegebenen Knoten nach dem FQDN für die Hauptknoten, und verwenden Sie die FQDNs, um eine Verbindung mit Ambari und anderen Webdiensten herzustellen. Verwenden Sie beispielsweise `http://<headnode-fqdn>:8080`, um auf Ambari zuzugreifen.

    > [!IMPORTANT]  
    > Einige der auf den Hauptknoten gehosteten Dienste sind jeweils nur auf einem Knoten aktiv. Wenn Sie versuchen, auf einen Dienst auf einem der Hauptknoten zuzugreifen, und der Fehler 404 zurückgegeben wird, wechseln Sie zum anderen Hauptknoten.

2. Informationen dazu, wie Sie den Knoten und Port ermitteln, auf dem ein Dienst verfügbar ist, finden Sie im Dokument [Von HDInsight verwendete Ports und URIs](./hdinsight-hadoop-port-settings-for-services.md).

## <a id="networktraffic"></a>Steuern des Netzwerkdatenverkehrs

### <a name="techniques-for-controlling-inbound-and-outbound-traffic-to-hdinsight-clusters"></a>Verfahren zum Steuern von ein- und ausgehendem Datenverkehr für HDInsight-Cluster

Sie können den Netzwerkdatenverkehr in einem Azure Virtual Network mit den folgenden Methoden steuern:

* Mit **Netzwerksicherheitsgruppen** (NSGs) können Sie ein- und ausgehenden Datenverkehr für das Netzwerk filtern. Weitere Informationen finden Sie im Dokument [Filtern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).

* **Virtuelle Netzwerkgeräte** (Network Virtual Appliances, NVAs) können nur mit ausgehendem Datenverkehr verwendet werden. Mit virtuellen Netzwerkgeräten wird die Funktionalität von Geräten, z. B. Firewalls und Routern, repliziert.  Weitere Informationen finden Sie im Dokument [Network Appliances](https://azure.microsoft.com/solutions/network-appliances).

Als verwalteter Dienst benötigt HDInsight uneingeschränkten Zugriff auf die HDInsight-Integritäts- und -Verwaltungsdienste für eingehenden und ausgehenden Datenverkehr aus dem VNET. Beim Verwenden von NSGs und müssen Sie sicherstellen, dass diese Dienste weiterhin mit dem HDInsight-Cluster kommunizieren können.

![Diagramm der HDInsight-Entitäten, erstellt in einem benutzerdefinierten virtuellen Azure-Netzwerk](./media/hdinsight-virtual-network-architecture/vnet-diagram.png)

### <a name="hdinsight-with-network-security-groups"></a>HDInsight mit Netzwerksicherheitsgruppen

Wenn Sie planen, **Netzwerksicherheitsgruppen** zum Steuern des Netzwerkdatenverkehrs zu verwenden, sollten Sie vor dem Installieren von HDInsight die folgenden Aktionen durchführen:

1. Identifizieren Sie die Azure-Region, die Sie für HDInsight verwenden möchten.

2. Identifizieren Sie die IP-Adressen, die für HDInsight erforderlich sind. Weitere Informationen finden Sie im Abschnitt [Für HDInsight erforderliche IP-Adressen](#hdinsight-ip).

3. Erstellen oder ändern Sie die Netzwerksicherheitsgruppen für das Subnetz, in dem Sie HDInsight installieren möchten.

    * __Netzwerksicherheitsgruppen__: Lassen Sie __eingehenden__ Datenverkehr über Port __443__ für die IP-Adressen zu. Dadurch wird sichergestellt, dass HDInsight-Verwaltungsdienste den Cluster außerhalb des virtuellen Netzwerks erreichen können.

Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie in der [Übersicht über Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).

### <a name="controlling-outbound-traffic-from-hdinsight-clusters"></a>Steuern des ausgehenden Datenverkehrs der HDInsight-Cluster

Weitere Informationen zum Steuern des ausgehenden Datenverkehrs von HDInsight-Clustern finden Sie unter [Konfigurieren von Einschränkungen des ausgehenden Netzwerkdatenverkehrs für Azure HDInsight-Cluster (Vorschau)](hdinsight-restrict-outbound-traffic.md).

#### <a name="forced-tunneling-to-on-premise"></a>Tunnelerzwingung für lokale Netzwerke

Die Tunnelerzwingung ist eine benutzerdefinierte Routingkonfiguration, bei der für den gesamten Datenverkehr eines Subnetzes die Weiterleitung in ein bestimmtes Netzwerk bzw. an einen bestimmten Standort erzwungen wird, z.B. Ihr lokales Netzwerk. HDInsight bietet __keine__ Unterstützung für die Tunnelerzwingung bei Datenverkehr zu lokalen Netzwerken. 

## <a id="hdinsight-ip"></a>Erforderliche IP-Adressen

Wenn Sie Netzwerksicherheitsgruppen oder benutzerdefinierte Routen zum Steuern von Datenverkehr verwenden, müssen Sie Datenverkehr von den IP-Adressen für Azure-Integritäts- und -Verwaltungsdienste zulassen, damit sie mit Ihrem HDInsight-Cluster kommunizieren können. Einige der IP-Adressen sind regionsspezifisch, und einige davon gelten für alle Azure-Regionen. Unter Umständen müssen Sie auch Datenverkehr über den Azure DNS-Dienst zulassen, wenn Sie kein benutzerdefiniertes DNS verwenden. Sie müssen auch Datenverkehr zwischen VMs innerhalb des Subnetzes zulassen. Ermitteln Sie anhand der folgenden Schritte die IP-Adressen, die zugelassen werden müssen:

> [!Note]  
> Wenn Sie zum Steuern des Datenverkehrs keine Netzwerksicherheitsgruppen oder benutzerdefinierte Routen verwenden, können Sie diesen Abschnitt ignorieren.

1. Wenn Sie den von Azure bereitgestellten DNS-Dienst verwenden, müssen Sie den Zugriff von __168.63.129.16__ an Port 53 zulassen. Weitere Informationen finden Sie im Dokument [Namensauflösung für virtuelle Computer und Rolleninstanzen](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Überspringen Sie diesen Schritt, wenn Sie das benutzerdefinierte DNS verwenden.

2. Lassen Sie Datenverkehr von den folgenden IP-Adressen für Azure-Integritäts- und -Verwaltungsdienste zu, die für alle Azure-Regionen gelten:

    | Quell-IP-Adresse | Ziel  | Direction |
    | ---- | ----- | ----- |
    | 168.61.49.99 | \*:443 | Eingehend |
    | 23.99.5.239 | \*:443 | Eingehend |
    | 168.61.48.131 | \*:443 | Eingehend |
    | 138.91.141.162 | \*:443 | Eingehend |

3. Lassen Sie Datenverkehr von den IP-Adressen zu, die für Azure-Integritäts- und -Verwaltungsdienste in der jeweiligen Region angegeben sind, in der sich Ihre Ressourcen befinden:

    > [!IMPORTANT]  
    > Wenn die verwendete Azure-Region nicht aufgeführt ist, verwenden Sie nur die vier IP-Adressen aus Schritt 1.

    | Country | Region | Zulässige Quell-IP-Adressen | Zulässiges Ziel | Direction |
    | ---- | ---- | ---- | ---- | ----- |
    | Asien | Asien, Osten | 23.102.235.122</br>52.175.38.134 | \*:443 | Eingehend |
    | &nbsp; | Asien, Südosten | 13.76.245.160</br>13.76.136.249 | \*:443 | Eingehend |
    | Australien | Australien (Osten) | 104.210.84.115</br>13.75.152.195 | \*:443 | Eingehend |
    | &nbsp; | Australien, Südosten | 13.77.2.56</br>13.77.2.94 | \*:443 | Eingehend |
    | Brasilien | Brasilien Süd | 191.235.84.104</br>191.235.87.113 | \*:443 | Eingehend |
    | Kanada | Kanada, Osten | 52.229.127.96</br>52.229.123.172 | \*:443 | Eingehend |
    | &nbsp; | Kanada, Mitte | 52.228.37.66</br>52.228.45.222 |\*: 443 | Eingehend |
    | China | China, Norden | 42.159.96.170</br>139.217.2.219</br></br>42.159.198.178</br>42.159.234.157 | \*:443 | Eingehend |
    | &nbsp; | China, Osten | 42.159.198.178</br>42.159.234.157</br></br>42.159.96.170</br>139.217.2.219 | \*:443 | Eingehend |
    | &nbsp; | China, Norden 2 | 40.73.37.141</br>40.73.38.172 | \*:443 | Eingehend |
    | &nbsp; | China, Osten 2 | 139.217.227.106</br>139.217.228.187 | \*:443 | Eingehend |
    | Europa | Nordeuropa | 52.164.210.96</br>13.74.153.132 | \*:443 | Eingehend |
    | &nbsp; | Europa, Westen| 52.166.243.90</br>52.174.36.244 | \*:443 | Eingehend |
    | Frankreich | Frankreich, Mitte| 20.188.39.64</br>40.89.157.135 | \*:443 | Eingehend |
    | Deutschland | Deutschland, Mitte | 51.4.146.68</br>51.4.146.80 | \*:443 | Eingehend |
    | &nbsp; | Deutschland, Nordosten | 51.5.150.132</br>51.5.144.101 | \*:443 | Eingehend |
    | Indien | Indien, Mitte | 52.172.153.209</br>52.172.152.49 | \*:443 | Eingehend |
    | &nbsp; | Indien (Süden) | 104.211.223.67<br/>104.211.216.210 | \*:443 | Eingehend |
    | Japan | Japan, Osten | 13.78.125.90</br>13.78.89.60 | \*:443 | Eingehend |
    | &nbsp; | Japan, Westen | 40.74.125.69</br>138.91.29.150 | \*:443 | Eingehend |
    | Korea | Korea, Mitte | 52.231.39.142</br>52.231.36.209 | \*:443 | Eingehend |
    | &nbsp; | Korea, Süden | 52.231.203.16</br>52.231.205.214 | \*:443 | Eingehend
    | Vereinigtes Königreich | UK, Westen | 51.141.13.110</br>51.141.7.20 | \*:443 | Eingehend |
    | &nbsp; | UK, Süden | 51.140.47.39</br>51.140.52.16 | \*:443 | Eingehend |
    | USA | USA (Mitte) | 13.89.171.122</br>13.89.171.124 | \*:443 | Eingehend |
    | &nbsp; | East US | 13.82.225.233</br>40.71.175.99 | \*:443 | Eingehend |
    | &nbsp; | USA Nord Mitte | 157.56.8.38</br>157.55.213.99 | \*:443 | Eingehend |
    | &nbsp; | USA, Westen-Mitte | 52.161.23.15</br>52.161.10.167 | \*:443 | Eingehend |
    | &nbsp; | USA (Westen) | 13.64.254.98</br>23.101.196.19 | \*:443 | Eingehend |
    | &nbsp; | USA, Westen 2 | 52.175.211.210</br>52.175.222.222 | \*:443 | Eingehend |

    Informationen zu den IP-Adressen, die für Azure Government verwendet werden, finden Sie im Dokument [Azure Government Intelligence + Analytics](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) (Azure Government – Daten und Analyse)

Weitere Informationen finden Sie im Abschnitt [Steuern des Netzwerkdatenverkehrs](#networktraffic).

Wenn Sie benutzerdefinierte Routen (User-Defined Routes, UDRs) verwenden, sollten Sie eine Route angeben und ausgehenden Datenverkehr vom VNET an die oben genannten IP-Adressen zulassen und den nächsten Hop dabei auf „Internet“ festlegen.
    
## <a id="hdinsight-ports"></a>Erforderliche Ports

Wenn Sie eine **Firewall** verwenden und extern über bestimmte Ports auf den Cluster zugreifen möchten, müssen Sie möglicherweise Datenverkehr an den für Ihr Szenario erforderlichen Ports zulassen. Standardmäßig ist keine spezifische Whitelist von Ports erforderlich, solange der im vorherigen Abschnitt erläuterte Azure-Verwaltungsdatenverkehr für Cluster an Port 443 zulässig ist.

Eine Liste mit Ports für bestimmte Dienste finden Sie im Dokument [Ports für Apache Hadoop-Dienste in HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Weitere Informationen zu Firewallregeln für virtuelle Geräte finden Sie im Dokument [Szenario für virtuelle Geräte](../virtual-network/virtual-network-scenario-udr-gw-nva.md).

## <a id="hdinsight-nsg"></a>Beispiel: Netzwerksicherheitsgruppen mit HDInsight

Die Beispiele in diesem Abschnitt veranschaulichen, wie Sie Netzwerksicherheitsgruppen-Regeln erstellen, die HDInsight die Kommunikation mit den Azure-Verwaltungsdiensten ermöglichen. Passen Sie vor dem Verwenden der Beispiele die IP-Adressen an die IP-Adressen der genutzten Azure-Region an. Sie finden diese Informationen im Abschnitt [HDInsight mit Netzwerksicherheitsgruppen und benutzerdefinierten Routen](#hdinsight-ip).

### <a name="azure-resource-management-template"></a>Azure Resource Manager-Vorlage

Mit der folgenden Resource Manager-Vorlage wird ein virtuelles Netzwerk erstellt, mit dem eingehender Datenverkehr eingeschränkt wird, während Datenverkehr von den IP-Adressen zugelassen wird, die für HDInsight benötigt werden. Mit dieser Vorlage wird auch ein HDInsight-Cluster im virtuellen Netzwerk erstellt.

* [Deploy a secured Azure Virtual Network and an HDInsight Hadoop cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/) (Bereitstellen eines geschützten Azure Virtual Network und eines HDInsight Hadoop-Clusters)

### <a name="azure-powershell"></a>Azure PowerShell

Verwenden Sie das folgende PowerShell-Skript, um ein virtuelles Netzwerk zu erstellen, bei dem eingehender Datenverkehr eingeschränkt und Datenverkehr von den IP-Adressen für die Region „Europa, Norden“ zugelassen wird.

> [!IMPORTANT]  
> Ändern Sie die IP-Adressen, die in diesem Beispiel für `hdirule1` und `hdirule2` verwendet werden, damit sie der von Ihnen genutzten Azure-Region entsprechen. Sie finden diese Informationen im Abschnitt [HDInsight mit Netzwerksicherheitsgruppen und benutzerdefinierten Routen](#hdinsight-ip).

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"

# Get the Virtual Network object
$vnet = Get-AzVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName

# Get the region the Virtual network is in.
$location = $vnet.Location

# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName

# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule3" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule4" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule5" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule6" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `

# Set the changes to the security group
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $nsg

# Apply the NSG to the subnet
Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
$vnet | Set-AzVirtualNetwork
```

In diesem Beispiel wird veranschaulicht, wie Sie Regeln hinzufügen, um eingehenden Datenverkehr für die erforderlichen IP-Adressen zuzulassen. Es enthält keine Regel zum Einschränken des Zugriffs in eingehender Richtung von anderen Quellen. Mit dem folgenden Code wird veranschaulicht, wie der SSH-Zugriff über das Internet aktiviert wird:

```powershell
Get-AzNetworkSecurityGroup -Name hdisecure -ResourceGroupName RESOURCEGROUP |
Add-AzNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Verwenden Sie die folgenden Schritte, um ein virtuelles Netzwerk zu erstellen, mit dem eingehender Datenverkehr eingeschränkt wird, während Datenverkehr von den IP-Adressen zugelassen wird, die für HDInsight benötigt werden.

1. Verwenden Sie den folgenden Befehl, um eine neue Netzwerksicherheitsgruppe namens `hdisecure`zu erstellen. Ersetzen Sie `RESOURCEGROUP` durch die Ressourcengruppe, die das virtuelle Azure-Netzwerk enthält. Ersetzen Sie `LOCATION` durch den Standort (Region), in dem die Gruppe erstellt wurde.

    ```azurecli
    az network nsg create -g RESOURCEGROUP -n hdisecure -l LOCATION
    ```

    Nachdem die Gruppe erstellt wurde, erhalten Sie Informationen über die neue Gruppe.

2. Gehen Sie wie folgt vor, um der neuen Netzwerksicherheitsgruppe Regeln hinzuzufügen, die eingehende Kommunikation auf Port 443 aus dem Azure HDInsight-Integritäts- und Verwaltungsdienst ermöglichen. Ersetzen Sie `RESOURCEGROUP` durch den Namen der Ressourcengruppe, die das virtuelle Azure-Netzwerk enthält.

    > [!IMPORTANT]  
    > Ändern Sie die IP-Adressen, die in diesem Beispiel für `hdirule1` und `hdirule2` verwendet werden, damit sie der von Ihnen genutzten Azure-Region entsprechen. Sie finden diese Informationen im Abschnitt [HDInsight mit Netzwerksicherheitsgruppen und benutzerdefinierten Routen](#hdinsight-ip).

    ```azurecli
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule3 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule4 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule6 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    ```

3. Verwenden Sie den folgenden Befehl, um den eindeutigen Bezeichner für diese Netzwerksicherheitsgruppe abzurufen:

    ```azurecli
    az network nsg show -g RESOURCEGROUP -n hdisecure --query "id"
    ```

    Dieser Befehl gibt einen Wert zurück, der in etwa wie folgt aussieht:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

4. Verwenden Sie den folgenden Befehl, um die Netzwerksicherheitsgruppe auf ein Subnetz anzuwenden. Ersetzen Sie die Werte `GUID` und `RESOURCEGROUP` durch die Rückgabewerte aus dem vorherigen Schritt. Ersetzen Sie `VNETNAME` und `SUBNETNAME` durch den Namen des zu erstellenden virtuellen Netzwerks bzw. durch den Namen des Subnetzes.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUP --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Nachdem die Ausführung dieses Befehls abgeschlossen ist, können Sie HDInsight im virtuellen Netzwerk installieren.


Mit diesen Schritten wird nur der Zugriff auf den HDInsight-Integritäts- und -Verwaltungsdienst in der Azure-Cloud ermöglicht. Jeder andere Zugriff auf den HDInsight-Cluster von außerhalb des virtuellen Netzwerks wird blockiert. Wenn Sie den Zugriff von außerhalb des virtuellen Netzwerks ermöglichen möchten, müssen Sie zusätzliche Netzwerksicherheitsgruppen-Regeln hinzufügen.

Mit dem folgenden Code wird veranschaulicht, wie der SSH-Zugriff über das Internet aktiviert wird:

```azurecli
az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n ssh --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
```

## <a id="example-dns"></a> Beispiel: DNS-Konfiguration

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Namensauflösung zwischen einem virtuellen Netzwerk und einem verbundenen lokalen Netzwerk

Für dieses Beispiel werden die folgenden Annahmen getroffen:

* Sie verfügen über ein Azure Virtual Network, das per VPN-Gateway mit einem lokalen Netzwerk verbunden ist.

* Auf dem benutzerdefinierten DNS-Server im virtuellen Netzwerk wird als Betriebssystem Linux oder Unix ausgeführt.

* Auf dem benutzerdefinierten DNS-Server ist [Bind](https://www.isc.org/downloads/bind/) installiert.

Auf dem benutzerdefinierten DNS-Server im virtuellen Netzwerk:

1. Verwenden Sie entweder Azure PowerShell oder Azure CLI, um nach dem DNS-Suffix des virtuellen Netzwerks zu suchen:

    Ersetzen Sie `RESOURCEGROUP` durch den Namen der Ressourcengruppe, die das virtuelle Netzwerk enthält, und geben Sie dann diesen Befehl ein:

    ```powershell
    $NICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP"
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Verwenden Sie auf dem benutzerdefinierten DNS-Server für das virtuelle Netzwerk den folgenden Text als Inhalt der Datei `/etc/bind/named.conf.local`:

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    Ersetzen Sie den Wert `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` durch das DNS-Suffix Ihres virtuellen Netzwerks.

    Bei dieser Konfiguration werden alle DNS-Anforderungen für das DNS-Suffix des virtuellen Netzwerks an den rekursiven Azure-Resolver geleitet.

2. Verwenden Sie auf dem benutzerdefinierten DNS-Server für das virtuelle Netzwerk den folgenden Text als Inhalt der Datei `/etc/bind/named.conf.options`:

    ```
    // Clients to accept requests from
    // TODO: Add the IP range of the joined network to this list
    acl goodclients {
        10.0.0.0/16; # IP address range of the virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent to the following
            forwarders {
                192.168.0.1; # Replace with the IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * Ersetzen Sie den Wert `10.0.0.0/16` durch den IP-Adressbereich Ihres virtuellen Netzwerks. Für diesen Eintrag sind Adressen für Namensauflösungsanforderungen innerhalb dieses Bereichs zulässig.

    * Fügen Sie den IP-Adressbereich des lokalen Netzwerks dem Abschnitt `acl goodclients { ... }` hinzu.  Für den Eintrag sind Namensauflösungsanforderungen von Ressourcen im lokalen Netzwerk zulässig.
    
    * Ersetzen Sie den Wert `192.168.0.1` durch die IP-Adresse Ihres lokalen DNS-Servers. Bei diesem Eintrag werden alle anderen DNS-Anforderungen an den lokalen DNS-Server geleitet.

3. Starten Sie Bind neu, um die Konfiguration zu verwenden. Beispiel: `sudo service bind9 restart`.

4. Fügen Sie dem lokalen DNS-Server eine bedingte Weiterleitung hinzu. Konfigurieren Sie die bedingte Weiterleitung, um Anforderungen für das DNS-Suffix aus Schritt 1 an den benutzerdefinierten DNS-Server zu senden.

    > [!NOTE]  
    > Die Dokumentation Ihrer DNS-Software enthält ausführliche Informationen zum Hinzufügen einer bedingten Weiterleitung.

Nach Ausführung dieser Schritte können Sie mit den Ressourcen in beiden Netzwerken eine Verbindung herstellen, indem Sie vollqualifizierte Domänennamen (FQDNs) verwenden. Sie können HDInsight jetzt im virtuellen Netzwerk installieren.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>Namensauflösung zwischen zwei verbundenen virtuellen Netzwerken

Für dieses Beispiel werden die folgenden Annahmen getroffen:

* Sie verfügen über zwei Azure Virtual Networks, die per VPN-Gateway oder Peering verbunden sind.

* Auf dem benutzerdefinierten DNS-Server in beiden Netzwerken wird als Betriebssystem Linux oder Unix ausgeführt.

* Auf den benutzerdefinierten DNS-Servern ist [Bind](https://www.isc.org/downloads/bind/) installiert.

1. Verwenden Sie entweder Azure PowerShell oder Azure CLI, um nach dem DNS-Suffix der beiden virtuellen Netzwerke zu suchen:

    Ersetzen Sie `RESOURCEGROUP` durch den Namen der Ressourcengruppe, die das virtuelle Netzwerk enthält, und geben Sie dann diesen Befehl ein:

    ```powershell
    $NICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP"
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Verwenden Sie den folgenden Text als Inhalt der Datei `/etc/bind/named.config.local` auf dem benutzerdefinierten DNS-Server. Nehmen Sie diese Änderung auf dem benutzerdefinierten DNS-Server in beiden virtuellen Netzwerken vor.

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    Ersetzen Sie den Wert `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` durch das DNS-Suffix des __anderen__ virtuellen Netzwerks. Mit diesem Eintrag werden Anforderungen für das DNS-Suffix des Remotenetzwerks an das benutzerdefinierte DNS in diesem Netzwerk geleitet.

3. Verwenden Sie auf den benutzerdefinierten DNS-Servern der beiden virtuellen Netzwerke den folgenden Text als Inhalt der Datei `/etc/bind/named.conf.options`:

    ```
    // Clients to accept requests from
    acl goodclients {
        10.1.0.0/16; # The IP address range of one virtual network
        10.0.0.0/16; # The IP address range of the other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
   Ersetzen Sie die Werte `10.0.0.0/16` und `10.1.0.0/16` durch die IP-Adressbereiche Ihrer virtuellen Netzwerke. Bei diesem Eintrag ist es für Ressourcen in beiden Netzwerken zulässig, Anforderungen an die DNS-Server zu senden.

    Alle Anforderungen, die nicht für die DNS-Suffixe der virtuellen Netzwerke bestimmt sind (z.B. „microsoft.com“), werden vom rekursiven Azure-Resolver verarbeitet.

4. Starten Sie Bind neu, um die Konfiguration zu verwenden. Beispiel: `sudo service bind9 restart` auf beiden DNS-Servern.

Nach Ausführung dieser Schritte können Sie mit den Ressourcen im virtuellen Netzwerk eine Verbindung herstellen, indem Sie vollqualifizierte Domänennamen (FQDNs) verwenden. Sie können HDInsight jetzt im virtuellen Netzwerk installieren.

## <a name="next-steps"></a>Nächste Schritte

* Ein umfassendes Beispiel für die Konfiguration von HDInsight zum Herstellen einer Verbindung mit einem lokalen Netzwerk finden Sie unter [Connect HDInsight to an on-premises network](./connect-on-premises-network.md) (Verbinden von HDInsight mit einem lokalen Netzwerk).
* Informationen zum Konfigurieren von Apache HBase-Clustern in virtuellen Azure-Netzwerken finden Sie unter [Erstellen von Apache HBase-Clustern in HDInsight in Azure Virtual Network](hbase/apache-hbase-provision-vnet.md).
* Informationen zum Konfigurieren der Apache HBase-Georeplikation finden Sie unter [Einrichten der Apache HBase-Clusterreplikation in virtuellen Azure-Netzwerken](hbase/apache-hbase-replication.md).
* Weitere Informationen zu virtuellen Azure-Netzwerken finden Sie in der [Übersicht zu virtuellen Azure-Netzwerken](../virtual-network/virtual-networks-overview.md).

* Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie in der [Übersicht zu Netzwerksicherheitsgruppen](../virtual-network/security-overview.md).

* Weitere Informationen zu benutzerdefinierten Routen finden Sie unter [Benutzerdefinierte Routen und IP-Weiterleitung](../virtual-network/virtual-networks-udr-overview.md).
