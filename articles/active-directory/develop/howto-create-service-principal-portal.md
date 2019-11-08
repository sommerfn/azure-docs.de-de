---
title: Erstellen einer Identität für eine Azure-App im Portal
titleSuffix: Microsoft identity platform
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
ms.date: 10/14/2019
ms.author: ryanwi
ms.reviewer: tomfitz
ms.custom: aaddev, seoapril2019, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 03ae780f86512ac401fcb7f6936e8f74cb595ca7
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73473850"
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
1. Wählen Sie **Neue Registrierung** aus.
1. Weisen Sie der Anwendung einen Namen zu. Wählen Sie einen unterstützten Kontotyp aus, der bestimmt, von wem die Anwendung verwendet werden kann. Wählen Sie unter **Umleitungs-URI** als Typ der Anwendung, die Sie erstellen möchten, **Web** aus. Geben Sie den URI ein, an den das Zugriffstoken gesendet wird. Sie können keine Anmeldeinformationen für eine [native Anwendung](../manage-apps/application-proxy-configure-native-client-application.md) erstellen. Sie können diesen Typ nicht für eine automatisierte Anwendung verwenden. Wählen Sie nach dem Festlegen der Werte **Registrieren** aus.

   ![Einen Namen für Ihre Anwendung eingeben](./media/howto-create-service-principal-portal/create-app.png)

Sie haben Ihre Azure AD-Anwendung und den Dienstprinzipal erstellt.

## <a name="assign-the-application-to-a-role"></a>Zuweisen der Anwendung zu einer Rolle

Um auf Ressourcen in Ihrem Abonnement zuzugreifen, müssen Sie die Anwendung einer Rolle zuweisen. Entscheiden Sie, welche Rolle über die geeigneten Berechtigungen für die Anwendung verfügt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: Integrierte Rollen](../../role-based-access-control/built-in-roles.md).

Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. Wenn z.B. der Leserolle für eine Ressourcengruppe eine Anwendung hinzugefügt wird, kann diese Rolle die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen.

1. Wählen Sie im Azure-Portal die Bereichsebene aus, der Sie die Anwendung zuweisen möchten. Um beispielsweise eine Rolle im Abonnementbereich zuzuweisen, suchen Sie nach **Abonnements** und wählen diese Option aus, oder wählen Sie **Abonnements** auf der **Homepage** aus.

   ![Beispiel: Zuweisen einer Rolle im Abonnementkontext](./media/howto-create-service-principal-portal/select-subscription.png)

1. Wählen Sie das Abonnement aus, dem die Anwendung zugewiesen werden soll.

   ![Auswählen des Abonnements für die Zuweisung](./media/howto-create-service-principal-portal/select-one-subscription.png)

   Falls Sie das gesuchte Abonnement nicht finden können, können Sie den **globalen Abonnementfilter** verwenden. Stellen Sie sicher, dass das gewünschte Abonnement für das Portal ausgewählt ist.

1. Wählen Sie die Option **Zugriffssteuerung (IAM)** .
1. Wählen Sie **Rollenzuweisung hinzufügen** aus.
1. Wählen Sie die Rolle aus, die Sie der Anwendung zuweisen möchten. Wählen Sie beispielsweise die Rolle **Mitwirkender** aus, um für die Anwendung das Ausführen von Aktionen wie das **Neustarten**, **Starten** und **Beenden** von Instanzen durchzuführen.  Erfahren Sie mehr über die [verfügbaren Rollen](../../role-based-access-control/built-in-roles.md). Azure AD-Anwendungen werden standardmäßig nicht in den verfügbaren Optionen angezeigt. Suchen Sie nach dem Namen Ihrer Anwendung, und wählen Sie ihn aus.

   ![Rolle auswählen, die der Anwendung zugewiesen werden soll](./media/howto-create-service-principal-portal/select-role.png)

1. Wählen Sie **Speichern** aus, um das Zuweisen der Rolle abzuschließen. Ihre Anwendung wird in der Liste der Benutzer angezeigt, die einer Rolle für diesen Kontext zugewiesen sind.

Ihr Dienstprinzipal wird eingerichtet. Sie können mit der Nutzung beginnen, um Ihre Skripts oder Apps auszuführen. Im nächsten Abschnitt wird veranschaulicht, wie Sie Werte abrufen, die beim programmgesteuerten Anmelden benötigt werden.

## <a name="get-values-for-signing-in"></a>Abrufen von Werten für die Anmeldung

Beim programmgesteuerten Anmelden müssen Sie mit Ihrer Authentifizierungsanforderung die Mandanten-ID übergeben. Sie benötigen auch die ID für Ihre Anwendung und einen Authentifizierungsschlüssel. Führen Sie die folgenden Schritte aus, um diese Werte abzurufen:

1. Wählen Sie **Azure Active Directory**.
1. Wählen Sie in Azure AD unter **App-Registrierungen** Ihre Anwendung aus.
1. Kopieren Sie die Verzeichnis-ID (Mandant), und speichern Sie sie in Ihrem Anwendungscode.

    ![Verzeichnis-ID (Mandanten-ID) kopieren und im Anwendungscode speichern](./media/howto-create-service-principal-portal/copy-tenant-id.png)

1. Kopieren Sie die **Anwendungs-ID**, und speichern Sie sie in Ihrem Anwendungscode.

   ![Anwendungs-ID (Client-ID) kopieren](./media/howto-create-service-principal-portal/copy-app-id.png)

## <a name="certificates-and-secrets"></a>Zertifikate und Geheimnisse
Daemon-Anwendungen können zwei Formen von Anmeldeinformationen verwenden, um sich bei Azure AD zu authentifizieren: Zertifikate und Anwendungsgeheimnisse.  Wir empfehlen die Verwendung eines Zertifikats, Sie können aber auch ein neues Anwendungsgeheimnis erstellen.

### <a name="upload-a-certificate"></a>Hochladen eines Zertifikats

Sie können ein vorhandenes Zertifikat verwenden, wenn Sie eins besitzen.  Optional können Sie für Testzwecke ein selbstsigniertes Zertifikat erstellen. Öffnen Sie PowerShell, und führen Sie [New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) mit den folgenden Parametern aus, um ein selbstsigniertes Zertifikat im Benutzerzertifikatspeicher auf Ihrem Computer zu erstellen: 

```powershell
$cert=New-SelfSignedCertificate -Subject "CN=DaemonConsoleCert" -CertStoreLocation "Cert:\CurrentUser\My"  -KeyExportPolicy Exportable -KeySpec Signature
```

Exportieren Sie dieses Zertifikat mit dem MMC-Snap-In [Benutzerzertifikat verwalten](/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in), auf das Sie in der Windows-Systemsteuerung zugreifen können, in eine Datei.

So laden Sie das Zertifikat hoch:

1. Wählen Sie **Zertifikate & Geheimnisse** aus.
1. Wählen Sie **Zertifikat hochladen** aus, und wählen Sie dann das Zertifikat (ein vorhandenes Zertifikat oder das von Ihnen exportierte selbstsignierte Zertifikat) aus.

    ![„Zertifikat hochladen“ auswählen und das hinzuzufügende Zertifikat auswählen](./media/howto-create-service-principal-portal/upload-cert.png)

1. Wählen Sie **Hinzufügen**.

Nach dem Registrieren des Zertifikats bei Ihrer Anwendung im Portal der Anwendungsregistrierung müssen Sie dem Code der Clientanwendung die Verwendung des Zertifikats ermöglichen.

### <a name="create-a-new-application-secret"></a>Erstellen eines neuen Anwendungsgeheimnisses

Wenn Sie sich entscheiden, kein Zertifikat zu verwenden, können Sie ein neues Anwendungsgeheimnis erstellen.

1. Wählen Sie **Zertifikate & Geheimnisse** aus.
1. Wählen Sie **Geheime Clientschlüssel -> Neuer geheimer Clientschlüssel** aus.
1. Geben Sie eine Beschreibung des Geheimnisses und eine Dauer ein. Wählen Sie anschließend **Hinzufügen** aus.

   Nachdem der geheime Clientschlüssel gespeichert wurde, wird dessen Wert angezeigt. Kopieren Sie diesen Wert jetzt, da Sie den Schlüssel später nicht mehr abrufen können. Sie geben den Schlüsselwert zusammen mit der Anwendungs-ID an, um die Anmeldung als Anwendung durchzuführen. Speichern Sie die Schlüsselwert an einem Ort, von dem Ihre Anwendung ihn abrufen kann.

   ![Den geheimen Wert kopieren, weil er später nicht mehr abgerufen werden kann](./media/howto-create-service-principal-portal/copy-secret.png)

## <a name="configure-access-policies-on-resources"></a>Konfigurieren von Zugriffsrichtlinien für Ressourcen
Beachten Sie, dass Sie möglicherweise zusätzliche Berechtigungen für Ressourcen konfigurieren müssen, auf die Ihre Anwendung zugreifen muss. Sie müssen beispielsweise auch die [Zugriffsrichtlinien eines Schlüsseltresors aktualisieren](/azure/key-vault/key-vault-secure-your-key-vault#data-plane-and-access-policies), um der Anwendung Zugriff auf Schlüssel, Geheimnisse oder Zertifikate zu gewähren.  

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Schlüsseltresor, und wählen Sie **Zugriffsrichtlinien** aus.  
1. Wählen Sie **Zugriffsrichtlinie hinzufügen** und anschließend die Schlüssel-, Geheimnis- und Zertifikatberechtigungen aus, die Sie Ihrer Anwendung gewähren möchten.  Wählen Sie den zuvor erstellten Dienstprinzipal aus.
1. Wählen Sie **Hinzufügen** aus, um die Zugriffsrichtlinie hinzuzufügen, und wählen Sie dann **Speichern** aus, um Ihre Änderungen zu übernehmen.
    ![Zugriffsrichtlinie hinzufügen](./media/howto-create-service-principal-portal/add-access-policy.png)

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Sie müssen über ausreichende Berechtigungen verfügen, um eine Anwendung bei Ihrem Azure AD-Mandanten zu registrieren und die Anwendung einer Rolle in Ihrem Azure-Abonnement zuzuweisen.

### <a name="check-azure-ad-permissions"></a>Überprüfen der Azure AD-Berechtigungen

1. Wählen Sie **Azure Active Directory**.
1. Achten Sie auf Ihre Rolle. Wenn Sie über die Rolle **Benutzer** verfügen, müssen Sie sicherstellen, dass keine Administratorrechte benötigt werden, um Anwendungen zu registrieren.

   ![Suchen Sie Ihre Rolle. Wenn Ihnen die Rolle „Benutzer“ zugewiesen ist, stellen Sie sicher, dass Nicht-Administratoren Apps registrieren können.](./media/howto-create-service-principal-portal/view-user-info.png)

1. Wählen Sie im linken Bereich **Benutzereinstellungen** aus.
1. Überprüfen Sie die Einstellung **App Registrierungen**. Dieser Wert kann nur von einem Administrator festgelegt werden. Die Einstellung **Ja** bedeutet, dass jeder Benutzer im Azure AD-Mandanten eine App registrieren kann.

Wenn die App-Registrierungseinstellung auf **Nein** festgelegt ist, können nur Benutzer mit Administratorrolle diese Arten von Anwendungen registrieren. Informationen zu verfügbaren Administratorrollen und den spezifischen Berechtigungen in Azure AD für die einzelnen Rollen finden Sie unter [verfügbare Rollen](../users-groups-roles/directory-assign-admin-roles.md#available-roles) und [Rollenberechtigungen](../users-groups-roles/directory-assign-admin-roles.md#role-permissions). Wenn Ihrem Konto die Rolle „Benutzer“ zugewiesen wurde, die App-Registrierungseinstellung jedoch auf Administratoren begrenzt ist, bitten Sie Ihren Administrator, entweder Ihnen die Rolle „Globaler Administrator“ zuzuweisen, die alle Aspekte von App-Registrierungen erstellen und verwalten kann, oder Benutzern das Registrieren von Apps zu ermöglichen.

### <a name="check-azure-subscription-permissions"></a>Überprüfen der Berechtigungen des Azure-Abonnements

Ihr Konto muss in Ihrem Azure-Abonnement über `Microsoft.Authorization/*/Write`-Zugriff verfügen, um einer Rolle eine AD-App zuzuweisen. Diese Aktion wird über die Rolle [Besitzer](../../role-based-access-control/built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](../../role-based-access-control/built-in-roles.md#user-access-administrator) gewährt. Wenn Ihr Konto der Rolle **Mitwirkender** zugewiesen ist, verfügen Sie nicht über die erforderliche Berechtigung. Beim Versuch, einer Rolle den Dienstprinzipal zuzuweisen, erhalten Sie eine Fehlermeldung.

So überprüfen Sie die Berechtigungen Ihres Abonnements

1. Wählen Sie oben rechts Ihr Konto und dann die Option **Meine Berechtigungen** aus.

   ![Ihr Konto und Ihre Benutzerberechtigungen auswählen](./media/howto-create-service-principal-portal/select-my-permissions.png)

1. Wählen Sie in der Dropdownliste das Abonnement aus, unter dem Sie den Dienstprinzipal erstellen möchten. Wählen Sie anschließend **Klicken Sie hier, um die vollständigen Zugangsdaten für dieses Abonnement anzuzeigen** aus.

   ![Das Abonnement auswählen, in dem der Dienstprinzipal erstellt werden soll](./media/howto-create-service-principal-portal/view-details.png)

1. Wählen Sie **Rollenzuweisungen** aus, um Ihre zugewiesenen Rollen anzuzeigen, und ermitteln Sie, ob Sie über die erforderlichen Berechtigungen verfügen, um einer Rolle eine AD-App zuzuweisen. Wenn dies nicht der Fall ist, bitten Sie Ihren Abonnementadministrator, Sie zur Rolle „Benutzerzugriffsadministrator“ hinzuzufügen. In der folgenden Abbildung wurde der Benutzer der Rolle „Besitzer“ zugeordnet. Dies bedeutet, dass dieser Benutzer über die erforderlichen Berechtigungen verfügt.

   ![Beispiel für den Benutzer, dem die Rolle „Besitzer“ zugewiesen ist](./media/howto-create-service-principal-portal/view-user-role.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Festlegen von Sicherheitsrichtlinien finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](../../role-based-access-control/role-assignments-portal.md).  
* Eine Liste der verfügbaren Aktionen, die Benutzern erteilt oder verweigert werden können, finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../../role-based-access-control/resource-provider-operations.md).
