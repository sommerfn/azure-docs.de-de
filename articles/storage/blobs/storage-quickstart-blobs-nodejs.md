---
title: 'Schnellstart: Azure Blob Storage-Bibliothek v12 – JavaScript'
description: In dieser Schnellstartanleitung erfahren Sie, wie mithilfe der Azure Blob Storage-Clientbibliothek, Version 12 für JavaScript ein Container und ein Blob im Blobspeicher (Objektspeicher) erstellt werden. Als Nächstes erfahren Sie, wie Sie den Blob auf Ihren lokalen Computer herunterladen, und wie Sie alle Blobs in einem Container auflisten.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/05/2019
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.openlocfilehash: 90ef1be4366205b1f655a0b93619a897d6d1f25d
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73929494"
---
# <a name="quickstart-azure-blob-storage-client-library-v12-for-javascript"></a>Schnellstart: Azure Blob Storage-Clientbibliothek v12 für JavaScript

Erfahren Sie etwas über die ersten Schritte mit der Azure Blob Storage-Clientbibliothek v12 für JavaScript. Azure Blob Storage ist die Objektspeicherlösung von Microsoft für die Cloud. Führen Sie die Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben. Blobspeicher ist für die Speicherung großer Mengen unstrukturierter Daten optimiert.

> [!NOTE]
> Informationen zu den ersten Schritten mit der vorherigen SDK-Version finden Sie unter [Schnellstart: Azure Blob Storage-Clientbibliothek für JavaScript](storage-quickstart-blobs-nodejs-v10.md).

Mit der Azure Blob Storage-Clientbibliothek v12 für JavaScript können Sie Folgendes erledigen:

* Erstellen eines Containers
* Hochladen eines Blobs in Azure Storage
* Auflisten aller Blobs in einem Container
* Herunterladen des Blobs auf den lokalen Computer
* Löschen eines Containers

[API-Referenzdokumentation](/javascript/api/@azure/storage-blob) | [Quellcode der Bibliothek](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-blob) | [Paket (Node Package Manager)](https://www.npmjs.com/package/@azure/storage-blob/v/12.0.0) | [Beispiele](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-blob/samples)

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/)
* Azure-Speicherkonto – [Erstellen eines Speicherkontos](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* Aktuelle [Node.js](https://nodejs.org/en/download/) für Ihr Betriebssystem.

## <a name="setting-up"></a>Einrichten

In diesem Abschnitt wird beschrieben, wie ein Projekt zur Verwendung mit der Azure Blob Storage-Clientbibliothek v12 für JavaScript vorbereitet wird.

### <a name="create-the-project"></a>Erstellen des Projekts

Erstellen Sie zunächst eine JavaScript-Anwendung mit dem Namen *blob-quickstart-v12*.

1. Erstellen Sie in einem Konsolenfenster (z. B. cmd, PowerShell oder Bash) ein neues Verzeichnis für das Projekt.

    ```console
    mkdir blob-quickstart-v12
    ```

1. Wechseln Sie zum neu erstellten *blob-quickstart-v12*-Verzeichnis.

    ```console
    cd blob-quickstart-v12
    ```

1. Erstellen Sie eine neue Textdatei mit dem Namen *package.json*. Diese Datei definiert das Node.js-Projekt. Speichern Sie diese Datei im Verzeichnis *blob-quickstart-v12*. Der Inhalt der Datei ist nachstehend aufgeführt:

    ```json
    {
        "name": "blob-quickstart-v12",
        "version": "1.0.0",
        "description": "Use the @azure/storage-blob SDK version 12 to interact with Azure Blob storage",
        "main": "blob-quickstart-v12.js",
        "scripts": {
            "start": "node blob-quickstart-v12.js"
        },
        "author": "Your Name",
        "license": "MIT",
        "dependencies": {
            "@azure/storage-blob": "^12.0.0",
            "@types/dotenv": "^4.0.3",
            "dotenv": "^6.0.0"
        }
    }
    ```
    
    Sie können ggf. Ihren eigenen Namen für das `author`-Feld eingeben.
   
### <a name="install-the-package"></a>Installieren des Pakets

Installieren Sie das Paket mit der Azure Blob Storage-Clientbibliothek für JavaScript im Verzeichnis *blob-quickstart-v12* mit dem Befehl `npm install`. Dieser Befehl lest die Datei *package.json* und installiert das Paket mit der Azure Blob Storage-Clientbibliothek v12 für JavaScript und alle Bibliotheken, von denen die Bibliothek abhängig ist.

```console
npm install
```

### <a name="set-up-the-app-framework"></a>Einrichten des App-Frameworks

Über das Projektverzeichnis:

1. Öffnen einer weiteren neuen Textdatei im Code-Editor
1. Hinzufügen von `require`-Aufrufen zum Laden von Azure- und Node.js-Modulen
1. Erstellen der Struktur für das Programm, einschließlich einer sehr einfachen Ausnahmebehandlung

    Der Code lautet wie folgt:

    ```javascript
    const { BlobServiceClient } = require('@azure/storage-blob');
    const uuidv1 = require('uuid/v1');
    
    async function main() {
        console.log('Azure Blob storage v12 - Javascript quickstart sample');
        // Quick start code goes here
    }
    
    main().then(() => console.log('Done')).catch((ex) => console.log(ex.message));
    ```

1. Speichern Sie die neue Datei als *blob-quickstart-v12.js* im Verzeichnis *blob-quickstart-v12*.

### <a name="copy-your-credentials-from-the-azure-portal"></a>Kopieren Ihrer Anmeldeinformationen aus dem Azure-Portal

Wenn die Beispielanwendung eine Anforderung an Azure Storage sendet, muss diese autorisiert werden. Fügen Sie zum Autorisieren einer Anforderung die Anmeldeinformationen für Ihr Speicherkonto in Form einer Verbindungszeichenfolge hinzu. Führen Sie zum Anzeigen der Anmeldeinformationen Ihres Speicherkontos die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Suchen Sie nach Ihrem Speicherkonto.
3. Wählen Sie im Abschnitt **Einstellungen** der Speicherkontoübersicht die Option **Zugriffsschlüssel**. Hier können Sie Ihre Kontozugriffsschlüssel und die vollständige Verbindungszeichenfolge für jeden Schlüssel anzeigen.
4. Suchen Sie unter **key1** nach dem Wert für die **Verbindungszeichenfolge**, und wählen Sie dann die Schaltfläche **Kopieren**, um die Verbindungszeichenfolge zu kopieren. Der Wert der Verbindungszeichenfolge wird in einem späteren Schritt einer Umgebungsvariablen hinzugefügt.

    ![Screenshot: Kopieren einer Verbindungszeichenfolge aus dem Azure-Portal](../../../includes/media/storage-copy-connection-string-portal/portal-connection-string.png)

### <a name="configure-your-storage-connection-string"></a>Konfigurieren der Speicherverbindungszeichenfolge

Schreiben Sie die Verbindungszeichenfolge nach dem Kopieren in eine neue Umgebungsvariable auf dem lokalen Computer, auf dem die Anwendung ausgeführt wird. Öffnen Sie zum Festlegen der Umgebungsvariablen ein Konsolenfenster, und befolgen Sie die Anleitung für Ihr Betriebssystem. Ersetzen Sie `<yourconnectionstring>` durch Ihre Verbindungszeichenfolge.

#### <a name="windows"></a>Windows

```cmd
setx CONNECT_STR "<yourconnectionstring>"
```

Sie müssen nach dem Hinzufügen der Umgebungsvariablen unter Windows eine neue Instanz des Befehlsfensters öffnen.

#### <a name="linux"></a>Linux

```bash
export CONNECT_STR="<yourconnectionstring>"
```

#### <a name="macos"></a>macOS

```bash
export CONNECT_STR="<yourconnectionstring>"
```

#### <a name="restart-programs"></a>Neustarten von Programmen

Nachdem Sie die Umgebungsvariable hinzugefügt haben, starten Sie alle ausgeführten Programme neu, in denen die Umgebungsvariable gelesen werden muss. Starten Sie beispielsweise die Entwicklungsumgebung oder den Editor neu, bevor Sie fortfahren.

## <a name="object-model"></a>Objektmodell

Azure Blob Storage ist für die Speicherung großer Mengen unstrukturierter Daten optimiert. Unstrukturierte Daten sind Daten, die keinem bestimmten Datenmodell und keiner bestimmten Definition entsprechen (also beispielsweise Text- oder Binärdaten). Blob Storage bietet drei Typen von Ressourcen:

* Das Speicherkonto
* Einen Container im Speicherkonto
* Ein Blob im Container

Im folgenden Diagramm ist die Beziehung zwischen diesen Ressourcen dargestellt.

![Diagramm der Blob Storage-Architektur](./media/storage-blob-introduction/blob1.png)

Verwenden Sie die folgenden JavaScript-Klassen für die Interaktion mit folgenden Ressourcen:

* [BlobServiceClient](/javascript/api/@azure/storage-blob/blobserviceclient): Die `BlobServiceClient`-Klasse ermöglicht Ihnen, Azure Storage-Ressourcen und Blobcontainer zu bearbeiten.
* [ContainerClient:](/javascript/api/@azure/storage-blob/containerclient) Die `ContainerClient`-Klasse ermöglicht Ihnen, Azure Storage-Container und deren Blobs zu bearbeiten.
* [BlobClient:](/javascript/api/@azure/storage-blob/blobclient) Die `BlobClient`-Klasse ermöglicht Ihnen, Azure Storage-Blobs zu bearbeiten.

## <a name="code-examples"></a>Codebeispiele

Diese Beispielcodeausschnitte veranschaulichen, wie folgende Vorgänge mit der Azure Blob Storage-Clientbibliothek für JavaScript durchgeführt werden:

* [Abrufen der Verbindungszeichenfolge](#get-the-connection-string)
* [Container erstellen](#create-a-container)
* [Hochladen von Blobs in einen Container](#upload-blobs-to-a-container)
* [Auflisten der Blobs in einem Container](#list-the-blobs-in-a-container)
* [Herunterladen von Blobs](#download-blobs)
* [Löschen eines Containers](#delete-a-container)

### <a name="get-the-connection-string"></a>Abrufen der Verbindungszeichenfolge

Der folgende Code ruft die Verbindungszeichenfolge für das Speicherkonto aus der Umgebungsvariablen ab, die im Abschnitt [Konfigurieren der Speicherverbindungszeichenfolge](#configure-your-storage-connection-string) erstellt wurde.

Fügen Sie diesen Code in der `main`-Funktion hinzu:

```javascript
// Retrieve the connection string for use with the application. The storage
// connection string is stored in an environment variable on the machine
// running the application called CONNECT_STR. If the environment variable is
// created after the application is launched in a console or with Visual Studio,
// the shell or application needs to be closed and reloaded to take the
// environment variable into account.
const CONNECT_STR = process.env.CONNECT_STR;
```

### <a name="create-a-container"></a>Erstellen eines Containers

Legen Sie einen Namen für den neuen Container fest. Der folgende Code hängt einen UUID-Wert an den Containernamen an, damit dieser eindeutig ist.

> [!IMPORTANT]
> Die Containernamen müssen klein geschrieben werden. Weitere Informationen zum Benennen von Containern und Blobs finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Benennen von Containern, Blobs und Metadaten und Verweisen auf diese).

Erstellen Sie eine Instanz der [BlobServiceClient](/javascript/api/@azure/storage-blob/blobserviceclient)-Klasse, indem Sie die [fromConnectionString](/javascript/api/@azure/storage-blob/blobserviceclient#fromconnectionstring-string--newpipelineoptions-)-Methode aufrufen. Rufen Sie dann die [getContainerClient](/javascript/api/@azure/storage-blob/blobserviceclient#getcontainerclient-string-)-Methode auf, um einen Verweis auf einen Container zu erhalten. Rufen Sie zum schließlich [create](/javascript/api/@azure/storage-blob/containerclient#create-containercreateoptions-) auf, um den Container in Ihrem Speicherkonto zu erstellen.

Fügen Sie diesen Code am Ende der `main`-Funktion hinzu:

```javascript
// Create the BlobServiceClient object which will be used to create a container client
const blobServiceClient = await new BlobServiceClient.fromConnectionString(CONNECT_STR);

// Create a unique name for the container
const containerName = 'quickstart' + uuidv1();

console.log('\nCreating container...');
console.log('\t', containerName);

// Get a reference to a container
const containerClient = await blobServiceClient.getContainerClient(containerName);

// Create the container
await containerClient.create();
```

### <a name="upload-blobs-to-a-container"></a>Hochladen von Blobs in einen Container

Der folgende Codeausschnitt führt folgende Aktionen durch:

1. Erstellen einer Textzeichenfolge, die in ein Blob hochgeladen werden soll.
1. Abrufen eines Verweises auf ein [BlockBlobClient](/javascript/api/@azure/storage-blob/blockblobclient)-Objekt durch Aufrufen der [getBlockBlobClient](/javascript/api/@azure/storage-blob/containerclient#getblockblobclient-string-)-Methode für [ContainerClient](/javascript/api/@azure/storage-blob/containerclient) im Abschnitt [Erstellen eines Container](#create-a-container).
1. Hochladen der Textzeichenfolgedaten in das Blob durch Aufrufen der [upload](/javascript/api/@azure/storage-blob/blockblobclient#upload-httprequestbody--number--blockblobuploadoptions-)-Methode.

Fügen Sie diesen Code am Ende der `main`-Funktion hinzu:

```javascript
// Create a unique name for the blob
const blobName = 'quickstart' + uuidv1() + '.txt';

// Get a block blob client
const blockBlobClient = containerClient.getBlockBlobClient(blobName);

console.log('\nUploading to Azure Storage as blob:\n\t', blobName);

// Upload data to the blob
const data = 'Hello, World!';
await blockBlobClient.upload(data, data.length);
```

### <a name="list-the-blobs-in-a-container"></a>Auflisten der Blobs in einem Container

Listen Sie die Blobs im Container auf, indem Sie die [listBlobsFlat](/javascript/api/@azure/storage-blob/containerclient#listblobsflat-containerlistblobsoptions-)-Methode aufrufen. In diesem Fall wurde dem Container nur ein Blob hinzugefügt, sodass beim Auflisten auch nur ein Blob zurückgegeben wird.

Fügen Sie diesen Code am Ende der `main`-Funktion hinzu:

```javascript
console.log('\nListing blobs...');

// List the blob(s) in the container.
for await (const blob of containerClient.listBlobsFlat()) {
    console.log('\t', blob.name);
}
```

### <a name="download-blobs"></a>Herunterladen von Blobs

Laden Sie das zuvor erstellte Blob herunter, indem Sie die [download](/javascript/api/@azure/storage-blob/blockblobclient#download-undefined---number--undefined---number--blobdownloadoptions-)-Methode aufrufen. Der Beispielcode enthält eine Hilfsfunktion namens `streamToString`, die verwendet wird, um einen lesbaren Node.js-Stream in eine Zeichenfolge einzulesen.

Fügen Sie diesen Code am Ende der `main`-Funktion hinzu:

```javascript
// Get blob content from position 0 to the end
// In Node.js, get downloaded data by accessing downloadBlockBlobResponse.readableStreamBody
// In browsers, get downloaded data by accessing downloadBlockBlobResponse.blobBody
const downloadBlockBlobResponse = await blockBlobClient.download(0);
console.log('\nDownloaded blob content...');
console.log('\t', await streamToString(downloadBlockBlobResponse.readableStreamBody));
```

Fügen Sie diese Hilfsfunktion *hinter* der `main`-Funktion hinzu:

```javascript
// A helper function used to read a Node.js readable stream into a string
async function streamToString(readableStream) {
  return new Promise((resolve, reject) => {
    const chunks = [];
    readableStream.on("data", (data) => {
      chunks.push(data.toString());
    });
    readableStream.on("end", () => {
      resolve(chunks.join(""));
    });
    readableStream.on("error", reject);
  });
}
```

### <a name="delete-a-container"></a>Löschen eines Containers

Im folgenden Code werden die mit der App erstellten Ressourcen bereinigt, indem der gesamte Container mit der [delete](/javascript/api/@azure/storage-blob/containerclient#delete-containerdeletemethodoptions-)-Methode gelöscht wird. Sie können ggf. auch die lokalen Dateien löschen.

Fügen Sie diesen Code am Ende der `main`-Funktion hinzu:

```javascript
console.log('\nDeleting container...');

// Delete container
await containerClient.delete();
```

## <a name="run-the-code"></a>Ausführen des Codes

Diese App erstellt eine Textzeichenfolge und lädt sie in den Blobspeicher hoch. Im Beispiel werden dann die Blobs im Container aufgelistet, das Blob heruntergeladen und die heruntergeladenen Daten angezeigt.

Navigieren Sie in einer Konsoleneingabeaufforderung zu dem Verzeichnis, das die Datei *blob-quickstart-v12.py* enthält, und führen Sie dann den folgenden `node`-Befehl aus, um die App auszuführen.

```console
node blob-quickstart-v12.js
```

Die Ausgabe der App sieht in etwa wie das folgende Beispiel aus:

```output
Azure Blob storage v12 - JavaScript quickstart sample

Creating container...
         quickstart4a0780c0-fb72-11e9-b7b9-b387d3c488da

Uploading to Azure Storage as blob:
         quickstart4a3128d0-fb72-11e9-b7b9-b387d3c488da.txt

Listing blobs...
         quickstart4a3128d0-fb72-11e9-b7b9-b387d3c488da.txt

Downloaded blob content...
         Hello, World!

Deleting container...
Done
```

Durchlaufen Sie den Code Schritt für Schritt im Debugger und überprüfen Sie das Azure-Portal während des gesamten Vorgangs. Stellen Sie fest, ob der Container erstellt wurde. Sie können das Blob im Container öffnen und den Inhalt anzeigen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung wurde beschrieben, wie Sie Blobs mit JavaScript hochladen, herunterladen und auflisten.

Weitere Beispiel-Apps für Blobspeicher finden Sie unter:

> [!div class="nextstepaction"]
> [Azure Blob Storage SDK v12 – JavaScript-Beispiele](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-blob/samples)

* Weitere Informationen finden Sie unter [Azure SDK für JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/storage/storage-blob/README.md).
* Tutorials, Beispiele, Schnellstartanleitungen und weitere Dokumentationen finden Sie in der [Dokumentation für das Azure SDK für JavaScript](/azure/javascript/).
