---
title: Azure Service Fabric CLI – sfctl-Ereignisse | Microsoft-Dokumentation
description: Beschreibt die sfctl events-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
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
ms.openlocfilehash: 858fd1971a22b1db2d243838558c3792d3a60cc9
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901865"
---
# <a name="sfctl-events"></a>sfctl events
Ruft Ereignisse aus dem Ereignisspeicher ab (wenn der EventStore-Dienst bereits installiert ist).

Der EventStore-Systemdienst kann durch ein Konfigurationsupgrade auf jeden SFRP-Cluster (>=6.4) hinzugefügt werden. Überprüfen Sie die folgende URL\: https\://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-eventstore.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| all-applications-list | Ruft alle anwendungsbezogenen Ereignisse ab. |
| all-nodes-list | Ruft alle knotenbezogenen Ereignisse ab. |
| all-partitions-list | Ruft alle partitionsbezogenen Ereignisse ab. |
| all-services-list | Ruft alle dienstbezogenen Ereignisse ab. |
| application-list | Ruft alle anwendungsbezogenen Ereignisse ab. |
| cluster-list | Ruft alle clusterbezogenen Ereignisse ab. |
| node-list | Ruft alle knotenbezogenen Ereignisse ab. |
| partition-all-replicas-list | Ruft alle replikatbezogenen Ereignisse für eine Partition ab. |
| partition-list | Ruft alle partitionsbezogenen Ereignisse ab. |
| partition-replica-list | Ruft alle auf Partitionsreplikate bezogenen Ereignisse ab. |
| service-list | Ruft alle dienstbezogenen Ereignisse ab. |

## <a name="sfctl-events-all-applications-list"></a>sfctl events all-applications-list
Ruft alle anwendungsbezogenen Ereignisse ab.

Die Antwort ist eine Liste von ApplicationEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-events-all-nodes-list"></a>sfctl events all-nodes-list
Ruft alle knotenbezogenen Ereignisse ab.

Die Antwort ist eine Liste von NodeEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-events-all-partitions-list"></a>sfctl events all-partitions-list
Ruft alle partitionsbezogenen Ereignisse ab.

Die Antwort ist eine Liste von PartitionEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-events-all-services-list"></a>sfctl events all-services-list
Ruft alle dienstbezogenen Ereignisse ab.

Die Antwort ist eine Liste von ServiceEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-events-application-list"></a>sfctl events application-list
Ruft alle anwendungsbezogenen Ereignisse ab.

Die Antwort ist eine Liste von ApplicationEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --application-id [erforderlich] | Die Identität (ID) der Anwendung. Dies ist üblicherweise der vollständige Name der Anwendung ohne das URI-Schema „fabric\:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „\~“ als Trennzeichen verwendet. Hat eine Anwendung beispielsweise den Namen „fabric\:/meineapp/app1“, weist die Anwendungsidentität in 6.0 und höher den Wert „meineapp\~app1“ und in früheren Versionen den Wert „meineapp/app1“ auf. |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-events-cluster-list"></a>sfctl events cluster-list
Ruft alle clusterbezogenen Ereignisse ab.

Die Antwort ist eine Liste von ClusterEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-events-node-list"></a>sfctl events node-list
Ruft alle knotenbezogenen Ereignisse ab.

Die Antwort ist eine Liste von NodeEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --node-name [erforderlich] | Der Name des Knotens. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-events-partition-all-replicas-list"></a>sfctl events partition-all-replicas-list
Ruft alle replikatbezogenen Ereignisse für eine Partition ab.

Die Antwort ist eine Liste von ReplicaEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --partition-id [erforderlich] | Die Identität (ID) der Partition. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-events-partition-list"></a>sfctl events partition-list
Ruft alle partitionsbezogenen Ereignisse ab.

Die Antwort ist eine Liste von PartitionEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --partition-id [erforderlich] | Die Identität (ID) der Partition. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-events-partition-replica-list"></a>sfctl events partition-replica-list
Ruft alle auf Partitionsreplikate bezogenen Ereignisse ab.

Die Antwort ist eine Liste von ReplicaEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --partition-id [erforderlich] | Die Identität (ID) der Partition. |
| --replica-id [Erforderlich] | Der Bezeichner (ID) des Replikats. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-events-service-list"></a>sfctl events service-list
Ruft alle dienstbezogenen Ereignisse ab.

Die Antwort ist eine Liste von ServiceEvent-Objekten.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --end-time-utc [Erforderlich] | Die Endzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --service-id [erforderlich] | Die Identität (ID) des Diensts. Diese ID ist üblicherweise der vollständige Name des Diensts ohne das URI-Schema „fabric\:“. Ab Version 6.0 wird für hierarchische Namen das Zeichen „\~“ als Trennzeichen verwendet. Hat der Dienst beispielsweise den Namen „fabric\:/meineapp/app1/svc1“, weist die Dienstidentität in 6.0 und höher den Wert „meineapp\~app1\~svc1“ und in früheren Versionen den Wert „meineapp/app1/svc1“ auf. |
| --start-time-utc [Erforderlich] | Die Startzeit einer Suchabfrage im ISO UTC-Format yyyy-MM-ddTHH\:mm\:ssZ. |
| --events-types-filter | Dies ist eine durch Trennzeichen getrennte Zeichenfolge, die die Typen von FabricEvents angibt, die nur in der Antwort enthalten sein sollten. |
| --exclude-analysis-events | Dieser Parameter deaktiviert den Abruf von AnalysisEvents, wenn „true“ übergeben wird. |
| --skip-correlation-lookup | Dieser Parameter deaktiviert die Suche nach CorrelatedEvents-Informationen, wenn „true“ übergeben wird. Andernfalls werden die CorrelationEvents verarbeitet und das Feld „HasCorrelatedEvents“ in jedem FabricEvent ausgefüllt. |
| --timeout -t | Der Servertimeout für die Ausführung des Vorgangs in Sekunden. Dieser Timeout gibt die Zeitdauer an, die der Client bereit ist, auf den Abschluss des angeforderten Vorgangs zu warten. Der Standardwert für diesen Parameter ist 60 Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

