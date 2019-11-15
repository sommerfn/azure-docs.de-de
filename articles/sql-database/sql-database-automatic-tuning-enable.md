---
title: Aktivieren der automatischen Optimierung
description: Sie können ganz einfach die automatische Optimierung für Ihre Azure SQL-Datenbank aktivieren.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 01/25/2019
ms.openlocfilehash: 0abf4bb015be52a10178423a566433b87127a167
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73821913"
---
# <a name="enable-automatic-tuning-to-monitor-queries-and-improve-workload-performance"></a>Aktivieren der automatischen Optimierung zum Überwachen von Abfragen und Verbessern der Workloadleistung

Azure SQL-Datenbank ist ein automatisch verwalteter Datendienst, der kontinuierlich Ihre Abfragen überwacht und Ihnen mitteilt, wie Sie die Leistung Ihrer Workload verbessern können. Sie können Empfehlungen prüfen und manuell anwenden oder Azure SQL-Datenbank die Maßnahmen automatisch anwenden lassen. Letzteres wird als **automatischer Optimierungsmodus** bezeichnet.

Die automatische Optimierung kann über das [Azure-Portal](sql-database-automatic-tuning-enable.md#azure-portal), [REST-API](sql-database-automatic-tuning-enable.md#rest-api)-Aufrufe und [T-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current)-Befehle auf Server- oder Datenbankebene aktiviert werden.

> [!NOTE]
> Für die verwaltete Instanz kann die unterstützte Option FORCE_LAST_GOOD_PLAN nur über [T-SQL](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management) konfiguriert werden. In diesem Artikel beschriebene portalbasierte Konfiguration und Optionen zur automatischen Indexoptimierung gelten nicht für die verwaltete Instanz.

> [!NOTE]
> Die Vorlage zum Konfigurieren der Optionen zur automatischen Optimierung über ARM (Azure Resource Manager) wird derzeit nicht unterstützt.

## <a name="enable-automatic-tuning-on-server"></a>Aktivieren der automatischen Optimierung für den Server

Auf Serverebene kann auf Wunsch die Konfiguration der automatischen Optimierung von „Azure-Standardwerte“ geerbt werden. In den Azure-Standardwerten sind FORCE_LAST_GOOD_PLAN und CREATE_INDEX aktiviert und DROP_INDEX deaktiviert.

### <a name="azure-portal"></a>Azure-Portal

Wenn Sie die automatische Optimierung für den logischen Azure SQL-Datenbank-**Server** aktivieren möchten, navigieren Sie im Azure-Portal zum entsprechenden Server, und aktivieren Sie im Menü die Option **Automatische Optimierung**.

![Server](./media/sql-database-automatic-tuning-enable/server.png)

> [!NOTE]
> Beachten Sie, dass die Option **DROP_INDEX** zurzeit nicht kompatibel mit Anwendungen ist, die Partitionswechsel und Indexhinweise verwenden. In diesen Fällen sollte sie nicht aktiviert werden. Das Löschen nicht verwendeter Indizes wird bei den Dienstebenen „Premium“ und „Unternehmenskritisch“ nicht unterstützt.
>

Wählen Sie die gewünschten Optionen für die automatische Optimierung und anschließend **Übernehmen** aus.

Die Optionen für die automatische Optimierung auf einem Server werden auf alle Datenbanken auf diesem Server angewendet. Standardmäßig erben alle Datenbanken die Konfiguration von ihrem übergeordneten Server. Dies kann jedoch außer Kraft gesetzt und einzeln für jede Datenbank angegeben werden.

### <a name="rest-api"></a>REST-API

Weitere Informationen darüber, wie die automatische Optimierung mithilfe der REST-API auf einem Server aktiviert werden kann, finden Sie unter [Automatische Serveroptimierung](https://docs.microsoft.com/rest/api/sql/serverautomatictuning).

## <a name="enable-automatic-tuning-on-an-individual-database"></a>Aktivieren der automatischen Optimierung für eine einzelne Datenbank

Über Azure SQL-Datenbank können Sie die Konfiguration der automatischen Optimierung für einzelne Datenbanken angeben. Auf Datenbankebene kann auf Wunsch die Konfiguration der automatischen Optimierung vom übergeordneten Server geerbt („Azure-Standardwerte“) oder nicht geerbt werden. Die Azure-Standardwerte sind wie folgt festgelegt: FORCE_LAST_GOOD_PLAN aktiviert, CREATE_INDEX aktiviert und DROP_INDEX deaktiviert.

> [!TIP]
> Es empfiehlt sich im Allgemeinen, die automatische Optimierung auf **Serverebene** zu verwalten, damit für jede Datenbank automatisch die gleichen Konfigurationseinstellungen angewendet werden können. Konfigurieren Sie die automatische Optimierung für eine einzelne Datenbank nur dann, wenn diese Datenbank andere Einstellungen haben muss als die anderen Datenbanken, die ihre Einstellungen vom gleichen Server erben.
>

### <a name="azure-portal"></a>Azure-Portal

Wenn Sie die automatische Optimierung für eine **einzelne Datenbank** aktivieren möchten, navigieren Sie im Azure-Portal zur entsprechenden Datenbank, und wählen Sie **Automatische Optimierung** aus.

Einzelne Einstellungen für die automatische Optimierung können für jede Datenbank separat konfiguriert werden. Sie können manuell eine einzelne automatische Optimierungsoption konfigurieren oder angeben, dass eine Option die Einstellungen vom Server erbt.

![Datenbank](./media/sql-database-automatic-tuning-enable/database.png)

Beachten Sie, dass die Option DROP_INDEX zurzeit nicht kompatibel mit Anwendungen ist, die Partitionswechsel und Indexhinweise verwenden. In diesen Fällen sollte sie nicht aktiviert werden.

Klicken Sie nach der Auswahl der gewünschten Konfiguration auf **Übernehmen**.

### <a name="rest-api"></a>REST-API

Weitere Informationen darüber, wie die automatische Optimierung mithilfe der REST-API in einer Einzeldatenbank aktiviert werden kann, finden Sie unter [Automatische Serveroptimierung](https://docs.microsoft.com/rest/api/sql/databaseautomatictuning).

### <a name="t-sql"></a>T-SQL

Wenn Sie die automatische Optimierung für eine einzelne Datenbank mithilfe von T-SQL aktivieren möchten, stellen Sie eine Verbindung mit der Datenbank her, und führen Sie die folgende Abfrage aus:

```SQL
ALTER DATABASE current SET AUTOMATIC_TUNING = AUTO | INHERIT | CUSTOM
```

Wenn Sie die automatische Optimierung auf „AUTO“ festlegen, werden die Azure-Standardwerte verwendet. Bei Verwendung von „INHERIT“ wird die Konfiguration der automatischen Optimierung vom übergeordneten Server geerbt. Bei Verwendung von „CUSTOM“ muss die automatische Optimierung manuell konfiguriert werden.

Wenn Sie über T-SQL einzelne Optionen der automatischen Optimierung konfigurieren möchten, stellen Sie eine Verbindung mit der Datenbank her, und führen Sie eine Abfrage wie die folgende aus:

```SQL
ALTER DATABASE current SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON, CREATE_INDEX = DEFAULT, DROP_INDEX = OFF)
```

Wenn Sie eine Optimierungsoption auf „ON“ festlegen, werden ggf. geerbte Einstellungen der Datenbank überschrieben, und die entsprechende Optimierungsoption wird aktiviert. Gleiches gilt für „OFF“: Auch hier werden ggf. geerbte Einstellungen der Datenbank überschrieben, und die entsprechende Optimierungsoption wird deaktiviert. Bei Verwendung der automatischen Optimierungsoption „DEFAULT“ wird die Konfiguration aus der Einstellung für die automatische Optimierung auf Datenbankebene geerbt.  

> [!IMPORTANT]
> Im Fall einer [aktiven Georeplikation](sql-database-auto-failover-group.md) muss die automatische Optimierung nur für die primäre Datenbank konfiguriert werden. Automatisch angewandte Aktionen zur Optimierung (z.B. das Erstellen oder Löschen des Index) werden automatisch in die schreibgeschützte sekundäre Datenbank repliziert. Der Versuch, die automatischen Optimierung in der sekundären schreibgeschützten Datenbank über T-SQL zu aktivieren, führt zu einem Fehler, da für die schreibgeschützte sekundäre Datenbank keine abweichende Optimierungskonfiguration unterstützt wird.
>

Informationen zu den T-SQL-Optionen für die Konfiguration der automatischen Optimierung finden Sie unter [ALTER DATABASE SET-Optionen (Transact-SQL) für SQL-Datenbank-Server](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azuresqldb-current).

## <a name="disabled-by-the-system"></a>Vom System deaktiviert

Die automatische Optimierung überwacht alle Aktionen, die sie für die Datenbank ausführt, und stellt manchmal unter Umständen fest, dass die automatische Optimierung für die Datenbank nicht ordnungsgemäß funktioniert. In diesem Fall wird die Optimierungsoption vom System deaktiviert. Das liegt meistens daran, dass der Abfragespeicher nicht aktiviert oder für eine bestimmte Datenbank schreibgeschützt ist.

## <a name="permissions"></a>Berechtigungen

Da die automatische Optimierung eine Azure-Funktion ist, müssen Sie für deren Nutzung die integrierten RBAC-Rollen von Azure verwenden. Die ausschließliche Verwendung der SQL-Authentifizierung reicht nicht aus, um die Funktion über das Azure-Portal zu nutzen.

Zur Verwendung der automatischen Optimierung muss dem Benutzer mindestens die Berechtigung der integrierten Azure-Rolle [SQL-DB-Mitwirkender](../role-based-access-control/built-in-roles.md#sql-db-contributor) erteilt werden. Sie können auch Rollen mit höheren Berechtigungen verwenden, z.B. Mitwirkender von SQL Server, Mitwirkender und Besitzer.

## <a name="configure-automatic-tuning-e-mail-notifications"></a>Konfigurieren der automatischen Optimierung von E-Mail-Benachrichtigungen

Weitere Informationen hierzu finden Sie im Leitfaden [E-Mail-Benachrichtigungen zur automatischen Optimierung](sql-database-automatic-tuning-email-notifications.md).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zur automatischen Optimierung sowie zu ihrer Rolle bei der Verbesserung der Leistung finden Sie in [diesem Artikel](sql-database-automatic-tuning.md).
* Eine Übersicht über die Leistungsempfehlungen von Azure SQL-Datenbank finden Sie [hier](sql-database-advisor.md).
* Unter [Query Performance Insight](sql-database-query-performance.md) erfahren Sie, wie Sie die Auswirkungen Ihrer wichtigsten Abfragen auf die Leistung untersuchen können.
