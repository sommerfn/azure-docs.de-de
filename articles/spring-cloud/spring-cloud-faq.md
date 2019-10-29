---
title: Häufig gestellte Fragen zu Azure Spring Cloud | Microsoft-Dokumentation
description: Lesen Sie die häufig gestellten Fragen zu Azure Spring Cloud.
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: 69b3827bbb53a5f989f3bdec0d5c4ea6035130f0
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72592309"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Dieser Artikel behandelt häufig gestellte Fragen zu Azure Spring Cloud. 

## <a name="general"></a>Allgemein

### <a name="why-azure-spring-cloud"></a>Vorteile von Azure Spring Cloud

Azure Spring Cloud bietet Spring-Entwicklern eine PaaS-Lösung (Platform-as-a-Service). Azure Spring Cloud verwaltet Ihre Anwendungsinfrastruktur, damit Sie sich auf den Anwendungscode und die Geschäftslogik konzentrieren können. Zu den wichtigsten in Azure Spring Cloud integrierten Funktionen gehören unter anderem Eureka, ein Konfigurationsserver, ein Dienstregistrierungsserver, ein Pivotal-Builddienst sowie Blaugrün-Bereitstellungen. Mit diesem Dienst können Entwickler außerdem Ihre Anwendungen an Azure-Dienste wie CosmosDB, MySQL und Azure Cache for Redis binden.

Azure Spring Cloud verbessert durch die Integration von Azure Monitor, Application Insights und Log Analytics die Anwendungsdiagnose für Entwickler und Operatoren.

### <a name="what-service-plans-does-azure-spring-cloud-offer"></a>Welche Dienstpläne sind für Azure Spring Cloud verfügbar?

Während der Vorschauphase bietet Azure Spring Cloud nur einen Serviceplan.  Eine Spring Cloud-Bereitstellung enthält 16 vCPU-Kerne und 32 GB Arbeitsspeicher.  Die obere Grenze für jede Microservice-Instanz innerhalb einer Bereitstellung beträgt 4 vCPU-Kerne mit 8 GB Arbeitsspeicher.

Resource | Amount
------- | -------
App-Instanzen pro Spring-Anwendung | 20
Gesamtanzahl der App-Instanzen pro Azure Spring Cloud-Dienstinstanz | 50*
Azure Spring Cloud-Dienstinstanzen pro Region und Abonnement | 2*
Persistente Volumes | 10 × 50 GB

*_Erstellen Sie ein [Supportticket](https://azure.microsoft.com/support/faq/), um den Grenzwert zu erhöhen._

Weitere Informationen finden Sie unter [Häufig gestellte Fragen zum Azure Support](https://azure.microsoft.com/support/faq/).

### <a name="how-secure-is-azure-spring-cloud"></a>Wie sicher ist Azure Spring Cloud?

Sicherheit und Datenschutz sind eine der wichtigsten Prioritäten für Azure- und Azure Spring Cloud-Kunden. Azure gewährleistet durch eine sichere Datenverschlüsselung, dass nur der Kunde Zugriff auf Anwendungsdaten, Protokolle oder Konfigurationen hat. In Azure Spring Cloud sind alle Dienstinstanzen voneinander isoliert.

Azure Spring Cloud bietet eine umfassende SSL- und Zertifikatverwaltung.

Wichtige Sicherheitspatches für OpenJDK- und Spring Cloud-Runtimes werden so bald wie möglich auf Azure Spring Cloud angewendet.

### <a name="which-regions-azure-spring-cloud-are-available"></a>In welchen Regionen ist Azure Spring Cloud verfügbar?

„USA, Osten“, „USA, Westen 2“, „Europa, Westen“ und „Asien, Südosten“.

### <a name="what-are-the-known-limitations-of-azure-spring-cloud"></a>Welche Einschränkungen sind bezüglich Azure Spring Cloud bekannt?

Im Folgenden finden Sie die bekannten Einschränkungen der Vorschauversion des Diensts.

* `spring.application.name` wird durch den Anwendungsnamen überschrieben, der zum Erstellen der einzelnen Anwendungen verwendet wird.
* `server.port` ist in der Konfigurationsdatei aus dem Git-Repository nicht zulässig. Das Hinzufügen zur Konfigurationsdatei führt wahrscheinlich dazu, dass Ihre Anwendung über andere Anwendungen oder das Internet nicht erreichbar ist.
* Das Hochladen von Anwendungspaketen wird vom Azure-Portal und von Resource Manager-Vorlagen nicht unterstützt. Dies kann nur über die Anwendungsbereitstellung durch die Azure CLI erfolgen.
* Informationen zu Kontingentbeschränkungen finden Sie unter [Welche Dienstpläne sind für Azure Spring Cloud verfügbar?](#what-service-plans-does-azure-spring-cloud-offer).

### <a name="how-can-i-provide-feedback-and-report-issues"></a>Wie kann ich Feedback geben und Probleme melden?

Wenn Sie Ihre Spring-Dienstinstanzen in Azure Spring Cloud erstellt haben, können Sie eine [Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) erstellen. Wenn Sie das Onboarding in Azure Spring Cloud noch nicht durchgeführt haben, können Sie zu [Azure-Feedback](https://feedback.azure.com/) navigieren, um Funktionen anzufordern oder Feedback zu geben.

## <a name="development"></a>Entwicklung

### <a name="i-am-a-spring-developer-but-new-to-azure-what-is-the-quickest-way-for-me-to-learn-how-to-develop-aazure-spring-cloud-application"></a>Ich bin Spring-Entwickler, habe aber noch nicht mit Azure gearbeitet. Wie lerne ich am schnellsten, wie ich eine Azure Spring Cloud-Anwendung entwickle?

Der Einstieg in Azure Spring Cloud gelingt am schnellsten, wenn Sie die Anweisungen in [diesem Schnellstart](spring-cloud-quickstart-launch-app-portal.md) befolgen.

### <a name="what-java-runtime-does-azure-spring-cloud-support"></a>Welche Java-Runtime unterstützt Azure Spring Cloud?

Azure Spring Cloud unterstützt Java 8 und 11.

### <a name="where-can-i-see-my-spring-application-logs-and-metrics"></a>Wo kann ich Spring-Anwendungsprotokolle und -Metriken einsehen?

Metriken finden Sie auf der Registerkarte „App Overview“ (App-Übersicht) sowie der Registerkarte [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics#interacting-with-azure-monitor-metrics).

Azure Spring Cloud unterstützt das Exportieren von Spring-Anwendungsprotokollen und -metriken in Azure Storage, EventHub und [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs#log-queries). Der Tabellenname in Log Analytics lautet `AppPlatformLogsforSpring`. Zum Aktivieren lesen Sie diesen Artikel zu unseren [Diagnosediensten](diagnostic-services.md).

### <a name="does-azure-spring-cloud-support-distributed-tracing"></a>Unterstützt Azure Spring Cloud die verteilte Ablaufverfolgung?

Ja, weitere Informationen finden Sie unter [Verteilte Ablaufverfolgung](spring-cloud-tutorial-distributed-tracing.md).

### <a name="what-resource-types-does-service-binding-support"></a>Welche Ressourcentypen werden von der Dienstbindung unterstützt?

Derzeit werden drei Dienste unterstützt: Azure Cosmos DB, Azure Database for MySQL und Azure Cache for Redis.

### <a name="can-i-viewaddmove-persistent-volumes-from-inside-my-applications"></a>Kann ich persistente Volumes innerhalb meiner Anwendungen anzeigen/hinzufügen/verschieben?
Ja.

## <a name="deployment"></a>Bereitstellung

### <a name="does-azure-spring-cloud-support-blue-green-deployment"></a>Unterstützt Azure Spring Cloud Blaugrün-Bereitstellungen?
Ja, weitere Informationen finden Sie im [Leitfaden zur Stagingumgebung](spring-cloud-howto-staging-environment.md).

### <a name="can-i-access-kubernetes-to-manipulate-my-application-containers"></a>Kann ich auf Kubernetes zugreifen, um meine Anwendungscontainer zu bearbeiten?

Nein.  Azure Spring Cloud trennt den Entwickler von der zugrunde liegenden Architektur, sodass Sie sich auf den Anwendungscode und die Geschäftslogik konzentrieren können.

### <a name="does-azure-spring-cloud-support-building-containers-from-source"></a>Unterstützt Azure Spring Cloud das Erstellen von Containern aus einer Quelle?

Ja, weitere Informationen finden Sie unter [Bereitstellen aus der Quelle](spring-cloud-launch-from-source.md).

### <a name="does-azure-spring-cloud-support-autoscaling-in-app-instances"></a>Unterstützt Azure Spring Cloud die automatische Skalierung in App-Instanzen?

Nein.

### <a name="what-are-the-best-practices-for-migrating-existing-spring-microservices-to-azure-spring-cloud"></a>Was sind bewährte Methoden für die Migration vorhandener Spring-Microservices zu Azure Spring Cloud?

Folgendes müssen Sie vor der Migration bestehender Spring-Microservices zu Azure Spring Cloud beachten:
* Alle Anwendungsabhängigkeiten müssen aufgelöst werden.
* Bereiten Sie Ihre Konfigurationseinträge, Umgebungsvariablen und JVM-Parameter so vor, dass Sie sie mit der Bereitstellung in Azure Spring Cloud vergleichen können.
* Wenn Sie die Dienstbindung verwenden möchten, überprüfen Sie Ihre Azure-Dienste, und stellen Sie sicher, dass Sie die entsprechenden Zugriffsberechtigungen festgelegt haben.
* Wir empfehlen das Entfernen oder Deaktivieren eingebetteter Dienste wie dem Dienstermittlungsdienst, dem Konfigurationsserver usw., die möglicherweise mit von Azure Spring Cloud verwalteten Diensten in Konflikt stehen.
*-* Wir empfehlen Ihnen, offizielle und stabile Pivotal Spring-Bibliotheken zu verwenden. Inoffizielle oder verzweigte Versionen bzw. Betaversionen von Pivotal Spring-Bibliotheken haben keine SLA-Unterstützung.

Überwachen Sie nach der Migration CPU-/RAM-Metriken und den Netzwerkdatenverkehr, um sicherzustellen, dass die Anwendungsinstanzen entsprechend skaliert werden.

## <a name="next-steps"></a>Nächste Schritte

[Wenn Sie weitere Fragen haben, finden Sie im Handbuch zur Problembehandlung ausführlichere Informationen](spring-cloud-troubleshoot.md).