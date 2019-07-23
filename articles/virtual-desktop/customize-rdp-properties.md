---
title: Anpassen von RDP-Eigenschaften mit PowerShell – Azure
description: Vorgehensweise zum Anpassen von RDP-Eigenschaften für Windows Virtual Desktop mit PowerShell-Cmdlets
services: virtual-desktop
author: v-hevem
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 06/03/2019
ms.author: v-hevem
ms.openlocfilehash: 21d0b45b974f4bc806b26423b7deaef96e4bf350
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082019"
---
# <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Anpassen der Remotedesktopprotokoll-Eigenschaften für einen Hostpool

Durch Anpassen von RDP-Eigenschaften (Remotedesktopprotokoll) eines Hostpools, z.B. Unterstützung mehrerer Monitore und Audioumleitung, können Sie für Benutzer eine optimale Bedienumgebung bereitstellen, die deren Anforderungen entspricht. RDP-Eigenschaften können Sie in Windows Virtual Desktop über den **-CustomRdpProperty**-Parameter im **Set-RdsHostPool**-Cmdlet anpassen.

Eine vollständige Liste der unterstützten Eigenschaften samt deren Standardwerten finden Sie unter [Unterstützte RDP-Dateieinstellungen für Remotedesktop](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files).

Zunächst müssen Sie das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview), um es in Ihrer PowerShell-Sitzung verwenden zu können.

## <a name="add-or-edit-a-single-custom-rdp-property"></a>Hinzufügen oder Bearbeiten einer einzelnen benutzerdefinierten RDP-Eigenschaft

Wenn Sie eine einzelne benutzerdefinierte RDP-Eigenschaft hinzufügen oder bearbeiten möchten, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty "<property>"
```
![Ein Screenshot des PowerShell-Cmdlets „Set-RdsHostPool“ mit hervorgehobener benutzerdefinierter RDP-Einstellung](media/singlecustomrdpproperty.png)

## <a name="add-or-edit-multiple-custom-rdp-properties"></a>Hinzufügen oder Bearbeiten von mehreren benutzerdefinierten RDP-Eigenschaften

Wenn Sie mehrere benutzerdefinierte RDP-Eigenschaften hinzufügen oder bearbeiten möchten, führen Sie die folgenden PowerShell-Cmdlets aus, wobei Sie die benutzerdefinierten RDP-Eigenschaften als eine Zeichenfolge mit Semikolons als Trennzeichen angeben:

```powershell
$properties="<property1>;<property2>;<property3>"
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty $properties
```
![Ein Screenshot des PowerShell-Cmdlets „Set-RdsHostPool“ mit hervorgehobener benutzerdefinierter RDP-Einstellung](media/multiplecustomrdpproperty.png)

## <a name="reset-all-custom-rdp-properties"></a>Zurücksetzen aller benutzerdefinierten RDP-Eigenschaften

Sie können eine einzelne benutzerdefinierte RDP-Eigenschaft auf deren Standardwert zurücksetzen, indem Sie den Anweisungen unter [Hinzufügen oder Bearbeiten einer einzelnen benutzerdefinierten RDP-Eigenschaft](#add-or-edit-a-single-custom-rdp-property) folgen, oder Sie können alle benutzerdefinierten RDP-Eigenschaften für einen Hostpool zurücksetzen, indem Sie das folgende PowerShell-Cmdlet ausführen:

```powershell
Set-RdsHostPool -TenantName <tenantname> -Name <hostpoolname> -CustomRdpProperty ""
```
![Ein Screenshot des PowerShell-Cmdlets „Set-RdsHostPool“ mit hervorgehobener benutzerdefinierter RDP-Einstellung](media/resetcustomrdpproperty.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die RDP-Eigenschaften eines bestimmten Hostpools angepasst haben, können Sie sich bei einem Windows Virtual Desktop-Client anmelden, um die Einstellungen als Teil einer Benutzersitzung zu testen. Wechseln Sie dafür zu den Vorgehensweisen für das Herstellen einer Verbindung mit Windows Virtual Desktop-Ressourcen:

- [Herstellen einer Verbindung über Windows 10 oder Windows 7](connect-windows-7-and-10.md)
- [Herstellen einer Verbindung über einen Webbrowser](connect-web.md)
