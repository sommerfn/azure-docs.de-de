---
title: Exportieren von Daten in Azure Blob Storage | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Daten aus Ihrer Azure IoT Central-Anwendung in Azure Blob Storage exportiert werden.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 09/26/2019
ms.topic: conceptual
ms.service: iot-central
manager: corywink
ms.openlocfilehash: 9ac650273a53da715b89e3233b30cf50710cfcfd
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973266"
---
# <a name="export-your-data-to-azure-blob-storage-preview-features"></a>Exportieren von Daten in Azure Blob Storage (Previewfunktion)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

*Dieses Thema gilt für Administratoren.*

In diesem Artikel wird beschrieben, wie Sie in Azure IoT Central das Feature für den fortlaufenden Datenexport verwenden, um Daten regelmäßig in Ihr **Azure Blob-Speicherkonto** oder Ihr **Azure Data Lake Storage Gen2-Speicherkonto** zu exportieren. Sie können **Telemetriedaten**, **Geräte** und **Gerätevorlagen** in Dateien im JSON-Format exportieren. Die exportierten Daten können für die Analyse von kalten Datenpfaden verwendet werden, z.B. Trainingsmodelle in Azure Machine Learning oder die langfristige Trendanalyse in Microsoft Power BI.

> [!Note]
> Wenn Sie den fortlaufenden Datenexport aktivieren, erhalten Sie nur die Daten ab dem jeweiligen Aktivierungszeitpunkt. Derzeit können Daten nicht für Zeiten abgerufen werden, in denen der Datenexport deaktiviert war. Aktivieren Sie den fortlaufenden Datenexport frühzeitig, um umfassendere Verlaufsdaten zu erhalten.


## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen Administrator in Ihrer IoT Central-Anwendung sein.

## <a name="create-storage-account"></a>Speicherkonto erstellen
Wenn Sie keinen vorhandenen Speicher als Exportziel haben, gehen Sie wie folgt vor:

1. Erstellen Sie ein [neues Speicherkonto im Azure-Portal](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM). Sie können sich zum Erstellen neuer [Azure Blob Storage-Konten](https://aka.ms/blobdocscreatestorageaccount) oder neuer [Azure Data Lake Storage v2-Speicherkonten](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-account) genauer informieren.

    > [!Note] 
    > Wenn Sie Daten in ein ADLS v2-Speicherkonto exportieren möchten, müssen Sie in **Kontoart** den Wert **BlobStorage** auswählen. 

    > [!Note] 
    > Sie können Daten in Speicherkonten in anderen Abonnements als demjenigen für Ihre IoT Central-Anwendung mit nutzungsbasierter Zahlung exportieren. In diesem Fall stellen Sie die Verbindung mithilfe einer Verbindungszeichenfolge her.

2. Erstellen Sie einen Container in Ihrem Speicherkonto. Wechseln Sie zum Speicherkonto. Wählen Sie unter **Blob-Dienst** die Option **Blobs durchsuchen**. Wählen Sie oben die Option **+ Container** aus, um einen neuen Container zu erstellen.


## <a name="set-up-continuous-data-export"></a>Einrichten des fortlaufenden Datenexports

Nachdem Sie nun einen Speicher als Exportziel für Daten haben, gehen Sie folgendermaßen vor, um den kontinuierlichen Datenexport einzurichten. 

1. Melden Sie sich bei Ihrer IoT Central-Anwendung an.

2. Wählen Sie im Menü auf der linken Seite die Option **Datenexport** aus.

    > [!Note]
    > Wenn im linken Menü „Datenexport“ nicht angezeigt wird, sind Sie kein Administrator in Ihrer App. Wenden Sie sich an Ihren Administrator, damit dieser den Datenexport einrichtet.

3. Wählen Sie oben rechts die Schaltfläche **+ Neu** aus. Wählen Sie **Azure Blob Storage** als Ziel für den Export aus. 

    > [!NOTE] 
    > Die maximale Anzahl von Exporten pro App beträgt 5. 

    ![Erstellen eine neuen kontinuierlichen Datenexports](media/howto-export-data-pnp/export-new2.png)

4. Wählen Sie im Dropdown-Listenfeld Ihren **Speicherkonto-Namespace** aus. Sie können auch die letzte Option in der Liste auswählen, bei der es sich um **Verbindungszeichenfolge eingeben** handelt. 

    > [!NOTE] 
    > Ihnen werden nur Speicherkonto-Namespaces aus **demselben Abonnement wie dem Ihrer IoT Central-App** angezeigt. Wenn Sie in ein Ziel außerhalb dieses Abonnements exportieren möchten, wählen Sie **Verbindungszeichenfolge eingeben** aus, und fahren Sie mit Schritt 5 fort.

    > [!NOTE] 
    > Bei 7-Tage-Test-Apps ist eine Verbindungszeichenfolge die einzige Möglichkeit zur Konfiguration des kontinuierlichen Datenexports. Dies liegt daran, dass 7-Tage-Test-Apps kein Azure-Abonnement zugeordnet ist.

    ![Erstellen eines neuen Exports in Blob](media/howto-export-data-pnp/export-create-blob2.png)

5. (Optional) Wenn Sie **Verbindungszeichenfolge eingeben** ausgewählt haben, wird ein neues Feld angezeigt, in das Sie Ihre Verbindungszeichenfolge einfügen können. Um die Verbindungszeichenfolge für Ihr Speicherkonto abzurufen, wechseln Sie im Azure-Portal zum Konto:
    - Wählen Sie unter **Einstellungen** die Option **Zugriffsschlüssel** aus.
    - Kopieren Sie entweder die Verbindungszeichenfolge „key1“ oder die Verbindungszeichenfolge „key2“.
 
6. Wählen Sie einen Container aus dem Dropdown-Listenfeld aus. Wenn Sie keinen Container haben, wechseln Sie im Azure-Portal mit den folgenden Schritten zu Ihrem Speicherkonto:
    - Wählen Sie unter **Blob-Dienst** die Option **Blobs** aus. Klicken Sie auf **+ Container**, und geben Sie Ihrem Container einen Namen. Wählen Sie eine öffentliche Zugriffsebene für Ihre Daten aus (jede Ebene funktioniert beim fortlaufenden Datenexport). Weitere Informationen finden Sie unter [Azure Storage-Dokumentation](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal#create-a-container).

7.  Geben Sie unter **Data to export** (Zu exportierende Daten) die Typen der zu exportierenden Daten an, indem Sie den Typ jeweils auf **Ein** festlegen.
   
    > [!NOTE] 
    > Die Daten werden im JSON-Format exportiert.

8. Um den kontinuierlichen Datenexport zu aktivieren, stellen Sie sicher, dass der Umschalter **Datenexport** auf „ein“ festgelegt ist. Wählen Sie **Speichern** aus.

   ![Konfigurieren des fortlaufenden Datenexports](media/howto-export-data-pnp/export-list-blob2.png)

9. Nach einigen Minuten werden die Daten unter Ihrem Speicherkonto angezeigt.


## <a name="path-structure"></a>Pfadstruktur

Telemetriedaten, Geräte und Gerätevorlagendaten werden einmal pro Minute in Ihr Speicherkonto exportiert, wobei jede Datei den Batch der Änderungen seit der letzten exportierten Datei enthält. Exportierte Daten werden in drei Ordnern im JSON-Format gespeichert. Die Standardpfade in Ihrem Speicherkonto sind:
- Telemetriedaten: {container}/{app-id}/telemetry/{YYYY}/{MM}/{dd}/{hh}/{mm}/{dateiname}
- Geräte: {container}/{app-id}/telemetry/{YYYY}/{MM}/{dd}/{hh}/{mm}/{dateiname}
- Gerätevorlagen: {container}/{app-id}/deviceTemplates/{JJJJ}/{MM}/{TT}/{hh}/{mm}/{dateiname}

Sie können die exportierten Dateien im Azure-Portal durchsuchen, indem Sie zu der jeweiligen Datei navigieren und die Registerkarte **Blob bearbeiten** auswählen.

## <a name="data-format"></a>Datenformat
### <a name="telemetry"></a>Telemetrie

Die exportierten Telemetriedaten umfassen alle neuen Nachrichten, die während dieses Zeitraums von IoT Central von allen Geräten empfangen wurden. Für die exportierten Dateien wird dasselbe Format wie für die Nachrichtendateien verwendet, die bei der [IoT Hub-Nachrichtenweiterleitung](https://docs.microsoft.com/azure/iot-hub/iot-hub-csharp-csharp-process-d2c) in Blobspeicher exportiert wurden.

> [!NOTE]
> Stellen Sie sicher, dass Ihre Geräte Nachrichten senden, die `contentType: application/JSON` und `contentEncoding:utf-8` (oder `utf-16`, `utf-32`) enthalten. Ein Beispiel finden Sie in der [Dokumentation zu IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax#message-routing-query-based-on-message-body).

> [!NOTE]
> Die Geräte, die die Telemetriedaten senden, werden durch Geräte-IDs dargestellt (siehe die folgenden Abschnitte). Exportieren Sie die Gerätemomentaufnahmen, um die Namen der Geräte zu erhalten. Korrelieren Sie die einzelnen Nachrichtendatensätze, indem Sie jeweils die **connectionDeviceId** verwenden, die mit der **deviceId** des Gerätedatensatzes übereinstimmt.

Das folgende Beispiel zeigt einen Datensatz im JSON-Format.

```json
{ 
  "EnqueuedTimeUtc":"2019-09-26T17:46:09.8870000Z",
  "Properties":{ 

  },
  "SystemProperties":{ 
    "connectionDeviceId":"<deviceid>",
    "connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "connectionDeviceGenerationId":"637051167384630591",
    "contentType":"application/json",
    "contentEncoding":"utf-8",
    "enqueuedTime":"2019-09-26T17:46:09.8870000Z"
  },
  "Body":{ 
    "temp":49.91322758395974,
    "humid":49.61214852573155,
    "pm25":25.87332214661367
  }
}
```

### <a name="devices"></a>Geräte

Wenn der fortlaufende Datenexport zum ersten Mal aktiviert wird, wird eine einzelne Momentaufnahme mit allen Geräten exportiert. Jedes Gerät enthält Folgendes:
- `@id` des Geräts in IoT Central
- `name` des Geräts
- `deviceId` von [Device Provisioning Service](https://aka.ms/iotcentraldocsdps)
- Gerätevorlageninformationen
- Eigenschaftswerte

Eine neue Momentaufnahme wird einmal pro Minute geschrieben. Die Momentaufnahme enthält Folgendes:

- Seit der letzten Momentaufnahme hinzugefügte neue Geräte
- Geräte mit geänderten Eigenschaftswerten seit der letzten Momentaufnahme.

> [!NOTE]
> Geräte, die seit der letzten Momentaufnahme gelöscht wurden, werden nicht exportiert. Derzeit verfügen die Momentaufnahmen nicht über Indikatoren für gelöschte Geräte.
>
> Die Gerätevorlage, der die einzelnen Geräte zugewiesen sind, wird durch das Feld `instanceOf` dargestellt. Exportieren Sie die Momentaufnahmen von Gerätevorlagen, um den Namen der Gerätevorlage zu erhalten.

Exportierte Dateien enthalten eine einzige Zeile pro Datensatz. Das folgende Beispiel zeigt einen Datensatz im JSON-Format.

```json
{ 
  "@id":"<id-value>",
  "@type":"Device",
  "displayName":"Airbox",
  "data":{ 
    "$cloudProperties":{ 
        "Color":"blue"
    },
    "EnvironmentalSensor":{ 
      "thsensormodel":{ 
        "reported":{ 
          "value":"Neque quia et voluptatem veritatis assumenda consequuntur quod.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "pm25sensormodel":{ 
        "reported":{ 
          "value":"Aut alias odio.",
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    },
    "urn_azureiot_DeviceManagement_DeviceInformation":{ 
      "totalStorage":{ 
        "reported":{ 
          "value":27900.9730905171,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      },
      "totalMemory":{ 
        "reported":{ 
          "value":4667.82916715811,
          "$lastUpdatedTimestamp":"2019-09-30T20:35:43.8478978Z"
        }
      }
    }
  },
  "instanceOf":"<template-id>",
  "deviceId":"<device-id>",
  "simulated":true
}
```

### <a name="device-templates"></a>Gerätevorlagen

Wenn der fortlaufende Datenexport zum ersten Mal aktiviert wird, wird eine einzelne Momentaufnahme mit allen Gerätevorlagen exportiert. Jede Gerätevorlage enthält Folgendes:
- `@id` der Gerätevorlage
- `name` der Gerätevorlage
- `version` der Gerätevorlage
- Das `capabilityModel` des Geräts, einschließlich seiner `interfaces`, sowie die Definitionen für Telemetriedaten, Eigenschaften und Befehle
- Definitionen für `cloudProperties`
- Außerkraftsetzungen und Anfangswerte, zusammen mit dem `capabilityModel`

Eine neue Momentaufnahme wird einmal pro Minute geschrieben. Die Momentaufnahme enthält Folgendes:

- Neue Gerätevorlagen, die seit der letzten Momentaufnahme hinzugefügt wurden Darin sind neue Versionen von Gerätevorlagen enthalten.
- Gerätevorlagen mit geänderten Außerkraftsetzungen, Anfangswerten und Definitionen von Cloudeigenschaften seit der letzten Momentaufnahme.

> [!NOTE]
> Gerätevorlagen, die seit der letzten Momentaufnahme gelöscht wurden, werden nicht exportiert. Derzeit verfügen die Momentaufnahmen nicht über Indikatoren für gelöschte Gerätevorlagen.

Exportierte Dateien enthalten eine einzige Zeile pro Datensatz. Das folgende Beispiel zeigt einen Datensatz im JSON-Format.

```json
{ 
  "@id":"<template-id>",
  "@type":"DeviceModelDefinition",
  "displayName":"Airbox",
  "capabilityModel":{ 
    "@id":"<id>",
    "@type":"CapabilityModel",
    "implements":[ 
      { 
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"EnvironmentalSensor",
        "schema":{ 
          "@id":"<id>",
          "@type":"Interface",
          "comment":"Requires temperature and humidity sensors.",
          "description":"Provides functionality to report temperature, humidity. Provides telemetry, commands and read-write properties",
          "displayName":"Environmental Sensor",
          "contents":[ 
            { 
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current temperature on the device",
              "displayName":"Temperature",
              "name":"temp",
              "schema":"double",
              "unit":"Units/Temperature/celsius",
              "valueDetail":{ 
                "@id":"<id>",
                "@type":"ValueDetail/NumberValueDetail",
                "minValue":{ 
                  "@value":"50"
                }
              },
              "visualizationDetail":{ 
                "@id":"<id>",
                "@type":"VisualizationDetail"
              }
            },
            { 
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current humidity on the device",
              "displayName":"Humidity",
              "name":"humid",
              "schema":"integer"
            },
            { 
              "@id":"<id>",
              "@type":"Telemetry",
              "description":"Current PM2.5 on the device",
              "displayName":"PM2.5",
              "name":"pm25",
              "schema":"integer"
            },
            { 
              "@id":"<id>",
              "@type":"Property",
              "description":"T&H Sensor Model Name",
              "displayName":"T&H Sensor Model",
              "name":"thsensormodel",
              "schema":"string"
            },
            { 
              "@id":"<id>",
              "@type":"Property",
              "description":"PM2.5 Sensor Model Name",
              "displayName":"PM2.5 Sensor Model",
              "name":"pm25sensormodel",
              "schema":"string"
            }
          ]
        }
      },
      { 
        "@id":"<id>",
        "@type":"InterfaceInstance",
        "name":"urn_azureiot_DeviceManagement_DeviceInformation",
        "schema":{ 
          "@id":"<id>",
          "@type":"Interface",
          "displayName":"Device information",
          "contents":[ 
            { 
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available storage on the device in kilobytes. Ex. 20480000 kilobytes.",
              "displayName":"Total storage",
              "name":"totalStorage",
              "displayUnit":"kilobytes",
              "schema":"long"
            },
            { 
              "@id":"<id>",
              "@type":"Property",
              "comment":"Total available memory on the device in kilobytes. Ex. 256000 kilobytes.",
              "displayName":"Total memory",
              "name":"totalMemory",
              "displayUnit":"kilobytes",
              "schema":"long"
            }
          ]
        }
      }
    ],
    "displayName":"AAEONAirbox52"
  },
  "solutionModel":{ 
    "@id":"<id>",
    "@type":"SolutionModel",
    "cloudProperties":[ 
      { 
        "@id":"<id>",
        "@type":"CloudProperty",
        "displayName":"Color",
        "name":"Color",
        "schema":"string",
        "valueDetail":{ 
          "@id":"<id>",
          "@type":"ValueDetail/StringValueDetail"
        },
        "visualizationDetail":{ 
          "@id":"<id>",
          "@type":"VisualizationDetail"
        }
      }
    ]
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt mit dem Exportieren Ihrer Daten vertraut sind, können Sie mit dem nächsten Schritt fortfahren:

> [!div class="nextstepaction"]
> [Verwenden der IoT Central-Geräte-Bridge zum Herstellen einer Verbindung mit anderen IoT-Clouds](howto-build-iotc-device-bridge.md)
