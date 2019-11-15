---
title: Skalieren von Ressourcen
description: In diesem Artikel wird beschrieben, wie Sie Ihre Datenbank skalieren, indem Sie zugeordnete Ressourcen hinzufügen oder entfernen.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnik, carlrab
ms.date: 06/25/2019
ms.openlocfilehash: 678096037da69bbddf95933e3fdf988f540ca4a6
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73819834"
---
# <a name="dynamically-scale-database-resources-with-minimal-downtime"></a>Dynamisches Skalieren von Datenbankressourcen bei minimaler Downtime

Mit Azure SQL-Datenbank können Sie Ihrer Datenbank bei minimaler [Downtime](https://azure.microsoft.com/support/legal/sla/sql-database/v1_2/) dynamisch zusätzliche Ressourcen hinzufügen. Allerdings wird die Datenbankverbindung aufgrund einer Umschaltzeit kurzzeitig unterbrochen, was sich durch Wiederholungslogik reduzieren lässt.

## <a name="overview"></a>Übersicht

Wenn die Nachfrage nach Ihrer App von einer Handvoll von Geräten und Kunden in den Millionenbereich steigt, kann Azure SQL-Datenbank im laufenden Betrieb mit minimaler Ausfallzeit skaliert werden. Die Skalierbarkeit ist eines der wichtigsten Merkmale von PaaS, mit dem Sie Ihrem Dienst bei Bedarf dynamisch weitere Ressourcen hinzufügen können. Mit Azure SQL-Datenbank können Sie Ressourcen (CPU-Leistung, Arbeitsspeicher, E/A-Durchsatz und Speicher), die Ihren Datenbanken zugeordnet sind, leicht ändern.

Sie können Leistungsprobleme beheben, die aufgrund der vermehrten Nutzung Ihrer Anwendung auftreten und sich per Indizierung oder mit Methoden zum Umschreiben von Abfragen nicht beseitigen lassen. Durch das Hinzufügen von weiteren Ressourcen können Sie schnell reagieren, wenn Ihre Datenbank die derzeitigen Ressourcenlimits erreicht und eine höhere Leistung benötigt, um die eingehende Workload verarbeiten zu können. Mit Azure SQL-Datenbank können Sie die Ressourcen auch zentral herunterskalieren, wenn sie nicht benötigt werden, um die Kosten zu senken.

Sie müssen sich nicht mit dem Kauf von Hardware und der Änderung der zugrunde liegenden Infrastruktur befassen. Sie können die Datenbank skalieren, indem Sie einfach im Azure-Portal einen Schieberegler verwenden.

![Skalieren der Datenbankleistung](media/sql-database-scalability/scale-performance.svg)

Azure SQL-Datenbank bietet das [DTU-basierte Kaufmodell](sql-database-service-tiers-dtu.md) sowie das [vCore-basierte Kaufmodell](sql-database-service-tiers-vcore.md).

- Das [DTU-basierte Kaufmodell](sql-database-service-tiers-dtu.md) bietet zur Unterstützung von einfachen bis hin zu komplexen Datenbankworkloads eine Mischung aus Compute-, Arbeitsspeicher- und E/A-Ressourcen auf drei Dienstebenen: Basic, Standard und Premium. Leistungsstufen auf den einzelnen Ebenen bieten unterschiedliche Ressourcenzusammenstellungen, durch zusätzliche Speicherressourcen ergänzt werden können.
- Beim [vCore-basierten Kaufmodell](sql-database-service-tiers-vcore.md) können Sie die Anzahl virtueller Kerne, die Arbeitsspeichermenge sowie Menge und Geschwindigkeit des Speichers auswählen. Dieses Kaufmodell bietet drei Dienstebenen: „Universell“, „Unternehmenskritisch“ und „Hyperscale“.

Sie können zu einer geringen monatlichen Gebühr Ihre erste App in einer kleinen Einzeldatenbank in den Dienstebenen „Basic“, „Standard“ oder „Universell“ erstellen und diese dann jederzeit manuell oder programmgesteuert in die Dienstebenen „Premium“ oder „Unternehmenskritisch“ ändern, um die Anforderungen Ihrer Lösung zu erfüllen. Die Leistungsanpassung ist möglich, ohne dass es für die App oder für Ihre Kunden zu Ausfallzeiten kommt. Dank der dynamischen Skalierbarkeit kann Ihre Datenbank in transparenter Form auf sich schnell ändernde Ressourcenanforderungen reagieren, und Sie zahlen nur für die Ressourcen, die Sie jeweils benötigen.

> [!NOTE]
> Dynamische Skalierbarkeit ist nicht dasselbe wie automatische Skalierung. Bei der automatischen Skalierung wird ein Dienst automatisch auf der Grundlage von Kriterien skaliert. Die dynamische Skalierbarkeit ermöglicht dagegen eine manuelle Skalierung mit minimaler Downtime.

Eine einzelne Azure SQL-Datenbank unterstützt die manuelle dynamische Skalierbarkeit, aber keine automatischen Skalierung. Ein höheres Maß an *Automatisierung* lässt sich bei Bedarf mithilfe von Pools für elastische Datenbanken erzielen, die die gemeinsame Nutzung eines Ressourcenpools auf der Grundlage individueller Datenbankanforderungen ermöglichen.
Es gibt allerdings auch Skripts, die Sie bei der Automatisierung der Skalierbarkeit für eine einzelne Azure SQL-Datenbank unterstützen. Ein Beispiel finden Sie unter [Überwachen und Skalieren einer einzelnen SQL­-Datenbank mit PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).

Sie können [DTU-Dienstebenen](sql-database-service-tiers-dtu.md) oder [V-Kern-Merkmale](sql-database-vcore-resource-limits-single-databases.md) jederzeit ändern und die Ausfallzeiten für Ihre Anwendung dabei gering halten (im Durchschnitt meist unter vier Sekunden). Für viele Unternehmen und Apps genügt es, wenn Datenbanken erstellt werden können und sich die Leistung nach oben oder unten anpassen lässt – insbesondere, wenn die Nutzungsmuster relativ gut vorhersagbar sind. Bei unvorhersagbaren Nutzungsmustern kann es jedoch schwer sein, die Kosten und Ihr Geschäftsmodell zu verwalten. In diesem Szenario verwenden Sie einen Pool für elastische Datenbanken mit einer bestimmten Anzahl von eDTUs, die von mehreren Datenbanken im Pool gemeinsam genutzt werden.

![Einführung in SQL-Datenbank: Einzeldatenbank-DTUs nach Dienst- und Leistungsebene](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Alle drei Arten von Azure SQL-Datenbank verfügen über Funktionen zum dynamischen Skalieren Ihrer Datenbanken:

- In einer [Einzeldatenbank](sql-database-single-database-scale.md) können Sie entweder [DTU](sql-database-dtu-resource-limits-single-databases.md)- oder [V-Kern](sql-database-vcore-resource-limits-single-databases.md)-Modelle nutzen, um die maximale Menge von Ressourcen zu definieren, die den einzelnen Datenbanken zugewiesen werden.
- Für eine [verwaltete Instanz](sql-database-managed-instance.md) wird der [V-Kern](sql-database-managed-instance.md#vcore-based-purchasing-model)-Modus verwendet, und Sie können die maximale Anzahl von CPU-Kernen und den maximalen Speicher für Ihre Instanz definieren. Alle Datenbanken innerhalb der Instanz nutzen die der Instanz zugeordneten Ressourcen gemeinsam.
- Bei [Pools für elastische Datenbanken](sql-database-elastic-pool-scale.md) können Sie das maximale Ressourcenlimit pro Datenbankgruppe im Pool definieren.

Wenn Sie die Aktion für zentrales Hoch- oder Herunterskalieren in einer der Varianten initiieren, wird der Datenbank-Engine-Prozess neu gestartet und bei Bedarf auf einen anderen virtuellen Computer verschoben. Das Verschieben des Datenbank-Engine-Prozesses auf einen neuen virtuellen Computer ist ein **Onlineprozess**, bei dem Sie den vorhandenen Azure SQL-Datenbank-Dienst weiterhin verwenden können, während der Prozess ausgeführt wird. Sobald die Zieldatenbank-Engine vollständig initialisiert und zum Verarbeiten der Abfragen bereit ist, werden die Verbindungen [von der Quell- zur Zieldatenbank-Engine umgeleitet](sql-database-single-database-scale.md#impact-of-changing-service-tier-or-rescaling-compute-size).

> [!NOTE]
> Sie müssen mit einer kurzen Unterbrechung der Verbindung rechnen, wenn das Hoch-/Herunterskalieren abgeschlossen ist. Wenn Sie [Wiederholungslogik bei vorübergehenden Standardfehlern](sql-database-connectivity-issues.md#retry-logic-for-transient-errors) implementiert haben, bemerken Sie den Failover nicht.

## <a name="alternative-scale-methods"></a>Alternative Skalierungsmethoden

Die Skalierung von Ressourcen ist die einfachste und effektivste Möglichkeit, die Leistung Ihrer Datenbank zu verbessern, ohne entweder den Datenbank- oder Anwendungscode zu ändern. In einigen Fällen kann es sein, dass auch die höchsten Dienstebenen, Computegrößen und Leistungsoptimierungen nicht zu einer erfolgreichen und kostengünstigen Verarbeitung Ihrer Workload führen. In diesen Fällen haben Sie weitere Möglichkeiten zum Skalieren Ihrer Datenbank:

- Die [horizontale Leseskalierung](sql-database-read-scale-out.md) ist ein verfügbares Feature, bei dem Sie ein schreibgeschütztes Replikat Ihrer Daten erhalten, über das Sie anspruchsvolle schreibgeschützte Abfragen, z.B. Berichte, ausführen können. Mit dem schreibgeschützten Replikat wird Ihre schreibgeschützte Workload verarbeitet, ohne dass sich Auswirkungen auf die Ressourcenverwendung in Ihrer primären Datenbank ergeben.
- Das [Datenbank-Sharding](sql-database-elastic-scale-introduction.md) umfasst eine Reihe von Verfahren, mit denen Sie Ihre Daten in mehrere Datenbanken aufteilen und unabhängig voneinander skalieren können.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Verbesserung der Datenbankleistung durch das Ändern des Datenbankcodes finden Sie unter [Suchen und Anwenden von Empfehlungen zur Leistung](sql-database-advisor-portal.md).
- Informationen zur Optimierung Ihrer Datenbank mit integrierter Datenbank-Intelligence finden Sie unter [Automatische Optimierung](sql-database-automatic-tuning.md).
- Informationen zur horizontalen Leseskalierung im Azure SQL-Datenbank-Dienst finden Sie unter [Verwenden von schreibgeschützten Replikaten für den Lastenausgleich schreibgeschützter Abfrageworkloads](sql-database-read-scale-out.md).
- Informationen zum Datenbank-Sharding finden Sie unter [Horizontales Hochskalieren mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md).
