---
title: Binden von Azure Database for MySQL an Ihre Azure Spring Cloud-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Azure MySQL an Ihre Azure Spring Cloud-Anwendung binden.
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: e2add139f5cfd8299ec809793dd822b051d0f542
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038349"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-database-for-mysql"></a>Tutorial: Binden von Azure-Diensten an Ihre Azure Spring Cloud-Anwendung: Azure Database for MySQL

Mit Azure Spring Cloud können Sie ausgewählte Azure-Dienste automatisch an Ihre Anwendungen binden, statt Ihre Spring Boot-Anwendung manuell zu konfigurieren. In diesem Tutorial wird gezeigt, wie Sie Ihre Anwendung an Azure MySQL binden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine bereitgestellte Azure Spring Cloud-Instanz
* Ein Azure Database for MySQL-Konto
* Azure-Befehlszeilenschnittstelle

Führen Sie ggf. den folgenden Befehl aus, um die Azure Spring Cloud-Erweiterung für die Azure CLI zu installieren:

```azurecli
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

>[!TIP]
> Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können.  Sie verfügt über allgemeine vorinstallierte Azure-Tools, u. a. die aktuellen Versionen von Git, JDK, Maven und der Azure-Befehlszeilenschnittstelle. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie [Azure Cloud Shell](https://shell.azure.com) über shell.azure.com.  Weitere Informationen zu Azure Cloud Shell finden Sie in der [Dokumentation](../cloud-shell/overview.md).

## <a name="bind-azure-database-for-mysql"></a>Binden von Azure Database for MySQL

1. Notieren Sie sich den Benutzernamen und das Kennwort des Administrators für Ihr Azure MySQL-Konto. Stellen Sie eine Verbindung mit dem Server her, und erstellen Sie auf der Grundlage eines MySQL-Clients eine Datenbank mit dem Namen `testdb`. Erstellen Sie ein neues Nicht-Administratorkonto.

1. Fügen Sie der Datei `pom.xml` Ihres Projekts die folgende Abhängigkeit hinzu:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
1. Entfernen Sie ggf. die `spring.datasource.*`-Eigenschaften in der Datei `application.properties`.

1. Aktualisieren Sie die aktuelle Bereitstellung mit `az spring-cloud app update`, oder erstellen Sie eine neue Bereitstellung für diese Änderung mithilfe von `az spring-cloud app deployment create`.  Mit diesen Befehlen wird die Anwendung entweder aktualisiert oder mit der neuen Abhängigkeit erstellt.

1. Navigieren Sie im Azure-Portal zur Seite Ihres Azure Spring Cloud-Diensts. Suchen Sie das **Anwendungsdashboard**, und wählen Sie die Anwendung aus, die an Azure MySQL gebunden werden soll.  Dabei handelt es sich um dieselbe Anwendung, die Sie im vorherigen Schritt aktualisiert oder bereitgestellt haben. Wählen Sie `Service binding` und dann die Schaltfläche `Create service binding` aus. Füllen Sie das Formular aus. Wählen Sie den **Bindungstyp** `Azure MySQL`, den gleichen Datenbanknamen, den Sie auch zuvor verwendet haben, sowie den Benutzernamen und das Kennwort, die Sie im ersten Schritt notiert haben, aus.

1. Starten Sie die App neu. Die Bindung sollte jetzt funktionieren.

1. Um sicherzustellen, dass die Dienstbindung richtig ist, wählen Sie den Bindungsnamen aus, und überprüfen Sie dessen Details. Das Feld `property` sollte wie folgt aussehen:
    ```
    spring.datasource.url=jdbc:mysql://some-server.mysql.database.azure.com:3306/testdb?useSSL=true&requireSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC
    spring.datasource.username=admin@some-server
    spring.datasource.password=abc******
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie Ihre Azure Spring Cloud-Anwendung an eine MySQL-Datenbank binden können.  Weitere Informationen zum Verwalten des Azure Spring Cloud-Diensts finden Sie im nächsten Tutorial zu Dienstermittlung und -registrierung.

> [!div class="nextstepaction"]
> [Ermitteln und Registrieren von Spring Cloud-Diensten](spring-cloud-service-registration.md)

