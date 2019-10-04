---
title: 'Tutorial: Konfigurieren von Foodee für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Foodee konfigurieren.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: fb48deae-4653-448a-ba2f-90258edab3a7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2019
ms.author: Zhchia
ms.openlocfilehash: 171a1141670e55814474390c59ae8d514491edbd
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71088091"
---
# <a name="tutorial-configure-foodee-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Foodee für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Foodee und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Foodee zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Dieser Connector befindet sich derzeit in der Public Preview-Phase. Weitere Informationen zu den allgemeinen Nutzungsbedingungen von Microsoft Azure für Previewfunktionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* Einen Azure AD-Mandanten.
* [Einen Foodee-Mandanten](https://www.food.ee/about/)
* Ein Benutzerkonto in Foodee mit Administratorberechtigungen.

## <a name="assigning-users-to-foodee"></a>Zuweisen von Benutzern zu Foodee 

Azure Active Directory ermittelt anhand von *Zuweisungen*, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Foodee benötigen. Anschließend können Sie Foodee diese Benutzer und/oder Gruppen zuweisen, indem Sie den folgenden Anweisungen folgen:
* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-foodee"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Foodee 

* Es wird empfohlen, Foodee einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

* Beim Zuweisen eines Benutzers zu Foodee müssen Sie im Dialogfeld für die Zuweisung eine gültige anwendungsspezifische Rolle auswählen (sofern verfügbar). Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="setup-foodee-for-provisioning"></a>Einrichten von Foodee für die Bereitstellung

Bevor Sie Foodee für die automatische Benutzerbereitstellung in Azure AD konfigurieren, müssen Sie in Foodee die SCIM-Bereitstellung aktivieren.

1. Melden Sie sich bei [Foodee](https://www.food.ee/login/) an. Klicken Sie auf Ihre **Mandanten-ID**.

    ![Foodee](media/Foodee-provisioning-tutorial/tenant.png)

2. Wählen Sie unter „Enterprise Portal“ (Unternehmensportal) die Option **Single Sign On** (Einmaliges Anmelden) aus.

    ![Foodee](media/Foodee-provisioning-tutorial/scim.png)

3. Kopieren Sie das **API-Token**. Diese Werte werden im Azure-Portal auf der Registerkarte „Bereitstellung“ Ihrer Foodee-Anwendung in das Feld **Geheimes Token** eingegeben.

    ![Foodee](media/Foodee-provisioning-tutorial/token.png)


## <a name="add-foodee-from-the-gallery"></a>Hinzufügen von Foodee aus dem Katalog

Um Foodee für die automatische Benutzerbereitstellung in Azure AD konfigurieren zu können, müssen Sie Ihrer Liste der verwalteten SaaS-Anwendungen Foodee aus dem Azure AD-Anwendungskatalog hinzufügen.

**Führen Sie die folgenden Schritte aus, um Foodee aus dem Azure AD-Anwendungskatalog hinzuzufügen:**

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Bereich auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld den Namen **Foodee** ein, wählen Sie im Ergebnisbereich die Anwendung **Foodee** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Foodee in der Ergebnisliste](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-foodee"></a>Konfigurieren der automatischen Benutzerbereitstellung für Foodee  

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts für das Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Foodee auf der Grundlage von Benutzer- und/oder Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Foodee aktivieren. Folgen Sie dazu den Anweisungen unter [Tutorial: Integrieren von Foodee in Azure Active Directory](Foodee-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-foodee-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Foodee in Azure AD:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste den Eintrag **Foodee** aus.

    ![Foodee-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt „Administratoranmeldeinformationen“ die zuvor abgerufenen Werte von ` https://concierge.food.ee/scim/v2` und **API-Token** in das Feld **Mandanten-URL** bzw. **Geheimes Token** ein. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Foodee herstellen kann. Wenn die Verbindung nicht hergestellt werden kann, stellen Sie sicher, dass Ihr Foodee-Konto über Administratorberechtigungen verfügt, und versuchen Sie es noch einmal.

    ![Mandanten-URL + Token](common/provisioning-testconnection-tenanturltoken.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Klicken Sie auf **Speichern**.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Foodee synchronisieren** aus.

    ![Foodee-Benutzerzuordnungen](media/Foodee-provisioning-tutorial/usermapping.png)

9. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Foodee synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Benutzerkonten in Foodee für Aktualisierungsvorgänge verwendet. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Foodee-Benutzerattribute](media/Foodee-provisioning-tutorial/userattribute.png)

10. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Gruppen mit Foodee synchronisieren** aus.

    ![Foodee-Benutzerattribute](media/Foodee-provisioning-tutorial/groupmapping.png)

11. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Foodee synchronisiert werden. Die als **übereinstimmende** Eigenschaften ausgewählten Attribute werden für den Abgleich der Gruppenkonten in Foodee für Aktualisierungsvorgänge verwendet. Wählen Sie die Schaltfläche **Speichern** aus, um alle Änderungen zu übernehmen.

    ![Foodee-Benutzerattribute](media/Foodee-provisioning-tutorial/groupattribute.png)

12. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Um den Azure AD-Bereitstellungsdienst für Foodee zu aktivieren, ändern Sie im Abschnitt **Einstellungen** den **Bereitstellungsstatus** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

14. Legen Sie die Benutzer und/oder Gruppen fest, die in Foodee bereitgestellt werden sollen, indem Sie im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte auswählen.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

15. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung nimmt mehr Zeit in Anspruch als die nachfolgenden Synchronisierungen. Unter [Überprüfen des Status der Benutzerbereitstellung](../manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users) finden Sie weitere Informationen dazu, wie lange das Bereitstellen von Benutzern und/oder Gruppen dauert.

Im Abschnitt **Aktueller Status** können Sie den Fortschritt überwachen und Links zu Ihrem Bereitstellungsaktivitätsbericht folgen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst für Foodee ausgeführt werden. Weitere Informationen finden Sie unter [Ermitteln, wann ein bestimmter Benutzer auf eine Anwendung zugreifen kann](../manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md). Informationen zum Lesen der Azure AD-Bereitstellungsprotokolle finden Sie unter [Tutorial: Berichterstellung zur automatischen Benutzerkontobereitstellung](../manage-apps/check-status-user-account-provisioning.md).



## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen.](../manage-apps/check-status-user-account-provisioning.md)
