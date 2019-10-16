---
title: 'Schnellstart: Starten einer Anwendung mithilfe von Maven – Azure Spring Cloud'
description: Starten einer Beispielanwendung mithilfe von Maven
services: spring-cloud
author: v-vasuke
manager: jeconnoc
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/05/2019
ms.author: v-vasuke
ms.openlocfilehash: 01140e94e8d0cc47570824970801bdd0043324d7
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72166514"
---
# <a name="quickstart-launch-an-azure-spring-cloud-app-using-the-maven-plug-in"></a>Schnellstart: Starten einer Azure Spring Cloud-App mithilfe des Maven-Plug-Ins

Mit dem Maven-Plug-In von Azure Spring Cloud können Sie ganz einfach Ihre Azure Spring Cloud-Dienstanwendungen erstellen und aktualisieren. Sie können Anwendungen in Ihrem vorhandenen Azure Spring Cloud-Dienst bereitstellen, indem Sie eine Konfiguration vordefinieren. In diesem Artikel wird eine Beispielanwendung namens PiggyMetrics verwendet, um dieses Feature zu veranschaulichen.

>[!Note]
> Vergewissern Sie sich, dass Ihr Azure-Abonnement auf Azure Spring Cloud zugreifen kann, bevor Sie mit dieser Schnellstartanleitung beginnen.  Da der Dienst als Vorschau verfügbar ist, bitten wir Kunden, sich an uns zu wenden, damit wir die Abonnements zur Zulassungsliste hinzufügen können.  Wenn Sie die Funktionen von Azure Spring Cloud ausprobieren möchten, füllen Sie [dieses Formular](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-LA2geqX-ZLhi-Ado1LD3tUNDk2VFpGUzYwVEJNVkhLRlcwNkZFUFZEUS4u
) aus.

>[!TIP]
> Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können.  Sie verfügt über allgemeine vorinstallierte Azure-Tools, u. a. die aktuellen Versionen von Git, JDK, Maven und der Azure-Befehlszeilenschnittstelle. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie [Azure Cloud Shell](https://shell.azure.com) über shell.azure.com.  Weitere Informationen zu Azure Cloud Shell finden Sie in der [Dokumentation](../cloud-shell/overview.md).

So führen Sie diesen Schnellstart durch:

1. [Installation von Git](https://git-scm.com/)
2. [Installieren von JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Installieren von Maven 3.0 oder höher](https://maven.apache.org/download.cgi)
4. [Installieren der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Registrieren für ein Azure-Abonnement](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle

Führen Sie den folgenden Befehl aus, um die Azure Spring Cloud-Erweiterung für die Azure CLI zu installieren:

```Azure CLI
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Bereitstellen einer Dienstinstanz im Azure-Portal

1. Öffnen Sie in einem Webbrowser das [Azure-Portal](https://portal.azure.com), und melden Sie sich bei Ihrem Konto an.

1. Suchen Sie nach **Azure Spring Cloud**, und wählen Sie den Eintrag aus, um zur Übersichtsseite zu gelangen. Wählen Sie die Schaltfläche **Erstellen** aus, um loszulegen.

1. Berücksichtigen Sie beim Ausfüllen des Formulars Folgendes:
    - Servicename: Geben Sie den Namen Ihrer Dienstinstanz an.  Der Name muss zwischen 4 und 32 Zeichen lang sein und darf nur Kleinbuchstaben, Ziffern und Bindestriche enthalten.  Das erste Zeichen des Dienstnamens muss ein Buchstabe und das letzte Zeichen entweder ein Buchstabe oder eine Ziffer sein.
    - Abonnement: Wählen Sie das Abonnement aus, unter dem diese Ressource abgerechnet werden soll.  Stellen Sie sicher, dass dieses Abonnement der Zulassungsliste für Azure Spring Cloud hinzugefügt wurde.
    - Ressourcengruppe: Die Erstellung neuer Ressourcengruppen für neue Ressourcen ist eine bewährte Methode.
    - Standort: Wählen Sie den Speicherort für Ihre Dienstinstanz aus. Derzeit werden die Standorte „USA, Osten“, „USA, Westen 2“, „Europa, Westen“ und „Asien, Südosten“ unterstützt.
    
Die Bereitstellung des Diensts dauert etwa fünf Minuten.  Nach der Bereitstellung wird die Seite **Übersicht** für die Dienstinstanz angezeigt.

## <a name="set-up-your-configuration-server"></a>Einrichten Ihres Konfigurationsservers

1. Navigieren Sie zur Seite **Übersicht**, und wählen Sie **Konfigurationsserver** aus.
1. Legen Sie im Abschnitt **Standardrepository** den Wert für **URI** auf „https://github.com/Azure-Samples/piggymetrics“ und **BEZEICHNUNG** auf „Konfiguration“ fest, und wählen Sie **Anwenden** aus, um Ihre Änderungen zu speichern.

## <a name="clone-and-build-the-sample-application-repository"></a>Klonen und Erstellen des Beispielanwendungsrepositorys

1. Führen Sie den folgenden Befehl aus, um das Git-Repository zu klonen:

    ```azurecli
    git clone https://github.com/Azure-Samples/PiggyMetrics
    ```
  
1. Führen Sie den folgenden Befehl aus, um das Verzeichnis zu ändern und das Projekt zu erstellen:

    ```azurecli
    cd PiggyMetrics
    mvn clean package -DskipTests
    ```

## <a name="generate-and-deploy-the-azure-spring-cloud-configuration"></a>Generieren und Bereitstellen der Azure Spring Cloud-Konfiguration

1. Fügen Sie `pom.xml` oder `settings.xml` Folgendes hinzu, damit Maven mit Azure Spring Cloud verwendet werden kann:

    ```xml
    <pluginRepositories>
      <pluginRepository>
        <id>maven.snapshots</id>
        <name>Maven Central Snapshot Repository</name>
        <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
        <releases>
          <enabled>false</enabled>
        </releases>
        <snapshots>
          <enabled>true</enabled>
        </snapshots>
      </pluginRepository>
    </pluginRepositories>
    ```

1. Führen Sie den folgenden Befehl aus, um eine Konfiguration zu generieren:

    ```azurecli
    mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:config
    ```

    1. Wählen Sie die Module `gateway`, `auth-service` und `account-service` aus.

    1. Wählen Sie Ihr Abonnement und den Azure Spring Cloud-Dienstcluster aus.

    1. Geben Sie in der Liste der angegebenen Projekte die Zahl für `gateway` ein, um öffentlichen Zugriff zu gewähren.
    
    1. Bestätigen Sie die Konfiguration.

1. Stellen Sie die Apps mit dem folgenden Befehl bereit:

   ```azurecli
   mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:deploy
   ```

1. Sie können mit der URL aus der Ausgabe des vorherigen Befehls auf PiggyMetrics zugreifen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Spring Cloud-Anwendung aus einem Maven-Repository bereitgestellt.  Weitere Informationen zu Azure Spring Cloud finden Sie im Tutorial zum Vorbereiten Ihrer App für die Bereitstellung.

> [!div class="nextstepaction"]
> [Tutorial: Vorbereiten einer Java Spring-Anwendung für die Bereitstellung in Azure Spring Cloud](spring-cloud-tutorial-prepare-app-deployment.md)
