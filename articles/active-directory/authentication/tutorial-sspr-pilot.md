---
title: Aktivieren der Azure AD-SSPR für eine Pilotgruppe
description: In diesem Tutorial aktivieren Sie die Azure AD-Self-Service-Kennwortzurücksetzung für eine Pilotgruppe von Benutzern.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 08/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 685c928a20e9bfb47365b5ffa769f1a223d2c563
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69561502"
---
# <a name="tutorial-complete-an-azure-ad-self-service-password-reset-pilot-roll-out"></a>Tutorial: Ausführen eines Rollouts der Azure AD-Self-Service-Kennwortzurücksetzung für eine Pilotgruppe

In diesem Tutorial aktivieren Sie das Rollout der Azure AD-Self-Service-Kennwortzurücksetzung (Self-Service Password Reset, SSPR) für eine Pilotgruppe in Ihrer Organisation und führen Tests mithilfe eines Kontos ohne Administratorrechte aus.

Alle Tests der Self-Service-Kennwortzurücksetzung müssen mit Konten ohne Administratorrechte ausgeführt werden. Microsoft verwaltet die Kennwortzurücksetzungsrichtlinie für Administratorkonten und erfordert die Verwendung von sichereren Authentifizierungsmethoden. Mit dieser Richtlinie können keine Sicherheitsfragen und -antworten verwendet werden, und zum Zurücksetzen sind zwei Methoden erforderlich.

> [!div class="checklist"]
> * Aktivieren der Self-Service-Kennwortzurücksetzung
> * Testen von SSPR als Benutzer

## <a name="prerequisites"></a>Voraussetzungen

* Ein Konto eines globalen Administrators

## <a name="enable-self-service-password-reset"></a>Aktivieren der Self-Service-Kennwortzurücksetzung

1. Melden Sie sich mit dem Konto eines globalen Administrators am [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Azure Active Directory**, und klicken Sie auf **Kennwortzurücksetzung**.
1. Beginnen Sie mit einer Pilotgruppe, indem Sie das Self-Service-Kennwort für eine Teilmenge von Benutzern in Ihrer Organisation aktivieren.
   * Wählen Sie auf der Seite **Eigenschaften** unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Aktiviert**, und wählen Sie eine Pilotgruppe aus.
      * Nur Mitglieder der von Ihnen ausgewählten Azure AD-Gruppe können die SSPR-Funktionalität nutzen. Es wird empfohlen, eine Gruppe mit Benutzern zu definieren und diese Einstellung zu verwenden, wenn Sie diese Funktionalität für einen Proof of Concept-Vorgang bereitstellen. Die Schachtelung von Sicherheitsgruppen wird hier unterstützt.
      * Stellen Sie sicher, dass die Benutzer in der von Ihnen ausgewählten Gruppe eine entsprechende Lizenz besitzen.
   * Klicken Sie unten auf der Seite auf **Speichern**.
1. Auf der Seite **Authentifizierungsmethoden**
   * Legen Sie die Option **Anzahl von Methoden, die zurückgesetzt werden müssen** auf **1** fest.
   * Legen Sie fest, welche **für Benutzer verfügbare Methoden** Ihre Organisation zulassen möchte. Aktivieren Sie für dieses Tutorial die Kontrollkästchen zum Aktivieren von **E-Mail**, **Mobiltelefon**, **Bürotelefon**, **Benachrichtigung über mobile App** und **Code der mobilen App**.
   * Klicken Sie unten auf der Seite auf **Speichern**.
1. Auf der Seite **Registrierung**
   * Wählen Sie für **Registrierung von Benutzern bei der Anmeldung verlangen** die Option **Ja** aus.
   * Legen Sie **Anzahl der Tage, bevor Benutzer aufgefordert werden, ihre Authentifizierungsinformationen erneut zu bestätigen** auf **180** fest.
   * Klicken Sie unten auf der Seite auf **Speichern**.
1. Auf der Seite **Benachrichtigungen**
   * Legen Sie **Benachrichtigen von Benutzern über Kennwortzurücksetzungen** auf **Ja** fest.
   * Legen Sie **Benachrichtigen aller Administratoren, wenn andere Administratoren ihr Kennwort zurücksetzen** auf **Ja** fest.
1. Auf der Seite **Anpassung**
   * Microsoft empfiehlt, **Benutzerdefinierter Helpdesklink** auf **Ja** festzulegen und im Feld **Benutzerdefinierte Helpdesk-E-Mail oder -URL** entweder eine E-Mail-Adresse oder eine Webseiten-URL anzugeben, unter der Benutzer weitere Unterstützung von Ihrer Organisation anfordern können.
   * In diesem Tutorial bleibt **Benutzerdefinierter Helpdesklink** auf **Nein** festgelegt.

Die Self-Service-Kennwortzurücksetzung ist nun für Cloudbenutzer in Ihrer Pilotgruppe konfiguriert.

## <a name="test-sspr-as-a-user"></a>Testen von SSPR als Benutzer

Testen Sie die Self-Service-Kennwortzurücksetzung mit einem Testbenutzer ohne Administratorrechte, der Mitglied der Pilotgruppe ist. **Bei Verwendung eines Kontos mit zugewiesenen Administratorrollen können die Authentifizierungsmethoden und die Anzahl unter Umständen von den ausgewählten Werten abweichen, da die Administratorrichtlinie von Microsoft verwaltet wird.**

1. Öffnen Sie ein neues Browserfenster im InPrivate- oder Inkognitomodus.
1. Verwenden Sie eine Testbenutzerregistrierung für die Self-Service-Kennwortzurücksetzung über das Registrierungsportal unter [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup).
1. Navigieren Sie mit dem gleichen Testbenutzer zum Portal für die Self-Service-Kennwortzurücksetzung ([https://aka.ms/sspr](https://aka.ms/sspr)), und versuchen Sie, Ihr Kennwort mithilfe der im vorherigen Schritt angegebenen Informationen zurückzusetzen.
1. Ihr Kennwort sollte zurückgesetzt werden.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die im Rahmen dieses Tutorials konfigurierte Funktionalität nicht mehr nutzen möchten, gehen Sie wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Azure Active Directory**, und klicken Sie auf **Kennwortzurücksetzung**.
1. Wählen Sie auf der Seite **Eigenschaften** unter **Self-Service-Kennwortzurücksetzung aktiviert** die Option **Keine**.
1. Klicken Sie unten auf der Seite auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die Azure AD-Self-Service-Kennwortzurücksetzung aktiviert. Im nächsten Tutorial erfahren Sie, wie Sie eine lokale Active Directory Domain Services-Infrastruktur in die Umgebung mit Self-Service-Kennwortzurücksetzung integrieren.

> [!div class="nextstepaction"]
> [Tutorial: Enabling password writeback](tutorial-enable-writeback.md) (Tutorial: Aktivieren des Kennwortrückschreibens)
