---
title: Routenplanung für Elektrofahrzeuge mit Azure Notebooks (Python) | Microsoft-Dokumentation
description: Routenplanung für Elektrofahrzeuge mit Routenplanungs-APIs von Azure Maps und Azure Notebooks.
author: walsehgal
ms.author: v-musehg
ms.date: 10/01/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: c4b46bc952782fc7c9b56d6f0c049fe17b63d0f2
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2019
ms.locfileid: "71836326"
---
# <a name="electric-vehicle-routing-using-azure-notebooks-python"></a>Routenplanung für Elektrofahrzeuge mit Azure Notebooks (Python)

Azure Maps ist ein Portfolio von nativ in Azure integrierten Geodienst-APIs, mit denen Entwickler, Unternehmen und unabhängige Softwarehersteller (Independent Software Vendor, ISV) standortbezogene Apps sowie Lösungen für IoT, Mobilität, Logistik und Inventarüberwachung erstellen können. Die Rest-APIs von Azure Maps können in Programmiersprachen wie Python und R aufgerufen werden, um Szenarien mit Geodatenanalyse und maschinellem Lernen zu ermöglichen. Azure Maps bietet stabile [Routenplanungs-APIs](https://docs.microsoft.com/rest/api/maps/route), mit denen Benutzer Routen zwischen mehreren Datenpunkten basierend auf verschiedenen Bedingungen (z. B. Fahrzeugtyp oder erreichbarer Bereich) berechnen können. In diesem Tutorial wird ein Szenario erläutert, das es dem Fahrer eines Elektrofahrzeugs mit niedrigem Akkustand ermöglicht, die in Bezug auf die Fahrtzeit nächstgelegene Ladestation zu finden.

In diesem Lernprogramm führen Sie folgende Schritte aus:

> [!div class="checklist"]
> * Erstellen und Ausführen eines Jupyter-Notebooks in [Azure Notebooks](https://docs.microsoft.com/azure/notebooks) in der Cloud
> * Aufrufen von Azure Maps-Rest-APIs in Python
> * Suchen nach einem erreichbaren Bereich auf Grundlage des Verbrauchsmodells des Elektrofahrzeugs
> * Suchen nach Ladestationen für Elektrofahrzeuge im erreichbaren Bereich (bzw. in der Isochrone)
> * Rendern der Grenze des erreichbaren Bereichs und der Ladestationen auf einer Karte
> * Ermitteln und Visualisieren der Route zur nächstgelegenen Ladestation auf Grundlage der Zeit


## <a name="prerequisites"></a>Voraussetzungen 

Für die Schritte in diesem Tutorial müssen Sie zuerst ein Azure Maps-Konto erstellen und Ihren Primärschlüssel (Abonnementschlüssel) abrufen. Befolgen Sie die Anleitung unter [Verwalten Ihres Azure Maps-Kontos](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys#create-a-new-account), um ein Azure Maps-Kontoabonnement mit S1-Tarif zu erstellen. Führen Sie außerdem die Schritte unter [Suchen nach Points of Interest in der Nähe mit Azure Maps](./tutorial-search-location.md#getkey) aus, um den primären Abonnementschlüssel für Ihr Konto abzurufen.

## <a name="create-an-azure-notebook"></a>Erstellen eines Azure-Notebooks

Um die Schritte in diesem Tutorial ausführen zu können, müssen Sie ein Azure Notebooks-Projekt erstellen und die Jupyter Notebook-Datei herunterladen und ausführen. Die Notebook-Datei enthält Python-Code, der das Szenario in diesem Tutorial implementiert. Führen Sie die folgenden Schritte aus, um ein Azure Notebooks-Projekt zu erstellen und das Jupyter Notebook-Dokument in das Projekt hochzuladen.

1. Navigieren Sie zu [Azure Notebooks](https://notebooks.azure.com), und melden Sie sich an Weitere Informationen finden Sie im [Schnellstart](https://docs.microsoft.com/azure/notebooks/quickstart-sign-in-azure-notebooks).
2. Wählen Sie oben auf Ihrer öffentlichen Profilseite die Option **Meine Projekte** aus.

    ![Meine Projekte](./media/tutorial-ev-routing/myproject.png)

3. Wählen Sie auf der Seite **Meine Projekte** die Option **Neues Projekt** aus.
 
   ![Neues Projekt](./media/tutorial-ev-routing/create-project.png)

4. Geben Sie im angezeigten Popupfenster **Neues Projekt erstellen** folgende Informationen ein, und klicken Sie auf **Erstellen**:
    * Projektname
    * Projekt-ID
 
    ![Erstellen des Projekts](./media/tutorial-ev-routing/create-project-window.png)

5. Nachdem das Projekt erstellt wurde, laden Sie die [Jupyter Notebook-Dokumentdatei](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/blob/master/AzureMapsJupyterSamples/Tutorials/EV%20Routing%20and%20Reachable%20Range/EVrouting.ipynb) aus dem [Repository für Jupyter Notebook in Azure Maps](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook) herunter. 

6. Wählen Sie in der Projektliste auf der Seite **Meine Projekte** Ihr Projekt aus, und klicken Sie auf **Hochladen**, um die Jupyter Notebook-Dokumentdatei hochzuladen. Laden Sie die Datei von Ihrem Computer hoch, und klicken Sie auf **Fertig**.

    ![Hochladen des Notebooks](./media/tutorial-ev-routing/upload-notebook.png)

7. Wenn der Upload erfolgreich war, sehen Sie die Datei auf Ihrer Projektseite. Klicken Sie auf die Notebook-Datei, um sie als Jupyter-Notebook zu öffnen.

Wir empfehlen, den Code im Notebook Zelle für Zelle auszuführen, damit Sie die in der Notebook-Datei implementierten Funktionen besser nachvollziehen können. Sie können den Code in jeder Zelle ausführen, indem Sie oben in der Notebook-App auf die Schaltfläche **Ausführen** klicken.

  ![Run](./media/tutorial-ev-routing/run.png)

## <a name="install-project-level-packages"></a>Installieren von Paketen auf Projektebene

Um den Code im Notebook auszuführen, müssen Sie Pakete auf Projektebene installieren. Führen Sie die folgenden Schritte aus, um die erforderlichen Pakete zu installieren:

1. Laden Sie die Datei [requirements.txt](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/blob/master/AzureMapsJupyterSamples/Tutorials/EV%20Routing%20and%20Reachable%20Range/requirements.txt) aus dem [Repository für Jupyter Notebook in Azure Maps](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook) herunter, und laden Sie sie in Ihr Projekt hoch.
2. Klicken Sie auf dem Projektdashboard auf **Project Settings** (Projekteinstellungen). 
3. Wählen Sie im angezeigten Popupfenster die Registerkarte **Umgebung** aus, und klicken Sie dann auf **Hinzufügen**.
4. Unter **Environment Setup Steps** (Schritte zum Einrichten der Umgebung): 
    * Wählen Sie in der ersten Dropdownliste **Requirements.txt** aus.
    * Wählen Sie in der zweiten Dropdownliste Ihre Datei „requirements.txt“ aus.
    * Wählen Sie in der dritten Dropdownliste die Python-Version 3.6 aus.
7. Wählen Sie **Speichern** aus.

    ![Installieren der Pakete](./media/tutorial-ev-routing/install-packages.png)

## <a name="load-required-modules-and-frameworks"></a>Laden der erforderliche Module und Frameworks

Führen Sie das folgende Skript aus, um alle erforderlichen Module und Frameworks zu laden.

```python
import time
import aiohttp
import urllib.parse
from IPython.display import Image, display
```

## <a name="request-for-reachable-range-boundary"></a>Anforderung für die Grenze des erreichbaren Bereichs

In unserem Szenario hat ein Paketzusteller einige Elektrofahrzeuge in seiner Flotte. Im Laufe des Tages müssen die Elektrofahrzeuge aufgeladen werden, ohne dafür zum Lager zurückkehren zu müssen. Jedes Mal, wenn die verbleibende Akkuladung des Elektrofahrzeugs weniger als eine Stunde beträgt (niedriger Akkustand), müssen wir nach Ladestationen innerhalb des erreichbaren Bereichs suchen und die Grenzinformationen für diesen Bereich abrufen. Da der Paketzusteller Routen mit einem ausgewogenen Verhältnis zwischen Verbrauch und Geschwindigkeit bevorzugt, wird der Routentyp (routeType) „eco“ angefordert. Das folgende Skript ruft die [API zum Abrufen des Routenbereichs](https://docs.microsoft.com/rest/api/maps/route/getrouterange) des Azure Maps-Routenplanungsdiensts mit Parametern für das Verbrauchsmodell des Fahrzeugs auf und analysiert die Antwort, um ein Polygonobjekt im GeoJSON-Format zu erstellen, das den maximalen erreichbaren Bereich des Fahrzeugs darstellt.

Führen Sie das Skript in der folgenden Zelle aus, um die Grenzen des erreichbaren Bereichs des Elektrofahrzeugs abzurufen.

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


# Get bounds for the electric vehicle's reachable range.
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

## <a name="search-electric-vehicle-charging-stations-within-reachable-range"></a>Suchen nach Ladestationen für Elektrofahrzeuge im erreichbaren Bereich

Wenn wir den erreichbaren Bereich (die Isochrone) für das Elektrofahrzeug haben, können wir darin nach Ladestationen suchen. Das folgende Skript ruft die Azure Maps-[API zum Veröffentlichen einer Suche innerhalb der Geometrie](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) auf, um nach Ladestationen für Elektrofahrzeuge innerhalb der Grenzen des maximalen erreichbaren Bereichs des Fahrzeugs zu suchen, und analysiert dann die Antwort, um ein Array erreichbarer Standorte zu erstellen.

Führen Sie das folgende Skript aus, um nach Ladestationen für Elektrofahrzeuge im erreichbaren Bereich zu suchen.

```python
# Search for EV stations within reachable range.
searchPolyResponse = await (await session.post(url = "https://atlas.microsoft.com/search/geometry/json?subscription-key={}&api-version=1.0&query=electric vehicle station&idxSet=POI&limit=50".format(subscriptionKey), json = boundsData)).json() 

reachableLocations = []
for loc in range(len(searchPolyResponse["results"])):
                location = list(searchPolyResponse["results"][loc]["position"].values())
                location[0], location[1] = location[1], location[0]
                reachableLocations.append(location)
```

## <a name="upload-reachable-range-and-charging-points-to-azure-maps-data-service"></a>Hochladen des erreichbaren Bereichs und der Ladestationen in den Azure Maps-Datendienst

Um die Ladestationen und die Grenze des maximalen erreichbaren Bereichs des Elektrofahrzeugs auf der Karte zu visualisieren, müssen Sie die Daten zur Grenze und die Daten zu den Ladestationen mit der [Datenupload-API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) als GeoJSON-Objekte in den Azure Maps-Datendienst hochladen. 

Führen Sie die folgenden beiden Zellen aus, um die Daten zur Grenze und zu den Ladestationen in den Azure Maps-Datendienst hochzuladen.

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

# Upload range data to Azure Maps data service.
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

# Upload EV charging stations data to Azure Maps data service.
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

## <a name="render-charging-stations-and-reachable-range-on-map"></a>Rendern der Ladestationen und des erreichbaren Bereichs auf der Karte

Nachdem wir die Daten in den Datendienst hochgeladen haben, führen wir das folgende Skript aus, um den Azure Maps-[Dienst zum Abrufen des Kartenbilds](https://docs.microsoft.com/rest/api/maps/render/getmapimage) aufzurufen und die Ladestationen sowie die Grenze des maximalen erreichbaren Bereichs auf dem statischen Kartenbild zu rendern.

```python
# Get bounds for bounding box.
def getBounds(polyBounds):
    maxLon = max(map(lambda x: x[0], polyBounds))
    minLon = min(map(lambda x: x[0], polyBounds))

    maxLat = max(map(lambda x: x[1], polyBounds))
    minLat = min(map(lambda x: x[1], polyBounds))
    
    # Buffer the bounding box by 10% to account for the pixel size of pins at the ends of the route.
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

# Render range and EV charging points on the map.
staticMapResponse =  await session.get("https://atlas.microsoft.com/map/static/png?api-version=1.0&subscription-key={}&pins={}&path={}&bbox={}&zoom=12".format(subscriptionKey,encodedPins,path,str(minLon)+", "+str(minLat)+", "+str(maxLon)+", "+str(maxLat)))

poiRangeMap = await staticMapResponse.content.read()

display(Image(poiRangeMap))
```

![Standortbereich](./media/tutorial-ev-routing/location-range.png)


## <a name="find-the-optimal-charging-station-to-stop"></a>Ermitteln der optimalen Ladestation

Nachdem wir alle potenziellen Ladestationen im erreichbaren Bereich haben, möchten wir wissen, welche der Stationen am schnellsten zu erreichen ist. Das folgende Skript ruft die Azure Maps-[API für die Matrixroutenplanung](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview) auf, die die Fahrtzeit und Entfernung zu allen Ladestationen für die angegebene Fahrzeugposition zurückgibt. Das Skript in der nächsten Zelle analysiert die Antwort, um den Standort der in Bezug auf die Zeit nächstgelegenen erreichbaren Ladestation abzurufen.

Führen Sie die folgende Zelle aus, um die erreichbare Ladestation zu ermitteln, bei der die Fahrtzeit am kürzesten ist.

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

# Get the travel time and distance to every given charging station location.
searchPolyRes = await (await session.post(url = "https://atlas.microsoft.com/route/matrix/json?subscription-key={}&api-version=1.0&routeType=shortest&waitForResults=true".format(subscriptionKey), json = locationData)).json()

distances = []
for dist in range(len(reachableLocations)):
    distances.append(searchPolyRes["matrix"][0][dist]["response"]["routeSummary"]["travelTimeInSeconds"])

minDistLoc = []
minDistIndex = distances.index(min(distances))
minDistLoc.extend([reachableLocations[minDistIndex][1], reachableLocations[minDistIndex][0]])
closestChargeLoc = ",".join(str(i) for i in minDistLoc)
```

## <a name="calculate-route-to-the-closest-charging-station"></a>Berechnen der Route zur nächstgelegenen Ladestation

Nachdem wir die nächstgelegene Ladestation gefunden haben, rufen wir als Nächstes die [API zum Abrufen von Wegbeschreibungen](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) auf, um die genaue Route vom aktuellen Standort des Elektrofahrzeugs zur Ladestation anzufordern.

Führen Sie das Skript in der folgenden Zelle aus. Das Skript ruft die Route ab und analysiert die Antwort, um ein GeoJSON-Objekt zu erstellen, das die Route darstellt.

```python
# Get route from current location to the closest charging station. 
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

Zum Visualisieren der Route laden wir zunächst die Routendaten mit der [Datenupload-API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) von Azure Maps als GeoJSON-Objekt in den Azure Maps-Datendienst hoch. Anschließend rufen wir den Renderingdienst (die [API zum Abrufen des Kartenbilds](https://docs.microsoft.com/rest/api/maps/render/getmapimage)) auf, um die Route auf der Karte zu rendern und zu visualisieren.

Führen Sie das folgende Skript aus, um ein Bild für die gerenderte Route auf der Karte abzurufen.

```python
# Upload route data to Azure data service.
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


# Get bounds for bounding box.
minLat, maxLat = (float(destination[0]),currentLocation[0]) if float(destination[0])<currentLocation[0] else (currentLocation[0], float(destination[0]))
minLon, maxLon = (float(destination[1]),currentLocation[1]) if float(destination[1])<currentLocation[1] else (currentLocation[1], float(destination[1]))

#Buffer the bounding box by 10% to account for the pixel size of pins at the ends of the route.
lonBuffer = (maxLon-minLon)*0.1
minLon -= lonBuffer
maxLon += lonBuffer

latBuffer = (maxLat-minLat)*0.1
minLat -= latBuffer
maxLat += latBuffer

# Render route on the map.
staticMapResponse = await session.get("https://atlas.microsoft.com/map/static/png?api-version=1.0&subscription-key={}&&path={}&pins={}&bbox={}&zoom=16".format(subscriptionKey,path,pins,str(minLon)+", "+str(minLat)+", "+str(maxLon)+", "+str(maxLat)))

staticMapImage = await staticMapResponse.content.read()

await session.close()
display(Image(staticMapImage))
```

![Route](./media/tutorial-ev-routing/route.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie Rest-APIs von Azure Maps direkt aufrufen und Azure Maps-Daten mithilfe von Python visualisieren.

Informationen zu den in diesem Tutorial verwendeten Azure Maps-APIs finden Sie unter:

* [Abrufen des Routenbereichs](https://docs.microsoft.com/rest/api/maps/route/getrouterange)
* [Veröffentlichen einer Suche innerhalb der Geometrie](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)
* [Datenupload](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)
* [Rendern: Abrufen des Kartenbilds](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Veröffentlichen der Routenmatrix](https://docs.microsoft.com/rest/api/maps/route/postroutematrixpreview)
* [Abrufen von Wegbeschreibungen](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)

Eine vollständige Liste mit Azure Maps-REST-APIs finden Sie unter:

* [Azure Maps-REST-APIs](https://docs.microsoft.com/azure/azure-maps/#reference)

Weitere Informationen zu Azure Notebooks finden Sie hier:

* [Azure Notebooks](https://docs.microsoft.com/azure/notebooks)