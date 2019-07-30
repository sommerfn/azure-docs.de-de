---
title: Zugreifen auf ein Lab in Azure DevTest Labs | Microsoft-Dokumentation
description: In diesem Tutorial greifen Sie auf das Lab zu, das unter Verwendung von Azure DevTest Labs erstellt wird, beanspruchen virtuelle Computer, verwenden diese, und heben den Anspruch wieder auf.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/18/2019
ms.author: spelluru
ms.openlocfilehash: ee9a68df685095244fc9471b7d4ab0f6cee0642d
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68360261"
---
# <a name="tutorial-access-a-lab-in-azure-devtest-labs"></a>Tutorial: Zugreifen auf ein Lab in Azure DevTest Labs
In diesem Tutorial verwenden Sie das Lab, das in [Tutorial: Erstellen eines Labs mithilfe von Azure DevTest Labs](tutorial-create-custom-lab.md) erstellt wurde.

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Beanspruchen eines virtuellen Computers (virtual machine, VM) im Lab
> * Herstellen der Verbindung zur VM
> * Aufheben des Anspruchs auf den virtuellen Computer

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="access-the-lab"></a>Zugreifen auf das Lab

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Menü die Option **Alle Ressourcen**. 
3. Wählen Sie **DevTest Labs** als Ressourcentyp aus. 
4. Wählen Sie das Lab aus. 

    ![Auswählen des Labs](./media/tutorial-use-custom-lab/search-for-select-custom-lab.png)

## <a name="claim-a-vm"></a>Beanspruchen eines virtuellen Computers

1. Klicken Sie in der Liste **Abrufbare virtuelle Computer** auf die Schaltfläche mit den drei Auslassungspunkten ( **...** ), und wählen Sie **Computer beanspruchen** aus.

    ![Beanspruchen eines virtuellen Computers](./media/tutorial-use-custom-lab/claim-virtual-machine.png)
1. Vergewissern Sie sich, dass der virtuelle Computer in der Liste **Meine virtuellen Computer** angezeigt wird.

    ![Mein virtueller Computer](./media/tutorial-use-custom-lab/my-virtual-machines.png)

## <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM

1. Wählen Sie Ihren virtuellen Computer aus der Liste aus. Die Seite **Virtueller Computer** wird angezeigt. Klicken Sie in der Symbolleiste auf **Verbinden**.

    ![Herstellen der Verbindung mit dem virtuellen Computer](./media/tutorial-use-custom-lab/connect-button.png)
2. Speichern Sie die heruntergeladene **RDP**-Datei auf Ihrer Festplatte, und verwenden Sie sie, um eine Verbindung mit dem virtuellen Computer herzustellen. Geben Sie den Benutzernamen und das Kennwort an, den bzw. das Sie beim Erstellen des virtuellen Computers im vorigen Abschnitt angegeben haben. 

    Für die VM muss SSH- bzw. RDP-Zugriff aktiviert sein, um eine Verbindung mit einer Linux-VM herzustellen. Schritte zum Herstellen einer Verbindung mit einer Linux-VM per RDP finden Sie unter [Installieren und Konfigurieren von Remotedesktop zum Herstellen einer Verbindung mit einem virtuellen Linux-Computer in Azure](../virtual-machines/linux/use-remote-desktop.md). 

    > [!NOTE]
    > Es gibt andere Möglichkeiten, zur Seite „Virtueller Computer“ für Ihren virtuellen Computer zu gelangen. Hier einige Beispiele: 
    > 
    > 1. Suchen Sie nach allen VMs in Ihrem Abonnement. Wählen Sie Ihre VM in der Liste der virtuellen Computer aus, um zur Seite **Virtueller Computer** zu gelangen.
    > 2. Navigieren Sie zur Seite **Ressourcengruppe** für die Ressourcengruppe. Wählen Sie dann Ihre VM aus der Liste der Ressourcen in der Ressourcengruppe aus, um zur Seite **Virtueller Computer** zu gelangen. 
    >
    > Verwenden Sie nicht die Schaltfläche **Verbinden** auf der Symbolleiste der Seite **Virtueller Computer**, zu der Sie mithilfe dieser Optionen gelangen. Navigieren Sie stattdessen von der Seite **DevTest Labs**, wie in diesem Artikel gezeigt, zur Seite **Virtueller Computer**, und verwenden Sie dann die Schaltfläche **Verbinden** auf der Symbolleiste.


## <a name="unclaim-the-vm"></a>Aufheben des Anspruchs auf den virtuellen Computer
Wenn Sie den virtuellen Computer nicht benötigen, führen Sie folgende Schritte aus, um den Anspruch auf den virtuellen Computer aufzuheben: 

1. Wählen Sie auf der Seite des virtuellen Computers auf der Symbolleiste die Option **Anspruch aufheben** aus. 

    ![Aufheben des Anspruchs auf den virtuellen Computer](./media/tutorial-use-custom-lab/unclaim-vm-menu.png)
1. Der virtuelle Computer wird heruntergefahren, bevor der Anspruch aufgehoben wird. Der Status dieses Vorgangs wird in den Benachrichtigungen angezeigt.  
3. Navigieren Sie wieder zur DevTest Lab-Seite, indem Sie im Breadcrumb-Menü im oberen Bereich auf den Namen Ihres Labs klicken. 
    
    ![Navigieren zum Lab](./media/tutorial-use-custom-lab/breadcrumb-to-lab.png)
1. Vergewissern Sie sich im unteren Bereich, dass der virtuelle Computer in der Liste **Abrufbare virtuelle Computer** enthalten ist.

    
## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurde gezeigt, wie Sie auf ein Lab zugreifen, das unter Verwendung von Azure DevTest Labs erstellt wurde, und wie Sie dieses Lab verwenden. Weitere Informationen zum Zugriff auf virtuelle Computer in einem Lab sowie zu deren Verwendung finden Sie hier: 

> [!div class="nextstepaction"]
> [Gewusst wie: eines virtuellen Computers zu einem Lab in Azure DevTest Labs](devtest-lab-add-vm.md)

