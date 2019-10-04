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
ms.openlocfilehash: f577059e1ebf70e3a9dfe9e538a9d3d49d7c8e96
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71199997"
---
# <a name="build-and-deploy-the-opc-vault-certificate-management-service"></a>Erstellen und Bereitstellen des OPC Vault-Zertifikatverwaltungsdiensts

In diesem Artikel wird erläutert, wie Sie den OPC Vault-Zertifikatverwaltungsdienst in Azure bereitstellen.

> [!NOTE]
> Weitere Informationen finden Sie im [OPC Vault-Repository](https://github.com/Azure/azure-iiot-opc-vault-service) von GitHub.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="install-required-software"></a>Installieren der erforderlichen Software

Der Build- und Bereitstellungsvorgang ist derzeit auf Windows beschränkt.
Sämtliche Beispiele sind für C# .NET Standard geschrieben, das Sie zum Erstellen des Diensts und von Beispielen für die Bereitstellung benötigen.
Alle für .NET Standard erforderlichen Tools sind in den .NET Core Tools enthalten. Weitere Informationen finden Sie unter [Erste Schritte mit .NET Core](https://docs.microsoft.com/dotnet/articles/core/getting-started).

1. [Installieren von .NET Core 2.1+][dotnet-install].
2. [Installieren Sie Docker][docker-url] (optional – nur wenn der lokale Docker-Build erforderlich ist).
4. Installieren Sie die [Azure-Befehlszeilentools für PowerShell][powershell-install].
5. Registrieren Sie sich für ein [Azure-Abonnement][azure-free].

### <a name="clone-the-repository"></a>Klonen des Repositorys

Wenn dieser Schritt noch nicht erfolgt ist, klonen Sie dieses GitHub-Repository. Öffnen Sie eine Eingabeaufforderung oder ein Terminal, und führen Sie Folgendes aus:

```bash
git clone https://github.com/Azure/azure-iiot-opc-vault-service
cd azure-iiot-opc-vault-service 
```

Alternativ können Sie das Repository auch direkt in Visual Studio 2017 klonen.

### <a name="build-and-deploy-the-azure-service-on-windows"></a>Erstellen und Bereitstellen des Azure-Diensts unter Windows

Ein PowerShell-Skript bietet eine einfache Möglichkeit zum Bereitstellen des OPC Vault-Microservices und der Anwendung.

1. Öffnen Sie ein PowerShell-Fenster im Repositorystamm. 
3. Wechseln Sie zum Bereitstellungsordner `cd deploy`.
3. Wählen Sie einen Namen für `myResourceGroup` aus, der wahrscheinlich keinen Konflikt mit anderen bereitgestellten Webseiten verursachen wird. Lesen Sie dazu den Abschnitt „Websitename wird bereits verwendet“ weiter unten in diesem Artikel.
5. Starten Sie die Bereitstellung mit `.\deploy.ps1` für eine interaktive Installation, oder geben Sie eine vollständige Befehlszeile ein:  
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

   > [!NOTE]
   > Sollten Probleme auftreten, lesen Sie den Abschnitt „Beheben von Problemen bei der Bereitstellung“ weiter unten in diesem Artikel.

8. Öffnen Sie Ihren bevorzugten Browser und dann die Anwendungsseite: `https://myResourceGroup.azurewebsites.net`
8. Lassen Sie der Web-App und dem OPC Vault-Microservice nach der Bereitstellung ein paar Minuten Zeit zum Aufwärmen. Die Web-Homepage reagiert bei der ersten Verwendung möglicherweise bis zu einer Minute lang nicht, bis Sie die ersten Antworten erhalten.
11. Wenn Sie sich die Swagger-API ansehen möchten, öffnen Sie: `https://myResourceGroup-service.azurewebsites.net`.
13. Zum Starten eines lokalen GDS-Servers mit „dotnet“, starten Sie `.\myResourceGroup-gds.cmd`. Bei Docker starten Sie `.\myResourceGroup-dockergds.cmd`.

Es ist möglich, einen Build mit genau denselben Einstellungen erneut bereitzustellen. Beachten Sie, dass bei solch einem Vorgang alle Anwendungsgeheimnisse erneuert und einige Einstellungen in den Azure Active Directory (Azure AD)-Anwendungsregistrierungen eventuell zurückgesetzt werden.

Es ist auch möglich, nur die Web-App-Binärdateien erneut bereitzustellen. Mit dem Parameter `-onlyBuild 1` werden neue ZIP-Pakete des Diensts und die App für die Webanwendungen bereitgestellt.

Nach erfolgreicher Bereitstellung können Sie mit der Nutzung der Dienste beginnen. Lesen Sie dazu [Verwalten des OPC Vault-Zertifikatverwaltungsdiensts](howto-opc-vault-manage.md).

## <a name="delete-the-services-from-the-subscription"></a>Löschen der Dienste aus dem Abonnement

Das geht so:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wechseln Sie zu der Ressourcengruppe, in der der Dienst bereitgestellt wurde.
3. Klicken Sie auf **Ressourcengruppe löschen**, und bestätigen Sie den Vorgang.
4. Nach kurzer Zeit werden alle bereitgestellten Dienstkomponenten gelöscht.
5. Wechseln Sie zu **Azure Active Directory** > **App-Registrierungen**.
6. Bei jeder bereitgestellten Ressourcengruppe sollten drei Registrierungen aufgeführt werden. Die Registrierungen haben folgende Namen: `resourcegroup-client`, `resourcegroup-module` und `resourcegroup-service`. Löschen Sie jede Registrierung einzeln.

Jetzt sind alle bereitgestellten Komponenten entfernt.

## <a name="troubleshooting-deployment-failures"></a>Beheben von Problemen bei der Bereitstellung

### <a name="resource-group-name"></a>Ressourcengruppenname

Verwenden Sie einen kurzen und einfachen Ressourcengruppennamen. Der Name wird auch zum Benennen von Ressourcen und des Präfixes der Dienst-URL verwendet. Daher muss er den Anforderungen für Ressourcenbenennung entsprechen.  

### <a name="website-name-already-in-use"></a>Websitename wird bereits verwendet.

Es ist möglich, dass der Name der Website bereits verwendet wird. Sie müssen einen anderen Ressourcengruppennamen verwenden. Die vom Bereitstellungsskript verwendeten Hostnamen lauten: https://resourcegroupname.azurewebsites.net und https://resourgroupname-service.azurewebsites.net.
Andere Namen für Dienste werden durch die Kombination von Kurznamenhashes erstellt und wahrscheinlich keinen Konflikt mit anderen Diensten verursachen.

### <a name="azure-ad-registration"></a>Azure AD-Registrierung 

Das Bereitstellungsskript versucht, drei Azure AD-Anwendungen in Azure AD zu registrieren. Je nach Ihren Berechtigungen im ausgewählten Azure AD-Mandanten schlägt dieser Vorgang möglicherweise fehl. Es gibt zwei Optionen:

- Wenn Sie in einer Liste von Mandanten einen Azure AD-Mandanten ausgewählt haben, starten Sie das Skript neu, und wählen Sie einen anderen Mandanten aus.
- Stellen Sie alternativ einen privaten Azure AD-Mandanten in einem anderen Abonnement bereit. Starten Sie das Skript erneut, und wählen Sie aus, das Sie es verwenden möchten.

## <a name="deployment-script-options"></a>Optionen des Bereitstellungsskripts

Dem Skript werden die folgenden Parameter übergeben:


```
-resourceGroupName
```

Dies kann der Name einer bereits bestehenden oder einer neuen Ressourcengruppe sein.

```
-subscriptionId
```


Dies ist die ID des Abonnements, in dem Ressourcen bereitgestellt werden sollen. Sie ist optional.

```
-subscriptionName
```


Alternativ können Sie den Namen des Abonnements verwenden.

```
-resourceGroupLocation
```


Dies ist ein Ressourcengruppenstandort. Wenn er angegeben wurde, versucht dieser Parameter, eine neue Ressourcengruppe an diesem Standort zu erstellen. Dieser Parameter ist ebenfalls optional.


```
-tenantId
```


Dies ist der Azure AD-Mandant, der verwendet werden soll. 

```
-development 0|1
```

Er muss für die Entwicklung bereitgestellt werden. Verwenden Sie einen Debug-Build, und legen Sie die ASP.NET-Umgebung auf „Entwicklung“ fest. Erstellen Sie `.publishsettings` für den Import in Visual Studio 2017, damit die App und der Dienst direkt bereitgestellt werden können. Dieser Parameter ist ebenfalls optional.

```
-onlyBuild 0|1
```

Er dient zum Neuerstellen und erneuten Bereitstellen nur der Web-Apps sowie zum Neuerstellen der Docker-Container. Dieser Parameter ist ebenfalls optional.

[azure-free]:https://azure.microsoft.com/free/
[powershell-install]:https://azure.microsoft.com/downloads/#powershell
[docker-url]: https://www.docker.com/
[dotnet-install]: https://www.microsoft.com/net/learn/get-started

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie OCP Vault von Grund auf bereitgestellt wird, können Sie sich jetzt zu Folgendem informieren:

> [!div class="nextstepaction"]
> [Verwalten von OPC Vault](howto-opc-vault-manage.md)
