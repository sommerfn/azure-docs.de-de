---
title: Binden von Azure Cache for Redis an Ihre Azure Spring Cloud-Anwendung | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure Cache for Redis an Ihre Azure Spring Cloud-Anwendung binden.
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/06/2019
ms.author: v-vasuke
ms.openlocfilehash: d8fa31207baecc80674fb11b492927800676b8a2
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038239"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-cache-for-redis"></a>Tutorial: Binden von Azure-Diensten an Ihre Azure Spring Cloud-Anwendung: Azure Cache for Redis

Mit Azure Spring Cloud können Sie ausgewählte Azure-Dienste automatisch an Ihre Anwendungen binden, statt Ihre Spring Boot-Anwendung manuell zu konfigurieren. In diesem Artikel wird gezeigt, wie Sie Ihre Anwendung an Azure Cache for Redis binden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine bereitgestellte Azure Spring Cloud-Instanz
* Eine Azure Cache for Redis-Dienstinstanz
* Die Azure Spring Cloud-Erweiterung für die Azure-Befehlszeilenschnittstelle

Führen Sie ggf. den folgenden Befehl aus, um die Azure Spring Cloud-Erweiterung für die Azure CLI zu installieren:

```azurecli
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

>[!TIP]
> Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können.  Sie verfügt über allgemeine vorinstallierte Azure-Tools, u. a. die aktuellen Versionen von Git, JDK, Maven und der Azure-Befehlszeilenschnittstelle. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie [Azure Cloud Shell](https://shell.azure.com) über shell.azure.com.  Weitere Informationen zu Azure Cloud Shell finden Sie in der [Dokumentation](../cloud-shell/overview.md).

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