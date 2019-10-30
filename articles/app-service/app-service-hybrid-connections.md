---
title: Hybridverbindungen – Azure App Service | Microsoft-Dokumentation
description: 'Gewusst wie: Erstellen und Verwenden von Hybridverbindungen für den Zugriff auf Ressourcen in unterschiedlichen Netzwerken'
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 72874e7b96e2ec8909a325b5ae598b900ebe8079
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72791897"
---
# <a name="azure-app-service-hybrid-connections"></a>Azure App Service-Hybridverbindungen #

Hybrid Connections ist sowohl ein Dienst in Azure als auch ein Feature in Azure App Service. Als Dienst gehen seine Einsatzmöglichkeiten und Funktionen über die von Azure App Service hinaus. Weitere Informationen zu Hybrid Connections und der Verwendung außerhalb von App Service finden Sie unter [Azure Relay Hybrid Connections][HCService].

In App Service kann mithilfe von Hybridverbindungen auf Anwendungsressourcen in anderen Netzwerken zugegriffen werden. Es ermöglicht den Zugriff von Ihrer App auf einen Anwendungsendpunkt. Die Funktion bietet keine alternative Möglichkeit für den Zugriff auf Ihre Anwendung. Bei der Verwendung in App Service entspricht jede Hybridverbindung einer Kombination aus einem einzelnen TCP-Host und einem Port. Dies bedeutet, dass sich der Hybridverbindungsendpunkt in einem beliebigen Betriebssystem und einer beliebigen Anwendung befinden kann, vorausgesetzt, der Zugriff erfolgt über einen TCP-Lauschport. Das Feature Hybrid Connections besitzt keine Informationen über das Anwendungsprotokoll oder den abzurufenden Inhalt und benötigt diese Informationen auch nicht. Sie ermöglichen lediglich den Netzwerkzugriff.  


## <a name="how-it-works"></a>So funktioniert's ##
Das Feature Hybrid Connections besteht aus zwei ausgehenden Aufrufen von Azure Service Bus Relay. Es gibt eine Verbindung von einer Bibliothek auf dem Host, auf dem die App in App Service ausgeführt wird. Außerdem besteht eine Verbindung vom Hybridverbindungs-Manager (HCM) zu Service Bus Relay. Der HCM ist ein Relaydienst, den Sie in dem Netzwerk bereitstellen, das die Ressource hostet, auf die Sie zugreifen möchten. 

Über die beiden verknüpften Verbindungen verfügt Ihre App über einen TCP-Tunnel zu einer festen Host:Port-Kombination auf der anderen Seite des HCM. Die Verbindung verwendet zum Schutz TLS 1.2 und zur Authentifizierung/Autorisierung SAS-Schlüssel.    

![Allgemeines Flussdiagramm zu Hybridverbindungen][1]

Wenn Ihre App eine DNS-Anforderung stellt, die mit einem konfigurierten Hybridverbindungsendpunkt übereinstimmt, wird der ausgehende TCP-Datenverkehr über die Hybridverbindung weitergeleitet.  

> [!NOTE]
> Sie sollten folglich versuchen, stets einen DNS-Namen für Ihre Hybridverbindung zu verwenden. Einige Clientsoftware führen keine DNS-Suche durch, wenn der Endpunkt stattdessen eine IP-Adresse verwendet. 
>

### <a name="app-service-hybrid-connection-benefits"></a>Vorteile der App Service-Hybridverbindungen ###

Die Funktion für Hybridverbindungen bietet eine Reihe von Vorteilen wie etwa Folgende:

- Sie ermöglicht Apps den sicheren Zugriff auf lokale Systeme und Dienste.
- Für das Feature ist kein Endpunkt erforderlich, der über das Internet zugänglich ist.
- Sie lässt sich schnell und einfach einrichten. 
- Jede Hybridverbindung entspricht einer einzelnen Host-Port-Kombination, die auch einen Sicherheitsvorteil darstellt.
- Sie erfordert normalerweise keine Firewalllücken. Die Verbindungen sind alle ausgehend über Standardwebports.
- Da die Funktion auf Netzwerkebene ausgeführt wird, ist sie nicht von der Sprache, die von Ihrer App verwendet wird, und von der Technologie, die vom Endpunkt verwendet wird, abhängig.
- Sie kann verwendet werden, um über eine einzige App Zugriff in mehreren Netzwerken bereitzustellen. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Einschränkungen bei Hybridverbindungen ###

Die folgenden Aktionen sind beispielsweise mit Hybridverbindungen nicht möglich:

- Bereitstellen eines Laufwerks.
- Verwenden von UDP.
- Zugreifen auf TCP-basierte Dienste, die dynamische Ports verwenden, wie der passive Modus für FTP oder der erweiterte passive Modus.
- Unterstützung von LDAP, weil UDP erforderlich sein kann.
- Unterstützung von Active Directory, da der Domänenbeitritt eines App Service-Workers nicht möglich ist.

### <a name="prerequisites"></a>Voraussetzungen ###
 - Windows App Service ist erforderlich. Es ist nur unter Windows verfügbar.  

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Hinzufügen und Erstellen von Hybridverbindungen in Ihrer App ##

Navigieren Sie zum Erstellen einer Hybridverbindung zum [Azure-Portal][portal], und wählen Sie Ihre App aus. Wählen Sie **Netzwerk** > **Endpunkte der Hybridverbindung konfigurieren** aus. Hier können Sie die Hybridverbindungen anzeigen, die für Ihre App konfiguriert sind.  

![Screenshot der Liste der Hybridverbindungen][2]

Um eine neue Hybridverbindung hinzuzufügen, wählen Sie **[+] Hybridverbindung hinzufügen** aus.  Es wird eine Liste der Hybridverbindungen angezeigt, die Sie bereits erstellt haben. Um Ihrer App eine oder mehrere dieser Hybridverbindungen hinzuzufügen, markieren Sie diese und wählen anschließend **Ausgewählte Hybridverbindung hinzufügen** aus.  

![Screenshot des Portals für Hybridverbindungen][3]

Wenn Sie eine neue Hybridverbindung erstellen möchten, wählen Sie **Neue Hybridverbindung erstellen** aus. Geben Sie Folgendes an: 

- Hybridverbindungsname
- Endpunkt-Hostname
- Endpunktport
- Zu verwendender Service Bus-Namespace

![Screenshot des Dialogfelds „Neue Hybridverbindung erstellen“][4]

Jede Hybridverbindung ist mit einem Service Bus-Namespace verbunden. Jeder Service Bus-Namespace befindet sich in einer Azure-Region. Es ist wichtig, einen Service Bus-Namespace in derselben Region wie Ihre App zu verwenden, um durch das Netzwerk verursachte Latenzen zu vermeiden.

Wenn Sie Ihre Hybridverbindung aus Ihrer App entfernen möchten, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Verbindung trennen** aus.  

Nachdem Ihrer App eine Hybridverbindung hinzugefügt wurde, können Sie die Details zu dieser anzeigen, indem Sie sie einfach auswählen. 

![Screenshot der Details von Hybridverbindungen][5]

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Erstellen einer Hybridverbindung im Azure Relay-Portal ###

Sie können Hybridverbindungen nicht nur über das Portal Ihrer App erstellen, sondern auch über das Azure Relay-Portal. Damit eine Hybridverbindung von App Service verwendet werden kann, muss sie:

* eine Clientautorisierung erfordern
* ein Metadatenelement namens „endpoint“ aufweisen, das eine Host-Port-Kombination als Wert enthält

## <a name="hybrid-connections-and-app-service-plans"></a>Hybrid Connections und App Service-Pläne ##

App Service-Hybridverbindungen sind nur in den Preis-SKUs Basic, Standard, Premium und Isolated verfügbar. Es gelten Beschränkungen, die am Tarif geknüpft sind.  

| Tarif | Anzahl der im Plan verwendbaren Hybridverbindungen |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolated | 200 |

Die Benutzeroberfläche des App Service-Plans zeigt auch, wie viele Hybridverbindungen verwendet werden und von welchen Apps.  

![Screenshot der Eigenschaften eines App Service-Plans][6]

Wählen Sie die Hybridverbindung aus, um Details anzuzeigen. Sie können alle Informationen sehen, die auch in der App-Ansicht angezeigt wurden. Außerdem können Sie sehen, wie viele andere Apps im gleichen Plan diese Hybridverbindung verwenden.

Es gilt eine Einschränkung für die Anzahl der Hybridverbindungs-Endpunkte, die in einem App Service-Plan verwendet werden können. Jede verwendete Hybridverbindung kann jedoch von einer beliebigen Anzahl von Apps in diesem Plan verwendet werden. Eine einzelne Hybridverbindung, die in fünf separaten Apps in einem App Service-Plan verwendet wird, zählt z.B. als eine Hybridverbindung.

### <a name="pricing"></a>Preise ###

Zusätzlich zur SKU-Anforderung eines App-Service-Plans fallen zusätzliche Kosten für die Nutzung von Hybridverbindungen an. Für jeden Listener, der von einer Hybridverbindung verwendet wird, fällt eine Gebühr an. Der Listener ist der Hybridverbindungs-Manager. Wenn Sie fünf Hybridverbindungen verwenden, die von zwei Hybridverbindungs-Managern unterstützt werden, würde dies 10 Listener bedeuten. Weitere Informationen finden Sie unter [Service Bus – Preise][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybrid Connection Manager ##

Damit das Feature Hybrid Connections funktioniert, ist ein Relay-Agent in dem Netzwerk erforderlich, das Ihren Hybridverbindungs-Endpunkt hostet. Dieser Relay-Agent wird als Hybrid Connection Manager (HCM) bezeichnet. Zum Herunterladen von HCM wählen Sie in Ihrer App im [Azure-Portal][portal] die Option **Netzwerk** > **Endpunkte der Hybridverbindung konfigurieren** aus.  

Dieses Tool wird unter Windows Server 2012 und höher ausgeführt. HCM wird als Dienst ausgeführt und verbindet ausgehenden Datenverkehr mit Azure Relay über Port 443.  

Sie können nach der Installation von HCM die Datei „HybridConnectionManagerUi.exe“ ausführen, um die Benutzeroberfläche für das Tool zu verwenden. Diese Datei befindet sich im Installationsverzeichnis des Hybridverbindungs-Managers. Unter Windows 10 können Sie auch einfach im Suchfeld nach *Hybridverbindungs-Manager-Benutzeroberfläche* suchen.  

![Screenshot von Hybridverbindungs-Manager][7]

Wenn Sie die Benutzeroberfläche des HCM starten, wird als Erstes eine Tabelle angezeigt, in der alle Hybridverbindungen aufgeführt werden, die mit dieser Instanz des HCM konfiguriert sind. Wenn Sie Änderungen vornehmen möchten, müssen Sie sich zunächst bei Azure authentifizieren. 

So fügen Sie Ihrem HCM eine oder mehrere Hybridverbindungen hinzu:

1. Starten Sie die Benutzeroberfläche des HCM.
2. Wählen Sie **Andere Hybridverbindung konfigurieren** aus.
![Screenshot der Konfiguration neuer Hybridverbindungen][8]

1. Melden Sie sich mit Ihrem Azure-Konto an, um die für Ihre Abonnements verfügbaren Hybrid Connections abzurufen. Der HCM verwendet Ihr Azure-Konto darüber hinaus nicht. 
1. Wählen Sie ein Abonnement aus.
1. Wählen Sie die Hybridverbindungen aus, die der HCM weiterleiten soll.
![Screenshot von Hybridverbindungen][9]

1. Wählen Sie **Speichern** aus.

Es werden nun die Hybridverbindungen angezeigt, die Sie hinzugefügt haben. Sie können die konfigurierte Hybridverbindung auch auswählen, um Details anzuzeigen.

![Screenshot von Details einer Hybridverbindung][10]

Damit der HCM die bei ihm konfigurierten Hybridverbindungen unterstützen kann, ist Folgendes erforderlich:

- TCP-Zugriff auf Azure über Port 443.
- TCP-Zugriff auf den Hybridverbindungs-Endpunkt
- Möglichkeit für DNS-Suchvorgänge auf dem Endpunkthost und im Service Bus-Namespace

> [!NOTE]
> Azure-Relay nutzt Websockets für die Konnektivität. Diese Funktion ist nur für Windows Server 2012 oder höher verfügbar. Aus diesem Grund wird der HCM von Versionen vor Windows Server 2012 nicht unterstützt.
>

### <a name="redundancy"></a>Redundanz ###

Jeder HCM kann mehrere Hybridverbindungen unterstützen. Darüber hinaus kann eine bestimmte Hybridverbindung durch mehrere HCMs unterstützt werden. Das Standardverhalten ist ein Weiterleiten von Datenverkehr über die konfigurierten HCMs für einen bestimmten Endpunkt. Wenn Sie Hochverfügbarkeit für Hybridverbindungen über Ihr Netzwerk wünschen, führen Sie mehrere HCMs auf unterschiedlichen Computern aus. Der Lastverteilungsalgorithmus, der vom Relaydienst verwendet wird, um Datenverkehr an die HCMs zu verteilen, weist nach dem Zufallsprinzip zu. 

### <a name="manually-add-a-hybrid-connection"></a>Manuelles Hinzufügen einer Hybridverbindung ###

Damit ein Benutzer außerhalb Ihres Abonnements eine HCM-Instanz für eine bestimmte Hybridverbindung hosten kann, geben Sie für diesen die Gateway-Verbindungszeichenfolge für die Hybridverbindung frei. Sie können die Gatewayverbindungszeichenfolge in den Eigenschaften einer Hybridverbindung im [Azure-Portal][portal] anzeigen. Um diese Zeichenfolge zu verwenden, wählen Sie im HCM **Manuell eingeben** aus, und fügen Sie die Gateway-Verbindungszeichenfolge ein.

![Manuelles Hinzufügen einer Hybridverbindung][11]

### <a name="upgrade"></a>Upgrade ###

Es sind regelmäßige Updates für den Hybridverbindungs-Manager zum Beheben von Problemen oder Bereitstellen von Verbesserungen verfügbar. Wenn Upgrades veröffentlicht werden, wird ein Popupfenster in der HCM-Benutzeroberfläche angezeigt. Durch Ausführen des Upgrades werden die Änderungen angewendet, und der HCM wird neu gestartet. 

## <a name="adding-a-hybrid-connection-to-your-app-programmatically"></a>Programmgesteuertes Hinzufügen einer Hybridverbindung zu Ihrer App ##

Die unten aufgeführten APIs können direkt zum Verwalten der Hybridverbindungen verwendet werden, die mit Ihren Apps verbunden sind. 

    /subscriptions/[subscription name]/resourceGroups/[resource group name]/providers/Microsoft.Web/sites/[app name]/hybridConnectionNamespaces/[relay namespace name]/relays/[hybrid connection name]?api-version=2016-08-01

Das JSON-Objekt, das einer Hybridverbindung zugeordnet ist, sieht folgendermaßen aus:

    {
      "name": "[hybrid connection name]",
      "type": "Microsoft.Relay/Namespaces/HybridConnections",
      "location": "[location]",
      "properties": {
        "serviceBusNamespace": "[namespace name]",
        "relayName": "[hybrid connection name]",
        "relayArmUri": "/subscriptions/[subscription id]/resourceGroups/[resource group name]/providers/Microsoft.Relay/namespaces/[namespace name]/hybridconnections/[hybrid connection name]",
        "hostName": "[endpoint host name]",
        "port": [port],
        "sendKeyName": "defaultSender",
        "sendKeyValue": "[send key]"
      }
    }

Eine Möglichkeit, diese Informationen zu verwenden, ist der ARM-Client, den Sie aus dem GitHub-Projekt [ARMClient][armclient] abrufen können. Das folgende Beispiel zeigt das Anfügen einer vorhandenen Hybridverbindung an Ihre App. Erstellen Sie eine JSON-Datei über das oben angegebene Schema:

    {
      "name": "relay-demo-hc",
      "type": "Microsoft.Relay/Namespaces/HybridConnections",
      "location": "North Central US",
      "properties": {
        "serviceBusNamespace": "demo-relay",
        "relayName": "relay-demo-hc",
        "relayArmUri": "/subscriptions/ebcidic-asci-anna-nath-rak1111111/resourceGroups/myrelay-rg/providers/Microsoft.Relay/namespaces/demo-relay/hybridconnections/relay-demo-hc",
        "hostName": "my-wkstn.home",
        "port": 1433,
        "sendKeyName": "defaultSender",
        "sendKeyValue": "Th9is3is8a82lot93of3774stu887ff122235="
      }
    }

Um diese API verwenden zu können, benötigen Sie den Sendeschlüssel und die Ressourcen-ID des Relays. Wenn Sie Ihre Informationen unter dem Dateinamen „hctest.json“ gespeichert haben, geben Sie diesen Befehl aus, um die Hybridverbindung an Ihre App anzufügen: 

    armclient login
    armclient put /subscriptions/ebcidic-asci-anna-nath-rak1111111/resourceGroups/myapp-rg/providers/Microsoft.Web/sites/myhcdemoapp/hybridConnectionNamespaces/demo-relay/relays/relay-demo-hc?api-version=2016-08-01 @hctest.json

## <a name="troubleshooting"></a>Problembehandlung ##

Der Status „Verbunden“ bedeutet, dass mindestens ein HCM mit dieser Hybridverbindung konfiguriert ist und Azure erreichen kann. Wenn der Status für die Hybridverbindung nicht **Verbunden** ist, wurde die Hybridverbindung nicht auf einem HCM konfiguriert, der auf Azure zugreifen kann.

Der Hauptgrund, warum Clients keine Verbindung mit dem jeweiligen Endpunkt herstellen können, ist, dass der Endpunkt mit einer IP-Adresse anstelle eines DNS-Namens angegeben wurde. Wenn Ihre App den gewünschten Endpunkt nicht erreichen kann und Sie eine IP-Adresse verwendet haben, gehen Sie zur Verwendung eines DNS-Namens über, der auf dem Host, auf dem der HCM ausgeführt wird, gültig ist. Überprüfen Sie außerdem, ob der DNS-Name auf dem Host, auf dem der HCM ausgeführt wird, ordnungsgemäß aufgelöst wird. Vergewissern Sie sich, dass zwischen dem Host, auf dem der HCM ausgeführt wird, und dem Hybridverbindungs-Endpunkt Konnektivität besteht.  

In App Service kann das Befehlszeilentool **tcpping** über die Konsole „Erweiterte Tools“ (Kudu) aufgerufen werden. Mit diesem Tool können Sie feststellen, ob Sie Zugriff auf einen TCP-Endpunkt haben, jedoch nicht, ob Sie Zugriff auf einen Hybridverbindungsendpunkt haben. Wenn Sie das Tool an der Konsole für einen Hybridverbindungs-Endpunkt verwenden, bestätigen Sie nur, dass eine Host-Port-Kombination verwendet wird.  

Wenn Sie über einen Befehlszeilenclient für Ihren Endpunkt verfügen, können Sie die Konnektivität an der App-Konsole testen. Sie können beispielsweise den Zugriff auf Webserver-Endpunkte mithilfe von cURL testen.

## <a name="biztalk-hybrid-connections"></a>BizTalk-Hybridverbindungen ##

Eine frühe Form dieses Features wurde als „BizTalk-Hybridverbindungen“ bezeichnet. Diese Funktion wurde am 31. Mai 2018 eingestellt und beendet. BizTalk-Hybridverbindungen wurden aus allen Apps entfernt, und auf sie kann nicht über das Portal oder die API zugegriffen werden. Wenn Sie diese älteren Verbindungen noch im Hybridverbindungs-Manager konfiguriert haben, werden der Status „Nicht mehr unterstützt“ und unten ein Einstellungshinweis angezeigt.

![BizTalk-Hybridverbindungen im HCM][12]


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png
[11]: ./media/app-service-hybrid-connections/hybridconn-manual.png
[12]: ./media/app-service-hybrid-connections/hybridconn-bt.png

<!--Links-->
[HCService]: https://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: https://portal.azure.com/
[oldhc]: https://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: https://azure.microsoft.com/pricing/details/service-bus/
[armclient]: https://github.com/projectkudu/ARMClient/
