---
title: 'Tutorial: Konfigurieren von Regeln und Aktionen in Azure IoT Central'
description: In diesem Tutorial wird gezeigt, wie Sie als Ersteller telemetriebasierte Regeln und Aktionen in Ihrer Azure IoT Central-Anwendung konfigurieren.
author: ankitscribbles
ms.author: ankitgup
ms.date: 11/13/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 9c2c8c1bacf4abfa775747a03d2a4a6121b67714
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74106597"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Tutorial: Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central

*Dieser Artikel gilt für Betreiber, Ersteller und Administratoren.*

[!INCLUDE [iot-central-original-pnp](../../../includes/iot-central-original-pnp-note.md)]

In diesem Tutorial erstellen Sie eine Regel, die eine E-Mail sendet, wenn die Temperatur in einer verbundenen Klimaanlage 90&deg; F überschreitet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer telemetriebasierten Regel
> * Hinzufügen einer Aktion

## <a name="prerequisites"></a>Voraussetzungen

Absolvieren Sie zuerst das Tutorial [1: Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung](tutorial-define-device-type.md), um die Gerätevorlage **Connected Air Conditioner** (Verbundene Klimaanlage) zu erstellen.

## <a name="create-a-telemetry-based-rule"></a>Erstellen einer telemetriebasierten Regel

1. Wählen Sie im linken Bereich die Option **Gerätevorlagen** aus, um Ihrer Anwendung eine neue telemetriebasierte Regel hinzuzufügen:

    ![Seite „Gerätevorlagen“](media/tutorial-configure-rules/templatespage1.png)

    Sie sehen die Gerätevorlage **Connected Air Conditioner (1.0.0)** , die Sie im vorherigen Tutorial erstellt haben.

2. Wählen Sie zum Anpassen Ihrer Gerätevorlage die Vorlage **Connected Air Conditioner** aus, die Sie im vorherigen Tutorial erstellt haben.

3. Wählen Sie zum Hinzufügen einer telemetriebasierten Regel in der Ansicht **Regeln** die Option **Regeln** aus. Wählen Sie anschließend **+ Neue Regel** und dann die Option **Telemetrie** aus:

    ![Ansicht „Regeln“](media/tutorial-configure-rules/newrule.png)

5. Nutzen Sie beim Festlegen der Regel die Informationen in der folgenden Tabelle:

    | Einstellung                                      | Wert                             |
    | -------------------------------------------- | ------------------------------    |
    | NAME                                         | Temperaturwarnung der Klimaanlage |
    | Aktivieren der Regel für alle Geräte von dieser Vorlage | Andererseits                                |
    | Bedingung                                    | Temperature is greater than 90    |
    | Aggregation                                  | Keine                              |

    ![Temperaturregelbedingung](media/tutorial-configure-rules/temperaturerule.png)

    Klicken Sie dann auf **Speichern**.

## <a name="add-an-action"></a>Hinzufügen einer Aktion

Beim Festlegen einer Regel definieren Sie auch eine Aktion, die bei Erfüllung der Regelbedingungen ausgeführt werden soll. In diesem Tutorial erstellen Sie eine Regel mit einer Aktion, mit der eine E-Mail-Benachrichtigung gesendet wird.

1. Zum Hinzufügen einer **Aktion** müssen Sie die Regel zunächst **speichern** und dann im Bereich **Configure Telemetry Rule** (Telemetrieregel konfigurieren) nach unten scrollen. Wählen Sie das Pluszeichen ( **+** ) neben **Aktionen** und anschließend die Option **E-Mail**:

    ![Temperaturregelaktion](media/tutorial-configure-rules/addaction.png)

2. Nutzen Sie beim Festlegen der Aktion die Informationen in der folgenden Tabelle:

    | Einstellung   | Wert                          |
    | --------- | ------------------------------ |
    | To        | Ihre E-Mail-Adresse             |
    | Notizen     | Die Temperatur der Klimaanlage hat den Schwellenwert überschritten. |

    > [!NOTE]
    > Um eine E-Mail-Benachrichtigung zu erhalten, muss es sich bei der E-Mail-Adresse um eine [Benutzer-ID in der Anwendung](howto-administer.md) handeln, und dieser Benutzer muss sich mindestens einmal bei der Anwendung angemeldet haben.

    ![Temperaturaktion](media/tutorial-configure-rules/temperatureaction.png)

3. Wählen Sie **Speichern** aus. Ihre Regel ist auf der Seite **Regeln** aufgeführt.

## <a name="test-the-rule"></a>Testen der Regel

Die Regel wird kurz nach dem Speichern aktiv. Wenn die in der Regel festgelegten Bedingungen erfüllt werden, sendet Ihre Anwendung eine Nachricht an die in der Aktion angegebene E-Mail-Adresse.

> [!NOTE]
> Deaktivieren Sie die Regel nach Abschluss der Tests, um keine weiteren Warnungen in Ihrem Posteingang zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Erstellen einer telemetriebasierten Regel
> * Hinzufügen einer Aktion

Sie haben eine schwellenwertbasierte Regel festgelegt. Als nächster Schritt wird das [Anpassen der Ansicht für Bediener](tutorial-customize-operator.md) empfohlen.

Weitere Informationen zu den verschiedenen Regeltypen in Azure IoT Central sowie zum Parametrisieren der Regeldefinition finden Sie in den folgenden Artikeln:
* [Erstellen einer Telemetrieregel und Einrichten von Benachrichtigungen in Ihrer Azure IoT Central-Anwendung](howto-create-telemetry-rules.md)
* [Erstellen eine Ereignisregel und Einrichten von Benachrichtigungen in Ihrer Azure IoT Central-Anwendung](howto-create-event-rules.md)

<!-- Next tutorials in the sequence -->