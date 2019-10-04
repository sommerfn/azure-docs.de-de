---
title: Aktivieren eines Pilotprojekts für Azure Multi-Factor Authentication
description: In diesem Tutorial aktivieren Sie die Azure AD Multi-Factor Authentication für eine Pilotgruppe mit Benutzern.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: a229eef3d49964e71becf066d67f60b18da6fc18
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67113241"
---
# <a name="tutorial-complete-an-azure-multi-factor-authentication-pilot-roll-out"></a>Tutorial: Durchführen eines Rollouts für ein Azure Multi-Factor Authentication-Pilotprojekt

In diesem Tutorial wird das Konfigurieren einer Richtlinie für bedingten Zugriff Schritt für Schritt beschrieben, um Azure Multi-Factor Authentication (Azure MFA) für die Anmeldung beim Azure-Portal zu aktivieren. Die Richtlinie wird für eine bestimmte Gruppe mit Pilotbenutzern bereitgestellt und getestet. Die Bereitstellung von Azure MFA per bedingtem Zugriff bietet Organisationen und Administratoren im Vergleich zur herkömmlichen erzwungenen Methode deutlich mehr Flexibilität.

> [!div class="checklist"]
> * Aktivieren von Azure Multi-Factor Authentication
> * Testen der Azure Multi-Factor Authentication

## <a name="prerequisites"></a>Voraussetzungen

* Ein funktionierender Azure AD-Mandant mit mindestens einer aktivierten Testlizenz.
* Ein Konto mit Berechtigungen vom Typ „Globaler Administrator“.
* Ein Testbenutzer ohne Administratorrechte mit einem Kennwort, das Ihnen zu Testzwecken bekannt ist. Wenn Sie einen Benutzer erstellen müssen, helfen Ihnen die Informationen unter [Schnellstart: Hinzufügen neuer Benutzer in Azure Active Directory](../add-users-azure-active-directory.md) weiter.
* Eine Pilotgruppe für Testzwecke, deren Mitglied der Testbenutzer ist. Wenn Sie eine Gruppe erstellen müssen, helfen Ihnen die Informationen im Artikel [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](../active-directory-groups-create-azure-portal.md) weiter.

## <a name="enable-azure-multi-factor-authentication"></a>Aktivieren von Azure Multi-Factor Authentication

1. Melden Sie sich mit dem Konto eines globalen Administrators am [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Azure Active Directory**  > **Bedingter Zugriff**.
1. Wählen Sie **Neue Richtlinie**.
1. Geben Sie Ihrer Richtlinie den Namen **MFA Pilot**.
1. Wählen Sie unter **Benutzer und Gruppen** das Optionsfeld **Benutzer und Gruppen auswählen** aus.
    * Wählen Sie Ihre Pilotgruppe aus, die Sie im Abschnitt „Voraussetzungen“ dieses Artikels erstellt haben.
    * Klicken Sie auf **Fertig**.
1. Wählen Sie unter **Cloud-Apps** das Optionsfeld **Apps auswählen** aus.
    * Die Cloud-App für das Azure-Portal ist **Microsoft Azure Management**.
    * Klicken Sie auf **Auswählen**.
    * Klicken Sie auf **Fertig**.
1. Überspringen Sie den Abschnitt **Bedingungen**.
1. Stellen Sie unter **Gewähren** sicher, dass das Optionsfeld **Zugriff gewähren** ausgewählt ist.
    * Aktivieren Sie das Kontrollkästchen **Multi-Factor Authentication erforderlich**.
    * Klicken Sie auf **Auswählen**.
1. Überspringen Sie den Abschnitt **Sitzung**.
1. Legen Sie den Umschalter **Richtlinie aktivieren** auf **Ein** fest.
1. Klicken Sie auf **Erstellen**

## <a name="test-azure-multi-factor-authentication"></a>Testen der Azure Multi-Factor Authentication

Um zu prüfen, ob Ihre Richtlinie für bedingten Zugriff funktioniert, testen Sie das Anmelden an einer Ressource, für die MFA nicht erforderlich ist, und dann am Azure-Portal mit MFA.

1. Öffnen Sie ein neues Browserfenster im InPrivate- oder Inkognitomodus, und navigieren Sie zu [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com).
   * Melden Sie sich mit dem Testbenutzer an, den Sie im Abschnitt „Voraussetzungen“ dieses Artikels erstellt haben. Sie sollten nicht zur Durchführung des MFA-Vorgangs aufgefordert werden.
   * Schließen Sie das Browserfenster.
2. Öffnen Sie ein neues Browserfenster im InPrivate- oder Inkognitomodus, und navigieren Sie zu [https://portal.azure.com](https://portal.azure.com).
   * Melden Sie sich mit dem Testbenutzer an, den Sie im Abschnitt „Voraussetzungen“ dieses Artikels erstellt haben. Sie sollten jetzt aufgefordert werden, die Registrierung für Azure Multi-Factor Authentication durchzuführen und diese Art der Authentifizierung zu nutzen.
   * Schließen Sie das Browserfenster.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die im Rahmen dieses Tutorials konfigurierte Funktionalität nicht mehr nutzen möchten, gehen Sie wie folgt vor:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu **Azure Active Directory**  > **Bedingter Zugriff**.
1. Wählen Sie die Richtlinie für bedingten Zugriff aus, die Sie erstellt haben.
1. Klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie die Azure Multi-Factor Authentication aktiviert. Im nächsten Tutorial erfahren Sie, wie Azure Identity Protection in die Self-Service-Kennwortzurücksetzung und Multi-Factor Authentication integriert werden kann.

> [!div class="nextstepaction"]
> [Tutorial: Use risk events to trigger Multi-Factor Authentication and password changes](tutorial-risk-based-sspr-mfa.md) (Tutorial: Auslösen von Multi-Factor Authentication und Kennwortänderungen mithilfe von Risikoereignissen)
