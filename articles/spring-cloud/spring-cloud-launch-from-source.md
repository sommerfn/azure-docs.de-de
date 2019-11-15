---
title: 'Schnellstart: Starten Ihrer Spring Cloud-Anwendung aus dem Quellcode'
description: Erfahren Sie, wie Sie Ihre Azure Spring Cloud-Anwendung direkt aus dem Quellcode starten.
author: jpconnock
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/30/2019
ms.author: jeconnoc
ms.openlocfilehash: 7ca80966ccab83991246f0ed7ea35cf2c9524b1d
ms.sourcegitcommit: bc7725874a1502aa4c069fc1804f1f249f4fa5f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73721274"
---
# <a name="launch-your-spring-cloud-application-from-source-code"></a>Starten Ihrer Spring Cloud-Anwendung aus dem Quellcode

Azure Spring Cloud ermöglicht Ihnen die einfache Ausführung von Spring Cloud-basierten Microserviceanwendungen in Azure.

Azure Spring Cloud ermöglicht Ihnen das direkte Starten Ihrer Anwendung aus Ihrem Java-Quellcode oder aus einer vorkonfigurierten JAR-Datei. In diesem Artikel werden die erforderlichen Schritte beschrieben.

In dieser Schnellstartanleitung wird Folgendes vermittelt:

> [!div class="checklist"]
> * Bereitstellen einer Dienstinstanz
> * Festlegen eines Konfigurationsservers für eine Instanz
> * Lokales Erstellen einer Microserviceanwendung
> * Bereitstellen der einzelnen Microservices
> * Zuweisen eines öffentlichen Endpunkts für Ihre Anwendung

## <a name="prerequisites"></a>Voraussetzungen

>[!Note]
> Azure Spring Cloud wird zurzeit als Public Preview angeboten. Angebote der Public Preview ermöglichen Kunden das Experimentieren mit neuen Funktionen vor der offiziellen Veröffentlichung.  Funktionen und Dienste in der Public Preview sind nicht zur Verwendung in der Produktion bestimmt.  Weitere Informationen zur Unterstützung während der Vorschauphase finden Sie in den [häufig gestellten Fragen](https://azure.microsoft.com/support/faq/). Sie können auch eine [Supportanfrage](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) einreichen, um weitere Informationen zu erhalten.

Stellen Sie vor dem Beginn sicher, dass Ihr Azure-Abonnement über die erforderlichen Abhängigkeiten verfügt:

1. [Installation von Git](https://git-scm.com/)
2. [Installation von JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
3. [Installation von Maven 3.0 oder höher](https://maven.apache.org/download.cgi)
4. [Installieren der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
5. [Registrierung für ein Azure-Abonnement](https://azure.microsoft.com/free/)

> [!TIP]
> Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können.  Sie verfügt über allgemeine vorinstallierte Azure-Tools, u. a. die aktuellen Versionen von Git, JDK, Maven und der Azure-Befehlszeilenschnittstelle. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie [Azure Cloud Shell](https://shell.azure.com) über shell.azure.com.  Weitere Informationen zu Azure Cloud Shell finden Sie in der [Dokumentation](../cloud-shell/overview.md).

## <a name="install-the-azure-cli-extension"></a>Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle

Führen Sie den folgenden Befehl aus, um die Azure Spring Cloud-Erweiterung für die Azure-Befehlszeilenschnittstelle zu installieren:

```Azure CLI
az extension add --name spring-cloud
```

## <a name="provision-a-service-instance-using-the-azure-cli"></a>Bereitstellen einer Dienstinstanz mithilfe der Azure-Befehlszeilenschnittstelle

Melden Sie sich bei der Azure-Befehlszeilenschnittstelle an, und wählen Sie Ihr aktives Abonnement aus. Vergewissern Sie sich, dass Sie das aktive Abonnement auswählen, das in der Whitelist für Azure Spring Cloud enthalten ist.

```Azure CLI
az login
az account list -o table
az account set --subscription
```

Öffnen Sie ein Fenster der Azure-Befehlszeilenschnittstelle, und führen Sie die folgenden Befehle aus, um eine Instanz von Azure Spring Cloud bereitzustellen. Beachten Sie, dass wir Azure Spring Cloud auch anweisen, hier eine öffentliche Domäne zuzuweisen.

```azurecli
    az spring-cloud create -n <resource name> -g <resource group name> --is-public true
```

Die Bereitstellung der Dienstinstanz dauert ungefähr fünf Minuten.

Legen Sie den Standardnamen für die Ressourcengruppe und den Clusternamen mithilfe der folgenden Befehle fest:

```azurecli
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
```

## <a name="create-the-spring-cloud-application"></a>Erstellen der Spring Cloud-Anwendung

Mit dem folgenden Befehl erstellen Sie eine Spring Cloud-Anwendung in Ihrem Abonnement.  Damit wird ein leerer Spring Cloud-Dienst erstellt, in den Sie Ihre Anwendung hochladen können.

```azurecli
az spring-cloud app create -n <app-name>
```

## <a name="deploy-your-spring-cloud-application"></a>Bereitstellen der Spring Cloud-Anwendung

Sie können Ihre Anwendung über eine vorkonfigurierte JAR-Datei oder ein Gradle- oder Maven-Repository bereitstellen.  Im Folgenden finden Sie Anweisungen für die einzelnen Fälle.

### <a name="deploy-a-built-jar"></a>Bereitstellen einer erstellten JAR-Datei

Stellen Sie zum Bereitstellen aus einer auf Ihrem lokalen Computer erstellten JAR-Datei sicher, dass der Build eine [fat-JAR](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-build.html#howto-create-an-executable-jar-with-maven)-Datei erzeugt.

So stellen Sie die fat-JAR-Datei für eine aktive Bereitstellung bereit

```azurecli
az spring-cloud app deploy -n <app-name> --jar-path <path-to-fat-JAR>
```

So stellen Sie die fat-JAR-Datei für eine bestimmte Bereitstellung bereit

```azurecli
az spring-cloud app deployment create --app <app-name> -n <deployment-name> --jar-path <path-to-built-jar>
```

### <a name="deploy-from-source-code"></a>Bereitstellen aus dem Quellcode

Azure Spring Cloud verwendet [kpack](https://github.com/pivotal/kpack), um Ihr Projekt zu erstellen.  Sie können mithilfe der Azure-Befehlszeilenschnittstelle Ihren Quellcode hochladen, das Projekt mit kpack erstellen und es in der Zielanwendung bereitstellen.

> [!WARNING]
> Das Projekt darf nur eine JAR-Datei mit einem `main-class`-Eintrag in der Datei `MANIFEST.MF` in `target` (für Maven-Bereitstellungen) oder `build/libs` (für Gradle-Bereitstellungen) erzeugen.  Wenn mehrere JAR-Dateien mit `main-class`-Einträgen vorhanden sind, führt die Bereitstellung zu einem Fehler.

Für Maven-/Gradle-Projekte mit einem Modul:

```azurecli
cd <path-to-maven-or-gradle-source-root>
az spring-cloud app deploy -n <app-name>
```

Für Maven-/Gradle-Projekte mit mehreren Modulen wiederholen Sie diesen Schritt für jedes Modul:

```azurecli
cd <path-to-maven-or-gradle-source-root>
az spring-cloud app deploy -n <app-name> --target-module <relative-path-to-module>
```

### <a name="show-deployment-logs"></a>Anzeigen von Bereitstellungsprotokollen

Sie überprüfen die kpack-Buildprotokolle mit dem folgenden Befehl:

```azurecli
az spring-cloud app show-deploy-log -n <app-name> [-d <deployment-name>]
```

> [!NOTE]
> In den kpack-Protokollen wird nur die letzte Bereitstellung angezeigt, wenn diese Bereitstellung aus dem Quellcode mithilfe von kpack erstellt wurde.

## <a name="assign-a-public-endpoint-to-gateway"></a>Zuweisen eines öffentlichen Endpunkts zum Gateway

1. Öffnen Sie die Seite **Anwendungs-Dashboard**.
2. Wählen Sie die `gateway`-Anwendung aus, um die Seite **Anwendungsdetails** anzuzeigen.
3. Wählen Sie **Domäne zuweisen** aus, um dem Gateway einen öffentlichen Endpunkt zuzuweisen. Dies kann einige Minuten dauern. 
4. Geben Sie in Ihrem Browser die zugewiesene öffentliche IP-Adresse ein, um die laufende Anwendung anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen einer Dienstinstanz
> * Festlegen eines Konfigurationsservers für eine Instanz
> * Lokales Erstellen einer Microserviceanwendung
> * Bereitstellen der einzelnen Microservices
> * Bearbeiten der Umgebungsvariablen für Anwendungen
> * Zuweisen einer öffentlichen IP-Adresse für Ihr Anwendungsgateway

> [!div class="nextstepaction"]
> [Vorbereiten der Azure Spring Cloud-Anwendung für die Bereitstellung](spring-cloud-tutorial-prepare-app-deployment.md)
