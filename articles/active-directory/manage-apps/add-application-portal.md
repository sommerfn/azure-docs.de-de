---
title: Hinzufügen einer App zum Azure Active Directory-Mandanten | Microsoft-Dokumentation
description: In dieser Schnellstartanleitung wird mithilfe des Azure-Portals eine Kataloganwendung zu Ihrem Azure Active Directory-Mandanten (Azure AD) hinzugefügt.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 10/29/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5ee40f6fcd77465b67a7c409493027fefa48ca9
ms.sourcegitcommit: f7f70c9bd6c2253860e346245d6e2d8a85e8a91b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2019
ms.locfileid: "73062947"
---
# <a name="quickstart-add-an-application-to-your-azure-active-directory-tenant"></a>Schnellstart: Hinzufügen einer Anwendung zu Ihrem Azure Active Directory-Mandanten

Azure Active Directory (Azure AD) enthält einen Katalog mit Tausenden von vorab integrierten Anwendungen. Einige der von Ihrer Organisation verwendeten Anwendungen sind wahrscheinlich im Katalog enthalten. In dieser Schnellstartanleitung wird mithilfe des Azure-Portals eine Kataloganwendung zu Ihrem Azure Active Directory-Mandanten (Azure AD) hinzugefügt.

Nach dem Hinzufügen einer Anwendung zum Azure AD-Mandanten haben Sie folgende Möglichkeiten:

- Verwalten des Benutzerzugriffs auf die Anwendung mit einer Richtlinie für bedingten Zugriff
- Konfigurieren von Benutzern für einmaliges Anmelden bei der Anwendung mit ihren Azure AD-Konten

## <a name="before-you-begin"></a>Voraussetzungen

Zum Hinzufügen einer Anwendung zu Ihrem Mandanten benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Abonnement für Ihre Anwendung, für das einmaliges Anmelden aktiviert ist

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als globaler Administrator für Ihren Azure AD-Mandanten, als Cloudanwendungsadministrator oder als Anwendungsadministrator an.

Es wird empfohlen, zum Testen der Schritte in diesem Tutorial keine Produktionsumgebung zu verwenden. Wenn Sie keine solche Umgebung besitzen, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-an-application-to-your-azure-ad-tenant"></a>Hinzufügen einer Anwendung zum Azure AD-Mandanten

So fügen Sie eine Kataloganwendung zu Ihrem Azure AD-Mandanten hinzu:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich **Azure Active Directory** aus.

2. Wählen Sie im Bereich **Azure Active Directory** die Option **Unternehmensanwendungen** aus. Der Bereich **Alle Anwendungen** wird geöffnet und zeigt eine nach dem Zufallsprinzip ausgewählte Gruppe von Anwendungen in Ihrem Azure AD-Mandanten an.

3. Wählen Sie zum Hinzufügen einer Katalog-App zu Ihrem Mandanten **Neue Anwendung** aus. 

    ![Auswählen von „Neue Anwendung“ zum Hinzufügen einer Katalog-App zu Ihrem Mandanten](media/add-application-portal/new-application.png)

 4. Wechseln Sie zur neuen Vorschaubenutzeroberfläche des Katalogs: Wählen Sie auf dem Banner oben auf der Seite **Eine Anwendung hinzufügen** den Link **Klicken Sie hier, um den neuen und verbesserten App-Katalog auszuprobieren.** aus.

5. Der Bereich **Azure AD-Katalog durchsuchen (Vorschau)** wird mit Kacheln für Cloudplattformen, lokale Anwendungen und ausgewählten Anwendungen geöffnet. Beachten Sie, dass für die im Abschnitt **Ausgewählte Anwendungen** aufgeführten Anwendungen mithilfe von Symbolen angegeben wird, ob sie die einmalige Verbundanmeldung und -bereitstellung unterstützen.

    ![Suchen nach einer App anhand des Namens oder der Kategorie](media/add-application-portal/browse-gallery.png)

6. Sie können den Katalog nach der hinzuzufügenden App durchsuchen oder nach der Anwendung suchen, indem Sie ihren Namen in das Suchfeld eingeben. Wählen Sie dann die Anwendung in den Ergebnissen aus. In dem Formular können Sie den Namen der Anwendung entsprechend den Anforderungen Ihres Unternehmens bearbeiten. In diesem Beispiel haben wir den Namen in **GitHub-test** geändert.

    ![Zeigt, wie Sie eine Anwendung aus dem Katalog hinzufügen](media/add-application-portal/create-application.png)

7. Klicken Sie auf **Erstellen**. Eine Seite mit ersten Schritten und den Optionen zum Konfigurieren der Anwendung für Ihre Organisation wird angezeigt.

Das Hinzufügen der Anwendung ist abgeschlossen. In den nächsten Abschnitten wird gezeigt, wie Sie das Logo ändern und weitere Eigenschaften für Ihre Anwendung bearbeiten.

## <a name="find-your-azure-ad-tenant-application"></a>Suchen nach Ihrer Azure AD-Mandantenanwendung

Angenommen, Sie mussten den Vorgang unterbrechen und möchten nun die Konfiguration Ihrer Anwendung fortsetzen. Zunächst müssen Sie die Anwendung suchen.

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus.
1. Wählen Sie im Bereich **Azure Active Directory** die Option **Unternehmensanwendungen** aus.
1. Wählen Sie im Dropdownmenü **Anwendungstyp** die Option **Alle Anwendungen** und dann **Anwenden** aus. Weitere Informationen zu den Anzeigeoptionen finden Sie unter [Anzeigen aller von Ihnen verwaltbaren Unternehmens-Apps in Azure Active Directory](view-applications-portal.md).
1. Sie können jetzt eine Liste aller Anwendungen im Azure AD-Mandanten anzeigen. Die Liste enthält eine nach dem Zufallsprinzip ausgewählte Gruppe. Wählen Sie zum Anzeigen weiterer Anwendungen mehrmals die Option **Mehr anzeigen** aus.
1. Um schnell eine Anwendung in Ihrem Mandanten zu finden, geben Sie den Anwendungsnamen ins Suchfeld ein, und wählen Sie **Anwenden** aus. In diesem Beispiel wird nach der zuvor hinzugefügten Anwendung „GitHub-test“ gesucht.

    ![Zeigt, wie Sie eine Anwendung über das Suchfeld suchen](media/add-application-portal/find-application.png)

## <a name="configure-user-sign-in-properties"></a>Konfigurieren der Eigenschaften für die Benutzeranmeldung

Sie haben die Anwendung gefunden. Nun können Sie sie öffnen und Anwendungseigenschaften konfigurieren.

So bearbeiten Sie die Anwendungseigenschaften:

1. Wählen Sie die Anwendung aus, um sie zu öffnen.
2. Wählen Sie **Eigenschaften** aus, um den Bereich „Eigenschaften“ zur Bearbeitung zu öffnen.

    ![Zeigt den Eigenschaftenbildschirm und App-Eigenschaften, die bearbeitet werden können](media/add-application-portal/edit-properties.png)

3. Nehmen Sie sich einen Moment Zeit, um die Anmeldeoptionen nachzuvollziehen. Die Optionen bestimmen, wie die der Anwendung zugewiesenen bzw. nicht zugewiesenen Benutzer sich bei der Anwendung anmelden können. Darüber hinaus legen die Optionen fest, ob ein Benutzer die Anwendung im Zugriffsbereich sehen kann.

    - Mit **Aktiviert für die Benutzeranmeldung?** wird festgelegt, ob sich Benutzer, die der Anwendung zugewiesen sind, anmelden können.
    - Mit **Benutzerzuweisung erforderlich?** wird festgelegt, ob sich Benutzer, die der Anwendung nicht zugewiesen sind, anmelden können.
    - Mit **Für Benutzer sichtbar?** wird festgelegt, ob für der App zugewiesene Benutzer die Anwendung im Zugriffsbereich und im Office 365-Startfeld angezeigt wird.

4. Anhand der folgenden Tabellen können Sie die Optionen auswählen, die am besten für Ihre Anforderungen geeignet sind:

   - Verhalten für **zugewiesene** Benutzer:

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

   - Verhalten für **nicht zugewiesene** Benutzer:

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

## <a name="use-a-custom-logo"></a>Verwenden eines benutzerdefinierten Logos

So verwenden Sie ein benutzerdefiniertes Logo:

1. Erstellen Sie ein Logo mit 215 x 215 Pixeln, und speichern Sie es im PNG-Format.
1. Da Sie Ihre Anwendung bereits gefunden haben, wählen Sie sie aus.
1. Wählen Sie im linken Bereich **Eigenschaften** aus.
1. Laden Sie das Logo hoch.
1. Wählen Sie abschließend **Speichern** aus. 

    ![Zeigt, wie sich das Logo auf der Eigenschaftenseite der App ändern lässt](media/add-application-portal/change-logo.png)

   > [!NOTE]
   > Die in diesem Bereich **Eigenschaften** angezeigte Miniaturansicht wird nicht sofort aktualisiert. Sie können die Eigenschaften schließen und wieder öffnen, um das aktualisierte Symbol anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihrer Azure AD-Organisation die Anwendung hinzugefügt haben, können Sie jetzt die gewünschte [Methode für einmaliges Anmelden auswählen](what-is-single-sign-on.md#choosing-a-single-sign-on-method). Lesen Sie dazu die folgenden Artikel:

- [Konfigurieren des SAML-basierten einmaligen Anmeldens](configure-single-sign-on-non-gallery-applications.md)
- [Konfigurieren des einmaligen Anmeldens per Kennwort](configure-password-single-sign-on-non-gallery-applications.md)
- [Konfigurieren der Anmeldung über Link](configure-linked-sign-on.md)
