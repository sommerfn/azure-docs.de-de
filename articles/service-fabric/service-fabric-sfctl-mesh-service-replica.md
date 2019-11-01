---
title: 'Azure Service Fabric CLI: sfctl mesh service-replica | Microsoft-Dokumentation'
description: Beschreibt die sfctl mesh service-replica-Befehle der Service Fabric CLI.
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
ms.openlocfilehash: e8eaa5fd7c9eeeff3f70a949b04d0fbf5f6c388f
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901171"
---
# <a name="sfctl-mesh-service-replica"></a>sfctl mesh service-replica
Ruft Replikatdetails ab und listet Replikate eines angegebenen Diensts in einer Anwendungsressource auf.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| list | Listet alle Replikate eines Diensts auf. |
| show | Ruft das angegebene Replikat des Diensts einer Anwendung ab. |

## <a name="sfctl-mesh-service-replica-list"></a>sfctl mesh service-replica list
Listet alle Replikate eines Diensts auf.

Ruft die Informationen zu allen Replikaten eines Diensts ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften des Dienstreplikats.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --app-name --application-name [erforderlich] | Der Namen der Anwendung. |
| --service-name                [erforderlich] | Der Name des Diensts. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-mesh-service-replica-show"></a>sfctl mesh service-replica show
Ruft das angegebene Replikat des Diensts einer Anwendung ab.

Ruft die Informationen zum Dienstreplikat mit dem angegebenen Namen ab. Die Informationen umfassen die Beschreibung und andere Eigenschaften des Dienstreplikats.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --app-name --application-name [erforderlich] | Der Namen der Anwendung. |
| --name -n                     [erforderlich] | Der Name des Dienstreplikats. |
| --service-name                [erforderlich] | Der Name des Diensts. |

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