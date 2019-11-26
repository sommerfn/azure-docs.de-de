---
title: include file
description: include file
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 10/30/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c141e67157f3ec17d475062ec76406ec765c4f50
ms.sourcegitcommit: fa5ce8924930f56bcac17f6c2a359c1a5b9660c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73199141"
---
Durch Anwenden von Tags auf Ihre Azure-Ressourcen erzeugen Sie Metadaten, um sie logisch in einer Taxonomie zu organisieren. Jedes Tag besteht aus einem Paar mit einem Namen und einem Wert. So können Sie beispielsweise den Namen „Umgebung“ und den Wert „Produktion“ auf alle Ressourcen in der Produktion anwenden.

Nach dem Anwenden von Tags können Sie alle Ressourcen in Ihrem Abonnement abrufen, die diesen Tagnamen und -wert besitzen. Mit Tags können Sie verwandte Ressourcen aus verschiedenen Ressourcengruppen abrufen. Das ist hilfreich, wenn Sie Ressourcen für die Abrechnung oder Verwaltung organisieren müssen.

Ihre Taxonomie sollte zusätzlich zu einer automatischen Kennzeichhnungsstrategie eine Self-Service-Metadatenkennzeichnungsstrategie berücksichtigen, um die Belastung für Benutzer zu verringern und die Genauigkeit zu verbessern.

Für Tags gelten folgende Einschränkungen:

* Nicht alle Ressourcentypen unterstützen Tags. Um festzustellen, ob Sie ein Tag auf einen Ressourcentyp anwenden können, lesen Sie [Tagunterstützung für Azure-Ressourcen](../articles/azure-resource-manager/tag-support.md).
* Jede Ressource oder Ressourcengruppe kann maximal 50 Tagname-Wert-Paare besitzen. Wenn Sie mehr Tags als die maximal zulässige Anzahl anwenden müssen, verwenden Sie für den Tagwert eine JSON-Zeichenfolge. Die JSON-Zeichenfolge kann zahlreiche Werte enthalten, die auf einen einzelnen Tagnamen angewendet werden. Eine Ressourcengruppe kann zahlreiche Ressourcen mit jeweils 50 Tagname-Wert-Paaren enthalten.
* Der Tagname ist auf 512 Zeichen beschränkt und der Tagwert auf 256 Zeichen. Bei Speicherkonten ist der Tagname auf 128 Zeichen beschränkt und der Tagwert auf 256 Zeichen.
* Generalisierte VMs unterstützen keine Tags.
* Auf die Ressourcengruppe angewendete Tags werden nicht von den in dieser Ressourcengruppe enthaltenen Ressourcen geerbt.
* Tags können nicht auf klassische Ressourcen wie Cloud Services angewendet werden.
* Tag-Namen dürfen die folgenden Zeichen nicht enthalten: `<`, `>`, `%`, `&`, `\`, `?`, `/`

   > [!NOTE]
   > Derzeit lassen Azure DNS-Zonen und Traffic Manager-Dienste auch keine Leerzeichen im Tag zu. 
