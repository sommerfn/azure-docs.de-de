---
title: 'Problembehandlung für Azure Application Gateway mit App Service: Umleitung zur App Service-URL'
description: Dieser Artikel enthält Informationen dazu, wie sich das Umleitungsproblem beheben lässt, wenn Azure Application Gateway mit Azure App Service verwendet wird.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 07/19/2019
ms.author: absha
ms.openlocfilehash: 7baadfb549b19bb12757c82d723578202b5cf8ad
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68347881"
---
# <a name="troubleshoot-app-service-issues-in-application-gateway"></a>Behandeln von App Service-Problemen in Application Gateway

Es wird beschrieben, wie Sie Probleme diagnostizieren und lösen, die auftreten, wenn App Server als Back-End-Ziel mit Application Gateway verwendet wird.

## <a name="overview"></a>Übersicht

In diesem Artikel erfahren Sie, wie Sie die folgenden Probleme beheben können:

> [!div class="checklist"]
> * App Service-URL wird bei einer Umleitung im Browser angezeigt
> * ARRAffinity-Cookiedomäne von App Service ist auf App Service-Hostname (example.azurewebsites.net) und nicht auf ursprünglichen Host festgelegt

Wenn eine Back-End-Anwendung eine Umleitungsantwort sendet, empfiehlt es sich u.U., den Client an eine andere als die von der Back-End-Anwendung angegebene URL umzuleiten. Beispielsweise sollten Sie dies tun, wenn ein App-Dienst hinter einem Anwendungsgateway gehostet wird und erfordert, dass der Client eine Umleitung zu seinem relativen Pfad durchführt. (Beispielsweise eine Umleitung von „contoso.azurewebsites.net/path1“ zu „contoso.azurewebsites.net/path2“.) Wenn der App Service eine Umleitungsantwort sendet, verwendet er den gleichen Hostnamen im Adressheader seiner Antwort wie in der Anforderung, die er vom Anwendungsgateway empfängt. So sendet der Client die Anforderung direkt an „contoso.azurewebsites.net/path2“ und durchläuft nicht das Anwendungsgateway („contoso.com/path2“). Das Umgehen des Anwendungsgateways ist nicht wünschenswert.

Dieses Problem kann folgende Hauptursachen haben:

- Sie haben für App Service eine Umleitung konfiguriert. Diese kann im einfachsten Fall auf einen nachgestellten Schrägstrich zurückzuführen sein, der der Anforderung hinzugefügt wurde.
- Die Umleitung wird durch die Nutzung der Azure AD-Authentifizierung ausgelöst.

Wenn Sie App Services hinter Application Gateway verwenden, unterscheidet sich zudem der Domänenname, der der Application Gateway-Instanz (example.com) zugeordnet ist, vom Domänennamen des App Service (z. B. „example.azurewebsites.net“). Sie werden daher merken, dass der Domänenname für das ARRAffinity-Cookie, das vom App Service festgelegt wird, über den Domänennamen „example.azurewebsites.net“ verfügt. Dies ist nicht wünschenswert. Der ursprüngliche Hostname (example.com) sollte dem Domänennamenwert im Cookie entsprechen.

## <a name="sample-configuration"></a>Beispielkonfiguration

- HTTP-Listener: Basislistener oder Listener für mehrere Standorte
- Back-End-Adresspool: App Service
- HTTP-Einstellungen: „Hostnamen aus Back-End-Adresse auswählen“ aktiviert
- Test: „Pick Hostname from Backend HTTP Settings“ (Hostnamen aus Back-End-HTTP-Einstellungen auswählen)

## <a name="cause"></a>Ursache

Da App Service ein mehrinstanzenfähiger Dienst ist, nutzt er den Hostheader in der Anforderung zur Weiterleitung der Anforderung an den richtigen Endpunkt. Der Standarddomänenname der App Service-Instanzen („*.azurewebsites.net“, z. B. „contoso.azurewebsites.net“) unterscheidet sich aber vom Domänennamen des Anwendungsgateways (z. B. „contoso.com“). Da die ursprüngliche Anforderung vom Client über den Domänennamen (contoso.com) des Anwendungsgateways als Hostname verfügt, müssen Sie das Anwendungsgateway konfigurieren, um den Hostnamen in der ursprünglichen Anforderung in den Hostnamen der App Service-Instanz zu ändern (für die Weiterleitung der Anforderung an das App Service-Back-End).  Verwenden Sie hierfür den Switch „Pick Hostname from Backend Address“ (Hostnamen aus Back-End-Adresse auswählen) in der Konfiguration der HTTP-Einstellungen von Application Gateway und den Switch „Pick Hostname from Backend HTTP Settings“ (Hostnamen aus Back-End-HTTP-Einstellungen auswählen) in der Konfiguration der Integritätstests.



![App Service (1)](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Wenn die App Service-Instanz nun eine Umleitung durchführt, wird anstelle des ursprünglichen Hostnamens „contoso.com“ der außer Kraft gesetzte Hostname „contoso.azurewebsites.net“ im Adressheader verwendet, falls keine andere Konfiguration vorgenommen wurde. Im Folgenden sehen Sie ein Beispiel für Anforderungs- und Antwortheader.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://contoso.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=contoso.azurewebsites.net

X-Powered-By: ASP.NET
```
Im obigen Beispiel verfügt der Antwortheader über den Statuscode 301 für die Umleitung. Außerdem entspricht der Adressheader nicht dem ursprünglichen Hostnamen „www.contoso.com“, sondern dem von App Service.

## <a name="solution-rewrite-the-location-header"></a>Lösung: Erneutes Generieren des Adressheaders

Sie müssen den Hostnamen im Adressheader auf den Domänennamen des Anwendungsgateways festlegen. Erstellen Sie hierzu eine [Regel zum erneuten Generieren](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) mit einer Bedingung, die auswertet, ob der Adressheader in der Antwort „azurewebsites.net“ enthält und eine Aktion zum erneuten Generieren des Adressheaders durchführt, sodass dieser den Hostnamen des Anwendungsgateways aufweist.  Eine Anleitung finden Sie unter [Ändern einer Umleitungs-URL](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#modify-a-redirection-url).

> [!NOTE]
> Die Unterstützung für das erneute Generieren von HTTP-Headern ist nur für die [Standard_V2- und WAF_v2-SKU](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) von Application Gateway verfügbar. Falls Sie eine V1-SKU verwenden, empfehlen wir Ihnen dringend die [Migration von V1 zu V2](https://docs.microsoft.com/azure/application-gateway/migrate-v1-v2), damit Sie die erneute Generierung und andere [erweiterte Funktionen](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#feature-comparison-between-v1-sku-and-v2-sku) der V2-SKU nutzen können.

## <a name="alternate-solution-use-app-services-custom-domain-instead-of-default-domain-name"></a>Alternativlösung: Verwenden der benutzerdefinierten App Service-Domäne anstelle des Standarddomänennamens

Bei Verwendung der V1-SKU können Sie den Adressheader nicht erneut generieren, da diese Funktion nur für die V2-SKU verfügbar ist. Zum Beheben des Umleitungsproblems müssen Sie denselben Hostnamen, der von Application Gateway empfangen wird, auch an den App Service übergeben, anstatt eine Außerkraftsetzung des Hosts durchzuführen.

Anschließend führt App Service Umleitungen nicht für den eigenen Header, sondern für denselben ursprünglichen Hostheader durch, der auf Application Gateway verweist.

Hierzu müssen Sie über eine benutzerdefinierte Domäne verfügen und die im Folgenden genannten Schritte ausführen:

- Tragen Sie die Domäne in die Liste der benutzerdefinierten Domänen von App Service ein. Dazu benötigen Sie in Ihrer benutzerdefinierten Domäne einen CNAME-Eintrag, der auf den FQDN von App Service verweist. Weitere Informationen finden Sie unter [Zuordnen eines vorhandenen benutzerdefinierten DNS-Namens zu Azure App Service](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain).

![App Service (2)](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- Anschließend kann Ihrer App Service-Instanz der Hostname „www.contoso.com“ übergeben werden. Ändern Sie nun den CNAME-Eintrag im DNS so, dass er wieder auf den FQDN von Application Gateway verweist. Beispiel: „appgw.eastus.cloudapp.azure.com“.

- Stellen Sie sicher, dass die Domäne „www.contoso.com“ in den FQDN von Application Gateway aufgelöst wird, wenn Sie eine DNS-Abfrage ausführen.

- Deaktivieren Sie für Ihren benutzerdefinierten Test „Pick Hostname from Backend HTTP Settings“ (Hostnamen aus Back-End-HTTP-Einstellungen auswählen). Dazu können Sie im Azure-Portal in den Testeinstellungen das entsprechende Kontrollkästchen deaktivieren oder in PowerShell bei Nutzung des Befehls „Set-AzApplicationGatewayProbeConfig“ auf die Option „-PickHostNameFromBackendHttpSettings“ verzichten. Geben Sie im Feld für den Hostnamen des Tests den FQDN „example.azurewebsites.net“ von App Service ein, da die von Application Gateway gesendeten Testanforderungen diesen Namen im Hostheader enthalten.

  > [!NOTE]
  > Stellen Sie vor dem nächsten Schritt sicher, dass der benutzerdefinierte Test nicht Ihren Back-End-HTTP-Einstellungen zugeordnet ist. Für Ihre HTTP-Einstellungen ist zu diesem Zeitpunkt nach wie vor die Option „Hostnamen aus Back-End-Adresse auswählen“ aktiviert.

- Passen Sie die HTTP-Einstellungen von Application Gateway so an, dass „Hostnamen aus Back-End-Adresse auswählen“ deaktiviert wird. Dazu können Sie im Azure-Portal das entsprechende Kontrollkästchen deaktivieren oder in PowerShell bei Nutzung des Befehls „Set-AzApplicationGatewayBackendHttpSettings“ auf die Option „-PickHostNameFromBackendAddress“ verzichten.

- Ordnen Sie den benutzerdefinierten Test wieder den Back-End-HTTP-Einstellungen zu, und überprüfen Sie die Back-End-Integrität.

- Der Hostname „www.contoso.com“ sollte nun von Application Gateway an die App Service-Instanz weitergeleitet und außerdem für die Umleitung verwendet werden. Im Folgenden sehen Sie ein Beispiel für Anforderungs- und Antwortheader.

Um die oben beschriebenen Schritte unter Verwendung von PowerShell für ein vorhandenes Setup zu implementieren, führen Sie das nachfolgende PowerShell-Beispielskript aus. Beachten Sie, dass die „-PickHostname“-Switches in der Konfiguration für die Test- und HTTP-Einstellungen nicht verwendet wurden.

```azurepowershell-interactive
$gw=Get-AzApplicationGateway -Name AppGw1 -ResourceGroupName AppGwRG
Set-AzApplicationGatewayProbeConfig -ApplicationGateway $gw -Name AppServiceProbe -Protocol Http -HostName "example.azurewebsites.net" -Path "/" -Interval 30 -Timeout 30 -UnhealthyThreshold 3
$probe=Get-AzApplicationGatewayProbeConfig -Name AppServiceProbe -ApplicationGateway $gw
Set-AzApplicationGatewayBackendHttpSettings -Name appgwhttpsettings -ApplicationGateway $gw -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 30
Set-AzApplicationGateway -ApplicationGateway $gw
```
  ```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: www.contoso.com

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
  ```
  ## <a name="next-steps"></a>Nächste Schritte

Sollte sich das Problem mit den oben genannten Schritten nicht beheben lassen, erstellen Sie ein [Supportticket](https://azure.microsoft.com/support/options/).
