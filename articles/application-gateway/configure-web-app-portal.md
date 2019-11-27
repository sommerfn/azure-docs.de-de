---
title: Verwalten von Datenverkehr zu mehrinstanzenfähigen Apps mithilfe des Portals
titleSuffix: Azure Application Gateway
description: In diesem Artikel erfahren Sie, wie Sie Azure App Service-Web-Apps als Mitglied in Back-End-Pools für ein vorhandenes oder neues Anwendungsgateway konfigurieren.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: absha
ms.openlocfilehash: 0ec417b3c7a025d2d05bdd74ec683a2891c3b0de
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74075163"
---
# <a name="configure-app-service-with-application-gateway"></a>Konfigurieren von App Service mit Application Gateway

Da es sich bei App Service nicht um eine dedizierte Bereitstellung, sondern um einen mehrinstanzenfähigen Dienst handelt, wird der Hostheader in der eingehenden Anforderung verwendet, um die Anforderung für den richtigen App Service-Endpunkt aufzulösen. In der Regel unterscheidet sich der DNS-Name der Anwendung – der wiederum dem DNS-Namen entspricht, der dem Anwendungsgateway für den App Service zugeordnet ist – vom Domänennamen des Back-End-Diensts. Deshalb ist der Hostheader, der in der vom Anwendungsgateway empfangenen ursprünglichen Anforderung enthalten ist, ein anderer als der Hostname des Back-End-Diensts. Wenn der Hostheader in der Anforderung vom Anwendungsgateway also nicht in den Hostnamen des Back-End-Diensts geändert wird, können mehrinstanzenfähige Back-Ends die Anforderung nicht zum richtigen Endpunkt auflösen.

Application Gateway verfügt über den Switch `Pick host name from backend address`, bei dem der Hostheader der Anforderung durch den Hostnamen des Back-Ends überschrieben wird, wenn die Anforderung vom Application Gateway an das Back-End geleitet wird. So werden mehrinstanzenfähige Back-Ends unterstützt, z. B. Azure App Service und API Management. 

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
>
> - Erstellen eines Back-End-Pools und Hinzufügen eines App-Diensts
> - Erstellen von HTTP-Einstellungen und benutzerdefiniertes Testen mit aktivierten „Pick Hostname“-Parametern (Hostname auswählen)

## <a name="prerequisites"></a>Voraussetzungen

- Anwendungsgateway: Wenn Sie noch kein Anwendungsgateway haben, erhalten Sie [hier](https://docs.microsoft.com/azure/application-gateway/quick-create-portal) Informationen zum Erstellen eines Anwendungsgateways.
- App-Dienst: Wenn Sie noch keinen App-Dienst haben, finden Sie in der [App Service-Dokumentation](https://docs.microsoft.com/azure/app-service/) weitere Informationen.

## <a name="add-app-service-as-backend-pool"></a>Hinzufügen eines App-Diensts als Back-End-Pool

1. Öffnen Sie im Azure-Portal die Konfigurationsansicht Ihres Anwendungsgateways.

2. Klicken Sie im Bereich **Back-End-Pools** auf **Hinzufügen**, um einen neuen Back-End-Pool zu erstellen.

3. Geben Sie dem Back-End-Pool einen passenden Namen. 

4. Klicken Sie unter **Ziele** auf das Dropdown, und wählen Sie als Option **App-Dienste** aus.

5. Direkt unterhalb des **Ziele**-Dropdown wird ein weiteres Dropdown angezeigt, in dem eine Liste Ihrer App-Dienste enthalten ist. Wählen Sie in dieser Dropdownliste den App-Dienst aus, den Sie als Back-End-Poolmitglied hinzufügen möchten, und klicken Sie auf „Hinzufügen“.

   ![Screenshot: App-Dienst-Backend](./media/configure-web-app-portal/backendpool.png)
   
   > [!NOTE]
   > In der Dropdownliste sind nur die App Services enthalten, die sich unter demselben Abonnement wie Ihr Application Gateway befinden. Wenn Sie einen App Service verwenden möchten, der sich unter einem anderen Abonnement als dem Abonnement des Application Gateway befindet, sollten Sie anstelle von **App Services** in der Dropdownliste **Ziele** die Option **IP-Adresse oder Hostname** wählen und den Hostnamen des App Service eingeben (z. B. „azurewebsites.net“).

## <a name="create-http-settings-for-app-service"></a>Erstellen von HTTP-Einstellungen für App-Dienste

1. Klicken Sie unter **HTTP-Einstellungen** auf **Hinzufügen**, um eine neue HTTP-Einstellung zu erstellen.

2. Geben Sie einen Namen für die HTTP-Einstellung ein, und aktivieren oder deaktivieren Sie je nach Ihren Anforderung die Option „Cookiebasierte Affinität“.

3. Wählen Sie je nach Anwendungsfall als Protokoll entweder HTTP oder HTTPS aus. 

   > [!NOTE]
   > Bei Auswahl von HTTPS müssen Sie kein Authentifizierungszertifikat oder vertrauenswürdiges Stammzertifikat hochladen, um das App Service-Back-End auf die Whitelist zu setzen, da es sich bei App Service um einen vertrauenswürdigen Azure-Dienst handelt.

4. Aktivieren Sie das Kontrollkästchen **Für App Service verwenden**. Beachten Sie, dass die Switches `Create a probe with pick host name from backend address` und `Pick host name from backend address` automatisch aktiviert werden. Bei `Pick host name from backend address` wird der Hostheader in der Anforderung durch den Hostnamen des Back-Ends überschrieben, wenn die Anforderung vom Application Gateway an das Back-End geleitet wird.  

   Bei `Create a probe with pick host name from backend address` wird automatisch ein Integritätstest erstellt und dieser HTTP-Einstellung zugeordnet. Sie müssen für diese HTTP-Einstellung keinen anderen Integritätstest erstellen. Sie können überprüfen, ob ein neuer Test mit dem Namen <HTTP Setting name><Unique GUID> der Liste mit den Integritätstests hinzugefügt wurde und bereits über den Switch `Pick host name from backend http settings enabled` verfügt.

   Falls Sie bereits über eine oder mehrere HTTP-Einstellungen verfügen, die für App Service verwendet werden, und wenn für diese HTTP-Einstellungen das gleiche Protokoll wie für Ihre Erstellung genutzt wird, erhalten Sie anstelle des Switch `Create a probe with pick host name from backend address` eine Dropdownliste zum Auswählen eines benutzerdefinierten Tests. Der Grund ist, dass bereits eine HTTP-Einstellung mit App Service vorhanden ist – und daher auch ein Integritätstest mit dem Switch `Pick host name from backend http settings enabled`. Wählen Sie in der Dropdownliste diesen benutzerdefinierten Test aus.

5. Klicken Sie auf **OK**, um die HTTP-Einstellung zu erstellen.

   ![Screenshot: HTTP-Einstellung 1](./media/configure-web-app-portal/http-setting1.png)

   ![Screenshot: HTTP-Einstellung 2](./media/configure-web-app-portal/http-setting2.png)



## <a name="create-rule-to-tie-the-listener-backend-pool-and-http-setting"></a>Erstellen einer Regel zum Binden des Listeners, des Back-End-Pools und der HTTP-Einstellung

1. Klicken Sie unter **Regeln** auf **Basic**, um eine neue Regel für den Tarif „Basic“ zu erstellen.

2. Wählen Sie einen passenden Namen aus, und wählen Sie den Listener aus, der die einkommenden Anforderungen für den App-Dienst entgegennimmt.

3. Wählen Sie im Dropdown für **Back-End-Pool** den Back-End-Pool aus, den Sie oben erstellt haben.

4. Wählen Sie im Dropdown für **HTTP-Einstellung** die HTTP-Einstellung aus, die Sie oben erstellt haben.

5. Klicken Sie auf **OK**, um diese Regel zu speichern.

   ![Regel](./media/configure-web-app-portal/rule.png)

## <a name="additional-configuration-in-case-of-redirection-to-app-services-relative-path"></a>Zusätzliche Konfiguration bei einer Umleitung an den relativen App Service-Pfad

Wenn der App Service eine Umleitungsantwort an den Client sendet, um eine Umleitung zum relativen Pfad durchzuführen (z. B. eine Umleitung von „contoso.azurewebsites.net/path1“ zu „contoso.azurewebsites.net/path2“), wird im Adressheader der Antwort der gleiche Hostname wie in der Anforderung verwendet, die vom Anwendungsgateway gesendet wurde. So sendet der Client die Anforderung direkt an „contoso.azurewebsites.net/path2“ und durchläuft nicht das Anwendungsgateway („contoso.com/path2“). Das Umgehen des Anwendungsgateways ist nicht wünschenswert.

Wenn in Ihrem Fall Szenarien vorhanden sind, in denen der App Service eine Umleitungsantwort an den Client senden muss, sollten Sie die [zusätzlichen Schritte zum erneuten Generieren des Adressheaders](https://docs.microsoft.com/azure/application-gateway/troubleshoot-app-service-redirection-app-service-url#sample-configuration) ausführen.

## <a name="restrict-access"></a>Beschränken des Zugriffs

Die in diesen Beispielen bereitgestellten Web-Apps verwenden öffentliche IP-Adressen, auf die aus dem Internet direkt zugegriffen werden kann. Dies hilft bei der Problembehandlung, wenn Sie von einem neuen Feature erfahren und neue Sachen ausprobieren. Wenn Sie jedoch ein Feature in einer Produktionsumgebung bereitstellen möchten, ist es sinnvoll, stärkere Einschränkungen vorzusehen.

Eine Möglichkeit, wie Sie den Zugriff auf Ihre Web-Apps einschränken können, besteht in der Verwendung von [Statischen Azure App Service-IP-Einschränkungen](../app-service/app-service-ip-restrictions.md). Beispielsweise können Sie die Web-App so einschränken, dass sie nur Datenverkehr vom Anwendungsgateway empfängt. Verwenden Sie die IP-Einschränkungsfunktion von App Service, um die Anwendungsgateway-VIP als einzige Adresse aufzulisten, auf die zugegriffen werden kann.

## <a name="next-steps"></a>Nächste Schritte

[In diesem Artikel](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview) finden Sie weitere Informationen zu App Service und anderen Möglichkeiten der Application Gateway-Unterstützung für mehrinstanzenfähige Back-Ends.
