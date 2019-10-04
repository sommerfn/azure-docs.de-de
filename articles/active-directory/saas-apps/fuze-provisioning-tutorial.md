---
title: 'Tutorial: Konfigurieren von Fuze für die automatische Benutzerbereitstellung in Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie Azure Active Directory für das automatische Bereitstellen und Aufheben der Bereitstellung von Benutzerkonten in Fuze konfigurieren.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 34718201-4f0e-4260-9af0-b3b70a1e8265
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 937ed160a6a4ee1e08070abd177bd5853f35e9e4
ms.sourcegitcommit: 15f7b641a67f3d6cf4fb4b4c11eaee18cf335923
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602199"
---
# <a name="tutorial-configure-fuze-for-automatic-user-provisioning"></a>Tutorial: Konfigurieren von Fuze für die automatische Benutzerbereitstellung

In diesem Tutorial werden die Schritte erläutert, die in Fuze und Azure Active Directory (Azure AD) ausgeführt werden müssen, um Azure AD zum automatischen Bereitstellen und Aufheben der Bereitstellung von Benutzern und/oder Gruppen in Fuze zu konfigurieren.

> [!NOTE]
> In diesem Tutorial wird ein Connector beschrieben, der auf dem Benutzerbereitstellungsdienst von Azure AD basiert. Wichtige Details zum Zweck und zur Funktionsweise dieses Diensts sowie häufig gestellte Fragen finden Sie unter [Automatisieren der Bereitstellung und Bereitstellungsaufhebung von Benutzern für SaaS-Anwendungen mit Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Dieser Connector befindet sich derzeit in der Public Preview-Phase. Weitere Informationen zu den allgemeinen Nutzungsbedingungen von Microsoft Azure für Previewfunktionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Das diesem Tutorial zu Grunde liegende Szenario setzt voraus, dass Sie bereits über die folgenden Voraussetzungen verfügen:

* Einen Azure AD-Mandanten.
* Einen [Fuze-Mandanten](https://www.fuze.com/)
* Ein Benutzerkonto in Fuze mit Administratorberechtigungen

## <a name="assigning-users-to-fuze"></a>Zuweisen von Benutzern zu Fuze

Azure Active Directory ermittelt anhand von *Zuweisungen*, welche Benutzer Zugriff auf bestimmte Apps erhalten sollen. Im Kontext der automatischen Benutzerbereitstellung werden nur die Benutzer und/oder Gruppen synchronisiert, die einer Anwendung in Azure AD zugewiesen wurden.

Vor dem Konfigurieren und Aktivieren der automatischen Benutzerbereitstellung müssen Sie entscheiden, welche Benutzer und/oder Gruppen in Azure AD Zugriff auf Fuze benötigen. Anschließend können Sie diese Benutzer und/oder Gruppen Fuze wie folgt zuweisen:

* [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-fuze"></a>Wichtige Tipps zum Zuweisen von Benutzern zu Fuze

* Es wird empfohlen, Fuze einen einzelnen Azure AD-Benutzer zuzuweisen, um die Konfiguration der automatischen Benutzerbereitstellung zu testen. Später können weitere Benutzer und/oder Gruppen zugewiesen werden.

* Beim Zuweisen eines Benutzers zu Fuze müssen Sie im Dialogfeld für die Zuweisung eine gültige anwendungsspezifische Rolle auswählen (sofern verfügbar). Benutzer mit der Rolle **Standardzugriff** werden von der Bereitstellung ausgeschlossen.

## <a name="setup-fuze-for-provisioning"></a>Einrichten von Fuze für die Bereitstellung

Bevor Sie Fuze für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie in Fuze die SCIM-Bereitstellung aktivieren. 

1. Wenden Sie sich zunächst an den für Sie zuständigen Fuze-Vertreter, um die folgenden erforderlichen Informationen einzuholen:

    * Liste der derzeit in Ihrem Unternehmen verwendeten Fuze-Produkt-SKUs
    * Liste der Standortcodes für die Standorte Ihres Unternehmens
    * Liste der Abteilungscodes für Ihr Unternehmen

2. Diese SKUs und Codes können Sie Ihrem Fuze-Vertrag sowie den Konfigurationsdokumenten entnehmen oder beim für Sie zuständigen Fuze-Vertreter anfordern.

3. Sobald die Anforderungen erfüllt sind, erhalten Sie von dem für Sie zuständigen Fuze-Vertreter das Fuze-Authentifizierungstoken, das zum Aktivieren der Integration erforderlich ist. Dieser Wert wird im Azure-Portal auf der Registerkarte „Bereitstellung“ Ihrer Fuze-Anwendung in das Feld „Geheimes Token“ eingegeben.

## <a name="add-fuze-from-the-gallery"></a>Hinzufügen von Fuze aus dem Katalog

Bevor Sie Fuze für die automatische Benutzerbereitstellung mit Azure AD konfigurieren, müssen Sie Fuze aus dem Azure AD-Anwendungskatalog der Liste mit den verwalteten SaaS-Anwendungen hinzufügen.

**Führen Sie zum Hinzufügen von Fuze aus dem Azure AD-Anwendungskatalog die folgenden Schritte aus:**

1. Wählen Sie im **[Azure-Portal](https://portal.azure.com)** im linken Navigationsbereich **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Bereich auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **Fuze** ein, wählen Sie im Ergebnisbereich **Fuze** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Fuze in der Ergebnisliste](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-fuze"></a>Konfigurieren der automatischen Benutzerbereitstellung in Fuze 

In diesem Abschnitt werden die Schritte zum Konfigurieren des Azure AD-Bereitstellungsdiensts zum Erstellen, Aktualisieren und Deaktivieren von Benutzern und/oder Gruppen in Fuze auf der Grundlage von Benutzer- bzw. Gruppenzuweisungen in Azure AD erläutert.

> [!TIP]
> Sie können auch das SAML-basierte einmalige Anmelden für Fuze aktivieren. Befolgen Sie hierzu die Anweisungen unter [Tutorial: Azure Active Directory-Integration mit Fuze](fuze-tutorial.md). Einmaliges Anmelden kann unabhängig von der automatischen Benutzerbereitstellung konfiguriert werden, obwohl diese beiden Features einander ergänzen.

### <a name="to-configure-automatic-user-provisioning-for-fuze-in-azure-ad"></a>So konfigurieren Sie die automatische Benutzerbereitstellung für Fuze in Azure AD:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Fuze** aus.

    ![Fuze-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie die Registerkarte **Bereitstellung**.

    ![Registerkarte „Bereitstellung“](common/provisioning.png)

4. Legen Sie den **Bereitstellungsmodus** auf **Automatisch** fest.

    ![Registerkarte „Bereitstellung“](common/provisioning-automatic.png)

5. Geben Sie im Abschnitt **Administratoranmeldeinformationen** im Feld **Mandanten-URL** die Zeichenfolge `https://api.fuze.com/scim/v2` ein. Geben Sie den Wert des **SCIM-Authentifizierungstokens**, den Sie zuvor vom Fuze-Vertreter erhalten haben, in das Feld **Geheimes Token** ein. Klicken Sie auf **Verbindung testen**, um sicherzustellen, dass Azure AD eine Verbindung mit Fuze herstellen kann. Vergewissern Sie sich im Falle eines Verbindungsfehlers, dass Ihr Fuze-Konto über Administratorberechtigungen verfügt, und wiederholen Sie den Vorgang.

    ![Mandanten-URL und Token](common/provisioning-testconnection-tenanturltoken.png)

6. Geben Sie im Feld **Benachrichtigungs-E-Mail** die E-Mail-Adresse einer Person oder einer Gruppe ein, die Benachrichtigungen zu Bereitstellungsfehlern erhalten soll, und aktivieren Sie das Kontrollkästchen **Bei Fehler E-Mail-Benachrichtigung senden**.

    ![Benachrichtigungs-E-Mail](common/provisioning-notification-email.png)

7. Klicken Sie auf **Speichern**.

8. Wählen Sie im Abschnitt **Zuordnungen** die Option **Azure Active Directory-Benutzer mit Fuze synchronisieren** aus.

    ![Benutzerzuordnungen in Fuze](media/fuze-provisioning-tutorial/image01.png)

9. Überprüfen Sie im Abschnitt **Attributzuordnungen** die Benutzerattribute, die von Azure AD mit Fuze synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzerkonten in Fuze für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche **Speichern**, um alle Änderungen zu übernehmen.

    ![Benutzerzuordnungen in Fuze](media/fuze-provisioning-tutorial/image00.png)

10. Wenn Sie Bereichsfilter konfigurieren möchten, lesen Sie die Anweisungen unter [Attributbasierte Anwendungsbereitstellung mit Bereichsfiltern](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

11. Um den Azure AD-Bereitstellungsdienst für Fuze zu aktivieren, ändern Sie den **Bereitstellungsstatus** im Abschnitt **Einstellungen** in **Ein**.

    ![Aktivierter Bereitstellungsstatus](common/provisioning-toggle-on.png)

12. Legen Sie die Benutzer und/oder Gruppen fest, die in Fuze bereitgestellt werden sollen. Wählen Sie dazu im Abschnitt **Einstellungen** unter **Bereich** die gewünschten Werte aus.

    ![Bereitstellungsbereich](common/provisioning-scope.png)

15. Wenn Sie fertig sind, klicken Sie auf **Speichern**.

    ![Speichern der Bereitstellungskonfiguration](common/provisioning-configuration-save.png)

Dadurch wird die Erstsynchronisierung aller Benutzer und/oder Gruppen gestartet, die im Abschnitt **Einstellungen** unter **Bereich** definiert sind. Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 40 Minuten erfolgen, solange der Azure AD-Bereitstellungsdienst ausgeführt wird. Im Abschnitt **Synchronisierungsdetails** können Sie den Fortschritt überwachen und Links zu Berichten zur Bereitstellungsaktivität aufrufen. Darin sind alle Aktionen aufgeführt, die vom Azure AD-Bereitstellungsdienst in Fuze ausgeführt werden.

Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Connector-Einschränkungen

* Fuze unterstützt benutzerdefinierte SCIM-Attribute namens **Berechtigungen**. Diese Attribute können nur erstellt und nicht aktualisiert werden. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Verwalten der Benutzerkontobereitstellung für Unternehmens-Apps im Azure-Portal](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie, wie Sie Protokolle überprüfen und Berichte zu Bereitstellungsaktivitäten abrufen](../manage-apps/check-status-user-account-provisioning.md).