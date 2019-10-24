---
title: azcopy sync | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy sync“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 8b4ab0e44f2432056c9c94061c59c99c89a6407d
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513420"
---
# <a name="azcopy-sync"></a>azcopy sync

Repliziert den Quellspeicherort an den Zielspeicherort.

## <a name="synopsis"></a>Zusammenfassung

Die Zeitpunkte der letzten Änderung werden für den Vergleich verwendet. Die Datei wird übersprungen, wenn der Zeitpunkt der letzten Änderung im Ziel aktueller ist.

Folgende Paare werden unterstützt:

- lokal <–> Azure-Blob (SAS- oder OAuth-Authentifizierung kann verwendet werden)
- Azure-Blob <-> Azure-Blob (Die Quelle muss eine SAS enthalten oder öffentlich zugänglich sein, für das Ziel kann die SAS- oder OAuth-Authentifizierung verwendet werden.)
- Azure-Datei <-> Azure-Datei (Die Quelle muss eine SAS enthalten oder öffentlich zugänglich sein, für das Ziel sollte die SAS-Authentifizierung verwendet werden.)

Der Befehl „sync“ unterscheidet sich auf mehrere Arten vom Befehl „copy“:

1. Das „recursive“-Flag ist standardmäßig „true“, und „sync“ kopiert alle Unterverzeichnisse. „sync“ kopiert nur die Dateien der obersten Ebene in einem Verzeichnis, wenn das „recursive“-Flag „false“ ist.
2. Fügen Sie beim Synchronisieren zwischen virtuellen Verzeichnissen dem Pfad einen nachstehenden Schrägstrich hinzu (siehe Beispiele), wenn ein Blob über denselben Namen wie eines der virtuellen Verzeichnisse verfügt.
3. Wenn das „deleteDestination“-Flag auf „true“ oder „prompt“ festgelegt ist, löscht „sync“ Dateien und Blobs am Ziel, die in der Quelle nicht vorhanden sind.

### <a name="advanced"></a>Erweitert

Wenn Sie keine Dateierweiterung angeben, erkennt AzCopy den Inhaltstyp der Dateien beim Hochladen vom lokalen Datenträger automatisch anhand der Dateierweiterung oder des Inhalts (falls keine Erweiterung angegeben ist).

Die integrierte Nachschlagetabelle ist klein, aber unter Unix wird sie, falls verfügbar, um die mime.types-Datei(en) des lokalen Systems erweitert. Hierfür werden diese Namen verwendet:

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

Unter Windows werden MIME-Typen aus der Registrierung extrahiert.

```azcopy
azcopy sync [flags]
```

## <a name="examples"></a>Beispiele

Synchronisieren einer einzelnen Datei:

```azcopy
azcopy sync "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Wie oben, aber berechnen Sie hierbei außerdem den MD5-Hash des Dateiinhalts, und speichern Sie ihn als „Content-MD5“-Eigenschaft des Blobs:

```azcopy
azcopy sync "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5
```

Synchronisieren eines gesamten Verzeichnisses einschließlich der zugehörigen Unterverzeichnisse (beachten Sie, dass „recursive“ standardmäßig aktiviert ist):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]"
```

oder

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --put-md5
```

Synchronisieren nur der obersten Dateien in einem Verzeichnis, aber nicht der zugehörigen Unterverzeichnisse:

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Synchronisieren Sie eine Teilmenge von Dateien in einem Verzeichnis (z.B. nur JPG- und PDF-Dateien oder wenn der Dateiname „exactName“ lautet):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --include="*.jpg;*.pdf;exactName"
```

Synchronisieren Sie ein gesamtes Verzeichnis, doch schließen Sie bestimmte Dateien aus dem Umfang aus (z.B. jede Datei, die mit „foo“ beginnt oder mit „bar“ endet):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --exclude="foo*;*bar"
```

Synchronisieren eines einzelnen Blobs:

```azcopy
azcopy sync "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"

Sync a virtual directory:

```azcopy
azcopy sync "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]?[SAS]" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=true
```

Synchronisieren eines virtuellen Verzeichnisses mit demselben Namen wie ein Blob (Fügen Sie dem Pfad einen nachstehenden Schrägstrich hinzu, um die Namen zu unterscheiden.):

```azcopy
azcopy sync "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]/?[SAS]" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]/" --recursive=true
```

Synchronisieren eines Azure-Dateiverzeichnisses (dieselbe Syntax wie bei einem Blob):

```azcopy
azcopy sync "https://[account].file.core.windows.net/[share]/[path/to/dir]?[SAS]" "https://[account].file.core.windows.net/[share]/[path/to/dir]" --recursive=true
```

> [!NOTE]
> Wenn „include“/„exclude“-Flags zusammen verwendet werden, würden nur Dateien berücksichtigt, die mit den „include“-Mustern übereinstimmen. Dateien, die mit den „exclude“-Mustern übereinstimmen, würden jedoch immer ignoriert.

## <a name="options"></a>Optionen

**--block-size-mb** float         Verwendet diese Blockgröße (in MiB) beim Hochladen in Azure Storage oder beim Herunterladen aus Azure Storage. Der Standardwert wird anhand der Dateigröße automatisch berechnet. Dezimalzahlen sind zulässig (Beispiel: 0,25).

**--check-md5** string            Gibt an, wie streng MD5-Hashes beim Herunterladen überprüft werden sollten. Diese Option ist nur beim Herunterladen verfügbar. Folgende Werte sind verfügbar: „NoCheck“, „LogOnly“, „FailIfDifferent“, „FailIfDifferentOrMissing“. (Standardwert: „FailIfDifferent“) (Die Standardeinstellung ist „FailIfDifferent“.)

**--delete-destination** string   Definiert, ob zusätzliche Dateien am Ziel gelöscht werden sollen, die in der Quelle nicht vorhanden sind. Könnte auf „true“, „false“ oder „prompt“ festgelegt werden. Wenn sie auf „prompt“ festgelegt wurde, wird dem Benutzer eine Frage gestellt, bevor Dateien und Blobs zum Löschen geplant werden. (Standardwert: „false“) (Der Standardwert lautet „false“.)

**--exclude-attributes** string   (Nur Windows) Schließt Dateien aus, deren Attribute mit der Attributliste übereinstimmen. Beispiel:  A;S;R

**--exclude-pattern** string      Schließt Dateien aus, deren Name der Musterliste entspricht. Beispiel: *.jpg;* .pdf;exactName

**-h, --help**                        Hilfe zu „sync“

**--include-attributes** string   (Nur Windows) Schließt nur Dateien ein, deren Attribute mit der Attributliste übereinstimmen. Beispiel:  A;S;R

**--include-pattern** string      Schließt nur Dateien ein, deren Name der Musterliste entspricht. Beispiel: *.jpg;* .pdf;exactName

**--log-level** string            Definiert, wie ausführlich die Protokolldatei sein soll. Verfügbare Stufen: INFO (alle Anforderungen und Antworten), WARNING (langsame Antworten), ERROR (nur fehlerhafte Anforderungen) und NONE (keine Ausgabeprotokolle). (Der Standardwert lautet „INFO“.) (Standardwert: „Info“)

**--put-md5**                     Erstellt einen MD5-Hash jeder Datei und speichert den Hash als „Content-MD5“-Eigenschaft des Zielblobs bzw. der Zieldatei. (Standardmäßig wird der Hash NICHT erstellt.) Nur beim Hochladen verfügbar.

**--recursive**                   Standardmäßig „true“; überprüft Unterverzeichnisse bei der Synchronisierung zwischen Verzeichnissen rekursiv. (Die Standardeinstellung ist „true“). (Standardwert: „true“)

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
