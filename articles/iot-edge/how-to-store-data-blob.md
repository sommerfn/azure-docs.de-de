---
title: Speichern von Blockblobs auf Geräten – Azure IoT Edge | Microsoft-Dokumentation
description: Enthält grundlegende Informationen zu Features für Tiering und Gültigkeitsdauer, zu unterstützten Blobspeichervorgängen und zur Verbindungsherstellung mit Ihrem Blobspeicherkonto.
author: arduppal
manager: mchad
ms.author: arduppal
ms.reviewer: arduppal
ms.date: 06/19/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: c5a27a8016202f7f8c9e256eaf6b3077fbef295b
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414516"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Speichern von Daten am Edge mit Azure Blob Storage in IoT Edge (Vorschau)

Mit Azure Blob Storage in IoT Edge erhalten Sie eine [Blockblob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs)-Speicherlösung im Edgebereich. Ein Blob Storage-Modul auf Ihrem IoT Edge-Gerät verhält sich wie ein Azure-Blockblob-Dienst – mit dem einzigen Unterschied, dass die Blockblobs lokal auf Ihrem IoT Edge-Gerät gespeichert werden. Sie können mit den gleichen Azure Storage SDK-Methoden oder Blockblob-API-Aufrufen auf Ihre Blobs zugreifen, mit denen Sie bereits arbeiten. In diesem Artikel werden die Konzepte zu Azure Blob Storage auf einem IoT Edge-Container erläutert, der einen Blobdienst auf Ihrem IoT Edge-Gerät ausführt.

Dieses Modul ist hilfreich in Szenarien, in denen Daten lokal gespeichert werden müssen, bis sie verarbeitet oder in die Cloud übertragen werden können. Bei diesen Daten kann es sich um Videos, Bilder, Finanzdaten, Krankenhausdaten oder andere unstrukturierte Daten handeln.

> [!NOTE]
> Azure Blob Storage in IoT Edge ist als [öffentliche Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) verfügbar.

Das folgende Video enthält eine kurze Einführung:
> [!VIDEO https://www.youtube.com/embed/QhCYCvu3tiM]

Dieses Modul gehört zum Umfang der Features **deviceToCloudUpload** und **deviceAutoDelete**.

**deviceToCloudUpload** ist eine konfigurierbare Funktion. Diese Funktion lädt die Daten aus Ihrem lokalen Blobspeicher automatisch in Azure hoch und unterstützt sporadische Internetkonnektivität. Die Funktion ermöglicht Folgendes:

- AKTIVIEREN/DEAKTIVIEREN des Features deviceToCloudUpload.
- Auswählen der Reihenfolge, in der die Daten in Azure kopiert werden sollen (beispielsweise „NewestFirst“ oder „OldestFirst“).
- Angeben des Azure Storage-Kontos, in das Ihre Daten hochgeladen werden sollen
- Angeben der Container, die Sie in Azure hochladen möchten. Mit diesem Modul können Sie sowohl Quell- als auch Zielcontainernamen angeben.
- Auswählen der Möglichkeit, die Blobs sofort nach dem Hochladen in den Cloudspeicher zu löschen
- Führen Sie einen vollständigen Blobupload (mithilfe des Vorgangs `Put Blob`) sowie Upload auf Blockebene (mithilfe der Vorgänge `Put Block` und `Put Block List`) durch.

Dieses Modul verwendet den Upload auf Blockebene, wenn Ihr Blob aus Blöcken besteht. Im Anschluss finden Sie einige gängige Szenarien:

- Wenn Ihre Anwendung einige Blöcke eines zuvor hochgeladenen Blobs aktualisiert, lädt dieses Modul nicht das gesamte Blob hoch, sondern nur die aktualisierten Blöcke.
- Falls beim Hochladen eines Blobs die Internetverbindung unterbrochen wird, lädt das Modul ebenfalls nur die restlichen Blöcke und nicht das gesamte Blob hoch, nachdem die Verbindung wiederhergestellt wurde.

Im Falle einer unerwarteten Prozessbeendigung während eines Blob-Uploads (etwa durch einen Stromausfall) werden alle Blöcke, die für den Upload vorgesehen waren, erneut hochgeladen, sobald das Modul wieder online ist.

**deviceAutoDelete** ist eine konfigurierbare Funktion. Diese Funktion löscht Ihre Blobs automatisch aus dem lokalen Speicher, wenn die angegebene Zeit (in Minuten) abläuft. Die Funktion ermöglicht Folgendes:

- AKTIVIEREN/DEAKTIVIEREN des Features deviceAutoDelete.
- Angeben der Zeit in Minuten (deleteAfterMinutes), nach der die Blobs automatisch gelöscht werden.
- Auswählen der Möglichkeit, das Blob während des Hochladens beizubehalten, wenn der deleteAfterMinutes-Wert abläuft.


## <a name="prerequisites"></a>Voraussetzungen

Ein Azure IoT Edge-Gerät:

- Sie können Ihren Entwicklungscomputer oder einen virtuellen Computer als IoT Edge-Gerät verwenden, indem Sie die Schritte ausführen, die in der Schnellstartanleitung für [Linux](quickstart-linux.md)- oder [Windows](quickstart.md)-Geräte beschrieben sind.

- Azure Blob Storage im IoT Edge-Modul unterstützt die folgenden Gerätekonfigurationen:

  | Betriebssystem | AMD64 | ARM32v7 | ARM64 |
  | ---------------- | ----- | ----- | ---- |
  | Raspbian-stretch | Nein | Ja | Nein |  
  | Ubuntu Server 16.04 | Ja | Nein | Ja |
  | Ubuntu Server 18.04 | Ja | Nein | Ja |
  | Windows 10 IoT Enterprise, Build 17763 | Ja | Nein | Nein |
  | Windows Server 2019, Build 17763 | Ja | Nein | Nein |
  

Cloudressourcen:

Ein [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) mit Standardtarif in Azure.

## <a name="devicetocloudupload-and-deviceautodelete-properties"></a>Eigenschaften deviceToCloudUpload und deviceAutoDelete

Verwenden Sie die gewünschten Eigenschaften des Moduls, um **deviceToCloudUploadProperties** und **deviceAutoDeleteProperties** festzulegen. Gewünschte Eigenschaften können im Rahmen der Bereitstellung festgelegt oder nachträglich durch Bearbeitung des Modulzwillings ohne erneute Bereitstellung geändert werden. Es empfiehlt sich, den Modulzwilling für `reported configuration` und `configurationValidation` zu überprüfen, um sicherzustellen, dass die Werte korrekt weitergegeben werden.

### <a name="devicetoclouduploadproperties"></a>deviceToCloudUploadProperties

Der Name dieser Einstellung lautet `deviceToCloudUploadProperties`.

| Feld | Mögliche Werte | Erklärung | Umgebungsvariable |
| ----- | ----- | ---- | ---- |
| uploadOn | true, false | Standardmäßig auf `false` festgelegt. Wenn Sie das Feature aktivieren möchten, legen Sie dieses Feld auf `true` fest. | `deviceToCloudUploadProperties__uploadOn={false,true}` |
| uploadOrder | NewestFirst, OldestFirst | Ermöglicht das Auswählen der Reihenfolge, in der die Daten in Azure kopiert werden. Standardmäßig auf `OldestFirst` festgelegt. Die Reihenfolge richtet sich nach dem Zeitpunkt der letzten Änderung des Blobs. | `deviceToCloudUploadProperties__uploadOrder={NewestFirst,OldestFirst}` |
| cloudStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"` ist eine Verbindungszeichenfolge, die es ermöglicht, das Azure Storage-Konto anzugeben, in das Ihre Daten hochgeladen werden sollen. Geben Sie `Azure Storage Account Name`, `Azure Storage Account Key`, `End point suffix` an. Fügen Sie ein geeignetes Endpunktsuffix (EndpointSuffix) von Azure für das Ziel des Datenuploads hinzu (variiert für Azure global, Azure Government und Microsoft Azure Stack). | `deviceToCloudUploadProperties__cloudStorageConnectionString=<connection string>` |
| storageContainersForUpload | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Ermöglicht das Angeben der Containernamen, die Sie in Azure hochladen möchten. Mit diesem Modul können Sie sowohl Quell- als auch Zielcontainernamen angeben. Falls Sie keinen Zielcontainernamen angeben, wird der Containername automatisch wie folgt zugewiesen: `<IoTHubName>-<IotEdgeDeviceID>-<ModuleName>-<SourceContainerName>`. Sie können Vorlagenzeichenfolgen für den Zielcontainernamen erstellen. Weitere Informationen finden Sie in der Spalte mit den möglichen Werten. <br>* %h -> IoT Hub-Name (drei bis 50 Zeichen) <br>* %d -> IoT Edge-Geräte-ID (1 bis 129 Zeichen) <br>* %m -> Modulname (ein bis 64 Zeichen) <br>* %c -> Quellcontainername (drei bis 63 Zeichen) <br><br>Der Containername darf maximal 63 Zeichen lang sein. Bei automatischer Zuweisung des Zielcontainernamens gilt Folgendes: Ist der Name länger als 63 Zeichen, werden die einzelnen Bestandteile (IoTHubName, IotEdgeDeviceID, ModuleName, SourceContainerName) jeweils auf 15 Zeichen gekürzt. | `deviceToCloudUploadProperties__storageContainersForUpload__<sourceName>__target: <targetName>` |
| deleteAfterUpload | true, false | Standardmäßig auf `false` festgelegt. Bei Festlegung auf `true` werden die Daten automatisch gelöscht, wenn der Upload in den Cloudspeicher abgeschlossen ist | `deviceToCloudUploadProperties__deleteAfterUpload={false,true}` |


### <a name="deviceautodeleteproperties"></a>deviceAutoDeleteProperties

Der Name dieser Einstellung lautet `deviceAutoDeleteProperties`.

| Feld | Mögliche Werte | Erklärung | Umgebungsvariable |
| ----- | ----- | ---- | ---- |
| deleteOn | true, false | Standardmäßig auf `false` festgelegt. Wenn Sie das Feature aktivieren möchten, legen Sie dieses Feld auf `true` fest. | `deviceAutoDeleteProperties__deleteOn={false,true}` |
| deleteAfterMinutes | `<minutes>` | Geben Sie die Zeit in Minuten an. Nach Ablauf dieser Zeit löscht das Modul Ihre Blobs automatisch aus dem lokalen Speicher. | `deviceAutoDeleteProperties__ deleteAfterMinutes=<minutes>` |
| retainWhileUploading | true, false | Die standardmäßige Einstellung ist `true`, und das Blob wird während des Uploads in den Cloudspeicher beibehalten, wenn deleteAfterMinutes abläuft. Sie können `false` festlegen, sodass die Daten gelöscht werden, sobald deleteAfterMinutes abgelaufen ist. Hinweis: Damit diese Eigenschaft funktioniert, setzen Sie uploadOn auf „true“.| `deviceAutoDeleteProperties__retainWhileUploading={false,true}` |

## <a name="configure-log-files"></a>Konfigurieren von Protokolldateien

Informationen zur Konfiguration von Protokolldateien für Ihr Modul finden Sie unter den [bewährten Methoden für die Produktion](https://docs.microsoft.com/azure/iot-edge/production-checklist#set-up-logs-and-diagnostics).

## <a name="connect-to-your-blob-storage-module"></a>Herstellen einer Verbindung mit Ihrem Blob Storage-Modul

Sie können mit dem Kontonamen und dem Kontoschlüssel, die Sie für Ihr Modul konfiguriert haben, auf Blob Storage auf Ihrem IoT Edge-Gerät zugreifen.

Geben Sie Ihr IoT Edge-Gerät als Blobendpunkt für Speicheranforderungen an, die Sie vornehmen. Mithilfe der IoT Edge-Geräteinformationen und dem Kontonamen, den Sie konfiguriert haben, können Sie eine [Verbindungszeichenfolge für einen bestimmten Speicherendpunkt erstellen](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint).

- Bei Modulen, die auf demselben Gerät bereitgestellt werden, auf dem auch das Modul „Azure Blob Storage auf IoT Edge“ ausgeführt wird, lautet der Blobendpunkt wie folgt: `http://<module name>:11002/<account name>`.
- Bei Modulen oder Anwendungen, die auf einem anderen Gerät ausgeführt werden, müssen Sie den richtigen Endpunkt für Ihr Netzwerk auswählen. Wählen Sie abhängig von Ihrer Netzwerkeinrichtung ein geeignetes Endpunktformat aus, damit der Datenverkehr von Ihrem externen Modul oder Ihrer externen Anwendung das Gerät erreichen kann, auf dem das Modul „Azure Blob Storage auf IoT Edge“ ausgeführt wird. In diesem Szenario wird einer der folgenden Blob-Endpunkte verwendet:
  - `http://<device IP >:11002/<account name>`
  - `http://<IoT Edge device hostname>:11002/<account name>`
  - `http://<fully qualified domain name>:11002/<account name>`

## <a name="azure-blob-storage-quickstart-samples"></a>Schnellstartbeispiele für Azure Blob Storage

Die Dokumentation zu Azure Blob Storage enthält Schnellstart-Beispielcode in mehreren Sprachen. Sie können diese Beispiele zum Testen von Azure Blob Storage in IoT Edge herunterladen, indem Sie den Blobendpunkt dahingehend ändern, dass er eine Verbindung mit Ihrem lokalen Blobspeichermodul herstellt.

Die folgenden Schnellstartbeispiele verwenden Sprachen, die auch von IoT Edge unterstützt werden, sodass Sie sie als IoT Edge-Module zusammen mit dem Blobspeichermodul bereitstellen können:

- [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
- [Java](../storage/blobs/storage-quickstart-blobs-java.md)
- [Python](../storage/blobs/storage-quickstart-blobs-python.md)
- [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="connect-to-your-local-storage-with-azure-storage-explorer"></a>Herstellen einer Verbindung mit Ihrem lokalen Speicher mit Azure Storage-Explorer

Sie können [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) verwenden, um eine Verbindung mit Ihrem lokalen Speicherkonto herzustellen.

1. Laden Sie den Azure Storage-Explorer herunter, und installieren Sie ihn.

1. Stellen Sie mithilfe einer Verbindungszeichenfolge eine Verbindung mit Azure Storage her.

1. Geben Sie die Verbindungszeichenfolge an: `DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`

1. Führen Sie die Schritte für die Verbindungsherstellung aus.

1. Erstellen Sie einen Container innerhalb Ihres lokalen Speicherkontos.

1. Beginnen Sie damit, Dateien als Blockblobs hochzuladen.
   > [!NOTE]
   > Dieses Modul unterstützt keine Seitenblobs.

1. Auf Wunsch können Sie Ihre Azure-Speicherkonten auch im Storage-Explorer verbinden. Mit dieser Konfiguration erhalten Sie eine zentrale Ansicht, die sowohl Ihr lokales Speicherkonto als auch Ihr Azure-Speicherkonto enthält.

## <a name="supported-storage-operations"></a>Unterstützte Speichervorgänge

Blob Storage-Module in IoT Edge verwenden die Azure Storage SDKs und entsprechen der Version „2017-04-17“ der Azure Storage-API für Blockblob-Endpunkte. 

Da nicht alle Azure Blob Storage-Vorgänge von Azure Blob Storage auf IoT Edge unterstützt werden, ist in diesem Abschnitt der jeweilige Status angegeben.

### <a name="account"></a>Konto

Unterstützt:

- Auflisten von Containern

Nicht unterstützt:

- Abrufen und Festlegen von Blobdiensteigenschaften
- Preflightüberprüfung von Blobanforderungen
- Abrufen von Statistiken zum Blobdienst
- Abrufen von Kontoinformationen

### <a name="containers"></a>Container

Unterstützt:

- Erstellen und Löschen von Containern
- Abrufen von Containereigenschaften und -metadaten
- Auflisten von Blobs
- Abrufen und Festlegen von Container-ACLs
- Festlegen von Containermetadaten

Nicht unterstützt:

- Abrufen der Lease von Containern

### <a name="blobs"></a>Blobs (in englischer Sprache)

Unterstützt:

- Festlegen, Abrufen und Löschen von Blobs
- Abrufen und Festlegen von Blobeigenschaften
- Abrufen und Festlegen von Blobmetadaten

Nicht unterstützt:

- Abrufen der Lease von Blobs
- Erstellen einer Momentaufnahme für Blobs
- Kopieren von Blobs und Abbrechen eines Blobkopiervorgangs
- Wiederherstellen von Blobs
- Festlegen des Blobtarifs

### <a name="block-blobs"></a>Blockblobs

Unterstützt:

- Festlegen von Blocks
- Festlegen und Abrufen von Blocklisten

Nicht unterstützt:

- Festlegen von Blocks über die URL

## <a name="release-notes"></a>Versionsinformationen

Dies sind die [Versionsinformationen im Docker-Hub](https://hub.docker.com/_/microsoft-azure-blob-storage) für dieses Modul.

## <a name="feedback"></a>Feedback

Ihr Feedback ist uns wichtig und trägt dazu bei, den Nutzen und die Benutzerfreundlichkeit dieses Moduls und seiner Features zu verbessern. Übermitteln Sie uns Ihr Feedback, und lassen Sie uns wissen, was wir verbessern können.

Sie erreichen uns unter absiotfeedback@microsoft.com.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über das [Bereitstellen von Azure Blob Storage auf IoT Edge](how-to-deploy-blob.md).
