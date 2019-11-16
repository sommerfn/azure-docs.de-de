---
title: Herunterladen einer Linux-VHD von Azure
description: Laden Sie eine Linux-VHD mithilfe der Azure-Befehlszeilenschnittstelle und dem Azure-Portal herunter.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 08/21/2019
ms.author: cynthn
ms.openlocfilehash: 257f3f723fc8a971b8253699f4beb002cf46ce52
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74036289"
---
# <a name="download-a-linux-vhd-from-azure"></a>Herunterladen einer Linux-VHD von Azure

In diesem Artikel erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle und des Azure-Portals eine Linux-VHD-Datei (Virtual Hard Disk, virtuelle Festplatte) von Azure herunterladen. 

Installieren Sie [Azure CLI](https://docs.microsoft.com/cli/azure/install-az-cli2), falls Sie dies noch nicht getan haben.

## <a name="stop-the-vm"></a>Beenden des virtuellen Computers

Eine VHD kann nicht von Azure heruntergeladen werden, wenn sie an eine ausgeführte VM angefügt ist. Sie müssen die VM beenden, um eine VHD herunterzuladen. Wenn Sie eine VHD als [Image](tutorial-custom-images.md) zum Erstellen anderer VMs mit neuen Datenträgern verwenden möchten, müssen Sie das in der Datei enthaltene Betriebssystem aufheben und generalisieren und dann die VM anhalten. Um die VHD als Datenträger für eine neue Instanz einer vorhandenen VM oder eines vorhandenen Datenträgers zu verwenden, müssen Sie nur die VM beenden und freigeben.

Um die VHD als Image zum Erstellen von anderen VMs zu verwenden, führen Sie die folgenden Schritte durch:

1. Verwenden Sie SSH, den Kontonamen und die öffentliche IP-Adresse der VM, um eine Verbindung zu ihr aufzubauen und die Bereitstellung aufzuheben. Die öffentliche IP-Adresse finden Sie mit [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip#az-network-public-ip-show). Der Parameter „+Benutzer“ entfernt auch das zuletzt bereitgestellte Benutzerkonto. Wenn Sie Kontoanmeldeinformationen in der VM sichern, lassen die den +Benutzer-Parameter weg. Im folgenden Beispiel wird das letzte bereitgestellte Benutzerkonto entfernt:

    ```bash
    ssh azureuser@<publicIpAddress>
    sudo waagent -deprovision+user -force
    exit 
    ```

2. Melden Sie sich mit [az login](https://docs.microsoft.com/cli/azure/reference-index) bei Ihrem Azure-Konto an.
3. Beenden und Aufheben der Zuordnung für die VM

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. Generalisieren der VM 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

Um die VHD-Datei als Datenträger für eine neue Instanz eines vorhandenen virtuellen Computers oder Datenträgers für Daten zu verwenden, führen Sie folgende Schritte aus:

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2.  Wählen Sie im linken Menü **Virtual Machines** aus.
3.  Wählen Sie die VM aus der Liste aus.
4.  Wählen Sie auf der Seite für den virtuellen Computer die Option **Beenden** aus.

    ![Beenden der VM](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a>Generieren der SAS-URL

Um die VHD-Datei herunterzuladen, müssen Sie eine [SAS-URL (Shared Access Signature)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) generieren. Wenn die URL generiert wird, wird der URL eine Ablaufzeit zugewiesen.

1.  Wählen Sie im Menü auf der Seite des virtuellen Computers die Option **Datenträger** aus.
2.  Wählen Sie den Betriebssystem-Datenträger für den virtuellen Computer und anschließend **Datenträgerexport** aus.
3.  Wählen Sie **URL generieren** aus.

    ![Generieren der URL](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a>Herunterladen der VHD

1.  Wählen Sie unter der generierten URL die Option **VHD-Datei herunterladen** aus.
**
    ![Herunterladen der VHD](./media/download-vhd/export-download.png)

2.  Gegebenenfalls muss im Browser die Option **Speichern** ausgewählt werden, um den Download zu starten. Der Standardname für die VHD-Datei lautet *abcd*.

    ![Auswählen von „Speichern“ im Browser](./media/download-vhd/export-save.png)

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum [Hochladen und Erstellen eines virtuellen Linux-Computers aus einem benutzerdefinierten Datenträger mithilfe von Azure CLI](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 
- [Verwalten von Azure-Datenträgern mit der Azure-CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

