---
title: include file
description: include file
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 07/11/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 099bca7483100da1a4ee2f8f10057c416ad145b0
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67841407"
---
Durch Anwenden von Tags auf Ihre Azure-Ressourcen erzeugen Sie Metadaten, um sie logisch in einer Taxonomie zu organisieren. Jedes Tag besteht aus einem Paar mit einem Namen und einem Wert. So können Sie beispielsweise den Namen „Umgebung“ und den Wert „Produktion“ auf alle Ressourcen in der Produktion anwenden.

Nach dem Anwenden von Tags können Sie alle Ressourcen in Ihrem Abonnement abrufen, die diesen Tagnamen und -wert besitzen. Mit Tags können Sie verwandte Ressourcen aus verschiedenen Ressourcengruppen abrufen. Das ist hilfreich, wenn Sie Ressourcen für die Abrechnung oder Verwaltung organisieren müssen.

Ihre Taxonomie sollte zusätzlich zu einer automatischen Kennzeichhnungsstrategie eine Self-Service-Metadatenkennzeichnungsstrategie berücksichtigen, um die Belastung für Benutzer zu verringern und die Genauigkeit zu verbessern.

Für Tags gelten folgende Einschränkungen:

* Nicht alle Ressourcentypen unterstützen Tags. Um festzustellen, ob Sie ein Tag auf einen Ressourcentyp anwenden können, lesen Sie [Tagunterstützung für Azure-Ressourcen](../articles/azure-resource-manager/tag-support.md).
* Jede Ressource oder Ressourcengruppe kann maximal 50 Tagname-Wert-Paare besitzen. Derzeit werden nur 15 Tags von Speicherkonten unterstützt – dieses Limit wird jedoch in einer zukünftigen Version auf 50 erhöht. Wenn Sie mehr Tags als die maximal zulässige Anzahl anwenden müssen, verwenden Sie für den Tagwert eine JSON-Zeichenfolge. Die JSON-Zeichenfolge kann zahlreiche Werte enthalten, die auf einen einzelnen Tagnamen angewendet werden. Eine Ressourcengruppe kann zahlreiche Ressourcen mit jeweils 50 Tagname-Wert-Paaren enthalten.
* Der Tagname ist auf 512 Zeichen beschränkt und der Tagwert auf 256 Zeichen. Bei Speicherkonten ist der Tagname auf 128 Zeichen beschränkt und der Tagwert auf 256 Zeichen.
* Generalisierte VMs unterstützen keine Tags.
* Auf die Ressourcengruppe angewendete Tags werden nicht von den in dieser Ressourcengruppe enthaltenen Ressourcen geerbt.
* Tags können nicht auf klassische Ressourcen wie Cloud Services angewendet werden.
* Tag-Namen dürfen die folgenden Zeichen nicht enthalten: `<`, `>`, `%`, `&`, `\`, `?`, `/`
