---
title: 'Tutorial: Hinzufügen einer verwalteten SQL-Datenbank-Instanz zu einer Failovergruppe'
description: Erfahren Sie, wie Sie eine Failovergruppe für die verwaltete Azure SQL-Datenbank-Instanz konfigurieren.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sashan, carlrab
manager: jroth
ms.date: 06/27/2019
ms.openlocfilehash: e4b7de3931c0d3508e5af6aa6bf85dfa18641aee
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69624983"
---
# <a name="tutorial-add-a-sql-database-managed-instance-to-a-failover-group"></a>Tutorial: Hinzufügen einer verwalteten SQL-Datenbank-Instanz zu einer Failovergruppe

Hinzufügen einer verwalteten SQL-Datenbank-Instanz zu einer Failovergruppe. In diesem Artikel wird Folgendes behandelt:

> [!div class="checklist"]
> - Erstellen einer primären verwalteten Instanz
> - Erstellen einer sekundären verwalteten Instanz als Teil einer [Failovergruppe](sql-database-auto-failover-group.md). 
> - Testfailover

  > [!NOTE]
  > Das Erstellen einer verwalteten Instanz kann sehr viel Zeit in Anspruch nehmen. Folglich kann es mehrere Stunden dauern, bis Sie dieses Tutorial fertiggestellt haben. Weitere Informationen zu den Bereitstellungszeiten finden Sie unter [Verwaltungsvorgänge für verwaltete Instanzen](sql-database-managed-instance.md#managed-instance-management-operations). Die Verwendung von Failovergruppen mit verwalteten Instanzen befindet sich zurzeit in der Vorschau. 

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie dieses Tutorial ausführen können, benötigen Sie folgende Komponenten: 

- Falls Sie noch nicht über ein Azure-Abonnement verfügen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/). 


## <a name="1----create-resource-group-and-primary-managed-instance"></a>1\. Erstellen einer Ressourcengruppe und einer primären verwalteten Instanz
In diesem Schritt erstellen Sie die Ressourcengruppe und die primäre verwaltete Instanz für Ihre Failovergruppe mithilfe des Azure-Portals. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**. 
1. Geben Sie `managed instance` in das Suchfeld ein, und wählen Sie die Option für die verwaltete Azure SQL-Instanz aus. 
1. Wählen Sie **Erstellen** aus, um die Erstellungsseite für **Verwaltete SQL-Instanz** zu starten. 
1. Auf der Seite **Verwaltete Azure SQL-Datenbank-Instanz erstellen** auf der Registerkarte **Grundlagen**:
    1. Wählen Sie unter **Projektdetails** Ihr **Abonnement** aus der Dropdownliste aus, und wählen Sie dann **Neu erstellen** für die Ressourcengruppe aus. Geben Sie einen Namen für Ihre Ressourcengruppe ein, etwa `myResourceGroup`. 
    1. Geben Sie unter **Details der verwalteten Instanz** den Namen Ihrer verwalteten Instanz und die Region an, in der Sie die verwaltete Instanz bereitstellen möchten. Achten Sie darauf, eine Region mit einem [Regionspaar](/azure/best-practices-availability-paired-regions) auszuwählen. Belassen Sie für **Compute und Speicher** die Standardwerte. 
    1. Geben Sie unter **Administratorkonto** eine Administratoranmeldung an (z.B. `azureuser`) sowie ein komplexes Administratorkennwort. 

    ![Erstellen der primären verwalteten Instanz](media/sql-database-managed-instance-failover-group-tutorial/primary-sql-mi-values.png)

1. Belassen Sie die restlichen Einstellungen bei den Standardwerten, und wählen Sie **Überprüfen und erstellen** aus, um die Einstellungen Ihrer verwalteten Instanz zu überprüfen. 
1. Wählen Sie **Erstellen** aus, um die primäre verwaltete Instanz zu erstellen. 


## <a name="2---create-a-virtual-network"></a>2\. Erstellen eines virtuelles Netzwerks
In diesem Schritt erstellen Sie ein virtuelles Netzwerk für die sekundäre verwaltete Instanz. Dieser Schritt ist notwendig, da es erforderlich ist, dass das Subnetz der primären und sekundären verwalteten Instanzen keine überlappenden Adressbereiche aufweist. 

Führen Sie die folgenden Schritte aus, um den Subnetzbereich Ihres primären virtuellen Netzwerks zu überprüfen:
1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Ressourcengruppe, und wählen Sie das virtuelle Netzwerk für Ihre primäre Instanz aus. 
1. Wählen Sie unter **Einstellungen** die Option **Subnetze**  aus, und notieren Sie sich den **Adressbereich**. Der Subnetzadressbereich des virtuellen Netzwerks für die sekundäre verwaltete Instanz darf keine Überschneidung mit diesem Adressbereich aufweisen. 


   ![Primäres Subnetz](media/sql-database-managed-instance-failover-group-tutorial/verify-primary-subnet-range.png)

Führen Sie die folgenden Schritte aus, um ein virtuelles Netzwerk zu erstellen:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf **Ressource erstellen**, und suchen Sie nach *virtuelles Netzwerk*. 
1. Wählen Sie die von Microsoft veröffentlichte Option **Virtuelles Netzwerk** aus, und klicken Sie dann auf der nächsten Seite auf **Erstellen**. 
1. Füllen Sie die Pflichtfelder aus, um das virtuelle Netzwerk für die sekundäre verwaltete Instanz zu konfigurieren, und wählen Sie dann **Erstellen** aus. 

   Die folgende Tabelle enthält die erforderlichen Werte für das sekundäre virtuelle Netzwerk:

    | **Feld** | Wert |
    | --- | --- |
    | **Name** |  Der Name des virtuellen Netzwerks, das von der sekundären verwalteten Instanz verwendet werden soll, z.B `vnet-sql-mi-secondary`. |
    | **Adressraum** | Der Adressraum für Ihr virtuelles Netzwerk, z.B. `10.128.0.0/16`. | 
    | **Abonnement** | Das Abonnement, in dem sich Ihre primäre verwaltete Instanz und die Ressourcengruppe befinden. |
    | **Region** | Der Speicherort, an dem die sekundäre verwaltete Instanz bereitgestellt werden soll. Diese sollte sich in einem [Regionspaar](/azure/best-practices-availability-paired-regions) der primären verwalteten Instanz befinden.  |
    | **Subnetz** | Der Name für Ihr Subnetz. `default` wird standardmäßig bereitgestellt. |
    | **Adressbereich**| Der Adressbereich für das Subnetz. Dieser muss sich von dem Subnetzadressbereich unterscheiden, der vom virtuellen Netzwerk der primären verwalteten Instanz verwendet wird, z.B. `10.128.0.0/24`.  |
    | &nbsp; | &nbsp; |

    ![Werte des sekundären virtuellen Netzwerks](media/sql-database-managed-instance-failover-group-tutorial/secondary-virtual-network.png)


## <a name="3---create-a-secondary-managed-instance"></a>3\. Erstellen einer sekundären verwalteten Instanz
In diesem Schritt erstellen Sie eine sekundäre verwaltete Instanz im Azure-Portal, durch die auch die Netzwerkverbindung zwischen den beiden verwalteten Instanzen konfiguriert wird. 

Die zweite verwaltete Instanz muss die folgenden Bedingungen erfüllen:
- Sie muss leer sein. 
- Sie muss sich in einem [Regionspaar](/azure/best-practices-availability-paired-regions) mit der zugehörigen primären verwalteten Instanz befinden. 
- Sie muss über ein anderes Subnetz und einen anderen IP-Adressbereich als die primäre verwaltete Instanz verfügen. 

Führen Sie die folgenden Schritte aus, um Ihre sekundäre verwaltete Instanz zu erstellen: 

1. Wählen Sie im [Azure-Portal](http://portal.azure.com) die Option **Ressource erstellen** aus, und suchen Sie dann nach *Verwaltete Azure SQL-Instanz*. 
1. Wählen Sie die von Microsoft veröffentlichte Option **Verwaltete Azure SQL-Instanz** aus, und klicken Sie dann auf der nächsten Seite auf **Erstellen**.
1. Füllen Sie auf der Registerkarte **Grundlagen** der Seite **Verwaltete Azure SQL-Datenbank-Instanz erstellen** die Pflichtfelder aus, um die sekundäre verwaltete Instanz zu konfigurieren. 

   Die folgende Tabelle enthält die erforderlichen Werte für die sekundäre verwaltete Instanz:
 
    | **Feld** | Wert |
    | --- | --- |
    | **Abonnement** |  Das Abonnement, in dem sich Ihre primäre verwaltete Instanz befindet. |
    | **Ressourcengruppe**| Die Ressourcengruppe, in der sich Ihre primäre verwaltete Instanz befindet. |
    | **Name der verwalteten Instanz** | Der Name der neuen sekundären verwalteten Instanz, z.B. `sql-mi-secondary`  | 
    | **Region**| Das [Regionspaar](/azure/best-practices-availability-paired-regions) für die sekundäre verwaltete Instanz.  |
    | **Administratoranmeldung für verwaltete Instanz** | Die Anmeldung, die Sie für Ihre neue sekundäre verwaltete Instanz verwenden möchten, z.B. `azureuser`. |
    | **Kennwort** | Ein komplexes Kennwort, das von der Administratoranmeldung für die neue sekundäre verwaltete Instanz verwendet wird.  |
    | &nbsp; | &nbsp; |

1. Wählen Sie auf der Registerkarte **Netzwerk** als **Virtuelles Netzwerk** das virtuelle Netzwerk aus, das Sie für die sekundäre verwaltete Instanz erstellt haben.

   ![Netzwerk der sekundären verwalteten Instanz](media/sql-database-managed-instance-failover-group-tutorial/networking-settings-for-secondary-mi.png)

1. Wählen Sie unter der Registerkarte **Weitere Einstellungen** für **Georeplikation** die Angabe **Ja** aus, um die _Verwendung als sekundäre Failoverinstanz_ anzugeben. Wählen Sie die primäre verwaltete Instanz aus der Dropdownliste aus. 
    1. Stellen Sie sicher, dass die Sortierung und die Zeitzone mit der primären verwalteten Instanz übereinstimmen. Die in diesem Tutorial erstellte primäre verwaltete Instanz verwendet den Standardwert `SQL_Latin1_General_CP1_CI_AS`-Sortierung und die Zeitzone `(UTC) Coordinated Universal Time`. 

   ![Netzwerk der sekundären verwalteten Instanz](media/sql-database-managed-instance-failover-group-tutorial/secondary-mi-failover.png)

1. Wählen Sie **Überprüfen und erstellen** aus, um die Einstellungen für die sekundäre verwaltete Instanz zu überprüfen. 
1. Wählen Sie **Erstellen** aus, um die sekundäre verwaltete Instanz zu erstellen. 


## <a name="4---create-primary-virtual-network-gateway"></a>4\. Erstellen eines Gateways für das virtuelle Netzwerk 

Damit zwei verwaltete Instanzen an einer Failovergruppe teilnehmen können, muss zwischen den virtuellen Netzwerken der beiden verwalteten Instanzen ein Gateway konfiguriert sein, um die Netzwerkkommunikation zuzulassen. Sie können das Gateway für die primäre verwaltete Instanz mithilfe des Azure-Portals erstellen:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Ressourcengruppe, und wählen Sie die Ressource **Virtuelles Netzwerk** für Ihre primäre verwaltete Instanz aus. 
1. Wählen Sie unter **Einstellungen**die Option **Subnetze** aus, und wählen Sie dann ein neues**Gatewaysubnetz** aus. Lassen Sie die Standardwerte unverändert. 

   ![Hinzufügen eines Gateways für die primäre verwaltete Instanz](media/sql-database-managed-instance-failover-group-tutorial/add-subnet-gateway-primary-vnet.png)

1. Nachdem das Subnetzgateway erstellt wurde, wählen Sie im linken Navigationsbereich **Ressource erstellen** aus, und geben Sie dann `Virtual network gateway` in das Suchfeld ein. Wählen Sie die Ressource **Virtuelles Netzwerkgateway** aus, die von **Microsoft** veröffentlicht wurde. 

   ![Erstellen eines neuen Gateways für das virtuelle Netzwerk](media/sql-database-managed-instance-failover-group-tutorial/create-virtual-network-gateway.png)

1. Füllen Sie die Pflichtfelder aus, um das Gateway ihrer primären verwalteten Instanz zu konfigurieren. 

   Die folgende Tabelle enthält die erforderlichen Werte das Gateway für die primäre verwaltete Instanz:
 
    | **Feld** | Wert |
    | --- | --- |
    | **Abonnement** |  Das Abonnement, in dem sich Ihre primäre verwaltete Instanz befindet. |
    | **Name** | Der Name für das Gateway des virtuellen Netzwerks, z.B. `primary-mi-gateway`. | 
    | **Region** | Die Region, in der sich die sekundäre verwaltete Instanz befindet. |
    | **Gatewaytyp** | Wählen Sie **VPN** aus. |
    | **VPN-Typ** | Wählen Sie **Routenbasiert** aus. |
    | **SKU**| Lassen Sie den Standardwert `VpnGw1` unverändert. |
    | **Location**| Der Standort, an dem sich Ihre primäre verwaltete Instanz und das primäre virtuelle Netzwerk befinden.   |
    | **Virtuelles Netzwerk**| Wählen Sie das virtuelle Netzwerk aus, das in Abschnitt 2 erstellt wurde, z.B. `vnet-sql-mi-primary`. |
    | **Öffentliche IP-Adresse**| Wählen Sie **Neu erstellen**. |
    | **Name der öffentlichen IP-Adresse**| Geben Sie einen Namen für die IP-Adresse ein, z.B. `primary-gateway-IP`. |
    | &nbsp; | &nbsp; |
1. Belassen Sie die anderen Werte als Standardwerte, und wählen Sie dann **Überprüfen und erstellen** aus, um die Einstellungen für Ihr virtuelles Netzwerkgateway zu überprüfen.

   ![Primäre Gatewayeinstellungen](media/sql-database-managed-instance-failover-group-tutorial/settings-for-primary-gateway.png)

1. Wählen Sie **Erstellen** aus, um das neue Gateway für Ihr virtuelles Netzwerk zu erstellen. 

## <a name="5---configure-secondary-virtual-network-gateway"></a>5\. Konfigurieren des Gateways für das sekundäre virtuelle Netzwerk 

Wiederholen Sie die Schritte im vorherigen Abschnitt, um das Subnetz des virtuellen Netzwerks und das Gateway für die sekundäre verwaltete Instanz zu erstellen. Füllen Sie die Pflichtfelder aus, um das Gateway für die sekundäre verwaltete Instanz zu konfigurieren. 

   Die folgende Tabelle enthält die erforderlichen Werte für das Gateway für die sekundäre verwaltete Instanz:

   | **Feld** | Wert |
   | --- | --- |
   | **Abonnement** |  Das Abonnement, in dem sich Ihre sekundäre verwaltete Instanz befindet. |
   | **Name** | Der Name für das Gateway des virtuellen Netzwerks, z.B. `secondary-mi-gateway`. | 
   | **Region** | Die Region, in der sich die sekundäre verwaltete Instanz befindet. |
   | **Gatewaytyp** | Wählen Sie **VPN** aus. |
   | **VPN-Typ** | Wählen Sie **Routenbasiert** aus. |
   | **SKU**| Lassen Sie den Standardwert `VpnGw1` unverändert. |
   | **Location**| Der Standort, an dem sich Ihre sekundäre verwaltete Instanz und das sekundäre virtuelle Netzwerk befinden.   |
   | **Virtuelles Netzwerk**| Wählen Sie das virtuelle Netzwerk aus, das in Abschnitt 2 erstellt wurde, z.B. `vnet-sql-mi-secondary`. |
   | **Öffentliche IP-Adresse**| Wählen Sie **Neu erstellen**. |
   | **Name der öffentlichen IP-Adresse**| Geben Sie einen Namen für die IP-Adresse ein, z.B. `secondary-gateway-IP`. |
   | &nbsp; | &nbsp; |

   ![Sekundäre Gatewayeinstellungen](media/sql-database-managed-instance-failover-group-tutorial/settings-for-secondary-gateway.png)

## <a name="6---connect-the-gateways"></a>6\. Verbinden der Gateways
In diesem Schritt erstellen Sie eine Verbindung zwischen Gateways. Es muss eine Verbindung vom primären zum sekundären Gateway hergestellt werden, und dann muss eine separate Verbindung zwischen dem sekundären und dem primären Gateway hergestellt werden. Achten Sie darauf, dass Sie beim Konfigurieren der Konnektivität zwischen beiden Gateways denselben **gemeinsam verwendeten Schlüssel** verwenden. 

Gehen Sie zum Konfigurieren der Konnektivität wie folgt vor:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu ihrer Ressourcengruppe, und wählen Sie das primäre Gateway aus, das Sie in Schritt 4 erstellt haben. 
1. Wählen Sie **Verbindungen** unter **Einstellungen** und dann **Hinzufügen** aus, um eine neue Verbindung zu erstellen. 

   ![Hinzufügen der Verbindung zum primären Gateway](media/sql-database-managed-instance-failover-group-tutorial/add-primary-gateway-connection.png)

1. Geben Sie einen Namen für die Verbindung ein, z.B. `Primary-connection`, und geben Sie einen Wert für den **Gemeinsam verwendeten Schlüssel** ein, z.B. `mi1mi2psk`. 
1. Wählen Sie das **zweite Gateway des virtuellen Netzwerks** aus, und wählen Sie dann das Gateway für die sekundäre verwaltete Instanz aus, z.B. `secondary-mi-gateway`. 

   ![Herstellen einer Verbindung zwischen dem primären und dem sekundären Gateway](media/sql-database-managed-instance-failover-group-tutorial/create-primary-to-secondary-connection.png)

1. Wählen Sie **OK** aus, um die neue Verbindung zwischen dem primären und dem sekundären Gateway hinzuzufügen.
1. Wiederholen Sie diese Schritte, um eine Verbindung zwischen dem Gateway der sekundären verwalteten Instanz und dem Gateway der primären verwalteten Instanz zu erstellen. 

   ![Herstellen einer Verbindung zwischen dem sekundären und dem primären Gateway](media/sql-database-managed-instance-failover-group-tutorial/create-secondary-to-primary-connection.png)


## <a name="7---create-a-failover-group"></a>7\. Erstellen einer Failovergruppe
In diesem Schritt erstellen Sie die Failovergruppe und fügen ihr beide verwalteten Instanzen hinzu. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu **Alle Dienste**, und geben Sie `managed instance` in das Suchfeld ein. 
1. (Optional) Wählen Sie den Stern neben **Verwaltete SQL-Instanzen** aus, um Ihrer linken Navigationsleiste verwaltete Instanzen als Verknüpfung hinzuzufügen. 
1. Wählen Sie **Verwaltete SQL-Instanzen** aus, und wählen Sie Ihre primäre verwaltete Instanz aus, beispielsweise `sql-mi-primary`. 
1. Navigieren Sie unter **Einstellungen** zu **Instanzfailovergruppen**, und wählen Sie dann **Gruppe hinzufügen** aus, um die Seite **Instanzfailovergruppe** zu öffnen. 

   ![Hinzufügen einer Failovergruppe](media/sql-database-managed-instance-failover-group-tutorial/add-failover-group.png)

1. Geben Sie auf der Seite **Instanzfailovergruppe** den Namen Ihrer Failovergruppe ein (z.B. `failovergrouptutorial`), und wählen Sie dann die sekundäre verwaltete Instanz (z.B. `sql-mi-secondary`) aus dem Dropdownmenü aus. Wählen Sie **Erstellen** aus, um die Failovergruppe zu erstellen. 

   ![Erstellen einer Failovergruppe](media/sql-database-managed-instance-failover-group-tutorial/create-failover-group.png)

1. Nachdem die Bereitstellung der Failovergruppe abgeschlossen wurde, gelangen Sie erneut auf die Seite **Failovergruppe**. 

## <a name="8---test-failover"></a>8\. Testen des Failovers
In diesem Schritt führen Sie ein Failover für Ihre Failovergruppe auf dem sekundären Server und anschließend ein Failback mit dem Azure-Portal aus. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer verwalteten Instanz, und wählen Sie unter „Einstellungen“ die Option **Instanzfailovergruppen** aus. 
1. Überprüfen Sie, welche verwaltete Instanz die primäre Instanz ist und welche verwaltete Instanz die sekundäre Instanz ist. 
1. Wählen Sie **Failover** aus, und klicken Sie dann in der Warnung zu TDS-Sitzungen, die getrennt werden, auf **Ja**. 

   ![Ausführen des Failovers der Failovergruppe](media/sql-database-managed-instance-failover-group-tutorial/failover-mi-failover-group.png)

1. Überprüfen Sie, welche verwaltete Instanz die primäre Instanz ist und welche Instanz die sekundäre Instanz ist. Wenn das Failover erfolgreich ausgeführt wurde, sollten die beiden Instanzen die Rollen getauscht haben. 

   ![Verwaltete Instanzen haben nach einem Failover die Rollen getauscht](media/sql-database-managed-instance-failover-group-tutorial/mi-switched-after-failover.png)

1. Wählen Sie erneut **Failover** aus, um ein Failback der primären Instanz in die primäre Rolle durchzuführen. 


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Bereinigen Sie Ressourcen, indem Sie zuerst die verwaltete Instanz, dann den virtuellen Cluster, dann alle verbleibenden Ressourcen und schließlich die Ressourcengruppe löschen. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Ressourcengruppe. 
1. Wählen Sie die verwaltete Instanz und dann **Löschen** aus. Geben Sie `yes` in das Textfeld ein, um zu bestätigen, dass Sie die Ressource löschen möchten, und wählen Sie dann **Löschen** aus. Dieser Vorgang kann im Hintergrund einige Zeit in Anspruch nehmen. Bis der Vorgang abgeschlossen ist, können Sie den *virtuellen Cluster* oder andere abhängige Ressourcen nicht löschen. Überwachen Sie den Löschvorgang auf der Registerkarte „Aktivität“, um zu bestätigen, dass die verwaltete Instanz gelöscht wurde. 
1. Nachdem die verwaltete Instanz gelöscht wurde, löschen Sie den *virtuellen Cluster*, indem Sie ihn in der Ressourcengruppe auswählen und dann **Löschen** auswählen. Geben Sie `yes` in das Textfeld ein, um zu bestätigen, dass Sie die Ressource löschen möchten, und wählen Sie dann **Löschen** aus. 
1. Löschen Sie alle verbleibenden Ressourcen. Geben Sie `yes` in das Textfeld ein, um zu bestätigen, dass Sie die Ressource löschen möchten, und wählen Sie dann **Löschen** aus. 
1. Löschen Sie die Ressourcengruppe, indem Sie die Option **Ressourcengruppe löschen** auswählen, geben Sie den Namen der Ressourcengruppe (`myResourceGroup`) ein, und wählen Sie dann **Löschen** aus. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine Failovergruppe zwischen zwei verwalteten Instanzen konfiguriert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> - Erstellen einer primären verwalteten Instanz
> - Erstellen einer sekundären verwalteten Instanz als Teil einer [Failovergruppe](sql-database-auto-failover-group.md). 
> - Testfailover

Fahren Sie mit dem nächsten Schnellstart fort, um zu erfahren, wie eine Verbindung mit Ihrer verwalteten Instanz hergestellt wird und wie Sie eine Datenbank in Ihrer verwalteten Instanz wiederherstellen können: 

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit der verwalteten Instanz](sql-database-managed-instance-configure-vm.md)
> [Wiederherstellen einer Datenbank in einer verwalteten Instanz](sql-database-managed-instance-get-started-restore.md)


