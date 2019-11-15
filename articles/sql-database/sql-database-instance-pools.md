---
title: Instanzenpools (Vorschauversion)
description: In diesem Artikel werden Azure SQL-Datenbank-Instanzenpools beschrieben (Vorschau).
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab
ms.date: 09/05/2019
ms.openlocfilehash: 8738d1ad54d3ab63d8d2efc939aa9daacbe91c13
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73810404"
---
# <a name="what-are-sql-database-instance-pools-preview"></a>Was sind SQL-Datenbank-Instanzenpools (Vorschau)?

Bei Instanzenpools handelt es sich um eine neue Ressource in Azure SQL-Datenbank, die eine bequeme und kosteneffiziente Möglichkeit bietet, kleinere SQL-Instanzen in beliebigem Umfang zur Cloud zu migrieren.

Mithilfe von Instanzenpools können Sie Computeressourcen entsprechend Ihren gesamten Migrationsanforderungen vorab bereitstellen. Anschließend können Sie mehrere einzelne verwaltete Instanzen bis zu der vorab bereitgestellten Computeebene bereitstellen. Wenn Sie z. B. 8 virtuelle Kerne vorab bereitstellen, können Sie zwei 2-V-Kern-Instanzen und eine 4-V-Kern-Instanz bereitstellen und dann Datenbanken in diese Instanzen migrieren. Vor der Verfügbarkeit von Instanzenpools mussten kleinere und weniger computeintensive Workloads bei der Migration zur Cloud häufig in einer größeren verwalteten Instanz konsolidiert werden. Die Notwendigkeit, Gruppen von Datenbanken zu einer großen Instanz zu migrieren, erforderte in der Regel eine sorgfältige Kapazitätsplanung und Ressourcenkontrolle, zusätzliche Sicherheitsüberlegungen und eine zusätzliche Datenkonsolidierung auf Instanzebene.

Darüber hinaus unterstützen Instanzenpools die native VNET-Integration, sodass mehrere Instanzenpools und mehrere Einzelinstanzen im gleichen Subnetz bereitgestellt werden können.


## <a name="key-capabilities-of-instance-pools"></a>Wichtige Funktionen von Instanzenpools

Instanzenpools bieten die folgenden Vorteile:

1. Möglichkeit zum Hosten von 2-V-Kern-Instanzen. *\*Nur für Instanzen in Instanzenpools*.
2. Vorhersagbare und schnelle Bereitstellungszeit von Instanzen (maximal 5 Minuten)
3. Minimale Zuordnung von IP-Adressen

In der folgenden Abbildung ist ein Instanzenpool mit mehreren in einem Subnetz des virtuellen Netzwerks bereitgestellten Instanzen dargestellt.

![Instanzenpool mit mehreren Instanzen](./media/sql-database-instance-pools/instance-pools1.png)

Instanzenpools ermöglichen die Bereitstellung mehrerer Instanzen auf demselben virtuellen Computer, wobei die Computegröße des virtuellen Computers auf der Gesamtanzahl der für den Pool zugeordneten virtuellen Kerne basiert. Diese Architektur ermöglicht die *Partitionierung* des virtuellen Computers in mehrere Instanzen in jeder unterstützten Größe, einschließlich 2-V-Kern-Instanzen (2-V-Kern-Instanzen sind nur für Instanzen in Pools verfügbar).

Verwaltungsvorgänge für Instanzen in einem Pool sind nach der ersten Bereitstellung des Pools deutlich schneller. Diese Vorgänge werden schneller durchgeführt, da die Bereitstellung oder Erweiterung eines [virtuellen Clusters](sql-database-managed-instance-connectivity-architecture.md#high-level-connectivity-architecture) (dedizierte Gruppe virtueller Computer) nicht Teil der Bereitstellung der verwalteten Instanz sind.

Da alle Instanzen in einem Pool denselben virtuellen Computer gemeinsam verwenden, hängt die gesamte IP-Zuordnung nicht von der Anzahl der bereitgestellten Instanzen ab. Dies eignet sich für die Bereitstellung in Subnetzen mit einem begrenzten IP-Adressbereich.

Jeder Pool verfügt über eine festgelegte IP-Zuordnung von nur neun IP-Adressen (ohne die fünf IP-Adressen im Subnetz, die für die Anforderungen des Subnetzes reserviert sind). Weitere Informationen finden Sie unter den [Anforderungen für die Subnetzgröße für Einzelinstanzen](sql-database-managed-instance-determine-size-vnet-subnet.md).

## <a name="application-scenarios-for-instance-pools"></a>Anwendungsszenarien für Instanzenpools

Die folgende Liste enthält die wichtigsten Anwendungsfälle, in denen Instanzenpools berücksichtigt werden sollten:

- Migration *einer Gruppe von SQL-Instanzen* zur gleichen Zeit, wobei der Großteil der Instanzen eine geringere Größe aufweist (z. B. 2 oder 4 virtuelle Kerne).
- Szenarien, bei denen die *vorhersagbare und schnelle Erstellung oder Skalierung von Instanzen* wichtig ist. Beispielsweise die Bereitstellung eines neuen Mandanten in einer mehrinstanzenfähigen SaaS-Anwendungsumgebung, in der Funktionen auf Instanzebene erforderlich sind.
- Szenarien, bei denen ein *Fixkosten-* oder *Ausgabenlimit* wichtig ist. Beispielsweise die Ausführung freigegebener Dev/Test- oder Demoumgebungen mit einer festen (oder selten wechselnden) Größe, in denen Sie bei Bedarf regelmäßig verwaltete Instanzen bereitstellen.
- Szenarien, bei denen die *minimale Zuordnung von IP-Adressen* in einem Subnetz des VNET wichtig ist. Ein virtueller Computer wird von allen Instanzen in einem Pool gemeinsam genutzt, sodass die Anzahl der zugeordneten IP-Adressen niedriger ist als bei Einzelinstanzen.


## <a name="architecture-of-instance-pools"></a>Architektur von Instanzenpools

Instanzenpools haben eine ähnliche Architektur wie reguläre verwaltete Instanzen (*Einzelinstanzen*). Zur Unterstützung von [Bereitstellungen in virtuellen Azure-Netzwerken (VNETs)](../virtual-network/virtual-network-for-azure-services.md#deploy-azure-services-into-virtual-networks) und zur Gewährleistung der Isolation und Sicherheit für Kunden basieren Instanzenpools außerdem auf [virtuellen Clustern](sql-database-managed-instance-connectivity-architecture.md#high-level-connectivity-architecture). Virtuelle Cluster stellen eine dedizierte Gruppe isolierter virtueller Computer dar, die im Subnetz des virtuellen Netzwerks des Kunden bereitgestellt werden.

Der Hauptunterschied zwischen den beiden Bereitstellungsmodellen besteht darin, dass Instanzenpools Bereitstellungen mehrerer SQL Server-Prozesse auf dem gleichen VM-Knoten ermöglichen, die über [Windows-Auftragsobjekte](https://docs.microsoft.com/windows/desktop/ProcThread/job-objects) ressourcengesteuert werden, während Einzelinstanzen sich immer allein auf einem VM-Knoten befinden.

In der folgenden Abbildung sind ein Instanzenpool und zwei Einzelinstanzen, die im gleichen Subnetz bereitgestellt sind, sowie die wesentlichen Architekturdetails für beide Bereitstellungsmodelle dargestellt:

![Instanzenpool und zwei Einzelinstanzen](./media/sql-database-instance-pools/instance-pools2.png)

Für jeden Instanzenpool wird ein untergeordneter separater virtueller Cluster erstellt. Instanzen innerhalb eines Pools und Einzelinstanzen, die im gleichen Subnetz bereitgestellt werden, verwenden die den SQL Server-Prozessen und Gatewaykomponenten zugeordneten Computeressourcen nicht gemeinsam, sodass die Vorhersagbarkeit der Leistung sichergestellt ist.

## <a name="instance-pools-resource-limitations"></a>Ressourceneinschränkungen für Instanzenpools

In Bezug auf Instanzenpools und Instanzen innerhalb von Pools bestehen verschiedene Ressourceneinschränkungen:

- Instanzenpools sind nur für Gen5-Hardware verfügbar.
- Instanzen innerhalb eines Pools verfügen über dedizierte CPU- und RAM-Werte, sodass die aggregierte Anzahl von virtuellen Kernen in allen Instanzen kleiner oder gleich der Anzahl von virtuellen Kernen sein muss, die dem Pool zugeordnet sind.
- Alle [Limits auf Instanzebene](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) gelten für die innerhalb eines Pools erstellten Instanzen.
- Neben den Limits auf Instanzebene gelten auch zwei Limits *auf Ebene des Instanzenpools*:
  - Gesamtspeichergröße pro Pool (8 TB)
  - Gesamtanzahl der Datenbanken pro Pool (100)

Die Gesamtspeicherzuordnung und die Gesamtanzahl der Datenbanken in allen Instanzen muss kleiner oder gleich der über Instanzenpools verfügbar gemachten Limits sein.

- Instanzenpools unterstützen 8, 16, 24, 32, 40, 64 und 80 virtuelle Kerne.
- Verwaltete Instanzen in Pools unterstützen 2, 4, 8, 16, 24, 32, 40, 64 und 80 virtuelle Kerne.
- Verwaltete Instanzen in Pools unterstützen Speichergrößen zwischen 32 GB und 8 TB, mit folgenden Ausnahmen:
  - 2-V-Kern-Instanzen unterstützen Größen zwischen 32 GB und 640 GB.
  - 4-V-Kern-Instanzen unterstützen Größen zwischen 32 GB und 2 TB.

Die [Dienstebeneneigenschaft](sql-database-managed-instance-resource-limits.md#service-tier-characteristics) ist der Instanzenpoolressource zugeordnet, sodass die Dienstebene aller Instanzen in einem Pool mit der Dienstebene des Pools identisch sein muss. Derzeit ist nur die Dienstebene „Universell“ verfügbar (siehe dazu den folgenden Abschnitt zu den Einschränkungen in der aktuellen Vorschauversion).

### <a name="public-preview-limitations"></a>Einschränkungen der öffentlichen Vorschauversion

Für die Public Preview gelten die folgenden Einschränkungen:

- Derzeit ist nur die Dienstebene „Universell“ verfügbar.
- Instanzenpools können in der Public Preview nicht skaliert werden, sodass eine sorgfältige Kapazitätsplanung vor der Bereitstellung wichtig ist.
- Die Erstellung und die Konfiguration von Instanzenpools werden im Azure-Portal noch nicht unterstützt. Alle Vorgänge für Instanzenpools werden nur über PowerShell unterstützt. Auch die erste Bereitstellung von Instanzen in einem vorab erstellten Pool wird nur über PowerShell unterstützt. Nach der Bereitstellung in einem Pool können verwaltete Instanzen über das Azure-Portal aktualisiert werden.
- Verwaltete Instanzen, die außerhalb des Pools erstellt werden, können nicht in einen vorhandenen Pool verschoben werden, und Instanzen, die in einem Pool erstellt werden, können nicht als Einzelinstanzen aus dem Pool und nicht in einen anderen Pool verschoben werden.
- Preise für reservierte Instanzen (enthaltene Lizenz oder mit Azure-Hybridvorteil) sind nicht verfügbar.

## <a name="sql-features-supported"></a>Unterstützte SQL-Features

Bei in Pools erstellten Instanzen werden die gleichen [Kompatibilitätsgrade und Funktionen wie bei verwalteten Einzelinstanzen](sql-database-managed-instance.md#sql-features-supported) unterstützt.

Jede in einem Pool bereitgestellte verwaltete Instanz verfügt über eine separate Instanz des SQL-Agents.

Optionale Funktionen oder Funktionen, für die spezifische Werte ausgewählt werden müssen (z. B. Sortierung auf Instanzebene, Zeitzone, öffentlicher Endpunkt für Datenverkehr, Failovergruppen), werden auf Instanzebene konfiguriert und können für jede Instanz in einem Pool unterschiedlich sein.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Obwohl verwaltete Instanzen innerhalb von Pools dedizierte V-Kern- und RAM-Werte aufweisen, nutzen sie den lokalen Datenträger (für die tempdb-Auslastung) und Netzwerkressourcen gemeinsam. Es ist nicht wahrscheinlich, jedoch möglich, dass der *Noisy Neighbor*-Effekt eintritt, wenn mehrere Instanzen zur gleichen Zeit einen hohen Ressourcenverbrauch haben. Wenn dieses Problem auftritt, können Sie die betreffenden Instanzen in einem größeren Pool oder als Einzelinstanzen bereitstellen.

## <a name="security-considerations"></a>Sicherheitshinweise

Da die in einem Pool bereitgestellten Instanzen denselben virtuellen Computer gemeinsam verwenden, empfiehlt es sich, Funktionen zu deaktivieren, die höhere Sicherheitsrisiken mit sich bringen, oder die Zugriffsberechtigungen für diese Funktionen streng zu kontrollieren. Dies betrifft beispielsweise die CLR-Integration, die native Sicherung und Wiederherstellung oder Datenbank-E-Mails.

## <a name="instance-pool-support-requests"></a>Supportanfragen für Instanzenpools

Sie können Supportanfragen für Instanzenpools im [Azure-Portal](https://portal.azure.com) erstellen und verwalten.

Wenn Probleme bei der Bereitstellung von Instanzenpools (Erstellung oder Löschung) auftreten, stellen Sie sicher, dass Sie **Instanzenpools** im Feld **Problemuntertyp** angeben.

![Supportanfrage für Instanzenpools](./media/sql-database-instance-pools/support-request.png)

Wenn Probleme im Zusammenhang mit Einzelinstanzen oder Datenbanken in einem Pool auftreten, empfiehlt es sich, ein reguläres Supportticket für verwaltete Azure SQL-Datenbank-Instanzen zu erstellen.

Zur Erstellung größerer Bereitstellungen von verwalteten Instanzen (mit oder ohne Instanzenpools) benötigen Sie möglicherweise ein größeres regionales Kontingent. Verwenden Sie die [Standardprozedur für verwaltete Instanzen zum Anfordern eines größeren Kontingents](sql-database-managed-instance-resource-limits.md#obtaining-a-larger-quota-for-sql-managed-instance). Beachten Sie dabei jedoch Folgendes: Bei Verwendung von Instanzenpools wird mit der Bereitstellungslogik der V-Kern-Gesamtverbrauch *auf Poolebene* mit Ihrem Kontingent verglichen, um zu ermitteln, ob Sie neue Ressourcen erstellen können, ohne Ihr Kontingent weiter erhöhen zu müssen.

## <a name="instance-pool-billing"></a>Abrechnung für Instanzenpools

Mit Instanzenpools lassen sich Compute- und Speicherressourcen unabhängig voneinander skalieren. Kunden zahlen für die mit der Poolressource verbundenen Computeressourcen gemessen in virtuellen Kernen und für die mit jeder Instanz verbundenen Speicherressourcen gemessen in Gigabyte (die ersten 32 GB sind für jede Instanz kostenlos).

Der V-Kern-Preis für einen Pool wird unabhängig davon berechnet, wie viele Instanzen im Pool bereitgestellt werden.

Für den Computepreis (gemessen in virtuellen Kernen) sind zwei Preisoptionen verfügbar:

  1. *Enthaltene Lizenz:* Anwendung vorhandener SQL Server-Lizenzen mit Software Assurance
  2. *Azure Hybrid Benefit* (Azure-Hybridvorteil): Reduzierter Preis einschließlich Azure-Hybridvorteil für SQL Server. Kunden können diesen Tarif nutzen, indem sie ihre vorhandenen SQL Server-Lizenzen mit Software Assurance verwenden. Informationen zur Berechtigung und weitere Details finden Sie unter [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-benefit/).

Für einzelne Instanzen in einem Pool können keine unterschiedlichen Preisoptionen festgelegt werden. Für alle Instanzen im übergeordneten Pool muss jeweils entweder die Preisoption „Lizenz enthalten“ oder „Azure-Hybridvorteil“ festgelegt sein. Das Lizenzmodell für den Pool kann nach der Erstellung des Pools geändert werden.

> [!IMPORTANT]
> Wenn Sie für eine Instanz ein Lizenzmodell angeben, das von dem des Pools abweicht, wird die für den Pool festgelegte Preisoption verwendet und der Wert auf Instanzebene ignoriert.

Wenn Sie Instanzenpools in [Abonnements mit Berechtigung für Dev/Test](https://azure.microsoft.com/pricing/dev-test/) erstellen, erhalten Sie automatisch vergünstigte Preise von bis zu 55 % für die verwaltete Azure SQL-Instanz.

Ausführliche Informationen zu den Preisen für Instanzenpools finden Sie im Abschnitt *Instanzenpools* auf der Seite mit den [Preisen für verwaltete Instanzen](https://azure.microsoft.com/pricing/details/sql-database/managed/).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu den ersten Schritten mit Instanzenpools finden Sie unter [Schrittanleitung zu Azure SQL-Datenbank-Instanzenpools](sql-database-instance-pools-how-to.md).
- Im [Schnellstarthandbuch](sql-database-managed-instance-get-started.md) erfahren Sie, wie Sie Ihre erste verwaltete Instanz erstellen.
- Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](sql-database-features.md).
- Weitere Informationen zur VNET-Konfiguration finden Sie unter [Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connectivity-architecture.md).
- Eine Schnellstartanleitung, in der eine verwaltete Instanz erstellt und eine Datenbank von einer Sicherungsdatei wiederhergestellt wird, finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started.md).
- Ein Tutorial mit Verwendung des Azure Database Migration Service (DMS) für die Migration finden Sie unter [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](../dms/tutorial-sql-server-to-managed-instance.md).
- Informationen zur erweiterten Überwachung der Datenbankleistung verwalteter Instanzen mit integrierten Problembehandlungsfunktionen finden Sie unter [Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse (Vorschauversion)](../azure-monitor/insights/azure-sql.md).
- Preisinformationen finden Sie unter [Preise – verwaltete Azure SQL-Datenbank-Instanzen ](https://azure.microsoft.com/pricing/details/sql-database/managed/).