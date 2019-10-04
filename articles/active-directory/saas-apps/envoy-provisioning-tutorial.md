---
title: 'Tutorial: Konfigurieren von Envoy für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Envoy konfigurieren.
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
ms.date: 06/3/2019
ms.author: jeedes
ms.openlocfilehash: df4c5895e15e7e9e63ad1f3d273af1c3fdab2e90
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672727"
---
# <a name="tutorial-configure-envoy-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Envoy für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Envoy und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD zum automatischen Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Envoy zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Dieser Connector befindet sich derzeit in der Public Preview-Phase. Weitere Informationen zu den allgemeinen Nutzungsbedingungen von Microsoft Azure für Previewfunktionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* Einen Azure AD-Mandanten
* [Einen Envoy-Mandanten](https://envoy.com/pricing/)
* Ein Benutzerkonto in Envoy mit Administratorrechten

## <a name="add-envoy-from-the-gallery"></a>Hinzufügen von Envoy aus dem Katalog

Bevor Sie Envoy für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Envoy aus dem Azure AD-Anwendungskatalog zu Ihrer Liste verwalteter SaaS-Anwendungen hinzufügen.

**Führen Sie die folgenden Schritte aus, um Envoy aus dem Azure AD-Anwendungskatalog hinzuzufügen:**

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Bereich auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Envoy** ein, wählen Sie im Ergebnisbereich **Envoy** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Envoy in der Ergebnisliste](common/search-new-app.png)

## <a name="assigning-users-to-envoy"></a>Zuweisen von Benutzern zu Envoy

Azure Active Directory ermittelt anhand von *Zuweisungen*, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Envoy benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen Envoy wie folgt zuweisen:

* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-envoy"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Envoy

* Es wird empfohlen, Envoy einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

* Beim Zuweisen eines Benutzers zu Envoy müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="configuring-automatic-user-provisioning-to-envoy"></a>Konfigurieren der automatischen Benutzerbereitstellung in Envoy 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Envoy auf Grundlage von Benutzer- und/oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Envoy aktivieren. Befolgen Sie dazu die Anweisungen im [SSO-Tutorial zu Envoy](envoy-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-envoy-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Envoy in Azure AD:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Envoy** aus.

    ![Envoy-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** im Feld **Mandanten-URL** die Zeichenfolge `https://app.envoy.com/scim/v2` ein. Führen Sie die Schritte der in Schritt 6 beschriebenen exemplarischen Vorgehensweise aus, um das **geheime Token** Ihres Envoy-Kontos abzurufen.

6. Melden Sie sich bei Ihrer [Envoy Admin-Konsole](https://dashboard.envoy.com/login) an. Klicken Sie auf **Integrationen**.

    ![„Integrationen“ in Envoy](media/envoy-provisioning-tutorial/envoy01.png)

    Klicken Sie für die **Microsoft Azure SCIM-Integration** auf **Installieren**.

    ![„Installieren“ in Envoy](media/envoy-provisioning-tutorial/envoy02.png)

    Klicken Sie für **Alle Benutzer synchronisieren** auf **Speichern**. 

    ![„Speichern“ in Envoy](media/envoy-provisioning-tutorial/envoy03.png)

    Rufen Sie das aufgefüllte geheime Token ab.
    
    ![OAUTH in Envoy](media/envoy-provisioning-tutorial/envoy04.png)

7. Klicken Sie nach dem Auffüllen der in Schritt 5 gezeigten Felder auf **Verbindung testen**, um sich zu vergewissern, dass Azure AD eine Verbindung mit Envoy herstellen kann. Falls der Verbindungsaufbau fehlschlägt, stellen Sie sicher, dass Ihr Envoy-Konto über Administratorberechtigungen verfügt, und versuchen Sie es noch einmal.

    ![Tokenverschlüsselung](common/provisioning-testconnection-tenanturltoken.png)

8. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

9. Klicken Sie auf **Speichern**.

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Envoy synchronisieren**.
    
    ![Benutzerattribute in Envoy](media/envoy-provisioning-tutorial/envoy-user-mappings.png)
    
11. Überprüfen Sie im Abschnitt **Attributzuordnung** die Benutzerattribute, die von Azure AD mit Envoy synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Envoy für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Benutzerattribute in Envoy](media/envoy-provisioning-tutorial/envoy-user-attribute.png)

12. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit Envoy synchronisieren** aus.

    ![Benutzerzuordnungen in Envoy](media/envoy-provisioning-tutorial/envoy-group-mapping.png)

13. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit Envoy synchronisiert werden. Die als **Übereinstimmend** ausgewählten Attribute werden verwendet, um die Gruppen in Envoy für Updatevorgänge abzugleichen. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Benutzerzuordnungen in Envoy](media/envoy-provisioning-tutorial/envoy-group-attributes.png)
    
14. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Um den Azure AD-Bereitstellungsdienst für Envoy zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

16. Legen Sie die Benutzer und/oder Gruppen fest, die in Envoy bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

17. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Envoy ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)

