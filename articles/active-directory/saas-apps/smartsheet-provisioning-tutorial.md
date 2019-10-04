---
title: 'Tutorial: Konfigurieren von Smartsheet für die automatische Benutzerbereitstellung mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und für das Aufheben der Bereitstellung von Benutzerkonten in Smartsheet konfigurieren.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2019
ms.author: jeedes
ms.openlocfilehash: f0ca2dfa90e1312db664962e7ffbe6b3f4dd96e1
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670934"
---
# <a name="tutorial-configure-smartsheet-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Smartsheet für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Smartsheet und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und für das Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Smartsheet zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Dieser Connector befindet sich derzeit in der Public Preview-Phase. Weitere Informationen zu den allgemeinen Nutzungsbedingungen von Microsoft Azure für Previewfunktionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* Einen Azure AD-Mandanten
* [Einen Smartsheet-Mandanten](https://www.smartsheet.com/pricing)
* Ein Benutzerkonto in einem Smartsheet Enterprise- oder Enterprise Premier-Plan mit Systemadministratorberechtigungen.

## <a name="assign-users-to-smartsheet"></a>Zuweisen von Benutzern zu Smartsheet

Azure Active Directory ermittelt anhand von *Zuweisungen*, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Smartsheet benötigen. Anschließend können Sie Smartsheet diese Benutzer und/oder Gruppen gemäß den folgenden Anweisungen zuweisen:

* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-smartsheet"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Smartsheet

* Es wird empfohlen, Smartsheet einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

* Beim Zuweisen eines Benutzers zu Smartsheet müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

* Um Parität der Benutzerrollenzuweisungen zwischen Smartsheet und Azure AD sicherzustellen, wird empfohlen, die gleichen Rollenzuweisungen zu verwenden, die in der vollständigen Smartsheet-Benutzerliste aufgefüllt sind. Wenn Sie diese Benutzerliste von Smartsheet abrufen möchten, navigieren Sie zu **Kontoadministrator > Benutzerverwaltung > Weitere Aktionen > Benutzerliste (CSV) herunterladen**.

* Damit ein Benutzer auf bestimmte Features in der App zugreifen kann, muss er in Smartsheet über mehrere Rollen verfügen. Weitere Informationen zu Benutzertypen und Berechtigungen in Smartsheet finden Sie unter [User Types and Permissions](https://help.smartsheet.com/learning-track/shared-users/user-types-and-permissions) (Benutzertypen und Berechtigungen).

*  Wenn einem Benutzer mehreren Rollen in Smartsheet zugewiesen sind, **MÜSSEN** Sie sicherstellen, dass diese Rollenzuweisungen in Azure AD repliziert werden, um ein Szenario zu vermeiden, in dem Benutzer den Zugriff auf Smartsheet-Objekte dauerhaft verlieren könnten. Jede eindeutige Rolle in Smartsheet **MUSS** einer anderen Gruppe in Azure AD zugewiesen werden. Der Benutzer **MUSS** dann allen Gruppen hinzugefügt werden, die den gewünschten Rollen entsprechen. 

## <a name="set-up-smartsheet-for-provisioning"></a>Einrichten von Smartsheet für die Bereitstellung

Bevor Sie Smartsheet für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie in Smartsheet die SCIM-Bereitstellung aktivieren.

1. Melden Sie sich als **Systemadministrator** beim **[Smartsheet-Portal](https://app.smartsheet.com/b/home)** an, und navigieren Sie zu **Kontoadministrator**.

    ![Smartsheet-Kontoadministrator](media/smartsheet-provisioning-tutorial/smartsheet-accountadmin.png)

2. Wechseln Sie zu **Sicherheitssteuerung > Automatisiertes Benutzer-Provisioning > Bearbeiten**.

    ![Smartsheet-Sicherheitssteuerung](media/smartsheet-provisioning-tutorial/smartsheet-securitycontrols.png)

3. Fügen Sie die E-Mail-Domänen für die Benutzer hinzu, die Sie aus Azure AD in Smartsheet bereitstellen möchten, und überprüfen Sie die Domänen. Wählen Sie **DEAKTIVIERT** aus, um sicherzustellen, dass alle Bereitstellungsaktionen nur von Azure AD stammen, und um außerdem sicherzustellen, dass Ihre Smartsheet-Benutzerliste mit Azure AD-Zuweisungen synchronisiert wird.

    ![Smartsheet-Benutzerbereitstellung](media/smartsheet-provisioning-tutorial/smartsheet-userprovisioning.png)

4. Sobald die Überprüfung abgeschlossen ist, müssen Sie die Domäne aktivieren. 

    ![Domäne in Smartsheet aktivieren](media/smartsheet-provisioning-tutorial/smartsheet-activatedomain.png)

5. Generieren Sie das **geheime Token**, das zum Konfigurieren der automatischen Benutzerbereitstellung mit Azure AD erforderlich ist, indem Sie zu **Apps & Integrationen** navigieren.

    ![Smartsheet-Installation](media/smartsheet-provisioning-tutorial/Smartsheet05.png)

6. Wählen Sie **API-Zugriff** aus. Klicken Sie auf **Neues Zugriffstoken generieren**.

    ![Smartsheet-Installation](media/smartsheet-provisioning-tutorial/Smartsheet06.png)

7. Legen Sie den Namen des API-Zugriffstokens fest. Klicken Sie auf **OK**.

    ![Smartsheet-Installation](media/smartsheet-provisioning-tutorial/Smartsheet07.png)

8. Kopieren Sie das API-Zugriffstoken, und speichern Sie es, weil Sie es nur zu diesem Zeitpunkt anzeigen können. Diese Angabe ist in Azure AD im Feld **Geheimes Token** erforderlich.

    ![Smartsheet-Token](media/smartsheet-provisioning-tutorial/Smartsheet08.png)

## <a name="add-smartsheet-from-the-gallery"></a>Hinzufügen von Smartsheet aus dem Katalog

Um Smartsheet für die automatische Benutzerbereitstellung mit Azure AD konfigurieren zu können, müssen Sie Ihrer Liste der verwalteten SaaS-Anwendungen Smartsheet aus dem Azure AD-Anwendungskatalog hinzufügen.

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Bereich auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld den Namen **Smartsheet**ein, und wählen Sie im Ergebnisbereich die Anwendung **Smartsheet** aus. 

    ![Smartsheet in der Ergebnisliste](common/search-new-app.png)

5. Wählen Sie die Schaltfläche **Anmeldung bei Smartsheet** aus, über die Sie an die Anmeldeseite von Smartsheet umgeleitet werden. 

    ![Smartsheet, OpenIDConnect-App hinzufügen](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-add.png)

6. Da Smartsheet eine OpenIDConnect-App ist, können Sie sich mit Ihrem Microsoft-Geschäftskonto bei Smartsheet anmelden.

    ![Smartsheet, OIDC-Anmeldung](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-login.png)

7. Akzeptieren Sie nach der erfolgreichen Authentifizierung auf der Zustimmungsseite die Zustimmungsaufforderung. Die Anwendung wird dann automatisch Ihrem Mandanten hinzugefügt, und Sie werden zu Ihrem Smartsheet-Konto umgeleitet.

    ![Smartsheet, OIDC-Zustimmung](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-consent.png)

## <a name="configure-automatic-user-provisioning-to-smartsheet"></a>Konfigurieren der automatischen Benutzerbereitstellung für Smartsheet 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Smartsheet auf der Grundlage von Benutzer- und/oder Gruppenzuweisungen in Azure AD erläutert.

### <a name="to-configure-automatic-user-provisioning-for-smartsheet-in-azure-ad"></a>Gehen Sie wie folgt vor, um die automatische Benutzerbereitstellung für Smartsheet in Azure AD zu konfigurieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste die Anwendung **Smartsheet** aus.

    ![Smartsheet-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** im Feld **Mandanten-URL** die Zeichenfolge `https://scim.smartsheet.com/v2/` ein. Geben Sie im Feld **Geheimes Token** den Wert ein, den Sie zuvor aus Smartsheet abgerufen und gespeichert haben. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Smartsheet herstellen kann. Wenn die Verbindung nicht hergestellt werden kann, stellen Sie sicher, dass Ihr Smartsheet-Konto über Systemadministratorberechtigungen verfügt, und versuchen Sie es erneut.

    ![Tokenverschlüsselung](common/provisioning-testconnection-tenanturltoken.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Klicken Sie auf **Speichern**.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Smartsheet synchronisieren** aus.

    ![Smartsheet-Benutzerzuordnungen](media/smartsheet-provisioning-tutorial/smartsheet-user-mappings.png)

9. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Smartsheet synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in Smartsheet für Aktualisierungsvorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Smartsheet-Benutzerattribute](media/smartsheet-provisioning-tutorial/smartsheet-user-attributes.png)

10. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

11. Um den Azure AD-Bereitstellungsdienst für Smartsheet zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

12. Legen Sie die Benutzer und/oder Gruppen fest, die in Smartsheet bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

13. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Bereitstellungsaktivitätsberichten aufrufen. In diesen Berichten sind alle vom Azure AD-Bereitstellungsdienst in Smartsheet ausgeführten Aktionen aufgeführt.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-Einschränkungen

* Smartsheet unterstützt keine vorläufigen Löschungen. Wenn das Attribut **active** eines Benutzers auf „False“ festgelegt ist, wird der Benutzer in Smartsheet dauerhaft gelöscht.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)
