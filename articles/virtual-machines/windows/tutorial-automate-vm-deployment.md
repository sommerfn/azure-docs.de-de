---
title: 'Tutorial: Installieren von Anwendungen auf einem virtuellen Windows-Computer in Azure'
description: In diesem Tutorial erfahren Sie, wie Sie die benutzerdefinierte Skripterweiterung zum Ausführen von Skripts und Bereitstellen von Anwendungen auf virtuellen Windows-Computern in Azure verwenden.
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
ms.date: 11/29/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 610f8efad473b5f4bed1abc6b2c063ec0ead66ed
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74065362"
---
# <a name="tutorial---deploy-applications-to-a-windows-virtual-machine-in-azure-with-the-custom-script-extension"></a>Tutorial: Bereitstellen von Anwendungen auf einem virtuellen Windows-Computer in Azure mit der benutzerdefinierten Skripterweiterung

Virtuelle Computer (virtual machines, VMs) können unter Verwendung der [benutzerdefinierten Skripterweiterung für Windows](extensions-customscript.md) schnell und konsistent konfiguriert werden. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verwenden der benutzerdefinierten Skripterweiterung zur Installation von IIS
> * Erstellen eines virtuellen Computers, der die benutzerdefinierte Skripterweiterung verwendet
> * Anzeigen einer ausgeführten IIS-Website aus nach Anwendung der Erweiterung

## <a name="launch-azure-cloud-shell"></a>Starten von Azure Cloud Shell

Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Tools und ist für die Verwendung mit Ihrem Konto konfiguriert. 

Wählen Sie zum Öffnen von Cloud Shell oben rechts in einem Codeblock einfach die Option **Ausprobieren**. Sie können Cloud Shell auch auf einer separaten Browserregisterkarte starten, indem Sie zu [https://shell.azure.com/powershell](https://shell.azure.com/powershell) navigieren. Wählen Sie **Kopieren**, um die Blöcke mit dem Code zu kopieren. Fügen Sie ihn anschließend in Cloud Shell ein, und drücken Sie die EINGABETASTE, um ihn auszuführen.

## <a name="custom-script-extension-overview"></a>Übersicht über benutzerdefinierte Skripterweiterungen
Die benutzerdefinierte Skripterweiterung lädt Skripts auf Azure-VMs herunter und führt sie aus. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Skripts können aus Azure Storage oder GitHub heruntergeladen oder dem Azure-Portal zur Laufzeit für die Erweiterung bereitgestellt werden.

Die benutzerdefinierte Skripterweiterung kann in Azure Resource Manager-Vorlagen integriert und auch mithilfe der Azure-Befehlszeilenschnittstelle, mithilfe von PowerShell, über das Azure-Portal oder unter Verwendung der REST-API für virtuelle Azure-Computer ausgeführt werden.

Sie können die benutzerdefinierte Skripterweiterung mit Windows- und Linux-VMs verwenden.


## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers
Legen Sie mit [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) den Benutzernamen und das Kennwort des Administrators des virtuellen Computers fest:

```azurepowershell-interactive
$cred = Get-Credential
```

Nun können Sie mit [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) den virtuellen Computer erstellen. Im folgenden Beispiel wird eine VM mit dem Namen *myVM* für den Standort *EastUS* erstellt. Falls sie nicht bereits vorhanden sind, werden die Ressourcengruppe *myResourceGroupAutomate* und unterstützende Netzwerkressourcen erstellt. Um Webdatenverkehr zuzulassen, öffnet das Cmdlet auch Port *80*.

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroupAutomate" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80 `
    -Credential $cred
```

Die Erstellung der Ressourcen und VM dauert einige Minuten.


## <a name="automate-iis-install"></a>Automatisieren der Installation von IIS
Verwenden Sie [Set-AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension), um die benutzerdefinierte Skripterweiterung zu installieren. Die Erweiterung führt `powershell Add-WindowsFeature Web-Server` zum Installieren des IIS-Webservers aus und aktualisiert dann die Seite *Default.htm* mit dem Hostnamen der VM:

```azurepowershell-interactive
Set-AzVMExtension -ResourceGroupName "myResourceGroupAutomate" `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location "EastUS" `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}'
```


## <a name="test-web-site"></a>Testen der Website
Rufen Sie mit [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) die öffentliche IP-Adresse Ihres Lastenausgleichs ab. Im folgenden Beispiel wird die IP-Adresse für *myPublicIPAddress* abgerufen, die wir zuvor erstellt haben:

```azurepowershell-interactive
Get-AzPublicIPAddress `
    -ResourceGroupName "myResourceGroupAutomate" `
    -Name "myPublicIPAddress" | select IpAddress
```

Geben Sie die öffentliche IP-Adresse in einem Webbrowser ein. Die Website wird mit dem Hostnamen der VM angezeigt, an die der Load Balancer den Datenverkehr wie im folgenden Beispiel verteilt hat:

![Ausführen der IIS-Website](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die IIS-Installation auf einem virtuellen Computer automatisiert. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Verwenden der benutzerdefinierten Skripterweiterung zur Installation von IIS
> * Erstellen eines virtuellen Computers, der die benutzerdefinierte Skripterweiterung verwendet
> * Anzeigen einer ausgeführten IIS-Website aus nach Anwendung der Erweiterung

Im nächsten Tutorial erfahren Sie, wie Sie benutzerdefinierte VM-Images erstellen.

> [!div class="nextstepaction"]
> [Erstellen von benutzerdefinierten VM-Images](./tutorial-custom-images.md)
