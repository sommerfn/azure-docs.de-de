---
title: Lokales Datengateway
description: Ein lokales Gateway ist erforderlich, wenn der Analysis Services-Server in Azure mit lokalen Datenquellen verbunden wird.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 190394481f17310784f87c9e2f642eeea0b2597f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67062234"
---
# <a name="connecting-to-on-premises-data-sources-with-on-premises-data-gateway"></a>Herstellen einer Verbindung mit lokalen Datenquellen über ein lokales Datengateway
Das lokale Datengateway sorgt für eine sichere Datenübertragung zwischen lokalen Datenquellen und den Azure Analysis Services-Servern in der Cloud. Zusätzlich zur Verwendung von mehreren Azure Analysis Services-Servern in derselben Region funktioniert die neueste Version des Gateways auch mit Azure Logic Apps, Power BI, Power Apps und Microsoft Flow. Sie können einem einzelnen Gateway mehrere Dienste im gleichen Abonnement und in derselben Region zuordnen. 

Das erstmalige Einrichten des Gateways ist ein Prozess mit vier Schritten:

- **Herunterladen und Ausführen des Setupprogramms:** Bei diesem Schritt wird ein Gatewaydienst auf einem Computer in Ihrer Organisation installiert. Sie melden sich bei Azure ebenfalls mit einem Konto in der Azure AD-Instanz Ihres [Mandanten](/previous-versions/azure/azure-services/jj573650(v=azure.100)#what-is-an-azure-ad-tenant) an. Azure B2B (Gast)-Konten werden nicht unterstützt.

- **Registrieren des Gateways**: In diesem Schritt geben Sie einen Namen und einen Wiederherstellungsschlüssel für Ihr Gateway ein, wählen eine Region aus und registrieren Ihr Gateway beim Gatewayclouddienst. Die Gatewayressource kann in jeder Region registriert werden, es empfiehlt sich aber, die gleiche Region zu verwenden, in der sich auch die Analysis Services-Server befinden. 

- **Erstellen einer Gatewayressource in Azure:** Bei diesem Schritt erstellen Sie eine Gatewayressource in Ihrem Azure-Abonnement.

- **Verbinden Ihrer Server mit der Gatewayressource:** Nachdem Sie in Ihrem Abonnement eine Gatewayressource eingerichtet haben, können Sie damit beginnen, Ihre Server mit dieser zu verbinden. Sie können mehrere Server und andere Ressourcen verbinden, sofern sie sich im gleichen Abonnement und in derselben Region befinden.

Wenn Sie sofort beginnen möchten, lesen Sie unter [Installieren und Konfigurieren eines lokalen Datengateways](analysis-services-gateway-install.md) nach.

## <a name="how-it-works"></a>Funktionsweise
Das Gateway, das Sie auf einem Computer im Netzwerk Ihrer Organisation installieren, wird als Windows-Dienst mit dem Namen **Lokales Datengateway** ausgeführt. Dieser lokale Dienst wird über Azure Service Bus beim Gateway-Clouddienst registriert. Anschließend erstellen Sie eine Gatewayressource als Gateway-Clouddienst für Ihr Azure-Abonnement. Ihre Azure Analysis Services-Server werden dann mit der Gatewayressource verbunden. Wenn Modelle auf Ihrem Server für Abfragen oder die Verarbeitung mit Ihren lokalen Datenquellen verbunden werden müssen, durchläuft ein Abfrage- und Datenfluss die Gatewayressource, Azure Service Bus, den lokalen Datengateway-Dienst und Ihre Datenquellen. 

![So funktioniert's](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Abfragen und Datenfluss:

1. Vom Clouddienst wird eine Abfrage mit den verschlüsselten Anmeldeinformationen für die lokale Datenquelle erstellt. Sie wird dann an eine Warteschlange für die Verarbeitung durch das Gateway gesendet.
2. Der Gatewayclouddienst analysiert die Abfrage und überträgt die Anforderung per Push an den [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. Das lokale Datengateway fragt den Azure Service Bus nach ausstehenden Anforderungen ab.
4. Das Gateway ruft die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt anhand dieser Anmeldeinformationen eine Verbindung mit den Datenquellen her.
5. Das Gateway sendet die Abfrage zur Ausführung an die Datenquelle.
6. Die Ergebnisse werden aus der Datenquelle zurück an das Gateway und dann an den Clouddienst und Ihren Server gesendet.

## <a name="windows-service-account"></a>Windows-Dienstkonto
Das lokale Datengateway ist zur Verwendung von *NT SERVICE\PBIEgwService* für die Anmeldeinformationen für den Windows-Dienst konfiguriert. Standardmäßig verfügt es über das Recht „Anmelden als Dienst“ im Kontext des Computers, auf dem Sie das Gateway installieren. Diese Anmeldeinformationen sind nicht mit denen des Kontos identisch, das zum Verbinden mit lokalen Datenquellen oder dem Azure-Konto verwendet wird.  

Wenn beim Proxyserver Authentifizierungsprobleme auftreten, sollten Sie das Windows-Dienstkonto in ein Domänenbenutzerkonto oder verwaltetes Dienstkonto ändern.

## <a name="ports"></a>Ports
Das Gateway stellt eine ausgehende Verbindung mit dem Azure Service Bus her. Es kommuniziert über ausgehende Ports: TCP 443 (Standard), 5671, 5672, 9350 bis 9354.  Das Gateway benötigt keine eingehenden Ports.

Es wird empfohlen, die IP-Adressen für Ihren Datenbereich in die Whitelist der Firewall aufzunehmen. Sie können die [Liste der IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) (in englischer Sprache) herunterladen. Diese Liste wird wöchentlich aktualisiert.

> [!NOTE]
> Die IP-Adressen in der Liste der IP-Bereiche des Azure-Rechenzentrums sind in der CIDR-Notation angegeben. Weitere Informationen finden Sie unter [Klassenloses domänenübergreifendes Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).
>
>

Nachfolgend sind die vollqualifizierten Domänennamen aufgeführt, die vom Gateway verwendet werden.

| Domänennamen | Ausgehende Ports | BESCHREIBUNG |
| --- | --- | --- |
| *.powerbi.com |80 |Zum Herunterladen des Installers wird HTTP verwendet. |
| *.powerbi.com |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *.login.windows.net |443 |HTTPS |
| *.servicebus.windows.net |5671-5672 |Advanced Message Queuing Protocol (AMQP) |
| *.servicebus.windows.net |443, 9350-9354 |Listener auf Service Bus Relay per TCP (443 für Access Control-Tokenbeschaffung erforderlich) |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *.msftncsi.com |443 |Wird verwendet, um die Internetkonnektivität zu testen, falls der Power BI-Dienst das Gateway nicht erreicht. |
| *.microsoftonline-p.com |443 |Wird je nach Konfiguration für die Authentifizierung verwendet. |

### <a name="force-https"></a>Erzwingen von HTTPS-Kommunikation mit Azure Service Bus
Sie können erzwingen, dass das Gateway mit Azure Service Bus über HTTPS (und nicht direkt über TCP) kommuniziert. Dies kann allerdings erheblich die Leistung beeinträchtigen. Sie können die Datei *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* ändern, indem Sie den Wert von `AutoDetect` in `Https` ändern. Diese Datei befindet sich standardmäßig in *C:\Programme\On-premises data gateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="tenant-level-administration"></a>Verwaltung auf Mandantenebene 

Derzeit können Mandantenadministratoren nicht alle Gateways, die andere Benutzer installiert und konfiguriert haben, zentral verwalten.  Als Mandantenadministrator sollten Sie die Benutzer in Ihrer Organisation bitten, Sie jedem Gateway, das sie installieren, als Administrator hinzuzufügen. So können Sie alle Gateways in Ihrer Organisation über die Seite „Gatewayeinstellungen“ oder über [PowerShell-Befehle](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters#powershell-support-for-gateway-clusters) verwalten. 


## <a name="faq"></a>Häufig gestellte Fragen

### <a name="general"></a>Allgemein

**F:** Benötige ich ein Gateway für Datenquellen in der Cloud (etwa für Azure SQL-Datenbank)? <br/>
**A**:  Nein. Ein Gateway ist nur zum Herstellen einer Verbindung mit lokalen Datenquellen erforderlich.

**F**: Muss das Gateway auf dem gleichen Computer installiert sein wie die Datenquelle? <br/>
**A**:  Nein. Das Gateway muss nur eine Verbindung mit dem Server herstellen können (üblicherweise im gleichen Netzwerk).

<a name="why-azure-work-school-account"></a>

**F**: Warum muss ich für die Anmeldung ein Geschäfts-, Schul- oder Unikonto verwenden? <br/>
**A**: Sie können beim Installieren des lokalen Datengateways nur ein Geschäfts-, Schul- oder Unikonto Ihrer Organisation verwenden. Dieses Konto muss sich darüber hinaus im gleichen Mandanten befinden wie das Abonnement, unter dem Sie die Gatewayressource konfigurieren. Ihr Anmeldekonto ist in einem Mandanten gespeichert, der von Azure Active Directory (Azure AD) verwaltet wird. Normalerweise entspricht der Benutzerprinzipalname (UPN) Ihres Azure AD-Kontos der E-Mail-Adresse.

**F:** Wo werden meine Anmeldeinformationen gespeichert? <br/>
**A**: Die Anmeldeinformationen, die Sie für eine Datenquelle eingeben, werden verschlüsselt und im Gatewayclouddienst gespeichert. Die Anmeldeinformationen werden im lokalen Datengateway entschlüsselt.

**F:** Gibt es Anforderungen an die Netzwerkbandbreite? <br/>
**A**: Es ist ratsam, für einen guten Durchsatz der Netzwerkverbindung zu sorgen. Jede Umgebung ist anders, und die Menge der zu sendenden Daten wirkt sich auf die Ergebnisse aus. ExpressRoute könnte ein Durchsatzniveau zwischen lokalen und Azure-Rechenzentren gewährleisten.
Sie können mithilfe des Drittanbietertools Azure Speed Test-App messen, wie hoch der Durchsatz ist.

**F**: Mit welcher Wartezeit ist beim Ausführen von Abfragen für eine Datenquelle über das Gateway zu rechnen? Welche Architektur ist die beste? <br/>
**A:** Installieren Sie das Gateway möglichst nah an der Datenquelle, um die Netzwerkwartezeit zu reduzieren. Wenn Sie das Gateway auf der tatsächlichen Datenquelle installieren können, wird die Wartezeit durch diese Nähe minimiert. Berücksichtigen Sie auch die Rechenzentren. Wenn für Ihren Dienst beispielsweise das Rechenzentrum „USA, Westen“ verwendet wird und Sie SQL Server auf einer Azure-VM hosten, sollte sich die Azure-VM ebenfalls in der Region „USA, Westen“ befinden. Aufgrund dieser Nähe wird die Wartezeit verringert, und es werden Gebühren für ausgehenden Datenverkehr auf der Azure-VM vermieden.

**F:** Wie werden Ergebnisse an die Cloud zurückgesendet? <br/>
**A**: Die Ergebnisse werden über Azure Service Bus gesendet.

**F**: Gibt es eingehende Verbindungen mit dem Gateway aus der Cloud? <br/>
**A**:  Nein. Das Gateway verwendet ausgehende Verbindungen mit dem Azure Service Bus.

**F**: Was geschieht, wenn ich ausgehende Verbindungen blockiere? Was muss ich öffnen? <br/>
**A:** Die Ports und Hosts, die das Gateway verwendet.

**F:** Wie heißt der eigentliche Windows-Dienst?<br/>
**A**: In „Dienste“ hat das Gateway den Namen „Lokaler Datengatewaydienst“.

**F**: Kann der Windows-Gatewaydienst mit einem Azure Active Directory-Konto ausgeführt werden? <br/>
**A**:  Nein. Der Windows-Dienst benötigt ein gültiges Windows-Konto. Standardmäßig wird er mit der Dienst-SID „NT SERVICE\PBIEgwService“ ausgeführt.

**F**: Wie übernehme ich ein Gateway? <br/>
**A**: Um ein Gateway übernehmen zu können (durch Ausführen von „Einrichten/Ausführen“ unter „Systemsteuerung > Programme und Funktionen“), müssen Sie Besitzer der Gatewayressource in Azure sein und über den Wiederherstellungsschlüssel verfügen. Besitzer von Gatewayressourcen können unter Zugriffssteuerung festgelegt werden.

### <a name="high-availability"></a>Hohe Verfügbarkeit und Notfallwiederherstellung

**F**: Wie können wir Hochverfügbarkeit erzielen?  
**A**: Sie können ein Gateway auf einem anderen Computer installieren, um einen Cluster zu erstellen. Weitere Informationen finden Sie in der Dokumentation zu Power BI Gateway unter [Hochverfügbarkeitscluster für lokale Datengateways](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters).

**F**: Welche Optionen sind für die Notfallwiederherstellung verfügbar? <br/>
**A:** Sie können den Wiederherstellungsschlüssel verwenden, um ein Gateway wiederherzustellen oder zu verschieben. Wenn Sie das Gateway installieren, geben Sie den Wiederherstellungsschlüssel an.

**F:** Welchen Vorteil bietet der Wiederherstellungsschlüssel? <br/>
**A:** Der Wiederherstellungsschlüssel bietet eine Möglichkeit zum Migrieren oder Wiederherstellen Ihrer Gatewayeinstellungen nach einem Notfall.

## <a name="troubleshooting"></a>Problembehandlung

**F**: Warum wird mein Gateway nicht in der Liste der Gatewayinstanzen angezeigt, wenn ich versuche, die Gatewayressource in Azure zu erstellen? <br/>
**A**: Es gibt zwei mögliche Gründe. Erstens könnte bereits eine Ressource für das Gateway im aktuellen oder einem anderen Abonnement erstellt sein. Um diese Möglichkeit auszuschließen, zählen Sie Ressourcen des Typs **Lokale Datengateways** aus dem Portal auf. Stellen Sie sicher, dass Sie beim Aufzählen aller Ressourcen alle Abonnements auswählen. Sobald die Ressource erstellt wurde, wird das Gateway auf der Portalseite „Gatewayressource erstellen“ nicht in der Liste der Gatewayinstanzen angezeigt. Der zweite mögliche Grund ist, dass die Azure AD-Identität des Benutzers, der das Gateway installiert hat, nicht dem Benutzer entspricht, der beim Azure-Portal angemeldet ist. Um dieses Problem zu beheben, melden Sie sich mit dem Konto des Benutzers beim Portal an, der das Gateway installiert hat.

**F**: Wie kann ich feststellen, welche Abfragen an die lokale Datenquelle gesendet werden? <br/>
**A:** Sie können die Abfrageablaufverfolgung aktivieren. Diese enthält auch die gesendeten Abfragen. Denken Sie daran, die Abfrageablaufverfolgung nach Abschluss der Problembehandlung wieder auf den ursprünglichen Wert zurückzusetzen. Wenn Sie die Abfrageablaufverfolgung aktiviert lassen, werden größere Protokolle erstellt.

Sie können auch Tools anzeigen, die Ihre Datenquelle für die Verfolgung von Abfrageabläufen bietet. Sie können z.B. Erweiterte Ereignisse oder SQL Profiler für SQL Server und Analysis Services verwenden.

**F**: Wo befinden sich die Gatewayprotokolle? <br/>
**A**: Siehe „Protokolle“ weiter unten in diesem Artikel.

### <a name="update"></a>Update auf die aktuelle Version

Es können vermehrt Probleme auftreten, wenn die Gatewayversion veraltet ist. Achten Sie daher am besten darauf, dass Sie immer die aktuelle Version verwenden. Wenn Sie das Gateway für einen Monat oder länger nicht aktualisiert haben, sollten Sie erwägen, die neueste Version des Gateways zu installieren und zu ermitteln, ob Sie das Problem reproduzieren können.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Fehler Fehler beim Hinzufügen des Benutzers zur Gruppe. (-2147463168 PBIEgwService Leistungsprotokollbenutzer)

Dieser Fehler wird ggf. angezeigt, wenn Sie versuchen, das Gateway auf einem Domänencontroller zu installieren, der nicht unterstützt wird. Stellen Sie sicher, dass Sie das Gateway auf einem Computer bereitstellen, beim dem es sich nicht um einen Domänencontroller handelt.

## <a name="logs"></a>Protokolle

Protokolldateien sind eine wichtige Ressource bei der Problembehandlung.

#### <a name="enterprise-gateway-service-logs"></a>Enterprise-Gatewaydienstprotokolle

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Konfigurationsprotokolle

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`

#### <a name="event-logs"></a>Ereignisprotokolle

Sie finden das Datenverwaltungsgateway- und PowerBIGateway-Protokoll unter **Anwendungs- und Dienstprotokolle**.

## <a name="next-steps"></a>Nächste Schritte
* [Installieren und Konfigurieren eines lokalen Datengateways](analysis-services-gateway-install.md)   
* [Verwalten von Analysis Services](analysis-services-manage.md)
* [Abrufen von Daten aus Azure Analysis Services](analysis-services-connect.md)
