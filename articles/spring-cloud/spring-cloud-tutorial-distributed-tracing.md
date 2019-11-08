---
title: 'Tutorial: Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie die verteilte Ablaufverfolgung von Spring Cloud über Azure Application Insights verwenden.
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: a9d2100103cdd5858d0d58cf6ef77a6ccac3745f
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607547"
---
# <a name="tutorial-using-distributed-tracing-with-azure-spring-cloud"></a>Tutorial: Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud

Die Tools für die verteilte Ablaufverfolgung von Spring Cloud ermöglicht das einfache Debuggen und Überwachen komplexer Probleme. Azure Spring Cloud integriert [Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth) in Azure [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview), um leistungsstarke Funktionen für die verteilte Ablaufverfolgung über das Azure-Portal bereitzustellen.

In diesem Artikel lernen Sie Folgendes:

> [!div class="checklist"]
> * Aktivieren der verteilten Ablaufverfolgung im Azure-Portal
> * Hinzufügen von Spring Cloud Sleuth zu Ihrer Anwendung
> * Anzeigen von Abhängigkeitszuordnungen für Ihre Microserviceanwendungen
> * Suchen nach Ablaufverfolgungsdaten mit unterschiedlichen Filtern

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Einen bereits bereitgestellten und ausgeführten Azure Spring Cloud-Dienst.  Führen Sie [diese Schnellstartanleitung](spring-cloud-quickstart-launch-app-cli.md) aus, um einen Azure Spring Cloud-Dienst bereitzustellen und zu starten.
    
## <a name="add-dependencies"></a>Hinzufügen von Abhängigkeiten

Ermöglichen Sie dem zipkin-Absender das Senden von Daten an das Web, indem Sie der Datei „application.properties“ die folgende Zeile hinzufügen:

```xml
spring.zipkin.sender.type = web
```

Sie können den nächsten Schritt überspringen, wenn Sie die [Anleitung zum Vorbereiten einer Azure Spring Cloud-Anwendung](spring-cloud-tutorial-prepare-app-deployment.md) befolgt haben. Wechseln Sie andernfalls zu Ihrer lokalen Entwicklungsumgebung, und bearbeiten Sie die Datei `pom.xml`, um die Spring Cloud Sleuth-Abhängigkeit einzufügen:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-sleuth</artifactId>
            <version>${spring-cloud-sleuth.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-sleuth</artifactId>
    </dependency>
</dependencies>
```

* Erstellen Sie den Azure Spring Cloud-Dienst, und stellen Sie ihn erneut bereit, damit diese Änderungen übernommen werden. 

## <a name="modify-the-sample-rate"></a>Ändern der Abtastrate
Durch Anpassen der Abtastrate können Sie die Rate für die Erfassung von Telemetriedaten ändern. Wenn Sie Daten beispielsweise nur halb so häufig abrufen möchten, navigieren Sie zur Datei `application.properties`, und ändern Sie die folgende Zeile:

```xml
spring.sleuth.sampler.probability=0.5
```

Wenn Sie bereits eine Anwendung erstellt und bereitgestellt haben, können Sie die Abtastrate ändern, indem Sie die obige Zeile als Umgebungsvariable in der Azure CLI oder im Portal hinzufügen. 

## <a name="enable-application-insights"></a>Aktivieren von Application Insights

1. Navigieren Sie im Azure-Portal zur Seite Ihres Azure Spring Cloud-Diensts.
1. Wählen Sie im Abschnitt „Überwachung“ die Option **Distributed Tracing** (Verteilte Ablaufverfolgung) aus.
1. Wählen Sie **Einstellung bearbeiten** aus, um eine neue Einstellung zu bearbeiten oder hinzuzufügen.
1. Erstellen Sie eine neue Application Insights-Abfrage, oder wählen Sie eine vorhandene aus.
1. Wählen Sie die zu überwachende Protokollkategorie aus, und geben Sie die Aufbewahrungsdauer (in Tagen) an.
1. Wählen Sie zum Anwenden der neuen Ablaufverfolgung die Option **Übernehmen** aus.

## <a name="view-application-map"></a>Anzeigen der Anwendungsübersicht

Kehren Sie zur Seite „Distributed Tracing“ (Verteilte Ablaufverfolgung) zurück, und wählen Sie **View application map** (Anwendungsübersicht anzeigen) aus. Überprüfen Sie die visuelle Darstellung Ihrer Anwendungs- und Überwachungseinstellungen. Informationen zur Verwendung der Anwendungsübersicht finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/azure-monitor/app/app-map).

## <a name="search"></a>Suchen,

Verwenden Sie die Suchfunktion, um andere spezifische Telemetrieelemente abzufragen. Wählen Sie auf der Seite **Distributed Tracing** (Verteilte Ablaufverfolgung) die Option **Suchen** aus. Weitere Informationen zur Verwendung der Suchfunktionen finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/azure-monitor/app/diagnostic-search).

## <a name="application-insights-page"></a>Application Insights-Seite

Application Insights bietet neben Anwendungsübersicht und Suche auch Überwachungsfunktionen. Suchen Sie im Azure-Portal nach dem Namen Ihrer Anwendung, und starten Sie dann eine Application Insights-Seite, um weitere Informationen zu erhalten. Weitere Informationen zur Verwendung dieser Tools finden Sie in der [Dokumentation](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language).


## <a name="disable-application-insights"></a>Deaktivieren von Application Insights

1. Navigieren Sie im Azure-Portal zur Seite Ihres Azure Spring Cloud-Diensts.
1. Klicken Sie im Abschnitt „Überwachung“ auf **Distributed Tracing** (Verteilte Ablaufverfolgung).
1. Klicken auf **Deaktivieren**, um Application Insights zu deaktivieren

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie weitere Informationen zur verteilten Ablaufverfolgung erhalten und erfahren, wie Sie die Funktion in Azure Spring Cloud aktivieren. Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Anwendung an Azure CosmosDB binden.

> [!div class="nextstepaction"]
> [Tutorial: Binden von Azure Cosmos DB an Ihre Azure Spring Cloud-Anwendung](spring-cloud-tutorial-bind-cosmos.md)
