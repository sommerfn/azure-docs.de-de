---
title: Bereitstellung virtueller Azure-Computer mit Chef | Microsoft Docs
description: Hier erfahren Sie, wie Sie Chef zur automatisierten Bereitstellung und Konfiguration virtueller Computer in Microsoft Azure verwenden.
services: virtual-machines-windows
documentationcenter: ''
author: diegoviso
manager: gwallace
tags: azure-service-management,azure-resource-manager
editor: ''
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 07/09/2019
ms.author: diviso
ms.openlocfilehash: 74b92c277b1d6eaa0984e55a70459bad59c2bf84
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719277"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatisieren der Bereitstellung virtueller Azure-Computer mit Chef

Chef ist ein hervorragendes Tool zur Automatisierung und für Konfigurationen mit gewünschten Zuständen.

Mit der aktuellen Veröffentlichung der Cloud-API bietet Chef eine nahtlose Integration in Azure und Ihnen die Möglichkeit, den Konfigurationsstatus über einen einzelnen Befehl bereitzustellen und zu implementieren.

Anhand dieses Artikels richten Sie die Chef-Umgebung für die Bereitstellung virtueller Azure-Computer ein. Zudem werden Sie schrittweise durch die Erstellung einer Richtlinie und die anschließende Bereitstellung dieses „Cookbooks“ auf einem virtuellen Azure-Computer geführt.

## <a name="chef-basics"></a>Grundlagen zu Chef
Machen Sie sich zuerst [mit den grundlegenden Konzepten von Chef vertraut](https://www.chef.io/chef).

Das folgende Diagramm zeigt die allgemeine Chef-Architektur:

![][2]

Die Architektur von Chef setzt sich aus drei Hauptkomponenten zusammen: Chef-Server, Chef-Client (Knoten) und Chef-Arbeitsstation.

Der Chef-Server ist der Verwaltungspunkt, für den es zwei Optionen gibt: eine gehostete oder eine lokale Lösung.

Der Chef-Client (Knoten) ist der Agent, der sich auf den Servern befindet, die Sie verwalten.

Die Chef-Arbeitsstation ist der Name für die Administrator-Arbeitsstation, auf der Sie Richtlinien erstellen und Verwaltungsbefehle ausführen, sowie für das Softwarepaket mit Chef-Tools.

Im Allgemeinen wird der Ort, an dem Sie Aktionen ausführen als _Ihre Arbeitsstation_ und das Softwarepaket als _Chef-Arbeitsstation_ bezeichnet.
Beispiel: Sie laden den Befehl „knife“ als Teil der _Chef-Arbeitsstation_ herunter, führen knife-Befehler jedoch über _Ihre Arbeitsstation_ aus, um die Infrastruktur zu verwalten.

Chef verwendet auch die Konzepte von „Cookbooks“ und „Recipes“, die im Grunde die Richtlinien sind, die wir definieren und auf die Server anwenden.

## <a name="preparing-your-workstation"></a>Vorbereiten der Arbeitsstation

Bereiten Sie zunächst Ihre Arbeitsstation vor, indem Sie ein Verzeichnis zum Speichern von Chef-Konfigurationsdateien und Rezeptbüchern (Cookbooks) erstellen.

Erstellen Sie ein Verzeichnis namens „C:\Chef“.

Laden Sie die neueste Version der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) herunter, und installieren Sie sie auf Ihrer Arbeitsstation.

## <a name="configure-azure-service-principal"></a>Konfigurieren eines Azure-Dienstprinzipals

Einfach ausgedrückt ist ein Azure-Dienstprinzipal ein Dienstkonto.   Sie verwenden einen Dienstprinzipal, um Azure-Ressourcen auf der Arbeitsstation „Chef“ zu erstellen.  Um den entsprechenden Dienstprinzipal mit den erforderlichen Berechtigungen zu erstellen, führen Sie die folgenden Befehle in PowerShell aus:
 
```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<yourSubscriptionName>"
$myApplication = New-AzureRmADApplication -DisplayName "automation-app" -HomePage "https://chef-automation-test.com" -IdentifierUris "https://chef-automation-test.com" -Password "#1234p$wdchef19"
New-AzureRmADServicePrincipal -ApplicationId $myApplication.ApplicationId
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $myApplication.ApplicationId
```

Notieren Sie Ihre Abonnement-ID, Ihre Mandanten-ID, Ihre Client-ID und Ihren geheimen Clientschlüssel (das oben festgelegte Kennwort) – Sie benötigen diese Informationen später. 

## <a name="setup-chef-server"></a>Einrichten des Chef-Servers

Dieser Leitfaden setzt voraus, dass Sie sich für Hosted Chef registrieren.

Wenn Sie noch keinen Chef-Server verwenden, haben Sie folgende Möglichkeiten:

* Registrieren Sie sich für [Hosted Chef](https://manage.chef.io/signup). Dies ist die schnellste Möglichkeit zum Einstieg in Chef.
* Installieren Sie einen eigenständigen Chef-Server auf einem Linux-basierten Computer. Folgen Sie dazu den [Installationsanweisungen](https://docs.chef.io/install_server.html) in der [Chef-Dokumentation](https://docs.chef.io/).

### <a name="creating-a-hosted-chef-account"></a>Erstellen eines verwalteten Chef-Kontos

Registrieren Sie sich [hier](https://manage.chef.io/signup) für ein gehostetes Chef-Konto.

Während der Registrierung werden Sie aufgefordert, eine neue Organisation zu erstellen.

![][3]

Sobald Ihre Organisation erstellt wurde, laden Sie das Starterkit herunter.

![][4]

> [!NOTE]
> Wenn Sie eine Meldung mit dem Hinweis erhalten, dass Ihre Schlüssel zurückgesetzt werden, ist es in Ordnung, wenn Sie fortfahren, da wir noch keine Infrastruktur konfiguriert haben.
>

Die ZIP-Datei dieses Starterkits enthält die Konfigurationsdateien und den Benutzerschlüssel für Ihre Organisation im Verzeichnis `.chef`.

Die Datei `organization-validator.pem` muss separat heruntergeladen werden, da es sich dabei um einen privaten Schlüssel handelt und private Schlüssel nicht auf dem Chef-Server gespeichert werden dürfen. Wählen Sie unter [Chef Manage](https://manage.chef.io/) im Verwaltungsbereich die Option „Reset Validation Key“ (Validierungsschlüssel zurücksetzen) aus. Damit wird eine separat herunterladbare Datei bereitgestellt. Speichern Sie die Datei unter „C:\chef“.

### <a name="configuring-your-chef-workstation"></a>Konfigurieren der Chef-Arbeitsstation

Extrahieren Sie den Inhalt der Datei „chef-starter.zip“ im Ordner „C:\chef“.

Kopieren Sie alle Dateien unter „chef-starter\chef-repo\.chef“ in das Verzeichnis „C:\chef“.

Kopieren Sie die Datei `organization-validator.pem` in „C:\chef“, falls sie unter „C:\Downloads“ gespeichert wurde.

Ihr Verzeichnis sollte in etwa wie folgt aussehen:

```powershell
    Directory: C:\Users\username\chef

Mode           LastWriteTime    Length Name
----           -------------    ------ ----
d-----    12/6/2018   6:41 PM           .chef
d-----    12/6/2018   5:40 PM           chef-repo
d-----    12/6/2018   5:38 PM           cookbooks
d-----    12/6/2018   5:38 PM           roles
-a----    12/6/2018   5:38 PM       495 .gitignore
-a----    12/6/2018   5:37 PM      1678 azuredocs-validator.pem
-a----    12/6/2018   5:38 PM      1674 user.pem
-a----    12/6/2018   5:53 PM       414 knife.rb
-a----    12/6/2018   5:38 PM      2341 README.md
```

Das Stammverzeichnis „C:\chef“ sollte jetzt fünf Dateien und vier Verzeichnisse (einschließlich des leeren Verzeichnisses „chef-repo“) enthalten.

### <a name="edit-kniferb"></a>Bearbeiten von „knife.rb“

Die PEM-Dateien enthalten die privaten Schlüssel für die Organisation und Administration für die Kommunikation, während die Datei „knife.rb“ die knife-Konfiguration enthält. Wir müssen die Datei knife.rb bearbeiten.

Öffnen Sie die Datei „knife.rb“ in einem Editor Ihrer Wahl. Die unveränderte Datei sollte etwa so aussehen:

```rb
current_dir = File.dirname(__FILE__)
log_level           :info
log_location        STDOUT
node_name           "mynode"
client_key          "#{current_dir}/user.pem"
chef_server_url     "https://api.chef.io/organizations/myorg"
cookbook_path       ["#{current_dir}/cookbooks"]
```

Fügen Sie der Datei „knife.rb“ die folgenden Informationen:

validation_client_name   "myorg-validator"

validation_key           "#{current_dir}/myorg.pem"

knife[:azure_tenant_id] =         "0000000-1111-aaaa-bbbb-222222222222"

knife[:azure_subscription_id] =   "11111111-bbbbb-cccc-1111-222222222222"

knife[:azure_client_id] =         "11111111-bbbbb-cccc-1111-2222222222222"

knife[:azure_client_secret] =     "#1234p$wdchef19"


Diese Zeilen stellen sicher, dass Knife auf „C:\chef\cookbooks“ verweist und bei Azure-Vorgängen den von Ihnen erstellten Azure-Dienstprinzipal verwendet.

Die Datei „knife.rb“ sollte in etwa wie im folgenden Beispiel aussehen:

![][14]

<!--- Giant problem with this section: Chef 12 uses a config.rb instead of knife.rb
// However, the starter kit hasn't been updated
// So, I don't think this will even work on the modern Chef -->

<!--- update image [6] knife.rb -->

```rb
current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "myorg"
client_key               "#{current_dir}/myorg.pem"
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg-validator.pem"
chef_server_url          "https://api.chef.io/organizations/myorg"
cookbook_path            ["#{current_dir}/../cookbooks"]
knife[:azure_tenant_id] = "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] = "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] = "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] = "#1234p$wdchef19"
```

## <a name="install-chef-workstation"></a>Installieren der Chef-Arbeitsstation

Als Nächstes wird die Chef-Arbeitsstation [heruntergeladen und installiert](https://downloads.chef.io/chef-workstation/).
Installieren Sie die Chef-Arbeitsstation am standardmäßigen Speicherort. Diese Installation kann einige Minuten dauern.

Auf dem Desktop wird „CW PowerShell“ angezeigt. Dabei handelt es sich um eine Umgebung, die mit dem Tool geladen wurde, das Sie für die Interaktion mit den Chef-Produkten benötigen. CW PowerShell macht neue Ad-hoc-Befehle wie `chef-run` sowie herkömmliche Chef-CLI-Befehle wie `chef` verfügbar. Mit `chef -v` zeigen Sie Ihre installierte Version der Chef-Arbeitsstation und der Chef-Tools an. Die Version Ihrer Arbeitsstation können Sie ebenfalls überprüfen, indem Sie „About Chef Workstation“ (Über Chef Workstation) in der Chef Workstation-App auswählen.

`chef --version` sollte etwa Folgendes zurückgeben:

```
Chef Workstation: 0.4.2
  chef-run: 0.3.0
  chef-client: 15.0.300
  delivery-cli: 0.0.52 (9d07501a3b347cc687c902319d23dc32dd5fa621)
  berks: 7.0.8
  test-kitchen: 2.2.5
  inspec: 4.3.2
```

> [!NOTE]
> Die Reihenfolge der Pfade ist wichtig! Wenn die "opscode"-Pfade nicht in der richtigen Reihenfolge angegeben sind, können Probleme auftreten.
>

Starten Sie die Arbeitsstation neu, bevor Sie fortfahren.

### <a name="install-knife-azure"></a>Installieren von Knife Azure

In diesem Tutorial wird davon ausgegangen, dass Sie Azure Resource Manager für die Interaktion mit Ihrem virtuellen Computer verwenden.

Installieren Sie die Azure Knife-Erweiterung. Dadurch erhält Knife das Azure-Plug-In.

Führen Sie den folgenden Befehl aus:

    chef gem install knife-azure ––pre

> [!NOTE]
> Das Argument „-pre“ stellt sicher, dass Sie die neueste RC-Version des Azure-Plug-Ins für Knife erhalten, das Zugriff auf die neuesten APIs bietet.
>
>

Voraussichtlich werden gleichzeitig auch eine Reihe von Abhängigkeiten installiert.

![][8]

Führen Sie den folgenden Befehl aus, um sich zu vergewissern, dass alles korrekt konfiguriert ist:

    knife azurerm server list

Wenn alles richtig konfiguriert ist, wird eine Liste der verfügbaren Azure-Images angezeigt.

Herzlichen Glückwunsch. Die Arbeitsstation ist eingerichtet!

## <a name="creating-a-cookbook"></a>Erstellen eines Cookbooks

Ein Cookbook wird von Chef dazu verwendet, um eine Reihe von Befehlen zu definieren, die auf dem verwalteten Client ausgeführt werden sollen. Die Cookbook-Erstellung ist unkompliziert. Verwenden Sie einfach den Befehl **chef generate cookbook** zum Erstellen der Cookbook-Vorlage. Dieses Cookbook ist für einen Webserver vorgesehen, der automatisch IIS bereitstellt.

Führen Sie im Verzeichnis „C:\Chef“ den folgenden Befehl aus:

    chef generate cookbook webserver

Mit diesem Befehl werden eine Reihe von Dateien im Verzeichnis C:\Chef\cookbooks\webserver generiert. Als Nächstes definieren Sie den Satz von Befehlen, die der Chef-Client auf dem verwalteten virtuellen Computer ausführen soll.

Die Befehle werden in der Datei default.rb gespeichert. In dieser Datei definieren Sie eine Reihe von Befehlen, die IIS installieren, starten und eine Vorlagendatei in den Ordner „wwwroot“ kopieren.

Ändern Sie die Datei „C:\chef\cookbooks\webserver\recipes\default.rb“, und fügen Sie die folgenden Zeilen hinzu:

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

Speichern Sie die Datei, sobald Sie fertig sind.

## <a name="creating-a-template"></a>Erstellen einer Vorlage
In diesem Schritt generieren Sie eine Vorlagendatei, die als Seite „default.html“ verwendet wird.

Führen Sie den folgenden Befehl aus, um die Vorlage zu generieren:

    chef generate template webserver Default.htm

Navigieren Sie zur Datei `C:\chef\cookbooks\webserver\templates\default\Default.htm.erb`. Fügen Sie der Datei etwas einfachen HTML-Code (Hello World) hinzu, und speichern Sie sie.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Hochladen des Cookbooks auf den Chef-Server
In diesem Schritt erstellen Sie eine Kopie des Cookbooks, das Sie auf dem lokalen Computer erstellt haben, und laden sie auf den von Chef gehosteten Server hoch. Sobald es hochgeladen ist, wird das Cookbook auf der Registerkarte **Richtlinie** angezeigt.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Bereitstellen eines virtuellen Computers mit Knife Azure
Stellen Sie einen virtuellen Azure-Computer bereit, und wenden Sie das Cookbook „Webserver“ an, das den IIS-Webdienst und die Standardwebseite installiert.

Verwenden Sie hierzu den Befehl **knife azurerm server create**.

Im Anschluss sehen Sie ein Beispiel für den Befehl.

    knife azurerm server create `
    --azure-resource-group-name rg-chefdeployment `
    --azure-storage-account store `
    --azure-vm-name chefvm `
    --azure-vm-size 'Standard_DS2_v2' `
    --azure-service-location 'westus' `
    --azure-image-reference-offer 'WindowsServer' `
    --azure-image-reference-publisher 'MicrosoftWindowsServer' `
    --azure-image-reference-sku '2016-Datacenter' `
    --azure-image-reference-version 'latest' `
    -x myuser -P myPassword123 `
    --tcp-endpoints '80,3389' `
    --chef-daemon-interval 1 `
    -r "recipe[webserver]"


Das obige Beispiel erstellt einen virtuellen Computer vom Typ „Standard_DS2_v2“ mit Windows Server 2016 in der Region „USA, Westen“. Ersetzen Sie Ihre bestimmten Variablen, und führen Sie den Befehl dann aus.

> [!NOTE]
> Über die Befehlszeile automatisiere ich mithilfe des Parameters „–tcp-endpoints“ auch die Filterregeln für mein Endpunktnetzwerk. Ich habe die Ports 80 und 3389 geöffnet, um Zugriff auf die Webseite und die RDP-Sitzung zu ermöglichen.
>
>

Nachdem Sie den Befehl ausgeführt haben, wechseln Sie zum Azure-Portal, wo Sie die Bereitstellung des virtuellen Computers verfolgen können.

![][15]

Anschließend erscheint die Eingabeaufforderung.

![][16]

Nach Abschluss der Bereitstellung wird die öffentliche IP-Adresse des neuen virtuellen Computers angezeigt. Sie können diese kopieren und in einen Webbrowser einfügen, um die Website anzuzeigen, die Sie bereitgestellt haben. Nach dem Bereitstellen des virtuellen Computers haben wir Port 80 geöffnet, sodass er auch extern verfügbar sein sollte.   

![][11]

In diesem Beispiel wird kreativer HTML-Code verwendet.

Sie können den Zustand des Knotens auch unter [Chef Manage](https://manage.chef.io/) anzeigen. 

![][17]

Denken Sie daran, dass Sie die Verbindung auch im Azure-Portal per RDP-Sitzung über den Port 3389 herstellen können.

Vielen Dank! Starten Sie noch heute mit Azure, um Ihre Infrastruktur als Code umzusetzen!

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png
[14]: media/chef-automation/14.png
[15]: media/chef-automation/15.png
[16]: media/chef-automation/16.png
[17]: media/chef-automation/17.png


<!--Link references-->
