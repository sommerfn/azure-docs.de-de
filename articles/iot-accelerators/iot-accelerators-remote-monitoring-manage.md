---
title: 'Konfigurieren von Geräten in der Remoteüberwachungslösung: Azure | Microsoft-Dokumentation'
description: In diesem Tutorial wird veranschaulicht, wie Sie die Geräte konfigurieren, die mit dem Solution Accelerator für die Remoteüberwachung verbunden sind.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 03/08/2019
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: e4236952bd41c4955e337813ff6d706263b8ef47
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73890899"
---
# <a name="tutorial-configure-devices-connected-to-your-monitoring-solution"></a>Tutorial: Konfigurieren von Geräten, die mit der Überwachungslösung verbunden sind

In diesem Tutorial verwenden Sie den Solution Accelerator für die Remoteüberwachung zum Konfigurieren und Verwalten Ihrer verbundenen IoT-Geräte. Sie fügen dem Solution Accelerator ein neues Gerät hinzu und konfigurieren das Gerät.

Contoso hat neue Maschinen bestellt, um eine seiner Betriebsstätten zu erweitern. Während Sie auf die Lieferung der neuen Maschinen warten, möchten Sie eine Simulation ausführen, um das Verhalten der Lösung zu testen. Zum Ausführen der Simulation fügen Sie dem Solution Accelerator für die Remoteüberwachung ein neues simuliertes Engine-Gerät hinzu und testen, ob dieses simulierte Gerät richtig auf Konfigurationsupdates reagiert. In diesem Tutorial werden simulierte Geräte genutzt, aber ein Geräteentwickler kann direkte Methoden auch auf einem [echten Gerät implementieren, das mit dem Solution Accelerator für die Remoteüberwachung verbunden ist](iot-accelerators-connecting-devices.md).

In diesem Tutorial führen Sie Folgendes durch:

>[!div class="checklist"]
> * Bereitstellen eines simulierten Geräts
> * Testen eines simulierten Geräts
> * Neukonfigurieren eines Geräts
> * Organisieren Ihrer Geräte

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

## <a name="add-a-simulated-device"></a>Hinzufügen eines simulierten Geräts

Navigieren Sie in der Lösung zur Seite **Device Explorer**, und klicken Sie dann auf **+ Neues Gerät**:

[![Bereitstellen eines simulierten Geräts](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovision-expanded.png#lightbox)

Wählen Sie im Panel **New device** (Neues Gerät) die Option **Simulated** (Simuliert), behalten Sie für die Anzahl von bereitzustellenden Geräten **1** bei, wählen Sie das Gerätemodell **Faulty Engine** (Fehlerhafte Engine), und wählen Sie dann **Apply** (Übernehmen):

[![Bereitstellen eines simulierten Engine-Geräts](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesprovisionengine-expanded.png#lightbox)

## <a name="test-the-simulated-device"></a>Testen des simulierten Geräts

Um zu testen, ob das simulierte Engine-Gerät Telemetriedaten sendet und Eigenschaftswerte meldet, wählen Sie es auf der Seite **Device Explorer** in der Liste mit den Geräten aus. Live-Informationen zur Engine werden im Bereich **Device Details** (Gerätedetails) angezeigt:

[![Anzeigen des neuen simulierten Engine-Geräts](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesviewnew-expanded.png#lightbox)

Überprüfen Sie unter **Device Details** (Gerätedetails), ob das neue Gerät Telemetriedaten sendet. Klicken Sie auf **Vibration**, um die gestreamten Telemetriedaten zur Vibration Ihres Geräts anzuzeigen:

[![Auswählen eines anzuzeigenden Telemetriedatenstroms](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesvibration-expanded.png#lightbox)

Im Bereich **Device Details** (Gerätedetails) werden weitere Informationen zum Gerät angezeigt, z.B. Tagwerte, die unterstützten Methoden und die vom Gerät gemeldeten Eigenschaften.

Scrollen Sie zum Anzeigen von detaillierten Diagnosedaten im Panel **Device Details** (Gerätedetails) nach unten, um den Abschnitt **Diagnose** anzuzeigen.

## <a name="reconfigure-a-device"></a>Neukonfigurieren eines Geräts

Um zu testen, ob Sie die Konfigurationseigenschaften der Engine aktualisieren können, wählen Sie sie auf der Seite **Device Explorer** in der Liste mit den Geräten aus. Klicken Sie anschließend auf **Aufträge**, und wählen Sie **Eigenschaften** aus. Im Panel „Jobs“ (Aufträge) werden die aktualisierbaren Eigenschaftswerte für das ausgewählte Gerät angezeigt:

[![Neukonfigurieren eines Geräts](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigure-expanded.png#lightbox)

Um den Standort der Engine zu aktualisieren, legen Sie den Auftragsnamen auf **UpdateEngineLocation**, den Längengrad auf **-122,15**, den Standort auf **Factory 2** und den Breitengrad auf **47,62** fest und klicken anschließend auf **Apply** (Übernehmen):

[![Aktualisieren des Eigenschaftswerts eines Geräts](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesreconfigurephysical-expanded.png#lightbox)

Klicken Sie auf **View job status** (Auftragsstatus anzeigen), um den Status des Auftrags nachzuverfolgen:

[![Aktualisieren des Eigenschaftswerts eines Geräts](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-inline.png)](./media/iot-accelerators-remote-monitoring-manage/locationjobstatus-expanded.png#lightbox)

Nachdem der Auftrag abgeschlossen ist, können Sie zur Seite **Dashboard** navigieren. Das Engine-Gerät wird auf der Karte am neuen Standort angezeigt:

[![Anzeigen des Engine-Standorts](./media/iot-accelerators-remote-monitoring-manage/enginelocation-inline.png)](./media/iot-accelerators-remote-monitoring-manage/enginelocation-expanded.png#lightbox)

## <a name="organize-your-devices"></a>Organisieren Ihrer Geräte

Um für Sie als Bediener das Organisieren und Verwalten Ihrer Geräte zu vereinfachen, sollten Sie sie mit einem Teamnamen kennzeichnen. Contoso verfügt über zwei unterschiedliche Teams für Außendienstaktivitäten:

* Das Team „Smart Vehicle“ (Intelligentes Fahrzeug) verwaltet LKW und Geräte für die Prototyperstellung.
* Das Team „Smart Building“ (Intelligentes Gebäude) verwaltet Kältemaschinen, Aufzüge und Motoren.

Navigieren Sie zum Anzeigen Ihrer gesamten Geräte zur Seite **Device Explorer**, und wählen Sie den Filter **Alle Geräte**:

[![Anzeigen aller Geräte](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesalldevices-expanded.png#lightbox)

### <a name="add-tags"></a>Hinzufügen von Tags

Wählen Sie alle Geräte vom Typ **Trucks** (LKW) und **Prototyping** (Prototyperstellung) aus. Klicken Sie dann auf **Jobs** (Aufträge).

Wählen Sie im Panel **Jobs** (Aufträge) die Option **Tag**, legen Sie den Auftragsnamen auf **AddConnectedVehicleTag** fest, und fügen Sie dann ein Texttag mit dem Namen **FieldService** und dem Wert **ConnectedVehicle** hinzu. Klicken Sie anschließend auf **Übernehmen**:

[![Hinzufügen von Tags zu Prototyp- und LKW-Geräten](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag-expanded.png#lightbox)

Wählen Sie auf der Geräteseite alle Geräte vom Typ **Chiller** (Kältemaschine), **Elevator** (Aufzug) und **Engine** aus. Klicken Sie dann auf **Jobs** (Aufträge).

Wählen Sie im Panel **Jobs** (Aufträge) die Option **Tag**, legen Sie den Auftragsnamen auf **AddSmartBuildingTag** fest, und fügen Sie dann ein Texttag mit dem Namen **FieldService** und dem Wert **SmartBuilding** hinzu. Klicken Sie anschließend auf **Übernehmen**:

[![Hinzufügen eines Tags zu Geräten vom Typ „Kältemaschine“, „Aufzug“ und „Engine“](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesaddtag2-expanded.png#lightbox)

### <a name="create-filters"></a>Erstellen von Filtern

Sie können jetzt die Tagwerte zum Erstellen von Filtern verwenden. Klicken Sie auf der Seite **Device Explorer** auf **Gerätegruppen verwalten**:

[![Verwalten von Gerätegruppen](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-inline.png)](./media/iot-accelerators-remote-monitoring-manage/devicesmanagefilters-expanded.png#lightbox)

Erstellen Sie einen Textfilter, für den in der Bedingung der Tagname **FieldService** und der Wert **SmartBuilding** verwendet werden. Speichern Sie den Filter unter dem Namen **Smart Building**:

[![Erstellen des Filters „Smart Building“](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/smartbuildingfilter-expanded.png#lightbox)

Erstellen Sie einen Textfilter, für den in der Bedingung der Tagname **FieldService** und der Wert **ConnectedVehicle** verwendet werden. Speichern Sie den Filter unter dem Namen **Connected Vehicle**.

[![Erstellen des Filters „Connected Vehicle“](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-inline.png)](./media/iot-accelerators-remote-monitoring-manage/connectedvehiclefilter-expanded.png#lightbox)

Der Contoso-Bediener kann Geräte jetzt basierend auf dem Betriebsteam abfragen:

[![Erstellen des Filters „Connected Vehicle“](./media/iot-accelerators-remote-monitoring-manage/filterinaction-inline.png)](./media/iot-accelerators-remote-monitoring-manage/filterinaction-expanded.png#lightbox)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde veranschaulicht, wie Sie die Geräte konfigurieren und verwalten, die mit dem Solution Accelerator für die Remoteüberwachung verbunden sind. Um zu erfahren, wie Sie mit dem Solution Accelerator eine Ursachenanalyse für eine unerwartete Warnung durchführen, fahren Sie mit dem nächsten Tutorial fort.

> [!div class="nextstepaction"]
> [Ausführen einer Ursachenanalyse für eine Warnung](iot-accelerators-remote-monitoring-root-cause-analysis.md)
