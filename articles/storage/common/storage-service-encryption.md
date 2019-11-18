---
title: Azure Storage-Verschlüsselung für ruhende Daten | Microsoft-Dokumentation
description: Azure Storage schützt Ihre Daten, indem der Dienst diese automatisch verschlüsselt, bevor diese in der Cloud gespeichert werden. Sie können von Microsoft verwaltete Schlüssel für die Verschlüsselung Ihres Speicherkontos nutzen, oder Sie können die Verschlüsselung mit Ihren eigenen Schlüsseln verwalten.
services: storage
author: tamram
ms.service: storage
ms.date: 10/02/2019
ms.topic: conceptual
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 42c674e236d769d48f6f17fc43494ac006219a8a
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795698"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Azure Storage-Verschlüsselung für ruhende Daten

Azure Storage verschlüsselt Ihre Daten beim Speichern in der Cloud automatisch. Die Verschlüsselung schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation. Daten in Azure Storage werden auf transparente Weise mit der [AES-256-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) ver- und entschlüsselt, einer der stärksten verfügbaren Blockchiffren, und sind mit dem FIPS 140-2-Standard konform. Die Azure Storage-Verschlüsselung ähnelt der BitLocker-Verschlüsselung unter Windows.

Die Azure Storage-Verschlüsselung wird für alle neuen und vorhandenen Speicherkonten aktiviert und kann nicht deaktiviert werden. Da Ihre Daten standardmäßig geschützt werden, müssen Sie weder Code noch Anwendungen ändern, um die Azure Storage-Verschlüsselung nutzen zu können.

Speicherkonten werden unabhängig von ihrer Leistungsstufe (Standard oder Premium) und ihrem Bereitstellungsmodell (Azure Resource Manager oder klassisch) verschlüsselt. Alle Redundanzoptionen in Azure Storage unterstützten die Verschlüsselung, und alle Kopien eines Speicherkontos werden verschlüsselt. Alle Azure Storage-Ressourcen werden verschlüsselt, z. B. Blobs, Datenträger, Dateien, Warteschlangen und Tabellen. Objektmetadaten werden ebenfalls verschlüsselt.

Die Verschlüsselung hat keine Auswirkungen auf die Leistung von Azure Storage. Es fallen keine zusätzlichen Kosten für die Azure Storage-Verschlüsselung an.

Weitere Informationen zu den kryptografischen Modulen, die der Azure Storage-Verschlüsselung zugrunde liegen, finden Sie unter [Kryptografie-API: Die nächste Generation](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="about-encryption-key-management"></a>Informationen zur Verwaltung von Verschlüsselungsschlüsseln

Sie können von Microsoft verwaltete Schlüssel für die Verschlüsselung Ihres Speicherkontos nutzen, oder Sie können die Verschlüsselung mit Ihren eigenen Schlüsseln verwalten. Wenn Sie die Verschlüsselungsverwaltung mit Ihren eigenen Schlüsseln wählen, haben Sie zwei Optionen:

- Sie können einen *vom Kunden verwalteten Schlüssel* angeben, der zum Verschlüsseln und Entschlüsseln aller Daten im Speicherkonto verwendet werden soll. Mit einem vom Kunden verwalteten Schlüssel werden sämtliche Daten in allen Diensten in Ihrem Speicherkonto verwaltet.
- Sie können einen *vom Kunden bereitgestellten Schlüssel* für Blob-Speichervorgänge angeben. Ein Client, der eine Lese- oder Schreibanforderung für Blob Storage sendet, kann einen Verschlüsselungsschlüssel für die Anforderung enthalten, um genau steuern zu können, wie Blobdaten verschlüsselt und entschlüsselt werden.

In der folgenden Tabelle werden die Schlüsselverwaltungsoptionen für Azure Storage-Verschlüsselung verglichen.

|                                        |    Von Microsoft verwaltete Schlüssel                             |    Vom Kunden verwaltete Schlüssel                                                                                                                        |    Vom Kunden bereitgestellte Schlüssel                                                          |
|----------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|    Verschlüsselungs-/Entschlüsselungsvorgänge    |    Azure                                              |    Azure                                                                                                                                        |    Azure                                                                         |
|    Unterstützte Azure Storage-Dienste    |    Alle                                                |    Blob Storage, Azure Files                                                                                                               |    Blob Storage                                                                  |
|    Schlüsselspeicher                         |    Microsoft-Schlüsselspeicher    |    Azure Key Vault                                                                                                                              |    Azure Key Vault oder ein beliebiger anderer Schlüsselspeicher                                                                 |
|    Verantwortlich für die Schlüsselrotation         |    Microsoft                                          |    Kunde                                                                                                                                     |    Kunde                                                                      |
|    Schlüsselverwendung                           |    Microsoft                                          |    Azure-Portal, REST-API für Speicherressourcenanbieter, Azure Storage-Verwaltungsbibliotheken, PowerShell, CLI        |    REST-API für Azure Storage (Blob Storage), Azure Storage-Clientbibliotheken    |
|    Schlüsselzugriff                          |    Nur Microsoft                                     |    Microsoft, Kunde                                                                                                                    |    Nur Kunde                                                                 |

In den folgenden Abschnitten werden die einzelnen Optionen für die Schlüsselverwaltung ausführlicher beschrieben.

## <a name="microsoft-managed-keys"></a>Von Microsoft verwaltete Schlüssel

Standardmäßig verwendet Ihr Speicherkonto von Microsoft verwaltete Verschlüsselungsschlüssel. Im Bereich **Verschlüsselung** im [Azure-Portal](https://portal.azure.com) können Sie sich wie auf dem folgenden Screenshot gezeigt die Verschlüsselungseinstellungen für Ihr Speicherkonto ansehen:

![Konto, das mit von Microsoft verwalteten Schlüsseln verschlüsselt wird](media/storage-service-encryption/encryption-microsoft-managed-keys.png)

## <a name="customer-managed-keys"></a>Vom Kunden verwaltete Schlüssel

Sie können wählen, dass Sie die Azure Storage-Verschlüsselung auf der Ebene des Speicherkontos mit Ihren eigenen Schlüsseln verwalten möchten. Wenn Sie einen vom Kunden verwalteten Schlüssel auf der Ebene des Speicherkontos angeben, wird dieser Schlüssel zum Verschlüsseln und Entschlüsseln aller Daten im Speicherkonto verwendet, einschließlich Blob-, Warteschlangen-, Datei-und Tabellendaten.  Vom Kunden verwaltete Schlüssel bieten größere Flexibilität beim Erstellen, Rotieren, Deaktivieren und Widerrufen von Zugriffssteuerungen. Außerdem können Sie die zum Schutz Ihrer Daten verwendeten Verschlüsselungsschlüssel überwachen.

Sie müssen Azure Key Vault zum Speichern Ihrer vom Kunden verwalteten Schlüssel verwenden. Sie können entweder Ihre eigenen Schlüssel erstellen und in einem Schlüsseltresor speichern oder mit den Azure Key Vault-APIs Schlüssel generieren. Das Speicherkonto und der Schlüsseltresor müssen sich in derselben Region befinden, dürfen aber zu verschiedenen Abonnements gehören. Weitere Informationen zum Azure Key Vault finden Sie unter [What is Azure Key Vault? (Was ist der Azure Key Vault?)](../../key-vault/key-vault-overview.md).

Dieses Diagramm zeigt, wie Azure Active Directory und Azure Key Vault von Azure Storage verwendet werden, um Anforderungen mit dem vom Kunden verwalteten Schlüssel zu stellen:

![Das Diagramm zeigt, wie vom Kunden verwaltete Schlüssel in Azure Storage funktionieren](media/storage-service-encryption/encryption-customer-managed-keys-diagram.png)

In der folgenden Liste werden die nummerierten Schritte im Diagramm erläutert:

1. Ein Azure Key Vault-Administrator erteilt Berechtigungen für Verschlüsselungsschlüssel an die dem Speicherkonto zugeordnete verwaltete Identität.
2. Ein Azure Storage-Administrator konfiguriert die Verschlüsselung mit einem vom Kunden verwalteten Schlüssel für das Speicherkonto.
3. Azure Storage verwendet die dem Speicherkonto zugeordnete verwaltete Identität, um den Zugriff auf Azure Key Vault über Azure Active Directory zu authentifizieren.
4. Azure Storage umschließt den Kontoverschlüsselungsschlüssel mit dem Kundenschlüssel in Azure Key Vault.
5. Bei Lese-/Schreibvorgängen sendet Azure Storage Anforderungen an Azure Key Vault, um den Kontoverschlüsselungsschlüssel zu umschließen und die Umschließung aufzuheben und so Verschlüsselungs- und Entschlüsselungsvorgänge auszuführen.

Informationen zum Widerrufen des Zugriffs auf vom Kunden verwalteten Schlüssel für das Speicherkonto finden Sie in den Artikeln zu [Azure Key Vault-PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) und zur [Azure Key Vault-CLI](https://docs.microsoft.com/cli/azure/keyvault). Durch das Widerrufen des Zugriffs wird der Zugriff auf alle Daten im Speicherkonto blockiert, da Azure Storage keinen Zugriff mehr auf den Verschlüsselungsschlüssel hat.

Von Kunden verwaltete Schlüssel sind auch für verwaltete Azure-Datenträger als öffentliche Vorschau verfügbar und funktionieren bei verwalteten Datenträgern ein wenig anders als beim übrigen Speicher. Weitere Informationen finden Sie in unserem [Artikel zum Thema](../../virtual-machines/linux/disk-encryption.md#customer-managed-keys-public-preview).

In den folgenden Artikeln erhalten Sie Informationen zum Verwenden von Kunden verwalteter Schlüsseln mit Azure Storage:

- [Configure customer-managed keys for Azure Storage encryption from the Azure portal (Konfigurieren von Kunden verwalteter Schlüssel für die Azure Storage-Verschlüsselung mithilfe des Azure-Portals)](storage-encryption-keys-portal.md)
- [Configure customer-managed keys for Azure Storage encryption from PowerShell (Konfigurieren von Kunden verwalteter Schlüssel für die Azure Storage-Verschlüsselung mithilfe von Azure PowerShell)](storage-encryption-keys-powershell.md)
- [Use customer-managed keys with Azure Storage encryption from Azure CLI (Verwenden von Kunden verwalteter Schlüssel mit der Azure Storage-Verschlüsselung mithilfe der Azure CLI)](storage-encryption-keys-cli.md)

> [!IMPORTANT]
> Von Kunden verwaltete Schlüssel basieren auf verwalteten Identitäten für Azure-Ressourcen, einem Feature von Azure Active Directory (Azure AD). Wenn Sie vom Kunden verwaltete Schlüssel im Azure-Portal konfigurieren, wird Ihrem Speicherkonto eine verwaltete Identität im Hintergrund automatisch zugewiesen. Wenn Sie anschließend das Abonnement, die Ressourcengruppe oder das Speicherkonto von einem Azure AD-Verzeichnis in ein anderes Verzeichnis verschieben, wird die dem Speicherkonto zugeordnete verwaltete Identität nicht an den neuen Mandanten übertragen, sodass vom Kunden verwaltete Schlüssel möglicherweise nicht mehr funktionieren. Weitere Informationen finden Sie in [Häufig gestellte Fragen und bekannte Probleme mit verwalteten Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories) unter **Übertragen eines Abonnements zwischen Azure AD-Verzeichnissen**.  

## <a name="customer-provided-keys-preview"></a>Vom Kunden bereitgestellte Schlüssel (Vorschau)

Clients, die Anforderungen für Azure Blob Storage stellen, haben die Möglichkeit, einen Verschlüsselungsschlüssel für eine einzelne Anforderung anzugeben. Die Einbeziehung des Verschlüsselungsschlüssels in die Anforderung ermöglicht eine präzise Steuerung der Verschlüsselungseinstellungen für Blob-Speichervorgänge. Vom Kunden bereitgestellte Schlüssel (Vorschau) können in Azure Key Vault oder einem anderen Schlüsselspeicher gespeichert werden.

### <a name="encrypting-read-and-write-operations"></a>Verschlüsseln von Lese-und Schreibvorgängen

Wenn eine Clientanwendung einen Verschlüsselungsschlüssel auf die Anforderung hin bereitstellt, führt Azure Storage die Verschlüsselung und Entschlüsselung beim Lesen und Schreiben von Blobdaten transparent aus. Ein SHA-256-Hash des Verschlüsselungsschlüssels wird zusammen mit dem Inhalt eines Blobs geschrieben und so überprüft, ob alle nachfolgenden Vorgänge für das Blob denselben Verschlüsselungsschlüssel verwenden. Azure Storage speichert oder verwaltet nicht den Verschlüsselungsschlüssel, den der Client mit der Anforderung sendet. Nach Abschluss des Verschlüsselungs- oder Entschlüsselungsprozesses wird der Schlüssel sicher verworfen.

Wenn ein Client ein Blob erstellt oder aktualisiert und dazu einen vom Kunden verwalteten Schlüssel verwendet, müssen nachfolgende Lese- und Schreibanforderungen für dieses Blob den Schlüssel ebenfalls bereitstellen. Wenn der Schlüssel bei einer Anforderung für ein Blob, das mit einem vom Kunden bereitgestellten Schlüssel bereits verschlüsselt wurde, nicht bereitgestellt wird, schlägt die Anforderung mit dem Fehlercode 409 (Konflikt) fehl.

Wenn die Clientanwendung bei der Anforderung einen Verschlüsselungsschlüssel sendet und das Speicherkonto mit einem von Microsoft oder vom Kunden verwalteten Schlüssel ebenfalls verschlüsselt wurde, verwendet Azure Storage den bei der Anforderung bereitgestellten Schlüssel zur Verschlüsselung und Entschlüsselung.

Zum Senden des Verschlüsselungsschlüssels als Teil der Anforderung muss ein Client über HTTPS eine sichere Verbindung zu Azure Storage herstellen.

Jede Blobmomentaufnahme kann einen eigenen Verschlüsselungsschlüssel haben.

### <a name="request-headers-for-specifying-customer-provided-keys"></a>Anforderungsheader zur Angabe von vom Kunden bereitgestellten Schlüsseln

Für REST-Aufrufe können Clients mithilfe der folgenden Header Informationen zum Verschlüsselungsschlüssel bei einer Anforderung an den Blobspeicher sicher übergeben:

|Anforderungsheader | BESCHREIBUNG |
|---------------|-------------|
|`x-ms-encryption-key` |Erforderlich bei Schreib- und Leseanforderungen. Ein Base64-codierter AES-256-Verschlüsselungsschlüsselwert. |
|`x-ms-encryption-key-sha256`| Erforderlich bei Schreib- und Leseanforderungen. Der Base64-codierte SHA256 des Verschlüsselungsschlüssels. |
|`x-ms-encryption-algorithm` | Erforderlich bei Schreibanforderungen, optional bei Leseanforderungen. Gibt den Algorithmus an, der beim Verschlüsseln von Daten mit dem angegebenen Schlüssel verwendet werden soll. Muss AES256 sein. |

Die Angabe von Verschlüsselungsschlüsseln bei der Anforderung ist optional. Wenn Sie aber einen der oben aufgeführten Header für einen Schreibvorgang angeben, müssen Sie alle Schlüssel angeben.

### <a name="blob-storage-operations-supporting-customer-provided-keys"></a>Blob-Speichervorgänge, die vom Kunden bereitgestellte Schlüssel unterstützen

Die folgenden Blob-Speichervorgänge unterstützen das Senden von vom Kunden bereitgestellten Verschlüsselungsschlüsseln bei einer Anforderung:

- [Put Blob](/rest/api/storageservices/put-blob)
- [Put Block List](/rest/api/storageservices/put-block-list)
- [Put Block](/rest/api/storageservices/put-block)
- [Put Block from URL](/rest/api/storageservices/put-block-from-url)
- [Put Page](/rest/api/storageservices/put-page)
- [Put Page from URL](/rest/api/storageservices/put-page-from-url)
- [Append Block](/rest/api/storageservices/append-block)
- [Set Blob Properties](/rest/api/storageservices/set-blob-properties)
- [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata)
- [Get Blob](/rest/api/storageservices/get-blob)
- [Get Blob Properties](/rest/api/storageservices/get-blob-properties)
- [Get Blob Metadata](/rest/api/storageservices/get-blob-metadata)
- [Snapshot Blob](/rest/api/storageservices/snapshot-blob)

### <a name="rotate-customer-provided-keys"></a>Rotieren der vom Kunden bereitgestellten Schlüssel

Wenn Sie einen bei der Anforderung übergebenen Verschlüsselungsschlüssel rotieren möchten, laden Sie das Blob herunter und anschließend mit dem neuen Verschlüsselungsschlüssel wieder hoch.

> [!IMPORTANT]
> Das Azure-Portal kann nicht zum Lesen aus oder Schreiben in einen Container oder ein Blob verwendet werden, der bzw. das mit einem Schlüssel verschlüsselt wird, der bei der Anforderung bereitgestellt wurde.
>
> Schützen Sie unbedingt den Verschlüsselungsschlüssel, den Sie bei einer Anforderung in einem Blobspeicher bereitstellen, in einem sicheren Schlüsselspeicher wie Azure Key Vault. Wenn Sie versuchen, einen Schreibvorgang für einen Container oder ein Blob ohne den Verschlüsselungsschlüssel auszuführen, schlägt der Vorgang fehl, und Sie verlieren Ihren Zugriff auf das Objekt.

### <a name="example-use-a-customer-provided-key-to-upload-a-blob-in-net"></a>Beispiel: Verwenden eines vom Kunden bereitgestellten Schlüssels zum Hochladen eines Blobs in .NET

Im folgenden Beispiel wird ein vom Kunden bereitgestellter Schlüssel erstellt und zum Hochladen eines Blobs verwendet. Der Code lädt einen Block hoch und führt dann einen Commit für die Blockliste aus, um das Blob in Azure Storage zu schreiben. Der Schlüssel wird im Objekt [BlobRequestOptions](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions) durch Festlegen der Eigenschaft [CustomerProvidedKey](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.customerprovidedkey) bereitgestellt.

Der Schlüssel wird mit der Klasse [AesCryptoServiceProvider](/dotnet/api/system.security.cryptography.aescryptoserviceprovider) erstellt. Wenn Sie eine Instanz dieser Klasse in Ihrem Code erstellen möchten, fügen Sie eine `using`-Anweisung hinzu, die auf den Namespace `System.Security.Cryptography` verweist:

```csharp
public static void UploadBlobWithClientKey(CloudBlobContainer container)
{
    // Create a new key using the Advanced Encryption Standard (AES) algorithm.
    AesCryptoServiceProvider keyAes = new AesCryptoServiceProvider();

    // Specify the key as an option on the request.
    BlobCustomerProvidedKey customerProvidedKey = new BlobCustomerProvidedKey(keyAes.Key);
    var options = new BlobRequestOptions
    {
        CustomerProvidedKey = customerProvidedKey
    };

    string blobName = "sample-blob-" + Guid.NewGuid();
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(blobName);

    try
    {
        // Create an array of random bytes.
        byte[] buffer = new byte[1024];
        Random rnd = new Random();
        rnd.NextBytes(buffer);

        using (MemoryStream sourceStream = new MemoryStream(buffer))
        {
            // Write the array of random bytes to a block.
            int blockNumber = 1;
            string blockId = Convert.ToBase64String(Encoding.ASCII.GetBytes(string.Format("BlockId{0}",
                blockNumber.ToString("0000000"))));

            // Write the block to Azure Storage.
            blockBlob.PutBlock(blockId, sourceStream, null, null, options, null);

            // Commit the block list to write the blob.
            blockBlob.PutBlockList(new List<string>() { blockId }, null, options, null);
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="azure-storage-encryption-versus-disk-encryption"></a>Vergleich: Azure Storage-Verschlüsselung und Datenträgerverschlüsselung

Mit der Azure Storage-Verschlüsselung werden alle Azure-Speicherkonten und darin enthaltene Ressourcen verschlüsselt, z. B. Seitenblobs, auf denen Datenträger auf Azure-VMs basieren. Darüber hinaus können Datenträger von Azure-VMs mit [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md) verschlüsselt werden. Für Azure Disk Encryption wird die branchenübliche [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview)-Verschlüsselung unter Windows und [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) unter Linux verwendet, um betriebssystembasierte Verschlüsselungslösungen bereitzustellen, die mit dem Azure Key Vault integriert werden können.

## <a name="next-steps"></a>Nächste Schritte

- [Was ist der Azure-Schlüsseltresor?](../../key-vault/key-vault-overview.md)
- [Configure customer-managed keys for Azure Storage encryption from the Azure portal (Konfigurieren von Kunden verwalteter Schlüssel für die Azure Storage-Verschlüsselung mithilfe des Azure-Portals)](storage-encryption-keys-portal.md)
- [Configure customer-managed keys for Azure Storage encryption from PowerShell (Konfigurieren von Kunden verwalteter Schlüssel für die Azure Storage-Verschlüsselung mithilfe von Azure PowerShell)](storage-encryption-keys-powershell.md)
- [Konfigurieren von von Kunden verwalteten Schlüsseln für die Azure Storage-Verschlüsselung mithilfe der Azure CLI](storage-encryption-keys-cli.md)
