---
title: Azure App Service – Synchronisieren der Netzwerkkonfiguration
description: In diesem Artikel wird erläutert, wie Sie die Netzwerkkonfiguration für den Azure App Service-Hostingplan synchronisieren.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 12/13/2018
ms.openlocfilehash: e7263d6a7716caf9f53e8496c6fb02b6d17b5509
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73687895"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Synchronisieren der Netzwerkkonfiguration für den Azure App Service-Hostingplan

Möglicherweise können Sie trotz [Integrieren Ihrer App in ein Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md) keine Verbindung mit der verwalteten Instanz herstellen. Sie können dann versuchen, die Netzwerkkonfiguration für Ihren Dienstplan zu aktualisieren.

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>Synchronisieren der Netzwerkkonfiguration für den App Service-Hostingplan

Gehen Sie dazu folgendermaßen vor:  

1. Navigieren Sie zum App Service-Plan für Ihre Web-Apps.

   ![App Service-Plan](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Klicken Sie auf **Netzwerk** und dann auf **Klicken Sie zur Verwaltung hier**.

   ![Verwalten des Dienstplans](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Wählen Sie Ihr **VNET** aus, und klicken Sie auf **Netzwerk synchronisieren**.

   ![Netzwerk synchronisieren](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Warten Sie, bis die Synchronisierung abgeschlossen ist.
  
   ![Synchronisierung abgeschlossen](./media/sql-database-managed-instance-sync-networking/sync-done.png)

Nun können Sie versuchen, die Verbindung mit der verwalteten Instanz erneut herzustellen.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Konfigurieren Ihres VNET für die verwaltete Instanz finden Sie unter [VNET-Architektur der verwalteten Instanz](sql-database-managed-instance-connectivity-architecture.md) und [Konfigurieren eines vorhandenen VNET](sql-database-managed-instance-configure-vnet-subnet.md).
