---
title: azcopy remove | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy remove“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 0cc366ab2cdad9c7258dca905d8f4a06472119fe
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196796"
---
# <a name="azcopy-remove"></a>azcopy remove

Löscht Entitäten aus Azure Storage Blob, Datei und Azure Data Lake Storage Gen2.

## <a name="synopsis"></a>Zusammenfassung

```azcopy
azcopy remove [resourceURL] [flags]
```

## <a name="examples"></a>Beispiele

Entfernen Sie einen einzelnen Blob mit SAS:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Entfernen Sie ein gesamtes virtuelles Verzeichnis mit einer SAS (Shared Access Signature):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Entfernen Sie nur die obersten Blobs in einem virtuellen Verzeichnis, aber nicht die zugehörigen Unterverzeichnisse:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Entfernen Sie eine Teilmenge von Blobs in einem virtuellen Verzeichnis (z.B. nur JPG- und PDF-Dateien oder wenn der Blobname „exactName“ lautet):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --include="*.jpg;*.pdf;exactName"
```

Entfernen Sie ein gesamtes virtuelles Verzeichnis, doch schließen Sie bestimmte Blobs aus dem Umfang aus (z.B. alle Blobs, die mit „foo“ beginnen oder mit „bar“ enden):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --exclude="foo*;*bar"
```

Entfernen Sie eine einzelne Datei aus Data Lake Storage Gen2 (Einschließen und Ausschließen wird nicht unterstützt):

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/file]?[SAS]"
```

Entfernen Sie ein einzelnes Verzeichnis aus Data Lake Storage Gen2 (Einschließen und Ausschließen wird nicht unterstützt):

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|–exclude string|Schließen Sie Dateien aus, in denen der Name der Musterliste entspricht. Beispiel: *.jpg;* .pdf;exactName|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „remove“.|
|–include string|Schließen Sie nur Dateien ein, in denen der Name der Musterliste entspricht. Beispiel: *.jpg;* .pdf;exactName|
|–log-level string|Definieren Sie die Protokollausführlichkeit für die Protokolldatei. Folgende Ebenen sind verfügbar: INFO (alle Anforderungen/Antworten), WARNUNG (langsame Antworten), FEHLER (nur fehlgeschlagene Anforderungen) und KEINE (keine Ausgabeprotokolle). (Standardwert: „Info“)|
|–recursive|Überprüfen Sie Unterverzeichnisse rekursiv, wenn Sie zwischen Verzeichnissen synchronisieren.|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate, in Megabit pro Sekunde. Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“, „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
