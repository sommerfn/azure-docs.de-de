---
title: Tutorial – Erstellen benutzerdefinierter VM-Images mit Azure PowerShell | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie Azure PowerShell zum Erstellen eines benutzerdefinierten VM-Images in Azure verwenden.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 58f7e530008303f16f2d015e29e60e95480cab2c
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390573"
---
# <a name="tutorial-create-a-custom-image-of-an-azure-vm-with-azure-powershell"></a>Tutorial: Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mit Azure PowerShell

Benutzerdefinierte Images sind wie Marketplace-Images, Sie erstellen sie jedoch selbst. Benutzerdefinierte Images können für das Bootstrapping von Bereitstellungen verwendet werden und sorgen für VM-übergreifende Konsistenz. In diesem Tutorial erstellen Sie mithilfe von PowerShell ein eigenes benutzerdefiniertes Image eines virtuellen Azure-Computers. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Sysprep und Generalisieren virtueller Computer
> * Erstellen eines benutzerdefinierten Images
> * Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image
> * Liste aller Images in Ihrem Abonnement
> * Löschen eines Images

In der öffentlichen Vorschauversion ist der Dienst [Azure VM Image Builder](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder-overview) verfügbar. Beschreiben Sie einfach Ihre Anpassungen in einer Vorlage, und diese übernimmt die Schritte zur Imageerstellung in diesem Artikel. Probieren Sie [Azure Image Builder (Vorschau)](https://docs.microsoft.com/azure/virtual-machines/windows/image-builder) aus.

## <a name="before-you-begin"></a>Voraussetzungen

In den folgenden Schritten wird erläutert, wie Sie eine vorhandene VM in ein wiederverwendbares benutzerdefiniertes Image umwandeln, mit dem Sie neue VM-Instanzen erstellen können.

Für das Beispiel in diesem Tutorial muss ein virtueller Computer vorhanden sein. Gegebenenfalls können Sie mit diesem [Beispielskript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) einen virtuellen Computer erstellen. Ersetzen Sie beim Durcharbeiten des Tutorials bei Bedarf den Namen der Ressourcengruppe und des virtuellen Computers.

## <a name="launch-azure-cloud-shell"></a>Starten von Azure Cloud Shell

Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. 

Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock einfach die Option **Ausprobieren**. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/powershell](https://shell.azure.com/powershell) navigieren. Wählen Sie **Kopieren**, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen.

## <a name="prepare-vm"></a>Vorbereiten der VM

Wenn Sie ein Image eines virtuellen Computers erstellen möchten, müssen Sie den virtuellen Quellcomputer vorbereiten, indem Sie ihn generalisieren, die Zuordnung aufheben und ihn dann mit Azure als generalisiert markieren.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Generalisieren der Windows-VM mithilfe von Sysprep

Sysprep entfernt unter anderem alle persönlichen Kontoinformationen, und bereitet den Computer darauf vor, als Image verwendet zu werden. Weitere Informationen zu Sysprep finden Sie unter [How to Use Sysprep: An Introduction](https://technet.microsoft.com/library/bb457073.aspx) (Verwenden von Sysprep: Eine Einführung).


1. Stellen Sie eine Verbindung mit dem virtuellen Computer her.
2. Öffnen Sie das Eingabeaufforderungsfenster als Administrator. Wechseln Sie in das Verzeichnis *%windir%\system32\sysprep*, und führen Sie anschließend `sysprep.exe` aus.
3. Wählen Sie unter **Systemvorbereitungsprogramm** die Option **Out-of-Box-Experience (OOBE) für System aktivieren**, und vergewissern Sie sich, dass das Kontrollkästchen **Verallgemeinern** aktiviert ist.
4. Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren** aus, und klicken Sie auf **OK**.
5. Nach Abschluss von Sysprep wird die virtuelle Maschine heruntergefahren. **Starten Sie den virtuellen Computer nicht neu**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Aufheben der Zuordnung und Markieren des virtuellen Computers als generalisiert

Um ein Image zu erstellen, muss die Zuordnung des virtuellen Computers aufgehoben werden, und er muss in Azure als generalisiert markiert werden.

Heben Sie mit [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) die Zuordnung des virtuellen Computers auf.

```azurepowershell-interactive
Stop-AzVM `
   -ResourceGroupName myResourceGroup `
   -Name myVM -Force
```

Legen Sie mithilfe von [Set-AzVm](https://docs.microsoft.com/powershell/module/az.compute/set-azvm) den Status des virtuellen Computers auf `-Generalized` fest. 
   
```azurepowershell-interactive
Set-AzVM `
   -ResourceGroupName myResourceGroup `
   -Name myVM -Generalized
```


## <a name="create-the-image"></a>Erstellen des Images

Nun können Sie mit [New-AzImageConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azimageconfig) und [New-AzImage](https://docs.microsoft.com/powershell/module/az.compute/new-azimage) ein Image des virtuellen Computers erstellen. Im folgenden Beispiel wird ein Image mit dem Namen *myImage* vom virtuellen Computer *myVM* erstellt.

Rufen Sie den virtuellen Computer ab. 

```azurepowershell-interactive
$vm = Get-AzVM `
   -Name myVM `
   -ResourceGroupName myResourceGroup
```

Erstellen Sie die Imagekonfiguration.

```azurepowershell-interactive
$image = New-AzImageConfig `
   -Location EastUS `
   -SourceVirtualMachineId $vm.ID 
```

Erstellen Sie das Image.

```azurepowershell-interactive
New-AzImage `
   -Image $image `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Erstellen von VMs anhand des Images

Nachdem Sie ein Image erstellt haben, können Sie anhand des Images eine oder mehrere neue VMs erstellen. Das Erstellen einer VM anhand eines benutzerdefinierten Images ist vergleichbar mit dem Erstellen einer VM mithilfe eines Marketplace-Images. Wenn Sie ein Marketplace-Image verwenden, müssen Sie Informationen zum Image, zu dessen Anbieter, zum Angebot, zur SKU und Version bereitstellen. Mit dem vereinfachten Parametersatz für das Cmdlet [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) müssen Sie nur den Namen des benutzerdefinierten Images angeben (vorausgesetzt, es befindet sich in der gleichen Ressourcengruppe). 

Dieses Beispiel erstellt auf der Grundlage des Images *myImage* einen virtuellen Computer namens *myVMfromImage* in *myResourceGroup*.


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```

Es wird empfohlen, die Anzahl gleichzeitiger Bereitstellungen auf der Grundlage eines einzelnen Images auf 20 virtuelle Computer zu beschränken. Falls Sie umfangreiche gleichzeitige Bereitstellungen von 20 virtuellen Computern auf der Grundlage desselben benutzerdefinierten Images planen, sollten Sie eine [Shared Image Gallery](shared-image-galleries.md)-Instanz mit mehreren Imagereplikaten verwenden. 


## <a name="image-management"></a>Verwaltung von Images 

Hier sind einige Beispiele für allgemeine Aufgaben des verwalteten Images und ihre Ausführung mithilfe von PowerShell.

Auflisten aller Images nach Namen.

```azurepowershell-interactive
$images = Get-AzResource -ResourceType Microsoft.Compute/images 
$images.name
```

Löschen eines Images. Dieses Beispiel löscht das Image mit dem Namen *myImage* aus *myResourceGroup*.

```azurepowershell-interactive
Remove-AzImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie ein benutzerdefiniertes Image eines virtuellen Computers erstellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Sysprep und Generalisieren virtueller Computer
> * Erstellen eines benutzerdefinierten Images
> * Erstellen eines virtuellen Computers aus einem benutzerdefinierten Image
> * Liste aller Images in Ihrem Abonnement
> * Löschen eines Images

Im nächsten Tutorial erhalten Sie Informationen zum Erstellen hoch verfügbarer virtueller Computer.

> [!div class="nextstepaction"]
> [Erstellen eines hoch verfügbaren virtuellen Computers](tutorial-availability-sets.md)



