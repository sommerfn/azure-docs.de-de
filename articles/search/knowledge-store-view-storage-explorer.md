---
title: Anzeigen eines Wissensspeichers mit Storage-Explorer
titleSuffix: Azure Cognitive Search
description: Anzeigen und Analysieren eines Azure Cognitive Search-Wissensspeichers mit dem Storage-Explorer des Azure-Portals
manager: nitinme
author: lisaleib
ms.author: v-lilei
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: cfa85e61059e27cd39a9701a835a725e16e5bc0a
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72789972"
---
# <a name="view-a-knowledge-store-with-storage-explorer"></a>Anzeigen eines Wissensspeichers mit Storage-Explorer

> [!Note]
> Der Wissensspeicher befindet sich in der Vorschauphase und sollte nicht in der Produktion verwendet werden. Dieses Feature wird durch die [Azure Cognitive Search-REST-API-Version 2019-05-06-Preview](search-api-preview.md) bereitgestellt. Das .NET SDK wird derzeit nicht unterstützt.
>
In diesem Artikel erfahren Sie, wie Sie mit dem Storage-Explorer im Azure-Portal eine Verbindung mit einem Wissensspeicher herstellen und ihn anschließend erkunden. Informationen zum Erstellen des in dieser exemplarischen Vorgehensweise verwendeten Beispielwissensspeichers finden Sie unter [Erstellen eines Wissensspeichers im Azure-Portal](knowledge-store-create-portal.md).

## <a name="prerequisites"></a>Voraussetzungen

+ Führen Sie die unter [Erstellen eines Wissensspeichers im Azure-Portal](knowledge-store-create-portal.md) beschriebenen Schritte aus, um den Beispielwissensspeicher zu erstellen, der in dieser exemplarischen Vorgehensweise verwendet wird.

+ Sie benötigen außerdem den Namen des Azure-Speicherkontos, das Sie zum Erstellen des Wissensspeichers verwendet haben, sowie den Zugriffsschlüssel aus dem Azure-Portal.

## <a name="view-edit-and-query-a-knowledge-store-in-storage-explorer"></a>Anzeigen, Bearbeiten und Abfragen eines Wissensspeichers im Storage-Explorer

1. Öffnen Sie im Azure-Portal das Speicherkonto, das Sie zum Erstellen des Wissensspeichers verwendet haben.

1. Klicken Sie im linken Navigationsbereich des Speicherkontos auf **Storage-Explorer**.

1. Erweitern Sie die Liste **TABELLEN**, um eine Liste der Azure-Tabellenprojektionen anzuzeigen, die erstellt wurden, als Sie den Assistenten zum **Importieren von Daten** für Ihre Beispieldaten zu Hotelrezensionen ausgeführt haben.

Wählen Sie eine beliebige Tabelle zum Anzeigen der erweiterten Daten aus, einschließlich der Stimmungswerte für Schlüsselbegriffe, der Breiten- und Längengraddaten und mehr.

   ![Anzeigen von Tabellen im Storage-Explorer](media/knowledge-store-view-storage-explorer/storage-explorer-tables.png "Anzeigen von Tabellen im Storage-Explorer")

Zum Ändern des Datentyps für einen beliebigen Tabellenwert oder zum Ändern einzelner Werte in der Tabelle klicken Sie auf **Bearbeiten**. Wenn Sie den Datentyp für eine Spalte in einer Tabellenzeile ändern, erfolgt die Änderung für alle Zeilen.

   ![Bearbeiten einer Tabelle im Storage-Explorer](media/knowledge-store-view-storage-explorer/storage-explorer-edit-table.png "Bearbeiten einer Tabelle im Storage-Explorer")

Klicken Sie zum Ausführen von Abfragen in der Befehlsleiste auf **Abfrage**, und geben Sie Ihre Bedingungen ein.  

   ![Abfragen einer Tabelle im Storage-Explorer](media/knowledge-store-view-storage-explorer/storage-explorer-query-table.png "Abfragen einer Tabelle im Storage-Explorer")

## <a name="clean-up"></a>Bereinigen

Wenn Sie in Ihrem eigenen Abonnement arbeiten, sollten Sie sich am Ende eines Projekts überlegen, ob Sie die erstellten Ressourcen noch benötigen. Ressourcen, die weiterhin ausgeführt werden, können Sie Geld kosten. Sie können entweder einzelne Ressourcen oder aber die Ressourcengruppe löschen, um den gesamten Ressourcensatz zu entfernen.

Ressourcen können im Portal über den Link **Alle Ressourcen** oder **Ressourcengruppen** im linken Navigationsbereich gesucht und verwaltet werden.

Denken Sie bei Verwendung eines kostenlosen Diensts an die Beschränkung auf maximal drei Indizes, Indexer und Datenquellen. Sie können einzelne Elemente über das Portal löschen, um unter dem Limit zu bleiben.

## <a name="next-steps"></a>Nächste Schritte

Im folgenden Artikel erfahren Sie, wie Sie diesen Wissensspeicher mit Power BI verbinden:

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit Power BI](knowledge-store-connect-power-bi.md)

Informationen zum Erstellen eines Wissensspeichers mithilfe der REST-APIs und Postman finden Sie im folgenden Artikel.  

> [!div class="nextstepaction"]
> [Erstellen von Wissensspeichern in REST](knowledge-store-howto.md)
