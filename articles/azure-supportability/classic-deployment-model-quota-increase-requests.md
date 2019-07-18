---
title: Klassisches Azure-Bereitstellungsmodell | Microsoft-Dokumentation
description: Klassisches Azure-Bereitstellungsmodell
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/20/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 5ae2a56c084116ae8d57a16696223e7990258558
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313226"
---
# <a name="classic-deployment-model"></a>Klassisches Bereitstellungsmodell

Das klassisches Bereitstellungsmodell ist ein älteres Bereitstellungsmodell für Azure. Es erzwingt einen globales Kontingentlimit für vCPUs für VMs und VM-Skalierungsgruppen. Das klassische Bereitstellungsmodell wird nicht mehr empfohlen. Stattdessen eignet sich das Resource Manager-Modell. Weitere Informationen über diese beiden Bereitstellungsmodelle und die Vorteile des Resource Managers finden Sie auf der Seite zum Resource Manager-Bereitstellungsmodell. Sobald ein Abonnement erstellt wird, wird diesem das Standardkontingent für vCPUs zugewiesen. Bei jeder Bereitstellung einer neuen VM über das klassische Bereitstellungsmodell werden die neuen und bereits vorhandenen CPUs für alle Regionen addiert. Dabei darf das für das klassische Bereitstellungsmodell zur Verfügung stehende vCPU-Kontingent nicht überschritten werden. Weitere Informationen zu den Kontingenten erhalten Sie auf der Seite zu Azure-Abonnements und Diensteinschränkungen.

Über das Blatt „Hilfe und Support“ oder über das Blatt „Usages + Quota“ (Nutzung und Kontingente) können Sie im Portal eine Erhöhung der Grenzwerte für vCPUs für das klassische Bereitstellungsmodell beantragen.

