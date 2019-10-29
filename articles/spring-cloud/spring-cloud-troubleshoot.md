---
title: Leitfaden zur Problembehandlung für Azure Spring Cloud | Microsoft-Dokumentation
description: Leitfaden zur Problembehandlung für Azure Spring Cloud
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: ee51841046962a6896b4c16e651f85ff761a69fc
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72592484"
---
# <a name="troubleshooting-guide-for-common-problems"></a>Leitfaden zur Problembehandlung für häufige Probleme

In diesem Artikel werden einige häufige Probleme und Schritte zur Problembehandlung für Entwickler beschrieben, die in Azure Spring Cloud arbeiten. Außerdem empfehlen wir Ihnen, den [Artikel zu den häufig gestellten Fragen](spring-cloud-faq.md) zu lesen.

## <a name="availability-performance-and-application-issues"></a>Verfügbarkeits-, Leistungs- und Anwendungsprobleme

### <a name="my-application-cannot-start-for-example-the-endpoint-cannot-be-connected-or-returns-502-after-few-retries"></a>Meine Anwendung kann nicht gestartet werden (z. B. kann für den Endpunkt keine Verbindung hergestellt werden, oder nach einigen Wiederholungsversuchen wird 502 zurückgegeben)

Exportieren Sie die Protokolle nach _Azure Log Analytics_. Die Tabelle für Spring-Anwendungsprotokolle heißt `AppPlatformLogsforSpring`. Weitere Informationen finden Sie unter [Analysieren von Protokollen und Metriken mit Diagnoseeinstellungen](diagnostic-services.md).

Wenn Sie in Ihren Protokollen die folgenden Fehler finden, liegt eines dieser beiden Probleme vor:

`org.springframework.context.ApplicationContextException: Unable to start web server`

* Eins der Beans oder eine seiner Abhängigkeiten fehlt.
* Eine der Bean-Eigenschaften fehlt oder ist ungültig. In diesem Fall wird wahrscheinlich `java.lang.IllegalArgumentException` angezeigt.

Dienstbindungen können auch dazu führen, dass beim Starten von Anwendungen Fehler auftreten. Verwenden Sie Schlüsselwörter im Zusammenhang mit den gebundenen Diensten, um die Protokolle abzufragen.  Angenommen, Ihre Anwendung verfügt über eine Bindung an eine MySQL-Instanz, für die die lokale Systemzeit festgelegt ist. Wenn die Anwendung nicht gestartet werden kann, enthält das Protokoll ggf. den folgenden Fehler:

`java.sql.SQLException: The server time zone value 'Coordinated Universal Time' is unrecognized or represents more than one time zone.`

Navigieren Sie zum Beheben dieses Fehlers zum `server parameters`-Element Ihrer MySql-Instanz, und ändern Sie `time_zone` von `SYSTEM` in `+0:00`.


### <a name="my-application-crashes-or-throws-an-unexpected-error"></a>Meine Anwendung stürzt ab oder löst einen unerwarteten Fehler aus

Falls beim Debuggen die Anwendung abstürzt, sollten Sie zunächst den Ausführungsstatus und den Ermittlungsstatus der Anwendung überprüfen. Navigieren Sie im Azure-Portal zu _App-Verwaltung_, um sicherzustellen, dass für alle Anwendungen _Wird ausgeführt_ und _UP_ (AKTIV) angegeben ist.

* Falls der Status _Wird ausgeführt_ lautet, aber als Ermittlungsstatus nicht _UP_ (AKTIV) angezeigt wird, helfen Ihnen die Informationen unter [Meine Anwendung kann nicht registriert werden](#my-application-cannot-be-registered) weiter.

* Wenn der Ermittlungsstatus _UP_ (AKTIV) lautet, können Sie zu _Metriken_ navigieren, um die Integrität der Anwendung zu überprüfen. Untersuchen Sie die folgenden Metriken:


  - `TomcatErrorCount` (_tomcat.global.error_): Alle Spring-Anwendungsausnahmen werden hier gezählt. Wenn dieser Wert hoch ist, wechseln Sie zu _Azure Log Analytics_, um Ihre Anwendungsprotokolle zu überprüfen.

  - `AppMemoryMax` (_jvm.memory.max_): Für die Anwendung steht die maximale Menge an Arbeitsspeicher zur Verfügung. Es kann sein, dass dieser Wert nicht definiert ist oder sich im Laufe der Zeit ändert, falls dies festgelegt ist. Die Menge des verwendeten und zugesicherten Arbeitsspeichers ist immer kleiner oder gleich dem maximalen Wert, wenn er definiert ist. Eine Speicherzuordnung kann jedoch mit `OutOfMemoryError` fehlschlagen, wenn sie versucht, den verwendeten Arbeitsspeicher so zu vergrößern, dass „verwendet“ > „zugesichert“ ist, selbst wenn „verwendet“ <= „Max“ weiterhin zutrifft („true“). Versuchen Sie in einer solchen Situation, die maximale Heapgröße über den Parameter `-Xmx` zu erhöhen.

  - `AppMemoryUsed` (_jvm.memory.used_): Die derzeit verwendete Arbeitsspeichermenge in Byte, die von der Anwendung verwendet wird. Bei einer normalen Load-Java-Anwendung ergibt diese Metrikreihe ein Sägezahnmuster, bei dem die Speicherauslastung in kleinen Schritten beständig zunimmt und abnimmt und plötzlich massiv abfällt, wobei sich das Muster wiederholt. Dies liegt an der Garbage Collection innerhalb eines virtuellen Java-Computers, wobei Sammlungsaktionen bei den „Sägezähnen“ einen Abfall darstellen.
    Diese Metrik ist wichtig für die Identifizierung von Arbeitsspeicherproblemen, z. B.: * Arbeitsspeicherexplosion direkt am Anfang * Surge-Arbeitsspeicherzuordnung für einen bestimmten Logikpfad * Allmähliche Speicherverluste

  Weitere Informationen finden Sie unter [Metriken](spring-cloud-concept-metrics.md).

Besuchen Sie [diesen Artikel zu den ersten Schritten](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal), um in _Azure Log Analytics_ einzusteigen.

### <a name="my-application-experiences-high-cpu-usage-or-high-memory-usage"></a>Meine Anwendung hat eine hohe CPU- oder Speicherauslastung.

Wenn Ihre Anwendung über eine hohe CPU-/Arbeitsspeicherauslastung verfügt, trifft einer der beiden folgenden Fälle zu:
* Alle App-Instanzen haben eine hohe CPU-/Arbeitsspeicherauslastung
* Einige App-Instanzen haben eine hohe CPU-/Arbeitsspeicherauslastung

Gehen Sie wie folgt vor, um herauszufinden, welche Situation zutrifft:

1. Wechseln Sie zu _Metriken_, und wählen Sie `Service CPU Usage Percentage` oder `Service Memory Used` aus.
2. Fügen Sie einen `App=`-Filter hinzu, um anzugeben, welche Anwendung Sie überwachen möchten, und
3. Teilen Sie die Metriken nach `Instance` auf.

Falls alle Instanzen eine hohe CPU-/Arbeitsspeicherauslastung aufweisen, müssen Sie entweder die Anwendung horizontal hochskalieren oder die CPU-/Arbeitsspeicherleistung zentral hochskalieren. Weitere Informationen finden Sie unter [Skalieren von Anwendungen](spring-cloud-tutorial-scale-manual.md).

Wenn einige Instanzen eine hohe CPU/Arbeitsspeicherauslastung aufweisen, sollten Sie den Instanzstatus und den zugehörigen Ermittlungsstatus überprüfen.

Weitere Informationen finden Sie unter [Metriken](spring-cloud-concept-metrics.md).

Wenn alle Instanzen aktiv sind und ausgeführt werden, wechseln Sie zu _Azure Log Analytics_, um Ihre Anwendungsprotokolle abzufragen. Überprüfen Sie außerdem Ihre Codelogik, um festzustellen, ob diese sich ggf. auf die Skalierungspartitionierung auswirken. Weitere Informationen finden Sie unter [Analysieren von Protokollen und Metriken mit Diagnoseeinstellungen](diagnostic-services.md).

Besuchen Sie [diesen Artikel zu den ersten Schritten](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal), um in _Azure Log Analytics_ einzusteigen. Fragen Sie die Protokolle mit der [Kusto-Abfragesprache](https://docs.microsoft.com/azure/kusto/query/) ab.

### <a name="checklist-before-onboarding-your-spring-application-to-azure-spring-cloud"></a>Prüfliste vor dem Onboarding Ihrer Spring-Anwendung in Azure Spring Cloud

* Die Anwendung kann lokal mit der angegebenen Java-Laufzeitversion ausgeführt werden.
* Die Umgebungskonfiguration (CPU/RAM/Instanzen) erfüllt die vom Anwendungsanbieter festgelegte Mindestanforderung.
* Die Konfigurationselemente besitzen die erwarteten Werte. Weitere Informationen finden Sie in [diesem Artikel zum Konfigurationsserver](spring-cloud-tutorial-config-server.md).
* Die Umgebungsvariablen verfügen über die erwarteten Werte.
* Die JVM-Parameter verfügen über die erwarteten Werte.
* Wir empfehlen Ihnen, den eingebetteten _Konfigurationsserver_ und die _Registrierung des Spring-Diensts_ zu deaktivieren bzw. aus dem Anwendungspaket zu entfernen.
* Wenn Azure-Ressourcen mittels _Dienstbindung_ gebunden werden sollen, stellen Sie sicher, dass die Zielressourcen in Betrieb sind und ausgeführt werden.

## <a name="configuration-and-management"></a>Konfiguration und Verwaltung

### <a name="i-encountered-a-problem-creating-an-azure-spring-cloud-service-instance"></a>Bei mir ist ein Problem beim Erstellen einer Azure Spring Cloud-Dienstinstanz aufgetreten

Wenn Sie versuchen, über das Azure-Portal eine Instanz eines _Azure Spring Cloud_-Diensts bereitzustellen, wird die Überprüfung für Sie durchgeführt.

Überprüfen Sie aber Folgendes, falls Sie versuchen, die Instanz des _Azure Spring Cloud_-Diensts über die [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) oder die [Resource Manager-Vorlage](https://docs.microsoft.com/azure/azure-resource-manager/) bereitzustellen:

* Das Abonnement ist aktiv.
* Der Standort wird von _Azure Spring Cloud_ [unterstützt](spring-cloud-faq.md).
* Die Ressourcengruppe für die Instanz wurde bereits erstellt.
* Der Ressourcenname entspricht der Benennungsregel. (Nur Kleinbuchstaben, Zahlen und Bindestriche sind zulässig. Das erste Zeichen muss ein Buchstabe sein. Das letzte Zeichen muss ein Buchstabe oder eine Zahl sein. Der Wert muss zwischen 2 und 32 Zeichen umfassen.)

Besuchen Sie https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates, um die Vorlagensyntax zu überprüfen, wenn Sie versuchen, die Instanz des _Azure Spring Cloud_-Diensts über die Resource Manager-Vorlage bereitzustellen.

Der Name der Instanz des _Azure Spring Cloud_-Diensts wird verwendet, um den Namen der Unterdomäne unter `azureapps.io` anzufordern. Die Bereitstellung ist nicht erfolgreich, wenn für den Namen ein Konflikt mit einem bereits vorhandenen Namen besteht. Weitere Informationen finden Sie in den Aktivitätsprotokollen.

### <a name="i-cannot-deploy-a-jar-package"></a>Ich kann ein JAR-Paket nicht bereitstellen

Sie können das JAR-/Quellpaket nicht über das Portal oder die Resource Manager-Vorlage hochladen.

Wenn Sie Ihr Anwendungspaket über die [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) bereitstellen, wird in regelmäßigen Abständen der Bereitstellungsfortschritt abgerufen und am Ende das Bereitstellungsergebnis angezeigt.

Sollte der Abruf unterbrochen werden, können Sie mithilfe des folgenden Befehls die Bereitstellungsprotokolle abrufen:

`az spring-cloud app show-deploy-log -n <app-name>`

Stellen Sie sicher, dass Ihre Anwendung im richtigen [ausführbaren JAR-Format](https://docs.spring.io/spring-boot/docs/current/reference/html/executable-jar.html) gepackt ist. Wenn dies nicht der Fall ist, wird ein ähnlicher Fehler wie folgt angezeigt:

`Error: Invalid or corrupt jarfile /jar/38bc8ea1-a6bb-4736-8e93-e8f3b52c8714`

### <a name="i-cannot-deploy-a-source-package"></a>Ich kann ein Quellpaket nicht bereitstellen

Sie können das JAR-/Quellpaket nicht über das Portal oder die Resource Manager-Vorlage hochladen.

Wenn Sie Ihr Anwendungspaket über die [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) bereitstellen, ruft es in regelmäßigen Abständen den Bereitstellungsfortschritt ab und zeigt am Ende das Bereitstellungsergebnis an.

Sollte der Abruf unterbrochen werden, können Sie mithilfe des folgenden Befehls die Build- und Bereitstellungsprotokolle abrufen:

`az spring-cloud app show-deploy-log -n <app-name>`

Beachten Sie aber, dass eine Instanz des _Azure Spring Cloud_-Diensts nur jeweils einen Buildauftrag pro Quellpaket auslösen kann. Weitere Informationen finden Sie unter [Bereitstellen von Anwendungen](spring-cloud-quickstart-launch-app-portal.md) und im [Leitfaden zur Stagingumgebung](spring-cloud-howto-staging-environment.md).

### <a name="my-application-cannot-be-registered"></a>Meine Anwendung kann nicht registriert werden

Dies tritt normalerweise auf, wenn in Ihrer POM-Datei die erforderlichen Abhängigkeiten bzw. die Dienstermittlung nicht richtig konfiguriert sind. Nach der Konfiguration wird der integrierte Serverendpunkt der Dienstregistrierung als Umgebungsvariable für Ihre Anwendung eingefügt. Die Anwendungen führen dann die Registrierung für den Dienstregistrierungsserver durch und können andere abhängige Microservices ermitteln.

Die Wartezeit, bis eine neu registrierte Instanz mit dem Empfang von Datenverkehr beginnen kann, beträgt mindestens zwei Minuten.

Stellen Sie beim Migrieren einer vorhandenen Spring Cloud-basierten Lösung zu Azure sicher, dass Ihre Ad-hoc-Instanzen der _Dienstregistrierung_ und des _Konfigurationsservers_ entfernt (oder deaktiviert) werden. So können Sie einen Konflikt mit den von _Azure Spring Cloud_ bereitgestellten verwalteten Instanzen vermeiden.

Sie können die Clientprotokolle der _Dienstregistrierung_ auch in _Azure Log Analytics_ überprüfen. Weitere Informationen finden Sie unter [Analysieren von Protokollen und Metriken mit Diagnoseeinstellungen](diagnostic-services.md).

Besuchen Sie [diesen Artikel zu den ersten Schritten](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal), um in _Azure Log Analytics_ einzusteigen. Fragen Sie die Protokolle mit der [Kusto-Abfragesprache](https://docs.microsoft.com/azure/kusto/query/) ab.

### <a name="i-want-to-inspect-my-applications-environment-variables"></a>Ich möchte die Umgebungsvariablen meiner Anwendung überprüfen.

Umgebungsvariablen informieren das Azure Spring Cloud-Framework und stellen sicher, dass Azure versteht, wo und wie die Dienste konfiguriert werden, aus denen Ihre Anwendung besteht.  Bei der Behebung potenzieller Probleme muss zunächst sichergestellt werden, dass Ihre Umgebungsvariablen richtig sind.  Sie können die Umgebungsvariablen mithilfe des Endpunkts für den Spring Boot-Aktor überprüfen.  

> [!WARNING]
> Mit dieser Prozedur werden die Umgebungsvariablen unter Verwendung Ihres Testendpunkts verfügbar gemacht.  Setzen Sie den Vorgang nicht fort, wenn der Testendpunkt öffentlich zugänglich ist oder Sie Ihrer Anwendung einen Domänennamen zugewiesen haben.

1. Navigieren Sie zur folgenden URL: `https://<your application test endpoint>/actuator/health`.  
    - Eine Antwort ähnlich wie `{"status":"UP"}` gibt an, dass der Endpunkt aktiviert wurde.
    - Ist die Antwort negativ, nehmen Sie die folgende Abhängigkeit in die Datei `POM.xml` auf:

        ```xml
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-actuator</artifactId>
            </dependency>
        ```

1. Wenn Sie den Endpunkt für den Spring Boot-Aktor aktiviert haben, navigieren Sie im Azure-Portal zur Konfigurationsseite Ihrer Anwendung.  Fügen Sie eine Umgebungsvariable mit dem Namen `MANAGEMENT_ENDPOINTS_WEB_EXPOSURE_INCLUDE' and the value `* hinzu. 

1. Starten Sie Ihre Anwendung neu.

1. Navigieren Sie zu `https://<the test endpoint of your app>/actuator/env`, und überprüfen Sie die Antwort.  Diese sollte wie folgt aussehen:

    ```json
    {
        "activeProfiles": [],
        "propertySources": {,
            "name": "server.ports",
            "properties": {
                "local.server.port": {
                    "value": 1025
                }
            }
        }
    }
    ```

Suchen Sie den untergeordneten Knoten mit dem Namen `systemEnvironment`.  Dieser Knoten enthält die Umgebungsvariablen Ihrer Anwendung.

> [!IMPORTANT]
> Denken Sie daran, das Verfügbarmachen Ihrer Umgebungsvariablen rückgängig zu machen, bevor Sie die Anwendung öffentlich zugänglich machen.  Wechseln Sie zum Azure-Portal, navigieren Sie zur Konfigurationsseite Ihrer Anwendung, und löschen Sie die folgende Umgebungsvariable: `MANAGEMENT_ENDPOINTS_WEB_EXPOSURE_INCLUDE`.

### <a name="i-cannot-find-metrics-or-logs-for-my-application"></a>Ich kann keine Metriken oder Protokolle für meine Anwendung finden

Navigieren Sie zu _App-Verwaltung_, um sicherzustellen, dass für die Anwendung _Wird ausgeführt_ und _UP_ (AKTIV) festgelegt ist.

Wenn Sie Metriken von _JVM_, aber keine Metriken von _Tomcat_ sehen können, sollten Sie überprüfen, ob die Abhängigkeit `spring-boot-actuator` in Ihrem Anwendungspaket aktiviert und der Startvorgang erfolgreich ist.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Wenn Ihre Anwendungsprotokolle in einem Speicherkonto archiviert, aber nicht an _Azure Log Analytics_ gesendet werden können, überprüfen Sie, ob Sie [Ihren Arbeitsbereich ordnungsgemäß eingerichtet haben](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace). Falls Sie einen kostenlosen Tarif von _Azure Log Analytics_ verwenden, sollten Sie beachten, dass [für den kostenlosen Tarif keine SLA gilt](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_3/).