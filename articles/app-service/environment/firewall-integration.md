---
title: Sperren des ausgehenden Datenverkehrs der App Service-Umgebung – Azure
description: Beschreibung der Integration mit Azure Firewall zum Sichern des ausgehenden Datenverkehrs
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 3e24b95704249a8a0b7588112cbaa678ae1e2c86
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619139"
---
# <a name="locking-down-an-app-service-environment"></a>Sperren einer App Service-Umgebung

Die App Service-Umgebung verfügt über externe Abhängigkeiten, auf die sie Zugriff benötigt, um ordnungsgemäß zu funktionieren. Die App Service-Umgebung befindet sich in der Azure Virtual Network-Instanz (VNET) des Benutzers. Benutzer müssen den Datenverkehr für Abhängigkeiten der App Service-Umgebung zulassen. Dies ist jedoch problematisch, wenn sie den gesamten ausgehenden Datenverkehr ihres VNET sperren wollen.

Eine App Service-Umgebung weist zahlreiche Abhängigkeiten für den eingehenden Datenverkehr auf. Der eingehende Verwaltungsdatenverkehr kann über eine Firewallgerät nicht gesendet werden. Die Quelladressen für diesen Datenverkehr sind bekannt und werden im Dokument [Verwaltungsadressen der App Service-Umgebung](https://docs.microsoft.com/azure/app-service/environment/management-addresses) veröffentlicht. Sie können Netzwerksicherheitsgruppen-Regeln mit diesen Informationen erstellen, um eingehenden Datenverkehr zu sichern.

Die Abhängigkeiten der App Service-Umgebung für den ausgehenden Datenverkehr werden fast ausschließlich mit FQDNs definiert, hinter denen sich keine statischen Adressen befinden. Das Fehlen statischer Adressen bedeutet, dass Netzwerksicherheitsgruppen (NSGs) nicht verwendet werden können, um den ausgehenden Datenverkehr einer App Service-Umgebung zu sperren. Die Adressen ändern sich häufig, sodass keine Regeln auf Grundlage der aktuellen Auflösung aufgestellt und keine NSGs damit erstellt werden können. 

Die Lösung zum Sichern ausgehender Adressen besteht in der Verwendung eines Firewallgeräts, das den ausgehenden Datenverkehr basierend auf Domänennamen kontrolliert. Azure Firewall kann ausgehenden HTTP- und HTTPS-Datenverkehr basierend auf den FQDN des Ziels beschränken.  

## <a name="system-architecture"></a>Systemarchitektur

Das Bereitstellen einer App Service-Umgebung (App Service Environment, ASE) mit ausgehendem Datenverkehr über ein Firewallgerät erfordert das Ändern von Routen im ASE-Subnetz. Routen funktionieren auf einer IP-Ebene. Wenn Sie die Routen nicht sorgfältig definieren, können Sie den TCP-Antwortdatenverkehr an die Quelle von einer anderen Adresse erzwingen. Dies wird als asymmetrisches Routing bezeichnet und unterbricht TCP.

Es müssen Routen definiert sein, damit der eingehende Datenverkehr an die ASE die Antwort auf die gleiche Weise zurücksenden kann, wie der Datenverkehr eintraf. Dies gilt sowohl für eingehende Verwaltungsanforderungen als auch für eingehende Anwendungsanforderungen.

Beim Datenverkehr an und von einer ASE müssen die folgenden Konventionen eingehalten werden:

* Der Datenverkehr an Azure SQL, Storage und Event Hub wird bei Verwendung eines Firewallgeräts nicht unterstützt. Dieser Datenverkehr muss direkt an diese Dienste gesendet werden. Um dies zu ermöglichen, müssen Dienstendpunkte für diese drei Dienste konfiguriert werden. 
* Es müssen Routingtabellenregeln definiert werden, die eingehenden Verwaltungsdatenverkehr an seinen Ursprungsort zurücksenden.
* Es müssen Routingtabellenregeln definiert werden, die eingehenden Verwaltungsdatenverkehr an seinen Ursprungsort zurücksenden. 
* Jeglicher sonstiger Datenverkehr von der ASE kann mit einer Routingtabellenregel an Ihr Firewallgerät gesendet werden.

![ASE mit Azure Firewall: Verbindungsfluss][5]

## <a name="configuring-azure-firewall-with-your-ase"></a>Konfigurieren von Azure Firewall mit Ihrer App Service-Umgebung 

Gehen Sie wie folgt vor, um ausgehenden Datenverkehr Ihrer bestehenden App Service-Umgebung (App Service Environment, ASE) mit Azure Firewall zu sperren:

1. Aktivieren Sie Dienstendpunkte für SQL, Storage und Event Hub in Ihrem ASE-Subnetz. Navigieren Sie dazu im Netzwerkportal zu „Subnetze“, und wählen Sie in der Dropdownliste „Dienstendpunkte“ die Optionen „Microsoft.EventHub“, „Microsoft.SQL“ und „Microsoft.Storage“ aus. Wenn Sie Dienstendpunkte für Azure SQL aktiviert haben, müssen alle Azure SQL-Abhängigkeiten Ihrer Apps ebenfalls mit Dienstendpunkten konfiguriert werden. 

   ![Auswählen von Dienstendpunkten][2]
  
1. Erstellen Sie ein Subnetz namens „AzureFirewallSubnet“ in dem VNET, in dem sich die ASE befindet. Folgen Sie den Anweisungen in der [Azure Firewall-Dokumentation](https://docs.microsoft.com/azure/firewall/), um Ihre Azure Firewall-Instanz zu erstellen.
1. Navigieren Sie in der Azure Firewall-Benutzeroberfläche zu „Regeln“ > „Anwendungsregelsammlung“, und wählen Sie „Anwendungsregelsammlung hinzufügen“ aus. Geben Sie einen Namen und eine Priorität an, und legen Sie „Zulassen“ fest. Geben Sie im Abschnitt „FQDN-Tags“ einen Namen an, legen Sie die Quelladressen auf „*“ fest, und wählen Sie die FQDN-Tags „AppServiceEnvironment“ und „WindowsUpdate“ aus. 
   
   ![Hinzufügen einer Anwendungsregel][1]
   
1. Navigieren Sie in der Azure Firewall-Benutzeroberfläche zu „Regeln“ > „Netzwerkregelsammlung“, und wählen Sie „Netzwerkregelsammlung hinzufügen“ aus. Geben Sie einen Namen und eine Priorität an, und legen Sie „Zulassen“ fest. Geben Sie im Abschnitt „Regeln“ einen Namen an, wählen Sie **Beliebig** aus, legen Sie die Quell- und Zieladressen auf „*“ und die Ports auf „123“ fest. Diese Regel ermöglicht es dem System, die Uhrsynchronisierung mit NTP durchzuführen. Erstellen Sie wie oben beschrieben eine weitere Regel für Port 12000, um die Selektierung von Systemproblemen zu erleichtern.

   ![Hinzufügen einer NTP-Netzwerkregel][3]

1. Erstellen Sie eine Routingtabelle mit den Verwaltungsadressen aus [Verwaltungsadressen der App Service-Umgebung]( https://docs.microsoft.com/azure/app-service/environment/management-addresses) mit dem nächsten Hop zum Internet. Die Routingtabelleneinträge werden benötigt, um asymmetrische Routingprobleme zu vermeiden. Fügen Sie mit dem nächsten Hop zum Internet den IP-Adressabhängigkeiten (s. weiter unten „IP-Adressabhängigkeiten“) Routen hinzu. Fügen Sie Ihrer Routingtabelle eine Route für ein virtuelles Gerät für 0.0.0.0/0 hinzu, und legen Sie dabei Ihre private Azure Firewall-IP-Adresse als nächsten Hop fest. 

   ![Erstellen einer Routingtabelle][4]
   
1. Weisen Sie die erstellte Routingtabelle dem Subnetz Ihrer App Service-Umgebung zu.

#### <a name="deploying-your-ase-behind-a-firewall"></a>Bereitstellen Ihrer ASE hinter einer Firewall

Die Schritte zum Bereitstellen Ihrer ASE hinter einer Firewall sind identisch mit der Konfiguration der bestehenden ASE mit einer Azure Firewall-Instanz. Der einzige Unterschied besteht darin, dass Sie Ihr ASE-Subnetz erstellen und anschließend die obigen Schritte ausführen müssen. Wenn Sie Ihre ASE in einem bereits vorhandenen Subnetz erstellen möchten, müssen Sie wie im Dokument [Erstellen einer ASE mit einer Azure Resource Manager-Vorlage](https://docs.microsoft.com/azure/app-service/environment/create-from-template) beschrieben eine Resource Manager-Vorlage verwenden.

## <a name="application-traffic"></a>Anwendungsdatenverkehr 

Mithilfe der oben genannten Schritte kann Ihre App Service-Umgebung problemlos ausgeführt werden. Sie müssen die Umgebung noch so konfigurieren, dass sie Ihren Anwendungsanforderungen entspricht. Es gibt zwei Probleme bei Anwendungen in einer App Service-Umgebung, die mit Azure Firewall konfiguriert wurde.  

- Anwendungsabhängigkeiten müssen der Azure Firewall-Instanz oder der Routingtabelle hinzugefügt werden. 
- Für den Anwendungsdatenverkehr müssen Routen erstellt werden, um Probleme durch asymmetrisches Routing zu vermeiden.

Wenn Ihre Anwendungen Abhängigkeiten aufweisen, müssen diese Ihrer Azure Firewall-Instanz hinzugefügt werden. Erstellen Sie Anwendungsregeln, um HTTP/HTTPS-Datenverkehr und Netzwerkregeln zuzulassen. 

Wenn Sie den Adressbereich kennen, aus dem der von Ihrer Anwendung angeforderte Datenverkehr kommt, können Sie ihn der Routingtabelle hinzufügen, die dem Subnetz Ihrer App Service-Umgebung zugeordnet ist. Wenn der Adressbereich groß oder nicht angegeben ist, können Sie ein Netzwerkgerät wie Application Gateway verwenden, um eine Adresse Ihrer Routingtabelle hinzufügen zu können. Weitere Informationen zum Konfigurieren einer Application Gateway-Instanz mit dem internen Lastenausgleichsmodul der App Service-Umgebung finden Sie unter [Integrieren eines internen Lastenausgleichs einer App Service-Umgebung in eine Application Gateway-Instanz](https://docs.microsoft.com/azure/app-service/environment/integrate-with-application-gateway).

Diese Verwendung von Application Gateway ist nur ein Beispiel dafür, wie Sie Ihr System konfigurieren können. Bei der obigen Konfiguration müssten Sie der Routingtabelle für das ASE-Subnetz eine Route hinzufügen, damit der an die Application Gateway-Instanz gesendete Antwortdatenverkehr direkt an diese weitergeleitet wird. 

## <a name="logging"></a>Protokollierung 

Azure Firewall kann Protokolle an Azure Storage, Event Hub oder Azure Monitor-Protokolle senden. Sie können ein beliebiges unterstütztes Ziel in Ihre App integrieren, indem Sie im Azure Firewall-Portal zu „Diagnoseprotokolle“ navigieren und die Protokolle für das gewünschte Ziel aktivieren. Wenn Sie sich für die Integration in Azure Monitor-Protokolle entscheiden, können Sie Protokolle für sämtlichen Datenverkehr einsehen, der an Azure Firewall gesendet wird. Öffnen Sie zum Anzeigen des abgelehnten Datenverkehrs das Portal mit dem Log Analytics-Arbeitsbereich, wählen Sie „Protokolle“ aus, und geben Sie eine Abfrage wie die folgende ein: 

    AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
 
Die Integration Ihrer Azure Firewall-Instanz in Azure Monitor-Protokolle ist sehr nützlich, wenn Sie eine Anwendung erstmals einrichten und nicht alle Anwendungsabhängigkeiten kennen. Weitere Informationen zu Azure Monitor-Protokollen finden Sie unter [Analysieren von Protokolldaten in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).
 
## <a name="dependencies"></a>Abhängigkeiten

Die folgenden Informationen sind nur erforderlich, wenn Sie ein anderes Firewallgerät als Azure Firewall konfigurieren möchten. 

- Dienste, die Dienstendpunkte unterstützen, sollten mit Dienstendpunkten konfiguriert werden.
- IP-Adressabhängigkeiten gelten für Nicht-HTTP/S-Datenverkehr (TCP- und UDP-Datenverkehr).
- FQDN-HTTP/HTTPS-Endpunkte können in Ihrem Firewallgerät bereitgestellt werden.
- Platzhalter-HTTP/HTTPS-Endpunkte sind Abhängigkeiten, die von Ihrer App Service-Umgebung abhängig sein können, basierend auf einer Reihe von Qualifizierern. 
- Linux-Abhängigkeiten sind nur relevant, wenn Sie in Ihrer App Service-Umgebung Linux-Apps bereitstellen. Wenn Sie in Ihrer App Service-Umgebung keine Linux-Apps bereitstellen, müssen diese Adressen Ihrer Firewall nicht hinzugefügt werden. 

#### <a name="service-endpoint-capable-dependencies"></a>Dienstendpunktfähige Abhängigkeiten 

| Endpunkt |
|----------|
| Azure SQL |
| Azure Storage |
| Azure Event Hub |

#### <a name="ip-address-dependencies"></a>IP-Adressabhängigkeiten

| Endpunkt | Details |
|----------| ----- |
| \*:123 | NTP-Uhrzeitüberprüfung. Datenverkehr wird an mehreren Endpunkten am Port 123 überprüft. |
| \*:12000 | Dieser Port wird für einige Systemüberwachungsfunktionen verwendet. Wenn er blockiert wird, ist die Selektierung mancher Probleme schwieriger, die ASE wird jedoch weiterhin ausgeführt. |
| 40.77.24.27:80 | Erforderlich für die Überwachung von ASE-Problemen und Warnungen zu diesen |
| 40.77.24.27:443 | Erforderlich für die Überwachung von ASE-Problemen und Warnungen zu diesen |
| 13.90.249.229:80 | Erforderlich für die Überwachung von ASE-Problemen und Warnungen zu diesen |
| 13.90.249.229:443 | Erforderlich für die Überwachung von ASE-Problemen und Warnungen zu diesen |
| 104.45.230.69:80 | Erforderlich für die Überwachung von ASE-Problemen und Warnungen zu diesen |
| 104.45.230.69:443 | Erforderlich für die Überwachung von ASE-Problemen und Warnungen zu diesen |
| 13.82.184.151:80 | Erforderlich für die Überwachung von ASE-Problemen und Warnungen zu diesen |
| 13.82.184.151:443 | Erforderlich für die Überwachung von ASE-Problemen und Warnungen zu diesen |

Mit einer Azure Firewall-Instanz erhalten Sie automatisch alle der unten aufgeführten Abhängigkeiten (mit den FQDN-Tags konfiguriert). 

#### <a name="fqdn-httphttps-dependencies"></a>FQDN-HTTP/HTTPS-Abhängigkeiten 

| Endpunkt |
|----------|
|graph.windows.net:443 |
|login.live.com:443 |
|login.windows.com:443 |
|login.windows.net:443 |
|login.microsoftonline.com:443 |
|client.wns.windows.com:443 |
|definitionupdates.microsoft.com:443 |
|go.microsoft.com:80 |
|go.microsoft.com:443 |
|www.microsoft.com:80 |
|www.microsoft.com:443 |
|wdcpalt.microsoft.com:443 |
|wdcp.microsoft.com:443 |
|ocsp.msocsp.com:443 |
|mscrl.microsoft.com:443 |
|mscrl.microsoft.com:80 |
|crl.microsoft.com:443 |
|crl.microsoft.com:80 |
|www.thawte.com:443 |
|crl3.digicert.com:80 |
|ocsp.digicert.com:80 |
|csc3-2009-2.crl.verisign.com:80 |
|crl.verisign.com:80 |
|ocsp.verisign.com:80 |
|cacerts.digicert.com:80 |
|azperfcounters1.blob.core.windows.net:443 |
|azurewatsonanalysis-prod.core.windows.net:443 |
|global.metrics.nsatc.net:80 |
|global.metrics.nsatc.net:443 |
|az-prod.metrics.nsatc.net:443 |
|antares.metrics.nsatc.net:443 |
|azglobal-black.azglobal.metrics.nsatc.net:443 |
|azglobal-red.azglobal.metrics.nsatc.net:443 |
|antares-black.antares.metrics.nsatc.net:443 |
|antares-red.antares.metrics.nsatc.net:443 |
|maupdateaccount.blob.core.windows.net:443 |
|clientconfig.passport.net:443 |
|packages.microsoft.com:443 |
|schemas.microsoft.com:80 |
|schemas.microsoft.com:443 |
|management.core.windows.net:443 |
|management.core.windows.net:80 |
|management.azure.com:443 |
|www.msftconnecttest.com:80 |
|shavamanifestcdnprod1.azureedge.net:443 |
|validation-v2.sls.microsoft.com:443 |
|flighting.cp.wd.microsoft.com:443 |
|dmd.metaservices.microsoft.com:80 |
|admin.core.windows.net:443 |
|prod.warmpath.msftcloudes.com:443 |
|prod.warmpath.msftcloudes.com:80 |
|azureprofileruploads.blob.core.windows.net:443 |
|azureprofileruploads2.blob.core.windows.net:443 |
|azureprofileruploads3.blob.core.windows.net:443 |
|azureprofileruploads4.blob.core.windows.net:443 |
|azureprofileruploads5.blob.core.windows.net:443 |
|azureprofilerfrontdoor.cloudapp.net:443 |

#### <a name="wildcard-httphttps-dependencies"></a>Platzhalter-HTTP/HTTPS-Abhängigkeiten 

| Endpunkt |
|----------|
|gr-Prod-\*.cloudapp.net:443 |
| \*.management.azure.com:443 |
| \*.update.microsoft.com:443 |
| \*.windowsupdate.microsoft.com:443 |

#### <a name="linux-dependencies"></a>Linux-Abhängigkeiten 

| Endpunkt |
|----------|
|wawsinfraprodbay063.blob.core.windows.net:443 |
|registry-1.docker.io:443 |
|auth.docker.io:443 |
|production.cloudflare.docker.com:443 |
|download.docker.com:443 |
|us.archive.ubuntu.com:80 |
|download.mono-project.com:80 |
|packages.treasuredata.com:80|
|security.ubuntu.com:80 |

<!--Image references-->
[1]: ./media/firewall-integration/firewall-apprule.png
[2]: ./media/firewall-integration/firewall-serviceendpoints.png
[3]: ./media/firewall-integration/firewall-ntprule.png
[4]: ./media/firewall-integration/firewall-routetable.png
[5]: ./media/firewall-integration/firewall-topology.png
