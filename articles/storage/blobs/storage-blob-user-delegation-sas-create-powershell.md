---
title: Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit PowerShell (Vorschau) – Azure Storage
description: Erfahren Sie, wie Sie eine SAS (Shared Access Signature) mithilfe von Azure Active Directory-Anmeldeinformationen in Azure Storage mit PowerShell erstellen können.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: 0164c97adf720a618179908298223c54bf48824e
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2019
ms.locfileid: "71673336"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-powershell-preview"></a>Erstellen einer SAS für die Benutzerdelegierung für einen Container oder ein Blob mit PowerShell (Vorschau)

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

In diesem Artikel wird beschrieben, wie Sie Azure Active Directory-Anmeldeinformationen (Azure AD) verwenden, um eine SAS für die Benutzerdelegierung für einen Container oder ein Blob mit PowerShell (Vorschau) zu erstellen.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="install-the-preview-module"></a>Installieren des Vorschaumoduls

Wenn Sie PowerShell verwenden möchten, um eine SAS für die Benutzerdelegierung zu erstellen, müssen Sie zunächst das Az.Storage 1.3.1-preview installieren. Führen Sie dazu folgende Schritte aus:

1. Deinstallieren Sie alle älteren Installationen von Azure PowerShell:

    - Entfernen Sie alle früheren Installationen von Azure PowerShell mit der Einstellung **Apps & Features** (unter **Einstellungen**) aus Windows.
    - Entfernen Sie alle **Azure**-Module aus `%Program Files%\WindowsPowerShell\Modules`.

1. Vergewissern Sie sich, dass die aktuelle Version von PowerShellGet installiert ist. Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie den folgenden Befehl aus, um die neueste Version zu installieren:

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

1. Schließen Sie nach dem Installieren von PowerShellGet das PowerShell-Fenster, und öffnen Sie es dann erneut.

1. Installieren Sie die neueste Version von Azure PowerShell:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Installieren ein Azure Storage-Vorschaumodul, das die SAS für die Benutzerdelegierung unterstützt:

    ```powershell
    Install-Module Az.Storage `
        –Repository PSGallery `
        -RequiredVersion 1.3.1-preview `
        –AllowPrerelease `
        –AllowClobber `
        –Force
    ```

1. Schließen Sie das PowerShell-Fenster, und öffnen Sie es dann erneut.

Da PowerShell standardmäßig das neueste Az.Storage-Modul lädt, müssen Sie möglicherweise das 1.3.1-Vorschaumodul explizit laden, wenn Sie die Konsole starten. Führen Sie den Befehl [Import-Module](/powershell/module/microsoft.powershell.core/import-module) aus, um das Vorschaumodul explizit zu laden:

```powershell
Import-Module Az.Storage -RequiredVersion 1.3.1
```

Weitere Informationen zum Installieren von Azure PowerShell finden Sie unter [Installieren von Azure PowerShell mit PowerShellGet](/powershell/azure/install-az-ps).

## <a name="sign-in-to-azure-powershell-with-azure-ad"></a>Anmelden bei Azure PowerShell mit Azure AD

Geben Sie den Befehl [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) aus, um sich mit Ihrem Azure AD-Konto anzumelden:

```powershell
Connect-AzAccount
```

Weitere Informationen zum Anmelden mit PowerShell finden Sie unter [Anmelden mit Azure PowerShell](/powershell/azure/authenticate-azureps).

## <a name="assign-permissions-with-rbac"></a>Zuweisen von Berechtigungen mit RBAC

Um eine SAS für die Benutzerdelegierung aus Azure PowerShell zu erstellen, muss dem Azure AD-Konto, mit dem die Anmeldung bei PowerShell erfolgt, eine Rolle zugewiesen werden, die die Aktion **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** beinhaltet. Diese Berechtigung ermöglicht es diesem Azure AD Konto, den *Benutzerdelegierungsschlüssel* anzufordern. Der Benutzerdelegierungsschlüssel wird zum Signieren der SAS für die Benutzerdelegierung verwendet. Die Rolle, die die Aktion **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** bereitstellt, muss auf der Ebene des Speicherkontos, der Ressourcengruppe oder des Abonnements zugewiesen werden. Weitere Informationen zu RBAC-Berechtigungen zum Erstellen einer SAS für die Benutzerdelegierung finden Sie im Abschnitt **Zuweisen von Berechtigungen mit RBAC** unter [Erstellen einer SAS für die Benutzerdelegierung](/rest/api/storageservices/create-user-delegation-sas).

Wenn Sie nicht über ausreichende Berechtigungen zum Zuweisen von RBAC-Rollen zu einem Azure AD-Sicherheitsprinzipal verfügen, müssen Sie möglicherweise den Kontobesitzer oder den Administrator bitten, die erforderlichen Berechtigungen zuzuweisen.

Im folgenden Beispiel wird die Rolle **Storage Blob Data Contributor** (Speicherblob-Datenmitwirkender) zugewiesen, die die Aktion **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** beinhaltet. Die Rolle wird auf der Ebene des Speicherkontos festgelegt.

Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

Weitere Informationen zu den integrierten Rollen, die die Aktion **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** enthalten, finden Sie unter [Integrierte Rollen für Azure-Ressourcen](../../role-based-access-control/built-in-roles.md).

## <a name="use-azure-ad-credentials-to-secure-a-sas"></a>Verwenden von Azure AD-Anmeldeinformationen zum Sichern einer SAS

Wenn Sie eine SAS für die Benutzerdelegierung mit Azure PowerShell erstellen, wird der Benutzerdelegierungsschlüssel, der zum Signieren der SAS verwendet wird, implizit für Sie erstellt. Die Startzeit und die Ablaufzeit, die Sie für die SAS angeben, werden auch als Startzeit und Ablaufzeit für den Benutzerdelegierungsschlüssel verwendet. 

Da das maximale Intervall, in dem der Benutzerdelegierungsschlüssel gültig ist, 7 Tage ab dem Startdatum beträgt, sollten Sie eine Ablaufzeit für die SAS angeben, die innerhalb von 7 Tagen der Startzeit liegt. Die SAS ist ungültig, nachdem der Benutzerdelegierungsschlüssel abgelaufen ist, sodass eine SAS mit einer Ablaufzeit von mehr als 7 Tagen trotzdem nur 7 Tage gültig ist.

Um eine SAS für die Benutzerdelegierung für einen Container oder ein Blob mit Azure PowerShell zu erstellen, erstellen Sie zunächst ein neues Azure Storage-Kontextobjekt und geben dabei den Parameter `-UseConnectedAccount` an. Der `-UseConnectedAccount`-Parameter gibt an, dass der Befehl das Kontextobjekt unter dem Azure AD-Konto erstellt, mit dem Sie sich angemeldet haben.

Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```powershell
$ctx = New-AzStorageContext -StorageAccountName <storage-account> -UseConnectedAccount
```

### <a name="create-a-user-delegation-sas-for-a-container"></a>Erstellen einer SAS für die Benutzerdelegierung für einen Container

Um ein SAS-Token für die Benutzerdelegierung für einen Container zurückzugeben, geben Sie den Befehl [New-AzStorageContainerSASToken](/powershell/module/az.storage/new-azstoragecontainersastoken) aus, und übergeben Sie dabei das Azure Storage-Kontextobjekt, das Sie zuvor erstellt haben.

Im folgenden Beispiel wird eine SAS für die Benutzerdelegierung für einen Container zurückgegeben. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```powershell
New-AzStorageContainerSASToken -Context $ctx `
    -Name <container> `
    -Permission racwdl `
    -ExpiryTime <date-time>
```

Das zurückgegebene SAS-Token für die Benutzerdelegierung ähnelt dem folgenden Token:

```
?sv=2018-11-09&sr=c&sig=<sig>&skoid=<skoid>&sktid=<sktid>&skt=2019-08-05T22%3A24%3A36Z&ske=2019-08-07T07%3A
00%3A00Z&sks=b&skv=2018-11-09&se=2019-08-07T07%3A00%3A00Z&sp=rwdl
```

### <a name="create-a-user-delegation-sas-for-a-blob"></a>Erstellen einer SAS für die Benutzerdelegierung für ein Blob

Um ein SAS-Token für die Benutzerdelegierung für ein Blob zurückzugeben, geben Sie den Befehl [New-AzStorageBlobSASToken](/powershell/module/az.storage/new-azstorageblobsastoken) aus, und übergeben Sie dabei das Azure Storage-Kontextobjekt, das Sie zuvor erstellt haben.

Die folgende Syntax gibt eine SAS für die Benutzerdelegierung für ein Blob zurück. Im Beispiel wird der `-FullUri`-Parameter angegeben, der den Blob-URI mit angefügtem SAS-Token zurückgibt. Denken Sie daran, die Platzhalterwerte in Klammern durch Ihre eigenen Werte zu ersetzen:

```powershell
New-AzStorageBlobSASToken -Context $ctx `
    -Container <container> `
    -Blob <blob> `
    -Permission racwd `
    -ExpiryTime <date-time>
    -FullUri
```

Der zurückgegebene SAS-URI für die Benutzerdelegierung ähnelt dem folgenden URI:

```
https://storagesamples.blob.core.windows.net/sample-container/blob1.txt?sv=2018-11-09&sr=b&sig=<sig>&skoid=<skoid>&sktid=<sktid>&skt=2019-08-06T21%3A16%3A54Z&ske=2019-08-07T07%3A00%3A00Z&sks=b&skv=2018-11-09&se=2019-08-07T07%3A00%3A00Z&sp=racwd
```

> [!NOTE]
> Eine SAS für die Benutzerdelegierung unterstützt nicht das Definieren von Berechtigungen mit einer gespeicherten Zugriffsrichtlinie.

## <a name="revoke-a-user-delegation-sas"></a>Widerrufen einer SAS für die Benutzerdelegierung

Um eine SAS für die Benutzerdelegierung aus Azure PowerShell zu widerrufen, rufen Sie den Befehl **Revoke-AzStorageAccountUserDelegationKeys** auf. Mit diesem Befehl werden alle Benutzerdelegierungsschlüssel widerrufen, die dem angegebenen Speicherkonto zugeordnet sind. Alle mit diesen Schlüsseln verknüpften Shared Access Signatures werden für ungültig erklärt.

Denken Sie daran, die Platzhalterwerte in eckigen Klammern durch Ihre eigenen Werte zu ersetzen:

```powershell
Revoke-AzStorageAccountUserDelegationKeys -ResourceGroupName <resource-group> `
    -StorageAccountName <storage-account>
```

> [!IMPORTANT]
> Sowohl der Benutzerdelegierungsschlüssel als auch die RBAC-Rollenzuweisungen werden von Azure Storage zwischengespeichert. Daher kann es zu einer Verzögerung zwischen der Initiierung des Sperrprozesses und dem Zeitpunkt kommen, zu dem eine SAS für die Benutzerdelegierung ungültig wird.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen einer SAS für die Benutzerdelegierung (REST-API)](/rest/api/storageservices/create-user-delegation-sas)
- [Vorgang zum Abrufen eines Benutzerdelegierungsschlüssels](/rest/api/storageservices/get-user-delegation-key)
