---
title: Überwachen Ihrer Geräte in Azure IoT Central | Microsoft-Dokumentation
description: Als Bediener können Sie mit der Azure IoT Central-Anwendung Ihre Geräte überwachen.
author: dominicbetts
ms.author: dobett
ms.date: 10/24/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: de83315f38766bc1e2f37ac82452bb023930f241
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73896228"
---
# <a name="quickstart-use-azure-iot-central-to-monitor-your-devices-preview-features"></a>Schnellstart: Überwachen Ihrer Geräte mithilfe von Azure IoT Central (Previewfunktionen)

*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Diese Schnellstartanleitung richtet sich an Bediener und erläutert, wie Sie mithilfe Ihrer Microsoft Azure IoT Central-Anwendung Ihre Geräte überwachen und Einstellungen ändern.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, sollten Sie die drei vorherigen Schnellstartanleitungen durcharbeiten: [Erstellen einer Azure IoT Central-Anwendung (Previewfunktionen)](./quick-deploy-iot-central.md), [Schnellstart: Hinzufügen eines simulierten Geräts zu Ihrer IoT Central-Anwendung (Previewfunktionen)](./quick-create-pnp-device.md) und [Schnellstart: Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central (Previewfunktionen)](quick-configure-rules.md).

## <a name="receive-a-notification"></a>Empfangen einer Benachrichtigung

Azure IoT Central sendet Benachrichtigungen für Geräte in Form von E-Mails. Der Ersteller hat eine Regel hinzugefügt, die dafür sorgt, dass eine Benachrichtigung gesendet wird, wenn die Temperatur in einem verbundenen Umgebungssensor einen Schwellenwert übersteigt. Überprüfen Sie die E-Mails, die an das Konto gesendet wurden, das der Ersteller für den Empfang von Benachrichtigungen ausgewählt hat.

Öffnen Sie die E-Mail, die Sie am Ende der Schnellstartanleitung [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](quick-configure-rules.md) erhalten haben. Wählen Sie in der E-Mail den Link zum Gerät aus:

![Benachrichtigungs-E-Mail mit Warnung](media/quick-monitor-devices/email.png)

Die Ansicht **Dashboard** für den simulierten Umgebungssensor, den Sie in den vorherigen Schnellstartanleitungen erstellt haben, wird in Ihrem Browser geöffnet:

![Gerät, das die Benachrichtigungs-E-Mail ausgelöst hat](media/quick-monitor-devices/dashboard.png)

## <a name="investigate-an-issue"></a>Untersuchen eines Problems

Als Bediener können Sie auf den Seiten **Übersicht**, **Environmental Sensor properties** (Eigenschaften des Umgebungssensors) und **Befehle** Informationen zum Gerät anzeigen. Der Ersteller hat die Seiten **Dashboard** und **Environmental Sensor properties** (Eigenschaften des Umgebungssensors) angepasst, um wichtige Informationen zu einem verbundenen Umgebungssensor anzuzeigen.

Wählen Sie die Ansicht **Übersicht** aus, um Informationen zum Gerät anzuzeigen.

Das Diagramm auf dem Dashboard zeigt einen Plot der Gerätetemperatur. Sie entscheiden, dass die Gerätetemperatur zu hoch ist.

## <a name="remediate-an-issue"></a>Beheben eines Problems

Über die Seite **Environmental Sensor properties** (Eigenschaften des Umgebungssensors) können Sie das Gerät ändern.

Wählen Sie **Environmental Sensor properties** (Eigenschaften des Umgebungssensors) aus. Legen Sie für **Brightness Level** (Helligkeitsstufe) 10 fest. Wählen Sie **Speichern** aus, um das Gerät zu aktualisieren. Wenn das Gerät die Einstellungsänderung bestätigt, ändert sich der Status der Eigenschaft in **Synchronisiert**:

![Aktualisieren der Einstellungen](media/quick-monitor-devices/change-settings.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

* Empfangen einer Benachrichtigung
* Untersuchen eines Problems
* Beheben eines Problems

Nachdem Sie nun wissen, wie Sie Ihr Gerät überwachen, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Einrichten und Verwalten einer Gerätevorlage (Previewfunktionen)](howto-set-up-template.md)
