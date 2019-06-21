---
title: 'Tutorial: Konfigurieren von Samanage für die automatische Benutzerbereitstellung mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Samanage konfigurieren.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 62d0392f-37d4-436e-9aff-22f4e5b83623
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: d474d9bfd6016885eaa21afcea5d44d39c624084
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "64730586"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Samanage für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die Sie in Samanage und Azure Active Directory (Azure AD) ausführen müssen, um Azure AD zum automatischen Bereitstellen und Aufheben der Bereitstellung von Benutzern und Gruppen in Samanage zu konfigurieren.

> [!NOTE]
> Das Tutorial enthält die Beschreibung eines Connectors, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Informationen zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen (Software as a Service) mit Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Voraussetzungen

Das in diesem Tutorial beschriebene Szenario setzt voraus, dass Sie über folgende Elemente verfügen:

* Einen Azure AD-Mandanten.
* Einen [Samanage-Mandanten](https://www.samanage.com/pricing/) mit dem Professional-Tarif.
* Ein Benutzerkonto in Samanage mit Teamadministratorberechtigungen.

> [!NOTE]
> Die Azure AD-Bereitstellungsintegration basiert auf der [Samanage-REST-API](https://www.samanage.com/api/). Diese API steht für Samanage-Entwickler für Konten mit dem Professional-Paket zur Verfügung.

## <a name="add-samanage-from-the-azure-marketplace"></a>Hinzufügen von Samanage aus dem Azure Marketplace

Bevor Sie Samanage für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, fügen Sie Samanage aus dem Azure Marketplace der Liste mit den verwalteten SaaS-Anwendungen hinzu.

Führen Sie zum Hinzufügen von Samanage aus dem Azure Marketplace die folgenden Schritte aus.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich die Option **Azure Active Directory** aus.

    ![Azure Active Directory-Symbol](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Wählen Sie oben im Dialogfeld **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Samanage** ein, und wählen Sie **Samanage** im Ergebnisbereich aus. Wählen Sie dann **Hinzufügen** aus, um die Anwendung hinzuzufügen.

    ![Samanage in der Ergebnisliste](common/search-new-app.png)

## <a name="assign-users-to-samanage"></a>Zuweisen von Benutzern zu Samanage

Azure Active Directory ermittelt anhand von *Zuweisungen*, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Zusammenhang mit der automatischen Benutzerbereitstellung werden nur die Benutzer oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Entscheiden Sie vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung, welche Benutzer oder Gruppen in Azure AD Zugriff auf Samanage benötigen. Zum Zuweisen dieser Benutzer oder Gruppen zu Samanage befolgen Sie die Anleitung unter [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-samanage"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Samanage

*    Samanage-Rollen werden derzeit automatisch und dynamisch auf der Benutzeroberfläche des Azure-Portals aufgefüllt. Stellen Sie vor dem Zuweisen von Samanage-Rollen zu Benutzern sicher, dass eine erste Synchronisierung mit Samanage ausgeführt wurde, um die aktuellen Rollen Ihres Samanage-Mandanten abzurufen.

*    Wir empfehlen, zuerst nur einen einzelnen Azure AD-Benutzer zu Samanage zuzuweisen, um die Ausgangskonfiguration der automatischen Benutzerbereitstellung zu testen. Später können Sie dann weitere Benutzer und Gruppen zuweisen, nachdem die Tests erfolgreich waren.

*    Beim Zuweisen eines Benutzers zu Samanage müssen Sie im Dialogfeld für die Zuweisung eine gültige anwendungsspezifische Rolle (sofern verfügbar) auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="configure-automatic-user-provisioning-to-samanage"></a>Konfigurieren der automatischen Benutzerbereitstellung für Samanage

Dieser Abschnitt führt Sie durch die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts. Verwenden Sie diese zum Erstellen, Aktualisieren und Deaktivieren von Benutzern oder Gruppen in Samanage anhand von Benutzer- oder Gruppenzuweisungen in Azure AD.

> [!TIP]
> Sie können auch SAML-basiertes einmaliges Anmelden für Samanage aktivieren. Befolgen Sie hierfür die Anleitung im [Tutorial für einmaliges Anmelden mit Samanage](samanage-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Konfigurieren der automatischen Benutzerbereitstellung für Samanage in Azure AD

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** > **Alle Anwendungen** > **Samanage** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Samanage**aus.

    ![Samanage-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Samanage-Bereitstellung](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Samanage-Bereitstellungsmodus](./media/samanage-provisioning-tutorial/ProvisioningCredentials.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** den Administratorbenutzernamen und das Administratorkennwort Ihres Samanage-Kontos ein. Beispiele für diese Werte:

   * Geben Sie im Feld **Administratorbenutzername** den Benutzernamen des Administratorkontos in Ihrem Samanage-Mandanten ein. Ein Beispiel ist admin@contoso.com.

   * Geben Sie im Feld **Administratorkennwort** das Kennwort des Administratorkontos für den Administratorbenutzernamen ein.

6. Nachdem Sie die in Schritt 5 gezeigten Felder ausgefüllt haben, wählen Sie **Verbindung testen** aus, um sicherzustellen, dass Azure AD eine Verbindung mit Samanage herstellen kann. Wenn die Verbindung nicht möglich ist, sollten Sie sicherstellen, dass Ihr Samanage-Konto über Administratorberechtigungen verfügt, und den Vorgang wiederholen.

    ![Verbindung testen in Samanage](./media/samanage-provisioning-tutorial/TestConnection.png)

7. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll. Aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Samanage-Benachrichtigungs-E-Mail](./media/samanage-provisioning-tutorial/EmailNotification.png)

8. Wählen Sie **Speichern** aus.

9. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Samanage synchronisieren**.

    ![Samanage-Benutzersynchronisierung](./media/samanage-provisioning-tutorial/UserMappings.png)

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Samanage synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Samanage für Updatevorgänge verwendet werden. Um alle Änderungen zu speichern, wählen Sie **Speichern** aus.

    ![Übereinstimmende Benutzerattribute in Samanage](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

11. Wählen Sie zum Aktivieren von Gruppenzuordnungen im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit Samanage synchronisieren**.

    ![Samanage-Gruppensynchronisierung](./media/samanage-provisioning-tutorial/GroupMappings.png)

12. Legen Sie **Aktiviert** auf **Ja** fest, um Gruppen zu synchronisieren. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit Samanage synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Samanage für Updatevorgänge verwendet werden. Um alle Änderungen zu speichern, wählen Sie **Speichern** aus.

    ![Übereinstimmende Gruppenattribute in Samanage](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. Wenn Sie Bereichsfilter konfigurieren möchten, befolgen Sie die Anleitung unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Um den Azure AD-Bereitstellungsdienst für Samanage zu aktivieren, ändern Sie im Abschnitt **Einstellungen** den **Bereitstellungsstatus** in **Ein**.

    ![Samanage-Bereitstellungsstatus](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

15. Definieren Sie die Benutzer oder Gruppen, die in Samanage bereitgestellt werden sollen. Wählen Sie im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus. Berücksichtigen Sie bei Auswahl der Option **Alle Benutzer und Gruppen synchronisieren** die im nachstehenden Abschnitt „Connectoreinschränkungen“ beschriebenen Einschränkungen.

    ![Samanage-Bereich](./media/samanage-provisioning-tutorial/ScopeSync.png)

16. Wählen Sie **Speichern** aus, wenn die Bereitstellung erfolgen kann.

    ![Speichern in Samanage](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


Dadurch wird die Erstsynchronisierung aller Benutzer oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung nimmt mehr Zeit in Anspruch als die folgenden Synchronisierungen. Diese erfolgen etwa alle 40 Minuten, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. 

Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und über Links zum Bereitstellungsaktivitätsbericht navigieren. Der Bericht beschreibt alle vom Azure AD-Bereitstellungsdienst in Samanage ausgeführten Aktionen.

Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-Einschränkungen

Wenn Sie die Option **Alle Benutzer und Gruppen synchronisieren** auswählen und einen Wert für das Samanage-Attribut **Rollen** festlegen, muss der Wert unter dem Feld **Standardwert bei NULL (optional)**  im folgenden Format ausgedrückt werden:

- {„displayName“:„role“}, wobei „role“ der gewünschte Standradwert ist.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps im Azure-Portal](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
