---
title: 'Erstellen von Benutzern in Azure Database for PostgreSQL: Hyperscale (Citus)'
description: In diesem Artikel wird beschrieben, wie Sie neue Benutzerkonten für die Interaktion mit einer Azure Database for PostgreSQL – Hyperscale (Citus) erstellen können.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 7187135b29f0a9a790c032330c73bcb1ae27229b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511239"
---
# <a name="create-users-in-azure-database-for-postgresql---hyperscale-citus"></a>Erstellen von Benutzern in Azure Database for PostgreSQL: Hyperscale (Citus)

In diesem Artikel wird beschrieben, wie Sie Benutzer in einer Hyperscale-Benutzergruppe (Citus) erstellen können. Um stattdessen mehr über Azure-Abonnementbenutzer und ihre Berechtigungen zu erfahren, lesen Sie den Artikel zur [Rollenbasierten Azure-Zugriffssteuerung (RBAC) ](../role-based-access-control/built-in-roles.md), oder informieren Sie sich darüber, [wie Rollen angepasst werden können](../role-based-access-control/custom-roles.md).

## <a name="the-server-admin-account"></a>Das Serveradministratorkonto

Eine neu erstellte Hyperscale-Servergruppe (Citus) verfügt über mehrere vordefinierte Rollen:

* Die [PostgreSQL-Standardrollen](https://www.postgresql.org/docs/current/default-roles.html)
* *azure_pg_admin*
* *postgres*
* *citus*

Der Serveradministratorbenutzer (*citus*) ist ein Mitglied der Rolle *azure_pg_admin*.
Er ist jedoch nicht Teil der Rolle *postgres* (Administrator oder superuser).  Da Hyperscale ein verwalteter PaaS-Dienst ist, ist nur Microsoft Mitglied der Administratorrolle.

Die PostgreSQL-Engine nutzt Berechtigungen zum Steuern des Zugriffs auf Datenbankobjekte, wie in der [PostgreSQL-Produktdokumentation](https://www.postgresql.org/docs/current/static/sql-createrole.html) erläutert.
In Azure Database for PostgreSQL werden dem Serveradministrator folgende Berechtigungen gewährt: LOGIN, NOSUPERUSER, INHERIT, CREATEDB, CREATEROLE, NOREPLICATION

## <a name="how-to-create-additional-users"></a>So erstellen Sie zusätzliche Benutzer

Das *citus*-Administratorkontos verfügt nicht über die Berechtigung zum Erstellen zusätzlicher Benutzer. Um einen Benutzer hinzuzufügen, verwenden Sie stattdessen das Azure-Portal.

1. Navigieren Sie zur Seite **Rollen** für Ihre Hyperscale-Servergruppe, und klicken Sie auf **+ Hinzufügen**:

   ![Die Seite „Rollen“](media/howto-hyperscale-create-users/1-role-page.png)

2. Geben Sie den Rollennamen und das Kennwort ein. Klicken Sie auf **Speichern**.

   ![Hinzufügen einer Rolle](media/howto-hyperscale-create-users/2-add-user-fields.png)

Der Benutzer wird auf dem Koordinatorknoten der Servergruppe erstellt und an alle Workerknoten weitergegeben.

## <a name="how-to-delete-a-user-or-change-their-password"></a>Löschen eines Benutzers oder Ändern seines Kennworts

Navigieren Sie zur Seite **Rollen** für Ihre Hyperscale-Servergruppe, und klicken Sie auf die Auslassungspunkte **...** neben einem Benutzer. Die Auslassungspunkte öffnen ein Menü, in dem der Benutzer gelöscht oder sein Kennwort zurückgesetzt werden kann.

   ![Bearbeiten einer Rolle](media/howto-hyperscale-create-users/edit-role.png)

Die Rolle *citus* ist privilegiert und kann nicht gelöscht werden.

## <a name="how-to-modify-privileges-for-role"></a>Ändern von Berechtigungen für eine Rolle

Neue Rollen werden häufig verwendet, um Datenbankzugriff mit eingeschränkten Berechtigungen bereitzustellen. Verwenden Sie zum Ändern von Benutzerberechtigungen PostgreSQL-Standardbefehle mithilfe eines Tools wie PgAdmin oder psql. (Weitere Informationen finden Sie im Schnellstart zu Hyperscale (Citus) unter [Herstellen einer Verbindung mit psql](quickstart-create-hyperscale-portal.md#connect-to-the-database-using-psql).)

Um z.B. *db_user* das Lesen von *mytable* zu erlauben, erteilen Sie die folgende Berechtigung:

```sql
GRANT SELECT ON mytable TO db_user;
```

Hyperscale (Citus) gibt eine GRANT-Anweisung für einzelne Tabellen über den gesamten Cluster weiter und wendet sie auf alle Workerknoten an. Systemweiten GRANTs (z.B. für alle Tabellen in einem Schema) müssen jedoch auf jedem Datumsknoten ausgeführt werden.  Verwenden Sie die Hilfsfunktion *run_command_on_workers()* :

```sql
-- applies to the coordinator node
GRANT SELECT ON ALL TABLES IN SCHEMA public TO db_user;

-- make it apply to workers as well
SELECT run_command_on_workers(
  'GRANT SELECT ON ALL TABLES IN SCHEMA public TO db_user;'
);
```

## <a name="next-steps"></a>Nächste Schritte

Öffnen Sie die Firewall für die IP-Adressen der Computer der neuen Benutzer, um ihnen das Herstellen einer Verbindung zu ermöglichen: [Erstellen und Verwalten von Firewallregeln für Hyperscale (Citus) mithilfe des Azure-Portals](howto-hyperscale-manage-firewall-using-portal.md).

Weitere Informationen zur Verwaltung von Datenbankbenutzerkonten finden Sie in der PostgreSQL-Produktdokumentation:

* [Datenbankrollen und Berechtigungen](https://www.postgresql.org/docs/current/static/user-manag.html)
* [GRANT-Syntax](https://www.postgresql.org/docs/current/static/sql-grant.html)
* [Berechtigungen](https://www.postgresql.org/docs/current/static/ddl-priv.html)
