---
title: Grundlegendes zu Metriken für Azure Spring Cloud
description: Hier erfahren Sie, wie Sie Metriken in Azure Spring Cloud überprüfen.
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: 952dfc1c707df3b7fa61443ea6ea18630352f0dc
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607805"
---
# <a name="metrics-for-azure-spring-cloud"></a>Metriken für Azure Spring Cloud

Der Azure Monitor-Metrik-Explorer ist eine Komponente des Microsoft Azure-Portals, die das Zeichnen von Diagrammen, das visuelle Korrelieren von Trends und das Untersuchen von Spitzen und Tiefen in Metriken ermöglicht. Verwenden Sie den Metrik-Explorer zum Untersuchen der Integrität und Auslastung Ihrer Ressourcen. Azure Spring Cloud bietet zwei Optionen zum Anzeigen von Metriken: Diagramme auf der Seite **Anwendungsübersicht** und die Seite mit Metriken auf Dienstebene.

## <a name="application-overview-page"></a>Seite „Anwendungsübersicht“

Die Seite **Anwendungsübersicht** jeder Anwendung enthält ein Metrikdiagramm, das eine schnelle Überprüfung des Anwendungsstatus ermöglicht.  Navigieren Sie zur Seite Ihres Azure Spring Cloud-Diensts, und wählen Sie **Anwendungsdashboard** und anschließend eine Anwendung in der Liste aus.  

Wir stellen fünf Diagramme mit minütlich aktualisierten Metriken für Folgendes bereit:

* **HTTP-Serverfehler**: Fehleranzahl für an Ihre App gesendete HTTP-Anforderungen
* **Eingehende Daten**: Von Ihrer App empfangene Bytes
* **Ausgehende Daten**: An Ihre App gesendete Bytes
* **Anforderungen**: Von Ihrer App empfangene Anforderungen
* **Durchschnittliche Antwortzeit**: Durchschnittliche Antwortzeit Ihrer App

Sie können für das Diagramm einen Zeitbereich zwischen einem Tag und sieben Tagen auswählen.

## <a name="service-level-metric-queries"></a>Metrikabfragen auf Dienstebene

Azure Spring Cloud ermöglicht die Überwachung verschiedener Anwendungsmetriken. Weitere Informationen zu diesem Dienst finden Sie im [Leitfaden zu den ersten Schritten](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started) mit Azure Monitor-Metriken.

Zum Überprüfen von Metrikdaten wählen Sie die Metrik, die **Aggregation** und den Zeitbereich aus.  Diese Konzepte werden nachfolgend erläutert.

### <a name="aggregation"></a>Aggregation 

Azure ruft minütlich Metriken ab und aktualisiert sie. Azure bietet drei Möglichkeiten zum Aggregieren von Daten für einen ausgewählten Zeitraum:

* **Total**: Summieren aller Metriken als Zielausgabe
* **Average**: Verwenden des Durchschnittswerts im Zeitraum als Zielausgabe
* **Maximum/Minimum**: Verwenden des maximalen/minimalen Werts im Zeitraum als Zielausgabe

### <a name="time-range"></a>Zeitbereich

Wählen Sie einen Standardzeitraum aus, oder definieren Sie einen eigenen Zeitraum.

### <a name="modifying-the-granularity-of-your-metric-query"></a>Ändern der Granularität der Metrikabfrage

Azure aggregiert Metriken standardmäßig für alle Anwendungen eines Azure Spring Cloud-Diensts. Verwenden Sie die Filterfunktion, um Metriken auf Anwendungs- oder Instanzebene zu überprüfen.  
Wählen Sie **Filter hinzufügen** aus, legen Sie die Eigenschaft auf **App** fest, und wählen Sie die zu überwachende Zielanwendung aus. Verwenden Sie optional die Option **Apply splitting** (Aufteilung anwenden), um separate Linien für jede App im Diagramm zu zeichnen.

>[!TIP]
> Sie können auf der Seite „Metriken“ Ihre eigenen Diagramme erstellen und an das **Dashboard** anheften. Geben Sie dem Diagramm zunächst einen Namen.  Wählen Sie anschließend oben rechts **An Dashboard anheften** aus. Sie können nun Ihre Anwendung auf dem **Dashboard** im Portal überprüfen.