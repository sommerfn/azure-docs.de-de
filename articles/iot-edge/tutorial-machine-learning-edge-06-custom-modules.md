---
title: 'Erstellen und Bereitstellen benutzerdefinierter Module: Machine Learning in Azure IoT Edge | Microsoft-Dokumentation'
description: Erstellen Sie IoT Edge-Module, die Daten von Blattgeräten durch ein Machine Learning-Modell verarbeiten und die Erkenntnisse dann an IoT Hub senden, und stellen Sie sie bereit.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 731abdde2160c16123ed648b5e552e9e62378438
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73494002"
---
# <a name="tutorial-create-and-deploy-custom-iot-edge-modules"></a>Tutorial: Erstellen und Bereitstellen von benutzerdefinierten IoT Edge-Modulen

> [!NOTE]
> Dieser Artikel ist Teil einer Tutorialreihe zur Verwendung von Azure Machine Learning für IoT Edge. Falls Sie direkt zu diesem Artikel navigiert sind, ist es ratsam, mit dem [ersten Artikel](tutorial-machine-learning-edge-01-intro.md) der Reihe zu beginnen.

In diesem Artikel erstellen wir drei IoT Edge-Module, die Nachrichten von Blattgeräten empfangen, die Daten in Ihrem Machine Learning-Modell verarbeiten lassen und dann Erkenntnisse an IoT Hub weiterleiten.

Der IoT Edge-Hub ermöglicht die Kommunikation zwischen Modulen. Die Verwendung des IoT Edge-Hubs als Nachrichtenbroker sorgt dafür, dass die Module voneinander unabhängig bleiben. Module müssen nur die Eingänge, an denen sie Nachrichten akzeptieren, und die Ausgänge, an die sie Nachrichten schreiben, angeben.

Das IoT Edge-Gerät soll vier Aufgaben für uns durchführen:

* Daten von den Blattgeräten empfangen
* Die verbleibende Nutzungsdauer (Remaining Useful Life, RUL) des Geräts vorhersagen, das die Daten gesendet hat
* Eine Nachricht an IoT Hub senden, die nur über die RUL des Geräts informiert (diese Funktion kann so geändert werden, dass sie nur dann Daten sendet, wenn die RUL unter einen bestimmten Wert fällt)
* Die Daten des Blattgeräts in einer lokalen Datei auf dem IoT Edge-Gerät speichern. Diese Datendatei wird regelmäßig in IoT Hub hochgeladen, um das Training des Machine Learning-Modells zu verfeinern. Die Verwendung des Dateiuploads anstelle von konstantem Nachrichtenstreaming ist kostengünstiger.

Um diese Aufgaben auszuführen, verwenden wir drei benutzerdefinierte Module:

* **RUL-Klassifizierer:** Das turboFanRulClassifier-Modul, das wir in [Trainieren und Bereitstellen eines Azure Machine Learning-Modells](tutorial-machine-learning-edge-04-train-model.md) erstellt haben, ist ein Standard-Machine Learning-Modul mit der Eingabe „amlInput“ und der Ausgabe „amlOutput“. Die in „amlInput“ eingegebenen Daten müssen mit der an den ACI-basierten Webdienst gesendeten Eingabe identisch sein. Entsprechend gibt „amlOutput“ die gleichen Daten wie der Webdienst zurück.

* **Avro-Writer:** Dieses Modul empfängt Nachrichten über die Eingabe „avroModuleInput“ und speichert die Nachricht für einen späteren Upload zu IoT Hub im Avro-Format auf der Festplatte.

* **Router-Modul:** Das Router-Modul empfängt Nachrichten von Downstreamblattgeräten, formatiert die Nachrichten dann und sendet sie an den Klassifizierer. Dann empfängt das Modul die Nachrichten vom Klassifizierer und leitet sie an das Avro-Writer-Modul weiter. Schließlich sendet das Modul nur die RUL-Vorhersage an den IoT-Hub.

  * Eingaben:
    * **deviceInput**: empfängt Nachrichten von Blattgeräten
    * **rulInput:** empfängt Nachrichten von „amlOutput“

  * Ausgaben:
    * **classify:** sendet Nachrichten an „amlInput“
    * **writeAvro:** sendet Nachrichten an „avroModuleInput“
    * **toIotHub:** sendet Nachrichten an $upstream, $upstream übergibt die Nachrichten an den verbundenen IoT-Hub

Das folgende Diagramm zeigt die Module, Eingaben, Ausgaben und IoT Edge Hub-Routen für die Komplettlösung:

![IoT Edge-Architekturdiagramm mit drei Modulen](media/tutorial-machine-learning-edge-06-custom-modules/modules-diagram.png)

Die Schritte in diesem Artikel werden normalerweise von einem Cloudentwickler ausgeführt.

## <a name="create-a-new-iot-edge-solution"></a>Erstellen einer neuen IoT Edge-Projektmappe

Während der Ausführung des zweiten unserer beiden Azure Notebooks haben wir ein Containerimage mit unserem RUL-Modell erstellt und veröffentlicht. Azure Machine Learning hat im Rahmen der Imageerstellung dieses Modell gepackt, damit das Image als Azure IoT Edge-Modul bereitgestellt werden kann. In diesem Schritt erstellen wir eine Azure IoT Edge-Projektmappe mit dem Modul „Azure Machine Learning“ und verweisen das Modul auf das Image, das wir mit Azure Notebooks veröffentlicht haben.

1. Öffnen Sie eine Remotedesktopsitzung auf Ihrem Entwicklungscomputer.

2. Öffnen Sie den Ordner **C:\\source\\IoTEdgeAndMlSample** in Visual Studio Code.

3. Klicken Sie mit der rechten Maustaste auf den Explorer-Bereich (in den leeren Bereich), und wählen Sie **Neue IoT Edge-Projektmappe** aus.

    ![Erstellen einer neuen IoT Edge-Projektmappe](media/tutorial-machine-learning-edge-06-custom-modules/new-edge-solution-command.png)

4. Akzeptieren Sie den standardmäßigen Projektmappennamen **EdgeSolution**.

5. Wählen Sie **Azure Machine Learning** als Modulvorlage aus.

6. Nennen Sie das Modul **turbofanRulClassifier**.

7. Wählen Sie Ihren Machine Learning-Arbeitsbereich aus.

8. Wählen Sie das Image aus, das Sie während der Ausführung des Azure Notebooks erstellt haben.

9. Sehen Sie sich die Projektmappe an, und achten Sie darauf, welche Dateien erstellt wurden:

   * **deployment.template.json:** Diese Datei enthält die Definitionen der einzelnen Module in der Projektmappe. In dieser Datei müssen Sie drei Abschnitte beachten:

     * **RegistryCredentials:** Definiert den Satz der benutzerdefinierten Containerregistrierungen, die Sie in der Projektmappe verwenden. Im Moment sollte er die Registrierung aus Ihrem Machine Learning-Arbeitsbereich enthalten, in dem Ihr Azure Machine Learning-Image gespeichert wurde. Sie können eine beliebige Anzahl von Containerregistrierungen haben, aber zur Vereinfachung verwenden wir diese eine Registrierung für alle Module.

       ```json
       "registryCredentials": {
         "<your registry>": {
           "username": "$CONTAINER_REGISTRY_USERNAME_<your registry>",
           "password": "$CONTAINER_REGISTRY_PASSWORD_<your registry>",
           "address": "<your registry>.azurecr.io"
         }
       }
       ```

     * **Modules:** Dieser Abschnitt enthält den Satz benutzerdefinierter Module, die zu dieser Projektmappe gehören. Sie werden feststellen, dass dieser Abschnitt derzeit zwei Module enthält: SimulatedTemperatureSensor und turbofanRulClassifier. Das Modul SimulatedTemperatureSensor wurde von der Visual Studio Code-Vorlage installiert, für diese Projektmappe benötigen wir es jedoch nicht. Sie können die Definition des SimulatedTemperatureSensor-Moduls aus dem Abschnitt „modules“ löschen. Beachten Sie, dass die turbofanRulClassifier-Moduldefinition auf das Image in Ihrer Containerregistrierung verweist. Weitere Module, die wir der Projektmappe hinzufügen, werden in diesem Abschnitt angezeigt.

       ```json
       "modules": {
         "SimulatedTemperatureSensor": {
           "version": "1.0",
           "type": "docker",
           "status": "running",
           "restartPolicy": "always",
           "settings": {
             "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
             "createOptions": {}
           }
         },
         "turbofanRulClassifier": {
           "version": "1.0",
           "type": "docker",
           "status": "running",
           "restartPolicy": "always",
           "settings": {
             "image": "<your registry>.azurecr.io/edgemlsample:1",
             "createOptions": {}
           }
         }
       }
       ```

     * **Routes:** Wir werden in diesem Tutorial recht viel mit Routen arbeiten. Routen definieren, wie Module miteinander kommunizieren. Die von der Vorlage definierten beiden Routen stimmen nicht mit dem Routing überein, das wir benötigen. Die erste Route sendet alle Daten aus jeder Ausgabe des Klassifizierers an den IoT-Hub ($upstream). Die andere Route ist für das SimulatedTemperatureSensor-Modul bestimmt, das wir gerade gelöscht haben. Löschen Sie die zwei Standardrouten.

       ```json
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
             "turbofanRulClassifierToIoTHub": "FROM /messages/modules/turbofanRulClassifier/outputs/\* INTO $upstream",
             "sensorToturbofanRulClassifier": "FROM /messages/modules/SimulatedTemperatureSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\\"/modules/turbofanRulClassifier/inputs/input1\\")"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       }
       ```

   * **deployment.debug.template.json:** Diese Datei ist die Debugversion von „deployment.template.json“. Wir sollten alle Änderungen aus „deployment.template.json“ in dieser Datei spiegeln.

   * **.env:** In dieser Datei sollten Sie den Benutzernamen und das Kennwort für den Zugriff auf Ihre Registrierung angeben.

      ```env
      CONTAINER_REGISTRY_USERNAME_<your registry name>=<ACR username>
      CONTAINER_REGISTRY_PASSWORD_<your registry name>=<ACR password>
      ```

10. Klicken Sie mit der rechten Maustaste auf die Datei „deployment.template.json“, und wählen Sie anschließend **IoT Edge-Projektmappe erstellen** aus.

11. Beachten Sie, dass dieser Befehl einen Ordner „config“ mit einer deployment.amd64.json-Datei erstellt. Diese Datei ist die konkrete Bereitstellungsvorlage für die Projektmappe.

## <a name="add-router-module"></a>Hinzufügen des Router-Moduls

Als Nächstes fügen wir unserer Projektmappe das Router-Modul hinzu. Das Router-Modul führt mehrere Aufgaben für unsere Projektmappe aus:

* **Empfangen von Nachrichten von Blattgeräten:** Wenn Nachrichten von Downstreamgeräten beim IoT Edge-Gerät eingehen, empfängt das Router-Modul die Nachricht und beginnt mit der Orchestrierung des Routings der Nachricht.
* **Senden von Nachrichten an das RUL-Klassifizierungsmodul:** Wenn eine neue Nachricht von einem Downstreamgerät empfangen wird, transformiert das Router-Modul die Nachricht in das Format, das der RUL-Klassifizierer erwartet. Der Router sendet die Nachricht zur RUL-Vorhersage an den Klassifizierer. Sobald der Klassifizierer eine Vorhersage getroffen hat, sendet er die Nachricht an des Router-Modul zurück.
* **Senden von RUL-Nachrichten an IoT Hub:** Wenn der Router Nachrichten vom Klassifizierer empfängt, transformiert er die Nachricht so, dass sie nur die wesentlichen Informationen enthält, Geräte-ID und RUL, und sendet die abgekürzte Nachricht an den IoT-Hub. Bei einer weiteren Verfeinerung, die wir hier nicht durchgeführt haben, würden Nachrichten nur dann an den IoT-Hub gesendet, wenn die RUL-Vorhersage unter einen bestimmten Schwellenwert fallen würde (wenn die RUL z.B. weniger als 100 Zyklen betragen würde). Eine solche Filterung würde die Anzahl der Nachrichten reduzieren und die Kosten des IoT-Hubs senken.
* **Senden von Nachrichten an das Avro-Writer-Modul:** Um alle vom Downstreamgerät gesendeten Daten beizubehalten, sendet das Router-Modul die gesamte vom Klassifizierer empfangene Nachricht an das Avro-Writer-Modul, das die Daten mithilfe des IoT Hub-Dateiuploads speichert und hochlädt.

> [!NOTE]
> Die Beschreibung der Modulaufgaben weckt möglicherweise den Eindruck einer sequenziellen Verarbeitung, aber der Ablauf basiert auf Nachrichten/Ereignissen. Deshalb benötigen wir ein Orchestrierungsmodul wie unser Router-Modul.

### <a name="create-module-and-copy-files"></a>Erstellen eines Moduls und Kopieren von Dateien

1. Klicken Sie mit der rechten Maustaste auf den modules-Ordner in Visual Studio Code, und wählen Sie **IoT Edge-Modul hinzufügen** aus.

2. Wählen Sie **C#-Modul** aus.

3. Nennen Sie das Modul **turbofanRouter**.

4. Wenn Sie nach Ihrem Docker Image Repository gefragt werden, verwenden Sie die Registrierung aus dem Machine Learning-Arbeitsbereich (Sie finden die Registrierung im registryCredentials-Knoten Ihrer *deployment.template.json*-Datei). Dieser Wert ist die vollqualifizierte Adresse der Registrierung, wie z.B. **\<Ihre Registrierung\>.azurecr.io/turbofanrouter**.

    > [!NOTE]
    > In diesem Artikel verwenden wir die Azure Container Registry-Instanz, die durch den Azure Machine Learning-Arbeitsbereich erstellt wurde, den wir zum Trainieren und Bereitstellen unseres Klassifizierers verwendet haben. Dies dient ausschließlich der Benutzerfreundlichkeit. Wir hätten eine neue Containerregistrierung erstellen und unsere Module dort veröffentlichen können.

5. Öffnen Sie ein neues Terminalfenster in Visual Studio Code (**Ansicht** > **Terminal**), und kopieren Sie Dateien aus dem modules-Verzeichnis.

    ```cmd
    copy c:\source\IoTEdgeAndMlSample\EdgeModules\modules\turbofanRouter\*.cs c:\source\IoTEdgeAndMlSample\EdgeSolution\modules\turbofanRouter\
    ```

6. Drücken Sie bei Aufforderung zum Überschreiben der Datei „program.cs“ `y` und dann `Enter`.

### <a name="build-router-module"></a>Erstellen des Router-Moduls

1. Wählen Sie in Visual Studio Code **Terminal** > **Standarderstellungsaufgabe konfigurieren** aus.

2. Klicken Sie auf **tasks.json-Datei aus Vorlage erstellen**.

3. Klicken Sie auf **.NET Core**.

4. Wenn „tasks.json“ geöffnet wird, ersetzen Sie den Inhalt mit:

    ```json
    {
      // See https://go.microsoft.com/fwlink/?LinkId=733558
      // for the documentation about the tasks.json format
      "version": "2.0.0",
      "tasks": [
        {
          "label": "build",
          "command": "dotnet",
          "type": "shell",
          "group": {
            "kind": "build",
            "isDefault": true
          },
          "args": [
            "build",
            "${workspaceFolder}/modules/turbofanRouter"
          ],
          "presentation": {
            "reveal": "always"
          },
          "problemMatcher": "$msCompile"
        }
      ]
    }
    ```

5. Speichern und schließen Sie „tasks.json“.

6. Führen Sie den Build mit `Ctrl + Shift + B` oder **Terminal** > **Buildtask ausführen** aus.

### <a name="set-up-module-routes"></a>Einrichten von Modulrouten

Wie bereits erwähnt, verwaltet die IoT Edge-Runtime die Kommunikation zwischen lose gekoppelten Modulen mit Routen, die in der *deployment.template.json*-Datei konfiguriert werden. In diesem Abschnitt wird ausführlich erläutert, wie die Routen für das turbofanRouter-Modul eingerichtet werden. Wir werden zuerst die Eingaberouten behandeln und dann zu den Ausgaben übergehen.

#### <a name="inputs"></a>Eingaben

1. In der Init()-Methode der Datei „Program.cs“ sehen wir zwei Rückrufe für das Modul:

   ```csharp
   await ioTHubModuleClient.SetInputMessageHandlerAsync(EndpointNames.FromLeafDevice, LeafDeviceInputMessageHandler, ioTHubModuleClient);
   await ioTHubModuleClient.SetInputMessageHandlerAsync(EndpointNames.FromClassifier, ClassifierCallbackMessageHandler, ioTHubModuleClient);
   ```

2. Der erste Rückruf lauscht auf Nachrichten, die an die **deviceInput**-Senke gesendet werden. Im obigen Diagramm sehen wir, dass Nachrichten von einem beliebigen Blattgerät an diese Eingabe umgeleitet werden sollen. Fügen Sie in der Datei *deployment.template.json* eine Route hinzu, die die Edge-Hub-Instanz anweist, jede vom IoT Edge-Gerät empfangene Nachricht, die nicht von einem IoT Edge-Modul gesendet wurde, in den Eingang „deviceInput“ des turbofanRouter-Moduls umzuleiten:

   ```json
   "leafMessagesToRouter": "FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/deviceInput\")"
   ```

3. Fügen Sie als nächstes eine Route für Nachrichten vom rulClassifier-Modul in das turbofanRouter-Modul hinzu:

   ```json
   "classifierToRouter": "FROM /messages/modules/turbofanRulClassifier/outputs/amloutput INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/rulInput\")"
   ```

#### <a name="outputs"></a>Ausgaben

Fügen Sie dem $edgeHub-Routeparameter vier zusätzliche Routen hinzu, um die Ausgaben des Router-Moduls zu verarbeiten.

1. „Program.cs“ definiert die Methode SendMessageToClassifier(), die mit dem Modulclient über diese Route eine Nachricht an den RUL-Klassifizierer sendet:

   ```json
   "routerToClassifier": "FROM /messages/modules/turbofanRouter/outputs/classOutput INTO BrokeredEndpoint(\"/modules/turbofanRulClassifier/inputs/amlInput\")"
   ```

2. SendRulMessageToIotHub() sendet mit dem Modulclient nur die RUL-Daten für das Gerät über diese Route an den IoT-Hub:

   ```json
   "routerToIoTHub": "FROM /messages/modules/turboFanRouter/outputs/hubOutput INTO $upstream"
   ```

3. SendMessageToAvroWriter() sendet mit dem Modulclient die Nachricht mit den hinzugefügten RUL-Daten an das avroFileWriter-Modul.

   ```json
   "routerToAvro": "FROM /messages/modules/turbofanRouter/outputs/avroOutput INTO BrokeredEndpoint(\"/modules/avroFileWriter/inputs/avroModuleInput\")"
   ```

4. HandleBadMessage() sendet Nachrichten, bei denen Fehler aufgetreten sind, an den IoT-Hub, wo sie später weitergeleitet werden können.

   ```json
   "deadLetter": "FROM /messages/modules/turboFanRouter/outputs/deadMessages INTO $upstream"
   ```

Mit allen Routen zusammen sollte Ihr „$edgeHub“-Knoten folgendem JSON-Code entsprechen:

```json
"$edgeHub": {
  "properties.desired": {
    "schemaVersion": "1.0",
    "routes": {
      "leafMessagesToRouter": "FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/deviceInput\")",
      "classifierToRouter": "FROM /messages/modules/turbofanRulClassifier/outputs/amlOutput INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/rulInput\")",
      "routerToClassifier": "FROM /messages/modules/turbofanRouter/outputs/classOutput INTO BrokeredEndpoint(\"/modules/turbofanRulClassifier/inputs/amlInput\")",
      "routerToIoTHub": "FROM /messages/modules/turboFanRouter/outputs/hubOutput INTO $upstream",
      "routerToAvro": "FROM /messages/modules/turbofanRouter/outputs/avroOutput INTO BrokeredEndpoint(\"/modules/avroFileWriter/inputs/avroModuleInput\")",
      "deadLetter": "FROM /messages/modules/turboFanRouter/outputs/deadMessages INTO $upstream"
    },
    "storeAndForwardConfiguration": {
      "timeToLiveSecs": 7200
    }
  }
}
```

> [!NOTE]
> Durch das Hinzufügen des turbofanRouter-Moduls entstand folgende zusätzliche Route: `turbofanRouterToIoTHub": "FROM /messages/modules/turbofanRouter/outputs/* INTO $upstream`. Entfernen Sie diese Route, sodass nur die oben aufgeführten Routen in Ihrer deployment.template.json-Datei übrig bleiben.

#### <a name="copy-routes-to-deploymentdebugtemplatejson"></a>Kopieren von Routen in „deployment.debug.template.json“

Als letzten Schritt, um unsere Dateien synchron zu halten, spiegeln Sie die Änderungen, die Sie an „deployment.template.json“ vorgenommen haben, in „deployment.debug.template.json“.

## <a name="add-avro-writer-module"></a>Hinzufügen des Avro-Writer-Moduls

Das Avro-Writer-Modul hat in unserer Projektmappe zwei Aufgaben, nämlich das Speichern von Nachrichten und das Hochladen von Dateien.

* **Speichern von Nachrichten**: Wenn das Avro-Writer-Modul eine Nachricht empfängt, schreibt es die Nachricht im Avro-Format in das lokale Dateisystem. Wir verwenden eine Bindung, bei der ein Verzeichnis (in diesem Fall „/data/avrofiles“) in einen Pfad im Container des Moduls eingebunden wird. Diese Bindung ermöglicht, dass das Modul in einen lokalen Pfad („/avrofiles“) schreiben kann, und dass direkt vom IoT Edge-Gerät aus auf diese Dateien zugegriffen werden kann.

* **Hochladen von Dateien**: Das Avro-Writer-Modul verwendet die Azure IoT Hub-Funktion zum Hochladen von Dateien in ein Azure-Speicherkonto. Sobald eine Datei erfolgreich hochgeladen ist, löscht das Modul die Datei vom Datenträger.

### <a name="create-module-and-copy-files"></a>Erstellen eines Moduls und Kopieren von Dateien

1. Wählen Sie in der Befehlspalette folgenden Befehl aus: **Python: Select Interpreter** ein.

1. Wählen Sie den unter „C:\\Python37“ gefundenen Interpreter aus.

1. Öffnen Sie die Befehlspalette erneut, und wählen Sie folgenden Befehl aus: **Terminal: Standardshell auswählen**.

1. Wählen Sie nach Aufforderung **Eingabeaufforderung** aus.

1. Öffnen Sie eine neue Terminalshell, **Terminal** > **Neues Terminal**.

1. Klicken Sie mit der rechten Maustaste auf den modules-Ordner in Visual Studio Code, und wählen Sie **IoT Edge-Modul hinzufügen** aus.

1. Wählen Sie **Python-Modul** aus.

1. Nennen Sie das Modul „avroFileWriter“.

1. Wenn Sie nach Ihrem Docker Image Repository gefragt werden, verwenden Sie die gleiche Registrierung wie beim Hinzufügen des Router-Moduls.

1. Kopieren Sie Dateien aus dem Beispielmodul in die Projektmappe.

   ```cmd
   copy C:\source\IoTEdgeAndMlSample\EdgeModules\modules\avroFileWriter\*.py C:\source\IoTEdgeAndMlSample\EdgeSolution\modules\avroFileWriter\
   ```

1. Wenn Sie aufgefordert werden, „main.py“ zu überschreiben, geben Sie `y` ein, und drücken Sie dann `Enter`.

1. Beachten Sie, dass „filemanager.py“ und „schema.py“ der Projektmappe hinzugefügt wurden und „main.py“ aktualisiert wurde.

> [!NOTE]
> Wenn Sie eine Python-Datei öffnen, werden Sie möglicherweise aufgefordert, PyLint zu installieren. Sie müssen den Linter zur Durchführung dieses Tutorials nicht installieren.

### <a name="bind-mount-for-data-files"></a>Bindung für Datendateien

Wie in der Einführung erwähnt, basiert das Writer-Modul auf dem Binden von Avro-Dateien in das Dateisystem des Geräts.

#### <a name="add-directory-to-device"></a>Hinzufügen des Verzeichnisses zum Gerät

1. Stellen Sie über SSH eine Verbindung mit Ihrer IoT Edge-Gerät-VM her.

   ```bash
   ssh -l <user>@IoTEdge-<extension>.<region>.cloudapp.azure.com
   ```

2. Erstellen Sie das Verzeichnis, das die gespeicherten Blattgerätnachrichten enthalten wird.

   ```bash
   sudo mkdir -p /data/avrofiles
   ```

3. Aktualisieren Sie die Verzeichnisberechtigungen, sodass der Container in das Verzeichnis schreiben kann.

   ```bash
   sudo chmod ugo+rw /data/avrofiles
   ```

4. Überprüfen Sie, ob Benutzer, Gruppe und Besitzer jetzt über die Schreibberechtigung (w) für das Verzeichnis verfügen.

   ```bash
   ls -la /data
   ```

   ![Verzeichnisberechtigungen für „avrofiles“](media/tutorial-machine-learning-edge-06-custom-modules/avrofiles-directory-permissions.png)

#### <a name="add-directory-to-the-module"></a>Hinzufügen des Verzeichnisses zum Modul

Um das Verzeichnis dem Container des Moduls hinzuzufügen, ändern wir die mit dem avroFileWriter-Modul verknüpften Dockerfiles. Drei Dockerfiles sind mit dem Modul verknüpft: Dockerfile.amd64, Dockerfile.amd64.debug und Dockerfile.arm32v7. Diese Dateien sollten für etwaiges Debuggen oder Bereitstellen auf einem arm32-Gerät synchron gehalten werden. Konzentrieren Sie sich für diesen Artikel nur auf „Dockerfile.amd64“.

1. Öffnen Sie auf dem Entwicklungscomputer die **Dockerfile.amd64**-Datei.

2. Ändern Sie die Datei, sodass sie wie folgt aussieht:

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
   python3-pip libboost-python1.58-dev libpython3-dev && rm -rf /var/lib/apt/lists/*

   RUN pip3 install --upgrade pip
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   USER moduleuser

   CMD [ "python3", "-u", "./main.py" ]
   ```

   Die Befehle `mkdir` und `chown` weisen den Docker-Buildprozess an, ein oberstes Verzeichnis namens „/avrofiles“ im Image zu erstellen und den Modulbenutzer dann zum Besitzer dieses Verzeichnisses zu machen. Es ist wichtig, dass diese Befehle eingefügt werden, nachdem der Modulbenutzer mit dem Befehl `useradd` dem Image hinzugefügt wurde, und bevor der Kontext zum „moduleuser“ („USER moduleuser“) wechselt.

3. Nehmen Sie die entsprechenden Änderungen an „Dockerfile.amd64.debug“ und „Dockerfile.arm32v7“ vor.

#### <a name="update-the-module-configuration"></a>Aktualisieren der Modulkonfiguration

Der letzte Schritt beim Erstellen der Bindung ist die Aktualisierung der Dateien „deployment.template.json“ (und „deployment.debug.template.json“) mit den Bindungsinformationen.

1. Öffnen Sie „deployment.template.json“.

2. Ändern Sie die Moduldefinition für avroFileWriter, indem Sie den `Binds`-Parameter hinzufügen, der das Containerverzeichnis „/avrofiles“ auf das lokale Verzeichnis des Edge-Geräts verweist. Ihre Moduldefinition sollte dem folgenden Beispiel entsprechen:

   ```json
   "avroFileWriter": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "settings": {
       "image": "${MODULES.avroFileWriter}",
       "createOptions": {
         "HostConfig": {
           "Binds": [
             "/data/avrofiles:/avrofiles"
           ]
         }
       }
     }
   }
   ```

3. Nehmen Sie die entsprechenden Änderungen an „deployment.debug.template.json“ vor.

### <a name="bind-mount-for-access-to-configyaml"></a>Bindung für den Zugriff auf „config.yaml“

Wir müssen eine weitere Bindung für das Writer-Modul hinzufügen. Diese Bindung bietet dem Modul Lesezugriff auf die Verbindungszeichenfolge aus der /etc/iotedge/config.yaml-Datei auf dem IoT Edge-Gerät. Wir benötigen die Verbindungszeichenfolge, um einen IoTHubClient zu erstellen, damit wir die upload\_blob\_async-Methode aufrufen können, um Dateien auf den IoT-Hub hochzuladen. Die Schritte zum Hinzufügen dieser Bindung ähneln denen im vorherigen Abschnitt.

#### <a name="update-directory-permission"></a>Aktualisieren von Verzeichnisberechtigungen

1. Stellen Sie über SSH eine Verbindung mit Ihrem IoT Edge-Gerät her.

   ```bash
   ssh -l <user>@IoTEdge-<extension>.<region>.cloudapp.azure.com
   ```

2. Fügen Sie die Leseberechtigung für die Datei „config.yaml“ hinzu.

   ```bash
   sudo chmod +r /etc/iotedge/config.yaml
   ```

3. Überprüfen Sie, ob die Berechtigungen richtig festgelegt sind.

   ```bash
   ls -la /etc/iotedge/
   ```

4. Stellen Sie sicher, dass die Berechtigungen für „config.yaml“ **-r--r--r--** sind.

#### <a name="add-directory-to-module"></a>Hinzufügen des Verzeichnisses zum Modul

1. Öffnen Sie auf dem Entwicklungscomputer die **Dockerfile.amd64**-Datei.

2. Fügen Sie der Datei einen zusätzlichen Satz von `mkdir`- und `chown`-Befehlen hinzu, sodass sie wie folgt aussieht:

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
   python3-pip libboost-python1.58-dev libpython3-dev && rm -rf /var/lib/apt/lists/\*

   RUN pip3 install --upgrade pip
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   RUN mkdir -p /app/iotconfig && chown moduleuser /app/iotconfig

   USER moduleuser

   CMD "python3", "-u", "./main.py"]
   ```

3. Nehmen Sie die entsprechenden Änderungen an „Dockerfile.amd64.debug“ und „Dockerfile.arm32v7“ vor.

#### <a name="update-the-module-configuration"></a>Aktualisieren der Modulkonfiguration

1. Öffnen Sie die Datei **deployment.template.json**.

2. Ändern Sie die Moduldefinition für avroFileWriter, indem Sie dem `Binds`-Parameter eine zweite Zeile hinzufügen, die das Containerverzeichnis („/app/iotconfig“) auf das lokale Verzeichnis des Geräts („/etc/iotedge“) verweist.

   ```json
   "avroFileWriter": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "settings": {
       "image": "${MODULES.avroFileWriter}",
       "createOptions": {
         "HostConfig": {
           "Binds": [
             "/data/avrofiles:/avrofiles",
             "/etc/iotedge:/app/iotconfig"
           ]
         }
       }
     }
   }
   ```

3. Nehmen Sie die entsprechenden Änderungen an „deployment.debug.template.json“ vor.

## <a name="install-dependencies"></a>Installieren von Abhängigkeiten

Das Writer-Modul ist von zwei Python-Bibliotheken abhängig, fastavro und PyYAML. Wir müssen die Abhängigkeiten auf unserem Entwicklungscomputer installieren und den Docker-Buildprozess anweisen, sie im Image unseres Moduls zu installieren.

### <a name="pyyaml"></a>PyYAML

1. Öffnen Sie auf dem Entwicklungscomputer die **requirements.txt**-Datei, und fügen Sie „pyyaml“ hinzu.

   ```txt
   azure-iothub-device-client~=1.4.3
   pyyaml
   ```

2. Öffnen Sie die **Dockerfile.amd64**-Datei, und fügen Sie einen `pip install`-Befehl hinzu, um Setuptools zu aktualisieren.

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && \
       apt-get install -y --no-install-recommends libcurl4-openssl-dev python3-pip libboost-python1.58-dev libpython3-dev && \
       rm -rf /var/lib/apt/lists/\*

   RUN pip3 install --upgrade pip
   RUN pip install -U pip setuptools
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   RUN mkdir -p /app/iotconfig && chown moduleuser /app/iotconfig
   USER moduleuser

   CMD [ "python3", "-u", "./main.py" ]
   ```

3. Nehmen Sie die entsprechenden Änderungen an „Dockerfile.amd64.debug“ vor. <!--may not be necessary. Add 'if needed'?-->

4. Installieren Sie „pyyaml“ lokal, indem Sie ein Terminal in Visual Studio Code öffnen und Folgendes eingeben:

   ```cmd
   pip install pyyaml
   ```

### <a name="fastavro"></a>Fastavro

1. Fügen Sie „fastavro“ in „requirements.txt“ nach „pyyaml“ hinzu.

   ```txt
   azure-iothub-device-client~=1.4.3
   pyyaml
   fastavro
   ```

2. Installieren Sie „fastavro“ mit dem Visual Studio Code-Terminal auf Ihrem Entwicklungscomputer.

   ```cmd
   pip install fastavro
   ```

## <a name="reconfigure-iot-hub"></a>Neukonfigurieren von IoT Hub

Mit der Einführung des IoT Edge-Geräts und der Module in das System haben wir unsere Erwartungen geändert, welche Daten zu welchem Zweck an den Hub gesendet werden. Wir müssen das Routing im Hub neu konfigurieren, um unserer neuen Wirklichkeit gerecht zu werden.

> [!NOTE]
> Wir konfigurieren den Hub vor dem Bereitstellen von Modulen neu, da einige der Hubeinstellungen, insbesondere der Dateiupload, korrekt eingerichtet sein müssen, damit das avroFileWriter-Modul korrekt ausgeführt werden kann.

### <a name="set-up-route-for-rul-messages-in-iot-hub"></a>Einrichten der Route für RUL-Nachrichten in IoT Hub

Wir erwarten, mit dem vorhandenen Router und Klassifizierer regelmäßig Nachrichten zu empfangen, die nur die Geräte-ID und die RUL-Vorhersage für das Gerät enthalten. Wir möchten die RUL-Daten an einen eigenen Speicherort leiten, wo wir nach Bedarf den Status der Geräte überwachen, Berichte erstellen und Warnungen auslösen können. Gleichzeitig sollen alle Gerätedaten, die noch direkt von einem Blattgerät gesendet werden, das noch nicht an unser IoT Edge-Gerät angeschlossen ist, weiter zum aktuellen Speicherort geleitet werden.

#### <a name="create-a-rul-message-route"></a>Erstellen einer RUL-Nachrichtenroute

1. Navigieren Sie im Azure-Portal zu Ihrem IoT-Hub.

2. Wählen Sie im linken Navigationsbereich **Nachrichtenweiterleitung** aus.

3. Wählen Sie **Hinzufügen**.

4. Benennen Sie die Route **RulMessageRoute**.

5. Wählen Sie **Hinzufügen** neben der **Endpunkt**-Auswahl aus, und wählen Sie **Blobspeicher** aus.

6. Benennen Sie den Endpunkt im Formular **Neuen Speicherendpunkt hinzufügen** mit **ruldata**.

7. Wählen Sie **Container auswählen**.

8. Wählen Sie das durchgängig in diesem Tutorial verwendete Speicherkonto aus, dessen Name **iotedgeandml\<eindeutiges Suffix\>** ähnelt.

9. Wählen Sie den gewünschten **ruldata**-Container aus, und klicken Sie auf **Auswählen**.

10. Klicken Sie auf **Erstellen**, um den Speicherendpunkt zu erstellen.

11. Geben Sie für **Routingabfrage** die folgende Abfrage ein:

    ```sql
    IS_DEFINED($body.PredictedRul) AND NOT IS_DEFINED($body.OperationalSetting1)
    ```

12. Erweitern Sie den Abschnitt **Test** und dann den Abschnitt **Nachrichtentext**. Ersetzen Sie die Nachricht mit diesem Beispiel unserer erwarteten Nachrichten:

    ```json
    {
      "ConnectionDeviceId": "aaLeafDevice_1",
      "CorrelationId": "b27e97bb-06c5-4553-a064-e9ad59c0fdd3",
      "PredictedRul": 132.62721409309165,
      "CycleTime": 64.0
    }
    ```

13. Wählen Sie **Route testen** aus. Wenn der Test erfolgreich ist, wird „Die Nachricht stimmte mit der Abfrage überein.“ angezeigt.

14. Klicken Sie auf **Speichern**.

#### <a name="update-turbofandevicetostorage-route"></a>Aktualisieren der turbofanDeviceToStorage-Route

Da wir die neuen Vorhersagedaten nicht an unseren alten Speicherort weiterleiten möchten, aktualisieren Sie die Route, um dies zu verhindern.

1. Wählen Sie auf der IoT Hub-Seite **Nachrichtenweiterleitung** die Registerkarte **Routen** aus.

2. Wählen Sie **turbofanDeviceDataToStorage** bzw. den Namen aus, den Sie der ersten Gerätedatenroute gegeben haben.

3. Aktualisieren Sie die Routingabfrage in

   ```sql
   IS_DEFINED($body.OperationalSetting1)
   ```

4. Erweitern Sie den Abschnitt **Test** und dann den Abschnitt **Nachrichtentext**. Ersetzen Sie die Nachricht mit diesem Beispiel unserer erwarteten Nachrichten:

   ```json
   {
     "Sensor13": 2387.96,
     "OperationalSetting1": -0.0008,
     "Sensor6": 21.61,
     "Sensor11": 47.2,
     "Sensor9": 9061.45,
     "Sensor4": 1397.86,
     "Sensor14": 8140.39,
     "Sensor18": 2388.0,
     "Sensor12": 522.87,
     "Sensor2": 642.42,
     "Sensor17": 391.0,
     "OperationalSetting3": 100.0,
     "Sensor1": 518.67,
     "OperationalSetting2": 0.0002,
     "Sensor20": 39.03,
     "DeviceId": 19.0,
     "Sensor5": 14.62,
     "PredictedRul": 212.00132402791962,
     "Sensor8": 2388.01,
     "Sensor16": 0.03,
     "CycleTime": 42.0,
     "Sensor21": 23.3188,
     "Sensor15": 8.3773,
     "Sensor3": 1580.09,
     "Sensor10": 1.3,
     "Sensor7": 554.57,
     "Sensor19": 100.0
   }
   ```

5. Wählen Sie **Route testen** aus. Wenn der Test erfolgreich ist, wird „Die Nachricht stimmte mit der Abfrage überein.“ angezeigt.

6. Wählen Sie **Speichern** aus.

### <a name="configure-file-upload"></a>Konfigurieren des Dateiuploads

Konfigurieren Sie die IoT Hub-Funktion zum Dateiupload, um dem Datei-Writer-Modul das Hochladen von Dateien in den Speicher zu ermöglichen.

1. Wählen Sie in Ihrem IoT-Hub im linken Navigatorbereich **Dateiupload** aus.

2. Wählen Sie **Azure-Speichercontainer** aus.

3. Wählen Sie Ihr Speicherkonto aus der Liste aus.

4. Wählen Sie den **uploadturbofanfiles**-Container aus, und klicken Sie auf **Auswählen**.

5. Wählen Sie **Speichern** aus. Das Portal benachrichtigt Sie, wenn der Speichervorgang abgeschlossen ist.

> [!Note]
> Wir aktivieren die Uploadbenachrichtigung für dieses Tutorial nicht, aber informieren Sie sich unter [Erhalten einer Benachrichtigung zum Dateiupload](../iot-hub/iot-hub-java-java-file-upload.md#receive-a-file-upload-notification) über Details der Durchführung der Benachrichtigung zum Dateiupload.

## <a name="build-publish-and-deploy-modules"></a>Erstellen, Veröffentlichen und Bereitstellen von Modulen

Nachdem wir nun die Konfigurationsänderungen vorgenommen haben, können wir die Images erstellen und in unserer Azure-Containerregistrierung veröffentlichen. Der Buildprozess bestimmt mit der deployment.template.json-Datei, welche Module erstellt werden müssen. Die Einstellungen für jedes Modul einschließlich der Version befinden sich in der module.json-Datei im Ordner „module“. Der Buildprozess führt zunächst einen Docker-Build auf den Dockerfiles aus, der der aktuellen Konfiguration in der Datei „module.json“ entspricht, um ein Image zu erstellen. Dann veröffentlicht er das Image in der Registrierung aus der Datei „module.json“ mit einem Versionstag, das dem in der Datei „module.json“ entspricht. Schließlich erzeugt er ein konfigurationsspezifisches Bereitstellungsmanifest (z.B. „deployment.amd64.json“), das wir auf dem IoT Edge-Gerät bereitstellen. Das IoT Edge-Gerät liest die Informationen aus dem Bereitstellungsmanifest und lädt basierend auf den Anweisungen die Module herunter, konfiguriert die Routen und legt alle gewünschten Eigenschaften fest. Diese Bereitstellungsmethode hat zwei Nebeneffekte, auf die Sie achten müssen:

* **Bereitstellungsverzögerung:** Da die IoT Edge-Runtime die Änderung ihrer gewünschten Eigenschaften erkennen muss, bevor sie mit der Neukonfiguration beginnt, kann es nach der Bereitstellung Ihrer Module einige Zeit dauern, bis die Runtime sie aufnimmt und beginnt, das IoT Edge-Gerät zu aktualisieren.

* **Modulversionen sind wichtig:** Wenn Sie eine neue Version des Containers eines Moduls in Ihrer Containerregistrierung mit den gleichen Versionstags wie das vorherige Modul veröffentlichen, wird die Runtime die neue Version des Moduls nicht herunterladen. Sie führt einen Vergleich des Versionstags des lokalen Images mit dem gewünschten Image aus dem Bereitstellungsmanifest durch. Wenn diese Versionen übereinstimmen, führt die Runtime keine Aktion aus. Aus diesem Grund ist es wichtig, die Version Ihres Moduls immer dann zu inkrementieren, wenn Sie neue Änderungen bereitstellen möchten. Inkrementieren Sie die Version durch Ändern der Eigenschaft **version** unter der Eigenschaft **tag** in der Datei „module.json“ für das Modul, das Sie ändern. Erstellen und veröffentlichen Sie dann das Modul.

    ```json
    {
      "$schema-version": "0.0.1",
      "description": "",
      "image": {
        "repository": "<your registry>.azurecr.io/avrofilewriter",
        "tag": {
          "version": "0.0.1",
          "platforms": {
            "amd64": "./Dockerfile.amd64",
            "amd64.debug": "./Dockerfile.amd64.debug",
            "arm32v7": "./Dockerfile.arm32v7"
          }
        },
        "buildOptions": []
      },
      "language": "python"
    }
    ```

### <a name="build-and-publish"></a>Erstellen und Veröffentlichen

1. Öffnen Sie in Visual Studio Code auf Ihrer Entwicklungs-VM ein Visual Studio Code-Terminalfenster, und melden Sie sich bei Ihrer Containerregistrierung an.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

1. Klicken Sie in Visual Studio Code mit der rechten Maustaste auf die Datei „deployment.template.json“, und wählen Sie **Erstellen und Pushen der IoT Edge-Projektmappe** aus.

### <a name="view-modules-in-the-registry"></a>Anzeigen von Modulen in der Registrierung

Nachdem der Build erfolgreich abgeschlossen wurde, können wir im Azure-Portal unsere veröffentlichten Module überprüfen.

1. Navigieren Sie im Azure-Portal zu Ihrem Azure Machine Learning-Arbeitsbereich, und klicken Sie auf den Hyperlink für **Registrierung**.

    ![Navigieren zur Registrierung vom Machine Learning Service-Arbeitsbereich aus](media/tutorial-machine-learning-edge-06-custom-modules/follow-registry-link.png)

2. Wählen Sie im seitlichen Navigator der Registrierung **Repositorys** aus.

3. Beachten Sie, dass beide von Ihnen erstellte Module, **avrofilewriter** und **turbofanrouter**, als Repositorys angezeigt werden.

4. Wählen Sie **turbofanrouter** aus, und beachten Sie, dass Sie ein Image mit dem Tag 0.0.1-amd64 veröffentlicht haben.

   ![Erste getaggte Version von „turbofanrouter“](media/tutorial-machine-learning-edge-06-custom-modules/tagged-image-turbofanrouter-repo.png)

### <a name="deploy-modules-to-iot-edge-device"></a>Bereitstellen von Modulen für ein IoT Edge-Gerät

Nachdem wir die Module jetzt in unserer Projektmappe erstellt und konfiguriert haben, stellen wir sie für das IoT Edge-Gerät bereit.

1. Klicken Sie in Visual Studio Code mit der rechten Maustaste im config-Ordner auf die **deployment.amd64.json**-Datei.

2. Klicken Sie auf **Bereitstellung für einzelnes Gerät erstellen**.

3. Wählen Sie Ihr IoT Edge-Gerät aus, **aaTurboFanEdgeDevice**.

4. Aktualisieren Sie im Visual Studio Code-Explorer den Azure IoT Hub-Gerätebereich. Es sollte angezeigt werden, dass die drei neuen Module zwar bereitgestellt sind, aber noch nicht ausgeführt werden.

5. Wenn Sie nach einigen Minuten erneut aktualisieren, sehen Sie die Ausführung der Module.

   ![Ansicht der ausgeführten Module in Visual Studio Code](media/tutorial-machine-learning-edge-06-custom-modules/view-running-modules-list.png)

> [!NOTE]
> Es kann einige Minuten dauern, bis die Module starten und einen stabilen Ausführungsstatus erreichen. Während dieser Zeit beobachten Sie möglicherweise, dass Module starten und anhalten, wenn sie versuchen, eine Verbindung mit dem IoT Edge-Hub-Modul herzustellen.

## <a name="diagnosing-failures"></a>Diagnostizieren von Fehlern

In diesem Abschnitt erläutern wir einige Verfahren, zu verstehen, was mit einem Modul oder Modulen nicht funktioniert hat. Häufig ist ein Fehler zuerst anhand des Status in Visual Studio Code erkennbar.

### <a name="identify-failed-modules"></a>Identifizieren fehlerhafter Module

* **Visual Studio Code:** Beachten Sie den Azure IoT Hub-Gerätebereich. Wenn die meisten Module sich in einem Ausführungsstatus befinden, aber eines angehalten ist, müssen Sie dieses angehaltene Modul weiter untersuchen. Wenn sich alle Module für einen langen Zeitraum im Status „Angehalten“ befinden, kann dies auch auf einen Fehler hindeuten.

* **Azure-Portal:** Wenn Sie zu Ihrem IoT-Hub im Portal navigieren und dann die Gerätedetailseite finden (unter IoT Edge unter Ihrem Gerät), könnten Sie feststellen, dass ein Modul einen Fehler oder nie etwas an den IoT-Hub gemeldet hat.

### <a name="diagnosing-from-the-device"></a>Diagnose vom Gerät aus

Durch die Anmeldung beim IoT Edge-Gerät erhalten Sie Zugriff auf viele Informationen über den Status Ihrer Module. Der Hauptmechanismus, den wir verwenden, sind die Docker-Befehle, mit denen wir die Container und Images auf dem Gerät untersuchen können.

1. Listen Sie alle ausgeführten Container auf. Wir erwarten, dass für jedes Modul ein Container mit einem Namen angezeigt wird, der dem Modul entspricht. Außerdem listet dieser Befehl das genaue Image für den Container einschließlich der Version auf, sodass Sie sehen, ob dies mit Ihren Erwartungen übereinstimmt. Sie können auch Images auflisten, indem Sie im Befehl „container“ durch „image“ ersetzen.

   ```bash
   sudo docker container ls
   ```

2. Rufen Sie die Protokolle für einen Container ab. Dieser Befehl gibt alles aus, was im Container in StdErr und StdOut geschrieben wurde. Dieser Befehl kann für Container angewandt werden, die starteten und dann aus irgendeinem Grund beendet wurden. Er ist auch nützlich, um zu verstehen, was mit den edgeAgent- oder edgeHub-Containern passiert ist.

   ```bash
   sudo docker container logs <container name>
   ```

3. Überprüfen Sie einen Container. Dieser Befehl liefert Ihnen zahlreiche Informationen zu dem Image. Die Daten können gefiltert werden, je nachdem, was Sie suchen. Wenn Sie beispielsweise sehen möchten, ob die Bindungen auf dem avroFileWriter korrekt sind, verwenden Sie folgenden Befehl:

   ```bash
   sudo docker container inspect -f "{{ json .Mounts }}" avroFileWriter | python -m json.tool
   ```

4. Stellen Sie eine Verbindung mit einem ausgeführten Container her. Dieser Befehl kann hilfreich sein, wenn Sie den Container während der Ausführung untersuchen möchten:

   ```bash
   sudo docker exec -it avroFileWriter bash
   ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben wir eine IoT Edge-Projektmappe in Visual Studio Code mit drei Modulen, einem Klassifizierer, einem Router und einem File-Writer/-Uploader erstellt. Wir haben die Routen so eingerichtet, dass die Module auf dem Edge-Gerät miteinander kommunizieren können, und haben die Konfiguration des Edge-Geräts geändert und die Dockerfiles aktualisiert, um Abhängigkeiten zu installieren und den Containern der Module Bindungen hinzuzufügen. Als nächstes haben wir die Konfiguration des IoT-Hubs aktualisiert, um unsere Nachrichten typabhängig weiterzuleiten und Dateiuploads zu verarbeiten. Nach Abschluss der Vorbereitungen haben wir die Module für das IoT Edge-Gerät bereitgestellt und sichergestellt, dass die Module korrekt ausgeführt werden.

Weitere Informationen finden Sie auf den folgenden Seiten:

* [Bereitstellen von Modulen und Einrichten von Routen in IoT Edge](module-composition.md)
* [Abfragesyntax für das IoT Hub-Nachrichtenrouting](../iot-hub/iot-hub-devguide-routing-query-syntax.md)
* [Azure IoT Hub-Nachrichtenweiterleitung: jetzt mit Routing nach Nachrichtentext](https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/)
* [Hochladen von Dateien mit IoT Hub](../iot-hub/iot-hub-devguide-file-upload.md)
* [Hochladen von Dateien von Ihrem Gerät in die Cloud mit IoT Hub](../iot-hub/iot-hub-python-python-file-upload.md)

Fahren Sie mit dem nächsten Artikel fort, um mit dem Senden von Daten zu beginnen und Ihre Projektmappe in Aktion zu sehen.

> [!div class="nextstepaction"]
> [Senden von Daten per transparentem Gateway](tutorial-machine-learning-edge-07-send-data-to-hub.md)
