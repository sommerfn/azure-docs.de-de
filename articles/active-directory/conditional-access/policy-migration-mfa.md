---
title: Migrieren einer klassischen Richtlinie, die mehrstufige Authentifizierung erfordert, in das Azure-Portal
description: Dieser Artikel zeigt, wie Sie eine klassische Richtlinie, die eine mehrstufige Authentifizierung erfordert, in das Azure-Portal migrieren.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: tutorial
ms.date: 06/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4819c283a136057ad7c3ffd755fd9e157d99a1bf
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509452"
---
# <a name="migrate-a-classic-policy-that-requires-multi-factor-authentication-in-the-azure-portal"></a>Migrieren einer klassischen Richtlinie, die mehrstufige Authentifizierung erfordert, in das Azure-Portal

Dieses Tutorial zeigt, wie Sie eine klassische Richtlinie migrieren, die eine **mehrstufige Authentifizierung** für eine Cloud-App erfordert. Es stellt zwar keine Voraussetzung dar, wir empfehlen Ihnen aber, [Migrieren klassischer Richtlinien in das Azure-Portal](policy-migration.md) zu lesen, bevor Sie mit der Migration Ihrer klassischen Richtlinien beginnen.

## <a name="overview"></a>Übersicht

Das Szenario in diesem Artikel zeigt, wie Sie eine klassische Richtlinie migrieren, die eine **mehrstufige Authentifizierung** für eine Cloud-App erfordert.

![Azure Active Directory](./media/policy-migration/33.png)

Der Migrationsvorgang besteht aus folgenden Schritten:

1. [Öffnen Sie die klassische Richtlinie](#open-a-classic-policy) zum Abrufen der Konfigurationseinstellungen.
1. Erstellen Sie eine neue Azure AD-Richtlinie für bedingten Zugriff, um Ihre klassische Richtlinie zu ersetzen. 
1. Deaktivieren Sie die klassische Richtlinie.

## <a name="open-a-classic-policy"></a>Öffnen einer klassischen Richtlinie

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf der linken Navigationsleiste auf **Azure Active Directory**.

   ![Azure Active Directory](./media/policy-migration-mfa/01.png)

1. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Verwalten** auf **Bedingter Zugriff**.

   ![Bedingter Zugriff](./media/policy-migration-mfa/02.png)

1. Klicken Sie im Abschnitt **Verwalten** auf **Klassische Richtlinien (Vorschau)** .

   ![Klassische Richtlinien](./media/policy-migration-mfa/12.png)

1. Klicken Sie in der Liste der klassischen Richtlinien auf die Richtlinie, die eine **mehrstufige Authentifizierung** für eine Cloud-App erfordert.

   ![Klassische Richtlinien](./media/policy-migration-mfa/13.png)

## <a name="create-a-new-conditional-access-policy"></a>Erstellen einer neuen Richtlinie für bedingten Zugriff

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf der linken Navigationsleiste auf **Azure Active Directory**.

   ![Azure Active Directory](./media/policy-migration/01.png)

1. Klicken Sie auf der Seite **Azure Active Directory** im Abschnitt **Verwalten** auf **Bedingter Zugriff**.

   ![Bedingter Zugriff](./media/policy-migration/02.png)

1. Klicken Sie zum Öffnen der Seite **Neu** auf der Seite **Bedingter Zugriff** oben auf der Symbolleiste auf **Hinzufügen**.

   ![Bedingter Zugriff](./media/policy-migration/03.png)

1. Geben Sie auf der Seite **Neu** im Textfeld **Name** einen Namen für Ihre Richtlinie ein.

   ![Bedingter Zugriff](./media/policy-migration/29.png)

1. Klicken Sie im Abschnitt **Zuweisungen** auf **Benutzer und Gruppen**.

   ![Bedingter Zugriff](./media/policy-migration/05.png)

   1. Wenn Sie alle Benutzer in der klassischen Richtlinie ausgewählt haben, klicken Sie auf **Alle Benutzer**. 

      ![Bedingter Zugriff](./media/policy-migration/35.png)

   1. Wenn Sie Gruppen in Ihrer klassischen Richtlinie ausgewählt haben, klicken Sie auf **Benutzer und Gruppen auswählen**, und wählen Sie dann die gewünschten Benutzer und Gruppen aus.

      ![Bedingter Zugriff](./media/policy-migration/36.png)

   1. Wenn Sie ausgeschlossene Gruppen haben, klicken Sie auf die Registerkarte **Ausschließen**, und wählen Sie dann die gewünschten Benutzer und Gruppen aus. 

      ![Bedingter Zugriff](./media/policy-migration/37.png)

1. Klicken Sie zum Öffnen der Seite **Cloud-Apps** auf der Seite **Neu** im Abschnitt **Zuweisung** auf **Cloud-Apps**.
1. Führen Sie auf der Seite **Cloud-Apps** die folgenden Schritte aus:
   1. Klicken Sie auf **Apps auswählen**.
   1. Klicken Sie auf **Auswählen**.
   1. Wählen Sie auf der Seite **Auswählen** Ihre Cloud-App aus, und klicken Sie dann auf **Auswählen**.
   1. Klicken Sie auf der Seite **Cloud-Apps** auf **Fertig**.
1. Falls **Mehrstufige Authentifizierung anfordern** ausgewählt ist:

   ![Bedingter Zugriff](./media/policy-migration/26.png)

   1. Klicken Sie im Abschnitt **Zugriffssteuerungen** auf **Gewähren**.

      ![Bedingter Zugriff](./media/policy-migration/27.png)

   1. Klicken Sie auf der Seite **Gewähren** auf **Zugriff gewähren**, und klicken Sie dann auf **Mehrstufige Authentifizierung anfordern**.
   1. Klicken Sie auf **Auswählen**.
1. Klicken Sie auf **Ein**, um Ihre Richtlinie zu aktivieren.

   ![Bedingter Zugriff](./media/policy-migration/30.png)

## <a name="disable-the-classic-policy"></a>Deaktivieren der klassischen Richtlinie

Klicken Sie zum Deaktivieren Ihrer klassischen Richtlinie in der Ansicht **Details** auf **Deaktivieren**.

![Klassische Richtlinien](./media/policy-migration-mfa/14.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen über die Migration von klassischen Richtlinien finden Sie unter [Migrieren klassischer Richtlinien in das Azure-Portal](policy-migration.md).
- Wenn Sie wissen möchten, wie Sie eine Richtlinie für den bedingten Zugriff konfigurieren, finden Sie Informationen unter [Schnellstart: Anfordern der mehrstufigen Authentifizierung (Multi-Factor Authentication, MFA) für bestimmte Apps über den bedingten Zugriff von Azure Active Directory](app-based-mfa.md).
- Wenn Sie bereit sind, Richtlinien für den bedingten Zugriff für Ihre Umgebung zu konfigurieren, helfen Ihnen die Informationen unter [Best Practices für den bedingten Zugriff in Azure Active Directory](best-practices.md) weiter.
