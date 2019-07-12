---
title: Verwalten von App-Gruppen für Windows Virtual Desktop (Vorschauversion) – Azure
description: Hier erfahren Sie, wie Sie in Azure Active Directory Mandanten mit Windows Virtual Desktop (Vorschauversion) einrichten.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 73425df1f0cfedd2a681650fc2b536a652b621d5
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206673"
---
# <a name="tutorial-manage-app-groups-for-windows-virtual-desktop-preview"></a>Tutorial: Verwalten von App-Gruppen für Windows Virtual Desktop (Vorschauversion)

Die Standard-App-Gruppe, die für einen neuen Hostpool für Windows Virtual Desktop (Vorschauversion) erstellt wird, veröffentlicht auch den vollständigen Desktop. Zusätzlich können Sie RemoteApp-Anwendungsgruppen für den Hostpool erstellen. In diesem Tutorial erfahren Sie, wie Sie eine RemoteApp-Gruppe erstellen und individuelle **Startmenü**-Apps veröffentlichen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer RemoteApp-Gruppe
> * Gewähren von Zugriff auf RemoteApp-Programme

Zur Vorbereitung müssen Sie ggf. zunächst das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), um es in Ihrer PowerShell-Sitzung verwenden zu können.

## <a name="create-a-remoteapp-group"></a>Erstellen einer RemoteApp-Gruppe

1. Führen Sie das folgende PowerShell-Cmdlet aus, um eine neue leere RemoteApp-App-Gruppe zu erstellen:

   ```powershell
   New-RdsAppGroup <tenantname> <hostpoolname> <appgroupname> -ResourceType "RemoteApp"
   ```

2. Optional: Vergewissern Sie sich, dass die App-Gruppe erstellt wurde. Führen Sie hierzu das folgende Cmdlet aus, um eine Liste mit allen App-Gruppen für den Hostpool anzuzeigen.

   ```powershell
   Get-RdsAppGroup <tenantname> <hostpoolname>
   ```

3. Führen Sie das folgende Cmdlet aus, um eine Liste mit allen **Startmenü**-Apps im VM-Image des Hostpools abzurufen. Notieren Sie sich die Werte für **FilePath**, **IconPath** und **IconIndex** sowie weitere wichtige Informationen für die zu veröffentlichende Anwendung.

   ```powershell
   Get-RdsStartMenuApp <tenantname> <hostpoolname> <appgroupname>
   ```
   
4. Führen Sie das folgende Cmdlet aus, um die Anwendung auf der Grundlage ihres App-Alias (`AppAlias`) zu installieren. `AppAlias` ist in der Ausgabe von Schritt 3 enthalten.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -AppAlias <appalias>
   ```

5. Optional: Führen Sie das folgende Cmdlet aus, um ein neues RemoteApp-Programm in der in Schritt 1 erstellten Anwendungsgruppe zu veröffentlichen.

   ```powershell
   New-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname> -Name <remoteappname> -Filepath <filepath>  -IconPath <iconpath> -IconIndex <iconindex>
   ```

6. Führen Sie das folgende Cmdlet aus, um sich zu vergewissern, dass die App veröffentlicht wurde:

   ```powershell
   Get-RdsRemoteApp <tenantname> <hostpoolname> <appgroupname>
   ```

7. Wiederholen Sie die Schritte 1 bis 5 für jede Anwendung, die Sie für diese App-Gruppe veröffentlichen möchten.
8. Führen Sie das folgende Cmdlet aus, um Benutzern Zugriff auf die RemoteApp-Programme in der App-Gruppe zu gewähren.

   ```powershell
   Add-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname> -UserPrincipalName <userupn>
   ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie App-Gruppen erstellen, sie mit RemoteApp-Programmen auffüllen und der App-Gruppe Benutzer hinzufügen. Informationen zum Erstellen eines Überprüfungshostpools finden Sie im folgenden Tutorial. Mit einem Überprüfungshostpool können Sie Dienstupdates überwachen, bevor Sie sie in Ihrer Produktionsumgebung bereitstellen.

> [!div class="nextstepaction"]
> [Erstellen eines Hostpools zum Überprüfen von Dienstupdates](./create-validation-host-pool.md)
