---
title: Konfigurieren Ihres eigenen Schlüssels zum Verschlüsseln ruhender Azure Event Hubs-Daten
description: Dieser Artikel enthält Informationen dazu, wie Sie einen eigenen Schlüssel für die Verschlüsselung ruhender Azure Event Hubs-Daten konfigurieren.
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: spelluru
ms.openlocfilehash: 63fe6c4a2d02489b5e25100aa6aa23407bbe6bc7
ms.sourcegitcommit: ec2b75b1fc667c4e893686dbd8e119e7c757333a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72809369"
---
# <a name="configure-customer-managed-keys-for-encrypting-azure-event-hubs-data-at-rest-by-using-the-azure-portal"></a>Konfigurieren von kundenseitig verwalteten Schlüsseln für die Verschlüsselung ruhender Azure Event Hubs-Daten mithilfe des Azure-Portals
Azure Event Hubs ermöglicht die Verschlüsselung ruhender Daten mit Azure Storage Service Encryption (Azure SSE). Event Hubs verwendet Azure Storage zum Speichern der Daten. Standardmäßig werden alle Daten, die mit Azure Storage gespeichert werden, durch von Microsoft verwaltete Schlüssel verschlüsselt. 

## <a name="overview"></a>Übersicht
Azure Event Hubs unterstützt jetzt die Option zum Verschlüsseln ruhender Daten mit von Microsoft oder vom Kunden verwalteten Schlüsseln (Bring your own Key, BYOK). Mit dieser Funktion können Sie kundenseitig verwaltete Schlüssel, die zum Verschlüsseln ruhender Azure Event Hubs-Daten verwendet werden, erstellen, rotieren, deaktivieren und den Zugriff darauf widerrufen.

Die Aktivierung der BYOK-Funktion ist ein einmaliger Setupvorgang für Ihren Namespace.

> [!NOTE]
> Die BYOK-Funktion wird von [Event Hubs Dedicated](event-hubs-dedicated-overview.md)-Clustern mit einem Mandanten unterstützt. Sie kann nicht für Event Hubs-Standardnamespaces aktiviert werden.

Verwenden Sie Azure Key Vault, um Ihre Schlüssel zu verwalten und die Schlüsselverwendung zu überwachen. Sie können entweder Ihre eigenen Schlüssel erstellen und in einem Schlüsseltresor speichern oder mit den Azure Key Vault-APIs Schlüssel generieren. Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](../key-vault/key-vault-overview.md)

In diesem Artikel erfahren Sie, wie Sie einen Schlüsseltresor mit kundenseitig verwalteten Schlüsseln mithilfe des Azure-Portals konfigurieren. Informationen zum Erstellen eines Schlüsseltresors über das Azure-Portal finden Sie unter [Schnellstart: Festlegen eines Geheimnisses und Abrufen des Geheimnisses aus Azure Key Vault mithilfe des Azure-Portals](../key-vault/quick-create-portal.md).

> [!IMPORTANT]
> Für die Verwendung kundenseitig verwalteter Schlüssel mit Azure Event Hubs müssen für den Schlüsseltresor zwei erforderliche Eigenschaften konfiguriert werden. Sie lauten wie folgt:  **Vorläufiges Löschen** und **Do Not Purge** (Nicht bereinigen). Diese Eigenschaften sind standardmäßig aktiviert, wenn Sie im Azure-Portal einen neuen Schlüsseltresor erstellen. Wenn Sie diese Eigenschaften jedoch für einen vorhandenen Schlüsseltresor aktivieren möchten, müssen Sie PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden.

## <a name="enable-customer-managed-keys"></a>Aktivieren von vom Kunden verwalteten Schlüsseln
Um vom Kunden verwaltete Schlüssel im Azure-Portal zu aktivieren, gehen Sie folgendermaßen vor:

1. Navigieren Sie zu Ihrem Event Hubs Dedicated-Cluster.
1. Wählen Sie den Namespace aus, für den Sie BYOK aktivieren möchten.
1. Wählen Sie auf der Seite **Einstellungen** des Event Hubs-Namespace die Option **Verschlüsselung (Vorschau)** aus. 
1. Wählen Sie die **Verschlüsselung im Ruhezustand mit kundenseitig verwalteten Schlüsseln**  wie in der folgenden Abbildung gezeigt aus. 

    ![Kundenseitig verwaltete Schlüssel aktivieren](./media/configure-customer-managed-key/enable-customer-managed-key.png)

## <a name="set-up-a-key-vault-with-keys"></a>Einrichten eines Schlüsseltresors mit Schlüsseln
Nachdem Sie kundenseitig verwaltete Schlüssel aktiviert haben, müssen Sie den kundenseitig verwalteten Schlüssel Ihrem Azure Event Hubs-Namespace zuordnen. Event Hubs unterstützt nur Azure Key Vault. Wenn Sie die Option **Verschlüsselung mit kundenseitig verwaltetem Schlüssel** im vorherigen Abschnitt aktivieren, muss der Schlüssel in Azure Key Vault importiert werden. Außerdem müssen die Eigenschaften **Vorläufiges Löschen** und **Do Not Purge** (Nicht bereinigen) für den Schlüssel konfiguriert sein. Diese Einstellungen können mithilfe von [PowerShell](../key-vault/key-vault-soft-delete-powershell.md) oder der [CLI](../key-vault/key-vault-soft-delete-cli.md#enabling-purge-protection) konfiguriert werden.

1. Befolgen Sie zum Erstellen eines neuen Schlüsseltresors die [Schnellstartanleitung](../key-vault/key-vault-overview.md) für Azure Key Vault. Weitere Informationen zum Importieren vorhandener Schlüssel finden Sie unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](../key-vault/about-keys-secrets-and-certificates.md).
1. Verwenden Sie den Befehl [az keyvault create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create), um beim Erstellen eines Tresors sowohl das vorläufige Löschen als auch den Bereinigungsschutz zu aktivieren.

    ```azurecli-interactive
    az keyvault create --name ContosoVault --resource-group ContosoRG --location westus --enable-soft-delete true --enable-purge-protection true
    ```    
1. Verwenden Sie den Befehl [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update), um einem vorhandenen Tresor (für den bereits das vorläufige Löschen aktiviert ist) den Bereinigungsschutz hinzuzufügen.

    ```azurecli-interactive
    az keyvault update --name ContosoVault --resource-group ContosoRG --enable-purge-protection true
    ```
1. Führen Sie diese Schritte durch, um Schlüssel zu erstellen:
    1. Wählen Sie zum Erstellen eines neuen Schlüssels **Generieren/Importieren** im Menü **Schlüssel** unter **Einstellungen** aus.
        
        ![Auswählen der Schaltfläche „Generate/Import“ (Generieren/Importieren)](./media/configure-customer-managed-key/select-generate-import.png)
    1. Legen Sie **Optionen** auf **Generieren** fest, und benennen Sie den Schlüssel.

        ![Erstellen eines Schlüssels](./media/configure-customer-managed-key/create-key.png) 
    1. Sie können diesen Schlüssel jetzt aus der Dropdownliste auswählen, um ihn dem Event Hubs-Namespace zum Verschlüsseln zuzuordnen. 

        ![Auswählen des Schlüssels aus dem Schlüsseltresor](./media/configure-customer-managed-key/select-key-from-key-vault.png)
    1. Geben Sie die Details für den Schlüssel ein, und klicken Sie auf **Auswählen**. Dadurch wird die Verschlüsselung von ruhenden Daten im Namespace mit einem kundenseitig verwalteten Schlüssel ermöglicht. 

        > [!NOTE]
        > Für die Vorschau können Sie nur einen einzelnen Schlüssel auswählen. 

## <a name="rotate-your-encryption-keys"></a>Rotieren der Verschlüsselungsschlüssel
Sie können Ihren Schlüssel im Schlüsseltresor mit dem Rotationsmechanismus von Azure Key Vault rotieren. Weitere Informationen finden Sie unter [Einrichten von Azure Key Vault mit Schlüsselrotation und Überwachung](../key-vault/key-vault-key-rotation-log-monitoring.md). Aktivierungs- und Ablaufdaten können auch festgelegt werden, um die Schlüsselrotation zu automatisieren. Der Event Hubs-Dienst erkennt neue Schlüsselversionen und beginnt automatisch mit ihrer Verwendung.

## <a name="revoke-access-to-keys"></a>Widerrufen des Zugriffs auf Schlüssel
Wenn Sie den Zugriff auf die Verschlüsselungsschlüssel widerrufen, werden die Daten dadurch nicht aus Event Hubs gelöscht. Der Zugriff auf die Daten ist jedoch nicht über den Event Hubs-Namespace möglich. Sie können den Verschlüsselungsschlüssel mithilfe von Zugriffsrichtlinien oder durch das Löschen des Schlüssels widerrufen. Unter [Sicherer Zugriff auf einen Schlüsseltresor](../key-vault/key-vault-secure-your-key-vault.md) erfahren Sie mehr über Zugriffsrichtlinien und das Sichern Ihres Schlüsseltresors.

Nachdem der Verschlüsselungsschlüssel widerrufen wurde, funktioniert der Event Hubs-Dienst im verschlüsselten Namespace nicht mehr. Wenn der Zugriff auf den Schlüssel aktiviert ist oder der gelöschte Schlüssel wiederhergestellt wurde, wählt der Event Hubs-Dienst den Schlüssel aus, sodass Sie aus dem verschlüsselten Event Hubs-Namespace auf die Daten zugreifen können.

> [!NOTE]
> Wenn Sie einen vorhandenen Verschlüsselungsschlüssel aus Ihrem Schlüsseltresor löschen und ihn durch einen neuen Schlüssel im Event Hubs-Namespace ersetzen, ist der Zugriff auf Ihre alten Daten (die mit dem alten Schlüssel verschlüsselt wurden) zusammen mit den neuen Daten, auf die jetzt nur mit dem neuen Schlüssel zugegriffen werden kann, unter Umständen weiterhin möglich, da der gelöschte Schlüssel noch bis zu einer Stunde lang gültig ist (weil er zwischengespeichert ist). Dieses Verhalten ist in der Vorschauversion des Features standardmäßig integriert. 

## <a name="set-up-diagnostic-logs"></a>Einrichten von Diagnoseprotokollen 
Durch das Einrichten von Diagnoseprotokollen für durch BYOK aktivierte Namespaces erhalten Sie die erforderlichen Informationen zu den Vorgängen, wenn ein Namespace mit kundenseitig verwalteten Schlüsseln verschlüsselt ist. Diese Protokolle können aktiviert und später in einen Event Hub gestreamt, mithilfe von Protokollanalysen analysiert oder in den Speicher gestreamt werden, um benutzerdefinierte Analysen auszuführen. Weitere Informationen zu Diagnoseprotokollen finden Sie unter [Übersicht über Azure-Diagnoseprotokolle](../azure-monitor/platform/resource-logs-overview.md).

## <a name="enable-user-logs"></a>Aktivieren von Benutzerprotokollen
Führen Sie die folgenden Schritte aus, um Protokolle für kundenseitig verwaltete Schlüssel zu aktivieren.

1. Navigieren Sie im Azure-Portal zum Namespace, für den BYOK aktiviert ist.
1. Wählen Sie unter **Überwachung** die Option **Diagnoseeinstellungen** aus.

    ![Auswählen der Diagnoseeinstellungen](./media/configure-customer-managed-key/select-diagnostic-settings.png)
1. Klicken Sie auf **Diagnoseeinstellung hinzufügen**. 

    ![Auswählen von „Diagnoseeinstellung hinzufügen“](./media/configure-customer-managed-key/select-add-diagnostic-setting.png)
1. Geben Sie einen **Namen** an, und wählen Sie aus, wohin die Protokolle gestreamt werden sollen.
1. Wählen Sie **CustomerManagedKeyUserLogs** und anschließend die Option **Speichern** aus. Diese Aktion aktiviert die Protokolle für BYOK im Namespace.

    ![Auswählen der Option für kundenseitig verwaltete Schlüsselbenutzerprotokolle](./media/configure-customer-managed-key/select-customer-managed-key-user-logs.png)

## <a name="log-schema"></a>Protokollschema 
Alle Protokolle werden im JavaScript Object Notation (JSON)-Format gespeichert. Jeder Eintrag enthält Zeichenfolgenfelder im Format, das in der nachfolgenden Tabelle beschrieben ist. 

| NAME | BESCHREIBUNG |
| ---- | ----------- | 
| TaskName | Beschreibung der Aufgabe, bei der ein Fehler aufgetreten ist |
| ActivityId | Interne ID zur Nachverfolgung. |
| category | Definiert die Klassifizierung der Aufgabe. Wenn beispielsweise der Schlüssel aus Ihrem Schlüsseltresor deaktiviert wird, dann handelt es sich hierbei um eine Informationskategorie. Wenn ein Schlüssel nicht entpackt werden kann, dann handelt es sich um einen Fehler. |
| resourceId | Azure Resource Manager-Ressourcen-ID |
| keyVault | Der vollständige Name des Schlüsseltresors |
| key | Der Schlüsselname, der zum Verschlüsseln des Event Hubs-Namespace verwendet wird |
| version | Die Version des verwendeten Schlüssels |
| operation | Der Vorgang, der für den Schlüssel in Ihrem Schlüsseltresor ausgeführt wird. Dazu zählen etwa das Deaktivieren/Aktvieren, das Packen und das Entpacken des Schlüssels. |
| code | Der Code, der dem Vorgang zugeordnet ist. Beispiel: Der Fehlercode 404 bedeutet, dass der Schlüssel nicht gefunden wurde. |
| message | Eine Fehlermeldung, die dem Vorgang zugeordnet ist |

Im Folgenden finden Sie ein Beispiel für das Protokoll für einen kundenseitig verwalteten Schlüssel:

```json
{
   "TaskName": "CustomerManagedKeyUserLog",
   "ActivityId": "11111111-1111-1111-1111-111111111111",
   "category": "error"
   "resourceId": "/SUBSCRIPTIONS/11111111-1111-1111-1111-11111111111/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
   "keyVault": "https://mykeyvault.vault-int.azure-int.net",
   "key": "mykey",
   "version": "1111111111111111111111111111111",
   "operation": "wrapKey",
   "code": "404",
   "message": "Key not found: ehbyok0/111111111111111111111111111111",
}



{
   "TaskName": "CustomerManagedKeyUserLog",
   "ActivityId": "11111111111111-1111-1111-1111111111111",
   "category": "info"
   "resourceId": "/SUBSCRIPTIONS/111111111-1111-1111-1111-11111111111/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
   "keyVault": "https://mykeyvault.vault-int.azure-int.net",
   "key": "mykey",
   "version": "111111111111111111111111111111",
   "operation": "disable" | "restore",
   "code": "",
   "message": "",
}
```

## <a name="troubleshoot"></a>Problembehandlung
Es wird empfohlen, Protokolle immer wie im vorherigen Abschnitt gezeigt zu aktivieren. Dies hilft dabei, die Aktivitäten nachzuverfolgen, wenn die BYOK-Verschlüsselung aktiviert ist. Außerdem können dadurch Probleme eingegrenzt werden.

Im Folgenden finden Sie die allgemeinen Fehlercodes, nach denen Sie suchen müssen, wenn die BYOK-Verschlüsselung aktiviert ist.

| Aktion | Fehlercode | Resultierender Zustand der Daten |
| ------ | ---------- | ----------------------- | 
| Widerrufen der Berechtigung zum Packen/Entpacken aus einem Schlüsseltresor | 403 |    Zugriff nicht möglich |
| Entfernen der AAD-Rollenmitgliedschaft aus einem AAD-Prinzipal, der die Berechtigung zum Packen/Entpacken gewährt hat | 403 |  Zugriff nicht möglich |
| Löschen eines Verschlüsselungsschlüssels aus dem Schlüsseltresor | 404 | Zugriff nicht möglich |
| Löschen des Schlüsseltresors | 404 | Zugriff nicht möglich (unter der Annahme, dass vorläufiges Löschen aktiviert ist, da dies eine erforderliche Einstellung ist) |
| Ändern des Ablaufzeitraums für den Verschlüsselungsschlüssel, sodass er bereits abgelaufen ist | 403 |   Zugriff nicht möglich  |
| Ändern des Werts für NBF (not before, nicht vor), sodass der Schlüsselverschlüsselungsschlüssel nicht aktiv ist | 403 | Zugriff nicht möglich  |
| Auswählen der Option **Allow MSFT Services** (MSFT-Dienste zulassen) für die Firewall des Schlüsseltresors oder anderweitiges Blockieren des Netzwerkzugriffs auf den Schlüsseltresor, der den Verschlüsselungsschlüssel enthält | 403 | Zugriff nicht möglich |
| Verschieben des Schlüsseltresors in einen anderen Mandanten | 404 | Zugriff nicht möglich |  
| Zeitweilig auftretendes Netzwerkproblem oder DNS-/AAD-/MSI-Ausfall |  | Zugriff mithilfe des zwischengespeicherten Datenverschlüsselungsschlüssels |

> [!IMPORTANT]
> Zum Aktivieren der georedundanten Notfallwiederherstellung in einem Namespace, der die BYOK-Verschlüsselung verwendet, muss sich der sekundäre Namespace für die Kopplung in einem dedizierten Cluster befinden, und eine vom System zugewiesene verwaltete Identität muss für ihn aktiviert sein. Weitere Informationen finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).

> [!NOTE]
> Wenn Dienstendpunkte eines virtuellen Netzwerks (VNET) in Azure Key Vault für Ihren Event Hubs-Namespace konfiguriert sind, wird BYOK nicht unterstützt. 


## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:
- [Übersicht über Event Hubs](event-hubs-about.md)
- [Übersicht über Key Vault](../key-vault/key-vault-overview.md)




