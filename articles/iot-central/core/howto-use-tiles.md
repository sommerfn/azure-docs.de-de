---
title: Verwenden von Kacheln im Dashboard der Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Kacheln in Dashboards für Anwendungen, Gerätegruppen und Geräte verwenden können.
author: v-krghan
ms.author: v-krghan
ms.date: 06/27/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 0803258c362279049a49a7eee00e7a4763621672
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72942235"
---
# <a name="how-to-use-tiles"></a>Verwenden von Kacheln
Sie können mit Kacheln Dashboards für Anwendungen, Gerätegruppen und Geräte anpassen. Sie können einem Dashboard mehrere Kacheln hinzufügen und diese so anpassen, dass sie für Ihre Anwendung relevante Informationen zeigen. Sie können auch die Größe von Kacheln ändern und das Layout auf jedem Dashboard anpassen. Der folgende Screenshot zeigt das Anwendungsdashboard mit unterschiedlichen Kacheln.

![Anwendungsdashboard](media/howto-use-tiles/image1a.png)


In der folgende Tabelle ist die Verwendung von Kacheln in Azure IoT Central zusammengefasst:

 
| Kachel | Dashboard | BESCHREIBUNG
| ----------- | ------- | ------- |
| Link | Dashboards von Anwendungen und Gerätegruppen |Auf Kacheln mit einem Link, auf denen Überschriften und Beschreibungstexte angezeigt werden, kann geklickt werden. Verwenden Sie eine Kachel mit einem Link, damit Benutzer zu einer URL navigieren können, die zu Ihrer Anwendung gehört. |
| Image | Dashboards von Anwendungen und Gerätegruppen |Auf Bildkacheln, auf denen ein benutzerdefiniertes Bild angezeigt wird, kann geklickt werden. Verwenden Sie eine Bildkachel, um einem Dashboard Grafiken hinzuzufügen und optional einem Benutzer die Navigation zu einer URL zu ermöglichen, die für Ihre Anwendung relevant ist.|
| Bezeichnung | Anwendungsdashboards |Auf Kacheln mit einer Beschriftung auf einem Dashboard wird benutzerdefinierter Text angezeigt. Sie können die Größe des Texts festlegen. Auf einer Kachel mit Beschriftung können Sie dem Dashboard relevante Informationen wie Beschreibungen, Kontaktinformationen oder Hilfe hinzufügen.|
| Map | Dashboards von Anwendungen und Gerätegruppen |Auf Kartenkacheln werden der Standort und Status eines Geräts auf einer Karte angezeigt. So können Sie beispielsweise anzeigen, wo sich ein Gerät befindet und ob der Lüfter eingeschaltet ist.|
| Liniendiagramm | Dashboards von Anwendungen und Geräten |Liniendiagrammkacheln zeigen eine Grafik der Aggregatmessung für ein Gerät über einen Zeitraum. Sie können beispielsweise ein Liniendiagramm anzeigen, das die durchschnittliche Temperatur und den durchschnittlichen Druck auf einem Gerät in der letzten Stunde anzeigt.|
| Balkendiagramm | Dashboards von Anwendungen und Geräten |Balkendiagrammkacheln zeigen eine Grafik der Aggregatmessung für ein Gerät über einen Zeitraum. Sie können beispielsweise ein Balkendiagramm anzeigen, das die durchschnittliche Temperatur und den durchschnittlichen Druck auf einem Gerät in der letzten Stunde anzeigt. |
| Ereignisverlauf | Dashboards von Anwendungen und Geräten |Auf einer Kachel mit dem Ereignisverlauf werden die Ereignisse auf einem Gerät in einem Zeitraum gezeigt. Damit können Sie beispielsweise alle Temperaturänderungen anzeigen, die in der letzten Stunde auf einem Gerät erfolgt sind. |
| Der Statusverlauf | Dashboards von Anwendungen und Geräten |Auf einer Statusverlaufskachel werden die Messwerte für einen bestimmten Zeitraum gezeigt. Damit können Sie beispielsweise die Temperaturwerte eines Geräts in der letzten Stunde anzeigen.|
| KPI | Dashboards von Anwendungen und Geräten | Auf einer KPI-Kachel wird eine aggregierte Telemetrie- oder Ereignismessung für einen Zeitraum gezeigt. Damit können Sie beispielsweise die Maximaltemperatur eines Geräts in der letzten Stunde anzeigen.|
| Letzter bekannter Wert | Dashboards von Anwendungen und Geräten |Auf der Kachel mit dem letzten bekannten Wert wird der neueste Wert für eine Telemetrie- oder Statusmessung ausgegeben. Auf dieser Kachel können Sie beispielsweise die neuesten Messungen von Temperatur, Druck und Feuchtigkeit für ein Gerät anzeigen.|
| Gerätegruppenraster | Dashboards von Anwendungen und Gerätegruppen | Im Gerätegruppenraster werden Informationen zur Gerätegruppe gezeigt. Zeigen Sie auf einer Kachel mit einem Gerätegruppenraster Informationen wie Name, Standort und Modell aller Geräte in der Gerätegruppe an.|


Weitere Informationen zum Konfigurieren eines Dashboards in Ihrer Azure IoT Central-Anwendung finden Sie unter [Hinzufügen von Kacheln zu Ihrem Dashboard](./howto-add-tiles-to-your-dashboard.md).
