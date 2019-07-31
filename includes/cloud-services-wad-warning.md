---
author: jpconnock
ms.service: cloud-services
ms.topic: include
ms.date: 11/25/2018
ms.author: jeconnoc
ms.openlocfilehash: f23d5246401f23694755e63a51c52da5dbf5ac1b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "67178433"
---
> [!WARNING]
> Wenn Sie die Diagnose für eine vorhandene Rolle aktivieren, werden alle Erweiterungen, die Sie festgelegt haben, beim Bereitstellen des Pakets deaktiviert. Das umfasst:
>
> * Microsoft Monitoring Agent-Diagnose
> * Microsoft Azure-Sicherheitsüberwachung
> * Microsoft Antimalware                 
> * Microsoft Monitoring Agent
> * Microsoft-Dienst-Profiler-Agent      
> * Windows Azure-Domänenerweiterung        
> * Windows Azure-Diagnoseerweiterung   
> * Windows Azure-Remotedesktoperweiterung
> * Windows Azure Log Collector
>
> Sie können Ihre Erweiterungen über das Azure-Portal oder PowerShell zurücksetzen, nachdem Sie die aktualisierte Rolle bereitgestellt haben.
>
