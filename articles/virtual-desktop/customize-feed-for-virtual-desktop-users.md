---
title: Anpassen des Feeds für Windows Virtual Desktop-Benutzer – Azure
description: Erfahren Sie, wie Sie den Feed für Windows Virtual Desktop-Benutzer mit PowerShell-Cmdlets anpassen.
services: virtual-desktop
author: v-hevem
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: v-hevem
ms.openlocfilehash: 5fe2a8b8ee5870ff7986ca2d91739f82a5128882
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618997"
---
# <a name="customize-feed-for-windows-virtual-desktop-users"></a>Anpassen von Feeds für Windows Virtual Desktop-Benutzer

Sie können den Feed anpassen, damit die RemoteApp- und Remotedesktopressourcen den Benutzern auf erkennbare Weise angezeigt werden.

Zunächst müssen Sie das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), um es in Ihrer PowerShell-Sitzung verwenden zu können.

## <a name="customize-the-display-name-for-a-remoteapp"></a>Anpassen des Anzeigenamens für eine RemoteApp

Sie können den Anzeigenamen für eine veröffentlichte RemoteApp ändern, indem Sie ihn festlegen. Standardmäßig ist der Anzeigename identisch mit dem Namen des RemoteApp-Programms.

Um eine Liste der veröffentlichten RemoteApps für eine App-Gruppe abzurufen, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Get-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```
![Screenshot des PowerShell-Cmdlets „Get-RDSRemoteApp“ mit hervorgehobenem Namen und Anzeigenamen](media/get-rdsremoteapp.png)

Um einer RemoteApp einen Anzeigenamen zuzuweisen, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Set-RdsRemoteApp -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -Name <existingappname> -FriendlyName <newfriendlyname>
```
![Screenshot des PowerShell-Cmdlets „Set-RDSRemoteApp“ mit hervorgehobenem Namen und neuem Anzeigenamen](media/set-rdsremoteapp.png)

## <a name="customize-the-display-name-for-a-remote-desktop"></a>Anpassen des Anzeigenamens für einen Remotedesktop

Sie können den Anzeigenamen für einen veröffentlichten Remotedesktop ändern, indem Sie einen Anzeigenamen festlegen. Wenn Sie manuell mit PowerShell einen Hostpool und eine Desktop-App-Gruppe erstellt haben, lautet der Anzeigename standardmäßig „Sitzungsdesktop“. Wenn Sie den Hostpool und die Desktop-App-Gruppe über die Azure Resource Manager-Vorlage von GitHub oder das Angebot im Azure Marketplace erstellt haben, ist der Standardanzeigename mit dem Namen des Hostpools identisch.

Führen Sie zum Abrufen der Remotedesktopressource das folgende PowerShell-Cmdlet aus:

```powershell
Get-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname>
```
![Screenshot des PowerShell-Cmdlets „Get-RDSRemoteApp“ mit hervorgehobenem Namen und Anzeigenamen](media/get-rdsremotedesktop.png)

Führen Sie zum Zuweisen eines Anzeigenamens zur Remotedesktopressource das folgende PowerShell-Cmdlet aus:

```powershell
Set-RdsRemoteDesktop -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName <appgroupname> -FriendlyName <newfriendlyname>
```
![Screenshot des PowerShell-Cmdlets „Set-RDSRemoteApp“ mit hervorgehobenem Namen und neuem Anzeigenamen](media/set-rdsremotedesktop.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie den Feed für Benutzer angepasst haben, können Sie sich bei einem Windows Virtual Desktop-Client anmelden, um ihn zu testen. Wechseln Sie dafür zu den Vorgehensweisen für das Herstellen einer Verbindung mit Windows Virtual Desktop:
    
 * [Herstellen einer Verbindung über Windows 10 oder Windows 7](connect-windows-7-and-10.md)
 * [Herstellen einer Verbindung über einen Webbrowser](connect-web.md) 
