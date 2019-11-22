---
title: Azure Files – Überlegungen zum Netzwerkbetrieb | Microsoft-Dokumentation
description: Hier finden Sie eine Übersicht über die Netzwerkoptionen für Azure Files.
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 10/19/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 596479652478bffb6d18a90fc53d5972b3839408
ms.sourcegitcommit: b45ee7acf4f26ef2c09300ff2dba2eaa90e09bc7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73126536"
---
# <a name="azure-files-networking-considerations"></a>Azure Files – Überlegungen zum Netzwerkbetrieb 
Sie können auf zwei Arten eine Verbindung mit einer Azure-Dateifreigabe herstellen:

- Sie greifen über die Protokolle SMB oder FileREST direkt auf die Freigabe zu. Dieses Zugriffsmuster kommt primär dann zum Einsatz, wenn so wenige lokale Server wie möglich einbezogen werden sollen.
- Sie erstellen mithilfe der Azure-Dateisynchronisierung einen Cache der Azure-Dateifreigabe auf einem lokalen Server und greifen vom lokalen Server über ein Protokoll Ihrer Wahl (SMB, NFS, FTPS usw.) auf die gewünschten Daten der Dateifreigabe zu. Dieses Zugriffsmuster ist praktisch, weil es die Vorteile einer lokalen Leistung mit serverlos anfügbaren Diensten im Cloudmaßstab – z. B. Azure Backup – kombiniert.

In diesem Artikel wird beschrieben, wie Sie das Netzwerk konfigurieren, wenn Ihr Anwendungsfall einen direkten Zugriff auf die Azure-Dateifreigabe statt der Nutzung der Azure-Dateisynchronisierung erfordert. Weitere Informationen zu Überlegungen zum Netzwerkbetrieb für eine Bereitstellung mit der Azure-Dateisynchronisierung finden Sie unter [Proxy- und Firewalleinstellungen der Azure-Dateisynchronisierung](storage-sync-files-firewall-and-proxy.md).

## <a name="storage-account-settings"></a>Speicherkontoeinstellungen
Ein Speicherkonto ist ein Verwaltungskonstrukt, das einen gemeinsam genutzten Pool mit Speicherplatz darstellt, in dem Sie mehrere Dateifreigaben sowie weitere Speicherressourcen wie Blobcontainer oder Warteschlangen bereitstellen können. Azure-Speicherkonten stellen zwei grundlegende Arten von Einstellungen zur Verfügung, um das Netzwerk zu schützen: Verschlüsselung während der Übertragung sowie Firewalls und virtuelle Netzwerke (VNETs).

### <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung
Standardmäßig ist in allen Azure-Speicherkonten die Verschlüsselung während der Übertragung aktiviert. Das bedeutet Folgendes: Wenn Sie eine Dateifreigabe über SMB einbinden oder über FileREST darauf zugreifen (per Azure-Portal, PowerShell/CLI oder Azure-SDKs), lässt Azure Files die Verbindung nur dann zu, wenn sie über SMB 3.0 oder höher mit Verschlüsselung oder über HTTPS hergestellt wird. Clients, die SMB 3.0 nicht unterstützen, oder Clients, die zwar SMB 3.0, aber nicht die SMB-Verschlüsselung unterstützen, können die Azure-Dateifreigabe nicht einbinden, wenn die Verschlüsselung während der Übertragung aktiviert ist. Weitere Informationen dazu, welche Betriebssysteme SMB 3.0 mit Verschlüsselung unterstützen, finden Sie in der ausführlichen Dokumentation zu [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md) und [Linux](storage-how-to-use-files-linux.md). Alle aktuellen PowerShell-, CLI- und SDK-Versionen unterstützen HTTPS.  

Sie können die Verschlüsselung während der Übertragung für ein Azure-Speicherkonto deaktivieren. Wenn die Verschlüsselung deaktiviert ist, lässt Azure Files auch SMB 2.1, SMB 3.0 ohne Verschlüsselung und nicht verschlüsselte FileREST-API-Aufrufe über HTTP zu. Der Hauptgrund für die Deaktivierung der Verschlüsselung während der Übertragung ist die Unterstützung einer älteren Anwendung, die unter einem älteren Betriebssystem wie z. B. Windows Server 2008 R2 oder einer älteren Linux-Distribution ausgeführt werden muss. Azure Files lässt nur SMB 2.1-Verbindungen innerhalb der gleichen Region zu, in der sich auch die Azure-Dateifreigabe befindet. Ein SMB 2.1-Client außerhalb der Azure-Region der Azure-Dateifreigabe – z. B. ein lokales System oder eine andere Azure-Region – kann nicht auf die Dateifreigabe zugreifen.

Weitere Informationen zur Verschlüsselung während der Übertragung finden Sie unter [Vorschreiben einer sicheren Übertragung in Azure Storage](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

### <a name="firewalls-and-virtual-networks"></a>Firewalls und virtuelle Netzwerke 
Eine Firewall ist eine Netzwerkrichtlinie, die steuert, welche Anforderungen auf die Azure-Dateifreigaben und andere Speicherressourcen in Ihrem Speicherkonto zugreifen dürfen. Wenn ein Speicherkonto mit den standardmäßigen Netzwerkeinstellungen erstellt wird, ist es nicht auf ein bestimmtes Netzwerk beschränkt und daher über das Internet erreichbar. Das bedeutet nicht, dass jeder beliebige Benutzer im Internet auf die Daten zugreifen kann, die in den Azure-Dateifreigaben in Ihrem Speicherkonto gehostet werden, sondern dass das Speicherkonto autorisierte Anforderungen aus jedem Netzwerk akzeptiert. Anforderungen können mit einem Speicherkontoschlüssel, einem SAS-Token (nur FileREST) oder einem Active Directory-Benutzerprinzipal autorisiert werden. 

Die Firewallrichtlinie für ein Speicherkonto kann verwendet werden, um den Zugriff auf bestimmte IP-Adressen oder -Adressbereiche oder auf ein virtuelles Netzwerk zu beschränken. Im Allgemeinen beschränken die meisten Firewallrichtlinien für Speicherkonten den Netzwerkzugriff auf ein virtuelles Netzwerk. 

Ein [virtuelles Netzwerk](../../virtual-network/virtual-networks-overview.md) (VNET) ähnelt einem herkömmlichen Netzwerk, das Sie in Ihrem eigenen Rechenzentrum betreiben. Es ermöglicht Ihnen, einen sicheren Kommunikationskanal zu erstellen, über den Ihre Azure-Ressourcen – z. B. Azure-Dateifreigaben, VMs, SQL-Datenbanken usw. – miteinander kommunizieren können. Ähnlich wie ein Azure-Speicherkonto oder eine Azure-VM ist ein VNET eine Azure-Ressource, die in einer Ressourcengruppe bereitgestellt wird. Mit einer zusätzlichen Netzwerkkonfiguration können Azure-VNETs auch mit Ihren lokalen Netzwerken verbunden werden.

Wenn Ressourcen wie beispielsweise eine Azure-VM zu einem virtuellen Netzwerk hinzugefügt werden, wird eine angefügte virtuelle Netzwerkschnittstelle auf speziell dieses VNET beschränkt. Dies ist möglich, weil Azure-VMs virtualisierte Computer sind, die natürlich über Netzwerkschnittstellen verfügen. Virtuelle Computer werden im Rahmen der IaaS-Produktfamilie (Infrastructure-as-a-Service) von Azure angeboten. Da es sich bei Azure-Dateifreigaben um serverlose Dateifreigaben handelt, verfügen sie nicht über eine Netzwerkschnittstelle, die Sie einem VNET hinzufügen können. Anders gesagt: Azure Files werden im Rahmen der PaaS-Produktfamilie (Platform-as-a-Service) von Azure angeboten. Damit ein Speicherkonto Bestandteil eines VNETs sein kann, unterstützt Azure ein Konzept für PaaS-Dienste, das als Dienstendpunkt bezeichnet wird. Ein Dienstendpunkt ermöglicht die Ausführung von PaaS-Diensten in einem virtuellen Netzwerk. Weitere Informationen zu Dienstendpunkten finden Sie unter [VNET-Dienstendpunkte](../../virtual-network/virtual-network-service-endpoints-overview.md).

Ein Speicherkonto kann einem oder mehreren virtuellen Netzwerken hinzugefügt werden. Weitere Informationen zum Hinzufügen eines Speicherkontos zu einem virtuellen Netzwerk oder zum Konfigurieren weiterer Firewalleinstellungen finden Sie unter [Konfigurieren von Azure Storage-Firewalls und virtuellen Netzwerken](../common/storage-network-security.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

## <a name="azure-networking"></a>Azure-Netzwerke
Standardmäßig kann über das Internet auf Azure-Dienste wie Azure Files zugegriffen werden. Da der Datenverkehr zu Ihrem Speicherkonto standardmäßig verschlüsselt ist (und SMB 1.2-Einbindungen außerhalb einer Azure-Region nie zulässig sind), besteht keine immanente Gefahr durch den Zugriff auf Ihre Azure-Dateifreigaben über das Internet. Je nach den Richtlinien Ihres Unternehmens oder gesetzlichen Anforderungen müssen Sie möglicherweise eine restriktivere Kommunikation mit Azure erzwingen. Daher bietet Azure verschiedene Möglichkeiten, den Azure-externen, an Azure Files gerichteten Datenverkehr zu beschränken. Sie können Ihren Netzwerkbetrieb noch weiter schützen, indem Sie über eins der folgenden Dienstangebote auf Ihre Azure-Dateifreigabe zugreifen:

- [Azure VPN Gateway](../../vpn-gateway/vpn-gateway-about-vpngateways.md): Ein VPN-Gateway ist eine spezielle Art von Gateway für virtuelle Netzwerke, das verwendet wird, um verschlüsselten Datenverkehr zwischen einem virtuellen Azure-Netzwerk und einem anderen Standort (beispielsweise einer lokalen Umgebung) über das Internet zu senden. Azure VPN Gateway ist eine Azure-Ressource, die neben einem Speicherkonto oder anderen Azure-Ressourcen in einer Ressourcengruppe bereitgestellt werden kann. VPN-Gateways machen zwei Arten von Verbindungen verfügbar:
    - [Point-to-Site-VPN](../../vpn-gateway/point-to-site-about.md)-Gatewayverbindungen (P2S-VPN) sind VPN-Verbindungen zwischen Azure und einem einzelnen Client. Diese Lösung ist primär für Geräte nützlich, die nicht zum lokalen Netzwerk Ihrer Organisation gehören, z. B. im Fall von Telearbeitern, die ihre Azure-Dateifreigabe von Zuhause, im Café oder im Hotel einbinden möchten. Um eine P2S-VPN-Verbindung mit Azure Files zu verwenden, muss für jeden Client, der eine Verbindung herstellen möchte, eine P2S-VPN-Verbindung konfiguriert werden. 
    - [Site-to-Site-VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)-Verbindungen (S2S-VPN) sind VPN-Verbindungen zwischen Azure und dem Netzwerk Ihrer Organisation. Mit dieser Lösung können Sie einmalig eine VPN-Verbindung für einen VPN-Server oder ein im Netzwerk Ihrer Organisation gehostetes Gerät konfigurieren und müssen diese Einrichtung nicht für jedes Clientgerät wiederholen, das auf Ihre Azure-Dateifreigabe zugreifen muss.
- [ExpressRoute](../../expressroute/expressroute-introduction.md) ermöglicht die Erstellung einer definierten Route zwischen Azure und Ihrem lokalen Netzwerk, die nicht über das Internet läuft. Da ExpressRoute einen dedizierten Pfad zwischen Ihrem lokalen Rechenzentrum und Azure bereitstellt, ist dieser Dienst sehr nützlich, wenn die Netzwerkleistung ein wichtiger Aspekt ist. ExpressRoute ist auch dann eine gute Option, wenn die Richtlinie Ihrer Organisation oder gesetzliche Vorschriften einen deterministischen Pfad zu den Ressourcen in der Cloud erfordern.

## <a name="securing-access-from-on-premises"></a>Schützen des Zugriffs aus lokalen Umgebungen 
Wenn Sie Dateifreigaben für allgemeine Zwecke (für Dateien wie beispielsweise Office-Dokumente, PDFs, CAD-Dokumente usw.) zu Azure Files migrieren, müssen Ihre Benutzer in der Regel weiterhin von lokalen Geräten (Arbeitsstationen, Laptops, Tablets) auf ihre Dateien zugreifen. Der wichtigste Aspekt bei einer Dateifreigabe für allgemeine Zwecke ist die Frage, wie lokale Benutzer sicher über das Internet oder WAN auf ihre Dateifreigaben zugreifen können.

Die einfachste Möglichkeit, aus einer lokalen Umgebung auf Ihre Azure-Dateifreigabe zuzugreifen, besteht darin, Port 445 (den von SMB verwendeten Port) Ihres lokalen Netzwerks zu öffnen und den über das Azure-Portal bereitgestellten UNC-Pfad einzubinden. Dafür sind keinen besonderen Netzwerkeinstellungen erforderlich. Viele Kunden öffnen Port 445 aufgrund veralteter Sicherheitsinformationen zu SMB 1.0 nur ungern. Microsoft betrachtet SMB 1.0 nicht als sicheres Protokoll für den Internetzugriff. Azure Files implementiert SMB 1.0 nicht. 

SMB 3.0 wurde mit der expliziten Anforderung konzipiert, ein sicheres Dateifreigabeprotokoll für den Internetzugriff darzustellen. Wenn Sie daher SMB 3.0 und höher verwenden, besteht aus Sicht des Computernetzwerks kein Unterschied, ob Sie Port 445 oder Port 443 öffnen, den für HTTPS-Verbindungen genutzten Port. Anstatt Port 445 zu blockieren, um unsicheren Datenverkehr über SMB 1.0 zu verhindern, empfiehlt Microsoft die folgenden Schritte:

> [!Important]  
> Auch wenn Sie Port 445 für ausgehenden Datenverkehr geschlossen lassen, empfiehlt Microsoft, die folgenden Schritte dennoch auszuführen, um SMB 1.0 aus Ihrer Umgebung zu entfernen.

1. Stellen Sie sicher, dass SMB 1.0 auf den Geräten Ihrer Organisation entfernt oder deaktiviert wurde. Alle derzeit unterstützten Versionen von Windows und Windows Server unterstützen das Entfernen oder Deaktivieren von SMB 1.0. Ab Windows 10, Version 1709, ist SMB 1.0 nicht mehr standardmäßig im Betriebssystem installiert. Weitere Informationen zum Deaktivieren von SMB 1.0 finden Sie auf den Seiten zum jeweiligen Betriebssystem:
    - [Sichern von Windows/Windows Server](storage-how-to-use-files-windows.md#securing-windowswindows-server)
    - [Sichern von Linux](storage-how-to-use-files-linux.md#securing-linux)
1. Stellen Sie sicher, dass keine Produkte in Ihrer Organisation SMB 1.0 benötigen, und entfernen Sie diejenigen Produkte, die diese Version verwenden. Im Blog [SMB1 Product Clearinghouse](https://aka.ms/stillneedssmb1) finden Sie alle Erst- und Drittanbieterprodukte, von denen Microsoft bekannt ist, dass sie SMB 1.0 erfordern. 
1. (Optional) Verwenden Sie eine Drittanbieterfirewall im lokalen Netzwerk Ihrer Organisation, um Datenverkehr über SMB 1.0 zu verhindern.

Wenn Port 445 in Ihrer Organisation aufgrund einer Richtlinie oder gesetzlichen Vorgabe blockiert werden muss, können Sie Azure VPN Gateway oder ExpressRoute verwenden, um Datenverkehr über Port 443 zu tunneln. Weitere Informationen zu den Schritten zur Bereitstellung dieser Optionen finden Sie auf den jeweiligen Seiten:
- [Konfigurieren eines S2S-VPN (Site-to-Site) zur Verwendung mit Azure Files](storage-files-configure-s2s-vpn.md)
- [Konfigurieren eines P2S-VPN (Point-to-Site) unter Windows zur Verwendung mit Azure Files](storage-files-configure-p2s-vpn-windows.md)
- [Konfigurieren eines P2S-VPN (Point-to-Site) unter Linux zur Verwendung mit Azure Files](storage-files-configure-p2s-vpn-linux.md)

In Ihrer Organisation gilt möglicherweise die weitere Anforderung, dass vom lokalen Standort ausgehender Datenverkehr einem deterministischen Pfad zu Ihren Ressourcen in der Cloud folgen muss. Wenn dies der Fall ist, kann ExpressRoute diese Anforderung erfüllen.

## <a name="securing-access-from-cloud-resources"></a>Schützen des Zugriffs von Cloudressourcen
Wenn eine lokale Anwendung per Lift & Shift in die Cloud verlagert wird, werden Anwendung und Anwendungsdaten gleichzeitig verschoben. Das bedeutet, dass bei Lift & Shift-Migrationen der Zugriff auf die Azure-Dateifreigabe auf diejenigen virtuellen Computer oder Azure-Dienste beschränkt werden muss, die auf diese Dateifreigabe zugreifen müssen, damit sie funktionieren. 

Sie können VNETs verwenden, um festzulegen, welche VMs oder anderen Azure-Ressourcen Netzwerkverbindungen (SMB-Einbindungen oder REST-API-Aufrufe Ihrer Azure-Dateifreigabe) herstellen dürfen. Es wird empfohlen, die Azure-Dateifreigabe in einem VNET zu platzieren, wenn Sie nicht verschlüsselten Datenverkehr zu Ihrem Speicherkonto zulassen. Andernfalls sollte die Entscheidung, ob ein VNET verwendet werden soll oder nicht, durch Ihre Geschäftsanforderungen und organisationsweite Richtlinie bestimmt werden.

Der Hauptgrund für die Zulassung von nicht verschlüsseltem Datenverkehr zu Ihrer Azure-Dateifreigabe ist die Unterstützung von Windows Server 2008 R2, Windows 7 oder einem anderen älteren Betriebssystem, das über SMB 2.1 (oder SMB 3.0 ohne Verschlüsselung bei einigen Linux-Distributionen) auf Ihre Azure-Dateifreigabe zugreifen muss. Die Verwendung von SMB 2.1 oder SMB 3.0 ohne Verschlüsselung in Betriebssystemen, die SMB 3.0 und höher mit Verschlüsselung unterstützen, wird nicht empfohlen.

## <a name="see-also"></a>Weitere Informationen
- [Azure Files: Übersicht](storage-files-introduction.md)
- [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)