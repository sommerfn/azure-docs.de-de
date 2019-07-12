---
title: Schnellstart – Fordern der Annahme von Nutzungsbedingungen vor dem Zugreifen auf Cloud-Apps, die durch bedingten Zugriff mit Azure Active Directory geschützt sind | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mit dem bedingten Zugriff mit Azure Active Directory erzwingen können, dass Ihre Nutzungsbedingungen akzeptiert werden, bevor der Zugriff auf ausgewählte Cloud-Apps gewährt wird.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 12/14/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: ba684209b497792cd2f520f6b530168959e62d7f
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67506911"
---
# <a name="quickstart-require-terms-of-use-to-be-accepted-before-accessing-cloud-apps"></a>Schnellstart: Fordern der Annahme von Nutzungsbedingungen vor dem Zugreifen auf Cloud-Apps

Bevor Sie den Zugriff auf bestimmte Cloud-Apps in Ihrer Umgebung gewähren, möchten Sie eventuell, dass Benutzer Ihre Nutzungsbedingungen akzeptieren. Der bedingte Zugriff mit Azure Active Directory (Azure AD) bietet Folgendes:

- Eine einfache Methode zum Konfigurieren der Nutzungsbedingungen
- Die Option zum Fordern der Annahme Ihrer Nutzungsbedingungen über eine Richtlinie für bedingten Zugriff  

Dieser Schnellstart veranschaulicht das Konfigurieren einer [Azure AD-Richtlinie für bedingten Zugriff](../active-directory-conditional-access-azure-portal.md), durch die für eine ausgewählte Cloud-App in Ihrer Umgebung erforderlich ist, dass Nutzungsbedingungen akzeptiert werden.

![Richtlinie erstellen](./media/require-tou/5555.png)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung des Szenarios im Rahmen dieses Schnellstarts benötigen Sie Folgendes:

- **Zugriff auf eine Azure AD Premium-Edition**: Der bedingte Zugriff von Azure AD ist eine Funktion, die in Azure AD Premium enthalten ist.
- **Ein Testkonto unter dem Namen Isabella Simonsen**: Wenn Sie nicht wissen, wie Sie ein Testkonto erstellen, lesen Sie [Hinzufügen von cloudbasierten Benutzern](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="test-your-sign-in"></a>Testen Ihrer Anmeldung

Das Ziel dieses Schritts besteht darin, den Anmeldevorgang ohne eine Richtlinie für bedingten Zugriff zu überprüfen.

**So testen Sie Ihre Anmeldung**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als Isabella Simonsen an.
1. Melden Sie sich ab.

## <a name="create-your-terms-of-use"></a>Erstellen Ihrer Nutzungsbedingungen

Dieser Abschnitt enthält die Schritte, die Sie zum Erstellen von Beispielnutzungsbedingungen ausführen müssen. Wenn Sie Nutzungsbedingungen erstellen, wählen Sie einen Wert für **Mit Richtlinienvorlagen für bedingten Zugriff erzwingen** aus. Nach dem Auswählen von **Benutzerdefinierte Richtlinie** wird das Dialogfeld zum Erstellen einer neuen Richtlinie für bedingten Zugriff geöffnet, sobald Ihre Nutzungsbedingungen erstellt wurden.

**So erstellen Sie Ihre Nutzungsbedingungen**

1. Erstellen Sie in Microsoft Word ein neues Dokument.
1. Geben Sie **Meine Nutzungsbedingungen** ein, und speichern Sie das Dokument unter **mytou.pdf** auf Ihrem Computer.
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator, Sicherheitsadministrator oder Administrator für bedingten Zugriff an.
1. Klicken Sie im Azure-Portal auf der linken Navigationsleiste auf **Azure Active Directory**.

   ![Azure Active Directory](./media/require-tou/02.png)

1. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Sicherheit** auf **Bedingter Zugriff**.

   ![Bedingter Zugriff](./media/require-tou/03.png)

1. Klicken Sie im Bereich **Verwalten** auf **Nutzungsbedingungen**.

   ![Nutzungsbedingungen](./media/require-tou/04.png)

1. Klicken Sie oben im Menü auf **Neue Bedingungen**.

   ![Nutzungsbedingungen](./media/require-tou/05.png)

1. Gehen Sie auf der Seite **Neue Nutzungsbedingungen** folgendermaßen vor:

   ![Nutzungsbedingungen](./media/require-tou/112.png)

   1. Geben Sie im Textfeld **Name** den Text **Meine Nutzungsbedingungen** ein.
   1. Geben Sie im Textfeld **Anzeigename** den Text **Meine Nutzungsbedingungen** ein.
   1. Laden Sie Ihre Nutzungsbedingungen als PDF-Datei hoch.
   1. Wählen Sie unter **Sprache** die Option **Englisch** aus.
   1. Wählen Sie für **Benutzer müssen die Nutzungsbedingungen erweitern** die Option **Ein** aus.
   1. Wählen Sie für **Mit Richtlinienvorlagen für bedingten Zugriff erzwingen** die Option **Benutzerdefinierte Richtlinie** aus.
   1. Klicken Sie auf **Create**.

## <a name="create-your-conditional-access-policy"></a>Erstellen Ihrer Richtlinie für bedingten Zugriff

In diesem Abschnitt wird gezeigt, wie Sie die erforderliche Richtlinie für bedingten Zugriff erstellen. Für das Szenario in diesem Schnellstart wird Folgendes verwendet:

- Azure-Portal als Platzhalter für eine Cloud-App, für die Ihre Nutzungsbedingungen akzeptiert werden müssen 
- Beispielbenutzer zum Testen der Richtlinie für bedingten Zugriff  

Legen Sie in Ihrer Richtlinie Folgendes fest:

| Einstellung | Wert |
| --- | --- |
| Benutzer und Gruppen | Isabella Simonsen |
| Cloud-Apps | Microsoft Azure Management |
| Gewähren von Zugriff | Meine Nutzungsbedingungen |

![Richtlinie erstellen](./media/require-tou/1234.png)

**So konfigurieren Sie die Richtlinie für bedingten Zugriff:**

1. Geben Sie auf der Seite **Neu** im Textfeld **Name** den Text **Nutzungsbedingungen für Isabella erforderlich** ein.

   ![NAME](./media/require-tou/71.png)

1. Klicken Sie im Abschnitt **Zuweisung** auf **Benutzer und Gruppen**.

   ![Benutzer und Gruppen](./media/require-tou/06.png)

1. Gehen Sie auf der Seite **Benutzer und Gruppen** folgendermaßen vor:

   ![Benutzer und Gruppen](./media/require-tou/24.png)

   1. Klicken Sie auf **Benutzer und Gruppen auswählen**, und wählen Sie dann **Benutzer und Gruppen** aus.
   1. Klicken Sie auf **Auswählen**.
   1. Wählen Sie auf der Seite **Auswählen** den Benutzer **Isabella Simonsen** aus, und klicken Sie dann auf **Auswählen**.
   1. Klicken Sie auf der Seite **Benutzer und Gruppen** auf **Fertig**.
1. Klicken Sie auf **Cloud-Apps**.

   ![Cloud-Apps](./media/require-tou/08.png)

1. Gehen Sie auf der Seite **Cloud-Apps** folgendermaßen vor:

   ![Auswählen der Cloud-Apps](./media/require-tou/26.png)

   1. Klicken Sie auf **Apps auswählen**.
   1. Klicken Sie auf **Auswählen**.
   1. Wählen Sie auf der Seite **Auswählen** die Option **Microsoft Azure Management** aus, und klicken Sie dann auf **Auswählen**.
   1. Klicken Sie auf der Seite **Cloud-Apps** auf **Fertig**.
1. Klicken Sie im Abschnitt **Zugriffssteuerungen** auf **Gewähren**.

   ![Zugriffssteuerung](./media/require-tou/10.png)

1. Gehen Sie auf der Seite **Gewähren** folgendermaßen vor:

   ![Gewährung](./media/require-tou/111.png)

   1. Wählen Sie **Zugriff gewähren** aus.
   1. Wählen Sie **Meine Nutzungsbedingungen** aus.
   1. Klicken Sie auf **Auswählen**.
1. Klicken Sie im Abschnitt **Richtlinie aktivieren** auf **Ein**.

   ![Richtlinie aktivieren](./media/require-tou/18.png)

1. Klicken Sie auf **Create**.

## <a name="evaluate-a-simulated-sign-in"></a>Auswerten einer simulierten Anmeldung

Nachdem Sie nun Ihre Richtlinie für bedingten Zugriff konfiguriert haben, möchten Sie wahrscheinlich wissen, ob sie erwartungsgemäß funktioniert. Verwenden Sie im ersten Schritt das What-If-Richtlinientool des bedingten Zugriffs, um eine Anmeldung für Ihren Testbenutzer zu simulieren. Die Simulation schätzt die Auswirkungen dieser Anmeldungen auf Ihre Richtlinien ab und generiert einen Simulationsbericht.  

Legen Sie zum Initialisieren des **What If**-Richtlinienauswertungstools Folgendes fest:

- Benutzer: **Isabella Simonsen**
- Cloud-App: **Microsoft Azure Management**

Durch Klicken auf **What If** wird ein Simulationsbericht mit folgenden Informationen erstellt:

- **Nutzungsbedingungen für Isabella erforderlich** unter **Anzuwendende Richtlinien**
- **Meine Nutzungsbedingungen** als **Steuerelemente zur Rechteerteilung**.

![What If-Richtlinientool](./media/require-tou/79.png)

**So werten Sie die Richtlinie für bedingten Zugriff aus:**

1. Klicken Sie auf der Seite [Bedingter Zugriff – Richtlinien](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) im Menü am oberen Rand auf **What If**.  

   ![What If](./media/require-tou/14.png)

1. Klicken Sie auf **Benutzer**, wählen Sie **Isabella Simonsen** aus, und klicken Sie dann auf **Auswählen**.

   ![Benutzer](./media/require-tou/15.png)

1. So wählen Sie eine Cloud-App aus

   ![Cloud-Apps](./media/require-tou/16.png)

   1. Klicken Sie auf **Cloud-Apps**.
   1. Klicken Sie auf der Seite **Cloud-Apps** auf **Apps auswählen**.
   1. Klicken Sie auf **Auswählen**.
   1. Wählen Sie auf der Seite **Auswählen** die Option **Microsoft Azure Management** aus, und klicken Sie dann auf **Auswählen**.
   1. Klicken Sie auf der Seite „Cloud-Apps“ auf **Fertig**.
1. Klicken Sie auf **What If**.

## <a name="test-your-conditional-access-policy"></a>Testen der Richtlinie für bedingten Zugriff

Im vorherigen Abschnitt haben Sie gelernt, wie Sie eine simulierte Anmeldung auswerten. Zusätzlich zu einer Simulation sollten Sie auch Ihre Richtlinie für bedingten Zugriff testen, um sicherzustellen, dass sie wie erwartet funktioniert.

Um Ihre Richtlinie zu testen, versuchen Sie, sich im [Azure-Portal](https://portal.azure.com) mit dem Testkonto **Isabella Simonsen** anzumelden. Es sollte ein Dialogfeld angezeigt werden, in dem Sie Ihre Nutzungsbedingungen akzeptieren müssen.

![Nutzungsbedingungen](./media/require-tou/57.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie den Testbenutzer, wenn er nicht mehr benötigt wird, und löschen Sie auch die Richtlinie für bedingten Zugriff:

- Wenn Sie nicht wissen, wie ein Azure AD-Benutzer gelöscht wird, lesen Sie [Löschen von Benutzern aus Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- Zum Löschen Ihrer Richtlinie wählen Sie die Richtlinie aus, und klicken Sie dann in der Symbolleiste für den Schnellzugriff auf **Löschen**.

    ![Multi-Factor Authentication](./media/require-tou/33.png)

- Um Ihre Nutzungsbedingungen zu löschen, wählen Sie sie aus, und klicken Sie dann auf der Symbolleiste oben auf **Bedingungen löschen**.

    ![Multi-Factor Authentication](./media/require-tou/29.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfordern von mehrstufiger Authentifizierung für bestimmte Apps](app-based-mfa.md)
> [Blockieren des Zugriffs bei erkanntem Sitzungsrisiko](app-sign-in-risk.md)
