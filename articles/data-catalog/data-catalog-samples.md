---
title: Azure Data Catalog – Entwicklerbeispiele
description: Dieser Artikel bietet eine Übersicht über die verfügbaren Entwicklerbeispiele für die REST-API für Data Catalog.
ms.service: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 7d0e27802745dda62f87e412053650907e9b812c
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "68950156"
---
# <a name="azure-data-catalog-developer-samples"></a>Azure Data Catalog – Entwicklerbeispiele

Hier finden Sie Informationen zu den ersten Schritten bei der Entwicklung von Azure Data Catalog-Apps unter Verwendung der Data Catalog-REST-API. Die Data Catalog-REST-API ist eine REST-basierte API, die einen programmgesteuerten Zugriff auf Data Catalog-Ressourcen ermöglicht, um diese Datenressourcen programmgesteuert zu registrieren, zu kommentieren und zu durchsuchen.

## <a name="samples-available-on-githubcom"></a>Auf GitHub.com verfügbare Beispiele

* [Erste Schritte mit Azure Data Catalog](https://github.com/Azure-Samples/data-catalog-dotnet-get-started/)
  
   Das Beispiel mit den ersten Schritten veranschaulicht die Authentifizierung mit Azure AD, um Datenassets mithilfe der Data Catalog-REST-API zu registrieren, zu durchsuchen und zu löschen.
   
* [Erste Schritte mit Azure Data Catalog unter Verwendung eines Dienstprinzipals](https://github.com/Azure-Samples/data-catalog-dotnet-service-principal-get-started/)

   Dieses Beispiel veranschaulicht die Vorgehensweise zum Registrieren, Suchen und Löschen von Datenassets unter Verwendung der Data Catalog-REST-API. In diesem Beispiel wird die Dienstprinzipalauthentifizierung verwendet.

* [Import/Export-Tool für Azure Data Catalog](https://github.com/Azure-Samples/data-catalog-dotnet-import-export/)

   Dieses Beispiel zeigt, wie Sie mithilfe der Data Catalog-REST-API Assets aus Azure Data Catalog abrufen und in einer Datei serialisieren. Zusätzlich veranschaulicht es, wie Sie JSON-serialisierte Assets in den Katalog schieben können. Es unterstützt das Exportieren einer Teilmenge des Katalogs mithilfe einer Suchabfrage.

* [Massenregistrierung und -kommentierung in Azure Data Catalog](https://github.com/Azure-Samples/data-catalog-dotnet-excel-register-data-assets/)
  
   Dieses Beispiel zeigt, wie Sie mithilfe von Data Catalog-REST-API und Open XML eine Massenregistrierung von Datenassets aus einer Excel-Arbeitsmappe durchführen.
  
* [Massenimport von Glossarbegriffen in Azure Data Catalog](https://github.com/Azure-Samples/data-catalog-bulk-import-glossary/)

   In diesem Beispiel wird gezeigt, wie Sie Glossarbegriffe aus CSV-Dateien in ein ADC-Glossar importieren.

* [Massenimport von Beziehungen in Azure Data Catalog](https://github.com/Azure-Samples/data-catalog-bulk-import-relationship/)

   Dieses Beispiel zeigt das programmgesteuerte Importieren von Beziehungsinformationen aus einer CSV-Datei in einen Datenkatalog.

* [Veröffentlichen von Beziehungen in Azure Data Catalog](https://github.com/Azure-Samples/data-catalog-dotnet-publish-relationships/)

   Dieses Beispiel zeigt das programmgesteuerte Veröffentlichen von Beziehungsinformationen in einem Datenkatalog.
   
## <a name="next-steps"></a>Nächste Schritte
[Referenz für die Azure Data Catalog-REST-API](/rest/api/datacatalog/)
