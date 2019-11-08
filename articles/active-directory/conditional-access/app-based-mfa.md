---
title: 'Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory | Microsoft-Dokumentation'
description: In diesem Schnellstart erfahren Sie, wie Sie Ihre Authentifizierungsanforderungen über den bedingten Zugriff von Azure Active Directory (Azure AD) an den Typ der Cloud-App binden, auf die zugegriffen wird.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 01/30/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7a23f8fc10e0e5a19be1f93cc6d6e5e8e301f86d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73474026"
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory

Um den Anmeldevorgang für Ihre Benutzer einfacher zu gestalten, möchten Sie ihnen vielleicht die Anmeldung bei Cloud-Apps über Benutzername und Kennwort gestatten. Allerdings gibt es in vielen Umgebungen mindestens ein paar Apps, bei denen es ratsam ist, eine stärkere Form der Kontoüberprüfung (z. B. die mehrstufige Authentifizierung) vorzuschreiben. Diese Richtlinie kann zum Beispiel beim Zugriff auf das E-Mail-System Ihres Unternehmens oder die Apps der Personalabteilung gelten. In Azure Active Directory (Azure AD) können Sie dieses Ziel mit einer Richtlinie für bedingten Zugriff erreichen.

Dieser Schnellstart veranschaulicht das Konfigurieren einer [Azure AD-Richtlinie für bedingten Zugriff](../active-directory-conditional-access-azure-portal.md), durch die für eine ausgewählte Cloud-App in Ihrer Umgebung die mehrstufige Authentifizierung erforderlich wird.

![Beispiel für eine Richtlinie für bedingten Zugriff im Azure-Portal](./media/app-based-mfa/32.png)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung des Szenarios im Rahmen dieses Schnellstarts benötigen Sie Folgendes:

- **Zugriff auf eine Azure AD Premium-Edition**: Der bedingte Zugriff von Azure AD ist eine Funktion, die in Azure AD Premium enthalten ist.
- **Ein Testkonto unter dem Namen Isabella Simonsen**: Wenn Sie nicht wissen, wie Sie ein Testkonto erstellen, lesen Sie [Hinzufügen von cloudbasierten Benutzern](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

Das Szenario in diesem Schnellstart setzt voraus, dass die MFA pro Benutzer für Ihr Testkonto nicht aktiviert ist. Weitere Informationen finden Sie unter [Vorgehensweise zum Erzwingen einer zweistufigen Überprüfung für einen Benutzer](../authentication/howto-mfa-userstates.md).

## <a name="test-your-experience"></a>Testen des Vorgangs

Das Ziel dieses Schritts besteht darin, den Vorgang ohne eine Richtlinie für bedingten Zugriff zu überprüfen.

**So initialisieren Sie Ihre Umgebung:**

1. Melden Sie sich bei Ihrem Azure-Portal als Isabella Simonsen an.
1. Melden Sie sich ab.

## <a name="create-your-conditional-access-policy"></a>Erstellen Ihrer Richtlinie für bedingten Zugriff

In diesem Abschnitt wird gezeigt, wie Sie die erforderliche Richtlinie für bedingten Zugriff erstellen. Für das Szenario in diesem Schnellstart wird Folgendes verwendet:

- Azure-Portal als Platzhalter für eine Cloud-App, für welche die mehrstufige Authentifizierung erforderlich ist 
- Beispielbenutzer zum Testen der Richtlinie für bedingten Zugriff  

Legen Sie in Ihrer Richtlinie Folgendes fest:

| Einstellung | Wert |
| --- | --- |
| Benutzer und Gruppen | Isabella Simonsen |
| Cloud-Apps | Microsoft Azure Management |
| Gewähren von Zugriff | Mehrstufige Authentifizierung erforderlich |

![Erweiterte Richtlinie für bedingten Zugriff](./media/app-based-mfa/31.png)

**So konfigurieren Sie die Richtlinie für bedingten Zugriff:**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Suchen Sie im Azure-Portal nach **Azure Active Directory**, und wählen Sie es aus.

   ![Azure Active Directory](./media/app-based-mfa/02.png)

1. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Sicherheit** auf **Bedingter Zugriff**.

   ![Bedingter Zugriff](./media/app-based-mfa/03.png)

1. Klicken Sie auf der Seite **Bedingter Zugriff** auf der Symbolleiste am oberen Rand auf **Neue Richtlinie**.

   ![Hinzufügen](./media/app-based-mfa/04.png)

1. Geben Sie auf der Seite **Neu** im Textfeld **Name** die Textfolge **Mehrstufige Authentifizierung für Zugriff auf Azure-Portal erforderlich** ein.

   ![NAME](./media/app-based-mfa/05.png)

1. Klicken Sie im Abschnitt **Zuweisung** auf **Benutzer und Gruppen**.

   ![Benutzer und Gruppen](./media/app-based-mfa/06.png)

1. Führen Sie auf der Seite **Benutzer und Gruppen** die folgenden Schritte aus:

   ![Benutzer und Gruppen](./media/app-based-mfa/24.png)

   1. Klicken Sie auf **Benutzer und Gruppen auswählen**, und wählen Sie dann **Benutzer und Gruppen** aus.
   1. Klicken Sie auf **Auswählen**.
   1. Wählen Sie auf der Seite **Auswählen** den Benutzer **Isabella Simonsen** aus, und klicken Sie dann auf **Auswählen**.
   1. Klicken Sie auf der Seite **Benutzer und Gruppen** auf **Fertig**.

1. Klicken Sie auf **Cloud-Apps**.

   ![Cloud-Apps](./media/app-based-mfa/08.png)

1. Führen Sie auf der Seite **Cloud-Apps** die folgenden Schritte aus:

   ![Auswählen der Cloud-Apps](./media/app-based-mfa/26.png)

   1. Klicken Sie auf **Apps auswählen**.
   1. Klicken Sie auf **Auswählen**.
   1. Wählen Sie auf der Seite **Auswählen** die Option **Microsoft Azure Management** aus, und klicken Sie dann auf **Auswählen**.
   1. Klicken Sie auf der Seite **Cloud-Apps** auf **Fertig**.

1. Klicken Sie im Abschnitt **Zugriffssteuerungen** auf **Gewähren**.

   ![Zugriffssteuerung](./media/app-based-mfa/10.png)

1. Führen Sie auf der Seite **Gewährung** die folgenden Schritte aus:

   ![Gewährung](./media/app-based-mfa/11.png)

   1. Wählen Sie **Zugriff gewähren** aus.
   1. Wählen Sie **Erfordern von Multi-Factor Authentication**.
   1. Klicken Sie auf **Auswählen**.

1. Klicken Sie im Abschnitt **Richtlinie aktivieren** auf **Ein**.

   ![Richtlinie aktivieren](./media/app-based-mfa/18.png)

1. Klicken Sie auf **Create**.

## <a name="evaluate-a-simulated-sign-in"></a>Auswerten einer simulierten Anmeldung

Nachdem Sie nun Ihre Richtlinie für bedingten Zugriff konfiguriert haben, möchten Sie wahrscheinlich wissen, ob sie erwartungsgemäß funktioniert. Verwenden Sie im ersten Schritt das What-If-Richtlinientool des bedingten Zugriffs, um eine Anmeldung für Ihren Testbenutzer zu simulieren. Die Simulation schätzt die Auswirkungen dieser Anmeldungen auf Ihre Richtlinien ab und generiert einen Simulationsbericht.  

Legen Sie zum Initialisieren des **What If**-Richtlinienauswertungstools Folgendes fest:

- Benutzer: **Isabella Simonsen**
- Cloud-App: **Microsoft Azure Management**

Durch Klicken auf **What If** wird ein Simulationsbericht mit folgenden Informationen erstellt:

- **Mehrstufige Authentifizierung für Zugriff auf Azure-Portal erforderlich** unter **Anzuwendende Richtlinien**
- **Mehrstufige Authentifizierung erforderlich** unter **Gewährungssteuerelemente**

![What If-Richtlinientool](./media/app-based-mfa/23.png)

**So werten Sie die Richtlinie für bedingten Zugriff aus:**

1. Klicken Sie auf der Seite [Bedingter Zugriff – Richtlinien](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) im Menü am oberen Rand auf **What If**.  

   ![What If](./media/app-based-mfa/14.png)

1. Klicken Sie auf **Benutzer**, wählen Sie **Isabella Simonsen** aus, und klicken Sie dann auf **Auswählen**.

   ![Benutzer](./media/app-based-mfa/15.png)

1. Um eine Cloud-App auszuwählen, führen Sie die folgenden Schritte aus:

   ![Cloud-Apps](./media/app-based-mfa/16.png)

   1. Klicken Sie auf **Cloud-Apps**.
   1. Klicken Sie auf der Seite **Cloud-Apps** auf **Apps auswählen**.
   1. Klicken Sie auf **Auswählen**.
   1. Wählen Sie auf der Seite **Auswählen** die Option **Microsoft Azure Management** aus, und klicken Sie dann auf **Auswählen**.
   1. Klicken Sie auf der Seite „Cloud-Apps“ auf **Fertig**.

1. Klicken Sie auf **What If**.

## <a name="test-your-conditional-access-policy"></a>Testen der Richtlinie für bedingten Zugriff

Im vorherigen Abschnitt haben Sie gelernt, wie Sie eine simulierte Anmeldung auswerten. Zusätzlich zu einer Simulation sollten Sie auch Ihre Richtlinie für bedingten Zugriff testen, um sicherzustellen, dass sie wie erwartet funktioniert.

Um Ihre Richtlinie zu testen, versuchen Sie, sich im [Azure-Portal](https://portal.azure.com) mit dem Testkonto **Isabella Simonsen** anzumelden. Es sollte ein Dialogfeld angezeigt werden, in dem Sie Ihr Konto für eine zusätzliche Sicherheitsüberprüfung einrichten müssen.

![Multi-Factor Authentication](./media/app-based-mfa/22.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie den Testbenutzer, wenn er nicht mehr benötigt wird, und löschen Sie auch die Richtlinie für bedingten Zugriff:

- Wenn Sie nicht wissen, wie ein Azure AD-Benutzer gelöscht wird, lesen Sie [Löschen von Benutzern aus Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- Zum Löschen Ihrer Richtlinie wählen Sie die Richtlinie aus, und klicken Sie dann in der Symbolleiste für den Schnellzugriff auf **Löschen**.

    ![Multi-Factor Authentication](./media/app-based-mfa/33.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Festlegen, dass die Nutzungsbedingungen akzeptiert werden müssen](require-tou.md)
> [Blockieren des Zugriffs bei erkanntem Sitzungsrisiko](app-sign-in-risk.md)
