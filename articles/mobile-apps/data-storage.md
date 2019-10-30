---
title: Speichern und Verwalten von Anwendungsdaten in der Cloud mit Visual Studio App Center und Azure-Diensten
description: Erfahren Sie mehr über die Dienste, z. B. App Center, mit denen Sie die Daten einer mobilen Anwendung in der Cloud speichern und verwalten können.
author: elamalani
ms.assetid: 12344321-0123-4678-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: b7043c7e9bd944bfb3633397edfa3c35609bd8ec
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795110"
---
# <a name="store-sync-and-query-mobile-application-data-from-the-cloud"></a>Speichern, Synchronisieren und Abfragen von Daten einer mobilen Anwendung aus der Cloud
Unabhängig von der Art der Anwendung, die Sie erstellen, werden Sie wahrscheinlich Daten erzeugen und verarbeiten. Die Erwartungen der Benutzer sind hoch, und die Benutzer möchten, dass die Anwendung unter allen Umständen schnell und reibungslos funktioniert. Die meisten Anwendungen funktionieren auf mehreren Geräten, sodass Sie entweder über ein Desktop- oder über ein mobiles Gerät auf Ihre Anwendung zugreifen können. Mehrere Benutzer können die Anwendung gleichzeitig nutzen und Daten teilen, wobei sie erwarten, dass sie sofortigen und Echtzeitzugriff auf die Daten erhalten.

Darüber hinaus haben die Benutzer Ihrer Anwendung nicht immer eine Internetverbindung. Anwendungen sind so konzipiert, und von Anwendungen wird erwartet, dass sie mit und ohne Internetverbindung funktionieren. Angesichts dieser ständig größer werdenden Komplexität ist es eine große Aufgabe für Entwickler, die richtige Lösung für das Speichern und Synchronisieren ihrer Daten in der Cloud zu wählen, um ein hervorragendes Kundenerlebnis für ihre Anwendung zu bieten.

Microsoft stellt eine Vielzahl von Diensten bereit, mit denen es überflüssig wird, Server einzurichten, Datenbanken auszuwählen oder sich Gedanken über die Skalierbarkeit oder Sicherheit zu machen. Vor diesem Hintergrund können Sie größtmögliche Benutzerfreundlichkeit bereitstellen. Diese Dienste bieten eine hervorragende Entwicklerumgebung, mit der Sie Anwendungsdaten in der Cloud über SQL- oder NoSQL-APIs speichern, Daten auf allen Geräten synchronisieren und die Anwendung für die Zusammenarbeit mit oder ohne Netzwerkverbindung einrichten können, um das Erstellen von skalierbaren und robusten Anwendungen zu unterstützen.

Verwenden Sie die folgenden Dienste, um Daten einer mobilen Anwendung in der Cloud zu verwalten und zu speichern.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Data](/appcenter/data/) ist ein Datenverwaltungsdienst, der es Anwendungen ermöglicht, Anwendungsdaten in der Cloud auf verschiedenen Geräten und Plattformen in Online- und Offlineszenarien zu verwalten, zu speichern und zu synchronisieren. Dieser Dienst, der **auf dem Cosmos DB-Dienst aufsetzt**, der entsprechend dem Wachstum Ihrer Benutzerbasis und Anzahl von Anwendungen skaliert, stellt geringe Latenz, Hochverfügbarkeit und hohe Skalierbarkeit für sämtliche Ihrer Daten sicher.

**Wichtige Features**
- Einfaches **Bereitstellen einer neuen Cosmos DB-Datenbank** oder **Herstellen einer Verbindung mit einer vorhandenen Cosmos DB-Datenbank** über das App Center-Portal.
- **Unterstützung von NoSQL-Datenbanken**, um Anwendungsdaten problemlos zu speichern, zu synchronisieren und abzufragen.
- Dieser Dienst, der **auf Cosmos DB aufsetzt**, erbt die meisten der wichtigen Features, die von Azure Cosmos DB bereitgestellt werden, und kann **global skalieren**, damit Ihre Geschäftsanforderungen erfüllt werden.
- Funktionalität für **Online- und Offlinesynchronisierung**, um Daten auf Geräten zu synchronisieren.
- **Für mobile Clients vorgesehene SDKs**, mit denen Sie private Anwendungsdaten problemlos verwalten können.
- **Plattformunterstützung**: iOS, Android, Xamarin, React Native.

**Referenzen**
- [Registrieren für App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Erste Schritte mit App Center Data](/appcenter/data/getting-started)

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) ist der global verteilte Multimodell-Datenbankdienst von Microsoft, mit dem Sie weltweit verfügbare Anwendungen erstellen können. Mit Cosmos DB können Sie Durchsatz und Speicher mit nur einem Klick elastisch und unabhängig voneinander über eine beliebige Anzahl von weltweiten Azure-Regionen hinweg skalieren. Sie können den Durchsatz und Speicherplatz elastisch skalieren und von einem schnellen Datenzugriff im einstelligen Millisekundenbereich über Ihre bevorzugten API-Oberflächen wie SQL, MongoDB, Cassandra, Tables oder Gremlin profitieren. Cosmos DB bietet einzigartig umfassende Vereinbarungen zum Service (Service Level Agreements, SLAs) für Durchsatz, Latenz, Verfügbarkeit und Konsistenz.

**Wichtige Features**
- Unterstützt eine **Vielzahl von APIs** einschließlich SQL(Core)-API, Cassandra-API, MongoDB-API, Gremlin-API und Table-API.
- **Globale, sofort einsatzbereite Verteilung** repliziert Ihre Daten, wo auch immer sich Ihre Benutzer befinden, sodass Ihre Benutzer mit dem Replikat der Daten interagieren können, das sich am nächsten befindet.
- **Keine Schema- oder Indexverwaltung**, da die Datenbank-Engine vollständig schemaunabhängig ist.
- **Universelle regionale Präsenz**, weil Cosmos DB in allen Azure-Regionen weltweit verfügbar ist, einschließlich 54+-Regionen in der Public Cloud.
- **Mehrere genau definierte Konsistenzoptionen**, denn das Cosmos DB-Protokoll für Multimasterreplikation bietet fünf klar definierte Konsistenzoptionen: starke Veraltung, begrenzte Veraltung, Sitzungskonsistenz, Präfixkonsistenz und letztliche Konsistenz.
- **99,999 % Verfügbarkeit** für Lese- und Schreibvorgänge.
- **Programmgesteuertes (oder über das Portal) Aufrufen des regionalen Failovers** Ihres Cosmos-Kontos, um sicherzustellen, dass Ihre Anwendung so konzipiert ist, dass sie auch nach einem regionalen Notfall weiter funktioniert.
- Weltweit **garantiert niedrige Latenz bei 99-Perzentil**.

**Referenzen**
- [Azure-Portal](https://portal.azure.com) 
- [Documentation](/azure/cosmos-db/introduction)   

## <a name="azure-sql-database"></a>Azure SQL-Datenbank
 [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) ist ein universeller relationaler verwalteter Datenbankdienst, mit dem Sie eine hochverfügbare und hochleistungsfähige Datenspeicherebene für Anwendungen und Lösungen in der Azure-Cloud erstellen können.

**Wichtige Features**
- **Elastische Datenbankmodelle und -tools**: Eine elastische Datenbank gibt Entwicklern die Möglichkeit, Ressourcen für eine Gruppe von Datenbanken zur Skalierung zu bündeln. Diese Datenbanken können dann administrativ verwaltet werden, indem ein Skript als Auftrag übermittelt wird und die SQL-Datenbank das Skript über die Datenbanken hinweg ausführt.
- **Hohe Leistung**: Anwendungen mit hohem Durchsatz die neueste Version nutzen, die 25 % mehr Premium-Datenbankleistung bereitstellt.
- **Sicherungen, Replikation und Hochverfügbarkeit**: Integrierte Replikation und eine von Microsoft unterstützte Vereinbarung zum Servicelevel (SLA) bieten Anwendungskontinuität und Schutz vor katastrophalen Ereignissen. Mit der aktiven Georeplikation können Sie Failover und Self-Service-Wiederherstellung konfigurieren, die vollständige Kontrolle über die „Hoppla-Wiederherstellung“ (Datenwiederherstellung aus verfügbaren Datensicherungen von bis zu 35 Tagen) bieten.
- **Wartung gegen null**: Automatische Software ist Teil des Dienstes, und integrierte Systemreplikate tragen dazu bei, inhärenten Datenschutz, inhärente Datenbankbetriebszeit und inhärente Systemstabilität zu gewährleisten. Systemreplikate werden automatisch auf neue Computer verschoben, die bei Ausfällen älterer Computer sofort bereitgestellt werden.
- **Sicherheit**: SQL-Datenbank bietet eine Reihe von Sicherheitsfunktionen, mit denen Sie sowohl Organisations- als auch branchenspezifische Konformitätsrichtlinien einhalten.
    - Überwachung bietet Entwicklern die Möglichkeit, konformitätsbezogene Aufgaben auszuführen und sich über Aktivitäten zu informieren.
    - Entwickler und IT-Experten können Richtlinien auf Datenbankebene implementieren, um den Zugriff auf vertrauliche Daten mithilfe der Funktionen für Sicherheit auf Zeilenebene, dynamische Datenmaskierung und Transparent Data Encryption für Azure SQL-Datenbank einzuschränken.
    - SQL-Datenbank wird von anerkannten Cloudauditoren im Rahmen wichtiger Azure-Konformitätszertifizierungen und -genehmigungen überprüft, z. B. HIPAA-Geschäftspartnerverträge, ISO/IEC 27001:2005, FedRAMP und EU-Modellklauseln.

**Referenzen**
- [Azure-Portal](https://portal.azure.com) 
- [Documentation](/azure/sql-database/)
   