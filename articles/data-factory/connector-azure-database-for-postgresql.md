---
title: Kopieren von Daten in und aus Azure Database for PostgreSQL mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten mit einer Kopieraktivität in einer Azure Data Factory-Pipeline für Azure Database for PostgreSQL kopieren.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: jingwang
ms.openlocfilehash: 4cb6f420b6d084539dc98a09632d0760a1344012
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71092171"
---
# <a name="copy-data-to-and-from-azure-database-for-postgresql-using-azure-data-factory"></a>Kopieren von Daten in und aus Azure Database for PostgreSQL mithilfe von Azure Data Factory

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Azure Database for PostgreSQL zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

Dieser Connector ist speziell auf den [Dienst „Azure Database for PostgreSQL“](../postgresql/overview.md) ausgelegt. Wenn Sie Daten aus einer generischen PostgreSQL-Datenbank in der lokalen Umgebung oder in der Cloud kopieren möchten, verwenden Sie den [PostgreSQL-Connector](connector-postgresql.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Dieser Azure Database for PostgreSQL-Connector wird für folgende Aktivitäten unterstützt:

- [Kopieraktivität](copy-activity-overview.md) mit [unterstützter Quellen/Senken-Matrix](copy-activity-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)

Sie können Daten aus Azure Database for PostgreSQL in beliebige unterstützte Senkendatenspeicher kopieren. Alternativ können Sie Daten auch aus jedem beliebigen unterstützten Quelldatenspeicher in Azure Database for PostgreSQL kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory enthält einen integrierten Treiber zum Sicherstellen der Konnektivität. Daher müssen Sie mit diesem Connector keinen Treiber manuell installieren.

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den Azure Database for PostgreSQL-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den verknüpften Azure Database for PostgreSQL-Dienst unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf Folgendes festgelegt werden: **AzurePostgreSql** | Ja |
| connectionString | Eine ODBC-Verbindungszeichenfolge zum Herstellen einer Verbindung mit Azure Database for PostgreSQL.<br/>Markieren Sie dieses Feld als „SecureString“, um es sicher in Data Factory zu speichern. Sie können auch das Kennwort in Azure Key Vault speichern und die `password`-Konfiguration aus der Verbindungszeichenfolge pullen. Ausführlichere Informationen finden Sie in den folgenden Beispielen und im Artikel [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure-Integrationslaufzeit oder selbstgehostete Integrationslaufzeit verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

Eine typische Verbindungszeichenfolge ist `Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>`. Weitere Eigenschaften, die Sie für Ihren Fall festlegen können:

| Eigenschaft | Beschreibung | Optionen | Erforderlich |
|:--- |:--- |:--- |:--- |
| EncryptionMethod (EM)| Diese Methode wird vom Treiber verwendet, um Daten zu verschlüsseln, die zwischen dem Treiber und dem Datenbankserver gesendet werden. Beispiel: `EncryptionMethod=<0/1/6>;`| 0 (keine Verschlüsselung) **(Standard)** / 1 (SSL) / 6 (RequestSSL) | Nein |
| ValidateServerCertificate (VSC) | Bestimmt, ob der Treiber das Zertifikat überprüft, das vom Datenbankserver gesendet wird, wenn die SSL-Verschlüsselung aktiviert ist (Encryption Method=1). Beispiel: `ValidateServerCertificate=<0/1>;`| 0 (Deaktiviert) **(Standard)** / 1 (Aktiviert) | Nein |

**Beispiel:**

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;Password=<Password>"
            }
        }
    }
}
```

**Beispiel: Speichern des Kennworts in Azure Key Vault**

```json
{
    "name": "AzurePostgreSqlLinkedService",
    "properties": {
        "type": "AzurePostgreSql",
        "typeProperties": {
            "connectionString": {
                 "type": "SecureString",
                 "value": "Server=<server>.postgres.database.azure.com;Database=<database>;Port=<port>;UID=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste mit den Eigenschaften, die vom Azure Database for PostgreSQL-Dataset unterstützt werden.

Legen Sie die „type“-Eigenschaft des Datasets auf **AzurePostgreSQLTable** fest, um Daten aus Azure Database for PostgreSQL zu kopieren. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf folgenden Wert festgelegt werden: **AzurePostgreSqlTable** | Ja |
| tableName | Name der Tabelle. | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |

**Beispiel**

```json
{
    "name": "AzurePostgreSqlDataset",
    "properties": {
        "type": "AzurePostgreSqlTable",
        "linkedServiceName": {
            "referenceName": "<AzurePostgreSql linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste mit den Eigenschaften, die von der Azure Database for PostgreSQL-Quelle unterstützt werden.

### <a name="azure-database-for-postgresql-as-source"></a>Azure-Datenbank für PostgreSQL als Quelle

Legen Sie zum Kopieren von Daten aus Azure Database for PostgreSQL den Quelltyp in der Kopieraktivität auf **AzureMySqlSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf Folgendes festgelegt werden: **AzurePostgreSqlSource** | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `"SELECT * FROM MyTable"`. | Nein (wenn „tableName“ im Dataset angegeben ist) |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromAzurePostgreSql",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<AzurePostgreSql input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzurePostgreSqlSource",
                "query": "<custom query e.g. SELECT * FROM MyTable>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-database-for-postgresql-as-sink"></a>Azure Database for PostgreSQL als Senke

Beim Kopieren von Daten nach Azure Database for PostgreSQL werden im Abschnitt **sink** der Kopieraktivität folgende Eigenschaften unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf Folgendes festgelegt sein: **AzurePostgreSQLSink** | Ja |
| preCopyScript | Geben Sie eine SQL-Abfrage für die Kopieraktivität an, die bei jeder Ausführung ausgeführt werden soll, bevor Daten in Azure Database for PostgreSQL geschrieben werden. Sie können diese Eigenschaft nutzen, um die vorab geladenen Daten zu bereinigen. | Nein |
| writeBatchSize | Fügt Daten in die Azure Database for PostgreSQL-Tabelle ein, wenn die Puffergröße „writeBatchSize“ erreicht.<br>Als Wert ist eine ganze Zahl (Integer) zulässig, mit der die Anzahl von Zeilen angegeben wird. | Nein (Standardwert ist 10.000) |
| writeBatchTimeout | Die Wartezeit für den Abschluss der Batcheinfügung, bis das Timeout wirksam wird.<br>Zulässige Werte sind Timespan-Werte. Beispiel: 00:30:00 (30 Minuten). | Nein (Standardwert ist 00:00:30) |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToAzureDatabaseForPostgreSQL",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure PostgreSQL output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzurePostgreSQLSink",
                "preCopyScript": "<custom SQL script>",
                "writeBatchSize": 100000
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Eigenschaften der Lookup-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [Lookup-Aktivität](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
