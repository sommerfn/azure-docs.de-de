---
title: Registrieren eines neuen Azure IoT Edge-Geräts | Microsoft-Dokumentation
description: Verwenden der IoT-Erweiterung für die Azure CLI zum Registrieren eines neuen IoT Edge-Geräts und Abrufen der Verbindungszeichenfolge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/21/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 65fb55a16e358624dceb42a268b035f826576f63
ms.sourcegitcommit: c4700ac4ddbb0ecc2f10a6119a4631b13c6f946a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2019
ms.locfileid: "72966069"
---
# <a name="register-an-azure-iot-edge-device"></a>Registrieren eines neuen Azure IoT Edge-Geräts

Bevor Sie Ihre IoT-Geräte mit Azure IoT Edge verwenden können, müssen Sie sie bei Ihrem IoT Hub registrieren. Nachdem ein Gerät registriert wurde, können Sie eine Verbindungszeichenfolge abrufen, um es für IoT Edge-Workloads einzurichten.

Sie haben die Möglichkeit, sich mit einem der folgenden Tools zu registrieren:

* [Azure-Portal](https://portal.azure.com) bietet eine grafische Benutzeroberfläche zum Erstellen, Anzeigen und Verwalten von Azure-Ressourcen.
* [Visual Studio Code](https://code.visualstudio.com/) ist ein Quellcode-Editor. Azure IoT-Erweiterungen erleichtern die Verwaltung von IoT-Ressourcen über dasselbe Tool, in dem Sie IoT-Lösungen entwickeln.
* [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) ist ein Befehlszeilentool zum Verwalten von Azure-Ressourcen. Seine wiederverwendbaren Befehle sind hilfreich für die Automatisierung von Aufgaben.

## <a name="register-in-the-azure-portal"></a>Registrieren im Azure-Portal

Sie können alle Registrierungsaufgaben im Azure-Portal ausführen.

### <a name="prerequisites-for-the-azure-portal"></a>Voraussetzungen für das Azure-Portal

Ein [IoT-Hub](../iot-hub/iot-hub-create-through-portal.md) in Ihrem Azure-Abonnement im Tarif „Free“ oder „Standard“.

### <a name="create-an-iot-edge-device-in-the-azure-portal"></a>Erstellen eines IoT Edge-Geräts im Azure-Portal

In Ihrem IoT Hub im Azure-Portal werden IoT Edge-Geräte separat von IoT-Geräten erstellt und verwaltet, die nicht mit Edge verwendet werden können.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem IoT Hub.
2. Wählen Sie im linken Bereich aus dem Menü die Option **IoT Edge** aus.
3. Wählen Sie **IoT Edge-Gerät hinzufügen** aus.
4. Geben Sie eine beschreibende Geräte-ID an. Verwenden Sie die Standardeinstellungen, um Authentifizierungsschlüssel automatisch zu generieren und das neue Gerät mit Ihrem Hub zu verbinden.
5. Wählen Sie **Speichern** aus.

### <a name="view-iot-edge-devices-in-the-azure-portal"></a>Anzeigen von IoT Edge-Geräten im Azure-Portal

Alle auf die Verwendung mit Edge ausgelegten Geräte, die eine Verbindung mit Ihrem IoT-Hub herstellen, sind auf der Seite **IoT Edge** aufgeführt.

![Anzeigen aller IoT Edge-Geräte in Ihrem IoT-Hub](./media/how-to-register-device/portal-view-devices.png)

### <a name="retrieve-the-connection-string-in-the-azure-portal"></a>Abrufen der Verbindungszeichenfolge im Azure-Portal

Wenn Sie bereit für die Einrichtung Ihres Geräts sind, benötigen Sie die Verbindungszeichenfolge, die Ihr physisches Gerät mit seiner Identität in IoT Hub verknüpft.

1. Klicken Sie im Portal auf der Seite **IoT Edge** in der Liste der IoT Edge-Geräte auf die Geräte-ID.
2. Kopieren Sie den Wert **Verbindungszeichenfolge (primärer Schlüssel)** oder **Verbindungszeichenfolge (sekundärer Schlüssel)** .

## <a name="register-with-visual-studio-code"></a>Registrieren bei Visual Studio Code

Es gibt mehrere Möglichkeiten zum Durchführen der meisten Vorgänge in Visual Studio Code. In diesem Artikel wird der Explorer verwendet, Sie können aber auch die Befehlspalette verwenden, um die Schritte auszuführen.

### <a name="prerequisites-for-visual-studio-code"></a>Voraussetzungen für Visual Studio Code

* Ein [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) in Ihrem Azure-Abonnement
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure IoT-Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) für Visual Studio Code

### <a name="sign-in-to-access-your-iot-hub"></a>Anmeldung zum Zugreifen auf Ihren IoT Hub

Sie können die Azure IoT-Erweiterungen für Visual Studio Code verwenden, um Vorgänge mit Ihrem IoT Hub auszuführen. Damit diese Vorgänge funktionieren, müssen Sie sich bei Ihrem Azure-Konto anmelden und Ihren IoT Hub auswählen.

1. Öffnen Sie in Visual Studio Code die **Explorer**-Ansicht.
1. Erweitern Sie im unteren Bereich des Explorers den Abschnitt **Azure IoT Hub**.

   ![Erweitern des Abschnitts „Azure IoT Hub Devices“ (Azure IoT Hub-Geräte)](./media/how-to-register-device/azure-iot-hub-devices.png)

1. Klicken Sie auf **...** in der Kopfzeile des Abschnitts **Azure IoT Hub**. Wenn die Ellipse nicht angezeigt wird, zeigen Sie auf den Header, oder klicken Sie darauf.
1. Wählen Sie **Select IoT Hub** (IoT Hub auswählen) aus.
1. Wenn Sie nicht bei Ihrem Azure-Konto angemeldet sind, befolgen Sie die angezeigten Aufforderungen.
1. Wählen Sie Ihr Azure-Abonnement.
1. Wählen Sie Ihren IoT Hub aus.

### <a name="create-an-iot-edge-device-with-visual-studio-code"></a>Erstellen eines IoT Edge-Geräts mit Visual Studio Code

1. Erweitern Sie im Visual Studio Code-Explorer den Abschnitt **Azure IoT Hub Devices** (Azure IoT Hub-Geräte).
1. Klicken Sie auf **...** in der Überschrift des Abschnitts **Azure IoT Hub Devices** (Azure IoT Hub-Geräte). Wenn die Ellipse nicht angezeigt wird, zeigen Sie auf den Header, oder klicken Sie darauf.
1. Wählen Sie **Create IoT Edge Device** (IoT Edge-Gerät erstellen) aus.
1. Geben Sie im geöffneten Textfeld eine ID für Ihr Gerät an.

Auf dem Ausgabebildschirm wird das Ergebnis des Befehls angezeigt. Die Geräteinformationen werden ausgegeben, einschließlich der **deviceID**, die Sie angegeben haben, und der Zeichenfolge **connectionString**, mit der Sie Ihr physisches Gerät mit Ihrem IoT Hub verbinden können.

Auf dem Ausgabebildschirm wird das Ergebnis des Befehls angezeigt. Die Geräteinformationen werden ausgegeben, einschließlich der **deviceID**, die Sie angegeben haben, und der Zeichenfolge **connectionString**, mit der Sie Ihr physisches Gerät mit Ihrem IoT Hub verbinden können.

### <a name="view-iot-edge-devices-with-visual-studio-code"></a>Anzeigen von IoT Edge-Geräten mit Visual Studio Code

Alle Geräte, die Verbindungen mit Ihrem IoT Hub herstellen, werden im Abschnitt **Azure IoT Hub** des Visual Studio Code-Explorers aufgeführt. IoT Edge-Geräte können von Nicht-Edge-Geräten unterschieden werden, da sie ein anderes Symbol aufweisen, und aufgrund der Tatsache, dass die Module **$edgeAgent** und **$edgeHub** auf jedem IoT Edge-Gerät bereitgestellt sind.

![Anzeigen aller IoT Edge-Geräte in Ihrem IoT-Hub](./media/how-to-register-device/view-devices.png)

### <a name="retrieve-the-connection-string-with-visual-studio-code"></a>Abrufen der Verbindungszeichenfolge mit Visual Studio Code

Wenn Sie bereit für die Einrichtung Ihres Geräts sind, benötigen Sie die Verbindungszeichenfolge, die Ihr physisches Gerät mit seiner Identität in IoT Hub verknüpft.

1. Klicken Sie mit der rechten Maustaste auf die ID des Geräts im Abschnitt **Azure IoT Hub**.
1. Wählen Sie **Copy Device Connection String** (Verbindungszeichenfolge des Geräts kopieren) aus.

   Die Verbindungszeichenfolge wird in die Zwischenablage kopiert.

Sie können auch im Kontextmenü **Get Device Info** (Geräteinformationen abrufen) auswählen, um alle Geräteinformationen einschließlich der Verbindungszeichenfolge im Ausgabefenster anzuzeigen.

## <a name="register-with-the-azure-cli"></a>Registrieren bei der Azure-Befehlszeilenschnittstelle (CLI)

Die [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) ist ein plattformübergreifendes Open-Source-Befehlszeilentool zum Verwalten von Azure-Ressourcen wie IoT Edge. Mit ihm können Sie Azure IoT Hub-Ressourcen, Instanzen des Device Provisioning Service und verknüpfte Hubs direkt ohne weitere Tools verwalten. Mit der neuen IoT-Erweiterung wird die Azure-Befehlszeilenschnittstelle um Features wie die Geräteverwaltung und um umfassende IoT Edge-Funktionen erweitert.

### <a name="prerequisites-for-the-azure-cli"></a>Voraussetzungen für die Azure-Befehlszeilenschnittstelle

* Ein [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) in Ihrem Azure-Abonnement.
* Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ist in Ihrer Umgebung vorhanden. Ihre Azure CLI-Version muss mindestens 2.0.24 lauten. Verwenden Sie `az --version`, um dies zu überprüfen. Diese Version unterstützt az-Erweiterungsbefehle, und das Framework für Knack-Befehle wird eingeführt.
* Die [IoT-Erweiterung für die Azure CLI](https://github.com/Azure/azure-iot-cli-extension) ist vorhanden.

### <a name="create-an-iot-edge-device-with-the-azure-cli"></a>Erstellen eines IoT Edge-Geräts mit der Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az iot hub device-identity create](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create), um eine neue Geräteidentität in Ihrem IoT-Hub zu erstellen. Beispiel:

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Dieser Befehl weist drei Parameter auf:

* **device-id**: Geben Sie einen beschreibenden Namen an, der für Ihren IoT Hub eindeutig ist.
* **hub-name**: Geben Sie den Namen Ihres IoT Hub an.
* **edge-enabled**: Dieser Parameter deklariert, dass das Gerät für die Verwendung mit IoT Edge ausgelegt ist.

   ![az iot hub device-identity create output](./media/how-to-register-device/Create-edge-device.png)

### <a name="view-iot-edge-devices-with-the-azure-cli"></a>Anzeigen von IoT Edge-Geräten mit der Azure-Befehlszeilenschnittstelle

Verwenden Sie den Befehl [az iot hub device-identity list](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-list), um alle Geräte in Ihrem IoT-Hub anzuzeigen. Beispiel:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Alle als IoT Edge-Gerät registrierten Geräte weisen die **capabilities.iotEdge**-Eigenschaft mit dem Wert **true** auf.

### <a name="retrieve-the-connection-string-with-the-azure-cli"></a>Abrufen der Verbindungszeichenfolge mit der Azure-Befehlszeilenschnittstelle

Wenn Sie bereit für die Einrichtung Ihres Geräts sind, benötigen Sie die Verbindungszeichenfolge, die Ihr physisches Gerät mit seiner Identität in IoT Hub verknüpft. Verwenden Sie den Befehl [az iot hub device-identity show-connection-string](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-show-connection-string), um die Verbindungszeichenfolge für ein einzelnes Gerät zurückzugeben:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name]
   ```

Beim Wert für den Parameter `device-id` muss die Groß-/Kleinschreibung beachtet werden. Kopieren Sie nicht die Anführungszeichen, in die die Verbindungszeichenfolge eingeschlossen ist.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie eine Geräteidentität in Ihrem IoT Hub registriert haben, können Sie jetzt die IoT Edge-Runtime auf Ihren Geräten installieren. Installieren Sie die Runtime entsprechend dem Betriebssystem des Geräts:

* [Installieren der Azure IoT Edge-Runtime unter Windows](how-to-install-iot-edge-windows.md)
* [Installieren der Azure IoT Edge-Runtime unter Linux](how-to-install-iot-edge-linux.md)
