---
title: Verwalten von Azure IoT Hub-Cloudgerätemessaging mit Cloud-Explorer für Visual Studio | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie mit Cloud-Explorer für Visual Studio in Azure IoT Hub Gerät-zu-Cloud-Nachrichten überwachen und Cloud-zu-Gerät-Nachrichten senden.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: xshi
ms.openlocfilehash: b8ea705b55c1485cab2e1478d2d455f1d4a427d3
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70050305"
---
# <a name="use-cloud-explorer-for-visual-studio-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Senden und Empfangen von Nachrichten zwischen Ihrem Gerät und IoT Hub mithilfe von Cloud-Explorer für Visual Studio

![Lückenloses Diagramm](./media/iot-hub-visual-studio-cloud-device-messaging/e-to-e-diagram.png)

[Cloud-Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) ist eine nützliche Erweiterung für Visual Studio, mit der Sie Ihre Azure-Ressourcen anzeigen, deren Eigenschaften überprüfen und wichtige Entwickleraktionen in Visual Studio ausführen können. Der Schwerpunkt dieses Artikels liegt auf dem Verwenden von Cloud-Explorer zum Senden und Empfangen von Nachrichten zwischen Ihrem Gerät und Ihrem Hub.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-learn"></a>Lerninhalt

In diesem Artikel erfahren Sie, wie Sie mit Cloud-Explorer für Visual Studio Gerät-zu-Cloud-Nachrichten (Device to Cloud, D2C) überwachen und Cloud-zu-Gerät-Nachrichten (Cloud to Device, C2D) senden. Gerät-zu-Cloud-Nachrichten können Sensordaten sein, die Ihr Gerät erfasst und dann an Ihren IoT Hub sendet. Cloud-zu-Gerät-Nachrichten können Befehle sein, die Ihr IoT Hub an Ihr Gerät sendet. Zum Beispiel um eine LED blinken zu lassen, die mit Ihrem Gerät verbunden ist.

## <a name="what-you-do"></a>Aufgaben

In diesem Artikel führen Sie die folgenden Aufgaben aus:

- Überwachen Sie Gerät-zu-Cloud-Nachrichten mithilfe von Cloud-Explorer für Visual Studio.

- Überwachen Sie Gerät-zu-Cloud-Nachrichten mithilfe von Cloud-Explorer für Visual Studio.

## <a name="what-you-need"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein:

- Ein aktives Azure-Abonnement.

- Eine Azure IoT Hub-Instanz in Ihrem Abonnement.

- Microsoft Visual Studio 2017 Update 9 oder höher. In diesem Artikel wird [Visual Studio 2019](https://www.visualstudio.com/vs/) verwendet.

- Die Cloud-Explorer-Komponente von Visual Studio-Installer, die standardmäßig zusammen mit Azure-Workload ausgewählt wird.

## <a name="update-cloud-explorer-to-latest-version"></a>Aktualisieren von Cloud-Explorer auf die neueste Version

Die Cloud-Explorer-Komponente von Visual Studio-Installer für Visual Studio 2017 unterstützt nur die Überwachung von Gerät-zu-Cloud- und Cloud-zu-Gerät-Nachrichten. Um Visual Studio 2017 zu verwenden, laden Sie den neuesten [Cloud-Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) herunter, und installieren Sie ihn.

## <a name="sign-in-to-access-your-hub"></a>Anmeldung zum Zugreifen auf Ihren Hub

Führen Sie die folgenden Schritte aus, um auf ihren Hub zuzugreifen:

1. Wählen Sie in Visual Studio **Ansicht** > **Cloud-Explorer** aus, um Cloud-Explorer zu öffnen.

1. Wählen Sie das Symbol „Kontoverwaltung“ aus, um Ihre Abonnements anzuzeigen.

    ![Symbol „Kontoverwaltung“](media/iot-hub-visual-studio-cloud-device-messaging/account-management-icon.png)

1. Wenn Sie bei Azure angemeldet sind, werden Ihre Konten angezeigt. Wenn Sie sich zum ersten Mal bei Azure anmelden möchten, wählen Sie **Konto hinzufügen** aus.

1. Wählen Sie die Azure-Abonnements, die Sie verwenden möchten, und dann **Anwenden** aus.

1. Erweitern Sie Ihr Abonnement und dann **IoT Hubs**.  Unter jedem Hub werden Ihre Geräte dafür angezeigt.

    ![Geräteliste](media/iot-hub-visual-studio-cloud-device-messaging/hub-device-list.png)

## <a name="monitor-device-to-cloud-messages"></a>Überwachen von Gerät-zu-Cloud-Nachrichten

Um Nachrichten zu überwachen, die von Ihrem Gerät an Ihren IoT Hub gesendet werden, gehen Sie folgendermaßen vor:

1. Klicken Sie mit der rechten Maustaste auf Ihren IoT Hub oder Ihr Gerät, und klicken Sie auf **Überwachung von D2C-Nachrichten starten**.

    ![Starten der Überwachung von D2C-Nachrichten](media/iot-hub-visual-studio-cloud-device-messaging/start-monitoring-d2c-message-vs2019.png)

1. Die überwachten Nachrichten werden unter **Ausgabe** angezeigt.

    ![Überwachung von D2C-Nachrichten – Ergebnis](media/iot-hub-visual-studio-cloud-device-messaging/monitor-d2c-message-result-vs2019.png)

1. Um die Überwachung anzuhalten, klicken Sie mit der rechten Maustaste auf einen IoT Hub oder ein Gerät, und wählen Sie **Überwachung von D2C-Nachrichten beenden** aus.

## <a name="send-cloud-to-device-messages"></a>Senden von C2D-Nachrichten.

Um eine Nachricht von Ihrem IoT Hub zu Ihrem Gerät zu senden, gehen Sie folgendermaßen vor:

1. Klicken Sie mit der rechten Maustaste auf Ihr Gerät, und klicken Sie auf **C2D-Nachricht senden**.

1. Geben Sie die Nachricht in das Eingabefeld ein.

    ![Senden einer C2D-Nachricht](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-test.png)

    Die Ergebnisse werden unter **Ausgabe** angezeigt.

    ![Senden einer C2D-Nachricht – Ergebnis](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-result-vs2019.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, Gerät-zu-Cloud-Nachrichten zu überwachen und Cloud-zu-Gerät-Nachrichten zwischen dem IoT-Gerät und Azure IoT Hub zu senden.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]