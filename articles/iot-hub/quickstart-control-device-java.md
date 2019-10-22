---
title: 'Schnellstart: Steuern eines Geräts über Azure IoT Hub mit Java'
description: In dieser Schnellstartanleitung führen Sie zwei Java-Beispielanwendungen aus. Eine dieser Anwendungen ist eine Back-End-Anwendung, die mit Ihrem Hub verbundene Geräte remote steuern kann. Die andere Anwendung simuliert ein Gerät, das mit Ihrem Hub verbunden ist und remote gesteuert werden kann.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: quickstart
ms.custom: mvc, seo-java-august2019, seo-java-september2019
ms.date: 06/21/2019
ms.openlocfilehash: eee8a3b17a23d34610951db8b881397a0649b53a
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72516734"
---
# <a name="quickstart-control-a-device-connected-to-an-azure-iot-hub-with-java"></a>Schnellstart: Steuern eines mit einer Azure IoT Hub-Instanz verbundenen Geräts mit Java

[!INCLUDE [iot-hub-quickstarts-2-selector](../../includes/iot-hub-quickstarts-2-selector.md)]

IoT Hub ist ein Azure-Dienst, mit dem Sie Ihre IoT-Geräte über die Cloud verwalten und große Mengen von Gerätetelemetriedaten zum Speichern oder Verarbeiten in der Cloud erfassen können. In dieser Schnellstartanleitung verwenden Sie eine *direkte Methode*, um mit einer Java-Anwendung ein simuliertes Gerät zu steuern, das mit Ihrer Azure IoT Hub-Instanz verbunden ist. Sie können direkte Methoden verwenden, um das Verhalten eines mit Ihrer IoT Hub-Instanz verbundenen Geräts zu ändern. 

In dieser Schnellstartanleitung werden zwei vorab geschriebene Java-Anwendungen verwendet:

* Eine simulierte Geräteanwendung, die auf direkte Methoden reagiert, die von einer Back-End-Anwendung aufgerufen werden. Um die Aufrufe der direkten Methode zu empfangen, stellt diese Anwendung eine Verbindung mit einem gerätespezifischen Endpunkt in Ihrer IoT Hub-Instanz her.

* Eine Back-End-Anwendung, die die direkten Methoden auf dem simulierten Gerät aufruft. Um eine direkte Methode auf einem Gerät aufzurufen, stellt diese Anwendung eine Verbindung mit einem dienstseitigen Endpunkt in Ihrer IoT Hub-Instanz her.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Die beiden in dieser Schnellstartanleitung ausgeführten Beispielanwendungen sind in Java geschrieben. Sie benötigen auf Ihrem Entwicklungscomputer Java SE 8.

Sie können Java SE Development Kit 8 für mehrere Plattformen unter [Langfristiger Java-Support für Azure und Azure Stack](https://docs.microsoft.com/en-us/java/azure/jdk/?view=azure-java-stable) herunterladen. Wählen Sie unter **Langfristiger Support** unbedingt **Java 8** aus, um zu den Downloads für JDK 8 zu gelangen.

Mit dem folgenden Befehl können Sie die aktuelle Java-Version auf Ihrem Entwicklungscomputer überprüfen:

```cmd/sh
java -version
```

Für die Erstellung der Beispiele müssen Sie Maven 3 installieren. Sie können Maven für mehrere Plattformen von [Apache Maven](https://maven.apache.org/download.cgi) herunterladen.

Mit dem folgenden Befehl können Sie die aktuelle Maven-Version auf Ihrem Entwicklungscomputer überprüfen:

```cmd/sh
mvn --version
```

Führen Sie den folgenden Befehl aus, um Ihrer Cloud Shell-Instanz die Microsoft Azure IoT-Erweiterung für die Azure-Befehlszeilenschnittstelle hinzuzufügen. Die IoT-Erweiterung fügt der Azure-Befehlszeilenschnittstelle spezifische Befehle für IoT Hub, IoT Edge und IoT Device Provisioning Service (DPS) hinzu.

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Laden Sie das Java-Beispielprojekt von https://github.com/Azure-Samples/azure-iot-samples-java/archive/master.zip herunter, und extrahieren Sie das ZIP-Archiv (falls nicht bereits erfolgt).

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs

Wenn Sie das vorherige Tutorial [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (Node.js)](quickstart-send-telemetry-java.md) absolviert haben, können Sie diesen Schritt überspringen.

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-device"></a>Registrieren eines Geräts

Wenn Sie das vorherige Tutorial [Schnellstart: Senden von Telemetriedaten von einem Gerät an eine IoT Hub-Instanz und Lesen der Telemetriedaten aus der IoT Hub-Instanz mit einer Back-End-Anwendung (Node.js)](quickstart-send-telemetry-java.md) absolviert haben, können Sie diesen Schritt überspringen.

Ein Gerät muss bei Ihrer IoT Hub-Instanz registriert sein, um eine Verbindung herstellen zu können. In dieser Schnellstartanleitung verwenden Sie Azure Cloud Shell, um ein simuliertes Gerät zu registrieren.

1. Führen Sie in Azure Cloud Shell den folgenden Befehl aus, um die Geräteidentität zu erstellen.

   **YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub ausgewählt haben.

   **MyJavaDevice**: Der Name des Geräts, das Sie registrieren. Es empfiehlt sich, **MyJavaDevice** wie gezeigt zu verwenden. Wenn Sie für Ihr Gerät einen anderen Namen auswählen, müssen Sie diesen innerhalb des gesamten Artikels verwenden und den Gerätenamen in den Beispielanwendungen aktualisieren, bevor Sie sie ausführen.

    ```azurecli-interactive
    az iot hub device-identity create \
      --hub-name {YourIoTHubName} --device-id MyJavaDevice
    ```

2. Führen Sie die folgenden Befehle in Azure Cloud Shell aus, um die _Geräteverbindungszeichenfolge_ für das soeben registrierte Gerät abzurufen:

   **YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub wählen.

    ```azurecli-interactive
    az iot hub device-identity show-connection-string \
      --hub-name {YourIoTHubName} \
      --device-id MyJavaDevice \
      --output table
    ```

    Notieren Sie sich die Geräteverbindungszeichenfolge, die wie folgt aussieht:

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyNodeDevice;SharedAccessKey={YourSharedAccessKey}`

    Dieser Wert wird später in der Schnellstartanleitung benötigt.

## <a name="retrieve-the-service-connection-string"></a>Abrufen der Verbindungszeichenfolge für den Dienst

Darüber hinaus benötigen Sie eine _Dienstverbindungszeichenfolge_, damit die Back-End-Anwendung eine Verbindung mit Ihrer IoT Hub-Instanz herstellen und die Nachrichten abrufen kann. Der folgende Befehl ruft die Dienstverbindungszeichenfolge für Ihre IoT Hub-Instanz ab:

**YourIoTHubName**: Ersetzen Sie diesen Platzhalter unten durch den Namen, den Sie für Ihren IoT-Hub ausgewählt haben.

```azurecli-interactive
az iot hub show-connection-string --policy-name service --name {YourIoTHubName} --output table
```

Notieren Sie sich die Dienstverbindungszeichenfolge, die wie folgt aussieht:

`HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}`

Dieser Wert wird später in der Schnellstartanleitung benötigt. Die Dienstverbindungszeichenfolge unterscheidet sich von der Geräteverbindungszeichenfolge, die Sie sich im vorherigen Schritt notiert haben.

## <a name="listen-for-direct-method-calls"></a>Lauschen auf Aufrufe direkter Methoden

Die simulierte Geräteanwendung stellt eine Verbindung mit einem gerätespezifischen Endpunkt in Ihrer IoT Hub-Instanz her, sendet simulierte Telemetriedaten und lauscht auf Aufrufe direkter Methoden aus Ihrem Hub. In dieser Schnellstartanleitung weist der Aufruf einer direkten Methode aus dem Hub das Gerät an, das Intervall zu ändern, in dem es Telemetriedaten sendet. Das simulierte Gerät sendet eine Bestätigung an Ihren Hub, nachdem es die direkte Methode ausgeführt hat.

1. Navigieren Sie in einem lokalen Terminalfenster zum Stammordner des Java-Beispielprojekts. Navigieren Sie anschließend zum Ordner **iot-hub\Quickstarts\simulated-device-2**.

2. Öffnen Sie die Datei **src/main/java/com/microsoft/docs/iothub/samples/SimulatedDevice.java** in einem Text-Editor Ihrer Wahl.

    Ersetzen Sie den Wert der Variablen `connString` durch die Geräteverbindungszeichenfolge, die Sie sich zuvor notiert haben. Speichern Sie dann die Änderungen an der Datei **SimulatedDevice.java**.

3. Führen Sie im lokalen Terminalfenster die folgenden Befehle aus, um die erforderlichen Bibliotheken zu installieren und die simulierte Geräteanwendung zu erstellen:

    ```cmd/sh
    mvn clean package
    ```

4. Führen Sie im lokalen Terminalfenster die folgenden Befehle aus, um die simulierte Geräteanwendung auszuführen:

    ```cmd/sh
    java -jar target/simulated-device-2-1.0.0-with-deps.jar
    ```

    Der folgende Screenshot zeigt die Ausgabe, während die Anwendung zur Simulation eines Geräts Telemetriedaten an Ihre IoT Hub-Instanz sendet:

    ![Ausgabe der Telemetrie, die vom Gerät an den IoT-Hub gesendet wurde](./media/quickstart-control-device-java/iot-hub-application-send-telemetry-output.png)

## <a name="call-the-direct-method"></a>Aufrufen der direkten Methode

Die Back-End-Anwendung stellt eine Verbindung mit einem dienstseitigen Endpunkt in Ihrer IoT Hub-Instanz her. Die Anwendung sendet über Ihren IoT-Hub Aufrufe direkter Methoden an ein Gerät und lauscht auf Bestätigungen. Eine IoT Hub-Back-End-Anwendung wird in der Regel in der Cloud ausgeführt.

1. Navigieren Sie in einem anderen lokalen Terminalfenster zum Stammordner des Java-Beispielprojekts. Navigieren Sie anschließend zum Ordner **iot-hub\Quickstarts\back-end-application**.

2. Öffnen Sie die Datei **src/main/java/com/microsoft/docs/iothub/samples/BackEndApplication.java** in einem Text-Editor Ihrer Wahl.

    Ersetzen Sie den Wert der Variablen `iotHubConnectionString` durch die Dienstverbindungszeichenfolge, die Sie sich zuvor notiert haben. Speichern Sie dann Ihre Änderungen an der Datei **BackEndApplication.java**.

3. Führen Sie im lokalen Terminalfenster die folgenden Befehle aus, um die erforderlichen Bibliotheken zu installieren und die Back-End-Anwendung zu erstellen:

    ```cmd/sh
    mvn clean package
    ```

4. Führen Sie im lokalen Terminalfenster die folgenden Befehle aus, um die Back-End-Anwendung auszuführen:

    ```cmd/sh
    java -jar target/back-end-application-1.0.0-with-deps.jar
    ```

    Der folgende Screenshot zeigt die Ausgabe, nachdem die Anwendung einen Aufruf einer direkten Methode an das Gerät gerichtet und eine Bestätigung empfangen hat:

    ![Ausgabe, wenn die Anwendung einen direkten Methodenaufruf über Ihren IoT-Hub durchführt](./media/quickstart-control-device-java/iot-hub-direct-method-call-output.png)

    Nachdem Sie die Back-End-Anwendung ausgeführt haben, sehen Sie eine Nachricht im Konsolenfenster, in dem das simulierte Gerät ausgeführt wird, und die Häufigkeit, mit der das Gerät Nachrichten sendet, ändert sich:

    ![Konsolenmeldung vom Gerät mit der Änderungsrate](./media/quickstart-control-device-java/iot-hub-sent-message-change-rate.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie in einer Back-End-Anwendung eine direkte Methode auf einem Gerät aufgerufen und in einer simulierten Geräteanwendung auf den Aufruf einer direkten Methode geantwortet.

Um zu erfahren, wie Sie Gerät-zu-Cloud-Nachrichten an verschiedene Ziele in der Cloud weiterleiten, fahren Sie mit dem nächsten Tutorial fort.

> [!div class="nextstepaction"]
> [Tutorial: Weiterleiten von Telemetriedaten zur Verarbeitung an verschiedene Endpunkte](tutorial-routing.md)
