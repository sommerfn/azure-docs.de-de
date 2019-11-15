---
title: 'Schnellstart: Skalieren von Computeressourcen – Azure-Portal '
description: Skalieren Sie Computeressourcen in Azure SQL Data Warehouse im Azure-Portal. Skalieren Sie Computeressourcen zentral hoch, um eine bessere Leistung zu erzielen, oder skalieren Sie sie zentral herunter, um Kosten einzusparen.
services: sql-data-warehouse
author: Antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: implement
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: jrasnick
ms.custom: seo-lt-2019
ms.openlocfilehash: 59d929165ac9618d68707e2f13741e7bbba7b37f
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73685966"
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Schnellstart: Skalieren von Computeressourcen in Azure SQL Data Warehouse im Azure-Portal

Skalieren Sie Computeressourcen in Azure SQL Data Warehouse im Azure-Portal. [Skalieren Sie Computeressourcen zentral hoch](sql-data-warehouse-manage-compute-overview.md), um eine bessere Leistung zu erzielen, oder skalieren Sie sie zentral herunter, um Kosten einzusparen. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="before-you-begin"></a>Voraussetzungen

Sie können ein bereits vorhandenes Data Warehouse skalieren oder anhand der Informationen unter [Schnellstart: Erstellen und Abfragen einer Azure SQL Data Warehouse-Instanz im Azure-Portal](create-data-warehouse-portal.md) ein Data Warehouse namens **mySampleDataWarehouse** erstellen.  Mit diesem Schnellstart wird **mySampleDataWarehouse** skaliert.

>[!Note]
>Ihr Data Warehouse muss zur Skalierung online sein. 

## <a name="scale-compute"></a>Skalieren von Computeressourcen

SQL Data Warehouse-Computeressourcen können durch Erhöhen bzw. Verringern der Data Warehouse-Einheiten skaliert werden. Mit dem [Schnellstart: Erstellen und Abfragen einer Azure SQL Data Warehouse-Instanz im Azure-Portal] (create-data-warehouse-portal.md) wurde **mySampleDataWarehouse** erstellt und mit 400 DWUs initialisiert. In den folgenden Schritten werden die DWUs für **mySampleDataWarehouse** angepasst.

So ändern Sie Data Warehouse-Einheiten

1. Klicken Sie auf der linken Seite im Azure-Portal auf **SQL Data Warehouses**.
2. Wählen Sie auf der Seite **SQL Data Warehouses** den Eintrag **mySampleDataWarehouse** aus. Das Data Warehouse wird geöffnet.
3. Klicken Sie auf **Skalieren**.

    ![Klicken Sie auf Skalieren.](media/quickstart-scale-compute-portal/click-scale.png)

2. Verschieben Sie im Bereich „Skalieren“ den Schieberegler nach links oder rechts, um die DWU-Einstellung zu ändern.

    ![Schieberegler bewegen](media/quickstart-scale-compute-portal/scale-dwu.png)

3. Klicken Sie auf **Speichern**. Eine Bestätigungsmeldung wird angezeigt. Klicken Sie zur Bestätigung auf **Ja** oder zum Abbrechen auf **Nein**.

    ![Klicken Sie auf Speichern.](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Computeressourcen für Ihr Data Warehouse skalieren. Weitere Informationen zu Azure SQL Data Warehouse erhalten Sie im Tutorial zum Laden von Daten.

> [!div class="nextstepaction"]
>[Laden von Daten in SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
