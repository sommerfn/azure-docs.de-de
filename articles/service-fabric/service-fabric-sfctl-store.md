---
title: Azure Service Fabric CLI – sfctl store | Microsoft-Dokumentation
description: Beschreibt die sfctl store-Befehle der Service Fabric-Befehlszeilenschnittstelle (Command Line Interface, CLI).
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: ccd9e8323f6e0de7b81c7600e7828e4858c51201
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035860"
---
# <a name="sfctl-store"></a>sfctl store
Ermöglicht es, grundlegende Vorgänge auf Dateiebene für den Clusterimagespeicher auszuführen.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| delete | Löscht den vorhandenen Imagespeicherinhalt. |
| root-info | Ruft die Inhaltsinformationen aus dem Stammverzeichnis des Imagespeichers ab. |
| stat | Ruft die Informationen zum Imagespeicherinhalt ab. |

## <a name="sfctl-store-delete"></a>sfctl store delete
Löscht den vorhandenen Imagespeicherinhalt.

Löscht den Imagespeicherinhalt, der im angegebenen relativen Pfad des Imagespeichers gefunden wird. Mit diesem Befehl können hochgeladene Anwendungspakete gelöscht werden, nachdem sie bereitgestellt wurden.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --content-path [erforderlich] | Relativer Pfad zu einer Datei oder einem Ordner im Imagespeicher ab dessen Stammverzeichnis. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-store-root-info"></a>sfctl store root-info
Ruft die Inhaltsinformationen aus dem Stammverzeichnis des Imagespeichers ab.

Gibt die Informationen über den Imagespeicherinhalt zurück, der sich im Stammverzeichnis des Imagespeichers befindet.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

## <a name="sfctl-store-stat"></a>sfctl store stat
Ruft die Informationen zum Imagespeicherinhalt ab.

Gibt die Informationen über den Imagespeicherinhalt zurück, der sich im angegebenen contentPath befindet. Der contentPath ist relativ zum Stammverzeichnis des Imagespeichers.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --content-path [erforderlich] | Relativer Pfad zu einer Datei oder einem Ordner im Imagespeicher ab dessen Stammverzeichnis. |
| --timeout -t | Servertimeout in Sekunden.  Standardwert\: 60. |

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