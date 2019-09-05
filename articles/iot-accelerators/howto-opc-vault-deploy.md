---
title: Bereitstellen des OPC Vault-Zertifikatverwaltungsdiensts – Azure | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie den OPC Vault-Zertifikatverwaltungsdienst von Grund auf neu bereitstellen.
author: mregen
ms.author: mregen
ms.date: 08/16/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 1625f0e6f9bfe8297cae2770e63107bf4f19f95e
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2019
ms.locfileid: "70012980"
---
# <a name="build-and-deploy-the-opc-vault-certificate-management-service"></a>Erstellen und Bereitstellen des OPC Vault-Zertifikatverwaltungsdiensts

In diesem Artikel wird erläutert, wie Sie den OPC Vault-Zertifikatverwaltungsdienst in Azure bereitstellen.

> [!NOTE]
> Weitere Informationen zu den Details und eine Anleitung zur Bereitstellung finden Sie im GitHub-[OPC Vault-Repository](https://github.com/Azure/azure-iiot-opc-vault-service).

## <a name="prerequisites"></a>Voraussetzungen

### <a name="install-required-software"></a>Installieren der erforderlichen Software

Der Build- und Bereitstellungsvorgang ist derzeit auf Windows beschränkt.
Sämtliche Beispiele sind für C# .NET Standard geschrieben, das zum Erstellen des Diensts und von Beispielen für die Bereitstellung benötigt wird.
Alle für .NET Standard erforderlichen Tools sind in den .NET Core Tools enthalten. Informationen zu dem, was Sie benötigen, finden Sie [hier](https://docs.microsoft.com/dotnet/articles/core/getting-started).

1. [Installieren von .NET Core 2.1+][dotnet-install].
2. [Installieren von Docker][docker-url] (optional – nur wenn der lokale Docker-Build erforderlich ist).
4. Installieren Sie die [Azure-Befehlszeilentools für PowerShell][powershell-install].
5. Registrieren Sie sich für ein [Azure-Abonnement][azure-free].

### <a name="clone-the-repository"></a>Klonen des Repositorys

Wenn dieser Schritt noch nicht erfolgt ist, klonen Sie dieses GitHub-Repository.  Öffnen Sie eine Eingabeaufforderung oder ein Terminal, und führen Sie Folgendes aus:

```bash
git clone https://github.com/Azure/azure-iiot-opc-vault-service
cd azure-iiot-opc-vault-service 
```

oder klonen Sie das Repository direkt in Visual Studio 2017.

### <a name="build-and-deploy-the-azure-service-on-windows"></a>Erstellen und Bereitstellen des Azure-Diensts unter Windows

Ein PowerShell-Skript bietet eine einfache Möglichkeit zum Bereitstellen des OPC Vault-Microservices und der Anwendung.<br>

1. Öffnen Sie ein PowerShell-Fenster im Repositorystamm. 
3. Wechseln Sie zum Bereitstellungsordner `cd deploy`.
3. Wählen Sie einen Namen für `myResourceGroup` aus, der wahrscheinlich keinen Konflikt mit anderen bereitgestellten Webseiten verursachen wird. Weiter [unten](#website-name-already-in-use) erfahren Sie, wie Webseitennamen basierend auf dem Namen der Ressourcengruppe ausgewählt werden.
5. Starten Sie die Bereitstellung mit `.\deploy.ps1` für die interaktive Installation<br>
oder geben Sie eine vollständige Befehlszeile ein:  
`.\deploy.ps1  -subscriptionName "MySubscriptionName" -resourceGroupLocation "East US" -tenantId "myTenantId" -resourceGroupName "myResourceGroup"`
7. Wenn Sie mit dieser Bereitstellung etwas entwickeln möchten, fügen Sie `-development 1` hinzu, um die Swagger-Benutzeroberfläche zu aktivieren und Debugbuilds bereitzustellen.
6. Folgen Sie den Anweisungen im Skript, um sich bei Ihrem Abonnement anzumelden und zusätzliche Informationen bereitzustellen.
9. Nach einem erfolgreichen Build-und Bereitstellungsvorgang sollte die folgende Meldung angezeigt werden:
   ```
   To access the web client go to:
   https://myResourceGroup.azurewebsites.net

   To access the web service go to:
   https://myResourceGroup-service.azurewebsites.net

   To start the local docker GDS server:
   .\myResourceGroup-dockergds.cmd

   To start the local dotnet GDS server:
   .\myResourceGroup-gds.cmd
   ```

Falls Probleme auftreten, führen Sie die Schritte weiter [unten](#troubleshooting-deployment-failures) aus.

8. Öffnen Sie Ihren bevorzugten Browser und dann die Anwendungsseite: `https://myResourceGroup.azurewebsites.net`
8. Lassen Sie der Web-App und dem OPC Vault-Microservice nach der Bereitstellung ein paar Minuten Zeit zum Aufwärmen. Die Web-Homepage reagiert bei der ersten Verwendung möglicherweise bis zu einer Minute lang nicht, bevor Sie die ersten Antworten erhalten.
11. Sehen Sie sich die geöffnete Swagger-API an: `https://myResourceGroup-service.azurewebsites.net`
13. Starten Sie einen lokalen GDS-Server mit Dotnet-Start `.\myResourceGroup-gds.cmd` oder mit Docker-Start `.\myResourceGroup-dockergds.cmd`.

Übrigens: Es ist möglich, einen Build mit genau denselben Einstellungen erneut bereitzustellen. Beachten Sie, dass bei solch einem Vorgang alle Anwendungsgeheimnisse erneuert und einige Einstellungen in den Azure Active Directory (Azure AD)-Anwendungsregistrierungen eventuell zurückgesetzt werden.

Es ist auch möglich, nur die Binärdateien der Web-App erneut bereitzustellen. Mit dem Parameter `-onlyBuild 1` werden neue ZIP-Pakete des Diensts und die App für die Webanwendungen bereitgestellt.

Nach erfolgreicher Bereitstellung können Sie mit der Verwendung der Dienste beginnen: [Verwalten des OPC Vault-Zertifikatverwaltungsdiensts](howto-opc-vault-manage.md)

## <a name="delete-the-services-from-the-subscription"></a>Löschen der Dienste aus dem Abonnement

1. Melden Sie sich beim Azure-Portal an: `https://portal.azure.com`.
2. Wechseln Sie zu der Ressourcengruppe, in der der Dienst bereitgestellt wurde.
3. Wählen Sie `Delete resource group` aus, und bestätigen Sie dies.
4. Nach kurzer Zeit werden alle bereitgestellten Dienstkomponenten gelöscht.
5. Wechseln Sie jetzt zu `Azure Active Directory/App registrations`.
6. Es sollten drei Registrierungen für jede bereitgestellte Ressourcengruppe mit den folgenden Namen aufgeführt werden: `resourcegroup-client`, `resourcegroup-module`, `resourcegroup-service`.
Jede Registrierung muss einzeln gelöscht werden.
7. Jetzt sind alle bereitgestellten Komponenten entfernt.

## <a name="troubleshooting-deployment-failures"></a>Beheben von Problemen bei der Bereitstellung

### <a name="resource-group-name"></a>Ressourcengruppenname

Verwenden Sie einen kurzen und einfachen Namen für die Ressourcengruppe.  Weil der Name auch zum Benennen von Ressourcen und des URL-Präfixes für den Dienst verwendet wird, muss er den Namenskonventionen für Ressourcen entsprechen.  

### <a name="website-name-already-in-use"></a>Websitename wird bereits verwendet.

Es ist möglich, dass der Name der Website bereits verwendet wird.  Wenn dieser Fehler auftritt, müssen Sie einen anderen Ressourcengruppennamen verwenden. Die vom Bereitstellungsskript verwendeten Hostnamen lauten: https://resourcegroupname.azurewebsites.net und https://resourgroupname-service.azurewebsites.net.
Andere Namen von Diensten werden durch die Kombination aus Kurznamenshashes erstellt und führen wahrscheinlich nicht zum Konflikt mit anderen Diensten.

### <a name="azure-active-directory-azure-ad-registration"></a>Azure Active Directory (Azure AD)-Registrierung 

Über das Bereitstellungsskript wird versucht, drei Azure AD-Anwendungen in Azure Active Directory zu registrieren.  
Je nach Ihren Berechtigungen im ausgewählten Azure AD-Mandanten schlägt dieser Vorgang möglicherweise fehl.   Es gibt zwei Optionen:

1. Wenn Sie in einer Liste von Mandanten einen Azure AD-Mandanten ausgewählt haben, starten Sie das Skript neu, und wählen Sie einen anderen Mandanten aus.
2. Alternativ können Sie einen privaten Azure AD-Mandanten in einem anderen Abonnement bereitstellen, das Skript neu starten und diesen Mandanten zur Verwendung auswählen.

## <a name="deployment-script-options"></a>Optionen des Bereitstellungsskripts

Dem Skript werden die folgenden Parameter übergeben:


```
-resourceGroupName
```

Dies kann der Name einer bestehenden oder einer neuen Ressourcengruppe sein.

```
-subscriptionId
```


Optional kann die Abonnement-ID verwendet werden, unter der die Ressourcen bereitgestellt werden.

```
-subscriptionName
```


Eine Alternative ist die Verwendung des Abonnementnamens.

```
-resourceGroupLocation
```


Optional kann dies ein Standort für die Ressourcengruppe sein. Wenn diese Option angegeben ist, wird versucht, eine neue Ressourcengruppe an dieser Position zu erstellen.


```
-tenantId
```


Der zu verwendende Azure AD-Mandant. 

```
-development 0|1
```

Optional – zur Bereitstellung für die Entwicklung. Verwenden Sie das Debugbuild, und legen Sie die ASP.NET-Umgebung auf „Entwicklung“ fest. Erstellen Sie ‚.publishsettings‘ für den Import in Visual Studio 2017, damit die App und der Dienst direkt bereitgestellt werden können.

```
-onlyBuild 0|1
```

Optional, um nur die Web-Apps neu zu erstellen und erneut bereitzustellen und um die Docker-Container neu zu erstellen.

[azure-free]:https://azure.microsoft.com/free/
[powershell-install]:https://azure.microsoft.com/downloads/#powershell
[docker-url]: https://www.docker.com/
[dotnet-install]: https://www.microsoft.com/net/learn/get-started

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie OPC Vault von Grund auf bereitstellen, lautet unser Vorschlag für den nächsten Schritt:

> [!div class="nextstepaction"]
> [Verwalten von OPC Vault](howto-opc-vault-manage.md)
