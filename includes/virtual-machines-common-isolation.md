---
title: include file
description: include file
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: include
ms.date: 09/18/2019
ms.author: azcspmt;ayshak;cynthn
ms.custom: include file
ms.openlocfilehash: 0f528117b19a3f26b964dee7ae453fc184c8dddb
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71123080"
---
Azure Compute bietet VM-Größen, die für einen bestimmten Hardwaretyp isoliert und für einen einzelnen Kunden bestimmt sind.  Diese VM-Größen eignen sich am besten für Workloads, die ein hohes Maß an Isolation von anderen Kunden erfordern, wenn es um Workloads mit Elementen wie Konformität und gesetzlichen Anforderungen geht.  Kunden können auch die Ressourcen dieser isolierten virtuellen Computer weiter unterteilen, indem sie die [Azure-Unterstützung für geschachtelte virtuelle Computer](https://azure.microsoft.com/blog/nested-virtualization-in-azure/) verwenden.

Durch die Verwendung isolierter Größen wird sichergestellt, dass Ihr virtueller Computer als einziger in der jeweiligen Serverinstanz ausgeführt wird.  Zu den aktuellen Angebote isolierter virtueller Computer zählen Folgende:
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5
* Standard_DS15_v2
* Standard_D15_v2
* Standard_F72s_v2

Weitere Informationen zu jeder verfügbaren isolierten Größe finden Sie [hier](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory).