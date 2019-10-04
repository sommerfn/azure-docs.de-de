---
title: Verwalten von privaten Endpunkten in Azure
description: Erfahren Sie, wie Sie mit dem Azure-Portal einen privaten Endpunkt erstellen.
services: private-link
author: KumudD
ms.service: private-link
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: 15b4d3208be693a5b8d858d30b663347515f5a68
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71130285"
---
# <a name="create-a-private-endpoint-using-azure-portal"></a>Erstellen eines privaten Endpunkts mit dem Azure-Portal

Ein privater Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Privaten Link-Ressourcen kommunizieren. In diesem Schnellstart erfahren Sie, wie Sie einen virtuellen Computer in einem Azure Virtual Network und einen SQL-Datenbank-Server mit einem privaten Azure-Endpunkt unter Verwendung von Azure PowerShell erstellen. Anschließend können Sie vom virtuellen Computer sicher auf den SQL-Datenbank-Server zugreifen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-vm"></a>Erstellen einer VM
In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird (in diesem Beispiel ein SQL-Server in Azure).

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks


In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird.

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.
1. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | NAME | Geben Sie *MyVirtualNetwork* ein. |
    | Adressraum | Geben Sie *10.1.0.0/16* ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie **Neue erstellen** aus, geben Sie *myResourceGroup* ein, und wählen Sie **OK** aus. |
    | Location | Wählen Sie **WestCentralUS** aus.|
    | Subnetzname | Geben Sie *mySubnet* ein. |
    | Subnetzadressbereich | Geben Sie *10.1.0.0/24* ein. |
    |||
1. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Erstellen** aus.


### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Compute** > **Virtueller Computer** aus.

1. Geben Sie in **Virtuellen Computer erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.  |
    | **INSTANZDETAILS** |  |
    | Name des virtuellen Computers | Geben Sie *myVm* ein. |
    | Region | Wählen Sie **WestCentralUS** aus. |
    | Verfügbarkeitsoptionen | Übernehmen Sie den Standardwert **Keine Infrastrukturredundanz erforderlich**. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter** aus. |
    | Size | Übernehmen Sie den Standardwert **Standard DS1 v2**. |
    | **ADMINISTRATORKONTO** |  |
    | Username | Geben Sie einen Benutzernamen Ihrer Wahl ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **REGELN FÜR EINGEHENDE PORTS** |  |
    | Öffentliche Eingangsports | Übernehmen Sie den Standardwert **Keine**. |
    | **SPAREN SIE GELD** |  |
    | Windows-Lizenz bereits vorhanden? | Übernehmen Sie den Standardwert **Nein**. |
    |||

1. Klicken Sie auf **Weiter: Datenträger**.

1. Übernehmen Sie unter **Virtuellen Computer erstellen – Datenträger** die Standardwerte, und wählen Sie **Weiter: Netzwerk**.

1. Wählen Sie in **Virtuellen Computer erstellen – Netzwerk** diese Informationen aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Virtuelles Netzwerk | Übernehmen Sie den Standardwert **MyVirtualNetwork**.  |
    | Adressraum | Übernehmen Sie den Standardwert **10.1.0.0/24**.|
    | Subnet | Übernehmen Sie den Standardwert **mySubnet (10.1.0.0/24)** .|
    | Öffentliche IP-Adresse | Übernehmen Sie den Standardwert **(neu) myVm-ip**. |
    | Öffentliche Eingangsports | Wählen Sie **Ausgewählte Ports zulassen** aus. |
    | Eingangsports auswählen | Wählen Sie **HTTP** und **RDP** aus.|
    |||


1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

## <a name="create-a-sql-database-server"></a>Erstellen eines SQL-Datenbank-Servers
In diesem Abschnitt erstellen Sie einen SQL-Datenbank-Server in Azure. 

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Datenbanken** > **SQL-Datenbank** aus.

1. Geben Sie unter **SQL-Datenbank erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Datenbankdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    | **INSTANZDETAILS** |  |
    | Datenbankname  | Geben Sie *mydatabase* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen. |
    |||
5. Wählen Sie unter  **Server** die Option **Neu erstellen** aus. 
6. Geben Sie bei  **Neuer Server** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |Servername  | Geben Sie *myserver* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen.|
    | Serveradministratoranmeldung| Geben Sie einen Administratornamen Ihrer Wahl ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens acht Zeichen lang sein und die festgelegten Anforderungen erfüllen. |
    | Location | Wählen Sie eine Azure-Region aus, in der sich Ihr SQL Server befinden soll. |
    
7. Wählen Sie  **OK** aus. 
8. Wählen Sie  **Review + create** (Überprüfen und erstellen) aus. Sie werden zur Seite  **Review + create** (Überprüfen und erstellen) weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
9. Wenn die Meldung „Überprüfung erfolgreich“ angezeigt wird, wählen Sie  **Erstellen** aus. 
10. Wenn die Meldung „Überprüfung erfolgreich“ angezeigt wird, wählen Sie „Erstellen“ aus. 

## <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts

In diesem Abschnitt erstellen Sie einen SQL-Server und fügen ihm einen privaten Endpunkt hinzu. 

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Netzwerk** > **Private Link-Center (Vorschau)** aus.
2. Wählen Sie unter  **Privat Link-Center – Übersicht** bei der Option  **Build a private connection to a service** (Private Verbindung mit einem Dienst herstellen)  **Start** aus.
1. Geben Sie unter **Privaten Endpunkt erstellen (Vorschau) – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    | **INSTANZDETAILS** |  |
    | NAME | Geben Sie * myPrivateEndpoint* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen. |
    |Region|Wählen Sie **WestCentralUS** aus.|
    |||
5. Wählen Sie **Weiter: Ressource** aus.
6. Geben Sie unter  **Privaten Endpunkt erstellen – Ressource** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |Verbindungsmethode  | Wählen Sie das Herstellen einer Verbindung mit einer Azure-Ressource im eigenen Verzeichnis aus.|
    | Subscription| Wählen Sie Ihr Abonnement aus. |
    | Ressourcentyp | Wählen Sie  **Microsoft.Sql/servers** aus. |
    | Resource |Wählen Sie *myServer* aus.|
    |Zielunterressource |Wählen Sie *sqlServer* aus.|
    |||
7. Wählen Sie  **Weiter: Konfiguration** aus.
8. Geben Sie unter **Privaten Endpunkt erstellen (Vorschau) – Konfiguration** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |**NETZWERK**| |
    | Virtuelles Netzwerk| Wählen Sie *MyVirtualNetwork* aus. |
    | Subnet | Wählen Sie  *mySubnet* aus. |
    |**PRIVATE DNS-INTEGRATION**||
    |Integration in eine private DNS-Zone |Wählen Sie  **Ja** aus. |
    |Private DNS-Zone |Wählen Sie *(New)privatelink.database.windows.net* aus. |
    |||

1. Wählen Sie  **Review + create** (Überprüfen und erstellen) aus. Sie werden zur Seite  **Review + create** (Überprüfen und erstellen) weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
2. Wenn die Meldung  **Überprüfung erfolgreich**  angezeigt wird, wählen Sie  **Erstellen** aus. 
 
## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Herstellen einer Verbindung mit einem virtuellen Computer mithilfe von Remotedesktop (RDP)


Nachdem Sie **myVm* erstellt haben, stellen Sie über das Internet eine Verbindung mit diesem virtuellen Computer her. 

1. Geben Sie in der Suchleiste des Portals *myVm* ein.

1. Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

1. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die heruntergeladene RDP*-Datei.

    1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    1. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

        > [!NOTE]
        > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

1. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.  

## <a name="access-the-sql-database-server-privately-from-the-vm"></a>Privates Zugreifen auf den SQL-Datenbank-Server vom virtuellen Computer

1. Öffnen Sie PowerShell auf dem Remotedesktop von  *myVm*.

2. Geben Sie  `nslookup myserver.database.windows.net` ein. 

    Sie erhalten eine Meldung wie die folgende:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
3. Install [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

4. In **Connect to server**, enter or select this information:

    | Setting | Value |
    | ------- | ----- |
    | Server type| Select **Database Engine**.|
    | Server name| Select *myserver.database.windows.net* |
    | User name | Enter a password provided during the SQL server creation. |
    |Password |Enter a password provided during the SQL server creation. |
    |Remember password|Select **Yes**.|
    |||
1. Select **Connect**.
2. Browse databases from left menu.
3. (Optionally) Create or query information from mydatabase.
4. Close the remote desktop connection to *myVm*. 

## Clean up resources 
When you're done using the private endpoint, SQL server, and the VM, delete the resource group and all of the resources it contains: 
1. Enter *myResourceGroup* in the **Search** box at the top of the portal and select *myResourceGroup* from the search results. 
2. Select **Delete resource group**. 
3. Enter myResourceGroup for **TYPE THE RESOURCE GROUP NAME** and select **Delete**.

## Next steps

In this quickstart, you created a VM on a virtual network, a SQL database server, and a private endpoint for private access. You connected to one VM from the internet and securely communicated to the SQL database server using Private Link. To learn more about private endpoints, see [What is Azure private endpoint?](private-endpoint-overview.md).

