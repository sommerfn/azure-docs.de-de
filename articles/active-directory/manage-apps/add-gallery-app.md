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
ms.date: 06/18/2019
ms.author: mimart
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1bdf275bbafa9c46cfc4577ac2843da0be74c7ef
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2019
ms.locfileid: "68477286"
---
# <a name="add-a-gallery-app-to-your-azure-ad-organization"></a>Hinzufügen einer Katalog-App zu Ihrer Azure AD-Organisation

Azure Active Directory (Azure AD) enthält einen Katalog mit Tausenden von vorab integrierten Anwendungen mit Enterprise Single Sign-On (Einmaliges Anmelden für Unternehmen (SSO)). In diesem Artikel werden die allgemeinen Schritte zum Hinzufügen einer App aus dem Katalog zu Ihrer Azure AD-Organisation beschrieben.

> [!IMPORTANT]
> Suchen Sie Ihre App zunächst in der Liste der [Tutorials zur Integration von SaaS-Anwendungen in Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/). Wahrscheinlich finden Sie eine Schritt-für-Schritt-Anleitung zum Hinzufügen und Konfigurieren der hinzuzufügenden Katalog-App.

## <a name="add-a-gallery-application"></a>Hinzufügen einer Kataloganwendung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator für Ihren Azure AD-Mandanten, als Cloudanwendungsadministrator oder als Anwendungsadministrator an.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich **Azure Active Directory** aus.

1. Wählen Sie im Bereich **Azure Active Directory** die Option **Unternehmensanwendungen** aus.

    ![Öffnen von „Unternehmensanwendungen“](media/add-application-portal/open-enterprise-apps.png)

1. Wählen Sie **Neue Anwendung** aus.

    ![Neue Anwendung](media/add-application-portal/new-application.png)

1. Geben Sie unter **Aus Katalog hinzufügen** den Namen der hinzuzufügenden Anwendung ins Suchfeld ein. 

    ![Suchen nach Name oder Kategorie](media/add-application-portal/categories.png)

1. Wählen Sie die Anwendung in den Ergebnissen aus.

1. (Optional) In dem anwendungsspezifischen Formular können Sie den Namen der Anwendung entsprechend den Anforderungen Ihres Unternehmens bearbeiten.

1. Wählen Sie **Hinzufügen**. Die Seite **Übersicht** der Anwendung wird geöffnet.

## <a name="configure-user-sign-in-properties"></a>Konfigurieren der Eigenschaften für die Benutzeranmeldung

1. Wählen Sie **Eigenschaften** aus, um den Bereich „Eigenschaften“ zur Bearbeitung zu öffnen.

    ![Bereich „Eigenschaften bearbeiten“](media/add-application-portal/edit-properties.png)

1. Legen Sie mit den folgenden Optionen fest, wie sich wie sich Benutzer mit bzw. ohne Anwendungszuweisung bei der Anwendung anmelden können und ob einem Benutzer die Anwendung im Zugriffsbereich angezeigt wird.

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

    ![Ändern des Logos](media/add-application-portal/change-logo.png)

1. Wählen Sie abschließend **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

Sie haben die Anwendung zu Ihrer Azure AD-Organisation hinzugefügt. Wählen Sie nun die gewünschte [Methode für einmaliges Anmelden](what-is-single-sign-on.md#choosing-a-single-sign-on-method) aus, und sehen Sie sich die folgenden Artikel an:

- [Konfigurieren des einmaligen Anmeldens bei nicht im Katalog enthaltenen Anwendungen auf der Microsoft Identity Platform](configure-single-sign-on-non-gallery-applications.md)
- [Konfigurieren des einmaligen Anmeldens per Kennwort für eine nicht im Katalog enthaltene Anwendung](configure-password-single-sign-on-non-gallery-applications.md)
- [Konfigurieren der Anmeldung über Link](configure-linked-sign-on.md)

