---
title: Generieren von Karten
description: Es wird beschrieben, wie Sie mit FarmBeats Karten generieren.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 6cdebb8f6abea2b50dd8aad58ccc9fb5e680cba9
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797475"
---
# <a name="generate-maps"></a>Generieren von Karten

Mit Azure FarmBeats können Sie die folgenden Karten erstellen, indem Sie Satellitenbilder und Sensordateneingaben verwenden. Karten dienen Ihnen als Hilfe beim Anzeigen der geografischen Lage Ihres landwirtschaftlichen Betriebs und ermöglichen die Ermittlung der geeigneten Orte für Ihre Geräte.

  -  **Karte zur Sensorplatzierung** (Sensor Placement): Enthält Empfehlungen dazu, wie viele Sensoren verwendet und wo diese auf der Fläche eines landwirtschaftlichen Betriebs platziert werden sollten.
  - **Karte zu Satellitenindizes** (Satellite Indices): Enthält den Vegetations- und Wasserindex für einen landwirtschaftlichen Betrieb.
  - **Karte zur Bodenfeuchtigkeit** (Soil Moisture): Zeigt die Verteilung der Bodenfeuchtigkeit an, indem Satelliten- und Sensordaten zusammengefügt werden.

## <a name="sensor-placement-maps"></a>Karten zur Sensorplatzierung

Die Karte zur Sensorplatzierung von FarmBeats dient Ihnen als Hilfe beim Platzieren der Sensoren für die Messung der Bodenfeuchtigkeit. Die Ausgabe der Karte umfasst eine Liste mit den Koordinaten für die Sensorbereitstellung. Die Eingaben dieser Sensoren werden zusammen mit Satellitenbildern verwendet, um das Wärmebild für die Bodenfeuchtigkeit zu generieren.  

Die Ableitung dieser Karte wird durchgeführt, indem das an unterschiedlichen Tagen des Jahres aufgenommene Blätterdach segmentiert wird. Teil dieses „Blätterdachs“ sind hierbei auch freie Bodenflächen und Gebäude. Sie können Sensoren entfernen, die an einem bestimmten Ort nicht benötigt werden. Diese Karte dient zu Informationszwecken. Sie können die Position und die Zahlen basierend auf Ihrem vorhandenen Wissen leicht anpassen (das Hinzufügen von Sensoren führt nicht zu einer Regression bei den Ergebnissen des Wärmebilds zur Bodenfeuchtigkeit, aber die Genauigkeit des Wärmebilds wird ggf. verringert, wenn die Anzahl von Sensoren reduziert wird).  

## <a name="before-you-begin"></a>Voraussetzungen  

Stellen Sie Folgendes sicher, bevor Sie versuchen, eine Karte für die Sensorplatzierung zu generieren:

- Der landwirtschaftliche Betrieb ist größer als 4.000 Quadratmeter.
- Die Anzahl von Sentinel-Szenen mit wolkenlosem Himmel muss für den ausgewählten Datumsbereich mehr als sechs betragen.  
- Für mindestens sechs Sentinel-Szenen mit wolkenlosem Himmel muss NDVI >= 0,3 gelten.

    > [!NOTE]
    > Bei Sentinel sind nur zwei gleichzeitige Threads pro Benutzer zulässig. Aus diesem Grund werden Aufträge in die Warteschlange eingereiht, sodass ihre Abarbeitung ggf. länger dauern kann.

### <a name="dependencies-on-sentinel"></a>Abhängigkeiten von Sentinel  

Hier sind die Sentinel-Abhängigkeiten aufgeführt:

- Wir benötigen die Sentinel-Leistung zum Herunterladen von Satellitenbildern. Überprüfen Sie den Status der Sentinel-Leistung und die [Wartungsaktivitäten](https://scihub.copernicus.eu/twiki/do/view/SciHubNews/WebHome).
- Bei Sentinel sind nur zwei gleichzeitige [Download-Threads](https://sentinels.copernicus.eu/web/sentinel/sentinel-data-access/typologies-and-services) zulässig.
- Die Generierung von Präzisionskarten ist von der [Sentinel-Abdeckung und -Aufrufhäufigkeit]( https://sentinel.esa.int/web/sentinel/user-guides/sentinel-2-msi/revisit-coverage) abhängig.

## <a name="create-sensor-placement-map"></a>Erstellen der Karte für die Sensorplatzierung
In diesem Abschnitt werden die Verfahren zum Erstellen von Karten für die Sensorplatzierung ausführlich beschrieben.

> [!NOTE]
> Sie können die Sensorplatzierung über die Seite **Karten** oder auf der Seite **Farm Details** (Details zum landwirtschaftlichen Betrieb) im Dropdownmenü **Generate Precision Maps** (Präzisionskarten generieren) initiieren.

Führen Sie die folgenden Schritte aus:

1. Greifen Sie auf der Startseite im linken Navigationsmenü auf **Karten** zu.
2. Wählen Sie **Create Maps** (Karten erstellen) und im Dropdownmenü dann die Option **Sensor Placement** (Sensorplatzierung) aus.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-1.png)

3. Wählen Sie die Option **Sensor Placement** (Sensorplatzierung) aus.
  Das Fenster „Sensor Placement“ (Sensorplatzierung) wird angezeigt.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/sensor-placement-1.png)

4. Wählen Sie im Dropdownmenü **Farm** (Landwirtschaftlicher Betrieb) einen Betrieb aus.  
   Zum Ermitteln und Auswählen Ihres landwirtschaftlichen Betriebs können Sie entweder in der Dropdownliste scrollen oder den Namen im Textfeld eingeben.
5. Wählen Sie die Option **Recommended** (Empfohlen) aus, wenn Sie eine Karte für das letzte Jahr generieren möchten.
6. Gehen Sie wie folgt vor, wenn Sie eine Karte für einen benutzerdefinierten Datumsbereich generieren möchten: Wählen Sie die Option **Datumsbereich auswählen** aus, und geben Sie das Start- und Enddatum des Zeitraums an, für den Sie die Karte für die Sensorplatzierung generieren möchten.
7. Wählen Sie **Karten generieren** aus.
 Eine Bestätigungsmeldung mit den Auftragsdetails wird angezeigt.

  Weitere Informationen zum Auftragsstatus finden Sie unter **Aufträge anzeigen**. Wenn als Auftragsstatus *Fehler* angezeigt wird, wird in der QuickInfo des Status *Fehler* eine ausführliche Fehlermeldung angegeben. Wiederholen Sie in diesem Fall die obigen Schritte, und versuchen Sie es noch einmal.

  Falls das Problem weiterhin besteht, sollten Sie den Abschnitt zur [Problembehandlung](troubleshoot-project-farmbeats.md) lesen oder sich mit den relevanten Protokollen an das [Azure FarmBeats-Forum](https://aka.ms/FarmBeatsMSDN) wenden, um Hilfe zu erhalten.

### <a name="view-and-download-sensor-placement-map"></a>Anzeigen und Herunterladen der Karte für die Sensorplatzierung

Führen Sie die folgenden Schritte aus:

1. Greifen Sie auf der Startseite im linken Navigationsmenü auf **Karten** zu.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Wählen Sie im linken Navigationsbereich des Fensters die Option **Filter** aus.
  Im Fenster **Filter** werden Suchkriterien angezeigt.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3. Wählen Sie im Dropdownmenü die Optionen **Typ**, **Datum** und **Name** und dann die Option **Übernehmen** aus, um nach der anzuzeigenden Karte zu suchen.
  Das Erstellungsdatum des Auftrags wird im Format „typ_betriebsname_JJJJ-MM-TT“ angezeigt.
4. Scrollen Sie durch die Liste mit den verfügbaren Karten, indem Sie die Navigationsleisten am Ende der Seite verwenden.
5. Wählen Sie die Karte aus, die Sie anzeigen möchten. In einem Popupfenster wird eine Vorschau für die ausgewählte Karte angezeigt.
6. Wählen Sie **Herunterladen** aus, und laden Sie die GeoJSON-Datei mit den Sensorkoordinaten herunter.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/download-sensor-placement-map-1.png)

## <a name="satellite-indices-map"></a>Karte „Satellitenindizes“

In den folgenden Abschnitten werden die Verfahren zum Erstellen und Anzeigen einer Karte vom Typ „Satellitenindizes“ beschrieben.

> [!NOTE]
> Sie können die Karte mit den Satellitenindizes über die Seite **Karten** oder auf der Seite **Farm Details** (Details zum landwirtschaftlichen Betrieb) im Dropdownmenü **Generate Precision Maps** (Präzisionskarten generieren) initiieren.

Mit FarmBeats können Sie für landwirtschaftliche Betriebe Karten vom Typ „Normalized Difference Vegetation Index“ (NDVI), „Enhanced Vegetation Index“ (EVI) und „Normalized Difference Water Index“ (NDWI) generieren. Mit diesen Indizes können Sie ermitteln, wie die Pflanzen derzeit wachsen bzw. in der zurückliegenden Zeit gewachsen sind und welche repräsentativen Wasserstände der Boden aufweist.


> [!NOTE]
> Für die Tage, für die die Karte generiert wird, ist ein Sentinel-Bild erforderlich.


## <a name="create-satellite-indices-map"></a>Erstellen der Karte „Satellitenindizes“

Führen Sie die folgenden Schritte aus:

1. Greifen Sie auf der Startseite im linken Navigationsmenü auf **Karten** zu.
2. Wählen Sie die Option **Create Maps** (Karten erstellen) und dann im Dropdownmenü **Satellite Indices** (Satellitenindizes) aus.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-satellite-indices-1.png)

3. Wählen Sie **Satellite Indices** (Satellitenindizes) aus.
  Das Fenster für Satellitenindizes wird angezeigt.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/satellitte-indices-1.png)

4. Wählen Sie im Dropdownmenü einen landwirtschaftlichen Betrieb aus.  
   Zum Ermitteln und Auswählen Ihres landwirtschaftlichen Betriebs können Sie entweder in der Dropdownliste scrollen oder den Namen eingeben.   
5. Wählen Sie die Option **Diese Woche** aus, wenn Sie eine Karte für die letzte Woche generieren möchten.
6. Gehen Sie wie folgt vor, wenn Sie eine Karte für einen benutzerdefinierten Datumsbereich generieren möchten: Wählen Sie die Option **Datumsbereich auswählen** aus, und geben Sie das Start- und Enddatum des Zeitraums an, für den Sie die Karte „Satellitenindizes“ generieren möchten.
7. Wählen Sie **Karten generieren** aus.
    Eine Bestätigungsmeldung mit den Auftragsdetails wird angezeigt.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/successful-satellitte-indices-1.png)

    Weitere Informationen zum Auftragsstatus finden Sie unter **Aufträge anzeigen**. Wenn als Auftragsstatus *Fehler* angezeigt wird, wird in der QuickInfo des Status *Fehler* eine ausführliche Fehlermeldung angegeben. Wiederholen Sie in diesem Fall die obigen Schritte, und versuchen Sie es noch einmal.

    Falls das Problem weiterhin besteht, sollten Sie den Abschnitt zur [Problembehandlung](troubleshoot-project-farmbeats.md) lesen oder sich mit den relevanten Protokollen an das [Azure FarmBeats-Forum](https://aka.ms/FarmBeatsMSDN) wenden, um Hilfe zu erhalten.

### <a name="view-and-download-map"></a>Anzeigen und Herunterladen der Karte

Führen Sie die folgenden Schritte aus:

1. Greifen Sie auf der Startseite im linken Navigationsmenü auf **Karten** zu.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Wählen Sie im linken Navigationsbereich des Fensters die Option **Filter** aus. Im Fenster **Filter** werden Suchkriterien angezeigt.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3. Wählen Sie im Dropdownmenü die Optionen **Typ**, **Datum** und **Name** und dann die Option **Übernehmen** aus, um nach der anzuzeigenden Karte zu suchen.
  Das Erstellungsdatum des Auftrags wird im Format „typ_betriebsname_JJJJ-MM-TT“ angezeigt.

4. Scrollen Sie durch die Liste mit den verfügbaren Karten, indem Sie die Navigationsleisten am Ende der Seite verwenden.
5. Für jede Kombination aus **Name des landwirtschaftlichen Betriebs** und **Datum** sind die folgenden drei Karten verfügbar:
    - NDVI
    - EVI
    - NDWI
6. Wählen Sie die Karte aus, die Sie anzeigen möchten. In einem Popupfenster wird eine Vorschau für die ausgewählte Karte angezeigt.
7. Wählen Sie das Dropdownmenü **Herunterladen** aus, um das Downloadformat auszuwählen. Die Karte wird heruntergeladen und im lokalen Ordner Ihres Computers gespeichert.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/download-satellite-indices-map-1.png)

## <a name="get-soil-moisture-heatmap"></a>Abrufen eines Wärmebilds zur Bodenfeuchtigkeit

Die Bodenfeuchtigkeit ist das Wasser, das sich in den Zwischenräumen zwischen den Partikeln des Erdreichs befindet. Mit dem Wärmebild zur Bodenfeuchtigkeit können Sie sich für Ihren landwirtschaftlichen Betrieb einen Überblick über die Bodenfeuchtigkeitsdaten verschaffen – in jeder Tiefe und in hoher Auflösung. Zum Generieren eines präzisen und verwertbaren Wärmebilds zur Bodenfeuchtigkeit ist eine einheitliche Bereitstellung von Sensoren erforderlich, bei der alle Sensoren von demselben Anbieter stammen müssen. Wenn Geräte unterschiedlicher Anbieter genutzt werden, ergeben sich Unterschiede bei der Art und Weise, wie die Bodenfeuchtigkeit gemessen wird, sowie bei der Kalibrierung. Das Wärmebild wird für eine bestimmte Tiefe generiert, indem die in dieser Tiefe angeordneten Sensoren verwendet werden.

### <a name="before-you-begin"></a>Voraussetzungen  

Stellen Sie die Erfüllung der folgenden Voraussetzungen sicher, bevor Sie versuchen, ein Wärmebild zur Bodenfeuchtigkeit zu generieren:

- Es müssen mindestens drei Sensoren zur Ermittlung der Bodenfeuchtigkeit verwendet werden. Microsoft empfiehlt Ihnen, die Erstellung einer Karte zur Bodenfeuchtigkeit erst durchzuführen, nachdem die Sensoren bereitgestellt und dem landwirtschaftlichen Betrieb zugeordnet wurden.  
- Für den Tag, an dem die Erstellung der Karte zur Bodenfeuchtigkeit angefordert wurde, muss innerhalb der letzten 120 Tage mindestens eine Sentinel-Szene mit wolkenlosem Himmel verfügbar sein.
- Mindestens die Hälfte der auf der Fläche des landwirtschaftlichen Betriebs bereitgestellten Sensoren muss online sein und Daten an den Datenhub streamen.
- Das Wärmebild sollte aus den Messergebnissen von Sensoren desselben Anbieters generiert werden.


## <a name="create-soil-moisture-heatmap"></a>Erstellen eines Wärmebilds zur Bodenfeuchtigkeit

Führen Sie die folgenden Schritte aus:

1. Greifen Sie auf der Startseite im linken Navigationsmenü auf **Karten** zu, um die Seite „Karten“ anzuzeigen.
2. Wählen Sie die Option **Create Maps** (Karten erstellen) und im Dropdownmenü dann **Soil Moisture** (Bodenfeuchtigkeit) aus.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-soil-moisture-1.png)

3. Wählen Sie **Soil Moisture** (Bodenfeuchtigkeit) aus.
    Das Fenster „Soil Moisture“ (Bodenfeuchtigkeit) wird angezeigt.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/soil-moisture-1.png)

4. Wählen Sie im Dropdownmenü **Farm** (Landwirtschaftlicher Betrieb) einen Betrieb aus.  
   Zum Ermitteln und Auswählen Ihres landwirtschaftlichen Betriebs können Sie entweder in der Dropdownliste scrollen oder den Namen im Dropdownmenü „Select Farm“ (Landwirtschaftlichen Betrieb auswählen) eingeben.
5. Wählen Sie im Dropdownmenü **Select Soil Moisture Sensor Measure** (Maß für Bodenfeuchtigkeitssensor auswählen) das Maß für den Bodenfeuchtigkeitssensor aus, das Sie zum Generieren der Karte verwenden möchten.
Navigieren Sie zur Ermittlung des Sensormaßes zu **Sensors** (Sensoren), und wählen Sie einen beliebigen Bodenfeuchtigkeitssensor aus. Verwenden Sie dann im Abschnitt **Sensor Properties** (Sensoreigenschaften) den Wert für **Measure Name** (Name des Maßes).
6. Wählen Sie die entsprechende Option aus – je nachdem, ob Sie die Generierung für **Heute** oder **Diese Woche** durchführen möchten.
7. Gehen Sie wie folgt vor, wenn die Generierung für einen benutzerdefinierten Datumsbereich erfolgen soll: Wählen Sie die Option **Datumsbereich auswählen** aus, und geben Sie das Start- und Enddatum des Zeitraums an, für den Sie die Karte für die Bodenfeuchtigkeit generieren möchten.
8. Wählen Sie **Karten generieren** aus.
 Eine Bestätigungsmeldung mit den Auftragsdetails wird angezeigt.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/successful-soil-moisture-1.png)

    Weitere Informationen zum Auftragsstatus finden Sie unter **Aufträge anzeigen**. Wenn als Auftragsstatus *Fehler* angezeigt wird, wird in der QuickInfo des Status *Fehler* eine ausführliche Fehlermeldung angegeben. Wiederholen Sie in diesem Fall die obigen Schritte, und versuchen Sie es noch einmal.

    Falls das Problem weiterhin besteht, sollten Sie den Abschnitt zur [Problembehandlung](troubleshoot-project-farmbeats.md) lesen oder sich mit den relevanten Protokollen an das [Azure FarmBeats-Forum](https://aka.ms/FarmBeatsMSDN) wenden, um Hilfe zu erhalten.

### <a name="view-and-download-map"></a>Anzeigen und Herunterladen der Karte

Führen Sie die folgenden Schritte aus:

1. Greifen Sie auf der Startseite im linken Navigationsmenü auf **Karten** zu.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Wählen Sie im linken Navigationsbereich des Fensters die Option **Filter** aus. Im angezeigten Fenster **Filter** können Sie nach Karten suchen.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3.  Wählen Sie im Dropdownmenü die Optionen **Typ**, **Datum** und **Name** und dann die Option **Übernehmen** aus, um nach der anzuzeigenden Karte zu suchen. Das Erstellungsdatum des Auftrags wird im Format „typ_betriebsname_JJJJ-MM-TT“ angezeigt.
4. Wählen Sie das Symbol **Sortieren** neben den Tabellenüberschriften aus, um nach landwirtschaftlichem Betrieb, Datum, Erstellungsdatum, Auftrags-ID oder Auftragstyp zu sortieren.
5. Scrollen Sie durch die Liste mit den verfügbaren Karten, indem Sie die Navigationsschaltfläche am Ende der Seite verwenden.
6. Wählen Sie die Karte aus, die Sie anzeigen möchten. In einem Popupfenster wird eine Vorschau für die ausgewählte Karte angezeigt.
7. Wählen Sie das Dropdownmenü **Herunterladen** aus, um das Downloadformat auszuwählen. Die Karte wird heruntergeladen und im lokalen Ordner Ihres Computers gespeichert.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/download-soil-moisture-map-1.png)
