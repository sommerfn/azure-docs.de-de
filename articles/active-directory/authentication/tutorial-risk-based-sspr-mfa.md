---
title: Risikobasierte MFA und Self-Service-Kennwortzurücksetzung mit Azure Identity Protection
description: In diesem Tutorial aktivieren Sie Azure Identity Protection-Integrationen für die Multi-Factor Authentication und Self-Service-Kennwortzurücksetzung, um riskantes Verhalten zu reduzieren.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 01/31/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9289bfe9e90186896a753e5853d81d2f06669917
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70125175"
---
# <a name="tutorial-use-risk-detections-to-trigger-multi-factor-authentication-and-password-changes"></a>Tutorial: Verwenden von Risikoerkennungen zum Auslösen von mehrstufiger Authentifizierung und Kennwortänderungen

In diesem Tutorial aktivieren Sie Features für Azure Active Directory (Azure AD) Identity Protection, ein Azure AD Premium P2-Feature, bei dem es sich um mehr als ein einfaches Tool für die Überwachung und Berichterstellung handelt. Zum Schützen der Identitäten Ihrer Organisation können Sie risikobasierte Richtlinien konfigurieren, mit denen automatisch auf riskantes Verhalten reagiert wird. Mit diesen Richtlinien können entweder automatisch Blockaden erstellt oder Lösungen initiiert werden, z.B. das Erzwingen von Kennwortänderungen und Multi-Factor Authentication.

Azure AD Identity Protection-Richtlinien können neben den Richtlinien für bedingten Zugriff als zusätzliche Schutzschicht genutzt werden. Unter Umständen lösen Ihre Benutzer niemals ein riskantes Verhalten aus, für das eine dieser Richtlinien erforderlich ist, aber als Administrator wissen Sie, dass die Benutzer geschützt sind.

Beispiele für Vorkommnisse, die eine Risikoerkennung auslösen können, sind:

* Benutzer mit kompromittierten Anmeldeinformationen
* Anmeldungen von anonymen IP-Adressen
* Unmöglicher Ortswechsel zu atypischen Orten
* Anmeldungen von infizierten Geräten
* Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten
* Anmeldungen von unbekannten Standorten

Weitere Informationen zu Azure AD Identity Protection finden Sie im Artikel [Was ist Azure AD Identity Protection?](../active-directory-identityprotection.md).

> [!div class="checklist"]
> * Aktivieren der Azure MFA-Registrierung
> * Aktivieren von risikobasierten Kennwortänderungen
> * Aktivieren von risikobasierter Multi-Factor Authentication

## <a name="prerequisites"></a>Voraussetzungen

* Zugriff auf einen funktionierenden Azure AD-Mandanten mit mindestens einer zugewiesenen Azure AD Premium P2-Testlizenz.
* Eine Konto mit Rechten vom Typ „Globaler Administrator“ in Ihrem Azure AD-Mandanten.
* Die vorherigen Tutorials zur Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) und Multi-Factor Authentication (MFA) wurden abgeschlossen.

## <a name="enable-risk-based-policies-for-sspr-and-mfa"></a>Aktivieren von risikobasierten Richtlinien für SSPR und MFA

Die Aktivierung der risikobasierten Richtlinien ist ein einfacher Prozess. Unten wird eine Beispielkonfiguration Schritt für Schritt beschrieben.

### <a name="enable-users-to-register-for-multi-factor-authentication"></a>Aktivieren der Multi-Factor Authentication-Registrierung für Benutzer

Azure AD Identity Protection umfasst eine Standardrichtlinie, mit der sich Ihre Benutzer für die Multi-Factor Authentication registrieren können und der aktuelle Registrierungsstatus leicht ermittelt werden kann. Wenn Sie diese Richtlinie aktivieren, wird für Benutzer nicht sofort die Durchführung der Multi-Factor Authentication erzwungen, sondern sie werden aufgefordert, sich vorab zu registrieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Klicken Sie auf **Alle Dienste**, und navigieren Sie zu **Azure AD Identity Protection**.
1. Klicken Sie auf **MFA-Registrierung**.
1. Legen Sie „Richtlinie erzwingen“ auf **Ein** fest.
   1. Wenn Sie diese Richtlinie festlegen, müssen alle Benutzer Methoden registrieren, um die Verwendung der Multi-Factor Authentication vorzubereiten.
1. Klicken Sie auf **Speichern**.

   ![Festlegen, dass sich Benutzer bei der Anmeldung für MFA registrieren müssen](./media/tutorial-risk-based-sspr-mfa/risk-based-require-mfa-registration.png)

### <a name="enable-risk-based-password-changes"></a>Aktivieren von risikobasierten Kennwortänderungen

Microsoft arbeitet mit Ermittlern, Strafverfolgungsbehörden, verschiedenen Sicherheitsteams bei Microsoft und anderen vertrauenswürdigen Quellen zusammen, um Benutzername/Kennwort-Paare zu finden. Wenn eines dieser Paare eine Übereinstimmung mit einem Konto in Ihrer Umgebung aufweist, kann eine risikobasierte Kennwortänderung über die folgende Richtlinie ausgelöst werden.

1. Klicken Sie auf die Richtlinie zum Benutzerrisiko.
1. Wählen Sie unter **Bedingungen** die Option **Benutzerrisiko** und dann **Mittel und darüber**.
1. Klicken Sie auf „Auswählen“ und dann auf „Fertig“.
1. Wählen Sie unter **Zugriff** die Option **Zugriff zulassen** und anschließend **Kennwortänderung anfordern**.
1. Klicken Sie auf „Auswählen“.
1. Legen Sie „Richtlinie erzwingen“ auf **Ein** fest.
1. Klicken Sie unten auf der Seite auf **Speichern**.

### <a name="enable-risk-based-multi-factor-authentication"></a>Aktivieren von risikobasierter Multi-Factor Authentication

Die meisten Benutzer weisen ein normales Verhalten auf, das nachverfolgt werden kann. Wenn sie sich aber außerhalb dieser Norm bewegen, ist es ggf. riskant, ihnen das Anmelden ohne Weiteres zu erlauben. Es kann ratsam sein, den entsprechenden Benutzer zu blockieren oder ggf. einfach um die Durchführung eines Multi-Factor Authentication-Vorgangs zu bitten. So kann bewiesen werden, ob es sich auch wirklich um die Person handelt, die vorgegeben wird. Aktivieren Sie die folgende Richtlinie, um MFA zu erzwingen, wenn eine riskante Anmeldung erkannt wird.

1. Klicken Sie auf „Richtlinie zum Anmelderisiko“.
1. Wählen Sie unter **Bedingungen** die Option **Benutzerrisiko** und dann **Mittel und darüber**.
1. Klicken Sie auf „Auswählen“ und dann auf „Fertig“.
1. Wählen Sie unter **Zugriff** die Option **Zugriff zulassen** und dann **Multi-Factor Authentication erforderlich**.
1. Klicken Sie auf „Auswählen“.
1. Legen Sie „Richtlinie erzwingen“ auf **Ein** fest.
1. Klicken Sie unten auf der Seite auf **Speichern**.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie das Testen abgeschlossen haben und die Aktivierung der risikobasierten Richtlinien beenden möchten, können Sie wie folgt vorgehen: Navigieren Sie jeweils zu der Richtlinie, die Sie deaktivieren möchten, und legen Sie **Richtlinie erzwingen** auf **Aus** fest.
