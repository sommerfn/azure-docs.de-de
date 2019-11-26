---
title: 'Tutorial: Anpassen der Ansichten für Bediener in Azure IoT Central'
description: 'Tutorial: Als Ersteller können Sie die Ansichten für Bediener in Ihrer Azure IoT Central-Anwendung anpassen.'
author: sandeeppujar
ms.author: sandeepu
ms.date: 11/13/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: d835ce5a1cbdcbd9741c775c4e9bd2eea7c803d7
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74112342"
---
# <a name="tutorial-customize-the-azure-iot-central-operators-view"></a>Tutorial: Anpassen der Azure IoT Central-Ansicht für Bediener

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

Dieses Tutorial richtet sich an Ersteller und erläutert, wie Sie die Darstellung Ihrer Anwendung für Bediener anpassen. Wenn Sie als Ersteller eine Änderung an der Anwendung vornehmen, können Sie in der Microsoft Azure IoT Central-Anwendung eine Vorschau der Ansicht für Bediener anzeigen.

In diesem Tutorial passen Sie die Anwendung an, damit einem Bediener relevante Informationen zur verbundenen Klimaanlage angezeigt werden. Ihre Anpassungen ermöglichen dem Bediener die Verwaltung der mit der Anwendung verbundenen Klimaanlagen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren Ihres Gerätedashboards
> * Konfigurieren des Layouts Ihrer Geräteeinstellungen
> * Konfigurieren des Layouts Ihrer Geräteeigenschaften
> * Anzeigen einer Vorschau des Geräts als Bediener
> * Konfigurieren Ihres Standardanwendungsdashboards
> * Anzeigen einer Vorschau des Standardanwendungsdashboards als Bediener

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird vorausgesetzt, dass Sie bereits die beiden vorherigen Tutorials absolviert haben:

* [Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung](tutorial-define-device-type.md)
* [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](tutorial-configure-rules.md)

## <a name="configure-your-device-dashboard"></a>Konfigurieren Ihres Gerätedashboards

Als Ersteller können Sie definieren, welche Informationen auf einem Gerätedashboard angezeigt werden sollen. Unter [Tutorial: Define a new device type in your Azure IoT Central application (New UI design)](tutorial-define-device-type.md) (Tutorial: Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung (neues Benutzeroberflächendesign)) haben Sie dem Dashboard für die verbundene Klimaanlage (**Connected Air Conditioner**) ein Liniendiagramm und andere Informationen hinzugefügt.

1. Klicken Sie zum Bearbeiten der Gerätevorlage **Connected Air Conditioner** (Verbundene Klimaanlage) im linken Bereich auf **Gerätevorlagen**:

    ![Seite „Gerätevorlagen“](media/tutorial-customize-operator/devicetemplates.png)

2. Wählen Sie zum Anpassen des Gerätedashboards das Gerätevorlagengerät **Connected Air Conditioner (1.0.0)** (Verbundene Klimaanlage (1.0.0)) aus, das Sie unter [Tutorial: Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung (neues Benutzeroberflächendesign)](tutorial-define-device-type.md) erstellt haben.

3. Wählen Sie zum Bearbeiten des Dashboards die Registerkarte **Dashboard** aus.

4. Klicken Sie auf **KPI**, um dem Dashboard eine KPI-Kachel (Key Performance Indicator) hinzuzufügen:

    Definieren Sie den KPI mit den Informationen aus der folgenden Tabelle:

    | Einstellung     | Wert |
    | ----------- | ----- |
    | NAME        | Maximum temperature |
    | Zeitbereich  | Past 1 week (Letzte Woche) |
    | Messungstyp | Telemetrie |
    | Messung | Temperatur |
    | Aggregation | Maximum |
    | Sichtbarkeit  | Enabled |

    ![Hinzufügen eines KPIs](media/tutorial-customize-operator/addkpi.png)

5. Wählen Sie **Speichern** aus. Auf dem Dashboard wird nun die KPI-Kachel angezeigt:

    ![KPI-Kachel](media/tutorial-customize-operator/temperaturekpi.png)

6. Wenn Sie eine Kachel verschieben oder ihre Größe ändern möchten, zeigen Sie mit dem Mauszeiger auf die Kachel. Sie können die Kachel an eine andere Position ziehen oder ihre Größe ändern.

## <a name="configure-your-settings-layout"></a>Konfigurieren des Layouts Ihrer Einstellungen

Als Ersteller können Sie auch die Darstellung der Geräteeinstellungen für Bediener konfigurieren. Bediener verwenden die Registerkarte mit den Geräteeinstellungen, um ein Gerät zu konfigurieren. So kann ein Bediener auf der Einstellungsregisterkarte beispielsweise die Zieltemperatur für die verbundene Klimaanlage festlegen.

1. Wählen Sie **Einstellungen**, um das Layout der Einstellungen für Ihre verbundene Klimaanlage zu bearbeiten.

2. Sie können die Einstellungskacheln verschieben und ihre Größe ändern:

    ![Bearbeiten des Layouts der Einstellungen](media/tutorial-customize-operator/settingslayout.png)

## <a name="configure-your-properties-layout"></a>Konfigurieren des Layouts Ihrer Eigenschaften

Neben dem Dashboard und den Einstellungen können Sie auch die Darstellung der Geräteeigenschaften für Bediener konfigurieren. Bediener verwenden die Registerkarte mit den Geräteeigenschaften zur Verwaltung von Gerätemetadaten. So kann ein Bediener über die Eigenschaftenregisterkarte beispielsweise die Seriennummer eines Geräts anzeigen oder die Kontaktdetails für den Hersteller aktualisieren.

1. Wählen Sie die Registerkarte **Eigenschaften**, um das Layout der Eigenschaften für Ihre verbundene Klimaanlage zu bearbeiten.

2. Sie können die Eigenschaftenfelder verschieben und ihre Größe ändern:

    ![Bearbeiten des Layouts der Eigenschaften](media/tutorial-customize-operator/propertieslayout.png)

## <a name="preview-the-device"></a>Vorschau des Geräts

Auf der Seite **Gerätevorlagen** können Sie die Registerkarten „Dashboard“, „Einstellungen“ und „Eigenschaften“ für einen Bediener anpassen. Auf der Seite **Device Explorer** können Sie die Gerätevorlage anzeigen und verwenden.

1. Wenn Sie die Vorlage für die verbundene Klimaanlage als Bediener anzeigen und verwenden möchten, navigieren Sie zur Seite **Device Explorer**, und wählen Sie das simulierte Gerät aus, das von IoT Central basierend auf Ihrer Vorlage erstellt wurde:

    ![Anzeigen und Verwenden der Gerätevorlage](media/tutorial-customize-operator/usetemplate.png)

2. Wählen Sie zum Aktualisieren des Standorts dieses Geräts die Option **Eigenschaften**, und bearbeiten Sie den Wert auf der Kachel für den Standort. Wählen Sie dann **Speichern** aus:

    ![Eingeben eines Eigenschaftswerts](media/tutorial-customize-operator/editproperty.png)

3. Klicken Sie auf **Einstellungen**, ändern Sie einen Einstellungswert in einer Kachel, und klicken Sie anschließend auf **Aktualisieren**, um eine Einstellung an Ihre verbundene Klimaanlage zu senden:

    ![Senden einer Einstellung an das Gerät](media/tutorial-customize-operator/sendsetting.png)

    Wenn das Gerät den neuen Einstellungswert bestätigt, wird die Einstellung auf der Kachel als **synchronisiert** angezeigt.

4. Als Bediener können Sie das vom Ersteller konfigurierte Gerätedashboard anzeigen:

    ![Ansicht des Gerätedashboards für Bediener](media/tutorial-customize-operator/operatordashboard.png)

## <a name="configure-the-default-dashboard"></a>Konfigurieren des Standarddashboards

Wenn sich ein Ersteller oder Bediener bei einer Azure IoT Central-Anwendung anmeldet, wird ein Anwendungsdashboard angezeigt. Ersteller können den Inhalt des Standarddashboards anpassen, sodass einem Bediener möglichst hilfreiche und relevante Inhalte angezeigt werden.

> [!NOTE]
> Benutzer können auch eigene persönliche Dashboards erstellen und ein Dashboard als Standard auswählen.

1. Navigieren Sie zum Anpassen des Standardanwendungsdashboards zur Seite **Dashboard**, und wählen Sie rechts oben auf der Seite **Bearbeiten** aus. Ein Panel mit einer Bibliothek mit Objekten wird angezeigt, die Sie zum Dashboard hinzufügen können.

    ![Dashboardseite](media/tutorial-customize-operator/builderhome.png)

2. Fügen Sie zum Anpassen des Dashboards Kacheln aus der **Bibliothek** hinzu. Klicken Sie auf **Link**, und fügen Sie Details zur Website Ihrer Organisation hinzu. Klicken Sie anschließend auf **Speichern**:

    ![Hinzufügen eines Links zum Dashboard](media/tutorial-customize-operator/addlink.png)

    > [!NOTE]
    > Sie können auch Links zu Seiten innerhalb Ihrer Azure IoT Central-Anwendung hinzufügen. So können Sie beispielsweise einen Link zu einem Gerätedashboard oder zu einer Einstellungsseite hinzufügen.

3. Wählen Sie optional **Bild** aus, und laden Sie ein Bild hoch, das auf Ihrem Dashboard angezeigt werden soll. Ein Bild kann über eine URL verfügen, zu der Sie navigieren, wenn Sie darauf klicken:

    ![Hinzufügen eines Bilds zum Dashboard](media/tutorial-customize-operator/addimage.png)

    Weitere Informationen finden Sie unter [Vorbereiten und Hochladen von Bildern in eine Azure IoT Central-Anwendung](howto-prepare-images.md).

## <a name="preview-the-dashboard"></a>Vorschau des Dashboards

Um als Bediener eine Vorschau des Dashboards anzuzeigen, wählen Sie rechts oben auf der Seite **Fertig** aus.

![Umschalten des Entwurfsmodus](media/tutorial-customize-operator/operatorviewhome.png)

Sie können auf die Link- und Bildkacheln klicken, um zu den URLs zu navigieren, die Sie als Ersteller festgelegt haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die Anwendungsdarstellung für Bediener anpassen.

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Konfigurieren Ihres Gerätedashboards
> * Konfigurieren des Layouts Ihrer Geräteeinstellungen
> * Konfigurieren des Layouts Ihrer Geräteeigenschaften
> * Anzeigen einer Vorschau des Geräts als Bediener
> * Konfigurieren Ihrer Standardstartseite
> * Anzeigen einer Vorschau der Standardstartseite als Bediener

Nachdem Sie nun wissen, wie Sie die Anwendungsdarstellung für Bediener anpassen, können Sie mit folgenden Schritten fortfahren:

* [überwachen Ihrer Geräte](tutorial-monitor-devices.md) (als Bediener)
* [Hinzufügen eines neuen Geräts zu Ihrer Anwendung](tutorial-add-device.md) (als Bediener und Geräteentwickler)