---
title: Kopieren von Daten auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz mit Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten mit Azure Data Factory auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz verschieben.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: jingwang
ms.openlocfilehash: 3e1978c761c365125ac94a1ecbef5f9ac7375eba
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67338616"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-by-using-azure-data-factory"></a>Kopieren von Daten auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz mit Azure Data Factory

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten auf eine bzw. von einer verwalteten Azure SQL-Datenbank-Instanz zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus einer verwalteten Azure SQL-Datenbank-Instanz in beliebige unterstützte Senkendatenspeicher und umgekehrt kopieren. Zudem können Sie Daten aus jedem unterstützten Quelldatenspeicher in eine verwaltete Instanz kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Diese verwaltete Azure SQL-Datenbank-Instanz unterstützt insbesondere Folgendes:

- Kopieren von Daten unter Verwendung der SQL-Authentifizierung
- Als Quelle das Abrufen von Daten mithilfe einer SQL-Abfrage oder gespeicherten Prozedur
- Als Senke das Anfügen von Daten an die Zieltabelle oder Aufrufen einer gespeicherten Prozedur mit benutzerdefinierter Logik während des Kopiervorgangs

>[!NOTE]
>[Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-mi-current) der verwalteten Azure SQL-Datenbank-Instanz wird derzeit von diesem Connector nicht unterstützt. Um dies zu umgehen, können Sie einen [generischen ODBC-Connector](connector-odbc.md) und einen SQL Server-ODBC-Treiber über eine selbstgehostete Integration Runtime verwenden. Befolgen Sie [diese Anleitung](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-mi-current) zum Herunterladen des ODBC-Treibers und zum Konfigurieren der Verbindungszeichenfolgen.

>[!NOTE]
>Die Authentifizierung des Dienstprinzipals und der verwalteten Identität werden derzeit von diesem Connector nicht unterstützt. Dies können Sie umgehen, indem Sie einen Azure SQL-Datenbank-Connector auswählen und den Server Ihrer verwalteten Instanz manuell angeben.

## <a name="prerequisites"></a>Voraussetzungen

Für den Zugriff auf den [öffentlichen Endpunkt](../sql-database/sql-database-managed-instance-public-endpoint-securely.md) der verwalteten Azure SQL-Datenbank-Instanz können Sie eine über Azure Data Factory verwaltete Azure Integration Runtime verwenden. Stellen Sie sicher, dass Sie den öffentlichen Endpunkt aktivieren und außerdem Datenverkehr über den öffentlichen Endpunkt für die Netzwerksicherheitsgruppe zulassen, damit Azure Data Factory eine Verbindung mit Ihrer Datenbank herstellen kann. Weitere Informationen finden Sie in [dieser Anleitung](../sql-database/sql-database-managed-instance-public-endpoint-configure.md).

Richten Sie für den Zugriff auf den privaten Endpunkt der verwalteten Azure SQL-Datenbank-Instanz eine [selbstgehostete Integration Runtime](create-self-hosted-integration-runtime.md) ein, die Zugriff auf die Datenbank hat. Wenn Sie die selbstgehostete Integration Runtime im gleichen virtuellen Netzwerk wie Ihre verwaltete Instanz bereitstellen, stellen Sie sicher, dass sich der Computer mit der Integration Runtime in einem anderen Subnetz als die verwaltete Instanz befindet. Wenn Sie die selbstgehostete Integration Runtime in einem anderen virtuellen Netzwerk bereitstellen als die verwaltete Instanz, empfiehlt sich ein Peering virtueller Netzwerke oder eine Verbindung zwischen den virtuellen Netzwerken. Weitere Informationen finden Sie in [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Datenbank-Instanz](../sql-database/sql-database-managed-instance-connect-app.md).

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Azure Data Factory-Entitäten speziell für den Connector für die verwaltete Azure SQL-Datenbank-Instanz verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit der verwalteten Azure SQL-Datenbank-Instanz verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **SqlServer** festgelegt sein. | Ja |
| connectionString |Diese Eigenschaft gibt die **connectionString**-Informationen an, die zum Herstellen einer Verbindung mit der verwalteten Instanz mithilfe der SQL-Authentifizierung benötigt werden. Weitere Informationen finden Sie in den folgenden Beispielen. <br/>Markieren Sie dieses Feld als **SecureString**, um es sicher in Azure Data Factory zu speichern. Sie können auch ein Kennwort in Azure Key Vault speichern. Bei Verwendung der SQL-Authentifizierung pullen Sie die `password`-Konfiguration aus der Verbindungszeichenfolge. Weitere Informationen finden Sie im JSON-Beispiel unter der Tabelle sowie unter [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | Diese [Integration Runtime](concepts-integration-runtime.md) wird zum Herstellen einer Verbindung mit dem Datenspeicher verwendet. Sie können eine selbstgehostete Integration Runtime oder eine Azure Integration Runtime verwenden, wenn die verwaltete Instanz über einen öffentlichen Endpunkt verfügt und den Zugriff von Azure Data Factory darauf zulässt. Wenn kein Wert angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Ja |

**Beispiel 1: SQL-Authentifizierung verwenden**

Der Standardport ist 1433. Wenn Sie die verwaltete Azure SQL-Datenbank-Instanz mit einem öffentlichen Endpunkt verwenden, geben Sie Port 3342 explizit an.

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Beispiel 2: Verwenden der SQL-Authentifizierung mit einem Kennwort in Azure Key Vault**

Der Standardport ist 1433. Wenn Sie die verwaltete Azure SQL-Datenbank-Instanz mit einem öffentlichen Endpunkt verwenden, geben Sie Port 3342 explizit an.

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
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

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste mit den Eigenschaften, die vom Dataset der verwalteten Azure SQL-Datenbank-Instanz unterstützt werden.

Zum Kopieren von Daten in die bzw. aus der verwaltete(n) Azure SQL-Datenbank-Instanz werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **SqlServerTable** festgelegt werden. | Ja |
| tableName |Diese Eigenschaft ist der Name der Tabelle oder Ansicht in der Datenbankinstanz, auf die der verknüpfte Dienst verweist. | Quelle: Nein, Senke: Ja |

**Beispiel**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste mit den Eigenschaften, die von der Quelle und Senke der verwalteten Azure SQL-Datenbank-Instanz unterstützt werden.

### <a name="azure-sql-database-managed-instance-as-a-source"></a>Verwaltete Azure SQL-Datenbank-Instanz als Quelle

Legen Sie zum Kopieren von Daten von der verwalteten Azure SQL-Datenbank-Instanz den Quelltyp in der Kopieraktivität auf **SqlSource** fest. Die folgenden Eigenschaften werden im Abschnitt „source“ der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **SqlSource** festgelegt sein. | Ja |
| sqlReaderQuery |Diese Eigenschaft verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Ein Beispiel ist `select * from MyTable`. |Nein |
| sqlReaderStoredProcedureName |Diese Eigenschaft ist der Name der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. Die letzte SQL-Anweisung muss eine SELECT-Anweisung in der gespeicherten Prozedur sein. |Nein |
| storedProcedureParameters |Diese Parameter werden für die gespeicherte Prozedur verwendet.<br/>Zulässige Werte sind Namen oder Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung der Parameter müssen denen der Parameter der gespeicherten Prozedur entsprechen. |Nein |

**Beachten Sie folgende Punkte:**

- Wenn **sqlReaderQuery** für **SqlSource** angegeben ist, wendet die Kopieraktivität diese Abfrage auf die Quelle „Verwaltete Instanz“ an, um die Daten abzurufen. Sie können auch eine gespeicherte Prozedur angeben, indem Sie **sqlReaderStoredProcedureName** und **storedProcedureParameters** angeben, sofern die gespeicherten Prozeduren Parameter verwenden.
- Ohne Angabe von **sqlReaderQuery** oder der Eigenschaft **sqlReaderStoredProcedureName** werden die im Abschnitt „structure“ des JSON-Codes des Datasets definierten Spalten zum Erstellen einer Abfrage verwendet. Die Abfrage `select column1, column2 from mytable` wird für die verwaltete Instanz ausgeführt. Falls die Datasetdefinition keinen Abschnitt „structure“ enthält, werden alle Spalten der Tabelle ausgewählt.

**Beispiel: Verwenden einer SQL-Abfrage**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Beispiel: Verwenden einer gespeicherten Prozedur**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Definition der gespeicherten Prozedur**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-managed-instance-as-a-sink"></a>Verwaltete Azure SQL-Datenbank-Instanz als Senke

> [!TIP]
> Weitere Informationen zum unterstützten Schreibverhalten, zu unterstützten Konfigurationen und bewährten Methoden finden Sie unter [Bewährte Methode zum Laden von Daten in die verwaltete Azure SQL-Datenbank-Instanz](#best-practice-for-loading-data-into-azure-sql-database-managed-instance).

Legen Sie zum Kopieren von Daten auf die verwaltete Azure SQL-Datenbank-Instanz den Senkentyp in der Kopieraktivität auf **SqlSink** fest. Die folgenden Eigenschaften werden im Abschnitt „sink“ der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **SqlSink** festgelegt sein. | Ja |
| writeBatchSize |Anzahl der Zeilen, die *pro Batch* in die SQL-Tabelle eingefügt werden sollen.<br/>Zulässige Werte sind Integer-Werte für die Anzahl der Zeilen. Standardmäßig bestimmt Azure Data Factory die geeignete Batchgröße dynamisch anhand der Zeilengröße.  |Nein |
| writeBatchTimeout |Diese Eigenschaft gibt die Wartezeit für den Abschluss der Batcheinfügung an, bevor ein Timeout auftritt.<br/>Zulässige Werte werden für den Zeitraum verwendet. Ein Beispiel ist „00:30:00“. Das sind 30 Minuten. |Nein |
| preCopyScript |Diese Eigenschaft gibt eine SQL-Abfrage für die Kopieraktivität an, die vor dem Schreiben von Daten in die verwaltete Instanz ausgeführt wird. Sie wird pro Ausführung der Kopieraktivität nur einmal aufgerufen. Sie können diese Eigenschaft nutzen, um die vorab geladenen Daten zu bereinigen. |Nein |
| sqlWriterStoredProcedureName |Dies ist der Name der gespeicherten Prozedur, die definiert, wie Quelldaten auf eine Zieltabelle angewandt werden. <br/>Diese gespeicherte Prozedur wird *pro Batch aufgerufen*. Auf einen Vorgang auszuführen, der nur einmal ausgeführt wird und nicht mit Quelldaten in Zusammenhang steht (etwa Löschen/Kürzen), verwenden Sie die `preCopyScript`-Eigenschaft. |Nein |
| storedProcedureParameters |Diese Parameter werden für die gespeicherte Prozedur verwendet.<br/>Zulässige Werte sind Namen oder Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung der Parameter müssen denen der Parameter der gespeicherten Prozedur entsprechen. |Nein |
| sqlWriterTableType |Diese Eigenschaft gibt einen Tabellentypnamen an, der in der gespeicherten Prozedur verwendet werden soll. Die Kopieraktivität macht die verschobenen Daten in einer temporären Tabelle mit diesem Tabellentyp verfügbar. Der gespeicherte Prozedurcode kann dann die kopierten Daten mit vorhandenen Daten zusammenführen. |Nein |

**Beispiel 1: Anfügen von Daten**

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Beispiel 2: Aufrufen einer gespeicherten Prozedur während des Kopiervorgangs**

Weitere Informationen finden Sie unter [Aufrufen einer gespeicherten Prozedur aus einer SQL-Senke](#invoke-a-stored-procedure-from-a-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="best-practice-for-loading-data-into-azure-sql-database-managed-instance"></a>Bewährte Methode zum Laden von Daten in die verwaltete Azure SQL-Datenbank-Instanz

Beim Kopieren von Daten in die verwaltete Azure SQL-Datenbank-Instanz ist möglicherweise ein anderes Schreibverhalten erforderlich:

- [Anfügen:](#append-data) Meine Quelldaten enthalten nur neue Datensätze.
- [Upsert:](#upsert-data) Meine Quelldaten umfassen sowohl Einfügungen als auch Aktualisierungen.
- [Überschreiben:](#overwrite-the-entire-table) Ich möchte die gesamte Dimensionstabelle jedes Mal neu laden.
- [Schreiben von Daten mit benutzerdefinierter Logik:](#write-data-with-custom-logic) Ich benötige eine zusätzliche Verarbeitung vor dem endgültigen Einfügen in die Zieltabelle. 

Informationen zur Konfiguration in Azure Data Factory und zu bewährten Methoden finden Sie in den entsprechenden Abschnitten.

### <a name="append-data"></a>Anfügen von Daten

Das Anfügen von Daten ist das Standardverhalten dieses Senkenconnectors für die verwaltete Azure SQL-Datenbank-Instanz. Azure Data Factory führt eine Masseneinfügung aus, um Daten effizient in Ihre Tabelle zu schreiben. Sie können die Quelle und Senke in der Kopieraktivität entsprechend konfigurieren.

### <a name="upsert-data"></a>Durchführen von Upsert für Daten

**Option 1:** Wenn Sie eine große Menge von Daten kopieren müssen, gehen Sie folgendermaßen vor, um einen Upsert-Vorgang auszuführen: 

- Verwenden Sie zunächst eine [temporäre Tabelle](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=sql-server-2017#temporary-tables) für das Massenladen aller Datensätze mithilfe der Kopieraktivität. Da Vorgänge für temporäre Tabellen nicht protokolliert werden, können Sie Millionen von Datensätzen in wenigen Sekunden laden.
- Führen Sie in Azure Data Factory eine Aktivität der gespeicherten Prozedur aus, um eine [MERGE](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current)- oder INSERT-/UPDATE-Anweisung anzuwenden. Verwenden Sie die temporäre Tabelle als Quelle für die Ausführung aller Aktualisierungen oder Einfügungen als einzelne Transaktion. Auf diese Weise reduzieren Sie die Anzahl von Roundtrips und Protokollvorgängen. Am Ende der Aktivität der gespeicherten Prozedur kann die temporäre Tabelle abgeschnitten werden, damit sie für den nächsten Upsert-Zyklus bereit ist.

Sie können beispielsweise in Azure Data Factory eine Pipeline mit einer **Kopieraktivität** erstellen, die mit einer **Aktivität der gespeicherten Prozedur** verkettet ist. Erstere kopiert Daten aus Ihrem Quellspeicher in eine temporäre Tabelle (z. B. **##UpsertTempTable** als Tabellenname im Dataset). Letztere ruft dann eine gespeicherte Prozedur auf, um die Quelldaten aus der temporären Tabelle in der Zieltabelle zusammenzuführen und die temporäre Tabelle zu bereinigen.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

Definieren Sie in Ihrer Datenbank eine gespeicherte Prozedur mit MERGE-Logik (wie im folgenden Beispiel), auf die über die vorherige Aktivität der gespeicherten Prozedur gezeigt wird. Angenommen, das Ziel ist die Tabelle **Marketing** mit den drei Spalten: **ProfileID**, **State** und **Category**. Führen Sie den Upsert-Vorgang basierend auf der Spalte **ProfileID** aus.

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING ##UpsertTempTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE ##UpsertTempTable
END
```

**Option 2:** Sie können auch [eine gespeicherte Prozedur in einer Kopieraktivität aufrufen](#invoke-a-stored-procedure-from-a-sql-sink). Dabei wird jede Zeile in der Quelltabelle ausgeführt, statt Masseneinfügen als Standardansatz in der Kopieraktivität zu verwenden, da dies für umfangreiche Upsert-Vorgänge nicht geeignet ist.

### <a name="overwrite-the-entire-table"></a>Überschreiben der gesamten Tabelle

Sie können die **preCopyScript**-Eigenschaft in einer Kopieraktivitätssenke konfigurieren. In diesem Fall führt Azure Data Factory für jede ausgeführte Kopieraktivität zuerst das Skript aus. Dann wird der Kopiervorgang ausgeführt, um die Daten einzufügen. Beispiel: Um die gesamte Tabelle mit den neuesten Daten zu überschreiben, geben Sie ein Skript an, um zunächst alle Datensätze zu löschen, bevor die neuen Daten durch Massenladen aus der Quelle eingefügt werden.

### <a name="write-data-with-custom-logic"></a>Schreiben von Daten mit benutzerdefinierter Logik

Die Schritte zum Schreiben von Daten mit benutzerdefinierter Logik ähneln den im Abschnitt [Durchführen von Upsert für Daten](#upsert-data) beschriebenen Schritten. Wenn Sie vor dem endgültigen Einfügen von Quelldaten in die Zieltabelle eine zusätzliche Verarbeitung im größeren Maßstab anwenden möchten, können Sie eine der beiden folgenden Aktionen ausführen: 

- Laden Sie die Daten in eine temporäre Tabelle, und rufen Sie dann eine gespeicherte Prozedur auf.
- Rufen Sie eine gespeicherte Prozedur während des Kopiervorgangs auf.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Aufrufen der gespeicherten Prozedur von der SQL-Senke

Beim Kopieren von Daten in die verwaltete Azure SQL-Datenbank-Instanz können Sie auch eine vom Benutzer angegebene gespeicherte Prozedur mit zusätzlichen Parametern konfigurieren und aufrufen.

> [!TIP]
> Beim Aufrufen einer gespeicherten Prozedur werden die Daten zeilenweise statt mithilfe eines Massenvorgangs verarbeitet, was bei umfangreichen Kopiervorgängen nicht empfohlen wird. Weitere Informationen finden Sie unter [Bewährte Methode zum Laden von Daten in die verwaltete Azure SQL-Datenbank-Instanz](#best-practice-for-loading-data-into-azure-sql-database-managed-instance).

Sie können eine gespeicherte Prozedur nutzen, wenn integrierte Kopiermechanismen nicht den Zweck erfüllen. Ein Beispiel hierfür ist ein Szenario, in dem Sie vor dem endgültigen Einfügen von Quelldaten in die Zieltabelle eine zusätzliche Verarbeitung anwenden möchten. Beispiele für eine zusätzliche Verarbeitung sind das Zusammenführen von Spalten, das Suchen nach zusätzlichen Werten und das Einfügen von Daten in mehrere Tabellen.

Das folgende Beispiel zeigt, wie Sie eine gespeicherte Prozedur verwenden, um einen einfachen Upsert-Vorgang in eine Tabelle in der SQL Server-Datenbank auszuführen. Im Beispiel wird angenommen, dass Eingabedaten vorhanden sind und die Senkentabelle **Marketing** drei Spalten enthält: **ProfileID**, **State** und **Category**. Führen Sie den Upsert-Vorgang basierend auf der Spalte **ProfileID** aus, und wenden Sie ihn nur auf eine bestimmte Kategorie an.

**Ausgabedataset:** „tableName“ entspricht dem Parameternamen des Tabellentyps in Ihrer gespeicherten Prozedur wie im folgenden Skript für die gespeicherte Prozedur gezeigt:

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Definieren Sie den Abschnitt **SqlSink** in der Kopieraktivität wie folgt:

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

Definieren Sie in Ihrer Datenbank die gespeicherte Prozedur mit dem gleichen Namen wie **SqlWriterStoredProcedureName**. Sie verarbeitet die Eingabedaten aus der angegebenen Quelle und führt sie mit der Ausgabetabelle zusammen. Der Parametername des Tabellentyps in der gespeicherten Prozedur entspricht dem im Dataset definierten **tableName**.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category);
END
```

Definieren Sie in Ihrer Datenbank den Tabellentyp mit dem gleichen Namen wie **sqlWriterTableType**. Das Schema des Tabellentyps muss mit dem Schema übereinstimmen, das von den Eingabedaten zurückgegeben wird.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL
)
```

Das Feature der gespeicherten Prozedur nutzt [Tabellenwertparameter](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Datentypzuordnung für verwaltete Azure SQL-Datenbank-Instanz

Beim Kopieren von Daten auf die bzw. von der verwalteten Azure SQL-Datenbank-Instanz werden die folgenden Zuordnungen von Datentypen der verwalteten Azure SQL-Datenbank-Instanz zu Azure Data Factory-Zwischendatentypen verwendet. Informationen dazu, wie die Kopieraktivität das Quellschema und den Datentyp zur Senke zuordnet, finden Sie unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md).

| Datentyp der verwalteten Azure SQL-Datenbank-Instanz | Azure Data Factory-Zwischendatentyp |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |DateTime |
| DateTime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| Xml |Xml |

>[!NOTE]
> Für Datentypen, die dem Zwischentyp „Decimal“ zugeordnet sind, unterstützt Azure Data Factory derzeit eine Genauigkeit von bis zu 28. Wenn Ihre Daten eine höhere Genauigkeit als 28 erfordern, erwägen Sie, sie per SQL-Abfrage in eine Zeichenfolge zu konvertieren.

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).
