---
title: 'Tutorial: Python-Web-App (Django) mit PostgreSQL unter Linux: Azure App Service'
description: Hier wird beschrieben, wie Sie eine datengesteuerte Python-Web-App (Django) in Azure mit Verbindung mit einer PostgreSQL-Datenbank ausführen.
services: app-service\web
documentationcenter: python
author: cephalin
manager: gwallace
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: cephalin
ms.custom:
- mvc
- seodec18
- seo-python-october2019
ms.openlocfilehash: 34dbce2a6da2395bbc627a30d464a9817dfe76fe
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72439431"
---
# <a name="tutorial-build-a-python-django-web-app-with-postgresql-in-azure-app-service"></a>Tutorial: Erstellen einer Python-Web-App (Django) mit PostgreSQL in Azure App Service

[App Service unter Linux](app-service-linux-intro.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching. In diesem Tutorial wird erläutert, wie Sie eine datengesteuerte Python-Web-App (Django) mit PostgreSQL als Datenbank-Back-End erstellen. Wenn Sie fertig sind, wird in Azure App Service für Linux eine Django-Webanwendung ausgeführt.

![Python-Django-App in Azure App Service für Linux](./media/tutorial-python-postgresql-app/run-python-django-app-in-azure.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer PostgreSQL-Datenbank in Azure
> * Herstellen einer Verbindung zwischen einer Python-Web-App und PostgreSQL
> * Bereitstellen der Python-Web-App in Azure
> * Anzeigen von Diagnoseprotokollen
> * Verwalten der Python-Web-App im Azure-Portal

> [!NOTE]
> Überprüfen Sie, [welche Computegeneration in Ihrer Region verfügbar ist](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores), bevor Sie eine Azure Database for PostgreSQL-Instanz erstellen.

Sie können die Schritte in diesem Artikel auch unter macOS ausführen. Die Anweisungen für Linux und Windows sind in den meisten Fällen identisch, doch die Unterschiede werden in diesem Tutorial nicht beschrieben.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

1. [Installation von Git](https://git-scm.com/)
2. [Installieren Sie Python.](https://www.python.org/downloads/)
3. [Laden Sie PostgreSQL herunter, und führen Sie es aus.](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testen der lokalen PostgreSQL-Installation und Erstellen einer Datenbank

Führen Sie in einem Terminalfenster `psql` aus, um eine Verbindung mit Ihrem lokalen PostgreSQL-Server herzustellen.

```bash
sudo -u postgres psql postgres
```

Wenn eine Fehlermeldung ähnlich wie `unknown user: postgres` angezeigt wird, wird Ihre PostgreSQL-Installation möglicherweise mit Ihrem angemeldeten Benutzernamen konfiguriert. Versuchen Sie dann stattdessen den folgenden Befehl.

```bash
psql postgres
```

Wenn die Verbindung erfolgreich ist, wird die PostgreSQL-Datenbank ausgeführt. Stellen Sie andernfalls sicher, dass die lokale PostgreSQL-Datenbank gestartet wurde, indem Sie die Anweisungen für Ihr Betriebssystem unter [Downloads: PostgreSQL Core Distribution](https://www.postgresql.org/download/) befolgen.

Erstellen Sie eine Datenbank mit dem Namen *pollsdb*, und richten Sie einen separaten Datenbankbenutzer mit dem Namen *manager* mit dem Kennwort *supersecretpass* ein.

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

Geben Sie `\q` ein, um den PostgreSQL-Client zu beenden.

<a name="step2"></a>

## <a name="create-local-python-app"></a>Erstellen einer lokalen Python-App

In diesem Schritt richten Sie das lokale Python Django-Projekt ein.

### <a name="clone-the-sample-app"></a>Klonen der Beispiel-App

Öffnen Sie das Terminalfenster, und wechseln Sie mit `CD` in ein Arbeitsverzeichnis.

Führen Sie die folgenden Befehle aus, um das Beispielrepository zu klonen.

```bash
git clone https://github.com/Azure-Samples/djangoapp.git
cd djangoapp
```

Dieses Beispielrepository enthält eine [Django](https://www.djangoproject.com/)-Anwendung. Es ist die gleiche datengesteuerte App, die Sie erhalten, wenn Sie [in der Django-Dokumentation das Tutorial zu den ersten Schritten](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) durcharbeiten. In diesem Tutorial wird nicht die Funktionsweise von Django erläutert, sondern es wird beschrieben, wie Sie eine Django-Web-App (oder eine andere datengesteuerte Python-App) in Azure App Service bereitstellen und ausführen.

### <a name="configure-environment"></a>Konfigurieren der Umgebung

Erstellen Sie eine virtuelle Python-Umgebung, und verwenden Sie ein Skript, um die Einstellungen für die Datenbankverbindung festzulegen.

```bash
# Bash
python3 -m venv venv
source venv/bin/activate
source ./env.sh

# PowerShell
py -3 -m venv venv
venv\scripts\activate
.\env.ps1
```

Die in *env.sh* und *env.ps1* definierten Umgebungsvariablen werden in _azuresite/settings.py_ verwendet, um die Datenbankeinstellungen zu definieren.

### <a name="run-app-locally"></a>Lokales Ausführen der App

Installieren Sie die erforderlichen Pakete, [führen Sie Django-Migrationen durch](https://docs.djangoproject.com/en/2.1/topics/migrations/), und [erstellen Sie Administratorbenutzer](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#creating-an-admin-user).

```bash
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

Führen Sie den Django-Server aus, nachdem der Administratorbenutzer erstellt wurde.

```bash
python manage.py runserver
```

Wenn die Django-Web-App vollständig geladen wurde, sollte eine ähnliche Meldung wie diese angezeigt werden:

```bash
Performing system checks...

System check identified no issues (0 silenced).
October 26, 2018 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

Navigieren Sie in einem Browser zu `http://localhost:8000`. Die Meldung `No polls are available.` sollte angezeigt werden. 

Navigieren Sie zu `http://localhost:8000/admin`, und melden Sie sich mit dem Administratorbenutzer an, den Sie im letzten Schritt erstellt haben. Wählen Sie neben **Fragen** die Option **Hinzufügen** aus, und erstellen Sie eine Frage für eine Umfrage mit mehreren Auswahlmöglichkeiten.

![Lokales Ausführen einer Python-Django-App in App Services](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Navigieren Sie erneut zu `http://localhost:8000`, und sehen Sie sich die angezeigte Frage an.

Die Django-Beispielanwendung speichert Benutzerdaten in der Datenbank. Nachdem Sie eine Frage für eine Umfrage erfolgreich hinzugefügt haben, schreibt Ihre App Daten in die lokale PostgreSQL-Datenbank.

Sie können den Django-Server jederzeit beenden, indem Sie im Terminal STRG+C eingeben.

## <a name="create-a-production-postgresql-database"></a>Erstellen einer PostgreSQL-Produktionsdatenbank

In diesem Schritt erstellen Sie eine PostgreSQL-Datenbank in Azure. Wenn Ihre App in Azure bereitgestellt ist, nutzt sie diese Clouddatenbank.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen einer Azure-Datenbank für PostgreSQL-Server

Erstellen Sie mit dem Befehl [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) in Cloud Shell einen PostgreSQL-Server.

Ersetzen Sie im folgenden Beispielbefehl *\<postgresql-name>* durch einen eindeutigen Servernamen und *\<admin-username>* und *\<admin-password>* durch die gewünschten Benutzeranmeldeinformationen. Die Benutzeranmeldeinformationen gelten für das Konto des Datenbankadministrators. Der Servername dient als Teil Ihrer PostgreSQL-Endpunkts (`https://<postgresql-name>.postgres.database.azure.com`). Daher muss der Name auf allen Servern in Azure eindeutig sein.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql-name> --location "West Europe" --admin-user <admin-username> --admin-password <admin-password> --sku-name B_Gen4_1
```

Nach dem Erstellen der Azure-Datenbank für den PostgreSQL-Server zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "administratorLogin": "<admin-username>",
  "fullyQualifiedDomainName": "<postgresql-name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql-name>",
  "location": "westus",
  "name": "<postgresql-name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 1,
    "family": "Gen4",
    "name": "B_Gen4_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

> [!NOTE]
> Notieren Sie die Werte für \<admin-username> und \<admin-password> für die spätere Verwendung. Sie benötigen diese Angaben zur Anmeldung beim PostgreSQL-Server und den zugehörigen Datenbanken.

### <a name="create-firewall-rules-for-the-postgresql-server"></a>Erstellen von Firewallregeln für den PostgreSQL-Server

Führen Sie in Cloud Shell die folgenden Azure CLI-Befehle aus, um den Zugriff von Azure-Ressourcen auf die Datenbank zu ermöglichen.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql-name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Diese Einstellung ermöglicht Netzwerkverbindungen von allen IP-Adressen im Azure-Netzwerk. Versuchen Sie für den Produktivbetrieb möglichst restriktive Firewallregeln zu konfigurieren, indem Sie [nur die ausgehenden IP-Adressen zulassen, die von Ihrer App verwendet werden](../overview-inbound-outbound-ips.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).

Führen Sie den Befehl in Cloud Shell erneut aus, um den Zugriff vom lokalen Computer zuzulassen. Ersetzen Sie dabei *\<your-ip-address>* durch [Ihre lokale IPv4-IP-Adresse](https://www.whatsmyip.org/).

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql-name> --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Herstellen einer Verbindung zwischen der Python-App und der Produktionsdatenbank

In diesem Schritt verbinden Sie Ihre Django-Web-App mit dem von Ihnen erstellten Azure Database for PostgreSQL-Server.

### <a name="create-empty-database-and-user-access"></a>Erstellen einer leeren Datenbank und des Benutzerzugriffs

Stellen Sie in Cloud Shell eine Verbindung mit der Datenbank her, indem Sie den unten angezeigten Befehl ausführen. Wenn Sie zur Eingabe des Administratorkennworts aufgefordert werden, verwenden Sie das gleiche Kennwort, das Sie auch unter [Erstellen eines Azure Database for PostgreSQL-Servers](#create-an-azure-database-for-postgresql-server) angegeben haben.

```bash
psql -h <postgresql-name>.postgres.database.azure.com -U <admin-username>@<postgresql-name> postgres
```

Erstellen Sie die Datenbank und Benutzer auf dem Azure PostgreSQL-Server genauso wie auf Ihrem lokalen PostgreSQL-Server.

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

Geben Sie `\q` ein, um den PostgreSQL-Client zu beenden.

> [!NOTE]
> Es empfiehlt sich, Datenbankbenutzer mit eingeschränkten Berechtigungen für bestimmte Anwendungen zu erstellen, anstatt den Administratorbenutzer zu verwenden. In diesem Beispiel besitzt der Benutzer `manager` vollständige Berechtigungen _nur_ für die `pollsdb`-Datenbank.

### <a name="test-app-connectivity-to-production-database"></a>Testen der App-Konnektivität mit der Produktionsdatenbank

Ändern Sie im lokalen Terminalfenster die Datenbank-Umgebungsvariablen (die Sie weiter oben durch die Ausführung von *env.sh* bzw. *env.ps1* konfiguriert haben):

```bash
# Bash
export DBHOST="<postgresql-name>.postgres.database.azure.com"
export DBUSER="manager@<postgresql-name>"
export DBNAME="pollsdb"
export DBPASS="supersecretpass"

# PowerShell
$Env:DBHOST = "<postgresql-name>.postgres.database.azure.com"
$Env:DBUSER = "manager@<postgresql-name>"
$Env:DBNAME = "pollsdb"
$Env:DBPASS = "supersecretpass"
```

Führen Sie die Django-Migration in die Azure-Datenbank aus, und erstellen Sie einen Administratorbenutzer.

```bash
python manage.py migrate
python manage.py createsuperuser
```

Führen Sie den Django-Server aus, nachdem der Administratorbenutzer erstellt wurde.

```bash
python manage.py runserver
```

Navigieren Sie erneut zu `http://localhost:8000`. Die Meldung `No polls are available.` sollte erneut angezeigt werden. 

Navigieren Sie zu `http://localhost:8000/admin`, und melden Sie sich mit dem von Ihnen erstellten Administratorbenutzer an. Erstellen Sie dann wie zuvor eine Frage für eine Umfrage.

![Lokales Ausführen einer Python-Django-App in App Services](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Navigieren Sie erneut zu `http://localhost:8000`, und sehen Sie sich die angezeigte Frage an. Ihre App schreibt nun Daten in die Datenbank in Azure.

## <a name="deploy-to-azure"></a>Bereitstellen in Azure

In diesem Schritt stellen Sie die mit PostgreSQL verbundene Python-Anwendung in Azure App Service bereit.

### <a name="configure-repository"></a>Konfigurieren des Repositorys

Django überprüft den `HTTP_HOST`-Header in eingehenden Anforderungen. Damit Ihre Django-Web-App in App Service funktioniert, müssen Sie den vollqualifizierten Domänennamen der App den zulässigen Hosts hinzufügen. Öffnen Sie _azuresite/settings.py_, und suchen Sie nach der Einstellung `ALLOWED_HOSTS`. Ändern Sie die Zeile wie folgt:

```python
ALLOWED_HOSTS = [os.environ['WEBSITE_SITE_NAME'] + '.azurewebsites.net', '127.0.0.1'] if 'WEBSITE_SITE_NAME' in os.environ else []
```

Django unterstützt auch nicht die [Bereitstellung von statischen Dateien in der Produktion](https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/), sodass Sie dies manuell aktivieren müssen. Für dieses Tutorial verwenden Sie [WhiteNoise](https://whitenoise.evans.io/en/stable/). Das WhiteNoise-Paket ist bereits in _requirements.txt_ enthalten. Sie müssen lediglich Django für dessen Verwendung konfigurieren. 

Suchen Sie in _azuresite/settings.py_ nach der Einstellung `MIDDLEWARE`, und fügen Sie die Middleware `whitenoise.middleware.WhiteNoiseMiddleware` direkt unterhalb der Middleware `django.middleware.security.SecurityMiddleware` hinzu. Ihre `MIDDLEWARE`-Einstellung sollte wie folgt aussehen:

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    ...
]
```

Fügen Sie am Ende von _azuresite/settings.py_ die folgenden Zeilen hinzu.

```python
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```

Weitere Informationen zum Konfigurieren von WhiteNoise finden Sie in der [Dokumentation zu WhiteNoise](https://whitenoise.evans.io/en/stable/).

> [!IMPORTANT]
> Im Abschnitt mit den Datenbankeinstellungen wird bereits die bewährte Sicherheitsmethode verwendet, bei der Umgebungsvariablen genutzt werden. Alle Empfehlungen zu Bereitstellung finden Sie unter [Django Documentation: Deployment Checklist](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/) (Django-Dokumentation: Checkliste zur Bereitstellung).

Committen Sie Ihre Änderungen im Repository.

```bash
git commit -am "configure for App Service"
```

### <a name="configure-deployment-user"></a>Konfigurieren des Bereitstellungsbenutzers

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-app-service-plan"></a>App Service-Plan erstellen

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Erstellen einer Web-App

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Weiter oben in diesem Tutorial haben Sie die Umgebungsvariablen für die Verbindung mit der PostgreSQL-Datenbank definiert.

In App Service verwenden Sie den Befehl [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in Cloud Shell, um Umgebungsvariablen als _App-Einstellungen_ festzulegen.

Im folgenden Beispiel werden die Details der Datenbankverbindung als App-Einstellungen angeben. 

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group myResourceGroup --settings DBHOST="<postgresql-name>.postgres.database.azure.com" DBUSER="manager@<postgresql-name>" DBPASS="supersecretpass" DBNAME="pollsdb"
```

Informationen dazu, wie in Ihrem Code auf diese App-Einstellungen zugegriffen wird, finden Sie unter [Zugreifen auf Umgebungsvariablen](how-to-configure-python.md#access-environment-variables).

### <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 775 bytes | 0 bytes/s, done.
Total 7 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6520eeafcc'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Python deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
. 
. 
. 
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app-name>.scm.azurewebsites.net/<app-name>.git 
   06b6df4..6520eea  master -> master
```  

Der App Service-Bereitstellungsserver sieht _requirements.txt_ im Stammverzeichnis des Repositorys und führt die Python-Paketverwaltung automatisch nach `git push` aus.

### <a name="browse-to-the-azure-app"></a>Navigieren zur Azure-App

Navigieren Sie zur bereitgestellten App. Das Starten dauert einen Moment, da bei der ersten Anforderung der App der Container heruntergeladen und ausgeführt werden muss. Wenn bei der Seite ein Timeout auftritt oder eine Fehlermeldung angezeigt wird, warten Sie einige Minuten, und aktualisieren Sie die Seite.

```bash
http://<app-name>.azurewebsites.net
```

Die weiter oben erstellte Frage einer Umfrage sollte angezeigt werden. 

App Service erkennt ein Django-Projekt in Ihrem Repository, indem in jedem Unterverzeichnis nach _wsgi.py_ gesucht wird. Das Unterverzeichnis wird standardmäßig durch `manage.py startproject` erstellt. Wenn die Datei gefunden wird, wird die Django-Web-App geladen. Weitere Informationen dazu, wie App Service Python-Apps lädt, finden Sie unter [Konfigurieren Ihrer Python-App für Azure App Service unter Linux](how-to-configure-python.md).

Navigieren Sie zu `<app-name>.azurewebsites.net`, und melden Sie sich als der Administratorbenutzer an, den Sie erstellt haben. Wenn Sie möchten, können Sie versuchen, weitere Fragen für die Umfrage zu erstellen.

![Ausführen einer Python-Django-App in App Services in Azure](./media/tutorial-python-postgresql-app/run-python-django-app-in-azure.png)

**Glückwunsch!** Sie führen eine Python-Web-App (Django) in Azure App Service für Linux aus.

## <a name="stream-diagnostic-logs"></a>Streamen von Diagnoseprotokollen

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="manage-your-app-in-the-azure-portal"></a>Überwachen Ihrer App im Azure-Portal

Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um die erstellte App anzuzeigen.

Wählen Sie im linken Menü **App Services** und anschließend den Namen Ihrer Azure-App aus.

![Navigieren zur Python-Django-App im Azure-Portal](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

Standardmäßig wird im Portal die Seite **Übersicht** Ihrer App angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten links auf der Seite zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![Verwalten der Python-Django-App auf der Seite „Übersicht“ im Azure-Portal](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen einer PostgreSQL-Datenbank in Azure
> * Herstellen einer Verbindung zwischen einer Python-Web-App und PostgreSQL
> * Bereitstellen der Python-Web-App in Azure
> * Anzeigen von Diagnoseprotokollen
> * Verwalten der Python-Web-App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"]
> [Tutorial: Zuordnen eines benutzerdefinierten DNS-Namens zu Ihrer App](../app-service-web-tutorial-custom-domain.md)

Oder sehen Sie sich weitere Ressourcen an:

> [!div class="nextstepaction"]
> [Konfigurieren einer Python-App](how-to-configure-python.md)
