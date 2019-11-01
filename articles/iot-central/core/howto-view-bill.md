---
title: Anzeigen Ihrer Rechnung und Umstellen von der Testversion auf die nutzungsbasierte Bezahlung in der Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Administrator Ihre Rechnung anzeigen und in der Azure IoT Central-Anwendung von der Testversion auf die nutzungsbasierte Bezahlung umstellen.
author: v-krghan
ms.author: v-krghan
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 16a58bfc3fa245ed1ede19b0439419ab4590234e
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72942219"
---
# <a name="view-your-bill-in-iot-central-application"></a>Anzeigen Ihrer Rechnung in der IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Administrator Ihre Rechnung in der Azure IoT Central-Anwendung im Verwaltungsbereich anzeigen können. Außerdem erfahren Sie, wie Sie Ihre Testversion auf die nutzungsbasierte Bezahlung umstellen können.

Um auf den Abschnitt **Verwaltung** zugreifen und diesen verwenden zu können, muss Ihnen die Rolle **Administrator** für eine Azure IoT Central-Anwendung zugewiesen sein. Wenn Sie eine Azure IoT Central-Anwendung erstellen, wird Ihnen automatisch die Rolle **Administrator** für die jeweilige Anwendung zugewiesen.

## <a name="view-your-bill"></a>Anzeigen Ihrer Rechnung

Um Ihre Rechnung anzuzeigen, navigieren Sie im Abschnitt **Verwaltung** zur Seite **Abrechnung**. Die Seite mit der Azure-Abrechnung wird auf einer neuen Registerkarte geöffnet. Auf dieser wird für jede Ihrer Azure IoT Central-Anwendung die jeweilige Rechnung angezeigt.

## <a name="convert-your-trial-to-pay-as-you-go"></a>Umstellen der kostenlosen Testversion auf nutzungsbasierte Zahlung

- Anwendungen mit einer **Testversion** können sieben Tage kostenlos genutzt werden, bevor die Version abläuft. Bevor das geschieht, können sie jederzeit auf die nutzungsbasierte Zahlung umgestellt werden.
- Anwendungen mit **nutzungsbasierter Zahlung** werden pro Gerät abgerechnet, wobei die ersten fünf Geräte pro Abonnement kostenlos sind.

Weitere Informationen zu den Preisen finden Sie unter [Azure IoT Central – Preise](https://azure.microsoft.com/pricing/details/iot-central/).

Im Abrechnungsabschnitt können Sie Ihre Testanwendungen auf die nutzungsbasierte Bezahlung umstellen.

Um diesen Self-Service-Prozess durchzuführen, gehen Sie folgendermaßen vor:

1. Navigieren Sie im Abschnitt **Verwaltung** zur Seite **Abrechnung**.

    ![Status der Testversion](media/howto-administer/freetrialbilling.png)

1. Wählen Sie **Auf nutzungsbasierte Zahlung umstellen** aus.

    ![Umwandeln der Testversion](media/howto-administer/convert.png)

1. Wählen Sie die entsprechende Azure Active Directory-Instanz und dann das Azure-Abonnement aus, das für Ihre Anwendung mit nutzungsbasierter Zahlung verwendet werden soll.

1. Wenn Sie **Umwandeln** auswählen, wird Ihre Anwendung auf die nutzungsbasierte Zahlung umgestellt. Von diesem Zeitpunkt an fallen Gebühren an.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über das Anzeigen Ihrer Rechnung in der Azure IoT Central-Anwendung erfahren haben, wird im nächsten Schritt empfohlen, sich mit dem [Anpassen der Anwendungsbenutzeroberfläche](howto-customize-ui.md) in Azure IoT Central vertraut zu machen.