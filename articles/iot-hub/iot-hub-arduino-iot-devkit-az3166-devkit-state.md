---
title: Steuern der Benutzer-LED des MXChip IoT DevKit mithilfe von Azure-Gerätezwillingen | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie mithilfe von Azure IoT Hub-Gerätezwillingen DevKit-Status überwacht und die Benutzer-LED gesteuert werden.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/04/2018
ms.author: liydu
ms.openlocfilehash: deb1ea8c7b41ad48bddebfbed1b15c667ee0071a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73483953"
---
# <a name="mxchip-iot-devkit"></a>MXChip IoT DevKit

Sie können dieses Beispiel verwenden, um mithilfe von Azure IoT Hub-Gerätezwillingen die WLAN-Informationen und Sensorstatus des MXChip IoT DevKit zu überwachen und die Farbe der Benutzer-LED zu steuern.

## <a name="what-you-learn"></a>Lerninhalt

- Überwachen der Sensorstatus des MXChip IoT DevKit

- Steuern der Farbe der RGB-LED des DevKit mithilfe von Azure-Gerätezwillingen

## <a name="what-you-need"></a>Voraussetzungen

- Richten Sie Ihre Entwicklungsumgebung anhand des [Leitfadens zu den ersten Schritten](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) ein.

- Geben Sie in Ihrem GitBash-Terminalfenster (oder an einer anderen Git-Befehlszeilenschnittstelle) die folgenden Befehle ein:

   ```bash
   git clone https://github.com/DevKitExamples/DevKitState.git
   cd DevKitState
   code .
   ```

## <a name="provision-azure-services"></a>Bereitstellen von Azure-Diensten

1. Klicken Sie in Visual Studio Code auf das Dropdownmenü **Aufgabe**, und wählen Sie **Aufgabe ausführen...**  - **cloud-provision**.

2. Der Fortschritt wird unter der Registerkarte **TERMINAL** des Bereichs **Willkommen** angezeigt.

3. Wenn eine Meldung mit der Frage angezeigt wird, *welches Abonnement Sie wählen möchten*, wählen Sie ein Abonnement aus.

4. Wählen Sie eine Ressourcengruppe aus. 
 
   > [!NOTE]
   > Wenn Sie bereits über eine kostenlose IoT Hub-Instanz verfügen, können Sie diesen Schritt überspringen.

5. Wenn eine Meldung mit der Frage angezeigt wird, *welche IoT Hub-Instanz Sie wählen möchten*, wählen Sie eine IoT Hub-Instanz aus, oder erstellen Sie eine.

6. Die Anzeige sieht etwa wie folgt aus: *Funktions-App: Name der Funktions-App: xxx*. Notieren Sie sich den Namen der Funktions-App; er wird in einem späteren Schritt verwendet.

7. Warten Sie, bis die Bereitstellung der Azure Resource Manager-Vorlage abgeschlossen ist. Dies wird durch die Meldung *Bereitstellung von Resource Manager-Vorlage: Abgeschlossen* angezeigt.

## <a name="deploy-function-app"></a>Bereitstellen der Funktionen-App

1. Klicken Sie in Visual Studio Code auf das Dropdownmenü **Aufgabe**, und wählen Sie **Aufgabe ausführen...**  - **cloud-deploy**.

2. Warten Sie, bis das Hochladen des Funktions-App-Codes abgeschlossen ist. Es wird die Meldung *Funktions-App-Bereitstellungen: Abgeschlossen* angezeigt.

## <a name="configure-iot-hub-device-connection-string-in-devkit"></a>Konfigurieren der Verbindungszeichenfolge für das IoT Hub-Gerät in DevKit

1. Stellen Sie eine Verbindung zwischen dem MXChip IoT DevKit und Ihrem Computer her.

2. Klicken Sie in Visual Studio Code auf das Dropdownmenü **Aufgabe**, und wählen Sie **Aufgabe ausführen...**  - **config-device-connection**.

3. Halten Sie im MXChip IoT DevKit die Taste **A** gedrückt, drücken Sie die **Rücksetztaste**, und lassen Sie dann die Taste **A** los, um den Konfigurationsmodus für das DevKit zu aktivieren.

4. Warten Sie, bis der Konfigurationsprozess für die Verbindungszeichenfolge abgeschlossen ist.

## <a name="upload-arduino-code-to-devkit"></a>Hochladen von Arduino-Code zum DevKit

Führen Sie die folgenden Schritte aus, während Ihr MXChip IoT DevKit mit Ihrem Computer verbunden ist:

1. Klicken Sie in Visual Studio Code auf das Dropdownmenü **Aufgabe**, und wählen Sie **Buildtask ausführen...** . Die Arduino-Skizze wird kompiliert und zum DevKit hochgeladen.

2. Nach dem erfolgreichen Hochladen der Skizze wird die Meldung *Build & Upload Sketch: success* (Skizze erstellen und hochladen: Erfolg) angezeigt.

## <a name="monitor-devkit-state-in-browser"></a>Überwachen des DevKit-Status im Browser

1. Öffnen Sie in einem Webbrowser die Datei `DevKitState\web\index.html`, die im Schritt „Voraussetzungen“ erstellt wurde.

2. Die folgende Webseite wird angezeigt:![Geben Sie den Namen der Funktions-App an.](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state-function-app-name.png)

3. Geben Sie Funktions-App-Namen ein, den Sie zuvor notiert haben.

4. Klicken Sie auf die Schaltfläche **Verbinden**.

5. Innerhalb weniger Sekunden wird die Seite aktualisiert und zeigt den WLAN-Verbindungsstatus des DevKit und den Status aller integrierten Sensoren an.

## <a name="control-the-devkits-user-led"></a>Steuern der Benutzer-LED des DevKit

1. Klicken Sie auf der Abbildung der Webseite auf die Grafik der Benutzer-LED.

2. Innerhalb weniger Sekunden wird der Bildschirm aktualisiert und zeigt den aktuellen Farbstatus der Benutzer-LED an.

3. Klicken Sie auf den RGB-Schiebereglern an verschiedenen Stellen, um den Farbwert der RGB-LED zu ändern.

## <a name="example-operation"></a>Beispielvorgang

![Beispieltestprozedur](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state.gif)

> [!NOTE]
> Sie können die unformatierten Daten des Gerätezwillings im Azure-Portal anzeigen: IoT Hub –\> IoT-Geräte –\> *\<Ihr Gerät\>*  -\> Gerätezwilling.

## <a name="next-steps"></a>Nächste Schritte

Folgendes wurde vermittelt:
- Herstellen der Verbindung des MXChip IoT DevKit-Geräts mit dem Solution Accelerator für die Azure IoT-Remoteüberwachung.
- Verwenden der Gerätezwillingsfunktion von Azure IoT, um die Farbe der RGB-LED des DevKit zu erkennen und zu steuern

Hier sind die empfohlenen nächsten Schritte aufgeführt:

* [Solution Accelerator für die Azure IoT-Remoteüberwachung: Übersicht](https://docs.microsoft.com/azure/iot-suite/)
* [Herstellen einer Verbindung zwischen einem MXChip IoT DevKit-Gerät und Ihrer Azure IoT Central-Anwendung](/azure/iot-central/core/howto-connect-devkit)
