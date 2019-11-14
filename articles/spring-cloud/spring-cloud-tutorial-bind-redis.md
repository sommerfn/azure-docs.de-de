---
title: Binden von Azure Cache for Redis an Ihre Azure Spring Cloud-Anwendung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure Cache for Redis an Ihre Azure Spring Cloud-Anwendung binden.
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/31/2019
ms.author: jeconnoc
ms.openlocfilehash: a901e4194909df85f53799d5937515e42ea87a69
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607573"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-cache-for-redis"></a>Tutorial: Binden von Azure-Diensten an Ihre Azure Spring Cloud-Anwendung: Azure Cache for Redis

Mit Azure Spring Cloud können Sie ausgewählte Azure-Dienste automatisch an Ihre Anwendungen binden, statt Ihre Spring Boot-Anwendung manuell zu konfigurieren. In diesem Artikel wird gezeigt, wie Sie Ihre Anwendung an Azure Cache for Redis binden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine bereitgestellte Azure Spring Cloud-Instanz
* Eine Azure Cache for Redis-Dienstinstanz
* Die Azure Spring Cloud-Erweiterung für die Azure-Befehlszeilenschnittstelle

Wenn Sie nicht über eine bereitgestellte Azure Spring Cloud-Instanz verfügen, folgen Sie den Schritten in diesem [Schnellstart](spring-cloud-quickstart-launch-app-portal.md), um Ihre erste Spring Cloud-App bereitzustellen.

## <a name="bind-azure-cache-for-redis"></a>Binden von Azure Cache for Redis

1. Fügen Sie der Datei `pom.xml` Ihres Projekts die folgende Abhängigkeit hinzu:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
    ```
1. Entfernen Sie ggf. die `spring.redis.*`-Eigenschaften in der Datei `application.properties`.

1. Aktualisieren Sie die aktuelle Bereitstellung mit `az spring-cloud app update`, oder erstellen Sie eine neue Bereitstellung mithilfe von `az spring-cloud app deployment create`.

1. Navigieren Sie im Azure-Portal zur Seite Ihres Azure Spring Cloud-Diensts. Suchen Sie das **Anwendungsdashboard**, und wählen Sie die Anwendung aus, die an Azure Cache for Redis gebunden werden soll.  Dabei handelt es sich um dieselbe Anwendung, die Sie im vorherigen Schritt aktualisiert oder bereitgestellt haben. Wählen Sie `Service binding` und dann die Schaltfläche `Create service binding` aus. Füllen Sie das Formular aus. Wählen Sie den **Bindungstyp** `Azure Cache for Redis`, Ihren Redis-Server und die Option „Primärschlüssel“ aus. 

1. Starten Sie die App neu. Die Bindung sollte jetzt funktionieren.

1. Um sicherzustellen, dass die Dienstbindung richtig ist, wählen Sie den Bindungsnamen aus, und überprüfen Sie dessen Details. Das Feld `property` sollte wie folgt aussehen:
    ```
    spring.redis.host=some-redis.redis.cache.windows.net
    spring.redis.port=6380
    spring.redis.password=abc******
    spring.redis.ssl=true
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie Ihre Azure Spring Cloud-Anwendung an Azure Redis Cache binden können.  Weitere Informationen zum Binden von Diensten an Ihre Anwendung finden Sie im Tutorial zum Binden einer Anwendung an eine MySQL-Datenbank.

> [!div class="nextstepaction"]
> [Tutorial: Binden von Azure-Diensten an Ihre Azure Spring Cloud-Anwendung: Azure Database for MySQL](spring-cloud-tutorial-bind-mysql.md)