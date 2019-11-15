---
title: Einrichten eines Konfigurationsservers in Azure Spring Cloud | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie über das Azure-Portal einen Spring Cloud-Konfigurationsserver für Azure Spring Cloud einrichten.
ms.service: spring-cloud
ms.topic: tutorial
ms.author: jeconnoc
author: jpconnock
ms.date: 10/18/2019
ms.openlocfilehash: 6cf7b4a52ba3a7dbda5fa3fa558c4b68d09f4eb2
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73646723"
---
# <a name="tutorial-set-up-a-spring-cloud-config-server-for-your-service"></a>Tutorial: Einrichten eines Spring Cloud-Konfigurationsservers für Ihren Dienst

In diesem Tutorial wird gezeigt, wie Sie einen Spring Cloud-Konfigurationsserver mit Ihrem Azure Spring Cloud-Dienst verbinden.

Die Spring Cloud-Konfiguration bietet server- und clientseitige Unterstützung für eine externalisierte Konfiguration in einem verteilten System. Der Konfigurationsserver bietet Ihnen einen zentralen Ort zum Verwalten externer Eigenschaften für Anwendungen in allen Umgebungen. Weitere Informationen finden Sie in der [Referenz zum Spring Cloud-Konfigurationsserver](https://spring.io/projects/spring-cloud-config).

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 
* Ein bereits bereitgestellter und ausgeführter Azure Spring Cloud-Dienst.  Führen Sie [diese Schnellstartanleitung](spring-cloud-quickstart-launch-app-cli.md) aus, um einen Azure Spring Cloud-Dienst bereitzustellen und zu starten.

## <a name="restriction"></a>Einschränkung

Bei der Verwendung eines __Konfigurationsservers__ mit einem Git-Back-End gelten einige Einschränkungen. Einige Eigenschaften werden für den Zugriff auf den __Konfigurationsserver__ und die __Dienstermittlung__ automatisch in Ihre Anwendungsumgebung eingefügt. Wenn Sie diese Eigenschaften auch in den Dateien Ihres **Konfigurationsservers** konfigurieren, treten unter Umständen Konflikte oder unerwartetes Verhalten auf. Zu den Eigenschaften zählen folgende: 

```yaml
eureka.client.service-url.defaultZone
eureka.client.tls.keystore
server.port
spring.cloud.config.tls.keystore
spring.application.name
```
> [!CAUTION]
> Es wird dringend empfohlen, die oben genannten Eigenschaften __NICHT__ in die Anwendungsdateien des __Konfigurationsservers__ einzufügen.

## <a name="create-your-config-server-files"></a>Erstellen der Konfigurationsserverdateien

Azure Spring Cloud unterstützt Azure DevOps, GitHub, GitLab und Bitbucket zum Speichern Ihrer Konfigurationsserverdateien. Wenn Sie Ihr Repository vorbereitet haben, erstellen Sie die Konfigurationsdateien anhand der folgenden Anweisungen, und speichern Sie sie dort.

Außerdem sind einige konfigurierbare Eigenschaften nur für einige Typen verfügbar. In den folgenden Unterabschnitten sind die Eigenschaften für die einzelnen Repositorytypen aufgeführt.

### <a name="public-repository"></a>Öffentliches Repository

Bei der Verwendung eines öffentlichen Repositorys sind die konfigurierbaren Eigenschaften stärker eingeschränkt.

Alle konfigurierbaren Eigenschaften, die zum Einrichten des öffentlichen `Git`-Repositorys verwendet werden, sind unten aufgeführt.

> [!NOTE]
> Das Verwenden eines Bindestrichs („-“) zum Trennen von Wörtern ist die einzige Namenskonvention, die derzeit unterstützt wird. Sie können beispielsweise `default-label`, aber nicht `defaultLabel` verwenden.

| Eigenschaft        | Erforderlich | Feature                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | Das `uri`-Element des `Git`-Repositorys, das als Konfigurationsserver-Back-End verwendet wird, muss mit `http://`, `https://`, `git@` oder `ssh://` beginnen. |
| `default-label` | `no`     | Als Standardbezeichnung des `Git`-Repositorys muss `branch name`, `tag name` oder `commit-id` des Repositorys verwendet werden. |
| `search-paths`  | `no`     | Ein Array von Zeichenfolgen, die zum Durchsuchen von Unterverzeichnissen des `Git`-Repositorys verwendet werden |

------

### <a name="private-repository-with-ssh-authentication"></a>Privates Repository mit SSH-Authentifizierung

Alle konfigurierbaren Eigenschaften, die zum Einrichten des privaten `Git`-Repositorys mit `Ssh` verwendet werden, sind unten aufgeführt.

> [!NOTE]
> Das Verwenden eines Bindestrichs („-“) zum Trennen von Wörtern ist die einzige Namenskonvention, die derzeit unterstützt wird. Sie können beispielsweise `default-label`, aber nicht `defaultLabel` verwenden.

| Eigenschaft                   | Erforderlich | Feature                                                      |
| :------------------------- | -------- | ------------------------------------------------------------ |
| `uri`                      | `yes`    | Das `uri`-Element des `Git`-Repositorys, das als Konfigurationsserver-Back-End verwendet wird, muss mit `http://`, `https://`, `git@` oder `ssh://` beginnen. |
| `default-label`            | `no`     | Als Standardbezeichnung des `Git`-Repositorys muss `branch name`, `tag name` oder `commit-id` des Repositorys verwendet werden. |
| `search-paths`             | `no`     | Ein Array von Zeichenfolgen, die zum Durchsuchen von Unterverzeichnissen des `Git`-Repositorys verwendet werden |
| `private-key`              | `no`     | Der private `Ssh`-Schlüssel für den Zugriff auf das `Git`-Repository, __erforderlich__, wenn `uri` mit `git@` oder `ssh://` beginnt |
| `host-key`                 | `no`     | Der Hostschlüssel des Git-Repositoryservers. Er darf nicht das Algorithmuspräfix (abgedeckt durch `host-key-algorithm`) enthalten. |
| `host-key-algorithm`       | `no`     | Der Hostschlüsselalgorithmus sollte `ssh-dss`, `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384` oder `ecdsa-sha2-nistp521`sein. Nur erforderlich, wenn `host-key` vorhanden ist. |
| `strict-host-key-checking` | `no`     | Gibt an, ob der Konfigurationsserver nicht gestartet werden kann, wenn das private `host-key`-Element genutzt wird. Muss `true` (Standardwert) oder `false` sein. |

-----

### <a name="private-repository-with-basic-authentication"></a>Privates Repository mit Standardauthentifizierung

Alle konfigurierbaren Eigenschaften, die zum Einrichten des privaten Git-Repositorys mit Standardauthentifizierung verwendet werden, sind unten aufgeführt.

> [!NOTE]
> Das Verwenden eines Bindestrichs („-“) zum Trennen von Wörtern ist die einzige Namenskonvention, die derzeit unterstützt wird. Verwenden Sie beispielsweise `default-label`, aber nicht `defaultLabel`.

| Eigenschaft        | Erforderlich | Feature                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | Das `uri`-Element des `Git`-Repositorys, das als Konfigurationsserver-Back-End verwendet wird, muss mit `http://`, `https://`, `git@` oder `ssh://` beginnen. |
| `default-label` | `no`     | Als Standardbezeichnung des `Git`-Repositorys muss `branch name`, `tag name` oder `commit-id` des Repositorys verwendet werden. |
| `search-paths`  | `no`     | Ein Array von Zeichenfolgen, die zum Durchsuchen von Unterverzeichnissen des `Git`-Repositorys verwendet werden |
| `username`      | `no`     | Das `username`-Element, das für den Zugriff auf den `Git`-Repositoryserver verwendet wird, __erforderlich__, wenn der `Git`-Repositoryserver `Http Basic Authentication` unterstützt |
| `password`      | `no`     | Das Kennwort, das für den Zugriff auf den `Git`-Repositoryserver verwendet wird, __erforderlich__, wenn der `Git`-Repositoryserver `Http Basic Authentication` unterstützt |

> [!NOTE]
> Einige `Git`-Repositoryserver wie GitHub unterstützen „personal-token“ oder „access-token“ als Kennwort für `HTTP Basic Authentication`. Sie können diese Art von Token auch hier als Kennwort verwenden. „personal-token“ oder „access-token“ laufen nicht ab. Bei Git-Repositoryservern wie Bitbucket und Azure DevOps läuft das Token jedoch in ein bis zwei Stunden ab. Daher eignet sich diese Option nicht für die Verwendung mit Azure Spring Cloud.

### <a name="git-repositories-with-pattern"></a>Git-Repositorys mit Muster

Alle konfigurierbaren Eigenschaften, die zum Einrichten von Repositorys mit Muster verwendet werden, sind unten aufgeführt.

> [!NOTE]
> Das Verwenden eines Bindestrichs („-“) zum Trennen von Wörtern ist die einzige Namenskonvention, die derzeit unterstützt wird. Verwenden Sie beispielsweise `default-label`, aber nicht `defaultLabel`.

| Eigenschaft                           | Erforderlich         | Feature                                                      |
| :--------------------------------- | ---------------- | ------------------------------------------------------------ |
| `repos`                            | `no`             | Eine Zuordnung aus den Einstellungen für ein `Git`-Repository mit einem angegebenen Namen |
| `repos."uri"`                      | `yes` für `repos` | Das `uri`-Element des `Git`-Repositorys, das als Konfigurationsserver-Back-End verwendet wird, muss mit `http://`, `https://`, `git@` oder `ssh://` beginnen. |
| `repos."name"`                     | `yes` für `repos` | Ein Name zum Identifizieren eines `Git`-Repositorys, nur __erforderlich__, wenn `repos` vorhanden ist. Beispielsweise von oben: `team-A`, `team-B` |
| `repos."pattern"`                  | `no`             | Ein Array von Zeichenfolgen zum Abgleichen eines Anwendungsnamens. Verwenden Sie für jedes Muster das Format `{application}/{profile}` mit Platzhaltern. |
| `repos."default-label"`            | `no`             | Als Standardbezeichnung des `Git`-Repositorys muss `branch name`, `tag name` oder `commit-id` des Repositorys verwendet werden. |
| `repos."search-paths`"             | `no`             | Ein Array von Zeichenfolgen, die zum Durchsuchen von Unterverzeichnissen des `Git`-Repositorys verwendet werden |
| `repos."username"`                 | `no`             | Das `username`-Element, das für den Zugriff auf den `Git`-Repositoryserver verwendet wird, __erforderlich__, wenn der `Git`-Repositoryserver `Http Basic Authentication` unterstützt |
| `repos."password"`                 | `no`             | Das Kennwort, das für den Zugriff auf den `Git`-Repositoryserver verwendet wird, __erforderlich__, wenn der `Git`-Repositoryserver `Http Basic Authentication` unterstützt |
| `repos."private-key"`              | `no`             | Der private `Ssh`-Schlüssel für den Zugriff auf das `Git`-Repository, __erforderlich__, wenn `uri` mit `git@` oder `ssh://` beginnt |
| `repos."host-key"`                 | `no`             | Der Hostschlüssel des Git-Repositoryservers. Er darf nicht das Algorithmuspräfix (abgedeckt durch `host-key-algorithm`) enthalten. |
| `repos."host-key-algorithm"`       | `no`             | Der Hostschlüsselalgorithmus sollte `ssh-dss`, `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384` oder `ecdsa-sha2-nistp521`sein. Nur __erforderlich__, wenn `host-key` vorhanden ist. |
| `repos."strict-host-key-checking"` | `no`             | Gibt an, ob der Konfigurationsserver nicht gestartet werden kann, wenn das private `host-key`-Element genutzt wird. Muss `true` (Standardwert) oder `false` sein. |

## <a name="attaching-your-config-server-repository-to-azure-spring-cloud"></a>Anfügen des Konfigurationsserverrepositorys an Azure Spring Cloud

Sie haben Ihre Konfigurationsdateien in einem Repository gespeichert und müssen nun eine Verbindung zwischen dem Repository und Azure Spring Cloud herstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

1. Navigieren Sie zur Seite **Übersicht** von Azure Spring Cloud.

1. Navigieren Sie im Menü auf der linken Seite unter der Überschrift **Einstellungen** zur Registerkarte **Konfigurationsserver**.

![Screenshot des Fensters](media/spring-cloud-tutorial-config-server/portal-config-server.png)

### <a name="input-repository-information-directly-to-the-azure-portal"></a>Direkte Eingabe von Repositoryinformationen im Azure-Portal

#### <a name="default-repository"></a>Standardrepository

* Öffentliches Repository: Fügen Sie im Abschnitt **Standardrepository** den Repository-URI in den Abschnitt **URI** ein.  Legen Sie die **Bezeichnung** auf `config` fest. Stellen Sie sicher, dass die Einstellung **Authentifizierung** den Wert **Public** aufweist, und wählen Sie dann **Anwenden** aus. 

* Privates Repository: Azure Spring Cloud unterstützt die grundlegende Authentifizierung per Kennwort/Token und SSH.

    * Standardauthentifizierung: Fügen Sie im Abschnitt **Standardrepository** den Repository-URI in den Abschnitt **URI** ein, und klicken Sie dann auf **Authentifizierung**. Wählen Sie **Basic** als **Authentifizierungstyp** aus, und geben Sie Ihren Benutzernamen und Ihr Kennwort/Token ein, um Zugriff auf Azure Spring Cloud zu gewähren. Klicken Sie auf **OK** und **Anwenden**, um die Einrichtung Ihres Konfigurationsservers abzuschließen.

    ![Screenshot des Fensters](media/spring-cloud-tutorial-config-server/basic-auth.png)
    
    > [!CAUTION]
    > Einige Git-Repositoryserver wie GitHub verwenden ein `personal-token` oder ein `access-token` wie ein Kennwort für die **Standardauthentifizierung**. Diese Art von Token kann als Kennwort in Azure Spring Cloud verwendet werden, da es nie abläuft. Für andere Git-Repositoryserver wie Bitbucket und Azure DevOps läuft das `access-token` in einer oder zwei Stunden ab. Dies bedeutet, dass sich diese Option nicht für die Verwendung dieser Repositoryserver mit Azure Spring Cloud eignet.

    * SSH: Fügen Sie im Abschnitt **Standardrepository** den Repository-URI in den Abschnitt **URI** ein, und klicken Sie dann auf **Authentifizierung**. Wählen Sie **SSH** als **Authentifizierungstyp** aus, und geben Sie Ihren **privaten Schlüssel** ein. Optional können Sie den **Hostschlüssel** und den **Hostschlüsselalgorithmus** angeben. Stellen Sie sicher, dass Sie Ihren öffentlichen Schlüssel in das Repository Ihres Konfigurationsservers einfügen. Klicken Sie auf **OK** und **Anwenden**, um die Einrichtung Ihres Konfigurationsservers abzuschließen.

    ![Screenshot des Fensters](media/spring-cloud-tutorial-config-server/ssh-auth.png)

#### <a name="pattern-repository"></a>Musterrepository

Wenn Sie ein optionales **Musterrepository** verwenden möchten, um den Dienst zu konfigurieren, geben Sie den **URI** und die **Authentifizierung** auf die gleiche Weise an wie beim **Standardrepository**. Geben Sie einen **Namen** für das Muster an, und klicken Sie dann auf **Anwenden**, um es an Ihre Instanz anzufügen. 

### <a name="enter-repository-information-into-a-yaml-file"></a>Eingeben der Repositoryinformationen in eine YAML-Datei

Wenn Sie eine YAML-Datei mit Ihren Repositoryeinstellungen erstellt haben, können Sie Ihre YAML-Datei direkt von Ihrem lokalen Computer in Azure Spring Cloud importieren. Eine einfache YAML-Datei für ein privates Repository mit Standardauthentifizierung würde wie folgt aussehen:

```yml
spring:
    cloud:
        config:
            server:
                git:
                    uri: https://github.com/azure-spring-cloud-samples/config-server-repository.git
                    username: <username>
                    password: <password/token>

```

Klicken Sie auf die Schaltfläche **Einstellungen importieren**, und wählen Sie dann die `.yml`-Datei in Ihrem Projektverzeichnis aus. Klicken Sie auf **Importieren**. Es wird ein `async`-Vorgang von Ihren **Benachrichtigungen** angezeigt. Nach 1 bis 2 Minuten sollte der erfolgreiche Abschluss gemeldet werden.

![Screenshot des Fensters](media/spring-cloud-tutorial-config-server/local-yml-success.png)


Die Informationen aus der YAML-Datei sollten im Azure-Portal angezeigt werden. Klicken Sie zum Abschließen auf **Anwenden**. 


## <a name="delete-your-app-configuration"></a>Löschen Ihrer App-Konfiguration

Wenn Sie Ihre Konfigurationsdatei gespeichert haben, wird auf der Registerkarte **Konfiguration** die Schaltfläche **Delete app configuration** (App-Konfiguration löschen) angezeigt. Dadurch werden die vorhandenen Einstellungen vollständig gelöscht. Führen Sie diesen Schritt aus, wenn Sie Ihren Konfigurationsserver mit einer anderen Quelle verbinden möchten, beispielsweise bei der Migration von GitHub zu Azure DevOps.



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie den Konfigurationsserver aktivieren und konfigurieren. Weitere Informationen zum Verwalten Ihrer Anwendung finden Sie im Tutorial zur manuellen Skalierung Ihrer App.

> [!div class="nextstepaction"]
> [Tutorial: Skalieren einer Anwendung in Azure Spring Cloud](spring-cloud-tutorial-scale-manual.md)

