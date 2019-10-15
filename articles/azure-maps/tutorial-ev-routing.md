---
title: Routenplanung für Elektrofahrzeuge mit Azure Notebooks (Python) | Microsoft-Dokumentation
description: Enthält Informationen zur Routenplanung für Elektrofahrzeuge mit Azure Maps-Routenplanungs-APIs und Azure Notebooks.
author: walsehgal
ms.author: v-musehg
ms.date: 10/01/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 9cc7fc1ba8c7f55700505ea8fca0dea4f836e333
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243300"
---
# <a name="route-electric-vehicles-by-using-azure-notebooks-python"></a>Routenplanung für Elektrofahrzeuge mit Azure Notebooks (Python)

Azure Maps ist ein Portfolio mit Geodienst-APIs, die nativ in Azure integriert sind. Mit diesen APIs können Entwickler, Unternehmen und unabhängige Softwarehersteller (ISVs) standortbezogene Apps und IoT-Geräte sowie Lösungen für die Bereiche Mobilität, Logistik und Assetnachverfolgung erstellen. 

Die Rest-APIs von Azure Maps können in Programmiersprachen wie Python und R aufgerufen werden, um Szenarien mit Geodatenanalyse und maschinellem Lernen zu ermöglichen. Azure Maps verfügt über stabile [APIs für die Routenplanung](https://docs.microsoft.com/rest/api/maps/route), mit denen Benutzer Routen zwischen mehreren Datenpunkten berechnen können. Die Berechnungen basieren auf unterschiedlichen Bedingungen, z. B. Fahrzeugtyp oder erreichbarer Bereich. 

In diesem Tutorial wird Schritt für Schritt ein Szenario beschrieben, in dem ein Fahrer, dessen Elektrofahrzeug einen niedrigen Ladezustand aufweist, die nächstgelegene Ladestation ermitteln kann (basierend auf der Fahrtzeit vom Standort des Fahrzeugs aus).

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen und Ausführen eines Jupyter-Notebooks in [Azure Notebooks](https://docs.microsoft.com/azure/notebooks) in der Cloud
> * Aufrufen von Azure Maps-REST-APIs in Python
> * Suchen nach einem erreichbaren Bereich auf Grundlage des Verbrauchsmodells des Elektrofahrzeugs
> * Suchen nach Ladestationen für Elektrofahrzeuge im erreichbaren Bereich bzw. in der Isochrone
> * Rendern der Grenze des erreichbaren Bereichs und der Ladestationen auf einer Karte
> * Ermitteln und Visualisieren einer Route zur nächstgelegenen Ladestation basierend auf der Fahrtzeit


## <a name="prerequisites"></a>Voraussetzungen 

Für dieses Tutorial müssen Sie zuerst ein Azure Maps-Konto erstellen und Ihren Primärschlüssel (Abonnementschlüssel) abrufen. 

Befolgen Sie die Anleitung unter [Verwalten Ihres Azure Maps-Kontos: Erstellen eines neuen Kontos](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account), um im Tarif S1 ein Azure Maps-Kontoabonnement zu erstellen. 

Befolgen Sie zum Abrufen des primären Abonnementschlüssels für Ihr Konto die Anleitung unter [Suchen nach Points of Interest in der Nähe mit Azure Maps](./tutorial-search-location.md#getkey) .

## <a name="create-an-azure-notebook"></a>Erstellen eines Azure-Notebooks

Um die Schritte in diesem Tutorial ausführen zu können, müssen Sie ein Azure-Notebookprojekt erstellen und die Jupyter Notebook-Datei herunterladen und ausführen. Die Notebook-Datei enthält Python-Code, der das Szenario in diesem Tutorial implementiert. Gehen Sie wie folgt vor, um ein Azure-Notebookprojekt zu erstellen und das Jupyter Notebook-Dokument in das Projekt hochzuladen:

1. Navigieren Sie zu [Azure Notebooks](https://notebooks.azure.com), und melden Sie sich an Weitere Informationen finden Sie unter [Quickstart: Anmelden und Festlegen einer Benutzer-ID](https://docs.microsoft.com/azure/notebooks/quickstart-sign-in-azure-notebooks).
1. Wählen Sie oben auf Ihrer öffentlichen Profilseite die Option **Meine Projekte** aus.

    ![Schaltfläche „Meine Projekte“](./media/tutorial-ev-routing/myproject.png)

1. Wählen Sie auf der Seite **Meine Projekte** die Option **Neues Projekt** aus.
 
   ![Schaltfläche „Neues Projekt“](./media/tutorial-ev-routing/create-project.png)

1. Geben Sie im Bereich **Neues Projekt erstellen** einen Projektnamen und eine Projekt-ID ein.
 
    ![Bereich „Neues Projekt erstellen“](./media/tutorial-ev-routing/create-project-window.png)

1. Klicken Sie auf **Erstellen**.

1. Nachdem das Projekt erstellt wurde, laden Sie die [Jupyter Notebook-Dokumentdatei](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/blob/master/AzureMapsJupyterSamples/Tutorials/EV%20Routing%20and%20Reachable%20Range/EVrouting.ipynb) aus dem [Repository für Jupyter Notebook in Azure Maps](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook) herunter. 

1. Wählen Sie in der Projektliste auf der Seite **Meine Projekte** Ihr Projekt und dann die Option **Hochladen** aus, um die Jupyter Notebook-Dokumentdatei hochzuladen. 

    ![Hochladen des Notebooks](./media/tutorial-ev-routing/upload-notebook.png)

1. Laden Sie die Datei von Ihrem Computer hoch, und wählen Sie anschließend **Fertig** aus.

1. Nachdem der Upload erfolgreich abgeschlossen wurde, wird die Datei auf Ihrer Projektseite angezeigt. Wählen Sie die Datei aus, um sie als Jupyter-Notebook zu öffnen.

Wir empfehlen Ihnen, den Code im Notebook Zelle für Zelle auszuführen, damit Sie die in der Notebookdatei implementierten Funktionen besser nachvollziehen können. Sie können den Code in jeder Zelle ausführen, indem Sie oben in der Notebook-App die Schaltfläche **Ausführen** auswählen.

  ![Schaltfläche „Ausführen“](./media/tutorial-ev-routing/run.png)

## <a name="install-project-level-packages"></a>Installieren von Paketen auf Projektebene

Installieren Sie die Pakete wie folgt auf der Projektebene, um den Code im Notebook auszuführen:

1. Laden Sie die Datei [*requirements.txt*](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/blob/master/AzureMapsJupyterSamples/Tutorials/EV%20Routing%20and%20Reachable%20Range/requirements.txt) aus dem [Repository für Jupyter Notebook in Azure Maps](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook) herunter, und laden Sie sie dann in Ihr Projekt hoch.
1. Klicken Sie auf dem Projektdashboard auf **Project Settings** (Projekteinstellungen). 
1. Wählen Sie im Bereich **Projekteinstellungen** die Registerkarte **Umgebung** und dann die Option **Hinzufügen** aus.
1. Gehen Sie unter **Schritte zum Einrichten der Umgebung** wie folgt vor:   
    a. Wählen Sie in der ersten Dropdownliste **Requirements.txt** aus.  
    b. Wählen Sie in der zweiten Dropdownliste Ihre Datei *requirements.txt* aus.  
    c. Wählen Sie in der dritten Dropdownliste **Python Version 3.6** als Version aus.
1. Wählen Sie **Speichern** aus.

    ![Installieren von Paketen](./media/tutorial-ev-routing/install-packages.png)

## <a name="load-the-required-modules-and-frameworks"></a>Laden der erforderlichen Module und Frameworks

Führen Sie das folgende Skript aus, um alle erforderlichen Module und Frameworks zu laden:

```python
import time
import aiohttp
import urllib.parse
from IPython.display import Image, display
```

## <a name="request-the-reachable-range-boundary"></a>Anfordern der Grenze des erreichbaren Bereichs

In unserem Szenario verfügt ein Kurierunternehmen in seiner Flotte über einige Elektrofahrzeuge. Im Laufe des Tages müssen die Elektrofahrzeuge aufgeladen werden, ohne dafür zum Lager zurückkehren zu müssen. Immer wenn der Ladestand auf unter eine Stunde fällt (also die Batterie einen niedrigen Stand aufweist), führen Sie eine Suche nach Ladestationen durch, die innerhalb des erreichbaren Bereichs liegen, und rufen die Informationen zur Grenze des jeweiligen Bereichs ab. 

Da das Unternehmen die Nutzung von Routen bevorzugt, bei denen gleichermaßen auf Wirtschaftlichkeit und Geschwindigkeit geachtet wird, wird *eco* als angeforderter Routentyp (routeType) festgelegt. Mit dem folgenden Skript wird die [Get Route Range-API](https://docs.microsoft.com/rest/api/maps/route/getrouterange) des Azure Maps-Routingdiensts aufgerufen, indem Parameter für das Verbrauchsmodell des Fahrzeugs verwendet werden. Im Skript wird dann die Antwort analysiert, um ein Polygonobjekt im GEOJSON-Format zu erstellen, mit dem der maximal erreichbare Bereich des Fahrzeugs dargestellt wird.

Führen Sie das Skript in der folgenden Zelle aus, um die Grenzen für den erreichbaren Bereich des Elektrofahrzeugs zu ermitteln:

```python
subscriptionKey = "Your Azure Maps primary subscription key"
currentLocation = [34.028115,-118.5184279]
session = aiohttp.ClientSession()

# Parameters for the vehicle consumption model 
travelMode = "car"
vehicleEngineType = "electric"
currentChargeInkWh=45
maxChargeInkWh=80
timeBudgetInSec=550
routeType="eco"
constantSpeedConsumptionInkWhPerHundredkm="50,8.2:130,21.3"


# Get boundaries for the electric vehicle's reachable range.
routeRangeResponse = await (await session.get("https://atlas.microsoft.com/route/range/json?subscription-key={}&api-version=1.0&query={}&travelMode={}&vehicleEngineType={}&currentChargeInkWh={}&maxChargeInkWh={}&timeBudgetInSec={}&routeType={}&constantSpeedConsumptionInkWhPerHundredkm={}"
                                              .format(subscriptionKey,str(currentLocation[0])+","+str(currentLocation[1]),travelMode, vehicleEngineType, currentChargeInkWh, maxChargeInkWh, timeBudgetInSec, routeType, constantSpeedConsumptionInkWhPerHundredkm))).json()

polyBounds = routeRangeResponse["reachableRange"]["boundary"]

for i in range(len(polyBounds)):
    coordList = list(polyBounds[i].values())
    coordList[0], coordList[1] = coordList[1], coordList[0]
    polyBounds[i] = coordList

polyBounds.pop()
polyBounds.append(polyBounds[0])

boundsData = {
               "geometry": {
                 "type": "Polygon",
                 "coordinates": 
                   [
                      polyBounds
                   ]
                }
             }
```

## <a name="search-for-electric-vehicle-charging-stations-within-the-reachable-range"></a>Suchen nach Ladestationen für Elektrofahrzeuge im erreichbaren Bereich

Nachdem Sie den erreichbaren Bereich (Isochrone) für das Elektrofahrzeug ermittelt haben, können Sie innerhalb dieses Bereichs nach Ladestationen suchen. 

Mit dem folgenden Skript wird die [Post Search Inside Geometry-API](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) von Azure Maps aufgerufen. Hiermit wird innerhalb der Grenzen des maximal erreichbaren Bereichs eines Elektrofahrzeugs nach Ladestationen gesucht und anschließend die Antwort analysiert, um ein Array mit erreichbaren Standorten zu erhalten.

Führen Sie das folgende Skript aus, um im erreichbaren Bereich nach Ladestationen für Elektrofahrzeuge zu suchen:

```python
# Search for electric vehicle stations within reachable range.
searchPolyResponse = await (await session.post(url = "https://atlas.microsoft.com/search/geometry/json?subscription-key={}&api-version=1.0&query=electric vehicle station&idxSet=POI&limit=50".format(subscriptionKey), json = boundsData)).json() 

reachableLocations = []
for loc in range(len(searchPolyResponse["results"])):
                location = list(searchPolyResponse["results"][loc]["position"].values())
                location[0], location[1] = location[1], location[0]
                reachableLocations.append(location)
```

## <a name="upload-the-reachable-range-and-charging-points-to-azure-maps-data-service"></a>Hochladen des erreichbaren Bereichs und der Ladestationen in den Azure Maps-Datendienst

Es ist ratsam, auf einer Karte die Ladestationen und die Grenze für den maximal erreichbaren Bereich des Elektrofahrzeugs zu visualisieren. Laden Sie hierzu die Daten zur Grenze und zu den Ladestationen als GEOJSON-Objekte in den Azure Maps-Datendienst hoch, indem Sie die [Data Upload-API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) verwenden. 

Führen Sie die beiden folgenden Zellen aus, um die Daten zur Grenze und zu den Ladestationen in den Azure Maps-Datendienst hochzuladen:

```python
rangeData = {
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {},
      "geometry": {
        "type": "Polygon",
        "coordinates": [
          polyBounds
        ]
      }
    }
  ]
}

# Upload the range data to Azure Maps Data Service.
uploadRangeResponse = await session.post("https://atlas.microsoft.com/mapData/upload?subscription-key={}&api-version=1.0&dataFormat=geojson".format(subscriptionKey), json = rangeData)

rangeUdidRequest = uploadRangeResponse.headers["Location"]+"&subscription-key={}".format(subscriptionKey)

while True:
    getRangeUdid = await (await session.get(rangeUdidRequest)).json()
    if 'udid' in getRangeUdid:
        break
    else:
        time.sleep(0.2)
rangeUdid = getRangeUdid["udid"]
```

```python
poiData = {
    "type": "FeatureCollection",
    "features": [
      {
        "type": "Feature",
        "properties": {},
        "geometry": {
            "type": "MultiPoint",
            "coordinates": reachableLocations
        }
    }
  ]
}

# Upload the electric vehicle charging station data to Azure Maps Data Service.
uploadPOIsResponse = await session.post("https://atlas.microsoft.com/mapData/upload?subscription-key={}&api-version=1.0&dataFormat=geojson".format(subscriptionKey), json = poiData)

poiUdidRequest = uploadPOIsResponse.headers["Location"]+"&subscription-key={}".format(subscriptionKey)

while True:
    getPoiUdid = await (await session.get(poiUdidRequest)).json()
    if 'udid' in getPoiUdid:
        break
    else:
        time.sleep(0.2)
poiUdid = getPoiUdid["udid"]
```

## <a name="render-the-charging-stations-and-reachable-range-on-a-map"></a>Rendern der Ladestationen und des erreichbaren Bereichs auf einer Karte

Rufen Sie nach dem Hochladen der Daten in den Datendienst den [Dienst „Get Map Image“](https://docs.microsoft.com/rest/api/maps/render/getmapimage) von Azure Maps auf, um die Ladestationen sowie die Grenze des maximalen erreichbaren Bereichs auf dem statischen Kartenbild zu rendern:

```python
# Get boundaries for the bounding box.
def getBounds(polyBounds):
    maxLon = max(map(lambda x: x[0], polyBounds))
    minLon = min(map(lambda x: x[0], polyBounds))

    maxLat = max(map(lambda x: x[1], polyBounds))
    minLat = min(map(lambda x: x[1], polyBounds))
    
    # Buffer the bounding box by 10 percent to account for the pixel size of pins at the ends of the route.
    lonBuffer = (maxLon-minLon)*0.1
    minLon -= lonBuffer
    maxLon += lonBuffer

    latBuffer = (maxLat-minLat)*0.1
    minLat -= latBuffer
    maxLat += latBuffer
    
    return [minLon, maxLon, minLat, maxLat]

minLon, maxLon, minLat, maxLat = getBounds(polyBounds)

path = "lcff3333|lw3|la0.80|fa0.35||udid-{}".format(rangeUdid)
pins = "custom|an15 53||udid-{}||https://raw.githubusercontent.com/Azure-Samples/AzureMapsCodeSamples/master/AzureMapsCodeSamples/Common/images/icons/ev_pin.png".format(poiUdid)

encodedPins = urllib.parse.quote(pins, safe='')

# Render the range and electric vehicle charging points on the map.
staticMapResponse =  await session.get("https://atlas.microsoft.com/map/static/png?api-version=1.0&subscription-key={}&pins={}&path={}&bbox={}&zoom=12".format(subscriptionKey,encodedPins,path,str(minLon)+", "+str(minLat)+", "+str(maxLon)+", "+str(maxLat)))

poiRangeMap = await staticMapResponse.content.read()

display(Image(poiRangeMap))
```

![Karte mit dem Bereich um den Standort](./media/tutorial-ev-routing/location-range.png)


## <a name="find-the-optimal-charging-station"></a>Finden der optimalen Ladestation

Nachdem Sie alle potenziellen Ladestationen im erreichbaren Bereich ermittelt haben, möchten Sie wissen, welche davon am schnellsten zu erreichen ist. 

Mit dem folgenden Skript wird die [API für die Matrixroutenplanung](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview) von Azure Maps aufgerufen, die für den angegebenen Fahrzeugstandort die Fahrtzeit und Entfernung zu den einzelnen Ladestationen zurückgibt. Mit dem Skript in der nächsten Zelle wird die Antwort analysiert, um basierend auf der Fahrtzeit die nächstgelegene erreichbare Ladestation zu erhalten.

Führen Sie das Skript in der folgenden Zelle aus, um die nächstgelegene erreichbare Ladestation zu ermitteln, die am schnellsten erreichbar ist:

```python
locationData = {
            "origins": {
              "type": "MultiPoint",
              "coordinates": [[currentLocation[1],currentLocation[0]]]
            },
            "destinations": {
              "type": "MultiPoint",
              "coordinates": reachableLocations
            }
         }

# Get the travel time and distance to each specified charging station.
searchPolyRes = await (await session.post(url = "https://atlas.microsoft.com/route/matrix/json?subscription-key={}&api-version=1.0&routeType=shortest&waitForResults=true".format(subscriptionKey), json = locationData)).json()

distances = []
for dist in range(len(reachableLocations)):
    distances.append(searchPolyRes["matrix"][0][dist]["response"]["routeSummary"]["travelTimeInSeconds"])

minDistLoc = []
minDistIndex = distances.index(min(distances))
minDistLoc.extend([reachableLocations[minDistIndex][1], reachableLocations[minDistIndex][0]])
closestChargeLoc = ",".join(str(i) for i in minDistLoc)
```

## <a name="calculate-the-route-to-the-closest-charging-station"></a>Berechnen der Route zur nächstgelegenen Ladestation

Nachdem Sie nun die nächstgelegene Ladestation gefunden haben, können Sie die [API zum Abrufen von Wegbeschreibungen](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) aufrufen, um die genaue Route vom aktuellen Standort des Elektrofahrzeugs zur Ladestation anzufordern.

Führen Sie das Skript in der folgenden Zelle aus, um die Route zur Ladestation abzurufen und die Antwort zu analysieren, damit Sie ein GeoJSON-Objekt für die Route erhalten:

```python
# Get the route from the electric vehicle's current location to the closest charging station. 
routeResponse = await (await session.get("https://atlas.microsoft.com/route/directions/json?subscription-key={}&api-version=1.0&query={}:{}".format(subscriptionKey, str(currentLocation[0])+","+str(currentLocation[1]), closestChargeLoc))).json()

route = []
for loc in range(len(routeResponse["routes"][0]["legs"][0]["points"])):
                location = list(routeResponse["routes"][0]["legs"][0]["points"][loc].values())
                location[0], location[1] = location[1], location[0]
                route.append(location)

routeData = {
         "type": "LineString",
         "coordinates": route
     }
```

## <a name="visualize-the-route"></a>Visualisieren der Route

Zum Visualisieren der Route laden Sie zunächst die Routendaten als GeoJSON-Objekt in den Azure Maps-Datendienst hoch, indem Sie die [API für den Datenupload](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) von Azure Maps verwenden. Anschließend rufen Sie den Renderingdienst ([API zum Abrufen des Kartenbilds](https://docs.microsoft.com/rest/api/maps/render/getmapimage)) auf, um die Route auf der Karte zu rendern und zu visualisieren.

Führen Sie das folgende Skript aus, um ein Bild mit der gerenderten Route auf der Karte abzurufen:

```python
# Upload the route data to Azure Maps Data Service.
routeUploadRequest = await session.post("https://atlas.microsoft.com/mapData/upload?subscription-key={}&api-version=1.0&dataFormat=geojson".format(subscriptionKey), json = routeData)

udidRequestURI = routeUploadRequest.headers["Location"]+"&subscription-key={}".format(subscriptionKey)

while True:
    udidRequest = await (await session.get(udidRequestURI)).json()
    if 'udid' in udidRequest:
        break
    else:
        time.sleep(0.2)

udid = udidRequest["udid"]

destination = route[-1]

destination[1], destination[0] = destination[0], destination[1]

path = "lc0f6dd9|lw6||udid-{}".format(udid)
pins = "default|codb1818||{} {}|{} {}".format(str(currentLocation[1]),str(currentLocation[0]),destination[1],destination[0])


# Get boundaries for the bounding box.
minLat, maxLat = (float(destination[0]),currentLocation[0]) if float(destination[0])<currentLocation[0] else (currentLocation[0], float(destination[0]))
minLon, maxLon = (float(destination[1]),currentLocation[1]) if float(destination[1])<currentLocation[1] else (currentLocation[1], float(destination[1]))

# Buffer the bounding box by 10 percent to account for the pixel size of pins at the ends of the route.
lonBuffer = (maxLon-minLon)*0.1
minLon -= lonBuffer
maxLon += lonBuffer

latBuffer = (maxLat-minLat)*0.1
minLat -= latBuffer
maxLat += latBuffer

# Render the route on the map.
staticMapResponse = await session.get("https://atlas.microsoft.com/map/static/png?api-version=1.0&subscription-key={}&&path={}&pins={}&bbox={}&zoom=16".format(subscriptionKey,path,pins,str(minLon)+", "+str(minLat)+", "+str(maxLon)+", "+str(maxLat)))

staticMapImage = await staticMapResponse.content.read()

await session.close()
display(Image(staticMapImage))
```

![Karte mit der Route](./media/tutorial-ev-routing/route.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie REST-APIs von Azure Maps direkt aufrufen und Azure Maps-Daten mit Python visualisieren.

Informationen zu den in diesem Tutorial verwendeten Azure Maps-APIs finden Sie unter:

* [Abrufen des Routenbereichs](https://docs.microsoft.com/rest/api/maps/route/getrouterange)
* [Veröffentlichen einer Suche innerhalb der Geometrie](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)
* [Datenupload](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)
* [Rendern: Abrufen des Kartenbilds](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Veröffentlichen der Routenmatrix](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview)
* [Abrufen von Wegbeschreibungen](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)

Eine vollständige Liste mit den Azure Maps-REST-APIs finden Sie unter [Azure Maps-Dokumentation](https://docs.microsoft.com/azure/azure-maps/#reference).

Weitere Informationen zu Azure Notebooks finden Sie unter [Azure Notebooks-Dokumentation](https://docs.microsoft.com/azure/notebooks).
