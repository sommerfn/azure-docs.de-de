---
title: Konfigurieren von Anmeldeinformationen für die Bereitstellung – Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Anmeldeinformationen für die Azure App Service-Bereitstellung verwenden.
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnoc
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/14/2019
ms.author: cephalin
ms.reviewer: byvinyal
ms.custom: seodec18
ms.openlocfilehash: fc9445b64baae0e625b62356fee381329b01e8fd
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70098496"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Konfigurieren von Anmeldeinformationen für die Azure App Service-Bereitstellung
[Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) unterstützt zwei Arten von Anmeldeinformationen für [lokale Git-Bereitstellungen](deploy-local-git.md) und [FTP/S-Bereitstellungen](deploy-ftp.md). Diese Anmeldeinformationen sind nicht identisch mit den Anmeldeinformationen Ihres Azure-Abonnements.

* **Anmeldeinformationen auf Benutzerebene**: ein Satz von Anmeldeinformationen für das gesamte Azure-Konto. Hiermit können Sie App Service für alle Apps in allen Abonnements bereitstellen, für die das Azure-Konto über Zugriffsberechtigungen verfügt. Dies ist der Standardsatz, der auf der Benutzeroberfläche des Portals angezeigt wird (z.B. **Übersicht** und **Eigenschaften** auf der [Ressourcenseite](../azure-resource-manager/manage-resources-portal.md#manage-resources) der App). Wenn ein Benutzer App-Zugriff über rollenbasierte Zugriffssteuerung (RBAC) oder Co-Administrator-Berechtigungen erhält, kann er die eigenen Anmeldeinformationen auf Benutzerebene verwenden, bis der Zugriff widerrufen wird. Teilen Sie diese Anmeldeinformationen nicht mit anderen Azure-Benutzern.

* **Anmeldeinformationen auf App-Ebene**: ein Satz von Anmeldeinformationen für jede App. Er kann nur verwendet werden, um diese App bereitzustellen. Die Anmeldeinformationen für eine App werden bei der Erstellung jeder App automatisch generiert. Sie können nicht manuell konfiguriert, jedoch jederzeit zurückgesetzt werden. Damit ein Benutzer Zugriff auf Anmeldeinformationen auf App-Ebene über die rollenbasierte Zugriffssteuerung (RBAC) erhalten kann, muss er in der App mindestens ein Mitwirkender sein. Benutzer mit Leseberechtigung dürfen nicht veröffentlichen und haben keinen Zugriff auf diese Anmeldeinformationen.

## <a name="userscope"></a>Konfigurieren von Anmeldeinformationen auf Benutzerebene

Sie können die Anmeldeinformationen auf Benutzerebene auf der [Ressourcenseite](../azure-resource-manager/manage-resources-portal.md#manage-resources) einer App konfigurieren. Unabhängig von der App, in der Sie diese Anmeldeinformationen konfigurieren, gelten sie für alle Apps und für alle Abonnements in Ihrem Azure-Konto. 

### <a name="in-the-cloud-shell"></a>In der Cloud Shell

Führen Sie zum Konfigurieren des Bereitstellungsbenutzers in der [Cloud Shell](https://shell.azure.com) den Befehl [az webapp deployment user set](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) aus. Ersetzen Sie „\<username>“ und „\<password>“ durch Ihren Benutzernamen und Ihr Kennwort für die Bereitstellung. 

- Der Benutzername muss in Azure eindeutig sein und darf bei lokalen Git-Pushes nicht das Symbol „@“ enthalten. 
- Das Kennwort muss mindestens acht Zeichen lang sein und zwei der folgenden drei Elemente enthalten: Buchstaben, Zahlen und Symbole. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

In der JSON-Ausgabe wird das Kennwort als `null` angezeigt. Wenn Sie den Fehler `'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler `'Bad Request'. Details: 400` erhalten, müssen Sie ein sichereres Kennwort verwenden. 

### <a name="in-the-portal"></a>Im Portal

Im Azure-Portal muss mindestens eine App vorhanden sein, bevor Sie auf die Seite mit den Anmeldeinformationen für die Bereitstellung zugreifen können. So konfigurieren Sie die Anmeldeinformationen auf Benutzerebene

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Menü **App Services** >  **\<beliebige_App>**  > **Bereitstellungscenter** > **FTP** > **Dashboard** aus.

    ![](./media/app-service-deployment-credentials/access-no-git.png)

    Oder wenn Sie Git-Bereitstellung bereits konfiguriert haben, wählen Sie **App Services** >  **&lt;beliebige_App>**  > **Bereitstellungscenter** > **FTP/Anmeldeinformationen** aus.

    ![](./media/app-service-deployment-credentials/access-with-git.png)

2. Wählen Sie **Benutzeranmeldeinformationen** aus, konfigurieren Sie den Benutzernamen und das Kennwort, und wählen Sie dann **Anmeldeinformationen speichern** aus.

Wenn Sie Anmeldeinformationen für die Bereitstellung festgelegt haben, finden Sie den *Git*-Benutzernamen für Bereitstellungen auf der Seite **Übersicht** Ihrer App.

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

Wenn die Git-Bereitstellung konfiguriert ist, wird auf der Seite ein **Git-/Bereitstellungsbenutzername** angezeigt, andernfalls ein **FTP-/Bereitstellungsbenutzername**.

> [!NOTE]
> Ihr Kennwort auf Benutzerebene für die Bereitstellung wird von Azure nicht angezeigt. Wenn Sie das Kennwort vergessen, können Sie Ihre Anmeldeinformationen über die Schritte in diesem Abschnitt zurücksetzen.
>
> 

## <a name="use-user-level-credentials-with-ftpftps"></a>Verwenden von Anmeldeinformationen auf Benutzerebene mit FTP/FTPS

Für die Authentifizierung an einem FTP/FTPS-Endpunkt mit Anmeldeinformationen auf Benutzerebene ist ein Benutzername im folgenden Format erforderlich: `<app-name>\<user-name>`

Weil Anmeldeinformationen auf Benutzerebene mit dem Benutzer und nicht einer bestimmten Ressource verknüpft sind, muss der Benutzername dieses Format aufweisen, damit die Anmeldeaktion an den richtigen App-Endpunkt weitergeleitet wird.

## <a name="appscope"></a>Abrufen und Zurücksetzen der Anmeldeinformationen auf App-Ebene
So rufen Sie die Anmeldeinformationen auf App-Ebene ab

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im linken Menü **App Services** >  **&lt;beliebige_App>**  > **Bereitstellungscenter** > **FTP/Anmeldeinformationen** aus.

2. Wählen Sie **App-Anmeldeinformationen** aus und dann den Link **Kopieren**, um den Benutzernamen oder das Kennwort zu kopieren.

Um die Anmeldeinformationen auf App-Ebene zurückzusetzen, wählen Sie im selben Dialogfeld **Anmeldeinformationen zurücksetzen** aus.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie diese Anmeldeinformationen zum Bereitstellen Ihrer App über [lokales Git](deploy-local-git.md) oder [FTP/S](deploy-ftp.md) verwenden.
