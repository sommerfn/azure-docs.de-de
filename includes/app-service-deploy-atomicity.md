---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/12/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 93332002cd2ac513d125e0f9eb75dff4a2d8760c
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836829"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Was geschieht während der Bereitstellung mit meiner App?

Alle offiziell unterstützten Bereitstellungsmethoden nehmen Änderungen an den Dateien im Ordner `/home/site/wwwroot` Ihrer App vor. Dabei handelt es sich um dieselben Dateien, die in der Produktionsumgebung ausgeführt werden. Daher kann die Bereitstellung aufgrund von gesperrten Dateien fehlschlagen. Die App in der Produktionsumgebung kann sich während der Bereitstellung unvorhersehbar verhalten, da nicht alle Dateien gleichzeitig aktualisiert werden. Es gibt mehrere Möglichkeiten, wie Sie diese Probleme umgehen können:

- Beenden Sie die App, oder aktivieren Sie den Offlinemodus für die App während der Bereitstellung. Weitere Informationen finden Sie unter [Deal with locked files during deployment (Umgang mit gesperrten Dateien während der Bereitstellung)](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Stellen Sie einen [Stagingslot](../articles/app-service/deploy-staging-slots.md) bereit, bei dem [Automatisch tauschen](../articles/app-service/deploy-staging-slots.md#configure-auto-swap) aktiviert ist. 
- Verwenden Sie [Aus Paketdatei ausführen](https://github.com/Azure/app-service-announcements/issues/84) anstelle von Continuous Deployment.
