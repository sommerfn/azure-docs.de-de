---
title: Unterstützte FHIR-Features in Azure – Azure API for FHIR
description: In diesem Artikel wird erläutert, welche Features der FHIR-Spezifikation in Azure API for FHIR implementiert sind.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: reference
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 74a17c4a433ebe1c1107230739086375fe165ed9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "66753727"
---
# <a name="features"></a>Features

Azure API for FHIR bietet eine vollständig verwaltete Bereitstellung des Microsoft FHIR-Servers für Azure. Der Server ist eine Implementierung des [FHIR](https://hl7.org/fhir)-Standards. In diesem Dokument sind die wichtigsten Features des FHIR-Servers aufgeführt.

## <a name="fhir-version"></a>FHIR-Version

Neueste unterstützte Version (nur auf OSS-Implementierung verfügbar): `4.0.0`

Frühere Versionen, die derzeit ebenfalls unterstützt werden, umfassen (sowohl auf PaaS- als auch OSS-Implementierungen verfügbar): `3.0.1`

## <a name="rest-api"></a>REST-API

| API                            | Unterstützt | Comment |
|--------------------------------|-----------|---------|
| read                           | Ja       |         |
| vread                          | Ja       |         |
| aktualisieren                         | Ja       |         |
| Update mit optimistischer Sperre | Ja       |         |
| update (bedingt)           | Nein        |         |
| patch                          | Nein        |         |
| delete                         | Ja       |         |
| delete (bedingt)           | Nein        |         |
| create                         | Ja       | Unterstützung für POST/PUT |
| create (bedingt)           | Nein        |         |
| search                         | Teilweise   | Siehe unten |
| capabilities                   | Ja       |         |
| Batch                          | Nein        |         |
| transaction                    | Nein        |         |
| history                        | Ja       |         |
| paging                         | Teilweise   | `self` und `next` werden unterstützt |
| intermediaries                 | Nein        |         |

## <a name="search"></a>Suchen,

Es werden alle Suchparametertypen unterstützt. Verkettete Parameter und umgekehrte Verkettung werden *nicht* unterstützt.

| Suchparametertyp | Unterstützt | Comment |
|-----------------------|-----------|---------|
| Number                | Ja       |         |
| Date/DateTime         | Ja       |         |
| string                | Ja       |         |
| Tokenverschlüsselung                 | Ja       |         |
| Verweis             | Ja       |         |
| Composite             | Ja       |         |
| Menge              | Ja       | Problem [#103](https://github.com/Microsoft/fhir-server/issues/103) |
| URI                   | Ja       |         |


| Modifizierer             | Unterstützt | Comment |
|-----------------------|-----------|---------|
|`:missing`             | Ja       |         |
|`:exact`               | Ja       |         |
|`:contains`            | Ja       |         |
|`:text`                | Ja       |         |
|`:in` (Token)          | Nein        |         |
|`:below` (Token)       | Nein        |         |
|`:above` (Token)       | Nein        |         |
|`:not-in` (Token)      | Nein        |         |
|`:[type]` (Referenz)  | Nein        |         |
|`:below` (URI)         | Ja       |         |
|`:above` (URI)         | Nein        | Problem [#158](https://github.com/Microsoft/fhir-server/issues/158) |

| Allgemeiner Suchparameter | Unterstützt | Comment |
|-------------------------| ----------|---------|
| `_id`                   | Ja       |         |
| `_lastUpdated`          | Ja       |         |
| `_tag`                  | Ja       |         |
| `_profile`              | Ja       |         |
| `_security`             | Ja       |         |
| `_text`                 | Nein        |         |
| `_content`              | Nein        |         |
| `_list`                 | Nein        |         |
| `_has`                  | Nein        |         |
| `_type`                 | Ja       |         |
| `_query`                | Nein        |         |

| Suchvorgänge       | Unterstützt | Comment |
|-------------------------|-----------|---------|
| `_filter`               | Nein        |         |
| `_sort`                 | Nein        |         |
| `_score`                | Nein        |         |
| `_count`                | Ja       |         |
| `_summary`              | Teilweise   | `_summary=count` wird unterstützt |
| `_include`              | Nein        |         |
| `_revinclude`           | Nein        |         |
| `_contained`            | Nein        |         |
| `_elements`             | Nein        |         |

## <a name="persistence"></a>Persistenz

Der Microsoft FHIR-Server verfügt über ein austauschbares Persistenzmodul (siehe [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence)).

Derzeit umfasst der Open Source-Code des FHIR-Servers eine Implementierung für [Azure Cosmos DB](../cosmos-db/index-overview.md).

Cosmos DB ist eine global verteilte Datenbank, die mehrere Modelle (SQL-API, MongoDB-API, usw.) unterstützt. Es unterstützt verschiedene [Konsistenzebenen](../cosmos-db/consistency-levels.md). Die standardmäßige Bereitstellungsvorlage konfiguriert den FHIR-Server mit `Strong`-Konsistenz, aber die Konsistenzrichtlinie kann auf Anforderungsbasis unter Verwendung des Anforderungsheaders `x-ms-consistency-level` geändert (allgemein gelockert) werden.

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Der FHIR-Server verwendet [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) für die Zugriffssteuerung. Insbesondere wird die rollenbasierte Zugriffssteuerung (RBAC) erzwungen, wenn der Konfigurationsparameter `FhirServer:Security:Enabled` auf `true` festgelegt ist, und bei allen Anforderungen (außer `/metadata`) an den FHIR-Server muss der Header der Anforderung `Authorization` auf `Bearer <TOKEN>` eingestellt sein. Das Token muss eine oder mehrere Rollen gemäß der Definition im `roles`-Anspruch enthalten. Eine Anforderung ist zulässig, wenn der Token eine Rolle enthält, die die angegebene Aktion für die angegebene Ressource gestattet.

Derzeit werden die zulässigen Aktionen für eine bestimmte Rolle *global* auf die API angewendet.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zu den unterstützten FHIR-Features in Azure API for FHIR erhalten. Als Nächstes wird eine FHIR-API in Azure bereitgestellt.
 
>[!div class="nextstepaction"]
>[Bereitstellen von Open Source-FHIR-Servern](fhir-oss-powershell-quickstart.md)