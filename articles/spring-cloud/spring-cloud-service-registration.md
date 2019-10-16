---
title: Automatisieren der Dienstregistrierung und -ermittlung
description: Erfahren Sie, wie Sie die Dienstermittlung und -registrierung mithilfe der Spring Cloud-Dienstregistrierung automatisieren.
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/05/2019
ms.author: jeconnoc
ms.openlocfilehash: 72327e116e498ce0f6881a5c585a08e56c8bf8c2
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038673"
---
# <a name="discover-and-register-your-spring-cloud-services"></a>Ermitteln und Registrieren Ihrer Spring Cloud-Dienste

Die Dienstermittlung ist eine wichtige Voraussetzung für eine auf Microservices basierende Architektur.  Das manuelle Konfigurieren der einzelnen Clients ist zeitaufwendig und kann menschliche Fehler verursachen.  Dieses Problem wird durch die Azure Spring Cloud-Dienstregistrierung gelöst.  Nach der Konfiguration steuert ein Dienstregistrierungsserver die Dienstregistrierung und -ermittlung für die Microservices Ihrer Anwendung. Der Dienstregistrierungsserver verwaltet eine Registrierung der bereitgestellten Microservices, aktiviert den clientseitigen Lastenausgleich und entkoppelt die Dienstanbieter von den Clients, ohne dafür DNS zu benötigen.

## <a name="register-your-application-using-spring-cloud-service-registry"></a>Registrieren Ihrer Anwendung mithilfe der Spring Cloud-Dienstregistrierung

Bevor Ihre Anwendung die Dienstregistrierung und -ermittlung mithilfe der Spring Cloud-Dienstregistrierung verwalten kann, müssen mehrere Abhängigkeiten in der Datei *pom.xml* der Anwendung hinzugefügt werden.

Fügen Sie zunächst im Abschnitt *repository* Ihrer Datei *pom.xml* ein Momentaufnahmenrepository hinzu.

```xml
    <repositories>
        <repository>
            <id>nexus-snapshots</id>
            <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>
```

## <a name="include-dependencies"></a>Hinzufügen von Abhängigkeiten

Als Nächstes fügen Sie in der Datei *pom.xml* Abhängigkeiten für *spring-cloud-starter-netflix-eureka-client* und *spring-cloud-starter-azure-spring-cloud-client* hinzu.

```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-spring-cloud-client</artifactId>
        <version>2.1.0-SNAPSHOT</version>
    </dependency>
```

## <a name="update-the-top-level-class"></a>Aktualisieren der Klasse der obersten Ebene

Abschließend fügen Sie der Klasse auf oberster Ebene Ihrer Anwendung eine Anmerkung hinzu.

 ```java
    package foo.bar;

    import org.springframework.boot.SpringApplication;
    import org.springframework.cloud.client.SpringCloudApplication;

    @SpringCloudApplication
    public class DemoApplication {
        public static void main(String... args) {
            SpringApplication.run(DemoApplication.class, args);
        }
    }
 ```

Der Serverendpunkt der Spring Cloud-Dienstregistrierung wird als Umgebungsvariable in Ihre Anwendung eingefügt.  Microservices können sich damit selbst beim Dienstregistrierungsserver registrieren und andere abhängige Microservices ermitteln.

Beachten Sie, dass es einige Minuten dauern kann, bis die Änderungen vom Server an alle Microservices weitergegeben wurden.
