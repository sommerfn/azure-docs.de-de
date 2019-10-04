---
title: azcopy sync | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy sync“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: fb6c3b711a89ae7e4ef403a75927c4c6172523d0
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196752"
---
# <a name="azcopy-sync"></a>azcopy sync

Repliziert den Quellspeicherort an den Zielspeicherort.

## <a name="synopsis"></a>Zusammenfassung

Die Zeitpunkte der letzten Änderung werden für den Vergleich verwendet. Die Datei wird übersprungen, wenn der Zeitpunkt der letzten Änderung im Ziel aktueller ist.

Folgende Paare werden unterstützt:

- lokal <–> Azure-Blob (SAS- oder OAuth-Authentifizierung kann verwendet werden)

Der Befehl „sync“ unterscheidet sich auf mehrere Arten vom Befehl „copy“:

  1. Das rekursive Flag ist standardmäßig aktiviert.
  2. Quelle und Ziel dürfen keine Muster enthalten (z.B. „*“ oder „?“).
  3. Die Flags „include“ und „exclude“ können eine Liste von Mustern sein, die mit den Dateinamen übereinstimmen. Eine Abbildung finden Sie im Abschnitt „Beispiele“.
  4. Wenn es am Ziel Dateien oder Blobs gibt, die an der Quelle nicht vorhanden sind, wird der Benutzer aufgefordert, sie zu löschen.

     Diese Eingabeaufforderung kann mithilfe der entsprechenden Flags zum automatischen Beantworten der Löschfrage unterdrückt werden.

### <a name="advanced"></a>Erweitert

AzCopy erkennt den Inhaltstyp der Dateien beim Hochladen vom lokalen Datenträger automatisch anhand der Dateierweiterung oder des Inhalts (falls keine Erweiterung angegeben ist).

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

> [!NOTE]
> Wenn „include“/„exclude“-Flags zusammen verwendet werden, würden nur Dateien, die mit den „include“-Mustern übereinstimmen, berücksichtigt: Dateien dagegen, die mit den „exclude“-Mustern übereinstimmen, würden immer ignoriert.

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|–block-size-mb float|Verwenden Sie diese Blockgröße (in MiB) beim Hochladen in Azure Storage oder beim Herunterladen aus Azure Storage. Der Standardwert wird anhand der Dateigröße automatisch berechnet. Dezimalzahlen sind zulässig (Beispiel: 0,25).|
|–check-md5 string|Gibt an, wie streng MD5-Hashes beim Herunterladen überprüft werden sollten. Diese Option ist nur beim Herunterladen verfügbar. Folgende Werte sind verfügbar: „NoCheck“, „LogOnly“, „FailIfDifferent“, „FailIfDifferentOrMissing“. (Die Standardeinstellung ist „FailIfDifferent“).|
|–delete-destination string|Definiert, ob zusätzliche Dateien aus dem Ziel gelöscht werden sollen, die an der Quelle nicht vorhanden sind. Könnte auf „true“, „false“ oder „prompt“ festgelegt werden. Wenn sie auf „prompt“ festgelegt wurde, wird dem Benutzer eine Frage gestellt, bevor Dateien und Blobs zum Löschen geplant werden. (Die Standardeinstellung ist „false“).|
|–exclude string|Schließen Sie Dateien aus, in denen der Name der Musterliste entspricht. Beispiel: *.jpg;* .pdf;exactName.|
|-h, --help|Dient zum Anzeigen von Hilfeinhalt zum Befehl „sync“.|
|–include string|Schließen Sie nur Dateien ein, in denen der Name der Musterliste entspricht. Beispiel: *.jpg;* .pdf;exactName.|
|–log-level string|Dient zum Definieren der Protokollausführlichkeit für die Protokolldatei. Verfügbare Ebenen: INFO (alle Anforderungen/Antworten), WARNING (langsame Antworten), ERROR (nur fehlgeschlagene Anforderungen) und NONE (keine Ausgabeprotokolle). (Die Standardeinstellung ist „INFO“).|
|--put-md5|Dient zum Erstellen eines MD5-Hashs jeder Datei und zum Speichern des Hashs als „Content-MD5“-Eigenschaft des Zielblobs bzw. der Zieldatei. (Standardmäßig wird der Hash NICHT erstellt.) Nur beim Hochladen verfügbar.|
|–recursive|Standardmäßig „true“; überprüfen Sie Unterverzeichnisse rekursiv, wenn Sie zwischen Verzeichnissen synchronisieren. (Die Standardeinstellung ist „true“).|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
