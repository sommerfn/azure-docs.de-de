---
title: 'Azure Active Directory Domain Services: Aktivieren des SharePoint-Benutzerprofildiensts | Microsoft-Dokumentation'
description: Konfigurieren von in Azure Active Directory Domain Services verwalteten Domänen für die Unterstützung der Profilsynchronisierung für SharePoint Server
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: iainfou
ms.openlocfilehash: 4a9ee05b37a69927d70269dccef2b74a2c251722
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234105"
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>Konfigurieren einer verwalteten Domäne für die Unterstützung der Profilsynchronisierung für SharePoint Server
SharePoint Server umfasst einen Benutzerprofildienst, der für die Benutzerprofilsynchronisierung verwendet wird. Zum Einrichten des Benutzerprofildiensts müssen in einer Active Directory-Domäne entsprechende Berechtigungen erteilt werden. Weitere Informationen finden Sie unter [Erteilen von Berechtigungen für Active Directory Domain Services für die Profilsynchronisierung in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).

In diesem Artikel wird erläutert, wie in Azure AD Domain Services verwaltete Domänen für die Bereitstellung des Benutzerprofil-Synchronisierungsdiensts von SharePoint Server konfiguriert werden können.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="the-aad-dc-service-accounts-group"></a>Die Gruppe „AAD-DC-Dienstkonten“
Eine Sicherheitsgruppe namens **AAD-DC-Dienstkonten** steht innerhalb der Organisationseinheit „Benutzer“ in Ihrer verwalteten Domäne zur Verfügung. Diese Gruppe wird im MMC-Snap-In **Active Directory-Benutzer und -Computer** für Ihre verwaltete Domäne angezeigt.

![Die Sicherheitsgruppe „AAD-DC-Dienstkonten“](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

An Mitglieder dieser Sicherheitsgruppe werden die folgenden Berechtigungen delegiert:
- Die Berechtigung „Verzeichnisänderungen replizieren“ für den Stamm-DSE der verwalteten Domäne.
- Die Berechtigung „Verzeichnisänderungen replizieren“ im Konfigurationsnamenskontext (cn=Konfigurationscontainer) der verwalteten Domäne.

Diese Sicherheitsgruppe ist außerdem ein Mitglied der integrierten Gruppe **Prä-Windows 2000 kompatibler Zugriff**.

![Die Sicherheitsgruppe „AAD-DC-Dienstkonten“](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>Aktivieren der verwalteten Domäne für die Unterstützung der Benutzerprofilsynchronisierung in SharePoint Server
Sie können den Dienstkontobenutzer für die SharePoint-Benutzerprofilsynchronisierung der Gruppe **AAD-DC-Dienstkonten** hinzufügen. Dadurch erhält das Synchronisierungskonto die entsprechenden Berechtigungen zum Replizieren von Änderungen in das Verzeichnis. Dieser Konfigurationsschritt ermöglicht eine ordnungsgemäße Ausführung der SharePoint Server-Benutzerprofilsynchronisierung.

![AAD-DC-Dienstkonten – Mitglieder hinzufügen](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD-DC-Dienstkonten – Mitglieder hinzufügen](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>Verwandte Inhalte
* [Technische Referenz – Erteilen von AD DS-Berechtigungen für die Profilsynchronisierung in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx)
