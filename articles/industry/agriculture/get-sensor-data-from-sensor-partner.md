---
title: Abrufen der Sensordaten von Partnern
description: Es wird beschrieben, wie Sie Sensordaten von Partnern abrufen.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 33fd2221d3f3a0562abf83f014a7b84df972eebc
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74128846"
---
# <a name="get-sensor-data-from-sensor-partners"></a>Abrufen der Sensordaten von Sensorpartnern

Mit Azure FarmBeats können Sie Streamingdaten von Ihren IoT-Geräten und -Sensoren an den Datenhub übertragen. Derzeit werden die folgenden Partnerunternehmen für Sensorgeräte unterstützt:

  ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/partner-information-1.png)

Durch die Integration von Gerätedaten in Azure FarmBeats können Sie Bodendaten von den IoT-Sensoren, die auf der Fläche Ihres landwirtschaftlichen Betriebs angeordnet sind, an Ihren Datenhub übertragen. Wenn die Daten verfügbar sind, können sie mit dem FarmBeats Accelerator visualisiert und für die Datenfusion und Erstellung von KI/ML-Modellen mit FarmBeats genutzt werden.

Stellen Sie Folgendes sicher, bevor Sie mit dem Streamen von Sensordaten beginnen:

-  Sie haben FarmBeats über Azure Marketplace installiert.
-  Sie haben die Sensoren und Geräte ausgewählt, die Sie auf der Fläche Ihres landwirtschaftlichen Betriebs installieren möchten.
-  Falls Sie den Einsatz von Sensoren zur Ermittlung der Bodenfeuchtigkeit planen, können Sie die FarmBeats-Karte zur Platzierung von Bodenfeuchtigkeitssensoren verwenden. Sie erhalten hierbei eine Empfehlung zur Anzahl von Sensoren und ihrer genauen Platzierung. Weitere Informationen finden Sie unter [Generieren von Karten](generate-maps.md).

- Erwerben Sie Geräte und Sensoren von Ihrem Gerätepartner, und stellen Sie sie auf der Fläche Ihres landwirtschaftlichen Betriebs bereit. Vergewissern Sie sich, dass Sie mit der Lösung Ihres Gerätepartners auf die Sensordaten zugreifen können.

### <a name="enable-device-integration-with-farmbeats"></a>Ermöglichen der Geräteintegration mit FarmBeats   

Nachdem Sie mit dem Streamen von Sensordaten begonnen haben, können Sie den Prozess zum Abrufen der Daten für Ihr FarmBeats-System starten. Sie müssen Ihrem Geräteanbieter die folgenden Informationen bereitstellen, um die Integration in FarmBeats zu ermöglichen:  

 - API-Endpunkt  
 - Mandanten-ID  
 - Client-ID  
 - Geheimer Clientschlüssel  
 - EventHub-Verbindungszeichenfolge

Die obigen Informationen erhalten Sie von Ihrem Systemintegrator. Wenden Sie sich an Ihren Systemintegrator, falls Sie Probleme beim Aktivieren der Geräteintegrationen haben.

Alternativ können Sie die Anmeldeinformationen auch generieren, indem Sie dieses Skript über die Azure Cloud Shell ausführen. Führen Sie die folgenden Schritte aus:

1. Laden Sie die [ZIP-Datei](https://aka.ms/farmbeatspartnerscript) herunter, und extrahieren Sie sie auf Ihrem lokalen Laufwerk. Die ZIP-Datei enthält zwei Dateien.
2. Melden Sie sich bei https://portal.azure.com/ an, und öffnen Sie Cloud Shell. (Die entsprechende Option finden Sie auf der Leiste oben rechts im Portal.)  

    ![FarmBeats-Projekt](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

3. Stellen Sie sicher, dass die Umgebung auf **PowerShell** festgelegt ist. Die Standardeinstellung ist „Bash“.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/power-shell-new-1.png)

4. Laden Sie die beiden Dateien (oben aus Schritt 1) in Ihre Cloud Shell-Instanz hoch.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/power-shell-two-1.png)

5. Navigieren Sie zu dem Verzeichnis, in das die Dateien hochgeladen wurden (standardmäßig „basisverzeichnis>benutzername“).
6. Führen Sie das folgende Skript aus:

    ```azurepowershell-interactive 

    ./generateCredentials.ps1   

    ```
7. Befolgen Sie die Anweisungen auf dem Bildschirm, um die Werte zu erfassen. (Dies sind die Werte für API-Endpunkt, Mandanten-ID, Client-ID, geheimer Clientschlüssel und EventHub-Verbindungszeichenfolge.)

**Integrieren von Gerätedaten mit den generierten Anmeldeinformationen**

Greifen Sie auf das Portal des Gerätepartners zu, um die Verknüpfung mit FarmBeats mit den Anmeldeinformationen durchzuführen, die Sie im vorherigen Abschnitt generiert haben.

 - API-Endpunkt  
 - EventHub-Verbindungszeichenfolge  
 - Client-ID  
 - Geheimer Clientschlüssel  
 - Mandanten-ID  

 Die erfolgreiche Integration wird vom Geräteanbieter bestätigt. Nach Erhalt der Bestätigung können Sie in Azure FarmBeats alle Geräte und Sensoren anzeigen.

## <a name="view-devices-and-sensors"></a>Anzeigen von Geräten und Sensoren

Verwenden Sie den folgenden Abschnitt, um die Geräte und Sensoren Ihres landwirtschaftlichen Betriebs anzuzeigen.

### <a name="view-devices"></a>Anzeigen von Geräten

Derzeit werden für FarmBeats die folgenden Geräte unterstützt:

- **Knoten**: Ein Gerät, an das mindestens ein Sensor angefügt ist.
- **Gateway**: Ein Gerät, an das mindestens ein Knoten angefügt ist.

Führen Sie die folgenden Schritte aus:

1. Wählen Sie auf der Startseite im Menü die Option **Geräte** aus.
  Auf der Seite „Geräte“ werden Typ, Modell und Status des Geräts, der zugehörige landwirtschaftliche Betrieb und das letzte Aktualisierungsdatum der Metadaten angezeigt. Standardmäßig ist die Spalte für den landwirtschaftlichen Betrieb auf NULL festgelegt. Sie können angeben, dass ein Gerät einem landwirtschaftlichen Betrieb zugewiesen werden soll. Weitere Informationen finden Sie unter [Zuweisen von Geräten](#assign-devices).
2. Wählen Sie das Gerät aus, um die Geräteeigenschaften, Telemetriedaten und untergeordneten Geräte anzuzeigen, die mit dem Gerät verbunden sind.  

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/view-devices-1.png)

### <a name="view-sensors"></a>Anzeigen von Sensoren

Führen Sie die folgenden Schritte aus:

1. Wählen Sie auf der Startseite im Menü die Option **Sensoren** aus.
  Auf der Seite „Sensoren“ werden Details wie Sensortyp, zugehöriger landwirtschaftlicher Betrieb, übergeordnetes Gerät, Portname, Porttyp und letzte Aktualisierung angezeigt.
2. Wählen Sie den Sensor aus, um Sensoreigenschaften, aktive Warnungen und Telemetriedaten für den Sensor anzuzeigen.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/view-sensors-1.png)

## <a name="assign-devices"></a>Zuweisen von Geräten  

Wenn die Sensordaten fließen, können Sie sie dem landwirtschaftlichen Betrieb zuweisen, für den Sie die Sensoren bereitgestellt haben.

1. Wählen Sie auf der Startseite im Menü die Option **Farms** (Landwirtschaftliche Betriebe) aus. Die Seite **Farms** (Landwirtschaftliche Betriebe) mit einer Liste wird angezeigt.  
2. Wählen Sie den landwirtschaftlichen Betrieb aus, dem Sie das Gerät zuweisen möchten, und wählen Sie die Option **Geräte hinzufügen** aus.  
3. Das Fenster **Geräte hinzufügen** wird angezeigt. Wählen Sie das Gerät aus, das Sie dem landwirtschaftlichen Betrieb zuweisen möchten.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/add-devices-1.png)

4. Wählen Sie **Geräte hinzufügen** aus. Alternativ können Sie zum Menü **Geräte** navigieren, die Geräte auswählen, die einem landwirtschaftlichen Betrieb zugewiesen werden sollen, und dann **Geräte zuordnen** auswählen.  
5. Wählen Sie im Fenster **Geräte zuordnen** in der Dropdownliste den landwirtschaftlichen Betrieb und dann **Auf alle anwenden** aus, um den landwirtschaftlichen Betrieb allen ausgewählten Geräten zuzuordnen.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/associate-devices-1.png)

6. Um jedes Gerät einem anderen landwirtschaftlichen Betrieb zuzuordnen, wählen Sie die Dropdownliste in der Spalte **Assign to Farm** (Landwirtschaftlichem Betrieb zuweisen) und dann einen landwirtschaftlichen Betrieb für jede Gerätezeile aus.  
7. Wählen Sie **Zuweisen** aus, um die Zuweisung der Geräte durchzuführen.

### <a name="visualize-sensor-data"></a>Visualisieren von Sensordaten

Führen Sie die folgenden Schritte aus:

1. Wählen Sie auf der Startseite im Menü die Option **Farms** (Landwirtschaftliche Betriebe) aus, um die Seite **Farms** (Landwirtschaftliche Betriebe) anzuzeigen.  
2. Wählen Sie den **landwirtschaftlichen Betrieb** aus, für den Sie die Sensordaten anzeigen möchten.  
3. Im Dashboard **Farm** (Landwirtschaftlicher Betrieb) können Sie die Telemetriedaten anzeigen. Sie können auch Livetelemetriedaten anzeigen oder **Benutzerdefinierter Bereich** verwenden, um einen bestimmten Datumsbereich für die Anzeige anzugeben.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/telemetry-data-1.png)

## <a name="delete-sensor"></a>Löschen eines Sensors

Folgen Sie diesen Schritten:

1. Wählen Sie auf der Startseite im Menü die Option **Sensoren** aus, um die Seite **Sensoren** zu öffnen.  
2. Wählen Sie das zu löschende Gerät und im Bestätigungsfenster dann die Option **Löschen** aus.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/delete-sensors-1.png)

Eine Bestätigungsmeldung mit dem Hinweis, dass das Löschen des Sensors erfolgreich war, wird angezeigt.  

## <a name="delete-devices"></a>Löschen von Geräten

Folgen Sie diesen Schritten:

1. Wählen Sie auf der Startseite im Menü die Option **Geräte** aus, um die Seite „Geräte“ zu öffnen.  
2. Wählen Sie das zu löschende Gerät und im Bestätigungsfenster dann die Option **Löschen** aus.

    ![FarmBeats-Projekt](./media/get-sensor-data-from-sensor-partner/delete-device-1.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun die Übertragung von Sensordaten an Ihre Azure FarmBeats-Instanz eingerichtet. Als Nächstes können Sie sich damit vertraut machen, wie Sie [Karten für Ihre landwirtschaftlichen Betriebe generieren](generate-maps.md#generate-maps).
