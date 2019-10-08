---
title: Auswählen des richtigen Bereitstellungstyps für Ihre Azure Database for MySQL
description: In diesem Artikel wird beschrieben, welche Überlegungen vor der Verwendung von IaaS (Infrastructure-as-a-Service) oder PaaS (Platform-as-a-Service) für Ihre Azure Database for MySQL berücksichtigt werden müssen.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 08/05/2019
ms.openlocfilehash: 7bcbf379ea8d046c8c477dc716711a6a6ffa1dc9
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71817365"
---
# <a name="choose-the-right-mysql-server-option-in-azure"></a>Auswählen der richtigen MySQL Server-Option in Azure

Mit Azure können Ihre MySQL-Serverworkloads auf einer gehosteten Infrastruktur-VM (IaaS) oder als gehosteter Dienst (PaaS) ausgeführt werden. PaaS stellt mehrere Bereitstellungsoptionen und Dienstebenen für jede Bereitstellungsoption zur Verfügung. Bei der Entscheidung zwischen PaaS und IaaS müssen Sie festlegen, ob Sie Ihre Datenbank verwalten, Patches anwenden und Backups erstellen oder diese Vorgänge an Azure delegieren möchten.</br>

Ziehen Sie basierend auf der Antwort auf diese Frage die folgenden Optionen in Betracht: <br/>

**Azure Database for MySQL** ist eine vollständig verwaltete MySQL-Datenbank-Engine, die auf der stabilen Version der Community-Edition basiert. Diese relationale DBaaS-Lösung (Database-as-a-Service), die in der Azure-Cloud gehostet wird, fällt in die Branchenkategorie Platform-as-a-Service (PaaS). Mit einer verwalteten Instanz von MySQL in Azure können Sie integrierte Features und Funktionen nutzen, die in MySQL Server eine umfangreiche Konfiguration erfordern würden (lokal oder auf einem virtuellen Azure-Computer). Wenn Sie mit MySQL als Dienst arbeiten, bezahlen Sie für das, was Sie nutzen, und haben die Möglichkeit, horizontal oder zentral hochzuskalieren und ohne Unterbrechung mehr Leistung hinzuzufügen. Darüber hinaus verfügt Azure Database for MySQL im Gegensatz zu einem eigenständigen MySQL-Server über zusätzliche Funktionen wie integrierte Hochverfügbarkeit, Intelligenz und Verwaltung. <br/><br/>
**MySQL auf einer Azure-VM** fällt in die Branchenkategorie „Infrastructure-as-a-Service (IaaS)“ und ermöglicht es Ihnen, MySQL-Server auf einem vollständig verwalteten virtuellen Computer in der Azure-Cloud auszuführen. Alle aktuellen Versionen und Editionen von MySQL können auf einem virtuellen IaaS-Computer installiert werden. Der wichtigste Unterschied zu Azure Database for MySQL ist, dass MySQL auf Azure-VMs die Kontrolle über die Datenbank-Engine ermöglicht. Diese Kontrolle geht jedoch zu Lasten der zusätzlichen Verantwortung für die Verwaltung der virtuellen Computer und zahlreicher DBA-Aufgaben wie Wartung/Patchen des Datenbankservers, Wiederherstellung und Hochverfügbarkeitsentwurf und vieles mehr.

Die Hauptunterschiede zwischen diesen Optionen werden in der folgenden Tabelle aufgeführt:

|            | **Azure Database for MySQL** | **MySQL auf Azure-VMs**    |
|:-------------------|:-----------------------------|:--------------------|
| **SLA**                | Bietet eine SLA mit einer Verfügbarkeit von 99,99 %.| Bis zu 99,95 % Verfügbarkeit mit mindestens zwei Instanzen in derselben Verfügbarkeitsgruppe. <br/>99,9 % Einzelinstanz-VM unter Verwendung von Storage Premium <br/> 99,99 % mit Verfügbarkeitszone mit mindestens zwei Instanzen in mindestens zwei Verfügbarkeitsgruppen.<br/> Hinweis: [Virtual Machine-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) |
| **Betriebssystempatching**        | Automatisch  | Von Kunden verwaltet |
|**MySQL-Patching**     | Automatisch  | Von Kunden verwaltet |
| **Hochverfügbarkeit** | Das Hochverfügbarkeitsmodell (HA) basiert auf integrierten Failovermechanismen, die bei einer Unterbrechung auf Knotenebene greifen. In solchen Fällen erstellt der Dienst automatisch eine Instanz und fügt Speicher an diese neue Instanz an. | Hochverfügbarkeit wird vom Kunden entworfen, implementiert, getestet und verwaltet. Dies kann abhängig von der verwendeten Version der MySQL-Engine auch Always-On (Failoverclustering oder Gruppenreplikation), Protokollversand und Transaktionsreplikation einschließen.|
| **Zonenredundanz** | Wird derzeit nicht unterstützt. | Azure-VMs können so eingerichtet werden, dass Sie in unterschiedlichen Verfügbarkeitszonen ausgeführt werden. Für eine lokale Lösung wird davon ausgegangen, dass Kunden ihr eigenes sekundäres Rechenzentrum erstellen, verwalten und pflegen.|
| **Hybridszenarien** | Die [Datenreplikation](https://docs.microsoft.com/azure/mysql/concepts-data-in-replication) ermöglicht das Synchronisieren von Daten von einem externen MySQL-Server mit dem Dienst Azure Database for MySQL. Der externe Server kann lokal, in virtuellen Computern oder in einem Datenbankdienst vorhanden sein, der von anderen Cloudanbietern gehostet wird.  <br/> <br/> Mit dem Feature für [Lesereplikate](https://docs.microsoft.com/azure/postgresql/concepts-read-replicas) können Sie Daten von einem Azure Database for MySQL-Server (Masterserver) auf bis zu fünf schreibgeschützte Server (Replikate) in derselben Azure-Region oder regionsübergreifend replizieren. Schreibgeschützte Replikate werden mithilfe der binlog-Replikationstechnologie asynchron aktualisiert.   <br/> <br/> Hinweis: Die regionsübergreifende Lesereplikation befindet sich derzeit in der öffentlichen Vorschau.| Von Kunden verwaltet <br/>
| **Sichern und Wiederherstellen** | [Serversicherungen](https://docs.microsoft.com/azure/mysql/concepts-backup#backups) werden automatisch erstellt und in einem vom Benutzer konfigurierten lokal redundanten oder georedundanten Speicher gespeichert. Dieser Dienst erstellt vollständige, differenzielle und Transaktionsprotokollsicherungen. | Von Kunden verwaltet |
| **Überwachen von Datenbankvorgängen** | Ermöglicht Kunden das [Festlegen von Warnungen](https://docs.microsoft.com/azure/mysql/concepts-monitoring) für den Daten Bankvorgang und das Reagieren beim Erreichen von Schwellenwerten. | Von Kunden verwaltet |
| **Advanced Threat Protection** | [Advanced Threat Protection](https://docs.microsoft.com/azure/mysql/howto-database-threat-protection-portal) erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf Datenbanken zuzugreifen oder diese zu nutzen. | Kunden müssen das System selbst erstellen.
| **Sicherungen und (Notfallwiederherstellung)** | Speichert automatisierte Sicherungen in einem Benutzer, der in [lokal redundantem oder georedundantem Speicher](https://docs.microsoft.com/azure/mysql/howto-restore-server-portal) konfiguriert ist. Sicherungen können auch verwendet werden, um für einen Server den Zustand zu einem bestimmten Zeitpunkt wiederherzustellen. Der Aufbewahrungszeitraum kann festgelegt werden und zwischen 7 und 35 Tage betragen. Eine Wiederherstellung ist über das Azure-Portal möglich. <br/> | Vollständig vom Kunden verwaltet, einschließlich, aber nicht beschränkt auf Planung, Testen, Archivierung, Speicherung und Aufbewahrung. Eine weitere Option ist die Verwendung von Azure Recovery Services Vault zur Sicherung von Azure-VMs und Datenbanken auf VMs (in der Vorschau). |
| **Leistungsempfehlung** | Bietet Kunden eine proaktive [Leistungsempfehlung](https://techcommunity.microsoft.com/t5/Azure-Database-for-MySQL/Azure-brings-intelligence-and-high-performance-to-Azure-Database/ba-p/769110) basierend auf den Nutzungstelemetriedaten, um die Optimierung von Workloads zu unterstützen. | Von Kunden verwaltet |


## <a name="business-motivations-for-choosing-paas-or-iaas"></a>Geschäftsmotive für die Entscheidung für PaaS oder IaaS

Es gibt verschiedene Faktoren, die Ihre Entscheidung zwischen PaaS und IaaS zum Hosten Ihrer MySQL-Datenbanken beeinflussen:

### <a name="cost"></a>Kosten

Unabhängig davon, ob Sie ein Startup-Unternehmen mit wenig liquiden Mitteln oder ein Team in einem etablierten Unternehmen sind, das unter engen Budgetbeschränkungen arbeitet, ist eine begrenzte Finanzierung oft die primäre Überlegung bei der Auswahl der besten Lösung für das Hosting Ihrer Datenbanken. In diesem Abschnitt werden die Grundlagen zur Abrechnung und Lizenzierung in Azure in Bezug auf Azure Database for MySQL und MySQL auf Azure-VMs beschrieben:

#### <a name="billing"></a>Abrechnung

Derzeit ist Azure Database for MySQL als Dienst in verschiedenen Tarifen mit unterschiedlichen Preisen für Ressourcen verfügbar, die alle auf Stundenbasis mit einer festen Gebühr abgerechnet werden. Die neuesten Informationen zu aktuell unterstützten Dienstebenen, Computegrößen und Speichermengen finden Sie unter [vCore-basiertes Kaufmodell](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers). Sie können Dienstebenen und Computegrößen entsprechend den unterschiedlichen Durchsatzanforderungen Ihrer Anwendung dynamisch anpassen. Ihnen wird der ausgehende Internetdatenverkehr basierend auf den üblichen [Datenübertragungsraten](https://azure.microsoft.com/pricing/details/data-transfers/)berechnet.

Bei Azure Database for MySQL wird die Datenbanksoftware automatisch von Microsoft konfiguriert, gepatcht und aktualisiert. Dadurch sinken Ihre Verwaltungskosten. Darüber hinaus helfen Ihnen die [integrierten Datensicherungsfunktionen](https://docs.microsoft.com/azure/mysql/concepts-backup) dabei, erhebliche Kosteneinsparungen zu erzielen, vor allem bei einer großen Anzahl von Datenbanken. Mit MySQL auf Azure-VMs können Sie eine beliebige MySQL-Version auswählen und ausführen. Unabhängig von der verwendeten MySQL-Version bezahlen Sie für den bereitgestellten virtuellen Computer und die Kosten für den für MySQL verwendeten spezifischen Lizenztyp.

Azure Database for MySQL bietet integrierte Hochverfügbarkeit für jede Art von Unterbrechung auf Knotenebene und bietet trotzdem eine SLA mit 99,99 % für den Dienst. Für Hochverfügbarkeit der Datenbank (HA) auf virtuellen Computern sollten Kunden jedoch die Hochverfügbarkeitsoptionen für die MySQL-Datenbank in Betracht ziehen, z.B [MySQL-Replikation](https://dev.mysql.com/doc/refman/8.0/en/replication.html). Durch die Verwendung einer Hochverfügbarkeitsoption erhalten Sie zwar keine zusätzliche SLA, können aber eine Datenbankverfügbarkeit von mehr als 99,99 Prozent bei zusätzlichen Kosten und Verwaltungsmehraufwand erreichen.

Weitere Informationen zur Preisberechnung finden Sie in den folgenden Ressourcen:
* [Azure Database for MySQL: Preise](https://azure.microsoft.com/pricing/details/mysql/)
* [Preise für virtuelle Computer](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Verwaltung

Bei vielen Unternehmen wird die Entscheidung zum Wechsel auf einen Clouddienst vom Bestreben um eine Vereinfachung der Verwaltung getrieben. Mit IaaS und PaaS verwaltet Microsoft die zugrunde liegende Infrastruktur und repliziert automatisch alle Daten für eine Notfallwiederherstellung, konfiguriert und aktualisiert die Datenbanksoftware, verwaltet den Lastenausgleich und führt bei einem Serverausfall ein transparentes Failover durch.

* Mit **Azure Database for MySQL** können Sie sich weiterhin um die Verwaltung Ihrer Datenbank kümmern, müssen aber die Datenbank-Engine, das Betriebssystem und die Hardware nicht mehr verwalten. Weiterhin verwalten können Sie z. B. die Datenbanken und Anmeldungen, die Index- und Abfrageoptimierung, die Überwachung und die Sicherheit. Darüber hinaus erfordert die Konfiguration der Hochverfügbarkeit für ein anderes Rechenzentrum einen minimalen oder gar keinen Konfigurations- oder Verwaltungsaufwand.<br/><br/>
* Mit **MySQL auf virtuellen Azure-Computern** haben Sie die uneingeschränkte Kontrolle über das Betriebssystem und die Konfiguration der MySQL-Serverinstanzen. Bei einem virtuellen Computer entscheiden Sie, wann das Betriebssystem und die Datenbanksoftware aktualisiert und wann zusätzliche Programme wie etwa eine Antivirenanwendung installiert werden sollen. Durch einige bereitgestellte Automatisierungsfeatures lassen sich Patching, Sicherungen und Hochverfügbarkeit erheblich vereinfachen. Darüber hinaus können Sie die Größe des virtuellen Computers, die Anzahl der Laufwerke und deren Speicherkonfigurationen steuern. Weitere Informationen finden Sie unter „Größen von virtuellen Computern und Clouddiensten für Azure“.

### <a name="time-to-move-to-azure-br"></a>Zeit für einen Wechsel nach Azure <br/>
* **Azure Database for MySQL** ist die geeignete Lösung für cloudbasierte Anwendungen, wenn Entwicklerproduktivität und eine schnelle Markteinführung für neue Lösungen entscheidend sind. Durch programmgesteuerte DBA-ähnliche Funktionen ist der Dienst perfekt für Cloudarchitekten und Entwickler geeignet, da er den Verwaltungsaufwand für das zugrunde liegende Betriebssystem und die Datenbank verringert.<br/><br/>
* **MySQL auf virtuellen Azure-Computern** eignet sich hervorragend für vorhandene oder neue Anwendungen, die eine MySQL-Datenbank oder den Zugriff auf Funktionen in der MySQL-Datenbank unter Windows/Linux benötigen, wenn Sie Zeit und Kosten für den Erwerb neuer lokaler Hardware vermeiden möchten. Diese Option ist auch eine gute Wahl für die unveränderte Migration vorhandener lokaler Anwendungen und Datenbanken zu Azure in den Fällen, in denen eine Azure Database for MySQL-Instanz nicht gut geeignet ist. Da keine Änderungen an der Präsentations-, Anwendungs- und Datenschicht erforderlich sind, sparen Sie sich die Zeit und das Geld, die sonst für den Neuentwurf der vorhandenen Lösung nötig wären. Sie können in diesem Fall alle Lösungen sofort zu Azure migrieren und müssen lediglich einige Leistungsoptimierungen durchführen, die möglicherweise von der Azure-Plattform verlangt werden.

## <a name="next-steps"></a>Nächste Schritte

* Siehe [Azure Database for MySQL: Preise](https://azure.microsoft.com/pricing/details/mysql/)
* [Erstellen Sie Ihren ersten Server.](https://review.docs.microsoft.com/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal)

