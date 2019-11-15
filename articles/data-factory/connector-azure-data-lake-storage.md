---
title: Kopieren von Daten nach und aus Azure Data Lake Storage Gen2 mithilfe von Data Factory
description: Hier erfahren Sie, wie Sie mithilfe von Azure Data Factory Daten in oder aus Azure Data Lake Storage Gen2 kopieren.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: jingwang
ms.openlocfilehash: e368597880bbbaee6c7aff7e72d88149840a23d8
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73681291"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen2-using-azure-data-factory"></a>Kopieren von Daten nach und aus Azure Data Lake Storage Gen2 mithilfe von Azure Data Factory

Azure Data Lake Storage Gen2 (ADLS Gen2) baut auf [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md) auf und bietet eine Reihe von Funktionen für die Big Data-Analyse. So haben Sie eine Schnittstelle zu Ihren Daten sowohl über das Dateisystem als auch den Objektspeicher.

In diesem Artikel wird beschrieben, wie Sie Daten in und aus Azure Data Lake Storage Gen2 kopieren. Informationen zu Azure Data Factory finden Sie im [Einführungsartikel](introduction.md).

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Dieser Azure Data Lake Storage Gen2-Connector wird für die folgenden Aktivitäten unterstützt:

- [Kopieraktivität](copy-activity-overview.md) mit [unterstützter Quellen/Senken-Matrix](copy-activity-overview.md)
- [Mapping Data Flow](concepts-data-flow-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [GetMetadata-Aktivität](control-flow-get-metadata-activity.md)
- [Delete-Aktivität](delete-activity.md)

Mit diesem Connector können Sie insbesondere:

- Dateien durch Verwendung eines Kontoschlüssels, eines Dienstprinzipals oder verwalteter Identitäten für die Authentifizierung von Azure-Ressourcen kopieren.
- Dateien im jeweiligen Zustand kopieren oder Dateien mit [unterstützten Dateiformaten und Komprimierungscodecs](supported-file-formats-and-compression-codecs.md) analysieren bzw. generieren.

>[!IMPORTANT]
>Wenn Sie die Option **Vertrauenswürdigen Microsoft-Diensten den Zugriff auf dieses Speicherkonto erlauben** in den Firewalleinstellungen von Azure Storage aktivieren und Azure Integration Runtime zum Herstellen einer Verbindung mit Data Lake Storage Gen2 verwenden möchten, müssen Sie die [Authentifizierung der verwalteten Identität](#managed-identity) für ADLS Gen2 verwenden.

>[!TIP]
>Wenn Sie den hierarchischen Namespace aktivieren, beachten Sie, dass derzeit keine Interoperabilität bei Vorgängen zwischen Blob- und Data Lake Storage Gen2-APIs besteht. Falls der Fehler „ErrorCode=FilesystemNotFound“ mit dem Meldungstext „Das angegebene Dateisystem ist nicht vorhanden“ auftritt, ist dies auf das angegebene Senkendateisystem zurückzuführen, das an anderer Stelle anstelle der Data Lake Storage Gen2-API über die Blob-API erstellt wurde. Geben Sie zum Beheben des Problems ein neues Dateisystem mit einem Namen an, der nicht als Name eines Blobcontainers vorhanden ist. Von Data Factory wird dieses Dateisystem automatisch während des Datenkopiervorgangs erstellt.

## <a name="get-started"></a>Erste Schritte

>[!TIP]
>Eine exemplarische Vorgehensweise zur Verwendung des Data Lake Storage Gen2-Connectors finden Sie unter [Laden von Daten in Azure Data Lake Storage Gen2](load-azure-data-lake-storage-gen2.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Informationen zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Data Lake Storage Gen2 verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Der Azure Data Lake Storage Gen2-Connector unterstützt die folgenden Authentifizierungstypen. Weitere Informationen finden Sie in den entsprechenden Abschnitten:

- [Kontoschlüsselauthentifizierung](#account-key-authentication)
- [Dienstprinzipalauthentifizierung](#service-principal-authentication)
- [Verwaltete Identitäten für Azure-Ressourcenauthentifizierung](#managed-identity)

>[!NOTE]
>Wenn Sie mit PolyBase Daten in SQL Data Warehouse laden und Ihr Quell-Data Lake Storage Gen2 mit einem Virtual Network-Endpunkt konfiguriert ist, müssen Sie zur Authentifizierung eine verwaltete Identität wie für PolyBase erforderlich verwenden. Im Abschnitt [Verwaltete Identitäten für Azure-Ressourcenauthentifizierung](#managed-identity) sind weitere Konfigurationsvoraussetzungen aufgeführt.

### <a name="account-key-authentication"></a>Kontoschlüsselauthentifizierung

Für die Verwendung der Authentifizierung mit dem Speicherkontoschlüssel werden die folgenden Eigenschaften unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **AzureBlobFS** festgelegt sein. |Ja |
| url | Endpunkt für Data Lake Storage Gen2 im Format `https://<accountname>.dfs.core.windows.net`. | Ja |
| accountKey | Kontoschlüssel für Data Lake Storage Gen2. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). |Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder eine selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn diese Eigenschaft nicht angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

>[!NOTE]
>Der sekundäre ADLS-Dateisystem-Endpunkt wird bei Verwendung der Kontoschlüsselauthentifizierung nicht unterstützt. Sie können andere Authentifizierungstypen verwenden.

**Beispiel:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "accountkey": { 
                "type": "SecureString", 
                "value": "<accountkey>" 
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

Zum Verwenden der Dienstprinzipalauthentifizierung führen Sie die folgenden Schritte aus.

1. Registrieren Sie eine Anwendungsentität in Azure Active Directory (Azure AD), indem Sie die Schritte unter [Registrieren der Anwendung bei einem Azure AD-Mandanten](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant) befolgen. Notieren Sie sich die folgenden Werte, die Sie zum Definieren des verknüpften Diensts verwenden:

    - Anwendungs-ID
    - Anwendungsschlüssel
    - Mandanten-ID

2. Erteilen Sie dem Dienstprinzipal geeignete Berechtigungen. Beispiele zur Funktionsweise von Berechtigungen in Data Lake Storage Gen2 finden Sie unter [Zugriffssteuerungslisten für Dateien und Verzeichnisse](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

    - **Als Quelle**: Weisen Sie im Storage-Explorer mindestens die Berechtigung **Ausführen** für ALLE Upstreamordner und das Dateisystem sowie die Berechtigung **Lesen** für die zu kopierenden Dateien zu. Weisen Sie alternativ in der Zugriffssteuerung (IAM) mindestens die Rolle **Storage-Blobdatenleser** zu.
    - **Als Senke**: Weisen Sie im Storage-Explorer mindestens die Berechtigung **Ausführen** für ALLE Upstreamordner und das Dateisystem sowie die Berechtigung **Schreiben** für den Senkenordner zu. Weisen Sie in der Zugriffssteuerung (IAM) mindestens die Rolle **Mitwirkender an Storage-Blobdaten** zu.

>[!NOTE]
>Wenn Sie die Data Factory-Benutzeroberfläche für die Erstellung verwenden und der Dienstprinzipal nicht mit der Rolle „Storage-Blobdatenleser/Mitwirkender an Storage-Blobdaten“ in IAM festgelegt ist, wählen Sie beim Testen der Verbindung oder beim Durchsuchen von/Navigieren in Ordnern die Option „Test connection to file path“ (Verbindung mit Dateipfad testen) bzw. „Browse from specified path“ (Von angegebenem Pfad suchen) aus. Geben Sie ein Dateisystem oder einen Pfad mit der Berechtigung „Ausführen“ an, um den Vorgang fortzusetzen.

Diese Eigenschaften werden im verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **AzureBlobFS** festgelegt sein. |Ja |
| url | Endpunkt für Data Lake Storage Gen2 im Format `https://<accountname>.dfs.core.windows.net`. | Ja |
| servicePrincipalId | Geben Sie die Client-ID der Anwendung an. | Ja |
| servicePrincipalKey | Geben Sie den Schlüssel der Anwendung an. Markieren Sie dieses Feld als `SecureString`, um es sicher in Data Factory zu speichern. Alternativ können Sie [auf ein in Azure Key Vault gespeichertes Geheimnis verweisen](store-credentials-in-key-vault.md). | Ja |
| tenant | Geben Sie die Mandanteninformationen (Domänenname oder Mandanten-ID) für Ihre Anwendung an. Diese können Sie abrufen, indem Sie im Azure-Portal mit der Maus auf den Bereich oben rechts zeigen. | Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder eine selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn kein Wert angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

**Beispiel:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
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

Eine Data Factory kann einer [verwalteten Identität für Azure-Ressourcen](data-factory-service-identity.md) zugeordnet werden, die diese spezielle Data Factory darstellt. Ähnlich wie bei der Verwendung Ihres eigenen Dienstprinzipals können Sie diese verwaltete Identität direkt für die Data Lake Storage Gen2-Authentifizierung verwenden. Sie erlaubt dieser bestimmten Factory den Zugriff auf Ihren Data Lake Storage Gen2 sowie das Kopieren von Daten nach oder aus Data Lake Storage Gen2.

Um verwaltete Identitäten für die Azure-Ressourcenauthentifizierung zu verwenden, gehen Sie folgendermaßen vor.

1. [Rufen Sie die verwalteten Data Factory-Identitätsinformationen ab](data-factory-service-identity.md#retrieve-managed-identity), indem Sie den Wert von **Dienstidentitätsanwendungs-ID** kopieren, der zusammen mit der Factory generiert wurde.

2. Erteilen Sie der verwalteten Entität geeignete Berechtigungen. Beispiele zur Funktionsweise von Berechtigungen in Data Lake Storage Gen2 finden Sie unter [Zugriffssteuerungslisten für Dateien und Verzeichnisse](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

    - **Als Quelle**: Weisen Sie im Storage-Explorer mindestens die Berechtigung **Ausführen** für ALLE Upstreamordner und das Dateisystem sowie die Berechtigung **Lesen** für die zu kopierenden Dateien zu. Weisen Sie alternativ in der Zugriffssteuerung (IAM) mindestens die Rolle **Storage-Blobdatenleser** zu.
    - **Als Senke**: Weisen Sie im Storage-Explorer mindestens die Berechtigung **Ausführen** für ALLE Upstreamordner und das Dateisystem sowie die Berechtigung **Schreiben** für den Senkenordner zu. Weisen Sie in der Zugriffssteuerung (IAM) mindestens die Rolle **Mitwirkender an Storage-Blobdaten** zu.

>[!NOTE]
>Wenn Sie die Data Factory-Benutzeroberfläche für die Erstellung verwenden und die verwaltete Identität nicht mit der Rolle „Storage-Blobdatenleser/Mitwirkender an Storage-Blobdaten“ in IAM festgelegt ist, wählen Sie beim Testen der Verbindung oder beim Durchsuchen von/Navigieren in Ordnern die Option „Test connection to file path“ (Verbindung mit Dateipfad testen) bzw. „Browse from specified path“ (Von angegebenem Pfad suchen) aus. Geben Sie ein Dateisystem oder einen Pfad mit der Berechtigung „Ausführen“ an, um den Vorgang fortzusetzen.

>[!IMPORTANT]
>Wenn Sie mit PolyBase Daten aus Data Lake Storage Gen2 in SQL Data Warehouse laden und eine verwaltete Identität zur Authentifizierung bei Data Lake Storage Gen2 verwenden, müssen Sie auch die Schritte 1 und 2 im [zugehörigen Leitfaden](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) ausführen. Registrieren Sie in Schritt 1 zuerst Ihren SQL-Datenbank-Server bei Azure Active Directory (Azure AD). Weisen Sie dann in Schritt 2 Ihrem SQL-Datenbank-Server die Rolle „Mitwirkender an Storage-Blobdaten“ zu. Alles Weitere erledigt Data Factory. Wenn Ihr Data Lake Storage Gen2 mit einem Azure Virtual Network-Endpunkt konfiguriert ist und Sie mit PolyBase Daten daraus laden, müssen Sie zur Authentifizierung eine verwaltete Identität wie für PolyBase erforderlich verwenden.

Diese Eigenschaften werden im verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft muss auf **AzureBlobFS** festgelegt sein. |Ja |
| url | Endpunkt für Data Lake Storage Gen2 im Format `https://<accountname>.dfs.core.windows.net`. | Ja |
| connectVia | Die [Integration Runtime](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden soll. Sie können die Azure Integration Runtime oder eine selbstgehostete Integration Runtime verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn kein Wert angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein |

**Beispiel:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md).

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Folgende Eigenschaften werden für Data Lake Storage Gen2 unter `location`-Einstellungen in formatbasierten Datasets unterstützt:

| Eigenschaft   | BESCHREIBUNG                                                  | Erforderlich |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Die „type“-Eigenschaft unter `location` im Dataset muss auf **AzureBlobFSLocation** festgelegt werden. | Ja      |
| fileSystem | Der Name des Data Lake Storage Gen2-Dateisystems.                              | Nein       |
| folderPath | Der Pfad zu einem Ordner im angegebenen Dateisystem. Wenn Sie einen Platzhalter verwenden möchten, um Ordner zu filtern, überspringen Sie diese Einstellung, und geben Sie entsprechende Aktivitätsquelleneinstellungen an. | Nein       |
| fileName   | Der Dateiname im angegebenen „fileSystem“ und „folderPath“. Wenn Sie einen Platzhalter verwenden möchten, um Dateien zu filtern, überspringen Sie diese Einstellung, und geben Sie ihn in den entsprechenden Aktivitätsquelleneinstellungen an. | Nein       |

**Beispiel:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobFSLocation",
                "fileSystem": "filesystemname",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="legacy-dataset-model"></a>Legacy-Datasetmodell

>[!NOTE]
>Das folgende Datasetmodell wird aus Gründen der Abwärtskompatibilität weiterhin unverändert unterstützt. Es wird jedoch empfohlen, in Zukunft das im Abschnitt oben erwähnte neue Modell zu verwenden, da das neue Modell nun von der Benutzeroberfläche für die ADF-Dokumentenerstellung generiert wird.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft des Datasets muss auf **AzureBlobFSFile** festgelegt sein. |Ja |
| folderPath | Pfad zum Ordner in Data Lake Storage Gen2. Wenn keine Angabe vorhanden ist, wird auf das Stammverzeichnis verwiesen. <br/><br/>Der Platzhalterfilter wird unterstützt. Folgende Platzhalter sind zulässig: `*` (entspricht null [0] oder mehr Zeichen) und `?` (entspricht null [0] oder einem einzelnen Zeichen). Verwenden Sie `^` als Escapezeichen, wenn der tatsächliche Ordnername einen Platzhalter oder dieses Escapezeichen enthält. <br/><br/>Beispiele: „Dateisystem/Ordner/“. Weitere Beispiele finden Sie unter [Beispiele für Ordner- und Dateifilter](#folder-and-file-filter-examples). |Nein |
| fileName | Name oder Platzhalterfilter für die Dateien unter dem angegebenen Wert für „folderPath“. Wenn Sie für diese Eigenschaft keinen Wert angeben, verweist das Dataset auf alle Dateien im Ordner. <br/><br/>Für Filter sind die Platzhalter `*` (entspricht null [0] oder mehr Zeichen) und `?` (entspricht null [0] oder einem einzelnen Zeichen) zulässig.<br/>- Beispiel 1: `"fileName": "*.csv"`<br/>- Beispiel 2: `"fileName": "???20180427.txt"`<br/>Verwenden Sie `^` als Escapezeichen, wenn der tatsächliche Dateiname einen Platzhalter oder dieses Escapezeichen enthält.<br/><br/>Wenn „fileName“ nicht für ein Ausgabedataset und **preserveHierarchy** nicht in der Aktivitätssenke angegeben sind, generiert die Kopieraktivität den Dateinamen automatisch mit dem folgenden Muster: „*Data.[GUID der Aktivitätsausführungs-ID].[GUID, sofern „FlattenHierarchy“].[Format, sofern konfiguriert].[Komprimierung, sofern konfiguriert]* “, z.B. „Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz“. Wenn Sie Daten aus einer Quelle im Tabellenformat kopieren und dabei anstelle einer Abfrage den Tabellennamen verwenden, lautet das Namensmuster „ *[Tabellenname].[Format].[Komprimierung, sofern konfiguriert]* “, z.B. „MyTable.csv“. |Nein |
| modifiedDatetimeStart | Dateifilterung basierend auf dem Attribut „Letzte Änderung“. Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br/><br/> Die generelle Leistung der Datenverschiebung wird beeinträchtigt, wenn Sie diese Einstellung aktivieren und eine Dateifilterung für eine große Zahl von Dateien vornehmen möchten. <br/><br/> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird. Wenn `modifiedDatetimeStart` einen datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist. Wenn `modifiedDatetimeEnd` einen datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist.| Nein |
| modifiedDatetimeEnd | Dateifilterung basierend auf dem Attribut „Letzte Änderung“. Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br/><br/> Die generelle Leistung der Datenverschiebung wird beeinträchtigt, wenn Sie diese Einstellung aktivieren und eine Dateifilterung für eine große Zahl von Dateien vornehmen möchten. <br/><br/> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird. Wenn `modifiedDatetimeStart` einen datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist. Wenn `modifiedDatetimeEnd` einen datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist.| Nein |
| format | Wenn Sie Dateien unverändert zwischen dateibasierten Speichern kopieren möchten (binäre Kopie), überspringen Sie den Formatabschnitt in den Definitionen der Eingabe- und Ausgabedatasets.<br/><br/>Für das Analysieren oder Generieren von Dateien mit einem bestimmten Format werden die folgenden Dateiformattypen unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** und **ParquetFormat**. Sie müssen die **type**-Eigenschaft unter **format** auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](supported-file-formats-and-compression-codecs.md#text-format), [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format), [Avro-Format](supported-file-formats-and-compression-codecs.md#avro-format), [ORC-Format](supported-file-formats-and-compression-codecs.md#orc-format) und [Parquet-Format](supported-file-formats-and-compression-codecs.md#parquet-format). |Nein (nur für Szenarien mit Binärkopien) |
| compression | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Weitere Informationen finden Sie unter [Unterstützte Dateiformate und Codecs für die Komprimierung](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Unterstützte Typen sind **GZip**, **Deflate**, **BZIP2** und **ZipDeflate**.<br/>Unterstützte Grade sind **Optimal** und **Schnellste**. |Nein |

>[!TIP]
>Wenn Sie alle Dateien eines Ordners kopieren möchten, geben Sie nur **folderPath** an.<br>Wenn Sie eine einzelne Datei mit einem bestimmten Namen kopieren möchten, geben Sie **folderPath** mit einem Ordner und **fileName** mit einem Dateinamen an.<br>Wenn Sie eine Teilmenge der Dateien eines Ordners kopieren möchten, geben Sie **folderPath** mit einem Ordner und **fileName** mit einem Platzhalterfilter an. 

**Beispiel:**

```json
{
    "name": "ADLSGen2Dataset",
    "properties": {
        "type": "AzureBlobFSFile",
        "linkedServiceName": {
            "referenceName": "<Azure Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "myfilesystem/myfolder",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie unter [Konfigurationen für die Kopieraktivität](copy-activity-overview.md#configuration) und [Pipelines und Aktivitäten](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Data Lake Storage Gen2-Quelle und -Senke unterstützt werden.

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Azure Data Lake Storage Gen2 als Quelltyp

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Folgende Eigenschaften werden für Data Lake Storage Gen2 unter `storeSettings`-Einstellungen in der formatbasierten Kopierquelle unterstützt:

| Eigenschaft                 | BESCHREIBUNG                                                  | Erforderlich                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | Die „type“-Eigenschaft unter `storeSettings` muss auf **AzureBlobFSReadSetting** festgelegt werden. | Ja                                           |
| recursive                | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. Wenn „recursive“ auf „true“ festgelegt ist und es sich bei der Senke um einen dateibasierten Speicher handelt, wird ein leerer Ordner oder Unterordner nicht in die Senke kopiert oder dort erstellt. Zulässige Werte sind **true** (Standard) und **false**. | Nein                                            |
| wildcardFolderPath       | Der in Datasets zum Filtern von Quellordnern konfigurierte Ordnerpfad mit Platzhalterzeichen im angegebenen Dateisystem. <br>Folgende Platzhalter sind zulässig: `*` (entspricht null [0] oder mehr Zeichen) und `?` (entspricht null [0] oder einem einzelnen Zeichen). Verwenden Sie `^` als Escapezeichen, wenn der tatsächliche Ordnername einen Platzhalter oder dieses Escapezeichen enthält. <br>Weitere Beispiele finden Sie unter [Beispiele für Ordner- und Dateifilter](#folder-and-file-filter-examples). | Nein                                            |
| wildcardFileName         | Der Dateiname mit Platzhalterzeichen im angegebenen Dateisystem und „folderPath/wildcardFolderPath“ zum Filtern von Quelldateien. <br>Folgende Platzhalter sind zulässig: `*` (entspricht null [0] oder mehr Zeichen) und `?` (entspricht null [0] oder einem einzelnen Zeichen). Verwenden Sie `^` als Escapezeichen, wenn der tatsächliche Ordnername einen Platzhalter oder dieses Escapezeichen enthält. Weitere Beispiele finden Sie unter [Beispiele für Ordner- und Dateifilter](#folder-and-file-filter-examples). | Ja, wenn `fileName` nicht im Dataset angegeben ist |
| modifiedDatetimeStart    | Dateifilterung basierend auf dem Attribut „Letzte Änderung“. Die Dateien werden ausgewählt, wenn der Zeitpunkt der letzten Änderung innerhalb des Zeitbereichs zwischen `modifiedDatetimeStart` und `modifiedDatetimeEnd` liegt. Die Zeit wird auf die UTC-Zeitzone im Format „2018-12-01T05:00:00Z“ angewandt. <br> Die Eigenschaften können NULL sein, was bedeutet, dass kein Dateiattributfilter auf das Dataset angewandt wird. Wenn `modifiedDatetimeStart` einen datetime-Wert aufweist, aber `modifiedDatetimeEnd` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung größer oder gleich dem datetime-Wert ist. Wenn `modifiedDatetimeEnd` einen datetime-Wert aufweist, aber `modifiedDatetimeStart` NULL ist, bedeutet dies, dass die Dateien ausgewählt werden, deren Attribut für die letzte Änderung kleiner als der datetime-Wert ist. | Nein                                            |
| modifiedDatetimeEnd      | Wie oben.                                               | Nein                                            |
| maxConcurrentConnections | Die Anzahl von Verbindungen, die gleichzeitig mit einem Speicher hergestellt werden können. Geben Sie diesen Wert nur an, wenn Sie die gleichzeitigen Verbindungen mit dem Datenspeicher begrenzen möchten. | Nein                                            |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
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
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureBlobFSReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="legacy-source-model"></a>Legacy-Quellmodell

>[!NOTE]
>Das folgende Modell für die Kopierquelle wird aus Gründen der Abwärtskompatibilität weiterhin unverändert unterstützt. Es wird jedoch empfohlen, in Zukunft das oben erwähnte neue Modell zu verwenden, da das neue Modell nun von der Benutzeroberfläche für die ADF-Dokumentenerstellung generiert wird.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft der Quelle der Kopieraktivität muss auf **AzureBlobFSSource** festgelegt sein. |Ja |
| recursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. Wenn „recursive“ auf „true“ festgelegt ist und es sich bei der Senke um einen dateibasierten Speicher handelt, wird ein leerer Ordner oder Unterordner nicht in die Senke kopiert oder dort erstellt.<br/>Zulässige Werte sind **true** (Standard) und **false**. | Nein |
| maxConcurrentConnections | Die Anzahl von Verbindungen, die gleichzeitig mit dem Datenspeicher hergestellt werden können. Geben Sie diesen Wert nur an, wenn Sie die gleichzeitigen Verbindungen mit dem Datenspeicher begrenzen möchten. | Nein |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen2 input dataset name>",
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
                "type": "AzureBlobFSSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-storage-gen2-as-a-sink-type"></a>Azure Data Lake Storage Gen2 als Senkentyp

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

Folgende Eigenschaften werden für Data Lake Storage Gen2 unter `storeSettings`-Einstellungen in formatbasierten Kopiersenken unterstützt:

| Eigenschaft                 | BESCHREIBUNG                                                  | Erforderlich |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | Die „type“-Eigenschaft unter `storeSettings` muss auf **AzureBlobFSWriteSetting** festgelegt werden. | Ja      |
| copyBehavior             | Definiert das Kopierverhalten, wenn es sich bei der Quelle um Dateien aus einem dateibasierten Datenspeicher handelt.<br/><br/>Zulässige Werte sind:<br/><b>- PreserveHierarchy (Standard)</b>: Behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner ist mit dem relativen Pfad der Zieldatei zum Zielordner identisch.<br/><b>- FlattenHierarchy</b>: Alle Dateien aus dem Quellordner befinden sich auf der ersten Ebene des Zielordners. Die Namen für die Zieldateien werden automatisch generiert. <br/><b>- MergeFiles</b>: Alle Dateien aus dem Quellordner werden in einer Datei zusammengeführt. Wenn der Dateiname angegeben wurde, entspricht der zusammengeführte Dateiname dem angegebenen Namen. Andernfalls wird der Dateiname automatisch generiert. | Nein       |
| maxConcurrentConnections | Die Anzahl von Verbindungen, die gleichzeitig mit dem Datenspeicher hergestellt werden können. Geben Sie diesen Wert nur an, wenn Sie die gleichzeitigen Verbindungen mit dem Datenspeicher begrenzen möchten. | Nein       |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "AzureBlobFSWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="legacy-sink-model"></a>Legacy-Senkenmodell

>[!NOTE]
>Das folgende Modell für die Kopiersenke wird aus Gründen der Abwärtskompatibilität weiterhin unverändert unterstützt. Es wird jedoch empfohlen, in Zukunft das oben erwähnte neue Modell zu verwenden, da das neue Modell nun von der Benutzeroberfläche für die ADF-Dokumentenerstellung generiert wird.

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die „type“-Eigenschaft der Senke der Kopieraktivität muss auf **AzureBlobFSSink** festgelegt sein. |Ja |
| copyBehavior | Definiert das Kopierverhalten, wenn es sich bei der Quelle um Dateien aus einem dateibasierten Datenspeicher handelt.<br/><br/>Zulässige Werte sind:<br/><b>- PreserveHierarchy (Standard)</b>: Behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner ist mit dem relativen Pfad der Zieldatei zum Zielordner identisch.<br/><b>- FlattenHierarchy</b>: Alle Dateien aus dem Quellordner befinden sich auf der ersten Ebene des Zielordners. Die Namen für die Zieldateien werden automatisch generiert. <br/><b>- MergeFiles</b>: Alle Dateien aus dem Quellordner werden in einer Datei zusammengeführt. Wenn der Dateiname angegeben wurde, entspricht der zusammengeführte Dateiname dem angegebenen Namen. Andernfalls wird der Dateiname automatisch generiert. | Nein |
| maxConcurrentConnections | Die Anzahl von Verbindungen, die gleichzeitig mit dem Datenspeicher hergestellt werden können. Geben Sie diesen Wert nur an, wenn Sie die gleichzeitigen Verbindungen mit dem Datenspeicher begrenzen möchten. | Nein |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Beispiele für Ordner- und Dateifilter

Dieser Abschnitt beschreibt das resultierende Verhalten für den Ordnerpfad und den Dateinamen mit Platzhalterfiltern.

| folderPath | fileName | recursive | Quellordnerstruktur und Filterergebnis (Dateien mit **Fettformatierung** werden abgerufen.)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (Leer, Standardwert verwenden) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5.csv<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `Folder*` | (Leer, Standardwert verwenden) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei5.csv**<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5.csv<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Datei1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Datei5.csv**<br/>AndererOrdnerB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei6.csv |

### <a name="some-recursive-and-copybehavior-examples"></a>Beispiele für „recursive“ und „copyBehavior“

Dieser Abschnitt beschreibt das resultierende Verhalten des Kopiervorgangs für verschiedene Kombinationen von „recursive“- und „copyBehavior“-Werten.

| recursive | copyBehavior | Struktur des Quellordners | Resultierendes Ziel |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der gleichen Struktur erstellt wie die Quelle:<br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 |
| true |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei5 |
| true |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Die Inhalte von Datei1 + Datei2 + Datei3 + Datei4 + Datei5 werden in einer Datei mit einem automatisch generierten Dateinamen zusammengeführt. |
| false |preserveHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |flattenHierarchy | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |mergeFiles | Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;Datei5 | Der Zielordner „Ordner1“ wird mit der folgenden Struktur erstellt: <br/><br/>Folder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Die Inhalte von Datei1 + Datei2 werden in einer Datei mit einem automatisch generierten Dateinamen zusammengeführt. Automatisch generierter Name für Datei1<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |

## <a name="preserve-acls-from-data-lake-storage-gen1"></a>Bewahren von Zugriffssteuerungslisten für Azure Data Lake Storage Gen1

>[!TIP]
>Allgemeine Informationen zum Kopieren von Daten von Azure Data Lake Storage Gen1 in Gen2 sowie eine exemplarische Vorgehensweise und bewährte Methoden finden Sie unter [Kopieren von Daten aus Azure Data Lake Storage Gen1 in Gen2 mit Azure Data Factory](load-azure-data-lake-storage-gen2-from-gen1.md).

Beim Kopieren von Dateien von Azure Data Lake Storage Gen1 in Gen2 können Sie die Option wählen, bei der die POSIX-Zugriffssteuerungslisten (Access Control Lists, ACLs) zusammen mit den Daten beibehalten werden. Weitere Informationen zur Zugriffssteuerung finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md) und [Zugriffssteuerung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md).

Mit der Azure Data Factory-Kopieraktivität können die folgenden Arten von ACLs beibehalten werden. Sie können einen oder mehrere Typen auswählen:

- **ACL**: Kopieren und Beibehalten von POSIX-Zugriffssteuerungslisten für Dateien und Verzeichnisse. Hierbei werden die vollständigen vorhandenen ACLs von der Quelle in die Senke kopiert. 
- **Besitzer:** Kopieren und Beibehalten des besitzenden Benutzers von Dateien und Verzeichnissen. Hierfür ist Administratorzugriff auf die Data Lake Storage Gen2-Senke erforderlich.
- **Gruppe**: Kopieren und Beibehalten der besitzenden Gruppe von Dateien und Verzeichnissen. Hierfür ist Administratorzugriff auf die Data Lake Storage Gen2-Senke oder der Zugriff des besitzenden Benutzers erforderlich (sofern der besitzende Benutzer auch Mitglied der Zielgruppe ist).

Wenn Sie das Kopieren aus einem Ordner angeben, repliziert Data Factory die ACLs für diesen Ordner und die darin enthaltenen Dateien und Verzeichnisse, sofern `recursive` auf „true“ festgelegt ist. Falls Sie das Kopieren aus einer einzelnen Datei angeben, werden die ACLs in dieser Datei kopiert.

>[!IMPORTANT]
>Bei Auswahl der Beibehaltung von ACLs sollten Sie sicherstellen, dass Sie ausreichende Berechtigungen für die Verwendung von Data Factory für Ihr Data Lake Storage Gen2-Senkenkonto gewähren. Verwenden Sie beispielsweise die Kontoschlüsselauthentifizierung, oder weisen Sie dem Dienstprinzipal bzw. der verwalteten Identität die Rolle „Besitzer von Speicherblobdaten“ zu.

Wenn Sie die Quelle als Data Lake Storage Gen1 mit binärer Kopieroption oder binärem Format und die Senke als Data Lake Storage Gen2 mit binärer Kopieroption oder binärem Format konfigurieren, können Sie die Option **Beibehalten** auf der Seite mit den **Einstellungen für das Tool zum Kopieren von Daten** oder auf der Registerkarte **Kopieraktivität** > **Einstellungen** für die Aktivitätserstellung verwenden.

![Bewahren von ACL von Data Lake Storage Gen1 auf Gen2](./media/connector-azure-data-lake-storage/adls-gen2-preserve-acl.png)

Hier ist ein Beispiel für die JSON-Konfiguration angegeben (siehe `preserve`): 

```json
"activities":[
    {
        "name": "CopyFromGen1ToGen2",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            },
            "preserve": [
                "ACL",
                "Owner",
                "Group"
            ]
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Lake Storage Gen1 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Lake Storage Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="mapping-data-flow-properties"></a>Eigenschaften von Mapping Data Flow

Weitere Informationen zur [Quellentransformation](data-flow-source.md) und [Senkentransformation](data-flow-sink.md) finden Sie unter der Mapping Data Flow-Funktion.

## <a name="lookup-activity-properties"></a>Eigenschaften der Lookup-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [Lookup-Aktivität](control-flow-lookup-activity.md).

## <a name="getmetadata-activity-properties"></a>Eigenschaften der GetMetadata-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [GetMetadata-Aktivität](control-flow-get-metadata-activity.md). 

## <a name="delete-activity-properties"></a>Eigenschaften der Delete-Aktivität

Ausführliche Informationen zu den Eigenschaften finden Sie unter [Delete-Aktivität](delete-activity.md).
## <a name="next-steps"></a>Nächste Schritte

Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).
