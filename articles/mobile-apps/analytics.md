---
title: Grundlegendes zur Verwendung von mobilen Anwendungen und zum Benutzerverhalten mit Visual Studio App Center und Azure-Diensten
description: Erfahren Sie mehr über die Dienste wie App Center, die Ihnen helfen, intelligente Geschäftsentscheidungen zu treffen, indem sie aufbereiten, wie Benutzer Ihre mobile Anwendung nutzen.
author: elamalani
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: d60e9437a2f33788eb183cfcad0f3a10d71fb79d
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795178"
---
# <a name="analyze-and-understand-usage-of-mobile-applications"></a>Analysieren und Erfassen der Verwendung mobiler Anwendungen
Wie gut erfassen Sie, wie Ihre Benutzer ihre Anwendungen verwenden? Wie viele aktive Benutzer gibt es für Ihre Anwendung, und wie ändert sich die Verwendung im Laufe der Zeit? Welche Features verwenden die Benutzer, und welche Features werden am häufigsten verwendet? Wo befinden sich diese Benutzer? Wie viele Benutzer verwenden die neueste Version der Anwendung? Für alle diese Fragen ist die jeweilige Antwort wichtig, damit Ihre App in ein erfolgreiches Unternehmen mündet. Um diese Arten von Fragen zur Nutzungsanalyse zu beantworten, müssen Sie Nutzungsdaten aus Ihren Apps sammeln.

Je tiefergehend Sie die Daten auswerten, desto mehr können Sie Wege finden, Ihre Anwendung zu verbessern und Ihre Benutzer zufriedenzustellen. Es ist unverzichtbar, anhand der entsprechenden Daten aussagekräftige Erkenntnisse zu gewinnen und die Benutzer zu überzeugen.

## <a name="importance-of-analytics"></a>Wichtigkeit der Analyse
- **Verstehen** Sie Ihre Benutzer, verstehen Sie, wie die Benutzer mit Ihrer Anwendung interagieren und was die Benutzer zur Rückkehr veranlasst. Auf dieser Basis können Sie Ihre Anwendung optimieren und ein hervorragendes Erlebnis bieten, um Ihr Geschäft auszubauen.
- **Verfolgen** Sie Ihre Nutzungsmetriken, um fundierte Entscheidungen darüber zu treffen, wie Sie Ihre Anwendung vermarkten und ihre Kunden besser bedienen können.
- **Messen** Sie die Leistung Ihrer Anwendung?
- **Bestimmen** Sie, welche Teile Ihrer Anwendung zur Wert- und Leistungssteigerung beitragen.
- **Datengesteuerte Erkenntnisse** zu Problemen hinsichtlich Abwanderung und Bindung.

Verwenden Sie die folgenden Dienste, um die Analyse mobiler Anwendungen zu ermöglichen.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
Mit [App Center Analytics](/appcenter/analytics/) können Sie Ihre Nutzergemeinde vergrößern, indem Sie sich auf die wichtigen Punkte konzentrieren – dank tiefgreifender Berichte und Erkenntnisse über Benutzersitzungen, die am häufigsten verwendeten Geräte, Betriebssystemversionen und Verhaltensanalysen. Erstellen Sie auf einfache Weise benutzerdefinierte Ereignisse, um alles mit umfassenden Anwendungsanalysen nachzuverfolgen.

   **Wichtige Features**
   - **Verfolgen** Sie ohne zusätzliche Kosten Nutzungsmuster, Benutzerakzeptanz und andere Metriken zur Kundenbindung.
   - **Bestimmen** Sie Trends, Benutzerverhalten und Bindung über benutzerdefinierte Ereignisse.
   - **Standardmäßige Metriken** und **ausführliche Erkenntnisse** zu Anwendungsnutzung (täglich, wöchentlich, monatlich), Sitzungen, Geräteeigenschaften und Benutzerdemografien in einem **einzigen Dashboard**.
   - **Fortlaufender Export Ihrer gesamten Analysedaten nach Azure** zur unbegrenzten Speicherung. Unterstützt den Export nach Azure Blob Storage und Azure Application Insights.
   - **Einbindung in Azure Application Insights**, um noch tiefere Einblicke zu erhalten, so z. B. Bindung, Trichter-Analysen und Kohorten
   - **Einzeilige SDK-Integration**, um innerhalb weniger Minuten starten zu können.
   - **Plattformunterstützung**: iOS, Android, macOS, tvOS, Xamarin, React Native, Unity, Cordova.

   **Referenzen**
   - [Registrieren für App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
   - [Erste Schritte mit App Center Analytics](/appcenter/analytics/)

## <a name="azure-monitor"></a>Azure Monitor
Azure Monitor umfasst [Application Insights](/azure/azure-monitor/app/app-insights-overview). Dieser Dienst stellt Tools für das Erfassen und Analysieren von Telemetriedaten bereit, um die Leistung maximieren und Ihre mobile Anwendung überwachen zu können. Sie können Application Insights nutzen, indem Sie mit App Center Analytics arbeiten und den Export nach Application Insights einrichten. Application Insights kann die Telemetriedaten zu benutzerdefinierten Ereignissen aus Ihren Anwendungen abfragen, segmentieren, filtern und analysieren. Diese Funktionen reichen über diejenigen in App Center hinaus.

**Wichtige Features**
   - **Abfragen** der Telemetriedaten zu benutzerdefinierten Ereignissen
   - **Filtern** von ereignisbezogenen Telemetriedaten mit leistungsstarken Segmentierungsfunktionen
   - **Analysieren** von Wechsel-, Bindungs- und Navigationsmustern in Ihrer Anwendung
     - **Verkaufstrichter** zum Analysieren und Überwachen der Wechselraten
     - **Vermerkdauer** zum Analysieren, wie gut Ihre Anwendung Benutzer über die Zeit bindet
     - **Workbooks**, in denen Visualisierungen und Text in einem Bericht zusammengefasst werden, um sie in dieser Form mit anderen zu teilen
     - **Kohorten** zum Benennen und Speichern spezifischer Benutzer- oder Ereignisgruppen, damit auf diese ganz leicht von anderen Analysetools verwiesen werden kann

**Referenzen**
- [Azure-Portal](https://portal.azure.com/)
- [Analysieren Ihrer mobilen Apps mit App Center und Application Insights](/azure/azure-monitor/learn/mobile-center-quickstart)
- [Nutzungsanalyse mit Application Insights](/azure/azure-monitor/app/usage-overview)

## <a name="azure-playfab"></a>Azure PlayFab
[Azure PlayFab](https://playfab.com/) stellt eine vollständige Back-End-Plattform mit Spielediensten, Echtzeitanalysen und LiveOps bereit, die Sie zum Erstellen von erstklassigen über die Cloud verfügbaren Spielen benötigen. Diese Dienste verkleinern die Markteintrittsbarrieren für Spieleentwickler und bieten sowohl großen als auch kleinen Studios kostengünstige Entwicklungslösungen, die entsprechend den Spielen skalierbar sind und ihnen helfen, Spieler anzuziehen, zu binden und zu monetarisieren. PlayFab versetzt Entwickler in die Lage, die intelligente Cloud zu verwenden, um Spiele zu entwickeln und zu betreiben, Spieledaten zu analysieren und Spielerlebnisse insgesamt zu verbessern.

**Wichtige Features**
   - **Überwachen** von Echtzeitdashboards
   - **Auswerten** der Leistung Ihres Spiels über Hauptmetriken
   - **Erstellen von Berichten**, um Zusammenfassungen der täglichen und monatlichen Leistung Ihres Spiels durch automatisch generierte Berichte zu überprüfen, die in Game Manager angezeigt und täglich in Ihren Posteingang heruntergeladen oder gesendet werden können
   - **A/B-Tests**, um Experimente auszuführen und die optimale Einstellung für eine bestimmte Variable zu bestimmen
   - **Segmentierung** für Spieler ermöglicht es Ihnen, automatisierte Gruppierungen von Spielern zu definieren.
    
**Referenzen**
- [PlayFab-Portal](https://developer.playfab.com/en-US/sign-up)
- [Analyse](/gaming/playfab/#pivot=documentation&panel=analytics)
- [Schnellstarts](/gaming/playfab/#pivot=documentation&panel=quickstarts)    
