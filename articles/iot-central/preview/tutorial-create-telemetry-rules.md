---
title: Erstellen und Verwalten von Regeln in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Azure IoT Central-Regeln ermöglichen Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen, wie das Senden einer E-Mail, durch Auslösen der Regel automatisch aufzurufen.
author: dominicbetts
ms.author: dobett
ms.date: 10/24/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 6327ee8a1b0d52c933844670d9b8098c2c3c4f09
ms.sourcegitcommit: 39da2d9675c3a2ac54ddc164da4568cf341ddecf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73958140"
---
# <a name="tutorial-create-a-rule-and-set-up-notifications-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Erstellen einer Regel und Einrichten von Benachrichtigungen in Ihrer Azure IoT Central-Anwendung (Previewfunktionen)

*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Mithilfe von Azure IoT Central können Sie Ihre verbundenen Geräte remote überwachen. Azure IoT Central-Regeln ermöglichen Ihnen, Ihre Geräte nahezu in Echtzeit zu überwachen und Aktionen (wie das Senden einer E-Mail) automatisch aufzurufen. Mit nur wenigen Mausklicks können Sie eine Bedingung für die Überwachung von Gerätetelemetriedaten definieren und eine entsprechende Aktion konfigurieren. In diesem Artikel wird erläutert, wie Regeln zum Überwachen von Telemetriedaten erstellt werden, die vom Gerät gesendet werden.

Telemetriedaten werden verwendet, um numerische Daten vom Gerät zu senden. Eine Regel wird ausgelöst, wenn die ausgewählten Gerätetelemetriedaten einen angegebenen Schwellenwert überschreiten.

In diesem Tutorial erstellen Sie eine Regel, um eine E-Mail zu senden, wenn die von einem Umgebungssensorgerät gemessene Temperatur 80 &deg;F übersteigt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Regel
> * Hinzufügen einer E-Mail-Aktion

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie zunächst mithilfe der Schnellstartanleitungen [Erstellen einer Azure IoT Central-Anwendung (Previewfunktionen)](./quick-deploy-iot-central.md) und [Schnellstart: Hinzufügen eines simulierten Geräts zu Ihrer IoT Central-Anwendung (Previewfunktionen)](./quick-create-pnp-device.md) die Gerätevorlage **Environment Sensor** (Umgebungssensor), um sie hier verwenden zu können.

## <a name="create-a-rule"></a>Erstellen einer Regel

Um eine Telemetrieregel zu erstellen, muss für die Gerätevorlage mindestens eine Telemetriemessung definiert sein. In diesem Tutorial wird ein Umgebungssensorgerät verwendet, das Telemetriedaten zur Temperatur und Luftfeuchtigkeit sendet. Die Schritte zum Hinzufügen dieser Gerätevorlage sowie zum Erstellen eines simulierten Geräts wurden im Rahmen der Schnellstartanleitung [Schnellstart: Hinzufügen eines simulierten Geräts zu Ihrer IoT Central-Anwendung (Previewfunktionen)](./quick-create-pnp-device.md) ausgeführt. Die Regel überwacht die vom Gerät gemeldete Temperatur und sendet eine E-Mail, sobald diese 80 Grad übersteigt.

1. Klicken Sie im linken Bereich auf **Regeln**.

1. Wenn Sie noch keine Regeln erstellt haben, wird der folgende Bildschirm angezeigt:

    ![Noch keine Regeln](media/tutorial-create-telemetry-rules/rules-landing-page1.png)

1. Wählen Sie **+** aus, um eine neue Regel hinzuzufügen.

1. Geben Sie den Namen _Temperature monitor_ ein, um die Regel zu identifizieren, und drücken Sie anschließend die EINGABETASTE.

1. Wählen Sie die Gerätevorlage **Environmental Sensor** (Umgebungssensor) aus. Die Regel gilt standardmäßig automatisch für alle Geräte, die der Gerätevorlage zugeordnet sind. Wenn Sie nach einer Teilmenge der Geräte filtern möchten, wählen Sie **+ Filter** aus, und verwenden Sie Geräteeigenschaften, um die Geräte anzugeben. Wenn Sie die Regel deaktivieren möchten, verwenden Sie die Umschaltfläche **Aktiviert/Deaktiviert** in der Kopfzeile der Regel:

    ![Filter und Aktivierung](media/tutorial-create-telemetry-rules/device-filters.png)

### <a name="configure-the-rule-conditions"></a>Konfigurieren der Regelbedingungen

Die von der Regel überwachten Kriterien werden mithilfe von Bedingungen definiert. In diesem Tutorial konfigurieren Sie die Regel so, dass sie ausgelöst wird, wenn die Temperatur den Wert 80 &deg;F übersteigt.

1. Wählen Sie in der Dropdownliste **Telemetrie** die Option **Temperatur** aus.

1. Wählen Sie als Nächstes unter **Operator** die Option **Ist größer als** aus, und geben Sie unter **Wert** die Zahl _80_ ein.

    ![Bedingung](media/tutorial-create-telemetry-rules/condition-filled-out1.png)

1. Optional kann auch eine **Zeitaggregation** festgelegt werden. Bei Verwendung einer Zeitaggregation muss im entsprechenden Dropdownmenü auch ein Aggregationstyp (beispielsweise „Durchschnitt“ oder „Summe“) ausgewählt werden.

    * Ohne Aggregation wird diese Regel für jeden Telemetriedatenpunkt ausgelöst, der die Bedingung erfüllt. Wenn beispielswiese die Regel so konfiguriert ist, dass sie bei einem Temperaturwert über 80 ausgelöst wird, erfolgt die Auslösung nahezu sofort, wenn das Gerät eine Temperatur von mehr als 80 meldet.
    * Mit Aggregation wird die Regel ausgelöst, wenn der Aggregatwert der Telemetriedatenpunkte im Zeitfenster die Bedingung erfüllt. Ein Beispiel: Die Regel ist so konfiguriert, dass sie ausgelöst wird, wenn die Temperatur über 80 Grad liegt, die Zeitaggregation ist auf zehn Minuten festgelegt, und als Aggregationstyp wurde „Durchschnitt“ ausgewählt. In diesem Fall wird die Regel ausgelöst, wenn vom Gerät eine durchschnittliche Temperatur von über 80 Grad gemeldet wird (berechnet für ein zehnminütiges Intervall).

     ![Aggregatbedingung](media/tutorial-create-telemetry-rules/aggregate-condition-filled-out1.png)

Sie können einer Regel mehrere Bedingungen hinzufügen, indem Sie **+ Bedingung** auswählen. Wenn mehrere Bedingungen angegeben werden, müssen alle Bedingungen erfüllt sein, damit die Regel ausgelöst wird. Jede Bedingung wird durch eine implizite `AND`-Klausel verknüpft. Bei Verwendung einer Zeitaggregation mit mehreren Bedingungen müssen alle Telemetriewerte aggregiert werden.

### <a name="configure-actions"></a>Konfigurieren von Aktionen

Nachdem Sie die Bedingung definiert haben, richten Sie die Aktionen ein, die ausgeführt werden sollen, wenn die Regel ausgelöst wird. Aktionen werden aufgerufen, wenn alle in der Regel angegebenen Bedingungen als „true“ ausgewertet werden. Aktuell steht nur die Aktion „E-Mail“ zur Verfügung.

1. Wählen Sie im Abschnitt **Aktionen** die Option **+ E-Mail** aus.

1. Geben Sie _Temperature warning_ als Anzeigenamen für die Aktion, Ihre E-Mail-Adresse in das Feld **An** und den Hinweis _You should check the device!_ als E-Mail-Text ein.

    > [!NOTE]
    > E-Mails werden nur an die Benutzer gesendet, die der Anwendung hinzugefügt wurden und sich mindestens einmal angemeldet haben. Erfahren Sie mehr über die [Benutzerverwaltung](howto-administer.md) in Azure IoT Central.

   ![Konfigurieren einer Aktion](media/tutorial-create-telemetry-rules/configure-action1.png)

1. Wählen Sie zum Speichern der Aktion die Option **Fertig** aus. Einer Regel können mehrere Aktionen hinzugefügt werden.

1. Um die neue Regel zu speichern, wählen Sie **Speichern** aus. Die Regel geht innerhalb weniger Minuten live und beginnt mit der Überwachung der Telemetriedaten, die an Ihre Anwendung gesendet werden. Wenn die in der Regel festgelegte Bedingung erfüllt ist, löst die Regel die konfigurierte E-Mail-Aktion aus.

Nach einer Weile erhalten Sie eine E-Mail, wenn die Regel ausgelöst wird:

![Beispiel-E-Mail](media/tutorial-create-telemetry-rules/email.png)

## <a name="delete-a-rule"></a>Löschen einer Regel

Wenn Sie eine Regel nicht mehr benötigen, löschen Sie sie, indem Sie die Regel öffnen und **Löschen** wählen.

## <a name="enable-or-disable-a-rule"></a>Aktivieren oder Deaktivieren einer Regel

Wählen Sie die Regel aus, die Sie aktivieren oder deaktivieren möchten. Verwenden Sie in der Regel die Schaltfläche **Aktivieren** oder **Deaktivieren**, um die Regel für alle in der Regel definierten Geräte zu aktivieren oder zu deaktivieren.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Aktivieren oder Deaktivieren einer Regel für ein Gerät

Wählen Sie die Regel aus, die Sie aktivieren oder deaktivieren möchten. Fügen Sie im Abschnitt **Bereiche** einen Filter hinzu, um ein bestimmtes Gerät in die Gerätevorlage aufzunehmen oder daraus auszuschließen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Erstellen einer telemetriebasierten Regel
* Hinzufügen einer Aktion

Nachdem Sie nun eine schwellenwertbasierte Regel definiert haben, können Sie sich als Nächstes mit Folgendem befassen:

> [!div class="nextstepaction"]
> [Exportieren Ihrer Azure IoT Central-Daten (Previewfunktionen)](./howto-export-data.md)
