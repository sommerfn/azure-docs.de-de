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
ms.openlocfilehash: e773b997cca3fa9a1f11fec2ac449e1fc11c5364
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554557"
---
# <a name="quickstart-launch-an-azure-spring-cloud-app-by-using-the-maven-plug-in"></a>Schnellstart: Starten einer Azure Spring Cloud-App mithilfe des Maven-Plug-Ins

Mit dem Maven-Plug-In von Azure Spring Cloud können Sie ganz einfach Ihre Azure Spring Cloud-Dienstanwendungen erstellen und aktualisieren. Sie können Anwendungen in Ihrem vorhandenen Azure Spring Cloud-Dienst bereitstellen, indem Sie eine Konfiguration vordefinieren. In diesem Artikel wird eine Beispielanwendung namens PiggyMetrics verwendet, um dieses Feature zu veranschaulichen.

>[!Note]
> Vergewissern Sie sich, dass Ihr Azure-Abonnement auf Azure Spring Cloud zugreifen kann, bevor Sie mit diesem Schnellstart beginnen. Da der Dienst als Vorschau verfügbar ist, bitten wir Kunden, sich an uns zu wenden, damit wir die Abonnements der Zulassungsliste hinzufügen können. Wenn Sie die Funktionen von Azure Spring Cloud ausprobieren möchten, senden Sie das ausgefüllte [Formular für Azure Spring Cloud (private Vorschau)](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR-LA2geqX-ZLhi-Ado1LD3tUNDk2VFpGUzYwVEJNVkhLRlcwNkZFUFZEUS4u) ein.

>[!TIP]
> Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Befehle in diesem Artikel ausführen können. Sie verfügt über allgemeine vorinstallierte Azure-Tools, u. a. die aktuellen Versionen von Git, Java Development Kit (JDK), Maven und der Azure-Befehlszeilenschnittstelle. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie [Azure Cloud Shell](https://shell.azure.com). Weitere Informationen finden Sie in der [Übersicht über Azure Cloud Shell](../cloud-shell/overview.md).

So führen Sie diesen Schnellstart durch:

1. [Git installieren](https://git-scm.com/).
2. [Installation von JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
3. [Installation von Maven 3.0 oder höher](https://maven.apache.org/download.cgi)
4. [Installieren Sie die Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
5. [Registrieren für eine kostenlose Azure-Testversion](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle

Führen Sie den folgenden Befehl aus, um die Azure Spring Cloud-Erweiterung für die Azure CLI zu installieren:

```Azure CLI
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```

## <a name="provision-a-service-instance-on-the-azure-portal"></a>Bereitstellen einer Dienstinstanz im Azure-Portal

1. Öffnen Sie in einem Webbrowser das [Azure-Portal](https://portal.azure.com), und melden Sie sich bei Ihrem Konto an.

1. Suchen Sie nach **Azure Spring Cloud**, und wählen Sie den Eintrag aus. 
1. Wählen Sie auf der Seite „Übersicht“ die Option **Erstellen** aus, und führen Sie dann die folgenden Schritte aus:  

    a. Geben Sie im Feld **Dienstname** den Namen Ihrer Dienstinstanz an. Der Name muss zwischen 4 und 32 Zeichen lang sein und darf nur Kleinbuchstaben, Ziffern und Bindestriche enthalten. Das erste Zeichen des Dienstnamens muss ein Buchstabe und das letzte Zeichen entweder ein Buchstabe oder eine Ziffer sein.  

    b. Wählen Sie in der Dropdownliste **Abonnement** das Abonnement aus, unter dem diese Ressource abgerechnet werden soll. Stellen Sie sicher, dass dieses Abonnement der Zulassungsliste für Azure Spring Cloud hinzugefügt wurde.  

    c. Erstellen Sie im Feld **Ressourcengruppe** eine neue Ressourcengruppe. Die Erstellung von Ressourcengruppen für neue Ressourcen ist eine bewährte Methode.  

    d. Wählen Sie in der Dropdownliste **Standort** den Standort Ihrer Dienstinstanz aus. Derzeit werden die Standorte „USA, Osten“, „USA, Westen 2“, „Europa, Westen“ und „Asien, Südosten“ unterstützt.
    
Die Bereitstellung des Diensts dauert etwa fünf Minuten. Nach der Bereitstellung des Diensts wird die Seite **Übersicht** für die Dienstinstanz angezeigt.

## <a name="set-up-your-configuration-server"></a>Einrichten Ihres Konfigurationsservers

1. Wählen Sie auf der Seite **Übersicht** die Option **Konfigurationsserver** aus.
1. Legen Sie im Abschnitt **Standardrepository** den Wert für **URI** auf **https://github.com/Azure-Samples/piggymetrics** und **Bezeichnung** auf **Konfiguration** fest, und wählen Sie **Anwenden** aus, um Ihre Änderungen zu speichern.

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

1. Fügen Sie der Datei *pom.xml* oder *settings.xml* den folgenden Code hinzu, damit Maven mit Azure Spring Cloud verwendet werden kann:

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

    a. Wählen Sie die Module `gateway`, `auth-service` und `account-service` aus.

    b. Wählen Sie Ihr Abonnement und den Azure Spring Cloud-Dienstcluster aus.

    c. Geben Sie in der Liste der angegebenen Projekte die Zahl für `gateway` ein, um öffentlichen Zugriff zu gewähren.
    
    d. Bestätigen Sie die Konfiguration.

1. Stellen Sie die Apps mit dem folgenden Befehl bereit:

   ```azurecli
   mvn com.microsoft.azure:azure-spring-cloud-maven-plugin:0.1.0-SNAPSHOT:deploy
   ```

1. Sie können mit der URL aus der Ausgabe des vorherigen Befehls auf PiggyMetrics zugreifen.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Spring Cloud-Anwendung aus einem Maven-Repository bereitgestellt. Weitere Informationen zu Azure Spring Cloud finden Sie im Tutorial zum Vorbereiten Ihrer App für die Bereitstellung.

> [!div class="nextstepaction"]
> [Vorbereiten der Azure Spring Cloud-Anwendung für die Bereitstellung](spring-cloud-tutorial-prepare-app-deployment.md)
