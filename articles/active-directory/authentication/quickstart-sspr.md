---
title: 'Schnellstart: Self-Service-Kennwortzurücksetzung in Azure AD'
description: In dieser Schnellstartanleitung führen Sie eine schnelle Konfiguration der Self-Service-Kennwortzurücksetzung in Azure AD durch, damit Benutzer ihre eigenen Kennwörter zurücksetzen können.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: quickstart
ms.date: 07/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9adf083f8e4c8c269b895dac87b4e4613ec1b023
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73474379"
---
# <a name="quickstart-self-service-password-reset"></a>Schnellstart: Self-Service-Kennwortzurücksetzung

In dieser Schnellstartanleitung wird Schritt für Schritt die Konfiguration der Self-Service-Kennwortzurücksetzung (SSPR) als einfaches Mittel für IT-Administratoren beschrieben, um für Benutzer das Zurücksetzen ihrer Kennwörter oder das Entsperren ihrer Konten zu ermöglichen.

## <a name="prerequisites"></a>Voraussetzungen

* Ein funktionierender Azure AD-Mandant mit mindestens einer aktivierten Testlizenz.
* Ein Konto mit Berechtigungen vom Typ „Globaler Administrator“.
* Ein Testbenutzer ohne Administratorrechte mit einem Kennwort, das Ihnen bekannt ist. Wenn Sie einen Benutzer erstellen müssen, helfen Ihnen die Informationen unter [Schnellstart: Hinzufügen neuer Benutzer in Azure Active Directory](../add-users-azure-active-directory.md) weiter.
* Eine Pilotgruppe zu Testzwecken, deren Mitglied der Benutzer ist. Wenn Sie eine Gruppe erstellen müssen, helfen Ihnen die Informationen im Artikel [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](../active-directory-groups-create-azure-portal.md) weiter.

## <a name="enable-self-service-password-reset"></a>Aktivieren der Self-Service-Kennwortzurücksetzung

[Diesen Prozess als Video auf YouTube anzeigen](https://youtu.be/Pa0eyqjEjvQ)

1. Wählen Sie im Menü des Azure-Portals oder auf der **Startseite** für Ihren vorhandenen Azure AD-Mandanten die Option **Azure Active Directory** aus. Wählen Sie anschließend **Kennwort zurücksetzen** aus.

2. Wählen Sie auf der Seite **Eigenschaften** unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Aktiviert**.
    * Wählen Sie unter **Gruppe auswählen** Ihre Pilotgruppe aus, die Sie im Abschnitt „Voraussetzungen“ dieses Artikels erstellt haben.
    * Klicken Sie auf **Speichern**.

3. Wählen Sie auf der Seite **Authentifizierungsmethoden** Folgendes aus:
   * Anzahl von erforderlichen Methoden zum Zurücksetzen: **1**
   * Für Benutzer verfügbare Methoden:
      * **E-Mail**
      * **Code in der mobilen App (Vorschauversion)**
   * Klicken Sie auf **Speichern**.

     ![Auswählen der Authentifizierungsmethoden für SSPR][Authentication]

4. Wählen Sie auf der Seite **Registrierung** Folgendes aus:
   * Erzwingen der Registrierung für Benutzer bei der Anmeldung: **Ja**
   * Festlegen der Anzahl von Tagen, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen: **365**

## <a name="test-self-service-password-reset"></a>Testen der Self-Service-Kennwortzurücksetzung

Jetzt testen wir Ihre SSPR-Konfiguration mit einem Testbenutzer. Da Microsoft für Azure-Administratorkonten strenge Anforderungen an die Authentifizierung stellt, kann das Testen mit einem Administratorkonto zu einem anderen Ergebnis führen. Weitere Informationen zur Richtlinie für Administratorkennwörter finden Sie im Artikel zu [Kennwortrichtlinien](concept-sspr-policy.md).

1. Öffnen Sie ein neues Browserfenster im InPrivate- oder Inkognitomodus, und navigieren Sie zu [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup).
2. Melden Sie sich als Testbenutzer ohne Administratorrechte an, und registrieren Sie Ihre Telefonnummer für die Authentifizierung.
3. Klicken Sie abschließend auf die Schaltfläche **Sieht gut aus**, und schließen Sie das Browserfenster.
4. Öffnen Sie ein neues Browserfenster im InPrivate- oder Inkognitomodus, und navigieren Sie zu [https://aka.ms/sspr](https://aka.ms/sspr).
5. Geben Sie die Benutzer-ID des Testbenutzers und die CAPTCHA-Zeichen ein, und klicken Sie anschließend auf **Weiter**.
6. Führen Sie die Schritte für die Verifizierung aus, um Ihr Kennwort zurückzusetzen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Das Deaktivieren der Self-Service-Kennwortzurücksetzung ist einfach. Öffnen Sie Ihren Azure AD-Mandanten, und navigieren Sie zu **Eigenschaften** > **Kennwortzurücksetzung**, und wählen Sie unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Keine**.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie gelernt, wie Sie die Self-Service-Kennwortzurücksetzung für Ihre reinen Cloudbenutzer schnell konfigurieren können. Fahren Sie mit dem Leitfaden für den Rollout fort, um sich darüber zu informieren, wie Sie einen ausführlicheren Rollout durchführen.

> [!div class="nextstepaction"]
> [Erfolgreicher Rollout der Self-Service-Kennwortzurücksetzung](howto-sspr-deployment.md)

[Authentication]: ./media/quickstart-sspr/sspr-authentication-methods.png "Verfügbare Azure AD-Authentifizierungsmethoden und erforderliche Menge"
