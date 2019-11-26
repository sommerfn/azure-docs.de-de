---
title: Authentifizieren mit verwalteten Identitäten – Azure Logic Apps
description: Zugreifen auf Ressourcen in anderen Azure Active Directory-Mandanten ohne Anmeldung mit Anmeldeinformationen oder Geheimnissen unter Verwendung einer verwalteten Identität
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 10/21/2019
ms.openlocfilehash: 2d1dbde2499dbe793a895f894e5ae83c36c54449
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73200629"
---
# <a name="authenticate-access-to-azure-resources-by-using-managed-identities-in-azure-logic-apps"></a>Authentifizieren des Zugriffs auf Azure-Ressourcen mithilfe verwalteter Identitäten in Azure Logic Apps

Zum Zugreifen auf die Ressourcen in anderen Azure Active Directory-Mandanten (Azure AD) und Authentifizieren Ihrer Identität ohne Anmeldung können Sie in Ihrer Logik-App die systemseitig zugewiesene [verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) (früher als verwaltete Dienstidentität (Managed Service Identity, MSI) bezeichnet) anstelle von Anmeldeinformationen oder Geheimnissen verwenden. Azure verwaltet diese Identität für Sie und dient als Hilfe beim Schützen Ihrer Anmeldeinformationen, da Sie keine Geheimnisse angeben oder eine Rotation dafür durchführen müssen. In diesem Artikel wird beschrieben, wie Sie die systemseitig zugewiesene verwaltete Identität in der Logik-App einrichten und verwenden. Derzeit funktionieren verwaltete Identitäten nur mit [spezifischen integrierten Triggern und Aktionen](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-to-outbound-calls), nicht aber mit verwalteten Connectors oder Verbindungen.

Weitere Informationen finden Sie in den folgenden Themen:

* [Trigger und Aktionen, die verwaltete Identitäten unterstützen](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
* [Azure-Dienste, die verwaltete Identitäten für die Azure AD-Authentifizierung unterstützen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)
* [Unterstützte Authentifizierungstypen bei ausgehenden Aufrufen](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
* [Grenzwerte für verwaltete Identitäten für Logik-Apps](../logic-apps/logic-apps-limits-and-config.md#managed-identity)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). Für die verwaltete Identität sowie die Azure-Zielressource, auf die Sie zugreifen möchten, muss dasselbe Azure-Abonnement verwendet werden.

* [Azure AD-Administratorberechtigungen](../active-directory/users-groups-roles/directory-assign-admin-roles.md), mit denen Rollen zu verwalteten Identitäten im gleichen Azure AD-Mandanten wie die Zielressource zugewiesen werden können. Damit eine verwaltete Identität Zugriff auf eine Azure-Ressource erhält, müssen Sie eine Rolle für diese Identität in der Zielressource hinzufügen.

* Die Azure-Zielressource, auf die Sie zugreifen möchten

* Eine Logik-App, in der [Trigger und Aktionen verwendet werden, die verwaltete Identitäten unterstützen](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)

<a name="system-assigned"></a>

## <a name="enable-system-assigned-identity"></a>Aktivieren systemseitig zugewiesener Identitäten

Im Unterschied zu benutzerseitig zugewiesenen Identitäten müssen Sie systemseitig zugewiesene Identitäten nicht manuell erstellen. Zum Einrichten einer systemseitig zugewiesenen Identität der Logik-App stehen folgende Optionen zur Verfügung:

* [Azure-Portal](#azure-portal-system-logic-app)
* [Azure-Ressourcen-Manager-Vorlagen](#template-system-logic-app)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/howto-assign-access-powershell.md)
* [Azure-Befehlszeilenschnittstelle](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md)

<a name="azure-portal-system-logic-app"></a>

### <a name="enable-system-assigned-identity-in-azure-portal"></a>Aktivieren der systemseitig zugewiesenen Identität im Azure-Portal

1. Öffnen Sie Ihre Logik-App im [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Optionen **Identität** > **Vom System zugewiesen** aus. Wählen Sie unter **Status** die Optionen **Ein** > **Speichern** > **Ja** aus.

   ![Aktivieren der systemseitig zugewiesenen Identität](./media/create-managed-service-identity/turn-on-system-assigned-identity.png)

   In der Logik-App kann nun die systemseitig zugewiesene Identität verwendet werden, die bei Azure Active Directory registriert ist und durch eine Objekt-ID angegeben wird.

   ![Objekt-ID für die systemseitig zugewiesene Identität](./media/create-managed-service-identity/object-id.png)

   | Eigenschaft | Wert | BESCHREIBUNG |
   |----------|-------|-------------|
   | **Objekt-ID** | <*Identität-Ressourcen-ID*> | Eine GUID (Globally Unique Identifier), die die systemseitig zugewiesene Identität für die Logik-App in Ihrem Azure AD-Mandanten angibt |
   ||||

1. Führen Sie dann die [Schritte für das Gewähren des Zugriffs der Identität auf die Ressource](#access-other-resources) aus.

<a name="template-system-logic-app"></a>

### <a name="enable-system-assigned-identity-in-azure-resource-manager-template"></a>Aktivieren der systemseitig zugewiesenen Identität in einer Azure Resource Manager-Vorlage

Sie können [Azure Resource Manager-Vorlagen](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) verwenden, um das Erstellen und Bereitstellen von Azure-Ressourcen wie Logik-Apps zu automatisieren. Um die systemseitig zugewiesene verwaltete Identität für die Logik-App in der Vorlage zu aktivieren, fügen Sie das `identity`-Objekt und die untergeordnete `type`-Eigenschaft in der Ressourcendefinition der Logik-App in der Vorlage hinzu, beispielsweise:

```json
{
   "apiVersion": "2016-06-01",
   "type": "Microsoft.logic/workflows",
   "name": "[variables('logicappName')]",
   "location": "[resourceGroup().location]",
   "identity": {
      "type": "SystemAssigned"
   },
   "properties": {
      "definition": {
         "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
         "actions": {},
         "parameters": {},
         "triggers": {},
         "contentVersion": "1.0.0.0",
         "outputs": {}
   },
   "parameters": {},
   "dependsOn": []
}
```

Wenn die Ressourcendefinition der Logik-App in Azure erstellt wird, erhält das `identity`-Objekt diese zusätzlichen Eigenschaften:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Eigenschaft (JSON) | Wert | BESCHREIBUNG |
|-----------------|-------|-------------|
| `principalId` | <*principal-ID*> | Die GUID (Globally Unique Identifier) des Dienstprinzipalobjekts für die verwaltete Identität, die die Logik-App im Azure AD-Mandanten angibt. Diese GUID wird manchmal als „Objekt-ID“ oder `objectID` angezeigt. |
| `tenantId` | <*Azure-AD-tenant-ID*> | Die GUID (Globally Unique Identifier), die den Azure AD-Mandanten angibt, in dem die Logik-App nun Mitglied ist. Unter dem Azure AD-Mandanten hat der Dienstprinzipal den gleichen Namen wie die Logik-App-Instanz. |
||||

<a name="access-other-resources"></a>

## <a name="give-identity-access-to-resources"></a>Zuweisen des Zugriffs der Identität auf Ressourcen

Nachdem Sie eine verwaltete Identität für die Logik-App eingerichtet haben, können Sie [dieser Identität Zugriff auf andere Azure-Ressourcen gewähren](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Anschließend können Sie diese Identität für die Authentifizierung verwenden.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu der Azure-Ressource, auf die die verwaltete Identität Zugriff erhalten soll.

1. Wählen Sie im Menü der Ressource **Zugriffssteuerung (IAM)**  > **Rollenzuweisungen** aus. Dort können Sie die aktuellen Rollenzuweisungen für die Ressource überprüfen. Wählen Sie auf der Symbolleiste die Option **Hinzufügen** > **Rollenzuweisung hinzufügen** aus.

   ![Auswählen von „Hinzufügen“ > „Rollenzuweisung hinzufügen“](./media/create-managed-service-identity/add-role-to-resource.png)

   > [!TIP]
   > Wenn die Option **Rollenzuweisung hinzufügen** deaktiviert ist, verfügen Sie wahrscheinlich nicht über die erforderlichen Berechtigungen. Weitere Informationen zu den Berechtigungen, mit denen Sie Rollen für Ressourcen verwalten können, finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

1. Wählen Sie unter **Rollenzuweisung hinzufügen** eine **Rolle** aus, mit der die Identität die erforderlichen Zugriffsberechtigungen für die Zielressource erhält.

   Für dieses Beispiel benötigt die Identität eine [Rolle, über die auf das Blob in einem Azure Storage-Container zugegriffen werden kann](../storage/common/storage-auth-aad.md#assign-rbac-roles-for-access-rights):

   ![Auswählen der Rolle „Mitwirkender an Storage-Blobdaten“](./media/create-managed-service-identity/assign-role.png)

1. Wählen Sie im Feld **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal** aus.

   ![Auswählen des Zugriffs für die systemseitig zugewiesene Identität](./media/create-managed-service-identity/assign-access-system.png)

1. Suchen Sie im Feld **Auswählen** nach Ihrer Logik-App, und wählen Sie sie aus.

   ![Auswählen der Logik-App für die systemseitig zugewiesene Identität](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

1. Klicken Sie auf **Speichern**, wenn Sie fertig sind.

   In der Liste der Rollenzuweisungen der Zielressource werden nun die ausgewählte verwaltete Identität und die ausgewählte Rolle angezeigt.

   ![Zur Zielressource hinzugefügte verwaltete Identitäten und Rollen](./media/create-managed-service-identity/added-roles-for-identities.png)

1. Führen Sie nun die [Schritte zum Authentifizieren des Zugriffs mit der Identität](#authenticate-access-with-identity) in einem Trigger oder einer Aktion aus, die verwaltete Identitäten unterstützen.

<a name="authenticate-access-with-identity"></a>

## <a name="authenticate-access-with-managed-identity"></a>Authentifizieren des Zugriffs mit der verwalteten Identität

Nachdem Sie [die verwaltete Identität für die Logik-App aktiviert](#azure-portal-system-logic-app) und [dieser Identität Zugriff auf die Zielressource oder Zielentität gewährt](#access-other-resources) haben, können Sie diese Identität in [Triggern und Aktionen verwenden, die verwaltete Identitäten unterstützen](logic-apps-securing-a-logic-app.md#managed-identity-authentication).

> [!IMPORTANT]
> Für eine Azure-Funktion, für die die systemseitig zugewiesene Identität verwendet werden soll, müssen Sie zunächst [die Authentifizierung für Azure-Funktionen aktivieren](../logic-apps/logic-apps-azure-functions.md#enable-authentication-for-azure-functions).

Die folgenden Schritte veranschaulichen, wie Sie die verwaltete Identität über das Azure-Portal mit einem Trigger oder einer Aktion verwenden. Informationen zum Angeben der verwalteten Identität in der zugrunde liegenden JSON-Definition eines Triggers oder einer Aktion finden Sie unter [Authentifizierung der verwalteten Identität](../logic-apps/logic-apps-securing-a-logic-app.md#managed-identity-authentication).

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Wenn dies noch nicht erfolgt ist, fügen Sie [den Trigger oder die Aktion hinzu, der bzw. die verwaltete Identitäten unterstützt](logic-apps-securing-a-logic-app.md#managed-identity-authentication).

   Beispielsweise kann die für die Logik-App aktivierte systemseitig zugewiesene Identität mit dem HTTP-Trigger oder der HTTP-Aktion verwendet werden. Im Allgemeinen werden im HTTP-Trigger oder der HTTP-Aktion die folgenden Eigenschaften verwendet, um die Ressource oder Entität anzugeben, auf die Sie zugreifen möchten:

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Methode** | Ja | Die HTTP-Methode, die in dem Vorgang verwendet wird, den Sie ausführen möchten. |
   | **URI** | Ja | Die Endpunkt-URL für den Zugriff auf die Azure-Zielressource oder Azure-Zielentität. Die URI-Syntax enthält normalerweise die [Ressourcen-ID](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) für die Azure-Ressource oder den Azure-Dienst. |
   | **Header** | Nein | Alle Headerwerte, die Sie in der ausgehenden Anforderung einfügen müssen oder möchten, z. B. den Inhaltstyp |
   | **Abfragen** | Nein | Alle Abfrageparameter, die Sie in der Anforderung einfügen müssen oder möchten, z. B. den Parameter für einen bestimmten Vorgang oder die API-Version für den auszuführenden Vorgang |
   | **Authentifizierung** | Ja | Der Authentifizierungstyp, der zum Authentifizieren des Zugriffs auf die Zielressource oder Zielentität verwendet wird. |
   ||||

   Als konkretes Beispiel wird angenommen, dass Sie den [Snapshot Blob-Vorgang](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob) für ein Blob in dem Azure Storage-Konto ausführen möchten, in dem Sie zuvor den Zugriff für die Identität eingerichtet haben. Jedoch unterstützt der [Azure Blob Storage-Connector](https://docs.microsoft.com/connectors/azureblob/) diesen Vorgang derzeit nicht. Stattdessen können Sie diesen Vorgang mithilfe der [HTTP-Aktion](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action) oder mit einem anderen [REST-API-Vorgang des Blob-Diensts](https://docs.microsoft.com/rest/api/storageservices/operations-on-blobs) ausführen.

   > [!IMPORTANT]
   > Um über HTTP-Anforderungen und verwaltete Identitäten auf Azure Storage-Konten hinter Firewalls zuzugreifen, müssen Sie sicherstellen, dass Sie auch das Speicherkonto mit der [Ausnahme einrichten, die den Zugriff durch vertrauenswürdige Microsoft-Dienste](../connectors/connectors-create-api-azureblobstorage.md#access-trusted-service) zulässt.

   Zum Ausführen des [Snapshot Blob-Vorgangs](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob) werden in der HTTP-Aktion die folgenden Eigenschaften angegeben:

   | Eigenschaft | Erforderlich | Beispielwert | BESCHREIBUNG |
   |----------|----------|---------------|-------------|
   | **Methode** | Ja | `PUT`| Die im Snapshot Blob-Vorgang verwendete HTTP-Methode |
   | **URI** | Ja | `https://{storage-account-name}.blob.core.windows.net/{blob-container-name}/{folder-name-if-any}/{blob-file-name-with-extension}` | Die Ressourcen-ID für eine Azure Blob Storage-Datei in der globalen (öffentlichen) Azure-Umgebung, in der diese Syntax verwendet wird. |
   | **Header** | Ja, für Azure Storage | `x-ms-blob-type` = `BlockBlob` <p>`x-ms-version` = `2019-02-02` | Die Headerwerte `x-ms-blob-type` und `x-ms-version`, die für Azure Storage-Vorgänge erforderlich sind. <p><p>**Wichtig**: In ausgehenden Anforderungen für HTTP-Trigger und HTTP-Aktionen für Azure Storage sind für den Header die `x-ms-version`-Eigenschaft und die API-Version für den auszuführenden Vorgang erforderlich. <p>Weitere Informationen finden Sie in den folgenden Themen: <p><p>- [Anforderungsheader: Snapshot Blob](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob#request) <br>- [Versionsverwaltung für Azure Storage-Dienste](https://docs.microsoft.com/rest/api/storageservices/versioning-for-the-azure-storage-services#specifying-service-versions-in-requests) |
   | **Abfragen** | Ja, für diesen Vorgang | `comp` = `snapshot` | Der Name und der Wert des Abfrageparameters für den Snapshot Blob-Vorgang |
   | **Authentifizierung** | Ja | `Managed Identity` | Der Authentifizierungstyp, der zum Authentifizieren des Zugriffs auf das Azure-Blob verwendet wird |
   |||||

   HTTP-Beispielaktion, in der alle diese Eigenschaftswerte angezeigt werden:

   ![Hinzufügen einer HTTP-Aktion zum Zugreifen auf eine Azure-Ressource](./media/create-managed-service-identity/http-action-example.png)

   Weitere Informationen zu allen verfügbaren Azure REST-API-Vorgängen finden Sie in der [Azure REST-API-Referenz](https://docs.microsoft.com/rest/api/azure/).

1. Wählen Sie in der Liste **Authentifizierung** den Eintrag **Verwaltete Identität** aus. Wenn die [**Authentication**-Eigenschaft unterstützt wird](logic-apps-securing-a-logic-app.md#add-authentication-outbound), aber ausgeblendet ist, öffnen Sie die Liste **Neuen Parameter hinzufügen**, und wählen Sie **Authentication** aus.

   > [!NOTE]
   > Sie können nicht für alle Trigger und Aktionen einen Authentifizierungstyp auswählen. Weitere Informationen finden Sie unter [Hinzufügen der Authentifizierung zu ausgehenden Aufrufen](logic-apps-securing-a-logic-app.md#add-authentication-outbound).

   ![Auswählen von „Verwaltete Identität“ in der Authentication-Eigenschaft](./media/create-managed-service-identity/select-managed-identity.png)

1. Nachdem Sie **Verwaltete Identität** ausgewählt haben, wird die **Audience**-Eigenschaft für einige Trigger und Aktionen angezeigt. Wenn die **Audience**-Eigenschaft unterstützt wird, aber ausgeblendet ist, öffnen Sie die Liste **Neuen Parameter hinzufügen**, und wählen Sie **Audience** aus.

1. Stellen Sie sicher, dass Sie den Wert für **Audience** auf die Ressourcen-ID für die Zielressource oder den Zieldienst festlegen. Andernfalls wird für die **Audience**-Eigenschaft standardmäßig die Ressourcen-ID `https://management.azure.com/` verwendet, bei der es sich um die Ressourcen-ID für Azure Resource Manager handelt.

   > [!IMPORTANT]
   > Vergewissern Sie sich, dass die Zielressourcen-ID *genau dem Wert entspricht*, der in Azure Active Directory (AD) erwartet wird, einschließlich aller erforderlichen nachgestellten Schrägstriche. Die Ressourcen-ID für alle Azure Blob Storage-Konten erfordert z. B. einen nachgestellten Schrägstrich. Allerdings erfordert die Ressourcen-ID für ein bestimmtes Speicherkonto keinen nachgestellten Schrägstrich. [Hier](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) finden Sie die Ressourcen-IDs für die Azure-Dienste, die die Azure AD-Authentifizierung unterstützen.

   In diesem Beispiel wird die **Audience**-Eigenschaft auf `https://storage.azure.com/` festgelegt, sodass die für die Authentifizierung verwendeten Zugriffstoken für alle Speicherkonten gültig sind. Sie können jedoch auch die Stammdienst-URL für ein bestimmtes Speicherkonto angeben, z. B. `https://fabrikamstorageaccount.blob.core.windows.net`.

   ![Angeben der Zielressourcen-ID in der Audience-Eigenschaft](./media/create-managed-service-identity/specify-audience-url-target-resource.png)

   Weitere Informationen zum Autorisieren des Zugriffs mit Azure AD für Azure Storage finden Sie in folgenden Themen:

   * [Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory](../storage/common/storage-auth-aad.md)
   * [Autorisieren des Zugriffs auf Azure Storage mit Azure Active Directory](https://docs.microsoft.com/rest/api/storageservices/authorize-with-azure-active-directory#use-oauth-access-tokens-for-authentication)

<a name="remove-identity"></a>

## <a name="remove-system-assigned-identity"></a>Entfernen systemseitig zugewiesener Identitäten

Wenn Sie die systemseitig zugewiesene Identität für die Logik-App nicht mehr verwenden möchten, haben Sie folgende Möglichkeiten:

* [Azure-Portal](#azure-portal-disable)
* [Azure-Ressourcen-Manager-Vorlagen](#template-disable)
* [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.resources/remove-azroleassignment)
* [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-delete)

Wenn Sie die Logik-App löschen, wird in Azure die verwaltete Identität automatisch aus Azure AD entfernt.

<a name="azure-portal-disable"></a>

### <a name="remove-system-assigned-identity-in-the-azure-portal"></a>Entfernen der systemseitig zugewiesenen Identität im Azure-Portal

Entfernen Sie im Azure-Portal die systemseitig zugewiesene Identität [aus der Logik-App](#disable-identity-logic-app) und den Zugriff dieser Identität [aus der Zielressource](#disable-identity-target-resource).

<a name="disable-identity-logic-app"></a>

#### <a name="remove-system-assigned-identity-from-logic-app"></a>Entfernen der systemseitig zugewiesenen Identität aus der Logik-App

1. Öffnen Sie Ihre Logik-App im [Azure-Portal](https://portal.azure.com) im Logik-App-Designer.

1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Optionen **Identität** > **Vom System zugewiesen** aus. Wählen Sie unter **Status** die Option **Aus** > **Speichern** > **Ja** aus.

   ![Beenden der Verwendung der systemseitig zugewiesenen Identität](./media/create-managed-service-identity/turn-off-system-assigned-identity.png)

<a name="disable-identity-target-resource"></a>

#### <a name="remove-identity-access-from-resources"></a>Entfernen des Zugriffs der Identität auf Ressourcen

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu der Azure-Zielressource, für die Sie den Zugriff für eine verwaltete Identität entfernen möchten.

1. Wählen Sie im Menü der Zielressource die Option **Zugriffssteuerung (IAM)** aus. Wählen Sie auf der Symbolleiste die Option **Rollenzuweisungen** aus.

1. Wählen Sie in der Liste der Rollen die verwalteten Identitäten aus, die Sie entfernen möchten. Wählen Sie auf der Symbolleiste die Option **Entfernen** aus.

   > [!TIP]
   > Wenn die Option **Entfernen** deaktiviert ist, verfügen Sie wahrscheinlich nicht über die erforderlichen Berechtigungen. Weitere Informationen zu den Berechtigungen, mit denen Sie Rollen für Ressourcen verwalten können, finden Sie unter [Berechtigungen der Administratorrolle in Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

Die verwaltete Identität ist damit entfernt und hat keinen Zugriff mehr auf die Zielressource.

<a name="template-disable"></a>

### <a name="disable-managed-identity-in-azure-resource-manager-template"></a>Deaktivieren der verwalteten Identität in einer Azure Resource Manager-Vorlage

Wenn Sie die systemseitig zugewiesene verwaltete Identität der Logik-App mithilfe einer Azure Resource Manager-Vorlage aktiviert haben, legen Sie die untergeordnete `type`-Eigenschaft des `identity`-Objekts auf `None` fest. Durch diese Aktion wird auch die Prinzipal-ID für die systemseitig zugewiesene verwaltete Identität in Azure AD gelöscht.

```json
"identity": {
   "type": "None"
}
```

## <a name="next-steps"></a>Nächste Schritte

* [Schützen des Zugriffs und der Daten in Azure Logic Apps](../logic-apps/logic-apps-securing-a-logic-app.md)
