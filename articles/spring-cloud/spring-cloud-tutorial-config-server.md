---
title: Einrichten eines Konfigurationsservers in Azure Spring Cloud | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie, wie Sie über das Azure-Portal einen Spring Cloud-Konfigurationsserver für Azure Spring Cloud einrichten.
services: spring-cloud
ms.service: spring-cloud
ms.topic: tutorial
ms.reviewer: jeconnoc
ms.author: v-vasuke
author: v-vasuke
ms.date: 08/08/2019
ms.openlocfilehash: f959d216859ded54de52de474ee3150f91bcfff1
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038652"
---
# <a name="tutorial-set-up-a-spring-cloud-config-server-for-your-service"></a>Tutorial: Einrichten eines Spring Cloud-Konfigurationsservers für Ihren Dienst

In diesem Tutorial wird gezeigt, wie Sie einen Spring Cloud-Konfigurationsserver mit Ihrem Azure Spring Cloud-Dienst verbinden.

Die Spring Cloud-Konfiguration bietet server- und clientseitige Unterstützung für eine externalisierte Konfiguration in einem verteilten System. Der Konfigurationsserver bietet Ihnen einen zentralen Ort zum Verwalten externer Eigenschaften für Anwendungen in allen Umgebungen. Weitere Informationen finden Sie in der [Referenz zum Spring Cloud-Konfigurationsserver](https://spring.io/projects/spring-cloud-config).

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 
* Ein bereits bereitgestellter und ausgeführter Azure Spring Cloud-Dienst.  Führen Sie [diese Schnellstartanleitung](spring-cloud-quickstart-launch-app-cli.md) aus, um einen Azure Spring Cloud-Dienst bereitzustellen und zu starten.


## <a name="restriction"></a>Einschränkung

Bei der Verwendung eines __Konfigurationsservers__ mit einem Git-Back-End gelten einige Einschränkungen. Einige Eigenschaften werden für den Zugriff auf den __Konfigurationsserver__ und die __Dienstermittlung__ automatisch in Ihre Anwendungsumgebung eingefügt. Wenn Sie diese Eigenschaften auch in den Dateien Ihres **Konfigurationsservers** konfigurieren, treten unter Umständen Konflikte oder unerwartetes Verhalten auf.  Zu den Eigenschaften zählen folgende: 

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

Azure Spring Cloud unterstützt Azure DevOps, GitHub, GitLab und Bitbucket zum Speichern Ihrer Konfigurationsserverdateien.  Wenn Sie Ihr Repository vorbereitet haben, erstellen Sie die Konfigurationsdateien anhand der folgenden Anweisungen, und speichern Sie sie dort.

Außerdem sind einige konfigurierbare Eigenschaften nur für einige Typen verfügbar. In den folgenden Unterabschnitten sind die Eigenschaften für die einzelnen Repositorytypen aufgeführt.


### <a name="public-repository"></a>Öffentliches Repository

Bei der Verwendung eines öffentlichen Repositorys sind die konfigurierbaren Eigenschaften stärker eingeschränkt.

Alle konfigurierbaren Eigenschaften, die zum Einrichten des öffentlichen `Git`-Repositorys verwendet werden, sind unten aufgeführt.

> [!NOTE]
> Derzeit wird nur die Namenskonvention mit Bindestrichen („-“) zum Trennen der Wörter unterstützt. Sie verwenden beispielsweise `default-label` und nicht `defaultLabel`.

| Eigenschaft        | Erforderlich | Feature                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | Das `uri`-Element des `Git`-Repositorys, das als Konfigurationsserver-Back-End verwendet wird, muss mit `http://`, `https://`, `git@` oder `ssh://` beginnen. |
| `default-label` | `no`     | Als Standardbezeichnung des `Git`-Repositorys muss `branch name`, `tag name` oder `commit-id` des Repositorys verwendet werden. |
| `search-paths`  | `no`     | Ein Array von Zeichenfolgen, die zum Durchsuchen von Unterverzeichnissen des `Git`-Repositorys verwendet werden |

------

### <a name="private-repository-with-ssh-authentication"></a>Privates Repository mit SSH-Authentifizierung

Alle konfigurierbaren Eigenschaften, die zum Einrichten des privaten `Git`-Repositorys mit `Ssh` verwendet werden, sind unten aufgeführt.

> [!NOTE]
> Derzeit wird nur die Namenskonvention mit Bindestrichen („-“) zum Trennen der Wörter unterstützt. Sie verwenden beispielsweise `default-label` und nicht `defaultLabel`.

| Eigenschaft                   | Erforderlich | Feature                                                      |
| :------------------------- | -------- | ------------------------------------------------------------ |
| `uri`                      | `yes`    | Das `uri`-Element des `Git`-Repositorys, das als Konfigurationsserver-Back-End verwendet wird, muss mit `http://`, `https://`, `git@` oder `ssh://` beginnen. |
| `default-label`            | `no`     | Als Standardbezeichnung des `Git`-Repositorys muss `branch name`, `tag name` oder `commit-id` des Repositorys verwendet werden. |
| `search-paths`             | `no`     | Ein Array von Zeichenfolgen, die zum Durchsuchen von Unterverzeichnissen des `Git`-Repositorys verwendet werden |
| `private-key`              | `no`     | Der private `Ssh`-Schlüssel für den Zugriff auf das `Git`-Repository, __erforderlich__, wenn `uri` mit `git@` oder `ssh://` beginnt |
| `host-key`                 | `no`     | Der Hostschlüssel des Git-Repositoryservers. Er darf nicht das Algorithmuspräfix (abgedeckt durch `host-key-algorithm`) enthalten. |
| `host-key-algorithm`       | `no`     | Der Hostschlüsselalgorithmus. Muss `ssh-dss`, `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384` oder `ecdsa-sha2-nistp521` sein. Nur erforderlich, wenn `host-key` vorhanden ist. |
| `strict-host-key-checking` | `no`     | Gibt an, ob der Konfigurationsserver nicht gestartet werden kann, wenn das angegebene `host-key`-Element genutzt wird. Muss `true` (Standardwert) oder `false` sein. |

-----

### <a name="private-repository-with-basic-authentication"></a>Privates Repository mit Standardauthentifizierung

Alle konfigurierbaren Eigenschaften, die zum Einrichten des privaten Git-Repositorys mit Standardauthentifizierung verwendet werden, sind unten aufgeführt.

> [!NOTE]
> Derzeit wird nur die Namenskonvention mit Bindestrichen („-“) zum Trennen der Wörter unterstützt. Sie können beispielsweise `default-label`, aber nicht `defaultLabel` verwenden.

| Eigenschaft        | Erforderlich | Feature                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | Das `uri`-Element des `Git`-Repositorys, das als Konfigurationsserver-Back-End verwendet wird, muss mit `http://`, `https://`, `git@` oder `ssh://` beginnen. |
| `default-label` | `no`     | Als Standardbezeichnung des `Git`-Repositorys muss `branch name`, `tag name` oder `commit-id` des Repositorys verwendet werden. |
| `search-paths`  | `no`     | Ein Array von Zeichenfolgen, die zum Durchsuchen von Unterverzeichnissen des `Git`-Repositorys verwendet werden |
| `username`      | `no`     | Das `username`-Element, das für den Zugriff auf den `Git`-Repositoryserver verwendet wird, __erforderlich__, wenn der `Git`-Repositoryserver `Http Basic Authentication` unterstützt |
| `password`      | `no`     | Das Kennwort, das für den Zugriff auf den `Git`-Repositoryserver verwendet wird, __erforderlich__, wenn der `Git`-Repositoryserver `Http Basic Authentication` unterstützt |

> [!NOTE]
> Einige `Git`-Repositoryserver wie GitHub unterstützen „personal-token“ oder „access-token“ als Kennwort für `HTTP Basic Authentication`. Sie können diese Art von Token auch hier als Kennwort verwenden. „personal-token“ oder „access-token“ läuft nicht ab. Bei Git-Repositoryservern wie BitBucket und Azure DevOps läuft das Token jedoch in ein bis zwei Stunden ab. Daher eignet sich dieses Token nicht für die Verwendung mit Azure Spring Cloud.

### <a name="git-repositories-with-pattern"></a>Git-Repositorys mit Muster

Alle konfigurierbaren Eigenschaften, die zum Einrichten von Repositorys mit Muster verwendet werden, sind unten aufgeführt.

> [!NOTE]
> Derzeit wird nur die Namenskonvention mit Bindestrichen („-“) zum Trennen der Wörter unterstützt. Sie können beispielsweise `default-label`, aber nicht `defaultLabel` verwenden.

| Eigenschaft                           | Erforderlich         | Feature                                                      |
| :--------------------------------- | ---------------- | ------------------------------------------------------------ |
| `repos`                            | `no`             | Eine Zuordnung besteht aus `Git`-Repositoryeinstellungen mit einem bestimmten Namen. |
| `repos."uri"`                      | `yes` für `repos` | Das `uri`-Element des `Git`-Repositorys, das als Konfigurationsserver-Back-End verwendet wird, muss mit `http://`, `https://`, `git@` oder `ssh://` beginnen. |
| `repos."name"`                     | `yes` für `repos` | Ein Name zum Identifizieren eines `Git`-Repositorys, nur __erforderlich__, wenn `repos` vorhanden ist. Beispielsweise von oben: `team-A`, `team-B` |
| `repos."pattern"`                  | `no`             | Ein Array von Zeichenfolgen, die zum Abgleichen des Anwendungsnamens verwendet werden. Verwenden Sie für jedes Muster das Format `{application}/{profile}` mit Platzhalterzeichen. |
| `repos."default-label"`            | `no`             | Als Standardbezeichnung des `Git`-Repositorys muss `branch name`, `tag name` oder `commit-id` des Repositorys verwendet werden. |
| `repos."search-paths`"             | `no`             | Ein Array von Zeichenfolgen, die zum Durchsuchen von Unterverzeichnissen des `Git`-Repositorys verwendet werden |
| `repos."username"`                 | `no`             | Das `username`-Element, das für den Zugriff auf den `Git`-Repositoryserver verwendet wird, __erforderlich__, wenn der `Git`-Repositoryserver `Http Basic Authentication` unterstützt |
| `repos."password"`                 | `no`             | Das Kennwort, das für den Zugriff auf den `Git`-Repositoryserver verwendet wird, __erforderlich__, wenn der `Git`-Repositoryserver `Http Basic Authentication` unterstützt |
| `repos."private-key"`              | `no`             | Der private `Ssh`-Schlüssel für den Zugriff auf das `Git`-Repository, __erforderlich__, wenn `uri` mit `git@` oder `ssh://` beginnt |
| `repos."host-key"`                 | `no`             | Der Hostschlüssel des Git-Repositoryservers. Er darf nicht das Algorithmuspräfix (abgedeckt durch `host-key-algorithm`) enthalten. |
| `repos."host-key-algorithm"`       | `no`             | Der Hostschlüsselalgorithmus. Muss `ssh-dss`, `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384` oder `ecdsa-sha2-nistp521` sein. Nur __erforderlich__, wenn `host-key` vorhanden ist. |
| `repos."strict-host-key-checking"` | `no`             | Gibt an, ob der Konfigurationsserver nicht gestartet werden kann, wenn das angegebene `host-key`-Element genutzt wird. Muss `true` (Standardwert) oder `false` sein. |

### <a name="import-applicationyml-file-from-spring-cloud-config"></a>Importieren der Datei `application.yml` aus der Spring Cloud-Konfiguration

Einige Standardeinstellungen für den Konfigurationsserver können direkt von der Website für die [Spring Cloud-Konfiguration](https://spring.io/projects/spring-cloud-config) importiert werden. Der Import kann direkt im Azure-Portal erfolgen, daher müssen Sie jetzt keine Schritte zum Vorbereiten der Konfigurationsserverdateien oder des Konfigurationsserverrepositorys ausführen.

## <a name="attaching-your-config-server-repository-to-azure-spring-cloud"></a>Anfügen des Konfigurationsserverrepositorys an Azure Spring Cloud

Sie haben Ihre Konfigurationsdateien in einem Repository gespeichert und müssen nun eine Verbindung zwischen dem Repository und Azure Spring Cloud herstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

1. Navigieren Sie zur Seite **Übersicht** von Azure Spring Cloud.

1. Navigieren Sie im Menü auf der linken Seite unter der Überschrift **Einstellungen** zur Registerkarte **Konfigurationsserver**.

### <a name="public-repository"></a>Öffentliches Repository

Ist Ihr Repository öffentlich, klicken Sie einfach auf die Schaltfläche **Öffentlich**, und fügen Sie die URL ein.

### <a name="private-repository"></a>Privates Repository

Azure Spring Cloud unterstützt die SSH-Authentifizierung. Befolgen Sie die Anweisungen im Azure-Portal, um Ihrem Repository den öffentlichen Schlüssel hinzuzufügen. Fügen Sie dann den privaten Schlüssel in die Konfigurationsdatei ein.

Klicken Sie auf **Anwenden**, um die Einrichtung Ihres Konfigurationsservers abzuschließen.


## <a name="delete-your-app-configuration"></a>Löschen Ihrer App-Konfiguration

Wenn Sie Ihre Konfigurationsdatei gespeichert haben, wird auf der Registerkarte **Konfiguration** die Schaltfläche **Delete app configuration** (App-Konfiguration löschen) angezeigt. Dadurch werden die vorhandenen Einstellungen vollständig gelöscht. Führen Sie diesen Schritt aus, wenn Sie Ihren Konfigurationsserver mit einer anderen Quelle verbinden möchten, beispielsweise bei der Migration von GitHub zu Azure DevOps.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie den Konfigurationsserver aktivieren und konfigurieren. Weitere Informationen zum Verwalten Ihrer Anwendung finden Sie im Tutorial zur manuellen Skalierung Ihrer App.

> [!div class="nextstepaction"]
> [Tutorial: Skalieren einer Anwendung in Azure Spring Cloud](spring-cloud-tutorial-scale-manual.md)

