---
title: Unterstützte Versionen in Azure Database for MySQL
description: Beschreibung der unterstützten Versionen in Azure-Datenbank für MySQL
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 08/09/2019
ms.openlocfilehash: 28d635dc5042799790d032ef4b46bf28118cb326
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947169"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>Unterstützte Azure-Datenbank für MySQL-Serverversionen

Azure Database for MySQL wurde basierend auf [MySQL Community Edition](https://www.mysql.com/products/community/) mithilfe der InnoDB-Engine entwickelt.

MySQL verwendet das Benennungsschema „X.Y.Z“. X ist die Hauptversion, Y die Nebenversion und Z die Version zur Fehlerbehebung. Weitere Informationen zum Schema finden Sie in der [MySQL-Dokumentation](https://dev.mysql.com/doc/refman/5.7/en/which-version.html).

> [!NOTE]
> Im Dienst wird ein Gateway verwendet, um die Verbindungen an die Serverinstanzen umzuleiten. Sobald die Verbindung hergestellt ist, zeigt der MySQL-Client die im Gateway festgelegte Version von MySQL an, nicht die tatsächliche Version, die auf Ihrer MySQL-Serverinstanz ausgeführt wird. Um die Version Ihrer MySQL-Serverinstanz zu ermitteln, geben Sie den `SELECT VERSION();`-Befehl an der MySQL-Eingabeaufforderung ein.

Azure Database for MySQL unterstützt derzeit die folgenden Versionen:

## <a name="mysql-version-56"></a>MySQL-Version 5.6

Version zur Fehlerbehebung: 5.6.42

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in MySQL 5.6.42 finden Sie in den [Versionshinweisen](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-42.html) für MySQL.

## <a name="mysql-version-57"></a>MySQL-Version 5.7

Version zur Fehlerbehebung: 5.7.24

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in MySQL 5.7.24 finden Sie in den [Versionshinweisen](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-24.html) für MySQL.

## <a name="mysql-version-80"></a>MySQL, Version 8.0

> [!IMPORTANT]
> MySQL 8.0 befindet sich derzeit in der Vorschauphase.

Version zur Fehlerbehebung: 8.0.15

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in MySQL 8.0.15 finden Sie in den [Versionshinweisen](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) zu MySQL.

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Das Patchen von Versionsupdates zur Fehlerbehebung wird vom Dienst automatisch verwaltet. Beispiel: 5.7.20 bis 5.7.21.  

Zurzeit werden Upgrades von Neben- und Hauptversionen nicht unterstützt. Ein Upgrade von MySQL 5.6 auf MySQL 5.7 wird beispielsweise nicht unterstützt. Wenn Sie von 5.6 auf 5.7 aktualisieren möchten, führen Sie eine [Sicherung und Wiederherstellung](./concepts-migrate-dump-restore.md) auf einem Server aus, der mit der neuen Modulversion erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrer **Dienstebene** finden Sie unter [Dienstebenen](./concepts-pricing-tiers.md).
