---
title: 'Bedingter Zugriff: Blockieren der Legacyauthentifizierung – Azure Active Directory'
description: Erstellen einer Richtlinien für den bedingten Zugriff, um Legacyauthentifizierungsprotokolle zu blockieren
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: b992973beb7cb132075e47e104733d812dc06ca0
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73151083"
---
# <a name="conditional-access-block-legacy-authentication"></a>Bedingter Zugriff: Blockieren älterer Authentifizierungsmethoden

Aufgrund des erhöhten Risikos, das mit Legacyauthentifizierungsprotokollen verbunden ist, empfiehlt Microsoft, dass Unternehmen entsprechende Authentifizierungsanforderungen, die diese Protokolle verwenden, blockieren und eine moderne Authentifizierung erfordern.

## <a name="create-a-conditional-access-policy"></a>Erstellen der Richtlinie für bedingten Zugriff

Durch Ausführen der folgenden Schritte können Sie eine Richtlinie für bedingten Zugriff erstellen, um Legacyauthentifizierungsanforderungen zu blockieren.

1. Melden Sie sich beim **Azure-Portal** als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Navigieren Sie zu **Azure Active Directory** > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Benennen Sie Ihre Richtlinie. Es wird empfohlen, dass Unternehmen einen aussagekräftigen Standard für die Namen ihrer Richtlinien erstellen.
1. Wählen Sie unter **Zuweisungen** die Option **Benutzer und Gruppen** aus.
   1. Wählen Sie unter **Einschließen** die Option **Alle Benutzer** aus.
   1. Wählen Sie unter **Ausschließen** die Option **Benutzer und Gruppen** und dann alle Konten aus, die die Möglichkeit aufweisen müssen, die Legacyauthentifizierung zu verwenden. 
   1. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Cloud-Apps oder -aktionen** > **Einschließen** die Option **Alle Cloud-Apps** aus.
   1. Wenn Sie bestimmte Anwendungen von Ihrer Richtlinie ausschließen müssen, können Sie sie auf der Registerkarte **Ausschließen** unter **Ausgeschlossene Cloudanwendungen auswählen** die Option **Auswählen** auswählen.
   1. Wählen Sie **Fertig**aus.
1. Legen Sie unter **Bedingungen** > **Client-Apps (Vorschau)** die Option **Konfigurieren** auf **Ja** fest.
   1. Aktivieren Sie nur die Kontrollkästchen **Mobile Apps und Desktopclients** > **Andere Clients**.
   2. Wählen Sie **Fertig**aus.
1. Wählen Sie unter **Zugriffssteuerung** > **Erteilen** die Option **Zugriff blockieren** aus.
   1. Wählen Sie **Auswählen**.
1. Bestätigen Sie die Einstellungen und legen Sie **Richtlinie aktivieren** auf **Ein** fest.
1. Wählen Sie **Erstellen** aus, um die Richtlinie zu erstellen und zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte

[Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)
