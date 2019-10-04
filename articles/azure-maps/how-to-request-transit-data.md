---
title: 'Gewusst wie: Anfordern von Daten zu öffentlichen Verkehrsmitteln bzw. Routen in Azure Maps | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie mit dem Mobility Service von Azure Maps Daten zu öffentlichen Verkehrsmitteln bzw. Routen anfordern.
author: walsehgal
ms.author: v-musehg
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: b28788ac7b3ce4e1997b71c683f8e0445406a391
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70915628"
---
# <a name="request-public-transit-data-using-the-azure-maps-mobility-service"></a>Anfordern von Daten zu öffentlichen Verkehrsmitteln bzw. Routen mit dem Mobility Service von Azure Maps 

In diesem Artikel wird beschrieben, wie Sie den [Mobility Service](https://aka.ms/AzureMapsMobilityService) von Azure Maps verwenden, um Daten zu öffentlichen Verkehrsmitteln bzw. Routen anzufordern, z. B. Haltestellen, Routen und geschätzte Dauer.

In diesem Artikel wird Folgendes behandelt:

* Rufen Sie die ID für einen Stadtbereich mit der [Get Metro Area-API](https://aka.ms/AzureMapsMobilityMetro) ab.
* Fordern Sie Informationen zu Haltestellen in der Nähe an, indem Sie den Dienst [Get Nearby Transit](https://aka.ms/AzureMapsMobilityNearbyTransit) verwenden.
* Führen Sie eine Abfrage über die [Get Transit Routes-API](https://aka.ms/AzureMapsMobilityTransitRoute) durch, um eine Route mit öffentlichen Verkehrsmitteln zu planen.
* Fordern Sie die Geometrie der Verkehrsroute und den zugehörigen detaillierten Zeitplan an, indem Sie die [Get Transit Itinerary-API](https://aka.ms/https://azure.microsoft.com/services/azure-maps/) verwenden.


## <a name="prerequisites"></a>Voraussetzungen

Zum Aufrufen der APIs für öffentliche Verkehrsmittel bzw. Routen von Azure Maps benötigen Sie ein Maps-Konto und einen entsprechenden Schlüssel. Informationen zum Erstellen eines Kontos finden Sie in den Anweisungen zum [Verwalten von Konten](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account), und führen Sie die Schritte unter [Abrufen des Primärschlüssels](./tutorial-search-location.md#getkey) aus, um einen primären Abonnementschlüssel für Ihr Konto abzurufen.

In diesem Artikel wird die [Postman-App](https://www.getpostman.com/apps) zum Erstellen von REST-Aufrufen verwendet. Sie können jedoch auch Ihre bevorzugte API-Entwicklungsumgebung verwenden.


## <a name="get-a-metro-area-id"></a>Abrufen einer ID für den Stadtbereich

Zum Abrufen von Informationen zu öffentlichen Verkehrsmitteln bzw. Routen für einen bestimmten Stadtbereich benötigen Sie die `metroId` für den gewünschten Bereich. Mit der [Get Metro Area-API](https://aka.ms/AzureMapsMobilityMetro) können Sie Stadtbereiche anfordern, in denen der Mobility Service von Azure Maps verfügbar ist. Die Antwort enthält dann Details wie `metroId`, `metroName` und eine Darstellung der Geometrie des Stadtbereichs im GeoJSON-Format.

Wir erstellen nun eine Anforderung zum Abrufen der Stadtbereich-ID für „Seattle-Tacoma“. Führen Sie die folgenden Schritte aus, um die ID für einen Stadtbereich anzufordern:

1. Erstellen Sie eine Sammlung, in der die Anforderungen gespeichert werden. Klicken Sie in der Postman-App auf **New** (Neu). Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Collection** (Sammlung) aus. Geben Sie einen Namen für die Sammlung ein, und klicken Sie dann auf **Create** (Erstellen).

2. Klicken Sie erneut auf **New** (Neu), um die Anforderung zu erstellen. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Request** (Anforderung) aus. Geben Sie einen Anforderungsnamen (**Request name**) für die Anforderung ein, wählen Sie die im vorherigen Schritt erstellte Sammlung als Speicherort für die Anforderung aus, und wählen Sie anschließend **Save** (Speichern) aus.
    
    ![Erstellen einer Anforderung in Postman](./media/how-to-request-transit-data/postman-new.png)

3. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-Methode „GET“ aus, und geben Sie die folgende URL ein, um eine GET-Anforderung zu erstellen.

    ```HTTP
    https://atlas.microsoft.com/mobility/metroArea/id/json?subscription-key={subscription-key}&api-version=1.0&query=47.63096,-122.126
    ```

4. Nach einer erfolgreichen Anforderung erhalten Sie die folgende Antwort:

    ```JSON
    {
        "results": [
            {
                "metroId": 522,
                "metroName": "Seattle–Tacoma–Bellevue, WA",
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 48.5853,
                        "longitude": -124.80934
                    },
                    "btmRightPoint": {
                        "latitude": 46.90534,
                        "longitude": -121.55032
                    }
                },
                "geometry": {
                    "type": "Polygon",
                    "coordinates": [
                        [
                            [
                                -121.99604,
                                47.16147
                            ],
                            [
                                -121.97051,
                                47.17222
                            ],
                            [
                                -121.96308,
                                47.17671
                            ],
                            ...,
                            ...,
                            ...,
                            [
                                -122.01525,
                                47.16008
                            ],
                            [
                                -122.00553,
                                47.15919
                            ],
                            [
                                -121.99604,
                                47.16147
                            ]
                        ]
                    ]
                }
            }
        ]
    }
    ```

5. Kopieren Sie die `metroId` zur späteren Verwendung.

## <a name="request-nearby-transit-stops"></a>Anfordern von Verkehrsmittel-Haltestellen in der Nähe

Mit dem Dienst [Get Nearby Transit](https://aka.ms/AzureMapsMobilityNearbyTransit) von Azure Maps können Sie Verkehrsobjekte durchsuchen, z. B. nach Haltestellen von öffentlichen Verkehrsmitteln und Stationen von Leihfahrrädern an einem bestimmten Ort. Die Details zum Verkehrsobjekt werden dann zurückgegeben. Als Nächstes senden wir eine Anforderung an den Dienst, um nach den Haltestellen von öffentlichen Verkehrsmitteln innerhalb eines Radius von 300 Metern um einen bestimmten Ort zu suchen. In die Anforderung müssen wir die `metroId` einfügen, die wir weiter oben abgerufen haben.

Führen Sie die hier angegebenen Schritte aus, um eine Anforderung an [Get Nearby Transit](https://aka.ms/AzureMapsMobilityNearbyTransit) zu senden:

1. Klicken Sie in Postman auf **New Request (Neue Anforderung)**  | **GET request (GET-Anforderung)** , und nennen Sie sie **Get Nearby stops** (Haltestellen in der Nähe abrufen).

2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, geben Sie die folgende Anforderungs-URL für Ihren API-Endpunkt ein, und klicken Sie auf **Send** (Senden).

    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=47.63096,-122.126&radius=300&objectType=stop
    ```

3. Nach einer erfolgreichen Anforderung sollte die Antwortstruktur in etwa wie folgt aussehen:

    ```JSON
    {
        "results": [
            {
                "id": "522---2060603",
                "type": "stop",
                "objectDetails": {
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631504,
                    "longitude": -122.125275
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.632413,
                        "longitude": -122.12659
                    },
                    "btmRightPoint": {
                        "latitude": 47.630594,
                        "longitude": -122.123959
                    }
                }
            },
            {
                "id": "522---2061020",
                "type": "stop",
                "objectDetails": {
                    "stopKey": "68372",
                    "stopName": "NE 24th St & 160th Ave NE",
                    "stopCode": "68372",
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631409,
                    "longitude": -122.127136
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.632318,
                        "longitude": -122.128451
                    },
                    "btmRightPoint": {
                        "latitude": 47.630499,
                        "longitude": -122.12582
                    }
                }
            },
            {
                "id": "522---2060604",
                "type": "stop",
                "objectDetails": {
                    "stopKey": "71310",
                    "stopName": "NE 24th St & 160th Ave NE",
                    "stopCode": "71310",
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631565,
                    "longitude": -122.127808
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.632474,
                        "longitude": -122.129124
                    },
                    "btmRightPoint": {
                        "latitude": 47.630655,
                        "longitude": -122.126492
                    }
                }
            }
        ]
    }   
    ```

Wenn Sie die Antwortstruktur genauer betrachten, sehen Sie, dass für jedes Verkehrsobjekt Parameter vorhanden sind, z. B. `id`, `type`, `stopName`, `mainTransitType`, `mainAgencyName` und die Position (Koordinaten) des Objekts.

Um die Verständlichkeit zu verbessern, verwenden wir im nächsten Abschnitt die `id` einer Bushaltestelle als Ausgangspunkt für unsere Route.  


## <a name="request-a-transit-route"></a>Anfordern einer Verkehrsroute

Mit der [Get Transit Routes-API](https://aka.ms/AzureMapsMobilityTransitRoute) von Azure Maps können Sie die Fahrt planen und die bestmöglichen Optionen für Routen zwischen einem Ausgangs- und einem Zielort erhalten. Der Dienst verfügt über verschiedene Fortbewegungsmodi, z. B. zu Fuß, Fahrrad und öffentliche Verkehrsmittel. Als Nächstes suchen wir nach einer Route von der nächstgelegenen Bushaltestelle zur Space Needle in Seattle.

### <a name="get-location-coordinates-for-destination"></a>Abrufen von Standortkoordinaten für ein Ziel

Zum Abrufen der Standortkoordinaten für die Space Needle verwenden wir den [Dienst für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) von Azure Maps.

Führen Sie diese Schritte aus, um eine Anforderung an den Dienst für die Fuzzysuche zu senden:

1. Klicken Sie in Postman auf **New Request (Neue Anforderung)**  | **GET request (GET-Anforderung)** , und nennen Sie sie **Get location coordinates** (Standortkoordinaten abrufen).

2.  Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, geben Sie die folgende Anforderungs-URL ein, und klicken Sie auf **Send** (Senden).
 
    ```HTTP
    https://atlas.microsoft.com/search/fuzzy/json?subscription-key={subscription-key}&api-version=1.0&query=space needle
    ```
    
3. Wenn Sie sich die Antwort genauer ansehen, fällt Ihnen Folgendes auf: Sie enthält in den Ergebnissen für die Space Needle mehrere Standorte und unter **position** jeweils die zugehörigen Informationen zu den Standortkoordinaten. Kopieren Sie die Werte unter `lat` und `lon` für die Position aus dem ersten Ergebnis.
    
   ```JSON
   {
        "summary": {
            "query": "space needle",
            "queryType": "NON_NEAR",
            "queryTime": 35,
            "numResults": 8,
            "offset": 0,
            "totalResults": 11,
            "fuzzyLevel": 1
        },
        "results": [
            {
                "type": "POI",
                "id": "US/POI/p0/6993440",
                "score": 4.67369,
                "info": "search:ta:840539001406144-US",
                "poi": {
                    "name": "Space Needle",
                    "phone": "+(1)-(206)-9052100",
                    "categorySet": [
                        {
                            "id": 7376009
                        }
                    ],
                    "url": "www.spaceneedle.com",
                    "categories": [
                        "important tourist attraction",
                        "tower"
                    ],
                    "classifications": [
                        {
                            "code": "IMPORTANT_TOURIST_ATTRACTION",
                            "names": [
                                {
                                    "nameLocale": "en-US",
                                    "name": "important tourist attraction"
                                },
                                {
                                    "nameLocale": "en-US",
                                    "name": "tower"
                                }
                            ]
                        }
                    ]
                },
                "address": {
                    "streetNumber": "400",
                    "streetName": "Broad St",
                    "municipalitySubdivision": "South Lake Union, Seattle, Lower Queen Anne",
                    "municipality": "Seattle",
                    "countrySecondarySubdivision": "King",
                    "countryTertiarySubdivision": "Seattle",
                    "countrySubdivision": "WA",
                    "postalCode": "98109",
                    "countryCode": "US",
                    "country": "United States",
                    "countryCodeISO3": "USA",
                    "freeformAddress": "400 Broad St, Seattle, WA 98109",
                    "localName": "Seattle",
                    "countrySubdivisionName": "Washington"
                },
                "position": {
                    "lat": 47.62039,
                    "lon": -122.34928
                },
                "viewport": {
                    "topLeftPoint": {
                        "lat": 47.62129,
                        "lon": -122.35061
                    },
                    "btmRightPoint": {
                        "lat": 47.61949,
                        "lon": -122.34795
                    }
                },
                "entryPoints": [
                    {
                        "type": "main",
                        "position": {
                            "lat": 47.61982,
                            "lon": -122.34886
                        }
                    }
                ]
            },
            ...,
            ...,
            ...
        ]
    }
    ``` 
    

### <a name="request-route"></a>Anfordern einer Route

Führen Sie die folgenden Schritte aus, um eine Route anzufordern:

1. Klicken Sie in Postman auf **New Request (Neue Anforderung)**  | **GET request (GET-Anforderung)** , und nennen Sie sie **Get Route info** (Routeninformationen abrufen).

2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, geben Sie die folgende Anforderungs-URL für Ihren API-Endpunkt ein, und klicken Sie auf **Send** (Senden).

    Wir fordern die Routen für das öffentliche Verkehrsmittel „Bus“ an, indem wir die Parameter `modeType` und `transitType` angeben. Die Anforderungs-URL enthält die Standorte, die in den vorherigen Abschnitten abgerufen wurden. Wir verfügen für `originType` jetzt über **stopId** und für `destionationType` über **position**.

    Sehen Sie sich die [Liste mit den URI-Parametern](https://aka.ms/AzureMapsMobilityTransitRoute#uri-parameters) an, die Sie in Ihrer Anforderung für die [Get Transit Routes-API](https://aka.ms/AzureMapsMobilityTransitRoute) verwenden können. 
  
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/route/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&originType=stopId&origin=522---2060603&destionationType=position&destination=47.62039,-122.34928&modeType=publicTransit&transitType=bus
    ```

3. Bei einer erfolgreichen Anforderung sollte die Antwortstruktur in etwa wie folgt aussehen:

    ```JSON
    {
        "results": [
            {
                "itineraryId": "cb6b6b6f-5cda-451e-b68d-2e97971dd60c---20190906BBBEC4D2219A436A9D794224978C9BBF:0---522",
                "departureTime": "2019-09-07T01:01:50Z",
                "arrivalTime": "2019-09-07T02:16:33Z",
                "travelTimeInSeconds": 4483,
                "numberOfLegs": 8,
                "legs": [
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T01:01:50Z",
                        "legEndTime": "2019-09-07T01:01:50Z",
                        "caption": "249"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T01:01:50Z",
                        "legEndTime": "2019-09-07T01:26:00Z",
                        "caption": "249",
                        "lengthInMeters": 9139
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T01:26:00Z",
                        "legEndTime": "2019-09-07T01:28:00Z",
                        "caption": "255"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T01:28:00Z",
                        "legEndTime": "2019-09-07T01:57:21Z",
                        "caption": "255",
                        "lengthInMeters": 13136
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-09-07T01:57:22Z",
                        "legEndTime": "2019-09-07T02:01:27Z",
                        "caption": "Denny Way",
                        "lengthInMeters": 308
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T02:01:27Z",
                        "legEndTime": "2019-09-07T02:06:33Z",
                        "caption": "8"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T02:06:33Z",
                        "legEndTime": "2019-09-07T02:12:41Z",
                        "caption": "8",
                        "lengthInMeters": 1060
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-09-07T02:12:42Z",
                        "legEndTime": "2019-09-07T02:16:33Z",
                        "lengthInMeters": 251
                    }
                ]
            },
            ...,
            {
                "itineraryId": "cb6b6b6f-5cda-451e-b68d-2e97971dd60c---20190906BBBEC4D2219A436A9D794224978C9BBF:2---522",
                "departureTime": "2019-09-07T00:49:32Z",
                "arrivalTime": "2019-09-07T02:20:06Z",
                "travelTimeInSeconds": 5434,
                "numberOfLegs": 10,
                "legs": [
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T00:49:32Z",
                        "legEndTime": "2019-09-07T00:49:32Z",
                        "caption": "226"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T00:49:32Z",
                        "legEndTime": "2019-09-07T01:15:00Z",
                        "caption": "226",
                        "lengthInMeters": 6792
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T01:15:00Z",
                        "legEndTime": "2019-09-07T01:20:00Z",
                        "caption": "241"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T01:20:00Z",
                        "legEndTime": "2019-09-07T01:28:00Z",
                        "caption": "241",
                        "lengthInMeters": 3397
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T01:28:00Z",
                        "legEndTime": "2019-09-07T01:33:00Z",
                        "caption": "550"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T01:33:00Z",
                        "legEndTime": "2019-09-07T01:58:00Z",
                        "caption": "550",
                        "lengthInMeters": 12899
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-09-07T01:58:01Z",
                        "legEndTime": "2019-09-07T01:59:21Z",
                        "caption": "4th Avenue South",
                        "lengthInMeters": 99
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T01:59:21Z",
                        "legEndTime": "2019-09-07T02:01:00Z",
                        "caption": "33"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T02:01:00Z",
                        "legEndTime": "2019-09-07T02:13:29Z",
                        "caption": "33,24",
                        "lengthInMeters": 2447
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-09-07T02:13:30Z",
                        "legEndTime": "2019-09-07T02:20:06Z",
                        "lengthInMeters": 457
                    }
                ]
            }
        ]
    }
    ```

4. Es ist zu erkennen, dass die Antwort mehrere Routen vom Typ **Bus** enthält. Jede Route verfügt über eine eindeutige Reiserouten-ID (**itinerary ID**) und eine Zusammenfassung, in der die einzelnen Etappen der Route beschrieben sind. Als Nächstes fordern wir Details zur schnellsten Route an, indem wir die `itineraryId` in der Antwort verwenden.

## <a name="request-fastest-route-itinerary"></a>Anfordern der schnellsten Reiseroute

Mit dem Dienst [Get Transit Itinerary](https://aka.ms/AzureMapsMobilityTransitItinerary) von Azure Maps können Sie Daten für eine bestimmte Route anfordern, indem Sie die **Reiserouten-ID** der Route verwenden, die von der [Get Transit Routes-API](https://aka.ms/AzureMapsMobilityTransitRoute) zurückgegeben wird. Führen Sie die folgenden Schritte aus, um eine Anforderung zu senden:

1. Klicken Sie in Postman auf **New Request (Neue Anforderung)**  | **GET request (GET-Anforderung)** , und nennen Sie sie **Get Transit info** (Verkehrsinformationen abrufen).

2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, geben Sie die folgende Anforderungs-URL für Ihren API-Endpunkt ein, und klicken Sie auf **Send** (Senden).

    Wir legen den Parameter `detailType` auf **geometry** fest, damit die Antwort Informationen zu Haltestellen von öffentlichen Verkehrsmitteln und eine genaue Wegbeschreibung für die Fußweg- und Fahrradabschnitte der Route enthält.

    ```HTTP
    https://atlas.microsoft.com/mobility/transit/itinerary/json?api-version=1.0&subscription-key={subscription-key}&query={itineraryId}&detailType=geometry
    ```
    
3. Bei einer erfolgreichen Anforderung sollte die Antwortstruktur in etwa wie folgt aussehen:

    ```JSON
    {
        "departureTime": "2019-09-07T01:01:50Z",
        "arrivalTime": "2019-09-07T02:16:33Z",
        "legs": [
            {
                "legType": "Wait",
                "legStartTime": "2019-09-07T01:01:50Z",
                "legEndTime": "2019-09-07T01:01:50Z",
                "lineGroup": {
                    "lineGroupId": "522---666077",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "caption1": "Overlake TC - South Bellevue P&R",
                    "caption2": "249 Overlake TC - South Bellevue P&R",
                    "color": "347E5D",
                    "transitType": "Bus"
                },
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R"
                },
                "stops": [
                    {
                        "stopId": "522---2060603",
                        "stopKey": "71300",
                        "stopName": "NE 24th St & 162nd Ave NE",
                        "stopCode": "71300",
                        "position": {
                            "latitude": 47.631504,
                            "longitude": -122.125275
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    },
                    {
                        "stopId": "522---2061703",
                        "stopKey": "74450",
                        "stopName": "South Kirkland P&R & 108th Ave NE",
                        "stopCode": "74450",
                        "position": {
                            "latitude": 47.643852,
                            "longitude": -122.196693
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    }
                ],
                "waitOnVehicle": false
            },
            {
                "legType": "Bus",
                "legStartTime": "2019-09-07T01:01:50Z",
                "legEndTime": "2019-09-07T01:26:00Z",
                "lineGroup": {
                    "lineGroupId": "522---666077",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "caption1": "Overlake TC - South Bellevue P&R",
                    "caption2": "249 Overlake TC - South Bellevue P&R",
                    "color": "347E5D",
                    "transitType": "Bus"
                },
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R"
                },
                "stops": [
                    {
                        "stopId": "522---2060603",
                        "stopKey": "71300",
                        "stopName": "NE 24th St & 162nd Ave NE",
                        "stopCode": "71300",
                        "position": {
                            "latitude": 47.631504,
                            "longitude": -122.125275
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    },
                    {
                        "stopId": "522---2060604",
                        "stopKey": "71310",
                        "stopName": "NE 24th St & 160th Ave NE",
                        "stopCode": "71310",
                        "position": {
                            "latitude": 47.631565,
                            "longitude": -122.127808
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    },
                    ...,
                    ...,
                    {
                        "stopId": "522---2061704",
                        "stopKey": "74451",
                        "stopName": "Northup Way & NE 33rd Pl",
                        "stopCode": "74451",
                        "position": {
                            "latitude": 47.640911,
                            "longitude": -122.194443
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    },
                    {
                        "stopId": "522---2061703",
                        "stopKey": "74450",
                        "stopName": "South Kirkland P&R & 108th Ave NE",
                        "stopCode": "74450",
                        "position": {
                            "latitude": 47.643852,
                            "longitude": -122.196693
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    }
                ],
                "geometry": {
                    "type": "LineString",
                    "coordinates": [
                        [
                            -122.12527,
                            47.63143
                        ],
                        [
                            -122.12529,
                            47.63143
                        ],
                        [
                            -122.12561,
                            47.63144
                        ],
                        [
                            -122.12701,
                            47.63148
                        ],
                        ...,
                        ...,
                        ...,
                        [
                            -122.19601,
                            47.64304
                        ],
                        [
                            -122.19584,
                            47.64315
                        ],
                        [
                            -122.19677,
                            47.6438
                        ]
                    ]
                }
            },
            ...,
            ...,
            ...,
            {
                "legType": "Walk",
                "legStartTime": "2019-09-07T02:12:42Z",
                "legEndTime": "2019-09-07T02:16:33Z",
                "steps": [
                    {
                        "direction": {
                            "relativeDirection": "depart"
                        },
                        "streetName": "Denny Way"
                    },
                    {
                        "direction": {
                            "relativeDirection": "right"
                        },
                        "streetName": "4th Avenue North"
                    },
                    {
                        "direction": {
                            "relativeDirection": "right"
                        },
                        "streetName": "Broad Street"
                    }
                ],
                "origin": {
                    "position": {
                        "latitude": 47.618578,
                        "longitude": -122.348058
                    }
                },
                "destination": {
                    "position": {
                        "latitude": 47.62039,
                        "longitude": -122.34928
                    }
                },
                "geometry": {
                    "type": "LineString",
                    "coordinates": [
                        [
                            -122.34806,
                            47.61857
                        ],
                        [
                            -122.3481,
                            47.61857
                        ],
                        [
                            -122.34894,
                            47.61858
                        ],
                        [
                            -122.34892,
                            47.61964
                        ],
                        [
                            -122.34877,
                            47.61975
                        ],
                        [
                            -122.3492,
                            47.62001
                        ],
                        [
                            -122.34918,
                            47.62003
                        ],
                        [
                            -122.34917,
                            47.62006
                        ],
                        [
                            -122.34916,
                            47.62008
                        ],
                        [
                            -122.34916,
                            47.62008
                        ],
                        [
                            -122.34916,
                            47.62008
                        ],
                        [
                            -122.34916,
                            47.62008
                        ],
                        [
                            -122.34928,
                            47.62039
                        ]
                    ]
                }
            }
        ]
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie Sie mithilfe des Mobility Service Echtzeitdaten anfordern:

> [!div class="nextstepaction"]
> [Anfordern von Echtzeitdaten](how-to-request-real-time-data.md)

Machen Sie sich mit der API-Dokumentation zum Mobility Service von Azure Maps vertraut:

> [!div class="nextstepaction"]
> [API-Dokumentation zum Mobility Service](https://aka.ms/AzureMapsMobilityService)

