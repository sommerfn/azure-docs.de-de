---
title: 'Konfigurieren der Lastenausgleichsmethode für Windows Virtual Desktop: Azure'
description: Es wird beschrieben, wie Sie die Lastenausgleichsmethode für eine Windows Virtual Desktop-Umgebung konfigurieren.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: helohr
ms.openlocfilehash: 3a940dbf592087878cb9dd19f856f1a3d94291c5
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71676778"
---
# <a name="configure-the-windows-virtual-desktop-load-balancing-method"></a>Konfigurieren der Lastenausgleichsmethode für Windows Virtual Desktop

Beim Konfigurieren der Lastenausgleichsmethode für einen Hostpool können Sie die Windows Virtual Desktop-Umgebung anpassen, damit Ihre Anforderungen besser erfüllt werden.

>[!NOTE]
> Dies gilt nicht für einen dauerhaften Desktophostpool, da Benutzer im Hostpool immer über eine 1:1-Zuordnung zu einem Sitzungshost verfügen.

## <a name="configure-breadth-first-load-balancing"></a>Konfigurieren der Lastenausgleichsmethode „Breiter Ansatz“

Die Lastenausgleichsmethode „Breiter Ansatz“ ist die Standardkonfiguration für neue nicht dauerhafte Hostpools. Bei der Lastenausgleichsmethode „Breiter Ansatz“ werden neue Benutzersitzungen auf alle verfügbaren Sitzungshosts im Hostpool verteilt. Wenn Sie die Lastenausgleichsmethode „Breiter Ansatz“ konfigurieren, können Sie ein maximales Sitzungslimit pro Sitzungshost im Hostpool festlegen.

Zunächst müssen Sie das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), um es in Ihrer PowerShell-Sitzung verwenden zu können. Führen Sie anschließend das folgende Cmdlet aus, um sich bei Ihrem Konto anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Hostpool zum Durchführen der Lastenausgleichsmethode „Breiter Ansatz“ zu konfigurieren, ohne das maximale Sitzungslimit anzupassen:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer
```

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Hostpool zum Durchführen der Lastenausgleichsmethode „Breiter Ansatz“ und zum Verwenden eines neuen maximalen Sitzungslimits zu konfigurieren:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer -MaxSessionLimit ###
```

## <a name="configure-depth-first-load-balancing"></a>Konfigurieren der Lastenausgleichsmethode „Tiefer Ansatz“

Bei der Lastenausgleichsmethode „Tiefer Ansatz“ werden neue Benutzersitzungen auf einen verfügbaren Sitzungshost mit der höchsten Anzahl von Verbindungen verteilt, für den der Schwellenwert des maximalen Sitzungslimits aber noch nicht erreicht wurde. Wenn Sie die Lastenausgleichsmethode „Tiefer Ansatz“ konfigurieren, **müssen** Sie ein maximales Sitzungslimit pro Sitzungshost im Hostpool festlegen.

Führen Sie das folgende PowerShell-Cmdlet aus, um einen Hostpool für die Durchführung der Lastenausgleichsmethode „Tiefer Ansatz“ zu konfigurieren:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -DepthFirstLoadBalancer -MaxSessionLimit ###
```
