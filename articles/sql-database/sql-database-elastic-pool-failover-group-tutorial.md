---
title: 'Tutorial: Hinzufügen eines Pools für elastische Azure SQL-Datenbank-Instanzen zu einer Failovergruppe | Microsoft-Dokumentation'
description: Fügen Sie einen Pool für elastische Azure SQL-Datenbanken mit dem Azure-Portal, PowerShell oder der Azure CLI zu einer Failovergruppe hinzu.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 06/19/2019
ms.openlocfilehash: 0507b3cb64b4b12bac92cc6bc90120ab4ec56dee
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68568745"
---
# <a name="tutorial-add-an-azure-sql-database-elastic-pool-to-a-failover-group"></a>Tutorial: Hinzufügen eines Pools für elastische Azure SQL-Datenbank-Instanzen zu einer Failovergruppe

Konfigurieren Sie eine ‚Failovergruppe für einen Pool für elastische Azure SQL-Datenbanken, und testen Sie das Failover mithilfe des Azure-Portals.  In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> - Erstellen einer Azure SQL-Einzeldatenbank.
> - Hinzufügen einer Einzeldatenbank in einen Pool für elastische Datenbanken. 
> - Erstellen einer [Failovergruppe](sql-database-auto-failover-group.md) für einen Pool für elastische Datenbanken zwischen zwei logischen SQL Server-Instanzen.
> - Testen des Failovers.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie dieses Tutorial ausführen können, benötigen Sie folgende Komponenten: 

- Ein Azure-Abonnement. [Erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/free/), wenn Sie noch keines besitzen.


## <a name="1---create-a-single-database"></a>1 – Erstellen einer Einzeldatenbank 

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="2---add-single-database-to-elastic-pool"></a>2 – Hinzufügen einer Einzeldatenbank zu einem Pool für elastische Datenbanken

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) links oben auf **Ressource erstellen**.
1. Geben Sie `elastic pool` in das Suchfeld ein, drücken Sie auf die Eingabetaste, wählen Sie das Symbol für **Pool für elastische SQL-Datenbanken** und anschließend **Erstellen** aus. 

    ![Auswählen des Pools für elastische Datenbanken im Marketplace](media/sql-database-elastic-pool-create-failover-group-tutorial/elastic-pool-market-place.png)

1. Konfigurieren Sie den Pool für elastische Datenbanken mit den folgenden Werten:
   - **Name**: Geben Sie einen eindeutigen Namen für den Pool für elastische Datenbanken an, wie z.B. `myElasticPool`. 
   - **Abonnement**: Wählen Sie in der Dropdownliste Ihr Abonnement aus.
   - **Ressourcengruppe**: Wählen Sie `myResourceGroup` aus der Dropdownliste aus. Das ist die Ressourcengruppe, die Sie im Abschnitt 1 erstellt haben. 
   - **Server**: Wählen Sie den in Abschnitt 1 erstellten Server aus der Dropdownliste aus.  

       ![Erstellen eines neuen Servers für den Pool für elastische Datenbanken](media/sql-database-elastic-pool-create-failover-group-tutorial/use-existing-server-for-elastic-pool.png)

   - **Compute und Speicher**: Wählen Sie **Pool für elastische Datenbanken konfigurieren** aus, um Ihren Computespeicher zu konfigurieren, und fügen Sie anschließend Ihre Einzeldatenbank zu Ihrem Pool für elastische Datenbanken hinzu. Belassen Sie auf der Registerkarte **Pooleinstellungen** die Standardeinstellung von Gen5 mit 2 virtuellen Kernen und 32 GB bei. 

1. Wählen Sie auf der Seite **Konfigurieren** die Registerkarte **Datenbanken** und dann **Datenbank hinzufügen** aus. Wählen Sie die in Abschnitt 1 erstellte Datenbank und dann **Anwenden** aus, um sie zu Ihrem Pool für elastische Datenbanken hinzuzufügen. Wählen Sie erneut **Anwenden** aus, um die Einstellungen für Ihren Pool anzuwenden und die Seite **Konfigurieren** zu schließen. 

    ![Hinzufügen von SQL DB zu einem Pool für elastische Datenbanken](media/sql-database-elastic-pool-create-failover-group-tutorial/add-database-to-elastic-pool.png)

1. Klicken Sie auf **Bewerten + erstellen**, um die Einstellung für den Pool für elastische Datenbanken anzuzeigen, und wählen Sie **Erstellen** aus, um Ihren Pool für elastische Datenbanken zu erstellen. 


## <a name="3---create-the-failover-group"></a>3 – Erstellen der Failovergruppe 
In diesem Schritt erstellen Sie eine [Failovergruppen](sql-database-auto-failover-group.md) zwischen einem vorhandenen Azure SQL Server und einem neuen Azure SQL-Server in einer anderen Region. Anschließend fügen Sie dann der Failovergruppe den Pool für elastische Datenbanken hinzu. 


1. Wählen Sie im [Azure-Portal](https://portal.azure.com) oben links die Option **Alle Dienste** aus. 
1. Geben Sie im Suchfeld als Suchbegriff `sql servers` ein. 
1. (Optional) Wählen Sie das Sternsymbol neben SQL Server aus, **um SQL Server-Instanzen** als Favoriten festzulegen, und fügen Sie es dem linken Navigationsbereich hinzu. 
    
    ![Suchen von SQL Server-Instanzen](media/sql-database-single-database-create-failover-group-tutorial/all-services-sql-servers.png)

1. Wählen Sie **SQL Server-Instanzen** und dann den Server aus, den Sie in Abschnitt 1 erstellt haben.
1. Wählen Sie **Failovergruppen** im Bereich **Einstellungen** aus, und klicken Sie dann auf **Gruppe hinzufügen**, um eine neue Failovergruppe zu erstellen. 

    ![Hinzufügen einer neuen Failovergruppe](media/sql-database-elastic-pool-create-failover-group-tutorial/add-elastic-pool-to-failover-group.png)

1. Wählen Sie auf der Seite **Failovergruppe** die folgenden Werte aus bzw. geben Sie sie ein, und wählen Sie dann **Erstellen**:
    - **Name der Failovergruppe**: Geben Sie einen eindeutigen Namen für die Failovergruppe ein, z.B. `failovergrouptutorial`. 
    - **Sekundärer Server**: Wählen Sie die Option  *zum Konfigurieren der erforderlichen Einstellungen* aus, und wählen Sie dann **Neuen Server erstellen** aus. Alternativ können Sie auch einen bereits vorhandenen Server als sekundären Server auswählen. Nachdem Sie die folgenden Werte eingegeben haben, wählen Sie **Auswählen** aus. 
        - **Servername**: Geben Sie einen eindeutigen Namen für den sekundären Server ein, z.B. `mysqlsecondary`. 
        - **Serveradministratoranmeldung**: Geben Sie Folgendes ein: `azureuser`
        - **Kennwort**: Geben Sie ein komplexes Kennwort ein, das die Anforderungen für Kennwörter erfüllt.
        - **Standort**: Wählen Sie in der Dropdownliste einen Standort aus, z.B. USA, Osten 2. Dieser Standort darf nicht mit dem Standort des primären Servers übereinstimmen.

       > [!NOTE]
       > Die Einstellungen für Serveranmeldung und Firewall müssen jedoch mit denen Ihres primären Servers übereinstimmen. 
    
       ![Erstellen eines sekundären Servers für die Failovergruppe](media/sql-database-single-database-create-failover-group-tutorial/create-secondary-failover-server.png)

1. Sobald ein sekundärer Server ausgewählt wurde, wird die Option **Datenbanken innerhalb der Gruppe** entsperrt. Wählen Sie die Option aus, um **Hinzuzufügende Datenbank auswählen** auszuwählen. Wählen Sie anschließend den in Abschnitt 2 erstellten Pool für elastische Datenbanken aus. Es wird eine Warnung angezeigt, in der Sie aufgefordert werden, einen Pool für elastische Datenbanken auf dem sekundären Server zu erstellen. Wählen Sie die Warnung aus, und klicken Sie dann auf **OK**, um den Pool für elastische Datenbanken auf dem sekundären Server zu erstellen. 
        
    ![Hinzufügen von SQL DB zur Failovergruppe](media/sql-database-single-database-create-failover-group-tutorial/add-sqldb-to-failover-group.png)
        
1. Wählen Sie **Auswählen** aus, um die Einstellungen für den Pool für elastische Datenbanken auf die Failovergruppe anzuwenden, und klicken Sie dann auf **Erstellen**, um Ihre Failovergruppe zu erstellen. Durch das Hinzufügen des Pool für elastische Datenbanken zur Failovergruppe wird automatisch der Georeplikationsprozess gestartet. 


## <a name="4---test-failover"></a>4 – Testen des Failovers 
In diesem Schritt führen Sie ein Failover für Ihre Failovergruppe auf dem sekundären Server und anschließend ein Failback mit dem Azure-Portal aus. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Server **SQL Server-Instanzen**. 
1. Wählen Sie **Failovergruppen** im Bereich **Einstellungen** aus, und wählen Sie dann die in Abschnitt 2 erstellte Failovergruppe aus. 
  
   ![Auswählen der Failovergruppe aus dem Portal](media/sql-database-single-database-create-failover-group-tutorial/select-failover-group.png)

1. Überprüfen Sie, welcher Server der primäre und welcher der sekundäre ist. 
1. Wählen Sie im Aufgabenbereich **Failover** aus, um ein Failover für die Failovergruppe mit dem Pool für elastische Datenbanken durchzuführen. 
1. Wählen Sie in der Meldung, dass die TDS-Sitzungen getrennt werden, **Ja** aus. 

   ![Failover für die Failovergruppe mit Ihrer SQL-Datenbank](media/sql-database-single-database-create-failover-group-tutorial/failover-sql-db.png)

1. Überprüfen Sie, welcher Server der primäre und welcher der sekundäre ist. Wenn das Failover erfolgreich ausgeführt wurde, sollten die beiden Server die Rollen getauscht haben. 
1. Wählen Sie erneut **Failover** aus, um die Failovergruppe wieder auf die ursprünglichen Einstellungen zurückzusetzen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 
Bereinigen Sie die Ressourcen, indem Sie die Ressourcengruppe löschen. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Ressourcengruppe.
1. Wählen Sie **Ressourcengruppe löschen** aus, um alle Ressourcen in der Gruppe sowie die Ressourcengruppe selbst zu löschen. 
1. Geben Sie den Namen der Ressourcengruppe `myResourceGroup` in das Textfeld ein, und wählen Sie dann **Löschen**, um die Ressourcengruppe zu löschen.  


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Azure SQL-Einzeldatenbank zu einer Failovergruppe hinzugefügt und das Failover getestet. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> - Erstellen einer Azure SQL-Einzeldatenbank.
> - Hinzufügen einer Einzeldatenbank in einen Pool für elastische Datenbanken. 
> - Erstellen einer [Failovergruppe](sql-database-auto-failover-group.md) für einen Pool für elastische Datenbanken zwischen zwei logischen SQL Server-Instanzen.
> - Testen des Failovers.

Fahren Sie mit dem nächsten Tutorial zur Migration mithilfe von DMS fort.

> [!div class="nextstepaction"]
> [Tutorial: Migrieren von SQL Server zu einer Pooldatenbank mit DMS](../dms/tutorial-sql-server-to-azure-sql.md?toc=/azure/sql-database/toc.json)
