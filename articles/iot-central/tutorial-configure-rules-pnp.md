---
title: Konfigurieren von Regeln und Aktionen in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Tutorial wird gezeigt, wie Sie als Ersteller telemetriebasierte Regeln und Aktionen in Ihrer Azure IoT Central-Anwendung konfigurieren.
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: c2aa6edfe7ce9b670ea1015e2da72f3ecc48c9ec
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878831"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central-preview-features"></a>Tutorial: Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central (Previewfunktionen)

*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

In diesem Tutorial erstellen Sie eine Regel, die eine E-Mail sendet, wenn die Temperatur in einem Umgebungssensor 90&deg; F überschreitet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer telemetriebasierten Regel
> * Hinzufügen einer Aktion

## <a name="prerequisites"></a>Voraussetzungen

Absolvieren Sie zuerst das Tutorial unter [Tutorial: Define a new device type in your Azure IoT Central application (preview features)](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Tutorial: Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung (Previewfunktionen)), um die Gerätevorlage **Environment Sensor** (Umgebungssensor) zu erstellen.

## <a name="create-a-telemetry-based-rule"></a>Erstellen einer telemetriebasierten Regel

1. Wählen Sie im linken Navigationsmenü die Option **Regeln** aus, um Ihrer Anwendung eine neue telemetriebasierte Regel hinzuzufügen:

1. Wählen Sie **+ Neu** aus, um eine neue Regel zu erstellen. Wählen Sie dann **Telemetrie** aus.

1. Geben Sie als Regelname **Environmental temperature** (Umgebungstemperatur) ein.

1. Wählen Sie im Abschnitt **Bereich** die Gerätevorlage **Environment Sensor** (Umgebungssensor) aus. Der Bereich der Geräte, auf die sich diese Regel bezieht. Mithilfe von **+ Filter** können Sie weitere Filterkriterien hinzufügen.

1. Im Abschnitt **Bedingung** definieren Sie, wodurch Ihre Regel ausgelöst wird. Verwenden Sie die folgenden Informationen, um eine Bedingung basierend auf Temperaturtelemetrie zu definieren:

    | Feld                                        | Wert                             |
    | -------------------------------------------- | ------------------------------    |
    | Messung                                  | Temperatur                       |
    | Operator                                     | größer als                   |
    | Wert                                        | 90                                |

    Wählen Sie **+ Bedingung** aus, um weitere Bedingungen hinzuzufügen.

    ![Erstellen einer Regelbedingung](./media/tutorial-configure-rules-pnp/condition.png)

1. Um eine Aktion hinzuzufügen, die bei Auslösung der Regel ausgeführt werden soll, wählen Sie **+ Aktion** und anschließend **E-Mail** aus.

1. Nutzen Sie beim Festlegen der Aktion die Informationen in der folgenden Tabelle:

    | Einstellung   | Wert                                             |
    | --------- | ------------------------------------------------- |
    | To        | Ihre E-Mail-Adresse                                |
    | Notizen     | Die Umgebungstemperatur hat den Schwellenwert überschritten. |

    > [!NOTE]
    > Um eine E-Mail-Benachrichtigung zu erhalten, muss es sich bei der E-Mail-Adresse um eine [Benutzer-ID in der Anwendung](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) handeln, und dieser Benutzer muss sich mindestens einmal bei der Anwendung angemeldet haben.

    ![Erstellen einer Regelaktion](./media/tutorial-configure-rules-pnp/action.png)

1. Wählen Sie **Speichern** aus. Ihre Regel ist auf der Seite **Regeln** aufgeführt.

## <a name="test-the-rule"></a>Testen der Regel

Die Regel wird kurz nach dem Speichern aktiv. Wenn die in der Regel festgelegten Bedingungen erfüllt werden, sendet Ihre Anwendung eine Nachricht an die in der Aktion angegebene E-Mail-Adresse.

> [!NOTE]
> Deaktivieren Sie die Regel nach Abschluss der Tests, um keine weiteren Warnungen in Ihrem Posteingang zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Erstellen einer telemetriebasierten Regel
* Hinzufügen einer Aktion

Sie haben eine schwellenwertbasierte Regel festgelegt. Als nächster Schritt wird Folgendes empfohlen:

> [!div class="nextstepaction"]
> [Erstellen eine Ereignisregel und Einrichten von Benachrichtigungen in Ihrer Azure IoT Central-Anwendung](howto-create-event-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
