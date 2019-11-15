---
title: Binden von Azure Database for MySQL an Ihre Azure Spring Cloud-Anwendung | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Azure MySQL an Ihre Azure Spring Cloud-Anwendung binden.
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: jeconnoc
ms.openlocfilehash: b6de5bb3b25c111d1b7775ea9570a4ae2cf45042
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607584"
---
# <a name="tutorial-bind-azure-services-to-your-azure-spring-cloud-application-azure-database-for-mysql"></a>Tutorial: Binden von Azure-Diensten an Ihre Azure Spring Cloud-Anwendung: Azure Database for MySQL

Mit Azure Spring Cloud können Sie ausgewählte Azure-Dienste automatisch an Ihre Anwendungen binden, statt Ihre Spring Boot-Anwendung manuell zu konfigurieren. In diesem Tutorial wird gezeigt, wie Sie Ihre Anwendung an Azure MySQL binden.

## <a name="prerequisites"></a>Voraussetzungen

* Eine bereitgestellte Azure Spring Cloud-Instanz
* Ein Azure Database for MySQL-Konto
* Azure-Befehlszeilenschnittstelle

Wenn Sie nicht über eine bereitgestellte Azure Spring Cloud-Instanz verfügen, folgen Sie den Schritten in diesem [Schnellstart](spring-cloud-quickstart-launch-app-portal.md), um Ihre erste Spring Cloud-App bereitzustellen.

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

