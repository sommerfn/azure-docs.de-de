---
title: Verwaltete Azure-Anwendung mit verwalteter Identität
description: Konfigurieren Sie „Verwaltete Anwendung“ mit „Verwaltete Identität“ für das Verknüpfen mit vorhandenen Ressourcen, das Verwalten von Azure-Ressourcen und das Bereitstellen der Betriebsidentität für das Aktivitätsprotokoll.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.reviewer: ''
ms.author: jobreen
author: jjbfour
ms.date: 05/13/2019
ms.openlocfilehash: 9e1f5072921104c749a0acef95b7da09f1cbb662
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2019
ms.locfileid: "72330222"
---
# <a name="azure-managed-application-with-managed-identity"></a>Verwaltete Azure-Anwendung mit verwalteter Identität

> [!NOTE]
> Die Unterstützung verwalteter Anwendungen durch verwaltete Identitäten befindet sich derzeit in der Vorschau. Verwenden Sie die API-Version „2018-09-01-preview“, um die verwaltete Identität einzusetzen.

Erfahren Sie, wie Sie eine verwaltete Anwendung konfigurieren, sodass sie eine verwaltete Identität enthält. Mit einer verwalteten Identität kann dem Kunden ermöglicht werden, der verwalteten Anwendung Zugriff zu weiteren vorhandenen Ressourcen zu gewähren. Da die Identität von der Azure-Plattform verwaltet wird, müssen Sie keine Geheimnisse bereitstellen oder rotieren. Weitere Informationen zu verwalteten Identitäten in Azure Active Directory (AAD) finden Sie unter [Verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md).

Ihrer Anwendung können zwei Arten von Identitäten zugewiesen werden:

- Eine **systemseitig zugewiesene Identität** ist an Ihre Anwendung gebunden und wird gelöscht, wenn Ihre App gelöscht wird. Eine App kann nur über eine systemseitig zugewiesene Identität verfügen.
- Eine **benutzerseitig zugewiesene Identität** ist eine eigenständige Azure-Ressource, die Ihrer App zugewiesen werden kann. Eine App kann über mehrere benutzerseitig zugewiesene Identitäten verfügen.

## <a name="how-to-use-managed-identity"></a>Gewusst wie: Verwenden der verwalteten Identität

Die verwaltete Identität ermöglicht viele Szenarien für verwaltete Anwendungen. Einige gängige Szenarien, die gelöst werden können:

- Bereitstellen einer verwalteten Anwendung, die mit vorhandenen Azure-Ressourcen verknüpft ist. Ein Beispiel ist die Bereitstellung eines virtuellen Azure-Computers (VM) in der verwalteten Anwendung, der einer [vorhandenen Netzwerkschnittstelle](../virtual-network/virtual-network-network-interface-vm.md) angefügt ist.
- Gewährung des Zugriffs auf Azure-Ressourcen außerhalb der **verwalteten Ressourcengruppe** für die verwaltete Anwendung und Herausgeber.
- Bereitstellen einer operativen Identität von verwalteten Anwendungen für das Aktivitätsprotokoll und andere Dienste in Azure.

## <a name="adding-managed-identity"></a>Hinzufügen der verwalteten Identität

Für die Erstellung einer verwalteten Anwendung mit einer verwalteten Identität muss eine zusätzliche Eigenschaft in der Azure-Ressource festgelegt werden. Das folgende Beispiel zeigt eine Beispieleigenschaft **identity**:

```json
{
"identity": {
    "type": "SystemAssigned, UserAssigned",
    "userAssignedIdentities": {
        "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity": {}
    }
}
```

Es gibt zwei gängige Möglichkeiten zum Erstellen einer verwalteten Anwendung mit **identity**: [CreateUIDefinition.json](./create-uidefinition-overview.md) und [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md). Für einfache Einzelerstellungsszenarien sollte CreateUIDefinition zum Aktivieren der verwalteten Identität verwendet werden, da es eine umfangreichere Benutzeroberfläche bereitstellt. Beim Umgang mit erweiterten oder komplexen Systemen, die automatisierte oder mehrere Bereitstellungen von verwalteten Anwendungen erfordern, können allerdings Vorlagen verwendet werden.

### <a name="using-createuidefinition"></a>Verwenden von CreateUIDefinition

Eine verwaltete Anwendung kann über [CreateUIDefinition.json](./create-uidefinition-overview.md) mit der verwalteten Identität konfiguriert werden. Im Abschnitt [outputs](./create-uidefinition-overview.md#outputs) kann mit dem Schlüssel `managedIdentity` die Identitätseigenschaft der Vorlage für die verwaltete Anwendung außer Kraft gesetzt werden. Im nachstehenden Beispiel wird die **vom System zugewiesene** Identität für die verwaltete Anwendung aktiviert. Komplexere Identitätsobjekte können mit CreateUIDefinition-Elementen gebildet werden, um den Consumer zu Eingaben aufzufordern. Diese Eingaben dienen zum Erstellen von verwalteten Anwendungen mit **vom Benutzer zugewiesener Identität**.

```json
"outputs": {
    "managedIdentity": "[parse('{\"Type\":\"SystemAssigned\"}')]"
}
```

#### <a name="when-to-use-createuidefinition-for-managed-identity"></a>Verwendung von CreateUIDefinition für die verwaltete Identität

Im Folgenden finden Sie einige Empfehlungen zur Verwendung von CreateUIDefinition zum Aktivieren der verwalteten Identität für verwaltete Anwendungen.

- Die Erstellung von verwalteten Anwendungen erfolgt im Azure-Portal oder Marketplace.
- Der verwaltete Identität erfordert komplexe Consumereingaben.
- Die verwaltete Identität wird zum Erstellen der verwalteten Anwendung benötigt.

#### <a name="systemassigned-createuidefinition"></a>SystemAssigned CreateUIDefinition

Eine grundlegende CreateUIDefinition, die die SystemAssigned-Identität für die verwaltete Anwendung ermöglicht.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
        ],
        "outputs": {
            "managedIdentity": "[parse('{\"Type\":\"SystemAssigned\"}')]"
        }
    }
}
```

#### <a name="userassigned-createuidefinition"></a>UserAssigned CreateUIDefinition

Eine grundlegende CreateUIDefinition, die eine **vom Benutzer zugewiesene Identität** als Ressource als Eingabe akzeptiert und die UserAssigned-Identität für die verwaltete Anwendung aktiviert.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "manageIdentity",
                "label": "Identity",
                "subLabel": {
                    "preValidation": "Manage Identities",
                    "postValidation": "Done"
                },
                "bladeTitle": "Identity",
                "elements": [
                    {
                        "name": "userAssignedText",
                        "type": "Microsoft.Common.TextBox",
                        "label": "User assigned managed identity",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity",
                        "visible": true
                    }
                ]
            }
        ],
        "outputs": {
            "managedIdentity": "[parse(concat('{\"Type\":\"UserAssigned\",\"UserAssignedIdentities\":{',string(steps('manageIdentity').userAssignedText),':{}}}'))]"
        }
    }
}
```

Die oben genannte Datei „CreateUIDefinition.json“ generiert eine Erstellungsbenutzeroberfläche mit einem Textfeld, wo der Consumer die Azure-Ressourcen-ID der **vom Benutzer zugewiesenen Identität** eingeben kann. Die generierte Benutzeroberfläche würde so aussehen:

![Beispiel für vom Benutzer zugewiesene Identität – CreateUIDefinition](./media/publish-managed-identity/user-assigned-identity.png)

### <a name="using-azure-resource-manager-templates"></a>Verwenden von Azure-Ressourcen-Manager-Vorlagen

> [!NOTE]
> Vorlagen für verwaltete Marketplace-Anwendungen werden automatisch für Kunden generiert, die über das Azure-Portal erstellen.
> Für diese Szenarien muss der `managedIdentity`-Ausgabeschlüssel von CreateUIDefinition zum Aktivieren der Identität verwendet werden.

Die verwaltete Identität kann auch über Azure Resource Manager-Vorlagen aktiviert werden. Im nachstehenden Beispiel wird die **vom System zugewiesene** Identität für die verwaltete Anwendung aktiviert. Komplexere Identitätsobjekte können mithilfe der Azure Resource Manager-Vorlagenparameter zum Bereitstellen von Eingaben gebildet werden. Diese Eingaben dienen zum Erstellen von verwalteten Anwendungen mit **vom Benutzer zugewiesener Identität**.

#### <a name="when-to-use-azure-resource-manager-templates-for-managed-identity"></a>Verwenden von Azure Resource Manager-Vorlagen für verwaltete Identität

Im Folgenden finden Sie einige Empfehlungen zur Verwendung von Azure Resource Manager-Vorlagen zum Aktivieren der verwalteten Identität für verwaltete Anwendungen.

- Verwaltete Anwendungen können programmgesteuert auf Grundlage einer Vorlage bereitgestellt werden.
- Benutzerdefinierte Rollenzuweisungen für die verwaltete Identität sind erforderlich, um die verwaltete Anwendung bereitzustellen.
- Die verwaltete Anwendung benötigt keinen Erstellungsflow in Azure-Portal und Marketplace.

#### <a name="systemassigned-template"></a>SystemAssigned-Vorlage

Eine einfache Azure Resource Manager-Vorlage, die eine verwaltete Anwendung mit **vom System zugewiesener** Identität bereitstellt.

```json
"resources": [
    {
        "type": "Microsoft.Solutions/applications",
        "name": "[parameters('applicationName')]",
        "apiVersion": "2018-09-01-preview",
        "location": "[parameters('location')]",
        "identity": {
            "type": "SystemAssigned"
        },
        "properties": {
            "ManagedResourceGroupId": "[parameters('managedByResourceGroupId')]",
            "parameters": { }
        }
    }
]
```

### <a name="userassigned-template"></a>UserAssigned-Vorlage

Eine einfache Azure Resource Manager-Vorlage, die eine verwaltete Anwendung mit **vom Benutzer zugewiesener** Identität bereitstellt.

```json
"resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('managedIdentityName')]",
      "apiVersion": "2018-11-30",
      "location": "[parameters('location')]"
    },
    {
        "type": "Microsoft.Solutions/applications",
        "name": "[parameters('applicationName')]",
        "apiVersion": "2018-09-01-preview",
        "location": "[parameters('location')]",
        "identity": {
            "type": "UserAssigned",
            "userAssignedIdentities": {
                "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',parameters('managedIdentityName'))]": {}
            }
        },
        "properties": {
            "ManagedResourceGroupId": "[parameters('managedByResourceGroupId')]",
            "parameters": { }
        }
    }
]
```

## <a name="granting-access-to-azure-resources"></a>Gewähren des Zugriffs auf Azure-Ressourcen

Sobald einer verwalteten Anwendung eine Identität gewährt wird, kann ihr Zugriff auf vorhandene Azure-Ressourcen gewährt werden. Dieser Vorgang kann über die Benutzeroberfläche der Zugriffssteuerung (IAM) im Azure-Portal ausgeführt werden. Der Name der verwalteten Anwendung oder der **vom Benutzer zugewiesenen Identität** kann gesucht werden, um eine Rollenzuweisung hinzuzufügen.

![Hinzufügen einer Rollenzuweisung für eine verwaltete Anwendung](./media/publish-managed-identity/identity-role-assignment.png)

## <a name="linking-existing-azure-resources"></a>Verknüpfen vorhandener Azure-Ressourcen

> [!NOTE]
> Eine **vom Benutzer zugewiesene Identität** muss vor dem Bereitstellen der verwalteten Anwendung [konfiguriert](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) werden. Außerdem wird die Bereitstellung verknüpfter Ressourcen von verwalteten Anwendungen nur für den **Marketplace** unterstützt.

Mit der verwalteten Identität kann auch eine verwaltete Anwendung bereitgestellt werden, die während ihrer Bereitstellung Zugriff auf vorhandene Ressourcen erfordert. Wenn die verwaltete Anwendung vom Kunden bereitgestellt wird, können **vom Benutzer zugewiesene Identitäten** hinzugefügt werden, um zusätzliche Autorisierungen für die **mainTemplate**-Bereitstellung bereitzustellen.

### <a name="authoring-the-createuidefinition-with-a-linked-resource"></a>Erstellen von CreateUIDefinition mit einer verknüpften Ressource

Wenn Sie die Bereitstellung der verwalteten Anwendung mit vorhandenen Ressourcen verknüpfen, muss sowohl die vorhandene Azure-Ressource als auch eine **vom Benutzer zugewiesene Identität** mit der entsprechenden Rollenzuweisung für diese Ressource angegeben werden.

 Ein CreateUIDefinition-Beispiel, das zwei Eingaben erfordert: eine Netzwerkschnittstellen-Ressourcen-ID und eine vom Benutzer zugewiesene Identitätsressourcen-ID.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "managedApplicationSetting",
                "label": "Managed Application Settings",
                "subLabel": {
                    "preValidation": "Managed Application Settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Managed Application Settings",
                "elements": [
                    {
                        "name": "networkInterfaceId",
                        "type": "Microsoft.Common.TextBox",
                        "label": "network interface resource id",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Network/networkInterfaces/existingnetworkinterface",
                        "toolTip": "Must represent the identity as an Azure Resource Manager resource identifer format ex. /subscriptions/sub1/resourcegroups/myGroup/providers/Microsoft.Network/networkInterfaces/networkinterface1",
                        "visible": true
                    },
                    {
                        "name": "userAssignedId",
                        "type": "Microsoft.Common.TextBox",
                        "label": "user assigned identity resource id",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity",
                        "toolTip": "Must represent the identity as an Azure Resource Manager resource identifer format ex. /subscriptions/sub1/resourcegroups/myGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/identity1",
                        "visible": true
                    }
                ]
            }
        ],
        "outputs": {
            "existingNetworkInterfaceId": "[steps('managedApplicationSetting').networkInterfaceId]",
            "managedIdentity": "[parse(concat('{\"Type\":\"UserAssigned\",\"UserAssignedIdentities\":{',string(steps('managedApplicationSetting').userAssignedId),':{}}}'))]"
        }
    }
}
```

Diese Datei „CreateUIDefinition.json“ generiert eine Erstellungsbenutzeroberfläche mit zwei Feldern. Das erste Feld ermöglicht dem Benutzer die Eingabe der Azure-Ressourcen-ID für die Ressource, die mit der Bereitstellung der verwalteten Anwendung verknüpft wird. Im zweiten kann ein Consumer die Azure-Ressourcen-ID der **vom Benutzer zugewiesenen Identität** eingeben, die Zugriff auf die verknüpfte Azure-Ressource hat. Die generierte Benutzeroberfläche würde so aussehen:

![Ein CreateUIDefinition-Beispiel mit zwei Eingaben: eine Netzwerkschnittstellen-Ressourcen-ID und eine vom Benutzer zugewiesene Identitätsressourcen-ID.](./media/publish-managed-identity/network-interface-cuid.png)

### <a name="authoring-the-maintemplate-with-a-linked-resource"></a>Erstellen von mainTemplate mit einer verknüpften Ressource

Neben CreateUIDefinition muss auch die Hauptvorlage aktualisiert werden, um die übergebene ID der verknüpften Ressource zu akzeptieren. Die Hauptvorlage kann durch Hinzufügen eines neuen Parameters aktualisiert werden, um die neue Ausgabe zu akzeptieren. Da die `managedIdentity`-Ausgabe den Wert für die generierte Vorlage für die verwaltete Anwendung überschreibt, wird sie nicht an die Hauptvorlage übergeben und sollte nicht im Abschnitt „parameters“ enthalten sein.

Eine Beispielhauptvorlage, die das Netzwerkprofil auf eine vorhandene, von CreateUIDefinition bereitgestellte Netzwerkschnittstelle festlegt.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "existingNetworkInterfaceId": { "type": "string" }
    },
    "variables": {
    },
    "resources": [
        {
            "apiVersion": "2016-04-30-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "myLinkedResourceVM",
            "location": "[resourceGroup().location]",
            "properties": {
                …,
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[parameters('existingNetworkInterfaceId')]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="consuming-the-managed-application-with-a-linked-resource"></a>Nutzen der verwalteten Anwendung mit einer verknüpften Ressource

Nachdem das Paket für die verwaltete Anwendung erstellt wurde, kann die verwaltete Anwendung über das Azure-Portal genutzt werden. Bevor sie verwendet werden kann, sind mehrere Schritte erforderlich.

- Eine Instanz der erforderlichen verknüpften Azure-Ressource muss erstellt werden.
- Die **vom Benutzer zugewiesene Identität** muss [erstellt werden und Rollenzuweisungen für die verknüpfte Ressource erhalten](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).
- Die vorhandene ID der verknüpften Ressource und die ID der vom **Benutzer zugewiesenen Identität** werden für CreateUIDefinition bereitgestellt.

## <a name="accessing-the-managed-identity-token"></a>Zugreifen auf das Token der verwalteten Identität

Auf das Token der verwalteten Anwendung kann nun über die `listTokens`-API vom Mandanten des Herausgebers aus zugegriffen werden. Eine Beispielanforderung könnte folgendermaßen aussehen:

``` HTTP
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Solutions/applications/{applicationName}/listTokens?api-version=2018-09-01-preview HTTP/1.1

{
    "authorizationAudience": "https://management.azure.com/",
    "userAssignedIdentities": [
        "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{userAssignedIdentityName}"
    ]
}
```

Anforderungstextparameter:

Parameter | Erforderlich | BESCHREIBUNG
---|---|---
authorizationAudience | *Nein* | Der App-ID-URI der Zielressource. Dieser ist auch der `aud`-Anspruch (Zielgruppe) des ausgestellten Tokens. Der Standardwert ist „https://management.azure.com/“.
userAssignedIdentities | *Nein* | Die Liste der vom Benutzer zugewiesenen verwalteten Identitäten, für die ein Token abgerufen werden kann. Falls nicht angegeben, gibt `listTokens` das Token für die vom System zugewiesene verwaltete Identität zurück.


Eine Beispielantwort könnte folgendermaßen aussehen:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json

{
    "value": [
        {
            "access_token": "eyJ0eXAi…",
            "expires_in": "2…",
            "expires_on": "1557…",
            "not_before": "1557…",
            "authorizationAudience": "https://management.azure.com/",
            "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Solutions/applications/{applicationName}",
            "token_type": "Bearer"
        }
    ]
}
```

Die Antwort enthält in der `value`-Eigenschaft ein Array von Token:

Parameter | BESCHREIBUNG
---|---
access_token | Das angeforderte Zugriffstoken.
expires_in | Die Anzahl der Sekunden, die das Zugriffstoken gültig ist.
expires_on | Der Zeitpunkt, zu dem das Zugriffstoken abläuft. Wird als die Anzahl der Sekunden in der Epoche dargestellt.
not_before | Die Zeitspanne, in der das Zugriffstoken gilt. Wird als die Anzahl der Sekunden in der Epoche dargestellt.
authorizationAudience | Die `aud` (Zielgruppe), für die das Zugriffstoken angefordert wurde. Ist identisch mit den in der `listTokens`-Anforderung bereitgestellten Daten.
resourceId | Die Azure-Ressourcen-ID für das ausgestellte Token. Ist entweder die ID der verwalteten Anwendung oder die dem Benutzer zugewiesene Identitäts-ID.
token_type | Der Typ des Tokens.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konfigurieren einer verwalteten Anwendung mit einem benutzerdefinierten Anbieter](./custom-providers-overview.md)
