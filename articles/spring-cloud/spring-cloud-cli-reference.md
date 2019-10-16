---
title: az spring-cloud
description: Verwalten von Azure Spring Cloud mit der Azure-Befehlszeilenschnittstelle
author: jpconnock
ms.service: spring-cloud
ms.topic: reference
ms.date: 10/03/2019
ms.author: jeconnoc
ms.openlocfilehash: c0694bf53f4a0644c8da2b50660dbfd6a5b339c7
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038709"
---
# <a name="az-spring-cloud"></a>az spring-cloud

## <a name="manage-azure-spring-cloud-using-the-azure-cli"></a>Verwalten von Azure Spring Cloud mit der Azure-Befehlszeilenschnittstelle

>[!Note]
> Azure Spring Cloud befindet sich derzeit in der Vorschauphase.  Diese Befehle können in einem zukünftigen Release geändert oder entfernt werden.

| az spring-cloud |  |
|------|------:|
| [az spring-cloud create](#az-spring-cloud-create) | Erstellt eine Azure Spring Cloud-Instanz |
| [az spring-cloud delete](#az-spring-cloud-delete) | Löscht eine Azure Spring Cloud-Instanz |
| [az spring-cloud list](#az-spring-cloud-list) | Listet alle Azure Spring Cloud-Instanzen in der angegebenen Ressourcengruppe auf, andernfalls werden die Abonnement-IDs aufgelistet |
| [az spring-cloud show](#az-spring-cloud-show) | Zeigt Details zu einer Azure Spring Cloud-Instanz an |

| az spring-cloud app | Befehle zum Verwalten von Apps in Azure Spring Cloud  |
| ---- | ----: |
| [az spring-cloud app create](#az-spring-cloud-app-create) | Erstellt eine neue App mit einer Standardbereitstellung in Azure Spring Cloud |
| [az spring-cloud app delete](#az-spring-cloud-app-delete) | Löscht eine App in Azure Spring Cloud |
| [az spring-cloud app deploy](#az-spring-cloud-app-deploy) | Stellt eine App aus dem Quellcode oder einer vorkonfigurierten Binärdatei bereit und aktualisiert die zugehörigen Konfigurationen |
| [az spring-cloud app list](#az-spring-cloud-app-list) | Listet alle Apps in Azure Spring Cloud auf |
| [az spring-cloud app restart](#az-spring-cloud-app-restart) | Startet Instanzen der App mit den Standardeinstellungen für die Produktionsbereitstellung neu |
| [az spring-cloud app scale](#az-spring-cloud-app-scale) | Skaliert eine App oder ihre Bereitstellungen manuell |
| [az spring-cloud app set-deployment](#az-spring-cloud-app-set-deployment) | Legt die Produktionsbereitstellung einer App fest |
| [az spring-cloud app show](#az-spring-cloud-app-show) | Zeigt Details einer App in Azure Spring Cloud an |
| [az spring-cloud app show-deploy-log](#az-spring-cloud-app-show-deploy-log) | Zeigt Buildprotokolle für die neueste Bereitstellung aus der Quelle an. Als Standard wird die Produktionsbereitstellung verwendet. |
| [az spring-cloud app start](#az-spring-cloud-app-start) | Startet Instanzen der App mit den Standardeinstellungen für die Produktionsbereitstellung |
| [az spring-cloud app stop](#az-spring-cloud-app-stop) | Hält Instanzen der App mit den Standardeinstellungen für die Produktionsbereitstellung an |
| [az spring-cloud app update](#az-spring-cloud-app-update) | Aktualisiert die Konfiguration der angegebenen App |

| az spring-cloud app binding | Befehle zum Verwalten von Bindungen mit Azure-Datendiensten.  Die App muss neu gestartet werden, damit diese Einstellungen wirksam werden. |
| --- | ---: |
| [az spring-cloud app binding list](#az-spring-cloud-app-binding-list) | Listet alle Dienstbindungen in einer App auf |
| [az spring-cloud app binding remove](#az-spring-cloud-app-binding-remove) | Entfernt eine Dienstbindung aus der App |
| [az spring-cloud app binding show](#az-spring-cloud-app-binding-show) | Zeigt Details einer Dienstbindung an |
| [az spring-cloud app binding cosmos add](#az-spring-cloud-app-binding-cosmos-add) | Bindet eine Azure Cosmos DB-Instanz an die App |
| [az spring-cloud app binding cosmos update](#az-spring-cloud-app-binding-cosmos-update) | Aktualisiert eine Azure Cosmos DB-Dienstbindung |
| [az spring-cloud app binding mysql add](#az-spring-cloud-app-binding-mysql-add) | Bindet eine Azure Database for MySQL-Instanz an die App |
| [az spring-cloud app binding mysql update](#az-spring-cloud-app-binding-mysql-update) | Aktualisiert eine Dienstbindung mit Azure Database for MySQL |
| [az spring-cloud app binding redis add](#az-spring-cloud-app-binding-redis-add) | Bindet eine Azure Cache for Redis-Instanz an die App |
| [az spring-cloud app binding redis update](#az-spring-cloud-app-binding-redis-update) | Aktualisiert eine Dienstbindung mit Azure Cache for Redis |

| az spring-cloud app deployment | Befehle zum Verwalten des Lebenszyklus von Bereitstellungen einer App in Azure Spring Cloud |
| --- | ---: |
| [az spring-cloud app deployment create](#az-spring-cloud-app-deployment-create) | Erstellt eine Stagingbereitstellung für die App |
| [az spring-cloud app deployment delete](#az-spring-cloud-app-deployment-delete) | Löscht eine Bereitstellung der App |
| [az spring-cloud app deployment list](#az-spring-cloud-app-deployment-list) | Listet alle Bereitstellungen in einer App auf |
| [az spring-cloud app deployment show](#az-spring-cloud-app-deployment-show) | Zeigt Details der Bereitstellung an |

| az spring-cloud config-server | Befehle zum Verwalten des Konfigurationsservers von Azure Spring Cloud |
| --- | ---: |
| [az spring-cloud config-server clear](#az-spring-cloud-config-server-clear) | Löscht alle Einstellungen auf dem Konfigurationsserver |
| [az spring-cloud config-server set](#az-spring-cloud-config-server-set) | Definiert den Konfigurationsserver aus einer YAML-Datei |
| [az spring-cloud config-server show](#az-spring-cloud-config-server-show) | Zeigt die Konfiguration des Konfigurationsservers an |
| [az spring-cloud config server git set](#az-spring-cloud-config-server-git-set) | Definiert Git-Eigenschaften für den Konfigurationsserver.  Ältere Werte werden überschrieben. |
| [az spring-cloud config server git repo add](#az-spring-cloud-config-server-git-repo-add) | Fügt dem Konfigurationsserver eine neue Git-Repositorykonfiguration hinzu |
| [az spring-cloud config server git repo list](#az-spring-cloud-config-server-git-repo-list) | Listet alle Git-Repositorykonfigurationen auf dem Konfigurationsserver auf |
| [az spring-cloud config server git repo remove](#az-spring-cloud-config-server-git-repo-remove) | Entfernt das angegebene Git-Repository vom Konfigurationsserver |

| az spring-cloud test-endpoint | Befehle zum Verwalten von Endpunkttests in Azure Spring Cloud |
| --- | ---: |
| [az spring-cloud test-endpoint disable](#az-spring-cloud-test-endpoint-disable) | Deaktiviert den Testendpunkt |
| [az spring-cloud test-endpoint enable](#az-spring-cloud-test-endpoint-enable) | Aktiviert den Testendpunkt |
| [az spring-cloud test-endpoint list](#az-spring-cloud-test-endpoint-list) | Listet die Endpunktschlüssel auf |
| [az spring-cloud test-endpoint renew-key](#az-spring-cloud-test-endpoint-renew-key) | Generiert einen Testendpunktschlüssel neu |

## <a name="az-spring-cloud-create"></a>az spring-cloud create

Erstellt eine neue App mit einer Standardbereitstellung in Azure Spring Cloud

```cli
az spring-cloud create  --name -n
                        --resource-group -g
                        --location -l
                        --no-wait
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name dieser Azure Spring Cloud-Instanz |
| --resource-group -g | Gibt die Ressourcengruppe für diese App an.  Sie konfigurieren die Standardgruppe mit `az configure --defaults group=<name>`. |

| Optionale Parameter | |
| --- | ---: |
| --location -l | Gibt den Serverstandort für diese App an.  Gültige Standorte suchen Sie mithilfe von `az account list-locations`. |
| --no-wait | Nicht auf den Abschluss zeitintensiver Vorgänge warten

### <a name="examples"></a>Beispiele

Erstellt eine neue Azure Spring Cloud-Instanz in der Region „USA, Westen“

```cli
az spring-cloud create -n MyService -g MyResourceGroup -l westus
```

## <a name="az-spring-cloud-delete"></a>az spring-cloud delete

Löscht eine Azure Spring Cloud-Instanz

```cli
az spring cloud --name -n
                --resource-group -g
                --no-wait
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der zu löschenden Azure Spring Cloud-Instanz |
| --resource-group -g | Name der Ressourcengruppe, zu der die Azure Spring Cloud-Instanz gehört |

| Optionale Parameter | |
| --- | ---: |
| -no-wait | Nicht auf den Abschluss zeitintensiver Vorgänge warten |

### <a name="example"></a>Beispiel

Löscht die Azure Spring Cloud-Instanz „MyService“ aus „MyResourceGroup“

```cli
az spring-cloud delete -n MyService -g MyResourceGroup
```

## <a name="az-spring-cloud-list"></a>az spring-cloud list

Listet alle Azure Spring Cloud-Instanzen auf, die der angegebenen Ressourcengruppe zugeordnet sind. Wenn keine Ressourcengruppe angegeben ist, werden die Abonnement-IDs aufgelistet.

```cli
az spring-cloud list --resource-group -g
```

| Erforderliche Parameter | |
| --- | ---: |
| --resource-group -g | Name der Ressourcengruppe |

## <a name="az-spring-cloud-show"></a>az spring-cloud show

Zeigt Details zur angegebenen Azure Spring Cloud-Instanz an

```cli
az spring-cloud show --name -n
                     -- resource-group -g
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der Azure Spring Cloud-Instanz |
| --resource-group -g | Name der Ressourcengruppe, zu der die Azure Spring Cloud-Instanz gehört

## <a name="az-spring-cloud-app-create"></a>az spring-cloud app create

Erstellt eine neue App in einer Azure Spring Cloud-Instanz

```cli
az spring-cloud app create --name -n
                            --resource-group -g
                            --service -s
                            --cpu
                            --enable-persistent-storage
                            --instance-count
                            --is-public
                            --memory
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --cpu | Anzahl virtueller Kerne pro Instanz  Standardwert: 1. |
| --enable-persistent-storage | Boolescher Wert.  Bei TRUE wird ein 50-GB-Datenträger mit dem Standardpfad eingebunden. |
| --instance-count | Anzahl der Instanzen  Standardwert: 1. |
| --is-public | Boolescher Wert.  Bei TRUE wird eine öffentliche Domäne zugewiesen. |
| --memory | GB an Arbeitsspeicher pro Instanz  Standardwert: 1. |

### <a name="examples"></a>Beispiele

Erstellt eine App mit der Standardkonfiguration.

```cli
az spring-cloud app create -n MyApp -s MyService
```

Erstellt eine öffentlich zugängliche App mit drei Instanzen.  Jede Instanz verfügt über 3 GB Arbeitsspeicher und 2 CPU-Kerne.

```cli
az spring-cloud app create -n MyApp -s MyService --is-public true --cpu 2 --memory 3
```

## <a name="az-spring-cloud-app-delete"></a>az spring-cloud app delete

Löscht eine App in Azure Spring Cloud

```cli
az spring cloud app delete  --name -n
                            --resource-group -g
                            --service -s
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

## <a name="az-spring-cloud-app-deploy"></a>az spring-cloud app deploy

Stellt eine App aus dem Quellcode oder einer vorkonfigurierten Binärdatei für eine App in Azure Spring Cloud bereit und aktualisiert die zugehörigen Konfigurationen

```cli
az spring cloud app deploy  --name -n
                            --resource-group -g
                            --service -s
                            --cpu
                            --deployment -d
                            --env
                            --instance-count
                            --jar-path
                            --jvm-options
                            --memory
                            --no-wait
                            --runtime-version
                            --target-module
                            --version
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --cpu | Anzahl virtueller CPU-Kerne pro Instanz |
| --deployment -d | Name einer vorhandenen App-Bereitstellung.  Wenn keine Angabe erfolgt, wird standardmäßig die Produktionsbereitstellung verwendet. |
| --env | Durch Leerzeichen getrennte Umgebungsvariablen im Format „Schlüssel[=Wert]“ |
| --instance-count | Anzahl von Instanzen |
| --jar-path | Bei Verwendung dieses Parameters wird eine JAR-Datei im angegebenen Pfad bereitgestellt. Andernfalls wird der aktuelle Ordner als TAR-Datei bereitgestellt. |
| --jvm-options | Eine Zeichenfolge mit JVM-Optionen.  Verwenden Sie „=“ anstelle von „ “, um Parserfehler der Shell zu vermeiden. Beispiel: `--jvm-options='-Xms1024m -Xmx2048m`. |
| --memory | GB an Arbeitsspeicher pro Instanz |
| --no-wait | Nicht auf den Abschluss zeitintensiver Vorgänge warten |
| --runtime-version | Runtimeversion der in der App verwendeten Sprache.  Zulässige Werte: `Java_11`, `Java_8`. |
| --target-module | Untergeordnetes Modul, das bereitgestellt werden soll.  Erforderlich, wenn mehrere JAR-Pakete aus dem Quellcode erstellt werden. |
| --version | Bereitstellungsversion.  Bleibt unverändert, wenn keine Festlegung erfolgt. |

### <a name="examples"></a>Beispiele

Stellen Sie Quellcode für eine App bereit. Dadurch wird das aktuelle Verzeichnis gepackt, eine Binärdatei mit dem Pivotal Build Service erstellt und anschließend für die App bereitgestellt.

```cli
az spring-cloud app deploy -n MyApp -s MyService
```

Stellen Sie mithilfe von JVM-Optionen und Umgebungsvariablen eine vorkonfigurierte JAR-Datei für eine App bereit.

```cli
az spring-cloud app deploy -n MyApp -s MyService --jar-path app.jar --jvm-options="-XX:+UseG1GC -XX:+UseStringDeduplication" --env foo=bar
```

Stellen Sie Quellcode für eine bestimmte Bereitstellung einer App bereit.

```cli
az spring-cloud app deploy -n MyApp -s Myspring-cloud -d green-deployment
```

## <a name="az-spring-cloud-app-list"></a>az spring-cloud app list

Listet alle Apps in der Azure Spring Cloud-Instanz auf

```cli
az spring-cloud app list --resource-group -g
                         --service -s
```

|Erforderliche Parameter | |
| --- | ---: |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

## <a name="az-spring-cloud-app-restart"></a>az spring-cloud app restart

Startet Instanzen der App neu.  Als Standard wird die Produktionsbereitstellung verwendet.

```cli
az spring-cloud app restart --name -n
                            --resource-group -g
                            --service -s
                            --deployment -d
                            --no-wait
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --deployment -d | Name der vorhandenen Bereitstellung der App.  Wenn keine Angabe erfolgt, wird standardmäßig die Produktionsbereitstellung verwendet. |
| --no-wait | Nicht auf den Abschluss zeitintensiver Vorgänge warten |

## <a name="az-spring-cloud-app-scale"></a>az spring-cloud app scale

Skaliert eine App oder ihre Bereitstellungen manuell

```cli
az spring-cloud app scale --name -n
                          --resource-group -g
                          --service -s
                          --cpu
                          --deployment -d
                          --instance-count
                          --memory
                          --no-wait
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --cpu | Anzahl virtueller CPU-Kerne pro App-Instanz |
| --deployment -d | Name der vorhandenen Bereitstellung der App.  Wenn keine Angabe erfolgt, wird standardmäßig die Produktionsbereitstellung verwendet. |
| --instance-count | Anzahl der Instanzen dieser App |
| --memory | GB an Arbeitsspeicher pro App-Instanz |
| --no-wait | Wartet nicht auf den Abschluss zeitintensiver Vorgänge |

### <a name="examples"></a>Beispiele

Skalieren Sie eine App zentral auf 4 CPU-Kerne und 8 GB Arbeitsspeicher pro Instanz hoch.

```cli
az spring-cloud app scale -n MyApp -s MyService --cpu 3 --memory 8
```

Erweitern Sie eine Bereitstellung der App auf fünf Instanzen.

```cli
az spring-cloud app scale -n MyApp -s MyService -d green-deployment --instance-count 5
```

## <a name="az-spring-cloud-app-set-deployment"></a>az spring-cloud app set-deployment

Legt die Konfigurationsoptionen für die Produktionsbereitstellung der App fest

```cli
az spring-cloud app set-deployment --deployment -d
                                   --name -n
                                   --resource-group -g
                                   --service -s
                                   --no-wait
```

| Erforderliche Parameter | |
| --- | ---: |
| --deployment -d | Name einer vorhandenen Bereitstellung der App |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --no-wait | Wartet nicht auf den Abschluss zeitintensiver Vorgänge |

### <a name="examples"></a>Beispiele

Tauschen Sie eine Stagingbereitstellung der App in der Produktion aus.

```cli
az spring-cloud app set-deployment -d green-deployment -n MyApp -s MyService
```

## <a name="az-spring-cloud-app-show"></a>az spring-cloud app show

Zeigt Details einer App in Azure Spring Cloud an

```cli
az spring-cloud app show --name -n
                         --resource-group -g
                         --service -s
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

## <a name="az-spring-cloud-app-show-deploy-log"></a>az spring-cloud app show-deploy-log

Zeigt das Buildprotokoll der letzten Bereitstellung aus dem Quellcode an.  Als Standard wird die Produktionsumgebung verwendet.

```cli
az spring-cloud app show-deploy-log --name -n
                                    --resource-group -g
                                    --service -s
                                    --deployment -d
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --deployment -d | Name einer vorhandenen Bereitstellung der App  Als Standard wird die Produktionsumgebung verwendet. |

## <a name="az-spring-cloud-app-start"></a>az spring-cloud app start

Startet Instanzen der App.  Als Standard wird die Produktionsumgebung verwendet.

```cli
az spring-cloud app start --name -n
                          --resource-group -g
                          --service -s
                          --deployment -d
                          --no-wait
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --deployment -d | Name einer vorhandenen Bereitstellung der App  Als Standard wird die Produktionsumgebung verwendet. |
| --no-wait | Wartet nicht auf den Abschluss zeitintensiver Vorgänge |

## <a name="az-spring-cloud-app-stop"></a>az spring-cloud app stop

Hält Instanzen der App an.  Als Standard wird die Produktionsumgebung verwendet.

```cli
az spring-cloud app stop --name -n
                         --resource-group -g
                         --service -s
                         --deployment -d
                         --no-wait
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --deployment -d | Name einer vorhandenen Bereitstellung der App  Als Standard wird die Produktionsumgebung verwendet. |
| --no-wait | Wartet nicht auf den Abschluss zeitintensiver Vorgänge |

## <a name="az-spring-cloud-app-update"></a>az spring-cloud app update

Aktualisiert die gespeicherte Konfiguration einer App

```cli
az spring-cloud app update --name -n
                           --resource-group -g
                           --service -s
                           --deployment -d
                           --enable-persistent-storage
                           --env
                           --is-public
                           --jvm-options
                           --no-wait
                           --runtime-version
```

| Erforderliche Parameter | |
| --- | ---: |
| --name -n | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --deployment -d | Name einer vorhandenen Bereitstellung der App  Als Standard wird die Produktionsumgebung verwendet. |
| --enable-persistent-storage | Boolesch.  Bei TRUE wird ein 50-GB-Datenträger mit dem Standardpfad eingebunden. |
| --env | Durch Leerzeichen getrennte Umgebungsvariablen im Format „Schlüssel[=Wert]“ |
| --is-public | Boolesch.  Bei TRUE wird der App eine öffentliche Domäne zugewiesen |
| --jvm-options | Eine Zeichenfolge mit JVM-Optionen.  Verwenden Sie „=“ anstelle von „ “, um Parserfehler der Shell zu vermeiden. Beispiel: `--jvm-options='-Xms1024m -Xmx2048m`. |
| --no-wait | Wartet nicht auf den Abschluss zeitintensiver Vorgänge |
| --runtime-version | Runtimeversion der in der App verwendeten Sprache.  Zulässige Werte: `Java_11`, `Java_8`. |

### <a name="example"></a>Beispiel

Fügt der App eine Umgebungsvariable hinzu

```cli
az spring-cloud app update --env foo=bar
```

## <a name="az-spring-cloud-app-binding-list"></a>az spring-cloud app binding list

Listet alle Dienstbindungen in einer App auf

```cli
az spring-cloud app binding list --app
                                 --resource-group -g
                                 --service -s
```

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

## <a name="az-spring-cloud-app-binding-remove"></a>az spring-cloud app binding remove

Entfernt eine Dienstbindung aus der App

```cli
az spring-cloud app binding list --app
                                 --name -n
                                 --resource-group -g
                                 --service -s
```

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --name | Name der zu entfernenden Dienstbindung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

## <a name="az-spring-cloud-app-binding-show"></a>az spring-cloud app binding show

Zeigt Details einer Dienstbindung an

```cli
az spring-cloud app binding show --app
                                 --name -n
                                 --resource-group -g
                                 --service -s
```

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --name | Name der Dienstbindung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

## <a name="az-spring-cloud-app-binding-cosmos-add"></a>az spring-cloud app binding cosmos add

Bindet eine Azure Cosmos DB-Instanz an die App

```cli
az spring-cloud app binding cosmos add --api-type
                                       --app
                                       --name -n
                                       --resource-group -g
                                       --resource-id
                                       --service -s
                                       --collection-name
                                       --database-name
                                       --key-space
```

```cli
az spring-cloud app binding list --app
                                 --name -n
                                 --resource-group -g
                                 --service -s
```

| Erforderliche Parameter | |
| --- | ---: |
| --api-type | Gibt den API-Typ mit einem der folgenden Werte an: cassandra, gremlin, mongo, sql, table. |
| --app | Name der App |
| --name | Name der Dienstbindung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

|Optionale Parameter | |
| --- | ---: |
| --collection-name | Name der Sammlung  Erforderlich bei Verwendung von Gremlin. |
| --database-name | Der Name der Datenbank.  Erforderlich bei Verwendung von Mongo, SQL und Gremlin. |
| --key-space | Cassandra-Schlüsselbereich.  Erforderlich bei Verwendung von Cassandra. |

## <a name="az-spring-cloud-app-binding-cosmos-update"></a>az spring-cloud app binding cosmos update

```cli
az spring-cloud app binding cosmos update --app
                                          --name -n
                                          --resource-group -g
                                          --service -s
                                          --collection-name
                                          --database-name
                                          --key-space
```

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --name | Name der Dienstbindung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

|Optionale Parameter | |
| --- | ---: |
| --collection-name | Name der Sammlung  Erforderlich bei Verwendung von Gremlin. |
| --database-name | Der Name der Datenbank.  Erforderlich bei Verwendung von Mongo, SQL und Gremlin. |
| --key-space | Cassandra-Schlüsselbereich.  Erforderlich bei Verwendung von Cassandra. |

## <a name="az-spring-cloud-app-binding-mysql-add"></a>az spring-cloud app binding mysql add

```cli
az spring-cloud app binding mysql add --app
                                      --database-name
                                      --key
                                      --name -n
                                      --resource-group -g
                                      --service -s
                                      --username
```

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --database-name | Der Name der Datenbank. |
| --key | API-Schlüssel des Diensts |
| --name | Name der Dienstbindung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --resource-id | Azure-Ressourcen-ID des Diensts für die Bindung |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |
| --username | Benutzername für den Datenbankzugriff |

## <a name="az-spring-cloud-app-binding-mysql-update"></a>az spring-cloud app binding mysql update

Aktualisiert die Verbindung der Dienstbindung der App mit einer Azure Database for MySQL-Instanz

```cli
az spring-cloud app binding mysql update --add
                                         --name -n
                                         --resource-group -g
                                         --service -s
                                         --database-name
                                         --key
                                         --username
```

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --name | Name der Dienstbindung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --database-name | Der Name der Datenbank. |
| --key | API-Schlüssel des Diensts |
| --username | Benutzername für den Datenbankzugriff |

## <a name="az-spring-cloud-app-binding-redis-add"></a>az spring-cloud app binding redis add

Bindet eine Azure Cache for Redis-Instanz an die App

```cli
az spring-cloud app binding redis add --app
                                      --name -n
                                      --resource-group -g
                                      --resource-id
                                      --service -s
                                      --disable-ssl
```

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --name | Name der Dienstbindung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --resource-id | Azure-Ressourcen-ID des Diensts für die Bindung |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --disable-ssl | Deaktiviert SSL |

## <a name="az-spring-cloud-app-binding-redis-update"></a>az spring-cloud app binding redis update

Aktualisiert eine Dienstbindung für Azure Cache for Redis

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --name | Name der Dienstbindung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --disable-ssl | Deaktiviert SSL |

## <a name="az-spring-cloud-app-deployment-create"></a>az spring-cloud app deployment create

Erstellt eine Stagingbereitstellung für die App

Für das Bereitstellen von Code oder das Aktualisieren von Einstellungen einer vorhandenen Bereitstellung verwenden Sie `az spring-cloud app deploy --deployment <staging-deployment>` oder „az spring-cloud app update --deployment <staging deployment>“.

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --name | Name der Dienstbindung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --cpu | Anzahl virtueller CPU-Kerne pro Instanz  Standardwert: 1 |
| --env | Durch Leerzeichen getrennte Umgebungsvariablen im Format „Schlüssel[=Wert]“ |
| --instance-count | Anzahl von Instanzen Standardwert: 1. |
| --jar-path | Bei Verwendung dieses Parameters wird eine JAR-Datei bereitgestellt.  Andernfalls wird der aktuelle Ordner als TAR-Datei bereitgestellt. |
| --jvm-options | Eine Zeichenfolge mit JVM-Optionen.  Verwenden Sie „=“ anstelle von „ “, um Parserfehler der Shell zu vermeiden. Beispiel: `--jvm-options='-Xms1024m -Xmx2048m`. |
| --memory | GB an Arbeitsspeicher pro Instanz |
| --no-wait | Nicht auf den Abschluss zeitintensiver Vorgänge warten |
| --runtime-version | Runtimeversion der in der App verwendeten Sprache.  Zulässige Werte: `Java_11`, `Java_8`. |
| --skip-clone-settings | Erstellt eine Stagingbereitstellung durch Klonen der Einstellungen der aktuellen Produktionsbereitstellung |
| --target-module | Untergeordnetes Modul, das bereitgestellt werden soll.  Erforderlich, wenn mehrere JAR-Pakete aus dem Quellcode erstellt werden. |
| --version | Bereitstellungsversion.  Bleibt unverändert, wenn keine Festlegung erfolgt. |

### <a name="examples"></a>Beispiele

Stellt Quellcode für eine neue Bereitstellung der App bereit.  Dadurch wird das aktuelle Verzeichnis gepackt, eine Binärdatei mit dem Pivotal Build System erstellt und anschließend bereitgestellt.

```cli
az spring-cloud app deployment create -n green-deployment --app MyApp -s MyService
```

Stellen Sie mithilfe von JVM-Optionen und Umgebungsvariablen eine vorkonfigurierte JAR-Datei für eine App bereit.

```cli
az spring-cloud app deployment create -n green-deployment --app MyApp -s MyService --jar-path app.jar --jvm-options="-XX:+UseStringDeDuplication" --env foo=bar
```

## <a name="az-spring-cloud-app-deployment-delete"></a>az spring-cloud app deployment delete

Löscht eine Bereitstellung der App

```cli
az spring-cloud app deployment delete --app
                                      --name -n
                                      --resource-group -g
                                      --service -s
```

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --name | Name der Bereitstellung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

## <a name="az-spring-cloud-app-deployment-list"></a>az spring-cloud app deployment list

Listet alle Bereitstellungen in einer App auf

```cli
az spring-cloud app deployment list --app
                                    --resource-group -g
                                    --service -s
```

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

## <a name="az-spring-cloud-app-deployment-show"></a>az spring-cloud app deployment show

Zeigt Details einer Bereitstellung an

```cli
az spring-cloud app deployment show --app
                                    --name -n
                                    --resource-group -g
                                    --service -s
```

| Erforderliche Parameter | |
| --- | ---: |
| --app | Name der App |
| --name | Name der Bereitstellung |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --service -s | Name der Azure Spring Cloud-Instanz.  Sie können den Standarddienst mit `az configure --defaults spring-cloud=<name>` konfigurieren. |

## <a name="az-spring-cloud-config-server-clear"></a>az spring-cloud config-server clear

Löscht alle Konfigurationseinstellungen auf dem Konfigurationsserver

```cli
az spring-cloud config-server clear --name
                                        --resource-group -g
```

| Erforderliche Parameter | |
| --- | ---: |
| --name | Name der Azure Spring Cloud-Instanz. |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |

## <a name="az-spring-cloud-config-server-set"></a>az spring-cloud config-server set

Legt die Konfigurationseinstellungen auf dem Konfigurationsserver mithilfe einer YAML-Datei fest

```cli
az spring-cloud config-server set --config-file
                                  --name -n
                                  --resource-group -g
                                  --no-wait
```

| Erforderliche Parameter | |
| --- | ---: |
| --config-file | Dateipfad zu einem YAML-Manifest für die Konfiguration des Konfigurationsservers |
| --name | Name der Azure Spring Cloud-Instanz. |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --no-wait | Nicht auf den Abschluss zeitintensiver Vorgänge warten

## <a name="az-spring-cloud-config-server-show"></a>az spring-cloud config-server show

Zeigt die Einstellungen des Konfigurationsservers an

```cli
az spring-cloud config-server show --name -n
                                   --resource-group -g
```

| Erforderliche Parameter | |
| --- | ---: |
| --name | Name der Azure Spring Cloud-Instanz. |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |

## <a name="az-spring-cloud-config-server-git-set"></a>az spring-cloud config-server git set

Legt die Git-Eigenschaften für den Konfigurationsserver fest.  Dadurch werden alle vorhandenen Git-Eigenschaften überschrieben.

```cli
az spring-cloud config-server git set --name -n
                                      --resource-group g
                                      --uri
                                      --defer
                                      --host-key
                                      --host-key-algorithm
                                      --label
                                      --password
                                      --private-key
                                      --search-paths
                                      --strict-host-key-checking
                                      --username
```

| Erforderliche Parameter | |
| --- | ---: |
| --name | Name der Azure Spring Cloud-Instanz. |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --uri | URI der hinzugefügten Konfiguration |

| Optionale Parameter | |
| --- | ---: |
| --defer | Speichert das Objekt temporär im lokalen Cache, anstatt es an Azure zu senden.  Verwenden Sie zum Anzeigen/Löschen `az cache`. |
| --host-key | Hostschlüssel für die hinzugefügte Konfiguration |
| --host-key-algorithm | Hostschlüsselalgorithmus für die hinzugefügte Konfiguration |
| --label | Bezeichnung der hinzugefügten Konfiguration |
| --password | Kennwort der hinzugefügten Konfiguration |
| --private-key | Privater Schlüssel der hinzugefügten Konfiguration |
| --search-paths | Suchpfade der hinzugefügten Konfiguration.  Verwenden Sie bei mehreren Pfaden Kommas als Trennzeichen. |
| --strict-host-key-checking | Aktiviert die strenge Überprüfung der Hostschlüssel für die hinzugefügte Konfiguration |
| --username | Benutzername der hinzugefügten Konfiguration |

## <a name="az-spring-cloud-config-server-git-repo-add"></a>az spring-cloud config-server git repo add

```cli
az spring-cloud config-server git repo add --name -n
                                           --repo-name
                                           --resource-group -g
                                           --uri
                                           --defer
                                           --host-key
                                           --host-key-algorithm
                                           --label
                                           --password
                                           --pattern
                                           --private-key
                                           --search-paths
                                           --strict-host-key-checking
                                           --username
```

| Erforderliche Parameter | |
| --- | ---: |
| --name | Name der Azure Spring Cloud-Instanz. |
| --repo-name | URI des Repositorys |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --uri | URI der hinzugefügten Konfiguration |

| Optionale Parameter | |
| --- | ---: |
| --defer | Speichert das Objekt temporär im lokalen Cache, anstatt es an Azure zu senden.  Verwenden Sie zum Anzeigen/Löschen `az cache`. |
| --host-key | Hostschlüssel für die hinzugefügte Konfiguration |
| --host-key-algorithm | Hostschlüsselalgorithmus für die hinzugefügte Konfiguration |
| --label | Bezeichnung der hinzugefügten Konfiguration |
| --password | Kennwort der hinzugefügten Konfiguration |
| --pattern | Muster für das Repository.  Verwenden Sie bei mehreren Pfaden Kommas als Trennzeichen.|
| --private-key | Privater Schlüssel der hinzugefügten Konfiguration |
| --search-paths | Suchpfade der hinzugefügten Konfiguration.  Verwenden Sie bei mehreren Pfaden Kommas als Trennzeichen. |
| --strict-host-key-checking | Aktiviert die strenge Überprüfung der Hostschlüssel für die hinzugefügte Konfiguration |
| --username | Benutzername der hinzugefügten Konfiguration |

## <a name="az-spring-cloud-config-server-git-repo-list"></a>az spring-cloud config-server git repo list

Listet alle Git-Repositorys auf, die auf dem Konfigurationsserver definiert sind

```cli
az spring-cloud config-server git repo list --name -n
                                       --resource-group -g
                                       --defer
```

| Erforderliche Parameter | |
| --- | ---: |
| --name | Name der Azure Spring Cloud-Instanz. |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --defer | Speichert das Objekt temporär im lokalen Cache, anstatt es an Azure zu senden.  Verwenden Sie zum Anzeigen/Löschen `az cache`. |

## <a name="az-spring-cloud-config-server-git-repo-remove"></a>az spring-cloud config-server git repo remove

Entfernt eine vorhandene Git-Repositorykonfiguration vom Konfigurationsserver

```cli
az spring-cloud config-server git repo remove --name -n
                                         --repo-name
                                         --resource-group -g
                                         --defer
```

| Erforderliche Parameter | |
| --- | ---: |
| --name | Name der Azure Spring Cloud-Instanz. |
| --repo-name | URI des Repositorys |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --defer | Speichert das Objekt temporär im lokalen Cache, anstatt es an Azure zu senden.  Verwenden Sie zum Anzeigen/Löschen `az cache`. |

## <a name="az-spring-cloud-test-endpoint-disable"></a>az spring-cloud test-endpoint disable

Deaktiviert den Testendpunkt von Azure Spring Cloud

```cli
az spring-cloud test-endpoint disable --name -n
                                      --resource-group -g
```

| Erforderliche Parameter | |
| --- | ---: |
| --name | Name der Azure Spring Cloud-Instanz. |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |

## <a name="az-spring-cloud-test-endpoint-enable"></a>az spring-cloud test-endpoint enable

Aktiviert den Testendpunkt für Azure Spring Cloud 

```cli 
az spring-cloud test-endpoint enable --name -n
                                     --resource-group -g
```

| Erforderliche Parameter | |
| --- | ---: |
| --name | Name der Azure Spring Cloud-Instanz. |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |

## <a name="az-spring-cloud-test-endpoint-list"></a>az spring-cloud test-endpoint list 

Listet die verfügbaren Testendpunktschlüssel für Azure Spring Cloud auf

```cli
az spring-cloud test-endpoint list --name -n
                                   --resource-group -g
                                   --app
                                   --deployment -d
```

| Erforderliche Parameter | |
| --- | ---: |
| --name | Name der Azure Spring Cloud-Instanz. |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |

| Optionale Parameter | |
| --- | ---: |
| --app | Name der App |
| --deployment -d | Name einer vorhandenen Bereitstellung der App  Wenn keine Angabe erfolgt, wird als Standard die Produktionsbereitstellung verwendet. |

## <a name="az-spring-cloud-test-endpoint-renew-key"></a>az spring-cloud test-endpoint renew-key

Generiert einen Testendpunktschlüssel für Azure Spring Cloud neu

```cli
az spring-cloud test-endpoint renew-key --name -n
                                        --resource-group -g
                                        --type
```

| Erforderliche Parameter | |
| --- | ---: |
| --name | Name der Azure Spring Cloud-Instanz. |
| --resource-group -g | Name der Ressourcengruppe  Sie können die Standardgruppe mit `az configure --defaults group=<name>` konfigurieren. |
| --type | Typ des Testendpunktschlüssels  Zulässige Werte:  Primary, Secondary |
