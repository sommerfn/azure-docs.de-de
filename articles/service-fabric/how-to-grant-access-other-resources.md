---
title: 'Azure Service Fabric: Gewähren des Zugriffs auf andere Azure-Ressourcen für eine Service Fabric-Anwendung| Microsoft-Dokumentation'
description: In diesem Artikel wird erläutert, wie Sie Ihrer Service Fabric-Anwendung, die verwaltete Identitäten nutzen kann, Zugriff auf andere Azure-Ressourcen gewähren, die die Azure Active Directory-basierte Authentifizierung unterstützen.
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.date: 08/08/2019
ms.author: atsenthi
ms.openlocfilehash: b6e1108ffee13f1583d920947404963a69616788
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "68957571"
---
# <a name="granting-a-service-fabric-applications-managed-identity-access-to-azure-resources"></a>Gewähren des Zugriffs auf Azure-Ressourcen für die verwaltete Identität einer Service Fabric-Anwendung

Bevor eine Anwendung ihre verwaltete Identität für den Zugriff auf andere Ressourcen nutzen kann, müssen dieser Identität Berechtigungen in der geschützten Azure-Ressource gewährt werden, auf die zugegriffen werden soll. Das Gewähren von Berechtigungen ist in der Regel eine Verwaltungsaktion auf der Steuerungsebene des Azure-Diensts, der im Besitz der geschützten Ressource ist. Diese Aktion wird über den Azure Resource Manager ausgeführt, der alle anwendbaren rollenbasierten Zugriffsüberprüfungen erzwingt.

Die genaue Abfolge der Schritte richtet sich nach dem Azure-Ressourcentyp, auf den zugegriffen wird, sowie danach, mit welcher Sprache und welchem Client Berechtigungen gewährt werden. Für diesen Artikel wird davon ausgegangen, dass die Anwendung über eine benutzerseitig zugewiesene Identität verfügt. Der Artikel stellt eine Reihe typischer Beispiele zu Ihrer Information vor, ist jedoch in keiner Weise als erschöpfende Referenz für dieses Thema zu betrachten. Aktuelle Anleitungen zum Gewähren von Berechtigungen finden Sie in der Dokumentation des jeweiligen Azure-Diensts.  

## <a name="granting-access-to-azure-storage"></a>Gewähren des Zugriffs auf Azure Storage
Sie können die verwaltete Identität einer Service Fabric-Anwendung (in diesem Fall benutzerseitig zugewiesen) verwenden, um die Daten aus einem Azure Storage-Blob abzurufen. Gehen Sie folgendermaßen vor, um der Identität im Azure-Portal die erforderlichen Berechtigungen zu gewähren:

1. Navigieren Sie zum Speicherkonto.
2. Klicken Sie im linken Bereich auf den Link „Zugriffssteuerung (IAM)“.
3. (Optional) Überprüfen Sie den vorhandenen Zugriff: Wählen Sie im Steuerelement „Suchen“ die Option für eine systemseitig oder eine benutzerseitig zugewiesene verwaltete Identität aus, und wählen Sie die entsprechende Identität aus der Ergebnisliste aus.
4. Klicken Sie oben auf der Seite auf „+ Rollenzuweisung hinzufügen“, um der Identität der Anwendung eine neue Rollenzuweisung hinzuzufügen.
Wählen Sie unter „Rolle“ in der Dropdownliste die Option „Storage-Blobdatenleser“ aus.
5. Wählen Sie in der nächsten Dropdownliste unter „Zugriff zuweisen zu“ die Option `User assigned managed identity` aus.
6. Stellen Sie im nächsten Schritt sicher, dass in der Dropdownliste „Abonnement“ das richtige Abonnement aufgeführt ist, und legen Sie dann für die Ressourcengruppe die Option „Alle Ressourcengruppen“ fest.
7. Wählen Sie unter „Auswählen“ die benutzerseitig zugewiesene Identität aus, die der Service Fabric-Anwendung entspricht, und klicken Sie auf „Speichern“.

Die Unterstützung von systemseitig zugewiesenen verwalteten Identitäten für Service Fabric ist nicht in das Azure-Portal integriert. Wenn Ihre Anwendung eine systemseitig zugewiesene Identität verwendet, müssen Sie zuerst die Client-ID der Anwendungsidentität finden und dann die oben beschriebenen Schritte wiederholen. Wählen Sie in diesem Fall im Steuerelement „Suchen“ die Option `Azure AD user, group, or service principal` aus.

## <a name="granting-access-to-azure-key-vault"></a>Gewähren des Zugriffs auf Azure Key Vault
Ähnlich wie beim Zugriff auf Storage können Sie die verwaltete Identität einer Service Fabric-Anwendung für den Zugriff auf eine Azure Key Vault-Instanz nutzen. Die Schritte zum Gewähren des Zugriffs im Azure-Portal entsprechen den oben aufgeführten und werden an dieser Stelle nicht wiederholt. Die Unterschiede sehen Sie in der folgenden Abbildung.

![Key Vault-Zugriffsrichtlinie](../key-vault/media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

Das folgende Beispiel veranschaulicht das Gewähren des Zugriffs auf einen Tresor per Vorlagenbereitstellung. Fügen Sie den unten stehenden Codeausschnitt als weiteren Eintrag unter dem `resources`-Element der Vorlage hinzu.

```json
    {
        "type": "Microsoft.KeyVault/vaults/accessPolicies",
        "name": "[concat(parameters('keyVaultName'), '/add')]",
        "apiVersion": "2018-02-14",
        "properties": {
            "accessPolicies": [
                {
                    "tenantId": "[reference(variables('userAssignedIdentityResourceId'), '2018-11-30').tenantId]",
                    "objectId": "[reference(variables('userAssignedIdentityResourceId'), '2018-11-30').principalId]",
                    "dependsOn": [
                        "[variables('userAssignedIdentityResourceId')]"
                    ],
                    "permissions": {
                        "keys":         ["get", "list"],
                        "secrets":      ["get", "list"],
                        "certificates": ["get", "list"]
                    }
                }
            ]
        }
    },
```

Weitere Informationen finden Sie unter [Tresore: Aktualisieren der Zugriffsrichtlinie](https://docs.microsoft.com/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="next-steps"></a>Nächste Schritte

* [Bereitstellen einer Azure Service Fabric-Anwendung mit einer systemseitig zugewiesenen verwalteten Identität](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)

* [Bereitstellen einer Azure Service Fabric-Anwendung mit einer benutzerseitig zugewiesenen verwalteten Identität](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)

## <a name="related-articles"></a>Verwandte Artikel

* Überprüfen der [Unterstützung für verwaltete Identitäten](./concepts-managed-identity.md) in Azure Service Fabric

* [Bereitstellen eines neuen](./configure-new-azure-service-fabric-enable-managed-identity.md) Azure Service Fabric-Clusters mit Unterstützung verwalteter Identitäten 

* [Aktivieren von verwalteten Identitäten](./configure-existing-cluster-enable-managed-identity-token-service.md) in einem vorhandenen Azure Service Fabric-Cluster

* Nutzen der [verwalteten Identität einer Service Fabric-Anwendung aus dem Quellcode](./how-to-managed-identity-service-fabric-app-code.md)

* Liste der [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](../active-directory/managed-identities-azure-resources/services-support-msi.md)
