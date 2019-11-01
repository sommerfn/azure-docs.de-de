---
title: Azure Service Fabric CLI – sfctl is | Microsoft-Dokumentation
description: Beschreibt die sfctl is-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
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
ms.openlocfilehash: 9a09d8c36fd282450767880a9ad144e1192dcd2e
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901580"
---
# <a name="sfctl-is"></a>sfctl is
Ermöglicht es, Befehle an den Infrastrukturdienst abzufragen und zu senden.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| command | Ruft einen Verwaltungsbefehl für die angegebene Infrastrukturdienstinstanz auf. |
| query | Ruft eine schreibgeschützte Abfrage für die angegebene Infrastrukturdienstinstanz auf. |

## <a name="sfctl-is-command"></a>sfctl is command
Ruft einen Verwaltungsbefehl für die angegebene Infrastrukturdienstinstanz auf.

Für Cluster, für die mindestens eine Instanz des Infrastrukturdiensts konfiguriert ist, bietet diese API eine Möglichkeit, infrastrukturspezifische Befehle an eine bestimmte Instanz des Infrastrukturdiensts zu senden. Die verfügbaren Befehle und ihre entsprechenden Antwortformate variieren je nach der Infrastruktur, auf der der Cluster ausgeführt wird. Diese API unterstützt die Service Fabric-Plattform. Sie ist nicht dazu vorgesehen, direkt aus Ihrem Code verwendet zu werden.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --command [erforderlich] | Der Text des Befehls, der aufgerufen werden soll. Der Inhalt des Befehls ist infrastrukturspezifisch. |
| --service-id | Die Identität des Infrastrukturdiensts. <br><br> Dies ist der vollständige Name des Infrastrukturdiensts ohne das URI-Schema „fabric\:“. Dieser Parameter ist nur für den Cluster erforderlich, in dem mehrere Instanzen des Infrastrukturdiensts ausgeführt werden. |
| --timeout -t | Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-is-query"></a>sfctl is query
Ruft eine schreibgeschützte Abfrage für die angegebene Infrastrukturdienstinstanz auf.

Für Cluster, für die mindestens eine Instanz des Infrastrukturdiensts konfiguriert ist, bietet diese API eine Möglichkeit, infrastrukturspezifische Abfragen an eine bestimmte Instanz des Infrastrukturdiensts zu senden. Die verfügbaren Befehle und ihre entsprechenden Antwortformate variieren je nach der Infrastruktur, auf der der Cluster ausgeführt wird. Diese API unterstützt die Service Fabric-Plattform. Sie ist nicht dazu vorgesehen, direkt aus Ihrem Code verwendet zu werden.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --command [erforderlich] | Der Text des Befehls, der aufgerufen werden soll. Der Inhalt des Befehls ist infrastrukturspezifisch. |
| --service-id | Die Identität des Infrastrukturdiensts. <br><br> Dies ist der vollständige Name des Infrastrukturdiensts ohne das URI-Schema „fabric\:“. Dieser Parameter ist nur für den Cluster erforderlich, in dem mehrere Instanzen des Infrastrukturdiensts ausgeführt werden. |
| --timeout -t | Standardwert\: 60. |

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