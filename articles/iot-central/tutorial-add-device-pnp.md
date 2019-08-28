---
title: Hinzufügen eines echten Geräts zu einer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Fügen Sie Ihrer Azure IoT Central-Anwendung als Bediener ein echtes Gerät hinzu.
author: sarahhubbard
ms.author: sahubbar
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 7191c98cfc522068dbea576a8f81776ae4301da8
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878861"
---
# <a name="tutorial-add-a-simulated-device-to-your-azure-iot-central-application-preview-features"></a>Tutorial: Hinzufügen eines simulierten Geräts zu Ihrer Azure IoT Central-Anwendung (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

In diesem Tutorial erfahren Sie, wie Sie Ihrer Microsoft Azure IoT Central-Anwendung ein simuliertes Gerät hinzufügen und das Gerät konfigurieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Hinzufügen eines neuen simulierten Geräts
> * Verwenden eines simulierten Geräts auf der Erstellungsoberfläche

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, muss der Ersteller das erste Tutorial für Ersteller absolvieren, um die Azure IoT Central-Anwendung zu erstellen:

* [Definieren eines neuen Gerätetyps](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (obligatorisch)

## <a name="add-a-simulated-device"></a>Hinzufügen eines simulierten Geräts

Um Ihrer Anwendung ein echtes Gerät hinzuzufügen, verwenden Sie die Gerätevorlage **Environmental Sensor** (Umgebungssensor), die Sie im Tutorial [Definieren eines neuen Gerätetyps](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) erstellt haben.

1. Wählen Sie als Bediener im linken Navigationsmenü die Option **Geräte** aus, um ein neues Gerät hinzuzufügen. Auf der Registerkarte **Geräte** werden die Option **Alle Geräte** und die Gerätevorlage **Environmental Sensor** (Umgebungssensor) angezeigt.

1. Wählen Sie zum Hinzufügen eines simulierten Umgebungssensors **+ Neu** aus. Verwenden Sie die vorgeschlagene **Geräte-ID**, oder geben Sie Ihre eigene **Geräte-ID** (in Kleinbuchstaben) ein. Sie können auch einen Namen für Ihr neues Gerät eingeben. Legen Sie die Umschaltfläche **Simuliert** auf **Ein** fest, und wählen Sie dann **Erstellen** aus.

    ![Simuliertes Gerät](./media/tutorial-add-device-pnp/simulated-device.png)

Sie können nun mit den Ansichten interagieren, die vom Ersteller für die Gerätevorlage mit simulierten Daten erstellt wurde.

## <a name="use-a-simulated-device-to-improve-views"></a>Verwenden eines simulierten Geräts zum Verbessern von Ansichten

Nach der Erstellung eines neuen simulierten Geräts kann der Ersteller dieses Gerät verwenden, um die Ansichten für die Gerätevorlage weiter zu verbessern und darauf aufzubauen.

1. Kopieren Sie in der Geräteansicht die **Geräte-ID** des simulierten Geräts, das Sie erstellt haben.

1. Wählen Sie im linken Navigationsmenü die Registerkarte **Gerätevorlagen** und dann die Vorlage **Environmental Sensor** (Umgebungssensor) aus.

1. Wählen Sie eine der Ansichten aus, die Sie bearbeiten möchten, oder erstellen Sie eine neue Ansicht. Klicken Sie auf **Configure preview device** (Vorschaugerät konfigurieren). Hier können Sie wählen, ob kein Vorschaugerät vorhanden ist oder ob Sie ein echtes Gerät verwenden möchten, das Sie zu Testzwecken konfigurieren können. Oder wählen Sie ein vorhandenes Gerät aus, das Sie in IoT Central hinzugefügt haben.

1. Wählen Sie **Select from a running device** (Von einem ausgeführten Gerät auswählen) aus, und geben Sie die **Geräte-ID** des simulierten Geräts ein, die Sie kopiert haben.

1. Wählen Sie **Anwenden** aus. Nun sehen Sie das gleiche simulierte Gerät auf der Erstellungsoberfläche für Gerätevorlagenansichten. Diese Ansicht ist besonders nützlich für Diagramme und andere Visualisierungen.

    ![Konfigurieren des Vorschaugeräts](./media/tutorial-add-device-pnp/configure-preview.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Hinzufügen eines neuen simulierten Geräts
* Verwenden eines simulierten Geräts auf der Erstellungsoberfläche

Nachdem Sie nun eine Verbindung zwischen einem simulierten Gerät und Ihrer Azure IoT Central-Anwendung hergestellt haben, schlagen wir Ihnen die folgenden weiteren Schritte vor:

Bediener:

> [!div class="nextstepaction"]
> [Konfigurieren von Regeln](tutorial-configure-rules.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)

Geräteentwickler:

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung zwischen einem MXChip IoT DevKit-Gerät und Ihrer Azure IoT Central-Anwendung](howto-connect-devkit-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)



