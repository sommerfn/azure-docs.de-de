---
title: Übersicht über Azure-Diensttags
titlesuffix: Azure Virtual Network
description: Hier erhalten Sie Informationen zu Diensttags. Diensttags reduzieren die Komplexität beim Erstellen von Sicherheitsregeln.
services: virtual-network
documentationcenter: na
author: jispar
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/22/2019
ms.author: jispar
ms.reviewer: kumud
ms.openlocfilehash: 0eb15d1b9b56522b9caa1bb10890eb2b485714e8
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73586851"
---
# <a name="virtual-network-service-tags"></a>Diensttags in virtuellen Netzwerken 
<a name="network-service-tags"></a>

Ein Diensttag steht für eine Gruppe von IP-Adresspräfixen eines bestimmten Azure-Diensts. Mit Diensttags lässt sich die Komplexität häufiger Aktualisierungen von Netzwerksicherheitsregeln verringern. Sie können Diensttags verwenden, um Netzwerkzugriffssteuerungen in [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) oder [Azure Firewall](https://docs.microsoft.com/azure/firewall/service-tags) zu definieren. Sie können Diensttags anstelle von spezifischen IP-Adressen nutzen, wenn Sie Sicherheitsregeln erstellen. Indem Sie den Diensttagnamen (z. B. **ApiManagement**) im entsprechenden Feld *Quelle* oder *Ziel* einer Regel angeben, können Sie den Datenverkehr für den entsprechenden Dienst zulassen oder verweigern. Microsoft verwaltet die Adresspräfixe, für die das Diensttag gilt, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern. 

## <a name="available-service-tags"></a>Verfügbare Diensttags
Die folgende Tabelle führt alle Diensttags auf, die zur Verwendung in Regeln für [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) verfügbar sind.

In den Spalten wird angegeben, ob für das jeweilige Tag Folgendes gilt:

- Geeignet für Regeln für eingehenden oder ausgehenden Datenverkehr
- Unterstützung für einen [regionalen](https://azure.microsoft.com/regions) Bereich 
- Verwendbar in [Azure Firewall](https://docs.microsoft.com/azure/firewall/service-tags)-Regeln

Standardmäßig spiegeln Diensttags die Bereiche für die gesamte Cloud wider.  Einige Diensttags ermöglichen eine präzisere Steuerung, indem sie die entsprechenden IP-Adressbereiche auf eine bestimmte Region beschränken.  Das Diensttag **Storage** beispielsweise repräsentiert Azure Storage für die gesamte Cloud, **Storage.WestUS** dagegen führt zu einer Beschränkung auf die IP-Adressbereiche der Region „USA, Westen“.  Die Beschreibungen für jedes Diensttag informieren darüber, ob die Tags einen solchen regionsbezogenen Bereich unterstützen.  



| Tag | Zweck | Eingehend oder ausgehend möglich? | Regional möglich? | Einsatz mit Azure Firewall möglich? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ApiManagement** | Verwaltungsdatenverkehr für dedizierte APIM-Bereitstellungen. | Beides | Nein | Ja |
| **AppService**    | App Service-Dienst. Dieses Tag wird für ausgehende Sicherheitsregeln zu WebApp-Front-Ends empfohlen. | Ausgehend | Ja | Ja |
| **AppServiceManagement** | Verwaltungsdatenverkehr für dedizierte Bereitstellungen der App Service-Umgebung. | Beides | Nein | Ja |
| **AzureActiveDirectory** | Azure Active Directory-Dienst. | Ausgehend | Nein | Ja |
| **AzureActiveDirectoryDomainServices** | Verwaltungsdatenverkehr für dedizierte Azure Active Directory Domain Services-Bereitstellungen. | Beides | Nein | Ja |
| **AzureBackup** |Azure Backup-Dienst.<br/><br/>*Hinweis:* Dieses Tag weist eine Abhängigkeit vom Tag **Storage** und **AzureActiveDirectory** auf. | Ausgehend | Nein | Ja |
| **AzureCloud** | Alle [öffentlichen IP-Adressen im Rechenzentrum](https://www.microsoft.com/download/details.aspx?id=41653). | Ausgehend | Ja | Ja |
| **AzureConnectors** | Logic Apps-Connectors für Test- oder Back-End-Verbindungen. | Eingehend | Ja | Ja |
| **AzureContainerRegistry** | Azure Container Registry-Dienst. | Ausgehend | Ja | Ja |
| **AzureCosmosDB** | Azure Cosmos-Datenbank-Dienst. | Ausgehend | Ja | Ja |
| **AzureDataLake** | Azure Data Lake-Dienst. | Ausgehend | Nein | Ja |
| **AzureKeyVault** | Azure Key Vault-Dienst.<br/><br/>*Hinweis:* Dieses Tag weist eine Abhängigkeit vom Tag **AzureActiveDirectory** auf. | Ausgehend | Ja | Ja |
| **AzureLoadBalancer** | Lastenausgleichsmodul der Azure-Infrastruktur. Das Tag wird in eine [virtuelle IP-Adresse des Hosts](security-overview.md#azure-platform-considerations) (168.63.129.16) umgewandelt, die als Ausgangspunkt für die Integritätstests von Azure fungiert. Sie können diese Regel außer Kraft setzen, wenn Sie den Lastenausgleich von Azure nicht verwenden. | Beides | Nein | Nein |
| **AzureMachineLearning** | Azure Machine Learning Service. | Ausgehend | Nein | Ja |
| **AzureMonitor** | Log Analytics, App Insights, AzMon und benutzerdefinierte Metriken (GiG-Endpunkte).<br/><br/>*Hinweis:* Für Log Analytics weist dieses Tag eine Abhängigkeit von dem Tag **Storage** auf. | Ausgehend | Nein | Ja |
| **AzurePlatformDNS** | Der grundlegende (standardmäßige) DNS-Dienst für die Infrastruktur.<br/><br>Sie können dieses Tag verwenden, um den standardmäßigen DNS-Dienst zu deaktivieren. Verwenden Sie dieses Tag mit Vorsicht. Informieren Sie sich über die notwendigen [Aspekte der Azure Platform](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations). Es wird empfohlen, vor Verwendung dieses Tags Tests durchzuführen. | Ausgehend | Nein | Nein |
| **AzurePlatformIMDS** | IMDS, ein grundlegender Infrastrukturdienst.<br/><br/>Sie können dieses Tag verwenden, um den standardmäßigen IMDS-Dienst zu deaktivieren.  Verwenden Sie dieses Tag mit Vorsicht. Informieren Sie sich über die notwendigen [Aspekte der Azure Platform](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations). Es wird empfohlen, vor Verwendung dieses Tags Tests durchzuführen. | Ausgehend | Nein | Nein |
| **AzurePlatformLKM** | Windows-Lizenzierungs- oder Schlüsselverwaltungsdienst.<br/><br/>Sie können dieses Tag verwenden, um die Standardwerte für die Lizenzierung zu deaktivieren. Verwenden Sie dieses Tag mit Vorsicht.  Informieren Sie sich über die notwendigen [Aspekte der Azure Platform](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations). Es wird empfohlen, vor Verwendung dieses Tags Tests durchzuführen. | Ausgehend | Nein | Nein |
| **AzureTrafficManaged** | Test-IP-Adressen von Azure Traffic Manager.<br/><br/>Weitere Informationen zu Test-IP-Adressen von Traffic Manager finden Sie unter [Häufig gestellte Fragen (FAQ) zu Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs). | Eingehend | Nein | Ja |  
| **BatchNodeManagement** | Verwaltungsdatenverkehr für dedizierte Azure Batch-Bereitstellungen. | Beides | Nein | Ja |
| **CognitiveServicesManagement** | Die Adressbereiche für den Datenverkehr für Cognitive Services. | Ausgehend | Nein | Nein |
| **Dynamics365ForMarketingEmail** | Die Adressbereiche für den Marketing-E-Mail-Dienst von Dynamics 365. | Ausgehend | Ja | Nein |
| **EventHub** | Azure Event Hub-Dienst. | Ausgehend | Ja | Ja |
| **GatewayManager** | Verwaltungsdatenverkehr für dedizierte VPN/App Gateway-Bereitstellungen. | Eingehend | Nein | Nein |
| **Internet** | Der IP-Adressraum, der außerhalb des virtuellen Netzwerks liegt und über das öffentliche Internet erreichbar ist.<br/><br/>Der Adressbereich schließt den [Azure-eigenen öffentlichen IP-Adressraum](https://www.microsoft.com/download/details.aspx?id=41653) ein. | Beides | Nein | Nein |
| **MicrosoftContainerRegistry** | Microsoft-Containerregistrierungsdienst. | Ausgehend | Ja | Ja |
| **ServiceBus** | Azure Service Bus-Dienst im Diensttarif „Premium“. | Ausgehend | Ja | Ja |
| **ServiceFabric** | Service Fabric-Dienst. | Ausgehend | Nein | Nein |
| **SQL** | Dienste für Azure SQL-Datenbank, Azure Database for MySQL, Azure Database for PostgreSQL und Azure SQL Data Warehouse.<br/><br/>*Hinweis:* Dieses Tag steht für den Dienst, nicht für bestimmte Instanzen des Diensts. Beispielsweise steht das Tag für den Azure SQL-Datenbank-Dienst, aber nicht für eine bestimmte SQL-Datenbank oder einen bestimmten SQL-Server. | Ausgehend | Ja | Ja |
| **SqlManagement** | Verwaltungsdatenverkehr für dedizierte SQL-Bereitstellungen. | Beides | Nein | Ja |
| **Speicher** | Azure Storage-Dienst. <br/><br/>*Hinweis:* Das Tag steht für den Dienst, aber nicht für bestimmte Instanzen des Diensts. Beispielsweise steht das Tag für den Azure Storage-Dienst, aber nicht für ein bestimmtes Azure Storage-Konto. | Ausgehend | Ja | Ja |
| **VirtualNetwork** | Der Adressraum des virtuellen Netzwerks (alle für das virtuelle Netzwerk definierten IP-Adressbereiche), alle verbundenen lokalen Adressräume, [per Peering verknüpfte](virtual-network-peering-overview.md) virtuelle Netzwerke oder virtuelle Netzwerke, die mit einem [Gateway des virtuellen Netzwerks](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%3ftoc.json) verbunden sind, die [virtuelle IP-Adresse des Hosts](security-overview.md#azure-platform-considerations) und Adresspräfixe, die in [benutzerdefinierten Routen](virtual-networks-udr-overview.md) verwendet werden. Beachten Sie, dass dieses Tag auch Standardrouten enthalten kann. | Beides | Nein | Nein |

>[!NOTE]
>Wenn Sie in einer *klassischen* Umgebung (vor Azure Resource Manager) arbeiten, wird ein ausgewählter Teil der oben beschriebenen Tags unterstützt.  Diese verwenden eine andere Schreibweise:

| Klassische Schreibweise | Entsprechendes Resource Manager-Tag |
|---|---|
| AZURE_LOADBALANCER | AzureLoadBalancer |
| INTERNET | Internet |
| VIRTUAL_NETWORK | VirtualNetwork |

> [!NOTE]
> Mit Diensttags von Azure-Diensten werden die Adresspräfixe der spezifischen verwendeten Cloud angegeben. Beispiel: Die zugrunde liegenden IP-Adressbereiche, die dem Wert des **Sql**-Tags entsprechen, unterscheiden sich zwischen der öffentlichen Azure-Cloud und der Azure-Cloud China.

> [!NOTE]
> Wenn Sie einen [VNET-Dienstendpunkt](virtual-network-service-endpoints-overview.md) für einen Dienst implementieren, z. B. Azure Storage oder Azure SQL-Datenbank, fügt Azure eine [Route](virtual-networks-udr-overview.md#optional-default-routes) zu einem Subnetz des virtuellen Netzwerks für den Dienst hinzu. Die Adresspräfixe für die Route sind die gleichen Adresspräfixe bzw. CIDR-Bereiche wie für das entsprechende Diensttag.



## <a name="service-tags-in-on-premises"></a>Diensttags in lokalen Firewalls  
Sie können die aktuellen Informationen zu Diensttag und Bereich abrufen und in Ihren lokalen Firewallkonfigurationen verwenden.  Bei diesen Informationen handelt es sich um die zum jetzigen Zeitpunkt gültige Liste der IP-Adressbereiche, die den einzelnen Diensttags entsprechen.  Die Informationen können wie folgt programmgesteuert oder per JSON-Dateidownload abgerufen werden.

### <a name="service-tag-discovery-api-public-preview"></a>Diensttagermittlungs-API (öffentliche Vorschau)
Sie können die aktuelle Liste der Diensttags mit Details zum IP-Adressbereich programmgesteuert abrufen:

- [REST](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
- [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag?view=azps-2.8.0&viewFallbackFrom=azps-2.3.2)
- [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/network?view=azure-cli-latest#az-network-list-service-tags)

> [!NOTE]
> In der öffentlichen Vorschau gibt die Ermittlungs-API möglicherweise Informationen zurück, die weniger aktuell als die JSON-Downloads sind (siehe unten).


### <a name="discover-service-tags-using-downloadable-json-files"></a>Ermitteln von Diensttags mithilfe von herunterladbaren JSON-Dateien 
Sie können JSON-Dateien mit der aktuellen Liste der Diensttags mit Details zum IP-Adressbereich herunterladen. Diese werden wöchentlich aktualisiert und veröffentlicht.  Hier finden Sie die Speicherorte für die einzelnen Clouds:

- [Azure öffentlich](https://www.microsoft.com/download/details.aspx?id=56519)
- [Azure Government](https://www.microsoft.com/download/details.aspx?id=57063)  
- [Azure China](https://www.microsoft.com/download/details.aspx?id=57062) 
- [Azure Deutschland](https://www.microsoft.com/download/details.aspx?id=57064)   

> [!NOTE]
>Ein Teil dieser Informationen wurde bereits in XML-Dateien für [Azure öffentlich](https://www.microsoft.com/download/details.aspx?id=41653), [Azure China](https://www.microsoft.com/download/details.aspx?id=42064) und [Azure Deutschland](https://www.microsoft.com/download/details.aspx?id=54770) veröffentlicht. Diese XML-Downloads werden zum 30. Juni 2020 eingestellt und stehen nach diesem Datum nicht mehr zur Verfügung. Verwenden Sie stattdessen die Ermittlungs-API oder JSON-Dateidownloads, wie oben beschrieben.

### <a name="tips"></a>Tipps 
- Sie können Updates zwischen verschiedenen Veröffentlichungen anhand der erhöhten *changeNumber*-Werte in der JSON-Datei erkennen. Jeder Teilabschnitt (z. B. **Storage.WestUS**) weist einen eigenen *changeNumber*-Wert auf, der bei Änderungen erhöht wird.  Die oberste Ebene des *changeNumber*-Werts einer Datei wird erhöht, wenn einer der Teilabschnitte geändert wurde.
- Beispiele für die Analyse der Diensttaginformationen (z. B. zum Abrufen aller Adressbereiche für „Storage“ in „USA, Westen) finden Sie in der Dokumentation zur [Diensttagermittlungs-API in PowerShell](https://aka.ms/discoveryapi_powershell).

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen einer Netzwerksicherheitsgruppe](tutorial-filter-network-traffic.md)

