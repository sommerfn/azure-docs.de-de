---
title: Azure IoT-Geräteverwaltung mit dem Cloud-Explorer für Visual Studio | Microsoft-Dokumentation
description: Verwenden Sie den Cloud Explorer für Visual Studio für die Geräteverwaltung mit Azure IoT Hub, indem direkte Methoden und gewünschte Eigenschaften von Gerätezwillingen verwendet werden.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: xshi
ms.openlocfilehash: e05ba421a4535e6e424e65a1f2271d19f9d9abf4
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70048696"
---
# <a name="use-cloud-explorer-for-visual-studio-for-azure-iot-hub-device-management"></a>Verwenden des Cloud-Explorers für Visual Studio für die Geräteverwaltung mit Azure IoT Hub

![Lückenloses Diagramm](media/iot-hub-device-management-visual-studio/iot-e2e-simple.png)

[Cloud-Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) ist eine nützliche Erweiterung für Visual Studio, mit der Sie Ihre Azure-Ressourcen anzeigen, deren Eigenschaften überprüfen und wichtige Entwickleraktionen in Visual Studio ausführen können. Es bietet Verwaltungsoptionen, die Sie zum Ausführen verschiedener Aufgaben verwenden können.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Verwaltungsoption          | Aufgabe                    |
|----------------------------|--------------------------------|
| Direkte Methoden             | Lassen Sie ein Gerät beispielsweise mit dem Senden von Nachrichten beginnen oder dies beenden, oder starten Sie es neu.                                        |
| Lesen des Gerätezwillings           | Rufen Sie den berichteten Status eines Geräts ab. Das Gerät meldet z.B., das die LED jetzt blinkt.                                    |
| Aktualisieren des Gerätezwillings         | Setzen Sie ein Gerät in bestimmte Status, stellen Sie z.B. das Leuchten einer grünen LED ein, oder legen Sie das Telemetriesendeintervall auf 30 Minuten fest.         |
| C2D-Nachrichten   | Senden Sie Benachrichtigungen an ein Gerät. Beispiel: „Heute wird es sehr wahrscheinlich regnen. Vergessen Sie Ihren Schirm nicht.“              |

Eine ausführlichere Erläuterung zu den Unterschieden und Anweisungen zur Verwendung dieser Optionen finden Sie im [Leitfaden zur D2C-Kommunikation](iot-hub-devguide-d2c-guidance.md) und [Leitfaden zur C2D-Kommunikation](iot-hub-devguide-c2d-guidance.md).

Gerätezwillinge sind JSON-Dokumente, in denen Gerätestatusinformationen gespeichert werden, einschließlich Metadaten, Konfigurationen und Bedingungen. Von IoT Hub wird für jedes Gerät, das eine Verbindung herstellt, dauerhaft ein Gerätezwilling gespeichert. Weitere Informationen zu Gerätezwillingen finden Sie unter [Erste Schritte mit Gerätezwillingen (Node)](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Lerninhalt

In diesem Artikel erfahren Sie, wie Sie den Cloud-Explorer für Visual Studio mit unterschiedlichen Verwaltungsoptionen auf Ihrem Entwicklungscomputer verwenden.

## <a name="what-you-do"></a>Aufgaben

Führen Sie in diesem Artikel den Cloud-Explorer für Visual Studio mit verschiedenen Verwaltungsoptionen aus.

## <a name="what-you-need"></a>Voraussetzungen

Die folgenden Voraussetzungen müssen erfüllt sein:

- Ein aktives Azure-Abonnement.

- Eine Azure IoT Hub-Instanz in Ihrem Abonnement.

- Microsoft Visual Studio 2017 Update 9 oder höher. In diesem Artikel [wird Visual Studio 2017 oder Visual Studio 2019](https://www.visualstudio.com/vs/) verwendet.

- Cloud-Explorer-Komponente von Visual Studio-Installer, die standardmäßig zusammen mit Azure-Workload ausgewählt wird.

## <a name="update-cloud-explorer-to-latest-version"></a>Aktualisieren von Cloud-Explorer auf die neueste Version

Die Cloud-Explorer-Komponente von Visual Studio-Installer für Visual Studio 2017 unterstützt nur die Überwachung von Gerät-zu-Cloud- und Cloud-zu-Gerät-Nachrichten. Um Visual Studio 2017 zu verwenden, laden Sie den neuesten [Cloud-Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) herunter, und installieren Sie ihn.

## <a name="sign-in-to-access-your-hub"></a>Anmeldung zum Zugreifen auf Ihren Hub

1. Wählen Sie in Visual Studio **Ansicht** > **Cloud-Explorer** aus, um Cloud-Explorer zu öffnen.

1. Wählen Sie das Symbol „Kontoverwaltung“ aus, um Ihre Abonnements anzuzeigen.

    ![Symbol „Kontoverwaltung“](media/iot-hub-visual-studio-cloud-device-messaging/account-management-icon.png)

1. Wenn Sie bei Azure angemeldet sind, werden Ihre Konten angezeigt. Wenn Sie sich zum ersten Mal bei Azure anmelden möchten, wählen Sie **Konto hinzufügen** aus.

1. Wählen Sie die Azure-Abonnements, die Sie verwenden möchten, und dann **Anwenden** aus.

1. Erweitern Sie Ihr Abonnement und dann **IoT Hubs**.  Unter jedem Hub werden Ihre Geräte dafür angezeigt. Klicken Sie mit der rechten Maustaste auf ein Gerät, um auf die Verwaltungsoptionen zuzugreifen.

    ![Verwaltungsoptionen](media/iot-hub-device-management-visual-studio/management-options-vs2019.png)

## <a name="direct-methods"></a>Direkte Methoden

Führen Sie die folgenden Schritte aus, um direkte Methoden zu verwenden:

1. Klicken Sie mit der rechten Maustaste auf Ihr Gerät, und wählen Sie **Direkte Gerätemethode aufrufen** aus.

1. Geben Sie in **Direkte Methode aufrufen** den Methodennamen und die Nutzlast ein, und wählen Sie **OK** aus.

    Die Ergebnisse werden in **Ausgabe** angezeigt.

## <a name="update-device-twin"></a>Aktualisieren des Gerätezwillings

Führen Sie die folgenden Schritte aus, um einen Gerätezwilling zu bearbeiten:

1. Klicken Sie mit der rechten Maustaste auf Ihr Gerät, und wählen Sie **Gerätezwilling bearbeiten**.

   Die Datei **azure-iot-device-twin.json** mit dem Inhalt des Gerätezwillings wird geöffnet.

1. Bearbeiten Sie die **tags**- oder **properties.desired**-Felder der **azure-iot-device-twin.json**-Datei.

1. Drücken Sie **STRG+S**, um den Gerätezwilling zu aktualisieren.

   Die Ergebnisse werden in **Ausgabe** angezeigt.

## <a name="send-cloud-to-device-messages"></a>Senden von C2D-Nachrichten.

Um eine Nachricht von Ihrem IoT Hub zu Ihrem Gerät zu senden, gehen Sie folgendermaßen vor:

1. Klicken Sie mit der rechten Maustaste auf Ihr Gerät, und klicken Sie auf **C2D-Nachricht senden**.

1. Geben Sie die Nachricht in **C2D-Nachricht senden** ein, und wählen Sie **OK** aus.

   Die Ergebnisse werden in **Ausgabe** angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun gelernt, den Cloud-Explorer für Visual Studio mit verschiedenen Verwaltungsoptionen auszuführen.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
