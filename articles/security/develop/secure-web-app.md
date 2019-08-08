---
title: Entwickeln einer sicheren Web-App | Microsoft-Dokumentation
description: Diese einfache Beispiel-App implementiert bewährte Sicherheitsmethoden, die den Sicherheitsstatus Ihrer Anwendung und Ihres Unternehmens bei der Entwicklung in Azure verbessern.
keywords: na
services: security
documentationcenter: na
author: isaiah-msft
manager: barbkess
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security
ms.subservice: security-develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: terrylan
ms.openlocfilehash: 0683c065285a6ddf8d966bbd3d22e88c39b34d5c
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2019
ms.locfileid: "68728800"
---
# <a name="develop-a-secure-web-app"></a>Entwickeln einer sicheren Web-App

Bei diesem Beispiel handelt es sich um eine einfache Python-Beispiel-App, die eine Webseite anzeigt, die Links zu Sicherheitsressourcen für die Entwicklung von Apps in Azure enthält. Diese App implementiert bewährte Sicherheitsmethoden, die den Sicherheitsstatus Ihrer Anwendung und Ihres Unternehmens bei der Entwicklung in Azure verbessern können.

Sie sollten die in diesem Artikel beschriebenen Schritte befolgen, um sicherzustellen, dass die Anwendungskomponenten ordnungsgemäß konfiguriert sind. Die Datenbank, die Azure App Service-Instanz, die Azure Key Vault-Instanz und die Azure Application Gateway-Instanz sind voneinander abhängig.

Die Bereitstellungsskripts richten die Infrastruktur ein. Nachdem Sie die Bereitstellungsskripts ausgeführt haben, müssen Sie einige manuelle Konfigurationen im Azure-Portal durchführen, um die Komponenten und Dienste zu verknüpfen.

Die Beispiel-App wurde für Anfänger konzipiert, die mit der Entwicklung von Anwendungen in Azure beginnen und Sicherheitsmaßnahmen in ihren Anwendungen implementieren möchten.

Bei der Entwicklung und Bereitstellung dieser App lernen Sie Folgendes:

- Erstellen einer Azure Key Vault-Instanz und Speichern und Abrufen von Geheimnissen
- Bereitstellen von Azure Database for PostgreSQL, Einrichten sicherer Kennwörter und Autorisieren des Zugriffs
- Ausführen eines Alpine Linux-Containers in Azure-Web-Apps für Linux und aktivieren verwalteter Identitäten für Azure-Ressourcen
- Erstellen und Konfigurieren einer Azure Application Gateway-Instanz mit einer Firewall, die das [Top 10 OWASP-Regelwerk](https://coreruleset.org/) anwendet
- Aktivieren der Verschlüsselung von Daten während der Übertragung und im Ruhezustand mithilfe von Azure-Diensten

Sobald Sie diese App entwickelt und bereitgestellt haben, haben Sie die folgende Beispiel-Web-App sowie die beschriebene Konfiguration und Sicherheitsmaßnahmen eingerichtet.

![Beispiel-Web-App](./media/secure-web-app/demo-app.png)

## <a name="architecture"></a>Architecture
Bei der App handelt es sich um eine n-schichtige Anwendung mit drei Schichten. Die Front-End-, Back-End- und Datenbankebenen mit integrierten Überwachungs- und Geheimnisverwaltungskomponenten werden in der folgenden Abbildung veranschaulicht:

![App-Architektur](./media/secure-web-app/architecture.png)

Die Architektur umfasst die folgenden Komponenten:

- [Azure Application Gateway:](https://docs.microsoft.com/azure/application-gateway/) Stellt das Gateway und die Firewall für die Anwendungsarchitektur zur Verfügung.
- [Azure-Web-Apps für Linux:](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro) Stellt die Containerruntime zum Ausführen der Python-App in einer Linux-Umgebung zur Verfügung.
- [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/). Speichert und verschlüsselt die Geheimnisse der App und verwaltet die Erstellung von Zugriffsrichtlinien.
- [Azure Database for PostgreSQL:](https://azure.microsoft.com/services/postgresql/) Speichert die Daten der App sicher.
- [Azure Security Center](https://docs.microsoft.com/azure/security-center/) und [Azure Application Insights:](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) Stellt Überwachung und Warnungen für die Ausführung der App zur Verfügung.

## <a name="threat-model"></a>Bedrohungsmodell
Die Bedrohungsmodellierung ist der Prozess, bei dem potenzielle Sicherheitsbedrohungen für Ihr Unternehmen und Ihre Anwendung identifiziert werden und dann sichergestellt wird, dass geeignete vorbeugende Maßnahmen wirksam sind.

In diesem Beispiel wird das [Microsoft Threat Modeling Tool](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) zum Implementieren der Bedrohungsmodellierung für die sichere Beispiel-App verwendet. Mithilfe von Diagrammen der Komponenten und Datenflüsse können Sie Probleme und Bedrohungen frühzeitig im Entwicklungsprozess identifizieren. Das spart Ihnen später Zeit und Geld.

Das Bedrohungsmodell für die App sieht wie folgt aus:

![Bedrohungsmodell](./media/secure-web-app/threat-model.png)

Im folgenden Screenshot werden einige Beispielbedrohungen und potenzielle Sicherheitsrisiken veranschaulicht, die das Threat Modeling Tool generiert. Das Bedrohungsmodell verschafft Ihnen einen Überblick über die freigestellte Angriffsfläche und fordert die Entwickler auf, sich Gedanken über die Problembehebung zu machen.

![Ausgabe des Bedrohungsmodells](./media/secure-web-app/threat-model-output.png)

Beispielsweise wird die SQL-Injektion in der vorangehenden Bedrohungsmodellausgabe mithilfe von Bereinigung der Benutzereingaben und mit gespeicherten Funktionen in Azure Database for PostgreSQL verhindert. Diese Risikominderung verhindert die willkürliche Ausführung von Abfragen, während Daten gelesen und geschrieben werden.

Entwickler verbessern die allgemeine Sicherheit des Systems, indem sie die Bedrohungen in der Ausgabe des Bedrohungsmodells verhindern.

## <a name="deployment"></a>Bereitstellung
Mithilfe der folgenden Optionen können Sie Linux in Azure App Service ausführen:

- Wählen Sie einen Container aus der Liste vorgefertigter Microsoft-Container in Azure aus, die mit unterstützenden Technologien erstellt wurden (Python, Ruby, PHP, Java, Node.js, .NET Core).
- Verwenden Sie einen benutzerdefinierten Container. Wählen Sie Ihre eigenen Containerregistrierungen als Quelle für das Image aus, und bauen Sie auf den vielen verfügbaren Technologien auf, die HTTP unterstützen.

In diesem Beispiel führen Sie das Bereitstellungsskript aus, das die Web-App in App Service bereitstellt, und erstellen die Ressourcen.

Die App kann die folgenden verschiedenen Bereitstellungsmodelle verwenden:

![Datenflussdiagramm für die Bereitstellung](./media/secure-web-app/deployment.png)

Es gibt viele Methoden für die Bereitstellung von Apps in Azure, einschließlich der folgenden:

- Azure-Ressourcen-Manager-Vorlagen
- PowerShell
- Azure-Befehlszeilenschnittstelle
- Azure-Portal
- Azure DevOps

Für diese Anwendung wurde Folgendes verwendet:

- [Docker](https://docs.docker.com/), um die Containerimages zu erstellen
- Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) für die Bereitstellung
- [Docker Hub](https://hub.docker.com/) als Containerregistrierung

## <a name="security-considerations"></a>Sicherheitshinweise

### <a name="network"></a>Netzwerk
Die Beispiel-App verwendet End-to-End-SSL-Verschlüsselung während der Übertragung für die Daten, die in und aus dem Netzwerk fließen. Das Gateway wurde mit einem selbstsignierten Zertifikat konfiguriert.
> [!IMPORTANT]
> In dieser Demonstration wird ein selbstsigniertes Zertifikat verwendet. In einer Produktionsumgebung sollten Sie Zertifikate von einer verifizierten Zertifizierungsstelle verwenden.

Die Anwendungsfirewall überprüft auch den eingehenden Datenverkehr und warnt Administratoren, wenn bösartiger Datenverkehr im Netzwerkdatenverkehr erkannt wird.
Application Gateway verhindert mögliche DDoS-Attacken und SQL-Injektionen, die im Bedrohungsmodell erkannt werden.

### <a name="identity"></a>Identity
Die Beispiel-App verwendet die mehrstufige Authentifizierung für Azure Active Directory-Administratoren, denen der Zugriff auf die Ressourcen zugewiesen ist, für die Anmeldung beim Portal.
Die Beispiel-App verwendet verwaltete Identitäten, um die Berechtigungen zum Lesen und Abrufen von Geheimnissen aus Azure Key Vault abzurufen, wodurch sichergestellt wird, dass die App keine hartcodierten Anmeldeinformationen und Tokens zum Lesen von Geheimnissen benötigt. Azure AD erstellt die zum Lesen und Ändern der Geheimnisse erforderlichen Dienstprinzipale automatisch, wenn verwaltete Identitäten verwendet werden.

Verwaltete Identitäten für Azure-Ressourcen und die mehrstufige Authentifizierung erschweren Angreifern das Erlangen von Berechtigungen im System. Diese Bedrohung wurde im Bedrohungsmodell aufgezeigt.
Die App verwendet OAuth, was in der OAuth-App registrierten Benutzern die Anmeldung bei der App ermöglicht.

### <a name="storage"></a>Storage
Die Daten in der PostgreSQL-Datenbank werden im Ruhezustand automatisch von Azure Database for PostgreSQL verschlüsselt. Die Datenbank autorisiert die App Service-IP-Adressen, sodass nur die bereitgestellte App Service-Web-App mit den richtigen Authentifizierungsanmeldeinformationen auf die Datenbankressourcen zugreifen kann.

### <a name="logging-and-auditing"></a>Protokollierung und Überwachung
Die App implementiert die Protokollierung mithilfe von Application Insights, um Metriken, Protokolle und auftretende Ausnahmen nachzuverfolgen. Diese Protokollierung stellt ausreichend App-Metadaten bereit, um Entwickler und Betriebsteammitglieder über den Status der App zu informieren. Sie stellt außerdem ausreichend Daten für die Rückverfolgung bei Sicherheitsvorfällen bereit.

## <a name="cost-considerations"></a>Kostenbetrachtung
Wenn Sie über kein Azure-Konto verfügen, können Sie ein kostenloses Konto erstellen. Rufen Sie zunächst die [Seite zur Erstellung eines kostenlosen Kontos](https://azure.microsoft.com/free/) auf, und sehen Sie sich an, was Sie mit einem kostenlosen Azure-Konto tun können und welche Produkte 12 Monate lang kostenlos genutzt werden können.

Sie müssen für einige Premium-Features zahlen, die Sie zur Bereitstellung der Ressourcen für die Beispiel-App mit Sicherheitsfeatures benötigen. Mit der Skalierung der App müssen die kostenlosen Tarife und Testversionen von Azure aktualisiert werden, um die Anwendungsanforderungen zu erfüllen. Dadurch können die Kosten steigen. Verwenden Sie den [Azure-Preisrechner](https://azure.microsoft.com/pricing/calculator/), um Ihre Kosten zu ermitteln.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung
### <a name="prerequisites"></a>Voraussetzungen
Sie müssen die folgenden Tools installieren, um die Anwendung ausführen zu können:

- Ein Code-Editor zum Ändern und Anzeigen des Anwendungscodes. [Visual Studio Code](https://code.visualstudio.com/) bietet sich hier als Open-Source-Option an.
- Die [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&viewFallbackFrom=azure-cli-latest,) auf Ihrem Entwicklungscomputer.
- [Git](https://git-scm.com/) muss auf Ihrem System installiert sein. Git wird verwendet, um den Quellcode lokal zu klonen.
- Sie benötigen auch [jq](https://stedolan.github.io/jq/). Dies ist ein benutzerfreundliches UNIX-Tool für das Abfragen von JSON.

Außerdem benötigen Sie ein Azure-Abonnement für die Bereitstellung der Ressourcen der Beispiel-App. Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/), um die Beispiel-App zu testen.

Sobald Sie diese Tools installiert haben, sind Sie bereit, die App in Azure bereitzustellen.

### <a name="environment-setup"></a>Einrichten der Umgebung
Führen Sie die Bereitstellungsskripts aus, um die Umgebung und das Abonnement einzurichten:

1. Verwenden Sie den folgenden Git-Befehl, um das Quellcoderepository zu klonen:

   ``` git
   git clone https://github.com/Azure-Samples/sample-linux-python-app tutorial-project
   ```
2. Verwenden Sie den folgenden Befehl, um das Verzeichnis aufzurufen:

   ```
   cd tutorial-project/scripts
   ```

3. Im Skriptordner befinden sich Dateien, die spezifisch für Ihre Plattform sind (Windows oder Linux). Da die Azure CLI bereits installiert ist, melden Sie sich mithilfe des folgenden Befehls über die Befehlszeilenschnittstelle mit Ihrem Azure-Konto an:

   ``` azurecli
   az login
   ```

Daraufhin wird der Browser geöffnet. Melden Sie sich mit Ihren Anmeldeinformationen an. Nachdem Sie sich angemeldet haben, können Sie beginnen, Ressourcen über die Eingabeaufforderung bereitzustellen.

Die Bereitstellungsskripts `deploy-powershell.ps1` und `deploy-bash.sh` enthalten Code, der die gesamte Anwendung bereitstellt.
So stellen Sie die Projektmappe bereit:

1. Wenn Sie PowerShell verwenden, führen Sie die `deploy-powershell.ps1`-Datei aus, indem Sie `./deploy-powershell.ps1 REGION RESOURCE_GROUP_NAME` eingeben und dabei die Region und den Ressourcengruppenname durch die entsprechende Azure-Region und einen Namen für die Ressourcengruppe ersetzen.
2. Wenn Sie Linux verwenden, führen Sie die `deploy-bash.sh`-Datei aus, indem Sie `/deploy-bash.sh REGION RESOURCE_GROUP_NAME` eingeben. Möglicherweise müssen Sie die Datei ausführbar machen, indem Sie `chmod +x deploy-bash.sh` eingeben.

In den folgenden Beispielen werden Codeausschnitte aus den Schlüsselkomponenten veranschaulicht. Sie können die Beispiele individuell oder mit den restlichen Komponenten bereitstellen, indem Sie die Bereitstellungsdateien ausführen.

### <a name="implementation-guidance"></a>Implementierungsleitfaden
Das Bereitstellungsskript kann in vier Phasen unterteilt werden. In jeder Phase wird eine Azure-Ressource bereitgestellt und konfiguriert, die im [Architekturdiagramm](#architecture) veranschaulicht wird.

Die vier Phasen lauten wie folgt:

- Bereitstellen von Azure Key Vault
- Bereitstellen von Azure Database for PostgreSQL
- Bereitstellen von Azure-Web-Apps für Linux
- Bereitstellen von Application Gateway mit Web Application Firewall

Jede Phase baut auf der vorherigen auf, indem die Konfiguration der zuvor bereitgestellten Ressourcen verwendet wird.

Stellen Sie sicher, dass Sie die unter [Voraussetzungen](#prerequisites) aufgeführten Tools installiert haben, um diese Implementierungsschritte durchführen zu können.

#### <a name="deploy-azure-key-vault"></a>Bereitstellen von Azure Key Vault
In diesem Abschnitt erstellen Sie eine Azure Key Vault-Instanz, die zum Speichen von Geheimnissen und Zertifikaten verwendet wird, und stellen diese bereit.

Sobald die Bereitstellung abgeschlossen ist, verfügen Sie über eine in Azure bereitgestellte Azure Key Vault-Instanz.

So stellen Sie Azure Key Vault mithilfe der Azure CLI bereit:

1. Deklarieren Sie die Variablen für Azure Key Vault.
2. Registrieren Sie den Azure Key Vault-Anbieter.
3. Erstellen Sie die Ressourcengruppe für die Instanz.
4. Erstellen Sie die Azure Key Vault-Instanz in der Ressourcengruppe, die Sie in Schritt 3 erstellt haben.

   ``` azurecli

    function Get-Hash() {
        return (New-Guid).Guid.Split('-')[4]
    }

   az provider register -n Microsoft.KeyVault

   # Create the Azure Key Vault instance
   Write-Host "Creating Azure Key Vault instance: $($kvName)"
   az keyvault create --name $kvName `
       --resource-group $ResourceGroup `
       --location $Location `
       --verbose

   # Generate usernames and passwords using system functions and environment variables
   Write-Host "Generating PostgreSQL username and password"
   $pgUsername = "$($env:Username)$(Get-Hash)"
   $pgPassword = (New-Guid).Guid

   # Set the username secret in the Azure Key Vault instance
   Write-Host "Setting PostgreSQL username in KeyVault"
   az keyvault secret set --vault-name $kvName `
       --name PGUSERNAME `
       --value $pgUsername `
       --verbose

   # Set the password secret in the Azure Key Vault instance
   Write-Host "Setting PostgreSQL password in KeyVault"
   az keyvault secret set --vault-name $kvName `
       --name PGPASSWORD `
       --value $pgPassword `
       --verbose

   ```
Die Verwendung verwalteter Identitäten für Azure-Ressourcen in Apps, die Azure Key Vault für den Zugriff auf Ressourcen verwenden, ist eine bewährte Methode. Ihr Sicherheitsstatus verbessert sich, wenn Zugriffsschlüssel für Azure Key Vault weder im Code noch in der Konfiguration gespeichert werden.

#### <a name="deploy-azure-database-for-postgresql"></a>Bereitstellen von Azure Database for PostgreSQL
Azure Database for PostgreSQL funktioniert wie folgt, zunächst wird der Datenbankserver erstellt und dann wird die Datenbank erstellt, in der das Schema und die Daten gespeichert werden.

Nach Abschluss der Bereitstellung verfügen Sie über einen PostgreSQL-Server und eine Datenbank in Azure.

So stellen Sie Azure Database for PostgreSQL mithilfe der Azure CLI bereit:

1. Öffnen Sie ein Terminal mit der Azure CLI und Ihrem Azure-Abonnement.
2. Generieren Sie eine sichere Kombination von Benutzername und Kennwort für den Zugriff auf die Datenbank. (Diese sollten für Apps, die sie verwenden, in Azure Key Vault gespeichert werden.)
3. Erstellen Sie die PostgreSQL-Serverinstanz.
4. Erstellen Sie eine Datenbank in der Serverinstanz, die Sie in Schritt 3 erstellt haben.
5. Führen Sie die PostgreSQL-Skripts in der PostgreSQL-Instanz aus.

Im folgenden Code werden die in Azure Key Vault gespeicherten Geheimnisse PGUSERNAME und PGPASSWORD aus dem obigen Schritt zur Bereitstellung von Azure Key Vault verwendet.

   ``` azurecli
   $pgUsername = $(az keyvault secret show --name PGUSERNAME --vault-name $kvName --query value) -replace '"',''
   $pgPassword = $(az keyvault secret show --name PGPASSWORD --vault-name $kvName --query value) -replace '"',''

   # Create an Azure Database for PostgreSQL server
   Write-Host "Creating the PostreSQL server: $($dbServer)"
   az  postgres server create -l $Location `
       --resource-group $ResourceGroup `
       --name $dbServer `
       --admin-user $pgUsername `
       --admin-password $pgPassword `
       --sku-name B_Gen5_1 `
       --verbose

   # Create a database in the server instance created above
   Write-Host "Creating the PostgreSQL database: $($dbName)"
   az postgres db create --resource-group $ResourceGroup `
       --server-name $dbServer `
       --name $dbName `
       --verbose

   # Retrieve the database information above
   $db = (az postgres server show --resource-group $ResourceGroup `
           --name $dbServer)

   $db = $db | ConvertFrom-Json

   # Get the Fully Qualified Domain Name (FQDN) of the database to use in
   # the database connection strings to be stored in Azure Key Vault
   Write-Host "Generating the PostgreSQL connection string"
   $PGCONNECTIONSTRING = "postgresql+psycopg2://${pgUsername}@$($db.fullyQualifiedDomainName):${pgPassword}@$($db.fullyQualifiedDomainName):$dbPort/$($dbName)?sslmode=$($dbSSLMode)"
   $PGCONNECTIONSTRING = $PGCONNECTIONSTRING + '"&"' + "sslrootcert=$($dbRootCertPath)"

   # Set the database connection string above into Azure Key Vault
   Write-Host "Setting the PostgreSQL connection string into KeyVault"
   az keyvault secret set --vault-name $kvName `
       --name PGCONNECTIONSTRING `
       --value $PGCONNECTIONSTRING `
       --verbose
   ```

Sobald Sie die Datenbank bereitgestellt haben, müssen Sie die zugehörigen Anmeldeinformationen und die Verbindungszeichenfolge in Azure Key Vault speichern.
Im Skriptordner befindet sich eine `functions.sql`-Datei, die den PL/pgSQL-Code enthält, mit dem gespeicherte Funktionen erstellt werden, wenn Sie ihn ausführen. Wenn Sie diese Datei ausführen, werden die Eingaben zur Einschränkung von SQL-Injektionen parametrisiert.

PostgreSQL ist mit einem Tool namens `psql` gebündelt, das zum Herstellen einer Verbindung mit der Datenbank verwendet wird. Zur Ausführung von `functions.sql` müssen Sie eine Verbindung mit der Azure Database for PostgreSQL-Instanz Ihres lokalen Computers herstellen und die Datei dort ausführen. Das psql-Tool wird im Rahmen der Standardinstallation von PostgreSQL auf jedem Betriebssystem installiert.
Weitere Informationen finden Sie in der [PostgreSQL-Dokumentation](https://www.postgresql.org/docs/9.3/app-psql.html).

Azure Cloud Shell enthält außerdem das `psql`-Tool. Sie können Cloud Shell direkt über das Azure-Portal verwenden, indem Sie auf das Cloud Shell-Symbol klicken.

Sie müssen die IP-Adresse in PostgreSQL autorisieren, um den Remotezugriff auf die PostgreSQL-Instanz zu ermöglichen.
Diesen Zugriff aktivieren Sie, indem Sie die Registerkarte **Verbindungssicherheit**  aufrufen, auf **Client-IP-Adresse hinzufügen** klicken und dann die neuen Einstellungen speichern.

![Autorisieren der Client-IP-Adresse](./media/secure-web-app/add-client-ip-postgres.png)

Wenn Sie Cloud Shell anstelle eines lokalen psql-Tools verwenden, wählen Sie **Zugriff auf Azure-Dienste erlauben** aus und ändern den Wert in **EIN**, um den Cloud Shell-Zugriff zuzulassen.

Stellen Sie dann eine Verbindung mit der Instanz her, indem Sie den folgenden psql-Befehl mit Verbindungszeichenfolgenparametern aus der Registerkarte **Verbindungszeichenfolgen** der PostgreSQL-Instanz im Azure-Portal ausführen.
Ersetzen Sie die leeren Klammern durch Parameter aus dem Blatt „Verbindungszeichenfolge“ der Datenbank und das Kennwort durch das Kennwort aus Azure Key Vault.

```sql
psql "host={} port=5432 dbname=hellodb user={} password=PGPASSWORD sslmode=require"
```

Führen Sie das folgende PL/pgSQL-Skript aus, nachdem Sie sichergestellt haben, dass Sie mit der Datenbank verbunden sind. Das Skript erstellt die gespeicherten Funktionen zum Einfügen von Daten in die Datenbank.

```sql
CREATE OR REPLACE FUNCTION insert_visitor(country VARCHAR(40), browser VARCHAR(40), operating_system VARCHAR(40)) RETURNS void AS $$
BEGIN
    INSERT INTO visitor(
        country,
        browser,
        operating_system,
        date_visited)
    VALUES (
        country,
        browser,
        operating_system,
        NOW()
    );
END;
$$ LANGUAGE PLPGSQL;


CREATE OR REPLACE FUNCTION insert_azure_document(title VARCHAR(40), url VARCHAR(100), category VARCHAR(40)) RETURNS void AS $$
BEGIN
    INSERT INTO azure_document(
        title,
        url,
        category)
    VALUES (
        title,
        url,
        category
    );
END;
$$ LANGUAGE PLPGSQL;
```


Weitere Informationen zum Einrichten der Überprüfung von SSL und den Zertifizierungsstellen für PostgreSQL finden Sie unter [Konfigurieren von SSL-Verbindungen in Azure Database for PostgreSQL](https://docs.microsoft.com/en-us/azure/postgresql/concepts-ssl-connection-security).

Im Container ist ein Stammzertifikat enthalten. Die Schritte zum Abrufen des Zertifikats lauten wie folgt:

1. Laden Sie die Zertifikatdatei von der [Zertifizierungsstelle](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt) herunter.
2. [Laden Sie OpenSSL auf Ihren Computer herunter, und installieren Sie die Software](https://docs.microsoft.com/en-us/azure/postgresql/concepts-ssl-connection-security).
3. Decodieren Sie die Zertifikatdatei:

   ```powershell
   openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
   ```

Weitere Informationen zum Konfigurieren der SSL-Sicherheit für PostgreSQL finden Sie unter [Konfigurieren von SSL-Verbindungen in Azure Database for PostgreSQL](https://docs.microsoft.com/en-gb/azure/postgresql/concepts-ssl-connection-security).

#### <a name="deploy-azure-web-apps-on-linux"></a>Bereitstellen von Azure-Web-Apps für Linux
Sie können Linux-Dienste ganz einfach auf Azure App Service aufbauen, da Azure vorgefertigte Container und Images für häufig verwendete Sprachen wie Python, Ruby, C# und Java bereitstellt. Azure unterstützt außerdem benutzerdefinierte Container, die praktisch alle Programmiersprachen auf der Azure App Service-Plattform zulassen.

Bei der App, die bereitgestellt wird, handelt es sich um eine einfache Python-App, die in der neuesten Ubuntu Linux-Verteilung ausgeführt wird. Sie stellt eine Verbindung mit den Azure Key Vault- und PostgreSQL-Instanzen her, die in den vorherigen Abschnitten jeweils für die Verwaltung von Anmeldeinformationen und Datenspeicherung erstellt wurden.

Die folgende Dockerfile wird im Stammordner der App bereitgestellt:

``` docker
# Docker file for the basic web app
# Using the latest Alpine Linux

FROM alpine:latest

# Copy requirements to the container
COPY requirements.txt /tmp/requirements.txt

# Install Python and PostgreSQL dependencies
RUN apk update && \
    apk add --update bash gcc libffi-dev musl-dev  postgresql-dev python3 python3-dev && \
    rm -r /usr/lib/python*/ensurepip && \
    pip3 install --upgrade pip setuptools && \
    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
    if [[ ! -e /usr/bin/python ]]; then ln -sf /usr/bin/python3 /usr/bin/python; fi && \
    pip3 install --no-cache-dir -r /tmp/requirements.txt && \
    rm -rf /.wh /root/.cache /var/cache /tmp/requirements.txt

# Change the working directory inside the container to /hello
WORKDIR /hello

# Copy the application code into the container
COPY . /hello

# Set the FLASK_APP environment variable used by flask migrate
ENV FLASK_APP=app.py

# Copy the init script to the container
COPY init.sh /usr/local/bin/

# Make the init script executable
RUN  chmod u+x /usr/local/bin/init.sh

# Expose the container web service endpoint
EXPOSE 8000

# Run the app with a non root user
RUN addgroup -g 1000 -S appgroup && \
    adduser  -u 1000 -S appuser -G appgroup

# Allow the non root user to access the folder
RUN chown -R appuser:appgroup /hello

# Switch to the non root user
USER appuser

# Set the init script as the file to be run during container startups
ENTRYPOINT ["/usr/local/bin/init.sh"]
```

Die obige Dockerfile wird zum Erstellen des Containers verwendet, der in Azure Container Registry unter `mcr.microsoft.com/samples/basic-linux-app` gehostet wird.

Der folgende Code:

1. deklariert die Variablen und Namen für die App Service-Instanz.
2. erstellt die Ressourcengruppe für den App Service-Plan.
3. stellt eine Containerinstanz für Azure-Web-Apps für Linux bereit.
4. aktiviert die Protokollierung für den Web-App-Container.
5. legt einige App-Konfigurationen in den App-Einstellungen des Containers fest.

   ```
   Write-Host "Retrieving the Azure Key Vault URL"
   $kvURI = $(az keyvault show --name $kvName --query properties.vaultUri)

   # Create the App Service plan, using --linux for running containers on Web Apps on Linux
   Write-Host "Creating App Service Plan: $($appName)"
   az appservice plan create --name $appServicePlanName `
       --resource-group $ResourceGroup `
       --location $Location `
       --number-of-workers 1 `
       --sku B1 `
       --is-linux `
       --verbose

   # Create the web app
   Write-Host "Creating Azure Web App for Linux: $($appName)"
   az webapp create --name $appName `
       --resource-group $ResourceGroup `
       --plan $appServicePlanName `
       --deployment-container-image-name $containerName `
       --verbose

   # Assign a system-assigned identity
   # This creates a service principal to be used for managed identities for Azure resources allowing access to Key Vault Secrets without using auth keys/tokens
   Write-Host "Assigning Service Principal Identity to webapp: $($appName)"
   az webapp identity assign --name $appName `
       --resource-group $ResourceGroup `
       --verbose

   # Configure logging for the Docker container on App Service
   Write-Host "Configuring logging for the web app: $($appName)"
   az webapp log config --name $appName `
       --resource-group $ResourceGroup `
       --application-logging true `
       --detailed-error-messages true `
       --docker-container-logging filesystem `
       --verbose

   # Set app configuration settings to be set in the container's environment variables
   Write-Host "Setting app settings for our web app: $($appName)"
   az webapp config appsettings set --name $appName `
       --resource-group $ResourceGroup `
       --settings WEBSITE_TIME_ZONE=$timezone KEY_VAULT_URI=$kvURI `
       --verbose

   # Stop the web app to enable us to first allow the created managed identities for Azure resources service principals access to the Key Vault instance
   az webapp stop --name $appName `
       --resource-group $ResourceGroup `
       --verbose

   # Get all the outbound IPs the App Service instance might use
   Write-Host "Adding outbound Azure App Service IP's to the PostgreSQL database firewall."
   $outboundIps = (az webapp show --resource-group $ResourceGroup `
       --name $appName `
       --query outboundIpAddresses `
       --output tsv)

   # Loop over all the outbound IP addresses and authorize them in the PostgreSQL firewall
   $outboundIps = $outboundIps.Split(',')
   for($i=0; $i -lt $outboundIps.length; $i++) {
       Write-Output "Adding IP Rule $($outboundIps[$i]) on PostgreSQL for App Service"

       az postgres server firewall-rule create --name "OUTBOUND_IP_RULE$i" `
           --resource-group $ResourceGroup `
           --server-name $dbServer `
           --start-ip-address $outboundIps[$i] `
           --end-ip-address $outboundIps[$i] `
           --verbose
   }

   ```

Dieses Skript erstellt eine zugewiesene Identität für die App Service-Instanz, die mit MSI für die Interaktion mit Azure Key Vault verwendet werden kann, ohne eine Hartcodierung für Geheimnisse im Code oder in der Konfiguration vorzunehmen.

Navigieren Sie im Portal zur Azure Key Vault-Instanz, um die zugewiesene Identität auf der Registerkarte „Zugriffsrichtlinie“ zu autorisieren. Klicken Sie auf **Neue Zugriffsrichtlinie hinzufügen**. Suchen Sie unter **Prinzipal auswählen** nach dem Anwendungsnamen, der dem Namen der erstellten App Service-Instanz ähnelt.
Sie sollten einen Dienstprinzipal sehen können, der der Anwendung angefügt ist. Wählen Sie ihn aus, und speichern Sie die Zugriffsrichtlinienseite wie im folgenden Screenshot gezeigt.

Da die Anwendung nur Schlüssel abrufen muss, wählen Sie die **Get**-Berechtigung in den Geheimnisoptionen aus, um den Zugriff zu gewähren und die gewährten Berechtigungen zu verringern.

![Schlüsseltresor-Zugriffsrichtlinie](./media/secure-web-app/kv-access-policy.png)

*Erstellen einer Schlüsseltresor-Zugriffsrichtlinie*

Speichern Sie die Zugriffsrichtlinie, und speichern Sie dann die neue Änderung auf der Registerkarte **Zugriffsrichtlinien**, um die Richtlinien zu aktualisieren.

#### <a name="deploy-application-gateway-with-web-application-firewall-enabled"></a>Bereitstellen von Application Gateway mit aktivierter Web Application Firewall
Es wird davon abgeraten, Dienste in Web-Apps direkt für Außenstehende im Internet zur Verfügung zu stellten.
Lastenausgleiche und Firewallregeln stellen mehr Sicherheit und Kontrolle über eingehenden Datenverkehr bereit und helfen Ihnen bei der Verwaltung.

So stellen Sie eine Application Gateway-Instanz bereit:

1. Erstellen Sie die Ressourcengruppe, die die Application Gateway-Instanz enthalten soll.
2. Stellen Sie ei virtuelles Netzwerk bereit, das an das Gateway angefügt wird.
3. Erstellen Sie ein Subnetz für das Gateway im virtuellen Netzwerk.
4. Stellen Sie eine öffentliche IP-Adresse bereit.
5. Stellen Sie das Anwendungsgateway bereit.
6. Aktivieren Sie die Web Application Firewall für das Gateway.

   ``` azurecli
   az keyvault certificate create --vault-name $kvName `
       --name $certName `
       --policy `@policy.json `
       --verbose

   az keyvault secret download --file $filePath `
       --encoding base64 `
       --name $certName `
       --vault-name $kvName `
       --verbose

   $pfxFile = Get-PfxData -FilePath $filePath

   $certPassword = Get-Random

   az keyvault secret set --vault-name $kvName `
       --name CERTPASSWORD `
       --value $certPassword `
       --verbose

   $signPassword = ConvertTo-SecureString $certPassword -Force -AsPlainText
   Export-PfxCertificate -PFXData $pfxFile -FilePath $certPath -Password $signPassword
   ```

Das vorherige Skript:

1. erstellt ein neues selbstsigniertes Stammzertifikat in Azure.
2. lädt das selbstsignierte Zertifikat als Base64-codierte Datei herunter.
3. generiert ein Kennwort für das selbstsignierte Zertifikat.
4. exportiert das Zertifikat als PFX-Datei, die mit dem Kennwort signiert ist.
5. speichert das Kennwort des Zertifikats in Azure Key Vault.

Der folgende Abschnitt stellt das Anwendungsgateway bereit:

```powershell
# Create a virtual network required by the gateway
Write-Host "Creating the Azure Virtual Network: $($vnetName)"
az network vnet create --name $vnetName `
    --resource-group $ResourceGroup `
    --location $Location `
    --address-prefix $vnetAddressPrefix `
    --verbose

# Add a subnet to the virtual network
Write-Host "Creating the Subnet: $($gwSubnet)"
az network vnet subnet create --name $gwSubnet `
    --resource-group $ResourceGroup `
    --vnet-name $vnetName `
    --address-prefix $gatewayAddressPrefix `
    --verbose

# Create a public IP address that will be used by clients to access the application gateway
Write-Host "Creating the Public IP Address: $($publicIpName)"
az network public-ip create --resource-group $ResourceGroup `
    --name $publicIpName `
    --verbose

# Create the application gateway
Write-Host "Creating the Application Gateway: $($gwName)"
az network application-gateway create `
    --name $gwName `
    --resource-group $ResourceGroup `
    --location $Location `
    --vnet-name $vnetName `
    --subnet $gwSubnet `
    --public-ip-address $publicIpName `
    --http-settings-cookie-based-affinity Disabled `
    --frontend-port 443 `
    --http-settings-protocol Https `
    --http-settings-port 443 `
    --capacity 2 `
    --sku WAF_Medium `
    --cert-file $certPath `
    --cert-password $certPassword `
    --verbose

# Enable the firewall with OWASP Ruleset 3.0 on the application gateway
Write-Host "Creating the Application Gateway WAF Configuration"
az network application-gateway waf-config set `
    --enabled true `
    --gateway-name $gwName `
    --resource-group $ResourceGroup `
    --firewall-mode Detection `
    --rule-set-version 3.0 `
    --verbose

# Retrieve the name of the HTTP settings that will be updated below
$gwHTTPSettings = $(az network application-gateway http-settings list --resource-group $ResourceGroup `
    --gateway-name $gwName)

$gwHTTPSettings = $gwHTTPSettings | ConvertFrom-Json
$gwHTTPSettingsName = $gwHTTPSettings.name

# Retrieve the name of the backend address pool that will be updated below
$gwAddressPool = $(az network application-gateway address-pool list --resource-group $ResourceGroup `
    --gateway-name $gwName)

$gwAddressPool = $gwAddressPool | ConvertFrom-Json
$gwAddressPoolName = $gwAddressPool.name

# Update the backend pool with the App Service host name
Write-Host "Updating the Azure Application Gateway backend pool host name: $($appHostName)"
az network application-gateway address-pool update --name $gwAddressPoolName `
    --resource-group $ResourceGroup `
    --gateway-name $gwName `
    --servers $appHostName `
    --verbose

# Create a probe that will check the backend pool's availability
Write-Host "Updating the Azure Application Gateway Probe: $($gwProbe)"
az network application-gateway probe create --gateway-name $gwName `
    --name $gwProbe `
    --resource-group $ResourceGroup `
    --protocol Https `
    --path $gwProbePath `
    --host-name-from-http-settings true `
    --verbose

# Update the app to user https and to pick the host name from the backend settings
Write-Host "Deploying the updated application gateway"
az network application-gateway http-settings update --gateway-name $gwName `
    --resource-group $ResourceGroup `
    --name $gwHTTPSettingsName `
    --connection-draining-timeout 0 `
    --enable-probe true `
    --host-name-from-backend-pool true `
    --probe $gwProbe `
    --protocol Https `
    --port 443 `
    --verbose
```

Nach Abschluss der Bereitstellung verfügen Sie über ein Anwendungsgateway mit aktivierter Web Application Firewall.

Die Application Gateway-Instanz stellt den Port 443 für HTTPS zur Verfügung. Mit dieser Konfiguration wird sichergestellt, dass die App nur über Port 443 per HTTPS zugänglich ist.

Das Sperren nicht verwendeter Ports und Einschränken der Angriffsfläche stellt eine bewährte Sicherheitsmaßnahme dar.

#### <a name="add-network-security-groups-to-the-app-service-instance"></a>Hinzufügen von Netzwerksicherheitsgruppen zur App Service-Instanz

App Service-Instanzen können in virtuelle Netzwerke integriert werden. Diese Integration ermöglicht die Konfiguration mit Richtlinien für Netzwerksicherheitsgruppen, die den eingehenden und ausgehenden Datenverkehr der App verwalten.

1. Klicken Sie auf dem Blatt der Azure App Service-Instanz unter **Einstellungen** auf **Netzwerk**, um dieses Feature zu aktivieren. Klicken Sie im rechten Bereich unter **VNET-Integration** auf **Zum Konfigurieren hier klicken**.

   ![Neue VNET-Integration](./media/secure-web-app/app-vnet-menu.png)

    *Neue VNET-Integration für App Service*
1. Klicken Sie auf der nächsten Seite auf **VNET hinzufügen (Vorschau)** .

1. Wählen Sie im nächsten Menü das in der Bereitstellung erstellte virtuelle Netzwerk aus, das mit `hello-vnet` beginnt. Sie können entweder ein neues Subnetz erstellen oder ein vorhandenes auswählen.
   Erstellen Sie in diesem Fall ein neues Subnetz. Legen Sie den **Adressbereich** auf **10.0.3.0/24** fest, und nennen Sie das Subnetz **app-subnet**.

   ![Konfiguration des virtuellen App Service-Netzwerks](./media/secure-web-app/app-vnet-config.png)

    *Konfiguration des virtuellen Netzwerks für App Service*

Da Sie die VNET-Integration nun aktiviert haben, können Sie die Netzwerksicherheitsgruppen zur App hinzufügen.

1. Suchen Sie mithilfe des Suchfelds nach **Netzwerksicherheitsgruppen**. Wählen Sie **Netzwerksicherheitsgruppen** aus den Suchergebnissen aus.

    ![Netzwerksicherheitsgruppen suchen](./media/secure-web-app/nsg-search-menu.png)

    *Suche nach Netzwerksicherheitsgruppen*

2. Klicken Sie im nächsten Menü auf **Hinzufügen**. Geben Sie die **Namen** der Netzwerksicherheitsgruppe und der **Ressourcengruppe** ein, in der sie enthalten ist. Diese Netzwerksicherheitsgruppe wird auf das Subnetz des Anwendungsgateways angewendet.

    ![Erstellen einer NSG](./media/secure-web-app/nsg-create-new.png)

    *Erstellen einer Netzwerksicherheitsgruppe*

3. Nachdem die Netzwerksicherheitsgruppe erstellt wurde, wählen Sie sie aus. Klicken Sie auf dem Blatt unter **Einstellungen** auf **Eingangssicherheitsregeln**. Konfigurieren Sie diese Einstellungen, um eingehende Verbindungen mit dem Anwendungsgateway über Port 443 zuzulassen.

   ![Konfigurieren der Netzwerksicherheitsgruppe](./media/secure-web-app/nsg-gateway-config.png)

   *Konfigurieren der Netzwerksicherheitsgruppe*

4. Fügen Sie in den Ausgangsregeln für die Netzwerksicherheitsgruppe des Gateways eine Regel hinzu, die ausgehende Verbindungen mit der App Service-Instanz zulässt, indem Sie eine Regel für den Diensttag `AppService` erstellen:

   ![Hinzufügen von Ausgangsregeln für die Netzwerksicherheitsgruppe](./media/secure-web-app/nsg-outbound-allowappserviceout.png)

   *Hinzufügen von Ausgangsregeln für die Netzwerksicherheitsgruppe*

    Fügen Sie eine weitere Ausgangsregel hinzu, um dem Gateway das Senden von Ausgangsregeln an ein virtuelles Netzwerk zu erlauben.

   ![Hinzufügen einer weiteren Ausgangsregel](./media/secure-web-app/nsg-outbound-vnet.png)

    *Hinzufügen einer weiteren Ausgangsregel*

5. Klicken Sie auf dem Subnetzblatt der Netzwerksicherheitsgruppe auf **Zuordnen**, und wählen Sie das in der Bereitstellung erstellte virtuelle Netzwerk und das Gatewaysubnetz namens **gw-subnet** aus. Die Netzwerksicherheitsgruppe wird auf das Subnetz angewendet.

6. Erstellen Sie eine weitere Netzwerksicherheitsgruppe wie in einem der vorherigen Schritte, dieses Mal für die App Service-Instanz. Geben Sie ihr einen Namen. Fügen Sie die Eingangsregel für Port 443 hinzu, wie Sie es bereits für die Netzwerksicherheitsgruppe für das Anwendungsgateway getan haben.

   Wenn Sie eine App Service-Instanz in einer App Service-Umgebung bereitgestellt haben, was bei dieser App nicht der Fall ist, können Sie Eingangsregeln hinzufügen, um Azure Service Health-Tests zuzulassen, indem Sie die Ports 454-455 in den Eingangssicherheitsgruppen Ihrer App Service-Netzwerksicherheitsgruppe öffnen. Die Konfiguration sieht wie folgt aus:

   ![Hinzufügen von Regeln für Azure Service Health-Tests](./media/secure-web-app/nsg-create-healthprobes.png)

    *Hinzufügen von Regeln für Azure Service Health-Tests (Nur für App Service-Umgebungen)*

7. Erstellen Sie eine neue Ausgangssicherheitsregel, die der App Service-Instanz die Kommunikation mit der PostgreSQL-Datenbank erlaubt. Konfigurieren Sie sie wie folgt:

   ![Regel zum Erlauben ausgehender PostgreSQL-Verbindungen](./media/secure-web-app/nsg-outbound-postgresql.png)

   *Hinzufügen einer Regel zum Erlauben ausgehender PostgreSQL-Verbindungen*

Ändern Sie die App Service-Netzwerkeinstellungen so, dass nur das Anwendungsgateway auf die Anwendung zugreifen kann, um die Angriffsfläche einzuschränken.
Öffnen Sie hierzu die App Service-Netzwerkregisterkarte, wählen Sie dann die Registerkarte **IP-Einschränkungen** aus, und erstellen Sie eine Zulassungsregel, die nur der IP-Adresse des Anwendungsgateways den Direktzugriff auf den Dienst erlaubt.

Sie können die IP-Adresse des Gateways auf der Übersichtsseite abrufen. Geben Sie die IP-Adresse auf der Registerkarte **IP Address CIDR (IP-Adressen-CIDR)** im folgenden Format ein: `<GATEWAY_IP_ADDRESS>/32`.

![Nur Zugriff des Gateways zulassen](./media/secure-web-app/app-allow-gw-only.png)

*Zulassen des Zugriffs auf App Service nur für die IP-Adresse des Gateways*


#### <a name="implement-azure-active-directory-oauth"></a>Implementieren von OAuth für Azure Active Directory

Bei den Azure-Dokumenten, die auf der Seite der Beispiel-Web-App verteilt werden, handelt es sich um Ressourcen der App, die möglicherweise geschützt werden müssen. Sie können Azure Active Directory zum Implementieren der Authentifizierung für Web-, Desktop- und Mobile Apps verwenden, indem Sie verschiedene Authentifizierungsflows anwenden.
Die App nutzt **Login With Microsoft** (Anmeldung mit Microsoft), wodurch der App das Lesen von Benutzerprofilen ermöglicht wird, die der Benutzerliste von Azure Active Directory für einzelne Mandanten hinzugefügt wurden.

Konfigurieren Sie die App im Azure-Portal für die Verwendung der erforderlichen Anmeldeinformationen:

1. Wählen Sie **Azure Active Directory** aus, oder suchen Sie mithilfe des Suchfelds danach.

2. Klicken Sie auf **Neue Registrierung**:

   ![Erstellen einer Registrierung](./media/secure-web-app/ad-auth-create.png)

   *Erstellen einer Azure AD-App-Registrierung*

3. Geben Sie auf der nächsten Seite den Namen der App ein. Wählen Sie unter **Unterstützte Kontotypen** die Option **Nur Konten in diesem Organisationsverzeichnis** aus.
    Geben Sie unter **Umleitungs-URI** die Basisdomäne, auf der die App ausgeführt wird, sowie den Tokenendpunkt ein. Beispiel:  *GATEWAY_HASH*.cloudapp.net/token.

   ![Konfigurieren der Azure AD-App-Registrierung](./media/secure-web-app/ad-auth-type.png)

   *Konfigurieren der Azure AD-App-Registrierung*

4. Ihnen wird eine Anzeige angezeigt, die die registrierte App und die zugehörigen Informationen zeigt. Diese Informationen müssen Sie zur Azure Key Vault-Instanz hinzufügen.
   1. Kopieren Sie die Anwendungs-ID (Client-ID), und speichern Sie sie in Key Vault als `CLIENTID`.
   2. Kopieren Sie den Umleitungs-URI, den Sie im vorherigen Schritt eingegeben haben, und speichern Sie ihn als `REDIRECTURI`.
   3. Kopieren Sie den Azure Active Directory-Standardverzeichnisnamen, der das Format „*name*.microsoftonline.com“ aufweist, und speichern Sie ihn in Key Vault als `TENANT`.
   4. Öffnen Sie die Registerkarte **Zertifikate & Geheimnisse** der Azure Active Directory-App, die Sie zuvor erstellt haben, und klicken Sie wie im folgenden Screenshot gezeigt auf **Neuer geheimer Clientschlüssel**. Legen Sie ein Ablaufdatum fest, und kopieren Sie dann den generierten Wert und speichern ihn in Key Vault als `CLIENTSECRET`.

      ![Azure Active Directory-Autorisierungsgeheimnis](./media/secure-web-app/ad-auth-secrets.png)

      *Azure Active Directory-Autorisierungsgeheimnis*

   5. Generieren Sie mithilfe eines beliebigen Befehlszeilen- oder Onlinetools einen sicheren, zufälligen geheimen Schlüssel. Speichern Sie diesen in Key Vault als `FLASKSECRETKEY`. Das Anwendungsframework verwendet diesen Schlüssel zum Erstellen von Sitzungen.
        Informationen zum Generieren eines geheimen Schlüssels finden Sie unter [Flask Sessions](http://flask.pocoo.org/docs/1.0/quickstart/#sessions).

5. Nachdem Sie die Anmeldedaten konfiguriert haben, müssen Sie Benutzer zur Azure AD-Verknüpfung hinzufügen, um ihnen die Anmeldung bei der Ressource zu ermöglichen. Öffnen Sie die Registerkarte **Benutzer** in Azure Active Directory, wählen Sie **Alle Benutzer** aus, und klicken Sie dann auf **Neuer Benutzer** oder **Neuer Gastbenutzer**, um sie hinzuzufügen. Für Testzwecke können Sie einen Gastbenutzer hinzufügen und den Benutzer in das Verzeichnis einladen. Alternativ können Sie einen neuen Benutzer hinzufügen, wenn die Domäne, in der die App ausgeführt wird, überprüft wurde. In diesem Beispiel können nur Benutzer für den Zugriff registriert werden, die im Azure AD-Mandanten registriert sind. Informationen über den Anmeldungszugriff mit mehreren Mandanten finden Sie in der Dokumentation.

   ![Hinzufügen von Benutzern zur Standarddomäne](./media/secure-web-app/ad-auth-add-user.png)

   *Hinzufügen von Benutzern zur Azure Active Directory-Standarddomäne*

Nachdem Sie die Azure AD-Konfiguration und die Geheimnisse zu Key Vault hinzugefügt haben, können Benutzer mithilfe der Azure OAuth-Authentifizierung authentifiziert werden.
Im App-Code wird dies von der Azure Active Directory-Authentifizierungsbibliothek (ADAL) verarbeitet.

Sobald sich die Geheimnisse in Key Vault befinden und die Anwendung über Zugriff auf die Geheimnisse und die Datenbank verfügt, ist der Anwendungsdienst über die Anwendungs-URL (https://GATEWAY_HASH.cloudapp.net) ) des Gateways erreichbar, die Sie über das entsprechende Blatt abrufen können.

Wenn Sie versuchen sich bei Azure Active Directory anmelden und die Fehlermeldung „User is not registered in the directory you're trying to log into“ (Der Benutzer ist nicht in dem Verzeichnis registriert, in der Sie versuchen sich anzumelden), müssen Sie den Benutzer hinzufügen. Öffnen Sie die Azure Active Directory-Registerkarte **Benutzer**, und fügen Sie den Benutzer manuell hinzu, indem Sie ihre Informationen eingeben. Alternativ können Sie den Benutzer einladen, indem Sie seine E-Mail-Adresse als Gastbenutzer im Blatt **Gast einladen** in Azure Active Directory eingeben.

#### <a name="deploy-application-insights"></a>Bereitstellen von Application Insights
Da die App jetzt bereitgestellt und funktional ist, müssen Sie in der App auftretende Fehler sowie die Protokollierung und die Erfassung von Ablaufverfolgungsdaten behandeln.
Die Protokollierung und Erfassung von Ablaufverfolgungsdaten verschafft Ihnen einen Überblick über Überwachungsereignisse in der App.

Application Insights ist ein Dienst, der Protokolle erfasst, die von Benutzern oder dem System generiert werden können.

So erstellen Sie eine Application Insights-Instanz:

1. Suchen Sie mithilfe des Suchfelds im Azure-Portal nach **Application Insights**.
2. Wählen Sie **Application Insights**. Geben Sie die hier gezeigten Informationen an, um eine Instanz zu erstellen.

   ![Erstellen einer Application Insights-Instanz](./media/secure-web-app/app-insights-data.png)

Sobald die Bereitstellung abgeschlossen ist, verfügen Sie über eine Application Insights-Instanz.

Nachdem Sie die Application Insights-Instanz erstellt haben, müssen Sie die App über den Instrumentierungsschlüssel in Kenntnis setzen, der das Senden von Protokollen an die Cloud ermöglicht. Hierzu rufen Sie den Application Insights-Schlüssel ab und verwenden ihn in den Anwendungsbibliotheken, die Azure für Application Insights bereitstellt. Die bewährte Methode besteht darin, Schlüssel und Geheimnisse in Azure Key Vault zu speichern, um die Sicherheit zu gewährleisten.

Nachdem Sie die Application Insights-Instanz für die einfache Beispiel-App erstellt haben, müssen Sie die App über den Instrumentierungsschlüssel in Kenntnis setzen, der das Senden von Protokollen an die Cloud ermöglicht.
Legen Sie ein `APPINSIGHTSKEY`-Geheimnis in Key Vault mit dem Instrumentierungsschlüssel als Wert fest. Dadurch kann die App Protokolle und Metriken an Application Insights senden.

#### <a name="implement-multi-factor-authentication-for-azure-active-directory"></a>Implementieren der mehrstufigen Authentifizierung für Azure Active Directory
Administratoren müssen sicherstellen, dass die Abonnementkonten im Portal geschützt sind. Das Abonnement ist für Angriffe anfällig, da es die von Ihnen erstellten Ressourcen verwaltet. Aktivieren Sie die mehrstufige Authentifizierung über die Registerkarte **Azure Active Directory** des Abonnements, um das Abonnement zu schützen.

Azure Active Directory funktioniert auf Basis von Richtlinien, die auf Benutzer oder Benutzergruppen angewendet werden, die bestimmte Kriterien erfüllen.
Azure erstellt eine Standardrichtlinie, die angibt, dass Administratoren die zweistufige Authentifizierung benötigen, um sich beim Portal anzumelden.
Sobald diese Richtlinie aktiviert ist, werden Sie möglicherweise dazu aufgefordert, sich beim Azure-Portal abzumelden und sich wieder anzumelden.

So aktivieren Sie die mehrstufige Authentifizierung für Administratoranmeldungen:

1. Öffnen Sie im Azure-Portal die Registerkarte **Azure Active Directory**.
2. Wählen Sie in der Kategorie „Sicherheit“ die Option „Bedingter Zugriff“ aus. Sie sollten nun die folgende Anzeige sehen:

   ![Bedingter Zugriff – Richtlinien](./media/secure-web-app/ad-mfa-conditional-add.png)

Gehen Sie wie folgt vor, wenn Sie keine neue Richtlinie erstellen können:

1. Öffnen Sie die Registerkarte **Mehrstufige Authentifizierung**.
2. Klicken Sie auf den Link zur **kostenlosen Testversion** von Azure Active Directory Premium, und abonnieren Sie sie.

   ![Kostenlose Azure AD Premium-Testversion](./media/secure-web-app/ad-trial-premium.png)

Kehren Sie zur Anzeige „Bedingter Zugriff“ zurück.

1. Wählen Sie die Registerkarte „Neue Richtlinie“ aus.
2. Geben Sie den Namen der Richtlinie ein.
3. Wählen Sie die Benutzer oder Gruppen aus, für die die mehrstufige Authentifizierung aktiviert werden soll.
4. Wählen Sie unter **Zugriffssteuerung** die Registerkarte **Zuweisung** aus, und wählen Sie dann **Require multi-factor authentication** (Mehrstufige Authentifizierung anfordern) (und gegebenenfalls andere Einstellungen) aus.

   ![Anfordern von MFA](./media/secure-web-app/ad-mfa-conditional-add.png)

Sie können die Richtlinie aktivieren, indem Sie das Kontrollkästchen in der Anzeige oben oder auf der Registerkarte **Bedingter Zugriff** aktivieren. Wenn die Richtlinie aktiviert ist, benötigen Benutzer eine mehrstufige Authentifizierung, um sich beim Portal anzumelden.

Es gibt eine Basisrichtlinie, die mehrstufige Authentifizierung für alle Azure-Administratoren erfordert. Sie können diese im Portal direkt aktivieren. Wenn Sie diese Richtlinie aktivieren, wird Ihre aktuelle Sitzung möglicherweise ungültig und erfordert eine erneute Anmeldung.

Gehen Sie wie folgt vor, wenn die Basisrichtlinie nicht aktiviert ist:
1.  Klicken Sie auf **Require MFA for admins** (Mehrstufige Authentifizierung für Administratoren anfordern).
2.  Wählen Sie dann **Richtlinie sofort verwenden** aus.

   ![Auswahl von „Richtlinie sofort verwenden“](./media/secure-web-app/ad-mfa-conditional-enable.png)

#### <a name="use-azure-sentinel-to-monitor-apps-and-resources"></a>Verwenden von Azure Sentinel zum Überwachen von Apps und Ressourcen

Mit dem Wachstum einer Anwendung wird es schwerer, alle von Ressourcen empfangenen Sicherheitssignale und Metriken zu aggregieren und auf aktionsorientierte Weise zu nutzen.

Azure Sentinel wurde zum Erfassen von Daten, Ermitteln möglicher Bedrohungstypen und zur Bereitstellung eines Überblicks über Sicherheitsvorfälle entworfen.
Während auf einen manuellen Eingriff gewartet wird, kann Azure Sentinel auf vorgeschriebene Playbooks zurückgreifen, um Warnungen und Prozesse für die Vorfallverwaltung auszulösen.

Die Beispiel-App besteht aus mehreren Ressourcen, die von Azure Sentinel überwacht werden können.
Sie müssen zunächst einen Log Analytics-Arbeitsbereich erstellen, der alle von den verschiedenen Ressourcen erfassten Daten speichert, um Azure Sentinel einzurichten.

So erstellen Sie diesen Arbeitsbereich:

1. Suchen Sie im Suchfeld des Azure-Portals nach **Log Analytics**. Wählen Sie **Log Analytics-Arbeitsbereiche** aus.

   ![Suche nach Log Analytics-Arbeitsbereichen](./media/secure-web-app/sentinel-log-analytics.png)

    *Suche nach Log Analytics-Arbeitsbereichen*

2. Klicken Sie auf der nächsten Seite auf **Hinzufügen**, und geben Sie dann einen Namen, eine Ressourcengruppe und einen Speicherort für den Arbeitsbereich an.
   ![Erstellen eines Log Analytics-Arbeitsbereichs](./media/secure-web-app/sentinel-log-analytics-create.png)

   *Erstellen eines Log Analytics-Arbeitsbereichs*

3. Suchen Sie über das Suchfeld nach **Azure Sentinel**.

   ![Suche nach Azure Sentinel](./media/secure-web-app/sentinel-add.png)

    *Suche nach Azure Sentinel*

4. Klicken Sie auf **Hinzufügen**, und wählen Sie dann den zuvor erstellten Log Analytics-Arbeitsbereich aus.

   ![Hinzufügen eines Log Analytics-Arbeitsbereichs](./media/secure-web-app/sentinel-workspace-add.png)

    *Hinzufügen eines Log Analytics-Arbeitsbereichs*

5. Wählen Sie auf der Seite **Azure Sentinel – Data connectors** (Azure Sentinel – Datenconnectors) unter **Konfiguration** die Option **Data connectors** (Datenconnectors) aus. Daraufhin werden Ihnen Azure-Dienste angezeigt, die Sie für die Analyse in Azure Sentinel mit der Log Analytics-Speicherinstanz verknüpfen können.

   ![Log Analytics-Datenconnectors](./media/secure-web-app/sentinel-connectors.png)

    *Hinzufügen eines Datenconnectors zu Azure Sentinel*

   Führen Sie die folgenden Schritte aus, um beispielsweise das Anwendungsgateway zu verknüpfen:

   1. Öffnen Sie das Blatt der Azure Application Gateway-Instanz.
   2. Wählen Sie unter **Überwachung** die Option **Diagnoseeinstellungen** aus.
   3. Klicken Sie auf **Diagnoseeinstellung hinzufügen**.

      ![Hinzufügen der Application Gateway-Diagnose](./media/secure-web-app/sentinel-gateway-connector.png)

      *Hinzufügen der Application Gateway-Diagnose*

   4. Wählen Sie auf der Seite **Diagnoseeinstellungen** den Log Analytics-Arbeitsbereich aus, den Sie erstellt haben, und wählen Sie dann alle Metriken aus, die Sie erfassen und an Azure Sentinel senden möchten. Wählen Sie **Speichern** aus.

        ![Einstellungen für den Azure Sentinel-Connector](./media/secure-web-app/sentinel-connector-settings.png)

        *Einstellungen für den Azure Sentinel-Connector*

  Die Metriken der Ressource befinden sich nun in Azure Sentinel, wo Sie sie abfragen und untersuchen können.

   Fügen Sie dieselben Metriken in Diagnoseeinstellung für Azure Key Vault, die öffentliche IP-Adresse, Azure Database for PostgreSQL und alle anderen Dienste Ihres Kontos hinzu, die Diagnoseprotokolle unterstützen.

Sobald Sie die Metriken eingerichtet haben, verfügt Azure Sentinel über Daten für die Analyse.

## <a name="evaluate-and-verify"></a>Auswerten und Überprüfen
Nachdem Sie die Architektur entwickelt und bereitgestellt haben, müssen Sie sicherstellen, dass der Code und die bereitgestellten Dienste gewisse Sicherheitsstandards erfüllen. Im Folgenden werden einige Schritte zur Überprüfung der Software beschrieben:

- Statische Codeanalyse
- Überprüfung auf Sicherheitsrisiken
- Suchen und Beheben von Sicherheitsrisiken in Anwendungsabhängigkeiten

Dies sind Grundbausteine für bewährte Methoden für die sichere Entwicklung.

### <a name="static-code-analysis"></a>Statische Codeanalyse
Für die Beispiel-App umfasst die Überprüfung mit Tools für die statische Analyse die Suche nach Sicherheitsrisiken im App-Code mithilfe von Verfahren wie die Taint-Prüfung und Datenflussanalyse. Mit Python-Tools für die statische Analyse können Sie sich der Sicherheit Ihrer App sicher sein.

**Linting**

PyFlakes, eine Python-Lintingbibliothek, unterstützt Sie wie im Folgenden gezeigt beim Entfernen von totem Code und nicht verwendeten Funktionen aus Apps:

![PyFlakes](./media/secure-web-app/pyflakes.png)

Linting verschafft Ihnen Hinweise und mögliche Änderungen, die Ihren Code sauberer und zur Laufzeit weniger fehleranfällig machen können.

**PyLint**

PyLint hat den größten Mehrwert für dieses Projekt generiert. Es führt Überprüfungen des Codestandards, Fehlerüberprüfungen durch und stellt Umgestaltungstipps bereit, um sicherzustellen, dass der auf dem Server ausgeführte Code sicher ist. Durch Verwendung von PyLint zum Aktualisieren Ihres Codes können Sie wie in den folgenden Abbildungen gezeigt Fehler beseitigen und die PyLint-Bewertung verbessern.

![Vor PyLint](./media/secure-web-app/before-pylint.png)

*Vor PyLint*

Nachdem Sie einige der Codefehler gefunden haben, die von den Lintingtools ermittelt wurden, haben Sie mehr Gewissheit, dass Ihr Code nicht fehleranfällig ist. Das Beheben dieser Fehler sorgt für eine signifikante Verringerung der Sicherheitsrisiken, die auftreten können, wenn der Code in Produktionsumgebungen bereitgestellt wird.

![Nach PyLint](./media/secure-web-app/after-pylint.png)

*Nach PyLint*

### <a name="vulnerability-scanning"></a>Überprüfung auf Sicherheitsrisiken
Bei dem [ZAP-Tool von OWASP](https://www.zaproxy.org/) handelt es sich um eine Open-Source-Überprüfung auf Sicherheitsrisiken von Webanwendungen, die Sie verwenden können, um die Beispiel-App auf Sicherheitsrisiken zu untersuchen. Beim Ausführen des Tools für die Beispiel-App werden einige mögliche Fehler und Angriffsvektoren aufgezeigt.

![ZAP-Tool](./media/secure-web-app/zap-tool.png)

*ZAP-Tool*

### <a name="find-and-fix-vulnerabilities-in-app-dependencies"></a>Suchen und Beheben von Sicherheitsrisiken in App-Abhängigkeiten
Sie können die [Abhängigkeitsüberprüfung von OWASP](https://www.owasp.org/index.php/OWASP_Dependency_Check) zum Suchen und Beheben von Anwendungsabhängigkeiten verwenden.

„Safety“ ist eine ähnliche Anwendung, die Abhängigkeiten überprüft. Sie können Sie auf [GitHub](https://github.com/pyupio/safety) finden. Safety scannt nach Sicherheitsrisiken, die in bekannten Datenbanken für Sicherheitsrisiken dokumentiert wurden.

![Safety](./media/secure-web-app/pysafety.png)

*Safety*

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Entwerfen, Entwickeln und Bereitstellen sicherer Anwendungen finden Sie in den folgenden Artikeln.

- [Entwurf](secure-design.md)
- [Entwickeln](secure-develop.md)
- [Bereitstellen](secure-deploy.md)
