---
title: Authentifizieren einer verwalteten Identität mit Azure Active Directory
description: Dieser Artikel enthält Informationen zur Authentifizierung einer verwalteten Identität mit Azure Active Directory, um auf Azure Event Hubs-Ressourcen zuzugreifen.
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
manager: ''
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: spelluru
ms.openlocfilehash: cbd7de7d526e1954aaad60f7d71e5cdf202f6a29
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "69992458"
---
# <a name="authenticate-a-managed-identity-with-azure-active-directory-to-access-event-hubs-resources"></a>Authentifizieren einer verwalteten Identität mit Azure Active Directory für den Zugriff auf Event Hubs-Ressourcen
Azure Event Hubs unterstützt die Azure AD-Authentifizierung (Azure Active Directory) mit [verwalteten Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md). Sie können verwaltete Identitäten für Azure-Ressourcen verwenden, um den Zugriff auf Event Hubs-Ressourcen mithilfe von Azure AD-Anmeldeinformationen über Anwendungen zu autorisieren, die auf virtuellen Azure-Computern, in Funktions-Apps, in VM-Skalierungsgruppen und anderen Diensten ausgeführt werden. Durch Verwendung von verwalteten Identitäten für Azure-Ressourcen zusammen mit der Azure AD-Authentifizierung können Sie vermeiden, dass Anmeldeinformationen mit den in der Cloud ausgeführten Anwendungen gespeichert werden.

In diesem Artikel wird die Autorisierung des Zugriffs auf einen Event Hub mit einer verwalteten Identität über einen virtuellen Azure-Computer erläutert.

## <a name="enable-managed-identities-on-a-vm"></a>Aktivieren von verwalteten Identitäten auf einem virtuellen Computer
Damit Sie verwaltete Identitäten für Azure-Ressourcen zum Autorisieren des Zugriffs auf Event Hubs-Ressourcen über Ihren virtuellen Computer verwenden können, müssen Sie zunächst verwaltete Identitäten für Azure-Ressourcen auf dem virtuellen Computer aktivieren. Informationen zum Aktivieren von verwalteten Identitäten für Azure-Ressourcen finden Sie in diesen Artikeln:

- [Azure-Portal](../active-directory/managed-service-identity/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-Befehlszeilenschnittstelle](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-Vorlage](../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager-Clientbibliotheken](../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="grant-permissions-to-a-managed-identity-in-azure-ad"></a>Erteilen von Berechtigungen für eine verwaltete Identität in Azure AD
Damit Sie eine Anforderung an den Event Hubs-Dienst über eine verwaltete Identität in Ihrer Anwendung autorisieren können, müssen Sie zuerst die Einstellungen der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) für diese verwaltete Identität konfigurieren. Für Azure Event Hubs werden RBAC-Rollen definiert, die Berechtigungen zum Senden an und Lesen aus Event Hubs umfassen. Wenn die RBAC-Rolle einer verwalteten Identität zugewiesen wird, wird der verwalteten Identität der Zugriff auf Event Hubs-Daten im entsprechenden Bereich erteilt.

Weitere Informationen zum Zuweisen von RBAC-Rollen finden Sie unter [Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Azure Active Directory](authorize-access-azure-active-directory.md).

## <a name="use-event-hubs-with-managed-identities"></a>Verwenden von Event Hubs mit verwalteten Identitäten
Wenn Sie Event Hubs mit verwalteten Identitäten verwenden möchten, müssen Sie der Identität die Rolle und den entsprechenden Bereich zuweisen. Im Verfahren in diesem Abschnitt wird eine einfache Anwendung verwendet, die unter einer verwalteten Identität ausgeführt wird und auf Event Hubs-Ressourcen zugreift.

Hier verwenden wir eine einfache Beispielwebanwendung, die in [Azure App Service](https://azure.microsoft.com/services/app-service/) gehostet wird. Eine Schritt-für-Schritt-Anleitung zum Erstellen einer Webanwendung finden Sie unter [Erstellen einer ASP.NET Core-Web-App in Azure](../app-service/app-service-web-get-started-dotnet.md).

Führen Sie nach der Erstellung der Anwendung die folgenden Schritte aus: 

1. Navigieren Sie zu **Einstellungen**, und wählen Sie **Identität** aus. 
1. Legen Sie den **Status** auf **Ein** fest. 
1. Klicken Sie auf **Speichern**, um die Einstellung zu speichern. 

    ![Verwaltete Identität für eine Web-App](./media/authenticate-managed-identity/identity-web-app.png)

Nachdem Sie diese Einstellung aktiviert haben, wird in Ihrer Azure AD-Instanz (Azure Active Directory) eine neue Dienstidentität erstellt und auf dem App Service-Host konfiguriert.

Weisen Sie dieser Dienstidentität jetzt eine Rolle im erforderlichen Bereich in Ihren Event Hubs-Ressourcen zu.

### <a name="to-assign-rbac-roles-using-the-azure-portal"></a>So weisen Sie RBAC-Rollen mit dem Azure-Portal zu
Navigieren Sie im Azure-Portal zur jeweiligen Ressource, um Event Hubs-Ressourcen eine Rolle zuzuweisen. Zeigen Sie die Einstellungen für die Zugriffssteuerung (IAM) für die Ressource an, und befolgen Sie diese Anleitung zum Verwalten von Rollenzuweisungen:

> [!NOTE]
> Mit den folgenden Schritten wird Ihren Event Hubs-Namespaces eine Dienstidentitätsrolle zugewiesen. Sie können dieselben Schritte ausführen, um eine Rolle zuzuweisen, die für eine Event Hubs-Ressource bestimmt ist. 

1. Navigieren Sie im Azure-Portal zu Ihrem Event Hubs-Namespace, und zeigen Sie die **Übersicht** für den Namespace an. 
1. Wählen Sie im linken Menü die Option **Zugriffssteuerung (IAM)** aus, um Zugriffssteuerungseinstellungen für den Event Hub anzuzeigen.
1.  Wählen Sie die Registerkarte **Rollenzuweisungen** aus, um die Liste mit den Rollenzuweisungen anzuzeigen.
3.  Klicken Sie auf **Hinzufügen**, um eine neue Rolle hinzuzufügen.
4.  Wählen Sie auf der Seite **Rollenzuweisung hinzufügen** die Event Hubs-Rollen aus, die Sie zuweisen möchten. Suchen Sie dann nach der Dienstidentität, die Sie registriert haben, um die Rolle zuzuweisen.
    
    ![Seite „Rollenzuweisung hinzufügen“](./media/authenticate-managed-identity/add-role-assignment-page.png)
5.  Wählen Sie **Speichern** aus. Die Identität, der Sie die Rolle zugewiesen haben, wird unter dieser Rolle angezeigt. Die folgende Abbildung zeigt, dass für die Dienstidentität der Besitzer „Event Hubs-Daten“ verwendet wird.
    
    ![Identität, die einer Rolle zugewiesen ist](./media/authenticate-managed-identity/role-assigned.png)

Nachdem Sie die Rolle zugewiesen haben, verfügt die Webanwendung über Zugriff auf die Event Hubs-Ressourcen unter dem definierten Bereich. 

### <a name="test-the-web-application"></a>Testen der Webanwendung
Sie können Ihre Webanwendung jetzt starten und für Ihren Browser auf die ASPX-Beispielseite verweisen. Sie finden die Beispielwebanwendung, mit der Daten für Event Hubs-Ressourcen gesendet und empfangen werden, im [GitHub-Repository](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/ManagedIdentityWebApp).

Installieren Sie das aktuelle Paket von [Nuget](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/), und beginnen Sie mit dem Senden und Empfangen von Daten für Event Hubs, indem Sie wie im folgenden Code das EventHubClient-Element verwenden: 

```csharp
var ehClient = EventHubClient.CreateWithManagedIdentity(new Uri($"sb://{EventHubNamespace}/"), EventHubName);
```

## <a name="next-steps"></a>Nächste Schritte
- Laden Sie das [Beispiel](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac/ManagedIdentityWebApp) von GitHub herunter.
- Informationen zu verwalteten Identitäten für Azure-Ressourcen finden Sie im folgenden Artikel: [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md)
- Informationen finden Sie in den folgenden verwandten Artikeln:
    - [Authentifizieren von Anforderungen an Event Hubs über eine Anwendung mithilfe von Azure Active Directory](authenticate-application.md)
    - [Authentifizieren von Anforderungen an Azure Event Hubs mithilfe von Shared Access Signature](authenticate-shared-access-signature.md)
    - [Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Azure Active Directory](authorize-access-azure-active-directory.md)
    - [Autorisieren des Zugriffs auf Event Hubs-Ressourcen mit Shared Access Signature](authorize-access-shared-access-signature.md)