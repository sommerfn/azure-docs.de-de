---
title: Bedrohungserkennung für Cloud Native Compute in Azure Security Center | Microsoft-Dokumentation
description: In diesem Thema werden die in Azure Security Center verfügbaren Warnungen für Cloud Native Compute vorgestellt. in Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 5aa5efcf-9f6f-4aa1-9f72-d651c6a7c9cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 14433806a28e31cef1a278e16cb69e7c9b1a2458
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295842"
---
# <a name="threat-detection-for-cloud-native-compute-in-azure-security-center"></a>Bedrohungserkennung für Cloud Native Compute in Azure Security Center

Als Cloudanbieter nutzt Security Center seine einzigartigen Möglichkeiten, um interne Protokolle zu analysieren und Angriffsmethoden für mehrere Ziele zu identifizieren. In diesem Thema werden die Warnungen vorgestellt, die für folgende Azure-Dienste zur Verfügung stehen:

* [Azure App Service](#app-services)
* [Container](#azure-containers) 

## Azure App Service <a name="app-services"></a>

Security Center nutzt die Kapazitäten der Cloud, um Angriffe auf Kundenanwendungen zu identifizieren, die über Azure App Service ausgeführt werden. Webanwendungen sind heutzutage praktisch in jedem modernen Netzwerk vorhanden, und Angreifer versuchen, diese ausfindig zu machen und Schwachstellen auszunutzen. Bevor Anforderungen für in Azure ausgeführte Anwendungen an bestimmte Umgebungen weitergeleitet werden, durchlaufen sie mehrere Gateways, wo sie jeweils überprüft und protokolliert werden. Diese Daten werden dann verwendet, um Exploits und Angreifer zu identifizieren sowie um neue Muster zu erkennen, die später zur Anwendung kommen.

Dank der Möglichkeiten, über die Azure als Cloudanbieter verfügt, kann Security Center interne App Service-Protokolle analysieren und Angriffsmethoden für mehrere Ziele identifizieren. Ein Beispiel wären etwa großflächige Scans und verteilte Angriffe. Bei dieser Art von Angriff, der in der Regel von einer kleinen Gruppe von IP-Adressen ausgeht, werden ähnliche Endpunkte auf mehreren Hosts durchforstet, um nach einer anfälligen Seite oder nach einem anfälligen Plug-In zu suchen. Dies kann mithilfe der Cloud erkannt werden, nicht jedoch aus der Perspektive eines einzelnen Hosts.

Security Center hat auch Zugriff auf die zugrunde liegenden Sandboxes und virtuellen Computer. In Kombination mit forensischen Techniken für den Arbeitsspeicher kann die Infrastruktur das gesamte Spektrum erfassen – von neu in Umlauf gebrachten Angriffen bis hin zu kompromittierten Kundencomputern. So kann Security Center Angriffe auf Webanwendungen erkennen, auch wenn diese bereits längere Zeit missbraucht werden.

> [!div class="mx-tableFixed"]

|Warnung|BESCHREIBUNG|
|---|---|
|**Suspicious WordPress theme invocation detected** (Verdächtigen Aufruf eines WordPress-Designs erkannt)|Das Aktivitätsprotokoll von Azure App Service enthält einen Hinweis auf eine mögliche Codeinjektionsaktivität für Ihre App Service-Ressource.<br/> Diese verdächtige Aktivität ähnelt einer Aktivität, die dazu dient, ein WordPress-Design zu verändern, sodass die serverseitige Ausführung von Code möglich wird – gefolgt von einer direkten Webanforderung zum Aufrufen der veränderten Designdatei. Diese Art von Aktivität wurde in der Vergangenheit bei Angriffen über WordPress beobachtet.|
|**Connection to web page from anomalous IP address detected** (Verbindung mit einer Webseite über eine anomale IP-Adresse erkannt)|Das Aktivitätsprotokoll von Azure App Service enthält einen Hinweis auf eine Verbindung mit einer sensiblen Webseite von einer Quelladresse, von der aus noch nie eine Verbindung mit dieser Seite hergestellt wurde. Dies deutet möglicherweise auf einen Brute-Force-Angriff auf Ihre Web-App-Verwaltungsseiten hin. Es ist aber auch denkbar, dass ein berechtigter Benutzer lediglich eine neue IP-Adresse verwendet.|
|**An IP that connected to your Azure App Service FTP Interface was found in Threat Intelligence** (Eine IP-Adresse, über die eine Verbindung mit Ihrer FTP-Schnittstelle von Azure App Service hergestellt wurde, wurde in Threat Intelligence gefunden.)|Bei der Analyse der Azure App Service-FTP-Protokolle wurde eine Verbindung mit einer Quelladresse aus dem Threat Intelligence-Feed erkannt. Im Rahmen dieser Verbindung hat ein Benutzer auf die im Anschluss aufgeführten Seiten zugegriffen.|
|**Web fingerprinting detected** (Erstellung eines digitalen Webfingerabdrucks erkannt)|Das Aktivitätsprotokoll von Azure App Service enthält einen Hinweis auf eine mögliche Aktivität zur Erstellung eines digitalen Webfingerabdrucks für Ihre App Service-Ressource. <br/>Diese verdächtige Aktivität hängt mit einem Tool namens Blind Elephant zusammen. Das Tool erstellt einen digitalen Fingerabdruck von Webservern und versucht, die installierten Anwendungen und deren Versionen zu ermitteln. Angreifer verwenden dieses Tool häufig, um die Webanwendungen auf Schwachstellen zu testen.|
|**Suspicious access to possibly vulnerable web page detected** (Verdächtigen Zugriff auf möglicherweise anfällige Webseite erkannt)|Das Aktivitätsprotokoll von Azure App Service enthält einen Hinweis auf einen Zugriff auf eine möglicherweise sensible Webseite. <br/>Diese verdächtige Aktivität geht von einer Quelladresse aus, deren Zugriffsmuster dem Zugriffsmuster eines Webscanners ähnelt. Diese Art von Aktivität ist häufig auf einen Angriffsversuch zurückzuführen, bei dem der Angreifer Ihr Netzwerk scannt und versucht, auf sensible oder anfällige Webseiten zuzugreifen.|
|**PHP file in upload folder** (PHP-Datei im Uploadordner)|Das Aktivitätsprotokoll von Azure App Service enthält einen Hinweis auf einen Zugriff auf eine verdächtige PHP-Seite im Uploadordner. <br/>Diese Art von Ordner enthält normalerweise keine PHP-Dateien. Ist ein solcher Dateityp vorhanden, kann dies auf die Ausnutzung von Sicherheitslücken beim Dateiupload hindeuten.|
|**An attempt to run Linux commands on a Windows App Service** (Versuchte Ausführung von Linux-Befehlen für eine App Service-Instanz unter Windows)|Bei der Analyse von App Service-Prozessen wurde festgestellt, dass versucht wurde, einen Linux-Befehl für eine App Service-Instanz unter Windows auszuführen. Diese Aktion wurde von der Webanwendung ausgeführt. Ein solches Verhalten ist häufig bei Angriffen zu beobachten, die sich eine Sicherheitslücke in einer gängigen Webanwendung zunutze machen.|
|**Suspicious PHP execution detected** (Verdächtige PHP-Ausführung erkannt)|Computerprotokolle enthalten einen Hinweis auf die Ausführung eines verdächtigen PHP-Prozesses. Bei der Aktion wurde versucht, mithilfe des PHP-Prozesses Betriebssystembefehle oder PHP-Code über die Befehlszeile auszuführen. Dieses Verhalten kann zwar legitim sein, in Webanwendungen ist es jedoch auch im Zusammenhang mit schädlichen Aktivitäten zu beobachten – etwa bei Versuchen, Websites mit Webshells zu infizieren.|
|**Process execution from temporary folder** (Ausführung eines Prozesses aus dem temporären Ordner)|Bei der Analyse von App Service-Prozessen wurde die Ausführung eines Prozesses aus dem temporären Ordner der App erkannt. Dieses Verhalten kann zwar legitim sein, in Webanwendungen ist es jedoch auch im Zusammenhang mit schädlichen Aktivitäten zu beobachten.|
|**Attempt to run high privilege command detected** (Versuchte Ausführung eines Befehls mit hohen Berechtigungen erkannt)|Bei der Analyse von App Service-Prozessen wurde die versuchte Ausführung eines Befehls erkannt, für den hohe Berechtigungen erforderlich sind. Der Befehl wurde im Kontext der Webanwendung ausgeführt. Dieses Verhalten kann zwar legitim sein, in Webanwendungen ist es jedoch auch im Zusammenhang mit schädlichen Aktivitäten zu beobachten.|

> [!NOTE]
> Die Security Center-Bedrohungserkennung für App Service ist momentan in Azure Government- und Sovereign Cloud-Regionen nicht verfügbar.

Weitere Informationen zu Warnungen der App Service-Bedrohungserkennung finden Sie unter „Schützen von App Services mit Azure Security Center“. Dort erfahren Sie auch, wie Sie die Überwachung und den Schutz Ihrer App Service-Workloads aktivieren.

## Container <a name="azure-containers"></a>

Security Center bietet eine Bedrohungserkennung in Echtzeit für Ihre Container auf Linux-Computern auf der Grundlage des AuditD-Frameworks. Die Warnungen identifizieren verschiedene verdächtige Docker-Aktivitäten, etwa die Erstellung eines privilegierten Containers auf dem Host (ein Anzeichen dafür, dass ein SSH-Server (Secure Shell) in einem Docker-Container ausgeführt wird) oder die Verwendung von Crypto-Minern. Mit diesen Informationen können Sie schnell Sicherheitsprobleme lösen und die Sicherheit Ihrer Container verbessern. Neben der Linux-Erkennung bietet Security Center auch spezifischere Analysen für Containerbereitstellungen.
