---
title: Includedatei
description: Includedatei
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 0b3af1bd7b3ab48074432d9e39552d72c6664b98
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58124730"
---
Bei der Bereitstellung von Ressourcen in Azure sind Sie äußerst flexibel in Bezug auf die bereitzustellenden Ressourcentypen sowie den Speicherort und die Einrichtung der Ressourcen. Durch diese Flexibilität können jedoch mehr Optionen bereitgestellt werden, als Sie in Ihrer Organisation zulassen möchten. Bei der Bereitstellung von Ressourcen in Azure stellen Sie unter Umständen folgende Überlegungen an:

* Wie erfülle ich die rechtlichen Vorgaben hinsichtlich der Datenhoheit in bestimmten Ländern?
* Wie können Kosten gesteuert werden?
* Wie stelle ich sicher, dass kritische Systeme nicht versehentlich geändert werden?
* Wie verfolge ich Ressourcenkosten nach und rechne sie korrekt ab?

In diesem Artikel werden diese Fragen beantwortet. Sie erhalten insbesondere Informationen zu den folgenden Schritten:

> [!div class="checklist"]
> * Zuweisen von Benutzern zu Rollen und Zuweisen der Rollen zu einem Bereich, sodass Benutzer Berechtigungen zum Ausführen der erwarteten Aktionen (und keinen weiteren Aktionen) erhalten
> * Anwenden von Richtlinien, die Konventionen für Ressourcen in Ihrem Abonnement vorgeben
> * Sperren von Ressourcen, die wichtig für Ihr System sind
> * Kennzeichnen von Ressourcen, sodass Sie sie anhand von Werten nachverfolgen können, die sinnvoll für Ihre Organisation sind

Dieser Artikel konzentriert sich auf die Aufgaben, die Sie zum Implementieren der Governance ausführen. Eine umfassendere Erläuterung der Konzepte finden Sie unter [Governance (Kontrolle) in Azure](../articles/security/governance-in-azure.md). 
