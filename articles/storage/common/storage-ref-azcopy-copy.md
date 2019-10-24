---
title: azcopy copy| Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy copy“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: e7f08c175972826a8b226d7e80f563ac71ba23db
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72514757"
---
# <a name="azcopy-copy"></a>azcopy copy

Kopiert Quelldaten an einen Zielspeicherort.

## <a name="synopsis"></a>Zusammenfassung

Kopiert Quelldaten an einen Zielspeicherort. Die folgenden Richtungen werden unterstützt:

  - Lokal <-> Azure-Blob (SAS- oder OAuth-Authentifizierung)
  - Lokal <-> Azure Files (SAS-Authentifizierung für Freigabe/Verzeichnis)
  - Lokal <-> ADLS Gen2 (SAS-, OAuth- oder SharedKey-Authentifizierung)
  - Azure-Blob (SAS oder öffentlich) -> Azure-Blob (SAS- oder OAuth-Authentifizierung)
  - Azure-Blob (SAS oder öffentlich) -> Azure Files (SAS)
  - Azure Files (SAS) -> Azure Files (SAS)
  - Azure Files (SAS) -> Azure-Blob (SAS- oder OAuth-Authentifizierung)
  - AWS S3 (Zugriffsschlüssel) -> Azure-Blockblob (SAS- oder OAuth-Authentifizierung)

Weitere Informationen finden Sie in den Beispielen.

## <a name="advanced"></a>Erweitert

AzCopy erkennt den Inhaltstyp der Dateien beim Hochladen vom lokalen Datenträger automatisch anhand der Dateierweiterung oder des Inhalts (falls keine Erweiterung angegeben ist).

Die integrierte Nachschlagetabelle ist klein, aber unter Unix wird sie, falls verfügbar, um die mime.types-Datei(en) des lokalen Systems erweitert. Hierfür werden diese Namen verwendet:

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

Unter Windows werden MIME-Typen aus der Registrierung extrahiert. Diese Funktion kann mit einem Flag deaktiviert werden. Informationen hierzu finden Sie im Abschnitt zu Flags.

Wenn Sie eine Umgebungsvariable über die Befehlszeile festlegen, kann diese Variable in Ihrem Befehlszeilenverlauf gelesen werden. Es empfiehlt sich gegebenenfalls, Variablen mit Anmeldeinformationen aus Ihrem Befehlszeilenverlauf zu löschen. Wenn Sie verhindern möchten, dass Variablen in Ihrem Verlauf angezeigt werden, können Sie den Benutzer mit einem Skript zur Eingabe seiner Anmeldeinformationen auffordern und die Umgebungsvariable festlegen.

```
azcopy copy [source] [destination] [flags]
```

## <a name="examples"></a>Beispiele

Hochladen einer einzelnen Datei unter Verwendung der OAuth-Authentifizierung. Wenn Sie sich noch nicht bei AzCopy angemeldet haben, sollten Sie den Befehl „azcopy login“ ausführen, bevor Sie folgenden Befehl ausführen.

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"

Wie oben, aber berechnen Sie hierbei außerdem den MD5-Hash des Dateiinhalts, und speichern Sie ihn als Content-MD5-Eigenschaft des Blobs:

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5

Hochladen einer einzelnen Datei unter Verwendung eines SAS-Tokens:

- azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"

Hochladen einer einzelnen Datei unter Verwendung eines SAS-Tokens und einer Weiterleitung (nur Blockblobs):
  
- cat "/path/to/file.txt" | azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"

Hochladen eines gesamten Verzeichnisses unter Verwendung eines SAS-Tokens:
  
- azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true

oder

- azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --put-md5

Hochladen einer Gruppe von Dateien unter Verwendung eines SAS-Tokens und Platzhalterzeichen (*):

- azcopy cp "/path/*foo/* bar/*.pdf" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]"

Hochladen von Dateien und Verzeichnissen unter Verwendung eines SAS-Tokens und Platzhalterzeichen (*):

- azcopy cp "/path/*foo/* bar*" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true

Herunterladen einer einzelnen Datei unter Verwendung der OAuth-Authentifizierung. Wenn Sie sich noch nicht bei AzCopy angemeldet haben, sollten Sie den Befehl „azcopy login“ ausführen, bevor Sie folgenden Befehl ausführen.

- azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]" "/path/to/file.txt"

Herunterladen einer einzelnen Datei unter Verwendung eines SAS-Tokens:

- azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "/path/to/file.txt"

Herunterladen einer einzelnen Datei unter Verwendung eines SAS-Tokens und anschließendes Weiterleiten der Ausgabe in eine Datei (nur Blockblobs):
  
- azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" > "/path/to/file.txt"

Herunterladen eines gesamten Verzeichnisses unter Verwendung eines SAS-Tokens:
  
- azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "/path/to/dir" --recursive=true

Hinweis zur Verwendung eines Platzhalterzeichens (*) in URLs:

Es gibt nur zwei Möglichkeiten, ein Platzhalterzeichen in einer URL zu verwenden. 

- Sie können einen Platzhalter direkt nach dem letzten Schrägstrich (/) einer URL verwenden. Dadurch werden alle Dateien in einem Verzeichnis direkt an das Ziel kopiert, ohne sie in ein Unterverzeichnis einzufügen.

- Sie können auch einen Platzhalter im Namen eines Containers verwenden, sofern die URL nur auf einen Container und nicht auf ein Blob verweist. Mit diesem Ansatz können Sie Dateien aus einer Teilmenge von Containern abrufen.

Herunterladen der Inhalte eines Verzeichnisses, ohne das Verzeichnis selbst zu kopieren.

- azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/folder]/*?[SAS]" "/path/to/dir"

Herunterladen eines vollständigen Speicherkontos.

- azcopy cp "https://[srcaccount].blob.core.windows.net/" "/path/to/dir" --recursive

Herunterladen einer Teilmenge der Container innerhalb eines Speicherkontos unter Verwendung eines Platzhaltersymbols (*) im Containernamen.

- azcopy cp "https://[srcaccount].blob.core.windows.net/[container*name]" "/path/to/dir" --recursive

Kopieren eines einzelnen Blobs in ein anderes Blob unter Verwendung eines SAS-Tokens.

- azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"

Kopieren eines einzelnen Blobs in ein anderes Blob unter Verwendung eines SAS-Tokens und eines OAuth-Tokens. Sie müssen am Ende der URL für das Quellkonto ein SAS-Token verwenden. Für das Zielkonto ist jedoch kein Token erforderlich, wenn Sie sich mit dem Befehl „azcopy login“ bei AzCopy anmelden. 

- azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]"

Kopieren eines virtuellen Blobverzeichnisses in ein anderes unter Verwendung eines SAS-Tokens:

- azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true

Kopieren aller Blobcontainer, Verzeichnisse und Blobs von einem Speicherkonto in ein anderes unter Verwendung eines SAS-Tokens:

- azcopy cp "https://[srcaccount].blob.core.windows.net?[SAS]" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true

Kopieren eines einzelnen Objekts aus Amazon Web Services (AWS) S3 in Blob Storage unter Verwendung eines Zugriffsschlüssels und eines SAS-Tokens. Legen Sie zunächst die Umgebungsvariablen AWS_ACCESS_KEY_ID und AWS_SECRET_ACCESS_KEY für die AWS S3-Quelle fest.
  
- azcopy cp "https://s3.amazonaws.com/ [bucket]/[object]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"

Kopieren eines gesamten Verzeichnisses aus AWS S3 in Blob Storage unter Verwendung eines Zugriffsschlüssels und eines SAS-Tokens. Legen Sie zunächst die Umgebungsvariablen AWS_ACCESS_KEY_ID und AWS_SECRET_ACCESS_KEY für die AWS S3-Quelle fest.

- azcopy cp "https://s3.amazonaws.com/ [bucket]/[folder]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true

Unter https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html finden Sie ausführlichere Informationen zum Platzhalter „[folder]“.

Kopieren aller Buckets aus Amazon Web Services (AWS) in Blob Storage unter Verwendung eines Zugriffsschlüssels und eines SAS-Tokens. Legen Sie zunächst die Umgebungsvariablen AWS_ACCESS_KEY_ID und AWS_SECRET_ACCESS_KEY für die AWS S3-Quelle fest.

- azcopy cp "https://s3.amazonaws.com/ " "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true

Kopieren aller Buckets aus einer Amazon Web Services (AWS)-Region in Blob Storage unter Verwendung eines Zugriffsschlüssels und eines SAS-Tokens. Legen Sie zunächst die Umgebungsvariablen AWS_ACCESS_KEY_ID und AWS_SECRET_ACCESS_KEY für die AWS S3-Quelle fest.

- azcopy cp "https://s3- [region].amazonaws.com/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true

Kopieren einer Teilmenge der Buckets unter Verwendung eines Platzhaltersymbols (*) im Bucketnamen. Wie in den vorherigen Beispielen benötigen Sie einen Zugriffsschlüssel und ein SAS-Token. Achten Sie darauf, die Umgebungsvariablen AWS_ACCESS_KEY_ID und AWS_SECRET_ACCESS_KEY für die AWS S3-Quelle festzulegen.

- azcopy cp "https://s3.amazonaws.com/ [bucket*name]/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true

## <a name="options"></a>Optionen

**--blob-type** string                     Definiert den Typ des Blobs am Ziel. Wird zum Hochladen von Blobs und beim Kopieren zwischen Konten verwendet (Standardeinstellung: „Detect“). Gültige Werte sind „Detect“, „BlockBlob“, „PageBlob“ und „AppendBlob“. Beim Kopieren zwischen Konten bewirkt der Wert „Detect“, dass AzCopy den Typ des Zielblobs anhand des Typs des Quellblobs bestimmt. Beim Hochladen einer Datei bestimmt „Detect“ anhand der Dateierweiterung, ob es sich um eine VHD- oder VHDX-Datei handelt. Eine VHD- oder VHDX-Datei wird von AzCopy als Seitenblob behandelt. (Standardwert: „Detect“)

**--block-blob-tier** string               Lädt ein Blockblob unter Verwendung dieses Blobtarifs in Azure Storage hoch. (Standardwert: „None“)

**--block-size-mb** float                  Verwendet diese Blockgröße (in MiB) beim Hochladen in Azure Storage und beim Herunterladen aus Azure Storage. Der Standardwert wird anhand der Dateigröße automatisch berechnet. Dezimalzahlen sind zulässig (z. B. 0,25).

**--cache-control** string                 Legt den Cache-Control-Header fest. Wird beim Herunterladen zurückgegeben.

**--check-length**                         Überprüft nach der Übertragung die Länge einer Datei am Ziel. Wenn die Quelle und das Ziel nicht übereinstimmen, wird die Übertragung als fehlerhaft gekennzeichnet. (Standardwert: „true“)

**--check-md5** string                     Gibt an, wie streng MD5-Hashes beim Herunterladen überprüft werden sollten. Nur beim Herunterladen verfügbar. Verfügbare Optionen: „NoCheck“, „LogOnly“, „FailIfDifferent“, „FailIfDifferentOrMissing“. (Standardwert: „FailIfDifferent“) (Der Standardwert lautet „FailIfDifferent“.)

**--content-disposition** string           Legt den Content-Disposition-Header fest. Wird beim Herunterladen zurückgegeben.

**--content-encoding** string              Legt den Content-Encoding-Header fest. Wird beim Herunterladen zurückgegeben.

**--content-language** string              Legt den Content-Language-Header fest. Wird beim Herunterladen zurückgegeben.

**--content-type** string                  Gibt den Inhaltstyp der Datei an. Impliziert „no-guess-mime-type“. Wird beim Herunterladen zurückgegeben.

**--decompress**                           Dekomprimiert Dateien beim Herunterladen automatisch, wenn ihr Content-Encoding-Wert angibt, dass sie komprimiert sind. Die unterstützten Content-Encoding-Werte sind „gzip“ und „deflate“. Die Dateierweiterungen „.gz“/„.gzip“ oder „.zz“ sind nicht erforderlich, werden jedoch entfernt, falls vorhanden.

**--exclude-attributes** string            (Nur Windows) Schließt Dateien aus, deren Attribute mit der Attributliste übereinstimmen. Beispiel:  A;S;R

**--exclude-blob-type** string             Gibt optional den Typ von Blob (BlockBlob/PageBlob/AppendBlob) an, der ausgeschlossen werden soll, wenn Blobs aus dem Container oder Konto kopiert werden. Die Verwendung dieses Flags gilt nicht für das Kopieren von Daten von einem nicht zu Azure gehörenden Dienst in einen Dienst. Bei Angabe von mehreren Blobs sollte „;“ als Trennzeichen verwendet werden.

**--exclude-path** string                  Schließt diese Pfade beim Kopieren aus. Diese Option unterstützt keine Platzhalterzeichen (*). Überprüft das Präfix des relativen Pfads (Beispiel: myFolder;myFolder/subDirName/file.pdf). Wenn die Option in Verbindung mit einem Kontodurchlauf verwendet wird, enthalten Pfade keinen Containernamen.

**--exclude-pattern** string               Schließt diese Dateien beim Kopieren aus. Diese Option unterstützt Platzhalterzeichen (*).

**--follow-symlinks**                      Verwendet beim Hochladen aus dem lokalen Dateisystem symbolische Links.

**--from-to** string                       Gibt optional die Kombination aus Quelle und Ziel an. Beispiel: LocalBlob, BlobLocal, LocalBlobFS.

**-h, --help**                                 Hilfe zu „copy“

**--include-attributes** string            (Nur Windows) Schließt Dateien ein, deren Attribute mit der Attributliste übereinstimmen. Beispiel:  A;S;R

**--include-path** string                  Schließt nur diese Pfade beim Kopieren ein. Diese Option unterstützt keine Platzhalterzeichen (*). Überprüft das Präfix des relativen Pfads (Beispiel: myFolder;myFolder/subDirName/file.pdf).

**--include-pattern** string               Schließt nur diese Dateien beim Kopieren ein. Diese Option unterstützt Platzhalterzeichen (*). Trennen Sie Dateien durch ein Semikolon (;).

**--log-level** string                     Definiert, wie ausführlich die Protokolldatei sein soll. Verfügbare Stufen: INFO (alle Anforderungen/Antworten), WARNING (langsame Antworten), ERROR (nur fehlgeschlagene Anforderungen) und NONE (keine Ausgabeprotokolle). (Der Standardwert lautet „INFO“). (Standardwert: „Info“)

**--metadata** string                      Führt Uploads in Azure Storage mit diesen Schlüssel-Wert-Paaren als Metadaten aus.

**--no-guess-mime-type**                   Verhindert, dass AzCopy anhand der Erweiterung oder des Inhalts der Datei den Inhaltstyp erkennt.

**--overwrite** string                     Überschreibt die in Konflikt stehenden Dateien und Blobs am Ziel, wenn dieses Flag auf „true“ festgelegt ist. (Standardwert: „true“) Mögliche Werte sind „true“, „false“ und „prompt“. (Der Standardwert lautet „true“.)

**--page-blob-tier** string                Lädt ein Seitenblob unter Verwendung dieses Blobtarifs in Azure Storage hoch. (Der Standardwert lautet „None“.) (Standardwert: „None“)

**--preserve-last-modified-time**          Nur verfügbar, wenn das Ziel ein Dateisystem ist.

**--put-md5**                             Erstellt einen MD5-Hash jeder Datei und speichert den Hash als „Content-MD5“-Eigenschaft des Zielblobs bzw. der Zieldatei. (Standardmäßig wird der Hash NICHT erstellt.) Nur beim Hochladen verfügbar.

**--recursive**                            Durchsucht beim Hochladen aus dem lokalen Dateisystem Unterverzeichnisse rekursiv.

**--s2s-detect-source-changed**           Überprüft, ob sich die Quelle nach dem Enumerieren geändert hat.

**--s2s-handle-invalid-metadata** string   Gibt an, wie ungültige Metadatenschlüssel behandelt werden. Verfügbare Optionen: ExcludeIfInvalid, FailIfInvalid, RenameIfInvalid. (Der Standardwert lautet „ExcludeIfInvalid“.) (Standardwert: „ExcludeIfInvalid“)

**--s2s-preserve-access-tier**             Behält die Zugriffsebene beim Kopieren zwischen Diensten bei. Informationen zur Sicherstellung, dass das Zielspeicherkonto das Festlegen der Zugriffsebene unterstützt, finden Sie unter [Azure Blob Storage: Zugriffsebenen „Heiß“, „Kalt“ und „Archiv“](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). Verwenden Sie in den Fällen, in denen das Festlegen der Zugriffsebene nicht unterstützt wird, „s2sPreserveAccessTier=false“, um das Kopieren der Zugriffsebene zu umgehen. (Die Standardeinstellung ist „true“).  (Standardwert: „true“)

**--s2s-preserve-properties**              Behält die vollständigen Eigenschaften beim Kopieren zwischen Diensten bei. Für AWS S3 und Azure Files mit mehr als einer Dateiquelle gibt der Auflistungsvorgang nicht die vollständigen Eigenschaften von Objekten und Dateien zurück. Um die vollständigen Eigenschaften beizubehalten, muss AzCopy eine zusätzliche Anforderung pro Objekt oder Datei senden. (Standardwert: „true“)

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

**--cap-mbps uint32**      Begrenzt die Übertragungsrate (in Megabits pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.

**--output-type** string   Das Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“. (Standardwert: „text“)

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
