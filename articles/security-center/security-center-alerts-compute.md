---
title: Bedrohungserkennung für cloudnatives Computing in Azure Security Center | Microsoft-Dokumentation
description: In diesem Artikel werden die in Azure Security Center verfügbaren Warnungen für cloudnatives Computing vorgestellt.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5aa5efcf-9f6f-4aa1-9f72-d651c6a7c9cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: f6fba7bc8e8b7d040805f0be62d436caebf638af
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73520903"
---
# <a name="threat-detection-for-cloud-native-computing-in-azure-security-center"></a>Bedrohungserkennung für cloudnatives Computing in Azure Security Center

Als native Lösung bietet Azure Security Center einzigartige Einblicke in interne Protokolle zur Erkennung von Angriffsmethoden über mehrere Ziele hinweg. In diesem Artikel werden die Warnungen vorgestellt, die für folgende Azure-Dienste zur Verfügung stehen:

* [Azure App Service](#app-services)
* [Azure-Container](#azure-containers) 

> [!NOTE]
> Diese Dienste sind derzeit nicht in Azure Government- und Sovereign Cloud-Regionen verfügbar.

## Azure App Service <a name="app-services"></a>

Security Center nutzt die Kapazitäten der Cloud, um Angriffe auf Anwendungen unter App Service zu identifizieren. Angreifer testen Webanwendungen, um Schwachstellen zu suchen und auszunutzen. Bevor Anforderungen für in Azure ausgeführte Anwendungen an bestimmte Umgebungen weitergeleitet werden, durchlaufen sie mehrere Gateways, wo sie jeweils überprüft und protokolliert werden. Diese Daten werden dann verwendet, um Exploits und Angreifer zu identifizieren sowie um neue Muster zu erkennen, die später zur Anwendung kommen.

Dank der Möglichkeiten, über die Azure als Cloudanbieter verfügt, kann Security Center interne App Service-Protokolle analysieren und Angriffsmethoden für mehrere Ziele identifizieren. Die Methodik umfasst beispielsweise großflächige Scans und verteilte Angriffe. Bei dieser Art von Angriff, der in der Regel von einer kleinen Untergruppe von IP-Adressen ausgeht, werden ähnliche Endpunkte auf mehreren Hosts durchforstet. Die Angriffe dienen dazu, anfällige Seiten oder Plug-Ins ausfindig zu machen, und sind aus der Perspektive eines einzelnen Hosts nicht erkennbar.

Security Center hat auch Zugriff auf die zugrunde liegenden Sandboxes und virtuellen Computer. In Kombination mit forensischen Techniken für den Arbeitsspeicher kann die Infrastruktur das gesamte Spektrum erfassen – von neu in Umlauf gebrachten Angriffen bis hin zu kompromittierten Kundencomputern. Auch wenn Security Center bereitgestellt wird, nachdem eine Web-App ausgenutzt wurde, können fortlaufende Angriffe möglicherweise erkannt werden.

> [!div class="mx-tableFixed"]

|Warnung|BESCHREIBUNG|
|---|---|
|**Suspicious WordPress theme invocation detected** (Verdächtigen Aufruf eines WordPress-Designs erkannt)|Das App Service-Aktivitätsprotokoll von enthält einen Hinweis auf eine mögliche Codeinjektionsaktivität für Ihre App Service-Ressource.<br/> Diese verdächtige Aktivität ähnelt einer Aktivität, die dazu dient, ein WordPress-Design zu verändern, sodass die serverseitige Ausführung von Code möglich wird – gefolgt von einer direkten Webanforderung zum Aufrufen der veränderten Designdatei. Diese Art von Aktivität kann Teil eines Angriffs über WordPress sein.|
|**Connection to web page from anomalous IP address detected** (Verbindung mit einer Webseite über eine anomale IP-Adresse erkannt)|Das App Service-Aktivitätsprotokoll enthält einen Hinweis auf eine Verbindung mit einer sensiblen Webseite von einer Quelladresse, von der aus noch nie eine Verbindung mit dieser Seite hergestellt wurde. Dies deutet möglicherweise auf einen Brute-Force-Angriff auf Ihre Web-App-Verwaltungsseiten hin. Es ist aber auch denkbar, dass ein berechtigter Benutzer eine neue IP-Adresse verwendet.|
|**An IP that connected to your Azure App Service FTP Interface was found in Threat Intelligence** (Eine IP-Adresse, über die eine Verbindung mit Ihrer FTP-Schnittstelle von Azure App Service hergestellt wurde, wurde in Threat Intelligence gefunden.)|Bei der Analyse der App Service-FTP-Protokolle wurde eine Verbindung mit einer Quelladresse aus dem Threat Intelligence-Feed erkannt. Im Rahmen dieser Verbindung hat ein Benutzer auf die aufgeführten Seiten zugegriffen.|
|**Web fingerprinting detected** (Erstellung eines digitalen Webfingerabdrucks erkannt)|Das App Service-Aktivitätsprotokoll enthält einen Hinweis auf eine mögliche Aktivität zur Erstellung eines digitalen Webfingerabdrucks für Ihre App Service-Ressource. <br/>Diese verdächtige Aktivität hängt mit einem Tool namens Blind Elephant zusammen. Das Tool erstellt einen digitalen Fingerabdruck von Webservern und versucht, die installierten Anwendungen und deren Versionen zu ermitteln. Angreifer verwenden dieses Tool häufig, um die Webanwendungen auf Schwachstellen zu testen.|
|**Suspicious access to possibly vulnerable web page detected** (Verdächtigen Zugriff auf möglicherweise anfällige Webseite erkannt)|Das App Service-Aktivitätsprotokoll enthält einen Hinweis auf einen Zugriff auf eine möglicherweise sensible Webseite. <br/>Diese verdächtige Aktivität geht von einer Quelladresse aus, deren Zugriffsmuster dem Zugriffsmuster eines Webscanners ähnelt. Diese Art von Aktivität ist häufig auf einen Angriffsversuch zurückzuführen, bei dem der Angreifer Ihr Netzwerk scannt und versucht, auf sensible oder anfällige Webseiten zuzugreifen.|
|**PHP file in upload folder** (PHP-Datei im Uploadordner)|Das App Service-Aktivitätsprotokoll enthält einen Hinweis auf einen Zugriff auf eine verdächtige PHP-Seite im Uploadordner. <br/>Diese Art von Ordner enthält normalerweise keine PHP-Dateien. Ist ein solcher Dateityp vorhanden, kann dies auf die Ausnutzung von Sicherheitslücken beim Dateiupload hindeuten.|
|**An attempt to run Linux commands on a Windows App Service** (Versuchte Ausführung von Linux-Befehlen für eine App Service-Instanz unter Windows)|Bei der Analyse von App Service-Prozessen wurde festgestellt, dass versucht wurde, einen Linux-Befehl für eine App Service-Instanz unter Windows auszuführen. Diese Aktion wurde von der Webanwendung ausgeführt. Ein solches Verhalten ist häufig bei Angriffen zu beobachten, die sich eine Sicherheitslücke in einer gängigen Webanwendung zunutze machen.|
|**Suspicious PHP execution detected** (Verdächtige PHP-Ausführung erkannt)|Computerprotokolle enthalten einen Hinweis auf die Ausführung eines verdächtigen PHP-Prozesses. Bei der Aktion wurde versucht, mithilfe des PHP-Prozesses Betriebssystembefehle oder PHP-Code über die Befehlszeile auszuführen. Dieses Verhalten kann zwar legitim sein, in Webanwendungen kann es jedoch auch auf schädliche Aktivitäten hindeuten – etwa bei Versuchen, Websites mit Webshells zu infizieren.|
|**Process execution from temporary folder** (Ausführung eines Prozesses aus dem temporären Ordner)|Bei der Analyse von App Service-Prozessen wurde die Ausführung eines Prozesses aus dem temporären Ordner der App erkannt. Dieses Verhalten kann zwar legitim sein, in Webanwendungen kann es jedoch auch auf schädliche Aktivitäten hindeuten.|
|**Attempt to run high privilege command detected** (Versuchte Ausführung eines Befehls mit hohen Berechtigungen erkannt)|Bei der Analyse von App Service-Prozessen wurde die versuchte Ausführung eines Befehls erkannt, für den hohe Berechtigungen erforderlich sind. Der Befehl wurde im Kontext der Webanwendung ausgeführt. Dieses Verhalten kann zwar legitim sein, in Webanwendungen kann es jedoch auch auf schädliche Aktivitäten hindeuten.|

## Azure-Container <a name="azure-containers"></a>

Security Center bietet eine Echtzeit-Bedrohungserkennung für Ihre Containerumgebungen und generiert Warnungen für verdächtige Aktivitäten. Mit diesen Informationen können Sie schnell Sicherheitsprobleme lösen und die Sicherheit Ihrer Container verbessern.

Bedrohungen auf unterschiedlichen Ebenen werden erkannt: 

* **Hostebene**: der Security Center-Agent (verfügbar im Tarif „Standard“, weitere Informationen unter [Preise](security-center-pricing.md)) überwacht Linux auf verdächtige Aktivitäten. Der Agent löst Warnungen für verdächtige Aktivitäten aus, die aus dem Knoten oder einem darauf ausgeführten Container stammen. Beispiele für derartige Aktivitäten sind Webshell-Erkennung und Verbindungen mit bekannten verdächtigen IP-Adressen. </br>
Um einen tieferen Einblick in die Sicherheit Ihrer Containerumgebung zu erhalten, überwacht der Agent containerspezifische Analysen. Er löst Warnungen für Ereignisse aus, z.B. die Erstellung privilegierter Container, den verdächtigen Zugriff auf API-Server und Secure Shell (SSH)-Server, die in einem Docker-Container ausgeführt werden.

    >[!NOTE]
    > Wenn Sie die Agents nicht auf Ihren Hosts installieren möchten, kommen Sie nur in den Genuss eines Teils der Vorteile und Warnungen der Bedrohungserkennung. Sie erhalten weiterhin Warnungen im Zusammenhang mit der Netzwerkanalyse und der Kommunikation mit schädlichen Servern.

* Für die **AKS-Clusterebene** ist eine Bedrohungserkennung basierend auf der Analyse der Kubernetes-Überwachungsprotokolle vorhanden. Um diese Überwachung **ohne Agents** zu aktivieren, fügen Sie die Kubernetes-Option über die Seite **Preise und Einstellungen** (siehe [Preise](security-center-pricing.md)) zu Ihrem Abonnement hinzu. Zum Generieren von Warnungen auf dieser Ebene überwacht Security Center Ihre von AKS verwalteten Dienste mithilfe der von AKS abgerufenen Protokolle. Beispiele für Ereignisse auf dieser Ebene sind offengelegte Kubernetes-Dashboards, die Erstellung von Rollen mit hohen Berechtigungen und sensiblen Einbindungen. 

    >[!NOTE]
    > Security Center generiert Erkennungswarnungen für Azure Kubernetes Service-Aktionen und -Bereitstellungen, die nach der Aktivierung der Kubernetes-Option in den Abonnementeinstellungen erfolgen. 

Außerdem wird die Bedrohungslandschaft von unserem globalen Team von Sicherheitsforschern ständig überwacht. Sie fügen containerspezifische Warnungen und Sicherheitsrisiken hinzu, sobald sie erkannt werden.