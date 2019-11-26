---
title: Azure Service Fabric-CLI – sfctl sa-cluster | Microsoft-Dokumentation
description: Beschreibt die sfctl-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI) für eigenständige Cluster.
services: service-fabric
documentationcenter: na
author: jeffj6123
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: ecdd288d7cb320b91ab4c69697d334f8d9459e62
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901020"
---
# <a name="sfctl-sa-cluster"></a>sfctl sa-cluster
Verwaltet eigenständige Service Fabric-Cluster.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| config | Ruft die Konfiguration eines eigenständigen Service Fabric-Clusters ab. |
| config-upgrade | Startet das Upgrade der Konfiguration eines eigenständigen Service Fabric-Clusters. |
| upgrade-status | Ruft den Upgradestatus der Clusterkonfiguration eines eigenständigen Service Fabric-Clusters ab. |

## <a name="sfctl-sa-cluster-config"></a>sfctl sa-cluster config
Ruft die Konfiguration eines eigenständigen Service Fabric-Clusters ab.

Die Clusterkonfiguration enthält Eigenschaften des Clusters wie z.B. die unterschiedlichen Knotentypen im Cluster, Sicherheitskonfigurationen, Fehler- und Upgradedomänentopologien usw.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --configuration-api-version [erforderlich] | Die API-Version der JSON-Konfiguration des eigenständigen Clusters. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-sa-cluster-config-upgrade"></a>sfctl sa-cluster config-upgrade
Startet das Upgrade der Konfiguration eines eigenständigen Service Fabric-Clusters.

Überprüft die bereitgestellten Parameter für das Konfigurationsupgrade und startet das Upgrade der Clusterkonfiguration, wenn die Parameter gültig sind.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --cluster-config            [erforderlich] | Die Clusterkonfiguration. |
| --application-health-policies | JSON-codiertes Wörterbuch von Paaren aus Anwendungstyp und dem Höchstprozentsatz an Integritätseinbußen vor der Auslösung eines Fehlers |
| --delta-unhealthy-nodes | Der während des Upgrades maximal zulässige Prozentsatz der Integritätsminderung (Delta). Zulässige Werte sind ganze Zahlen von 0 bis 100. |
| --health-check-retry | Die Zeitspanne zwischen den Versuchen, Integritätsprüfungen durchzuführen, wenn die Anwendung oder der Cluster nicht fehlerfrei ist.  Standardwert\: PT0H0M0S. |
| --health-check-stable | Die Zeitspanne, während der die Anwendung oder der Cluster fehlerfrei bleiben muss, bevor das Upgrade mit der nächsten Upgradedomäne fortgesetzt wird.  Standardwert\: PT0H0M0S. <br><br> Wird zuerst als Zeichenfolge interpretiert, die für eine ISO 8601-Dauer steht. Wenn dies nicht erfolgreich ist, erfolgt die Interpretation als Zahl, die für die Gesamtzahl an Millisekunden steht. |
| --health-check-wait | Die Zeitspanne, während der nach dem Abschließen einer Upgradedomäne gewartet werden soll, bevor der Integritätsprüfungsprozess gestartet wird.  Standardwert\: PT0H0M0S. |
| --timeout -t | Standardwert\: 60. |
| --unhealthy-applications | Der maximal zulässige Prozentsatz von Anwendungen mit Fehlern während des Upgrades. Zulässige Werte sind ganze Zahlen von 0 bis 100. |
| --unhealthy-nodes | Der maximal zulässige Prozentsatz von Knoten mit Fehlern während des Upgrades. Zulässige Werte sind ganze Zahlen von 0 bis 100. |
| --upgrade-domain-delta-unhealthy-nodes | Der während des Upgrades maximal zulässige Prozentsatz der Integritätsminderung in der Upgradedomäne (Delta). Zulässige Werte sind ganze Zahlen von 0 bis 100. |
| --upgrade-domain-timeout | Die Zeitspanne, während der jede Upgradedomäne abgeschlossen werden muss, bevor die Fehleraktion (FailureAction) ausgeführt wird.  Standardwert\: PT0H0M0S. <br><br> Wird zuerst als Zeichenfolge interpretiert, die für eine ISO 8601-Dauer steht. Wenn dies nicht erfolgreich ist, erfolgt die Interpretation als Zahl, die für die Gesamtzahl an Millisekunden steht. |
| --upgrade-timeout | Die Zeitspanne, während der das gesamte Upgrade abgeschlossen werden muss, bevor die Fehleraktion (FailureAction) ausgeführt wird.  Standardwert\: PT0H0M0S. <br><br> Wird zuerst als Zeichenfolge interpretiert, die für eine ISO 8601-Dauer steht. Wenn dies nicht erfolgreich ist, erfolgt die Interpretation als Zahl, die für die Gesamtzahl an Millisekunden steht. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

### <a name="examples"></a>Beispiele

Startet eine Aktualisierung der Clusterkonfiguration.
``` 
sfctl sa-cluster config-upgrade --cluster-config <YOUR CLUSTER CONFIG> --application-health-    
policies "{"fabric:/System":{"ConsiderWarningAsError":true}}"   
```

## <a name="sfctl-sa-cluster-upgrade-status"></a>sfctl sa-cluster upgrade-status
Ruft den Upgradestatus der Clusterkonfiguration eines eigenständigen Service Fabric-Clusters ab.

Ruft die Details zum Upgradestatus der Clusterkonfiguration eines eigenständigen Service Fabric-Clusters ab.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |


## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)