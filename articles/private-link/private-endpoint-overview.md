---
title: Was ist privater Endpunkt in Azure?
description: Informationen zum privaten Endpunkt in Azure
services: private-link
author: KumudD
ms.service: private-link
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: 1fff9c076349d98d7a72c4bf69edb0a2795ac88f
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71937373"
---
# <a name="what-is-azure-private-endpoint"></a>Was ist privater Endpunkt in Azure?

Ein privater Endpunkt in Azure ist eine Netzwerkschnittstelle, die Sie privat und sicher mit einem von Azure Private Link betriebenen Dienst verbindet. Der private Endpunkt verwendet eine private IP-Adresse in Ihrem VNet und bindet den Dienst effektiv in Ihr VNet ein. Dabei kann es sich um einen Azure-Dienst wie z. B. Azure Storage, Azure SQL-Datenbank usw. oder Ihren eigenen [Private Link-Dienst](private-link-service-overview.md) handeln.
  
## <a name="private-endpoint-properties"></a>Eigenschaften eines privaten Endpunkts 
 Für einen privaten Endpunkt werden die folgenden Eigenschaften angegeben: 


|Eigenschaft  |Description |
|---------|---------|
|NAME    |    Ein eindeutiger Name innerhalb der Ressourcengruppe.      |
|Subnet    |  Das Subnetz, dem private IP-Adressen aus einem virtuellen Netzwerk bereitgestellt und zugeordnet werden. Anforderungen an das Subnetz finden Sie im Abschnitt „Einschränkungen“ in diesem Artikel.         |
|Private Link-Ressource    |   Die Private Link-Ressource, mit der anhand der Liste verfügbarer Typen eine Verbindung über die Ressourcen-ID oder den Alias hergestellt werden soll. Für den gesamten Datenverkehr an diese Ressource wird ein eindeutiger Netzwerkbezeichner generiert.       |
|Unterressource des Ziels   |      Die Unterressource, mit der eine Verbindung hergestellt wird. Jeder Private Link-Ressourcentyp verfügt über verschiedene Optionen, die je nach Präferenz ausgewählt werden können.    |
|Methode zur Genehmigung der Verbindung    |  Automatisch oder manuell. Je nach Berechtigungen gemäß rollenbasierter Zugriffssteuerung (RBAC) kann Ihr privater Endpunkt automatisch genehmigt werden. Wenn Sie versuchen, sich mit einer Private Link-Ressource ohne RBAC zu verbinden, verwenden Sie die manuelle Methode, um dem Besitzer der Ressource zu ermöglichen, die Verbindung zu genehmigen.        |
|Anforderungsnachricht     |  Sie können eine Nachricht für angeforderte Verbindungen angeben, die manuell genehmigt werden sollen. Mithilfe dieser Nachricht kann eine bestimmte Anforderung identifiziert werden.        |
|Verbindungsstatus   |   Eine schreibgeschützte Eigenschaft, die angibt, ob der private Endpunkt aktiv ist. Nur private Endpunkte in genehmigtem Zustand können zum Senden von Datenverkehr verwendet werden. Weitere verfügbare Zustände: <br>-**Genehmigt**: Die Verbindung wurde automatisch oder manuell genehmigt und ist zur Verwendung bereit.</br><br>-**Ausstehend**: Die Verbindung wurde manuell erstellt, wobei die Genehmigung des Besitzers der Private Link-Ressource aussteht.</br><br>-**Abgelehnt**: Die Verbindung wurde vom Besitzer der Private Link-Ressource abgelehnt.</br><br>-**Getrennt**: Die Verbindung wurde vom Besitzer der Private Link-Ressource entfernt. Der private Endpunkt dient nur noch Informationszwecken und sollte zur Bereinigung gelöscht werden. </br>|

Im Folgenden finden Sie einige wichtige Details zu privaten Endpunkten: 
- Ein privater Endpunkt ermöglicht die Verbindung zwischen den Consumern im selben VNet, in regionalen VNets mit Peering, globalen VNets mit Peering und lokalen Standorten mit [VPN](https://azure.microsoft.com/services/vpn-gateway/) oder [Express Route](https://azure.microsoft.com/services/expressroute/) und Diensten auf Basis von Private Link.
 
- Beim Erstellen eines privaten Endpunkts wird für die Lebensdauer der Ressource auch eine Netzwerkschnittstelle eingerichtet. Der Schnittstelle wird eine private IP-Adresse aus dem Subnetz zugewiesen, das dem Private Link-Dienst zugeordnet ist.
 
- Der private Endpunkt muss in derselben Region wie das virtuelle Netzwerk bereitgestellt werden. 
 
- Die Private Link-Ressource kann in einer anderen Region als das virtuelle Netzwerk und der private Endpunkt bereitgestellt werden.
 
- Mehrere private Endpunkte können mithilfe derselben Private Link-Ressource erstellt werden. Für ein einzelnes Netzwerk mit einer herkömmlichen DNS-Serverkonfiguration wird empfohlen, einen einzigen privaten Endpunkt für eine bestimmte Private Link-Ressource zu verwenden, um doppelte Einträge oder Konflikte bei der DNS-Auflösung zu vermeiden. 
 
- Mehrere private Endpunkte können im gleichen oder in verschiedenen Subnetzen innerhalb desselben virtuellen Netzwerks erstellt werden. Die Anzahl der privaten Endpunkte, die Sie in einem Abonnement anlegen können, ist begrenzt. Ausführliche Informationen finden Sie im Artikel zu  [Azure-Grenzwerten](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits).


 
## <a name="private-link-resource"></a>Private Link-Ressource 
Eine Private Link-Ressource ist das Ziel eines bestimmten privaten Endpunkts. Es folgt eine Liste der verfügbaren Private Link-Ressourcentypen: 
 
|Name der Private Link-Ressource  |Ressourcentyp   |Unterressourcen  |
|---------|---------|---------|
|**Private Link-Dienst** (Ihr eigener Dienst)   |  Microsoft.Network/privateLinkServices       | empty |
|**Azure SQL-Datenbank** | Microsoft.Sql/servers    |  SQL Server (sqlServer)        |
|**Azure SQL Data Warehouse** | Microsoft.Sql/servers    |  SQL Server (sqlServer)        |
|**Azure Storage (in englischer Sprache)**  | Microsoft.Storage/storageAccounts    |  Blob (blob, blob_secondary)<BR> Tabelle (table, table_secondary)<BR> Warteschlange (queue, queue_secondary)<BR> Datei (file, file_secondary)<BR> Web (web, web_secondary)        |
|**Azure Data Lake Storage Gen2**  | Microsoft.Storage/storageAccounts    |  Blob (blob, blob_secondary)       |
 
 
## <a name="network-security-of-private-endpoints"></a>Netzwerksicherheit privater Endpunkte 
Bei Verwenden privater Endpunkte für Azure-Dienste wird der Datenverkehr zu einer bestimmten Private Link-Ressource abgesichert. Die Plattform führt eine Zugriffssteuerung durch, um zu bestätigen, dass Netzwerkverbindungen nur die angegebene Private Link-Ressource erreichen. Um auf zusätzliche Ressourcen innerhalb desselben Azure-Diensts zuzugreifen, sind zusätzliche private Endpunkte erforderlich. 
 
Sie können Ihre Workloads vollständig vom Zugriff auf öffentliche Endpunkte ausschließen, um sich mit einem unterstützten Azure-Dienst zu verbinden. Dieses Steuerungsinstrument bietet Ihren Ressourcen eine zusätzliche Netzwerksicherheitsebene, indem es einen integrierten Exfiltrationsschutz bereitstellt, der den Zugriff auf andere Ressourcen verhindert, die im gleichen Azure-Dienst gehostet werden. 
 
## <a name="access-to-a-private-link-resource-using-approval-workflow"></a>Zugriff auf eine Private Link-Ressource mithilfe des Genehmigungsworkflows 
Sie können sich mit einer Private Link-Ressource mit den folgenden Methoden zur Genehmigung von Verbindungen verbinden:
- **Automatische** Genehmigung, wenn Sie die spezifische private Link-Ressource besitzen oder über eine Berechtigung verfügen. Die erforderliche Berechtigung basiert auf dem Private Link-Ressourcentyp im folgenden Format: Microsoft.\<Provider>/<Ressourcentyp>/privateEndpointConnectionApproval/action
- **Manuelle** Anforderung, wenn Sie nicht über die erforderliche Berechtigung verfügen und den Zugriff anfordern möchten. Ein Genehmigungsworkflow wird ausgelöst. Der private Endpunkt und die anschließende Verbindung mit dem privaten Endpunkt werden im Zustand „Ausstehend“ erstellt. Der Besitzer der Private Link-Ressource ist für die Genehmigung der Verbindung verantwortlich. Nach der Genehmigung ist der private Endpunkt in der Lage, Datenverkehr normal zu senden (siehe das folgende Diagramm des Genehmigungsworkflows).  

![Genehmigungsworkflow](media/private-endpoint-overview/private-link-paas-workflow.png)
 
Der Besitzer der Private Link-Ressource kann über eine private Endpunktverbindung die folgenden Aktionen ausführen: 
- Überprüfen aller Details von Verbindungen mit privaten Endpunkten. 
- Genehmigen einer Verbindung mit einem privaten Endpunkt. Der entsprechende private Endpunkt wird für das Senden von Datenverkehr an die Private Link-Ressource aktiviert. 
- Ablehnen einer Verbindung mit einem privaten Endpunkt. Der entsprechende private Endpunkt wird aktualisiert, um den Status widerzuspiegeln.
- Löschen einer Verbindung mit einem privaten Endpunkt in beliebigem Zustand. Der entsprechende private Endpunkt wird mit getrenntem Zustand aktualisiert, um die Aktion widerzuspiegeln. Nur der Besitzer des privaten Endpunkts kann die Ressource an dieser Stelle löschen. 
 
> [!NOTE]
> Nur ein privater Endpunkt im genehmigten Zustand kann Datenverkehr an eine angegebene Private Link-Ressource senden. 

### <a name="connecting-using-alias"></a>Verbindungsherstellung per Alias
Der Alias ist ein eindeutiger Moniker, der generiert wird, wenn der Dienstbesitzer den Private Link-Dienst hinter einem Standardlastenausgleich erstellt. Der Dienstbesitzer kann diesen Alias mit seinen Consumern offline gemeinsam nutzen. Verbraucher können eine Verbindung mit dem Private Link-Dienst anfordern, indem sie entweder den Ressourcen-URI oder Alias verwenden. Wenn Sie eine Verbindung per Alias herstellen möchten, müssen Sie einen privaten Endpunkt mithilfe der manuellen Verbindungsgenehmigungsmethode erstellen. Um die manuelle Verbindungsgenehmigungsmethode zu verwenden, legen Sie den Parameter für die manuelle Anforderung während des Flows zum Erstellen eines privaten Endpunkts auf TRUE fest. Ausführliche Informationen finden Sie unter [New-AzPrivateEndpoint](https://docs.microsoft.com/en-us/powershell/module/az.network/new-azprivateendpoint?view=azps-2.6.0) und [az network private-endpoint create](https://docs.microsoft.com/en-us/cli/azure/network/private-endpoint?view=azure-cli-latest#az-network-private-endpoint-create). 

## <a name="dns-configuration"></a>DNS-Konfiguration 
Wenn Sie eine Verbindung mit einer Private Link-Ressource über einen voll qualifizierten Domänennamen (FQDN) als Teil der Verbindungszeichenfolge herstellen, ist es wichtig, Ihre DNS-Einstellungen so zu konfigurieren, dass sie in die zugewiesene private IP-Adresse aufgelöst werden. Bestehende Azure-Dienste verfügen möglicherweise bereits über eine DNS-Konfiguration, die beim Herstellen einer Verbindung über einen öffentlichen Endpunkt verwendet werden kann. Diese muss überschrieben werden, um eine Verbindung mithilfe Ihres privaten Endpunkts herzustellen. 
 
Die Netzwerkschnittstelle, die dem privaten Endpunkt zugeordnet ist, enthält alle Informationen, die zur Konfiguration Ihres DNS erforderlich sind, einschließlich FQDN und privater IP-Adressen, die einer bestimmten Private Link-Ressource zugewiesen wurden. 
 
Mit den folgenden Optionen können Sie Ihre DNS-Einstellungen für private Endpunkte konfigurieren: 
- **Hostdatei (wird nur für Tests empfohlen)** . Sie können die Hostdatei auf einem virtuellen Computer verwenden, um das DNS zu überschreiben.  
- **Private DNS-Zone**. Sie können private DNS-Zonen verwenden, um die DNS-Auflösung für einen bestimmten privaten Endpunkt zu überschreiben. Eine private DNS-Zone kann mit Ihrem virtuellen Netzwerk verknüpft werden, um bestimmte Domänen aufzulösen.
- **Benutzerdefinierter DNS-Server**. Sie können Ihren eigenen DNS-Server verwenden, um die DNS-Auflösung für eine angegebene Private Link-Ressource zu überschreiben. Wenn Ihr DNS-Server in einem virtuellen Netzwerk gehostet wird, können Sie zum Verwenden einer privaten DNS-Zone eine DNS-Weiterleitungsregel erstellen, um die Konfiguration für alle Private Link-Ressourcen zu vereinfachen.
 
> [!IMPORTANT]
> Es wird nicht empfohlen, eine Zone zu überschreiben, die aktiv zur Auflösung öffentlicher Endpunkte genutzt wird. Verbindungen mit Ressourcen können ohne DNS-Weiterleitung an das öffentliche DNS nicht ordnungsgemäß aufgelöst werden. Um Probleme zu vermeiden, erstellen Sie einen anderen Domänennamen, oder halten Sie sich an den vorgeschlagenen Namen für jeden der folgenden Dienste. 
 
Verwenden Sie für Azure-Dienste die empfohlenen Zonennamen in der folgenden Tabelle:

|Typ der Private Link-Ressource   |Unterressource  |Zonenname  |
|---------|---------|---------|
|SQL DB/DW (Microsoft.Sql/servers)    |  SQL Server (sqlServer)        |   privatelink.database.windows.net       |
|Speicherkonto (Microsoft.Storage/storageAccounts)    |  Blob (blob, blob_secondary)        |    privatelink.blob.core.windows.net      |
|Speicherkonto (Microsoft.Storage/storageAccounts)    |    Tabelle (table, table_secondary)      |   privatelink.table.core.windows.net       |
|Speicherkonto (Microsoft.Storage/storageAccounts)    |    Warteschlange (queue, queue_secondary)     |   privatelink.queue.core.windows.net       |
|Speicherkonto (Microsoft.Storage/storageAccounts)   |    Datei (file, file_secondary)      |    privatelink.file.core.windows.net      |
|Speicherkonto (Microsoft.Storage/storageAccounts)     |  Web (web, web_secondary)        |    privatelink.web.core.windows.net      |
|Data Lake-Dateisystem Gen2 (Microsoft.Storage/storageAccounts)  |  Data Lake-Dateisystem Gen2 (dfs, dfs_secondary)        |     privatelink.dfs.core.windows.net     |
||||
 

Azure erstellt einen DNS-Eintrag des Typs CNAME (kanonischer Name) im öffentlichen DNS zum Umleiten der Auflösung an die vorgeschlagenen Domänennamen. Sie können die Auflösung mit der privaten IP-Adresse Ihrer privaten Endpunkte überschreiben. 
 
Ihre Anwendungen müssen die Verbindungs-URL nicht ändern. Beim Versuch der Auflösung mithilfe eines öffentlichen DNS wird der DNS-Server nun in Ihre privaten Endpunkten aufgelöst. Der Prozess wirkt sich nicht auf Ihre Anwendungen aus. 
 
## <a name="limitations"></a>Einschränkungen
 
Die folgende Tabelle enthält eine Liste der bekannten Einschränkungen bei Verwendung privater Endpunkte: 


|Einschränkung |BESCHREIBUNG |Lösung  |
|---------|---------|---------|
|Regeln für Netzwerksicherheitsgruppen (NSGs) gelten nicht für den privaten Endpunkt    |NSGs werden für private Endpunkte nicht unterstützt. Während Subnetzen, die den privaten Endpunkt enthalten, eine NSG zugeordnet sein kann, gelten die Regeln nicht für den vom privaten Endpunkt verarbeiteten Datenverkehr. Sie müssen die [Durchsetzung von Netzwerkrichtlinien deaktivieren](disable-private-endpoint-network-policy.md), um private Endpunkte in einem Subnetz bereitzustellen zu können. Die NSG wird weiterhin für andere Workloads erzwungen, die im selben Subnetz gehostet werden.   | Steuern Sie den Datenverkehr, indem Sie auf Quellclients NSG-Regeln für ausgehenden Datenverkehr verwenden.        |
|Private Endpunkte können nicht in Subnetzen erstellt werden, die für Dienstendpunkte oder spezialisierte Workloads aktiviert sind    |Private Endpunkte können nicht in Subnetzen erstellt werden, die für Dienstendpunkte oder Subnetze aktiviert sind, die an spezialisierte Workloads delegiert wurden|  Erstellen Sie ein separates Subnetz, um die privaten Endpunkte bereitzustellen.        |
|Der private Endpunkt kann nur dem Private Link-Dienst (im Besitz des Kunden) in derselben Region zugeordnet werden    |   Das Herstellen einer Verbindung mit einem Private Link-Dienst (Ihrem eigenen) aus einer anderen Region wird nicht unterstützt       |  In der Vorschauphase müssen Sie Ihren Private Link-Dienst in derselben Region bereitstellen.        |
|  Virtuelle Netzwerke mit Peering mit nur privaten Endpunkten werden nicht unterstützt.   |   Das Herstellen einer Verbindung mit privaten Endpunkten in einem virtuellen Netzwerk mit Peering ohne andere Workload wird nicht unterstützt.       | Stellen Sie einen einzelnen virtuellen Computer im virtuellen Netzwerk mit Peering bereit, um die Konnektivität zu ermöglichen. |
|Spezialisierte Workloads können nicht auf private Endpunkte zugreifen    |   Die folgenden Dienste, die in Ihrem virtuellen Netzwerk bereitgestellt werden, können nicht über private Endpunkte auf Private Link-Ressourcen zugreifen:<br>App Service-Plan</br>Azure Container Instances</br>Azure NetApp Files</br>Dediziertes HSM von Azure<br>       |   Keine Risikominderung in der Vorschauphase.       |


## <a name="next-steps"></a>Nächste Schritte
- [Erstellen eines privaten Endpunkts mit dem Azure-Portal](create-private-endpoint-portal.md)
- [Erstellen eines privaten Endpunkts über PowerShell](create-private-endpoint-powershell.md)
- [Erstellen eines privaten Endpunkts über die Azure CLI](create-private-endpoint-cli.md)
- [Erstellen eines privaten Endpunkts für das Speicherkonto im Portal](create-private-endpoint-storage-portal.md)
- [Erstellen eines eigenen Private Link-Diensts mit Azure PowerShell](create-private-link-service-powershell.md)
