---
title: Verbindung einer verwalteten Azure SQL-Instanz für die Suchindizierung – Azure Search
description: Aktivieren Sie den öffentlichen Endpunkt, um Verbindungen mit verwalteten SQL-Instanzen von einem Indexer auf Azure Search zuzulassen.
author: vl8163264128
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: victliu
ms.openlocfilehash: 07bc1a55c0222fda87b28acbaa8bfe552fb8e6ed
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70186683"
---
# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-managed-instance"></a>Konfigurieren einer Verbindung eines Azure Search-Indexers mit einer verwalteten SQL-Instanz
Wie in [Verbinden von Azure SQL-Datenbank mit Azure Search mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq) beschrieben, wird das Erstellen von Indexern für **verwaltete SQL-Instanzen** von Azure Search über den öffentlichen Endpunkt unterstützt.

## <a name="create-azure-sql-managed-instance-with-public-endpoint"></a>Erstellen einer verwalteten Azure SQL-Instanz mit öffentlichem Endpunkt
Erstellen Sie eine verwaltete SQL-Instanz mit der ausgewählten Option **Öffentlichen Endpunkt aktivieren**.

   ![Öffentlichen Endpunkt aktivieren](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/enable-public-endpoint.png "Öffentlichen Endpunkt aktivieren")

## <a name="enable-azure-sql-managed-instance-public-endpoint"></a>Aktivieren einer verwalteten Azure SQL-Instanz mit öffentlichem Endpunkt
Sie können den öffentlichen Endpunkt auch auf einer vorhandenen verwalteten SQL-Instanz unter **Sicherheit** > **Virtuelles Netzwerk** > **Öffentlicher Endpunkt**  > **Aktivieren** aktivieren.

   ![Öffentlichen Endpunkt aktivieren](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-vnet.png "Öffentlichen Endpunkt aktivieren")

## <a name="verify-nsg-rules"></a>Überprüfen von NSG-Regeln
Überprüfen Sie, ob die Netzwerksicherheitsgruppe die richtigen **Eingangssicherheitsregeln** hat, die Verbindungen von Azure-Diensten erlauben.

   ![NSG-Eingangssicherheitsregel](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/nsg-rule.png "NSG-Eingangssicherheitsregel")

## <a name="get-public-endpoint-connection-string"></a>Abrufen der Verbindungszeichenfolge für den öffentlichen Endpunkt
Stellen Sie sicher, dass Sie die Verbindungszeichenfolge für den **öffentlichen Endpunkt** verwenden (Port 3342, nicht Port 1433).

   ![Verbindungszeichenfolge für den öffentlichen Endpunkt](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-connection-string.png "Verbindungszeichenfolge für den öffentlichen Endpunkt")

## <a name="next-steps"></a>Nächste Schritte
Da die Konfiguration nun abgeschlossen ist, können Sie jetzt entweder über das Portal oder mit der REST-API eine verwaltete SQL-Instanz als Datenquelle für einen Azure Search-Indexer angeben. Weitere Informationen finden Sie unter [Verbinden von Azure SQL-Datenbank mit Azure Search mithilfe von Indexern](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) .
