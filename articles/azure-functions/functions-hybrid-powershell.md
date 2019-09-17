---
title: Remoteverwaltung von lokalen Ressourcen mit PowerShell Azure Functions
description: Erfahren Sie, wie Sie Hybrid Connections in Azure Relay konfigurieren können, um eine PowerShell-Funktions-App mit lokalen Ressourcen zu verbinden, die dann zur Remoteverwaltung der lokalen Ressource verwendet werden kann.
author: eamono
manager: gailey777
ms.service: azure-functions
ms.topic: conceptual
ms.date: 9/5/2019
ms.author: eamono
ms.openlocfilehash: 0acbe0892be50e9e1747e5839101110d4adcebcf
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70775094"
---
# <a name="managing-hybrid-environments-with-powershell-in-azure-functions-and-app-service-hybrid-connections"></a>Verwalten von Hybridumgebungen mit PowerShell in Azure Functions und App Service Hybrid Connections

Azure App Service Hybrid Connections ermöglicht den Zugriff auf Ressourcen in anderen Netzwerken. Weitere Informationen zu dieser Funktion finden Sie in der Dokumentation zu [Hybrid Connections](../app-service/app-service-hybrid-connections.md). Die folgenden Informationen beschreiben, wie Sie diese Funktion zum Ausführen von PowerShell-Funktionen verwenden, die auf einen lokalen Server abzielen. Dieser Server kann dann verwendet werden, um alle Ressourcen in der lokalen Umgebung über eine Azure PowerShell Funktion zu verwalten.


## <a name="configure-an-on-premises-server-for-powershell-remoting"></a>Konfigurieren eines lokalen Servers für PowerShell-Remoting

Das folgende Skript ermöglicht PowerShell-Remoting, erstellt eine neue Firewallregel und einen WinRM-HTTPS-Listener. Zu Testzwecken wird ein selbstsigniertes Zertifikat verwendet. Es wird empfohlen, ein signiertes Zertifikat für die Produktion zu verwenden.

```powershell
# For configuration of WinRM, please see
# https://docs.microsoft.com/windows/win32/winrm/installation-and-configuration-for-windows-remote-management

# Enable PowerShell remoting
Enable-PSRemoting -Force

# Create firewall rule for WinRM. The default HTTPS port is 5986 
New-NetFirewallRule -Name "WinRM HTTPS" `
                    -DisplayName "WinRM HTTPS" `
                    -Enabled True `
                    -Profile "Any" `
                    -Action "Allow" `
                    -Direction "Inbound" `
                    -LocalPort 5986 `
                    -Protocol "TCP"

# Create new self-signed-certificate to be used by WinRM
$Thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME  -CertStoreLocation Cert:\LocalMachine\My).Thumbprint

# Create WinRM HTTPS listener
$Cmd = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:COMPUTERNAME ""; CertificateThumbprint=""$Thumbprint""}"
cmd.exe /C $Cmd
```

## <a name="create-a-powershell-function-app-in-the-portal"></a>Erstellen einer PowerShell-Funktions-App im Portal

App Service Hybrid Connections sind nur in den Tarifplänen Basic, Standard und Isolated verfügbar. Wenn Sie die Funktions-App mit PowerShell erstellen, erstellen Sie einen dieser Tarife, oder wählen Sie ihn aus.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

1. Wählen Sie **+ Ressource erstellen** auf der linken Seite aus, und wählen Sie dann **Funktions-App** aus.

1. Wählen Sie als **Hostingplan** den **App Service-Plan** aus und dann **App Service-Plan/Standort**.

1. Wählen Sie **Neu erstellen** aus, geben Sie einen Namen für **App Service-Plan** ein, wählen Sie einen **Standort** in einer [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe oder in der Nähe anderer Dienste aus, auf die Ihre Funktionen zugreifen, und wählen Sie dann **Tarif** aus.

1. Wählen Sie den Tarif S1 Standard aus, und wählen Sie dann **Anwenden** aus.

1. Wählen Sie **OK** aus, um den Plan zu erstellen, und verwenden Sie dann die in der Tabelle unter der Abbildung angegebenen Einstellungen für die Funktions-App. 

    ![PowerShell Core-Funktions-App](./media/functions-hybrid-powershell/create-function-powershell-app.png)  

    | Einstellung      | Empfohlener Wert  | BESCHREIBUNG                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **App-Name** | Global eindeutiger Name | Der Name, der Ihre neue Funktionen-App bezeichnet Gültige Zeichen sind `a-z`, `0-9` und `-`.  | 
    | **Abonnement** | Ihr Abonnement | Das Abonnement, unter dem diese neue Funktions-App erstellt wird. |
    | **Ressourcengruppe** |  myResourceGroup | Der Name der neuen Ressourcengruppe, in der die Funktionen-App erstellt wird Sie können auch den vorgeschlagenen Wert verwenden. |
    | **Betriebssystem** | Bevorzugtes Betriebssystem | Wählen Sie „Windows“ aus. |
    | **Laufzeitstapel** | Bevorzugte Sprache | Wählen Sie die PowerShell Core aus. |
    | **Speicher** |  Global eindeutiger Name |  Erstellen Sie ein Speicherkonto, das von Ihrer Funktions-App verwendet wird. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten. Sie können außerdem ein vorhandenes Konto verwenden.
    | **Application Insights** | Standard | Erstellt eine Application Insights-Ressource mit dem gleichen *App-Namen* in der nächstgelegenen unterstützten Region. Durch Erweitern dieser Einstellung können Sie den **neuen Ressourcennamen** ändern oder einen anderen **Standort** in einer [Azure-Region](https://azure.microsoft.com/global-infrastructure/geographies/) wählen, in der Sie Ihre Daten speichern möchten. |

1. Wählen Sie nach der Überprüfung Ihrer Einstellungen **Erstellen** aus.

1. Wählen Sie oben rechts im Portal das Benachrichtigungssymbol aus, und achten Sie auf die Meldung **Bereitstellung erfolgreich**.

1. Wählen Sie **Zu Ressource wechseln**, um Ihre neue Funktionen-App anzuzeigen. Sie können auch die Option **An Dashboard anheften** auswählen. Wenn Sie die Funktions-App anheften, können Sie einfacher über das Dashboard auf sie zugreifen.

## <a name="create-a-hybrid-connection-for-the-function-app"></a>Erstellen einer Hybridverbindung für die Funktions-App

Hybridverbindungen werden über den Abschnitt „Networking“ (Netzwerk) der Funktions-App konfiguriert.

1. Wählen Sie die Registerkarte „Platform“ (Plattform) der Funktions-App aus, und wählen Sie dann „Networking“ (Netzwerk) aus.
![App-Übersicht für Plattformnetzwerke](./media/functions-hybrid-powershell/app-overview-platform-networking.png)  
1. Wählen Sie „Configure your hybrid connections endpoints“ (Endpunkte der Hybridverbindung konfigurieren) aus.
![Netzwerk](./media/functions-hybrid-powershell/select-network-feature.png)  
1. Wählen Sie „Add hybrid connection“ (Hybridverbindung hinzufügen) aus.
![Hybrid Connection (Hybridverbindung)](./media/functions-hybrid-powershell/hybrid-connection-overview.png)  
1. Geben Sie wie unten gezeigt Informationen zur Hybridverbindung ein. Der Endpunkthost kann optional mit dem Hostnamen des lokalen Servers übereinstimmen, damit Sie sich den Server später beim Ausführen von Remotebefehlen leichter merken können. Der Port stimmt mit dem Standardport für den Windows-Remoteverwaltungsdienst überein, der zuvor auf dem Server definiert wurde.
![Add Hybrid Connection (Hybridverbindung hinzufügen)](./media/functions-hybrid-powershell/add-hybrid-connection.png)  

    **Hybrid connection name (Name der Hybridverbindung)** : ContosoHybridOnPremisesServer
    
    **Endpoint Host (Endpunkthost)** : finance1
    
    **Endpoint Port (Endpunktport)** : 5986
    
    **Servicebus namespace (Service Bus-Namespace)** : Create neu (Neu erstellen)
    
    **Location (Standort)** : Wählen Sie einen verfügbaren Standort aus.
    
    **Name**: contosopowershellhybrid

5. Klicken Sie auf „OK“, um die Hybridverbindung zu erstellen.

## <a name="download-and-install-the-hybrid-connection"></a>Herunterladen und Installieren der Hybridverbindung

1. Wählen Sie das Downloadsymbol von Connection Manager aus, um die MSI-Datei lokal auf Ihrem Computer zu speichern.
![Herunterladen des Installers](./media/functions-hybrid-powershell/download-hybrid-connection-installer.png)  
1. Kopieren Sie die MSI-Datei von Ihrem lokalen Computer auf den lokalen Server.
1. Führen Sie den Hybridverbindungs-Installer aus, um den Dienst auf dem lokalen Server zu installieren.
![Installieren der Hybridverbindung](./media/functions-hybrid-powershell/hybrid-installation.png)  
1. Öffnen Sie im Portal die Hybridverbindung, und kopieren Sie die Gatewayverbindungszeichenfolge in die Zwischenablage.
![Kopieren der Hybridverbindungszeichenfolge](./media/functions-hybrid-powershell/copy-hybrid-connection.png)  
1. Öffnen Sie die Benutzeroberfläche des Hybrid Connection-Managers auf dem lokalen Server.
![Öffnen der Benutzeroberfläche von Hybrid Connection](./media/functions-hybrid-powershell/hybrid-connection-ui.png)  
1. Wählen Sie die Schaltfläche „Enter manually“ (Manuell eingeben) aus, und fügen Sie die Verbindungszeichenfolge aus der Zwischenablage ein.
![Einfügen der Verbindung](./media/functions-hybrid-powershell/enter-manual-connection.png)  
1. Starten Sie den Hybrid Connection-Manager aus PowerShell neu, wenn er nicht als verbunden angezeigt wird.
```powershell
Restart-Service HybridConnectionManager
```

## <a name="create-an-app-setting-for-the-password-of-an-administrator-account"></a>Erstellen einer App-Einstellung für das Kennwort eines Administratorkontos

1. Wählen Sie die Registerkarte „Platform“ (Plattform) in der Funktions-App aus.
1. Wählen Sie die Konfiguration im Abschnitt „General Settings“ (Allgemeine Einstellungen) aus. ![Auswählen der Plattformkonfiguration](./media/functions-hybrid-powershell/select-configuration.png)  
1. Wählen Sie „New application setting“ (Neue Anwendungseinstellung) aus, um eine neue Einstellung für das Kennwort zu erstellen.
1. Geben Sie der Einstellung den Namen ContosoUserPassword, und geben Sie das Kennwort ein.
1. Wählen Sie „OK“ und dann „Save“ (Speichern) aus, um das Kennwort in der Funktions-App zu speichern. ![Hinzufügen der App-Einstellung für das Kennwort](./media/functions-hybrid-powershell/add-appsetting-password.png)  

## <a name="create-a-function-http-trigger-to-test"></a>Erstellen eines zu testenden HTTP-Funktionstriggers

1. Erstellen Sie eine neue HTTP-Triggerfunktion aus der Funktions-App. ![Erstellen eines neuen HTTP-Triggers](./media/functions-hybrid-powershell/create-http-trigger-function.png)  
1. Ersetzen Sie den PowerShell-Code aus der Vorlage durch den folgenden Code:

    ```powershell
    # Input bindings are passed in via param block.
    param($Request, $TriggerMetadata)
    
    # Write to the Azure Functions log stream.
    Write-Output "PowerShell HTTP trigger function processed a request."
    
    # Note that ContosoUserPassword is a function app setting, so I can access it as $env:ContosoUserPassword
    $UserName = "ContosoUser"
    $securedPassword = ConvertTo-SecureString  $Env:ContosoUserPassword -AsPlainText -Force
    $Credential = [System.management.automation.pscredential]::new($UserName, $SecuredPassword)
    
    # This is the name of the hybrid connection Endpoint.
    $HybridEndpoint = "finance1"
    
    $Script = {
        Param(
            [Parameter(Mandatory=$True)]
            [String] $Service
        )
        Get-Service $Service
    }
    
    Write-Output "Scenario 1: Running command via Invoke-Command"
    Invoke-Command -ComputerName $HybridEndpoint `
                   -Credential $Credential `
                   -Port 5986 `
                   -UseSSL `
                   -ScriptBlock $Script `
                   -ArgumentList "*" `
                   -SessionOption (New-PSSessionOption -SkipCACheck)
    ```

3. Klicken Sie auf „Save“ (Speichern), und führen Sie den Code aus, um ihn zu testen. ![Testen der Funktions-App](./media/functions-hybrid-powershell/test-function-hybrid.png)  

## <a name="managing-other-systems-on-premises"></a>Lokale Verwaltung anderer Systeme

Sie können den verbundenen lokalen Server verwenden, um eine Verbindung mit anderen Servern und Verwaltungssystemen in der lokalen Umgebung herzustellen. Auf diese Weise können Sie Ihre Rechenzentrumsvorgänge über Azure mithilfe von PowerShell-Funktionen verwalten. Mit dem folgenden Skript wird eine PowerShell-Konfigurationssitzung registriert, die unter den angegebenen Anmeldeinformationen ausgeführt wird. Dabei muss es sich um die Anmeldeinformationen für einen Administrator für die Remoteserver handeln. Anschließend können Sie diese Konfiguration verwenden, um auf andere Endpunkte auf dem lokalen Server oder im Rechenzentrum zuzugreifen.

```powershell
# Input bindings are passed in via param block.
param($Request, $TriggerMetadata)

# Write to the Azure Functions log stream.
Write-Host "PowerShell HTTP trigger function processed a request."

# Note that ContosoUserPassword is a function app setting, so I can access it as $env:ContosoUserPassword
$UserName = "ContosoUser"
$SecuredPassword = ConvertTo-SecureString  $Env:ContosoUserPassword -AsPlainText -Force
$Credential = [System.management.automation.pscredential]::new($UserName, $SecuredPassword)

# This is the name of the hybrid connection Endpoint.
$HybridEndpoint = "finance1"

# The remote server that will be conected to run remote PowerShell commands on
$RemoteServer = "finance2"

Write-Output "Use hybrid connection server as a jump box to connect to a remote machine"

# We are registering an endpoint that runs under credentials ($Credential) that has access to the remote server.
$SessionName = "HybridSession"
$ScriptCommand = {
    param (
        [Parameter(Mandatory=$True)]
        $SessionName)

    if (-not (Get-PSSessionConfiguration -Name $SessionName -ErrorAction SilentlyContinue))
    {
        Register-PSSessionConfiguration -Name $SessionName -RunAsCredential $Using:Credential
    }
}

Write-Output "Registering session on hybrid connection jumpbox"
Invoke-Command -ComputerName $HybridEndpoint `
               -Credential $Credential `
               -Port 5986 `
               -UseSSL `
               -ScriptBlock $ScriptCommand `
               -ArgumentList $SessionName `
               -SessionOption (New-PSSessionOption -SkipCACheck)

# Script to run on the jump box to run against the second machine
$RemoteScriptCommand = {
    param (
        [Parameter(Mandatory=$True)]
        $ComputerName)
        # Write out the hostname of the hybrid connection server
        hostname
        # Write out the hostname of the remote server
        Invoke-Command -ComputerName $ComputerName -Credential $Using:Credential -ScriptBlock {hostname} `
                        -UseSSL -Port 5986 -SessionOption (New-PSSessionOption -SkipCACheck)
}

Write-Output "Running command against remote machine via jumpbox by connecting to the PowerShell configuration session"
Invoke-Command -ComputerName $HybridEndpoint `
               -Credential $Credential `
               -Port 5986 `
               -UseSSL `
               -ScriptBlock $RemoteScriptCommand `
               -ArgumentList $RemoteServer `
               -SessionOption (New-PSSessionOption -SkipCACheck) `
               -ConfigurationName $SessionName
```

Ersetzen Sie diese Variablen im Skript oben durch die Werte, die Sie in Ihrer Umgebung verwenden:
* $HybridEndpoint
* $RemoteServer

In den beiden oben aufgeführten Szenarien können Sie mithilfe von PowerShell in Azure Functions und Hybrid Connections eine Verbindung mit lokalen Umgebungen herstellen und diese verwalten. Weitere Informationen zu [Hybrid Connections](../app-service/app-service-hybrid-connections.md) und [PowerShell in Funktionen](./functions-reference-powershell.md) finden Sie in der Dokumentation.

Sie können auch [virtuelle Netzwerke](./functions-create-vnet.md) in Azure verwenden, um sich mit Ihrer lokalen Umgebung über Azure Functions zu verbinden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"] 
> [Weitere Informationen zum Arbeiten mit PowerShell-Funktionen](functions-reference-powershell.md)
