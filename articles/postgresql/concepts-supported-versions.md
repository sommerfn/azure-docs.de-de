---
title: Unterstützte Versionen in Azure Database for PostgreSQL (Einzelserver)
description: Beschreibung der unterstützten Versionen in Azure Database for PostgreSQL (Einzelserver)
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/06/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: b77fd082be43b8cbdedf7cbe5875a8931eb0474a
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68837897"
---
# <a name="supported-postgresql-database-versions"></a>Unterstützte PostgreSQL-Datenbankversionen
Microsoft strebt in Azure Database for PostgreSQL (Einzelserver) die Unterstützung von n-2 Versionen der PostgreSQL-Engine an. Das bedeutet, dass die aktuelle Hauptversion (n) und die beiden vorherigen Hauptversionen (-2) unterstützt werden.

Azure Database for PostgreSQL unterstützt derzeit die folgenden Hauptversionen:

## <a name="postgresql-version-11"></a>PostgreSQL Version 11
Die aktuelle Nebenversion ist 11.4. Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-4.html).

## <a name="postgresql-version-10"></a>PostgreSQL Version 10
Die aktuelle Nebenversion ist 10.9. Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-9.html).

## <a name="postgresql-version-96"></a>PostgreSQL Version 9.6
Die aktuelle Nebenversion ist 9.6.14. Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-14.html).

## <a name="postgresql-version-95"></a>PostgreSQL Version 9.5
Die aktuelle Nebenversion ist 9.5.18. Informationen zu Verbesserungen und Fehlerbehebungen in älteren Versionen finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-18.html).

## <a name="managing-upgrades"></a>Verwalten von Upgrades
Upgrades für Nebenversionen werden von Azure Database for PostgreSQL automatisch verwaltet. 

Automatische Upgrades von Hauptversionen werden nicht unterstützt. Es erfolgt beispielsweise kein automatisches Upgrade von PostgreSQL 9.5 auf PostgreSQL 9.6. Um ein Upgrade auf die nächste Hauptversion durchzuführen, erstellen Sie ein [Datenbankspeicherabbild, und führen Sie eine Wiederherstellung](./howto-migrate-using-dump-and-restore.md) auf einem Server aus, der mit der neuen Engine-Version erstellt wurde.

### <a name="version-syntax"></a>Versionssyntax
Vor PostgreSQL-Version 10 wurde ein _Hauptversionsupgrade_ von der [PostgreSQL-Versionsrichtlinie](https://www.postgresql.org/support/versioning/) als eine Erhöhung der ersten _oder_ zweiten Versionsziffer betrachtet (z. B. wurde der Wechsel von 9.5 auf 9.6 als _Hauptversionsupgrade_ angesehen). Ab Version 10 gilt nur eine Änderung der ersten Ziffer als Hauptversionsupgrade. So ist 10.0 auf 10.1 beispielsweise ein _Nebenversionsupgrade_. Version 10 auf 11 ist ein _Hauptversionsupgrade_.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu unterstützten PostgreSQL-Erweiterungen finden Sie im [Dokument zu Erweiterungen](concepts-extensions.md).
