---
title: Übersicht über Azure Maps | Microsoft-Dokumentation
description: Einführung in Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 02/04/2019
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 5f41263113568cf9f3771119135be8db37119181
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442942"
---
# <a name="what-is-azure-maps"></a>Was ist Azure Maps?

Azure Maps ist eine Sammlung von Geodiensten, bei denen aktuelle Kartendaten verwendet werden, um einen präzisen geografischen Kontext für Webanwendungen und mobile Anwendungen bereitzustellen. Azure Maps umfasst Folgendes:

* REST-APIs zum Rendern von Karten in verschiedenen Stilen und in Satellitenbildern
* Suche nach Adressen, Orten und Points of Interest auf der ganzen Welt
* Routenplanung (A nach B, mehrere Punkte, Optimierung bei Verwendung mehrerer Punkte, Isochrone, gewerbliche Fahrzeuge, Berücksichtigung der Verkehrslage und Matrixroutenplanung) sowie Anzeige von Daten zu Verkehrsfluss und -störungen
* Mobilitätsdienste zum Anfordern von Informationen zum öffentlichen Nahverkehr und zu alternativen Transportmitteln (z. B. Mietfahrräder, -roller und -autos) und zum Planen von Routen in Echtzeit 
* Ermittlung des Benutzerstandorts mittels Geolocation und Konvertierung des Standorts in Zeitzonen 
* Dienste für Geofencing und Speicherung von Kartendaten mit in Azure gehosteten Standortinformationen 
* Location Intelligence mittels Geoanalysen 

Neben der Verwendung als REST-APIs sind Azure Maps-Dienste über das Web SDK oder das Android SDK verfügbar. Mit diesen Tools können Entwickler im Handumdrehen Lösungen entwickeln und skalieren, die Standortinformationen in Azure-Lösungen integrieren. 

Sie können sich für ein kostenloses [Azure Maps-Konto](https://azure.microsoft.com/services/azure-maps/) registrieren und direkt loslegen.

Im folgenden Video wird Azure Maps ausführlich beschrieben:

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Kartensteuerelemente

### <a name="web-sdk"></a>Web-SDK

Mit dem Azure Maps Web SDK können Sie interaktive Karten mit eigenen Inhalten und Bildern anpassen, die in Ihren webbasierten oder mobilen Anwendungen angezeigt werden sollen. Dieses Steuerelement nutzt WebGL, sodass Sie umfangreiche Datasets mit hoher Leistung rendern können. Verwenden Sie für die Entwicklung mit dem SDK JavaScript oder TypeScript.

![Beispielkarte zur Bevölkerungsveränderung](media/about-azure-maps/Introduction_WebMapControl.png)

### <a name="android-sdk"></a>Android-SDK

Mit dem Azure Maps Android SDK können Sie mobile Kartenanwendungen erstellen. 

![Kartenbeispiele auf einem mobilen Gerät](media/about-azure-maps/AndroidSDK.png)

## <a name="services-in-azure-maps"></a>Dienste in Azure Maps

Azure Maps besteht aus den folgenden neun Diensten, die für Ihre Azure-Anwendungen geografischen Kontext liefern können.

### <a name="data-service"></a>Datendienst

Daten sind bei Karten ein Muss. Mit dem Datendienst können Sie räumliche Daten zur Verwendung mit räumlichen Vorgängen oder mit der Bildkomposition hochladen und speichern.  Durch die Einbindung von Kundendaten in den Azure Maps-Dienst verringern Sie die Wartezeit und steigern die Produktivität und können neue Szenarien in Ihren Anwendungen erstellen. Ausführliche Informationen zu diesem Dienst finden Sie in der [Dokumentation zu den Datendienst-APIs](https://docs.microsoft.com/rest/api/maps/data).

### <a name="mobility-service"></a>Mobilitätsdienst

Der Azure Maps Mobility Service ermöglicht die Routenplanung in Echtzeit. Dabei werden die bestmöglichen Routenoptionen sowie verschiedene Transportmittel zurückgeben. Im Stadtbereich kann dies zu Fuß, mit dem Fahrrad und mit öffentlichen Verkehrsmitteln sein. Entwickler können Einzelheiten zu Strecken anfordern. Hierzu zählen beispielsweise Routengeometrie, Liste der Haltestellen, geplante Ankunft und Echtzeitankunft sowie Dienstwarnungen.

Der Dienst ermöglicht auch die Suche nach spezifischen Objekttypen, z. B. nach Mietfahrrädern, -rollern oder -autos in der Nähe eines bestimmten Standorts. Benutzer können Informationen zur Anzahl der verfügbaren Mietfahrräder an der nächstgelegenen Station abfragen. Sie können nach verfügbaren Carsharing-Fahrzeugen suchen und Details wie zukünftige Verfügbarkeit und aktueller Kraftstoffstand abrufen.

Weitere Informationen zu diesem Dienst finden Sie in der [Dokumentation zur Mobility-API](https://docs.microsoft.com/rest/api/maps/mobility).

### <a name="render-service"></a>Renderingdienst

Mit dem Renderingdienst können Entwickler Web- und mobile Anwendungen mit Kartenfunktionen erstellen. Für den Dienst werden entweder qualitativ hochwertige Rastergrafikbilder, die in 19 Zoomebenen verfügbar sind, oder vollständig anpassbare Vektorformat-Kartenbilder verwendet.

![Beispiel für eine Karte vom Renderingdienst](media/about-azure-maps/Introduction_Map.png)

Über den Renderingdienst werden jetzt Vorschau-APIs bereitgestellt, um Entwicklern die Verwendung von Satellitenbildern zu ermöglichen. Weitere Informationen finden Sie in der [Dokumentation zu den Render-APIs](https://docs.microsoft.com/rest/api/maps/render).

### <a name="route-service"></a>Routendienst

Der Routendienst umfasst stabile Geometrieberechnungen für reale Infrastruktur und Wegbeschreibungen für verschiedene Transportmodi. Entwickler können mithilfe des Diensts Wegbeschreibungen für eine Reihe von Fortbewegungsmitteln (z.B. PKW, LKW, Fahrrad oder zu Fuß) generieren. Zudem kann der Dienst Faktoren wie Verkehrslage, Gewichtsbeschränkungen oder Transport von Gefahrgütern berücksichtigen.

![Beispiel für eine Karte vom Routendienst](media/about-azure-maps/Introduction_Route.png)

Der Routendienst ermöglicht eine Vorschau für erweiterte Funktionen, beispielsweise: 

* Batchverarbeitung für mehrere Routenanforderungen
* Matrizen für Fahrzeit und Entfernung zwischen verschiedenen Ausgangsorten und Zielen
* Ermittlung von Routen oder Entfernungen, die Benutzer basierend auf Zeit- oder Kraftstoffanforderungen zurücklegen können 

Ausführliche Informationen zu den Routingfunktionen finden Sie in der [Dokumentation zu Routen-APIs](https://docs.microsoft.com/rest/api/maps/route).

### <a name="search-service"></a>Suchdienst

Mit dem Suchdienst können Entwickler nach Adressen, Orten, Geschäftseinträgen nach Name oder Kategorie und anderen geografischen Informationen suchen. Mit dem Suchdienst kann eine [inverse Geocodierung](https://en.wikipedia.org/wiki/Reverse_geocoding) für Adressen und Querstraßen basierend auf Breiten- und Längengraden durchgeführt werden.

![Beispiel für eine Suche in einer Karte](media/about-azure-maps/Introduction_Search.png)

Der Suchdienst umfasst außerdem erweiterte Funktionen, beispielsweise:

* Suche entlang einer Route
* Suche innerhalb eines weiteren Bereichs
* Zusammenfassen einer Gruppe von Suchanfragen zu einem Batch
* Suche nach einem größeren Bereich anstelle eines Standorts 

Die APIs für Batches und die Bereichssuche befinden sich derzeit in der Vorschauphase. Weitere Informationen zu den Suchfunktionen finden Sie in der [Dokumentation zu den Such-APIs](https://docs.microsoft.com/rest/api/maps/search).

### <a name="spatial-operations-service"></a>Dienst „Räumliche Operationen“

Mit dem Azure Maps-Dienst „Räumliche Operationen“ können Sie Standortinformationen direkt und ohne Vorbereitung analysieren, um Kunden über laufende Ereignisse zu bestimmten Zeiten und an verschiedenen Orten zu informieren. Er ermöglicht die Analyse- und Vorhersagemodellierung von Ereignissen nahezu in Echtzeit. 

Der Dienst ermöglicht Kunden die Optimierung von Location Intelligence mit einer Bibliothek aus häufig genutzten räumlichen Berechnungen, z. B. zum Geofencing, zur Ermittlung des am nächsten gelegenen Punkts und des Kreisabstands sowie für Puffer. Weitere Informationen zum Dienst und zu den verschiedenen Funktionen finden Sie in der [Dokumentation zu den APIs für räumliche Operationen](https://docs.microsoft.com/rest/api/maps/spatial).

### <a name="time-zone-service"></a>Zeitzonendienst

Mit dem Zeitzonendienst können Sie aktuelle, vergangene und zukünftige Zeitzoneninformationen abfragen, indem Sie entweder Breitengrad-Längengrad-Paare oder eine [IANA-ID](https://www.iana.org/) verwenden. Mit dem Zeitzonendienst ist Folgendes möglich:

* Konvertieren von Microsoft Windows-Zeitzonen-IDs in IANA-Zeitzonen
* Abrufen eines Zeitzonenoffsets zu UTC
* Abrufen der aktuellen Zeit einer Zeitzone 

Eine typische JSON-Antwort für eine Abfrage mit dem Zeitzonendienst sieht wie folgt aus:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

Ausführliche Informationen zu diesem Dienst finden Sie in der [Dokumentation zu den Zeitzonen-APIs](https://docs.microsoft.com/rest/api/maps/timezone).

### <a name="traffic-service"></a>Verkehrsinfodienst

Der Verkehrsinfodienst ist eine Suite mit Webdiensten, mit denen Entwickler Web- und mobile Anwendungen erstellen können, für die Verkehrsinfos erforderlich sind. Der Dienst stellt zwei Typen von Daten bereit:

* Verkehrsfluss: In Echtzeit ermittelte Geschwindigkeiten und Reisezeiten für alle wichtigen Straßen des Verkehrsnetzes
* Verkehrsmeldungen: Aktuelle Übersicht über die Staus und Störungen bzw. Unfälle im Straßennetz

![Beispiel für eine Karte mit Verkehrsinformationen](media/about-azure-maps/Introduction_Traffic.png)

Weitere Informationen finden Sie in der [Dokumentation zu den Verkehrsinfo-APIs](https://docs.microsoft.com/rest/api/maps/traffic).

### <a name="ip-to-location-service"></a>IP to Location-Dienst

Mit dem Dienst „IP to Location“ können Sie eine Vorschau für den abgerufenen zweistelligen Ländercode für eine IP-Adresse erstellen. Mithilfe dieses Diensts können Sie die Benutzeroberfläche anpassen und verbessern, indem Sie benutzerdefinierte Anwendungen basierend auf dem geografischen Standort bereitstellen.

Weitere Informationen zu den REST-APIs für den Dienst „IP to Location“ finden Sie in der [Dokumentation zu den Geolocation-APIs von Azure Maps](https://docs.microsoft.com/rest/api/maps/geolocation).

## <a name="programming-model"></a>Programmiermodell

Azure Maps ist auf Mobilität ausgelegt und kann zum Entwickeln plattformübergreifender Anwendungen eingesetzt werden. Es wird ein Programmiermodell genutzt, das sprachunabhängig ist und die JSON-Ausgabe über [REST-APIs](https://docs.microsoft.com/rest/api/maps/) unterstützt.

Außerdem verfügt Azure Maps über ein komfortables [JavaScript-Kartensteuerelement](https://docs.microsoft.com/javascript/api/azure-maps-control) mit einem einfachen Programmiermodell für die schnelle und einfache Entwicklung webbasierter und mobiler Anwendungen.

## <a name="usage"></a>Verwendung

Für den Zugriff auf die Azure Maps-Dienste navigieren Sie zum [Azure-Portal](https://portal.azure.com) und erstellen ein Azure Maps-Konto.

Für Azure Maps wird ein schlüsselbasiertes Authentifizierungsschema verwendet. Für Ihr Konto wurden bereits zwei Schlüssel generiert. Beginnen Sie damit, diese Standortfunktionen in Ihre Anwendungen zu integrieren, indem Sie einen Ihrer Schlüssel verwenden und eine Anforderung an den Azure Maps-Dienst senden.

## <a name="supported-regions"></a>Unterstützte Regionen

Die Azure Maps-API ist derzeit in allen Ländern und Regionen verfügbar, mit folgenden Ausnahmen:

* Argentinien
* China
* Indien
* Marokko
* Pakistan
* Südkorea

Vergewissern Sie sich, dass sich der Standort der aktuellen IP-Adresse nicht in einem bzw. einer der nicht unterstützten Länder oder Regionen befindet.

## <a name="next-steps"></a>Nächste Schritte

Testen Sie eine Beispiel-App zur Veranschaulichung von Azure Maps:

> [!div class="nextstepaction"]
> [Schnellstart: Erstellen einer Web-App](quick-demo-map-app.md)

Halten Sie sich über Azure Maps auf dem Laufenden: 

> [!div class="nextstepaction"]
> [Blog zu Azure Maps](https://azure.microsoft.com/blog/topics/azure-maps/)
