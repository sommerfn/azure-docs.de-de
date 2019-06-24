---
title: Kopieren von Daten aus DB2 mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Daten aus DB2 mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: jingwang
ms.openlocfilehash: 4bf4c5c8339c8c56d91737fa1ff62f55b9c38696
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "60786372"
---
# <a name="copy-data-from-db2-by-using-azure-data-factory"></a>Kopieren von Daten aus DB2 mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Wählen Sie die von Ihren verwendete Version des Data Factory-Diensts aus:"]
> * [Version 1](v1/data-factory-onprem-db2-connector.md)
> * [Aktuelle Version](connector-db2.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus einer DB2-Datenbank zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer DB2-Datenbank in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der DB2-Connector unterstützt insbesondere die folgenden IBM DB2-Plattformen und -Versionen mit Distributed Relational Database Architecture (DRDA) SQL Access Manager (SQLAM) Version 9, 10 und 11:

* IBM DB2 für z/OS 11.1
* IBM DB2 für z/OS 10.1
* IBM DB2 für i 7.2
* IBM DB2 für i 7.1
* IBM DB2 für LUW 11
* IBM DB2 für LUW 10.5
* IBM DB2 für LUW 10.1

> [!TIP]
> Wenn die Fehlermeldung „Das einer Anforderung zum Ausführen einer SQL-Anweisung entsprechende Paket wurde nicht gefunden. SQLSTATE=51002 SQLCODE=-805“ angezeigt wird, ist die Ursache dafür, dass für einen normalen Benutzer unter diesem Betriebssystem ein erforderliches Paket nicht erstellt wird. Befolgen Sie basierend auf Ihrem DB2-Servertyp die folgenden Anweisungen:
> - DB2 für i (AS400): Lassen Sie vor der Verwendung der Kopieraktivität einen Poweruser die Sammlung für den Anmeldebenutzer erstellen. Befehl: `create collection <username>`
> - DB2 für z/OS oder LUW: Verwenden Sie ein Konto mit weitreichenden Berechtigungen (Poweruser oder Administrator mit Paketberechtigungen und den Berechtigungen BIND, BINDADD und GRANT EXECUTE TO PUBLIC), um die Kopieraktivität einmal auszuführen. Anschließend wird das erforderliche Paket während des Kopiervorgangs automatisch erstellt. Anschließend können Sie für die folgenden Kopiervorgänge wieder das normale Benutzerkonto verwenden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Kopieren von Daten aus einer DB2-Datenbank, die nicht öffentlich zugänglich ist, müssen Sie eine selbstgehostete Integrationslaufzeit einrichten. Details zur selbstgehosteten Integrationslaufzeit finden Sie im Artikel [Selbstgehostete Integrationslaufzeit](create-self-hosted-integration-runtime.md). Die Integrationslaufzeit bietet einen integrierten DB2-Treiber. Daher müssen beim Kopieren von Daten aus DB2 keine Treiber manuell installiert werden.

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für den DB2-Connector verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit DB2 verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf Folgendes festgelegt werden: **Db2** | Ja |
| server |Name des DB2-Servers. Sie können die Portnummer hinter dem Servernamen und einem Semikolon angeben, z.B.: `server:port`. |Ja |
| database |Name der DB2-Datenbank. |Ja |
| authenticationType |Typ der Authentifizierung für die Verbindung mit der DB2-Datenbank.<br/>Zulässiger Wert: **Basic**. |Ja |
| userName |Geben Sie einen Benutzernamen für das Herstellen der Verbindung mit der DB2-Datenbank an. |Ja |
| password |Geben Sie das Kennwort für das Benutzerkonto an, das Sie für den Benutzernamen angegeben haben. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). |Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die selbstgehostete Integration Runtime oder Azure Integration Runtime verwenden (sofern Ihr Datenspeicher öffentlich zugänglich ist). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

**Beispiel:**

```json
{
    "name": "Db2LinkedService",
    "properties": {
        "type": "Db2",
        "typeProperties": {
            "server": "<servername:port>",
            "database": "<dbname>",
            "authenticationType": "Basic",
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom DB2-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus DB2 die type-Eigenschaft des Datasets auf **RelationalTable** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf folgenden Wert festgelegt werden: **RelationalTable** | Ja |
| tableName | Name der Tabelle in der DB2-Datenbank | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |

**Beispiel**

```json
{
    "name": "DB2Dataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<DB2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {}
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der DB2-Quelle unterstützt werden.

### <a name="db2-as-source"></a>DB2 als Quelle

Legen Sie zum Kopieren von Daten aus DB2 den Quelltyp in der Kopieraktivität auf **RelationalSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf Folgendes festgelegt werden: **RelationalSource** | Ja |
| query | Verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Beispiel: `"query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""`. | Nein (wenn „tableName“ im Dataset angegeben ist) |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromDB2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<DB2 input dataset name>",
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
                "type": "RelationalSource",
                "query": "SELECT * FROM \"DB2ADMIN\".\"Customers\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-db2"></a>Datentypzuordnung für DB2

Beim Kopieren von Daten aus DB2 werden die folgenden Zuordnungen von DB2-Datentypen zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

| Typ "DB2-Datenbank" | Data Factory-Zwischendatentyp |
|:--- |:--- |
| BigInt |Int64 |
| Binary |Byte[] |
| Blob |Byte[] |
| Char |String |
| Clob |String |
| Date |DateTime |
| DB2DynArray |String |
| DbClob |String |
| Decimal |Decimal |
| DecimalFloat |Decimal |
| Double |Double |
| Float |Double |
| Graphic |String |
| Integer |Int32 |
| LongVarBinary |Byte[] |
| LongVarChar |String |
| LongVarGraphic |String |
| Numeric |Decimal |
| Real |Single |
| SmallInt |Int16 |
| Time |TimeSpan |
| Timestamp |DateTime |
| VarBinary |Byte[] |
| VarChar |String |
| VarGraphic |String |
| Xml |Byte[] |


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).
