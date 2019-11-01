---
title: 'Bereitstellen des Azure Blob Storage-Moduls für Geräte: Azure IoT Edge | Microsoft-Dokumentation'
description: Stellen Sie ein Azure Blob Storage-Modul für Ihr IoT Edge-Gerät bereit, um Daten am Edge zu speichern.
author: arduppal
ms.author: arduppal
ms.date: 08/07/2019
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.reviewer: arduppal
manager: mchad
ms.openlocfilehash: 603c407f93294f2c9697dcee4c3d5d829033d5d9
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73176961"
---
# <a name="deploy-the-azure-blob-storage-on-iot-edge-module-to-your-device"></a>Bereitstellen des Moduls „Azure Blob Storage auf IoT Edge“ auf Ihrem Gerät

Es gibt mehrere Möglichkeiten, wie Sie Module auf einem IoT Edge-Gerät bereitstellen können. Alle sind für Azure Blob Storage auf IoT Edge-Module geeignet. Die beiden einfachsten Methoden stellen die Verwendung des Azure-Portals oder von Visual Studio Code-Vorlagen dar.

## <a name="prerequisites"></a>Voraussetzungen

- Ein [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) in Ihrem Azure-Abonnement.
- Ein [IoT Edge-Gerät](how-to-register-device.md) mit installierter IoT Edge-Runtime.
- [Visual Studio Code](https://code.visualstudio.com/) und die [Azure IoT Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools), wenn Sie die Bereitstellung über Visual Studio Code durchführen.

## <a name="deploy-from-the-azure-portal"></a>Bereitstellen über das Azure-Portal

Im Azure-Portal werden Sie durch das Erstellen eines Bereitstellungsmanifests und die Übertragung der Bereitstellung auf ein IoT Edge-Gerät per Pushvorgang geführt.

### <a name="select-your-device"></a>Auswählen Ihres Geräts

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem IoT Hub.
1. Wählen Sie im Menü **IoT Edge** aus.
1. Klicken Sie in der Liste der Geräte auf die ID des Zielgeräts.
1. Wählen Sie **Module festlegen** aus.

### <a name="configure-a-deployment-manifest"></a>Konfigurieren eines Bereitstellungsmanifests

Ein Bereitstellungsmanifest ist ein JSON-Dokument, das beschreibt, welche Module bereitgestellt werden sollen, wie Daten zwischen den Modulen übermittelt werden und welche Eigenschaften die Modulzwillinge aufweisen sollen. Das Azure-Portal verfügt über einen Assistenten, der Sie durch die Erstellung eines Bereitstellungsmanifests führt, damit Sie das JSON-Dokument nicht manuell erstellen müssen. Der Assistent umfasst drei Schritte: **Hinzufügen von Modulen**, **Angeben von Routen** und **Überprüfen der Bereitstellung**.

#### <a name="add-modules"></a>Hinzufügen von Modulen

1. Wählen Sie im Abschnitt **Bereitstellungsmodule** der Seite die Option **Hinzufügen** aus.

1. Wählen Sie in der Dropdownliste mit den Modultypen den Eintrag **IoT Edge-Modul** aus.

1. Geben Sie einen Namen für das Modul und dann das Containerimage an:

   - **Name**: azureblobstorageoniotedge
   - **Image-URI**: mcr.microsoft.com/azure-blob-storage:latest

   > [!IMPORTANT]
   > Bei Modulaufrufen für Azure IoT Edge muss die Groß-/Kleinschreibung beachtet werden, und auch für das Storage SDK werden standardmäßig Kleinbuchstaben verwendet. Der Name des Moduls im [Azure Marketplace](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace) lautet **AzureBlobStorageonIoTEdge**. Indem Sie den Namen aber ändern und Kleinbuchstaben verwenden, können Sie sicherstellen, dass Ihre Verbindungen mit dem Modul „Azure Blob Storage auf IoT Edge“ nicht unterbrochen werden.

1. Mit den Standardwerten für **Optionen für Containererstellung** werden zwar die Portbindungen definiert, die Ihr Container benötigt, aber Sie müssen auch Informationen zu Ihrem Speicherkonto sowie eine Bereitstellung für den Speicher auf Ihrem Gerät hinzufügen. Ersetzen Sie den JSON-Standardcode im Portal durch den folgenden JSON-Code:

   ```json
   {
     "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
     ],
     "HostConfig":{
       "Binds":[
           "<storage mount>"
       ],
       "PortBindings":{
         "11002/tcp":[{"HostPort":"11002"}]
       }
     }
   }
   ```

1. Aktualisieren Sie den kopierten JSON-Code mit folgenden Informationen:

   - Ersetzen Sie `<your storage account name>` durch einen leicht zu merkenden Namen. Kontonamen sollten 3 bis 24 Zeichen lang sein und nur Kleinbuchstaben und Zahlen enthalten. Leerzeichen sind nicht zulässig.

   - Ersetzen Sie `<your storage account key>` durch einen Base64-Schlüssel mit 64 Bytes. Sie können einen Schlüssel mit Tools wie [GeneratePlus](https://generate.plus/en/base64) generieren. Mit diesen Anmeldeinformationen greifen Sie über andere Module auf Blob Storage zu.

   - Ersetzen Sie `<storage mount>` gemäß Ihrem Containerbetriebssystem. Geben Sie den Namen eines [Volumes](https://docs.docker.com/storage/volumes/) oder den absoluten Pfad zu einem Verzeichnis auf Ihrem IoT Edge-Gerät an, auf bzw. unter dem das Blobmodul Daten speichern soll. Die Speicherbereitstellung ordnet einen Ort auf Ihrem Gerät einem festen Ort im Modul zu.

     - Für Linux-Container lautet das Format *\<Speicherpfad oder Volume>:/blobroot*. Beispiel:
         - Verwenden einer [Volumebereitstellung](https://docs.docker.com/storage/volumes/): **my-volume:/blobroot**. 
         - Verwenden einer [Bindungsbereitstellung](https://docs.docker.com/storage/bind-mounts/): **/srv/containerdata:/blobroot**. Führen Sie unbedingt die Schritte zum [Gewähren des Verzeichniszugriffs für den Containerbenutzer](how-to-store-data-blob.md#granting-directory-access-to-container-user-on-linux) aus.
     - Für Windows-Container lautet das Format *\<Speicherpfad oder Volume>:C:/BlobRoot*. Beispiel:
         - Verwenden einer [Volumebereitstellung](https://docs.docker.com/storage/volumes/): **my-volume:C:/blobroot**. 
         - Verwenden einer [Bindungsbereitstellung](https://docs.docker.com/storage/bind-mounts/): **C:/ContainerData:C:/BlobRoot**.
         - Sie können Ihre SMB-Netzwerkadresse zuordnen, statt das lokale Laufwerk zu verwenden. Weitere Informationen hierzu finden Sie unter [Speichern von Daten am Edge mit Azure Blob Storage in IoT Edge (Vorschau)](how-to-store-data-blob.md#using-smb-share-as-your-local-storage).

     > [!IMPORTANT]
     > Die zweite Hälfte des Werts für die Speicherbereitstellung verweist auf einen bestimmten Ort im Modul und darf nicht geändert werden. Die Speicherbereitstellung muss immer auf **:/blobroot** (Linux-Container) bzw. auf **:C:/BlobRoot** (Windows-Container) enden.

1. Legen Sie [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) und [deviceAutoDeleteProperties](how-to-store-data-blob.md#deviceautodeleteproperties) für das Modul fest, indem Sie den folgenden JSON-Code kopieren und in das Feld **Set module twin's desired properties** (Gewünschte Eigenschaften für Modulzwilling festlegen) einfügen. Konfigurieren Sie die jeweilige Eigenschaft mit einem entsprechenden Wert, speichern Sie, und fahren Sie mit der Bereitstellung fort. Wenn Sie den IoT Edge-Simulator verwenden, legen Sie die Werte auf die zugehörigen Umgebungsvariablen für diese Eigenschaften fest, die Sie im Abschnitt „Erläuterung“ von [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) und [deviceAutoDeleteProperties](how-to-store-data-blob.md#deviceautodeleteproperties) finden können.

   ```json
   {
     "properties.desired": {
       "deviceAutoDeleteProperties": {
         "deleteOn": <true, false>,
         "deleteAfterMinutes": <timeToLiveInMinutes>,
         "retainWhileUploading":<true,false>
       },
       "deviceToCloudUploadProperties": {
         "uploadOn": <true, false>,
         "uploadOrder": "<NewestFirst, OldestFirst>",
         "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>; EndpointSuffix=<your end point suffix>",
         "storageContainersForUpload": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         },
         "deleteAfterUpload":<true,false>
       }
     }
   }

      ```

   ![Festlegen der Optionen für die Containererstellung sowie der Eigenschaften „deviceAutoDeleteProperties“ und „deviceToCloudUploadProperties“](./media/how-to-deploy-blob/iotedge-custom-module.png)

   Informationen zum Konfigurieren von „deviceToCloudUploadProperties“ und „deviceAutoDeleteProperties“ nach Bereitstellung Ihres Moduls finden Sie unter [Edit the Module Twin](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin) (Bearbeiten des Modulzwillings). Weitere Informationen zu gewünschten Eigenschaften finden Sie unter [Definieren oder Aktualisieren gewünschter Eigenschaften](module-composition.md#define-or-update-desired-properties).

1. Wählen Sie **Speichern** aus.

1. Klicken Sie auf **Weiter**, um mit dem Abschnitt über Routen fortzufahren.

#### <a name="specify-routes"></a>Angeben von Routen

Behalten Sie Standardrouten bei, und wählen Sie **Weiter**, um mit dem Abschnitt für die Überprüfung fortzufahren.

#### <a name="review-deployment"></a>Überprüfen der Bereitstellung

Im Abschnitt zur Überprüfung wird das JSON-Bereitstellungsmanifest angezeigt, das anhand Ihrer Auswahl in den vorherigen beiden Abschnitten erstellt wurde. Es wurden auch zwei Module deklariert, die Sie nicht hinzugefügt haben: **$edgeAgent** und **$edgeHub**. Diese beiden Module bilden die [IoT Edge-Runtime](iot-edge-runtime.md) und sind in jeder Bereitstellung erforderliche Standardvorgaben.

Überprüfen Sie die Bereitstellungsinformationen, und klicken Sie dann auf **Senden**.

### <a name="verify-your-deployment"></a>Überprüfen Ihrer Bereitstellung

Nach Übermittlung der Bereitstellung wird wieder die Seite **IoT Edge** Ihres IoT-Hubs angezeigt.

1. Wählen Sie das IoT Edge-Zielgerät für die Bereitstellung aus, um dessen Details zu öffnen.
1. Vergewissern Sie sich anhand der Gerätedetails, dass das Blob Storage-Modul als **In Bereitstellung angegeben** und als **Vom Gerät gemeldet** aufgeführt ist.

Unter Umständen dauert es einen Moment, bis das Modul auf dem Gerät gestartet und an IoT Hub gemeldet wurde. Aktualisieren Sie die Seite, um einen aktualisierten Status anzuzeigen.

## <a name="deploy-from-visual-studio-code"></a>Bereitstellen über Visual Studio Code

Azure IoT Edge bietet Vorlagen in Visual Studio Code, mit denen Sie Edgelösungen entwickeln können. Führen Sie die folgenden Schritte aus, um eine neue IoT Edge-Lösung mit einem Blob Storage-Modul zu erstellen und das Bereitstellungsmanifest zu konfigurieren.

1. Wählen Sie **Ansicht** > **Befehlspalette** aus.

1. Geben Sie in der Befehlspalette den folgenden Befehl ein, und führen Sie ihn aus: **Azure IoT Edge: New IoT Edge Solution** (Azure IoT Edge: Neue IoT Edge-Projektmappe) ein, und führen Sie ihn aus.

   ![Ausführen einer neuen IoT Edge-Projektmappe](./media/how-to-develop-csharp-module/new-solution.png)

   Folgen Sie den Anweisungen in der Befehlspalette, um Ihre Projektmappe zu erstellen.

   | Feld | Wert |
   | ----- | ----- |
   | Ordner auswählen | Wählen Sie den Speicherort auf Ihrem Entwicklungscomputer aus, an dem Visual Studio Code die Projektmappendateien erstellen soll. |
   | Provide a solution name (Projektmappennamen angeben) | Geben Sie für Ihre Projektmappe einen aussagekräftigen Namen ein, oder übernehmen Sie den Standardnamen **EdgeSolution**. |
   | Select module template (Modulvorlage auswählen) | Wählen Sie die Option **Vorhandenes Modul (vollständige Image-URL angeben)** aus. |
   | Provide a module name (Modulname angeben) | Geben Sie einen klein geschriebenen Namen für das Modul ein (beispielsweise **azureblobstorageoniotedge**).<br /><br />Es ist wichtig, für das Modul „Azure Blob Storage auf IoT Edge“ einen Namen in Kleinbuchstaben zu verwenden. IoT Edge beachtet bei Modulverweisen die Groß-/Kleinschreibung, und das Storage SDK verwendet standardmäßig Kleinbuchstaben. |
   | Provide Docker image for the module (Docker-Image für das Modul angeben) | Geben Sie den Image-URI an: **mcr.microsoft.com/azure-blob-storage:latest** |

   Visual Studio Code verwendet die angegebenen Informationen, erstellt eine IoT Edge-Projektmappe, und lädt diese in einem neuen Fenster. Die Lösungsvorlage erstellt eine Bereitstellungsmanifestvorlage, die Ihr Blob Storage-Modulimage enthält, allerdings müssen Sie die Erstellungsoptionen des Moduls konfigurieren.

1. Öffnen Sie *deployment.template.json* in Ihrem neuen Projektmappenarbeitsbereich, und machen Sie den Abschnitt **Module** ausfindig. Nehmen Sie folgende Konfigurationsänderungen vor:

   1. Löschen Sie das Modul **SimulatedTemperatureSensor**, da es für diese Bereitstellung nicht erforderlich ist.

   1. Kopieren Sie den folgenden Code, und fügen Sie ihn in das Feld `createOptions` ein:

      ```json
      "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
      ],
      "HostConfig":{
        "Binds": ["<storage mount>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Aktualisieren des Moduls „createOptions“: Visual Studio Code](./media/how-to-deploy-blob/create-options.png)

1. Ersetzen Sie `<your storage account name>` durch einen leicht zu merkenden Namen. Kontonamen sollten 3 bis 24 Zeichen lang sein und nur Kleinbuchstaben und Zahlen enthalten. Leerzeichen sind nicht zulässig.

1. Ersetzen Sie `<your storage account key>` durch einen Base64-Schlüssel mit 64 Bytes. Sie können einen Schlüssel mit Tools wie [GeneratePlus](https://generate.plus/en/base64) generieren. Mit diesen Anmeldeinformationen greifen Sie über andere Module auf Blob Storage zu.

1. Ersetzen Sie `<storage mount>` gemäß Ihrem Containerbetriebssystem. Geben Sie den Namen eines [Volumes](https://docs.docker.com/storage/volumes/) oder den absoluten Pfad zu einem Verzeichnis auf Ihrem IoT Edge-Gerät an, auf bzw. unter dem das Blobmodul Daten speichern soll. Die Speicherbereitstellung ordnet einen Ort auf Ihrem Gerät einem festen Ort im Modul zu.  

      
     - Für Linux-Container lautet das Format *\<Speicherpfad oder Volume>:/blobroot*. Beispiel:
         - Verwenden einer [Volumebereitstellung](https://docs.docker.com/storage/volumes/): **my-volume:/blobroot**. 
         - Verwenden einer [Bindungsbereitstellung](https://docs.docker.com/storage/bind-mounts/): **/srv/containerdata:/blobroot**. Führen Sie unbedingt die Schritte zum [Gewähren des Verzeichniszugriffs für den Containerbenutzer](how-to-store-data-blob.md#granting-directory-access-to-container-user-on-linux) aus.
     - Für Windows-Container lautet das Format *\<Speicherpfad oder Volume>:C:/BlobRoot*. Beispiel:
         - Verwenden einer [Volumebereitstellung](https://docs.docker.com/storage/volumes/): **my-volume:C:/blobroot**. 
         - Verwenden einer [Bindungsbereitstellung](https://docs.docker.com/storage/bind-mounts/): **C:/ContainerData:C:/BlobRoot**.
         - Sie können Ihre SMB-Netzwerkadresse zuordnen, statt das lokale Laufwerk zu verwenden. Weitere Informationen hierzu finden Sie unter [Speichern von Daten am Edge mit Azure Blob Storage in IoT Edge (Vorschau)](how-to-store-data-blob.md#using-smb-share-as-your-local-storage).

     > [!IMPORTANT]
     > Die zweite Hälfte des Werts für die Speicherbereitstellung verweist auf einen bestimmten Ort im Modul und darf nicht geändert werden. Die Speicherbereitstellung muss immer auf **:/blobroot** (Linux-Container) bzw. auf **:C:/BlobRoot** (Windows-Container) enden.

1. Konfigurieren Sie [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) und [deviceAutoDeleteProperties](how-to-store-data-blob.md#deviceautodeleteproperties) für Ihr Modul, indem Sie der Datei *deployment.template.json* den folgenden JSON-Code hinzufügen. Konfigurieren Sie die jeweilige Eigenschaft mit einem entsprechenden Wert, und speichern Sie die Datei. Wenn Sie den IoT Edge-Simulator verwenden, legen Sie die Werte auf die zugehörigen Umgebungsvariablen für diese Eigenschaften fest, die Sie im Abschnitt „Erläuterung“ von [deviceToCloudUploadProperties](how-to-store-data-blob.md#devicetoclouduploadproperties) und [deviceAutoDeleteProperties](how-to-store-data-blob.md#deviceautodeleteproperties) finden können.

   ```json
   "<your azureblobstorageoniotedge module name>":{
     "properties.desired": {
       "deviceAutoDeleteProperties": {
         "deleteOn": <true, false>,
         "deleteAfterMinutes": <timeToLiveInMinutes>,
         "retainWhileUploading": <true, false>
       },
       "deviceToCloudUploadProperties": {
         "uploadOn": <true, false>,
         "uploadOrder": "<NewestFirst, OldestFirst>",
         "cloudStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "storageContainersForUpload": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         },
         "deleteAfterUpload": <true, false>
       }
     }
   }
   ```

   ![Festlegen der gewünschten Eigenschaften für azureblobstorageoniotedge: Visual Studio Code](./media/how-to-deploy-blob/devicetocloud-deviceautodelete.png)

   Informationen zum Konfigurieren von „deviceToCloudUploadProperties“ und „deviceAutoDeleteProperties“ nach Bereitstellung Ihres Moduls finden Sie unter [Edit the Module Twin](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin) (Bearbeiten des Modulzwillings). Weitere Informationen zu Containererstellungsoptionen, Neustartrichtlinien und gewünschtem Status finden Sie unter [Gewünschte EdgeAgent-Eigenschaften](module-edgeagent-edgehub.md#edgeagent-desired-properties).

1. Speichern Sie die Datei *deployment.template.json*.

1. Klicken Sie mit der rechten Maustaste auf **deployment.template.json**, und klicken Sie dann auf **IoT Edge-Bereitstellungsmanifest generieren**.

1. Visual Studio Code erstellt anhand der Informationen, die Sie in der Datei *deployment.template.json* angegeben haben, eine neue Bereitstellungsmanifestdatei. Das Bereitstellungsmanifest wird in einem neuen **config**-Ordner in Ihrem Projektmappenarbeitsbereich erstellt. Sobald Sie über diese Datei verfügen, können Sie die Schritte unter [Bereitstellen von Azure IoT Edge-Modulen über Visual Studio Code](how-to-deploy-modules-vscode.md) oder [Bereitstellen von Azure IoT Edge-Modulen mit der Azure CLI](how-to-deploy-modules-cli.md) durchführen.

## <a name="deploy-multiple-module-instances"></a>Bereitstellen mehrerer Modulinstanzen

Wenn Sie mehrere Instanzen des Moduls „Azure Blob Storage auf IoT Edge“ bereitstellen möchten, müssen Sie einen anderen Speicherpfad angeben und den Wert `HostPort` ändern, an den das Modul gebunden wird. Die Blob Storage-Module machen immer Port 11002 im Container verfügbar, aber Sie können deklarieren, an welchen Port dieser auf dem Host gebunden wird.

Bearbeiten Sie das Feld **Optionen für Containererstellung** (im Azure-Portal) oder **createOptions** (in der Datei *deployment.template.json* in Visual Studio Code), um den Wert `HostPort` zu ändern:

```json
"PortBindings":{
  "11002/tcp": [{"HostPort":"<port number>"}]
}
```

Wenn Sie zusätzliche Blob Storage-Module verbinden, ändern Sie den Endpunkt dahingehend, dass er auf den aktualisierten Hostport zeigt.

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich ausführlicher über [Azure Blob Storage in IoT Edge](how-to-store-data-blob.md).

Weitere Informationen zur Funktionsweise und Erstellung von Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können](module-composition.md).
