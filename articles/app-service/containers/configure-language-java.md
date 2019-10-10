---
title: Konfigurieren von Linux-Java-Apps – Azure App Service | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Java-Apps konfigurieren, die in Azure App Service unter Linux ausgeführt werden.
keywords: Azure App Service, Web-App, Linux, OSS, Java, Java EE, JEE, Javaee
services: app-service
author: bmitchell287
manager: barbkess
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/26/2019
ms.author: brendm
ms.custom: seodec18
ms.openlocfilehash: 26f9bac42ef98f1063194340a5aa20aef6fe316e
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "71972946"
---
# <a name="configure-a-linux-java-app-for-azure-app-service"></a>Konfigurieren einer Linux-Java-App für Azure App Service

Mit Azure App Service unter Linux können Java-Entwickler schnell ihre mit Tomcat oder Java Standard Edition (SE) paketierten Webanwendungen in einem vollständig verwalteten Linux-basierten Dienst erstellen, bereitstellen und skalieren. Stellen Sie Anwendungen mit Maven-Plug-Ins über die Befehlszeile oder in Editoren wie IntelliJ, Eclipse oder Visual Studio Code bereit.

Dieser Leitfaden enthält wichtige Konzepte und Anleitungen für Java-Entwickler, die in App Service einen integrierten Linux-Container verwenden. Falls Sie Azure App Service noch nicht verwendet haben, arbeiten Sie zunächst die [Java-Schnellstartanleitung](quickstart-java.md) und das [Tutorial „Java mit PostgreSQL“](tutorial-java-enterprise-postgresql-app.md) durch.

## <a name="deploying-your-app"></a>Bereitstellen Ihrer App

Sie können das [Maven-Plug-In für Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) zum Bereitstellen von JAR- und WAR-Dateien verwenden. Die Bereitstellung mit beliebten IDEs wird außerdem mit [Azure Toolkit für IntelliJ](/java/azure/intellij/azure-toolkit-for-intellij) oder [Azure Toolkit für Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse) unterstützt.

In allen anderen Fällen hängt Ihre Bereitstellungsmethode von Ihrem Archivtyp ab:

- Um WAR-Dateien in Tomcat bereitzustellen, verwenden Sie den `/api/wardeploy/`-Endpunkt, um Ihre Archivdatei mit POST zu veröffentlichen. Weitere Informationen zu dieser API finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/app-service/deploy-zip#deploy-war-file).
- Um JAR-Dateien in den Java SE-Images bereitzustellen, verwenden Sie den `/api/zipdeploy/`-Endpunkt der Kudu-Website. Weitere Informationen zu dieser API finden Sie in [dieser Dokumentation](https://docs.microsoft.com/azure/app-service/deploy-zip#rest).

Stellen Sie Ihre WAR- oder JAR-Dateien nicht mithilfe von FTP bereit. Der FTP-Tool dient zum Hochladen von Startskripts, Abhängigkeiten oder anderen Laufzeitdateien. Es ist nicht die optimale Wahl für die Bereitstellung von Web-Apps.

## <a name="logging-and-debugging-apps"></a>Protokollieren und Debuggen von Apps

Leistungsberichte, Datenverkehrsvisualisierungen und Integritätsprüfungen für die einzelnen Apps stehen über das Azure-Portal zur Verfügung. Weitere Informationen hierzu finden Sie unter [Azure App Service-Pläne – Diagnoseübersicht](../overview-diagnostics.md).

### <a name="ssh-console-access"></a>SSH-Konsolenzugriff

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

### <a name="stream-diagnostic-logs"></a>Streamen von Diagnoseprotokollen

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

Weitere Informationen finden Sie unter [Streamen von Protokollen in Cloud Shell](../troubleshoot-diagnostic-logs.md#in-cloud-shell).

### <a name="app-logging"></a>App-Protokollierung

Aktivieren Sie die [Anwendungsprotokollierung](../troubleshoot-diagnostic-logs.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#enable-application-logging-windows) über das Azure-Portal oder die [Azure-Befehlszeilenschnittstelle](/cli/azure/webapp/log#az-webapp-log-config), um App Service so zu konfigurieren, dass die Streams mit der Standardkonsolenausgabe und den Standardkonsolenfehlern Ihrer Anwendung in das lokale Dateisystem oder Azure Blob Storage geschrieben werden. Die Protokollierung in der lokalen App Service-Dateisysteminstanz wird 12 Stunden nach der Konfiguration deaktiviert. Wenn Sie eine längere Beibehaltung benötigen, konfigurieren Sie die Anwendung für die Ausgabe in einen Blob Storage-Container. Ihre Java- und Tomcat-App-Protokolle befinden sich im Verzeichnis */home/LogFiles/Application/* .

Wenn Ihre Anwendung [Logback](https://logback.qos.ch/) oder [Log4j](https://logging.apache.org/log4j) für die Ablaufverfolgung verwendet, können Sie diese Ablaufverfolgungen mithilfe der Konfigurationsanweisungen für das Protokollierungsframework unter [Untersuchen von Java-Ablaufverfolgungsprotokollen in Application Insights](/azure/application-insights/app-insights-java-trace-logs) zur Überprüfung an Azure Application Insights weiterleiten.

### <a name="troubleshooting-tools"></a>Problembehandlungstools

Die integrierten Java-Images basieren auf dem [Alpine Linux](https://alpine-linux.readthedocs.io/en/latest/getting_started.html)-Betriebssystem. Verwenden Sie den `apk`-Paket-Manager zur Installation von Tools oder Befehlen zur Problembehandlung.

### <a name="flight-recorder"></a>Flight Recorder

In allen Linux Java-Images auf App Service ist Zulu Flight Recorder installiert. So können Sie sich ganz einfach mit der JVM verbinden und eine Profileraufnahme erstellen oder einen Heapdump generieren.

#### <a name="timed-recording"></a>Zeitgesteuerte Aufzeichnung

Stellen Sie zuerst eine SSH-Verbindung mit Ihrem App Service her und führen Sie den `jcmd`-Befehl aus, um eine Liste aller laufenden Java-Prozesse zu sehen. Neben jcmd sollten Sie auch Ihre ausgeführte Java-Anwendung mit einer Prozess-ID (pid) sehen.

```shell
078990bbcd11:/home# jcmd
Picked up JAVA_TOOL_OPTIONS: -Djava.net.preferIPv4Stack=true
147 sun.tools.jcmd.JCmd
116 /home/site/wwwroot/app.jar
```

Führen Sie den folgenden Befehl aus, um eine 30-sekündige Aufnahme der JVM zu beginnen. Damit wird die JVM profiliert und eine JFR-Datei namens *jfr_example.jfr* im Stammverzeichnis erstellt. (Ersetzen Sie 116 mit der pid Ihrer Java-App.)

```shell
jcmd 116 JFR.start name=MyRecording settings=profile duration=30s filename="/home/jfr_example.jfr"
```

Während des 30-sekündigen Intervalls können Sie durch Ausführung von `jcmd 116 JFR.check` überprüfen, ob die Aufnahme wirklich stattfindet. Daraufhin werden alle Aufzeichnungen für den entsprechenden Java-Prozess gezeigt.

#### <a name="continuous-recording"></a>Fortlaufende Aufzeichnung

Sie können Zulu Flight Recorder einsetzen, um Ihre Java-Anwendung mit minimalen Auswirkungen auf die Laufzeitleistung zu profilieren ([Quelle](https://assets.azul.com/files/Zulu-Mission-Control-data-sheet-31-Mar-19.pdf)). Führen Sie dafür den folgenden Azure CLI-Befehl aus, um eine App-Einstellung namens JAVA_OPTS mit der notwendigen Konfiguration zu erschaffen. Der Inhalt der JAVA_OPTS-App-Einstellung wird an den `java`-Befehl übermittelt, wenn Ihre App gestartet wird.

```azurecli
az webapp config appsettings set -g <your_resource_group> -n <your_app_name> --settings JAVA_OPTS=-XX:StartFlightRecording=disk=true,name=continuous_recording,dumponexit=true,maxsize=1024m,maxage=1d
```

Nachdem die Aufzeichnung gestartet wurde, können Sie die aktuellen Aufzeichnungsdaten jederzeit mithilfe des `JFR.dump`-Befehls sichern.

```shell
jcmd <pid> JFR.dump name=continuous_recording filename="/home/recording1.jfr"
```

Weitere Informationen finden Sie in der [Jcmd-Befehlsreferenz](https://docs.oracle.com/javacomponents/jmc-5-5/jfr-runtime-guide/comline.htm#JFRRT190).

### <a name="analyzing-recordings"></a>Analysieren der Aufzeichnungen

Verwenden Sie [FTPS](../deploy-ftp.md) zum Herunterladen Ihrer JFR-Datei auf Ihren lokalen Computer. Laden Sie zum analysieren der JFR-Datei [Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/) herunter und installieren Sie es. Weitere Informationen zu Zulu Mission Control finden Sie in [„Azul-Dokumentation“](https://docs.azul.com/zmc/) und den [Installationsanweisungen](https://docs.microsoft.com/java/azure/jdk/java-jdk-flight-recorder-and-mission-control).

## <a name="customization-and-tuning"></a>Anpassung und Optimierung

Azure App Service für Linux unterstützt eine sofort verfügbare Optimierung und Anpassung über das Azure-Portal und die CLI. Lesen Sie die folgenden Artikel zur nicht Java-spezifischen Web-App-Konfiguration:

- [Konfigurieren von App-Einstellungen](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)
- [Einrichten einer benutzerdefinierten Domäne](../app-service-web-tutorial-custom-domain.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Aktivieren von SSL](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Hinzufügen eines CDN](../../cdn/cdn-add-to-web-app.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Konfigurieren der Kudu-Website](https://github.com/projectkudu/kudu/wiki/Configurable-settings#linux-on-app-service-settings)

### <a name="set-java-runtime-options"></a>Festlegen von Java-Runtimeoptionen

Um reservierten Arbeitsspeicher oder andere JVM-Runtimeoptionen in den Tomcat- und Java SE-Umgebungen festzulegen, erstellen Sie mit den Optionen eine [App-Einstellung](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) mit dem Namen `JAVA_OPTS`. App Service für Linux übergibt diese Einstellung beim Start als Umgebungsvariable an die Java-Runtime.

Erstellen Sie im Azure-Portal unter **Anwendungseinstellungen** für die Web-App eine neue App-Einstellung namens `JAVA_OPTS`, welche die zusätzlichen Einstellungen enthält, z.B. `-Xms512m -Xmx1204m`.

Um die App-Einstellung über das Maven-Plug-In zu konfigurieren, fügen Sie Einstellungs-/Werttags im Azure-Plug-In-Abschnitt hinzu. Im folgenden Beispiel werden bestimmte Mindest- und Höchstwerte für die Java-Heapgröße festgelegt:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Xms512m -Xmx1204m</value>
    </property>
</appSettings>
```

Entwickler, die eine einzige Anwendung mit einem Bereitstellungsslot in ihrem App Service-Plan ausführen, können die folgenden Optionen verwenden:

- B1- und S1-Instanzen: `-Xms1024m -Xmx1024m`
- B2- und S2-Instanzen: `-Xms3072m -Xmx3072m`
- B3- und S3-Instanzen: `-Xms6144m -Xmx6144m`

Überprüfen Sie beim Optimieren Ihrer Anwendungsheapeinstellungen Ihre App Service-Plandetails, und berücksichtigen Sie mehrere Anwendungs- und Bereitstellungsslotanforderungen, um die optimale Zuordnung von Arbeitsspeicher zu ermitteln.

Wenn Sie eine JAR-Anwendung bereitstellen, sollte sie den Namen *app.jar* tragen, damit das integrierte Image Ihre App ordnungsgemäß identifizieren kann. (Das Maven-Plug-In nimmt diese Umbenennung automatisch vor.) Wenn Sie Ihre JAR-Datei nicht in *app.jar* umbenennen möchten, können Sie ein Shellskript mit dem Befehl zum Ausführen Ihrer JAR-Datei hochladen. Fügen Sie anschließend den vollständigen Pfad zu diesem Skript in das Textfeld [Startdatei](app-service-linux-faq.md#built-in-images) im Abschnitt „Konfiguration“ des Portals ein. Das Startskript wird nicht aus dem Verzeichnis ausgeführt, in dem es abgelegt wurde. Verwenden Sie daher immer absolute Pfade, um auf Dateien in Ihrem Startskript zu verweisen (z. B.: `java -jar /home/myapp/myapp.jar`).

### <a name="turn-on-web-sockets"></a>Aktivieren von Websockets

Aktivieren Sie die Unterstützung für Websockets im Azure-Portal in den **Anwendungseinstellungen** für die Anwendung. Sie müssen die Anwendung neu starten, damit die Einstellung in Kraft tritt.

Aktivieren Sie die Websocketunterstützung über die Azure CLI mithilfe des folgenden Befehls:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --web-sockets-enabled true
```

Starten Sie Ihre Anwendung anschließend neu:

```azurecli-interactive
az webapp stop --name <app-name> --resource-group <resource-group-name>
az webapp start --name <app-name> --resource-group <resource-group-name>
```

### <a name="set-default-character-encoding"></a>Festlegen der Standardzeichencodierung

Erstellen Sie im Azure-Portal unter **Anwendungseinstellungen** für die Web-App eine neue App-Einstellung namens `JAVA_OPTS` mit dem Wert `-Dfile.encoding=UTF-8`.

Alternativ können Sie die App-Einstellung mit dem App Service-Maven-Plug-In konfigurieren. Fügen Sie die name- und value-Tags der Einstellung in der Plug-In-Konfiguration hinzu:

```xml
<appSettings>
    <property>
        <name>JAVA_OPTS</name>
        <value>-Dfile.encoding=UTF-8</value>
    </property>
</appSettings>
```

### <a name="adjust-startup-timeout"></a>Anpassen des Timeoutwerts für den Start

Wenn Ihre Java-Anwendung besonders groß ist, sollten Sie das Zeitlimit für den Start heraufsetzen. Erstellen Sie zu diesem Zweck eine Anwendungseinstellung `WEBSITES_CONTAINER_START_TIME_LIMIT`, und legen Sie sie auf die Anzahl Sekunden fest, die App Service vor dem Timeout warten soll. Der Maximalwert beträgt `1800` Sekunden.

### <a name="pre-compile-jsp-files"></a>Vorkompilieren von JSP-Dateien

Zur Verbesserung der Leistung von Tomcat-Anwendungen können Sie die JSP-Dateien kompilieren, bevor sie in App Service bereitgestellt werden. Sie können das von Apache Sling bereitgestellte [Maven-Plug-In](https://sling.apache.org/components/jspc-maven-plugin/plugin-info.html) oder diese [Ant-Builddatei](https://tomcat.apache.org/tomcat-9.0-doc/jasper-howto.html#Web_Application_Compilation) verwenden.

## <a name="secure-applications"></a>Sichere Anwendungen

Für App Service für Linux ausgeführte Java-Anwendungen gelten dieselben [bewährten Sicherheitsmethoden](/azure/security/security-paas-applications-using-app-services) wie für andere Anwendungen.

### <a name="authenticate-users-easy-auth"></a>Authentifizieren von Benutzern (einfache Authentifizierung)

Richten Sie die App-Authentifizierung im Azure-Portal über die Option **Authentifizierung und Autorisierung** ein. Von dort aus können Sie die Authentifizierung über Azure Active Directory oder soziale Netzwerke wie Facebook, Google oder GitHub aktivieren. Die Konfiguration des Azure-Portals funktioniert nur, wenn Sie einen einzelnen Authentifizierungsanbieter konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren Ihrer App Service-App zur Verwendung der Azure Active Directory-Anmeldung](../configure-authentication-provider-aad.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) und in den entsprechenden Artikeln für andere Identitätsanbieter. Wenn Sie mehrere Anmeldungsanbieter aktivieren müssen, befolgen Sie die Anweisungen im Artikel [Anpassen der Authentifizierung und Autorisierung in Azure App Service](../app-service-authentication-how-to.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).

#### <a name="tomcat-and-wildfly"></a>Tomcat und WildFly

Ihre Tomcat- oder WildFly-Anwendung kann direkt aus dem Servlet auf die Ansprüche des Benutzers zugreifen, indem sie das Principal-Objekt in ein Map-Objekt umwandelt. Das Map-Objekt ordnet jeden Anspruchstyp einer Sammlung der Ansprüche für diesen Typ zu. Im folgenden Code ist `request` eine Instanz von `HttpServletRequest`.

```java
Map<String, Collection<String>> map = (Map<String, Collection<String>>) request.getUserPrincipal();
```

Nun können Sie das `Map`-Objekt auf bestimmte Ansprüche überprüfen. Der folgende Codeausschnitt durchläuft beispielsweise alle Anspruchstypen und gibt den Inhalt der einzelnen Sammlungen aus.

```java
for (Object key : map.keySet()) {
        Object value = map.get(key);
        if (value != null && value instanceof Collection {
            Collection claims = (Collection) value;
            for (Object claim : claims) {
                System.out.println(claims);
            }
        }
    }
```

Verwenden Sie den `/.auth/ext/logout`-Pfad, um Benutzer abzumelden. Lesen Sie die Dokumentation unter [Erweiterte Verwendung der Authentifizierung und Autorisierung in Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-authentication-how-to), um andere Aktionen auszuführen. Es gibt auch offizielle Dokumentation zur Tomcat [HttpServletRequest-Schnittstelle](https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/http/HttpServletRequest.html) und ihren Methoden. Die folgenden Servletmethoden werden auch basierend auf Ihrer App Service-Konfiguration aktualisiert:

```java
public boolean isSecure()
public String getRemoteAddr()
public String getRemoteHost()
public String getScheme()
public int getServerPort()
```

Um dieses Feature zu deaktivieren, erstellen Sie eine Anwendungseinstellung mit dem Namen `WEBSITE_AUTH_SKIP_PRINCIPAL` und dem Wert `1`. Um alle Servletfilter zu deaktivieren, die von App Service hinzugefügt wurden, erstellen Sie eine Einstellung namens `WEBSITE_SKIP_FILTERS` mit dem Wert `1`.

#### <a name="spring-boot"></a>Spring Boot

Spring Boot-Entwickler können die [Azure Active Directory-Startoption für Spring Boot](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-active-directory?view=azure-java-stable) verwenden, um Anwendungen mithilfe von vertrauten Spring Security-Anmerkungen und -APIs absichern. Achten Sie darauf, dass Sie die maximale Headergröße in Ihrer *application.properties*-Datei erhöhen. Wir empfehlen den Wert `16384`.

### <a name="configure-tlsssl"></a>Konfigurieren von TLS/SSL

Befolgen Sie die Anweisungen unter [Tutorial: Binden eines vorhandenen benutzerdefinierten SSL-Zertifikats an Azure-Web-Apps](../app-service-web-tutorial-custom-ssl.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), um ein vorhandenes SSL-Zertifikat hochzuladen und an den Domänennamen Ihrer Anwendung zu binden. Standardmäßig lässt Ihre Anwendung dennoch HTTP-Verbindungen zu. Befolgen Sie die spezifischen Schritte im Tutorial, um SSL und TLS zu erzwingen.

### <a name="use-keyvault-references"></a>Verwenden von KeyVault-Verweisen

[Azure KeyVault](../../key-vault/key-vault-overview.md) bietet eine zentrale Verwaltung von Geheimnissen mit Zugriffsrichtlinien und Überprüfungsverlauf. Sie können in KeyVault Geheimnisse (wie Kennwörter oder Verbindungszeichenfolgen) speichern und über Umgebungsvariablen auf diese Geheimnisse in Ihrer Anwendung zugreifen.

Befolgen Sie zunächst die Anweisungen zum [Gewähren des Zugriffs auf KeyVault für Ihre App](../app-service-key-vault-references.md#granting-your-app-access-to-key-vault) und zum [Erstellen eines KeyVault-Verweises auf Ihr Geheimnis in einer Anwendungseinstellung](../app-service-key-vault-references.md#reference-syntax). Sie können überprüfen, ob der Verweis auf das Geheimnis aufgelöst wird, indem Sie die Umgebungsvariable ausgeben, während Sie remote auf das App Service-Terminal zugreifen.

Um diese Geheimnisse in Ihre Spring- oder Tomcat-Konfigurationsdatei einzufügen, verwenden Sie die Syntax der Umgebungsvariablen (`${MY_ENV_VAR}`). Informationen zu Spring-Konfigurationsdateien finden Sie in dieser Dokumentation zu [externalisierten Konfigurationen](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

## <a name="configure-apm-platforms"></a>Konfigurieren von APM-Plattformen

In diesem Abschnitt wird veranschaulicht, wie Sie Java-Anwendungen, die in Azure App Service für Linux bereitgestellt werden, mit den Plattformen NewRelic und AppDynamics für die Überwachung der Anwendungsleistung (Application Performance Monitoring, APM) verbinden.

### <a name="configure-new-relic"></a>Konfigurieren von NewRelic

1. Erstellen Sie ein NewRelic-Konto unter [NewRelic.com](https://newrelic.com/signup).
2. Laden Sie den Java-Agent von NewRelic herunter. Der Dateiname hat etwa das Format *newrelic-java-x.x.x.zip*.
3. Kopieren Sie Ihren Lizenzschlüssel. Sie benötigen ihn später zum Konfigurieren des Agents.
4. [Stellen Sie eine SSH-Verbindung mit Ihrer App Service-Instanz her](app-service-linux-ssh-support.md), und erstellen Sie das neue Verzeichnis */home/site/wwwroot/apm*.
5. Laden Sie die entpackten Dateien für den NewRelic-Java-Agent in ein Verzeichnis unter */home/site/wwwroot/apm* herunter. Die Dateien für Ihren Agenten sollten Sie unter */home/site/wwwroot/apm/newrelic* finden.
6. Ändern Sie die YAML-Datei unter */home/site/wwwroot/apm/newrelic/newrelic.yml*, und ersetzen Sie den Platzhalter-Lizenzwert durch Ihren eigenen Lizenzschlüssel.
7. Navigieren Sie im Azure-Portal zu Ihrer Anwendung in App Service, und erstellen Sie eine neue Anwendungseinstellung.
    - Wenn für Ihre App **Java SE** verwendet wird, sollten Sie die Umgebungsvariable `JAVA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar` erstellen.
    - Wenn **Tomcat** verwendet wird, sollten Sie die Umgebungsvariable `CATALINA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/newrelic/newrelic.jar` erstellen.
    - Wenn Sie **WildFly** verwenden, finden Sie in der Dokumentation zu New Relic [hier](https://docs.newrelic.com/docs/agents/java-agent/additional-installation/wildfly-version-11-installation-java) eine Anleitung zur Installation des Java-Agents und der JBoss-Konfiguration.

### <a name="configure-appdynamics"></a>Konfigurieren von AppDynamics

1. Erstellen Sie unter [AppDynamics.com](https://www.appdynamics.com/community/register/) ein AppDynamics-Konto.
2. Laden Sie den Java-Agent von der AppDynamics-Website herunter. Der Dateiname hat etwa das Format *AppServerAgent-x.x.x.xxxxx.zip*.
3. [Stellen Sie eine SSH-Verbindung mit Ihrer App Service-Instanz her](app-service-linux-ssh-support.md), und erstellen Sie das neue Verzeichnis */home/site/wwwroot/apm*.
4. Laden Sie die Dateien für den Java-Agent in ein Verzeichnis unter */home/site/wwwroot/apm* herunter. Die Dateien für Ihren Agenten sollten Sie unter */home/site/wwwroot/apm/appdynamics* finden.
5. Navigieren Sie im Azure-Portal zu Ihrer Anwendung in App Service, und erstellen Sie eine neue Anwendungseinstellung.
    - Erstellen Sie bei Verwendung von **Java SE** die Umgebungsvariable `JAVA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`, wobei `<app-name>` für Ihren App Service-Namen steht.
    - Erstellen Sie bei Verwendung von **Tomcat** die Umgebungsvariable `CATALINA_OPTS` mit dem Wert `-javaagent:/home/site/wwwroot/apm/appdynamics/javaagent.jar -Dappdynamics.agent.applicationName=<app-name>`, wobei `<app-name>` für Ihren App Service-Namen steht.
    - Wenn Sie **WildFly** verwenden, finden Sie in der Dokumentation zu AppDynamics [hier](https://docs.appdynamics.com/display/PRO45/JBoss+and+Wildfly+Startup+Settings) eine Anleitung zur Installation des Java-Agents und der JBoss-Konfiguration.

>  Falls Sie bereits über eine Umgebungsvariable für `JAVA_OPTS` oder `CATALINA_OPTS` verfügen, können Sie die Option `-javaagent:/...` am Ende des aktuellen Werts anhängen.

## <a name="configure-jar-applications"></a>Konfigurieren von JAR-Anwendungen

### <a name="starting-jar-apps"></a>Starten von JAR-Apps

Standardmäßig erwartet App Service, dass Ihre JAR-Anwendung den Namen *app.jar* hat. Wenn sie diesen Namen aufweist, wird sie automatisch ausgeführt. Für Maven-Benutzer können Sie den JAR-Namen festlegen, indem Sie in den Abschnitt `<build>` der Datei *pom.xml* die Zeichenfolge `<finalName>app</finalName>` einfügen. [Sie können dies auch in Gradle durchführen](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html#org.gradle.api.tasks.bundling.Jar:archiveFileName), indem Sie die `archiveFileName`-Eigenschaft festlegen.

Falls Sie einen anderen Namen für Ihre JAR-Anwendung wählen, müssen Sie auch den [Startbefehl](app-service-linux-faq.md#built-in-images) angeben, mit dem Ihre JAR-Datei ausgeführt wird. Beispiel: `java -jar my-jar-app.jar`. Sie können den Wert für Ihren Startbefehl im Portal festlegen, indem Sie zu „Konfiguration“ > „Allgemeine Einstellungen“ navigieren oder die Anwendungseinstellung `STARTUP_COMMAND` verwenden.

### <a name="server-port"></a>Serverport

Da App Service für Linux eingehende Anforderungen an Port 80 leitet, sollte Ihre Anwendung auch an Port 80 lauschen. Sie können dies in der Konfiguration Ihrer Anwendung (z. B. Datei*application.properties*  von Spring) oder über Ihren Startbefehl (z. B. `java -jar spring-app.jar --server.port=80`) durchführen. Die folgende Dokumentation enthält Informationen zu gängigen Java-Frameworks:

- [Spring Boot](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-properties-and-configuration.html#howto-use-short-command-line-arguments)
- [SparkJava](http://sparkjava.com/documentation#embedded-web-server)
- [Micronaut](https://docs.micronaut.io/latest/guide/index.html#runningSpecificPort)
- [Play Framework](https://www.playframework.com/documentation/2.6.x/ConfiguringHttps#Configuring-HTTPS)
- [Vertx](https://vertx.io/docs/vertx-core/java/#_start_the_server_listening)
- [Quarkus](https://quarkus.io/guides/application-configuration-guide)

## <a name="data-sources"></a>Datenquellen

### <a name="tomcat"></a>Tomcat

Diese Anweisungen gelten für alle Datenbankverbindungen. Für die Platzhalter müssen der Treiberklassenname und die JAR-Datei der gewählten Datenbank angegeben werden. In der folgenden Tabelle finden Sie Klassennamen und Treiberdownloads für gängige Datenbanken:

| Datenbank   | Treiberklassenname                             | JDBC-Treiber                                                                      |
|------------|-----------------------------------------------|------------------------------------------------------------------------------------------|
| PostgreSQL | `org.postgresql.Driver`                        | [Download](https://jdbc.postgresql.org/download.html)                                    |
| MySQL      | `com.mysql.jdbc.Driver`                        | [Download](https://dev.mysql.com/downloads/connector/j/) (Wählen Sie „Platform Independent“ (Plattformunabhängig) aus.) |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | [Download](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server?view=sql-server-2017#available-downloads-of-jdbc-driver-for-sql-server)                                                           |

Wenn Sie Tomcat für die Verwendung von Java Database Connectivity (JDBC) oder der Java-Persistenz-API (JPA) konfigurieren möchten, passen Sie zuerst die Umgebungsvariable `CATALINA_OPTS` an, die von Tomcat beim Start eingelesen wird. Diese Werte können mithilfe einer App-Einstellung im [App Service-Maven-Plug-In](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) festgelegt werden:

```xml
<appSettings>
    <property>
        <name>CATALINA_OPTS</name>
        <value>"$CATALINA_OPTS -Ddbuser=${DBUSER} -Ddbpassword=${DBPASSWORD} -DconnURL=${CONNURL}"</value>
    </property>
</appSettings>
```

Alternativ können Sie die Umgebungsvariablen im Azure-Portal auf der Seite **Konfiguration** > **Anwendungseinstellungen** festlegen.

Legen Sie als Nächstes fest, ob die Datenquelle nur für eine einzelne Anwendung oder für alle im Tomcat-Servlet ausgeführten Anwendungen verfügbar sein soll.

#### <a name="application-level-data-sources"></a>Datenquellen auf Anwendungsebene

1. Erstellen Sie eine *context.xml*-Datei im Verzeichnis *META-INF/* Ihres Projekts. Erstellen Sie das Verzeichnis *META-INF/* , falls es noch nicht vorhanden ist.

2. Fügen Sie in *context.xml* ein Element vom Typ `Context` hinzu, um die Datenquelle mit einer JNDI-Adresse zu verknüpfen. Ersetzen Sie den Platzhalter `driverClassName` durch den Klassennamen Ihres Treibers aus der obigen Tabelle.

    ```xml
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ```

3. Aktualisieren Sie die Datei *web.xml* Ihrer Anwendung, sodass die Datenquelle in Ihrer Anwendung verwendet wird.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="shared-server-level-resources"></a>Gemeinsam verwendete Ressourcen auf Serverebene

1. Kopieren Sie den Inhalt von */usr/local/tomcat/conf* über SSH in */home/tomcat/conf* in Ihrer Linux-Instanz von App Service, wenn dort noch keine Konfiguration vorliegt.

    ```bash
    mkdir -p /home/tomcat
    cp -a /usr/local/tomcat/conf /home/tomcat/conf
    ```

2. Fügen Sie in *server.xml* innerhalb des Elements `<Server>` ein Kontextelement hinzu.

    ```xml
    <Server>
    ...
    <Context>
        <Resource
            name="jdbc/dbconnection"
            type="javax.sql.DataSource"
            url="${dbuser}"
            driverClassName="<insert your driver class name>"
            username="${dbpassword}"
            password="${connURL}"
        />
    </Context>
    ...
    </Server>
    ```

3. Aktualisieren Sie die Datei *web.xml* Ihrer Anwendung, sodass die Datenquelle in Ihrer Anwendung verwendet wird.

    ```xml
    <resource-env-ref>
        <resource-env-ref-name>jdbc/dbconnection</resource-env-ref-name>
        <resource-env-ref-type>javax.sql.DataSource</resource-env-ref-type>
    </resource-env-ref>
    ```

#### <a name="finalize-configuration"></a>Abschließen der Konfiguration

Platzieren Sie abschließend die JAR-Dateien des Treibers im Tomcat-Klassenpfad, und starten Sie Ihre App Service-Instanz neu.

1. Stellen Sie sicher, dass die JDBC-Treiberdateien für den Tomcat-Klassenladeprogramm verfügbar sind, indem Sie sie im Verzeichnis */home/tomcat/lib* ablegen. (Erstellen Sie dieses Verzeichnis, falls es noch nicht vorhanden ist.) Um diese Dateien in Ihre App Service-Instanz hochzuladen, gehen Sie folgendermaßen vor:

    1. Installieren Sie in der [Cloud Shell](https://shell.azure.com) die Web-App-Erweiterung:

      ```azurecli-interactive
      az extension add -–name webapp
      ```

    2. Führen Sie den folgenden CLI-Befehl aus, um einen SSH-Tunnel von Ihrem lokalen System zu App Service zu erstellen:

      ```azurecli-interactive
      az webapp remote-connection create --resource-group <resource-group-name> --name <app-name> --port <port-on-local-machine>
      ```

    3. Stellen Sie mit Ihrem SFTP-Client eine Verbindung mit dem lokalen Tunnelport her, und laden Sie die Dateien in den Ordner */home/tomcat/lib* hoch.

    Alternativ können Sie zum Hochladen des JDBC-Treibers auch einen FTP-Client verwenden. Führen Sie dazu die [Schritte zum Abrufen Ihrer FTP-Anmeldeinformationen](../deploy-configure-credentials.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) aus.

2. Wenn Sie eine Datenquelle auf Serverebene erstellt haben, starten Sie die App Service-Linux-Anwendung neu. Tomcat setzt `CATALINA_BASE` auf `/home/tomcat` zurück und verwendet die aktualisierte Konfiguration.

### <a name="spring-boot"></a>Spring Boot

Für das Verbinden mit Datenquellen in Spring Boot-Anwendungen wird empfohlen, Verbindungszeichenfolgen zu erstellen und in Ihre Datei *application.properties* einzufügen.

1. Legen Sie auf der App Service-Seite im Abschnitt „Konfiguration“ einen Namen für die Zeichenfolge fest, fügen Sie Ihre JDBC-Verbindungszeichenfolge in das Feld für den Wert ein, und legen Sie den Typ auf „Benutzerdefiniert“ fest. Sie können diese Verbindungszeichenfolge optional als Sloteinstellung festlegen.

    Diese Verbindungszeichenfolge ist für die Anwendung als die Umgebungsvariable `CUSTOMCONNSTR_<your-string-name>` verfügbar. Die Verbindungszeichenfolge, die Sie oben erstellt haben, erhält z. B. den Namen `CUSTOMCONNSTR_exampledb`.

2. Verweisen Sie in Ihrer Datei *application.properties* über den Namen der Umgebungsvariable auf diese Verbindungszeichenfolge. In unserem Beispiel würden wir Folgendes verwenden.

    ```yml
    app.datasource.url=${CUSTOMCONNSTR_exampledb}
    ```

Weitere Informationen zu diesem Thema finden Sie in der Spring Boot-Dokumentation unter [Data Access](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-data-access.html) (Datenzugriff) und [Externalized Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) (Externalisierte Konfiguration).

## <a name="configure-java-ee-wildfly"></a>Konfigurieren von Java EE (WildFly)

> [!NOTE]
> Java Enterprise Edition für App Service für Linux befindet sich derzeit in der Vorschau. Dieser Stapel wird für die produktionsrelevante Aufgaben **nicht** empfohlen.

Mit Azure App Service für Linux können Java-Entwickler Java Enterprise-Anwendungen (Java EE) in einem vollständig verwalteten Linux-basierten Dienst erstellen, bereitstellen und skalieren.  Der Java Enterprise Runtime-Umgebung liegt der Open-Source-Anwendungsserver [WildFly](https://wildfly.org/) zugrunde.

Dieser Abschnitt enthält die folgenden Unterabschnitte:

- [Skalierung mit App Service](#scale-with-app-service)
- [Anpassen der Anwendungsserverkonfiguration](#customize-application-server-configuration)
- [Installieren von Modulen und Abhängigkeiten](#install-modules-and-dependencies)
- [Konfigurieren von Datenquellen](#configure-data-sources)
- [Aktivieren von Messaginganbietern](#enable-messaging-providers)

### <a name="scale-with-app-service"></a>Skalierung mit App Service

Der in App Service unter Linux ausgeführte WildFly-Anwendungsserver wird im eigenständigen Modus und nicht in einer Domänenkonfiguration ausgeführt. Wenn Sie den App Service-Plan erweitern, wird jede WildFly-Instanz als eigenständiger Server konfiguriert.

Sie können Ihre Anwendung mit [Skalierungsregeln](../../monitoring-and-diagnostics/monitoring-autoscale-get-started.md) und durch [Erhöhen der Anzahl der Instanzen](../manage-scale-up.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) vertikal oder horizontal skalieren.

### <a name="customize-application-server-configuration"></a>Anpassen der Anwendungsserverkonfiguration

Web-App-Instanzen sind zustandslos. Daher muss jede neue Instanz beim Start konfiguriert werden, um die von der Anwendung benötigte WildFly-Konfiguration zu unterstützen.
Sie können ein Bash-Startskript schreiben, um über die WildFly-Befehlszeilenschnittstelle folgende Aktionen auszuführen:

- Einrichten von Datenquellen
- Konfigurieren von Messaginganbietern
- Hinzufügen weiterer Module und Abhängigkeiten zur WildFly-Serverkonfiguration.

Das Skript wird ausgeführt, wenn WildFly betriebsbereit ist, jedoch vor dem Start der Anwendung. Im Skript sollte die von */opt/jboss/wildfly/bin/jboss-cli.sh* aufgerufene [JBOSS CLI](https://docs.jboss.org/author/display/WFLY/Command+Line+Interface) verwendet werden, um den Anwendungsserver mit allen Konfigurationen oder Änderungen zu konfigurieren, die nach dem Start des Servers erforderlich sind.

Konfigurieren Sie WildFly nicht mit dem interaktiven Modus der CLI. Stattdessen können Sie mit dem Befehl `--file` ein Skript von Befehlen für die JBoss CLI bereitstellen, z.B.:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Verwenden Sie FTP zum Hochladen des Startskripts in Ihre App Service-Instanz in Ihrem */home*-Verzeichnis, wie z. B. */home/site/deployments/tools*. Weitere Informationen finden Sie unter [Bereitstellen der App in Azure App Service mit FTP/S](https://docs.microsoft.com/azure/app-service/deploy-ftp).

Legen Sie das Feld **Startskript** im Azure-Portal auf den Speicherort Ihres Startshellskripts fest, z.B. */home/site/deployments/tools/your-startup-script.sh*.

Geben Sie [App-Einstellungen](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) in der Anwendungskonfiguration an, um Umgebungsvariablen für die Verwendung im Skript zu übergeben. Anwendungseinstellungen enthalten Verbindungszeichenfolgen und andere Geheimnisse, die zum Konfigurieren Ihrer Anwendung außerhalb der Versionskontrolle erforderlich sind.

### <a name="install-modules-and-dependencies"></a>Installieren von Modulen und Abhängigkeiten

Um Module und die zugehörigen Abhängigkeiten über die JBoss CLI im WildFly-Klassenpfad zu installieren, müssen Sie die folgenden Dateien in Ihrem Verzeichnis erstellen. Für einige Module und Abhängigkeiten sind möglicherweise zusätzliche Konfigurationsschritte erforderlich, z.B. JNDI-Benennung oder andere API-spezifische Konfigurationen. In der folgenden Liste sind daher die Komponenten aufgeführt, die Sie in den meisten Fällen zum Konfigurieren einer Abhängigkeit mindestens benötigen.

- Ein [XML-Moduldeskriptor](https://jboss-modules.github.io/jboss-modules/manual/#descriptors). Diese XML-Datei definiert den Namen, die Attribute und die Abhängigkeiten Ihres Moduls. Diese [module.xml-Beispieldatei](https://access.redhat.com/documentation/en-us/jboss_enterprise_application_platform/6/html/administration_and_configuration_guide/example_postgresql_xa_datasource) definiert ein Postgres-Modul, die zugehörige JAR-Datei für die JDBC-Abhängigkeit und andere erforderliche Modulabhängigkeiten.
- Alle erforderlichen JAR-Dateiabhängigkeiten für Ihr Modul.
- Ein Skript mit den JBoss CLI-Befehlen zum Konfigurieren des neuen Moduls. Diese Datei enthält die über die JBoss CLI auszuführenden Befehle, um den Server zur Verwendung der Abhängigkeit zu konfigurieren. Informationen zu den Befehlen zum Hinzufügen von Modulen, Datenquellen und Messaginganbietern finden Sie in [diesem Dokument](https://access.redhat.com/documentation/red_hat_jboss_enterprise_application_platform/7.0/html-single/management_cli_guide/#how_to_cli).
- Ein Bash-Startskript zum Aufrufen der JBoss CLI und zum Ausführen des Skripts im vorherigen Schritt. Diese Datei wird ausgeführt, wenn Ihre App Service-Instanz neu gestartet wird oder wenn neue Instanzen bei einer horizontalen Skalierung bereitgestellt werden. In diesem Startskript können Sie andere Konfigurationen für Ihre Anwendung ausführen, während die JBoss-Befehle an die JBoss CLI übergeben werden. Diese Datei kann mindestens ein einzelner Befehl zum Übergeben des JBoss CLI-Befehlsskripts an die JBoss CLI sein:

```bash
/opt/jboss/wildfly/bin/jboss-cli.sh -c --file=/path/to/your/jboss_commands.cli
```

Nachdem Sie die Dateien und Inhalte für Ihr Modul definiert haben, führen Sie die folgenden Schritte aus, um das Modul dem WildFly-Anwendungsserver hinzuzufügen.

1. Verwenden Sie FTP zum Hochladen Ihrer Dateien in Ihre App Service-Instanz in Ihrem */home*-Verzeichnis, wie z. B. */home/site/deployments/tools*. Weitere Informationen finden Sie unter [Bereitstellen der App in Azure App Service mit FTP/S](../deploy-ftp.md).
2. Legen Sie im Azure-Portal auf der Seite **Konfiguration** > **Allgemeine Einstellungen** das Feld „**Startskript**“ auf den Speicherort Ihres Startshellskripts fest, z. B. */home/site/deployments/tools/startup.sh*.
3. Starten Sie Ihre App Service-Instanz neu, indem Sie im Abschnitt **Übersicht** des Portals auf die Schaltfläche **Neu starten** klicken oder die Azure-Befehlszeilenschnittstelle verwenden.

### <a name="configure-data-sources"></a>Konfigurieren von Datenquellen

Um WildFly/JBoss für den Zugriff auf eine Datenquelle zu konfigurieren, führen Sie die gleichen Schritte aus, die oben im Abschnitt „Installieren Sie Module und Abhängigkeiten“ beschrieben sind. Der folgende Abschnitt enthält spezifische Informationen über diesen Prozess für PostgreSQL, MySQL und SQL Server-Datenquellen.

In diesem Abschnitt wird vorausgesetzt, dass Sie bereits eine Anwendung, eine App Service-Instanz und eine Azure Database-Dienstinstanz besitzen. Die untenstehenden Anweisungen beziehen sich auf den Namen Ihres App Services, seine Ressourcengruppe und die Daten Ihrer Datenbankverbindung. Diese Informationen finden Sie im Azure-Portal.

Wenn Sie den gesamten Vorgang von Anfang an mit einer Beispielanwendung starten möchten, finden Sie weitere Informationen im [ Tutorial: Erstellen einer Java EE- und Postgres-Web-App in Azure ](tutorial-java-enterprise-postgresql-app.md).

In den folgenden Schritten werden die Anforderungen für die Verbindung Ihres vorhandenen App Services und einer Datenbank erklärt.

1. Laden Sie den JDBC-Treiber für [PostgreSQL](https://jdbc.postgresql.org/download.html), [MySQL](https://dev.mysql.com/downloads/connector/j/) oder [SQL Server](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server) herunter. Entpacken Sie das heruntergeladene Archiv, um die JAR-Treiberdatei zu erhalten.

2. Erstellen Sie eine Datei mit einem Namen wie *„module.xml“* und fügen Sie das folgende Markup hinzu. Ersetzen Sie den Platzhalter `<module name>` (einschließlich der spitzen Klammern) mit `org.postgres` für PostgreSQL, `com.mysql` für MySQL oder `com.microsoft` für SQL Server. Ersetzen Sie `<JDBC .jar file path>` mit dem Namen der JAR-Datei aus dem vorherigen Schritt. Dazu gehört auch der vollständige Pfad an den Ort, an den Sie die Datei in Ihrer App Service-Instanz verschieben werden. Dabei kann es sich um einen beliebigen Ort im Verzeichnis */home* handeln.

    ```xml
    <?xml version="1.0" ?>
    <module xmlns="urn:jboss:module:1.1" name="<module name>">
        <resources>
           <resource-root path="<JDBC .jar file path>" />
        </resources>
        <dependencies>
            <module name="javax.api"/>
            <module name="javax.transaction.api"/>
        </dependencies>
    </module>
    ```

3. Erstellen Sie eine Date mit einem Namen wie *„datasource-commands.cli“* und fügen Sie den folgenden Code hinzu. Ersetzen Sie `<JDBC .jar file path>` durch den Wert, den Sie im vorherigen Schritt verwendet haben. Ersetzen Sie `<module file path>` mit dem Dateinamen und dem App Service-Pfad aus dem vorherigen Schritt, z. B. */home/module.xml*.

    **PostgreSQL**

    ```console
    module add --name=org.postgres --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=postgres:add(driver-name=postgres,driver-module-name=org.postgres,driver-class-name=org.postgresql.Driver,driver-xa-datasource-class-name=org.postgresql.xa.PGXADataSource)

    data-source add --name=postgresDS --driver-name=postgres --jndi-name=java:jboss/datasources/postgresDS --connection-url=$DATABASE_CONNECTION_URL --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=org.postgresql.Driver --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLExceptionSorter --jta=true --use-java-context=true --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.postgres.PostgreSQLValidConnectionChecker

    reload --use-current-server-config=true
    ```

    **MySQL**

    ```console
    module add --name=com.mysql --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=mysql:add(driver-name=mysql,driver-module-name=com.mysql,driver-class-name=com.mysql.cj.jdbc.Driver)

    data-source add --name=mysqlDS --jndi-name=java:jboss/datasources/mysqlDS --connection-url=$DATABASE_CONNECTION_URL --driver-name=mysql --user-name=$DATABASE_SERVER_ADMIN_FULL_NAME --password=$DATABASE_SERVER_ADMIN_PASSWORD --use-ccm=true --max-pool-size=5 --blocking-timeout-wait-millis=5000 --enabled=true --driver-class=com.mysql.cj.jdbc.Driver --jta=true --use-java-context=true --exception-sorter-class-name=com.mysql.cj.jdbc.integration.jboss.ExtendedMysqlExceptionSorter

    reload --use-current-server-config=true
    ```

    **SQL Server**

    ```console
    module add --name=com.microsoft --resources=<JDBC .jar file path> --module-xml=<module file path>

    /subsystem=datasources/jdbc-driver=sqlserver:add(driver-name=sqlserver,driver-module-name=com.microsoft,driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver,driver-datasource-class-name=com.microsoft.sqlserver.jdbc.SQLServerDataSource)

    data-source add --name=sqlDS --jndi-name=java:jboss/datasources/sqlDS --driver-name=sqlserver --connection-url=$DATABASE_CONNECTION_URL --validate-on-match=true --background-validation=false --valid-connection-checker-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLValidConnectionChecker --exception-sorter-class-name=org.jboss.jca.adapters.jdbc.extensions.mssql.MSSQLExceptionSorter

    reload --use-current-server-config=true
    ```

    Diese Datei wird vom Startskript wie im nächsten Schritt beschrieben ausgeführt. Sie installiert den JDBC-Treiber als WildFly-Modul, erstellt die entsprechende WildFly-Datenquelle und lädt den Server neu, damit die Änderungen wirksam werden.

4. Erstellen Sie eine Datei mit einem Namen wie *„startup.sh“* und fügen Sie den folgenden Code hinzu. Ersetzen Sie `<JBoss CLI script>` durch den Namen der im vorherigen Schritt erstellten Datei. Achten Sie darauf, dass Sie den vollständigen Pfad zu dem Ort einfügen, in den Sie die Datei in Ihrer App Service-Instanz verschieben, wie z. B. */home/datasource-commands.cli*.

    ```bash
    #!/usr/bin/env bash
    /opt/jboss/wildfly/bin/jboss-cli.sh -c --file=<JBoss CLI script>
    ```

5. Verwenden Sie FTP zum Hochladen der JDBC .jar-Datei, der modularen XML-Datei, des JBoss CLI-Skripts und des Startskripts in Ihre App Service-Instanz. Verschieben Sie diese Dateien an den Ort, den Sie in den vorherigen Schritten angegeben haben, wie z. B. */home*. Weitere Informationen über FTP finden Sie unter [Bereitstellen der App in Azure App Service mit FTP/S](https://docs.microsoft.com/azure/app-service/deploy-ftp).

6. Verwenden Sie die Azure CLI, um Ihrem App Service Einstellungen mit Ihren Datenbankverbindungsdaten hinzuzufügen. Ersetzen Sie `<resource group>` und `<webapp name>` mit den Werten, die Ihr App Service einsetzt. Ersetzen Sie `<database server name>`, `<database name>`, `<admin name>` und `<admin password>` mit Ihren Datenbankverbindungsdaten. Sie erhalten Ihre App Service- und Datenbankdaten im Azure-Portal.

    **PostgreSQL:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:postgresql://<database server name>:5432/<database name>?ssl=true \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **MySQL:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT \
            DATABASE_SERVER_ADMIN_FULL_NAME=<admin name> \
            DATABASE_SERVER_ADMIN_PASSWORD=<admin password>
    ```

    **SQL Server:**

    ```bash
    az webapp config appsettings set \
        --resource-group <resource group> \
        --name <webapp name> \
        --settings \
            DATABASE_CONNECTION_URL=jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
    ```

    Die DATABASE_CONNECTION_URL-Werte unterscheiden sich für jeden Datenbankserver und auch von den Werten im Azure-Portal. Die hier gezeigten URL-Formate (und die Ausschnitte oben) müssen von WildFly genutzt werden:

    * **PostgreSQL:** `jdbc:postgresql://<database server name>:5432/<database name>?ssl=true`
    * **MySQL:** `jdbc:mysql://<database server name>:3306/<database name>?ssl=true\&useLegacyDatetimeCode=false\&serverTimezone=GMT`
    * **SQL Server:** `jdbc:sqlserver://<database server name>:1433;database=<database name>;user=<admin name>;password=<admin password>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;`

7. Navigieren Sie im Azure-Portal zu Ihrem App Service und suchen Sie dort die Seite **„Konfiguration“**  >  **„Allgemeine Einstellungen“** . Legen Sie das Feld **Startskript** auf den Namen und den Ort Ihres Startskript fest, z.B. */home/startup.sh*.

Wenn Ihr App Service das nächste Mal neugestartet wird, führt er das Startskript aus und führt die notwendigen Konfigurationsschritte durch. Sie können mit SSH auf Ihren App Service zugreifen, um zu überprüfen, ob diese Konfiguration richtig ist. Führen Sie dann das Startskript aus der Bash-Aufforderung selber aus. Sie können auch die App Service-Protokolle untersuchen. Weitere Informationen über diese Optionen finden sie unter [„Protokollieren und Debuggen von Apps“](#logging-and-debugging-apps).

Als Nächstes müssen Sie die WildFly-Konfiguration für Ihre Anwendung aktualisieren und erneut bereitstellen. Führen Sie die folgenden Schritte aus:

1. Öffnen Sie die Datei *src/main/resources/META-INF/persistence.xml* für Ihre Anwendung und suchen Sie das Element `<jta-data-source>`. Ersetzen Sie seinen Inhalt wie folgt:

    **PostgreSQL**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

    **MySQL**

    ```xml
    <jta-data-source>java:jboss/datasources/mysqlDS</jta-data-source>
    ```

    **SQL Server**

    ```xml
    <jta-data-source>java:jboss/datasources/postgresDS</jta-data-source>
    ```

2. Erstellen Sie Ihre App neu und stellen Sie sie mit dem folgenden Befehl in der Bash-Aufforderung bereit:

    ```bash
    mvn package -DskipTests azure-webapp:deploy
    ```

3. Starten Sie Ihre App Service-Instanz neu, indem Sie im Abschnitt **Übersicht** des Azure-Portals auf die Schaltfläche **Neu starten** klicken oder die Azure CLI verwenden.

Ihre App Service-Instanz ist jetzt so konfiguriert, dass sie auf Ihre Datenbank zugreift.

Weitere Informationen über die Konfigurierung der Datenbankverbindungen mit WildFly finden Sie unter [PostgreSQL](https://developer.jboss.org/blogs/amartin-blog/2012/02/08/how-to-set-up-a-postgresql-jdbc-driver-on-jboss-7), [MySQL](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#Using_other_Databases-Using_MySQL_as_the_Default_DataSource) oder [SQL Server](https://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/Using_other_Databases.html#d0e3898).

### <a name="enable-messaging-providers"></a>Aktivieren von Messaginganbietern

So aktivieren Sie nachrichtengesteuerte Beans mit Service Bus als Messagingmechanismus

1. Verwenden Sie die [Apache QPId JMS-Messagingbibliothek](https://qpid.apache.org/proton/index.html). Fügen Sie diese Abhängigkeit in der Datei „pom.xml“ (oder einer anderen Builddatei) für die Anwendung ein.

2. Erstellen Sie [Service Bus-Ressourcen](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp). Erstellen Sie einen Azure Service Bus-Namespace und eine Warteschlange in diesem Namespace und eine SAS-Richtlinie mit Funktionen zum Senden und Empfangen.

3. Übergeben Sie den Schlüssel für die SAS-Richtlinie an den Code, entweder durch URL-Codierung des Primärschlüssels der Richtlinie oder durch [Verwenden des Service Bus SDK](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp#setup-jndi-context-and-configure-the-connectionfactory).

4. Führen Sie die im Abschnitt „Module und Abhängigkeiten“ beschriebenen Schritte mit dem XML-Moduldeskriptor, den JAR-Abhängigkeiten, den JBoss CLI-Befehlen und dem Startskript für den JMS-Anbieter aus. Zusätzlich zu diesen vier Dateien müssen Sie auch eine XML-Datei erstellen, die den JNDI-Namen für die JMS-Warteschlange und das JMS-Thema definiert. Referenzkonfigurationsdateien finden Sie in [diesem Repository](https://github.com/JasonFreeberg/widlfly-server-configs/tree/master/appconfig).

## <a name="use-redis-as-a-session-cache-with-tomcat"></a>Verwenden von Redis als Sitzungscache mit Tomcat

Sie können Tomcat zur Verwendung eines externen Sitzungsspeichers konfigurieren, z. B. [Azure Cache for Redis](/azure/azure-cache-for-redis/). Dies ermöglicht es Ihnen, den Benutzersitzungszustand (z. B. Warenkorbdaten) beizubehalten, wenn ein Benutzer zu einer anderen Instanz Ihrer App übertragen wird, z. B. wenn es zur automatischen Skalierung, einem Neustart oder einem Failover kommt.

Um Tomcat mit Redis zu verwenden, müssen Sie Ihre App für die Verwendung einer [PersistentManager](http://tomcat.apache.org/tomcat-8.5-doc/config/manager.html)-Implementierung konfigurieren. In den folgenden Schritten wird dieser Prozess erläutert, wobei [Pivotal-Sitzungs-Manager:redis-store](https://github.com/pivotalsoftware/session-managers/tree/master/redis-store) als Beispiel verwendet wird.

1. Öffnen Sie in Bash-Terminal, und verwenden Sie `export <variable>=<value>`, um jede der folgenden Umgebungsvariablen festzulegen.

    | Variable                 | Wert                                                                      |
    |--------------------------|----------------------------------------------------------------------------|
    | RESOURCEGROUP_NAME       | Der Name der Ressourcengruppe, die Ihre App Service-Instanz enthält.       |
    | WEBAPP_NAME              | Der Name Ihrer App Service-Instanz.                                     |
    | WEBAPP_PLAN_NAME         | Der Name Ihres App Service-Plans.                                          |
    | REGION                   | Der Name der Region, in der Ihre App gehostet wird.                           |
    | REDIS_CACHE_NAME         | Der Name Ihrer Azure Cache for Redis-Instanz.                           |
    | REDIS_PORT               | Der SSL-Port, an dem Ihre Redis-Cache lauscht.                             |
    | REDIS_PASSWORD           | Der primäre Zugriffsschlüssel für Ihre Instanz.                                  |
    | REDIS_SESSION_KEY_PREFIX | Ein Wert, den Sie angeben, um Sitzungsschlüssel zu identifizieren, die von Ihrer App stammen. |

    Sie finden den Namen, den Port und die Zugriffsschlüsselinformationen im Azure-Portal, indem Sie in den Abschnitten **Eigenschaften** oder **Zugriffsschlüssel** Ihrer Dienstinstanz nachsehen.

2. Erstellen oder aktualisieren Sie die Datei *src/main/webapp/META-INF/context.xml* Ihrer App mit folgendem Inhalt:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Context path="">
        <!-- Specify Redis Store -->
        <Valve className="com.gopivotal.manager.SessionFlushValve" />
        <Manager className="org.apache.catalina.session.PersistentManager">
            <Store className="com.gopivotal.manager.redis.RedisStore"
                   connectionPoolSize="20"
                   host="${REDIS_CACHE_NAME}.redis.cache.windows.net"
                   port="${REDIS_PORT}"
                   password="${REDIS_PASSWORD}"
                   sessionKeyPrefix="${REDIS_SESSION_KEY_PREFIX}"
                   timeout="2000"
            />
        </Manager>
    </Context>
    ```

    Diese Datei gibt die Sitzungs-Manager-Implementierung für Ihre App an und konfiguriert diese. Sie verwendet die Umgebungsvariablen, die Sie im vorherigen Schritt festgelegt haben, um Ihre Kontoinformationen aus ihren Quelldateien herauszuhalten.

3. Verwenden Sie FTP, um die JAR-Datei des Sitzungs-Managers in Ihre App Service-Instanz hochzuladen, indem Sie sie im Verzeichnis */home/tomcat/lib* ablegen. Weitere Informationen finden Sie unter [Bereitstellen der App in Azure App Service mit FTP/S](https://docs.microsoft.com/azure/app-service/deploy-ftp).

4. Deaktivieren Sie das [Sitzungsaffinitätscookie](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) für Ihre App Service-Instanz. Sie können dies über das Azure-Portal tun, indem Sie zu Ihrer App navigieren und dann **Konfiguration > Allgemeine Einstellungen > ARR-Affinität** auf **Aus** festlegen. Alternativ können Sie den folgenden Befehl verwenden:

    ```azurecli
    az webapp update -g <resource group> -n <webapp name> --client-affinity-enabled false
    ```

    Standardmäßig werden in App Service Sitzungsaffinitätscookies verwendet, um sicherzustellen, dass Clientanforderungen mit vorhandenen Sitzungen an die gleiche Instanz der Anwendung weitergeleitet werden. Dieses Standardverhalten erfordert keine Konfiguration, kann aber den Benutzersitzungszustand nicht beibehalten, wenn Ihre App-Instanz neu gestartet oder Datenverkehr zu einer anderen Instanz umgeleitet wird. Wenn Sie [die vorhandene ARR-Instanzaffinitäts](https://azure.microsoft.com/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)konfiguration deaktivieren, um die cookiebasierte Weiterleitung der Sitzung zu deaktivieren, ermöglichen Sie, dass der konfigurierte Sitzungsspeicher ohne Beeinträchtigung ausgeführt wird.

5. Navigieren Sie zum Abschnitt **Eigenschaften** Ihrer App Service-Instanz, und suchen Sie **Zusätzliche ausgehende IP-Adressen**. Diese repräsentieren alle möglichen ausgehenden IP-Adressen für Ihre App. Kopieren Sie sie, um sie im nächsten Schritt zu verwenden.

6. Erstellen Sie für jede IP-Adresse in ihrer Azure Cache for Redis-Instanz eine Firewallregel. Dies können Sie im Azure-Portal im Abschnitt **Firewall** Ihrer Redis-Instanz durchführen. Geben Sie einen eindeutigen Namen für jede Regel an, und legen Sie die Werte für **Start-IP-Adresse** und **End-IP-Adresse** auf dieselbe IP-Adresse fest.

7. Navigieren Sie zum Abschnitt **Erweiterte Einstellungen** Ihrer Redis-Instanz, und legen Sie **Nur Zugriff über SSL zulassen** auf **Nein** fest. Dadurch kann Ihre App Service Instanz über die Azure-Infrastruktur mit Ihrem Redis Cache kommunizieren.

8. Aktualisieren Sie die `azure-webapp-maven-plugin`-Konfiguration in der Datei *pom.xml* Ihrer App so, dass sie auf Ihre Redis-Kontoinformationen verweist. Diese Datei verwendet die Umgebungsvariablen, die Sie zuvor festgelegt haben, um Ihre Kontoinformationen aus ihren Quelldateien herauszuhalten.

    Ändern Sie, falls notwendig, `1.7.0` in die aktuelle Version des [Maven-Plug-Ins für Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme).

    ```xml
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.7.0</version>
        <configuration>

            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appServicePlanName>${WEBAPP_PLAN_NAME}-${REGION}</appServicePlanName>
            <appName>${WEBAPP_NAME}-${REGION}</appName>
            <region>${REGION}</region>
            <linuxRuntime>tomcat 9.0-jre8</linuxRuntime>

            <appSettings>
                <property>
                    <name>REDIS_CACHE_NAME</name>
                    <value>${REDIS_CACHE_NAME}</value>
                </property>
                <property>
                    <name>REDIS_PORT</name>
                    <value>${REDIS_PORT}</value>
                </property>
                <property>
                    <name>REDIS_PASSWORD</name>
                    <value>${REDIS_PASSWORD}</value>
                </property>
                <property>
                    <name>REDIS_SESSION_KEY_PREFIX</name>
                    <value>${REDIS_SESSION_KEY_PREFIX}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Xms2048m -Xmx2048m -DREDIS_CACHE_NAME=${REDIS_CACHE_NAME} -DREDIS_PORT=${REDIS_PORT} -DREDIS_PASSWORD=${REDIS_PASSWORD} IS_SESSION_KEY_PREFIX=${REDIS_SESSION_KEY_PREFIX}</value>
                </property>

            </appSettings>

        </configuration>
    </plugin>
    ```

9. Erstellen Sie Ihre App neu, und stellen Sie sie erneut bereit.

    ```bash
    mvn package
    mvn azure-webapp:deploy
    ```

Ihre App verwendet nun Ihren Redis Cache für die Sitzungsverwaltung.

Ein Beispiel, das Sie verwenden können, um diese Anweisungen zu testen, finden Sie im Repository [scaling-stateful-java-web-app-on-azure](https://github.com/Azure-Samples/scaling-stateful-java-web-app-on-azure) auf GitHub.

## <a name="docker-containers"></a>Docker-Container

Wenn Sie das von Azure unterstützte Zulu JDK in Ihren Containern verwenden möchten, rufen Sie die auf der [Downloadseite für Azul Zulu Enterprise for Azure](https://www.azul.com/downloads/azure-only/zulu/) aufgeführten vorgefertigten Images per Pull ab, oder nutzen Sie die `Dockerfile`-Beispiele aus dem [Microsoft Java-GitHub-Repository](https://github.com/Microsoft/java/tree/master/docker).

## <a name="statement-of-support"></a>Unterstützungserklärung

### <a name="runtime-availability"></a>Runtimeverfügbarkeit

App Service für Linux unterstützt zwei Runtimes zum verwalteten Hosten von Java-Webanwendungen:

- [Tomcat-Servlet-Container](https://tomcat.apache.org/) zum Ausführen von Anwendungen, die als Webarchivdateien (WAR) paketiert wurden. Unterstützt werden die Versionen 8.5 und 9.0.
- Java SE Runtime Environment zum Ausführen von Anwendungen, die als Java-Archivdateien (JAR) paketiert wurden. Unterstützt werden die Versionen Java 8 und 11.

### <a name="jdk-versions-and-maintenance"></a>JDK-Versionen und -Wartung

Bei Azul Zulu Enterprise-Builds von OpenJDK handelt es sich um eine kostenlose, plattformübergreifende und produktionsbereite Distribution von OpenJDK für Azure und Azure Stack, die von Microsoft und Azul Systems unterstützt wird. Sie enthält alle Komponenten, die zum Erstellen und Ausführen von Java SE-Anwendungen benötigt werden. Sie können die JDK aus der [Java JDK-Installation](https://aka.ms/azure-jdks) heraus installieren.

Unterstützte JDKs werden jedes Vierteljahr im Januar, April, Juli und Oktober automatisch gepatcht.

### <a name="security-updates"></a>Sicherheitsupdates

Patches und Fixes für größere Sicherheitsrisiken werden veröffentlicht, sobald sie von Azul Systems zur Verfügung gestellt werden. Ein „größeres“ Sicherheitsrisiko wird durch eine Gesamtbewertung von 9.0 oder höher im [NIST Common Vulnerability Scoring System, Version 2](https://nvd.nist.gov/cvss.cfm), definiert.

### <a name="deprecation-and-retirement"></a>Einstellung und Außerbetriebnahme

Wenn eine unterstützte Java-Runtime eingestellt wird, erhalten Azure-Entwickler, die die betreffende Runtime verwenden, mindestens sechs Monate vor Außerbetriebnahme der Runtime eine entsprechende Benachrichtigung.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das Center [Azure für Java-Entwickler](/java/azure/), um Azure-Schnellstarts, Tutorials und Java-Referenzdokumentation zu finden.

Allgemeine Fragen zur Verwendung von App Service für Linux, die sich nicht speziell auf die Java-Entwicklung beziehen, werden unter [Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux](app-service-linux-faq.md) beantwortet.
