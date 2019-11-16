---
title: Auswählen des richtigen Bereitstellungstyps für Azure Database for MySQL
description: In diesem Artikel wird beschrieben, welche Faktoren vor der Bereitstellung von Azure Database for MySQL als IaaS (Infrastructure-as-a-Service) oder PaaS (Platform-as-a-Service) zu berücksichtigen sind.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 08/05/2019
ms.openlocfilehash: 5cdd65d5509d8f46f095d91c509a1fda288517c4
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74132431"
---
# <a name="choose-the-right-mysql-server-option-in-azure"></a>Auswählen der richtigen MySQL Server-Option in Azure

Mit Azure können Ihre MySQL-Serverworkloads auf einer gehosteten IaaS-VM (Infrastructure-as-a-Service) oder als gehostete PaaS-Instanz (Platform-as-a-Service) ausgeführt werden. PaaS stellt mehrere Bereitstellungsoptionen und Dienstebenen für jede Bereitstellungsoption zur Verfügung. Bei der Auswahl zwischen IaaS und PaaS müssen Sie entscheiden, ob Sie Ihre Datenbank verwalten, Patches anwenden und Sicherungen erstellen oder diese Vorgänge an Azure delegieren möchten.

Berücksichtigen Sie bei der Entscheidung die beiden folgenden Optionen:

- **Azure Database for MySQL**: Diese Option ist eine vollständig verwaltete MySQL-Datenbank-Engine, die auf der stabilen Version der MySQL Community Edition basiert. Diese relationale DBaaS-Lösung (Database-as-a-Service), die auf der Azure-Cloudplattform gehostet wird, fällt in die Branchenkategorie PaaS.

  Mit einer verwalteten Instanz von MySQL in Azure können Sie integrierte Features nutzen, die ansonsten eine umfangreiche Konfiguration erfordern, wenn der MySQL-Server entweder lokal oder auf einer Azure-VM gehostet wird.

  Wenn Sie mit MySQL als Dienst arbeiten, bezahlen Sie für das, was Sie nutzen. Darüber hinaus haben Sie eine bessere Kontrolle, indem Sie ohne Unterbrechung horizontal oder zentral hochskalieren. Darüber hinaus verfügt Azure Database for MySQL im Gegensatz zu einem eigenständigen MySQL-Server über zusätzliche Funktionen wie integrierte Hochverfügbarkeit, Intelligence und Verwaltung.

- **MySQL auf Azure-VMs**: Diese Option fällt in die Branchenkategorie IaaS. Mit diesem Dienst können Sie den MySQL-Server innerhalb eines vollständig verwalteten virtuellen Computers auf der Azure-Cloudplattform ausführen. Alle aktuellen Versionen und Editionen von MySQL können auf einem virtuellen IaaS-Computer installiert werden.

  Der wichtigste Unterschied zu Azure Database for MySQL ist, dass MySQL auf Azure-VMs die Kontrolle über die Datenbank-Engine ermöglicht. Diese Kontrolle ist aber mit der Verantwortung für die Verwaltung der VMs und vielen Datenbankverwaltungsaufgaben (DBA) verbunden. Zu diesen Aufgaben gehören die Wartung und das Patchen von Datenbankservern, die Datenbankwiederherstellung und der Entwurf für Hochverfügbarkeit.

Die Hauptunterschiede zwischen diesen Optionen werden in der folgenden Tabelle aufgeführt:

|            | Azure Database for MySQL | MySQL auf Azure-VMs    |
|:-------------------|:-----------------------------|:--------------------|
| Vereinbarung zum Servicelevel (SLA)                | Bietet eine SLA mit einer Verfügbarkeit von 99,99 %.| Bis zu 99,95 % Verfügbarkeit mit mindestens zwei Instanzen in derselben Verfügbarkeitsgruppe.<br/><br/>99,9 % Verfügbarkeit mit einer Einzelinstanz-VM unter Verwendung von Storage Premium.<br/><br/>99,99 % bei Verwendung von Verfügbarkeitszonen mit mehreren Instanzen in mehreren Verfügbarkeitsgruppen.<br/><br/>Weitere Informationen finden Sie in der [SLA für Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/). |
| Betriebssystempatching        | Automatisch  | Von Kunden verwaltet |
| MySQL-Patching     | Automatisch  | Von Kunden verwaltet |
| Hochverfügbarkeit | Das Hochverfügbarkeitsmodell (HA) basiert auf integrierten Failovermechanismen, die bei einer Unterbrechung auf Knotenebene greifen. In solchen Fällen erstellt der Dienst automatisch eine neue Instanz und fügt Speicher an diese Instanz an. | Kunden übernehmen den Entwurf, die Implementierung, das Testen und die Wartung der Hochverfügbarkeit. Zu den Funktionen können Always On-Failoverclustering, Always On-Gruppenreplikation, Protokollversand oder Transaktionsreplikation gehören.|
| Zonenredundanz | Wird derzeit nicht unterstützt. | Azure-VMs können so eingerichtet werden, dass Sie in unterschiedlichen Verfügbarkeitszonen ausgeführt werden. Für eine lokale Lösung müssen Kunden ihr eigenes sekundäres Rechenzentrum erstellen, verwalten und warten.|
| Hybridszenario | Die [Datenreplikation](https://docs.microsoft.com/azure/mysql/concepts-data-in-replication) ermöglicht Ihnen das Synchronisieren von Daten von einem externen MySQL-Server mit dem Dienst Azure Database for MySQL. Der externe Server kann lokal, in virtuellen Computern oder in einem Datenbankdienst vorhanden sein, der von anderen Cloudanbietern gehostet wird.<br/><br/> Mit dem Feature für [Lesereplikate](https://docs.microsoft.com/azure/mysql/concepts-read-replicas) können Sie Daten von einem Azure Database for MySQL-Masterserver auf bis zu fünf schreibgeschützten Replikatservern replizieren. Die Replikate befinden sich entweder innerhalb derselben Azure-Region oder sind regionsübergreifend. Schreibgeschützte Replikate werden mithilfe der binlog-Replikationstechnologie asynchron aktualisiert.| Von Kunden verwaltet
| Sichern und Wiederherstellen | [Serversicherungen](https://docs.microsoft.com/azure/mysql/concepts-backup#backups) werden automatisch erstellt und in einem vom Benutzer konfigurierten Speicher, der entweder lokal redundant oder georedundant ist, gespeichert. Dieser Dienst erstellt vollständige, differenzielle und Transaktionsprotokollsicherungen. | Von Kunden verwaltet |
| Überwachen von Datenbankvorgängen | Ermöglicht Kunden das [Festlegen von Warnungen](https://docs.microsoft.com/azure/mysql/concepts-monitoring) für den Datenbankvorgang und das Reagieren beim Erreichen von Schwellenwerten. | Von Kunden verwaltet |
| Erweiterter Schutz vor Bedrohungen | Bietet [Advanced Threat Protection](https://docs.microsoft.com/azure/mysql/howto-database-threat-protection-portal). Bei diesem Schutz werden anomale Aktivitäten erkannt, die auf ungewöhnliche und potenziell schädliche Versuche hindeuten, auf Datenbanken zuzugreifen oder sie missbräuchlich zu nutzen. | Kunden müssen diesen Schutz selbst erstellen.
| Notfallwiederherstellung | Speichert automatisierte Sicherungen in einem vom Benutzer konfigurierten [lokal redundanten oder georedundanten Speicher](https://docs.microsoft.com/azure/mysql/howto-restore-server-portal). Mithilfe von Sicherungen kann auch der Zustand eines Servers zu einem bestimmten Zeitpunkt wiederhergestellt werden. Der Aufbewahrungszeitraum liegt zwischen 7 und 35 Tagen. Die Wiederherstellung erfolgt über das Azure-Portal. | Vollständig von Kunden verwaltet. Die Zuständigkeiten umfassen (sind aber nicht beschränkt auf) Planung, Testen, Archivierung, Speicherung und Aufbewahrung. Eine weitere Option ist die Verwendung eines Azure Recovery Services-Tresors zur Sicherung von Azure-VMs und Datenbanken auf VMs. Diese Option befindet sich in der Vorschauphase. |
| Empfehlungen zur Leistung | Bietet Kunden [Leistungsempfehlungen](https://techcommunity.microsoft.com/t5/Azure-Database-for-MySQL/Azure-brings-intelligence-and-high-performance-to-Azure-Database/ba-p/769110) basierend auf vom System generierten Nutzungsprotokolldateien. Die Empfehlungen helfen bei der Optimierung von Workloads. | Von Kunden verwaltet |

## <a name="business-motivations-for-choosing-paas-or-iaas"></a>Geschäftsmotive für die Entscheidung für PaaS oder IaaS

Es gibt verschiedene Faktoren, die Ihre Entscheidung zwischen PaaS und IaaS zum Hosten Ihrer MySQL-Datenbanken beeinflussen.

### <a name="cost"></a>Kosten

Begrenzte Mittel sind oft der wichtigste Aspekt, der die beste Lösung für das Hosting Ihrer Datenbanken bestimmt. Dies gilt unabhängig davon, ob Sie ein Startup-Unternehmen mit wenig liquiden Mitteln oder ein Team in einem etablierten Unternehmen sind, das unter engen Budgetbeschränkungen arbeitet. In diesem Abschnitt werden die Grundlagen zur Abrechnung und Lizenzierung in Azure in Bezug auf Azure Database for MySQL und MySQL auf Azure-VMs beschrieben.

#### <a name="billing"></a>Abrechnung

Azure Database for MySQL ist derzeit als Dienst in verschiedenen Tarifen mit unterschiedlichen Preisen für Ressourcen verfügbar. Alle Ressourcen werden auf Stundenbasis mit einer festen Gebühr abgerechnet. Die neuesten Informationen zu aktuell unterstützten Dienstebenen, Computegrößen und Speichermengen finden Sie unter dem [V-Kern-basierten Kaufmodell](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers). Sie können Dienstebenen und Computegrößen entsprechend den unterschiedlichen Durchsatzanforderungen Ihrer Anwendung dynamisch anpassen. Ihnen wird der ausgehende Internetdatenverkehr basierend auf den üblichen [Datenübertragungsraten](https://azure.microsoft.com/pricing/details/data-transfers/) berechnet.

Mit Azure Database for MySQL wird die Datenbanksoftware automatisch von Microsoft konfiguriert, gepatcht und aktualisiert. Diese automatisierten Aktionen senken Ihre Verwaltungskosten. Außerdem verfügt Azure Database for MySQL über [integrierte Sicherungsfunktionen](https://docs.microsoft.com/azure/mysql/concepts-backup). Mithilfe dieser Funktionen können Sie erhebliche Kosteneinsparungen erzielen, vor allem bei einer großen Anzahl von Datenbanken. Im Gegensatz dazu können Sie mit MySQL auf Azure-VMs eine beliebige MySQL-Version auswählen und ausführen. Unabhängig von der verwendeten MySQL-Version bezahlen Sie für die bereitgestellte VM und die Kosten für den jeweils verwendeten MySQL-Lizenztyp.

Azure Database for MySQL bietet integrierte Hochverfügbarkeit für jede Art von Unterbrechung auf Knotenebene und bietet gleichzeitig eine SLA-Garantie von 99,99 % für den Dienst. Zur Erzielung von Hochverfügbarkeit der Datenbank auf VMs sollten Kunden aber Hochverfügbarkeitsoptionen, z. B. die [MySQL-Replikation](https://dev.mysql.com/doc/refman/8.0/en/replication.html) verwenden, die auf einer MySQL-Datenbank zur Verfügung stehen. Durch Verwendung einer unterstützten Hochverfügbarkeitsoption erhalten Sie keine zusätzliche SLA. Allerdings können Sie mit zusätzlichen Kosten und höherem Verwaltungsaufwand eine Datenbankverfügbarkeit von mehr als 99,99 % erzielen.

Weitere Informationen zu Preisen finden Sie in den folgenden Artikeln:
* [Azure Database for MySQL: Preise](https://azure.microsoft.com/pricing/details/mysql/)
* [Preise für virtuelle Computer](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Verwaltung

Bei vielen Unternehmen geht es bei der Entscheidung für den Wechsel zu einem Clouddienst ebenso um eine Vereinfachung der Verwaltung wie um die Kosten. Bei IaaS und PaaS übernimmt Microsoft folgende Aufgaben:

- Verwaltet die zugrunde liegende Infrastruktur
- Nimmt eine automatische Replikation aller Daten vor, um eine Notfallwiederherstellung zu ermöglichen
- Konfiguriert und aktualisiert die Datenbanksoftware
- Verwaltet den Lastenausgleich
- Führt bei einem Serverausfall ein transparentes Failover aus

In der folgenden Liste sind Verwaltungsaspekte der einzelnen Optionen beschrieben:

* Mit Azure Database for MySQL können Sie Ihre Datenbank weiterhin verwalten. Es ist aber keine Verwaltung von Datenbank-Engine, Betriebssystem oder Hardware mehr erforderlich. Hier sind einige Beispiele für Elemente angegeben, die Sie weiterhin verwalten können:

  - Datenbanken
  - Anmeldung
  - Indexoptimierung
  - Abfrageoptimierung
  - Überwachung
  - Sicherheit

  Darüber hinaus erfordert die Konfiguration der Hochverfügbarkeit für ein anderes Rechenzentrum nur noch einen minimalen bzw. gar keinen Konfigurations- oder Verwaltungsaufwand mehr.

* Mit MySQL auf virtuellen Azure-Computern haben Sie die uneingeschränkte Kontrolle über das Betriebssystem und die Konfiguration der MySQL-Serverinstanzen. Bei einer VM entscheiden Sie, wann ein Update oder Upgrade des Betriebssystems und der Datenbanksoftware durchgeführt werden soll. Außerdem entscheiden Sie, wann zusätzliche Software, z. B. eine Antivirenanwendung, installiert werden soll. Durch einige bereitgestellte Automatisierungsfeatures lassen sich Patching, Sicherungen und Hochverfügbarkeit erheblich vereinfachen. Sie können die Größe der VM, die Anzahl von Datenträgern sowie deren Speicherkonfigurationen steuern. Weitere Informationen finden Sie unter [Größen virtueller Computer und Clouddienste für Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).

### <a name="time-to-move-to-azure"></a>Zeit für einen Wechsel nach Azure

* Azure Database for MySQL ist die geeignete Lösung für cloudbasierte Anwendungen, wenn Entwicklerproduktivität und eine schnelle Markteinführung für neue Lösungen entscheidend sind. Aufgrund von programmgesteuerten Funktionen wie DBA ist der Dienst für Cloudarchitekten und Entwickler geeignet, da er den Verwaltungsaufwand für das zugrunde liegende Betriebssystem und die Datenbank verringert.

* Wenn Sie Zeit und Kosten für den Erwerb neuer lokaler Hardware vermeiden möchten, stellt MySQL auf Azure-VMs die richtige Lösung für Anwendungen dar, die eine MySQL-Datenbank oder Zugriff auf MySQL-Funktionen unter Windows oder Linux benötigen. Diese Lösung eignet sich auch in Fällen, in denen Azure Database for MySQL nicht geeignet ist, für die Migration vorhandener lokaler Anwendungen und Datenbanken in unverändertem Zustand zu Azure.

  Da keine Änderungen an der Darstellungs-, Anwendungs- und Datenschicht erforderlich sind, sparen Sie sich die Zeit und das Geld, die sonst für den Neuentwurf der vorhandenen Lösung nötig wären. Sie können sich stattdessen auf die Migration Ihrer gesamten Lösungen zu Azure konzentrieren und müssen lediglich einige Leistungsoptimierungen durchführen, die für die Azure-Plattform unter Umständen erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

* [Azure Database for MySQL: Preise](https://azure.microsoft.com/pricing/details/MySQL/)
* [Erstellen Sie Ihren ersten Server.](https://review.docs.microsoft.com/azure/MySQL/quickstart-create-MySQL-server-database-using-azure-portal)
