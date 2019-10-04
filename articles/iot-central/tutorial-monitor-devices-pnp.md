---
title: Überwachen Ihrer Geräte in Azure IoT Central | Microsoft-Dokumentation
description: Als Bediener können Sie mit der Azure IoT Central-Anwendung Ihre Geräte überwachen.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: cf50fd80bdbce5895bd1da39700d1c6e4cdcc230
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878781"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices-preview-features"></a>Tutorial: Überwachen Ihrer Geräte mithilfe von Azure IoT Central (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Dieses Tutorial richtet sich an Bediener und erläutert, wie Sie mithilfe Ihrer Microsoft Azure IoT Central-Anwendung Ihre Geräte überwachen und Einstellungen ändern.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Empfangen einer Benachrichtigung
> * Untersuchen eines Problems
> * Beheben eines Problems

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, muss der Ersteller die drei Tutorials für Ersteller absolvieren, um die Azure IoT Central-Anwendung zu erstellen:

* [Definieren eines neuen Gerätetyps](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
* [Hinzufügen eines Geräts](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
* [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)

## <a name="receive-a-notification"></a>Empfangen einer Benachrichtigung

Azure IoT Central sendet Benachrichtigungen für Geräte in Form von E-Mails. Der Ersteller hat eine Regel hinzugefügt, die dafür sorgt, dass eine Benachrichtigung gesendet wird, wenn die Temperatur in einem verbundenen Umgebungssensor einen Schwellenwert übersteigt. Überprüfen Sie die E-Mails, die an das Konto gesendet wurden, das der Ersteller für den Empfang von Benachrichtigungen ausgewählt hat.

Öffnen Sie die E-Mail, die Sie am Ende des Tutorials [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) erhalten haben. Wählen Sie in der E-Mail den Link zum Gerät aus:

![Benachrichtigungs-E-Mail mit Warnung](media/tutorial-monitor-devices-pnp/email.png)

Die Ansicht **Dashboard** für den simulierten Umgebungssensor, den Sie in den vorherigen Tutorials erstellt haben, wird in Ihrem Browser geöffnet:

![Gerät, das die Benachrichtigungs-E-Mail ausgelöst hat](media/tutorial-monitor-devices-pnp/dashboard.png)

## <a name="investigate-an-issue"></a>Untersuchen eines Problems

Als Bediener können Sie auf den Seiten **Dashboard**, **Environmental Sensor properties** (Eigenschaften des Umgebungssensor) und **Befehle** Informationen zum Gerät anzeigen. Der Ersteller hat die Seiten **Dashboard** und **Environmental Sensor properties** (Eigenschaften des Umgebungssensors) angepasst, um wichtige Informationen zu einem verbundenen Umgebungssensor anzuzeigen.

Klicken Sie auf die Ansicht **Dashboard**, um Informationen zum Gerät anzuzeigen.

Das Diagramm auf dem Dashboard zeigt einen Plot der Gerätetemperatur. Auf der Kachel **Geräteeigenschaften** wird zudem die aktuelle Zieltemperatur für das Gerät angezeigt. Sie entscheiden, dass die Zieltemperatur zu hoch ist.

## <a name="remediate-an-issue"></a>Beheben eines Problems

Über die Seite **Environmental Sensor properties** (Eigenschaften des Umgebungssensors) können Sie das Gerät ändern:

1. Wählen Sie **Environmental Sensor properties** (Eigenschaften des Umgebungssensors) aus. Legen Sie für **Brightness Level** (Helligkeitsstufe) 10 fest. Wählen Sie **Speichern** aus, um das Gerät zu aktualisieren. Wenn das Gerät die Einstellungsänderung bestätigt, ändert sich der Status der Eigenschaft in **Synchronisiert**:

    ![Aktualisieren der Einstellungen](media/tutorial-monitor-devices-pnp/change-settings.png)

2. Wählen Sie **Dashboard** aus, und überprüfen Sie den neuen Eigenschaftswert:

    ![Aktualisiertes Gerätedashboard](media/tutorial-monitor-devices-pnp/new-settings.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Empfangen einer Benachrichtigung
* Untersuchen eines Problems
* Beheben eines Problems

Nachdem Sie nun wissen, wie Sie Ihr Gerät überwachen, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](tutorial-configure-rules.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)