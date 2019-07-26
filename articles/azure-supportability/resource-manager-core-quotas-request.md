---
title: Anforderungen zur Erhöhung von Azure Resource Manager-vCPU-Kontingenten | Microsoft-Dokumentation
description: Anforderungen zur Erhöhung von Azure Resource Manager-vCPU-Kontingenten
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9a997af984b92ea59cc02d99fbd66d8967ca31bd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076807"
---
# <a name="quota-increase-requests"></a>Anfordern einer Kontingenterhöhung

Resource Manager-vCPU-Kontingente für virtuelle Computer und VM-Skalierungsgruppen werden in jeder Region für jedes Abonnement auf zwei Ebenen erzwungen. 

Die erste Ebene ist der Grenzwert „Regionale vCPUs gesamt“ (für alle VM-Serien). Die zweite Ebene ist der Grenzwert „vCPUs pro VM-Serie“ (etwa die vCPUs der D-Serie). Bei jeder Bereitstellung eines neuen virtuellen Computers darf die Summe aus neuer und bereits vorhandener vCPU-Nutzung für diese VM-Serie das für diese spezielle VM-Serie genehmigte vCPU-Kontingent nicht überschreiten. Ferner darf die Gesamtzahl der neuen und vorhandenen vCPUs, die insgesamt für alle VM-Serien bereitgestellt werden, das für das Abonnement genehmigte Kontingent „Regionale vCPUs gesamt“ nicht überschreiten. Wird eines dieser Kontingente überschritten, wird die VM-Bereitstellung nicht zugelassen.
Über das Azure-Portal können Sie eine Erhöhung des vCPU-Kontingentlimits für die VM-Serie anfordern. Bei einer Erhöhung des Kontingents für die VM-Serie erhöht sich automatisch auch der Grenzwert „Regionale vCPUs gesamt“ um den gleichen Wert. 

Wenn ein neues Abonnement erstellt wird, entspricht der Standardwert für „Regionale vCPUs gesamt“ möglicherweise nicht der Summe der vCPU-Standardkontingente für alle einzelnen VM-Serien. Dadurch kann es passieren, dass ein Abonnement zwar über ein ausreichendes Kontingent für die einzelnen bereitzustellenden VM-Serien verfügt, das Kontingent „Regionale vCPUs gesamt“ jedoch nicht für alle Bereitstellungen ausreicht. In diesem Fall müssen Sie explizit eine Erhöhung des Grenzwerts „Regionale vCPUs gesamt“ anfordern. Der Grenzwert „Regionale vCPUs gesamt“ darf die Summe des genehmigten Kontingents für alle VM-Serien der Region nicht überschreiten.

Weitere Informationen zu Kontingenten finden Sie auf den Seiten [vCPU-Kontingente für virtuelle Computer](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) und [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](https://aka.ms/quotalimits). 

