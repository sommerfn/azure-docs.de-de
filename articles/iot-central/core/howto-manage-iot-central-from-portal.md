---
title: Verwalten von IoT Central über das Azure-Portal | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Ihre IoT Central-Anwendungen über das Azure-Portal erstellen und verwalten.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 10/02/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 59fc01920a94e8ab3e7037e4bb226ea18a4314bf
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73806505"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Verwalten von IoT Central über das Azure-Portal

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

Statt IoT Central-Anwendungen auf der Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) zu erstellen und zu verwalten, können Sie Ihre Anwendungen auch über das [Azure-Portal](https://portal.azure.com) verwalten.

## <a name="create-iot-central-applications"></a>Erstellen von IoT Central-Anwendungen

Zum Erstellen einer Anwendung navigieren Sie zum [Azure-Portal](https://ms.portal.azure.com), und wählen Sie im Hauptbereich auf der linken Seite **Ressource erstellen** aus.

![Verwaltungsportal: Navigationsmenü](media/howto-manage-iot-central-from-portal/image0.png)

Geben Sie in der Suchleiste den Begriff **IoT Central** ein.

![Verwaltungsportal: Suchen](media/howto-manage-iot-central-from-portal/image0a1.png)

Wählen Sie in den Suchergebnissen die Position **IoT Central-Anwendung** aus.

![Verwaltungsportal: Suchergebnisse](media/howto-manage-iot-central-from-portal/image0b1.png)

Wählen Sie jetzt **Erstellen**.

![Verwaltungsportal: IoT Central-Ressource](media/howto-manage-iot-central-from-portal/image0c1.png)

Füllen Sie alle Felder im Formular aus. Dieses Formular ähnelt dem Formular, das Sie ausfüllen müssen, um Anwendungen auf der Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) zu erstellen. Weitere Informationen finden Sie unter dem Schnellstart [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md).

**Ort** ist der physische Standort oder die [geografische Region](https://azure.microsoft.com/global-infrastructure/geographies/), an dem bzw. in der Sie Ihre Anwendung erstellen möchten. Normalerweise wählen Sie den Standort aus, der Ihren Geräten physisch am nächsten liegt, um eine optimale Leistung zu erzielen. Die Regionen, in denen Azure IoT Central verfügbar ist, finden Sie auf der Seite [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=iot-central). Nachdem Sie einen Standort ausgewählt haben, können Sie Ihre Anwendung nicht zu einem späteren Zeitpunkt an einen anderen Standort verschieben.

> [!NOTE]
> Die Vorlage **Vorschauanwendung** ist derzeit nur in den Regionen **Europa, Norden** und **USA, Mitte** verfügbar.

![Verwaltungsportal: IoT Central-Ressource erstellen](media/howto-manage-iot-central-from-portal/image1a.png)  

Wählen Sie **Erstellen** aus, wenn Sie alle Felder ausgefüllt haben.

## <a name="manage-existing-iot-central-applications"></a>Verwalten vorhandener IoT Central-Anwendungen

Wenn Sie bereits über eine Azure IoT Central-Anwendung verfügen, können Sie sie löschen oder aber im Azure-Portal in ein anderes Abonnement oder eine andere Ressourcengruppe verschieben.

> [!NOTE]
> Testversionsanwendungen werden im Azure-Portal nicht angezeigt, da sie nicht mit Ihrem Abonnement verknüpft sind.

Wählen Sie zuerst im Hauptbereich auf der linken Seite **Alle Ressourcen** aus. Geben Sie in das Suchfeld den Namen Ihrer Anwendung ein, um sie in Ihrer Liste von Ressourcen zu finden. Wählen Sie dann die IoT Central-Anwendung aus, die Sie verwalten möchten.

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