---
title: Einführung in Azure Spring Cloud
description: Lernen Sie die Features und Vorteile von Azure Spring Cloud zur Bereitstellung und Verwaltung von Java Spring-Anwendungen in Azure kennen.
author: jpconnock
ms.service: spring-cloud
ms.topic: overview
ms.date: 11/4/2019
ms.author: jeconnoc
ms.openlocfilehash: 9709d7edb7c48ccfdb551d0e837a098289a5097a
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721269"
---
# <a name="what-is-azure-spring-cloud"></a>Was ist Azure Spring Cloud?

Azure Spring Cloud vereinfacht das Bereitstellen von Spring Boot-basierten Microserviceanwendungen ohne Codeänderungen in Azure.  Azure Spring Cloud verwaltet den Lebenszyklus von Spring Cloud-Anwendungen und ermöglicht es Entwicklern so, sich auf ihren Code zu konzentrieren.  Spring Cloud bietet Lebenszyklusverwaltung durch umfassende Überwachung und Diagnose, Konfigurationsverwaltung, Dienstermittlung, CI/CD-Integration, Blaugrün-Bereitstellungen und mehr.

Als Teil des Azure-Ökosystems ermöglicht Azure Spring Cloud eine einfache Anbindung an andere Azure-Dienste wie Speicher, Datenbanken, Überwachung und mehr.

Azure Spring Cloud wird zurzeit als Public Preview angeboten. Angebote der Public Preview ermöglichen Kunden das Experimentieren mit neuen Funktionen vor der offiziellen Veröffentlichung.  Funktionen und Dienste in der Public Preview sind nicht zur Verwendung in der Produktion bestimmt.  Weitere Informationen zur Unterstützung während der Vorschauphase finden Sie in den [häufig gestellten Fragen](https://azure.microsoft.com/support/faq/). Sie können auch eine [Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) einreichen, um weitere Informationen zu erhalten.

Führen Sie zum Einstieg den Spring Cloud-Schnellstart mit der [Azure CLI](spring-cloud-quickstart-launch-app-cli.md), dem [Azure-Portal](spring-cloud-quickstart-launch-app-portal.md) oder [Maven](spring-cloud-quickstart-launch-app-maven.md) durch.

## <a name="application-configuration"></a>Anwendungskonfiguration

### <a name="spring-cloud-config-server"></a>Spring Cloud-Konfigurationsserver

Der Azure Spring Cloud-Konfigurationsserver bietet externalisierte Konfiguration in einem verteilten System mit server- und clientseitiger Unterstützung.  Der Konfigurationsserver bietet einen zentralen Ort zum Verwalten von Anwendungseigenschaften in allen Umgebungen.  Weitere Informationen finden Sie in der [Referenz zum Spring Cloud-Konfigurationsserver](https://spring.io/projects/spring-cloud-config.md) und im entsprechenden Tutorial.

### <a name="enable-bluegreen-deployments"></a>Blau/Grün-Bereitstellungen aktivieren

Azure Spring Cloud unterstützt Blau/Grün-Bereitstellungen zur Freigabe und Aktualisierung von Code für Produktionsumgebungen.  Dieses Change Management-Muster ermöglicht es Entwicklern, Funktionen und Codeänderungen mit der Sicherheit eines sofortigen Fallbacks bei Bedarf zu implementieren.  Azure ermöglicht es Entwicklern, sich auf das Schreiben von Code zu konzentrieren, indem mehrere Produktionsumgebungen verwaltet werden und das Aktualisieren oder Rückgängigmachen von Codeänderungen ohne Unterbrechung der Anwendung problemlos möglich ist.  Weitere Informationen zu Stagingumgebungen und Blau/Grün-Bereitstellungen finden Sie in diesem [Anleitungsartikel](spring-cloud-howto-staging-environment.md).

### <a name="automate-cicd-pipelines"></a>Automatisieren von CI/CD-Pipelines

Azure Spring Cloud ermöglicht die Integration mit Azure DevOps mithilfe der Azure CLI.  Mithilfe von Azure DevOps können Sie die Integration und Bereitstellung von Code für Ihre Spring-Anwendung automatisieren.  Weitere Informationen finden Sie in diesem [Artikel](spring-cloud-howto-cicd.md).

### <a name="scale-your-application"></a>Skalieren Ihrer Anwendung

Azure Spring Cloud ermöglicht Ihnen das einfache Skalieren der Microservices in Ihrem Azure Spring Cloud-Dashboard.  Sowohl die Anzahl der vCPUs als auch die Speichermenge, die für Ihre Microservices zur Verfügung steht, können je nach Bedarf erhöht oder verringert werden.  Die Skalierung wird in Sekunden wirksam und erfordert weder Codeänderungen noch die erneute Bereitstellung.  Führen Sie dieses [Tutorial](spring-cloud-tutorial-scale-manual.md) aus, um mehr zu erfahren.

## <a name="application-monitoring"></a>Anwendungsüberwachung

### <a name="monitor-your-application-using-distributed-tracing-and-azure-app-insights"></a>Überwachen Ihrer Anwendung mit verteilter Ablaufverfolgung und Azure App Insights

Die Tools für die verteilte Ablaufverfolgung von Spring Cloud ermöglichen Entwicklern das Debuggen und Überwachen der komplexen Verbindungen zwischen Microservices in der Anwendung.  Durch die Integration von [Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth) in Azure [Application Insights](../azure-monitor/insights/insights-overview.md) stellt Azure leistungsstarke Funktionen für die verteilte Ablaufverfolgung direkt über das Azure-Portal bereit.  Führen Sie dieses [Tutorial](spring-cloud-tutorial-distributed-tracing.md) aus, um mehr zu erfahren.

## <a name="next-steps"></a>Nächste Schritte

- [Starten Ihrer Spring Cloud-App über die Befehlszeilenschnittstelle](spring-cloud-quickstart-launch-app-cli.md)
