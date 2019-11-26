---
title: Lizenzanforderung für die Verwendung von Privileged Identity Management – Azure Active Directory | Microsoft-Dokumentation
description: Beschreibt die Lizenzierungsanforderungen für die Verwendung von Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 10/23/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6e336ec40ba2b9f6d3018e6a4f5b2ac721077aa
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72895115"
---
# <a name="license-requirements-to-use-privileged-identity-management"></a>Lizenzanforderungen für die Verwendung von Privileged Identity Management

Damit Sie Azure Active Directory (Azure AD) Privileged Identity Management (PIM) verwenden können, muss das Verzeichnis über eine gültige Lizenz verfügen. Darüber hinaus müssen den Administratoren und den erforderlichen Benutzern Lizenzen zugewiesen werden. In diesem Artikel werden die Lizenzanforderungen für die Verwendung von Privileged Identity Management beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Um Privileged Identity Management verwenden zu können, muss Ihr Verzeichnis über eine der folgenden kostenpflichtigen Lizenzen oder Testlizenzen verfügen:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5
- Microsoft 365 M5

Weitere Informationen finden Sie unter [Was ist Azure Active Directory?](../fundamentals/active-directory-whatis.md).

## <a name="which-users-must-have-licenses"></a>Welche Benutzer benötigen Lizenzen?

Jeder Administrator oder Benutzer, der einen Vorteil von Privileged Identity Management nutzt oder erhält, muss über eine Lizenz verfügen. Beispiele:

- Administratoren mit Azure AD-Rollen, die mit PIM verwaltet werden
- Administratoren mit Azure-Ressourcenrollen, die mit PIM verwaltet werden
- Administratoren, die der Rolle „Administrator für privilegierte Rollen“ zugeordnet sind
- Als für Azure AD-Rollen berechtigt zugewiesene Benutzer, die mit PIM verwaltet werden
- Benutzer mit der Berechtigung zum Genehmigen/Ablehnen von Anforderungen in PIM
- Benutzer mit einer Azure-Ressourcenrolle mit Just-in-Time- oder direkten (zeitbasierten) Zuweisungen  
- Benutzer mit einer Zugriffsüberprüfung
- Benutzer, die Zugriffsüberprüfungen ausführen

Weitere Informationen zum Zuweisen von Lizenzen zu Ihren Benutzern finden Sie unter [Zuweisen oder Entfernen von Lizenzen im Azure Active Directory-Portal](../fundamentals/license-users-groups.md).

## <a name="what-happens-when-a-license-expires"></a>Was passiert, wenn eine Lizenz abläuft?

Wenn eine Lizenz vom Typ Azure AD Premium P2 oder EMS E5 bzw. eine Testlizenz abläuft, stehen die Funktionen von Privileged Identity Management in Ihrem Verzeichnis nicht mehr zur Verfügung:

- Permanente Rollenzuweisungen für Azure AD-Rollen sind nicht betroffen.
- Der Privileged Identity Management-Dienst im Azure-Portal sowie die Graph-API-Cmdlets und PowerShell-Schnittstellen von Privileged Identity Management sind für Benutzer nicht mehr verfügbar, um privilegierte Rollen zu aktivieren, den privilegierten Zugriff zu verwalten oder Zugriffsüberprüfungen privilegierter Rollen auszuführen.
- Berechtigte Rollenzuweisungen von Azure AD-Rollen werden entfernt, da Benutzer keine privilegierten Rollen mehr aktivieren können.
- Alle laufenden Zugriffsüberprüfungen von Azure AD-Rollen werden beendet, und die Privileged Identity Management-Konfigurationseinstellungen werden entfernt.
- Privileged Identity Management sendet keine E-Mails mehr zu Rollenzuweisungsänderungen.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von Azure AD Privileged Identity Management (PIM)](pim-deployment-plan.md)
- [Einstieg in Privileged Identity Management](pim-getting-started.md)
- [Rollen, die nicht in Privileged Identity Management verwaltet werden können](pim-roles.md)
