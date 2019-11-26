---
title: Microsoft Authentication Library (MSAL) für iOS und macOS
titleSuffix: Microsoft identity platform
description: Enthält eine Beschreibung der Unterschiede bei Verwendung der Microsoft Authentication Library (MSAL) unter iOS und macOS.
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2019
ms.author: twhitney
ms.reviewer: ''
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc57f68983b044a9196bc308c8f443635bdc7eb8
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73175757"
---
# <a name="microsoft-authentication-library-for-ios-and-macos-differences"></a>Microsoft Authentication Library für iOS und macOS: Unterschiede

In diesem Artikel werden die Funktionalitätsunterschiede bei Verwendung der Microsoft Authentication Library (MSAL) unter iOS und macOS beschrieben.

> [!NOTE]
> Auf einem Macintosh werden von der MSAL nur macOS-Apps unterstützt.

## <a name="general-differences"></a>Allgemeine Unterschiede

Die MSAL für macOS umfasst eine Teilmenge der Funktionalität von iOS.

Folgendes wird von der MSAL für macOS nicht unterstützt:

- Verschiedene Browsertypen, z. B. `ASWebAuthenticationSession`, `SFAuthenticationSession` und `SFSafariViewController`.
- Die Authentifizierung per Broker über die Microsoft Authenticator-App wird für macOS nicht unterstützt.

Für die Keychainfreigabe zwischen Apps desselben Herausgebers gelten unter macOS 10.14 und früher stärkere Einschränkungen. Verwenden Sie [Zugriffssteuerungslisten](https://developer.apple.com/documentation/security/keychain_services/access_control_lists?language=objc), um die Pfade zu den Apps anzugeben, von denen die Keychain gemeinsam verwendet werden soll. Es kann sein, dass Benutzern zusätzliche Keychain-Eingabeaufforderungen angezeigt werden.

Unter macOS 10.15+ ist das Verhalten der MSAL für iOS und macOS identisch. Für die MSAL werden [Keychain-Zugriffsgruppen](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) für die Keychainfreigabe verwendet. 

### <a name="conditional-access-authentication-differences"></a>Unterschiede bei der Authentifizierung für bedingten Zugriff

Bei Szenarien mit bedingtem Zugriff werden weniger Benutzereingabeaufforderungen angezeigt, wenn Sie die MSAL für iOS verwenden. Der Grund ist, dass für iOS die Broker-App (Microsoft Authenticator) genutzt wird, sodass die Aufforderung zur Eingabe des Benutzers in einigen Fällen nicht erforderlich ist.

### <a name="project-setup-differences"></a>Unterschiede bei der Projekteinrichtung

**macOS**

- Stellen Sie beim Einrichten Ihres Projekts unter macOS sicher, dass Ihre Anwendung mit einem gültigen Entwicklungs- oder Produktionszertifikat signiert ist. Die MSAL funktioniert auch im nicht signierten Modus, weist aber in Bezug auf die Cachepersistenz ein anderes Verhalten auf. Die App sollte nur für Debugzwecke unsigniert ausgeführt werden. Wenn Sie die App unsigniert herausgeben, gilt Folgendes:
1. Unter Version 10.14 und früher fordert die MSAL den Benutzer bei jedem neuen Start der App zur Eingabe eines Keychainkennworts auf.
2. Unter Version 10.15 und höher fordert die MSAL den Benutzer bei jedem Tokenabruf zur Eingabe der Anmeldeinformationen auf. 

- Für macOS-Apps muss der AppDelegate-Aufruf nicht implementiert werden.

**iOS**

- Es müssen weitere Schritte zur Einrichtung Ihres Projekts ausgeführt werden, damit der Ablauf zur Authentifizierung per Broker unterstützt wird. Die Schritte sind im Tutorial beschrieben.
- Für iOS-Projekte müssen in der Datei „info.plist“ benutzerdefinierte Schemas registriert werden. Unter macOS ist dies nicht erforderlich.
