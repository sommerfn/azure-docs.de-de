---
title: Erstellen eines Azure Databricks-Arbeitsbereichs in einem virtuellen Netzwerk
description: Dieser Artikel beschreibt, wie Sie Azure Databricks in Ihrem virtuellen Netzwerk bereitstellen.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: 69afe2aab3c10707f7160d727b970ad73d59a952
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72791555"
---
# <a name="quickstart-create-an-azure-databricks-workspace-in-a-virtual-network"></a>Schnellstart: Erstellen eines Azure Databricks-Arbeitsbereichs in einem virtuellen Netzwerk

In dieser Schnellstartanleitung erfahren Sie, wie Sie einen Azure Databricks-Arbeitsbereich in einem virtuellen Netzwerk erstellen. Außerdem erstellen Sie in diesem Arbeitsbereich einen Apache Spark-Cluster.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

> [!Note]
> Dieses Tutorial kann nicht mit dem **kostenlosen Azure-Testabonnement** absolviert werden.
> Wenn Sie ein kostenloses Konto haben, rufen Sie Ihr Profil auf, und ändern Sie Ihr Abonnement auf **Nutzungsbasierte Bezahlung**. Weitere Informationen finden Sie unter [Kostenloses Azure-Konto](https://azure.microsoft.com/free/). [Entfernen Sie das dann Ausgabenlimit](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit), und [fordern Sie die Erhöhung des Kontingents](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) für vCPUs in Ihrer Region an. Wenn Sie Ihren Azure Databricks-Arbeitsbereich erstellen, können Sie den Tarif **Testversion (Premium – 14 Tage kostenlosen DBUs)** auswählen, damit Sie über den Arbeitsbereich 14 Tage lang auf kostenlose Premium Azure Databricks-DBUs zugreifen können.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Wählen Sie im Azure-Portal **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.

2. Legen Sie unter **Virtuelles Netzwerk erstellen** die folgenden Einstellungen fest: 

    |Einstellung|Empfohlener Wert|BESCHREIBUNG|
    |-------|---------------|-----------|
    |NAME|databricks-quickstart|Wählen Sie einen Namen für Ihr virtuelles Netzwerk.|
    |Adressraum|10.1.0.0/16|Der Adressbereich des virtuellen Netzwerks in CIDR-Notation.|
    |Subscription|\<Ihr Abonnement\>|Wählen Sie das gewünschte Azure-Abonnement aus.|
    |Resource group|databricks-quickstart|Klicken Sie auf **Neu erstellen**, und geben Sie einen neuen Ressourcengruppennamen für Ihr Konto ein.|
    |Location|\<Die Region, die Ihren Benutzern am nächsten liegt\>|Wählen Sie einen geografischen Standort aus, an dem Sie Ihr virtuelles Netzwerk hosten können. Verwenden Sie den Standort, der Ihren Benutzern am nächsten ist.|
    |Subnetzname|default|Wählen Sie für das Standardsubnetz in Ihrem virtuellen Netzwerk einen Namen aus.|
    |Subnetzadressbereich|10.1.0.0/24|Der Adressbereich des Subnetzes in CIDR-Notation. Er muss innerhalb des Adressraums des virtuellen Netzwerks liegen. Der Adressbereich eines aktiven Subnetzes kann nicht bearbeitet werden.|

    ![Erstellen eines virtuellen Netzwerks im Azure-Portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-virtual-network.png)

3. Sobald die Bereitstellung abgeschlossen ist, navigieren Sie zu Ihrem virtuellen Netzwerk und klicken unter **Einstellungen** auf **Adressraum**. Fügen Sie `10.179.0.0/16` im Feld *Zusätzlichen Adressbereich hinzufügen* ein, und klicken Sie auf **Speichern**.

    ![Adressraum des virtuellen Azure-Netzwerks](./media/quickstart-create-databricks-workspace-vnet-injection/add-address-space.png)

## <a name="create-an-azure-databricks-workspace"></a>Erstellen eines Azure Databricks-Arbeitsbereichs

1. Klicken Sie im Azure-Portal auf **Ressource erstellen** > **Analytics** > **Databricks**.

2. Legen Sie für den **Azure Databricks-Dienst** folgende Werte fest:

    |Einstellung|Empfohlener Wert|BESCHREIBUNG|
    |-------|---------------|-----------|
    |Arbeitsbereichname|databricks-quickstart|Wählen Sie einen Namen für Ihren Azure Databricks-Arbeitsbereich aus.|
    |Subscription|\<Ihr Abonnement\>|Wählen Sie das gewünschte Azure-Abonnement aus.|
    |Resource group|databricks-quickstart|Wählen Sie die gleiche Ressourcengruppe wie für das virtuelle Netzwerk.|
    |Location|\<Die Region, die Ihren Benutzern am nächsten liegt\>|Verwenden Sie denselben Standort wie für das virtuelle Netzwerk.|
    |Preisstufe|Wählen Sie entweder „Standard“ oder „Premium“.|Weitere Informationen zu diesen Tarifen, finden Sie auf der Seite [Databricks – Preise](https://azure.microsoft.com/pricing/details/databricks/).|
    |Deploy Azure Databricks workspace in your Virtual Network (Azure Databricks-Arbeitsbereich in Ihrem virtuellen Netzwerk bereitstellen)|Ja|Mit dieser Einstellung können Sie einen Azure Databricks-Arbeitsbereich in Ihrem virtuellen Netzwerk bereitstellen.|
    |Virtual Network|databricks-quickstart|Wählen Sie das im vorherigen Abschnitt erstellte virtuelle Netzwerk aus.|
    |Public Subnet Name (Name des öffentlichen Subnetzes)|public-subnet|Übernehmen Sie den Standardnamen des öffentlichen Subnetzes.|
    |Public Subnet CIDR Range (CIDR-Bereich des öffentlichen Subnetzes)|10.179.64.0/18|Der CIDR-Bereich für dieses Subnetz muss im Bereich/18 bis /26 liegen.|
    |Private Subnet Name (Name des privaten Subnetzes)|private-subnet|Übernehmen Sie den Standardnamen des privaten Subnetzes.|
    |Private Subnet CIDR Range (CIDR-Bereich des privaten Subnetzes)|10.179.0.0/18|Der CIDR-Bereich für dieses Subnetz muss im Bereich/18 bis /26 liegen.|

    ![Erstellen eines Azure Databricks-Arbeitsbereichs im Azure-Portal](./media/quickstart-create-databricks-workspace-vnet-injection/create-databricks-workspace.png)

3. Sobald die Bereitstellung abgeschlossen ist, navigieren Sie zur Ressource „Azure Databricks“. Beachten Sie, dass Peering in virtuellen Netzwerken deaktiviert ist. Beachten Sie auch die Ressourcengruppe und die verwaltete Ressourcengruppe auf der Seite „Übersicht“. 

    ![Übersicht über Azure Databricks im Azure-Portal](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-overview-portal.png)

    Die verwaltete Ressourcengruppe enthält den physischen Speicherort des Speicherkontos (DBFS), die Netzwerksicherheitsgruppe (worker-sg) und das virtuelle Netzwerk (workers-vnet). Dies ist auch der Speicherort, in dem virtuelle Computer, Datenträger, IP-Adressen und Netzwerkschnittstellen erstellt werden. Diese Ressourcengruppe ist standardmäßig gesperrt. Wenn jedoch im virtuellen Netzwerk ein Cluster gestartet wird, wird eine Netzwerkschnittstelle zwischen „workers-vnet“ in der verwalteten Ressourcengruppe und dem als „Hub“ fungierenden virtuellen Netzwerk erstellt.

    ![Verwaltete Ressourcengruppe in Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group.png)

## <a name="create-a-cluster"></a>Erstellen eines Clusters

> [!NOTE]
> Navigieren Sie vor dem Erstellen des Clusters zu Ihrem Profil, und legen Sie für Ihr Abonnement die **nutzungsbasierte Bezahlung** fest, um für die Erstellung des Azure Databricks-Clusters ein kostenloses Konto zu verwenden. Weitere Informationen finden Sie unter [Kostenloses Azure-Konto](https://azure.microsoft.com/free/).

1. Kehren Sie zu Ihrer Instanz des Azure Databricks-Diensts zurück, und klicken Sie auf der Seite **Übersicht** auf **Arbeitsbereich starten**.

2. Klicken Sie auf **Cluster** >  **+ Cluster erstellen**. Geben Sie einen Clusternamen wie *databricks-quickstart-cluster* ein, und übernehmen Sie die restlichen Standardeinstellungen. Wählen Sie **Cluster erstellen** aus.

    ![Erstellen eines Azure Databricks-Clusters](./media/quickstart-create-databricks-workspace-vnet-injection/create-cluster.png)

3. Sobald der Cluster ausgeführt wird, kehren Sie im Azure-Portal zur verwalteten Ressourcengruppe zurück. Beachten Sie die neuen virtuellen Computer, Datenträger, IP-Adressen und Netzwerkschnittstellen. In jedem der öffentlichen und privaten Subnetze wird mit IP-Adressen eine Netzwerkschnittstelle eingerichtet.  

    ![Verwaltete Ressourcengruppe in Azure Databricks nach Erstellen des Clusters](./media/quickstart-create-databricks-workspace-vnet-injection/managed-resource-group2.png)

4. Kehren Sie zu Ihrem Azure Databricks-Arbeitsbereich zurück, und wählen Sie den von Ihnen erstellten Cluster aus. Navigieren Sie dann auf der Seite mit der **Spark-Benutzeroberfläche** zur Registerkarte **Executors**. Beachten Sie, dass sich die Adressen für den Treiber und die Executors im Bereich des privaten Subnetzes befinden. In diesem Beispiel hat der Treiber die Adresse 10.179.0.6, während die Executors die Adressen 10.179.0.4 und 10.179.0.5 haben. Ihre IP-Adressen können abweichen.

    ![Executors auf der Spark-Benutzeroberfläche in Azure Databricks](./media/quickstart-create-databricks-workspace-vnet-injection/databricks-sparkui-executors.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Am Ende dieses Artikels können Sie den Cluster beenden. Klicken Sie hierzu im linken Bereich des Azure Databricks-Arbeitsbereichs auf **Cluster**. Bewegen Sie den Cursor zum Beenden des Clusters auf die Auslassungspunkte in der Spalte **Aktionen**, und klicken Sie auf das Symbol **Beenden**. Dies beendet den Cluster.

Wenn Sie den Cluster nicht manuell beenden, wird er automatisch beendet, sofern Sie bei der Erstellung des Clusters das Kontrollkästchen **Terminate after \_\_ minutes of inactivity** (Nach \_\_ Minuten Inaktivität beenden) aktiviert haben. Der Cluster wird dann automatisch beendet, wenn er für den angegebenen Zeitraum inaktiv war.

Wenn Sie den Cluster nicht wiederverwenden möchten, können Sie die Ressourcengruppe löschen, die Sie im Azure-Portal erstellt haben.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie in Azure Databricks einen Spark-Cluster erstellt, den Sie in einem virtuellen Netzwerk bereitgestellt haben. Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie einen Linux Docker-Container für SQL Server im virtuellen Netzwerk mithilfe von JDBC aus einem Azure Databricks-Notebook abfragen können.  

> [!div class="nextstepaction"]
>[Abfragen eines Linux-Docker-Containers für SQL Server in einem virtuellen Netzwerk aus einem Azure Databricks-Notebook](vnet-injection-sql-server.md)
