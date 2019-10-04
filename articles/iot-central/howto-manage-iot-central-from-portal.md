---
title: Verwalten von IoT Central über das Azure-Portal | Microsoft-Dokumentation
description: Verwalten Sie IoT Central über das Azure-Portal.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 07/11/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: d4d2a843248985c4fe17bf5401b261862d71fecf
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383039"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Verwalten von IoT Central über das Azure-Portal

[!INCLUDE [iot-central-selector-manage](../../includes/iot-central-selector-manage.md)]

Statt IoT Central-Anwendungen auf der Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) zu erstellen und zu verwalten, können Sie Ihre Anwendungen auch über das [Azure-Portal](https://portal.azure.com) verwalten.

## <a name="create-iot-central-applications"></a>Erstellen von IoT Central-Anwendungen

Um eine Anwendung zu erstellen, navigieren Sie zum [Azure-Portal](https://ms.portal.azure.com), und wählen Sie im Hauptnavigationsmenü auf der linken Seite **Ressource erstellen** aus.

![Verwaltungsportal: Navigationsmenü](media/howto-manage-iot-central-from-portal/image0.png)

Geben Sie in der Suchleiste den Begriff **IoT Central** ein.

![Verwaltungsportal: Suchen](media/howto-manage-iot-central-from-portal/image0a1.png)

Wählen Sie in den Suchergebnissen die Position **IoT Central-Anwendung** aus.

![Verwaltungsportal: Suchergebnisse](media/howto-manage-iot-central-from-portal/image0b1.png)

Wählen Sie jetzt **Erstellen**.

![Verwaltungsportal: IoT Central-Ressource](media/howto-manage-iot-central-from-portal/image0c1.png)

Füllen Sie alle Felder im Formular aus. Dieses Formular ähnelt dem Formular, das Sie ausfüllen müssen, um Anwendungen auf der Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) zu erstellen. Weitere Informationen finden Sie unter dem Schnellstart [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md).

> [!NOTE]
> Die Vorlage **Vorschauanwendung** ist derzeit nur in den Regionen **Europa, Norden** und **USA, Mitte** verfügbar.

![Verwaltungsportal: IoT Central-Ressource erstellen](media/howto-manage-iot-central-from-portal/image1a.png)  

Wählen Sie **Erstellen** aus, wenn Sie alle Felder ausgefüllt haben.

## <a name="manage-existing-iot-central-applications"></a>Verwalten vorhandener IoT Central-Anwendungen

Wenn Sie bereits über eine Azure IoT Central-Anwendung verfügen, können Sie sie löschen oder aber im Azure-Portal in ein anderes Abonnement oder eine andere Ressourcengruppe verschieben.

> [!NOTE]
> Testversionsanwendungen werden im Azure-Portal nicht angezeigt, da sie nicht mit Ihrem Abonnement verknüpft sind.

Wählen Sie zuerst im Hauptnavigationsmenü auf der linken Seite **Alle Ressourcen** aus. Geben Sie in das Suchfeld den Namen Ihrer Anwendung ein, um sie in Ihrer Liste von Ressourcen zu finden. Wählen Sie dann die IoT Central-Anwendung aus, die Sie verwalten möchten.

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image2a.png)

Wählen Sie die URL der IoT Central-Anwendung aus, um zur Anwendung zu navigieren.

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image3.png)

Um die Anwendung in eine andere Ressourcengruppe zu verschieben, wählen Sie neben der Ressourcengruppe **Ändern** aus. Wählen Sie auf der Seite **Ressourcen verschieben** die Ressourcengruppe aus, in die Sie diese Anwendung migrieren möchten.

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image4a.png)

Um die Anwendung in ein anderes Abonnement zu verschieben, wählen Sie neben dem Abonnement den Link **Ändern** aus. Wählen Sie in dem daraufhin angezeigten Dialogfeld das Abonnement aus, in das Sie diese Anwendung migrieren möchten.

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Azure IoT Central-Anwendungen über das Azure-Portal verwalten, wird der folgende nächste Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten Ihrer Anwendung](howto-administer.md)