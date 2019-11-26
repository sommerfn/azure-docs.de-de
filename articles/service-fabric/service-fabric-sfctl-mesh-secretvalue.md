---
title: 'Azure Service Fabric CLI: sfctl mesh secretvalue | Microsoft-Dokumentation'
description: Beschreibt die sfctl mesh secretvalue-Befehle der Service Fabric CLI.
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
ms.openlocfilehash: 96ce4efe00d89c136a0c11d445170b2f67be6fcd
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72901189"
---
# <a name="sfctl-mesh-secretvalue"></a>sfctl mesh secretvalue
Ruft die Mesh-secretvalue-Ressourcen ab bzw. löscht sie.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| delete | Löscht den angegebenen Wert der benannten Geheimnisressource. |
| list | Listet die Namen aller Werte der angegebenen Geheimnisressource auf. |
| show | Listet den angegebenen Wert der Geheimnisressource auf. |

## <a name="sfctl-mesh-secretvalue-delete"></a>sfctl mesh secretvalue delete
Löscht den angegebenen Wert der benannten Geheimnisressource.

Löscht die Geheimniswertressource, die anhand des Namens identifiziert wird. Der Name der Ressource ist normalerweise die Version, die diesem Wert zugeordnet ist. Beim Löschen tritt ein Fehler auf, wenn der angegebene Wert verwendet wird.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --secret-name -n [Erforderlich] | Der Name der Geheimnisressource. |
| --version -v     [Erforderlich] | Der Name der Geheimnisversion. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-mesh-secretvalue-list"></a>sfctl mesh secretvalue list
Listet die Namen aller Werte der angegebenen Geheimnisressource auf.

Ruft Informationen zu allen Geheimniswertressourcen der angegebenen Geheimnisressource ab. Die Informationen enthalten den Namen der Geheimniswertressourcen, aber nicht die tatsächlichen Werte.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --secret-name -n [Erforderlich] | Der Name der Geheimnisressource. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-mesh-secretvalue-show"></a>sfctl mesh secretvalue show
Listet den angegebenen Wert der Geheimnisressource auf.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --secret-name -n [Erforderlich] | Der Name der Geheimnisressource. |
| --version -v     [Erforderlich] | Der Name der Geheimnisversion. |
| --show-value | Zeigt den tatsächlichen Wert der Geheimnisversion an. |

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