---
title: Selbstgehostetes Azure API Management-Gateway – Übersicht | Microsoft-Dokumentation
description: Erfahren Sie, wie ein selbstgehostetes Azure API Management-Gateway Organisationen beim Verwalten von APIs in Hybridumgebungen und Umgebungen mit mehreren Clouds unterstützt.
services: api-management
documentationcenter: ''
author: vlvinogr
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: 415f0e209e607a863d715b1a66a2435603a662f0
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510555"
---
# <a name="self-hosted-api-management-gateway-overview"></a>Selbstgehostetes API Management-Gateway – Übersicht

In diesem Artikel wird erläutert, wie das Feature „selbstgehostetes Gateway“ die API-Verwaltung in Hybridumgebungen und Umgebungen mit mehreren Clouds vereinfacht. Außerdem werden die allgemeine Architektur und die grundlegenden Funktionen vorgestellt.

> [!NOTE]
> Das Feature „selbstgehostetes Gateway“ befindet sich in der Vorschau. Während der Vorschauphase ist das selbstgehostete Gateway nur in den Tarifen „Developer“ und „Premium“ ohne zusätzliche Kosten verfügbar. Der Tarif „Developer“ ist auf eine Bereitstellung mit einem einzelnen selbstgehosteten Gateway beschränkt.

## <a name="hybrid-and-multi-cloud-api-management"></a>API-Verwaltung in Hybridumgebungen und Umgebungen mit mehreren Clouds

Das Feature „selbstgehostetes Gateway“ weitet die API Management-Unterstützung auf Hybridumgebungen und Umgebungen mit mehreren Clouds aus und ermöglicht es Organisationen, lokal und cloudübergreifend gehostete APIs über einen einzigen API Management-Dienst in Azure effizient und sicher zu verwalten.

Mit dem selbstgehosteten Gateway erhalten Kunden die Flexibilität, eine containerbasierte Version der API Management-Gatewaykomponente in den gleichen Umgebungen bereitzustellen, in denen auch die APIs gehostet werden. Alle selbstgehosteten Gateways werden über den API Management-Dienst verwaltet, mit dem sie sich im Verbund befinden. So profitieren Kunden von Transparenz und einheitlichen Verwaltungsfunktionen für alle internen und externen APIs. Indem die Gateways in der Nähe der APIs platziert werden, können Kunden die Datenverkehrsflows der APIs optimieren und Sicherheits- und Complianceanforderungen erfüllen.

Jeder API Management-Dienst besteht aus den folgenden Hauptkomponenten:

-   Eine Verwaltungsebene – als API verfügbar gemacht –, die zum Konfigurieren des Dienst über das Azure-Portal, PowerShell oder andere unterstützte Mechanismen verwendet wird
-   Eine Gatewayebene (bzw. Datenebene), auf der die Proxyweiterleitung von API-Anforderungen, die Anwendung von Richtlinien und die Sammlung von Telemetriedaten erfolgt
-   Ein Entwicklerportal, in dem Entwickler APIs ermitteln, kennen lernen und integrieren

Standardmäßig werden all diese Komponenten in Azure bereitgestellt, sodass sämtlicher API-Datenverkehr (auf dem Bild unten als durchgehende schwarze Pfeile dargestellt) durch Azure geleitet wird, unabhängig davon, wo die Back-End-Systeme gehostet werden, die die APIs implementieren. Mit dem einfachen Betrieb dieses Modells gehen allerdings höhere Latenzen, Complianceprobleme und in einigen Fällen zusätzliche Gebühren für die Datenübertragung einher.

![API-Datenverkehrsflow ohne selbstgehostete Gateways](media/self-hosted-gateway-overview/without-gateways.png)

Indem selbstgehostete Gateways in den gleichen Umgebungen wie die Implementierungen der Back-End-APIs bereitgestellt und zum API Management-Dienst hinzugefügt werden, kann der API-Datenverkehr direkt an die Back-End-APIs geleitet werden. Dies verbessert die Latenz, optimiert die Datenübertragungskosten und sorgt für Compliance. Gleichzeitig bleiben die Vorteile erhalten: Die Verwaltung und Ermittlung aller APIs in der Organisation erfolgt an einer zentralen Stelle – unabhängig davon, wo die Implementierungen gehostet werden.

![API-Datenverkehrsflow mit selbstgehosteten Gateways](media/self-hosted-gateway-overview/with-gateways.png)

## <a name="packaging-and-features"></a>Paketerstellung und Features

Das selbstgehostete Gateway ist eine containerbasierte, funktional gleichwertige Version des verwalteten Gateways, das im Rahmen jedes API Management-Diensts in Azure bereitgestellt wird. Das selbstgehostete Gateway ist in der Microsoft Container Registry als Linux-basierter Docker-Container verfügbar. Es kann in Docker, Kubernetes oder jeder anderen Lösung für die Containerorchestrierung bereitgestellt werden, die auf einem Desktopcomputer, in einem Servercluster oder in einer Cloudinfrastruktur ausgeführt wird.

> [!IMPORTANT]
> Einige Funktionen des verwalteten Gateways sind in der Vorschau noch nicht verfügbar. Dies gilt insbesondere für Folgendes: Richtlinie für Protokollierung in Event Hub, Service Fabric-Integration, Downstream-HTTP/2. Es gibt keine Pläne, im selbstgehosteten Gateway einen integrierten Cache zur Verfügung zu stellen.

## <a name="connectivity-to-azure"></a>Konnektivität mit Azure

Das selbstgehostete Gateway erfordert ausgehende TCP/IP-Konnektivität mit Azure an Port 443. Jedes selbstgehostete Gateway muss einer einzelnen API Management-Dienstinstanz zugeordnet werden und wird über deren Verwaltungsebene konfiguriert. Das selbstgehostete Gateway nutzt die Konnektivität mit Azure für Folgendes:

-   Melden des Status durch minütliches Senden von Heartbeatmeldungen
-   Regelmäßiges Überprüfen auf Konfigurationsupdates (alle 10 Sekunden) und Anwenden der Updates, sobald sie verfügbar sind
-   Senden von Anforderungsprotokollen und -metriken an Azure Monitor, sofern konfiguriert
-   Senden von Ereignissen an Application Insights, sofern eingerichtet

Wenn die Konnektivität mit Azure unterbrochen wird, kann das selbstgehostete Gateway keine Konfigurationsupdates empfangen, Statusmeldungen senden oder Telemetriedaten hochladen.

Das selbstgehostete Gateway ist für „fail-static“ konzipiert und übersteht temporäre Unterbrechungen der Konnektivität mit Azure. Es kann mit oder ohne aktivierte lokale Konfigurationssicherung bereitgestellt werden. Im ersten Fall speichern selbstgehostete Gateways in regelmäßigen Abständen eine Sicherungskopie der Konfiguration auf einem persistenten Volume, das an den Container oder Pod angeschlossen ist.

Wenn die Konfigurationssicherung deaktiviert ist und die Konnektivität mit Azure unterbrochen wird, geschieht Folgendes:

-   Ausgeführte selbstgehostete Gateways funktionieren weiterhin und verwenden eine arbeitsspeicherinterne Kopie der Konfiguration.
-   Angehaltene selbstgehostete Gateways können nicht gestartet werden.

Wenn die Konfigurationssicherung aktiviert ist und die Konnektivität mit Azure unterbrochen wird, geschieht Folgendes:

-   Ausgeführte selbstgehostete Gateways funktionieren weiterhin und verwenden eine arbeitsspeicherinterne Kopie der Konfiguration.
-   Angehaltene selbstgehostete Gateways starten unter Verwendung einer Sicherungskopie der Konfiguration.

Wenn die Konnektivität wiederhergestellt ist, stellt jedes vom Ausfall betroffene selbstgehostete Gateway automatisch wieder eine Verbindung mit dem zugehörigen API Management-Dienst her und lädt alle Konfigurationsupdates herunter, die zur Verfügung gestellt wurden, während das Gateway offline war.

## <a name="next-steps"></a>Nächste Schritte

-   [Whitepaper mit weiteren Hintergrundinformationen zu diesem Thema](https://aka.ms/hybrid-and-multi-cloud-api-management)
-   [Bereitstellen eines selbstgehosteten Gateways in Docker](api-management-howto-deploy-self-hosted-gateway-to-docker.md)
-   [Bereitstellen eines selbstgehosteten Gateways in Kubernetes](api-management-howto-deploy-self-hosted-gateway-to-k8s.md)
