---
title: Hinzufügen der Volltextsuche zu Azure Blob Storage
titleSuffix: Azure Search
description: Extrahieren Sie Inhalte, und versehen Sie Azure-Blobs mit Struktur, wenn Sie in Azure Search einen Volltextsuchindex erstellen.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: search
ms.topic: conceptual
ms.date: 10/09/2019
ms.openlocfilehash: 62ad2d32af10f72974b28bb582eeb5084a5c5ca0
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72515837"
---
# <a name="add-full-text-search-to-azure-blob-data-using-azure-search"></a>Hinzufügen der Volltextsuche zu Azure-Blobdaten mithilfe von Azure Search

Das Durchsuchen verschiedenster in Azure Blob Storage gespeicherter Inhalte kann ein schwer zu lösendes Problem sein. Allerdings können Sie mit [Azure Search](search-what-is-azure-search.md) den Inhalt Ihrer Blobs mit nur wenigen Klicks indizieren und durchsuchen. Azure Search bietet eine vordefinierte Integration für die Indizierung von Blobspeicher über einen [*Blobindexer*](search-howto-indexing-azure-blob-storage.md), der der Indizierung datenquellenabhängige Funktionen hinzufügt.

## <a name="what-it-means-to-add-full-text-search-to-blob-data"></a>Bedeutung des Hinzufügens der Volltextsuche zu Blobdaten

Azure Search ist ein Cloudsuchdienst, der Indizierungs- und Abfragemodule bereitstellt, die mit in Ihrem Suchdienst gehosteten benutzerdefinierten Indizes arbeiten. Die Zusammenstellung Ihrer durchsuchbaren Inhalte mit dem Abfragemodul in der Cloud ist aus Leistungsgründen erforderlich, damit Ergebnisse mit einer Geschwindigkeit zurückgegeben werden, die Benutzer von Suchabfragen erwarten.

Azure Search und Azure Blob Storage sind auf Indexierungsebene integriert. Dabei werden Ihre Blobinhalte als Suchdokumente importiert, die in *invertierte Indizes* und andere Suchstrukturen indiziert werden, die Textabfragen in freier Form und Filterausdrücke unterstützen. Da Ihre Blobinhalte in einem Suchindex indiziert werden, kann beim Zugriff auf Blobinhalte die gesamte Bandbreite der Abfragefunktionen von Azure Search genutzt werden.

Nach Erstellen und Auffüllen des Index existiert er unabhängig von Ihrem Blobcontainer. Sie können jedoch Indizierungsvorgänge erneut ausführen, um Ihren Index mit Änderungen am zugrunde liegenden Container zu aktualisieren. Zeitstempelinformationen zu einzelnen Blobs werden zur Erkennung von Änderungen verwendet. Sie können sich entweder für die geplante Ausführung oder die bedarfsgesteuerte Indizierung als Aktualisierungsmechanismus entscheiden.

Als Eingaben fungieren Ihre Blobs in einem einzelnen Container in Azure Blob Storage. Blobs können aus nahezu beliebigen Arten von Textdaten bestehen. Wenn Ihre Blobs Bilder enthalten, können Sie der [Blobindizierung KI-Erweiterungen hinzufügen](search-blob-ai-integration.md), um Text anhand von Bildern zu erstellen und zu extrahieren.

Die Ausgabe ist stets ein Azure Search-Index, der für schnelles Suchen, Abrufen und Erkunden von Texten in Clientanwendungen genutzt wird. Dazwischen befindet sich die Architektur der Indizierungspipeline selbst. Die Pipeline basiert auf der *Indexer*-Funktion, die im weiteren Verlauf dieses Artikels erläutert wird.

## <a name="start-with-services-and-data"></a>Erste Schritte mit Diensten und Daten

Sie benötigen Azure Search und Azure Blob Storage. In Azure Blob Storage benötigen Sie einen Container, der Quellinhalt bereitstellt.

Sie können direkt auf der Portalseite des Speicherkontos beginnen. Klicken Sie auf der linken Navigationsseite unter **Blob-Dienst** auf **Azure Search hinzufügen**, um einen neuen Dienst zu erstellen oder einen bestehenden auszuwählen. 

Nachdem Sie Ihrem Speicherkonto Azure Search hinzugefügt haben, können Sie den Standardprozess zum Indizieren von Blobdaten befolgen. Wir empfehlen für eine einfache erste Eingabe von Daten den Assistenten **Daten importieren** in Azure Search. Oder rufen Sie die REST-APIs mit einem Tool wie Postman auf. Dieses Tutorial führt Sie durch die Schritte zum Aufrufen der Rest-API in Postman: [Indizieren und Durchsuchen von teilweise strukturierten Daten (JSON-Blobs) in Azure Search](search-semi-structured-data.md). 

## <a name="use-a-blob-indexer"></a>Verwenden eines Blobindexers

Ein *Indexer* ist ein datenquellenabhängiger Subdienst mit interner Logik zum Nehmen von Stichproben von Daten, Lesen von Metadaten, Abrufen von Daten und Serialisieren von Daten aus nativen Formaten in JSON-Dokumente zum anschließenden Import. 

Blobs in Azure Storage werden mithilfe des [Blob Storage-Indexers von Azure Search ](search-howto-indexing-azure-blob-storage.md) indiziert. Sie können diesen Indexer mit dem Assistenten **Daten importieren**, einer REST-API oder dem .NET SDK aufrufen. Im Code verwenden Sie diesen Indexer, indem Sie den Typ festlegen und Verbindungsinformationen angeben, die ein Azure Storage-Konto sowie einen Blobcontainer enthalten. Sie können Ihre Blobs unterteilen, indem Sie ein virtuelles Verzeichnis erstellen, das Sie dann als Parameter übergeben können, oder indem Sie nach einer Dateityperweiterung filtern.

Ein Indexer übernimmt die Dokumententschlüsselung und öffnet ein Blob, um den Inhalt zu inspizieren. Nach dem Herstellen der Verbindung mit der Datenquelle ist dies der erste Schritt in der Pipeline. Bei Blobdaten werden an dieser Stelle PDF-Dateien, Office-Dokumente und andere Inhaltstypen erkannt. Die Dokumententschlüsselung mit Textextraktion erfolgt gebührenfrei. Wenn Ihre Blobs Bildinhalte enthalten, werden Bilder ignoriert, es sei denn, Sie fügen [KI-Erweiterungen](search-blob-ai-integration.md) hinzu. Die Standardindizierung gilt nur für Textinhalte.

Der Blobindexer verfügt über Konfigurationsparameter und unterstützt die Nachverfolgung von Änderungen, wenn die zugrunde liegenden Daten genügend Informationen liefern. Weitere Informationen zur Kernfunktionalität finden Sie unter [Indizieren von Dokumenten in Azure Blob Storage mit Azure Search](search-howto-indexing-azure-blob-storage.md).

### <a name="supported-content-types"></a>Unterstützte Inhaltstypen

Wenn Sie einen Blobindexer auf einen Container anwenden, können Sie mit einer einzigen Abfrage aus den folgenden Inhaltstypen Text und Metadaten extrahieren:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

### <a name="indexing-blob-metadata"></a>Indizieren der Metadaten von Blobs

Das Indizieren von benutzerdefinierten Metadaten sowie von Systemeigenschaften für jedes Blob ist ein häufig auftretendes Szenario, in dem Sie Blobs jedes Inhaltstyps unkompliziert durchsehen können. Auf diese Weise werden die Informationen für alle Blobs unabhängig vom Dokumenttyp indiziert und in einem Index in Azure Search gespeichert. Mit dem neuen Index können Sie dann den gesamten Blob Storage-Inhalt sortieren und filtern sowie Facetten bilden.

### <a name="indexing-json-blobs"></a>Indizieren von JSON-Blobs
Azure Search kann so konfiguriert werden, dass es strukturierten Inhalt aus Blobs extrahiert, die eine JSON-Datei enthalten. Azure Search kann aus JSON-Blobs lesen und den strukturierten Inhalt in den entsprechenden Felder in einem Azure Search-Dokument analysieren. Azure Search kann außerdem Blobs annehmen, die ein Array von JSON-Objekten enthalten und jedes Element mit einem separaten Azure Search-Dokument verknüpfen. Sie können einen Analysemodus festlegen, der den Typ des vom Indexer erzeugten JSON-Objekts beeinflusst.

## <a name="search-blob-content-in-a-search-index"></a>Durchsuchen von Blobinhalten in einem Suchindex 

Die Ausgabe einer Indizierung ist ein Suchindex, der für die interaktive Erkundung mithilfe von Freitext- und gefilterten Abfragen in einer Clientanwendung verwendet wird. Für die erste Erkundung und Überprüfung von Inhalten empfehlen wir, im Portal im [Suchexplorer](search-explorer.md) die Dokumentstruktur zu untersuchen. Sie können im Suchexplorer die [einfache Abfragesyntax](query-simple-syntax.md), [vollständige Abfragesyntax](query-lucene-syntax.md) und [Filterausdruckssyntax](query-odata-filter-orderby-syntax.md) verwenden.

Eine dauerhaftere Lösung besteht darin, Abfrageeingaben zu sammeln und die Antwort als Suchergebnisse in einer Clientanwendung darzustellen. Im folgenden C#-Tutorial wird das Erstellen einer Suchanwendung erläutert: [Erstellen Ihrer ersten Anwendung in Azure Search](tutorial-csharp-create-first-app.md)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Einrichten eines Blobindexers](search-howto-indexing-azure-blob-storage.md) 
