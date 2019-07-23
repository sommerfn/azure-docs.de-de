---
title: Unterstützte Versionen in Azure Database for MariaDB
description: Beschreibung der unterstützten Versionen in Azure Database for MariaDB
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 760cb39ea1f3980faba348c7aa1de68a66b20a8d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065719"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Unterstützte Serverversionen von Azure Database for MariaDB

Azure Database for MariaDB wurde mithilfe der InnoDB-Engine auf der Grundlage des [MariaDB-Servers](https://downloads.mariadb.org/) entwickelt, einem Open Source-Produkt. Azure Database for MariaDB unterstützt derzeit die folgende Version:

## <a name="mariadb-version-102"></a>MariaDB Version 10.2

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in MariaDB 10.2.23 finden Sie in der [Dokumentation zu MariaDB](https://mariadb.com/kb/en/library/mariadb-10223-release-notes/).

## <a name="mariadb-version-103"></a>MariaDB Version 10.3

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in MariaDB 10.3.14 finden Sie in der [Dokumentation zu MariaDB](https://mariadb.com/kb/en/library/mariadb-10314-release-notes/).

> [!NOTE]
> Im Dienst wird ein Gateway verwendet, um die Verbindungen an die Serverinstanzen umzuleiten. Sobald die Verbindung hergestellt ist, zeigt der MySQL-Client die im Gateway festgelegte Version von MariaDB an, nicht die tatsächliche Version, die auf Ihrer MariaDB-Serverinstanz ausgeführt wird. Wenn Sie die Version Ihrer MariaDB-Serverinstanz ermitteln möchten, geben Sie den `SELECT VERSION();`-Befehl an der MySQL-Eingabeaufforderung ein.

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades

Das Patchen von Updates zu Nebenversionen wird vom Dienst automatisch verwaltet.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrer **Dienstebene** finden Sie unter [Dienstebenen](./concepts-pricing-tiers.md).
