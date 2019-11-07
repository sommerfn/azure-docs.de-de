---
title: Knoten in Azure Database for PostgreSQL – Hyperscale (Citus)
description: Erfahren Sie mehr über die beiden Knotentypen (Koordinator und Worker) in einer Servergruppe in Azure Database for PostgreSQL.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 5348c8bbe432cd024a8da93ca866ae4458f956e4
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73468089"
---
# <a name="nodes-in-azure-database-for-postgresql--hyperscale-citus"></a>Knoten in Azure Database for PostgreSQL – Hyperscale (Citus)

Der Hostingtyp von Hyperscale (Citus) ermöglicht es Azure Database for PostgreSQL-Servern (als „Knoten“ bezeichet), sich in einer „Shared-Nothing-Architektur“ zu koordinieren. Die Knoten in einer Servergruppe speichern zusammen mehr Daten und verwenden mehr CPU-Kerne als es auf einem einzelnen Server möglich wäre. Außerdem ermöglicht die Architektur das Skalieren der Datenbank durch Hinzufügen weiterer Knoten zur Servergruppe.

## <a name="coordinator-and-workers"></a>Koordinator und Worker

Jede Servergruppe verfügt über einen Koordinatorknoten und mehrere Worker(knoten). Anwendungen senden ihre Abfragen an den Koordinatorknoten, der sie an die relevanten Worker weiterleitet und die Ergebnisse sammelt. Anwendungen können keine direkte Verbindung mit Workern herstellen.

Der Koordinator leitet entweder jede Abfrage an einen einzelnen Workerknoten weiter oder parallelisiert sie auf mehrere Knoten, abhängig davon, ob sich die erforderlichen Daten auf einem einzelnen oder auf mehreren Knoten befinden. Der Koordinator entscheidet anhand von Metadatentabellen, welche Aktion auszuführen ist. In diesen Tabellen werden die DNS-Namen und die Integrität der Workerknoten sowie die Verteilung der Daten auf Knoten nachverfolgt.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie [verteilte Daten](concepts-hyperscale-distributed-data.md) auf Knoten gespeichert werden.
