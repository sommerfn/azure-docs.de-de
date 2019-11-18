---
title: Änderungsfeed in Azure Blob Storage (Vorschau) | Microsoft-Dokumentation
description: Erfahren Sie mehr über Änderungsfeedprotokolle in Azure Blob Storage und deren Verwendung.
author: normesta
ms.author: normesta
ms.date: 11/04/2019
ms.topic: conceptual
ms.service: storage
ms.subservice: blobs
ms.reviewer: sadodd
ms.openlocfilehash: 07123fd5701e9041ff377ea5309cf1291e737ca6
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73693811"
---
# <a name="change-feed-support-in-azure-blob-storage-preview"></a>Unterstützung für Änderungsfeeds in Azure Blob Storage (Vorschau)

Zweck des Änderungsfeeds ist es, Transaktionsprotokolle für alle Änderungen bereitzustellen, die in den Blobs und Blobmetadaten in Ihrem Speicherkonto auftreten. Der Änderungsfeed bietet **sortierte**, **garantierte**, **permanente**, **unveränderliche** und **schreibgeschützte** Protokolle dieser Änderungen. Clientanwendungen können diese Protokolle jederzeit lesen, entweder im Streaming- oder im Batchmodus. Der Änderungsfeed ermöglicht es Ihnen, effiziente und skalierbare Lösungen zu erstellen, mit denen Änderungsereignisse, die in Ihrem Blob Storage-Konto auftreten, kostengünstig verarbeitet werden.

> [!NOTE]
> Der Änderungsfeed befindet sich in der öffentlichen Vorschauphase und ist in den Regionen **USA, Westen-Mitte** und **USA, Westen 2** verfügbar. Informationen hierzu finden Sie im Abschnitt [Bedingungen](#conditions) dieses Artikels. Informationen zum Registrieren für die Vorschau finden Sie im Abschnitt [Registrieren Ihres Abonnements](#register) dieses Artikels.

Der Änderungsfeed wird in Form von [Blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) in einem speziellen Container Ihres Speicherkontos zu standardmäßigen [Blobpreisen](https://azure.microsoft.com/pricing/details/storage/blobs/) gespeichert. Sie können den Aufbewahrungszeitraum dieser Dateien Ihren Anforderungen entsprechend steuern (Informationen hierzu finden Sie in den [Bedingungen](#conditions) für das aktuelle Release). Änderungsereignisse werden in der [Apache Avro](https://avro.apache.org/docs/1.8.2/spec.html)-Formatspezifikation als Datensätze an den Änderungsfeed angehängt. Dies ist ein kompaktes, schnelles Binärformat, das umfangreiche Datenstrukturen mit Inlineschemas bereitstellt. Dieses Format wird häufig im Hadoop-Ökosystem, von Stream Analytics und von Azure Data Factory verwendet.

Sie können diese Protokolle asynchron, inkrementell oder vollständig verarbeiten. Eine beliebige Anzahl von Clientanwendungen kann den Änderungsfeed unabhängig, gleichzeitig und in der jeweils eigenen Geschwindigkeit lesen. Analyseanwendungen wie [Apache Drill](https://drill.apache.org/docs/querying-avro-files/) oder [Apache Spark](https://spark.apache.org/docs/latest/sql-data-sources-avro.html) können Protokolle direkt als Avro-Dateien nutzen, sodass sie kostengünstig, mit hoher Bandbreite und ohne benutzerdefinierte Anwendung verarbeitet werden können.

Die Unterstützung eines Änderungsfeeds eignet sich gut für Szenarien, in denen Daten basierend auf geänderten Objekten verarbeitet werden. Anwendungen können beispielsweise Folgendes ausführen:

  - Aktualisieren eines sekundären Index, Synchronisieren mit einem Cache oder einer Suchmaschine oder andere Content Management-Szenarien.
  
  - Extrahieren von Erkenntnissen und Metriken aus geschäftlichen Analysen, basierend auf Änderungen an Ihren Objekten – entweder per Streaming oder im Batchmodus.
  
  - Speichern, Überwachen und Analysieren von Änderungen an Ihren Objekten über einen beliebigen Zeitraum hinweg, um Sicherheit, Compliance oder Business Intelligence für die Verwaltung von Unternehmensdaten zu erzielen.

  - Erstellen von Lösungen zum Sichern, Spiegeln oder Replizieren des Objektzustands in Ihrem Konto für Notfallverwaltung oder Compliance.

  - Erstellen von verbundenen Anwendungspipelines, die auf Änderungsereignisse reagieren oder Ausführungen basierend auf erstellten oder geänderten Objekten planen.

> [!NOTE]
> [Blob Storage-Ereignisse](storage-blob-event-overview.md) bieten einmalige Echtzeitereignisse, mit denen Ihre Azure Functions-Lösung oder Ihre Anwendungen auf die Änderungen reagieren können, die in einem Blob auftreten. Der Änderungsfeed bietet ein permanentes, sortiertes Protokollmodell der Änderungen. Änderungen werden innerhalb weniger Minuten nach ihrem Eintreten im Änderungsfeed verfügbar gemacht. Wenn Ihre Anwendung wesentlich schneller auf Ereignisse reagieren muss, sollten Sie stattdessen [Blob Storage-Ereignisse](storage-blob-event-overview.md) in Betracht ziehen. Blob Storage-Ereignisse ermöglichen es Ihrer Azure Functions-Lösung oder Ihren Lösungen, in Echtzeit auf einzelne Ereignisse zu reagieren.

## <a name="enabling-and-disabling-the-change-feed"></a>Aktivieren und Deaktivieren des Änderungsfeeds

Sie müssen den Änderungsfeed aktivieren, damit Änderungen erfasst werden. Deaktivieren Sie den Änderungsfeed, um die Erfassung von Änderungen zu beenden. Sie können Änderungen mithilfe von Azure Resource Manager-Vorlagen im Portal oder in PowerShell aktivieren und deaktivieren.

### <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

So stellen Sie die Vorlage mithilfe des Azure-Portals bereit

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.

2. Geben Sie in **Marketplace durchsuchen** den Begriff **Vorlagenbereitstellung** ein, und drücken Sie dann die **EINGABETASTE**.

3. Wählen Sie **Vorlagenbereitstellung** aus, klicken Sie auf **Erstellen**, und wählen Sie dann **Eigene Vorlage im Editor erstellen** aus.

5. Fügen Sie folgenden JSON-Code im Vorlagen-Editor ein. Ersetzen Sie den Platzhalter `<accountName>` durch den Namen Ihres Speicherkontos.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [{
        "type": "Microsoft.Storage/storageAccounts/blobServices",
        "apiVersion": "2019-04-01",
        "name": "<accountName>/default",
        "properties": {
            "changeFeed": {
            "enabled": true
            }
        } 
     }]
}
```
4. Klicken Sie auf die Schaltfläche **Speichern**, geben Sie die Ressourcengruppe des Kontos an, und klicken Sie dann auf die Schaltfläche**Kaufen**, um den Änderungsfeed zu aktivieren.

### <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

So stellen Sie die Vorlage mithilfe von PowerShell bereit

1. Installieren Sie die neueste Version von PowerShellGet.

   ```powershell
   install-Module PowerShellGet –Repository PSGallery –Force
   ```

2. Schließen Sie die PowerShell-Konsole, und öffnen Sie sie dann erneut.

3. Installieren Sie das Vorschaumodul **Az.Storage**.

   ```powershell
   Install-Module Az.Storage –Repository PSGallery -RequiredVersion 1.8.1-preview –AllowPrerelease –AllowClobber –Force
   ```

4. Melden Sie sich mit dem Befehl `Connect-AzAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Authentifizierungsanweisungen auf dem Bildschirm.

   ```powershell
   Connect-AzAccount
   ```

5. Aktivieren Sie den Änderungsfeed für Ihr Speicherkonto.

   ```powershell
   Update-AzStorageBlobServiceProperty -ResourceGroupName -StorageAccountName -EnableChangeFeed $true
   ```

---

Folgende Aspekte müssen Sie berücksichtigen, wenn Sie den Änderungsfeed aktivieren.

- In jedem Speicherkonto gibt es nur einen Änderungsfeed für den Blobdienst. 

- Änderungen werden nur auf Ebene des Blobdiensts aufgezeichnet.

- Der Änderungsfeed zeichnet *alle* Änderungen für alle verfügbaren Ereignisse auf, die im Konto auftreten. Clientanwendungen können Ereignistypen nach Bedarf herausfiltern. (Informationen hierzu finden Sie in den [Bedingungen](#conditions) für das aktuelle Release.)

- Konten, die einen hierarchischen Namespace aufweisen, werden nicht unterstützt.

## <a name="consuming-the-change-feed"></a>Nutzen des Änderungsfeeds

Der Änderungsfeed erzeugt verschiedene Metadaten- und Protokolldateien. Diese Dateien befinden sich im Container **$blobchangefeed** des Speicherkontos. 

>[!NOTE]
> Im aktuellen Release ist der Container **$blobchangefeed** im Storage-Explorer oder im Azure-Portal nicht sichtbar. 

Ihre Clientanwendungen können den Änderungsfeed nutzen, indem sie die Prozessorbibliothek für den Änderungsfeed im Blob verwenden, die mit dem SDK bereitgestellt wird. 

Informationen dazu finden Sie unter [Verarbeiten von Änderungsfeedprotokollen in Azure Blob Storage](storage-blob-change-feed-how-to.md).

## <a name="understanding-change-feed-organization"></a>Grundlegendes zur Organisation von Änderungsfeeds

<a id="segment-index"></a>

### <a name="segments"></a>Segmente

Ein Änderungsfeed ist ein Protokoll der Änderungen, das in **stündliche** *Segmente* unterteilt ist (Näheres dazu finden Sie in den [Spezifikationen](#specifications)). So kann Ihre Clientanwendung Änderungen nutzen, die innerhalb bestimmter Zeiträume auftreten, ohne das gesamte Protokoll durchsuchen zu müssen.

Ein verfügbares stündliches Segment des Änderungsfeeds wird in einer Manifestdatei beschrieben, die die Pfade zu den Änderungsfeeddateien für dieses Segment angibt. Die Auflistung des virtuellen Verzeichnisses `$blobchangefeed/idx/segments/` zeigt diese Segmente nach Uhrzeit geordnet an. Der Pfad des Segments beschreibt den Start des stündlichen Zeitbereichs, den das Segment repräsentiert. (Näheres dazu finden Sie in den [Spezifikationen](#specifications).) Sie können diese Liste verwenden, um die Protokollsegmente herauszufiltern, die für Sie von Interesse sind.

```text
Name                                                                    Blob Type    Blob Tier      Length  Content Type    
----------------------------------------------------------------------  -----------  -----------  --------  ----------------
$blobchangefeed/idx/segments/1601/01/01/0000/meta.json                  BlockBlob                      584  application/json
$blobchangefeed/idx/segments/2019/02/22/1810/meta.json                  BlockBlob                      584  application/json
$blobchangefeed/idx/segments/2019/02/22/1910/meta.json                  BlockBlob                      584  application/json
$blobchangefeed/idx/segments/2019/02/23/0110/meta.json                  BlockBlob                      584  application/json
```

> [!NOTE]
> Die Datei `$blobchangefeed/idx/segments/1601/01/01/0000/meta.json` wird automatisch erstellt, wenn Sie den Änderungsfeed aktivieren. Sie können diese Datei unbesorgt ignorieren. Sie ist immer leer. 

Die Segmentmanifestdatei (`meta.json`) zeigt den Pfad der Änderungsfeeddateien für dieses Segment in der Eigenschaft `chunkFilePaths` an. Beispiel für eine Segmentmanifestdatei:

```json
{
    "version": 0,
    "begin": "2019-02-22T18:10:00.000Z",
    "intervalSecs": 3600,
    "status": "Finalized",
    "config": {
        "version": 0,
        "configVersionEtag": "0x8d698f0fba563db",
        "numShards": 2,
        "recordsFormat": "avro",
        "formatSchemaVersion": 1,
        "shardDistFnVersion": 1
    },
    "chunkFilePaths": [
        "$blobchangefeed/log/00/2019/02/22/1810/",
        "$blobchangefeed/log/01/2019/02/22/1810/"
    ],
    "storageDiagnostics": {
        "version": 0,
        "lastModifiedTime": "2019-02-22T18:11:01.187Z",
        "data": {
            "aid": "55e507bf-8006-0000-00d9-ca346706b70c"
        }
    }
}
```

> [!NOTE]
> Wenn Sie die Container in Ihrem Speicherkonto auflisten, wird der Container `$blobchangefeed` erst dann angezeigt, wenn Sie das Änderungsfeedfeature in Ihrem Konto aktiviert haben. Nach dem Aktivieren des Änderungsfeeds dauert es einige Minuten, bis der Container angezeigt wird.

<a id="log-files"></a>

### <a name="change-event-records"></a>Ändern von Ereignisdatensätzen

Die Änderungsfeeddateien enthalten eine Reihe von Datensätzen mit Änderungsereignissen. Jeder Änderungsereignis-Datensatz entspricht einer Änderung an einem einzelnen Blob. Die Datensätze werden mithilfe der [Apache Avro](https://avro.apache.org/docs/1.8.2/spec.html)-Formatspezifikation serialisiert und in die Datei geschrieben. Die Datensätze können mithilfe der Avro-Dateiformatspezifikation gelesen werden. Zum Verarbeiten von Dateien in diesem Format stehen verschiedene Bibliotheken zur Verfügung.

Änderungsfeeddateien werden im virtuellen Verzeichnis `$blobchangefeed/log/` als [Anfügeblobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-append-blobs) gespeichert. Die erste Änderungsfeeddatei in jedem Pfad weist einen Zähler `00000` im Dateinamen auf (z. B. `00000.avro`). Dieser Zähler wird bei jeder Protokolldatei, die dem Pfad danach hinzugefügt wird, um 1 erhöht (z. B. `00001.avro`).

Im Folgenden sehen Sie ein Beispiel für einen in JSON konvertierten Änderungsfeed-Datensatz aus einer Änderungsfeeddatei:

```json
{
     "schemaVersion": 1,
     "topic": "/subscriptions/dd40261b-437d-43d0-86cf-ef222b78fd15/resourceGroups/sadodd/providers/Microsoft.Storage/storageAccounts/mytestaccount",
     "subject": "/blobServices/default/containers/mytestcontainer/blobs/mytestblob",
     "eventType": "BlobCreated",
     "eventTime": "2019-02-22T18:12:01.079Z",
     "id": "55e5531f-8006-0000-00da-ca3467000000",
     "data": {
         "api": "PutBlob",
         "clientRequestId": "edf598f4-e501-4750-a3ba-9752bb22df39",
         "requestId": "00000000-0000-0000-0000-000000000000",
         "etag": "0x8D698F13DCB47F6",
         "contentType": "application/octet-stream",
         "contentLength": 128,
         "blobType": "BlockBlob",
         "url": "",
         "sequencer": "000000000000000100000000000000060000000000006d8a",
         "storageDiagnostics": {
             "bid": "11cda41c-13d8-49c9-b7b6-bc55c41b3e75",
             "seq": "(6,5614,28042,28038)",
             "sid": "591651bd-8eb3-c864-1001-fcd187be3efd"
         }
  }
}

```
Eine ausführliche Beschreibung der einzelnen Eigenschaften finden Sie unter [Azure Event Grid-Ereignisschema für Blob Storage](https://docs.microsoft.com/azure/event-grid/event-schema-blob-storage?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#event-properties).

> [!NOTE]
> Die Änderungsfeeddateien für ein Segment werden nicht sofort nach dem Erstellen eines Segments angezeigt. Die Verzögerung bewegt sich im normalen Intervall der Veröffentlichungslatenz des Änderungsfeeds, die wenige Minuten nach der Änderung beträgt.

<a id="specifications"></a>

## <a name="specifications"></a>Spezifikationen

- Änderungsereignis-Datensätze werden an den Änderungsfeed nur angefügt. Nachdem diese Datensätze angefügt wurden, sind sie unveränderlich und ihre Position bleibt stabil. Clientanwendungen können einen eigenen Prüfpunkt an der Leseposition des Änderungsfeeds verwalten.

- Änderungsereignis-Datensätze werden innerhalb weniger Minuten nach der Änderung angefügt. Clientanwendungen können Datensätze per Streamingzugriff direkt nach deren Anfügung oder zu jedem anderen Zeitpunkt per Massenzugriff nutzen.

- Änderungsereignis-Datensätze werden **pro Blob** in der Reihenfolge des Auftretens der Änderung sortiert. Die Reihenfolge von Änderungen über mehrere Blobs hinweg ist in Azure Blob Storage nicht definiert. Alle Änderungen an einem Segment sind vor allen Änderungen an nachfolgenden Segmenten aufgetreten.

- Änderungsereignis-Datensätze werden mithilfe der Formatspezifikation [Apache Avro 1.8.2](https://avro.apache.org/docs/1.8.2/spec.html) in die Protokolldatei serialisiert.

- Änderungsereignis-Datensätze, bei denen der `eventType` den Wert `Control` aufweist, sind interne Systemdatensätze und spiegeln keine Änderung an Objekten in Ihrem Konto wider. Sie sollten diese Datensätze ignorieren.

- Werte im Eigenschaftenbehälter `storageDiagnonstics` dienen nur zur internen Verwendung und sind nicht für die Verwendung durch Ihre Anwendung konzipiert. Ihre Anwendungen sollten keine vertragliche Abhängigkeit von diesen Daten aufweisen.

- Die vom Segment dargestellte Uhrzeit ist ein **ungefährer** Wert mit einem Spielraum von 15 Minuten vorher und nachher. Um also sicherzustellen, dass alle Datensätze einer angegebenen Uhrzeit genutzt werden, schließen Sie auch das vorherige und das nachfolgende Stundensegment ein.

- Jedes Segment kann eine andere Anzahl von `chunkFilePaths` aufweisen. Dies liegt an der internen Partitionierung des Protokollstreams zur Verwaltung des Durchsatzes bei der Veröffentlichung. Die Protokolldateien in jedem `chunkFilePath` enthalten garantiert sich gegenseitig ausschließende Blobs und können parallel genutzt und verarbeitet werden, ohne dass dabei die Reihenfolge der Änderungen pro Blob während der Iteration verändert wird.

- Die Segmente beginnen im Status `Publishing`. Nachdem das Anfügen der Datensätze an das Segment abgeschlossen ist, lautet der Status `Finalized`. Protokolldateien in einem Segment, das nach dem Datum der `LastConsumable`-Eigenschaft in der Datei `$blobchangefeed/meta/Segments.json` datiert ist, sollten von Ihrer Anwendung nicht genutzt werden. Im Folgenden sehen Sie ein Beispiel der `LastConsumable`-Eigenschaft in einer `$blobchangefeed/meta/Segments.json`-Datei:

```json
{
    "version": 0,
    "lastConsumable": "2019-02-23T01:10:00.000Z",
    "storageDiagnostics": {
        "version": 0,
        "lastModifiedTime": "2019-02-23T02:24:00.556Z",
        "data": {
            "aid": "55e551e3-8006-0000-00da-ca346706bfe4",
            "lfz": "2019-02-22T19:10:00.000Z"
        }
    }
}

```

<a id="register"></a>

## <a name="register-your-subscription-preview"></a>Registrieren Ihres Abonnements (Vorschau)

Da der Änderungsfeed nur in der öffentlichen Vorschau vorhanden ist, müssen Sie Ihr Abonnement registrieren, um das Feature verwenden zu können.

### <a name="register-by-using-powershell"></a>Registrieren über PowerShell

Führen Sie die folgenden Befehle in einer PowerShell-Konsole aus:

   ```powershell
   Register-AzProviderFeature -FeatureName Changefeed -ProviderNamespace Microsoft.Storage
   Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
   ```
### <a name="register-by-using-azure-cli"></a>Registrieren mithilfe der Azure CLI

Führen Sie die folgenden Befehle in der Azure Cloud Shell aus:

```cli
az feature register --namespace Microsoft.Storage --name Changefeed
az provider register --namespace 'Microsoft.Storage'
```

<a id="conditions"></a>

## <a name="conditions-and-known-issues-preview"></a>Bedingungen und bekannte Probleme (Vorschau)

Dieser Abschnitt beschreibt bekannte Probleme und Bedingungen in der aktuellen öffentlichen Vorschauversion des Änderungsfeeds.

- Der Änderungsfeed zeichnet nur Erstellungs-, Aktualisierungs-, Lösch- und Kopiervorgänge auf.
- Änderungsereignis-Datensätze für eine einzelne Änderung werden in Ihrem Änderungsfeed möglicherweise mehrmals angezeigt.
- Sie können die Lebensdauer der Änderungsfeed-Protokolldateien noch nicht durch Festlegen einer zeitbasierten Aufbewahrungsrichtlinie verwalten.
- Die `url`-Eigenschaft der Protokolldatei ist immer leer.
- Die `LastConsumable`-Eigenschaft der segments.json-Datei listet das allererste Segment, das vom Änderungsfeed abgeschlossen wird, nicht auf. Dieses Problem tritt nur nach Abschluss des ersten Segments auf. Alle nachfolgenden Segmente nach der ersten Stunde werden ordnungsgemäß in der `LastConsumable`-Eigenschaft aufgezeichnet.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich ein Beispiel dafür an, wie der Änderungsfeed von einer .NET-Clientanwendung gelesen werden kann. Informationen dazu finden Sie unter [Verarbeiten von Änderungsfeedprotokollen in Azure Blob Storage](storage-blob-change-feed-how-to.md).
- Erfahren Sie mehr über die Reaktion auf Ereignisse in Echtzeit. Informationen dazu finden Sie unter [Reaktion auf Blob Storage-Ereignisse](storage-blob-event-overview.md).
