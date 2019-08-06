---
title: Konfigurieren des einmaligen Anmeldens per Kennwort für Azure AD-Apps | Microsoft-Dokumentation
description: Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) per Kennwort für Ihre Azure AD-Unternehmensanwendungen auf der Microsoft Identity Platform (Azure AD)
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: d191abafbaad123ed47f8eaae6cdd4e48478da7a
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422642"
---
# <a name="configure-password-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens per Kennwort

Wenn Sie Ihren Azure AD-Unternehmensanwendungen [eine Katalog-App](add-gallery-app.md) oder eine [andere Web-App](add-non-gallery-app.md) hinzufügen, können Sie als SSO-Option beispielsweise [einmaliges Anmelden per Kennwort](what-is-single-sign-on.md#password-based-sso) verwenden. Diese Option ist für alle Webanwendungen mit einer HTML-Anmeldeseite verfügbar. Das kennwortbasierte einmalige Anmelden (auch als „Password Vaulting“ oder „Kennworttresor“ bezeichnet) ermöglicht es Ihnen, den Benutzerzugriff und die Kennwörter für Webanwendungen zu verwalten, die keinen Identitätsverbund unterstützen. Das Feature ist auch für Szenarien nützlich, in denen mehrere Benutzer ein Konto gemeinsam verwenden müssen, wie z.B. bei den App-Konten für die sozialen Medien Ihrer Organisation. 

Einmaliges Anmelden per Kennwort eignet sich hervorragend, um Anwendungen schnell in Azure AD zu integrieren, und ermöglicht Ihnen Folgendes:

-   Aktivieren Sie das **einmalige Anmelden für Ihre Benutzer**, indem Sie Benutzernamen und Kennwörter für die in Azure AD integrierte Anwendung sicher speichern und später in der Anwendung automatisch ausfüllen.

-   **Unterstützen Sie Anwendungen, die mehrere Anmeldefelder erfordern** für Anwendungen, die über Benutzername und Kennwort hinaus weitere Informationen zur Anmeldung erfordern.

-   **Definieren Sie die Bezeichnungen** der Eingabefelder für Benutzer und Kennwort, die Ihre Benutzer beim Eingeben ihrer Anmeldeinformationen im [Anwendungszugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) sehen.

-   Ermöglichen Sie es Ihren **Benutzern**, für vorhandene Anwendungskonten eigene Benutzernamen und Kennwörter festzulegen und diese manuell im [Anwendungszugriffsbereich](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) einzugeben.

-   Ermöglichen Sie es einem **Mitglied der Gruppe „Business“** , mithilfe des Features [Self-Service-Anwendungszugriff](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) die Benutzernamen und Kennwörter festzulegen, die einem Benutzer zugewiesen werden.

-   Ermöglichen Sie einem **Administrator** das Angeben eines Benutzernamens und Kennworts für Einzelpersonen oder Gruppen, wenn diese sich mit der Funktion „Anmeldeinformationen aktualisieren“ an der Anwendung anmelden. 

## <a name="before-you-begin"></a>Voraussetzungen

Falls die Anwendung Ihrem Azure AD-Mandanten noch nicht hinzugefügt wurde, helfen Ihnen die Artikel [Hinzufügen einer Katalog-App zu Ihrer Azure AD-Organisation](add-gallery-app.md) bzw. [Add an unlisted (non-gallery) application to your Azure AD organization](add-non-gallery-app.md) (Hinzufügen einer nicht aufgeführten (nicht im Katalog enthaltenen) Anwendung zu Ihrer Azure AD-Organisation) weiter.

## <a name="open-the-app-and-select-password-single-sign-on"></a>Öffnen der App und Auswählen des einmaligen Anmeldens per Kennwort

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Cloudanwendungsadministrator oder Anwendungsadministrator für Ihren Azure AD-Mandanten an.

2. Navigieren Sie zu **Azure Active Directory** > **Unternehmensanwendungen**. Eine zufällige Stichprobe von Anwendungen in Ihrem Azure AD-Mandanten wird angezeigt. 

3. Wählen Sie im Menü **Anwendungstyp** die Option **Alle Anwendungen** und dann **Übernehmen** aus.

4. Geben Sie den Namen Ihrer Anwendung in das Suchfeld ein, und wählen Sie in den Ergebnissen Ihre Anwendung aus.

5. Wählen Sie im Abschnitt **Verwalten** die Option **Einmaliges Anmelden** aus. 

6. Wählen Sie **Kennwortbasiert**.

7. Geben Sie die URL der webbasierten Anmeldeseite der Anwendung ein. Bei dieser Zeichenfolge muss es sich um die Seite handeln, die das Eingabefeld für den Benutzernamen enthält.

   ![Kennwortbasiertes einmaliges Anmelden](./media/configure-single-sign-on-non-gallery-applications/password-based-sso.png)

8. Wählen Sie **Speichern** aus. Azure AD versucht, die Anmeldeseite im Hinblick auf die Eingabe eines Benutzernamens und eines Kennworts zu analysieren. Wenn der Versuch erfolgreich ist, sind Sie fertig. 
 
> [!NOTE]
> Der nächste Schritt ist das [Zuweisen von Benutzern oder Gruppen zur Anwendung](methods-for-assigning-users-and-groups.md). Nachdem Sie Benutzer und Gruppen zugewiesen haben, können Sie die Anmeldeinformationen angeben, die für einen Benutzer verwendet werden können, wenn dieser sich an der Anwendung anmeldet. Wählen Sie die Option **Benutzer und Gruppen**, aktivieren Sie das Kontrollkästchen für die Zeile des Benutzers bzw. der Gruppe, und klicken Sie anschließend auf **Anmeldeinformationen aktualisieren**. Geben Sie dann den Benutzernamen und das Kennwort ein, der bzw. das für den Benutzer oder die Gruppe verwendet werden soll. Andernfalls werden die Benutzer beim Start der Anwendung aufgefordert, die Anmeldeinformationen selbst einzugeben.
 

## <a name="manual-configuration"></a>Manuelle Konfiguration

Falls der Analyseversuch von Azure AD nicht erfolgreich ist, können Sie die Anmeldung manuell konfigurieren.

1. Wählen Sie unter **\<Anwendungsname>-Konfiguration** die Option **\<Anwendungsname>-Einstellungen für einmaliges Anmelden über ein Kennwort konfigurieren**, um die Seite **Anmeldung konfigurieren** anzuzeigen. 

2. Wählen Sie **Anmeldefelder manuell erkennen** aus. Es werden weitere Anweisungen angezeigt, die die manuelle Erkennung von Anmeldefeldern beschreiben.

   ![Manuelle Konfiguration des kennwortbasierten einmaligen Anmeldens](./media/configure-password-single-sign-on/password-configure-sign-on.png)
3. Wählen Sie **Anmeldefelder erfassen** aus. Auf einer neuen Registerkarte wird eine Seite mit dem Erfassungsstatus geöffnet, auf der die Meldung **Die Metadatenerfassung wird aktuell ausgeführt.** angezeigt wird.

4. Wenn auf einer neuen Registerkarte das Feld **Erweiterung für Zugriffsbereich erforderlich** angezeigt wird, klicken Sie auf **Jetzt installieren**, um die Browsererweiterung **Erweiterung zur sicheren Anmeldung bei "Meine Apps"** zu installieren. (Die Browsererweiterung erfordert Microsoft Edge, Chrome oder Firefox.) Installieren, starten und aktivieren Sie die Erweiterung, und aktualisieren Sie die Seite mit dem Erfassungsstatus.

   Die Browsererweiterung wird in einer anderen Registerkarte geöffnet, die die eingegebene URL anzeigt.
5. Führen Sie auf der Registerkarte mit der eingegebenen URL den Anmeldevorgang durch. Füllen Sie die Felder für Benutzername und Kennwort aus, und versuchen Sie, sich anzumelden. (Sie müssen nicht das richtige Kennwort angeben.)

   Sie werden aufgefordert, die erfassten Anmeldefelder zu speichern.
6. Klicken Sie auf **OK**. Die Browsererweiterung aktualisiert die Seite mit dem Erfassungsstatus mit der Meldung **Die Metadaten für die Anwendung wurden aktualisiert**. Die Registerkarte des Browsers wird geschlossen.

7. Wählen Sie auf der Azure AD-Seite **Anmeldung konfigurieren** die Option **OK, ich konnte mich erfolgreich bei der App anmelden** aus.

8. Klicken Sie auf **OK**.

Nach dem Erfassen der Anmeldeseite können Sie Benutzer und Gruppen zuweisen sowie Richtlinien für Anmeldeinformationen einrichten, genauso wie bei normalen [Anwendungen für das einmalige Anmelden mit Kennwort](what-is-single-sign-on.md).

> [!NOTE]
> Sie können ein Kachellogo für die Anwendung hochladen. Verwenden Sie hierzu die Schaltfläche **Logo hochladen** auf der Registerkarte **Konfigurieren** für die Anwendung.

## <a name="next-steps"></a>Nächste Schritte

- [Zuweisen von Benutzern und Gruppen zu einer Anwendung in Azure Active Directory](methods-for-assigning-users-and-groups.md)
- [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps im Azure-Portal](configure-automatic-user-provisioning-portal.md)
