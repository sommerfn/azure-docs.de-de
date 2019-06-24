---
title: Anfordern von Echtzeitdaten in Azure Maps | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Echtzeitdaten mithilfe von Azure Maps Mobility Service anfordern.
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: bb99a3c063f69aa5aeb00efdb51319a53d05b2d1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067603"
---
# <a name="request-real-time-data-using-the-azure-maps-mobility-service"></a>Anfordern von Echtzeitdaten mithilfe von Azure Maps Mobility Service

Dieser Artikel zeigt, wie Sie Azure Maps [Mobility Service](https://aka.ms/AzureMapsMobilityService) verwenden, um Echtzeitdaten zu öffentlichen Verkehrsmitteln bzw. Routen anzufordern.

In diesem Artikel lernen Sie Folgendes:


 * Anfordern der nächsten Ankunftszeiten für alle Linien, die an der angegebenen Haltestelle ankommen (in Echtzeit)
 * Anfordern von Echtzeitinformationen für eine bestimmte Fahrradstation


## <a name="prerequisites"></a>Voraussetzungen

Zum Aufrufen der APIs für öffentliche Verkehrsmittel bzw. Routen von Azure Maps benötigen Sie ein Maps-Konto und einen entsprechenden Schlüssel. Informationen zum Erstellen eines Kontos und Abrufen eines Schlüssels finden Sie unter [Verwalten Ihres Azure Maps-Kontos und der dazugehörigen Schlüssel](how-to-manage-account-keys.md).

In diesem Artikel wird die [Postman-App](https://www.getpostman.com/apps) zum Erstellen von REST-Aufrufen verwendet. Sie können jedoch auch Ihre bevorzugte API-Entwicklungsumgebung verwenden.


## <a name="request-real-time-arrivals-for-a-stop"></a>Anfordern der Ankunftszeiten für eine Haltestelle in Echtzeit

Um für eine bestimmte Haltestelle von öffentlichen Verkehrsmitteln Ankunftszeiten in Echtzeit anzufordern, müssen Sie eine Anforderung an die [API für Ankunftszeiten in Echtzeit](https://aka.ms/AzureMapsMobilityRealTimeArrivals) von Azure Maps [Mobility Service](https://aka.ms/AzureMapsMobilityService) richten. Für die Anforderung benötigen Sie die ID der Stadt (**metroID**) und die ID der Haltestelle (**stopID**). Weitere Informationen zum Anfordern dieser Parameter finden Sie in der [Anleitung zum Anfordern von Routen für den öffentlichen Nahverkehr](https://aka.ms/AMapsHowToGuidePublicTransitRouting). 

In unserem Beispiel verwenden wir die Stadt-ID „522“ (für den Bereich „Seattle-Tacoma-Bellevue, WA“) und die Haltestellen-ID „2060603“ (für die Bushaltestelle „Ne 24th St & 162nd Ave Ne, Bellevue WA“). Gehen Sie wie folgt vor, um Echtzeitdaten zu den nächsten fünf Ankunftszeiten dieser Haltestelle anzufordern:

1. Erstellen Sie eine Sammlung, in der die Anforderungen gespeichert werden. Klicken Sie in der Postman-App auf **New** (Neu). Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Collection** (Sammlung) aus. Geben Sie einen Namen für die Sammlung ein, und klicken Sie dann auf **Create** (Erstellen).

2. Klicken Sie erneut auf **New** (Neu), um die Anforderung zu erstellen. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **Request** (Anforderung) aus. Geben Sie einen Anforderungsnamen (**Request name**) für die Anforderung ein, wählen Sie die im vorherigen Schritt erstellte Sammlung als Speicherort für die Anforderung aus, und wählen Sie anschließend **Save** (Speichern) aus.

    ![Erstellen einer Anforderung in Postman](./media/how-to-request-transit-data/postman-new.png)

3. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-Methode „GET“ aus, und geben Sie die folgende URL ein, um eine GET-Anforderung zu erstellen.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=2060603&transitType=bus
    ```

4. War die Anforderung erfolgreich, erhalten Sie die folgende Antwort.  Der Parameter „-scheduleType“ definiert, ob die voraussichtliche Ankunftszeit auf Echtzeitdaten oder auf statischen Daten basiert.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 4,
                "scheduleType": "realTime",
                "patternId": 3860436,
                "line": {
                    "lineId": 2756599,
                    "lineGroupId": 666063,
                    "direction": "forward",
                    "agencyId": 5872,
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": 2060603,
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 30,
                "scheduleType": "scheduledTime",
                "patternId": 3860436,
                "line": {
                    "lineId": 2756599,
                    "lineGroupId": 666063,
                    "direction": "forward",
                    "agencyId": 5872,
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": 2060603,
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": 5872,
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }


## Real-time data for bike docking station

The [Get Transit Dock Info API](https://aka.ms/AzureMapsMobilityTransitDock) of the Azure Maps Mobility Service, allows to request static and real-time information such as availability and vacancy information for a given bike or scooter docking station. We will make a request to get real-time data for a docking station for bikes.

In order to make a request to the Get Transit Dock Info API, you will need the **dockId** for that station. You can get the dock ID by making a search request to the [Get Nearby Transit API](https://aka.ms/AzureMapsMobilityNearbyTransit) and setting the **objectType** parameter to "bikeDock". Follow the steps below to get real-time data of a docking station for bikes.


### Get dock ID

To get **dockID**, follow the steps below to make a request to the Get Nearby Transit API:

1. In Postman, click **New Request** | **GET request** and name it **Get dock ID**.

2.  On the Builder tab, select the **GET** HTTP method, enter the following request URL, and click **Send**.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=121&query=40.7663753,-73.9627498&radius=100&objectType=bikeDock
    ```

3. War die Anforderung erfolgreich, erhalten Sie die folgende Antwort. Beachten Sie, dass die Antwort nun die**ID** enthält. Diese können wir später als Abfrageparameter in der Anforderung verwenden, die wir an die API zum Anfordern von Stationsinformationen richten.

    ```JSON
    {
        "results": [
            {
                "id": "121---4640799",
                "type": "bikeDock",
                "objectDetails": {
                    "availableVehicles": 0,
                    "vacantLocations": 30,
                    "lastUpdated": "2019-05-21T20:06:59-04:00",
                    "operatorInfo": {
                        "id": "80",
                        "name": "Citi Bike"
                    }
                },
                "position": {
                    "latitude": 40.767128,
                    "longitude": -73.962243
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 40.768039,
                        "longitude": -73.963413
                    },
                    "btmRightPoint": {
                        "latitude": 40.766216,
                        "longitude": -73.961072
                    }
                }
            }
        ]
    }
    ```


### <a name="get-real-time-bike-dock-status"></a>Abrufen des Echtzeitstatus einer Fahrradstation

Gehen Sie wie folgt vor, um eine Anforderung an die API zum Anfordern von Stationsinformationen zu richten und Echtzeitdaten für die ausgewählte Station abzurufen.

1. Klicken Sie in Postman auf **New Request** (Neue Anforderung) | **GET request** (GET-Anforderung), und nennen Sie sie **Get real-time dock data** (Echtzeit-Stationsinformationen abrufen).

2.  Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, geben Sie die folgende Anforderungs-URL ein, und klicken Sie auf **Send** (Senden).
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/dock/json?subscription-key={subscription-key}&api-version=1.0&query=121---4640799
    ```

3. War die Anforderung erfolgreich, erhalten Sie eine Antwort mit folgender Struktur:

    ```JSON
    {
        "availableVehicles": 1,
        "vacantLocations": 29,
        "position": {
            "latitude": 40.767128,
            "longitude": -73.962246
        },
        "lastUpdated": "2019-05-21T20:26:47-04:00",
        "operatorInfo": {
            "id": "80",
            "name": "Citi Bike"
        }
    }
    ```


## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich, wie Sie mithilfe von Mobility Service Daten zu öffentlichen Verkehrsmitteln bzw. Routen anfordern:

> [!div class="nextstepaction"]
> [Anfordern von Daten zu öffentlichen Verkehrsmitteln bzw. Routen](how-to-request-transit-data.md)

Machen Sie sich mit der API-Dokumentation für Azure Maps Mobility Service vertraut:

> [!div class="nextstepaction"]
> [API-Dokumentation für Mobility Service](https://aka.ms/AzureMapsMobilityService)
