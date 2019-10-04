---
title: Query Performance Insight in Azure Database for PostgreSQL – Einzelserver
description: In diesem Artikel wird das Feature Query Performance Insight in Azure Database for PostgreSQL (Einzelserver) beschrieben.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: 748c6f08da535013724f68b8be424e50a2d49dfd
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764428"
---
# <a name="query-performance-insight"></a>Query Performance Insight 

**Anwendungsbereich:** Azure Database for PostgreSQL – Einzelserverversionen 9.6, 10, 11

Mithilfe von Query Performance Insight können Sie schnell die Abfragen mit den längsten Ausführungszeiten identifizieren, wie sie sich im Laufe der Zeit ändern und welche Wartezeiten sie beeinflussen.

## <a name="permissions"></a>Berechtigungen
Zum Anzeigen des Abfragetexts in Query Performance Insight sind die Berechtigungen **Besitzer** oder **Mitwirkender** erforderlich. Mit **Leser** können Diagramme und Tabellen angezeigt werden, aber nicht im Abfragetext.

## <a name="prerequisites"></a>Voraussetzungen
Damit Query Performance Insight funktioniert, müssen Daten im [Abfragespeicher](concepts-query-store.md) vorhanden sein.

## <a name="viewing-performance-insights"></a>Anzeigen von Einblicken in die Leistung
Die [Query Performance Insight](concepts-query-performance-insight.md)-Ansicht im Azure-Portal zeigt wichtige Informationen aus dem Abfragespeicher an. 

Wählen Sie in der Portalseite Ihres Azure Database for PostgreSQL-Servers im Abschnitt **Intelligente Leistung** in der Menüleiste die Option **Query Performance Insight**.

![Abfragen mit langer Ausführungszeit in Query Performance Insight](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

Die Registerkarte **Abfragen mit langer Ausführungszeit** zeigt die ersten fünf Abfragen nach durchschnittlicher Dauer pro Ausführung an, zusammengefasst in Intervallen von 15 Minuten. Sie können mehr Abfragen anzeigen, indem Sie in der Dropdownliste **Anzahl der Abfragen** eine Auswahl treffen. Dabei ändern sich unter Umständen die Diagrammfarben für eine bestimmte Abfrage-ID.

Durch Klicken und Ziehen im Diagramm können Sie die Zeit auf ein bestimmtes Zeitfenster eingrenzen. Alternativ zeigen Sie mit den Symbolen zum Vergrößern oder Verkleinern einen kürzeren bzw. längeren Zeitraum an.

In der Tabelle unter dem Diagramm finden Sie weitere Informationen zu Abfragen mit langer Ausführungszeit in diesem Zeitfenster.

Auf der Registerkarte **Wartestatistik** werden die entsprechenden Visualisierungen zu Wartevorgängen angezeigt.

![Query Performance Insight-Wartestatistiken](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="considerations"></a>Überlegungen
* Query Performance Insight ist für [Lesereplikate](concepts-read-replicas.md) nicht verfügbar.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [Überwachung und Optimierung](concepts-monitoring.md) in Azure Database for PostgreSQL.


