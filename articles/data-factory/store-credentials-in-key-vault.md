---
title: Speichern von Anmeldeinformationen in Azure Key Vault
description: Erfahren Sie, wie Sie Anmeldeinformationen für verwendete Datenspeicher in einem Azure-Schlüsseltresor speichern können, die von Azure Data Factory zur Laufzeit automatisch abgerufen werden können.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: jingwang
ms.openlocfilehash: 1e2458fa125ee4d223ef46001534fb7ae72b805e
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684007"
---
# <a name="store-credential-in-azure-key-vault"></a>Speichern von Anmeldeinformationen in Azure Key Vault

Sie können Anmeldeinformationen für Datenspeicher und Computervorgänge in einer [Azure Key Vault](../key-vault/key-vault-overview.md)-Instanz speichern. Azure Data Factory ruft die Anmeldeinformationen ab, wenn eine Aktivität ausgeführt wird, die den Datenspeicher/Computevorgänge verwendet.

Derzeit unterstützen mit Ausnahme von benutzerdefinierten Aktivitäten alle Aktivitätstypen dieses Feature. Lesen Sie insbesondere für die Connectorkonfiguration die Details im Abschnitt „Eigenschaften des verknüpften Diensts“ in den [Themen zu den einzelnen Connectors](copy-activity-overview.md#supported-data-stores-and-formats).

## <a name="prerequisites"></a>Voraussetzungen

Diese Funktion basiert auf der verwalteten Data Factory-Identität. Informationen zur Funktionsweise finden Sie unter [Verwaltete Data Factory-Identität](data-factory-service-identity.md). Stellen Sie sicher, dass Ihrer Data Factory eine verwaltete Identität zugeordnet ist.

## <a name="steps"></a>Schritte

Führen Sie die folgenden Schritte aus, um auf in Azure Key Vault gespeicherte Anmeldeinformationen zu verweisen:

1. **Rufen Sie die verwaltete Data Factory-Identität ab**, indem Sie den Wert von „Anwendungs-ID der verwalteten Identität“ kopieren, der zusammen mit der Factory generiert wurde. Bei Verwendung der ADF-Erstellungsbenutzeroberfläche wird die Anwendungs-ID der verwalteten Identität im Erstellungsfenster des verknüpften Azure Key Vault-Diensts angezeigt. Sie können sie auch vom Azure-Portal aus abrufen, siehe [Abrufen der verwalteten Data Factory-Identität](data-factory-service-identity.md#retrieve-managed-identity).
2. **Gewähren Sie der verwalteten Identität Zugriff auf Ihren Azure Key Vault**. Suchen Sie in Ihrem Schlüsseltresor unter „Zugriffsrichtlinien“ -> „Neue hinzufügen“ nach dieser Anwendungs-ID der verwalteten Identität, um ihr in der Dropdownliste „Berechtigungen für Geheimnis“ die **Abrufberechtigung** zu erteilen. Dies ermöglicht der angegebenen Data Factory den Zugriff auf das Geheimnis im Schlüsseltresor.
3. **Erstellen Sie einen verknüpften Dienst, der auf Ihren Azure Key Vault-Tresor verweist.** Siehe [Mit Azure Key Vault verknüpfter Dienst](#azure-key-vault-linked-service).
4. **Erstellen Sie einen mit einem Datenspeicher verknüpften Dienst, in dem Sie auf das entsprechende Geheimnis verweisen, das im Schlüsseltresor gespeichert ist.** Informationen finden Sie unter [Verweisen auf ein im Schlüsseltresor gespeichertes Geheimnis](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Mit Azure Key Vault verknüpfter Dienst

Folgende Eigenschaften werden für den mit Azure Key Vault verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf Folgendes festgelegt werden: **AzureKeyVault**. | Ja |
| baseUrl | Geben Sie die Azure Key Vault-URL an. | Ja |

**Verwendung der Erstellungsbenutzeroberfläche:**

Klicken Sie auf **Verbindungen** -> **Verknüpfte Dienste** ->  **+Neu**, und suchen Sie nach „Azure Key Vault“:

![Nach AKV suchen](media/store-credentials-in-key-vault/search-akv.png)

Wählen Sie die bereitgestellte Azure Key Vault-Instanz aus, in der Ihre Anmeldeinformationen gespeichert sind. Sie können die **Verbindung testen**, um sicherzustellen, dass Ihre AKV Verbindung gültig ist. 

![AKV konfigurieren](media/store-credentials-in-key-vault/configure-akv.png)

**JSON-Beispiel**:

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-secret-stored-in-key-vault"></a>Verweisen auf ein im Schlüsseltresor gespeichertes Geheimnis

Die folgenden Eigenschaften werden unterstützt, wenn Sie ein Feld in einem verknüpften Dienst konfigurieren, das auf ein Geheimnis im Schlüsseltresor verweist:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Felds muss auf folgenden Wert festgelegt werden: **AzureKeyVaultSecret**. | Ja |
| secretName | Der Name des Geheimnisses in Azure Key Vault | Ja |
| secretVersion | Die Version des Geheimnisses in Azure Key Vault<br/>Falls nicht angegeben, wird immer die neueste Version des Geheimnisses verwendet.<br/>Falls angegeben, wird die angegebene Version verwendet.| Nein |
| store | Verweist auf einen mit Azure Key Vault verknüpften Dienst, in dem Sie die Anmeldeinformationen speichern. | Ja |

**Verwendung der Erstellungsbenutzeroberfläche:**

Wählen Sie beim Erstellen der Verbindung mit Ihrem Datenspeicher/Computedienst **Azure Key Vault** für Geheimnisfelder aus. Wählen Sie den bereitgestellten verknüpften Azure Key Vault-Dienst aus, und geben Sie den **Geheimnisnamen** an. Optional können Sie auch eine Geheimnisversion bereitstellen. 

>[!TIP]
>Für Connectors, die eine Verbindungszeichenfolge in verknüpften Diensten wie SQL Server, BLOB-Speicher usw. verwenden, können Sie entweder nur das Geheimnisfeld speichern, z. B. das Kennwort in AKV, oder die gesamte Verbindungszeichenfolge in AKV speichern. Sie finden beide Optionen in der Benutzeroberfläche.

![AKV-Geheimnis konfigurieren](media/store-credentials-in-key-vault/configure-akv-secret.png)

**JSON-Beispiel: (siehe den Abschnitt „Kennwort“)**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).
