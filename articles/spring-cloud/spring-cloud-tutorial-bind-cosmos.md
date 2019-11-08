---
title: Binden von Azure Cosmos DB an Ihre Azure Spring Cloud-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Cosmos DB an Ihre Azure Spring Cloud-Anwendung binden.
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: 76a2b9f305f041a19b8d7ace8234a804825f6a0e
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607129"
---
# <a name="tutorial-bind-an-azure-cosmos-db-to-your-azure-spring-cloud-application"></a>Tutorial: Binden von Azure Cosmos DB an Ihre Azure Spring Cloud-Anwendung

Azure Spring Cloud ermöglicht Ihnen, ausgewählte Azure-Dienste automatisch an Ihre Anwendungen zu binden, anstatt Ihre Spring Boot-Anwendung manuell zu konfigurieren. In diesem Artikel wird gezeigt, wie Sie Ihre Anwendung an Azure Cosmos DB binden.

Voraussetzungen:
* Eine bereitgestellte Azure Spring Cloud-Instanz.  Befolgen Sie unseren [Schnellstart](spring-cloud-quickstart-launch-app-cli.md), um zu beginnen.
* Ein Azure Cosmos DB-Konto mit mindestens der Berechtigungsebene „Mitwirkender“.

## <a name="bind-azure-cosmos-db"></a>Binden von Azure Cosmos DB

Azure Cosmos DB verfügt über fünf verschiedene API-Typen, die Bindung unterstützen:

1. Erstellen Sie eine Azure Cosmos DB-Datenbank. Weitere Informationen zum Erstellen der Datenbank finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/cosmos-db/create-cosmosdb-resources-portal). Notieren Sie den Namen der Datenbank. Unsere Datenbank trägt den Namen `testdb`.

1. Fügen Sie eine der folgenden Abhängigkeiten in der Datei `pom.xml` der Spring Cloud-Anwendung gemäß Ihrem API-Typ hinzu.
    
    #### <a name="api-type-core-sql"></a>API-Typ: Core (SQL)

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
        <version>2.1.6</version>
    </dependency>
    ```
    
    #### <a name="api-type-mongodb"></a>API-Typ: MongoDB

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-mongodb</artifactId>
    </dependency>
    ```

    #### <a name="api-type-cassandra"></a>API-Typ: Cassandra

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-cassandra</artifactId>
    </dependency>
    ```

    #### <a name="api-type-gremlin-graph"></a>API-Typ: Gremlin (Graph)

    ```xml
    <dependency>
        <groupId>com.microsoft.spring.data.gremlin</groupId>
        <artifactId>spring-data-gremlin</artifactId>
        <version>2.1.7</version>
    </dependency>
    ```

    #### <a name="api-type-azure-table"></a>API-Typ: Azure Table

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-storage-spring-boot-starter</artifactId>
        <version>2.0.5</version>
    </dependency>
    ```

1. Aktualisieren Sie die aktuelle Bereitstellung mit `az spring-cloud app update`, oder erstellen Sie eine neue Bereitstellung für diese Änderung mithilfe von `az spring-cloud app deployment create`.  Mit diesen Befehlen wird die Anwendung entweder aktualisiert oder mit der neuen Abhängigkeit erstellt.

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Spring Cloud-Dienstseite. Dabei handelt es sich um dieselbe Anwendung, die Sie im vorherigen Schritt aktualisiert oder bereitgestellt haben. Suchen Sie das **Anwendungsdashboard**, und wählen Sie die Anwendung aus, die an Cosmos DB gebunden werden soll. Wählen Sie dann `Service binding` aus, und klicken Sie auf die Schaltfläche `Create service binding`. Füllen Sie das Formular aus, und wählen Sie den **Bindungstyp** „`Azure Cosmos DB`“, den API-Typ, den Datenbanknamen und das Azure Cosmos DB-Konto aus.

    > [!NOTE]
    > Verwenden Sie einen Schlüsselraum für den Datenbanknamen, wenn Sie mit Cassandra arbeiten.

1. Starten Sie die Anwendung neu, indem Sie auf der Anwendungsseite die Schaltfläche **Neu starten** auswählen.

1. Um sicherzustellen, dass der Dienst ordnungsgemäß gebunden ist, wählen Sie den Bindungsnamen aus, und überprüfen Sie dessen Details. Das Feld `property` sollte in etwa wie folgt aussehen:

    ```
    azure.cosmosdb.uri=https:/<some account>.documents.azure.com:443
    azure.cosmosdb.key=abc******
    azure.cosmosdb.database=testdb
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie Ihre Azure Spring Cloud-Anwendung an CosmosDB binden können.  Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Anwendung an Azure Redis Cache binden.

> [!div class="nextstepaction"]
> [Binden einer Azure Spring Cloud-Anwendung an Azure Redis Cache](spring-cloud-tutorial-bind-redis.md).
