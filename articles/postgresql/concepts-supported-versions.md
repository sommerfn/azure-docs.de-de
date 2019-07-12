---
title: Unterstützte Versionen in Azure Database for PostgreSQL (Einzelserver)
description: Beschreibung der unterstützten Versionen in Azure Database for PostgreSQL (Einzelserver)
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/11/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: c328f0dc510c1b7d36b547e6560a292f98d72f6f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448039"
---
# <a name="supported-postgresql-database-versions"></a>Unterstützte PostgreSQL-Datenbankversionen
Microsoft strebt in Azure Database for PostgreSQL (Einzelserver) die Unterstützung von n-2 Versionen der PostgreSQL-Engine an. Das bedeutet, dass die aktuelle Hauptversion (n) und die beiden vorherigen Hauptversionen (-2) unterstützt werden.

Azure-Datenbank für PostgreSQL unterstützt derzeit die folgenden Versionen:

## <a name="postgresql-version-112"></a>PostgreSQL-Version 11.2
Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-2.html).

>[!NOTE]
> Die PostgreSQL-Version 11 ist als Vorschauversion verfügbar. Unterstützung für die Erstellung über das Azure-Portal wird eingeführt, und ist in Ihrer Region möglicherweise noch nicht verfügbar. Sie können die [Azure CLI](quickstart-create-server-database-azure-cli.md) verwenden, um einen Postgres 11-Server in einer beliebigen Region zu erstellen. Beispiel: `az postgres server create -g group -n server -u username -p password -l westeurope --sku-name GP_Gen5_2 --version 11`.

## <a name="postgresql-version-107"></a>PostgreSQL-Version 10.7
Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-7.html).

## <a name="postgresql-version-9612"></a>PostgreSQL-Version 9.6.12
Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-12.html).

## <a name="postgresql-version-9516"></a>PostgreSQL-Version 9.5.16
Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-16.html).

## <a name="managing-updates-and-upgrades"></a>Verwalten von Updates und Upgrades
Patches für Nebenversionen werden von Azure Database for PostgreSQL automatisch verwaltet. Derzeit werden Upgrades von Hauptversionen nicht unterstützt. Beispielsweise wird das Upgrade von PostgreSQL 9.5 auf PostgreSQL 9.6 nicht unterstützt. Wenn Sie auf die nächste Hauptversion upgraden möchten, erstellen Sie eine Datenbank, und führen Sie für diese eine [Sicherung und Wiederherstellung](./howto-migrate-using-dump-and-restore.md) auf einem Server aus, der mit der neuen Engine-Version erstellt wurde.

> Beachten Sie, dass vor PostgreSQL-Version 10 die [PostgreSQL-Versionsrichtlinie](https://www.postgresql.org/support/versioning/) ein _Hauptversion_supgrade als eine Erhöhung der ersten _oder_ zweiten Nummer betrachtet hat (z. B. wurde 9.5 auf 9.6 als ein _Haupt_versionsupgrade betrachtet).
> Seit Version 10 gilt nur noch eine Änderung der ersten Zahl als Hauptversionsupgrade (z. B. ist 10.0 auf 10.1 ein _Neben_versionsupgrade, und 10 auf 11 ist ein _Haupt_versionsupgrade).

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Unterstützung der verschiedenen PostgreSQL-Erweiterungen finden Sie unter [PostgreSQL-Erweiterungen](concepts-extensions.md).
