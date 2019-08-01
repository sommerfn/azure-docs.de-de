---
title: Konfigurieren des Sitzungsverhaltens – Azure Active Directory B2C | Microsoft-Dokumentation
description: Konfigurieren des Sitzungsverhaltens in Azure Active Directory B2C
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: b18a7d98654422951773c0a5497f69db93782f51
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849451"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Konfigurieren des Sitzungsverhaltens in Azure Active Directory B2C

Diese Funktion ermöglicht Ihnen eine präzisere Steuerung der folgenden Einstellungen auf [Basis eines Benutzerflows](active-directory-b2c-reference-policies.md):

- Lebensdauer von Webanwendungssitzungen, die von Azure AD B2C verwaltet werden.
- SSO-Verhalten über verschiedene Apps und Benutzerflows in Ihrem Azure AD B2C-Mandanten hinweg.

Diese Einstellungen sind für Benutzerflows zur Kennwortzurücksetzung nicht verfügbar.

Azure AD B2C unterstützt das [OpenID Connect-Authentifizierungsprotokoll](active-directory-b2c-reference-oidc.md) zum Aktivieren der sicheren Anmeldung bei Webanwendungen. Sie können die folgenden Eigenschaften zum Verwalten von Webanwendungssitzungen verwenden:

## <a name="session-behavior-properties"></a>Eigenschaften des Sitzungsverhaltens

- **Lebensdauer der Web-App-Sitzung (Minuten)** : Die Gültigkeitsdauer von Azure AD B2C-Sitzungscookies, die nach erfolgreicher Authentifizierung des Benutzers im Browser gespeichert werden
    - Standardwert: 1.440 Minuten.
    - Minimum (inklusive): 15 Minuten.
    - Maximum (inklusive): 1.440 Minuten.
- **Timeout für Web-App-Sitzung**: Wenn diese Option auf **Absolut** festgelegt ist, muss der Benutzer sich nach dem in **Lebensdauer der Web-App-Sitzung (Minuten)** angegebenen Zeitraum erneut authentifizieren. Wenn diese Option auf **Rollen** festgelegt ist (Standardeinstellung), bleibt der Benutzer angemeldet, solange er ständig in Ihrer Webanwendung aktiv ist.
- **Konfiguration des einmaligen Anmeldens** Wenn Sie in Ihrem B2C-Mandanten über mehrere Anwendungen und Benutzerflows verfügen, können Sie die Interaktionen der Benutzer mithilfe der Eigenschaft **Konfiguration des einmaligen Anmeldens** übergreifend verwalten. Sie können die Eigenschaft auf eine der folgenden Optionen festlegen:
    - **Mandant**: Dies ist die Standardeinstellung. Mit dieser Einstellung können mehrere Anwendungen und Benutzerflows in Ihrem B2C-Mandanten die gleiche Benutzersitzung gemeinsam nutzen. Beispiel: Sobald sich ein Benutzer bei einer Anwendung angemeldet hat, erfolgt die Anmeldung bei einer anderen Anwendung (etwa Contoso Pharmacy) nahtlos beim Zugriff auf die Anwendung.
    - **Anwendung**: Mit dieser Einstellung können Sie eine Benutzersitzung ausschließlich für eine Anwendung beibehalten, unabhängig von anderen Anwendungen. Verwenden Sie diese Einstellung beispielsweise, wenn Sie möchten, dass sich der Benutzer (mit denselben Anmeldeinformationen) bei Contoso Pharmacy anmeldet – auch wenn der Benutzer bereits bei Contoso Shopping angemeldet ist, einer anderen Anwendung im selben B2C-Mandanten.
    - **Richtlinie**: Mit dieser Einstellung können Sie eine Benutzersitzung ausschließlich für einen Benutzerflow beibehalten, unabhängig von den Anwendungen, die diesen verwenden. Wenn der Benutzer sich beispielsweise bereits angemeldet und einen Multi-Factor Authentication-Schritt (MFA) abgeschlossen hat, kann er in mehreren Anwendungen Zugriff auf Komponenten mit höherer Sicherheit erhalten, solange die an den Benutzerflow gebundene Sitzung nicht abläuft.
    - **Deaktiviert**: Diese Einstellung erzwingt, dass der Benutzer bei jeder Ausführung der Richtlinie den gesamten Benutzerflow durchlaufen muss.

Mit diesen Eigenschaften werden die folgenden Anwendungsfälle ermöglicht:

- Erfüllen Sie die Sicherheits- und Complianceanforderungen Ihrer Branche durch Festlegung der entsprechenden Lebensdauer für Webanwendungssitzungen.
- Erzwingen Sie die Authentifizierung nach einem festgelegten Zeitraum während der Interaktion eines Benutzers mit einer Komponente Ihrer Webanwendung, die eine hohe Sicherheit erfordert.

## <a name="configure-the-properties"></a>Konfigurieren der Eigenschaften

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen, das Ihren Azure AD B2C-Mandanten enthält.
3. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
4. Wählen Sie **Benutzerflows (Richtlinien)** aus.
5. Öffnen Sie den Benutzerflow, den Sie zuvor erstellt haben.
6. Wählen Sie **Eigenschaften** aus.
7. Konfigurieren Sie **Lebensdauer der Web-App-Sitzung (Minuten)** , **Timeout für Web-App-Sitzung**, **Konfiguration des einmaligen Anmeldens** und **ID-Token in Abmeldeanforderungen erforderlich** nach Bedarf.

    ![Eigenschafteneinstellungen für das Sitzungsverhalten im Azure-Portal](./media/session-behavior/session-behavior.png)

8. Klicken Sie auf **Speichern**.
