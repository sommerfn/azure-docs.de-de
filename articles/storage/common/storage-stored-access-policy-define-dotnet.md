---
title: Definieren einer gespeicherten Zugriffsrichtlinie mit .NET – Azure Storage
description: Erfahren Sie, wie Sie eine gespeicherte Zugriffsrichtlinie mithilfe der .NET-Clientbibliothek definieren.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/06/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 272d676d0a5a55262b1c68d0bae9a9ab229df72c
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990606"
---
# <a name="define-a-stored-access-policy-with-net"></a>Definieren einer gespeicherten Zugriffsrichtlinie mit .NET

Eine gespeicherte Zugriffsrichtlinie bietet eine zusätzliche Steuerungsebene für Shared Access Signatures (SAS) der Dienstebene auf Serverseite. Das Definieren einer gespeicherten Zugriffsrichtlinie dient zum Gruppieren von Shared Access Signatures und zum Bereitstellen zusätzlicher Einschränkungen für Shared Access Signatures, die von der Richtlinie gebunden werden. Mithilfe der gespeicherten Zugriffsrichtlinie können Sie die Startzeit, die Ablaufzeit oder die Berechtigungen für eine Shared Access Signature ändern oder die Signatur widerrufen, nachdem sie ausgegeben wurde.
  
 Die folgenden Speicherressourcen unterstützen gespeicherte Zugriffsrichtlinien:  
  
- Blobcontainer  
- Dateifreigaben  
- Warteschlangen  
- Tabellen  
  
> [!NOTE]
> Eine gespeicherte Zugriffsrichtlinie für einen Container kann einer Shared Access Signature zugeordnet werden, die dem Container selbst oder den darin enthaltenen Blobs Berechtigungen erteilt. Entsprechend kann eine gespeicherte Zugriffsrichtlinie für eine Dateifreigabe einer Shared Access Signature zugeordnet werden, die der Freigabe selbst oder den darin enthaltenen Dateien Berechtigungen erteilt.  
>
> Gespeicherte Zugriffsrichtlinien werden nur für Dienst-SAS unterstützt. Gespeicherte Zugriffsrichtlinien werden für die Konto-SAS oder die SAS für die Benutzerdelegierung nicht unterstützt.  

## <a name="create-a-stored-access-policy"></a>Erstellen einer gespeicherten Zugriffsrichtlinie

Mit dem folgenden Code wird eine gespeicherte Zugriffsrichtlinie für einen Container erstellt. Sie können mit der Zugriffsrichtlinie Einschränkungen für eine Dienst-SAS für den Container oder seine Blobs festlegen.

```csharp
private static async Task CreateStoredAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new stored access policy and define its constraints.
    // The access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When the start time for the SAS is omitted, the start time is assumed to be the time when Azure Storage receives the request.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get the container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

## <a name="see-also"></a>Weitere Informationen

- [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](storage-sas-overview.md)
- [Definieren einer gespeicherten Zugriffsrichtlinie](/rest/api/storageservices/define-stored-access-policy)

