---
title: Verwenden der systemseitig zugewiesenen verwalteten Identität eines virtuellen Windows-Computers für den Zugriff auf Azure Storage
description: In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität eines virtuellen Windows-Computers verwenden, um auf Azure Storage zuzugreifen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/24/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 147ee2450a6a67f8ca02149105533401d038a53a
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191079"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-storage-via-access-key"></a>Tutorial: Verwenden einer systemseitig zugewiesenen verwalteten Identität eines virtuellen Windows-Computers für den Zugriff auf Azure Storage über Zugriffsschlüssel

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


> [!IMPORTANT] 
> Azure Storage unterstützt jetzt die Azure AD-Authentifizierung. Verwenden Sie als bewährte Methode die [Azure AD-Authentifizierung](tutorial-vm-windows-access-storage.md) anstelle von Zugriffsschlüsseln. 


In diesem Tutorial erfahren Sie, wie Sie eine systemseitig zugewiesene verwaltete Identität für einen virtuellen Windows-Computer verwenden, um Speicherkonto-Zugriffsschlüssel abzurufen. Sie können Speicherzugriffsschlüssel wie gewohnt bei Speichervorgängen verwenden, z.B. bei der Verwendung des Storage SDK. Für dieses Tutorial werden Blobs mithilfe von Azure Storage PowerShell hoch- und heruntergeladen. Sie lernen Folgendes:


> [!div class="checklist"]
> * Speicherkonto erstellen
> * Gewähren des Zugriffs auf Speicherkonto-Zugriffsschlüssel für Ihren virtuellen Computer in Resource Manager 
> * Abrufen eines Zugriffstokens mithilfe der Identität Ihres virtuellen Computers und Verwenden dieses Zugriffstokens zum Abrufen von Speicherzugriffsschlüsseln aus Resource Manager 

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="create-a-storage-account"></a>Speicherkonto erstellen 

Erstellen Sie ein Speicherkonto, sofern Sie über keines verfügen. Sie können diesen Schritt auch überspringen und der systemseitig zugewiesenen verwalteten Identität Ihres virtuellen Computers Zugriff auf die Schlüssel eines vorhandenen Speicherkontos gewähren. 

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **+/Neuen Dienst erstellen**.
2. Klicken Sie auf **Speicher**, dann auf **Speicherkonto**, und anschließend wird ein neuer Bereich namens „Speicherkonto erstellen“ angezeigt.
3. Geben Sie einen Namen für das Speicherkonto ein, das Sie später verwenden werden.  
4. **Bereitstellungsmodell** und **Kontoart** sollten jeweils auf „Resource Manager“ und „Allgemein“ festgelegt werden. 
5. Stellen Sie sicher, dass **Abonnement** und **Ressourcengruppe** dem entsprechen, was Sie bei der Erstellung Ihrer VM im vorherigen Schritt angegeben haben.
6. Klicken Sie auf **Create**.

    ![Erstellen eines neuen Speicherkontos](./media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Erstellen eines Blob-Containers im Speicherkonto

Später werden wir eine Datei in das neue Speicherkonto hoch- und daraus herunterladen. Da Dateien Blob-Speicher erfordern, müssen wir einen Blob-Container erstellen, in dem die Datei gespeichert wird.

1. Navigieren Sie zurück zum neu erstellten Speicherkonto.
2. Klicken Sie auf der linken Seite unter „Blob-Dienst“ auf den Link **Container**.
3. Klicken Sie oben auf der Seite auf **+ Container**, und ein Bereich namens „Neuer Container“ wird geöffnet.
4. Geben Sie dem Container einen Namen, wählen Sie eine Zugriffsebene aus, und klicken Sie dann auf **OK**. Der angegebene Name wird später in diesem Tutorial verwendet. 

    ![Erstellen eines Speichercontainers](./media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-system-assigned-managed-identity-access-to-use-storage-account-access-keys"></a>Gewähren des Zugriffs für die systemseitig zugewiesene verwaltete Identität Ihres virtuellen Computers zur Verwendung von Speicherkonto-Zugriffsschlüsseln 

Azure Storage unterstützt die Azure AD-Authentifizierung nicht nativ.  Sie können die systemseitig zugewiesene verwaltete Identität Ihres virtuellen Computers jedoch zum Abrufen von Speicherkonto-Zugriffsschlüsseln aus Resource Manager verwenden und dann mithilfe eines Schlüssels auf den Speicher zugreifen.  In diesem Schritt gewähren Sie der systemseitig zugewiesenen verwalteten Identität Ihres virtuellen Computers Zugriff auf die Schlüssel des Speicherkontos.   

1. Navigieren Sie zurück zum neu erstellten Speicherkonto.  
2. Klicken Sie im linken Bereich auf den Link **Zugriffssteuerung (IAM)**.  
3. Klicken Sie oben auf der Seite auf **+ Rollenzuweisung hinzufügen**, um dem virtuellen Computer eine neue Rollenzuweisung hinzuzufügen.
4. Legen Sie **Rolle** rechts auf der Seite auf „Dienstrolle Speicherkonto-Schlüsseloperator“ fest. 
5. Legen Sie in der nächsten Dropdownliste **Zugriff zuweisen zu** auf die Ressource „Virtueller Computer“ fest.  
6. Stellen Sie im nächsten Schritt sicher, dass das richtige Abonnement in der Dropdownliste **Abonnement** aufgeführt wird, und legen Sie dann **Ressourcengruppe** auf „Alle Ressourcengruppen“ fest.  
7. Wählen Sie schließlich unter **Auswählen** in der Dropdownliste Ihren virtuellen Windows-Computer aus, und klicken Sie dann auf **Speichern**. 

    ![Alternativer Bildtext](./media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-system-assigned-managed-identity-to-call-azure-resource-manager"></a>Abrufen eines Zugriffstokens mithilfe der systemseitig zugewiesenen verwalteten Identität des virtuellen Computers zum Aufrufen von Azure Resource Manager 

Für den Rest des Tutorials arbeiten wir von dem virtuellen Computer aus, den wir zuvor erstellt haben. 

In diesem Abschnitt müssen Sie PowerShell-Cmdlets von Azure Resource Manager verwenden.  Wenn Sie das Programm nicht installiert haben, [laden Sie die neuesten Version herunter](https://docs.microsoft.com/powershell/azure/overview), bevor Sie fortfahren.

1. Navigieren Sie im Azure-Portal zu **Virtuelle Computer**, wechseln Sie zu Ihrem virtuellen Windows-Computer, und klicken Sie dann oben auf der Seite **Übersicht** auf **Verbinden**. 
2. Geben Sie Ihren **Benutzernamen** und Ihr **Kennwort** ein, das Sie beim Erstellen des virtuellen Windows-Computers hinzugefügt haben. 
3. Sie haben nun eine **Remotedesktopverbindung** mit dem virtuellen Computer erstellt. Öffnen Sie jetzt PowerShell in der Remotesitzung.
4. Erstellen Sie mithilfe des PowerShell-Befehls „Invoke-WebRequest“ eine Anforderung an den lokalen Endpunkt für die verwaltete Identität für Azure-Ressourcen, um ein Zugriffstoken für Azure Resource Manager zu erhalten.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > Der Wert des „resource“-Parameters muss exakt mit dem von Azure AD erwarteten Wert übereinstimmen. Wenn Sie die Azure Resource Manager-Ressourcen-ID verwenden, müssen Sie den nachgestellten Schrägstrich im URI verwenden.
    
    Extrahieren Sie im nächsten Schritt das Element „Content“, das als JSON-formatierte (JavaScript Object Notation) Zeichenfolge im $response-Objekt gespeichert ist. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Extrahieren Sie dann das Zugriffstoken aus der Antwort.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
 
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Abrufen von Speicherkonto-Zugriffsschlüsseln aus Azure Resource Manager, um Speicheraufrufe auszuführen  

Nun rufen Sie mit PowerShell und dem Zugriffstoken, das im vorherigen Abschnitt abgerufen wurde, Resource Manager auf, um den Speicherzugriffsschlüssel abzurufen. Sobald wir den Speicherzugriffsschlüssel haben, können wir Vorgänge zum Up- und Download von Speicher aufrufen.

```powershell
$keysResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
> [!NOTE] 
> Bei der URL wird die Groß-/Kleinschreibung beachtet. Achten Sie daher darauf, dieselbe Groß- und Kleinschreibung zu verwenden wie zuvor beim Benennen der Ressourcengruppe, einschließlich des Großbuchstaben „G“ in „resourceGroups“. 

```powershell
$keysContent = $keysResponse.Content | ConvertFrom-Json
$key = $keysContent.keys[0].value
```

Als Nächstes erstellen wir eine Datei namens „test.txt“. Führen Sie dann mithilfe des Speicherschlüssels die Authentifizierung mit dem `New-AzStorageContent`-Cmdlet aus, und laden Sie die Datei in unseren Blob-Container hoch und anschließend wieder herunter.

```bash
echo "This is a test text file." > test.txt
```

Installieren Sie unbedingt zunächst die Azure Storage-Cmdlets mit `Install-Module Az.Storage`. Laden Sie dann das Blob, das Sie soeben erstellt haben, mit dem PowerShell-Cmdlet `Set-AzStorageBlobContent` hoch:

```powershell
$ctx = New-AzStorageContext -StorageAccountName <STORAGE-ACCOUNT> -StorageAccountKey $key
Set-AzStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Antwort:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

Sie können das Blob, das Sie soeben erstellt haben, auch mit dem PowerShell-Cmdlet `Get-AzStorageBlobContent` herunterladen:

```powershell
Get-AzStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Antwort:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie eine systemseitig zugewiesene verwaltete Identität für den Zugriff auf Azure Storage unter Verwendung eines Zugriffsschlüssels erstellen.  Weitere Informationen zu Azure Storage-Zugriffsschlüsseln finden Sie hier:

> [!div class="nextstepaction"]
>[Verwalten von Speicherzugriffsschlüsseln](/azure/storage/common/storage-create-storage-account)

