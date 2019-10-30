---
title: Verbindung einer verwalteten Azure SQL-Datenbank-Instanz für die Suchindizierung
titleSuffix: Azure Cognitive Search
description: Aktivieren Sie den öffentlichen Endpunkt, um Verbindungen mit verwalteten SQL-Datenbank-Instanzen von einem Indexer in der kognitiven Azure-Suche zuzulassen.
manager: nitinme
author: vl8163264128
ms.author: victliu
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 16daf4a79252134703715ccd88f0b10dda7f4fa6
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792168"
---
# <a name="configure-a-connection-from-an-azure-cognitive-search-indexer-to-sql-managed-instance"></a>Konfigurieren einer Verbindung eines Indexers der kognitiven Azure-Suche mit einer verwalteten SQL-Datenbank-Instanz

Wie unter [Verbinden von Azure SQL-Datenbank mit der kognitiven Azure-Suche mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq) beschrieben, wird das Erstellen von Indexern für **verwaltete SQL-Datenbank-Instanzen** in der kognitiven Azure-Suche über den öffentlichen Endpunkt unterstützt.

## <a name="create-azure-sql-managed-instance-with-public-endpoint"></a>Erstellen einer verwalteten Azure SQL-Instanz mit öffentlichem Endpunkt
Erstellen Sie eine verwaltete SQL-Instanz mit der ausgewählten Option **Öffentlichen Endpunkt aktivieren**.

   ![Öffentlichen Endpunkt aktivieren](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/enable-public-endpoint.png "Aktivieren des öffentlichen Endpunkts")

## <a name="enable-azure-sql-managed-instance-public-endpoint"></a>Aktivieren einer verwalteten Azure SQL-Instanz mit öffentlichem Endpunkt
Sie können den öffentlichen Endpunkt auch auf einer vorhandenen verwalteten SQL-Instanz unter **Sicherheit** > **Virtuelles Netzwerk** > **Öffentlicher Endpunkt**  > **Aktivieren** aktivieren.

   ![Öffentlichen Endpunkt aktivieren](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-vnet.png "Aktivieren des öffentlichen Endpunkts")

## <a name="verify-nsg-rules"></a>Überprüfen von NSG-Regeln
Überprüfen Sie, ob die Netzwerksicherheitsgruppe die richtigen **Eingangssicherheitsregeln** hat, die Verbindungen von Azure-Diensten erlauben.

   ![NSG-Eingangssicherheitsregel](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/nsg-rule.png "NSG-Eingangssicherheitsregel")

## <a name="get-public-endpoint-connection-string"></a>Abrufen der Verbindungszeichenfolge für den öffentlichen Endpunkt
Stellen Sie sicher, dass Sie die Verbindungszeichenfolge für den **öffentlichen Endpunkt** verwenden (Port 3342, nicht Port 1433).

   ![Verbindungszeichenfolge für den öffentlichen Endpunkt](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-connection-string.png "Verbindungszeichenfolge für den öffentlichen Endpunkt")

## <a name="next-steps"></a>Nächste Schritte
Da die Konfiguration nun abgeschlossen ist, können Sie jetzt entweder über das Portal oder mit der REST-API eine verwaltete SQL-Datenbank-Instanz als Datenquelle für einen Indexer der kognitiven Azure-Suche angeben. Weitere Informationen finden Sie unter [Verbinden von Azure SQL-Datenbank mit der kognitiven Azure-Suche mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md).
