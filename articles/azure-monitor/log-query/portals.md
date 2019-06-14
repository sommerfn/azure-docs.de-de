---
title: Anzeigen und Analysieren von Protokolldaten in Azure Monitor | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die Verwendung von Log Analytics im Azure-Portal zum Erstellen und Bearbeiten von Protokollabfragen in Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: bwren
ms.openlocfilehash: 0e5b9b43e528b37fd994f9131f145abadb33c53b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "61425934"
---
# <a name="viewing-and-analyzing-log-data-in-azure-monitor"></a>Anzeigen und Analysieren von Protokolldaten in Azure Monitor
Log Analytics ist die primäre Oberfläche zum Arbeiten mit Protokolldaten und Erstellen von Abfragen in Azure Monitor. Öffnen Sie Log Analytics über **Protokolle** im **Azure Monitor**-Menü. Unter [Erste Schritte mit Log Analytics im Azure-Portal](get-started-portal.md) finden Sie eine Einführung in dieses Portal und können die entsprechenden Funktionen überprüfen.

Log Analytics bietet die folgenden Funktionen zum Arbeiten mit Protokollabfragen.

* Mehrere Registerkarten: Erstellen Sie separate Registerkarten für die Arbeit mit mehreren Abfragen.
* Umfangreiche Visualisierungen dank vielfältiger Diagrammoptionen.
* Verbesserungen bei IntelliSense und automatische Sprachvervollständigung.
* Syntaxhervorhebung zur besseren Lesbarkeit von Abfragen. 
* Abfrage-Explorer: Zugriff auf gespeicherte Abfragen und Funktionen.
* Schemaansicht: Überprüfen Sie die Struktur Ihrer Daten, um das Schreiben von Abfragen zu unterstützen.
* Freigeben: Erstellen Sie Links zu Abfragen, oder heften Sie Abfragen an ein freigegebenes Azure-Dashboard.
* Intelligente Analysen: Identifizieren Sie Spitzen in den Diagrammen, und erhalten Sie eine schnelle Analyse der Fehlerursache.
* Spaltenauswahl: Sortieren und gruppieren Sie Spalten in den Abfrageergebnissen.

> [!NOTE]
> Log Analytics verfügt über die gleiche Funktionalität wie das Advanced Analytics-Portal, ein externes Tool außerhalb des Azure-Portals. Das Advanced Analytics-Portal ist weiterhin verfügbar, allerdings werden Links und andere Verweise darauf im Azure-Portal durch diese neue Seite ersetzt.

![Log Analytics](media/portals/log-analytics.png)

## <a name="resource-logs"></a>Ressourcenprotokolle
Log Analytics ist in verschiedene Azure-Ressourcen, wie etwa VMs, integriert. Das bedeutet, dass Sie Log Analytics direkt über das Überwachungsmenü der Ressource öffnen können, ohne zu Azure Monitor zu wechseln und den Ressourcenkontext zu verlieren. **Protokolle** wurde noch nicht für alle Azure-Ressourcen aktiviert, es taucht jedoch nach und nach in den Portalmenüs verschiedener Ressourcentypen auf.

Beim Öffnen von Log Analytics aus einer bestimmten Ressource ist der Umfang automatisch so festgelegt, dass nur Datensätze der betreffenden Ressource protokolliert werden.   Wenn Sie eine Abfrage schreiben möchten, die andere Datensätze enthält, müssen Sie es über das Azure Monitor-Menü öffnen.

Die folgenden Optionen stehen noch nicht über die Ressourcenansicht von Log Analytics zur Verfügung:

- Speichern
- Warnung festlegen
- Abfrage-Explorer
- Wechseln zu einem anderen Arbeitsbereich/einer anderen Ressource (derzeit nicht geplant)


## <a name="firewall-requirements"></a>Firewallanforderungen
Ihr Browser erfordert Zugriff auf die folgenden Adressen, um auf Log Analytics zuzugreifen.  Wenn der Browser über eine Firewall auf das Azure-Portal zugreift, müssen Sie den Zugriff auf diese Adressen aktivieren.

| Uri | IP | Ports |
|:---|:---|:---|
| portal.loganalytics.io | Dynamisch | 80, 443 |
| api.loganalytics.io    | Dynamisch | 80, 443 |
| docs.loganalytics.io   | Dynamisch | 80, 443 |


## <a name="next-steps"></a>Nächste Schritte

- Arbeiten Sie ein [Tutorial zur Verwendung von Log Analytics](../../azure-monitor/log-query/get-started-portal.md) durch.
- Arbeiten Sie ein [Tutorial zur Verwendung der Protokollsuche](../../azure-monitor/learn/tutorial-viewdata.md) durch.

