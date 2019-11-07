---
title: Herstellen einer Verbindung mit einem Azure Cosmos-Konto mit Azure Private Link
description: Erfahren Sie, wie Sie über einen virtuellen Computer sicher auf das Azure Cosmos-Konto zugreifen, indem Sie einen privaten Endpunkt erstellen.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: sngun
ms.openlocfilehash: 80a0a3440a7fb5d056d1d76fb9d82931721b6e16
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510707"
---
# <a name="connect-privately-to-an-azure-cosmos-account-using-azure-private-link"></a>Herstellen einer privaten Verbindung mit einem Azure Cosmos-Konto mithilfe von Azure Private Link

Der private Azure-Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Private Link-Ressourcen kommunizieren.

In diesem Artikel erfahren Sie, wie Sie einen virtuellen Computer in einem virtuellen Azure-Netzwerk und ein Azure Cosmos-Konto mit einem privaten Endpunkt im Azure-Portal erstellen. Anschließend können Sie über den virtuellen Computer sicher auf das Azure Cosmos-Konto zugreifen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich am [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-vm"></a>Erstellen einer VM

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird (in diesem Beispiel ein Azure Cosmos-Konto).

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

### <a name="create-the-virtual-machine"></a>Erstellen des virtuellen Computers

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
    | Kennwort | Geben Sie ein entsprechendes Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
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

## <a name="create-an-azure-cosmos-account"></a>Erstellen eines Azure Cosmos-Kontos

Erstellen Sie ein [Azure Cosmos SQL-API-Konto](../cosmos-db/create-cosmosdb-resources-portal.md#create-an-azure-cosmos-db-account). Der Einfachheit halber können Sie das Azure Cosmos-Konto in derselben Region wie die anderen Ressourcen erstellen (im Beispiel „WestCentralUS“).

## <a name="create-a-private-endpoint-for-your-azure-cosmos-account"></a>Erstellen eines privaten Endpunkts für das Azure Cosmos-Konto

Erstellen Sie einen Private Link für Ihr Azure Cosmos-Konto, wie im Abschnitt [Erstellen eines Private Links mithilfe des Azure-Portals](../cosmos-db/how-to-configure-private-endpoints.md#create-a-private-link-using-the-azure-portal) des verknüpften Artikel beschrieben wird.

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Stellen Sie wie folgt eine Verbindung mit dem virtuellen Computer *myVm* aus dem Internet her:

1. Geben Sie in der Suchleiste des Portals *myVm* ein.

1. Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

1. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die heruntergeladene *RDP*-Datei.

    1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    1. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

        > [!NOTE]
        > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

1. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.  

## <a name="access-the-azure-cosmos-account-privately-from-the-vm"></a>Privates Zugreifen auf das Azure Cosmos-Konto über den virtuellen Computer

In diesem Abschnitt stellen Sie unter Verwendung des privaten Endpunkts eine private Verbindung mit dem Azure Cosmos-Konto her. 

> [!IMPORTANT]
> Die DNS-Konfiguration für das Azure Cosmos-Konto muss manuell geändert werden, damit die hosts-Datei den FQDN des jeweiligen Kontos enthält. In Produktionsszenarien konfigurieren Sie den DNS-Server für die Verwendung der privaten IP-Adressen. Für Demozwecke können Sie jedoch Administratorberechtigungen auf der VM verwenden und die Datei `c:\Windows\System32\Drivers\etc\hosts` (unter Windows) oder `/etc/hosts` (unter Linux) so ändern, dass sie die IP-Adresse und die DNS-Zuordnung enthält.

1. Um die IP-Adresse und die DNS-Zuordnung einzubeziehen, melden Sie sich bei Ihrem virtuellen Computer *myVM* an, öffnen Sie die Datei `c:\Windows\System32\Drivers\etc\hosts`, und fügen Sie die DNS-Informationen aus dem vorherigen Schritt im folgenden Format hinzu:

   [Private IP-Adresse] [Kontoendpunkt].documents.azure.com

   **Beispiel:**

   10.1.255.13 mycosmosaccount.documents.azure.com

   10.1.255.14 mycosmosaccount-eastus.documents.azure.com


1. Installieren Sie im Remotedesktop von  *myVM* [Microsoft Azure Storage-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows).

1. Wählen Sie mit der rechten Maustaste **Cosmos DB-Konten (Vorschau)** aus.

1. Wählen Sie **Verbindung mit Cosmos DB herstellen** aus.

1. Wählen Sie die Option **API** aus.

1. Geben Sie die Verbindungszeichenfolge ein, indem Sie die zuvor kopierten Informationen einfügen.

1. Klicken Sie auf **Weiter**.

1. Wählen Sie **Verbinden**aus.

1. Durchsuchen Sie die Azure Cosmos-Datenbanken und -Container aus *mycosmosaccount*.

1. (Optional) Fügen Sie *mycosmosaccount* neue Elemente hinzu.

1. Schließen Sie die Remotedesktopverbindung mit  *myVM*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie Ihre Arbeit mit dem privaten Endpunkt, dem Azure Cosmos-Konto und dem virtuellen Computer abgeschlossen haben, löschen Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen: 

1. Geben Sie oben im Portal die Zeichenfolge  *myResourceGroup* im Feld **Suchen** ein, und wählen Sie in den Suchergebnissen *myResourceGroup* aus.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie *myResourceGroup* für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie einen virtuellen Computer in einem virtuellen Netzwerk, ein Azure Cosmos-Konto und einen privaten Endpunkt erstellt. Sie haben über das Internet eine Verbindung mit dem virtuellen Computer hergestellt und über Private Link sicher mit dem Azure Cosmos-Konto kommuniziert.

* Weitere Informationen zu privaten Endpunkten finden Sie unter  [Was ist privater Endpunkt in Azure?](private-endpoint-overview.md).

* Weitere Informationen zu Einschränkungen des privaten Endpunkts bei der Verwendung mit Azure Cosmos DB finden Sie im Artikel [Azure Private Link mit Azure Cosmos DB](../cosmos-db/how-to-configure-private-endpoints.md).