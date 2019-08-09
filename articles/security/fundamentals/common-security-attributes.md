---
title: Sicherheitsattribute für Azure-Dienste
description: Eine Prüfliste der Sicherheitsattribute für die Auswertung von Azure-Diensten
services: security
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: security
ms.subservice: security-fundamentals
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: mbaldwin
ms.openlocfilehash: 43eb7e5c4cab722eb97f9e2fe819c9c79bae45d9
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828769"
---
# <a name="security-attributes-for-azure-services"></a>Sicherheitsattribute für Azure-Dienste

In diesem Artikel werden die Sicherheitsattribute für ausgewählte Azure-Dienste beschrieben. Ein Sicherheitsattribut ist eine Eigenschaft oder ein Feature eines Azure-Diensts. Es ermöglicht es dem Dienst, Sicherheitsrisiken zu verhindern, zu erkennen oder darauf zu reagieren.

Sicherheitsattribute fallen in folgende Kategorien:
* Prävention
* Netzwerksegmentierung
* Erkennung
* Unterstützung von Identitäts- und Zugriffsverwaltung
* Überwachungspfad
* Zugriffssteuerungen (falls verwendet)
* Konfigurationsverwaltung (falls verwendet)

In jeder Kategorie wird mit „Ja“ oder „Nein“ angegeben, ob ein Attribut verwendet wird. Für einige Dienste gibt „–“ an, dass das Attribut nicht anwendbar ist. Außerdem kann ein Hinweis oder ein Link zu weiteren Informationen über ein Attribut angegeben sein.

## <a name="api-managementazureapi-managementapi-management-security-attributes"></a>[API Management](/azure/api-management/api-management-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | Vertrauliche Daten, z. B. Zertifikate, Schlüssel und als geheim gekennzeichnete Werte, werden mit vom Dienst verwalteten Schlüsseln für die jeweilige Dienstinstanz verschlüsselt. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Ja | [ExpressRoute](/azure/expressroute/index)- und VNET-Verschlüsselung erfolgen über [Azure-Netzwerke](/azure/virtual-network/index). |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Nein | Alle Verschlüsselungsschlüssel gelten pro Dienstinstanz und werden vom Dienst verwaltet. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Aufrufe auf Verwaltungsebene erfolgen über [Azure Resource Manager](/azure/azure-resource-manager/index) und TLS. Ein gültiges JSON Web Token (JWT) ist erforderlich.  Aufrufe auf Datenebene können mit TLS und einem der unterstützten Authentifizierungsmechanismen (z. B. Clientzertifikat oder JWT) geschützt werden.
 |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Nein | |
| Unterstützung der VNET-Einschleusung| Ja | |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Mithilfe von Netzwerksicherheitsgruppen (NSGs) bzw. Azure Application Gateway (oder anderen Softwareappliances) |
| Unterstützung der Tunnelerzwingung| Ja | Azure-Netzwerke unterstützen die Tunnelerzwingung. |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | |
| Authorization| Ja | |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | [Azure Monitor-Aktivitätsprotokolle](/azure/azure-monitor/platform/activity-logs-overview) |
| Protokollierung und Überwachung auf Datenebene| Ja | [Azure Monitor-Diagnoseprotokolle](/azure/azure-monitor/platform/diagnostic-logs-overview) und (optional) [Azure Application Insights](/azure/azure-monitor/app/app-insights-overview)|

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Verwendung des [Azure API Management DevOps Resource Kit](https://aka.ms/apimdevops) |

### <a name="vulnerability-scans-false-positives"></a>Überprüfungen auf Sicherheitsrisiken: Falsch positive Ergebnisse

In diesem Abschnitt sind häufige Sicherheitsrisiken dokumentiert, die sich nicht auf Azure API Management auswirken.

| Sicherheitsrisiko               | BESCHREIBUNG                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed ist ein Sicherheitsrisiko bei der Implementierung der TLS SessionTicket-Erweiterung in einigen F5-Produkten. Hierbei wird das Abschöpfen („Bleeding“) von bis zu 31 Byte an Daten aus nicht initialisiertem Arbeitsspeicher ermöglicht. Die Ursache ist, dass der TLS-Stapel eine vom Client übergebene Sitzungs-ID mit Daten auffüllt, um eine Länge von 32 Bit zu erzielen. |


## <a name="app-serviceazureapp-serviceapp-service-security-attributes"></a>[App Service](/azure/app-service/app-service-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | Ja | Die Inhalte von Websitedateien werden in Azure Storage gespeichert, das die ruhenden Inhalte automatisch verschlüsselt. Weitere Informationen finden Sie unter [Azure Storage-Verschlüsselung für ruhende Daten](/azure/storage/common/storage-service-encryption).<br><br>Vom Kunden bereitgestellte Geheimnisse werden im Ruhezustand verschlüsselt. Die Geheimnisse werden im Ruhezustand verschlüsselt, während sie in App Service-Konfigurationsdatenbanken gespeichert sind.<br><br>Lokal angefügte Datenträger können von Websites als temporärer Speicher verwendet werden („D:\local“ und %TMP%). Lokal angefügte Datenträger werden im Ruhezustand nicht verschlüsselt. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Ja | Kunden können Websites so konfigurieren, dass für sie HTTPS für eingehenden Datenverkehr gefordert und verwendet wird. Weitere Informationen finden Sie im Blogbeitrag [How to make an Azure App Service HTTPS only](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/) (Erzwingen von HTTPS für Azure App Service). |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Ja | Kunden können wählen, dass Anwendungsgeheimnisse in Key Vault gespeichert werden, und können diese zur Laufzeit abrufen. Weitere Informationen finden Sie unter [Verwenden von Key Vault-Verweisen in App Service und Azure Functions (Vorschauversion)](/azure/app-service/app-service-key-vault-references).|
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Verwaltungsaufrufe zum Konfigurieren von App Service werden mit [Azure Resource Manager](/azure/azure-resource-manager/index)-Aufrufen über HTTPS gesendet. |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | Derzeit in der Vorschauversion für App Service verfügbar. Weitere Informationen finden Sie unter [Azure App Service – Zugriffseinschränkungen](/azure/app-service/app-service-ip-restrictions). |
| Unterstützung der VNet-Einschleusung| Ja | App Service-Umgebungen sind private Implementierungen von App Service, die für einen einzelnen Kunden dediziert sind, der in das virtuelle Netzwerk eines Kunden eingefügt wurde. Weitere Informationen finden Sie unter [Einführung in die App Service-Umgebungen](/azure/app-service/environment/intro). |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Für die öffentliche mehrinstanzenfähige Variante von App Service können Kunden Netzwerk-Zugriffssteuerungslisten (IP-Einschränkungen) konfigurieren, um zulässigen eingehenden Datenverkehr zu sperren.  Weitere Informationen finden Sie unter [Azure App Service – Zugriffseinschränkungen](/azure/app-service/app-service-ip-restrictions).  App Service-Umgebungen werden direkt in virtuellen Netzwerken bereitgestellt und können daher mit Netzwerksicherheitsgruppen geschützt werden. |
| Unterstützung der Tunnelerzwingung| Ja | App Service-Umgebungen können im virtuellen Netzwerk eines Kunden bereitgestellt werden, in dem Tunnelerzwingung konfiguriert ist. Kunden müssen die Anweisungen unter [Konfigurieren Ihrer App Service-Umgebung mit erzwungenem Tunneling](/azure/app-service/environment/forced-tunnel-support) befolgen. |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | App Service ist in Application Insights eingebunden für Sprachen, die Application Insights (Vollversion von .NET Framework, .NET Core, Java und Node.JS) unterstützen.  Weitere Informationen finden Sie unter [Überwachen der Leistung von Azure App Service](/azure/azure-monitor/app/azure-web-apps). App Service sendet außerdem Anwendungsmetriken an Azure Monitor. Weitere Informationen finden Sie unter [Überwachen von Apps in Azure App Service](/azure/app-service/web-sites-monitor). |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Kunden können Anwendungen in App Service erstellen, die automatisch in [Azure Active Directory (Azure AD)](/azure/active-directory/index) sowie in andere OAuth-kompatible Identitätsanbieter eingebunden werden. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in Azure App Service](/azure/app-service/overview-authentication-authorization). Für Verwaltungszugriff auf App Service-Ressourcen werden sämtliche Zugriffe mit einer Kombination aus Azure AD-authentifizierten Prinzipal- und Azure Resource Manager-RBAC-Rollen gesteuert. |
| Authorization| Ja | Für Verwaltungszugriff auf App Service-Ressourcen werden sämtliche Zugriffe mit einer Kombination aus Azure AD-authentifizierten Prinzipal- und Azure Resource Manager-RBAC-Rollen gesteuert.  |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Alle Verwaltungsvorgänge, die für App Service-Objekte ausgeführt werden, erfolgen über [Azure Resource Manager](/azure/azure-resource-manager/index). Verlaufsprotokolle für diese Vorgänge sind sowohl im Portal als auch über die CLI verfügbar. Weitere Informationen finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](/azure/role-based-access-control/resource-provider-operations#microsoftweb) und [az monitor activity-log](/cli/azure/monitor/activity-log). |
| Protokollierung und Überwachung auf Datenebene | Nein | Die Datenebene für App Service ist eine Remotedateifreigabe, die die bereitgestellten Websiteinhalte eines Kunden enthält.  Es gibt keine Überwachung der Remotedateifreigabe. |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Für Verwaltungsvorgänge kann der Status einer App Service-Konfiguration als eine Azure Resource Manager-Vorlage exportiert und im Lauf der Zeit versioniert werden. Für Runtimevorgänge können Kunden App Service-Bereitstellungsslots verwenden, um mehrere verschiedene aktive Versionen einer Anwendung zu verwalten. | 



## <a name="azure-resource-managerazureazure-resource-managerazure-resource-manager-security-attributes"></a>[Azure Resource Manager](/azure/azure-resource-manager/azure-resource-manager-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | Ja |  |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Ja | HTTPS/TLS. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| – | Azure Resource Manager speichert keine Kundeninhalte, sondern nur Steuerdaten. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| Ja | |
| Verschlüsselte API-Aufrufe| Ja | |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Nein | |
| Unterstützung der VNET-Einschleusung| Ja | |
| Unterstützung von Netzwerkisolation und Firewall| Nein |  |
| Unterstützung der Tunnelerzwingung| Nein |  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Nein | |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Basierend auf [Azure Active Directory](/azure/active-directory).|
| Authorization| Ja | |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Aktivitätsprotokolle machen alle Schreibvorgänge (PUT, POST, DELETE) sichtbar, die auf Ihre Ressourcen angewendet werden. Siehe [Anzeigen von Aktivitätsprotokollen zu Überwachungsaktionen für Ressourcen](/azure/azure-resource-manager/resource-group-audit). |
| Protokollierung und Überwachung auf Datenebene| – | |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja |  |


## <a name="azure-backupazurebackupbackup-security-attributes"></a>[Azure Backup](/azure/backup/backup-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen)| Ja | Verwenden der Speicherdienstverschlüsselung für Speicherkonten |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Nein | Mithilfe von HTTPS |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Nein |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| Nein |  |
| Verschlüsselte API-Aufrufe| Ja |  |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Nein |  |
| Unterstützung der VNet-Einschleusung| Nein |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Die Tunnelerzwingung wird für VM-Sicherungen unterstützt. Die Tunnelerzwingung wird für Workloads in virtuellen Computern nicht unterstützt. |
| Unterstützung der Tunnelerzwingung| Nein |  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Log Analytics wird über Diagnoseprotokolle unterstützt. Weitere Informationen finden Sie unter [Monitor Azure Backup protected workloads using Log Analytics (Überwachen von Azure Backup-geschützten Workloads mit Log Analytics)](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/). |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |
| Authorization| Ja | Vom Kunden erstellte und integrierte RBAC-Rollen werden verwendet. Weitere Informationen finden Sie unter [Use Role-Based Access Control to manage Azure Backup recovery points (Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Backup-Wiederherstellungspunkten)](/azure/backup/backup-rbac-rs-vault). |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Alle vom Kunden über das Azure-Portal ausgelösten Aktionen werden in Aktivitätsprotokollen protokolliert. |
| Protokollierung und Überwachung auf Datenebene| Nein | Die Azure Backup-Datenebene kann nicht direkt aufgerufen werden.  |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja|  |

## <a name="cosmos-dbazurecosmos-dbcosmos-db-security-attributes"></a>[Cosmos DB](/azure/cosmos-db/cosmos-db-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | Ja | Alle Cosmos DB-Datenbanken und -Sicherungen werden standardmäßig verschlüsselt. Weitere Informationen finden Sie unter [Datenverschlüsselung in Azure Cosmos DB](/azure/cosmos-db/database-encryption-at-rest). Die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln wird nicht unterstützt. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| Ja | Alle Azure Cosmos DB-Daten werden während der Übertragung verschlüsselt. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Nein |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| Ja | Nur in der Tabellen-API (Premium). Diese Funktion wird nicht von allen APIs unterstützt. Siehe [Einführung in Azure Cosmos DB: Tabellen-API](/azure/cosmos-db/table-introduction). |
| Verschlüsselte API-Aufrufe| Ja | Alle Verbindungen mit Azure Cosmos DB unterstützen HTTPS. Azure Cosmos DB unterstützt auch TLS 1.2-Verbindungen, dies ist jedoch noch keine zwingende Voraussetzung. Wenn Kunden Low-Level-TLS auf ihrer Seite deaktivieren, können sie sicherstellen, dass Verbindungen mit Cosmos DB hergestellt werden.  |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja |  |
| Unterstützung der VNet-Einschleusung| Ja | Bei einem VNET-Dienstendpunkt können Sie ein Azure Cosmos DB-Konto so konfigurieren, dass der Zugriff nur aus einem bestimmten Subnetz eines virtuellen Netzwerks (VNET) zugelassen wird. Sie können den VNET-Zugriff auch mit Firewallregeln kombinieren.  Siehe [Zugreifen auf Azure Cosmos DB über virtuelle Netzwerke](/azure/cosmos-db/vnet-service-endpoint). |
| Netzwerkisolation und Firewallunterstützung| Ja | Durch die Firewallunterstützung können Sie Ihr Azure Cosmos-Konto so konfigurieren, dass der Zugriff nur über eine genehmigte Gruppe von IP-Adressen, einen IP-Adressbereich und/oder Clouddienste zugelassen wird. Siehe [Konfigurieren der IP-Firewall in Azure Cosmos DB](/azure/cosmos-db/how-to-configure-firewall).|
| Unterstützung der Tunnelerzwingung| Ja | Kann auf Clientseite in dem VNET konfiguriert werden, in dem sich die virtuellen Computer befinden.   |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Alle an Azure Cosmos DB gesendeten Anforderungen werden protokolliert. Die [Azure-Überwachung](/azure/azure-monitor/overview), Azure-Metriken und die Azure-Überwachungsprotokollierung werden unterstützt.  Sie können Informationen protokollieren, die Datenebenenanforderungen, Abfragelaufzeitstatistiken, Abfragetext oder MongoDB-Anforderungen entsprechen. Sie können auch Benachrichtigungen einrichten. |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Ja, auf Ebene des Datenbankkontos und der Datenebene. Cosmos DB verwendet Ressourcentoken und Schlüsselzugriff. |
| Authorization| Ja | Für das Azure Cosmos-Konto mit (primärem und sekundärem) Hauptschlüssel und Ressourcentoken unterstützt. Sie können Hauptschlüssel verwenden, um Lese-/Schreibzugriff oder schreibgeschützten Zugriff auf Daten zu erhalten. Ressourcentoken ermöglichen den zeitlich begrenzten Zugriff auf Ressourcen wie Dokumente und Container. |

### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Azure-Aktivitätsprotokoll für Vorgänge auf Kontoebene, z. B. Firewalls, VNETs, Schlüsselzugriff und IAM. |
| Protokollierung und Überwachung auf Datenebene | Ja | Diagnoseüberwachungsprotokolle für Vorgänge auf Containerebene, z. B. das Erstellen von Containern, Bereitstellen von Durchsatz, Indizieren von Richtlinien sowie für CRUD-Vorgänge für Dokumente. |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Nein  | | 

### <a name="additional-security-attributes-for-cosmos-db"></a>Zusätzliche Sicherheitsattribute für Cosmos DB

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS) | Ja | Siehe [Konfigurieren der Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS)](/azure/cosmos-db/how-to-configure-cross-origin-resource-sharing). |


## <a name="event-hubsazureevent-hubsevent-hubs-security-attributes"></a>[Event Hubs](/azure/event-hubs/event-hubs-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) |  Ja | |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Ja | |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Nein |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja |  |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja |  |
| Unterstützung der VNet-Einschleusung| Nein | |
| Unterstützung von Netzwerkisolation und Firewall| Ja |  |
| Unterstützung der Tunnelerzwingung| Nein |  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | |
| Authorization|  Ja | |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja |  |
| Protokollierung und Überwachung auf Datenebene| Ja |   |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | |


## <a name="expressrouteazureexpressrouteexpressroute-security-attributes"></a>[ExpressRoute](/azure/expressroute/expressroute-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) |  – | ExpressRoute speichert keine Kundendaten. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Nein | |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| – |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Über [Azure Resource Manager](/azure/azure-resource-manager/index) und HTTPS. |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| – |  |
| Unterstützung der VNet-Einschleusung| – | |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Jeder Kunde ist in seiner eigenen Routingdomäne enthalten und wird zu seinem eigenen VNET getunnelt |
| Unterstützung der Tunnelerzwingung| – | Über Border Gateway Protocol (BGP). |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Siehe [ExpressRoute-Überwachung, Metriken und Warnungen](/azure/expressroute/expressroute-monitoring-metrics-alerts).|

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Dienstkonto für Gateway für Microsoft (GWM), Controller; JIT-Zugriff (Just-in-Time) für Entwicklung und Betrieb. |
| Authorization|  Ja |Dienstkonto für Gateway für Microsoft (GWM), Controller; JIT-Zugriff (Just-in-Time) für Entwicklung und Betrieb. |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen| 
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja |  |
| Protokollierung und Überwachung auf Datenebene| Nein |   |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Über Netzwerkressourcenanbieter (Network Resource Provider, NRP). |


## <a name="key-vaultkey-vaultkey-vault-security-attributesmd"></a>[Schlüsseltresor](../../key-vault/key-vault-security-attributes.md)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | Ja | Alle Objekte werden verschlüsselt. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Ja | Die gesamte Kommunikation erfolgt über verschlüsselte API-Aufrufe. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Ja | Der Kunde überwacht alle Schlüssel im Schlüsseltresor. Wenn vom Hardwaresicherheitsmodul (HSM) unterstützte Schlüssel angegeben werden, bietet ein FIPS Level 2 HSM Schutz für Schlüssel, Zertifikate oder Geheimnisse. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – |  |
| Verschlüsselte API-Aufrufe| Ja | Mithilfe von HTTPS |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | Mithilfe von Dienstendpunkten im virtuellen Netzwerk (VNET) |
| Unterstützung der VNet-Einschleusung| Nein |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Mithilfe von VNET-Firewallregeln |
| Unterstützung der Tunnelerzwingung| Nein |  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Mithilfe von Log Analytics |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |
| Authorization| Ja | Mithilfe der Schlüsseltresor-Zugriffsrichtlinie |

### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Protokollierung und Überwachung| Ja | Mithilfe von Log Analytics |
| Protokollierung und Überwachung auf Datenebene| Ja | Mithilfe von Log Analytics |

### <a name="access-controls"></a>Zugriffssteuerung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Steuerungs-/Verwaltungsebene – Zugriffskontrollen | Ja | Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) im Rahmen von Azure Resource Manager |
| Datenebene – Zugriffskontrollen (auf jeder Dienstebene) | Ja | Schlüsseltresor-Zugriffsrichtlinie |

## <a name="load-balancerload-balancerload-balancer-security-attributesmd"></a>[Load Balancer](../../load-balancer/load-balancer-security-attributes.md)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | – | |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| – | |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| – | |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Über [Azure Resource Manager](/azure/azure-resource-manager/index) |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| – | |
| Unterstützung der VNet-Einschleusung| – | . |
| Netzwerkisolation und Firewallunterstützung| – |  |
| Unterstützung der Tunnelerzwingung| – | |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Siehe [Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“](/azure/load-balancer/load-balancer-monitor-log) |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| – |  |
| Authorization| – |  |

### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Siehe [Azure Monitor-Protokolle für öffentlichen Load Balancer vom Typ „Basic“](/azure/load-balancer/load-balancer-monitor-log) |
| Protokollierung und Überwachung auf Datenebene | – |  |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| – |  | 

## <a name="service-bus-messagingazureservice-bus-messagingservice-bus-messaging-security-attributes"></a>[Service Bus Messaging](/azure/service-bus-messaging/service-bus-messaging-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) |  Ja, standardmäßig für serverseitige Verschlüsselung ruhender Daten. | Von Kunden verwaltete Schlüssel und BYOK werden noch nicht unterstützt. Clientseitige Verschlüsselung liegt in der Verantwortung des Clients. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Ja | Unterstützt HTTPS/TLS-Standardverfahren. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Nein |   |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | API-Aufrufe erfolgen über [Azure Resource Manager](/azure/azure-resource-manager/index) und HTTPS. |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja (nur Premium-Tarif) | VNET-Dienstendpunkte werden nur für den [Premium-Tarif von Service Bus](/azure/service-bus-messaging/service-bus-premium-messaging) unterstützt. |
| Unterstützung der VNet-Einschleusung| Nein | |
| Unterstützung von Netzwerkisolation und Firewall| Ja (nur Premium-Tarif) |  |
| Unterstützung der Tunnelerzwingung| Nein |  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Unterstützt über [Azure Monitor und -Warnungen](/azure/service-bus-messaging/service-bus-metrics-azure-monitor). |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Verwaltet durch [verwaltete Dienstidentität von Azure Active Directory](/azure/service-bus-messaging/service-bus-managed-service-identity); siehe [Service Bus-Authentifizierung und -Autorisierung](/azure/service-bus-messaging/service-bus-authentication-and-authorization).|
| Authorization| Ja | Unterstützt Autorisierung über [rollenbasierte Zugriffssteuerung (RBAC)](/azure/service-bus-messaging/service-bus-role-based-access-control) und SAS-Token; siehe [Service Bus-Authentifizierung und -Autorisierung](/azure/service-bus-messaging/service-bus-authentication-and-authorization). |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Vorgangsprotokolle sind verfügbar; siehe [Service Bus-Diagnoseprotokolle](/azure/service-bus-messaging/service-bus-diagnostic-logs).  |
| Protokollierung und Überwachung auf Datenebene| Nein |  |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Unterstützt Versionsverwaltung durch den Ressourcenanbieter über [Azure Resource Manager-API](/rest/api/resources/).|


## <a name="service-bus-relayazureservice-bus-relayservice-bus-relay-security-attributes"></a>[Service Bus Relay](/azure/service-bus-relay/service-bus-relay-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) |  – | Relay ist ein Websocket, das Daten nicht beibehält. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Ja | Der Dienst erfordert TLS. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Nein | Verwendet nur Microsoft-TLS-Zertifikate.  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | HTTPS. |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Nein |  |
| Unterstützung von Netzwerkisolation und Firewall| Nein |  |
| Unterstützung der Tunnelerzwingung| – | Relay ist der TLS-Tunnel  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Über SAS. |
| Authorization|  Ja | Über SAS. |

### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Über [Azure Resource Manager](/azure/azure-resource-manager/index). |
| Protokollierung und Überwachung auf Datenebene| Ja | Verbindungserfolg/-fehler und Fehler werden protokolliert.  |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Über [Azure Resource Manager](/azure/azure-resource-manager/index).|

## <a name="service-fabricservice-fabricservice-fabric-security-attributesmd"></a>[Service Fabric](../../service-fabric/service-fabric-security-attributes.md)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | Ja | Der Cluster und die VM-Skalierungsgruppe, auf der der Cluster basiert, befinden sich im Besitz des Kunden. Azure Disk Encryption kann für die VM-Skalierungsgruppe aktiviert werden. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Ja | Der Cluster und die VM-Skalierungsgruppe, auf der der Cluster basiert, befinden sich im Besitz des Kunden. Azure Disk Encryption kann für die VM-Skalierungsgruppe aktiviert werden. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – |  |
| Verschlüsselte API-Aufrufe| Ja | Service Fabric-API-Aufrufe erfolgen über Azure Resource Manager. Ein gültiges JSON Web Token (JWT) ist erforderlich. |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja |  |
| Unterstützung der VNet-Einschleusung| Ja |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja | Verwendung von Netzwerksicherheitsgruppen (NSG) |
| Unterstützung der Tunnelerzwingung| Ja | Azure-Netzwerke unterstützen die Tunnelerzwingung. |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Durch die Unterstützung der Azure-Überwachung und von Drittanbietern. |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |
| Authorization| Ja | Identitäts- und Zugriffsverwaltung (IAM) für Aufrufe über SFRP. Direkte Aufrufe von Clusterendpunkten unterstützen zwei Rollen: „Benutzer“ und „Administrator“. Der Kunde kann die APIs einer der beiden Rollen zuordnen. |

### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Alle Vorgänge auf Steuerungsebene durchlaufen Überwachungs- und Genehmigungsprozesse. |
| Protokollierung und Überwachung auf Datenebene| – | Der Cluster befindet sich im Besitz des Kunden.  |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | |

## <a name="sql-databasesql-databasesql-database-security-attributesmd"></a>[SQL-Datenbank](../../sql-database/sql-database-security-attributes.md)

SQL-Datenbank umfasst Konzepte für [Einzeldatenbanken](/azure/sql-database/sql-database-single-index) und für [verwaltete Instanzen](/azure/sql-database/sql-database-managed-instance). Die folgenden Einträge gelten für beide Angebote, sofern nichts anderes angegeben ist.

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | Ja | Wird als „Verschlüsselung während der Verwendung“ bezeichnet, wie im Artikel [Always Encrypted](/azure/sql-database/sql-database-always-encrypted) beschrieben. Dienstseitige Verschlüsselung verwendet die [transparente Datenverschlüsselung](/azure/sql-database/transparent-data-encryption-azure-sql).|
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Ja | Mithilfe von HTTPS |
| Behandeln von Verschlüsselungsschlüsseln, wie z. B. CMK oder BYOK| Ja | Es wird sowohl die Verarbeitung von vom Dienst verwalteten Schlüsseln als auch die Verarbeitung von vom Kunden verwalteten Schlüsseln angeboten. Letztere Option wird über [Azure Key Vault](/azure/key-vault/index) angeboten. |
| Verschlüsselung auf Spaltenebene, die von Azure Data Services bereitgestellt wird| Ja | Durch [Always Encrypted](/azure/sql-database/sql-database-always-encrypted). |
| Verschlüsselte API-Aufrufe| Ja | Mithilfe von HTTPS/SSL. |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | Gilt nur für eine [Einzeldatenbank](/azure/sql-database/sql-database-single-index). |
| Unterstützung für das Einfügen in Azure Virtual Network| Ja | Gilt nur für eine [verwaltete Instanz](/azure/sql-database/sql-database-managed-instance). |
| Netzwerkisolation und Firewallunterstützung| Ja | Firewall sowohl auf Datenbankebene als auch auf Serverebene. Netzwerkisolation wird ausschließlich für die [verwaltete Instanz](/azure/sql-database/sql-database-managed-instance) angeboten. |
| Unterstützung der Tunnelerzwingung| Ja | [Verwaltete Instanz](/azure/sql-database/sql-database-managed-instance) über [Azure ExpressRoute](/azure/expressroute/index)-VPN. |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung, z. B. Protokollanalyse oder Application Insights| Ja | SecureSphere, die SIEM-Lösung von Imperva, wird ebenfalls unterstützt, und zwar mittels [Azure Event Hubs](/azure/event-hubs/index)-Integration über [SQL-Überwachung](/azure/sql-database/sql-database-auditing). |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Azure Active Directory (Azure AD) |
| Authorization| Ja | Keine |

### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Ja, für einige Ereignisse |
| Protokollierung und Überwachung auf Datenebene | Ja | Über [SQL-Überwachung](/azure/sql-database/sql-database-auditing) |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung, z. B. Versionsverwaltung der Konfiguration| Nein  | Keine |

### <a name="additional-security-attributes-for-sql-database"></a>Zusätzliche Sicherheitsattribute für SQL-Datenbank

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Prävention: Sicherheitsrisikobewertung | Ja | Weitere Informationen finden Sie unter [Mit dem Dienst zur SQL-Sicherheitsrisikobewertung können Sie Datenbankschwachstellen erkennen](/azure/sql-database/sql-vulnerability-assessment). |
| Prävention: Datenermittlung und -klassifizierung  | Ja | Weitere Informationen finden Sie unter [Azure SQL-Datenbank und SQL Data Warehouse: Datenermittlung und -klassifizierung](/azure/sql-database/sql-database-data-discovery-and-classification). |
| Erkennung: Bedrohungserkennung | Ja | Weitere Informationen finden Sie unter [Advanced Threat Protection für Azure SQL-Datenbank](/azure/sql-database/sql-database-threat-detection-overview). |


## <a name="storageazurestoragecommonstorage-security-attributes"></a>[Speicher](/azure/storage/common/storage-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | Ja |  |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung) | Ja | Unterstützung für die Standardverfahren HTTPS/TLS.  Benutzer können Daten auch verschlüsseln, bevor sie mit dem Dienst übertragen werden. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Ja | Weitere Informationen finden Sie unter [Speicherdienstverschlüsselung mit von Kunden verwalteten Schlüsseln im Azure Key Vault](/azure/storage/common/storage-service-encryption-customer-managed-keys?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – |  |
| Verschlüsselte API-Aufrufe| Ja |  |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja |  |
| Unterstützung der VNet-Einschleusung| – |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja | |
| Unterstützung der Tunnelerzwingung| – |  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Azure Monitor-Metriken bereits verfügbar, Azure Monitor-Protokolle in der Vorschauversion |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | Azure Active Directory, gemeinsam verwendeter Schlüssel, SAS-Token. |
| Authorization| Ja | Unterstützung für die Autorisierung über rollenbasierte Zugriffssteuerung (RBAC), POSIX-Zugriffssteuerungslisten (ACLs) und SAS-Token |

### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene | Ja | Azure Resource Manager-Aktivitätsprotokoll |
| Protokollierung und Überwachung auf Datenebene| Ja | Diagnoseprotokolle des Diensts, Azure Monitor-Protokolle in der Vorschauversion  |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Unterstützung für die Versionierung durch den Ressourcenanbieter über Azure Resource Manager-APIs |

## <a name="virtual-machines-and-virtual-machine-scale-sets"></a>Virtuelle Computer und VM-Skalierungsgruppen

[Linux-VMs](/azure/virtual-machines/windows/virtual-machines-windows-security-attributes) | [Windows-VMs](/azure/virtual-machines/windows/virtual-machines-windows-security-attributes) | [VM-Skalierungsgruppen](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-security-attributes)


### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | Ja | Siehe [Verschlüsseln eines virtuellen Linux-Computers in Azure](/azure/virtual-machines/linux/encrypt-disks) und [Verschlüsseln virtueller Datenträger auf einer Windows-VM](/azure/virtual-machines/windows/encrypt-disks). |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| Ja | Azure Virtual Machines unterstützt [ExpressRoute](/azure/expressroute)- und VNET-Verschlüsselung. Siehe [Verschlüsselung während der Übertragung zwischen virtuellen Computern](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Ja | Von Kunden verwaltete Schlüssel werden bei der Azure-Verschlüsselung unterstützt; siehe [Übersicht über die Azure-Verschlüsselung](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms).|
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Per HTTPS und SSL |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | |
| Unterstützung der VNet-Einschleusung| Ja | . |
| Netzwerkisolation und Firewallunterstützung| Ja |  |
| Unterstützung der Tunnelerzwingung| Ja | Siehe [Konfigurieren der Tunnelerzwingung mit dem Azure Resource Manager-Bereitstellungsmodell](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Siehe [Überwachen und Aktualisieren eines virtuellen Linux-Computers in Azure](/azure/virtual-machines/linux/tutorial-monitoring) und [Überwachen und Aktualisieren eines virtuellen Windows-Computers in Azure](/azure/virtual-machines/windows/tutorial-monitoring). |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja |  |
| Authorization| Ja |  |


### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja |  |
| Protokollierung und Überwachung auf Datenebene | Nein |  |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja |  | 


## <a name="vpn-gatewayazurevpn-gatewayvpn-gateway-security-attributes"></a>[VPN Gateway](/azure/vpn-gateway/vpn-gateway-security-attributes)

### <a name="preventative"></a>Prävention

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Verschlüsselung ruhender Daten (z. B. serverseitige Verschlüsselung, serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln und andere Verschlüsselungsfunktionen) | – | VPN Gateway überträgt Kundendaten, speichert diese aber NICHT. |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| Ja | VPN Gateway verschlüsselt Kundenpakete zwischen Azure VPN Gateways und lokalen VPN-Geräte (S2S) oder VPN-Clients (P2S) des Kunden. VPN Gateways unterstützen auch die VNet-zu-VNet-Verschlüsselung. |
| Verarbeitung von Verschlüsselungsschlüsseln (CMK, BYOK usw.)| Nein | Kundenspezifische vorinstallierte Schlüssel werden im Ruhezustand verschlüsselt; aber noch nicht in die CMK integriert. |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Über [Azure Resource Manager](/azure/azure-resource-manager/index) und HTTPS.  |

### <a name="network-segmentation"></a>Netzwerksegmentierung

| Sicherheitsattribut | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| – | |
| Unterstützung der VNet-Einschleusung| – | . |
| Netzwerkisolation und Firewallunterstützung| Ja | VPN Gateways sind dedizierte VM-Instanzen für das virtuelle Kundennetzwerk.  |
| Unterstützung der Tunnelerzwingung| Ja |  |

### <a name="detection"></a>Erkennung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Weitere Informationen finden Sie unter [Azure Monitor-Diagnoseprotokolle/Warnung](/azure/vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log) & [Azure Monitor-Metriken/Warnung](/azure/vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric).  |

### <a name="identity-and-access-management"></a>Identitäts- und Zugriffsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Authentication| Ja | [Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis) für die Verwaltung des Diensts und Konfiguration des Azure VPN Gateways. |
| Authorization| Ja | Unterstützung der Autorisierung über [RBAC](/azure/role-based-access-control/overview). |

### <a name="audit-trail"></a>Überwachungspfad

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Azure Resource Manager-Aktivitätsprotokoll. |
| Protokollierung und Überwachung auf Datenebene | Ja | [Azure Monitor-Diagnoseprotokolle](/azure/azure-resource-manager/resource-group-audit) für die Protokollierung und Überwachung von VPN-Verbindungen. |

### <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitsattribut | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Für Verwaltungsvorgänge kann der Status einer Azure VPN Gateway-Konfiguration als eine Azure Resource Manager-Vorlage exportiert und im Lauf der Zeit versioniert werden. | 

