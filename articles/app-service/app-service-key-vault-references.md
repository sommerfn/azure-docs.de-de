---
title: Key Vault-Referenzen – Azure App Service | Microsoft-Dokumentation
description: Konzeptverweis und Einrichtungsleitfaden für Azure Key Vault-Referenzen in Azure App Service und Azure Functions
services: app-service
author: mattchenderson
manager: jeconnoc
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/09/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 8f237e4c676a53f6df15940a196a998bee529f6b
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73817969"
---
# <a name="use-key-vault-references-for-app-service-and-azure-functions"></a>Verwenden von Key Vault-Verweisen für App Service und Azure Functions

> [!NOTE] 
> Zurzeit sind Key Vault-Verweise in Linux-Verbrauchstarifen nicht verfügbar.

Dieses Thema zeigt Ihnen, wie Sie mit Geheimnissen von Azure Key Vault in Ihrer App Service- oder Azure Functions-Anwendung arbeiten können, ohne dass Codeänderungen erforderlich sind. [Azure Key Vault](../key-vault/key-vault-overview.md) ist ein Dienst, der eine zentralisierte Verwaltung von Geheimnissen mit voller Kontrolle über Zugriffsrichtlinien und Überprüfungsverlauf ermöglicht.

## <a name="granting-your-app-access-to-key-vault"></a>Gewähren des Zugriffs auf Key Vault für Ihre App

Um Geheimnisse aus Key Vault auslesen zu können, müssen Sie einen Tresor erstellen und Ihrer App die Berechtigung erteilen, darauf zuzugreifen.

1. Erstellen Sie einen Schlüsseltresor anhand dieser [Key Vault-Schnellstartanleitung](../key-vault/quick-create-cli.md).

1. Erstellen Sie eine [systemseitig zugewiesene verwaltete Identität](overview-managed-identity.md) für Ihre App.

   > [!NOTE] 
   > Key Vault-Verweise unterstützen derzeit nur systemseitig zugewiesene verwaltete Identitäten. Vom Benutzer zugewiesene Identitäten können nicht verwendet werden.

1. Erstellen Sie eine [Zugriffsrichtlinie im Schlüsseltresor](../key-vault/key-vault-secure-your-key-vault.md#key-vault-access-policies) für die zuvor von Ihnen erstellte Anwendungsidentität. Aktivieren Sie die „Get“-Geheimnisberechtigung für diese Richtlinie. Konfigurieren Sie nicht die Einstellungen „Autorisierte Anwendung“ oder `applicationId`, da dies mit einer verwalteten Identität nicht kompatibel ist.

    > [!NOTE]
    > Key Vault-Verweise können derzeit keine Geheimnisse in einem Schlüsseltresor mit [Netzwerkeinschränkungen](../key-vault/key-vault-overview-vnet-service-endpoints.md) auflösen.

## <a name="reference-syntax"></a>Verweissyntax

Ein Key Vault-Verweis hat die Form `@Microsoft.KeyVault({referenceString})`, wobei `{referenceString}` durch eine der folgenden Optionen ersetzt wird:

> [!div class="mx-tdBreakAll"]
> | Verweiszeichenfolge                                                            | BESCHREIBUNG                                                                                                                                                                                 |
> |-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | SecretUri=_secretUri_                                                       | **SecretUri** ist die vollständige URI der Datenebene eines Geheimnisses in Key Vault, einschließlich einer Version, z.B. https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931.  |
> | VaultName=_vaultName_;SecretName=_secretName_;SecretVersion=_secretVersion_ | **VaultName** ist der Name Ihrer Key Vault-Ressource. **SecretName** ist der Name des Zielgeheimnisses. **SecretVersion** ist die Version des zu verwendenden Geheimnisses. |

Ein vollständiger Verweis mit Version sieht beispielsweise wie folgt aus:

```
@Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931)
```
Alternativ:

```
@Microsoft.KeyVault(VaultName=myvault;SecretName=mysecret;SecretVersion=ec96f02080254f109c51a1f14cdb1931)
```


## <a name="source-application-settings-from-key-vault"></a>Einbinden von Anwendungseinstellungen aus Key Vault

Key Vault-Verweise können als Werte für [Anwendungseinstellungen](configure-common.md#configure-app-settings) verwendet werden, sodass Sie Geheimnisse in Key Vault anstelle der Websitekonfiguration behalten können. Die Anwendungseinstellungen sind im Ruhezustand sicher verschlüsselt, aber wenn Sie die Funktion zum Verwalten von Geheimnissen benötigen, sollten sie in Key Vault aufgenommen werden.

Um einen Key Vault-Verweis für eine Anwendungseinstellung zu verwenden, legen Sie den Verweis als Wert für die Einstellung fest. Ihre App kann wie gewohnt durch seinen Schlüssel auf das Geheimnis verweisen. Es sind keine Codeänderungen erforderlich.

> [!TIP]
> Die meisten Anwendungseinstellungen, die Key Vault-Verweise verwenden, sollten als Slot-Einstellungen markiert werden, da Sie für jede Umgebung eigene Tresore verwenden sollten.

### <a name="azure-resource-manager-deployment"></a>Azure Resource Manager-Bereitstellung

Wenn Sie Ressourcenbereitstellungen über Azure Resource Manager-Vorlagen automatisieren, müssen Sie möglicherweise Ihre Abhängigkeiten in einer bestimmten Reihenfolge sortieren, damit diese Funktion funktioniert. Beachten Sie, dass Sie Ihre Anwendungseinstellungen als eigene Ressource definieren müssen, anstatt eine `siteConfig`-Eigenschaft in der Websitedefinition zu verwenden. Grund dafür ist, dass zuerst die Website definiert werden muss, damit die systemseitig zugeordnete Identität gleichzeitig erstellt und in der Zugriffsrichtlinie verwendet werden kann.

Ein Beispiel für eine Pseudovorlage für eine Funktions-App könnte wie folgt aussehen:

```json
{
    //...
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            //...
        },
        {
            "type": "Microsoft.Insights/components",
            "name": "[variables('appInsightsName')]",
            //...
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[variables('functionAppName')]",
            "identity": {
                "type": "SystemAssigned"
            },
            //...
            "resources": [
                {
                    "type": "config",
                    "name": "appsettings",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('storageConnectionStringName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('appInsightsKeyName'))]"
                    ],
                    "properties": {
                        "AzureWebJobsStorage": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "APPINSIGHTS_INSTRUMENTATIONKEY": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('appInsightsKeyResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_ENABLE_SYNC_UPDATE_SITE": "true"
                        //...
                    }
                },
                {
                    "type": "sourcecontrols",
                    "name": "web",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.Web/sites/config', variables('functionAppName'), 'appsettings')]"
                    ],
                }
            ]
        },
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[variables('keyVaultName')]",
            //...
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]"
            ],
            "properties": {
                //...
                "accessPolicies": [
                    {
                        "tenantId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').tenantId]",
                        "objectId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').principalId]",
                        "permissions": {
                            "secrets": [ "get" ]
                        }
                    }
                ]
            },
            "resources": [
                {
                    "type": "secrets",
                    "name": "[variables('storageConnectionStringName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
                    ],
                    "properties": {
                        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountResourceId'),'2015-05-01-preview').key1)]"
                    }
                },
                {
                    "type": "secrets",
                    "name": "[variables('appInsightsKeyName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
                    ],
                    "properties": {
                        "value": "[reference(resourceId('microsoft.insights/components/', variables('appInsightsName')), '2015-05-01').InstrumentationKey]"
                    }
                }
            ]
        }
    ]
}
```

> [!NOTE] 
> In diesem Beispiel hängt die Bereitstellung der Quellcodeverwaltung von den Anwendungseinstellungen ab. Dies ist normalerweise ein unsicheres Verhalten, da sich das App-Einstellungsupdate asynchron verhält. Da wir aber die Anwendungseinstellung `WEBSITE_ENABLE_SYNC_UPDATE_SITE` integriert haben, verläuft das Update synchron. Dies bedeutet, dass die Bereitstellung der Quellcodeverwaltung erst dann beginnt, wenn die Anwendungseinstellungen vollständig aktualisiert wurden.

## <a name="troubleshooting-key-vault-references"></a>Problembehandlung von Key Vault-Verweisen

Wird ein Verweis nicht ordnungsgemäß aufgelöst, wird stattdessen der Verweiswert verwendet. Dies bedeutet, dass für Anwendungseinstellungen eine Umgebungsvariable erstellt wird, deren Wert die `@Microsoft.KeyVault(...)`-Syntax hat. Dies kann dazu führen, dass die Anwendung Fehler auslöst, da sie ein Geheimnis einer bestimmten Struktur erwartet hat.

Meist ist die Ursache hierfür eine fehlerhafte Konfiguration der [Key Vault-Zugriffsrichtlinie](#granting-your-app-access-to-key-vault). Ursache kann jedoch auch sein, dass ein Geheimnis nicht mehr vorhanden ist, oder dass ein Syntaxfehler im Verweis vorliegt.

Ist die Syntax richtig, können Sie weitere Fehlerursachen anzeigen, indem Sie den aktuellen Auflösungsstatus im Portal überprüfen. Navigieren Sie zu Anwendungseinstellungen, und wählen Sie „Bearbeiten“ für den fraglichen Verweis aus. Unterhalb der Einstellungskonfiguration sollten Statusinformationen, einschließlich aller Fehler, angezeigt werden. Das Fehlen dieser Angaben impliziert, dass die Verweissyntax ungültig ist.

Sie können auch einen der integrierten Detektoren verwenden, um zusätzliche Informationen abzurufen.

### <a name="using-the-detector-for-app-service"></a>Verwenden der Erkennung für App Service

1. Navigieren Sie im Portal zu Ihrer App.
2. Wählen Sie **Probleme diagnostizieren und beheben** aus.
3. Wählen Sie **Leistung und Verfügbarkeit** aus, und wählen Sie **Web app down** aus.
4. Suchen Sie nach **Key Vault Application Settings Diagnostics**, und klicken Sie auf **Details**.


### <a name="using-the-detector-for-azure-functions"></a>Verwenden der Erkennung für Azure Functions

1. Navigieren Sie im Portal zu Ihrer App.
2. Navigieren Sie zu **Plattformfeatures**.
3. Wählen Sie **Probleme diagnostizieren und beheben** aus.
4. Wählen Sie **Leistung und Verfügbarkeit** aus, und wählen Sie **Function app down or reporting errors** aus.
5. Klicken Sie auf **Key Vault Application Settings Diagnostics**.
