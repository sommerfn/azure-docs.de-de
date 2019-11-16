---
title: Sicherheitshärtung bei AKS-Hosts für virtuelle Computer
description: Hier erfahren Sie mehr über die Sicherheitshärtung beim AKS-Hostbetriebssystem für virtuelle Computer.
services: container-service
author: saudas
ms.service: container-service
ms.topic: article
ms.date: 09/11/2019
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 8b7e50fdc02ab47c50cecb95073f1b51393db898
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73885643"
---
# <a name="security-hardening-in-aks-virtual-machine-hosts"></a>Sicherheitshärtung bei AKS-Hosts für virtuelle Computer 

Azure Kubernetes Service (AKS) ist ein sicherer Dienst, der die Anforderungen der SOC-, ISO-, PCI-DSS- und HIPAA-Standards erfüllt. Dieser Artikel befasst sich mit der Sicherheitshärtung, die auf AKS-Hosts für virtuelle Computer angewendet wird. Weitere Informationen zur AKS-Sicherheit finden Sie unter [Sicherheitskonzepte für Anwendungen und Cluster in Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/concepts-security).

AKS-Cluster werden auf virtuellen Hostcomputern bereitgestellt, auf denen ein sicherheitsoptimiertes Betriebssystem ausgeführt wird. Dieses Hostbetriebssystem basiert derzeit auf einem Ubuntu 16.04.LTS-Image, auf das eine Reihe zusätzlicher Maßnahmen zur Sicherheitshärtung angewendet wird (siehe Details der Sicherheitshärtung).   

Das Ziel des sicherheitsgehärteten Hostbetriebssystems ist es, die Angriffsfläche zu verringern und die Bereitstellung von Containern auf sichere Weise zu ermöglichen. 

> [!Important]
> Das sicherheitsgehärtete Betriebssystem weist KEINE CIS-Benchmark auf. Obwohl es Überschneidungen mit CIS-Benchmarks gibt, ist das Ziel nicht, CIS-kompatibel zu sein. Die Zielsetzung für die Härtung des Hostbetriebssystems ist, eine Sicherheitsstufe zu erreichen, die den eigenen internen Microsoft-Sicherheitsstandards für Hosts entspricht. 

## <a name="security-hardening-features"></a>Features für die Sicherheitshärtung 

* AKS stellt standardmäßig ein sicherheitsoptimiertes Hostbetriebssystem bereit. Es gibt aktuell keine Option zur Auswahl eines alternativen Betriebssystems. 

* Azure wendet tägliche Patches (einschließlich Sicherheitspatches) auf AKS-Hosts für virtuelle Computer an. Für einige dieser Patches ist ein Neustart erforderlich, für andere hingegen nicht. Es ist Ihre Aufgabe, die Neustarts von AKS-VM-Hosts bedarfsgerecht zu planen. Anleitungen zum Automatisieren von AKS-Patches finden Sie unter [Patchen von AKS-Knoten](https://docs.microsoft.com/azure/aks/node-updates-kured).

Es folgt eine Zusammenfassung der Maßnahmen zur Härtung des Images, die in der AKS-Engine implementiert sind, um das sicherheitsoptimierte Hostbetriebssystem zu erzeugen. Die Maßnahmen wurden [in diesem GitHub-Projekt](https://github.com/Azure/aks-engine/projects/7) implementiert.  

Die AKS-Engine fördert oder entspricht derzeit keinem bestimmten Sicherheitsstandard, doch sind Überwachungs-IDs des CIS (Center for Internet Security) soweit zutreffend angegeben. 

## <a name="whats-configured"></a>Was ist konfiguriert?

| CIS  | Überwachungsbeschreibung| 
|---|---|
| 1.1.1.1 |Sicherstellen, dass die Einbindung von cramfs-Dateisystemen deaktiviert ist|
| 1.1.1.2 |Sicherstellen, dass die Einbindung von freevxfs-Dateisystemen deaktiviert ist|
| 1.1.1.3 |Sicherstellen, dass die Einbindung von jffs2-Dateisystemen deaktiviert ist|
| 1.1.1.4 |Sicherstellen, dass die Einbindung von HFS-Dateisystemen deaktiviert ist|
| 1.1.1.5 |Sicherstellen, dass die Einbindung von HFS Plus-Dateisystemen deaktiviert ist|
|1.4.3 |Sicherstellen, dass eine Authentifizierung für den Einzelbenutzermodus erforderlich ist |
|1.7.1.2 |Sicherstellen, dass das Warnbanner für lokale Anmeldung richtig konfiguriert ist |
|1.7.1.3 |Sicherstellen, dass das Warnbanner für Remoteanmeldung richtig konfiguriert ist |
|1.7.1.5 |Sicherstellen, dass Berechtigungen für „/etc/issue“ konfiguriert sind |
|1.7.1.6 |Sicherstellen, dass Berechtigungen für „/etc/issue.net“ konfiguriert sind |
|2.1.5 |Sicherstellen, dass „--streaming-connection-idle-timeout“ nicht auf 0 festgelegt ist |
|3.1.2 |Sicherstellen, dass das Senden von Paketumleitungen deaktiviert ist |
|3.2.1 |Sicherstellen, dass an die Quelle geleitete Pakete nicht akzeptiert werden |
|3.2.2 |Sicherstellen, dass ICMP-Umleitungen nicht akzeptiert werden |
|3.2.3 |Sicherstellen, dass sichere ICMP-Umleitungen nicht akzeptiert werden |
|3.2.4 |Sicherstellen, dass verdächtige Pakete protokolliert werden |
|3.3.1 |Sicherstellen, dass IPv6-Routerankündigungen nicht akzeptiert werden |
|3.5.1 |Sicherstellen, dass DCCP deaktiviert ist |
|3.5.2 |Sicherstellen, dass SCTP deaktiviert ist |
|3.5.3 |Sicherstellen, dass RDS deaktiviert ist |
|3.5.4 |Sicherstellen, dass TIPC deaktiviert ist |
|4.2.1.2 |Sicherstellen, dass die Protokollierung konfiguriert ist |
|5.1.2 |Sicherstellen, dass Berechtigungen für „/etc/crontab“ konfiguriert sind |
|5.2.4 |Sicherstellen, dass X11-Forwarding für SSH deaktiviert ist |
|5.2.5 |Sicherstellen, dass „MaxAuthTries“ für SSH auf höchstens 4 festgelegt ist |
|5.2.8 |Sicherstellen, dass Root-Anmeldung für SSH deaktiviert ist |
|5.2.10 |Sicherstellen, dass „PermitUserEnvironment“ für SSH deaktiviert ist |
|5.2.11 |Sicherstellen, dass nur genehmigte MAX-Algorithmen verwendet werden |
|5.2.12 |Sicherstellen, dass das Timeoutintervall für Leerlauf für SSH konfiguriert ist |
|5.2.13 |Sicherstellen, dass „LoginGraceTime“ für SSH auf höchstens eine Minute festgelegt ist |
|5.2.15 |Sicherstellen, dass das Warnbanner für SSH konfiguriert ist |
|5.3.1 |Sicherstellen, dass Anforderungen für die Kennworterstellung konfiguriert sind |
|5.4.1.1 |Sicherstellen, dass der Kennwortablauf höchstens 90 Tage beträgt |
|5.4.1.4 |Sicherstellen, dass die Sperre für inaktive Kennwörter höchstens 30 Tage beträgt |
|5.4.4 |Sicherstellen, dass der Befehl „umask“ für Standardbenutzer 027 oder stärker einschränkend ist |
|5.6 |Sicherstellen, dass der Zugriff auf den Befehl „su“ eingeschränkt ist|

## <a name="additional-notes"></a>Zusätzliche Hinweise
 
* Um die Angriffsfläche weiter zu verringern, wurden einige unnötige Kernelmodultreiber im Betriebssystem deaktiviert. 

* Das sicherheitsgehärtete Betriebssystem wird außerhalb der AKS-Plattform NICHT unterstützt. 

## <a name="next-steps"></a>Nächste Schritte  

Weitere Informationen zur AKS-Sicherheit finden Sie in den folgenden Artikeln: 

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/intro-kubernetes)

[AKS – Sicherheitshinweise](https://docs.microsoft.com/azure/aks/concepts-security)

[AKS – Bewährte Methoden](https://docs.microsoft.com/azure/aks/best-practices)
