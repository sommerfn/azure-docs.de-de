---
title: Schützen der SQL-Datenbankverbindung mittels einer verwalteten Identität – Azure App Service | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Sicherheit der Datenbankverbindung mithilfe einer verwalteten Identität erhöhen und wie Sie dieses Wissen auf andere Azure-Dienste übertragen.
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 06/21/2019
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 31535642526c608ad0ae29e5c0e3c93368e184ad
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67481012"
---
# <a name="tutorial-secure-azure-sql-database-connection-from-app-service-using-a-managed-identity"></a>Tutorial: Schützen der Azure SQL-Datenbank-Verbindung von App Service mittels einer verwalteten Identität

[App Service](overview.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching in Azure. Außerdem steht eine [verwaltete Identität](overview-managed-identity.md) für Ihre App zur Verfügung. Hierbei handelt es sich um eine vorgefertigte Lösung zum Schutz des Zugriffs auf [Azure SQL-Datenbank](/azure/sql-database/) und andere Azure-Dienste. Verwaltete Identitäten in App Service machen Ihre App frei von Geheimnissen (wie etwa Anmeldeinformationen in Verbindungszeichenfolgen) und verbessern so die Sicherheit Ihrer App. In diesem Tutorial fügen Sie der ASP.NET-Beispiel-Web-App eine verwaltete Identität hinzu, die Sie hier erstellt: [Tutorial: Erstellen einer ASP.NET-App in Azure mit SQL-Datenbank](app-service-web-tutorial-dotnet-sqldatabase.md). Danach stellt Ihre Beispiel-App ganz ohne Benutzername und Kennwort eine sichere Verbindung mit SQL-Datenbank her.

> [!NOTE]
> Dieses Szenario wird aktuell von .NET Framework 4.7.2 und höheren Versionen unterstützt. [.NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2) unterstützt das Szenario zwar, es wurde jedoch noch nicht in die Standardimages in App Service aufgenommen. 
>

Folgendes wird vermittelt:

> [!div class="checklist"]
> * Aktivieren von verwalteten Identitäten
> * Gewähren von SQL-Datenbank-Zugriff für die verwaltete Identität
> * Konfigurieren von Entity Framework für die Verwendung der Azure AD-Authentifizierung mit SQL-Datenbank
> * Herstellen einer Verbindung mit SQL-Datenbank über Visual Studio unter Verwendung der Azure AD-Authentifizierung

> [!NOTE]
>Die Azure AD-Authentifizierung _unterscheidet sich_ von der [integrierten Windows-Authentifizierung](/previous-versions/windows/it-pro/windows-server-2003/cc758557(v=ws.10)) im lokalen Active Directory (AD DS). AD DS und Azure AD verwenden grundverschiedene Authentifizierungsprotokolle. Weitere Informationen finden Sie in der [Dokumentation zu Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Dieser Artikel fährt dort fort, wo Sie hier aufgehört haben: [Tutorial: Erstellen einer ASP.NET-App in Azure mit SQL-Datenbank](app-service-web-tutorial-dotnet-sqldatabase.md). Sollten Sie das Tutorial noch nicht absolviert haben, holen Sie dies jetzt nach. Alternativ können Sie die Schritte für Ihre eigene ASP.NET-App mit SQL-Datenbank anpassen.

Vergewissern Sie sich zum Debuggen Ihrer App mit SQL-Datenbank als Back-End, dass Sie [Clientverbindung von Ihrem Computer aus zugelassen](app-service-web-tutorial-dotnet-sqldatabase.md#allow-client-connection-from-your-computer) haben.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="grant-azure-ad-user-access-to-database"></a>Gewähren von Datenbankzugriff für Azure AD-Benutzer

Aktivieren Sie zunächst die Azure AD-Authentifizierung für SQL-Datenbank, indem Sie einen Azure AD-Benutzer als Active Directory-Administrator des SQL-Datenbank-Servers zuweisen. Dieser Benutzer unterscheidet sich von dem Microsoft-Konto, das Sie bei der Registrierung für Ihr Azure-Abonnement verwendet haben. Es muss sich um einen Benutzer handeln, den Sie in Azure AD erstellt, importiert, synchronisiert oder eingeladen haben. Weitere Informationen zu zulässigen Azure AD-Benutzern finden Sie unter [Funktionen und Einschränkungen von Azure AD](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations). 

Ermitteln Sie die Objekt-ID des Azure AD-Benutzers mithilfe von [`az ad user list`](/cli/azure/ad/user?view=azure-cli-latest#az-ad-user-list), und ersetzen Sie den Platzhalter *\<user-principal-name>* . Das Ergebnis wird in einer Variablen gespeichert.

```azurecli-interactive
azureaduser=$(az ad user list --filter "userPrincipalName eq '<user-principal-name>'" --query [].objectId --output tsv)
```
> [!TIP]
> Wenn Sie eine Liste mit allen Benutzerprinzipalnamen in Azure AD anzeigen möchten, führen Sie `az ad user list --query [].userPrincipalName` aus.
>

Fügen Sie diesen Azure AD-Benutzer in Cloud Shell mithilfe des Befehls [`az sql server ad-admin create`](/cli/azure/sql/server/ad-admin?view=azure-cli-latest#az-sql-server-ad-admin-create) als Active Directory-Administrator hinzu. Ersetzen Sie im folgenden Befehl den Platzhalter *\<server-name>* :

```azurecli-interactive
az sql server ad-admin create --resource-group myResourceGroup --server-name <server-name> --display-name ADMIN --object-id $azureaduser
```

Weitere Informationen zum Hinzufügen eines Active Directory-Administrators finden Sie unter [Bereitstellen eines Azure Active Directory-Administrators für Ihren Azure SQL-Datenbank-Server](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server).

## <a name="set-up-visual-studio"></a>Einrichten von Visual Studio

Fügen Sie in Visual Studio Ihren Azure AD-Benutzer hinzu, um in Visual Studio entwickeln und debuggen zu können. Wählen Sie dazu über das Menü **Datei** > **Kontoeinstellungen** aus, und klicken Sie auf **Konto hinzufügen**.

Wählen Sie über das Menü **Extras** > **Optionen** und anschließend **Azure Service Authentication** (Azure-Dienstauthentifizierung) > **Kontoauswahl** aus, um den Azure AD-Benutzer für die Azure-Dienstauthentifizierung festzulegen. Wählen Sie den Azure AD-Benutzer aus, den Sie hinzugefügt haben, und klicken Sie auf **OK**.

Nun können Sie Ihre App mit der SQL-Datenbank als Back-End entwickeln und debuggen und dabei die Azure AD-Authentifizierung verwenden.

## <a name="modify-aspnet-project"></a>Ändern des ASP.NET-Projekts

Öffnen Sie in Visual Studio die Paket-Manager-Konsole, und fügen Sie das NuGet-Paket [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) hinzu:

```powershell
Install-Package Microsoft.Azure.Services.AppAuthentication -Version 1.2.0
```

Nehmen Sie in *Web.config* nacheinander folgende Änderungen vor:

- Fügen Sie in `<configSections>` die folgende Abschnittsdeklaration ein:

    ```xml
    <section name="SqlAuthenticationProviders" type="System.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
    ```

- (Die Deklaration muss nach dem schließenden Tag `</configSections>` eingefügt werden.) Fügen Sie anschließend den folgenden XML-Code für `<SqlAuthenticationProviders>` hinzu:

    ```xml
    <SqlAuthenticationProviders>
      <providers>
        <add name="Active Directory Interactive" type="Microsoft.Azure.Services.AppAuthentication.SqlAppAuthenticationProvider, Microsoft.Azure.Services.AppAuthentication" />
      </providers>
    </SqlAuthenticationProviders>
    ```    

- Suchen Sie nach der Verbindungszeichenfolge `MyDbConnection`, und ersetzen Sie den Wert `connectionString` durch `"server=tcp:<server-name>.database.windows.net;database=<db-name>;UID=AnyString;Authentication=Active Directory Interactive"`. Ersetzen Sie die Platzhalter _\<server-name>_ und _\<db-name>_ durch Ihren Servernamen bzw. durch Ihren Datenbanknamen.

Drücken Sie `Ctrl+F5`, um die App erneut auszuführen. Die gleiche CRUD-App in Ihrem Browser stellt nun unter Verwendung der Azure AD-Authentifizierung eine Direktverbindung mit der Azure SQL-Datenbank her. Dieses Setup ermöglicht das Ausführen von Datenbankmigrationen. Wenn Sie Ihre Änderungen später in App Service bereitstellen, können die gleichen Einstellungen mit der verwalteten Identität der App verwendet werden.

## <a name="use-managed-identity-connectivity"></a>Verwenden von Konnektivität mit verwalteter Identität

Als Nächstes konfigurieren Sie Ihre App Service-App so, dass sie beim Herstellen der Verbindung mit SQL-Datenbank eine vom System zugewiesene verwaltete Identität verwendet.

### <a name="enable-managed-identity-on-app"></a>Aktivieren einer verwalteten Identität für die App

Verwenden Sie den Befehl [az webapp identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) in Cloud Shell, um eine verwaltete Identität für Ihre Azure-App zu aktivieren. Ersetzen Sie im folgenden Befehl den Platzhalter *\<app-name>* .

```azurecli-interactive
az webapp identity assign --resource-group myResourceGroup --name <app-name>
```

Beispiel für die Ausgabe:

```json
{
  "additionalProperties": {},
  "principalId": "21dfa71c-9e6f-4d17-9e90-1d28801c9735",
  "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47",
  "type": "SystemAssigned"
}
```

### <a name="add-managed-identity-to-an-azure-ad-group"></a>Hinzufügen der verwalteten Identität zu einer Azure AD-Gruppe

Um dieser Identität Zugriff auf Ihre SQL-Datenbank zu gewähren, müssen Sie sie einer [Azure AD-Gruppe](../active-directory/fundamentals/active-directory-manage-groups.md) hinzufügen. Fügen Sie sie in Cloud Shell einer neuen Gruppe namens _myAzureSQLDBAccessGroup_ hinzu, wie im folgenden Skript zu sehen:

```azurecli-interactive
groupid=$(az ad group create --display-name myAzureSQLDBAccessGroup --mail-nickname myAzureSQLDBAccessGroup --query objectId --output tsv)
msiobjectid=$(az webapp identity show --resource-group myResourceGroup --name <app-name> --query principalId --output tsv)
az ad group member add --group $groupid --member-id $msiobjectid
az ad group member list -g $groupid
```

Wenn Sie die Parameter `--query objectId --output tsv` weglassen, wird die vollständige JSON-Ausgabe für die einzelnen Befehle angezeigt.

### <a name="grant-permissions-to-azure-ad-group"></a>Erteilen von Berechtigungen für die Azure AD-Gruppe

Melden Sie sich in Cloud Shell mithilfe des Befehls „sqlcmd“ bei SQL-Datenbank an. Ersetzen Sie _\<server-name>_ durch den Namen Ihres SQL-Datenbank-Servers, _\<db-name>_ durch den von Ihrer App verwendeten Datenbanknamen sowie _\<aad-user-name>_ und _\<aad-password>_ durch die Anmeldeinformationen Ihres Azure AD-Benutzers.

```azurecli-interactive
sqlcmd -S <server-name>.database.windows.net -d <db-name> -U <aad-user-name> -P "<aad-password>" -G -l 30
```

Führen Sie an der SQL-Eingabeaufforderung für die gewünschte Datenbank die folgenden Befehle aus, um die Azure AD-Gruppe hinzuzufügen und der App die nötigen Berechtigungen zu erteilen. Beispiel: 

```sql
CREATE USER [myAzureSQLDBAccessGroup] FROM EXTERNAL PROVIDER;
ALTER ROLE db_datareader ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_datawriter ADD MEMBER [myAzureSQLDBAccessGroup];
ALTER ROLE db_ddladmin ADD MEMBER [myAzureSQLDBAccessGroup];
GO
```

Geben Sie `EXIT` ein, um zur Cloud Shell-Eingabeaufforderung zurückzukehren.

### <a name="modify-connection-string"></a>Ändern der Verbindungszeichenfolge

Zur Erinnerung: Die in `Web.config` vorgenommenen Änderungen funktionieren auch mit der verwalteten Identität. Sie müssen also lediglich die vorhandene Verbindungszeichenfolge entfernen, die von Visual Studio im Zuge der erstmaligen Bereitstellung Ihrer App erstellt wurde. Verwenden Sie den folgenden Befehl, und ersetzen Sie dabei den Platzhalter *\<app-name>* durch den Namen Ihrer App:

```azurecli-interactive
az webapp config connection-string delete --resource-group myResourceGroup --name <app-name> --setting-names MyDbConnection
```

## <a name="publish-your-changes"></a>Veröffentlichen der Änderungen

Nun müssen die Änderungen nur noch in Azure veröffentlicht werden.

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **DotNetAppSqlDb**, und wählen Sie **Veröffentlichen** aus.

![Veröffentlichen über den Projektmappen-Explorer](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Klicken Sie auf der Veröffentlichungsseite auf **Veröffentlichen**. Wenn die neue Webseite Ihre Aufgabenliste anzeigt, stellt Ihre App unter Verwendung der verwalteten Identität eine Verbindung mit der Datenbank her.

![Azure-App nach Code First-Migration](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Die Aufgabenliste sollte sich nun wie gewohnt bearbeiten lassen.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben Folgendes gelernt:

> [!div class="checklist"]
> * Aktivieren von verwalteten Identitäten
> * Gewähren von SQL-Datenbank-Zugriff für die verwaltete Identität
> * Konfigurieren von Entity Framework für die Verwendung der Azure AD-Authentifizierung mit SQL-Datenbank
> * Herstellen einer Verbindung mit SQL-Datenbank über Visual Studio unter Verwendung der Azure AD-Authentifizierung

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer Web-App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"]
> [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure App Service](app-service-web-tutorial-custom-domain.md)
