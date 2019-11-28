---
title: 'PHP (Laravel) mit MySQL unter Linux: Azure App Service | Microsoft-Dokumentation'
description: Informationen zum Ausführen einer PHP-App in Azure App Service unter Linux mit einer Verbindung mit einer MySQL-Datenbank. In diesem Tutorial wird Laravel verwendet.
services: app-service\web
author: cephalin
manager: jeconnoc
ms.service: app-service-web
ms.workload: web
ms.devlang: php
ms.topic: tutorial
ms.date: 11/25/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 4fade03d798096e250cb5b56fbb2003ea4b58e1b
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74481323"
---
# <a name="build-a-php-and-mysql-app-in-azure-app-service-on-linux"></a>Erstellen einer PHP- und MySQL-App in Azure App Service unter Linux

> [!NOTE]
> In diesem Artikel wird eine App in App Service unter Linux bereitgestellt. Informationen zur Bereitstellung in App Service unter _Windows_ finden Sie unter [Erstellen einer PHP- und MySQL-App in Azure](../app-service-web-tutorial-php-mysql.md).
>

[App Service unter Linux](app-service-linux-intro.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching unter Linux-Betriebssystemen. In diesem Tutorial wird gezeigt, wie Sie eine PHP-App erstellen und mit einer MySQL-Datenbank verbinden. Nachdem Sie diese Schritte ausgeführt haben, verfügen Sie über eine [Laravel](https://laravel.com/)-App, die in App Service unter Linux ausgeführt wird.

![In Azure App Service ausgeführte PHP-App](./media/tutorial-php-mysql-app/complete-checkbox-published.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer MySQL-Datenbank in Azure
> * Verbinden einer PHP-App mit MySQL
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Verwalten der App im Azure-Portal

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Installation von Git](https://git-scm.com/)
* [Installation von PHP 5.6.4 oder höher](https://php.net/downloads.php)
* [Installation von Composer](https://getcomposer.org/doc/00-intro.md)
* Aktivieren Sie die folgenden PHP-Erweiterungen, die Laravel benötigt: OpenSSL, PDO-MySQL, Mbstring, Tokenizer, XML
* [Installieren und Starten von MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

## <a name="prepare-local-mysql"></a>Vorbereiten der lokalen MySQL-Instanz

In diesem Schritt erstellen Sie eine Datenbank auf einem lokalen MySQL-Server für die Verwendung in diesem Tutorial.

### <a name="connect-to-local-mysql-server"></a>Verbinden mit dem lokalen MySQL-Server

Stellen Sie in einem Terminalfenster eine Verbindung mit Ihrem lokalen MySQL-Server her. Sie können dieses Terminalfenster verwenden, um alle Befehle dieses Tutorials auszuführen.

```bash
mysql -u root -p
```

Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, geben Sie das Kennwort für das `root`-Konto ein. Wenn Sie das Kennwort für Ihr Root-Konto vergessen haben, lesen Sie unter [MySQL: How to Reset the Root Password (MySQL: Zurücksetzen des Root-Kennworts)](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html) nach.

Wenn der Befehl erfolgreich ausgeführt wurde, wird der MySQL-Server ausgeführt. Stellen Sie andernfalls anhand der [Nachinstallationsschritte für MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) sicher, dass Ihr lokaler MySQL-Server gestartet wurde.

### <a name="create-a-database-locally"></a>Lokales Erstellen einer Datenbank

Erstellen Sie an der Eingabeaufforderung `mysql` eine Datenbank.

```sql 
CREATE DATABASE sampledb;
```

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a>Lokales Erstellen einer PHP-App
In diesem Schritt rufen Sie eine Laravel-Beispielanwendung ab, konfigurieren ihre Datenbankverbindung und führen sie lokal aus. 

### <a name="clone-the-sample"></a>Klonen des Beispiels

Wechseln Sie im Terminalfenster mit `cd` in ein Arbeitsverzeichnis.

Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen.

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

Wechseln Sie mit `cd` in das geklonte Verzeichnis.
Installieren Sie die erforderlichen Pakete.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>Konfigurieren der MySQL-Verbindung

Erstellen Sie im Repositorystamm eine Datei mit dem Namen *.env*. Kopieren Sie die folgenden Variablen in die Datei *.env*. Ersetzen Sie den Platzhalter _&lt;root_password>_ durch das Kennwort des MySQL-Root-Benutzers.

```txt
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sampledb
DB_USERNAME=root
DB_PASSWORD=<root_password>
```

Informationen dazu, wie Laravel die _ENV_-Datei verwendet, finden Sie unter [Laravel Environment Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration) (Konfigurieren der Laravel-Umgebung).

### <a name="run-the-sample-locally"></a>Lokales Ausführen des Beispiels

Führen Sie [Laravel-Datenbankmigrationen](https://laravel.com/docs/5.4/migrations) durch, um die von der Anwendung benötigten Tabellen zu erstellen. Im Verzeichnis _database/migrations_ im Git-Repository können Sie sehen, welche Tabellen bei den Migrationen erstellt werden.

```bash
php artisan migrate
```

Generieren Sie einen neuen Anwendungsschlüssel für Laravel.

```bash
php artisan key:generate
```

Führen Sie die Anwendung aus.

```bash
php artisan serve
```

Navigieren Sie in einem Browser zu `http://localhost:8000`. Fügen Sie auf der Seite einige Aufgaben hinzu.

![Erfolgreiche Verbindung zwischen PHP und MySQL](./media/tutorial-php-mysql-app/mysql-connect-success.png)

Geben Sie zum Beenden von PHP im Terminal `Ctrl + C` ein.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Erstellen von MySQL in Azure

In diesem Schritt erstellen Sie eine MySQL-Datenbank in [Azure Database for MySQL](/azure/mysql). Später konfigurieren Sie die PHP-Anwendung für eine Verbindung mit dieser Datenbank.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-a-mysql-server"></a>Erstellen eines MySQL-Servers

Erstellen Sie in Azure Database for MySQL mit dem Befehl [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az-mysql-server-create) einen Server.

Ersetzen Sie im folgenden Befehl den Platzhalter *\<mysql-server-name>* durch einen eindeutigen Servernamen, den Platzhalter *\<admin-user>* durch einen Benutzernamen und den Platzhalter *\<admin-password>* durch ein Kennwort. Der Servername dient als Teil Ihres MySQL-Endpunkts (`https://<mysql-server-name>.mysql.database.azure.com`). Daher muss der Name auf allen Servern in Azure eindeutig sein. Ausführlichere Informationen zum Auswählen einer MySQL-Datenbank-SKU finden Sie unter [Erstellen eines Servers für Azure-Datenbank für MySQL](https://docs.microsoft.com/azure/mysql/quickstart-create-mysql-server-database-using-azure-cli#create-an-azure-database-for-mysql-server).

```azurecli-interactive
az mysql server create --resource-group myResourceGroup --name <mysql-server-name> --location "West Europe" --admin-user <admin-user> --admin-password <admin-password> --sku-name B_Gen5_1
```

Nach dem Erstellen des MySQL-Servers zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "<admin-user>",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql-server-name>.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql-server-name>",
  "location": "westeurope",
  "name": "<mysql-server-name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Konfigurieren der Serverfirewall

Erstellen Sie mit dem Befehl [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az-mysql-server-firewall-rule-create) eine Firewallregel für Ihren MySQL-Server, um Clientverbindungen zuzulassen. Wenn sowohl Start- als auch End-IP auf 0.0.0.0 festgelegt ist, wird die Firewall nur für andere Azure-Ressourcen geöffnet. 

```azurecli-interactive
az mysql server firewall-rule create --name allAzureIPs --server <mysql-server-name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP] 
> Sie können Ihre Firewallregel auch noch restriktiver gestalten und [nur die ausgehenden IP-Adressen verwenden, die Ihre App verwendet](../overview-inbound-outbound-ips.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).
>

Führen Sie den Befehl in Cloud Shell erneut aus, um den Zugriff vom lokalen Computer zuzulassen. Ersetzen Sie dabei *\<your-ip-address>* durch [Ihre lokale IPv4-IP-Adresse](https://www.whatsmyip.org/).

```azurecli-interactive
az mysql server firewall-rule create --name AllowLocalClient --server <mysql-server-name> --resource-group myResourceGroup --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address>
```

### <a name="connect-to-production-mysql-server-locally"></a>Lokales Verbinden mit dem MySQL-Produktionsserver

Stellen Sie im Terminalfenster eine Verbindung mit dem MySQL-Server in Azure her. Verwenden Sie die Werte, die Sie zuvor für _&lt;admin-user>_ und _&lt;mysql-server-name>_ angegeben haben. Wenn Sie zur Eingabe eines Kennworts aufgefordert werden, verwenden Sie das Kennwort, das Sie bei der Datenbankerstellung in Azure angegeben haben.

```bash
mysql -u <admin-user>@<mysql-server-name> -h <mysql-server-name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-production-database"></a>Erstellen einer Produktionsdatenbank

Erstellen Sie an der Eingabeaufforderung `mysql` eine Datenbank.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Erstellen eines Benutzers mit Berechtigungen

Erstellen Sie einen Datenbankbenutzer mit dem Namen _phpappuser_, und weisen Sie ihm alle Berechtigungen in der Datenbank `sampledb` zu.

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'phpappuser';
```

Beenden Sie die Serververbindung durch Eingabe von `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Verbinden der App mit Azure MySQL

In diesem Schritt verbinden Sie die PHP-Anwendung mit der MySQL-Datenbank, die Sie in Azure Database for MySQL erstellt haben.

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Konfigurieren der Datenbankverbindung

Erstellen Sie im Stammverzeichnis des Repositorys eine Datei _.env.production_, und kopieren Sie die folgenden Variablen in die Datei. Ersetzen Sie den Platzhalter _&lt;mysql-server-name>_ .

```txt
APP_ENV=production
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=<mysql-server-name>.mysql.database.azure.com
DB_DATABASE=sampledb
DB_USERNAME=phpappuser@<mysql-server-name>
DB_PASSWORD=MySQLAzure2017
MYSQL_SSL=true
```

Speichern Sie die Änderungen.

> [!TIP]
> Um Ihre MySQL-Verbindungsinformationen zu schützen, ist diese Datei bereits aus dem Git-Repository ausgeschlossen (siehe _.gitignore_ im Repositorystamm). Später erfahren Sie, wie Sie die Umgebungsvariablen in App Service so konfigurieren, dass eine Verbindung mit Ihrer Datenbank in Azure Database for MySQL hergestellt wird. Bei Umgebungsvariablen benötigen Sie die *ENV*-Datei in App Service nicht.
>

### <a name="configure-ssl-certificate"></a>Konfigurieren des SSL-Zertifikats

Standardmäßig erzwingt Azure-Datenbank für MySQL SSL-Verbindungen von Clients. Zum Herstellen einer Verbindung mit Ihrer MySQL-Datenbank in Azure müssen Sie das [_PEM_-Zertifikat verwenden, das von Azure Database for MySQL bereitgestellt wird](../../mysql/howto-configure-ssl.md).

Öffnen Sie _config/database.php_, und fügen Sie `connections.mysql` die Parameter _sslmode_ und _options_ hinzu. Dies wird im folgenden Code veranschaulicht.

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL') && extension_loaded('pdo_mysql')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/BaltimoreCyberTrustRoot.crt.pem',
    ] : []
],
```

Das Zertifikat `BaltimoreCyberTrustRoot.crt.pem` wird in diesem Tutorial der Einfachheit halber im Repository bereitgestellt.

### <a name="test-the-application-locally"></a>Lokales Testen der Anwendung

Führen Sie die Laravel-Datenbankmigrationen mit der Datei _.env.production_ als Umgebungsdatei aus, um die Tabellen in der MySQL-Datenbank in Azure Database for MySQL zu erstellen. Beachten Sie, dass _.env.production_ die Verbindungsinformationen für Ihre MySQL-Datenbank in Azure enthält.

```bash
php artisan migrate --env=production --force
```

_.env.production_ verfügt noch nicht über einen gültigen Anwendungsschlüssel. Generieren Sie im Terminal einen neuen.

```bash
php artisan key:generate --env=production --force
```

Führen Sie die Beispielanwendung mit _.env.production_ als Umgebungsdatei aus.

```bash
php artisan serve --env=production
```

Navigieren Sie zu `http://localhost:8000`. Wenn die Seite ohne Fehler geladen wird, ist die PHP-Anwendung mit der MySQL-Datenbank in Azure verbunden.

Fügen Sie auf der Seite einige Aufgaben hinzu.

![Erfolgreiche Verbindung zwischen PHP und Azure Database for MySQL](./media/tutorial-php-mysql-app/mysql-connect-success.png)

Geben Sie zum Beenden von PHP im Terminal `Ctrl + C` ein.

### <a name="commit-your-changes"></a>Committen Ihrer Änderungen

Führen Sie die folgenden Git-Befehle aus, um für Ihre Änderungen einen Commit durchzuführen:

```bash
git add .
git commit -m "database.php updates"
```

Ihre App kann jetzt bereitgestellt werden.

## <a name="deploy-to-azure"></a>Bereitstellen in Azure

In diesem Schritt stellen Sie die mit MySQL verbundene PHP-Anwendung in Azure App Service bereit.

Die Laravel-Anwendung wird im Verzeichnis _/public_ gestartet. Für das PHP-Docker-Image für App Service wird Apache verwendet, und das Anpassen des `DocumentRoot`-Elements für Laravel ist nicht möglich. Sie können aber `.htaccess` verwenden, um alle Anforderungen so umzuschreiben, dass sie nicht auf das Stammverzeichnis verweisen, sondern auf _/public_. Im Repositorystamm wurde bereits ein `.htaccess`-Element für diese Zwecke hinzugefügt. Hiermit ist Ihre Laravel-Anwendung bereit für die Bereitstellung.

Weitere Informationen finden Sie unter [Ändern des Stammverzeichnisses der Website](configure-language-php.md#change-site-root).

### <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Erstellen einer Web-App

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-php-linux-no-h.md)] 

### <a name="configure-database-settings"></a>Konfigurieren der Datenbankeinstellungen

Legen Sie in App Service mit dem Befehl [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) Umgebungsvariablen als _App-Einstellungen_ fest.

Mit dem folgenden Befehl werden die App-Einstellungen `DB_HOST`, `DB_DATABASE`, `DB_USERNAME` und `DB_PASSWORD` konfiguriert. Ersetzen Sie die Platzhalter _&lt;appname>_ und _&lt;mysql-server-name>_ .

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group myResourceGroup --settings DB_HOST="<mysql-server-name>.mysql.database.azure.com" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql-server-name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

Sie können die PHP-Methode [getenv](https://php.net/manual/en/function.getenv.php) verwenden, um [auf die Einstellungen zuzugreifen](configure-language-php.md#access-environment-variables). Im Laravel-Code wird ein [env](https://laravel.com/docs/5.4/helpers#method-env)-Wrapper für die PHP-Methode `getenv` verwendet. Die MySQL-Konfiguration in _config/database.php_ sieht beispielsweise wie im folgenden Code aus:

```php
'mysql' => [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    ...
],
```

### <a name="configure-laravel-environment-variables"></a>Konfigurieren der Laravel-Umgebungsvariablen

Für Laravel ist in App Service ein Anwendungsschlüssel erforderlich. Sie können ihn mit App-Einstellungen konfigurieren.

Verwenden Sie `php artisan`, um einen neuen Anwendungsschlüssel zu generieren, ohne ihn in der Datei _.env_ zu speichern.

```bash
php artisan key:generate --show
```

Legen Sie den Anwendungsschlüssel in der App Service-App fest, indem Sie den Befehl [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) verwenden. Ersetzen Sie die Platzhalter _&lt;appname>_ und _&lt;outputofphpartisankey:generate>_ .

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group myResourceGroup --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

`APP_DEBUG="true"` weist Laravel an, Debuginformationen zurückzugeben, wenn die bereitgestellte App Fehler feststellt. Legen Sie beim Ausführen einer Produktionsanwendung `false` fest, weil dies sicherer ist.

### <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

Fügen Sie Ihrem lokalen Git-Repository eine Azure-Remoteinstanz hinzu.

```bash
git remote add azure <paste_copied_url_here>
```

Führen Sie einen Pushvorgang an die Azure-Remoteinstanz durch, um die PHP-Anwendung bereitzustellen. Sie werden zur Angabe des Kennworts aufgefordert, das Sie zuvor bei der Erstellung des Bereitstellungsbenutzers angegeben haben.

```bash
git push azure master
```

Während der Bereitstellung meldet Azure App Service seinen Status mit Git.

```bash
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
```

> [!NOTE]
> Sie werden feststellen, dass am Ende der Bereitstellung [Composer](https://getcomposer.org/)-Pakete installiert werden. App Service führt diese Automatisierungen nicht während der Bereitstellung aus. Daher enthält dieses Beispielrepository drei zusätzliche Dateien im Stammverzeichnis für die Aktivierung:
>
> - `.deployment`: Diese Datei weist App Service an, `bash deploy.sh` als benutzerdefiniertes Bereitstellungsskript auszuführen.
> - `deploy.sh`: Das Der benutzerdefinierte Bereitstellungsskript. Wenn Sie die Datei überprüfen, sehen Sie, dass `php composer.phar install` nach `npm install` ausgeführt wird.
> - `composer.phar`: Der Composer-Paket-Manager.
>
> Mit diesem Ansatz können Sie der Git-basierten Bereitstellung in App Service beliebige Schritte hinzufügen. Weitere Informationen finden Sie unter [Ausführen von Composer](configure-language-php.md#run-composer).
>

### <a name="browse-to-the-azure-app"></a>Navigieren zur Azure-App

Browsen Sie zu `http://<app-name>.azurewebsites.net`, und fügen Sie der Liste einige Aufgaben hinzu.

![In Azure App Service ausgeführte PHP-App](./media/tutorial-php-mysql-app/php-mysql-in-azure.png)

Glückwunsch! Sie führen eine datengesteuerte PHP-App in Azure App Service aus.

## <a name="update-model-locally-and-redeploy"></a>Lokales Aktualisieren und erneutes Bereitstellen des Modells

In diesem Schritt nehmen Sie eine einfache Änderung am `task`-Datenmodell und der Web-App vor und veröffentlichen das Update dann in Azure.

Für das tasks-Szenario ändern Sie die Anwendung, damit Sie eine Aufgabe als abgeschlossen kennzeichnen können.

### <a name="add-a-column"></a>Hinzufügen eines Spaltennamens

Navigieren Sie im Terminal zum Stamm des Git-Repositorys.

Generieren Sie eine neue Datenbankmigration für die Tabelle `tasks`:

```bash
php artisan make:migration add_complete_column --table=tasks
```

Dieser Befehl zeigt den Namen der Migrationsdatei an, die generiert wird. Suchen Sie unter _database/migrations_ nach dieser Datei, und öffnen Sie sie.

Ersetzen Sie die `up`-Methode durch den folgenden Code:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

Mit dem obigen Code wird der Tabelle `tasks` die boolesche Spalte `complete` hinzugefügt.

Ersetzen Sie die `down`-Methode durch den folgenden Code für die Rollbackaktion:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

Führen Sie im Terminal Laravel-Datenbankmigrationen aus, um die Änderung in der lokalen Datenbank vorzunehmen.

```bash
php artisan migrate
```

Das Modell `Task` (siehe _app/Task.php_) wird basierend auf der [Laravel-Namenskonvention](https://laravel.com/docs/5.4/eloquent#defining-models) standardmäßig der Tabelle `tasks` zugeordnet.

### <a name="update-application-logic"></a>Aktualisieren der Anwendungslogik

Öffnen Sie die Datei *routes/web.php*. Die Anwendung definiert hier die Routen und Geschäftslogik.

Fügen Sie mit folgendem Code am Ende der Datei eine Route hinzu:

```php
/**
 * Toggle Task completeness
 */
Route::post('/task/{id}', function ($id) {
    error_log('INFO: post /task/'.$id);
    $task = Task::findOrFail($id);

    $task->complete = !$task->complete;
    $task->save();

    return redirect('/');
});
```

Der obige Code führt eine einfache Aktualisierung des Datenmodells durch, indem der Wert von `complete` geändert wird.

### <a name="update-the-view"></a>Aktualisieren der Ansicht

Öffnen Sie die Datei *resources/views/tasks.blade.php*. Suchen Sie das öffnende Tag `<tr>`, und ersetzen Sie es durch Folgendes:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

Mit dem obigen Code wird die Zeilenfarbe abhängig davon geändert, ob die Aufgabe abgeschlossen ist.

Die nächste Zeile enthält folgenden Code:

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

Ersetzen Sie die gesamte Zeile durch den folgenden Code:

```html
<td>
    <form action="{{ url('task/'.$task->id) }}" method="POST">
        {{ csrf_field() }}

        <button type="submit" class="btn btn-xs">
            <i class="fa {{$task->complete ? 'fa-check-square-o' : 'fa-square-o'}}"></i>
        </button>
        {{ $task->name }}
    </form>
</td>
```

Der obige Code fügt die Schaltfläche zum Übermitteln hinzu, die auf die zuvor definierte Route verweist.

### <a name="test-the-changes-locally"></a>Lokales Testen der Änderungen

Führen Sie den Development Server aus dem Stammverzeichnis des Git-Repositorys aus.

```bash
php artisan serve
```

Navigieren Sie zu `http://localhost:8000`, um die Änderung des Aufgabenstatus anzuzeigen, und aktivieren Sie das Kontrollkästchen.

![Hinzugefügtes Kontrollkästchen in der Aufgabe](./media/tutorial-php-mysql-app/complete-checkbox.png)

Geben Sie zum Beenden von PHP im Terminal `Ctrl + C` ein.

### <a name="publish-changes-to-azure"></a>Veröffentlichen von Änderungen in Azure

Führen Sie im Terminal Laravel-Datenbankmigrationen mit der Produktions-Verbindungszeichenfolge aus, um die Änderung in der Azure-Datenbank vorzunehmen.

```bash
php artisan migrate --env=production --force
```

Führen Sie für alle Änderungen in Git einen Commit aus, und übertragen Sie dann die Codeänderungen per Pushvorgang an Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Wechseln Sie nach Abschluss des `git push`-Vorgangs zur Azure-App, und testen Sie die neuen Funktionen.

![Auf Azure veröffentlichte Änderungen an Modell und Datenbank](media/tutorial-php-mysql-app/complete-checkbox-published.png)

Wenn Sie Aufgaben hinzugefügt haben, werden sie in der Datenbank beibehalten. Bei Updates des Datenschemas bleiben vorhandene Daten erhalten.

## <a name="stream-diagnostic-logs"></a>Streamen von Diagnoseprotokollen

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="manage-the-azure-app"></a>Verwalten der Azure-App

Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um die erstellte App zu verwalten.

Klicken Sie im linken Menü auf **App Services** und anschließend auf den Namen Ihrer Azure-App.

![Portalnavigation zur Azure-App](./media/tutorial-php-mysql-app/access-portal.png)

Die Übersichtsseite Ihrer App wird angezeigt. Hier können Sie einfache Verwaltungsaufgaben wie Beenden, Starten, Neustarten, Durchsuchen und Löschen durchführen.

Im linken Menü werden Seiten für die Konfiguration Ihrer App angezeigt.

![App Service-Seite im Azure-Portal](./media/tutorial-php-mysql-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer MySQL-Datenbank in Azure
> * Verbinden einer PHP-App mit MySQL
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Verwalten der App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"]
> [Tutorial: Zuordnen eines benutzerdefinierten DNS-Namens zu Ihrer App](../app-service-web-tutorial-custom-domain.md)

Oder sehen Sie sich weitere Ressourcen an:

> [!div class="nextstepaction"]
> [Konfigurieren einer PHP-App](configure-language-php.md)