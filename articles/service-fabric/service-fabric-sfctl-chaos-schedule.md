---
title: Azure Service Fabric-CLI – sfctl chaos schedule | Microsoft-Dokumentation
description: Beschreibt die sfctl chaos-Befehle der Service Fabric-CLI.
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
ms.openlocfilehash: 443a8186ac6e57360105e59e30f84db997cd2251
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72897541"
---
# <a name="sfctl-chaos-schedule"></a>sfctl chaos schedule
Ruft den Zeitplan des Chaostests ab und legt diesen fest.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| get | Ruft den Chaostestzeitplan ab, der definiert, wann und wie der Chaostest ausgeführt werden soll. |
| set | Legt den von Chaos verwendeten Zeitplan fest. |

## <a name="sfctl-chaos-schedule-get"></a>sfctl chaos schedule get
Ruft den Chaostestzeitplan ab, der definiert, wann und wie der Chaostest ausgeführt werden soll.

Ruft die Version des verwendeten Chaostestzeitplans und den Chaostestzeitplan ab, der definiert, wann und wie der Chaostest ausgeführt werden soll.

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

## <a name="sfctl-chaos-schedule-set"></a>sfctl chaos schedule set
Legt den von Chaos verwendeten Zeitplan fest.

Der Chaostest wird automatisch basierend auf dem Chaostestzeitplan ausgeführt. Der Chaostestzeitplan wird aktualisiert, wenn die angegebene Version mit der Version auf dem Server übereinstimmt. Beim Aktualisieren des Chaostestzeitplans wird die Version auf dem Server um 1 erhöht. Die Version auf dem Server wird nach Erreichen einer großen Zahl auf 0 zurückgesetzt. Wenn der Chaostest während der Durchführung dieses Aufrufs ausgeführt wird, tritt beim Aufruf ein Fehler auf.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --chaos-parameters-dictionary | JSON-codierte Liste, die eine Zuordnung von Zeichenfolgennamen für die ChaosParameters-Klasse darstellt, die bei vom Jobs-Argument verwendet werden soll. |
| --expiry-date-utc | Datum und Uhrzeit, wann die Verwendung des Chaostestzeitplans für die Planung des Chaostests beendet werden soll.  Standardwert\: „9999-12-31T23\:59\:59.999Z“. |
| --jobs | JSON-codierte Liste von ChaosScheduleJobs, die angibt, wann der Chaostest ausgeführt werden soll und welche Parameter beim Chaostest ausgeführt werden sollen. |
| --start-date-utc | Datum und Uhrzeit, wann der Zeitplan für die Planung des Chaostests gestartet werden soll.  Standardwert\: „1601-01-01T00\:00\:00.000Z“. |
| --timeout -t | Standardwert\: 60. |
| --version | Die Versionsnummer des Zeitplans. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

### <a name="examples"></a>Beispiele

Der folgende Befehl legt einen Zeitplan fest (vorausgesetzt, dass der aktuelle Zeitplan Version 0 aufweist), der am 01.01.2016 beginnt und am 01.01.2038 abläuft und den Chaostest rund um die Uhr ausführt. Der Chaostest wird auf dem Cluster für diesen Zeitpunkt geplant.

    sfctl chaos schedule set --version 0 --start-date-utc "2016-01-01T00:00:00.000Z" --expiry-date-utc "2038-01-01T00:00:00.000Z"
    --chaos-parameters-dictionary
    [
    {
        "Key":"adhoc",
        "Value":{
            "MaxConcurrentFaults":3,
            "EnableMoveReplicaFaults":true,
            "ChaosTargetFilter":{
                "NodeTypeInclusionList":[
                "N0010Ref",
                "N0020Ref",
                "N0030Ref",
                "N0040Ref",
                "N0050Ref"
                ]
            },
            "MaxClusterStabilizationTimeoutInSeconds":60,
            "WaitTimeBetweenIterationsInSeconds":15,
            "WaitTimeBetweenFaultsInSeconds":30,
            "TimeToRunInSeconds":"600",
            "Context":{
                "Map":{
                "test":"value"
                }
            },
            "ClusterHealthPolicy":{
                "MaxPercentUnhealthyNodes":0,
                "ConsiderWarningAsError":true,
                "MaxPercentUnhealthyApplications":0
            }
        }
    }
    ]
    --jobs
    [
    {
        "ChaosParameters":"adhoc",
        "Days":{
            "Sunday":true,
            "Monday":true,
            "Tuesday":true,
            "Wednesday":true,
            "Thursday":true,
            "Friday":true,
            "Saturday":true
        },
        "Times":[
            {
                "StartTime":{
                "Hour":0,
                "Minute":0
                },
                "EndTime":{
                "Hour":23,
                "Minute":59
                }
            }
        ]
    }
    ]


## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)
