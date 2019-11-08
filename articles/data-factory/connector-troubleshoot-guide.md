---
title: Problembehandlung für Azure Data Factory-Connectors
description: Es wird beschrieben, wie Sie in Azure Data Factory Connectorprobleme beheben.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 08/26/2019
ms.author: jingwang
ms.reviewer: craigg
ms.openlocfilehash: 8cabc1031f9d0be772ba087109ae1dfc881ce163
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73680080"
---
# <a name="troubleshoot-azure-data-factory-connectors"></a>Problembehandlung für Azure Data Factory-Connectors

In diesem Artikel werden die gängigen Problembehandlungsmethoden für Connectors in Azure Data Factory beschrieben.

## <a name="azure-data-lake-storage"></a>Azure Data Lake Store

### <a name="error-message-the-remote-server-returned-an-error-403-forbidden"></a>Fehlermeldung: Der Remoteserver hat einen Fehler zurückgegeben: (403) Unzulässig

- **Symptome:** Für die Kopieraktivität tritt der folgende Fehler auf: 

    ```
    Message: The remote server returned an error: (403) Forbidden.. 
    Response details: {"RemoteException":{"exception":"AccessControlException""message":"CREATE failed with error 0x83090aa2 (Forbidden. ACL verification failed. Either the resource does not exist or the user is not authorized to perform the requested operation.)....
    ```

- **Ursache:** Eine mögliche Ursache ist, dass der von Ihnen verwendete Dienstprinzipal oder die verwaltete Identität nicht über die Berechtigung zum Zugreifen auf den jeweiligen Ordner bzw. die Datei verfügt.

- **Lösung:** Gewähren Sie die entsprechenden Berechtigungen für alle Ordner und Unterordner, die Sie kopieren müssen. Weitere Informationen finden Sie in [diesem Dokument](connector-azure-data-lake-store.md#linked-service-properties).

### <a name="error-message-failed-to-get-access-token-by-using-service-principal-adal-error-service_unavailable"></a>Fehlermeldung: Fehler beim Abrufen des Zugriffstokens mit dem Dienstprinzipal. ADAL-Fehler: service_unavailable

- **Symptome**: Für die Kopieraktivität tritt der folgende Fehler auf:

    ```
    Failed to get access token by using service principal. 
    ADAL Error: service_unavailable, The remote server returned an error: (503) Server Unavailable.
    ```

- **Ursache:** Wenn der Tokenserver des Diensts von Azure Active Directory nicht verfügbar ist – also die Auslastung für die Verarbeitung von Anforderungen zu hoch ist –, wird der HTTP-Fehler 503 zurückgegeben. 

- **Lösung:** Führen Sie die Kopieraktivität nach einigen Minuten erneut aus.

## <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

### <a name="error-message-conversion-failed-when-converting-from-a-character-string-to-uniqueidentifier"></a>Fehlermeldung: Fehler beim Konvertieren einer Zeichenfolge in „uniqueidentifier“.

- **Symptome:** Wenn Sie Daten von der Tabellendatenquelle (z. B. SQL Server) in Azure SQL Data Warehouse kopieren, indem Sie das gestaffelte Kopieren und PolyBase verwenden, tritt der folgende Fehler auf:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into SQL Data Warehouse.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Conversion failed when converting from a character string to uniqueidentifier...
    ```

- **Ursache:** Mit Azure SQL Data Warehouse PolyBase kann eine leere Zeichenfolge nicht in eine GUID konvertiert werden.

- **Lösung:** Legen Sie in der Senke der Kopieraktivität unter den PolyBase-Einstellungen die Option „**use type default**“ auf „false“ fest.

### <a name="error-message-expected-data-type-decimalxx-offending-value"></a>Fehlermeldung: Erwarteter Datentyp: DECIMAL(x,x), Auslösender Wert

- **Symptome:** Wenn Sie Daten von der Tabellendatenquelle (z. B. SQL Server) in SQL DW kopieren, indem Sie das gestaffelte Kopieren und PolyBase verwenden, tritt der folgende Fehler auf:

    ```
    ErrorCode=FailedDbOperation,Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
    Message=Error happened when loading data into SQL Data Warehouse.,
    Source=Microsoft.DataTransfer.ClientLibrary,Type=System.Data.SqlClient.SqlException,
    Message=Query aborted-- the maximum reject threshold (0 rows) was reached while reading from an external source: 1 rows rejected out of total 415 rows processed. (/file_name.txt) 
    Column ordinal: 18, Expected data type: DECIMAL(x,x), Offending value:..
    ```

- **Ursache:** Azure SQL Data Warehouse PolyBase kann in eine Dezimalspalte keine leere Zeichenfolge (NULL-Wert) einfügen.

- **Lösung:** Legen Sie in der Senke der Kopieraktivität unter den PolyBase-Einstellungen die Option „**use type default**“ auf „false“ fest.

### <a name="error-message-java-exception-messagehdfsbridgecreaterecordreader"></a>Fehlermeldung: Java-Ausnahmemeldung: HdfsBridge::CreateRecordReader

- **Symptome:** Sie kopieren Daten mit PolyBase in Azure SQL Data Warehouse und erhalten den folgenden Fehler:

    ```
    Message=110802;An internal DMS error occurred that caused this operation to fail. 
    Details: Exception: Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException, 
    Message: Java exception raised on call to HdfsBridge_CreateRecordReader. 
    Java exception message:HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.: Error [HdfsBridge::CreateRecordReader - Unexpected error encountered creating the record reader.] occurred while accessing external file.....
    ```

- **Ursache:** Die mögliche Ursache ist, dass das Schema (gesamte Spaltenbreite) zu groß ist (größer als 1 MB). Überprüfen Sie das Schema der SQL DW-Zieltabelle, indem Sie die Größe aller Spalten hinzufügen:

    - Int -> 4 Byte
    - Bigint -> 8 Byte
    - Varchar(n),char(n),binary(n), varbinary(n) -> N Byte
    - Nvarchar(n), nchar(n) -> N*2 Byte
    - Date -> 6 Byte
    - Datetime/(2), smalldatetime -> 16 Byte
    - Datetimeoffset -> 20 Byte
    - Decimal -> 19 Byte
    - Float -> 8 Byte
    - Money -> 8 Byte
    - Smallmoney -> 4 Byte
    - Real -> 4 Byte
    - Smallint -> 2 Byte
    - Time -> 12 Byte
    - Tinyint -> 1 Byte

- **Lösung:** Reduzieren Sie die Spaltenbreite auf weniger als 1 MB.

- Sie können auch das Masseneinfügen verwenden, indem Sie PolyBase deaktivieren.

### <a name="error-message-the-condition-specified-using-http-conditional-headers-is-not-met"></a>Fehlermeldung: Die mit bedingtem HTTP-Header angegebene Bedingung ist nicht erfüllt.

- **Symptome:** Sie verwenden die SQL-Abfrage zum Abrufen von Daten per Pullvorgang aus Azure SQL Data Warehouse und erhalten den folgenden Fehler:

    ```
    ...StorageException: The condition specified using HTTP conditional header(s) is not met...
    ```

- **Ursache:** Für Azure SQL Data Warehouse ist beim Abfragen der externen Tabelle in Azure Storage ein Fehler aufgetreten.

- **Lösung:** Führen Sie die gleiche Abfrage in SSMS aus, und überprüfen Sie, ob das gleiche Ergebnis angezeigt wird. Wenn ja: Erstellen Sie ein Supportticket für Azure SQL Data Warehouse, und geben Sie für die weitere Problembehandlung den Namen Ihres SQL DW-Servers und der Datenbank an.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

### <a name="error-message-request-size-is-too-large"></a>Fehlermeldung: Anforderung ist zu groß.

- **Symptome:** Sie kopieren Daten mit der Standardgröße für Batchvorgänge in Azure Cosmos DB und erhalten einen Fehler der Art *„**Anforderung ist zu groß**“* .

- **Ursache:** In Cosmos DB ist die Größe einer einzelnen Anforderung auf 2 MB begrenzt. Die Formel lautet: Anforderungsgröße = Einzeldokumentgröße * Schreibbatchgröße. Wenn Ihr Dokument zu groß ist, ist beim Standardverhalten auch die Anforderung zu groß. Sie können die Batchgröße für Schreibvorgänge optimieren.

- **Lösung:** Reduzieren Sie in der Senke der Kopieraktivität den Wert für die „Schreibbatchgröße“ (Standardwert ist „10.000“).

### <a name="error-message-unique-index-constraint-violation"></a>Fehlermeldung: Einschränkungsverletzung für eindeutigen Index

- **Symptome:** Beim Kopieren von Daten in Cosmos DB tritt der folgende Fehler auf:

    ```
    Message=Partition range id 0 | Failed to import mini-batch. 
    Exception was Message: {"Errors":["Encountered exception while executing function. Exception &#61; Error: {\"Errors\":[\"Unique index constraint violation.\"]}... 
    ```

- **Ursache:** Es gibt zwei mögliche Ursachen:

    - Wenn Sie **Einfügen** als Schreibverhalten verwenden, bedeutet dieser Fehler, dass Ihre Quelldaten Zeilen/Objekte mit der gleichen ID enthalten.

    - Wenn Sie **Upsert** als Schreibverhalten verwenden und einen anderen eindeutigen Schlüssel für den Container festlegen, bedeutet dieser Fehler, dass Ihre Quelldaten über Zeilen/Objekte mit unterschiedlichen IDs verfügen, aber für den definierten eindeutigen Schlüssel den gleichen Wert aufweisen.

- **Lösung:** 

    - Legen Sie für Ursache 1 die Option **Upsert** als Schreibverhalten fest.
    - Stellen Sie für Ursache 2 sicher, dass jedes Dokument über einen anderen Wert für den definierten eindeutigen Schlüssel verfügt.

### <a name="error-message-request-rate-is-large"></a>Fehlermeldung: Anforderungsrate ist hoch

- **Symptome:** Beim Kopieren von Daten in Cosmos DB tritt der folgende Fehler auf:

    ```
    Type=Microsoft.Azure.Documents.DocumentClientException,
    Message=Message: {"Errors":["Request rate is large"]}
    ```

- **Ursache:** Die verwendeten Anforderungseinheiten sind größer als die verfügbare Anforderungseinheit in Cosmos DB. Informationen zur Berechnung von Anforderungseinheiten in Cosmos DB finden Sie [hier](../cosmos-db/request-units.md#request-unit-considerations).

- **Lösung:** Hier sind zwei Lösungen beschrieben:

    1. **Legen Sie in Cosmos DB für die Anforderungseinheit für Container einen höheren Wert fest**, um die Leistung der Kopieraktivität zu verbessern, auch wenn hierdurch der Kostenaufwand in Cosmos DB erhöht wird. 

    2. Reduzieren Sie **writeBatchSize** auf einen niedrigeren Wert (z. B. 1.000), und legen Sie auch **parallelCopies** auf einen niedrigeren Wert fest (z. B. 1). Hierdurch verschlechtert sich die Leistung für die Ausführung des Kopiervorgangs, aber in Cosmos DB fallen keine höheren Kosten an.

### <a name="column-missing-in-column-mapping"></a>Fehlende Spalte in Spaltenzuordnung

- **Symptome:** Beim Importieren des Schemas für Cosmos DB für die Spaltenzuordnung fehlen einige Spalten. 

- **Ursache:** ADF leitet das Schema aus den ersten zehn Cosmos DB-Dokumenten ab. Falls einige Spalten/Eigenschaften in diesen Dokumenten nicht über einen Wert verfügen, werden sie von ADF nicht erkannt und somit auch nicht angezeigt.

- **Lösung:** Sie können die Abfrage wie unten angegeben optimieren, um zu erzwingen, dass die Spalte im Resultset mit einem leeren Wert angezeigt wird (Annahme: Spalte „impossible“ fehlt in den ersten zehn Dokumenten). Alternativ können Sie die Spalte auch manuell für die Zuordnung hinzufügen.

    ```sql
    select c.company, c.category, c.comments, (c.impossible??'') as impossible from c
    ```

### <a name="error-message-the-guidrepresentation-for-the-reader-is-csharplegacy"></a>Fehlermeldung: GuidRepresentation für den Reader ist CSharpLegacy

- **Symptome:** Beim Kopieren von Daten aus Cosmos DB MongoAPI/MongoDB mit UUID-Feld tritt der folgende Fehler auf:

    ```
    Failed to read data via MongoDB client.,
    Source=Microsoft.DataTransfer.Runtime.MongoDbV2Connector,Type=System.FormatException,
    Message=The GuidRepresentation for the reader is CSharpLegacy which requires the binary sub type to be UuidLegacy not UuidStandard.,Source=MongoDB.Bson,’“,
    ```

- **Ursache:** Es gibt zwei Möglichkeiten, UUID in BSON darzustellen: UuidStandard und UuidLegacy. Standardmäßig wird UuidLegacy zum Lesen von Daten verwendet. Ein Fehler tritt auf, wenn für Ihre UUID-Daten in MongoDB „UuidStandard“ festgelegt ist.

- **Lösung:** Fügen Sie der MongoDB-Verbindungszeichenfolge die Option „**uuidRepresentation=standard**“ hinzu. Weitere Informationen finden Sie im Artikel zur [MongoDB-Verbindungszeichenfolge](connector-mongodb.md#linked-service-properties).

## <a name="sftp"></a>SFTP

### <a name="error-message-invalid-sftp-credential-provided-for-sshpublickey-authentication-type"></a>Fehlermeldung: Für den Authentifizierungstyp „SshPublicKey“ wurden ungültige SFTP-Anmeldeinformationen angegeben.

- **Symptome:** Sie verwenden die `SshPublicKey`-Authentifizierung und erhalten den folgenden Fehler:

    ```
    Invalid Sftp credential provided for 'SshPublicKey' authentication type
    ```

- **Ursache:** Es gibt drei mögliche Ursachen:

    1. Wenn Sie die ADF-Benutzeroberfläche für die Erstellung verwenden, um den verknüpften SFTP-Dienst zu erstellen, bedeutet dieser Fehler, dass der von Ihnen gewählte Schlüssel das falsche Format hat. Sie können für den privaten SSH-Schlüssel das PKCS#8-Format wählen. Beachten Sie aber, dass für ADF nur das herkömmliche SSH-Schlüsselformat unterstützt wird. Genauer gesagt: Der Unterschied zwischen dem PKCS#8-Format und dem herkömmlichen Schlüsselformat besteht darin, dass PKCS#8-Schlüsselinhalt mit „ *-----BEGIN ENCRYPTED PRIVATE KEY-----* “ beginnt, während beim herkömmlichen Schlüsselformat „ *-----BEGIN RSA PRIVATE KEY-----* “ verwendet wird.
    2. Wenn Sie Azure Key Vault zum Speichern des privaten Schlüsselinhalts oder einen programmgesteuerten Ansatz zum Erstellen des verknüpften SFTP-Diensts verwenden, bedeutet dieser Fehler, dass der Inhalt des privaten Schlüssels fehlerhaft ist. Wahrscheinlich wurde keine Base64-Codierung durchgeführt.
    3. Anmeldeinformationen oder Inhalt des privaten Schlüssels ungültig.

- **Lösung:** 

    - Führen Sie für Ursache 1 die folgenden Befehle aus, um den Schlüssel in das herkömmliche Schlüsselformat zu konvertieren und dann auf der ADF-Benutzeroberfläche für die Erstellung zu nutzen.

        ```
        # Decrypt the pkcs8 key and convert the format to traditional key format
        openssl pkcs8 -in pkcs8_format_key_file -out traditional_format_key_file

        chmod 600 traditional_format_key_file

        # Re-encrypte the key file using passphrase
        ssh-keygen -f traditional_format_key_file -p
        ```

    - Für Ursache 2 hat der Kunde zum Generieren dieser Zeichenfolge die folgenden beiden Möglichkeiten:
    - Verwendung eines Base64-Konvertierungstools eines Drittanbieters: Fügen Sie den gesamten Inhalt des privaten Schlüssels in Tools wie [Base64 Encode/Decode](https://www.base64encode.org/) ein, und führen Sie die Codierung in eine Zeichenfolge im Base64-Format durch. Fügen Sie diese Zeichenfolge dann in einen Schlüsseltresor ein, oder verwenden Sie diesen Wert für die programmgesteuerte Erstellung eines verknüpften SFTP-Diensts.
    - Verwendung von C#-Code:

        ```c#
        byte[] keyContentBytes = File.ReadAllBytes(privateKeyPath);
        string keyContent = Convert.ToBase64String(keyContentBytes, Base64FormattingOptions.None);
        ```

    - Überprüfen Sie für Ursache 3 mit anderen Tools, ob die Schlüsseldatei bzw. das Kennwort korrekt ist. So können Sie sich vergewissern, ob hiermit der fehlerfreie Zugriff auf den SFTP-Server möglich ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Problembehandlung finden Sie in diesen Ressourcen:

*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN-Forum](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)



