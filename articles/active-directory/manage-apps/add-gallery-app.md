---
title: 'Hinzufügen einer Katalog-App: Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie Ihren Azure-Unternehmensanwendungen eine App aus dem Azure AD-Katalog hinzufügen.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: tutorial
ms.workload: identity
ms.date: 10/29/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: af0a826c499790c86a20ea7061075a6c3e66b3a4
ms.sourcegitcommit: f7f70c9bd6c2253860e346245d6e2d8a85e8a91b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73062573"
---
# <a name="add-a-gallery-app-to-your-azure-ad-organization"></a>Hinzufügen einer Katalog-App zu Ihrer Azure AD-Organisation

Azure Active Directory (Azure AD) enthält einen Katalog mit Tausenden von vorab integrierten Anwendungen mit Enterprise Single Sign-On (Einmaliges Anmelden für Unternehmen (SSO)). In diesem Artikel werden die allgemeinen Schritte zum Hinzufügen einer App aus dem Katalog zu Ihrer Azure AD-Organisation beschrieben.

> [!IMPORTANT]
> Suchen Sie Ihre App zunächst in der Liste der [Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/). Wahrscheinlich finden Sie eine Schritt-für-Schritt-Anleitung zum Hinzufügen und Konfigurieren der hinzuzufügenden Katalog-App.

## <a name="add-a-gallery-application"></a>Hinzufügen einer Kataloganwendung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator für Ihren Azure AD-Mandanten, als Cloudanwendungsadministrator oder als Anwendungsadministrator an.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich **Azure Active Directory** aus.

1. Wählen Sie im Bereich **Azure Active Directory** die Option **Unternehmensanwendungen** aus.

    ![Öffnen von „Unternehmensanwendungen“](media/add-gallery-app/open-enterprise-apps.png)


3. Wählen Sie zum Hinzufügen einer Katalog-App zu Ihrem Mandanten **Neue Anwendung** aus.

    ![Auswählen von „Neue Anwendung“ zum Hinzufügen einer Katalog-App zu Ihrem Mandanten](media/add-gallery-app/new-application.png)

 4. Wechseln Sie zur neuen Vorschaubenutzeroberfläche des Katalogs: Wählen Sie auf dem Banner oben auf der Seite **Eine Anwendung hinzufügen** den Link **Klicken Sie hier, um den neuen und verbesserten App-Katalog auszuprobieren.** aus.

5. Der Bereich **Azure AD-Katalog durchsuchen** wird mit Kacheln für Cloudplattformen, lokale Anwendungen und ausgewählten Anwendungen geöffnet. Beachten Sie, dass für die im Abschnitt **Ausgewählte Anwendungen** aufgeführten Anwendungen mithilfe von Symbolen angegeben wird, ob sie die einmalige Verbundanmeldung und -bereitstellung unterstützen.

    ![Suchen nach einer App anhand des Namens oder der Kategorie](media/add-gallery-app/browse-gallery.png)

6. Durchsuchen Sie den Katalog nach der hinzuzufügenden App, oder suchen Sie nach der Anwendung, indem Sie ihren Namen in das Suchfeld eingeben. Wählen Sie dann die Anwendung in den Ergebnissen aus. (Optional) In dem Formular können Sie den Anwendungsnamen entsprechend den Anforderungen Ihres Unternehmens bearbeiten.

    ![Zeigt, wie Sie eine Anwendung aus dem Katalog hinzufügen](media/add-gallery-app/create-application.png)

7. Klicken Sie auf **Erstellen**. Eine Seite mit ersten Schritten und den Optionen zum Konfigurieren der Anwendung für Ihre Organisation wird angezeigt.

## <a name="configure-user-sign-in-properties"></a>Konfigurieren der Eigenschaften für die Benutzeranmeldung

1. Wählen Sie **Eigenschaften** aus, um den Bereich „Eigenschaften“ zur Bearbeitung zu öffnen.

    ![Bereich „Eigenschaften bearbeiten“](media/add-gallery-app/edit-properties.png)

1. Legen Sie mit den folgenden Optionen fest, wie sich Benutzer mit bzw. ohne Anwendungszuweisung bei der Anwendung anmelden können und ob einem Benutzer die Anwendung im Zugriffsbereich angezeigt wird.

    - Mit **Aktiviert für die Benutzeranmeldung?** wird festgelegt, ob sich Benutzer, die der Anwendung zugewiesen sind, anmelden können.
    - Mit **Benutzerzuweisung erforderlich?** wird festgelegt, ob sich Benutzer, die der Anwendung nicht zugewiesen sind, anmelden können.
    - Mit **Für Benutzer sichtbar?** wird festgelegt, ob für der App zugewiesene Benutzer die Anwendung im Zugriffsbereich und im Office 365-Startfeld angezeigt wird.

      Verhalten für **zugewiesene** Benutzer:

       | Eigenschafteneinstellungen der Anwendung | | | Zugewiesene Benutzer | |
       |---|---|---|---|---|
       | Aktiviert für die Benutzeranmeldung? | Benutzerzuweisung erforderlich? | Für Benutzer sichtbar? | Können sich zugewiesene Benutzer anmelden? | Können zugewiesene Benutzer die Anwendung sehen?* |
       | Ja | Ja | Ja | Ja | Ja  |
       | Ja | Ja | no  | Ja | no   |
       | Ja | no  | Ja | Ja | Ja  |
       | Ja | no  | no  | Ja | no   |
       | no  | Ja | Ja | no  | no   |
       | no  | Ja | no  | no  | no   |
       | no  | no  | Ja | no  | no   |
       | no  | no  | no  | no  | no   |

      Verhalten für **nicht zugewiesene** Benutzer:

       | Eigenschafteneinstellungen der Anwendung | | | Nicht zugewiesene Benutzer | |
       |---|---|---|---|---|
       | Aktiviert für die Benutzeranmeldung? | Benutzerzuweisung erforderlich? | Für Benutzer sichtbar? | Können sich nicht zugewiesene Benutzer anmelden? | Können nicht zugewiesene Benutzer die Anwendung sehen?* |
       | Ja | Ja | Ja | no  | no   |
       | Ja | Ja | no  | no  | no   |
       | Ja | no  | Ja | Ja | no   |
       | Ja | no  | no  | Ja | no   |
       | no  | Ja | Ja | no  | no   |
       | no  | Ja | no  | no  | no   |
       | no  | no  | Ja | no  | no   |
       | no  | no  | no  | no  | no   |

     *Kann der Benutzer die Anwendung im Zugriffsbereich und im Office 365-App-Startfeld sehen?

1. Wenn Sie ein benutzerdefiniertes Logo verwenden möchten, erstellen Sie ein Logo mit 215 x 215 Pixeln, und speichern Sie es im PNG-Format. Navigieren Sie dann zu Ihrem Logo, und laden Sie es hoch.

    ![Ändern des Logos](media/add-gallery-app/change-logo.png)

1. Wählen Sie abschließend **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihrer Azure AD-Organisation die Anwendung hinzugefügt haben, können Sie jetzt die gewünschte [Methode für einmaliges Anmelden auswählen](what-is-single-sign-on.md#choosing-a-single-sign-on-method). Lesen Sie dazu die folgenden Artikel:

- [Konfigurieren des SAML-basierten einmaligen Anmeldens](configure-single-sign-on-non-gallery-applications.md)
- [Konfigurieren des einmaligen Anmeldens per Kennwort](configure-password-single-sign-on-non-gallery-applications.md)
- [Konfigurieren der Anmeldung über Link](configure-linked-sign-on.md)

