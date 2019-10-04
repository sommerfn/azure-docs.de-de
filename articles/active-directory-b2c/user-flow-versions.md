---
title: Benutzerflowversionen in Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie, welche Versionen von Benutzerflows in Azure Active Directory B2C verfügbar sind.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: df411ee288b9759adbb3f8f84c28e4fed05f3c4c
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71258092"
---
# <a name="user-flow-versions-in-azure-active-directory-b2c"></a>Benutzerflowversionen in Azure Active Directory B2C

Benutzerflows in Azure Active Directory B2C (Azure AD B2C) helfen Ihnen beim Einrichten allgemeiner [Richtlinien](active-directory-b2c-reference-policies.md), die Benutzeroberflächen für Kundenidentitäten vollständig beschreiben. Diese Benutzeroberflächen umfassen Registrierung, Anmeldung und Profilbearbeitung. In Azure AD B2C können Sie aus einer Sammlung von empfohlenen und in der Vorschau befindlichen Benutzerflows auswählen.

Neue Benutzerflows werden als neue Versionen hinzugefügt. Sobald Benutzerflows stabil sind, werden sie zur Verwendung empfohlen. Benutzerflows werden als **empfohlen** gekennzeichnet, wenn sie ausführlich getestet wurden. Benutzerflows gelten als Vorschauversion, bis sie als empfohlen gekennzeichnet werden. Verwenden Sie für Produktionsanwendungen die empfohlenen Benutzerflows. Mithilfe der anderen Versionen können Sie neue Funktionen testen. Sie sollten keine älteren Versionen von empfohlenen Benutzerflows verwenden.

>[!IMPORTANT]
> Sofern ein Benutzerflow nicht als **Empfohlen** definiert ist, gilt er als *in Vorschau*. Sie sollten für Produktionsanwendungen nur empfohlene Benutzerflows verwenden.

## <a name="v1"></a>V1

| Benutzerflow | Empfohlen | BESCHREIBUNG |
| --------- | ----------- | ----------- |
| Zurücksetzen des Kennworts | Ja | Ermöglicht einem Benutzer nach Überprüfung der E-Mail-Adresse die Auswahl eines neuen Kennworts. Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Mehrstufige Authentifizierung](active-directory-b2c-reference-mfa.md)</li><li>Tokenkompatibilitätseinstellungen</li><li>[Anforderungen an die Komplexität von Kennwörtern](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Profilbearbeitung | Ja | Ermöglicht dem Benutzer die Konfiguration seiner Benutzerattribute. Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Tokengültigkeitsdauer](active-directory-b2c-reference-tokens.md)</li><li>Tokenkompatibilitätseinstellungen</li><li>Sitzungsverhalten</li></ul> |
| Mit ROPC anmelden | Nein | Ermöglicht einem Benutzer mit einem lokalen Konto die Anmeldung direkt in nativen Anwendungen (kein Browser erforderlich). Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Tokengültigkeitsdauer](active-directory-b2c-reference-tokens.md)</li><li>Tokenkompatibilitätseinstellungen</li></ul> |
| Anmelden | Nein | Ermöglicht einem Benutzer die Anmeldung bei seinem Konto. Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Mehrstufige Authentifizierung](active-directory-b2c-reference-mfa.md)</li><li>[Tokengültigkeitsdauer](active-directory-b2c-reference-tokens.md)</li><li>Tokenkompatibilitätseinstellungen</li><li>Sitzungsverhalten</li><li>Anmeldung sperren</li><li>Kennwortzurücksetzung erzwingen</li><li>Angemeldet bleiben</ul><br>Sie können die Benutzeroberfläche mit diesem Benutzerflow nicht anpassen. |
| Registrieren | Nein | Ermöglicht einem Benutzer, ein Konto zu erstellen. Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Mehrstufige Authentifizierung](active-directory-b2c-reference-mfa.md)</li><li>[Tokengültigkeitsdauer](active-directory-b2c-reference-tokens.md)</li><li>Tokenkompatibilitätseinstellungen</li><li>Sitzungsverhalten</li><li>[Anforderungen an die Komplexität von Kennwörtern](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Registrieren und Anmelden | Ja | Ermöglicht einem Benutzer, ein Konto zu erstellen oder sich bei seinem Konto anzumelden. Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Mehrstufige Authentifizierung](active-directory-b2c-reference-mfa.md)</li><li>[Tokengültigkeitsdauer](active-directory-b2c-reference-tokens.md)</li><li>Tokenkompatibilitätseinstellungen</li><li>Sitzungsverhalten</li><li>[Anforderungen an die Komplexität von Kennwörtern](active-directory-b2c-reference-password-complexity.md)</li></ul>|

## <a name="v11"></a>V1.1

| Benutzerflow | Empfohlen | BESCHREIBUNG |
| --------- | ----------- | ----------- |
| Kennwortzurücksetzung v1.1 | Nein | Ermöglicht einem Benutzer nach Überprüfung seiner E-Mail-Adresse die Auswahl eines neuen Kennworts (neues Seitenlayout verfügbar). Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Mehrstufige Authentifizierung](active-directory-b2c-reference-mfa.md)</li><li>Tokenkompatibilitätseinstellungen</li><li>[Anforderungen an die Komplexität von Kennwörtern](active-directory-b2c-reference-password-complexity.md)</li></ul> |

## <a name="v2"></a>V2

| Benutzerflow | Empfohlen | BESCHREIBUNG |
| --------- | ----------- | ----------- |
| Kennwortzurücksetzung v2 | Nein | Ermöglicht einem Benutzer nach Überprüfung der E-Mail-Adresse die Auswahl eines neuen Kennworts. Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Mehrstufige Authentifizierung](active-directory-b2c-reference-mfa.md)</li><li>Tokenkompatibilitätseinstellungen</li><li>[Altersbeschränkung](basic-age-gating.md)</li><li>[Anforderungen an die Komplexität von Kennwörtern](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Profilbearbeitung v2 | Ja | Ermöglicht dem Benutzer die Konfiguration seiner Benutzerattribute. Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Tokengültigkeitsdauer](active-directory-b2c-reference-tokens.md)</li><li>Tokenkompatibilitätseinstellungen</li><li>Sitzungsverhalten</li></ul> |
| Anmeldung v2 | Nein | Ermöglicht einem Benutzer die Anmeldung bei seinem Konto. Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Mehrstufige Authentifizierung](active-directory-b2c-reference-mfa.md)</li><li>[Tokengültigkeitsdauer](active-directory-b2c-reference-tokens.md)</li><li>Tokenkompatibilitätseinstellungen</li><li>Sitzungsverhalten</li><li>[Altersbeschränkung](basic-age-gating.md)</li><li>Anpassung der Anmeldeseite</li></ul> |
| Registrierung v2 | Nein | Ermöglicht einem Benutzer, ein Konto zu erstellen. Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Mehrstufige Authentifizierung](active-directory-b2c-reference-mfa.md)</li><li>[Tokengültigkeitsdauer](active-directory-b2c-reference-tokens.md)</li><li>Tokenkompatibilitätseinstellungen</li><li>Sitzungsverhalten</li><li>[Altersbeschränkung](basic-age-gating.md)</li><li>[Anforderungen an die Komplexität von Kennwörtern](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Registrierung und Anmeldung v2 | Nein | Ermöglicht einem Benutzer, ein Konto zu erstellen oder sich bei seinem Konto anzumelden. Mithilfe dieses Benutzerflows können Sie Folgendes konfigurieren: <ul><li>[Mehrstufige Authentifizierung](active-directory-b2c-reference-mfa.md)</li><li>[Altersbeschränkung](basic-age-gating.md)</li><li>[Anforderungen an die Komplexität von Kennwörtern](active-directory-b2c-reference-password-complexity.md)</li></ul> |
