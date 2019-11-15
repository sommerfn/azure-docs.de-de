---
title: Kopieren von Daten nach und aus Azure SQL-Datenbank mithilfe von Data Factory
description: Erfahren Sie, wie mithilfe der Data Factory Daten von unterstützten Quelldatenspeichern nach Azure SQL-Datenbank oder aus SQL-Datenbank in unterstützte Senkendatenspeicher kopiert werden.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: jingwang
ms.openlocfilehash: 4ef47bc1064c095792a90ed69880106af77e9dfd
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681123"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Kopieren von Daten nach und aus Azure SQL-Datenbank mithilfe von Azure Data Factory
> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version von Azure Data Factory aus:"]
> * [Version 1](v1/data-factory-azure-sql-connector.md)
> * [Aktuelle Version](connector-azure-sql-database.md)

In diesem Artikel wird beschrieben, wie Sie Daten in die und aus der Azure SQL-Datenbank kopieren. Informationen zu Azure Data Factory finden Sie im [Einführungsartikel](introduction.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Dieser Connector für Azure SQL-Datenbank wird für die folgenden Aktivitäten unterstützt:

- [Kopieraktivität](copy-activity-overview.md) mit [unterstützter Quellen/Senken-Matrix](copy-activity-overview.md)-Tabelle
- [Mapping Data Flow](concepts-data-flow-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [GetMetadata-Aktivität](control-flow-get-metadata-activity.md)

Der Azure SQL-Datenbank-Connector unterstützt insbesondere folgende Funktionen:

- Kopieren von Daten mit SQL-Authentifizierung und Azure Active Directory-Anwendungstokenauthentifizierung (Azure AD) mit einem Dienstprinzipal oder verwalteten Identitäten für Azure-Ressourcen
- Als Quelle das Abrufen von Daten mithilfe einer SQL-Abfrage oder gespeicherten Prozedur
- Als Senke das Anfügen von Daten an die Zieltabelle oder Aufrufen einer gespeicherten Prozedur mit benutzerdefinierter Logik während des Kopiervorgangs

>[!NOTE]
>[Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-current) für Azure SQL-Datenbank wird derzeit von diesem Connector nicht unterstützt. Um dies zu umgehen, können Sie einen [generischen ODBC-Connector](connector-odbc.md) und einen SQL Server-ODBC-Treiber über eine selbstgehostete Integration Runtime verwenden. Befolgen Sie [diese Anleitung](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-current) zum Herunterladen des ODBC-Treibers und zum Konfigurieren der Verbindungszeichenfolgen.

> [!IMPORTANT]
> Wenn Sie Daten mithilfe der Integration Runtime von Azure Data Factory kopieren, konfigurieren Sie die [Azure SQL Server-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure), damit Azure-Dienste Zugriff auf den Server erhalten.
> Wenn Sie Daten mithilfe einer selbstgehosteten Integration Runtime kopieren, konfigurieren Sie die Azure SQL-Server-Firewall, um den entsprechenden IP-Adressbereich zuzulassen. Dieser Bereich schließt die IP-Adresse des Computers ein, der für die Verbindung mit Azure SQL-Datenbank verwendet wird.

## <a name="get-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Azure Data Factory-Entitäten speziell für einen Azure SQL-Datenbank-Connector verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Diese Eigenschaften werden für den mit Azure SQL-Datenbank verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft muss auf **AzureSqlDatabase** festgelegt sein. | Ja |
| connectionString | Geben Sie Informationen, die zur Verbindung mit der Azure SQL-Datenbank-Instanz erforderlich sind, für die **connectionString**-Eigenschaft ein. <br/>Kennzeichnen Sie dieses Feld als **SecureString**, um es sicher in Azure Data Factory zu speichern. Sie können auch ein Kennwort oder einen Dienstprinzipalschlüssel in Azure Key Vault eingeben. Bei Verwendung der SQL-Authentifizierung pullen Sie die `password`-Konfiguration aus der Verbindungszeichenfolge. Weitere Informationen finden Sie im JSON-Beispiel unter der Tabelle sowie unter [Speichern von Anmeldeinformationen in Azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| servicePrincipalId | Geben Sie die Client-ID der Anwendung an. | Ja, bei Azure AD-Authentifizierung mit einem Dienstprinzipal |
| servicePrincipalKey | Geben Sie den Schlüssel der Anwendung an. Markieren Sie dieses Feld als **SecureString**, um es sicher in Azure Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). | Ja, bei Azure AD-Authentifizierung mit einem Dienstprinzipal |
| tenant | Geben Sie die Mandanteninformationen, wie Domänenname oder Mandanten-ID, für Ihre Anwendung an. Diese können Sie abrufen, indem Sie im Azure-Portal mit der Maus auf den Bereich oben rechts zeigen. | Ja, bei Azure AD-Authentifizierung mit einem Dienstprinzipal |
| connectVia | Diese [Integration Runtime](concepts-integration-runtime.md) wird zum Herstellen einer Verbindung mit dem Datenspeicher verwendet. Sie können die Azure Integration Runtime oder eine selbstgehostete Integration Runtime verwenden, sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet. Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. | Nein |

Weitere Voraussetzungen und JSON-Beispiele für die verschiedenen Authentifizierungstypen finden Sie in den folgenden Abschnitten:

- [SQL-Authentifizierung](#sql-authentication)
- [Azure AD-Anwendungstokenauthentifizierung: Dienstprinzipal](#service-principal-authentication)
- [Azure AD-Anwendungstokenauthentifizierung: Verwaltete Identitäten für Azure-Ressourcen](#managed-identity)

>[!TIP]
>Wenn ein Fehler mit dem Fehlercode „UserErrorFailedToConnectToSqlServer“ auftritt und eine Meldung wie „Das Sitzungslimit für die Datenbank ist XXX und wurde erreicht“ angezeigt wird, fügen Sie `Pooling=false` zu Ihrer Verbindungszeichenfolge hinzu, und versuchen Sie es erneut.

### <a name="sql-authentication"></a>SQL-Authentifizierung

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Beispiel eines verknüpften Diensts mit SQL-Authentifizierung

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Kennwort in Azure Key Vault** 

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
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

### <a name="service-principal-authentication"></a>Dienstprinzipalauthentifizierung

Um die Azure AD-Anwendungstokenauthentifizierung basierend auf dem Dienstprinzipal zu verwenden, gehen Sie folgendermaßen vor:

1. [Erstellen Sie eine Azure Active Directory-Anwendung](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) im Azure-Portal. Notieren Sie sich den Anwendungsnamen und die folgenden Werte zum Definieren des verknüpften Diensts:

    - Anwendungs-ID
    - Anwendungsschlüssel
    - Mandanten-ID

2. [Geben Sie einen Azure Active Directory-Administrator](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) für Ihre Azure SQL Server-Instanz im Azure-Portal an, falls dies noch nicht geschehen ist. Der Azure AD-Administrator muss ein Azure AD-Benutzer oder eine Azure AD-Gruppe sein, jedoch kein Dienstprinzipal. Dieser Schritt ist erforderlich, damit Sie im nachfolgenden Schritt eine Azure AD-Identität verwenden können, um einen Benutzer einer eigenständigen Datenbank für den Dienstprinzipal erstellen können.

3. [Erstellen Sie Benutzer der eigenständigen Datenbank](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) für den Dienstprinzipal. Stellen Sie eine Verbindung mit der Datenbank her, aus der bzw. in die Sie Daten mithilfe von Tools wie SQL Server Management Studio kopieren möchten. Verwenden Sie dazu eine Azure AD-Identität, die mindestens die Berechtigung ALTER ANY USER aufweist. Führen Sie den folgenden T-SQL-Befehl aus: 
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. Gewähren Sie dem Dienstprinzipal die notwendigen Berechtigungen, wie bei SQL- oder anderen Benutzern üblich. Führen Sie den folgenden Code aus. Weitere Optionen finden Sie in [diesem Dokument](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. Konfigurieren Sie einen mit Azure SQL-Datenbank verknüpften Dienst in Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Beispiel eines verknüpften Diensts mit Dienstprinzipalauthentifizierung

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a>Verwaltete Identitäten für Azure-Ressourcenauthentifizierung

Eine Data Factory kann einer [verwalteten Identität für Azure-Ressourcen](data-factory-service-identity.md) zugeordnet werden, die diese spezielle Data Factory darstellt. Sie können diese verwaltete Identität für die Azure SQL-Datenbank-Authentifizierung verwenden. Die angegebene Factory kann mithilfe dieser Identität auf Daten in Ihrer Datenbank zuzugreifen und diese kopieren.

Führen Sie die folgenden Schritte aus, um die Authentifizierung mit einer verwalteten Identität zu verwenden.

1. [Geben Sie einen Azure Active Directory-Administrator](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) für Ihre Azure SQL Server-Instanz im Azure-Portal an, falls dies noch nicht geschehen ist. Der Azure AD-Administrator kann ein Azure AD-Benutzer oder eine Azure AD-Gruppe sein. Wenn Sie der Gruppe mit der verwalteten Identität eine Administratorrolle zuweisen möchten, überspringen Sie die Schritte 3 und 4. Der Administrator erhält Vollzugriff auf die Datenbank.

2. [Erstellen Sie für eine eigenständige Datenbank Benutzer](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) für die verwaltete Azure Data Factory-Identität. Stellen Sie eine Verbindung mit der Datenbank her, aus der bzw. in die Sie Daten mithilfe von Tools wie SQL Server Management Studio kopieren möchten. Verwenden Sie dazu eine Azure AD-Identität, die mindestens die Berechtigung ALTER ANY USER aufweist. Führen Sie den folgenden T-SQL-Befehl aus: 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. Gewähren Sie der verwalteten Data Factory-Identität die notwendigen Berechtigungen, und gehen Sie dabei so vor wie für SQL-Benutzer und andere Benutzer. Führen Sie den folgenden Code aus. Weitere Optionen finden Sie in [diesem Dokument](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your Data Factory name];
    ```

4. Konfigurieren Sie einen mit Azure SQL-Datenbank verknüpften Dienst in Azure Data Factory.

**Beispiel**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
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

Eine vollständige Liste mit den verfügbaren Abschnitten und Eigenschaften zum Definieren von Datasets finden Sie unter [Datasets](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Azure SQL-Datenbank-Dataset unterstützt werden.

Zum Kopieren von Daten aus der oder in die Azure SQL-Datenbank werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft des Datasets muss auf **AzureSqlTable** festgelegt sein. | Ja |
| schema | Name des Schemas. |Quelle: Nein, Senke: Ja  |
| table | Name der Tabelle/Ansicht. |Quelle: Nein, Senke: Ja  |
| tableName | Name der Tabelle/Ansicht mit Schema. Diese Eigenschaft wird aus Gründen der Abwärtskompatibilität weiterhin unterstützt. Verwenden Sie für eine neue Workload `schema` und `table`. | Quelle: Nein, Senke: Ja |

#### <a name="dataset-properties-example"></a>Beispiel für Dataseteigenschaften

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "schema": "<schema_name>",
            "table": "<table_name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste der verfügbaren Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie unter [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Azure SQL-Datenbank-Quelle und -Senke unterstützt werden.

### <a name="azure-sql-database-as-the-source"></a>Azure SQL-Datenbank als Quelle

Zum Kopieren von Daten aus Azure SQL-Datenbank werden die folgenden Eigenschaften im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft der Quelle der Kopieraktivität muss auf **AzureSqlSource** festgelegt werden. Der Typ „SqlSource“ wird aus Gründen der Abwärtskompatibilität weiterhin unterstützt. | Ja |
| sqlReaderQuery | Diese Eigenschaft verwendet die benutzerdefinierte SQL-Abfrage zum Lesen von Daten. Ein Beispiel ist `select * from MyTable`. | Nein |
| sqlReaderStoredProcedureName | Name der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. Die letzte SQL-Anweisung muss eine SELECT-Anweisung in der gespeicherten Prozedur sein. | Nein |
| storedProcedureParameters | Parameter für die gespeicherte Prozedur.<br/>Zulässige Werte sind Namen oder Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung von Parametern müssen den Namen und der Groß-/Kleinschreibung der Parameter der gespeicherten Prozedur entsprechen. | Nein |

**Beachten Sie Folgendes:**

- Wenn **sqlReaderQuery** für **AzureSqlSource** angegeben ist, führt die Kopieraktivität diese Abfrage für die Azure SQL-Datenbankquelle aus, um die Daten abzurufen. Sie können auch eine gespeicherte Prozedur angeben, indem Sie **sqlReaderStoredProcedureName** und **storedProcedureParameters** angeben, sofern die gespeicherten Prozeduren Parameter verwenden.
- Ohne Angabe von **sqlReaderQuery** oder **sqlReaderStoredProcedureName** werden die im Abschnitt „structure“ des JSON-Codes des Datasets definierten Spalten zum Erstellen einer Abfrage verwendet. Die Abfrage `select column1, column2 from mytable` wird für Azure SQL-Datenbank ausgeführt. Falls die Datasetdefinition keinen Abschnitt „structure“ enthält, werden alle Spalten der Tabelle ausgewählt.

#### <a name="sql-query-example"></a>Beispiel für eine SQL-Abfrage

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "AzureSqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="stored-procedure-example"></a>Beispiel für eine gespeicherte Prozedur

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "AzureSqlSource",
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

### <a name="stored-procedure-definition"></a>Definition der gespeicherten Prozedur

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

### <a name="azure-sql-database-as-the-sink"></a>Azure SQL-Datenbank als Senke

> [!TIP]
> Weitere Informationen zum unterstützten Schreibverhalten, zu unterstützten Konfigurationen und bewährten Methoden finden Sie unter [Bewährte Methode zum Laden von Daten in Azure SQL-Datenbank](#best-practice-for-loading-data-into-azure-sql-database).

Zum Kopieren von Daten in Azure SQL-Datenbank werden die folgenden Eigenschaften im Abschnitt **sink** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die **type**-Eigenschaft der Senke der Kopieraktivität muss auf **AzureSqlSink** festgelegt werden. Der Typ „SqlSink“ wird aus Gründen der Abwärtskompatibilität weiterhin unterstützt. | Ja |
| writeBatchSize | Anzahl der Zeilen, die in die SQL-Tabelle *pro Batch* eingefügt werden sollen.<br/> Zulässiger Wert: **integer** (Anzahl der Zeilen) Standardmäßig bestimmt Azure Data Factory die geeignete Batchgröße dynamisch anhand der Zeilengröße. | Nein |
| writeBatchTimeout | Wartezeit für den Abschluss der Batcheinfügung, bis das Timeout wirksam wird.<br/> Zulässiger Wert: **timespan**. Beispiel: 00:30:00 (30 Minuten). | Nein |
| preCopyScript | Geben Sie eine auszuführende SQL-Abfrage für die Kopieraktivität an, ehe Sie Daten in Azure SQL-Datenbank schreiben. Sie wird pro Ausführung der Kopieraktivität nur einmal aufgerufen. Sie können diese Eigenschaft nutzen, um vorab geladene Daten zu bereinigen. | Nein |
| sqlWriterStoredProcedureName | Der Name der gespeicherten Prozedur, die definiert, wie Quelldaten auf eine Zieltabelle angewandt werden. <br/>Diese gespeicherte Prozedur wird *pro Batch aufgerufen*. Für nur einmalig ausgeführte Vorgänge, die nicht mit Quelldaten in Zusammenhang stehen (etwa Löschen/Kürzen), verwenden Sie die `preCopyScript`-Eigenschaft. | Nein |
| storedProcedureTableTypeParameterName |Der Parametername des Tabellentyps, der in der gespeicherten Prozedur angegeben ist.  |Nein |
| sqlWriterTableType |Der Tabellentypname, der in der gespeicherten Prozedur verwendet werden soll. Die Kopieraktivität macht die verschobenen Daten in einer temporären Tabelle mit diesem Tabellentyp verfügbar. Der gespeicherte Prozedurcode kann dann die kopierten Daten mit vorhandenen Daten zusammenführen. |Nein |
| storedProcedureParameters |Parameter für die gespeicherte Prozedur.<br/>Zulässige Werte: Name-Wert-Paare. Die Namen und die Groß-/Kleinschreibung von Parametern müssen denen der Parameter der gespeicherten Prozedur entsprechen. | Nein |
| tableOption | Gibt an, ob die Senkentabelle auf Basis des Quellschemas automatisch erstellt werden soll, wenn sie nicht vorhanden ist. Die automatische Tabellenerstellung wird nicht unterstützt, wenn die Senke die gespeicherte Prozedur angibt oder das gestaffelte Kopieren in der Kopieraktivität konfiguriert ist. Zulässige Werte: `none` (Standard), `autoCreate`. |Nein |
| disableMetricsCollection | Data Factory sammelt Metriken wie Azure SQL-Datenbank-DTUs für die Leistungsoptimierung von Kopiervorgängen und Empfehlungen. Wenn Sie sich wegen dieses Verhaltens Gedanken machen, geben Sie `true` an, um es zu deaktivieren. | Nein (Standard = `false`) |

**Beispiel 1: Anfügen von Daten**

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSqlSink",
                "writeBatchSize": 100000,
                "tableOption": "autoCreate"
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
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "storedProcedureTableTypeParameterName": "MyTable",
                "sqlWriterTableType": "MyTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="best-practice-for-loading-data-into-azure-sql-database"></a>Kopieren von Daten nach und aus Azure SQL-Datenbank

Beim Kopieren von Daten in Azure SQL-Datenbank ist möglicherweise ein anderes Schreibverhalten erforderlich:

- [Anfügen](#append-data): Meine Quelldaten enthalten nur neue Datensätze.
- [Upsert:](#upsert-data) Meine Quelldaten umfassen sowohl Einfügungen als auch Aktualisierungen.
- [Überschreiben](#overwrite-the-entire-table): Ich möchte eine gesamte Dimensionstabelle jedes Mal neu laden.
- [Schreiben von Daten mit benutzerdefinierter Logik](#write-data-with-custom-logic): Ich benötige eine zusätzliche Verarbeitung vor dem endgültigen Einfügen in die Zieltabelle.

Informationen zur Konfiguration in Azure Data Factory und zu den bewährten Methoden finden Sie in den entsprechenden Abschnitten.

### <a name="append-data"></a>Anfügen von Daten

Das Anfügen von Daten ist das Standardverhalten dieses Senkenconnectors für Azure SQL-Datenbank. Azure Data Factory führt eine Masseneinfügung aus, um Daten effizient in Ihre Tabelle zu schreiben. Sie können die Quelle und Senke in der Kopieraktivität entsprechend konfigurieren.

### <a name="upsert-data"></a>Durchführen von Upsert für Daten

**Option 1:** Wenn Sie eine große Menge von Daten kopieren müssen, gehen Sie folgendermaßen vor, um einen Upsert-Vorgang auszuführen: 

- Verwenden Sie zunächst eine [datenbankbezogene temporäre Tabelle](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current#database-scoped-global-temporary-tables-azure-sql-database) für das Massenladen aller Datensätze mithilfe der Kopieraktivität. Da Vorgänge für datenbankbezogene temporäre Tabellen nicht protokolliert werden, können Sie Millionen von Datensätzen in wenigen Sekunden laden.
- Führen Sie in Azure Data Factory eine Aktivität der gespeicherten Prozedur aus, um eine [MERGE](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current)- oder INSERT-/UPDATE-Anweisung anzuwenden. Verwenden Sie die temporäre Tabelle als Quelle für die Ausführung aller Aktualisierungen oder Einfügungen als einzelne Transaktion. Auf diese Weise reduzieren Sie die Anzahl von Roundtrips und Protokollvorgängen. Am Ende der Aktivität der gespeicherten Prozedur kann die temporäre Tabelle abgeschnitten werden, damit sie für den nächsten Upsert-Zyklus bereit ist.

Sie können beispielsweise in Azure Data Factory eine Pipeline mit einer **Kopieraktivität** erstellen, die mit einer **Aktivität der gespeicherten Prozedur** verkettet ist. Erstere kopiert Daten aus Ihrem Quellspeicher in eine temporäre Azure SQL-Datenbanktabelle (z. B. **##UpsertTempTable** als Tabellenname im Dataset). Letztere ruft dann eine gespeicherte Prozedur auf, um die Quelldaten aus der temporären Tabelle in der Zieltabelle zusammenzuführen und die temporäre Tabelle zu bereinigen.

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

**Option 2:** Sie können auch [eine gespeicherte Prozedur in der Kopieraktivität aufrufen](#invoke-a-stored-procedure-from-a-sql-sink). Dabei wird jede Zeile in der Quelltabelle ausgeführt, statt Masseneinfügen als Standardansatz in der Kopieraktivität zu verwenden, was für umfangreiche Upsert-Vorgänge nicht geeignet ist.

### <a name="overwrite-the-entire-table"></a>Überschreiben der gesamten Tabelle

Sie können die **preCopyScript**-Eigenschaft in der Kopieraktivitätssenke konfigurieren. In diesem Fall führt Azure Data Factory für jede ausgeführte Kopieraktivität zuerst das Skript aus. Dann wird der Kopiervorgang ausgeführt, um die Daten einzufügen. Beispiel: Um die gesamte Tabelle mit den neuesten Daten zu überschreiben, geben Sie ein Skript an, um zunächst alle Datensätze zu löschen, bevor die neuen Daten durch Massenladen aus der Quelle eingefügt werden.

### <a name="write-data-with-custom-logic"></a>Schreiben von Daten mit benutzerdefinierter Logik

Die Schritte zum Schreiben von Daten mit benutzerdefinierter Logik ähneln den im Abschnitt [Durchführen von Upsert für Daten](#upsert-data) beschriebenen Schritten. Wenn Sie vor dem endgültigen Einfügen von Quelldaten in die Zieltabelle eine zusätzliche Verarbeitung im größeren Maßstab anwenden möchten, können Sie eine der beiden folgenden Aktionen ausführen:

- Laden Sie die Daten in eine datenbankbezogene temporäre Tabelle, und rufen Sie dann eine gespeicherte Prozedur auf. 
- Rufen Sie eine gespeicherte Prozedur während des Kopiervorgangs auf.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Aufrufen der gespeicherten Prozedur von der SQL-Senke

Beim Kopieren von Daten in Azure SQL-Datenbank können Sie auch eine vom Benutzer angegebene gespeicherte Prozedur mit zusätzlichen Parametern konfigurieren und aufrufen. Das Feature der gespeicherten Prozedur nutzt [Tabellenwertparameter](https://msdn.microsoft.com/library/bb675163.aspx).

> [!TIP]
> Beim Aufrufen einer gespeicherten Prozedur werden die Daten zeilenweise statt mithilfe eines Massenvorgangs verarbeitet, was bei umfangreichen Kopiervorgängen nicht empfohlen wird. Weitere Informationen finden Sie unter [Kopieren von Daten nach und aus Azure SQL-Datenbank](#best-practice-for-loading-data-into-azure-sql-database).

Sie können eine gespeicherte Prozedur nutzen, wenn integrierte Kopiermechanismen nicht den Zweck erfüllen. Ein Beispiel hierfür ist ein Szenario, in dem Sie vor dem endgültigen Einfügen von Quelldaten in die Zieltabelle eine zusätzliche Verarbeitung anwenden möchten. Beispiele für eine zusätzliche Verarbeitung sind das Zusammenführen von Spalten, das Suchen nach zusätzlichen Werten und das Einfügen in mehr als eine Tabelle.

Das folgende Beispiel zeigt, wie Sie eine gespeicherte Prozedur verwenden, um einen einfachen Upsert-Vorgang in eine Tabelle in Azure SQL-Datenbank auszuführen. Im Beispiel wird angenommen, dass Eingabedaten vorhanden sind und die Senkentabelle **Marketing** drei Spalten enthält: **ProfileID**, **State** und **Category**. Führen Sie den Upsert-Vorgang basierend auf der Spalte **ProfileID** aus, und wenden Sie ihn nur auf die Kategorie „ProductA“ an.

1. Definieren Sie in Ihrer Datenbank den Tabellentyp mit dem gleichen Namen wie **sqlWriterTableType**. Das Schema des Tabellentyps muss mit dem Schema übereinstimmen, das von den Eingabedaten zurückgegeben wird.

    ```sql
    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL，
        [Category] [varchar](256) NOT NULL
    )
    ```

2. Definieren Sie in Ihrer Datenbank die gespeicherte Prozedur mit dem gleichen Namen wie **SqlWriterStoredProcedureName**. Sie verarbeitet die Eingabedaten aus der angegebenen Quelle und führt sie mit der Ausgabetabelle zusammen. Der Parametername des Tabellentyps in der gespeicherten Prozedur entspricht dem im Dataset definierten **tableName**.

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

3. Definieren Sie in Azure Data Factory den Abschnitt **SQL-Senke** in der Kopieraktivität wie folgt:

    ```json
    "sink": {
        "type": "AzureSqlSink",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing",
        "storedProcedureTableTypeParameterName": "Marketing",
        "SqlWriterTableType": "MarketingType",
        "storedProcedureParameters": {
            "category": {
                "value": "ProductA"
            }
        }
    }
    ```

## <a name="mapping-data-flow-properties"></a>Eigenschaften von Mapping Data Flow

Ausführliche Informationen hierzu finden Sie unter [Quellentransformation](data-flow-source.md) und [Senkentransformation](data-flow-sink.md) in Mapping Data Flow.

## <a name="data-type-mapping-for-azure-sql-database"></a>Datentypzuordnung für Azure SQL-Datenbank

Beim Kopieren von Daten aus bzw. nach Azure SQL-Datenbank werden die folgenden Zuordnungen von Azure SQL-Datenbank-Datentypen zu Azure Data Factory-Zwischendatentypen verwendet. Informationen dazu, wie die Kopieraktivität das Quellschema und den Datentyp zur Senke zuordnet, finden Sie unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md).

| Azure SQL-Datenbank-Datentyp | Azure Data Factory-Zwischendatentyp |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |Datetime |
| Datetime |Datetime |
| datetime2 |Datetime |
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
| smalldatetime |Datetime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Byte |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| Xml |Xml |

>[!NOTE]
> Für Datentypen, die dem Zwischentyp „Decimal“ zugeordnet sind, unterstützt Azure Data Factory derzeit eine Genauigkeit von bis zu 28. Daten mit einer höheren Genauigkeit als 28 müssen in SQL-Abfragen ggf. in eine Zeichenfolge konvertiert werden.

## <a name="lookup-activity-properties"></a>Eigenschaften der Lookup-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [Lookup-Aktivität](control-flow-lookup-activity.md).

## <a name="getmetadata-activity-properties"></a>Eigenschaften der GetMetadata-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [GetMetadata-Aktivität](control-flow-get-metadata-activity.md). 

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher und -formate](copy-activity-overview.md##supported-data-stores-and-formats).
