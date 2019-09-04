---
title: Definieren eines neuen Gerätetyps in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Tutorial für Ersteller erfahren Sie, wie Sie in Ihrer Azure IoT Central-Anwendung einen neuen Gerätetyp erstellen. Sie definieren die Telemetriedaten, den Zustand, die Eigenschaften und die Befehle für den Typ.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 660b2ead146695657ae13444cb7936eff8224f3a
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70099518"
---
# <a name="tutorial-define-a-new-device-type-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

In diesem Tutorial für Ersteller erfahren Sie, wie Sie in Ihrer Microsoft Azure IoT Central-Anwendung mithilfe einer Gerätevorlage eine neue Art von Gerät definieren. Mit einer Gerätevorlage werden die Funktionen Ihres Geräts definiert. Zu den Funktionen zählen die vom Gerät gesendeten Telemetriedaten, Geräteeigenschaften und die Befehle, auf die ein Gerät reagiert.

In diesem Tutorial erstellen Sie die Gerätevorlage **Environment Sensor** (Umgebungssensor). Ein Umgebungssensorgerät ermöglicht Folgendes:

* Senden von Telemetriedaten, z. B. Temperatur
* Melden von gerätespezifischen Eigenschaften, z. B. Helligkeitsstufe
* Reagieren auf Befehle, z. B. Aktivieren und Deaktivieren
* Melden von generischen Geräteeigenschaften, z. B. Firmwareversion und Seriennummer

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer neuen Gerätevorlage
> * Importieren eines Gerätefunktionsmodells
> * Erstellen von Cloudeigenschaften
> * Definieren einer Visualisierung für die Gerätetelemetriedaten
> * Veröffentlichen Ihrer Gerätevorlage
> * Erstellen eines simulierten Geräts für Ihre Gerätevorlage
> * Anzeigen Ihres simulierten Geräts

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie eine Azure IoT Central-Anwendung. Wenn Sie die Schritte der Schnellstartanleitung [Create an Azure IoT Central application](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Erstellen einer Azure IoT Central-Anwendung) ausgeführt haben, können Sie die so erstellte Anwendung verwenden. Führen Sie andernfalls die folgenden Schritte aus, um eine leere Azure IoT Central-Anwendung zu erstellen:

1. Navigieren Sie zur Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral).

1. Gehen Sie bei Vorhandensein eines Azure-Abonnements so vor, dass Sie sich mit den üblichen Anmeldeinformationen anmelden. Verwenden Sie andernfalls ein Microsoft-Konto:

    ![Eingeben Ihres Organisationskontos](./media/tutorial-define-device-type-pnp/sign-in.png)

1. Wählen Sie **Neue Anwendung** aus, um mit der Erstellung einer neuen Azure IoT Central-Anwendung zu beginnen.

Erstellen Sie wie folgt eine neue Azure IoT Central-Anwendung, für die Previewfunktionen genutzt werden, z. B. IoT Plug & Play:

1. Wählen Sie **Testversion** aus. Sie benötigen kein Azure-Abonnement, um eine Testanwendung zu erstellen.

    Weitere Informationen zu Verzeichnissen und Abonnements finden Sie unter [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

1. Wählen Sie **Vorschauanwendung**.

1. Sie können optional auch einen Anzeigenamen für die Anwendung angeben (beispielsweise **Contoso Air Conditioners**). Azure IoT Central generiert automatisch ein eindeutiges URL-Präfix. Dieses URL-Präfix kann in einen einprägsameren Wert geändert werden.

1. Wenn Sie eine Testanwendung erstellen, müssen Sie Ihre Kontaktdaten angeben.

1. Klicken Sie auf **Erstellen**.

    ![Azure IoT Central-Seite „Anwendung erstellen“](./media/tutorial-define-device-type-pnp/iotcentralcreate.png)

    Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer Anwendung](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

Außerdem benötigen Sie eine lokale Kopie der Datei **EnvironmentalSensorInline.capabilitymodel.json**, die das Gerätefunktionsmodell [IoT Plug & Play](../iot-pnp/overview-iot-plug-and-play.md) enthält. Sie können es [hier](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json)herunterladen. Klicken Sie mit der rechten Maustaste auf die Seite, und wählen Sie **Speichern unter**.

Öffnen Sie die Datei nach dem Herunterladen in einem Text-Editor, und ersetzen Sie die beiden Instanzen von `<YOUR_COMPANY_NAME_HERE>` durch Ihren Namen. Verwenden Sie nur die Zeichen a - z, A - Z, 0 - 9 und Unterstrich.

## <a name="create-a-template"></a>Erstellen einer Vorlage

Als Ersteller können Sie die Gerätevorlagen in Ihrer Anwendung erstellen und bearbeiten. Nachdem Sie eine Gerätevorlage veröffentlicht haben, können Sie ein simuliertes Gerät generieren oder echte Geräte verbinden, mit denen die Gerätevorlage implementiert wird. Mit simulierten Geräten können Sie das Verhalten Ihrer Anwendung testen, bevor Sie eine Verbindung mit einem echten Gerät herstellen.

Navigieren Sie zur Seite **Gerätevorlagen**, falls Sie Ihrer Anwendung eine neue Gerätevorlage hinzufügen möchten. Wählen Sie hierzu links im Navigationsmenü die Option **Gerätevorlagen**.

![Seite „Gerätevorlagen“](./media/tutorial-define-device-type-pnp/devicedefinitions.png)

### <a name="add-a-device-capability-model"></a>Hinzufügen eines Gerätefunktionsmodells

Es gibt mehrere Optionen zum Erstellen eines Gerätefunktionsmodells in IoT Central. Sie können ein benutzerdefiniertes Modell von Grund auf neu erstellen, aus einer Datei importieren, aus dem Gerätekatalog auswählen oder über eine gerätepriorisierende Verbindung mit einem IoT Plug & Play-Gerät verbinden, bei der das Gerätefunktionsmodell im öffentlichen Repository veröffentlicht wurde. In diesem Tutorial importieren Sie ein Gerätefunktionsmodell aus einer Datei.

In den folgenden Schritten wird veranschaulicht, wie Sie das Funktionsmodell für ein Gerät vom Typ **Environmental Sensor** (Umgebungssensor) importieren. Diese Geräte senden Telemetriedaten, z. B. die Temperatur, an Ihre Anwendung:

1. Wählen Sie zum Hinzufügen einer neuen Gerätevorlage auf der Seite **Gerätevorlagen** die Option **+ Neu**.

1. Wählen Sie in der Liste der Vorlagen die Option **Benutzerdefiniert** aus.

1. Geben Sie **Environmental Sensor** (Umgebungssensor) als Namen für Ihre Gerätevorlage ein.

1. Wählen Sie **Funktionsmodell importieren**, um ein neues Gerätefunktionsmodell aus einer JSON-Datei zu erstellen. Navigieren Sie zu dem Ordner, in dem Sie die Datei **EnvironmentalSensorInline.capabilitymodel.json** auf Ihrem lokalen Computer gespeichert haben. Wählen Sie die Datei **EnvironmentalSensorInline.capabilitymodel.json** und dann die Option **Öffnen** aus. Das Umgebungssensor-Funktionsmodell enthält die Schnittstellen **Environmental Sensor** (Umgebungssensor) und **Device Information** (Geräteinformationen):

    ![Gerätefunktionsmodell „Environmental Sensor“ (Umgebungssensor)](./media/tutorial-define-device-type-pnp/newdevicecapabilitymodel.png)

    Mit diesen Schnittstellen werden die Funktionen eines Geräts vom Typ **Environmental Sensor** (Umgebungssensor) definiert. Zu den Funktionen gehören die von einem Gerät gesendeten Telemetriedaten, die von einem Gerät gemeldeten Eigenschaften und die Befehle, auf die ein Gerät antwortet.

### <a name="add-cloud-properties"></a>Hinzufügen von Cloudeigenschaften

Eine Gerätevorlage kann Cloudeigenschaften enthalten. Cloudeigenschaften sind nur in der IoT Central-Anwendung vorhanden und werden niemals an ein Gerät gesendet bzw. von einem Gerät empfangen.

1. Wählen Sie **Cloudeigenschaften** und dann **+ Cloudeigenschaft hinzufügen**. Verwenden Sie die Informationen in der folgenden Tabelle, um Ihrer Gerätevorlage eine Cloudeigenschaft hinzuzufügen.

    | Anzeigename      | Semantischer Typ | Schema |
    | ----------------- | ------------- | ------ |
    | Datum der letzten Wartung | Keine          | Date   |
    | Kundenname     | Keine          | Zeichenfolge |

1. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern:

    ![Cloudeigenschaften](media/tutorial-define-device-type-pnp/cloudproperties.png)

### <a name="add-customizations"></a>Hinzufügen von Anpassungen

Verwenden Sie Anpassungen, wenn Sie eine Schnittstelle ändern oder IoT Central-spezifische Features einer Funktion hinzufügen müssen, bei der Sie Ihr Gerätefunktionsmodell nicht mit einer Versionsnummer versehen müssen. Sie können Felder anpassen, wenn sich das Funktionsmodell im Entwurfszustand oder veröffentlichten Zustand befindet. Sie können nur Felder anpassen, die die Schnittstellenkompatibilität nicht beeinträchtigen. Sie haben beispielsweise folgende Möglichkeiten:

- Anpassen des Anzeigenamens und der Einheiten einer Funktion
- Hinzufügen einer Standardfarbe, die beim Anzeigen des Werts in einem Diagramm verwendet werden soll
- Angeben der anfänglichen, minimalen und maximalen Werte für eine Eigenschaft

(Der Funktionsname oder -typ kann nicht angepasst werden.)


## <a name="create-views"></a>Erstellen von Ansichten

Als Ersteller können Sie die Anwendung so anpassen, dass relevante Informationen zum Umgebungssensorgerät einem Bediener angezeigt werden. Ihre Anpassungen ermöglichen dem Bediener die Verwaltung des mit der Anwendung verbundenen Umgebungssensors. Sie können zwei Arten von Ansichten erstellen, die von einem Bediener zum Interagieren mit Geräten verwendet werden:

* Formulare zum Anzeigen und Bearbeiten von Geräte- und Cloudeigenschaften
* Dashboards zum Visualisieren von Geräten

### <a name="generate-default-views"></a>Generieren von Standardansichten

Das Generieren von Standardansichten ist eine schnelle Möglichkeit, um mit der Visualisierung Ihrer wichtigen Geräteinformationen zu beginnen. Sie können für Ihre Gerätevorlage bis zu drei Standardansichten generieren:

* Über die Ansicht **Befehle** kann der Bediener Befehle an Ihr Gerät senden.
* In der Ansicht **Übersicht** werden Diagramme und Metriken verwendet, um Gerätetelemetriedaten anzuzeigen.
* In der Ansicht **Info** werden Geräteeigenschaften angezeigt.

Nachdem Sie die Option **Standardansichten generieren** gewählt haben, werden diese im Abschnitt **Ansichten** Ihrer Gerätevorlage automatisch hinzugefügt.

### <a name="configure-a-view-to-visualize-devices"></a>Konfigurieren einer Ansicht zum Visualisieren von Geräten

Mit einem Gerätedashboard kann ein Bediener ein Gerät visualisieren, indem er Diagramme und Metriken verwendet. Als Ersteller können Sie definieren, welche Informationen auf einem Gerätedashboard angezeigt werden sollen. Sie können mehrere Dashboards für Geräte definieren. Wählen Sie zum Erstellen eines Dashboards für die Visualisierung der Umgebungssensor-Telemetriedaten die Option **Ansichten** und dann **Gerät visualisieren**:

1. Unter **Eigenschaften** sind alle Geräteeigenschaften, Cloudeigenschaften, Telemetriedaten und statischen Optionen aufgeführt. Sie können diese Elemente per Drag & Drop in die Ansicht ziehen. Ziehen Sie die Eigenschaft **Helligkeitsstufe** in die Ansicht. Sie können die Kachel konfigurieren, indem Sie das Zahnradsymbol verwenden.

1. Wählen Sie zum Hinzufügen eines Diagramms, mit dem Telemetriedaten ausgegeben werden, die Optionen **Luftfeuchtigkeit** und **Temperatur** und dann **Kombinieren**. Um dieses Diagramm in einem anderen Format anzuzeigen, z. B. als Kreis- oder Balkendiagramm, wählen Sie oben auf der Kachel die Schaltfläche **Visualisierung ändern** aus.

1. Wählen Sie **Speichern**, um Ihre Ansicht zu speichern:

Sie können weitere Kacheln hinzufügen, auf denen andere Eigenschaften oder Telemetriewerte angezeigt werden. Sie können auch statischen Text, Links und Bilder hinzufügen. Bewegen Sie zum Verschieben oder Ändern der Größe einer Kachel im Dashboard den Mauszeiger auf die Kachel, und ziehen Sie die Kachel an eine neue Position bzw. ändern Sie ihre Größe.

### <a name="add-a-device-form"></a>Hinzufügen eines Geräteformulars

Mit einem Geräteformular kann ein Bediener schreibbare Geräteeigenschaften und Cloudeigenschaften bearbeiten. Als Ersteller können Sie mehrere Formulare definieren und auswählen, welche Geräte- und Cloudeigenschaften in einem Formular jeweils angezeigt werden. Sie können in einem Formular auch schreibgeschützte Geräteeigenschaften anzeigen.

Erstellen Sie wie folgt ein Formular, um Umgebungssensoreigenschaften anzuzeigen und zu bearbeiten:

1. Navigieren Sie in der Vorlage **Environmental Sensor** (Umgebungssensor) zu **Ansichten**. Wählen Sie die Kachel **Geräte- und Clouddaten bearbeiten** aus, um eine neue Ansicht hinzuzufügen.

1. Geben Sie den Formularnamen **Environmental Sensor properties** (Umgebungssensoreigenschaften) ein.

1. Ziehen Sie die Cloudeigenschaften **Kundenname** und **Datum der letzten Wartung** in den vorhandenen Abschnitt im Formular.

1. Wählen Sie die Geräteeigenschaften **Helligkeitsstufe** und **Gerätestatus** aus. Wählen Sie anschließend die Option **Abschnitt hinzufügen**. Bearbeiten Sie den Titel des Abschnitts so, dass er **Sensoreigenschaften** lautet. Wählen Sie **Übernehmen**.

1. Wählen Sie die Geräteeigenschaften **Gerätemodell**, **Softwareversion**, **Hersteller** und **Processor manufacturer** (Prozessorhersteller) aus. Wählen Sie anschließend die Option **Abschnitt hinzufügen**. Bearbeiten Sie den Titel des Abschnitts so, dass er **Geräteeigenschaften** lautet. Wählen Sie **Übernehmen**.

1. Wählen Sie **Speichern**, um Ihre Ansicht zu speichern.

## <a name="publish-device-template"></a>Veröffentlichen der Gerätevorlage

Bevor Sie einen simulierten Umgebungssensor erstellen oder einen echten Umgebungssensor verbinden können, müssen Sie Ihre Gerätevorlage veröffentlichen.

Veröffentlichen Sie wie folgt eine Gerätevorlage:

1. Navigieren Sie über die Seite **Gerätevorlagen** zu Ihrer Gerätevorlage.

1. Wählen Sie **Veröffentlichen**.

1. Wählen Sie im Dialogfeld **Publish a Device Template** (Gerätevorlage veröffentlichen) die Option **Veröffentlichen**:

    ![Veröffentlichtes Modell](media/tutorial-define-device-type-pnp/publishedmodel.png)

Nachdem eine Gerätevorlage veröffentlicht wurde, wird sie auf der Seite **Geräte** und für den Bediener angezeigt. In einer veröffentlichten Gerätevorlage können Sie ein Gerätefunktionsmodell nicht bearbeiten, ohne eine neue Versionsnummer zu erstellen. Sie können aber Aktualisierungen für Cloudeigenschaften, Anpassungen und Ansichten in einer veröffentlichten Gerätevorlage ohne Versionsvergabe durchführen. Wählen Sie nach dem Vornehmen von Änderungen die Option **Veröffentlichen**, um diese Änderungen für Ihren Bediener bereitzustellen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Erstellen einer neuen Gerätevorlage
* Importieren eines Gerätefunktionsmodells
* Erstellen von Cloudeigenschaften
* Erstellen von Anpassungen
* Definieren einer Visualisierung für die Gerätetelemetriedaten
* Veröffentlichen Ihrer Gerätevorlage

Nachdem Sie nun eine Gerätevorlage in Ihrer Azure IoT Central-Anwendung erstellt haben, wird als Nächstes dieser Schritt empfohlen:

> [!div class="nextstepaction"]
> [Hinzufügen eines Geräts](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
