---
title: Cookie-Definitionen – Azure Active Directory B2C | Microsoft-Dokumentation
description: Enthält Definitionen für die Cookies, die in Azure Active Directory B2C verwendet werden.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: af3244a32e9d02a1ba5053da85547bf614053127
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67587409"
---
# <a name="cookies-definitions-for-azure-active-directory-b2c"></a>Cookie-Definitionen für Azure Active Directory B2C

Die folgende Tabelle enthält die Cookies, die in Azure Active Directory B2C verwendet werden.

| NAME | Domäne | Ablauf | Zweck |
| ----------- | ------ | -------------------------- | --------- |
| x-ms-cpim-admin | main.b2cadmin.ext.azure.com | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Enthält mandantenübergreifende Daten zur Benutzermitgliedschaft: Mandanten, zu denen ein Benutzer gehört, und die Ebene der Mitgliedschaft („Admin“ oder „Benutzer“). |
| x-ms-cpim-slice | login.microsoftonline.com, b2clogin.com, mit Branding versehene Domäne | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Zum Weiterleiten von Anforderungen an die entsprechende Produktionsinstanz. |
| x-ms-cpim-trans | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Zum Nachverfolgen von Transaktionen (Anzahl der Authentifizierungsanforderungen an Azure AD B2C) und der aktuellen Transaktion. |
| x-ms-cpim-sso:{Id} | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Zum Verwalten der Sitzung mit einmaligem Anmelden (SSO). |
| x-ms-cpim-cache:{id}_n | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md), erfolgreiche Authentifizierung | Zum Verwalten des Anforderungsstatus. |
| x-ms-cpim-csrf | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Token der websiteübergreifenden Anforderungsfälschung zum Schutz vor CSRF-Angriffen. |
| x-ms-cpim-dc | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Für Azure AD B2C-Netzwerkrouting. |
| x-ms-cpim-ctx | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Kontext |
| x-ms-cpim-rp | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Zum Speichern der Mitgliedschaftsdaten für den Ressourceourcenanbieter-Mandanten. |
| x-ms-cpim-rc | login.microsoftonline.com, b2clogin.com, marken-/unternehmensbezogener Domänenname | Ende der [Browsersitzung](active-directory-b2c-token-session-sso.md) | Zum Speichern des Relay-Cookies. |

