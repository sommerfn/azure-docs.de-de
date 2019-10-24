---
title: Auswählen des richtigen Bereitstellungstyps für Azure Database for MariaDB
description: In diesem Artikel wird beschrieben, welche Faktoren vor der Bereitstellung von Azure Database for MariaDB als IaaS (Infrastructure-as-a-Service) oder PaaS (Platform-as-a-Service) zu berücksichtigen sind.
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 819b8551650f2e0b484021638ab5554cb557a750
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330035"
---
# <a name="choose-the-right-mariadb-server-option-in-azure"></a>Auswählen der richtigen MariaDB Server-Option in Azure

Mit Azure können Ihre MariaDB-Serverworkloads auf einer gehosteten IaaS-VM (Infrastructure-as-a-Service) oder als gehosteter PaaS (Platform-as-a-Service) ausgeführt werden. PaaS stellt mehrere Bereitstellungsoptionen und Dienstebenen für jede Bereitstellungsoption zur Verfügung. Bei der Auswahl zwischen IaaS und PaaS müssen Sie entscheiden, ob Sie Ihre Datenbank verwalten, Patches anwenden und Sicherungen erstellen oder diese Vorgänge an Azure delegieren möchten.

Berücksichtigen Sie bei der Entscheidung die beiden folgenden Optionen:

- **Azure Database for MariaDB:** Diese Option ist eine vollständig verwaltete MariaDB-Datenbank-Engine, die auf der stabilen Version der MariaDB Community Edition basiert. Diese relationale DBaaS-Lösung (Database-as-a-Service), die auf der Azure-Cloudplattform gehostet wird, fällt in die Branchenkategorie PaaS.

  Mit einer verwalteten Instanz von MariaDB in Azure können Sie integrierte Features nutzen, die ansonsten eine umfangreiche Konfiguration erfordern, wenn der MariaDB-Server entweder lokal oder auf einer Azure-VM gehostet wird.

  Wenn Sie mit MariaDB als Dienst arbeiten, bezahlen Sie für das, was Sie nutzen, und haben die Möglichkeit, für eine bessere Kontrolle ohne Unterbrechung horizontal oder zentral hochzuskalieren. Azure Database for MariaDB verfügt im Gegensatz zu einem eigenständigen MariaDB-Server über zusätzliche Funktionen wie integrierte Hochverfügbarkeit, Intelligenz und Verwaltung.

- **MariaDB auf Azure-VMs:** Diese Option fällt in die Branchenkategorie IaaS. Mit diesem Dienst können Sie den MariaDB-Server innerhalb eines vollständig verwalteten virtuellen Computers auf der Azure-Cloudplattform ausführen. Alle aktuellen Versionen und Editionen von MariaDB können auf einem virtuellen IaaS-Computer installiert werden.

  Der wichtigste Unterschied zu Azure Database for MariaDB ist, dass MariaDB auf Azure-VMs Kontrolle über die Datenbank-Engine bietet. Diese Kontrolle bedeutet jedoch Verantwortung für die Verwaltung der VMs und viele Datenbankverwaltungsaufgaben (DBA). Zu diesen Aufgaben gehören die Wartung und das Patchen von Datenbankservern, die Datenbankwiederherstellung und der Entwurf für Hochverfügbarkeit.

Die Hauptunterschiede zwischen diesen Optionen werden in der folgenden Tabelle aufgeführt:

|            | Azure Database for MariaDB | MariaDB auf Azure-VMs    |
|:-------------------|:-----------------------------|:--------------------|
| Vereinbarung zum Servicelevel (SLA)                | Bietet eine SLA mit einer Verfügbarkeit von 99,99 %.| Bis zu 99,95 % Verfügbarkeit mit mindestens zwei Instanzen in derselben Verfügbarkeitsgruppe.<br/><br/>99,9 % Verfügbarkeit mit einer Einzelinstanz-VM unter Verwendung von Storage Premium.<br/><br/>99,99 % bei Verwendung von Verfügbarkeitszonen mit mehreren Instanzen in mehreren Verfügbarkeitsgruppen.<br/><br/>Weitere Informationen finden Sie in der [SLA für Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/). |
| Betriebssystempatching        | Automatisch  | Von Kunden verwaltet |
| MariaDB-Patching     | Automatisch  | Von Kunden verwaltet |
| Hochverfügbarkeit | Das Hochverfügbarkeitsmodell (HA) basiert auf integrierten Failovermechanismen, die bei einer Unterbrechung auf Knotenebene greifen. In solchen Fällen erstellt der Dienst automatisch eine neue Instanz und fügt Speicher an diese Instanz an. | Kunden entwerfen, implementieren, testen und pflegen Hochverfügbarkeit. Zu den Funktionen können Always-On-Failoverclustering, Always-On-Gruppenreplikation, Protokollversand oder Transaktionsreplikation gehören.|
| Zonenredundanz | Wird derzeit nicht unterstützt. | Azure-VMs können so eingerichtet werden, dass Sie in unterschiedlichen Verfügbarkeitszonen ausgeführt werden. Für eine lokale Lösung müssen Kunden ihr eigenes sekundäres Rechenzentrum erstellen, verwalten und pflegen.|
| Hybridszenario | Mithilfe der [Datenreplikation](https://docs.microsoft.com/azure/MariaDB/concepts-data-in-replication) können Sie Daten von einem externen MariaDB-Server mit dem Azure Database for MariaDB-Dienst synchronisieren. Der externe Server kann lokal, in virtuellen Computern oder in einem Datenbankdienst vorhanden sein, der von anderen Cloudanbietern gehostet wird.<br/><br/> Mit dem Feature für [Lesereplikate](https://docs.microsoft.com/azure/postgresql/concepts-read-replicas) können Sie Daten von einem Azure Database for MariaDB-Masterserver auf bis zu fünf schreibgeschützte Replikatserver replizieren. Die Replikate befinden sich entweder innerhalb derselben Azure-Region oder sind regionsübergreifend. Schreibgeschützte Replikate werden mithilfe der binlog-Replikationstechnologie asynchron aktualisiert.<br/><br/>Die regionsübergreifende Lesereplikation befindet sich derzeit in der Public Preview.| Von Kunden verwaltet
| Sichern und Wiederherstellen | [Serversicherungen](https://docs.microsoft.com/azure/MariaDB/concepts-backup#backups) werden automatisch erstellt und in einem vom Benutzer konfigurierten Speicher, der entweder lokal redundant oder georedundant ist, gespeichert. Dieser Dienst erstellt vollständige, differenzielle und Transaktionsprotokollsicherungen. | Von Kunden verwaltet |
| Überwachen von Datenbankvorgängen | Ermöglicht Kunden das [Festlegen von Warnungen](https://docs.microsoft.com/azure/MariaDB/concepts-monitoring) für den Datenbankvorgang und das Reagieren beim Erreichen von Schwellenwerten. | Von Kunden verwaltet |
| Erweiterter Schutz vor Bedrohungen | Bietet [Advanced Threat Protection](https://docs.microsoft.com/azure/MariaDB/howto-database-threat-protection-portal). Dieser Schutz erkennt anormale Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hindeuten, auf Datenbanken zuzugreifen oder sie missbräuchlich zu nutzen.<br/><br/>Advanced Threat Protection befindet sich derzeit in der Public Preview.| Kunden müssen diesen Schutz selbst erstellen.
| Notfallwiederherstellung | Speichert automatisierte Sicherungen in einem vom Benutzer konfigurierten [lokal redundanten oder georedundanten Speicher](https://docs.microsoft.com/azure/MariaDB/howto-restore-server-portal). Mithilfe von Sicherungen kann auch der Zustand eines Servers zu einem bestimmten Zeitpunkt wiederhergestellt werden. Der Aufbewahrungszeitraum liegt zwischen 7 und 35 Tagen. Die Wiederherstellung erfolgt über das Azure-Portal. | Vollständig von Kunden verwaltet. Die Zuständigkeiten umfassen (sind aber nicht beschränkt auf) Planung, Testen, Archivierung, Speicherung und Aufbewahrung. Eine weitere Option ist die Verwendung eines Azure Recovery Services-Tresors zur Sicherung von Azure-VMs und Datenbanken auf VMs. Diese Option befindet sich in der Vorschauphase. |
| Empfehlungen zur Leistung | Bietet Kunden [Leistungsempfehlungen](https://techcommunity.microsoft.com/t5/Azure-Database-for-MariaDB/Azure-brings-intelligence-and-high-performance-to-Azure-Database/ba-p/769110) basierend auf vom System generierten Nutzungsprotokolldateien. Die Empfehlungen helfen bei der Optimierung von Workloads.<br/><br/>Leistungsempfehlungen befinden sich derzeit in der Public Preview. | Von Kunden verwaltet |

## <a name="business-motivations-for-choosing-paas-or-iaas"></a>Geschäftsmotive für die Entscheidung für PaaS oder IaaS

Es gibt verschiedene Faktoren, die Ihre Entscheidung zwischen PaaS und IaaS zum Hosten Ihrer MariaDB-Datenbanken beeinflussen.

### <a name="cost"></a>Kosten

Begrenzte Mittel sind oft der wichtigste Aspekt, der die beste Lösung für das Hosting Ihrer Datenbanken bestimmt. Dies gilt unabhängig davon, ob Sie ein Startup-Unternehmen mit wenig liquiden Mitteln oder ein Team in einem etablierten Unternehmen sind, das unter engen Budgetbeschränkungen arbeitet. In diesem Abschnitt werden die Grundlagen der Abrechnung und Lizenzierung in Azure in Bezug auf Azure Database for MariaDB und MariaDB auf Azure-VMs beschrieben.

#### <a name="billing"></a>Abrechnung

Azure Database for MariaDB ist derzeit als Dienst in verschiedenen Tarifen mit unterschiedlichen Preisen für Ressourcen verfügbar. Alle Ressourcen werden auf Stundenbasis mit einer festen Gebühr abgerechnet. Die neuesten Informationen zu aktuell unterstützten Dienstebenen, Computegrößen und Speichermengen finden Sie unter dem [V-Kern-basierten Kaufmodell](https://docs.microsoft.com/azure/MariaDB/concepts-pricing-tiers). Sie können Dienstebenen und Computegrößen entsprechend den unterschiedlichen Durchsatzanforderungen Ihrer Anwendung dynamisch anpassen. Ihnen wird der ausgehende Internetdatenverkehr basierend auf den üblichen [Datenübertragungsraten](https://azure.microsoft.com/pricing/details/data-transfers/) berechnet.

Mit Azure Database for MariaDB wird die Datenbanksoftware automatisch von Microsoft konfiguriert, gepatcht und aktualisiert. Diese automatisierten Aktionen senken Ihre Verwaltungskosten. Außerdem verfügt Azure Database for MariaDB über [integrierte Sicherungsfunktionen](https://docs.microsoft.com/azure/MariaDB/concepts-backup). Mithilfe dieser Funktionen können Sie erhebliche Kosteneinsparungen erzielen, vor allem bei einer großen Anzahl von Datenbanken. Im Gegensatz dazu können Sie mit MariaDB auf Azure-VMs eine beliebige MariaDB-Version auswählen und ausführen. Unabhängig von der verwendeten MariaDB-Version bezahlen Sie für die bereitgestellte VM und die Kosten für den jeweils verwendeten MariaDB-Lizenztyp.

Azure Database for MariaDB bietet integrierte Hochverfügbarkeit für jede Art von Unterbrechung auf Knotenebene und bietet dennoch eine SLA-Garantie von 99,99 % für den Dienst. Für Hochverfügbarkeit der Datenbank auf VMs sollten Kunden jedoch die Hochverfügbarkeitsoptionen wie [MariaDB-Replikation](https://mariadb.com/kb/en/library/setting-up-replication/) verwenden, die auf einer MariaDB-Datenbank zur Verfügung stehen. Durch Verwendung einer unterstützten Hochverfügbarkeitsoption erhalten Sie keine zusätzliche SLA. Allerdings können Sie mit zusätzlichen Kosten und höherem Verwaltungsaufwand eine Datenbankverfügbarkeit von mehr als 99,99 % erzielen.

Weitere Informationen zu Preisen finden Sie in den folgenden Artikeln:
* [Azure Database for MariaDB – Preise](https://azure.microsoft.com/pricing/details/MariaDB/)
* [Preise für virtuelle Computer](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Verwaltung

Bei vielen Unternehmen geht es bei der Entscheidung für den Wechsel zu einem Clouddienst ebenso stark um eine Vereinfachung der Verwaltung wie um die Kosten. Mit IaaS und PaaS übernimmt Microsoft folgende Aufgaben:

- Verwaltet die zugrunde liegende Infrastruktur
- Nimmt eine automatische Replikation aller Daten vor, um eine Notfallwiederherstellung zu ermöglichen
- Konfiguriert und aktualisiert die Datenbanksoftware
- Verwaltet den Lastenausgleich
- Führt bei einem Serverausfall ein transparentes Failover durch

In der folgenden Liste sind Verwaltungsaspekte der einzelnen Optionen beschrieben:

* Mit Azure Database for MariaDB können Sie Ihre Datenbank weiterhin verwalten. Es ist jedoch keine Verwaltung von Datenbank-Engine, Betriebssystem oder Hardware mehr erforderlich. Hier einige Beispiele für Elemente, die Sie weiterhin verwalten können:

  - Datenbanken
  - Anmeldung
  - Indexoptimierung
  - Abfrageoptimierung
  - Überwachung
  - Sicherheit

  Darüber hinaus erfordert die Konfiguration der Hochverfügbarkeit für ein anderes Rechenzentrum einen minimalen bis gar keinen Konfigurations- oder Verwaltungsaufwand.

* Mit MariaDB auf Azure-VMs haben Sie die uneingeschränkte Kontrolle über das Betriebssystem und die Konfiguration der MariaDB-Serverinstanzen. Bei einer VM entscheiden Sie, wann ein Update oder Upgrade des Betriebssystems und der Datenbanksoftware durchgeführt werden soll. Außerdem entscheiden Sie, wann zusätzliche Software wie z.B. eine Antivirenanwendung installiert werden soll. Durch einige bereitgestellte Automatisierungsfeatures lassen sich Patching, Sicherungen und Hochverfügbarkeit erheblich vereinfachen. Sie können die Größe der VM, die Anzahl der Datenträger und deren Speicherkonfigurationen steuern. Weitere Informationen finden Sie unter [Größen virtueller Computer und Clouddienste für Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).

### <a name="time-to-move-to-azure"></a>Zeit für einen Wechsel nach Azure

* Azure Database for MariaDB ist die geeignete Lösung für cloudbasierte Anwendungen, wenn Entwicklerproduktivität und eine schnelle Markteinführung für neue Lösungen entscheidend sind. Durch programmgesteuerte Funktionen wie DBA ist der Dienst für Cloudarchitekten und Entwickler geeignet, da er den Verwaltungsaufwand für das zugrunde liegende Betriebssystem und die Datenbank verringert.

* Wenn Sie Zeit und Kosten für den Erwerb neuer lokaler Hardware vermeiden möchten, stellt MariaDB auf Azure-VMs die richtige Lösung für Anwendungen dar, die eine MariaDB-Datenbank oder Zugriff auf MariaDB-Funktionen unter Windows oder Linux benötigen. Diese Lösung eignet sich auch für die unveränderte Migration vorhandener lokaler Anwendungen und Datenbanken zu Azure in den Fällen, in denen Azure Database for MariaDB nicht geeignet ist.

  Da keine Änderungen an der Darstellungs-, Anwendungs- und Datenschicht erforderlich sind, sparen Sie sich die Zeit und das Geld, die sonst für den Neuentwurf der vorhandenen Lösung nötig wären. Sie können sich stattdessen auf die Migration aller Ihrer Lösungen zu Azure konzentrieren und müssen lediglich einige Leistungsoptimierungen durchführen, die möglicherweise für die Azure-Plattform erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Azure Database for MariaDB – Preise](https://azure.microsoft.com/pricing/details/MariaDB/).
* [Erstellen Sie Ihren ersten Server.](https://review.docs.microsoft.com/azure/MariaDB/quickstart-create-MariaDB-server-database-using-azure-portal)
