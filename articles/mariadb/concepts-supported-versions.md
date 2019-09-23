---
title: Unterstützte Versionen in Azure Database for MariaDB
description: Beschreibung der unterstützten Versionen in Azure Database for MariaDB
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/12/2019
ms.openlocfilehash: e5d1dbc8c212d4cdefb12fb740a454324d3adfa1
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70962918"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>Unterstützte Serverversionen von Azure Database for MariaDB

Azure Database for MariaDB wurde mithilfe der InnoDB-Engine auf der Grundlage von [MariaDB Server](https://downloads.mariadb.org/) entwickelt, einem Open-Source-Produkt. 

MariaDB verwendet das Benennungsschema „X.Y.Z“. X ist die Hauptversion, Y die Nebenversion und Z die Patchversion.

> [!NOTE]
> Im Dienst wird ein Gateway verwendet, um die Verbindungen an die Serverinstanzen umzuleiten. Sobald die Verbindung hergestellt ist, zeigt der MySQL-Client die im Gateway festgelegte Version von MariaDB an, nicht die tatsächliche Version, die auf Ihrer MariaDB-Serverinstanz ausgeführt wird. Wenn Sie die Version Ihrer MariaDB-Serverinstanz ermitteln möchten, verwenden Sie den Befehl `SELECT VERSION();`.

Azure Database for MariaDB unterstützt derzeit die folgende Version:

## <a name="mariadb-version-102"></a>MariaDB Version 10.2

Patchversion: 10.2.25

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in dieser Version finden Sie in der [Dokumentation zu MariaDB](https://mariadb.com/kb/en/library/mariadb-10225-release-notes/).

## <a name="mariadb-version-103"></a>MariaDB Version 10.3

Patchversion: 10.3.16

Weitere Informationen zu Verbesserungen und Fehlerbehebungen in dieser Version finden Sie in der [Dokumentation zu MariaDB](https://mariadb.com/kb/en/library/mariadb-10316-release-notes/).

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Der Dienst verwaltet Upgrades für Patchupdates automatisch. Beispiel: 10.2.21 auf 10.2.23.  

Zurzeit werden Upgrades von Neben- und Hauptversionen nicht unterstützt. Ein Upgrade von MariaDB 10.2 auf MariaDB 10.3 wird beispielsweise nicht unterstützt. Wenn Sie von 10.2 auf 10.3 aktualisieren möchten, führen Sie eine [Sicherung und Wiederherstellung](./howto-migrate-dump-restore.md) auf einem Server aus, der mit der neuen Modulversion erstellt wurde.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu bestimmten Ressourcenkontingenten und -beschränkungen basierend auf Ihrer **Dienstebene** finden Sie unter [Dienstebenen](./concepts-pricing-tiers.md).
