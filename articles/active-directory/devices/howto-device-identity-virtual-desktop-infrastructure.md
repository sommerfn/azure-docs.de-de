---
title: Geräteidentität und Desktopvirtualisierung – Azure Active Directory
description: Erfahren Sie, wie Sie die virtuelle Desktopinfrastruktur (VDI) und Azure AD-Geräteidentitäten gemeinsam verwenden können.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a1cba2c4572b2f898f631aefbbf316fae1195ac
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72596354"
---
# <a name="device-identity-and-desktop-virtualization"></a>Geräteidentität und Desktopvirtualisierung

Administratoren stellen in ihren Organisationen häufig VDI-Plattformen (Virtual Desktop Infrastructure) bereit, auf denen Windows-Betriebssysteme gehostet werden. Administratoren stellen VDI-Plattformen für Folgendes bereit:

- Einfachere Verwaltung
- Kostensenkung durch Konsolidierung und Zentralisierung von Ressourcen
- Endbenutzermobilität mit der Freiheit, jederzeit, überall und von jedem Gerät aus auf virtuelle Desktops zuzugreifen

Es gibt zwei Hauptarten von virtuellen Desktops:

- Beständig
- Nicht beständig

Bei der beständigen Version wird ein eindeutiges Desktopimage für jeden Benutzer oder einen Benutzerpool verwendet. Diese eindeutigen Desktops können angepasst und zur späteren Verwendung gespeichert werden. 

Bei der nicht beständigen Version wird eine Sammlung von Desktops verwendet, auf die Benutzer bei Bedarf zugreifen können. Diese nicht beständigen Desktops werden nach dem Abmelden des Benutzers wieder in ihren ursprünglichen Zustand versetzt.

Dieser Artikel enthält den Microsoft-Leitfaden für Administratoren zur Unterstützung von Geräteidentität und VDI. Weitere Informationen zur Geräteidentität finden Sie im Artikel [Was ist eine Geräteidentität](overview.md).

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Bevor Sie die Geräteidentitäten in Azure AD für Ihre VDI-Umgebung konfigurieren, machen Sie sich mit den unterstützten Szenarien vertraut. In der folgenden Tabelle wird veranschaulicht, welche Bereitstellungsszenarien unterstützt werden. Eine Bereitstellung in diesem Zusammenhang impliziert, dass ein Administrator Geräteidentitäten skaliert konfigurieren kann, ohne dass eine Endbenutzerinteraktion erforderlich ist.

| Geräteidentitätstyp | Identitätsinfrastruktur | Windows-Geräte | VDI-Plattformversion | Unterstützt |
| --- | --- | --- | --- | --- |
| Hybrid in Azure AD eingebunden | Im Verbund* | Aktuelle Windows-Geräte*** und kompatible Windows-Geräte**** | Beständig | Ja |
|   |   |   | Nicht beständig | Ja |
|   | Verwaltet** | Aktuelle Windows-Geräte und kompatible Windows-Geräte | Beständig | Ja |
|   |   | Kompatible Windows-Geräte | Nicht beständig | Ja |
|   |   | Aktuelle Windows-Geräte | Nicht beständig | Nein |
| In Azure AD eingebunden | Im Verbund | Aktuelle Windows-Geräte | Beständig | Nein |
|   |   |   | Nicht beständig | Nein |
|   | Verwaltet | Aktuelle Windows-Geräte | Beständig | Nein |
|   |   |   | Nicht beständig | Nein |
| Bei Azure AD registriert | Im Verbund | Aktuelle Windows-Geräte | Beständig | Nein |
|   |   |   | Nicht beständig | Nein |
|   | Verwaltet | Aktuelle Windows-Geräte | Beständig | Nein |
|   |   |   | Nicht beständig | Nein |

\* Eine Identitätsinfrastruktur-Umgebung des Typs **Im Verbund** ist eine Umgebung mit einem Identitätsanbieter wie AD FS oder einem Drittanbieter.

\*\* Eine Identitätsinfrastruktur-Umgebung des Typs **Verwaltet** ist eine Umgebung mit Azure AD als Identitätsanbieter und wird entweder mit [Kennworthashsynchronisierung (PHS)](../hybrid/whatis-phs.md) oder [Passthrough-Authentifizierung (PTA) ](../hybrid/how-to-connect-pta.md) und [nahtloser einmaliger Anmeldung](../hybrid/how-to-connect-sso.md) bereitgestellt.

\*\*\* **Aktuelle Windows-Geräte** bezieht sich auf Geräte mit Windows 10, Windows Server 2016 und Windows Server 2019.

\*\*\*\* **Kompatible Windows-Geräte** bezieht sich auf Geräte mit Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2. Supportinformationen zu Windows 7 finden Sie unter [Der Support für Windows 7 läuft aus](https://www.microsoft.com/microsoft-365/windows/end-of-windows-7-support). Supportinformationen zu Windows Server 2008 R2 finden Sie unter [Der Support für Windows Server 2008 wird eingestellt](https://www.microsoft.com/cloud-platform/windows-server-2008).

## <a name="microsofts-guidance"></a>Leitfaden von Microsoft

Administratoren sollten auf Grundlage ihrer Identitätsinfrastruktur die folgenden Artikel heranziehen, um zu erfahren, wie sie die Azure AD-Hybrideinbindung konfigurieren können.

- [Konfigurieren der Azure Active Directory-Hybrideinbindung für eine Verbundumgebung](hybrid-azuread-join-federated-domains.md)
- [Konfigurieren der Azure Active Directory-Hybrideinbindung für eine verwaltete Umgebung](hybrid-azuread-join-managed-domains.md)

Wenn Sie das Systemvorbereitungstool (sysprep.exe) und ein Image einer niedrigeren Version als Windows 10 1809 für die Installation verwenden, stellen Sie sicher, dass dieses Image nicht von einem Gerät stammt, das bereits als Azure AD-Hybrideinbindung bei Azure AD registriert ist.

Wenn Sie zusätzliche VMs mit einer Momentaufnahme des virtuellen Computers erstellen, stellen Sie sicher, dass diese Momentaufnahme nicht von einem Computer stammt, der bereits als Azure AD-Hybrideinbindung bei Azure AD registriert ist.

Bei der Bereitstellung einer nicht beständigen VDI-Plattform müssen IT-Administratoren besonders auf die Verwaltung veralteter Geräte in Azure AD achten. Microsoft empfiehlt IT-Administratoren, den folgenden Leitfaden zu implementieren. Versäumnisse in diesem Bereich führen dazu, dass das Verzeichnis eine Vielzahl veralteter Geräte mit Azure AD-Hybrideinbindung enthält, die von der nicht beständigen VDI-Plattform aus registriert wurden.

- Erstellen und verwenden Sie ein Präfix für den Anzeigenamen des Computers, das darauf hinweist, dass der Desktop VDI-basiert ist.
- Implementieren Sie die folgenden Befehle als Teil des Abmeldeskripts. Mit diesen Befehlen wird der bestmögliche Aufruf an Azure AD zum Löschen des Geräts ausgegeben.
   - Aktuelle Windows-Geräte: dsregcmd.exe /leave
   - Kompatible Windows-Geräte: autoworkplace.exe /leave
- Definieren und implementieren Sie einen Prozess zum [Verwalten veralteter Geräte](manage-stale-devices.md).
   - Sobald Sie über eine Strategie zum Identifizieren Ihrer nicht beständigen Geräte mit Azure AD-Hybrideinbindung verfügen, können Sie die Bereinigung dieser Geräte aggressiver betreiben, um sicherzustellen, dass Ihr Verzeichnis nicht von vielen veralteten Geräten genutzt wird.
 
## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren der Azure Active Directory-Hybrideinbindung für eine Verbundumgebung](hybrid-azuread-join-federated-domains.md)
