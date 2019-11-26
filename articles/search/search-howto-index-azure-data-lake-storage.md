---
title: Suche über Azure Data Lake Storage Gen2 (Vorschauversion)
titleSuffix: Azure Cognitive Search
description: Erfahren Sie, wie Sie Inhalte und Metadaten in Azure Data Lake Storage Gen2 indizieren. Dieses Feature befindet sich zurzeit in der öffentlichen Vorschau.
manager: nitinme
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 628b8bb5c3cb83ae6038a7150420893d7abe61d5
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74112289"
---
# <a name="indexing-documents-in-azure-data-lake-storage-gen2"></a>Indizieren von Dokumenten in Azure Data Lake Storage Gen2

> [!IMPORTANT] 
> Azure Data Lake Storage Gen2 befindet sich zurzeit in der öffentlichen Vorschau. Die Vorschaufunktionalität wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Füllen Sie [dieses Formular](https://aka.ms/azure-cognitive-search/indexer-preview) aus, wenn Sie Zugriff auf die Vorschauversionen anfordern möchten. Dieses Feature wird durch die [REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Aktuell werden weder das Portal noch das .NET SDK unterstützt.


Beim Einrichten eines Azure-Speicherkontos haben Sie die Möglichkeit, einen [hierarchischen Namespace](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-namespace) zu aktivieren. Auf diese Weise kann die Sammlung von Inhalten in einem Konto in einer Hierarchie von Verzeichnissen und geschachtelten Unterverzeichnissen organisiert werden. Durch das Aktivieren eines hierarchischen Namespace aktivieren Sie [Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction).

In diesem Artikel wird beschrieben, wie Sie mit dem Indizieren von Dokumenten beginnen, die in Azure Data Lake Storage Gen2 vorliegen.

## <a name="set-up-azure-data-lake-storage-gen2-indexer"></a>Einrichten des Azure Data Lake Storage Gen2-Indexers

Sie müssen einige wenige Schritte ausführen, um Inhalte aus Data Lake Storage Gen2 zu indizieren.

### <a name="step-1-sign-up-for-the-preview"></a>Schritt 1: Anmelden für die Vorschau

Melden Sie sich für die Data Lake Storage Gen2-Indexervorschau an, indem Sie [dieses Formular](https://aka.ms/azure-cognitive-search/indexer-preview) ausfüllen. Sie erhalten eine Bestätigungs-E-Mail, sobald Sie für die Vorschau registriert wurden.

### <a name="step-2-follow-the-azure-blob-storage-indexing-setup-steps"></a>Schritt 2: Ausführen der Schritte zum Einrichten der Azure Blob Storage-Indizierung

Sobald Sie die Bestätigung erhalten haben, dass Ihre Registrierung für die Vorschau erfolgreich war, können Sie die Indizierungspipeline erstellen.

Sie können Inhalte und Metadaten aus Data Lake Storage Gen2 unter Verwendung der [REST-API-Version 2019-05-06-Preview](search-api-preview.md) indizieren. Derzeit werden weder das Portal noch das .NET SDK unterstützt.

Das Indizieren von Inhalten in Data Lake Storage Gen2 ist mit dem Indizieren von Inhalten in Azure Blob Storage identisch. Deshalb finden Sie die Informationen zum Einrichten von Datenquelle, Index und Indexer von Data Lake Storage Gen2 unter [Indizieren von Dokumenten in Azure Blob Storage mit Azure Cognitive Search](search-howto-indexing-azure-blob-storage.md). In diesem Artikel werden auch Informationen dazu bereitgestellt, welche Dokumentformate unterstützt werden, welche Blobmetadaten-Eigenschaften extrahiert wird, wie die inkrementelle Indizierung funktioniert usw. Diese Informationen gelten auch für Data Lake Storage Gen2.

## <a name="access-control"></a>Zugriffssteuerung

Azure Data Lake Storage Gen2 implementiert ein [Zugriffssteuerungsmodell](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control), das sowohl die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) in Azure als auch POSIX-ähnliche Zugriffssteuerungslisten (Access Control Lists, ACLs) unterstützt. Beim Indizieren von Inhalten aus Data Lake Storage Gen2 extrahiert Azure Cognitive Search keine RBAC- und ACL-Informationen aus den Inhalten. Als Ergebnis sind diese Informationen nicht in Ihrem Azure Cognitive Search-Index enthalten.

Wenn die Verwaltung der Zugriffssteuerung für jedes Dokument im Index wichtig ist, kann der Anwendungsentwickler [Sicherheitsfilter](https://docs.microsoft.com/azure/search/search-security-trimming-for-azure-search) implementieren.