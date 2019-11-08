---
title: Autorisieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen – Azure Storage
description: Azure Blob Storage und Azure Queue Storage unterstützen das Autorisieren des Zugriffs auf Ressourcen mit Azure Active Directory und verwaltete Identitäten für Azure-Ressourcen. Sie können verwaltete Identitäten für Azure-Ressourcen verwenden, um den Zugriff auf Blobs und Warteschlangen über Anwendungen zu autorisieren, die auf virtuellen Azure-Computern, in Funktions-Apps, in VM-Skalierungsgruppen o. Ä. ausgeführt werden.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 10/17/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: d77ab142e227cfaa6533395cc256d992e698dd17
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73495924"
---
# <a name="authorize-access-to-blobs-and-queues-with-azure-active-directory-and-managed-identities-for-azure-resources"></a>Autorisieren des Zugriffs auf Blobs und Warteschlangen mit Azure Active Directory und verwalteten Identitäten für Azure-Ressourcen

Azure Blob Storage und Azure Queue Storage unterstützen die Azure Active Directory-Authentifizierung (Azure AD-Authentifizierung) mit [verwalteten Identitäten für Azure-Ressourcen](../../active-directory/managed-identities-azure-resources/overview.md). Sie können verwaltete Identitäten für Azure-Ressourcen verwenden, um den Zugriff auf Blob- und Warteschlangendaten mithilfe von Azure AD-Anmeldeinformationen über Anwendungen zu autorisieren, die auf virtuellen Azure-Computern, in Funktions-Apps, in VM-Skalierungsgruppen und anderen Diensten ausgeführt werden. Durch Verwendung von verwalteten Identitäten für Azure-Ressourcen zusammen mit der Azure AD-Authentifizierung können Sie vermeiden, dass Anmeldeinformationen mit den in der Cloud ausgeführten Anwendungen gespeichert werden.  

In diesem Artikel wird die Autorisierung des Zugriffs auf Blob- oder Warteschlangendaten über einen virtuellen Azure-Computer unter Verwendung verwalteter Identitäten erläutert. Darüber hinaus erfahren Sie, wie Sie Ihren Code in der Entwicklungsumgebung testen.

## <a name="enable-managed-identities-on-a-vm"></a>Aktivieren von verwalteten Identitäten auf einem virtuellen Computer

Damit Sie verwaltete Identitäten für Azure-Ressourcen zum Autorisieren des Zugriffs auf Blobs und Warteschlangen über Ihren virtuellen Computer verwenden können, müssen Sie zunächst verwaltete Identitäten für Ressourcen auf dem virtuellen Computer aktivieren. Informationen zum Aktivieren von verwalteten Identitäten für Azure-Ressourcen finden Sie in diesen Artikeln:

- [Azure-Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-Vorlage](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager-Clientbibliotheken](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

Weitere Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="authenticate-with-the-azure-identity-library-preview"></a>Authentifizieren mit der Azure Identity-Bibliothek (Vorschauversion)

Die Azure Identity-Clientbibliothek für .NET (Vorschauversion) dient zur Authentifizierung eines Sicherheitsprinzipals. Wenn Ihr Code in Azure ausgeführt wird, ist der Sicherheitsprinzipal eine verwaltete Identität für Azure-Ressourcen.

Wenn Ihr Code in der Entwicklungsumgebung ausgeführt wird, erfolgt die Authentifizierung ggf. automatisch. Abhängig von den verwendeten Tools kann aber auch eine Browseranmeldung erforderlich sein. Microsoft Visual Studio unterstützt einmaliges Anmelden (Single Sign-On, SSO), sodass automatisch das aktive Azure AD-Benutzerkonto für die Authentifizierung verwendet wird. Weitere Informationen zu SSO finden Sie unter [Einmaliges Anmelden bei Anwendungen in Azure Active Directory](../../active-directory/manage-apps/what-is-single-sign-on.md).

Bei anderen Entwicklungstools ist ggf. eine Anmeldung über einen Webbrowser erforderlich. Für die Authentifizierung über die Entwicklungsumgebung kann auch ein Dienstprinzipal verwendet werden. Weitere Informationen finden Sie unter [Gewusst wie: Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff über das Portal](../../active-directory/develop/howto-create-service-principal-portal.md).

Nach der Authentifizierung erhält die Azure Identity-Clientbibliothek Tokenanmeldeinformationen. Diese Tokenanmeldeinformationen werden im Dienstclientobjekt gekapselt, das Sie erstellen, um Vorgänge für Azure Storage auszuführen. Die Bibliothek führt diesen Schritt automatisch für Sie durch und ruft die entsprechenden Tokenanmeldeinformationen ab.

Weitere Informationen zur Azure Identity-Clientbibliothek finden Sie unter [Azure Identity-Clientbibliothek für .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity).

## <a name="assign-rbac-roles-for-access-to-data"></a>Zuweisen von RBAC-Rollen für den Datenzugriff

Wenn ein Azure AD-Sicherheitsprinzipal versucht, auf Blob- oder Warteschlangendaten zuzugreifen, muss dieser Sicherheitsprinzipal über Berechtigungen für die Ressource verfügen. Dem Sicherheitsprinzipal muss eine RBAC-Rolle zugewiesen werden, die den Zugriff auf Blob- oder Warteschlangendaten in Azure Storage ermöglicht. Dabei spielt es keine Rolle, ob es sich bei dem Sicherheitsprinzipal um eine verwaltete Identität in Azure oder um ein Azure AD-Benutzerkonto handelt, mit dem Code in der Entwicklungsumgebung ausgeführt wird. Informationen zur Zuweisung von Berechtigungen per RBAC finden Sie unter **Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory** im Abschnitt [Zuweisen von RBAC-Rollen für Zugriffsrechte](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights).

## <a name="install-the-preview-packages"></a>Installieren der Vorschaupakete

In den Beispielen in diesem Artikel wird die neueste Vorschauversion der Azure Storage-Clientbibliothek für Blob Storage verwendet. Führen Sie zum Installieren des Vorschaupakets den folgenden Befehl an der NuGet-Paket-Manager-Konsole aus:

```powershell
Install-Package Azure.Storage.Blobs -IncludePrerelease
```

In den Beispielen in diesem Artikel wird auch die neueste Vorschauversion der [Azure Identity-Clientbibliothek für .NET](https://www.nuget.org/packages/Azure.Identity/) zur Authentifizierung mit Azure AD-Anmeldeinformationen verwendet. Führen Sie zum Installieren des Vorschaupakets den folgenden Befehl an der NuGet-Paket-Manager-Konsole aus:

```powershell
Install-Package Azure.Identity -IncludePrerelease
```

## <a name="net-code-example-create-a-block-blob"></a>Codebeispiel für .NET: Erstellen eines Blockblobs

Fügen Sie dem Code die folgenden `using`-Anweisungen hinzu, um die Vorschauversionen der Azure Identity- und Azure Storage-Clientbibliotheken zu verwenden.

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Azure.Identity;
using Azure.Storage;
using Azure.Storage.Sas;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
```

Erstellen Sie eine Instanz der Klasse [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential), um Tokenanmeldeinformationen abzurufen, die in Ihrem Code zum Autorisieren von Anforderungen für Azure Storage verwendet werden können. Das folgende Codebeispiel zeigt, wie Sie die authentifizierten Tokenanmeldeinformationen abrufen, mit ihnen ein Dienstclientobjekt erstellen und anschließend unter Verwendung des Dienstclients ein neues Blob hochladen:

```csharp
async static Task CreateBlockBlobAsync(string accountName, string containerName, string blobName)
{
    // Construct the blob container endpoint from the arguments.
    string containerEndpoint = string.Format("https://{0}.blob.core.windows.net/{1}",
                                                accountName,
                                                containerName);

    // Get a credential and create a client object for the blob container.
    BlobContainerClient containerClient = new BlobContainerClient(new Uri(containerEndpoint),
                                                                    new DefaultAzureCredential());

    try
    {
        // Create the container if it does not exist.
        await containerClient.CreateIfNotExistsAsync();

        // Upload text to a new block blob.
        string blobContents = "This is a block blob.";
        byte[] byteArray = Encoding.ASCII.GetBytes(blobContents);

        using (MemoryStream stream = new MemoryStream(byteArray))
        {
            await containerClient.UploadBlobAsync(blobName, stream);
        }
    }
    catch (StorageRequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

> [!NOTE]
> Zum Autorisieren von Anforderungen für Blob- oder Warteschlangendaten mit Azure AD müssen Sie für die Anforderungen HTTPS verwenden.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu RBAC-Rollen für Azure Storage finden Sie unter [Verwalten der Zugriffsrechte für Speicherdaten mit RBAC](storage-auth-aad-rbac.md).
- Informationen zum Autorisieren des Zugriffs auf Container und Warteschlangen aus Ihren Speicheranwendungen finden Sie unter [Verwenden von Azure AD mit Speicheranwendungen](storage-auth-aad-app.md).
- Informationen zum Ausführen von Azure CLI- und PowerShell-Befehlen mit Azure AD-Anmeldeinformationen finden Sie unter [Ausführen von Azure CLI- oder PowerShell-Befehlen mit Azure AD-Anmeldeinformationen für den Zugriff auf Blob- oder Warteschlangendaten](storage-auth-aad-script.md).
