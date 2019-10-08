---
title: 'Delegierter Zugriff in Windows Virtual Desktop: Azure'
description: Es wird beschrieben, wie Sie Verwaltungsfunktionen in einer Windows Virtual Desktop-Bereitstellung delegieren, und es werden Beispiele gezeigt.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: e8d1ba00043f43f626043d78ce0ab8953a0b3fbe
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679566"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Delegierter Zugriff in Windows Virtual Desktop

Windows Virtual Desktop verfügt über ein Modell mit delegiertem Zugriff, mit dem Sie für Benutzer eine bestimmte Zugriffsebene festlegen können, indem Sie ihnen eine Rolle zuweisen. Eine Rollenzuweisung besteht aus drei Komponenten: Sicherheitsprinzipal, Rollendefinition und Bereich. Das Modell für delegierten Zugriff von Windows Virtual Desktop basiert auf dem Azure RBAC-Modell (rollenbasierte Zugriffssteuerung). Weitere Informationen zu bestimmten Rollenzuweisungen und den zugehörigen Komponenten finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles).

Beim delegierten Zugriff von Windows Virtual Desktop werden für jedes Element der Rollenzuweisung die folgenden Werte unterstützt:

* Sicherheitsprinzipal
    * Benutzer
    * Dienstprinzipale
* Rollendefinition
    * Integrierte Rollen
* `Scope`
    * Mandantengruppen
    * Mandanten
    * Hostpools
    * App-Gruppen

## <a name="built-in-roles"></a>Integrierte Rollen

Der delegierte Zugriff in Windows Virtual Desktop verfügt über mehrere integrierte Rollendefinitionen, die Sie Benutzern und Dienstprinzipalen zuweisen können.

* RDS-Besitzer können alles verwalten, einschließlich des Zugriffs auf Ressourcen.
* RDS-Mitwirkende können alles mit Ausnahme des Zugriffs auf Ressourcen verwalten.
* RDS-Leser können alles anzeigen, aber keine Änderungen vornehmen.
* RDS-Bediener können Diagnoseaktivitäten anzeigen.

## <a name="powershell-cmdlets-for-role-assignments"></a>PowerShell-Cmdlets für Rollenzuweisungen

Sie können die folgenden Cmdlets ausführen, um Rollenzuweisungen zu erstellen, anzuzeigen und zu entfernen:

* Mit **Get-RdsRoleAssignment** wird eine Liste mit Rollenzuweisungen angezeigt.
* Mit **New-RdsRoleAssignment** wird eine neue Rollenzuweisung erstellt.
* Mit **Remove-RdsRoleAssignment** werden Rollenzuweisungen gelöscht.

### <a name="accepted-parameters"></a>Akzeptierte Parameter

Sie können die drei grundlegenden Cmdlets mit den folgenden Parametern ändern:

* **AadTenantId**: Gibt die ID des Azure Active Directory-Mandanten an, dessen Mitglied der Dienstprinzipal ist.
* **AppGroupName**: Der Name der Remotedesktop-App-Gruppe.
* **Diagnostics**: Gibt den Diagnosebereich an. (Muss entweder mit dem Parameter **Infrastructure** oder **Tenant** gekoppelt werden.)
* **HostPoolName**: Name des Remotedesktop-Hostpools.
* **Infrastructure**: Gibt den Infrastrukturbereich an.
* **RoleDefinitionName**: Der Name der Remotedesktopdienste-Rolle der rollenbasierten Zugriffssteuerung, die dem Benutzer, der Gruppe oder der App zugewiesen ist. (Beispiel: Remotedesktopdienste-Besitzer, Remotedesktopdienste-Leser usw.)
* **ServerPrincipleName**: Der Name der Azure Active Directory-Anwendung.
* **SignInName**: Die E-Mail-Adresse oder der Benutzerprinzipalname des Benutzers.
* **TenantName**: Der Name des Remotedesktop-Mandanten.

## <a name="next-steps"></a>Nächste Schritte

Eine vollständigere Liste mit PowerShell-Cmdlets, die von den einzelnen Rollen verwendet werden können, finden Sie in der [PowerShell-Referenz](/powershell/windows-virtual-desktop/overview).

Eine Anleitung zum Einrichten einer Windows Virtual Desktop-Umgebung finden Sie unter [Windows Virtual Desktop-Umgebung](environment-setup.md).
