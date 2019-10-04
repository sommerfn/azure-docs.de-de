---
title: Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Operator Gerätegruppen in Ihrer Azure IoT Central-Anwendung verwenden.
author: ellenfosborne
ms.author: elfarber
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpfr
ms.openlocfilehash: 07f1df101442e8ae43b26cebc60c8452d475d0f3
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878982"
---
# <a name="use-device-groups-in-your-azure-iot-central-application-preview-features"></a>Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

In diesem Artikel wird beschrieben, wie Sie als Operator Gerätegruppen in Ihrer Azure IoT Central-Anwendung verwenden.

Eine Gerätegruppe ist eine Liste der Geräte, die gruppiert werden, weil sie einigen der angegebenen Kriterien entsprechen. Gerätegruppen unterstützen Sie bei der Verwaltung, Visualisierung und Analyse aller im Umfang enthaltenen Geräte, indem Geräte in kleineren, logischen Gruppen gruppiert werden. Sie können beispielsweise eine Gerätegruppe zum Auflisten aller Klimageräte in Seattle erstellen, damit ein Techniker alle Geräte finden kann, für die er zuständig ist. In diesem Artikel erfahren Sie, wie Sie Gerätegruppen erstellen und konfigurieren.

## <a name="create-a-device-group"></a>Erstellen einer Gerätegruppe

Gehen Sie wie folgt vor, um eine Gerätegruppe zu erstellen:

1. Wählen Sie im linken Navigationsmenü die Option **Gerätegruppen** aus.

1. Wählen Sie **+ Neu** aus.

    ![Neue Gerätegruppe](media/howto-use-device-groups-pnp/image1.png)

1. Weisen Sie Ihrer Gerätegruppe einen Namen zu, der in der gesamten Anwendung eindeutig ist. Sie können auch eine Beschreibung hinzufügen. Eine Gerätegruppe kann nur Geräte aus einer einzigen Gerätevorlage enthalten. Wählen Sie die Gerätevorlage aus, die für diese Gruppe verwendet werden soll.

1. Erstellen Sie die Abfrage, um die Geräte für die Gerätegruppe zu identifizieren, indem Sie eine Eigenschaft, einen Vergleichsoperator und einen Wert auswählen. Sie können mehrere Abfragen hinzufügen, und Geräte, die **alle** Kriterien erfüllen, werden in die Gerätegruppe aufgenommen. Die von Ihnen erstellte Gerätegruppe ist für jeden zugänglich, der Zugriff auf die Anwendung hat, sodass jeder die Gerätegruppe anzeigen, ändern oder löschen kann.

    ![Gerätegruppenabfrage](media/howto-use-device-groups-pnp/image2.png)

    > [!NOTE]
    > Die Gerätegruppe ist eine dynamische Abfrage. Jedes Mal, wenn Sie die Liste der Geräte anzeigen, werden möglicherweise andere Geräte in der Liste angezeigt. Die Liste hängt davon ab, welche Geräte derzeit die Kriterien der Abfrage erfüllen.

1. Wählen Sie **Speichern** aus.

## <a name="analytics"></a>Analytics

Die Analyse in Gerätegruppen entspricht der Hauptregisterkarte für die Analyse im linken Navigationsmenü. Mehr über Analysen erfahren Sie im Artikel [Erstellen von Analysen](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Gerätegruppen in Ihrer Azure IoT Central-Anwendung verwenden, wird als Nächstes der folgende Schritt empfohlen:

> [!div class="nextstepaction"]
> [Erstellen von Telemetrieregeln](howto-create-telemetry-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
