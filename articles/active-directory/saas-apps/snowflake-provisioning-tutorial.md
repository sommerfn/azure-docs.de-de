---
title: 'Tutorial: Konfigurieren von Snowflake für die automatische Benutzerbereitstellung mit Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Snowflake konfigurieren.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: f9ce85f4-0992-4bc6-8276-4b2efbce8dcb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 04d9ec8cad2404466d2df649df4d5c461768b76f
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68693583"
---
# <a name="tutorial-configure-snowflake-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Snowflake für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Snowflake und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Snowflake zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Dieser Connector befindet sich derzeit in der Public Preview-Phase. Weitere Informationen zu den allgemeinen Nutzungsbedingungen von Microsoft Azure für Previewfunktionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* Einen Azure AD-Mandanten.
* [Einen Snowflake-Mandanten](https://www.Snowflake.com/pricing/)
* Ein Benutzerkonto in Snowflake mit Administratorberechtigungen

## <a name="assigning-users-to-snowflake"></a>Zuweisen von Benutzern zu Snowflake

Azure Active Directory ermittelt anhand von *Zuweisungen*, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Snowflake benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen Snowflake anhand der folgenden Anweisungen zuweisen:
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-snowflake"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Snowflake

* Es wird empfohlen, Snowflake einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

* Beim Zuweisen eines Benutzers zu Snowflake müssen Sie eine gültige anwendungsspezifische Rolle (sofern verfügbar) im Dialogfeld für die Zuweisung auswählen. Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="setup-snowflake-for-provisioning"></a>Einrichten von Snowflake für die Bereitstellung

Bevor Sie Snowflake für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie in Snowflake die SCIM-Bereitstellung aktivieren.

> [!NOTE]
> Diese Integration befindet sich in Snowflake aktuell in der privaten Vorschau. Wenn Sie dieses Feature in Ihrem Snowflake-Konto aktivieren möchten, setzen Sie sich mit Ihrem Snowflake-Vertriebsbeauftragten in Verbindung.

1. Melden Sie sich bei Ihrer Snowflake-Administratorkonsole an. Geben Sie die unten angezeigte Abfrage im hervorgehobenen Arbeitsbereich ein, und klicken Sie auf **Ausführen**.

    ![Snowflake-Administratorkonsole](media/Snowflake-provisioning-tutorial/image00.png)

2.  Für Ihren Snowflake-Mandanten wird ein SCIM-Zugriffstoken generiert. Um dieses abzurufen, klicken Sie auf den unten hervorgehoben dargestellten Link.

    ![Snowflake: SCIM hinzufügen](media/Snowflake-provisioning-tutorial/image01.png)

3. Kopieren Sie den generierten Tokenwert, und klicken Sie auf **Done** (Fertig). Dieser Wert wird auf der Registerkarte „Bereitstellung“ für Ihre Snowflake-Anwendung im Azure-Portal in das Feld **Geheimes Token** eingegeben.

    ![Snowflake: SCIM hinzufügen](media/Snowflake-provisioning-tutorial/image02.png)

## <a name="add-snowflake-from-the-gallery"></a>Hinzufügen von Snowflake aus dem Katalog

Bevor Sie Snowflake für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Snowflake aus dem Azure AD-Anwendungskatalog zu Ihrer Liste mit verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie zum Hinzufügen von Snowflake aus dem Azure AD-Anwendungskatalog die folgenden Schritte aus:**

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Bereich auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie in das Suchfeld **Snowflake** ein, klicken Sie im Ergebnisbereich erst auf **Snowflake** und dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Snowflake in der Ergebnisliste](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-snowflake"></a>Konfigurieren der automatischen Benutzerbereitstellung für Snowflake 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Snowflake auf der Grundlage von Benutzer- oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Snowflake aktivieren. Befolgen Sie hierzu die Anweisungen im [SSO-Tutorial zu Snowflake](Snowflake-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-snowflake-in-azure-ad"></a>Gehen Sie wie folgt vor, um die automatische Benutzerbereitstellung für Snowflake in Azure AD zu konfigurieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Snowflake**aus.

    ![Snowflake-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt „Administratoranmeldeinformationen“ im Feld „Mandanten-URL“ den Wert `https://<Snowflake Account URL>/scim/v2` ein. Beispiel für die Mandanten-URL: `https://acme.snowflakecomputing.com/scim/v2`

6. Geben Sie den Wert des **SCIM-Authentifizierungstokens** ein, den Sie zuvor unter **geheimes Token** abgerufen haben. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Snowflake herstellen kann. Wenn die Verbindung nicht möglich ist, müssen Sie sicherstellen, dass Ihr Snowflake-Konto über Administratorberechtigungen verfügt. Versuchen Sie es anschließend noch mal.

    ![Mandanten-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

7. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

8. Klicken Sie auf **Speichern**.

9. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Snowflake synchronisieren** aus.

    ![Benutzerzuordnungen in Snowflake](media/Snowflake-provisioning-tutorial/user-mapping.png)

10. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Snowflake synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Snowflake für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Benutzerattribute in Snowflake](media/Snowflake-provisioning-tutorial/user-attribute.png)

11. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit Snowflake synchronisieren** aus.

    ![Gruppenzuordnungen in Snowflake](media/Snowflake-provisioning-tutorial/group-mapping.png)

12. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Gruppenattribute, die von Azure AD mit Snowflake synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Gruppen in Snowflake für Updatevorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Gruppenattribute in Snowflake](media/Snowflake-provisioning-tutorial/group-attribute.png)

13. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Um den Azure AD-Bereitstellungsdienst für Snowflake zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

15. Legen Sie die Benutzer und/oder Gruppen fest, die in Snowflake bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

16. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

    Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Snowflake ausgeführt werden.

    Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Reporting on automatic user account provisioning (Tutorial: Meldung zur automatischen Benutzerkontobereitstellung)](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-Einschränkungen

* Die von Snowflake generierten SCIM-Token laufen nach 6 Monaten ab. Beachten Sie, dass die Token vor ihrem Ablauf erneuert werden müssen, damit die Bereitstellungssynchronisierung weiterhin fortgesetzt werden kann. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte
* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen](../manage-apps/check-status-user-account-provisioning.md).