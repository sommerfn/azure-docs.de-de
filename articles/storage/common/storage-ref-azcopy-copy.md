---
title: azcopy copy| Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy copy“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: c15d188e333bea5e74fa65d2bbdf38ae7fadc246
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196744"
---
# <a name="azcopy-copy"></a>azcopy copy

Kopiert Quelldaten an einen Zielspeicherort.

## <a name="synopsis"></a>Zusammenfassung

Die folgenden Richtungen werden unterstützt:

- Lokal <-> Azure-Blob (SAS- oder OAuth-Authentifizierung)
- Lokal <-> Azure-Datei (SAS-Authentifizierung für Freigabe/Verzeichnis)
- Lokal <-> ADLS Gen2 (SAS-, OAuth- oder SharedKey-Authentifizierung)
- Azure-Blob (SAS oder öffentlich) <-> Azure-Blob (SAS- oder OAuth-Authentifizierung)
- Azure-Datei (SAS) -> Azure-Blockblob (SAS- oder OAuth-Authentifizierung)
- AWS S3 (Zugriffsschlüssel) -> Azure-Blockblob (SAS- oder OAuth-Authentifizierung)

Weitere Informationen finden Sie in den Beispielen.

### <a name="advanced"></a>Erweitert

AzCopy erkennt den Inhaltstyp der Dateien beim Hochladen vom lokalen Datenträger automatisch anhand der Dateierweiterung oder des Inhalts (falls keine Erweiterung angegeben ist).

Die integrierte Nachschlagetabelle ist klein, aber unter Unix wird sie, falls verfügbar, um die mime.types-Datei(en) des lokalen Systems erweitert. Hierfür werden diese Namen verwendet:

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

Unter Windows werden MIME-Typen aus der Registrierung extrahiert. Diese Funktion kann mit einem Flag deaktiviert werden. Informationen hierzu finden Sie im Abschnitt zu Flags.

> [!IMPORTANT]
> Wenn Sie eine Umgebungsvariable über die Befehlszeile festlegen, kann diese Variable in Ihrem Befehlszeilenverlauf gelesen werden. Erwägen Sie das Löschen von Variablen mit Anmeldeinformationen aus Ihrem Befehlszeilenverlauf. Wenn Sie verhindern möchten, dass Variablen in Ihrem Verlauf angezeigt werden, können Sie den Benutzer mit einem Skript zur Eingabe seiner Anmeldeinformationen auffordern und die Umgebungsvariable festlegen.

```azcopy
azcopy copy [source] [destination] [flags]
```

## <a name="examples"></a>Beispiele

Laden Sie eine einzelne Datei hoch, indem Sie die OAuth-Authentifizierung verwenden.

Wenn Sie sich noch nicht an AzCopy angemeldet haben, sollten Sie vor dem Ausführen des folgenden Befehls den Befehl `azcopy login` verwenden.

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Wie oben, aber berechnen Sie hierbei außerdem den MD5-Hash des Dateiinhalts, und speichern Sie ihn als Content-MD5-Eigenschaft des Blobs:

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5
```

Hochladen einer einzelnen Datei mit einer SAS:

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Hochladen einer einzelnen Datei mit einer SAS per Piping (nur Blockblobs):

```azcopy
cat "/path/to/file.txt" | azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Hochladen eines gesamten Verzeichnisses mit einer SAS:

```azcopy
azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

oder

```azcopy
azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --put-md5
```

Hochladen eines Dateisatzes per SAS mit Platzhaltern:

```azcopy
azcopy cp "/path/*foo/*bar/*.pdf" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

Hochladen von Dateien und Verzeichnissen per SAS mit Platzhaltern:

```azcopy
azcopy cp "/path/*foo/*bar*" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Laden Sie eine einzelne Datei herunter, indem Sie die OAuth-Authentifizierung verwenden.

Wenn Sie sich noch nicht an AzCopy angemeldet haben, sollten Sie vor dem Ausführen des folgenden Befehls den Befehl `azcopy login` verwenden.

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]" "/path/to/file.txt"
```

Herunterladen einer einzelnen Datei mit einer SAS:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "/path/to/file.txt"
```

Herunterladen einer einzelnen Datei mit einer SAS per Piping (nur Blockblobs):

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" > "/path/to/file.txt"
```

Herunterladen eines gesamten Verzeichnisses mit einer SAS:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "/path/to/dir" --recursive=true
```

Herunterladen eines Dateisatzes per SAS mit Platzhaltern:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/foo*?[SAS]" "/path/to/dir"
```

Herunterladen von Dateien und Verzeichnissen per SAS mit Platzhaltern:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/foo*?[SAS]" "/path/to/dir" --recursive=true
```

Kopieren eines einzelnen Blobs mit SAS in ein anderes Blob mit SAS:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Kopieren eines einzelnen Blobs mit SAS in ein anderes Blob mit OAuth-Token:

Wenn Sie sich noch nicht an AzCopy angemeldet haben, sollten Sie vor dem Ausführen des folgenden Befehls den Befehl `azcopy login` verwenden. Das OAuth-Token wird verwendet, um auf das Zielspeicherkonto zuzugreifen.

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]"
```

Kopieren eines gesamten Verzeichnisses aus dem virtuellen Blobverzeichnis mit SAS in ein anderes virtuelles Blobverzeichnis mit SAS:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Kopieren der gesamten Kontodaten aus einem Blobkonto mit SAS in ein anderes Blobkonto mit SAS:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net?[SAS]" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true
```

Kopieren eines einzelnen Objekts aus S3 mit Zugriffsschlüssel in ein Blob mit SAS:

Legen Sie die Umgebungsvariablen AWS_ACCESS_KEY_ID und AWS_SECRET_ACCESS_KEY für die S3-Quelle fest.

```azcopy
azcopy cp "https://s3.amazonaws.com/[bucket]/[object]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Kopieren eines gesamten Verzeichnisses aus S3 mit Zugriffsschlüssel in ein virtuelles Blobverzeichnis mit SAS:

```azcopy
azcopy cp "https://s3.amazonaws.com/[bucket]/[folder]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Informationen dazu, was „[folder]“ für S3 bedeutet, finden Sie unter https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html. Legen Sie die Umgebungsvariablen AWS_ACCESS_KEY_ID und AWS_SECRET_ACCESS_KEY für die S3-Quelle fest.

Kopieren aller Buckets im S3-Dienst mit Zugriffsschlüssel in ein Blobkonto mit SAS:

Legen Sie die Umgebungsvariablen AWS_ACCESS_KEY_ID und AWS_SECRET_ACCESS_KEY für die S3-Quelle fest.

```azcopy
azcopy cp "https://s3.amazonaws.com/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true
```

Kopieren aller Buckets in einer S3-Region mit Zugriffsschlüssel in ein Blobkonto mit SAS:

```azcopy
azcopy cp "https://s3-[region].amazonaws.com/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true
```

Legen Sie die Umgebungsvariablen AWS_ACCESS_KEY_ID und AWS_SECRET_ACCESS_KEY für die S3-Quelle fest.

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|--blob-type string|Definiert den Typ des Blobs auf dem Ziel. Wird zum Hochladen von Blobs und beim Kopieren zwischen Konten verwendet (Standardeinstellung: „None“).|
|--block-blob-tier string|Laden Sie ein Blockblob in Azure Storage hoch, indem Sie diesen Blobtarif verwenden. (Die Standardeinstellung ist „None“.)|
|--block-size-mb float |Verwenden Sie diese Blockgröße (in MiB) beim Hochladen in Azure Storage und beim Herunterladen aus Azure Storage. Der Standardwert wird anhand der Dateigröße automatisch berechnet. Dezimalzahlen sind zulässig (z. B. 0,25).|
|--cache-control string|Dient zum Festlegen des Cache-Control-Headers. Wird beim Herunterladen zurückgegeben.|
|--check-md5 string|Gibt an, wie streng MD5-Hashes beim Herunterladen überprüft werden. Nur beim Herunterladen verfügbar. Verfügbare Optionen: NoCheck, LogOnly, FailIfDifferent, FailIfDifferentOrMissing. (Die Standardeinstellung ist „FailIfDifferent“.)|
|--content-disposition string|Dient zum Festlegen des Content-Disposition-Headers. Wird beim Herunterladen zurückgegeben.|
|--content-encoding string|Dient zum Festlegen des Content-Encoding-Headers. Wird beim Herunterladen zurückgegeben.|
|--content-language string|Dient zum Festlegen des Content-Language-Headers. Wird beim Herunterladen zurückgegeben.|
|--content-type string |Gibt den Inhaltstyp der Datei an. Impliziert „no-guess-mime-type“. Wird beim Herunterladen zurückgegeben.|
|--exclude string|Dient zum Ausschließen dieser Dateien beim Kopieren. Die Verwendung von „*“ wird unterstützt.|
|--exclude-blob-type string|Gibt optional den Typ von Blob (BlockBlob/PageBlob/AppendBlob) an, der ausgeschlossen werden soll, wenn Blobs aus dem Container oder Konto kopiert werden. Die Verwendung dieses Flags gilt nicht für das Kopieren von Daten von einem nicht zu Azure gehörenden Dienst in einen Dienst. Bei Angabe von mehreren Blobs sollte „;“ als Trennzeichen verwendet werden.|
|--follow-symlinks|Verwenden Sie beim Hochladen aus dem lokalen Dateisystem symbolische Links.|
|--from-to string|Gibt optional die Quelle/Ziel-Kombination an. Beispiel: LocalBlob, BlobLocal, LocalBlobFS.|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „copy“ an. |
|--log-level string|Dient zum Definieren der Protokollausführlichkeit für die Protokolldatei. Verfügbare Ebenen: INFO (alle Anforderungen/Antworten), WARNING (langsame Antworten), ERROR (nur fehlgeschlagene Anforderungen) und NONE (keine Ausgabeprotokolle). (Die Standardeinstellung ist „INFO“.)|
|--metadata string|Dient zum Hochladen in Azure Storage mit diesen Schlüssel-Wert-Paaren als Metadaten.|
|--no-guess-mime-type|Verhindert, dass AzCopy anhand der Erweiterung oder des Inhalts der Datei den Inhaltstyp erkennt.|
|--overwrite|Dient zum Überschreiben der in Konflikt stehenden Dateien/Blobs auf dem Ziel, wenn dieses Flag auf „true“ festgelegt ist. (Die Standardeinstellung ist „true“.)|
|--page-blob-tier string |Dient zum Hochladen eines Seitenblobs in Azure Storage mit diesem Blobtarif. (Die Standardeinstellung ist „None“.)|
|--preserve-last-modified-time|Nur verfügbar, wenn das Ziel ein Dateisystem ist.|
|--put-md5|Dient zum Erstellen eines MD5-Hashs jeder Datei und zum Speichern des Hashs als Content-MD5-Eigenschaft des Zielblobs bzw. der Zieldatei. (Standardmäßig wird der Hash NICHT erstellt.) Nur beim Hochladen verfügbar.|
|--recursive|Dient beim Hochladen aus dem lokalen Dateisystem zum rekursiven Durchsuchen von Unterverzeichnissen.|
|--s2s-detect-source-changed|Dient zum Überprüfen, ob sich die Quelle nach dem Enumerieren geändert hat. Da die Quelle eine Remoteressource ist, fällt bei einer S2S-Kopie für die Überprüfung, ob sich die Quelle geändert hat, zusätzlicher Anforderungsaufwand an.|
|--s2s-handle-invalid-metadata string |Gibt an, wie ungültige Metadatenschlüssel verarbeitet werden. Verfügbare Optionen: ExcludeIfInvalid, FailIfInvalid, RenameIfInvalid. (Die Standardeinstellung ist „ExcludeIfInvalid“.)|
|--s2s-preserve-access-tier|Dient zum Beibehalten der Zugriffsebene beim Kopieren von Dienst zu Dienst. Informationen zur Sicherstellung, dass das Zielspeicherkonto das Festlegen der Zugriffsebene unterstützt, finden Sie unter [Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](../blobs/storage-blob-storage-tiers.md). Verwenden Sie in den Fällen, in denen das Festlegen der Zugriffsebene nicht unterstützt wird, „s2sPreserveAccessTier=false“, um das Kopieren der Zugriffsebene zu umgehen.  (Die Standardeinstellung ist „true“.)|
|--s2s-preserve-properties|Dient zum Beibehalten der vollständigen Eigenschaften beim Kopieren von Dienst zu Dienst. Da beim Auflistungsvorgang nicht die vollständigen Eigenschaften von Objekten und Dateien zurückgegeben werden, muss AzCopy für S3 und Azure Files mit mehr als einer Dateiquelle eine zusätzliche Anforderung pro Objekt und Datei senden, damit die vollständigen Eigenschaften beibehalten werden. (Die Standardeinstellung ist „true“.)|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|--cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|--output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
