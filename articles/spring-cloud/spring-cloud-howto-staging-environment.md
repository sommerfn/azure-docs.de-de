---
title: Einrichten einer Stagingumgebung in Azure Spring Cloud | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Blaugrün-Bereitstellung mit Azure Spring Cloud verwenden.
services: spring-cloud
author: v-vasuke
manager: gwallace
editor: ''
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 10/07/2019
ms.author: v-vasuke
ms.openlocfilehash: 454eeaa2568891ec35fe698cdb20c5448e10887e
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038299"
---
# <a name="how-to-set-up-a-staging-environment"></a>Gewusst wie: Einrichten einer Stagingumgebung

In diesem Artikel erfahren Sie, wie Sie unter Verwendung des Blaugrün-Bereitstellungsmusters in Azure Spring Cloud eine Stagingbereitstellung nutzen. Darüber hinaus wird gezeigt, wie Sie diese Stagingbereitstellung in die Produktion überführen, ohne die Produktionsbereitstellung direkt zu ändern.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie die PiggyMetrics-Anwendung aus dem [Tutorial zum Starten einer Anwendung](spring-cloud-quickstart-launch-app-portal.md) bereits bereitgestellt haben. PiggyMetrics umfasst drei Anwendungen: „gateway“, „account-service“ und „auth-service“.  

Wenn Sie eine andere Anwendung besitzen, die Sie für dieses Beispiel verwenden möchten, müssen Sie eine kleine Änderung am öffentlichen Teil der Anwendung vornehmen.  Durch diese Änderung wird die Stagingbereitstellung von der Produktion unterschieden.

>[!NOTE]
> Vergewissern Sie sich, dass Ihr Azure-Abonnement auf Azure Spring Cloud zugreifen kann, bevor Sie mit dieser Schnellstartanleitung beginnen.  Da der Dienst als Vorschau verfügbar ist, bitten wir Sie, sich an uns zu wenden, damit wir Ihr Abonnement der Zulassungsliste hinzufügen können.  Wenn Sie die Funktionen von Azure Spring Cloud ausprobieren möchten, senden Sie uns eine E-Mail an die folgende Adresse: azure-spring-cloud@service.microsoft.com.

>[!TIP]
> Azure Cloud Shell ist eine kostenlose interaktive Shell, mit der Sie die Schritte in diesem Artikel ausführen können.  Sie verfügt über allgemeine vorinstallierte Azure-Tools, u. a. die aktuellen Versionen von Git, JDK, Maven und der Azure-Befehlszeilenschnittstelle. Wenn Sie bei Ihrem Azure-Abonnement angemeldet sind, starten Sie [Azure Cloud Shell](https://shell.azure.com) über shell.azure.com.  Weitere Informationen zu Azure Cloud Shell finden Sie in der [Dokumentation](../cloud-shell/overview.md).

Führen Sie für diesen Artikel die folgenden Schritte aus:

1. [Installation von Git](https://git-scm.com/)
1. [Installieren von JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
1. [Installieren von Maven 3.0 oder höher](https://maven.apache.org/download.cgi)
1. [Installieren der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
1. [Registrieren für ein Azure-Abonnement](https://azure.microsoft.com/free/)

## <a name="install-the-azure-cli-extension"></a>Installieren der Erweiterung für die Azure-Befehlszeilenschnittstelle

Führen Sie den folgenden Befehl aus, um die Azure Spring Cloud-Erweiterung für die Azure CLI zu installieren:

```azurecli
az extension add -y --source https://azureclitemp.blob.core.windows.net/spring-cloud/spring_cloud-0.1.0-py2.py3-none-any.whl
```
    
## <a name="view-all-deployments"></a>Anzeigen aller Bereitstellungen

Navigieren Sie im Azure-Portal zu Ihrer Dienstinstanz, und wählen Sie **Deployment management** (Bereitstellungsverwaltung) aus, um alle Bereitstellungen anzuzeigen. Wählen Sie bei Bedarf die einzelnen Bereitstellungen aus, um weitere Details anzuzeigen.

## <a name="create-a-staging-deployment"></a>Erstellen einer Stagingbereitstellung

1. Nehmen Sie in Ihrer lokalen Entwicklungsumgebung eine kleine Änderung an der Piggy Metrics-Gatewayanwendung vor. Ändern Sie beispielsweise die Farbe in `gateway/src/main/resources/static/css/launch.css`. Auf diese Weise können Sie die beiden Bereitstellungen problemlos unterscheiden. Führen Sie den folgenden Befehl aus, um das JAR-Paket zu erstellen: 

    ```azurecli
    mvn clean package
    ```

1. Erstellen Sie mit der Azure CLI eine neue Bereitstellung, und geben Sie Ihr den Stagingbereitstellungsnamen „green“.

    ```azurecli
    az spring-cloud app deployment create -g <resource-group-name> -s <service-instance-name> --app gateway -n green --jar-path gateway/target/gateway.jar
    ```

1. Wenn die Bereitstellung abgeschlossen wurde, navigieren Sie über das **Anwendungsdashboard** zur Gatewayseite. Alle Ihre Instanzen werden links auf der Registerkarte **App Instances** (App-Instanzen) angezeigt.
  
> [!NOTE]
> Der Ermittlungsstatus lautet „OUT_OF_SERVICE“. Daher wird erst dann Datenverkehr an diese Bereitstellung weitergeleitet, wenn die Überprüfung abgeschlossen ist.

## <a name="verify-the-staging-deployment"></a>Überprüfen der Stagingbereitstellung

1. Kehren Sie zur Seite **Deployment management** (Bereitstellungsverwaltung) zurück, und wählen Sie Ihre neue Bereitstellung aus. Der Bereitstellungsstatus sollte **Wird ausgeführt** lauten. Die Schaltfläche „Assign/Unassign domain“ (Domäne zuweisen/Zuweisung der Domäne aufheben) ist deaktiviert, da es sich um eine Stagingumgebung handelt.

1. Auf der Seite **Übersicht** sollte ein **Testendpunkt** angezeigt werden. Kopieren Sie ihn, und fügen Sie ihn auf einer neuen Browserseite ein. Anschließend sollte die neue Piggy Metrics-Seite angezeigt werden.

>[!TIP]
> * Vergewissern Sie sich, dass der Testendpunkt auf „/“ endet, um sicherzustellen, dass das CSS richtig geladen wird.  
> * Wenn Sie zum Anzeigen der Seite Anmeldeinformationen im Browser eingeben müssen, verwenden Sie [URL-Decodierung](https://www.urldecoder.org/), um den Testendpunkt zu decodieren. Nach der URL-Decodierung wird eine URL im Format „https://\<Benutzername>:\<Kennwort>@\<Clustername>.test.azureapps.io/gateway/green“ zurückgegeben.  Verwenden Sie diese URL für den Zugriff auf Ihren Endpunkt.

>[!NOTE]    
> Die Konfigurationsservereinstellungen gelten sowohl für die Stagingumgebung als auch für die Produktion. Beispiel: Wenn Sie als Kontextpfad (`server.servlet.context-path`) für App Gateway auf dem Konfigurationsserver *somepath* festgelegt haben, ändert sich der Pfad zu Ihrer grünen Bereitstellung: „https://\<Benutzername>:\<Kennwort>@\<Clustername>.test.azureapps.io/gateway/green/somepath/...“.
 
 Wenn Sie jetzt die öffentliche App Gateway-Instanz aufrufen, wird die alte Seite ohne die Änderung angezeigt.
    
## <a name="set-the-green-as-production-deployment"></a>Festlegen der grünen Bereitstellung als Produktionsbereitstellung

1. Wenn Sie die Änderung in Ihrer Stagingumgebung überprüft haben, können Sie sie in die Produktion überführen. Kehren Sie zu **Deployment management** (Bereitstellungsverwaltung) zurück, und aktivieren Sie das Kontrollkästchen vor der Anwendung „gateway“.
2. Wählen Sie „Set deployment“ (Bereitstellung festlegen) aus.
3. Wählen Sie im Menü „PRODUKTIONSBEREITSTELLUNG“ die Option „Grün“ und anschließend **Anwenden** aus.
4. Navigieren Sie zur Seite **Übersicht** der Gatewayanwendung. Wenn Sie bereits eine Domäne für Ihre Gatewayanwendung zugewiesen haben, wird die URL auf der Seite **Übersicht** angezeigt. Rufen Sie die URL auf, und die geänderte Piggy Metrics-Seite wird angezeigt.

>[!NOTE]
> Wenn die grüne Bereitstellung als Produktionsumgebung festgelegt wurde, wird die vorherige Bereitstellung zur Stagingbereitstellung.

## <a name="modify-the-staging-deployment"></a>Ändern der Stagingbereitstellung

Sind Sie mit Ihrer Änderung nicht zufrieden, können Sie über die Azure CLI den Anwendungscode ändern und ein neues JAR-Paket erstellen und in die grüne Bereitstellung hochladen.

```azurecli
az spring-cloud app deploy  -g <resource-group-name> -s <service-instance-name> -n gateway -d green --jar-path gateway.jar
```

## <a name="delete-a-staging-deployment"></a>Löschen einer Stagingbereitstellung

Löschen Sie die Stagingbereitstellung aus dem Azure-Portal, indem Sie zur Seite der Stagingbereitstellung navigieren und die Schaltfläche **Löschen** auswählen.

Alternativ können Sie die Stagingbereitstellung mit dem folgenden Befehl über die Azure CLI löschen:

```azurecli
az spring-cloud app deployment delete -n <staging-deployment-name> -g <resource-group-name> -s <service-instance-name> --app gateway
```
