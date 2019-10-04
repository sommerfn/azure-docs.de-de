---
title: Herstellen einer privaten Verbindung mit einem Speicherkonto mithilfe eines privaten Azure-Endpunkts
description: Erfahren Sie, wie Sie eine private Verbindung mit einem Speicherkonto in Azure mithilfe eines privaten Endpunkts herstellen.
services: private-link
author: KumudD
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: bb0c6e9d20c12df3532a52df1fe4d9574344d4b3
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71104717"
---
# <a name="connect-privately-to-a-storage-account-using-azure-private-endpoint"></a>Herstellen einer privaten Verbindung mit einem Speicherkonto mithilfe eines privaten Azure-Endpunkts
Der private Azure-Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Private Link-Ressourcen kommunizieren.

In diesem Schnellstart erfahren Sie, wie Sie einen virtuellen Computer in einem virtuellen Azure-Netzwerk und ein Speicherkonto mit einem privaten Endpunkt im Azure-Portal erstellen. Anschließend können Sie vom virtuellen Computer sicher auf das Speicherkonto zugreifen.


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-vm"></a>Erstellen einer VM
In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird (in diesem Beispiel ein Speicherkonto).

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
    ||

1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

## <a name="create-your-private-endpoint"></a>Erstellen Ihres privaten Endpunkts
In diesem Abschnitt erstellen Sie ein privates Speicherkonto und verwenden für das Speicherkonto einen privaten Endpunkt. 

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Speicher** > **Speicherkonto** aus.

1. Geben Sie in **Speicherkonto erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    | **INSTANZDETAILS** |  |
    | Speicherkontoname  | Geben Sie *mystorageaccount* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen. |
    | Region | Wählen Sie **WestCentralUS** aus. |
    | Leistung| Übernehmen Sie den Standardwert **Standard**. |
    | Kontoart | Übernehmen Sie den Standardwert **Storage (universell, Version 2)** . |
    | Replikation | Wählen Sie **Georedundanter Speicher mit Lesezugriff (RA-GRS)** aus. |
    |||
  
3. Wählen Sie **Weiter: Netzwerk** aus.
4. Wählen Sie in  **Speicherkonto erstellen – Netzwerke**, „Verbindungsmethode“, **Privater Endpunkt** aus.
5. Wählen Sie in  **Speicherkonto erstellen – Netzwerke** die Option **Privaten Endpunkt hinzufügen** aus. 
6. Geben Sie unter  **Privaten Endpunkt erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    |Location|Wählen Sie **WestCentralUS** aus.|
    |NAME|Geben Sie *myPrivateEndpoint* ein.  |
    |Speicherunterressource|Übernehmen Sie den Standardwert **Blob**. |
    | **NETZWERK** |  |
    | Virtuelles Netzwerk  | Wählen Sie *MyVirtualNetwork* in der Ressourcengruppe *myResourceGroup* aus. |
    | Subnet | Wählen Sie *mySubnet* aus. |
    | **PRIVATE DNS-INTEGRATION**|  |
    | Integration in eine private DNS-Zone  | Übernehmen Sie den Standardwert **Ja**. |
    | Private DNS-Zone  | Übernehmen Sie den Standardwert ** (Neu) privatelink.blob.core.windows.net**. |
    |||
7. Wählen Sie **OK** aus. 
8. Wählen Sie **Überprüfen und erstellen** aus. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
9. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus. 
10. Navigieren Sie zur gerade erstellten Speicherkontoressource.
11. Wählen Sie im Inhaltsmenü auf der linken Seite **Zugriffsschlüssel** aus.
12. Wählen Sie **Kopieren** für die Verbindungszeichenfolge für „key1“ aus.
 
## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Stellen Sie wie folgt eine Verbindung mit dem virtuellen Computer *myVm* aus dem Internet her:

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

## <a name="access-storage-account-privately-from-the-vm"></a>Privates Zugreifen auf das Speicherkonto von der VM

In diesem Abschnitt stellen Sie unter Verwendung des privaten Endpunkts eine private Verbindung mit dem Speicherkonto her.

> [!IMPORTANT]
> Die DNS-Konfiguration für den Speicher erfordert eine manuelle Änderung der Hostsdatei, um den FQDN des jeweiligen Kontos einzuschließen. Ändern Sie die folgende Datei unter Verwendung von Administratorberechtigungen unter Windows: „c:\Windows\System32\Drivers\etc\hosts“ oder „Linux/etc/hosts“. Fügen Sie die DNS-Informationen für das Konto aus dem vorherigen Schritt im folgenden Format ein: „[Private IP-Adresse] myaccount.blob.core.windows.net“.

1. Öffnen Sie PowerShell auf dem Remotedesktop von  *myVm*.
2. Geben Sie `nslookup mystorageaccount.blob.core.windows.net` ein. Sie erhalten eine Meldung wie die folgende:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mystorageaccount123123.privatelink.blob.core.windows.net
    Address:  10.0.0.5
    Aliases:  mystorageaccount.blob.core.windows.net
3. Install [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows).
4. Select **Storage accounts** with the right-click.
5. Select **Connect to an azure storage**.
6. Select **Use a connection string**.
7. Select **Next**.
8. Enter the connection string by pasting the information previously copied.
9. Select **Next**.
10. Select **Connect**.
11. Browse the Blob containers from mystorageaccount 
12. (Optionally) Create folders and/or upload files to *mystorageaccount*. 
13. Close the remote desktop connection to *myVM*. 

Additional options to access the storage account:
- Microsoft Azure Storage Explorer is a standalone free app from Microsoft that enables you to work visually with Azure storage data on Windows, macOS, and Linux. You can install the application to browse privately the storage account content. 
 
- The AzCopy utility is another option for high-performance scriptable data transfer for Azure storage. Use AzCopy to transfer data to and from Blob, File, and Table storage. 


## Clean up resources 
When you're done using the Private Endpoint, storage account and the VM, delete the resource group and all of the resources it contains: 
1. Enter *myResourceGroup* in the **Search** box at the top of the portal and select *myResourceGroup* from the search results. 
2. Select **Delete resource group**. 
3. Enter *myResourceGroup* for **TYPE THE RESOURCE GROUP NAME** and select **Delete**. 

## Next steps
In this Quickstart, you created a VM on a virtual network and storage account and a Private Endpoint. You connected to one VM from the internet and securely communicated to the storage account using Private Link. To learn more about Private Endpoint, see [What is Azure Private Endpoint?](private-endpoint-overview.md).
