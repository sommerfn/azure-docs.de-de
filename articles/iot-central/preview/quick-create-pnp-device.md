---
title: Hinzufügen eines simulierten Geräts zu Azure IoT Central | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Gerätevorlage erstellen und Ihrer IoT Central-Anwendung ein simuliertes Gerät hinzufügen.
author: dominicbetts
ms.author: dobett
ms.date: 10/24/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: 8b6720af6328242a1fe297e60c9d413d0b83f3d2
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896298"
---
# <a name="quickstart-add-a-simulated-device-to-your-iot-central-application-preview-features"></a>Schnellstart: Hinzufügen eines simulierten Geräts zu Ihrer IoT Central-Anwendung (Previewfunktionen)

*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Eine Gerätevorlage definiert die Funktionen eines Geräts, das eine Verbindung mit Ihrer IoT Central-Anwendung herstellt. Zu den Funktionen zählen die vom Gerät gesendeten Telemetriedaten, Geräteeigenschaften und die Befehle, auf die ein Gerät reagiert. Auf der Grundlage einer Gerätevorlage kann ein Ersteller oder Bediener sowohl echte als auch simulierte Geräte zu einer Anwendung hinzufügen. Mithilfe von simulierten Geräten können Sie das Verhalten Ihrer IoT Central-Anwendung testen, bevor Sie eine Verbindung mit echten Geräten herstellen.

In dieser Schnellstartanleitung wird eine Gerätevorlage vom Typ **Environmental Sensor** (Umgebungssensor) erstellt und ein simuliertes Gerät hinzugefügt. Ein Umgebungssensorgerät ermöglicht Folgendes:

* Senden von Telemetriedaten, z. B. Temperatur
* Melden von gerätespezifischen Eigenschaften, z. B. Helligkeitsstufe
* Reagieren auf Befehle, z. B. Aktivieren und Deaktivieren
* Melden von generischen Geräteeigenschaften, z. B. Firmwareversion und Seriennummer

## <a name="prerequisites"></a>Voraussetzungen

Arbeiten Sie die Schnellstartanleitung [Erstellen einer Azure IoT Central-Anwendung (Previewfunktionen)](./quick-deploy-iot-central.md) durch, um mit der Vorlage **Benutzerdefinierte App > Vorschauanwendung** eine IoT Central-Anwendung zu erstellen.

Außerdem benötigen Sie eine lokale Kopie der Datei **EnvironmentalSensorInline.capabilitymodel.json**, die das Gerätefunktionsmodell [IoT Plug & Play](../../iot-pnp/overview-iot-plug-and-play.md) enthält. Sie können es [hier](https://raw.githubusercontent.com/Azure/IoTPlugandPlay/master/samples/EnvironmentalSensorInline.capabilitymodel.json)herunterladen. Klicken Sie mit der rechten Maustaste auf die Seite, und wählen Sie **Speichern unter**.

Öffnen Sie die Datei nach dem Herunterladen in einem Text-Editor, und ersetzen Sie die beiden Instanzen von `<YOUR_COMPANY_NAME_HERE>` durch Ihren Namen. Verwenden Sie nur die Zeichen a - z, A - Z, 0 - 9 und Unterstrich.

## <a name="create-a-template"></a>Erstellen einer Vorlage

Als Ersteller können Sie die Gerätevorlagen in Ihrer Anwendung erstellen und bearbeiten. Nachdem Sie eine Gerätevorlage veröffentlicht haben, können Sie ein simuliertes Gerät generieren oder echte Geräte verbinden, mit denen die Gerätevorlage implementiert wird. Mit simulierten Geräten können Sie das Verhalten Ihrer Anwendung testen, bevor Sie eine Verbindung mit einem echten Gerät herstellen.

Navigieren Sie zur Seite **Gerätevorlagen**, falls Sie Ihrer Anwendung eine neue Gerätevorlage hinzufügen möchten. Wählen Sie hierzu im linken Bereich die Option **Gerätevorlagen** aus.

![Seite „Gerätevorlagen“](./media/quick-create-pnp-device/devicedefinitions.png)

### <a name="add-a-device-capability-model"></a>Hinzufügen eines Gerätefunktionsmodells

Es gibt mehrere Optionen zum Erstellen eines Gerätefunktionsmodells in IoT Central. Sie können ein benutzerdefiniertes Modell von Grund auf neu erstellen, aus einer Datei importieren, aus dem Gerätekatalog auswählen oder über eine gerätepriorisierende Verbindung mit einem IoT Plug & Play-Gerät verbinden, bei der das Gerätefunktionsmodell im öffentlichen Repository veröffentlicht wurde. In diesem Tutorial importieren Sie ein Gerätefunktionsmodell aus einer Datei.

In den folgenden Schritten wird veranschaulicht, wie Sie das Funktionsmodell für ein Gerät vom Typ **Environmental Sensor** (Umgebungssensor) importieren. Diese Geräte senden Telemetriedaten, z. B. die Temperatur, an Ihre Anwendung:

1. Wählen Sie zum Hinzufügen einer neuen Gerätevorlage auf der Seite **Gerätevorlagen** die Option **+** aus.

1. Wählen Sie in der Liste mit den benutzerdefinierten Gerätevorlagen die Option **IoT-Gerät** aus. Wählen Sie dann **Weiter: Anpassen** > **Weiter: Überprüfen** und anschließend **Erstellen** aus.

1. Geben Sie **Environmental Sensor** (Umgebungssensor) als Namen für Ihre Gerätevorlage ein.

1. Wählen Sie **Funktionsmodell importieren**, um ein neues Gerätefunktionsmodell aus einer JSON-Datei zu erstellen. Navigieren Sie zu dem Ordner, in dem Sie die Datei **EnvironmentalSensorInline.capabilitymodel.json** auf Ihrem lokalen Computer gespeichert haben. Wählen Sie die Datei **EnvironmentalSensorInline.capabilitymodel.json** und dann die Option **Öffnen** aus. Das Umgebungssensor-Funktionsmodell enthält die Schnittstellen **Environmental Sensor** (Umgebungssensor) und **Device Information** (Geräteinformationen):

    ![Gerätefunktionsmodell „Environmental Sensor“ (Umgebungssensor)](./media/quick-create-pnp-device/newdevicecapabilitymodel.png)

    Mit diesen Schnittstellen werden die Funktionen eines Geräts vom Typ **Environmental Sensor** (Umgebungssensor) definiert. Zu den Funktionen gehören die von einem Gerät gesendeten Telemetriedaten, die von einem Gerät gemeldeten Eigenschaften und die Befehle, auf die ein Gerät antwortet.

### <a name="add-cloud-properties"></a>Hinzufügen von Cloudeigenschaften

Eine Gerätevorlage kann Cloudeigenschaften enthalten. Cloudeigenschaften sind nur in der IoT Central-Anwendung vorhanden und werden niemals an ein Gerät gesendet bzw. von einem Gerät empfangen.

1. Wählen Sie **Cloudeigenschaften** und dann **+ Cloudeigenschaft hinzufügen**. Verwenden Sie die Informationen in der folgenden Tabelle, um Ihrer Gerätevorlage eine Cloudeigenschaft hinzuzufügen.

    | Anzeigename      | Semantischer Typ | Schema |
    | ----------------- | ------------- | ------ |
    | Datum der letzten Wartung | Keine          | Date   |
    | Customer Name     | Keine          | Zeichenfolge |

1. Wählen Sie **Speichern**, um Ihre Änderungen zu speichern:

    ![Cloudeigenschaften](media/quick-create-pnp-device/cloudproperties.png)

## <a name="create-views"></a>Erstellen von Ansichten

Als Ersteller können Sie die Anwendung so anpassen, dass relevante Informationen zum Umgebungssensorgerät einem Bediener angezeigt werden. Ihre Anpassungen ermöglichen dem Bediener die Verwaltung des mit der Anwendung verbundenen Umgebungssensors. Sie können zwei Arten von Ansichten erstellen, die von einem Bediener zum Interagieren mit Geräten verwendet werden:

* Formulare zum Anzeigen und Bearbeiten von Geräte- und Cloudeigenschaften
* Dashboards zum Visualisieren von Geräten

### <a name="generate-default-views"></a>Generieren von Standardansichten

Das Generieren von Standardansichten ist eine schnelle Möglichkeit, um mit der Visualisierung Ihrer wichtigen Geräteinformationen zu beginnen. Sie können für Ihre Gerätevorlage bis zu drei Standardansichten generieren:

* Über die Ansicht **Befehle** kann der Bediener Befehle an Ihr Gerät senden.
* In der Ansicht **Übersicht** werden Diagramme und Metriken verwendet, um Gerätetelemetriedaten anzuzeigen.
* In der Ansicht **Info** werden Geräteeigenschaften angezeigt.

Wählen Sie **Ansichten** > **Standardansichten generieren** aus.

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

    ![Veröffentlichtes Modell](media/quick-create-pnp-device/publishedmodel.png)

Nachdem eine Gerätevorlage veröffentlicht wurde, wird sie auf der Seite **Geräte** und für den Bediener angezeigt. In einer veröffentlichten Gerätevorlage können Sie ein Gerätefunktionsmodell nicht bearbeiten, ohne eine neue Versionsnummer zu erstellen. Sie können aber Aktualisierungen für Cloudeigenschaften, Anpassungen und Ansichten in einer veröffentlichten Gerätevorlage ohne Versionsvergabe durchführen. Wählen Sie nach dem Vornehmen von Änderungen die Option **Veröffentlichen**, um diese Änderungen für Ihren Bediener bereitzustellen.

## <a name="add-a-simulated-device"></a>Hinzufügen eines simulierten Geräts

Verwenden Sie die von Ihnen erstellte Gerätevorlage **Environmental Sensor** (Umgebungssensor), um Ihrer Anwendung ein simuliertes Gerät hinzuzufügen.

1. Wählen Sie als Bediener im linken Bereich die Option **Geräte** aus, um ein neues Gerät hinzuzufügen. Auf der Registerkarte **Geräte** werden die Option **Alle Geräte** und die Gerätevorlage **Environmental Sensor** (Umgebungssensor) angezeigt. Wählen Sie **Environmental Sensor** (Umgebungssensor) aus.

1. Wählen Sie zum Hinzufügen eines simulierten Umgebungssensors **+ Neu** aus. Verwenden Sie die vorgeschlagene **Geräte-ID**, oder geben Sie Ihre eigene **Geräte-ID** (in Kleinbuchstaben) ein. Sie können auch einen Namen für Ihr neues Gerät eingeben. Legen Sie die Umschaltfläche **Simuliert** auf **Ein** fest, und wählen Sie dann **Erstellen** aus.

    ![Simuliertes Gerät](./media/quick-create-pnp-device/simulated-device.png)

Sie können nun mit den Ansichten interagieren, die vom Ersteller für die Gerätevorlage mit simulierten Daten erstellt wurde.

## <a name="use-a-simulated-device-to-improve-views"></a>Verwenden eines simulierten Geräts zum Verbessern von Ansichten

Nach der Erstellung eines neuen simulierten Geräts kann der Ersteller dieses Gerät verwenden, um die Ansichten für die Gerätevorlage weiter zu verbessern und darauf aufzubauen.

1. Wählen Sie im linken Bereich die Option **Gerätevorlagen** und anschließend die Vorlage **Environmental Sensor** (Umgebungssensor) aus.

1. Wählen Sie eine der Ansichten aus, die Sie bearbeiten möchten, oder erstellen Sie eine neue Ansicht. Klicken Sie auf **Konfigurieren des Vorschaugeräts** und anschließend auf **Ausgeführtes Gerät auswählen**. Hier können Sie wählen, ob Sie kein Vorschaugerät, ein echtes, zu Testzwecken konfigurierbares Gerät oder ein vorhandenes Gerät verwenden möchten, das Sie in IoT Central hinzugefügt haben.

1. Wählen Sie in der Liste Ihr simuliertes Gerät aus. Wählen Sie anschließend **Anwenden** aus. Nun sehen Sie das gleiche simulierte Gerät auf der Erstellungsoberfläche für Gerätevorlagenansichten. Diese Ansicht ist nützlich für Diagramme und andere Visualisierungen.

    ![Konfigurieren des Vorschaugeräts](./media/quick-create-pnp-device/configure-preview.png)

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie eine Gerätevorlage vom Typ **Environmental Sensor** (Umgebungssensor) erstellen und Ihrer Anwendung ein simuliertes Gerät hinzufügen.

Weitere Informationen zum Überwachen von Geräten, die mit Ihrer Anwendung verbunden sind, finden Sie in der folgenden Schnellstartanleitung:

> [!div class="nextstepaction"]
> [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central (Previewfunktionen)](./quick-configure-rules.md)
