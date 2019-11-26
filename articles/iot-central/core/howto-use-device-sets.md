---
title: Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Operator Gerätegruppen in der Azure IoT Central-Anwendung verwenden.
author: ellenfosborne
ms.author: elfarber
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 87bbab041405a085d405707ff419fbad55fdcd09
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72942275"
---
# <a name="use-device-sets-in-your-azure-iot-central-application"></a>Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

In diesem Artikel wird beschrieben, wie Sie als Operator Gerätegruppen in Ihrer Azure IoT Central-Anwendung verwenden.

Eine Gerätegruppe ist eine Liste mit Geräten, die zusammengefasst werden, da sie angegebenen Kriterien entsprechen. Gerätegruppen helfen Ihnen bei der Verwaltung, Visualisierung und Analyse aller im Umfang enthaltenen Geräte, indem sie Geräte in kleinere, logische Gruppen zusammenfassen. Sie können beispielsweise eine Gerätegruppe mit einer Liste aller Klimageräte in Seattle erstellen, damit ein Techniker in Seattle alle Geräte finden kann, für die er verantwortlich ist. In diesem Artikel erfahren Sie, wie Sie Gerätegruppen erstellen und konfigurieren.

## <a name="create-a-device-set"></a>Erstellen einer Gerätegruppe

So erstellen Sie eine Gerätegruppe

1. Wählen Sie im linken Bereich **Gerätegruppen** aus.

1. Wählen Sie **+ Neu** aus.

    ![Neue Gerätegruppe](media/howto-use-device-sets/image1.png)

1. Geben Sie Ihrer Gerätegruppe einen Namen, der in der gesamten Anwendung eindeutig ist. Sie können auch eine Beschreibung hinzufügen. Eine Gerätegruppe kann nur Geräte aus einer einzigen Gerätevorlage enthalten. Wählen Sie die Gerätevorlage, die für diese Gruppe verwendet werden soll.

1. Erstellen Sie die Abfrage, um die Geräte für die Gerätegruppe zu identifizieren, indem Sie eine Eigenschaft, einen Vergleichsoperator und einen Wert auswählen. Sie können mehrere Abfragen hinzufügen und Geräte, die **alle** Kriterien erfüllen, werden in die Gerätegruppe aufgenommen. Die von Ihnen erstellte Gerätegruppe ist für jeden zugänglich, der Zugriff auf die Anwendung hat, sodass jeder die Gerätegruppe anzeigen, ändern oder löschen kann.

    ![Abfrage der Gerätegruppe](media/howto-use-device-sets/image2.png)

    > [!NOTE]
    > Die Gerätegruppe ist eine dynamische Abfrage. Jedes Mal, wenn Sie die Liste der Geräte anzeigen, werden möglicherweise andere Geräte in der Liste angezeigt. Die Liste hängt davon ab, welche Geräte derzeit die Kriterien der Abfrage erfüllen.

1. Wählen Sie **Speichern** aus.

## <a name="configure-the-dashboard-for-your-device-set"></a>Konfigurieren des Dashboards für Ihre Gerätegruppe

Nachdem Sie Ihre Gerätegruppe erstellt haben, können Sie deren **Dashboard** konfigurieren. Das **Dashboard** ist die Startseite, auf der Sie Bilder und Links platzieren. Sie können auch Raster hinzufügen, die die Geräte in der Gerätegruppe auflisten.

1. Wählen Sie im linken Bereich **Gerätegruppen** aus.

1. Wählen Sie Ihre Gerätegruppe aus.

1. Klicken Sie auf die Registerkarte **Dashboard** .

1. Wählen Sie **Bearbeiten** aus.

    ![Entwurfsmodus aktiviert](media/howto-use-device-sets/image3.png)

1. Informationen zum Hinzufügen eines Bilds finden Sie unter [Vorbereiten und Hochladen von Bildern in eine Azure IoT Central-Anwendung](howto-prepare-images.md).

1. So fügen Sie eine Kachel mit einem Link hinzu
    1. Wählen Sie im rechten Bereich **Link** aus.
    1. Geben Sie für Ihren Link einen **Titel** ein.
    1. Wählen Sie eine URL aus, die bei Auswahl des Links geöffnet werden soll.
    1. Geben Sie für den Link eine Beschreibung ein, die unter dem **Titel** angezeigt wird.
    1. Wählen Sie **Speichern** aus.

        ![Speichern des Links](media/howto-use-device-sets/image7.png)

    1. Sie können die Kachel mit dem Link auf dem **Dashboard** verschieben und dessen Größe verändern.

1. Fügen Sie ein Raster hinzu. Ein Raster ist eine Tabelle von Geräten in der Gerätegruppe mit den von Ihnen gewählten Spalten.
    1. Wählen Sie im rechten Fensterbereich **Raster** aus.
    1. Geben Sie für Ihr Raster einen **Titel** ein.
    1. Wählen Sie über die Spalten aus, die angezeigt werden sollen über **Hinzufügen/Entfernen**. Im eingeblendeten Bereich wählen Sie nun die gewünschte Spalte und anschließend den Pfeil nach rechts, um sie auszuwählen.
    1. Klicken Sie auf **OK**.
    1. Wählen Sie **Speichern** aus.

        ![Speichern des Rasters](media/howto-use-device-sets/image9.png)

    1. Ziehen Sie das Raster per Drag & Drop auf das **Dashboard**.

        > [!NOTE]
        > Sie können mehrere Bilder, Links und Raster hinzufügen.
  
    1. Wählen Sie **Fertig**aus.

Mehr über die Verwendung von Kacheln in Azure IoT Central erfahren Sie unter [Verwenden von Dashboardkacheln](howto-use-tiles.md).

### <a name="configure-a-location-map-in-your-device-sets-dashboard"></a>Konfigurieren einer Standortkarte im Dashboard Ihrer Gerätegruppe

Sie können eine Karte hinzufügen, um den Standort der Geräte in Ihrer Gerätegruppe zu visualisieren.

Um eine Karte zum Dashboard Ihrer Gerätegruppe hinzuzufügen, muss in Ihrer Gerätevorlage eine Standortmessung oder Standorteigenschaft konfiguriert sein. Weitere Informationen finden Sie unter [Erstellen einer Standortmessung](howto-set-up-template.md) oder [Erstellen einer Standorteigenschaft](howto-set-up-template.md).

1. Wählen Sie im **Dashboard** der Gerätegruppe in der Bibliothek **Karte** aus.
2. Fügen Sie einen Titel hinzu, und wählen Sie die Standortmessung oder -eigenschaft, die Sie zuvor konfiguriert haben.
3. Klicken Sie auf **Speichern**. Die Kartenkachel zeigt dann die letzten bekannten Standorte der Geräte in Ihrer Gerätegruppe an.
4. Wenn ein Techniker das Dashboard der Gerätegruppe betrachtet, sieht er alle von Ihnen konfigurierten Kacheln, einschließlich Standortkarte.

Sie können die Größe der Kartenkachel auf dem Dashboard ändern. Bei Auswahl einer Stecknadel auf der Karte werden Geräteinformationen, Name und Standort angezeigt. Wählen Sie das Popupfenster aus, um zur Seite mit den Geräteeigenschaften zu wechseln.

## <a name="configure-the-list-for-your-device-set"></a>Konfigurieren der Liste für Ihre Gerätegruppe

Nachdem Sie Ihre Gerätegruppe erstellt haben, können Sie die **Liste** konfigurieren. Die **Liste** zeigt alle Geräte der Gerätegruppe in einer Tabelle mit den von Ihnen gewählten Spalten.

1. Wählen Sie im linken Bereich **Gerätegruppen** aus.

1. Wählen Sie die Registerkarte **Liste**.

1. Wählen Sie **Spaltenoptionen**.

    ![Spaltenoptionen](media/howto-use-device-sets/image11.png)

1. Wählen Sie die anzuzeigenden Spalten aus, indem Sie die gewünschte Spalte markieren und den Pfeil nach rechts wählen.

    ![Auswählen von Spalten](media/howto-use-device-sets/image12.png)

1. Klicken Sie auf **OK**.

## <a name="analytics"></a>Analytics

Die Analyse in Gerätegruppen entspricht der Hauptregisterkarte für die Analyse im linken Bereich. Mehr über Analysen erfahren Sie im Artikel [Erstellen von Analysen](howto-use-device-sets.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Gerätegruppen in Ihrer Azure IoT Central-Anwendung verwendet werden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Erstellen von Telemetrieregeln](howto-create-telemetry-rules.md)
