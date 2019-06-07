---
title: Erstellen einer Identität für die Azure-App im Portal | Microsoft-Dokumentation
description: Beschreibt das Erstellen einer neuen Azure Active Directory-Anwendung und eines Dienstprinzipals, der mit der rollenbasierten Zugriffskontrolle in Azure Resource Manager zum Verwalten des Zugriffs auf Ressourcen verwendet werden kann.
services: active-directory
documentationcenter: na
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2019
ms.author: ryanwi
ms.reviewer: tomfitz
ms.custom: seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0208d25e4583672ad2110d959f8e255affbf3e0
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2019
ms.locfileid: "65764850"
---
# <a name="how-to-use-the-portal-to-create-an-azure-ad-application-and-service-principal-that-can-access-resources"></a>Gewusst wie: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal

In diesem Artikel wird beschrieben, wie Sie eine neue Azure Active Directory-Anwendung (Azure AD) und einen Dienstprinzipal erstellen, der mit der rollenbasierten Zugriffssteuerung verwendet werden kann. Wenn Sie über Code verfügen, für den der Zugriff auf oder die Änderung von Ressourcen erforderlich ist, können Sie eine Identität für die App erstellen. Diese Identität wird als Dienstprinzipal bezeichnet. Sie können die erforderlichen Berechtigungen dann dem Dienstprinzipal zuweisen. In diesem Artikel wird veranschaulicht, wie Sie das Portal zum Erstellen des Dienstprinzipals verwenden. Es konzentriert sich auf eine Anwendung mit nur einem Mandanten, die nur zur Ausführung in einer einzigen Organisation vorgesehen ist. Anwendungen mit nur einem Mandanten werden in der Regel für innerhalb Ihrer Organisation ausgeführte Branchenanwendungen verwendet.

> [!IMPORTANT]
> Anstatt einen Dienstprinzipal zu erstellen, sollten Sie die Verwendung verwalteter Identitäten für Azure-Ressourcen für Ihre Anwendungsidentität erwägen. Wenn Ihr Code unter einem Dienst ausgeführt wird, der verwaltete Identitäten unterstützt und auf Ressourcen zugreift, die die Azure AD-Authentifizierung unterstützen, sind verwaltete Identitäten für Sie die besser geeignete Option. Weitere Informationen zu verwalteten Identitäten für Azure-Ressourcen (z.B. auch zu den Diensten, die diese zurzeit unterstützen) finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../managed-identities-azure-resources/overview.md).

## <a name="create-an-azure-active-directory-application"></a>Erstellen einer Azure Active Directory-Anwendung

Wir beginnen gleich mit der Erstellung der Identität. Falls ein Problem auftritt, sollten Sie die [erforderlichen Berechtigungen](#required-permissions) überprüfen, um sicherzustellen, dass die Identität mit Ihrem Konto erstellt werden kann.

1. Melden Sie sich über das [Azure-Portal](https://portal.azure.com) bei Ihrem Azure-Konto an.
1. Wählen Sie **Azure Active Directory**.
1. Wählen Sie **App-Registrierungen** aus.

   ![Auswählen von App-Registrierungen](./media/howto-create-service-principal-portal/select-app-registrations.png)

1. Wählen Sie **Registrierung einer neuen Anwendung** aus.

   ![Hinzufügen der App](./media/howto-create-service-principal-portal/select-add-app.png)

1. Geben Sie einen Namen und eine URL für die Anwendung an. Wählen Sie als Typ für die zu erstellende Anwendung **Web-App/API** aus. Sie können keine Anmeldeinformationen für eine [native Anwendung](../manage-apps/application-proxy-configure-native-client-application.md) erstellen. Sie können diesen Typ nicht für eine automatisierte Anwendung verwenden. Wählen Sie nach dem Festlegen der Werte **Erstellen** aus.

   ![Benennen der Anwendung](./media/howto-create-service-principal-portal/create-app.png)

Sie haben Ihre Azure AD-Anwendung und den Dienstprinzipal erstellt.

## <a name="assign-the-application-to-a-role"></a>Zuweisen der Anwendung zu einer Rolle

Um auf Ressourcen in Ihrem Abonnement zuzugreifen, müssen Sie die Anwendung einer Rolle zuweisen. Entscheiden Sie, welche Rolle über die geeigneten Berechtigungen für die Anwendung verfügt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: Integrierte Rollen](../../role-based-access-control/built-in-roles.md).

Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. Wenn z.B. der Leserolle für eine Ressourcengruppe eine Anwendung hinzugefügt wird, kann diese Rolle die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen.

1. Navigieren Sie zur Bereichsebene, der Sie die Anwendung zuweisen möchten. Um z. B. einer Gruppe im Abonnementkontext eine Rolle zuzuweisen, wählen Sie **Alle Dienste** und **Abonnements** aus.

   ![Wählen Sie das Abonnement aus.](./media/howto-create-service-principal-portal/select-subscription.png)

1. Wählen Sie das Abonnement aus, dem die Anwendung zugewiesen werden soll.

   ![Auswählen des Abonnements für die Zuweisung](./media/howto-create-service-principal-portal/select-one-subscription.png)

   Falls Sie das gesuchte Abonnement nicht finden können, können Sie den **globalen Abonnementfilter** verwenden. Stellen Sie sicher, dass das gewünschte Abonnement für das Portal ausgewählt ist. 

1. Wählen Sie die Option **Zugriffssteuerung (IAM)** .
1. Wählen Sie **Rollenzuweisung hinzufügen** aus.

   ![Auswählen von „Rollenzuweisung hinzufügen“](./media/howto-create-service-principal-portal/select-add.png)

1. Wählen Sie die Rolle aus, die Sie der Anwendung zuweisen möchten. Wählen Sie die Rolle **Mitwirkender**, um für die Anwendung das Ausführen von Aktionen wie das **Neustarten**, **Starten** und **Beenden** von Instanzen durchzuführen. Azure AD-Anwendungen werden standardmäßig nicht in den verfügbaren Optionen angezeigt. Suchen Sie nach dem Namen Ihrer Anwendung, und wählen Sie ihn aus.

   ![Rolle wählen](./media/howto-create-service-principal-portal/select-role.png)

1. Wählen Sie **Speichern** aus, um das Zuweisen der Rolle abzuschließen. Ihre Anwendung wird in der Liste der Benutzer angezeigt, die einer Rolle für diesen Kontext zugewiesen sind.

Ihr Dienstprinzipal wird eingerichtet. Sie können mit der Nutzung beginnen, um Ihre Skripts oder Apps auszuführen. Im nächsten Abschnitt wird veranschaulicht, wie Sie Werte abrufen, die beim programmgesteuerten Anmelden benötigt werden.

## <a name="get-values-for-signing-in"></a>Abrufen von Werten für die Anmeldung

### <a name="get-tenant-id"></a>Abrufen der Mandanten-ID

Beim programmgesteuerten Anmelden müssen Sie mit Ihrer Authentifizierungsanforderung die Mandanten-ID übergeben.

1. Wählen Sie **Azure Active Directory**.
1. Wählen Sie **Eigenschaften** aus.

   ![Auswählen von Azure AD-Eigenschaften](./media/howto-create-service-principal-portal/select-ad-properties.png)

1. Kopieren Sie die **Verzeichnis-ID**, um Ihre Mandanten-ID abzurufen.

   ![Mandanten-ID](./media/howto-create-service-principal-portal/copy-directory-id.png)

### <a name="get-application-id-and-authentication-key"></a>Abrufen der Anwendungs-ID und des Authentifizierungsschlüssels

Sie benötigen auch die ID für Ihre Anwendung und einen Authentifizierungsschlüssel. Führen Sie die folgenden Schritte aus, um diese Werte abzurufen:

1. Wählen Sie in Azure AD unter **App-Registrierungen** Ihre Anwendung aus.

   ![Auswählen einer Anwendung](./media/howto-create-service-principal-portal/select-app.png)

1. Kopieren Sie die **Anwendungs-ID**, und speichern Sie sie in Ihrem Anwendungscode.

   ![Client-ID](./media/howto-create-service-principal-portal/copy-app-id.png)

1. Wählen Sie **Zertifikate & Geheimnisse** aus.

   ![Klicken auf „Einstellungen“](./media/howto-create-service-principal-portal/select-certs-secrets.png)

1. Wählen Sie **Geheime Clientschlüssel -> Neuer geheimer Clientschlüssel** aus.
1. Geben Sie eine Beschreibung des Geheimnisses und eine Dauer ein. Wählen Sie anschließend **Hinzufügen** aus.

   ![Speichern des geheimen Clientschlüssels](./media/howto-create-service-principal-portal/save-secret.png)

   Nachdem der geheime Clientschlüssel gespeichert wurde, wird dessen Wert angezeigt. Kopieren Sie diesen Wert jetzt, da Sie den Schlüssel später nicht mehr abrufen können. Sie geben den Schlüsselwert zusammen mit der Anwendungs-ID an, um die Anmeldung als Anwendung durchzuführen. Speichern Sie die Schlüsselwert an einem Ort, von dem Ihre Anwendung ihn abrufen kann.

   ![Kopieren des geheimen Clientschlüssels](./media/howto-create-service-principal-portal/copy-secret.png)

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Sie müssen über ausreichende Berechtigungen verfügen, um eine Anwendung bei Ihrem Azure AD-Mandanten zu registrieren und die Anwendung einer Rolle in Ihrem Azure-Abonnement zuzuweisen.

### <a name="check-azure-ad-permissions"></a>Überprüfen der Azure AD-Berechtigungen

1. Wählen Sie **Azure Active Directory**.
1. Achten Sie auf Ihre Rolle. Wenn Sie über die Rolle **Benutzer** verfügen, müssen Sie sicherstellen, dass keine Administratorrechte benötigt werden, um Anwendungen zu registrieren.

   ![Suchen nach Benutzern](./media/howto-create-service-principal-portal/view-user-info.png)

1. Wählen Sie **Benutzereinstellungen** aus.

   ![Auswählen von Benutzereinstellungen](./media/howto-create-service-principal-portal/select-user-settings.png)

1. Überprüfen Sie die Einstellung **App Registrierungen**. Dieser Wert kann nur von einem Administrator festgelegt werden. Die Einstellung **Ja** bedeutet, dass jeder Benutzer im Azure AD-Mandanten eine App registrieren kann.

   ![Anzeigen von App-Registrierungen](./media/howto-create-service-principal-portal/view-app-registrations.png)

Wenn die App-Registrierungseinstellung auf **Nein** festgelegt ist, können nur Benutzer mit Administratorrolle diese Arten von Anwendungen registrieren. Informationen zu verfügbaren Administratorrollen und den spezifischen Berechtigungen in Azure AD für die einzelnen Rollen finden Sie unter [verfügbare Rollen](../users-groups-roles/directory-assign-admin-roles.md#available-roles) und [Rollenberechtigungen](../users-groups-roles/directory-assign-admin-roles.md#role-permissions). Wenn Ihrem Konto die Rolle „Benutzer“ zugewiesen wurde, die App-Registrierungseinstellung jedoch auf Administratoren begrenzt ist, bitten Sie Ihren Administrator, entweder Ihnen die Rolle „Globaler Administrator“ zuzuweisen, die alle Aspekte von App-Registrierungen erstellen und verwalten kann, oder Benutzern das Registrieren von Apps zu ermöglichen.

### <a name="check-azure-subscription-permissions"></a>Überprüfen der Berechtigungen des Azure-Abonnements

Ihr Konto muss in Ihrem Azure-Abonnement über `Microsoft.Authorization/*/Write`-Zugriff verfügen, um einer Rolle eine AD-App zuzuweisen. Diese Aktion wird über die Rolle [Besitzer](../../role-based-access-control/built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](../../role-based-access-control/built-in-roles.md#user-access-administrator) gewährt. Wenn Ihr Konto der Rolle **Mitwirkender** zugewiesen ist, verfügen Sie nicht über die erforderliche Berechtigung. Beim Versuch, einer Rolle den Dienstprinzipal zuzuweisen, erhalten Sie eine Fehlermeldung.

So überprüfen Sie die Berechtigungen Ihres Abonnements

1. Wählen Sie oben rechts Ihr Konto und dann die Option **Meine Berechtigungen** aus.

   ![Auswählen von Benutzerberechtigungen](./media/howto-create-service-principal-portal/select-my-permissions.png)

1. Wählen Sie in der Dropdownliste das Abonnement aus, unter dem Sie den Dienstprinzipal erstellen möchten. Wählen Sie anschließend **Klicken Sie hier, um die vollständigen Zugangsdaten für dieses Abonnement anzuzeigen** aus.

   ![Suchen nach Benutzern](./media/howto-create-service-principal-portal/view-details.png)

1. Wählen Sie **Rollenzuweisungen** aus, um Ihre zugewiesenen Rollen anzuzeigen, und ermitteln Sie, ob Sie über die erforderlichen Berechtigungen verfügen, um einer Rolle eine AD-App zuzuweisen. Wenn dies nicht der Fall ist, bitten Sie Ihren Abonnementadministrator, Sie zur Rolle „Benutzerzugriffsadministrator“ hinzuzufügen. In der folgenden Abbildung wurde der Benutzer der Rolle „Besitzer“ zugeordnet. Dies bedeutet, dass dieser Benutzer über die erforderlichen Berechtigungen verfügt.

   ![Anzeigen von Berechtigungen](./media/howto-create-service-principal-portal/view-user-role.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Einrichten einer Anwendung mit mehreren Mandanten finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](../../azure-resource-manager/resource-manager-api-authentication.md).
* Informationen zum Festlegen von Sicherheitsrichtlinien finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../../role-based-access-control/role-assignments-portal.md).  
* Eine Liste der verfügbaren Aktionen, die Benutzern erteilt oder verweigert werden können, finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../../role-based-access-control/resource-provider-operations.md).
