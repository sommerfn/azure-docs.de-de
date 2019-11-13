---
title: Generieren eines Wärmebilds zur Bodenfeuchtigkeit
description: Es wird beschrieben, wie Sie in Azure FarmBeats ein Wärmebild zur Bodenfeuchtigkeit generieren.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 61deb5361c9857956318f8b9b4ca836fcfa65d48
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797367"
---
# <a name="generate-soil-moisture-heatmap"></a>Generieren eines Wärmebilds zur Bodenfeuchtigkeit

Die Bodenfeuchtigkeit ist das Wasser, das sich in den Zwischenräumen zwischen den Partikeln des Erdreichs befindet. Mit dem Wärmebild zur Bodenfeuchtigkeit können Sie sich für Ihren landwirtschaftlichen Betrieb einen Überblick über die Feuchtigkeitsdaten verschaffen – in jeder Tiefe und in hoher Auflösung. Zum Generieren eines präzisen und verwertbaren Wärmebilds zur Bodenfeuchtigkeit ist eine einheitliche Bereitstellung von Sensoren desselben Anbieters erforderlich. Wenn Geräte unterschiedlicher Anbieter genutzt werden, ergeben sich Unterschiede bei der Art und Weise, wie die Bodenfeuchtigkeit gemessen wird, sowie bei der Kalibrierung. Das Wärmebild wird für eine bestimmte Tiefe generiert, indem die in dieser Tiefe angeordneten Sensoren verwendet werden.

In diesem Artikel wird der Prozess zum Generieren eines Wärmebilds zur Bodenfeuchtigkeit für Ihren landwirtschaftlichen Betrieb mit dem Azure FarmBeats Accelerator beschrieben. In diesem Artikel wird Folgendes behandelt:

- [Erstellen von landwirtschaftlichen Betrieben](#create-a-farm)
- [Zuweisen von Sensoren zu landwirtschaftlichen Betrieben](#get-soil-moisture-sensor-data-from-partner)
- [Generieren eines Wärmebilds zur Bodenfeuchtigkeit](#generate-soil-moisture-heatmap)

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie Folgendes sicher:  

- Ein Azure-Abonnement.
- Ausführung einer Instanz von Azure FarmBeats.
- Mindestens drei verfügbare Sensoren zur Ermittlung der Bodenfeuchtigkeit für den landwirtschaftlichen Betrieb.

## <a name="create-a-farm"></a>Erstellen eines landwirtschaftlichen Betriebs

Ein landwirtschaftlicher Betrieb ist der geografische Bereich, für den Sie ein Wärmebild zur Bodenfeuchtigkeit erstellen möchten. Sie können einen landwirtschaftlichen Betrieb erstellen, indem Sie die [Farms-API](https://aka.ms/FarmBeatsDatahubSwagger) oder die [Benutzeroberfläche von FarmsBeats Accelerator](manage-farms.md#create-farms) verwenden.

## <a name="deploy-sensors"></a>Bereitstellen von Sensoren

Sie sollten Bodenfeuchtigkeitssensoren physisch auf der Fläche des landwirtschaftlichen Betriebs bereitstellen. Bodenfeuchtigkeitssensoren erhalten Sie bei unseren autorisierten Partnerunternehmen: [Davis Instruments](https://www.davisinstruments.com/product/enviromonitor-gateway/) und [Teralytic](https://teralytic.com/). Wenden Sie sich wegen der physischen Bereitstellung auf der Fläche Ihres landwirtschaftlichen Betriebs an Ihren Sensoranbieter.

## <a name="get-soil-moisture-sensor-data-from-partner"></a>Abrufen der Sensordaten zur Bodenfeuchtigkeit vom Partner

Wenn die Sensoren mit dem Streaming beginnen, können die Daten über das Dashboard des Partners an Azure FarmBeats fließen. Dies kann über die Partneranwendung erfolgen.

Wenn Sie beispielsweise Sensoren von Davis erworben haben, melden Sie sich an Ihrem Weather Link-Konto an und geben die erforderlichen Anmeldeinformationen an, um das Streamen von Daten an Azure FarmBeats zu ermöglichen. Befolgen Sie die Anleitung unter [Abrufen von Sensordaten](get-sensor-data-from-sensor-partner.md#get-sensor-data-from-sensor-partners), um die erforderlichen Anmeldeinformationen zu erhalten.

Nachdem Sie Ihre Anmeldeinformationen eingegeben und in der Partneranwendung die Option zum **Absenden** ausgewählt haben, können die Daten an Azure FarmBeats fließen.

### <a name="assign-soil-moisture-sensors-to-the-farm"></a>Zuweisen von Bodenfeuchtigkeitssensoren zum landwirtschaftlichen Betrieb

Nachdem Sie Ihr Sensorkonto mit Azure FarmBeats verknüpft haben, müssen Sie die Bodenfeuchtigkeitssensoren dem gewünschten landwirtschaftlichen Betrieb zuweisen.

1.  Wählen Sie auf der Startseite im Menü die Option **Farms** (Landwirtschaftliche Betriebe) aus. Die Seite **Farms** (Landwirtschaftliche Betriebe) mit einer Liste wird angezeigt.
2.  Wählen Sie **MyFarm** > **Geräte hinzufügen** aus.
3.  Das Fenster **Geräte hinzufügen** wird angezeigt. Wählen Sie ein beliebiges Gerät aus, das mit den Bodenfeuchtigkeitssensoren Ihres landwirtschaftlichen Betriebs verbunden ist.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/add-devices-1.png)

4. Wählen Sie **Geräte hinzufügen** aus.     

## <a name="generate-soil-moisture-heatmap"></a>Generieren eines Wärmebilds zur Bodenfeuchtigkeit

Mit diesem Schritt erstellen Sie einen Auftrag oder einen langfristigen Vorgang, mit dem ein Wärmebild zur Bodenfeuchtigkeit für Ihren landwirtschaftlichen Betrieb generiert wird.

1.  Greifen Sie auf der Startseite im linken Navigationsmenü auf **Farms** (Landwirtschaftliche Betriebe) zu, um die Seite „Farms“ (Landwirtschaftliche Betriebe) anzuzeigen.
2.  Wählen Sie **MyFarm** aus.
3.  Wählen Sie auf der Seite **Farm Details** (Details zum landwirtschaftlichen Betrieb) die Option **Generate Precision Map** (Präzisionskarte generieren) aus.
4.  Wählen Sie im Dropdownmenü die Option **Soil Moisture** (Bodenfeuchtigkeit) aus.
5.  Wählen Sie im Fenster **Soil Moisture** (Bodenfeuchtigkeit) die Option **Diese Woche** aus.
6.  Geben Sie unter **Select Soil Moisture** **Sensor Measure** (Maß für Bodenfeuchtigkeitssensor auswählen) das Maß ein, das Sie für die Karte verwenden möchten.
    Wählen Sie zur Ermittlung der Sensormessung unter **Sensors** (Sensoren) einen beliebigen Bodenfeuchtigkeitssensor aus. Verwenden Sie unter **Sensor Properties** (Sensoreigenschaften) den Wert **Measure Name** (Name des Maßes) aus.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/soil-moisture-1.png)


7.  Wählen Sie **Karten generieren** aus.
    Eine Bestätigungsmeldung mit den Auftragsdetails wird angezeigt. Weitere Informationen finden Sie unter der Option „Auftragsstatus“ unter „Aufträge“.

    >[!NOTE]
    > Es dauert ungefähr drei bis vier Stunden, bis der Auftrag abgeschlossen ist.

### <a name="download-the-soil-moisture-heatmap"></a>Herunterladen des Wärmebilds für die Bodenfeuchtigkeit

Führen Sie die folgenden Schritte aus:

1. Überprüfen Sie auf der Seite **Aufträge** den **Auftragsstatus** für den Auftrag, den Sie im letzten Verfahren erstellt haben.
2. Klicken Sie im Menü auf **Karten**, wenn als Auftragsstatus *Erfolgreich* angezeigt wird.
3. Suchen Sie anhand des Tags der Erstellung nach der Karte (im Format „<bodenfeuchtigkeit_MyFarm_JJJJ-MM-TT>“).
4. Wählen Sie in der Spalte **Name** eine Karte aus. Ein Popupfenster mit einer Vorschau der ausgewählten Karte wird angezeigt.
5. Wählen Sie **Herunterladen** aus. Die Karte wird heruntergeladen und im lokalen Ordner Ihres Computers gespeichert.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/download-soil-moisture-map-1.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfolgreich ein Wärmebild zur Bodenfeuchtigkeit generiert haben, können Sie sich über das [Generieren der Sensorplatzierung](generate-maps.md#sensor-placement-maps) und die [Erfassung von Telemetrieverlaufsdaten](ingest-historical-telemetry-data.md) informieren. 
