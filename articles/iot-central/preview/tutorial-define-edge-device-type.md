---
title: Definieren eines neuen Azure IoT Edge-Gerätetyps in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Tutorial für Lösungsentwickler erfahren Sie, wie Sie in Ihrer Azure IoT Central-Anwendung ein neues Azure IoT Edge-Gerät erstellen. Sie definieren die Telemetriedaten, den Zustand, die Eigenschaften und die Befehle für den Typ.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: c205b4dd4871ed53e32dce72f12cc2dcfb3baf41
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73892026"
---
# <a name="tutorial-define-a-new-azure-iot-edge-device-type-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Definieren eines neuen Azure IoT Edge-Gerätetyps in Ihrer Azure IoT Central-Anwendung (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Tutorial für Ersteller erfahren Sie, wie Sie in Ihrer Azure IoT Central-Anwendung mithilfe einer Gerätevorlage eine neue Art von Azure IoT Edge-Gerät definieren. 

Einen Überblick über Azure IoT Edge finden Sie in [diesem Artikel](overview-iot-central.md). 

Azure IoT Edge besteht aus drei Komponenten:
* **IoT Edge-Module** sind Container, die Azure-Dienste, Drittanbieterdienste oder Ihren eigenen Code ausführen. Module werden auf IoT Edge-Geräten bereitgestellt und lokal auf diesen Geräten ausgeführt.
* Die **IoT Edge-Runtime** wird auf jedem IoT Edge-Gerät ausgeführt und dient zum Verwalten der Module, die auf einem Gerät jeweils bereitgestellt wurden.
* Mit einer **cloudbasierten Schnittstelle** können Sie für IoT Edge-Geräte die Remoteüberwachung und -verwaltung durchführen. IoT Central bildet die Cloudschnittstelle.

Ein **Azure IoT Edge**-Gerät kann ein Gatewaygerät mit nachgeschalteten Geräten sein, die eine Verbindung mit dem Azure IoT Edge-Gerät herstellen. Konnektivitätsmuster für nachgeschaltete Geräte werden in diesem Tutorial behandelt.

Mit einer **Gerätevorlage** werden die Funktionen Ihrer Geräte- und IoT Edge-Module definiert. Zu den Funktionen zählen die vom Modul gesendeten Telemetriedaten, die Moduleigenschaften und die Befehle, auf die ein Modul reagiert.

In diesem Tutorial erstellen Sie die Gerätevorlage **Environment Sensor** (Umgebungssensor). Ein Umgebungssensorgerät ermöglicht Folgendes:

* Senden von Telemetriedaten, z. B. Temperatur
* Reagieren auf schreibbare Eigenschaften, wenn diese in der Cloud aktualisiert werden (Beispiel: Sendeintervall für Telemetriedaten)
* Reagieren auf Befehle (Beispiel: Zurücksetzen der Temperatur)

In diesem Tutorial wird auch eine Gerätevorlage für ein **Umgebungsgateway** erstellt. Ein Umgebungsgatewaygerät ermöglicht Folgendes:

* Senden von Telemetriedaten, z. B. Temperatur
* Reagieren auf schreibbare Eigenschaften, wenn diese in der Cloud aktualisiert werden (Beispiel: Sendeintervall für Telemetriedaten)
* Reagieren auf Befehle (Beispiel: Zurücksetzen der Temperatur)
* Zulassen von Beziehungen mit anderen Gerätefunktionsmodellen


In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer neuen Gerätevorlage für Azure IoT Edge-Geräte
> * Hochladen eines Bereitstellungsmanifests
> * Erstellen von Funktionen mit Telemetriedaten, Eigenschaften und Befehlen für die einzelnen Module
> * Definieren einer Visualisierung für die Modultelemetriedaten
> * Hinzufügen von Beziehungen mit Vorlagen für nachgeschaltete Geräte
> * Veröffentlichen Ihrer Gerätevorlage

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine Azure IoT Central-Anwendung. Gehen Sie wie in [dieser Schnellstartanleitung](quick-deploy-iot-central.md) beschrieben vor, um eine Azure IoT Central-Anwendung zu erstellen.


## <a name="downstream-device-relationships-with-gateway--modules"></a>Beziehungen nachgeschalteter Geräte mit dem Gateway und den Modulen

Nachgeschaltete Geräte können über das Modul „$edgeHub“ eine Verbindung mit dem Azure IoT Edge-Gatewaygerät herstellen. In diesem Szenario wird das Azure IoT Edge-Gerät zu einem transparenten Gateway.

![Zentrale Anwendungsseite](./media/tutorial-define-edge-device-type/gateway-transparent.png)

Nachgeschaltete Geräte können über ein benutzerdefiniertes Modul eine Verbindung mit dem Azure IoT Edge-Gatewaygerät herstellen. Im folgenden Szenario stellen nachgeschaltete Geräte eine Verbindung über ein benutzerdefiniertes ModBus-Modul her. Außerdem können nachgeschaltete Geräte über das Modul „$edgeHub“ eine Verbindung mit dem Azure IoT Edge-Gatewaygerät herstellen.

![Zentrale Anwendungsseite](./media/tutorial-define-edge-device-type/gateway-module.png)

Nachgeschaltete Geräte können über ein benutzerdefiniertes Modul eine Verbindung mit dem Azure IoT Edge-Gatewaygerät herstellen. Im folgenden Szenario stellen nachgeschaltete Geräte eine Verbindung über ein benutzerdefiniertes ModBus-Modul her. 

![Zentrale Anwendungsseite](./media/tutorial-define-edge-device-type/gateway-module-transparent.png)

Nachgeschaltete Geräte können über mehrere benutzerdefinierte Module eine Verbindung mit dem Azure IoT Edge-Gatewaygerät herstellen. Im folgenden Szenario stellen nachgeschaltete Geräte eine Verbindung über ein benutzerdefiniertes ModBus-Modul und über ein benutzerdefiniertes BLE-Modul her. Außerdem können nachgeschaltete Geräte über das Modul „$edgeHub“ eine Verbindung mit dem Azure IoT Edge-Gatewaygerät herstellen. 

![Zentrale Anwendungsseite](./media/tutorial-define-edge-device-type/gateway-module2-transparent.png)


## <a name="create-a-template"></a>Erstellen einer Vorlage

Als Lösungsentwickler können Sie Azure IoT Edge-Gerätevorlagen in Ihrer Anwendung erstellen und bearbeiten. Nachdem Sie eine Gerätevorlage veröffentlicht haben, können Sie eine Verbindung mit echten Geräten herstellen, die die Gerätevorlage implementieren.

### <a name="select-device-template-type"></a>Auswählen des Gerätevorlagentyps 

Navigieren Sie zur Seite **Gerätevorlagen**, falls Sie Ihrer Anwendung eine neue Gerätevorlage hinzufügen möchten. Wählen Sie hierzu im linken Bereich die Option **Gerätevorlagen** aus.

![Zentrale Anwendungsseite](./media/tutorial-define-edge-device-type/edgedevicetemplate.png)

Klicken Sie auf **+ Neu**, um mit der Erstellung einer neuen Gerätevorlage zu beginnen.

![Gerätevorlagen: Neu](./media/tutorial-define-edge-device-type/edgedevicetemplatenew.png)

Daraufhin wird die Seite zum Auswählen des Gerätevorlagentyps angezeigt. Wählen Sie die Kachel **Azure IoT Edge** aus, und klicken Sie am unteren Rand auf die Schaltfläche **Weiter: Anpassen**.

![Auswählen der Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/selectiotedge.png)

### <a name="customize-device-template"></a>Anpassen der Gerätevorlage

Azure IoT Edge ermöglicht die Bereitstellung und Verwaltung von Geschäftslogik in Form von Modulen. **Azure IoT Edge-Module** sind die kleinste der von IoT Edge verwalteten Recheneinheiten und können Azure-Dienste (z. B. Azure Stream Analytics) oder Ihren eigenen lösungsspezifischen Code enthalten. Informationen zur Entwicklung, Bereitstellung und Verwaltung von Modulen finden Sie unter [Grundlegendes zu Azure IoT Edge-Modulen](../../iot-edge/iot-edge-modules.md).

Im Grunde ist ein Bereitstellungsmanifest eine Liste von Modulzwillingen, die mit den gewünschten Eigenschaften konfiguriert sind. Ein Bereitstellungsmanifest teilt einem IoT Edge-Gerät (oder einer Gruppe von Geräten) mit, welche Module installiert und wie sie konfiguriert werden müssen. Bereitstellungsmanifeste enthalten die gewünschten Eigenschaften für jeden Modulzwilling. IoT Edge-Geräte geben die gemeldeten Eigenschaften für jedes Modul zurück.

Verwenden Sie Visual Studio Code, um ein Bereitstellungsmanifest zu erstellen. Eine Anleitung zum Erstellen eines Bereitstellungsmanifests finden Sie [hier](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

In diesem Tutorial wird das folgende Beispiel für ein einfaches Bereitstellungsmanifest mit einem einzelnen Modul verwendet. Kopieren Sie den folgenden JSON-Code, und speichern Sie ihn als JSON-Datei: 

   ```JSON
   {
     "modulesContent": {
       "$edgeAgent": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "runtime": {
             "type": "docker",
             "settings": {
               "minDockerVersion": "v1.25",
               "loggingOptions": "",
               "registryCredentials": {}
             }
           },
           "systemModules": {
             "edgeAgent": {
               "type": "docker",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "SimulatedTemperatureSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "SimulatedTemperatureSensor": {
         "properties.desired": {
              "SendData": true,
              "SendInterval": 10
         }
       }
     }
   }
   ```

**Hochladen eines Azure IoT Edge-Bereitstellungsmanifests**

Klicken Sie auf die Schaltfläche **Durchsuchen**. 

![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgedevicetemplateuploadmanifest.png)

Wenn Sie eine Azure IoT Edge-Gatewaygerätevorlage erstellen möchten, aktivieren Sie das Kontrollkästchen **Gatewaygerät mit nachgeschalteten Geräten**.

![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-upload.png)

Ein Dialogfeld für die Dateiauswahl wird angezeigt. Wählen Sie die Bereitstellungsmanifestdatei aus, und klicken Sie auf die Schaltfläche **Öffnen**.

Die Bereitstellungsmanifestdatei wird anhand eines Schemas validiert. Klicken Sie nach erfolgreicher Validierung auf die Schaltfläche **Überprüfen**.

![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/deploymentmanifestvalidate.png)

Die folgende Abbildung zeigt den Zyklus einer Bereitstellungsmanifestdatei in IoT Central:

![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/dmflow.png)

Die Seite „Überprüfen“ wird mit Details zum Bereitstellungsmanifest angezeigt. Auf der Seite „Überprüfen“ befindet sich eine Liste mit den Modulen aus dem Bereitstellungsmanifest. In diesem Tutorial wird das Modul „SimulatedTemperatureSensor“ aufgeführt. Klicken Sie auf die Schaltfläche **Erstellen**.

![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgedevicetemplatereview.png)

Wenn Sie die Gatewaygeräteoption ausgewählt haben, wird die folgende Überprüfungsseite angezeigt:

![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-review.png)


Während der Erstellung der Gerätevorlage in IoT Central wird ein Wartekreisel mit dem Text „Neue Gerätevorlage wird erstellt...“ angezeigt.

Die Gerätevorlage wird mit Modulfunktionsmodellen erstellt. In diesem Tutorial wird das Modulfunktionsmodell „SimulatedTemperatureSensor“ erstellt. 

Ändern Sie den Titel der Gerätevorlage in „Environment Sensor Device Template“.

![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgedevicetemplatelanding.png)

Die Plug & Play-Modellierung für das Azure IoT Edge-Gerät läuft wie folgt ab:
* Jede Azure IoT Edge-Gerätevorlage verfügt über ein **Gerätefunktionsmodell**.
* Für jedes benutzerdefinierte Modul aus dem Bereitstellungsmanifest wird ein **Modulfunktionsmodell** generiert.
* Zwischen den einzelnen Modulfunktionsmodellen und einem Gerätefunktionsmodell wird eine **Beziehung** eingerichtet.
* Vom Modulfunktionsmodell werden **Modulschnittstellen** implementiert.
* Jede Modulschnittstelle enthält Folgendes:
   - Telemetrie
   - Properties
   - Befehle

![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgemodelling.png)

**Hinzufügen von Funktionen zum Modulfunktionsmodell**

Hier sehen Sie eine Beispielausgabe des Moduls „SimulatedTemperatureSensor“:
```json
{

    "machine": {

        "temperature": 75.0,
        "pressure": 40.2
    },
    "ambient": {
        "temperature": 23.0,
        "humidity": 30.0
    },
    "timeCreated": ""
}
```

Fügen Sie dem Modul „SimulatedTemperatureSensor“ Funktionen hinzu, die dem obigen JSON-Code entsprechen. 

* Klicken Sie unter einer Schnittstelle des Modulfunktionsmodells „SimulatedTemperatureSensor“ auf **Verwalten**. Klicken Sie auf **Funktion hinzufügen**. 

    ![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplateaddcapability.png)
  
* Fügen Sie „machine“ als Objekttyp hinzu, da es sich um einen komplexen Typ handelt.
  
    ![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplatemachineobject.png)

    Klicken Sie auf **Definieren**. Ändern Sie im modalen Popupfenster den Objektnamen in „machine“, erstellen Sie die Eigenschaften „temperature“ und „pressure“, und klicken Sie anschließend auf **Anwenden**.
  
    ![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplatemachineattributes.png)
  
* Fügen Sie „ambient“ als Objekttyp hinzu, da es sich um einen komplexen Typ handelt.

    Klicken Sie auf **Definieren**. Ändern Sie im modalen Popupfenster den Objektnamen in „ambient“, erstellen Sie die Eigenschaften „temperature“ und „humidity“, und klicken Sie anschließend auf **Anwenden**.
  
    ![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplateambientattributes.png)

  
* Fügen Sie „timeCreated“ als DateTime-Typ hinzu, und klicken Sie auf **Speichern**.
  
    ![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplateallattributes.png)


### <a name="add-relationships"></a>Hinzufügen von Beziehungen

Wenn Sie angegeben haben, dass es sich bei dem Azure IoT Edge-Gerät um ein Gatewaygerät handelt, können Sie Downstreambeziehungen mit Gerätefunktionsmodellen für Geräte hinzufügen, für die Sie eine Verbindung mit dem Gatewaygerät herstellen.
  
  ![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-add-relationship.png)

Eine Beziehung kann auf der Geräte- oder auf der Modulebene hinzugefügt werden.
  
  ![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-relationship-types.png)


Sie können ein nachgeschaltetes Gerätefunktionsmodell oder das Sternchen auswählen. 
  
  ![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-downstream-rel.png)

  In diesem Tutorial wählen wir das Sternchen aus, um alle Downstreambeziehungen zuzulassen. Klicken Sie unten auf der Seite auf **Speichern**.

  ![Gerätevorlage: Azure IoT Edge](./media/tutorial-define-edge-device-type/gateway-add-relationship-asterix.png)


### <a name="add-cloud-properties"></a>Hinzufügen von Cloudeigenschaften

Eine Gerätevorlage kann Cloudeigenschaften enthalten. Cloudeigenschaften sind nur in der IoT Central-Anwendung vorhanden und werden niemals an ein Gerät gesendet bzw. von einem Gerät empfangen.

1. Wählen Sie **Cloudeigenschaften** und dann **+ Cloudeigenschaft hinzufügen**. Verwenden Sie die Informationen in der folgenden Tabelle, um Ihrer Gerätevorlage eine Cloudeigenschaft hinzuzufügen.

    | Anzeigename      | Semantischer Typ | Schema |
    | ----------------- | ------------- | ------ |
    | Datum der letzten Wartung | Keine          | Date   |
    | Kundenname     | Keine          | Zeichenfolge |

2. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern:

  
    ![Cloudeigenschaften: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplatecloudproperties.png)

### <a name="add-customizations"></a>Hinzufügen von Anpassungen

Verwenden Sie Anpassungen, wenn Sie eine Schnittstelle ändern oder IoT Central-spezifische Features einer Funktion hinzufügen müssen, bei der Sie Ihr Gerätefunktionsmodell nicht mit einer Versionsnummer versehen müssen. Sie können Felder anpassen, wenn sich das Funktionsmodell im Entwurfszustand oder veröffentlichten Zustand befindet. Sie können nur Felder anpassen, die die Schnittstellenkompatibilität nicht beeinträchtigen. Sie haben beispielsweise folgende Möglichkeiten:

- Anpassen des Anzeigenamens und der Einheiten einer Funktion
- Hinzufügen einer Standardfarbe, die beim Anzeigen des Werts in einem Diagramm verwendet werden soll
- Angeben der anfänglichen, minimalen und maximalen Werte für eine Eigenschaft

(Der Funktionsname oder -typ kann nicht angepasst werden.) Klicken Sie unten auf der Seite auf **Speichern**.
  
![Anpassungen: Azure IoT Edge](./media/tutorial-define-edge-device-type/edgetemplatecustomize.png)


### <a name="create-views"></a>Erstellen von Ansichten

Als Ersteller können Sie die Anwendung so anpassen, dass relevante Informationen zum Umgebungssensorgerät einem Bediener angezeigt werden. Ihre Anpassungen ermöglichen dem Bediener die Verwaltung des mit der Anwendung verbundenen Umgebungssensors. Sie können zwei Arten von Ansichten erstellen, die von einem Bediener zum Interagieren mit Geräten verwendet werden:

* Formulare zum Anzeigen und Bearbeiten von Geräte- und Cloudeigenschaften
* Dashboards zum Visualisieren von Geräten

### <a name="configure-a-view-to-visualize-devices"></a>Konfigurieren einer Ansicht zum Visualisieren von Geräten

Mit einem Gerätedashboard kann ein Bediener ein Gerät visualisieren, indem er Diagramme und Metriken verwendet. Als Ersteller können Sie definieren, welche Informationen auf einem Gerätedashboard angezeigt werden sollen. Sie können mehrere Dashboards für Geräte definieren. Wählen Sie zum Erstellen eines Dashboards für die Visualisierung der Umgebungssensor-Telemetriedaten die Option **Ansichten** und dann **Gerät visualisieren**:

  
![Ansichten: Azure IoT Edge](./media/tutorial-define-edge-device-type/visualizingthedevice.png)


Bei „Ambient Telemetry“ und „Machine Telemetry“ handelt es sich um komplexe Objekte. Gehen Sie zum Erstellen von Diagrammen wie folgt vor:

Ziehen Sie „Ambient Telemetry“, und wählen Sie „Liniendiagramm“ aus. 
  
![Ansichten: Azure IoT Edge](./media/tutorial-define-edge-device-type/sensorambientchart.png)

Klicken Sie auf das Symbol „Konfigurieren“, wählen Sie „temperature“ und „humidity“ aus, um die Daten zu visualisieren, und klicken Sie anschließend auf die Schaltfläche **Konfiguration aktualisieren**. 
  
![Ansichten: Azure IoT Edge](./media/tutorial-define-edge-device-type/sensorambienttelemetrychart.png)

Wählen Sie **Speichern**, um Ihre Ansicht zu speichern:

Sie können weitere Kacheln hinzufügen, auf denen andere Eigenschaften oder Telemetriewerte angezeigt werden. Sie können auch statischen Text, Links und Bilder hinzufügen. Bewegen Sie zum Verschieben oder Ändern der Größe einer Kachel im Dashboard den Mauszeiger auf die Kachel, und ziehen Sie die Kachel an eine neue Position bzw. ändern Sie ihre Größe.
  
![Ansichten: Azure IoT Edge](./media/tutorial-define-edge-device-type/viewsdashboard.png)

### <a name="add-a-device-form"></a>Hinzufügen eines Geräteformulars

Mit einem Geräteformular kann ein Bediener schreibbare Geräteeigenschaften und Cloudeigenschaften bearbeiten. Als Ersteller können Sie mehrere Formulare definieren und auswählen, welche Geräte- und Cloudeigenschaften in einem Formular jeweils angezeigt werden. Sie können in einem Formular auch schreibgeschützte Geräteeigenschaften anzeigen.

Erstellen Sie wie folgt ein Formular, um Umgebungssensoreigenschaften anzuzeigen und zu bearbeiten:

Navigieren Sie in der Vorlage **Environmental Sensor** (Umgebungssensor) zu **Ansichten**. Wählen Sie die Kachel **Geräte- und Clouddaten bearbeiten** aus, um eine neue Ansicht hinzuzufügen.
  
![Ansichten: Azure IoT Edge](./media/tutorial-define-edge-device-type/editingdeviceandclouddata.png)

Geben Sie den Formularnamen **Environmental Sensor properties** (Umgebungssensoreigenschaften) ein.

Ziehen Sie die Cloudeigenschaften **Kundenname** und **Datum der letzten Wartung** in den vorhandenen Abschnitt im Formular.
  
![Ansichten: Azure IoT Edge](./media/tutorial-define-edge-device-type/views-properties.png)

Wählen Sie **Speichern**, um Ihre Ansicht zu speichern.

### <a name="generate-default-views"></a>Generieren von Standardansichten

Die Funktion „Standardansichten generieren“ wird für Azure IoT Edge-Vorlagen nicht unterstützt. 

## <a name="publish-device-template"></a>Veröffentlichen der Gerätevorlage

Bevor Sie einen simulierten Umgebungssensor erstellen oder einen echten Umgebungssensor verbinden können, müssen Sie Ihre Gerätevorlage veröffentlichen.

Veröffentlichen Sie wie folgt eine Gerätevorlage:

1. Navigieren Sie über die Seite **Gerätevorlagen** zu Ihrer Gerätevorlage.

2. Wählen Sie **Veröffentlichen**.
  
    ![Ansichten: Veröffentlichen](./media/tutorial-define-edge-device-type/edgetemplatepublish.png)

1. Wählen Sie im Dialogfeld **Publish a Device Template** (Gerätevorlage veröffentlichen) die Option **Veröffentlichen**:
  
    ![Ansichten: Veröffentlichen](./media/tutorial-define-edge-device-type/edgepublishtemplate.png)

Nachdem eine Gerätevorlage veröffentlicht wurde, wird sie auf der Seite **Geräte** und für den Bediener angezeigt. In einer veröffentlichten Gerätevorlage können Sie ein Gerätefunktionsmodell nicht bearbeiten, ohne eine neue Versionsnummer zu erstellen. Sie können aber Aktualisierungen für Cloudeigenschaften, Anpassungen und Ansichten in einer veröffentlichten Gerätevorlage ohne Versionsvergabe durchführen. Wählen Sie nach dem Vornehmen von Änderungen die Option **Veröffentlichen**, um diese Änderungen für Ihren Bediener bereitzustellen.
  
![Ansichten: Veröffentlichen](./media/tutorial-define-edge-device-type/publishedtemplate.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Erstellen eines neuen Edge als Blattgerätevorlage
* Generieren von Modulen auf der Grundlage eines hochgeladenen Bereitstellungsmanifests
* Hinzufügen von komplexer Telemetrie und Eigenschaften
* Erstellen von Cloudeigenschaften
* Erstellen von Anpassungen
* Definieren einer Visualisierung für die Gerätetelemetriedaten
* Veröffentlichen Ihrer Edgegerätevorlage

Nachdem Sie nun eine Gerätevorlage in Ihrer Azure IoT Central-Anwendung erstellt haben, wird als Nächstes dieser Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verbinden eines Geräts](./tutorial-connect-pnp-device.md)
