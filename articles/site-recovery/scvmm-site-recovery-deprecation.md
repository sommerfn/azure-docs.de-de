---
title: Bevorstehende Einstellung der Notfallwiederherstellung zwischen Kundenstandorten, die Hyper-V verwenden, und zwischen Standorten, die von SCVMM in Azure verwaltet werden | Microsoft-Dokumentation
description: Details zur bevorstehenden Einstellung der Notfallwiederherstellung zwischen Kundenstandorten, die Hyper-V verwenden, und zwischen Standorten, die von SCVMM in Azure verwaltet werden, sowie alternative Optionen
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 09/13/2019
ms.author: rajanaki
ms.openlocfilehash: 68881d57f251d2f26996b2a837780106326ec1d3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73492288"
---
# <a name="upcoming-deprecation-of-dr-between-customer-owned-sites-using-hyper-v-and-between-sites-managed-by-scvmm-to-azure"></a>Bevorstehende Einstellung der Notfallwiederherstellung zwischen Kundenstandorten, die Hyper-V verwenden, und zwischen Standorten, die von SCVMM in Azure verwaltet werden

In diesem Artikel werden der Plan zur bevorstehenden Einstellung, die entsprechenden Auswirkungen und die für die Kunden verfügbaren alternativen Optionen für die folgenden Szenarien beschrieben, die von Azure Site Recovery unterstützt wurden. 

- Notfallwiederherstellung von Hyper-V-VMs, die durch SCVMM zwischen Kundenstandorten verwaltet werden 
- Notfallwiederherstellung von Hyper-V-VMs, die von SCVMM in Azure verwaltet werden

> [!IMPORTANT]
> Diese Szenarien sind derzeit zur Einstellung markiert, und es wird von Kunden erwartet, dass sie schnellstmöglich die erforderlichen Wartungsschritte ausführen, um jegliche Unterbrechung in ihrer Umgebung zu vermeiden. 
 

## <a name="what-changes-should-you-expect"></a>Welche Änderungen sind zu erwarten?

- Ab November 2019 sind keine neuen Benutzerintegrationen für diese Szenarien mehr zulässig. **Vorhandene Replikationen und Verwaltungsvorgänge**, einschließlich Failover, Testfailover, Überwachung usw. **, werden davon nicht betroffen**.

- Nachdem die Szenarien eingestellt wurden, führt dies zu den folgenden Auswirkungen, sofern der Kunde nicht die empfohlenen Schritte ausführt.

    - Notfallwiederherstellung zwischen Hyper-V-VMS, die von SCVMM zwischen Kundenstandorten verwaltet werden: Die Replikationen funktionieren weiterhin, da die zugrunde liegende Funktion des Hyper-V-Replikats weiterhin funktioniert. Kunden können aber Vorgänge im Zusammenhang mit der Notfallwiederherstellung über die Azure Site Recovery-Oberfläche im Azure-Portal nicht anzeigen, verwalten oder ausführen. 
    - Notfallwiederherstellung von Hyper-V-VMS, die von SCVMM in Azure verwaltet werden: Die vorhandenen Replikationen werden unterbrochen, und Kunden können Vorgänge im Zusammenhang mit der Notfallwiederherstellung über das Azure-Portal nicht anzeigen, verwalten oder ausführen.


## <a name="recommended-actions-to-be-taken"></a>Empfohlene Maßnahmen für diese Fälle

Nachstehend finden Sie die Optionen, mit denen der Kunde dafür sorgen muss, dass seine Notfallwiederherstellungsstrategie nicht beeinträchtigt wird, nachdem das jeweilige Szenario eingestellt wurde. 

- Wählen Sie [Start using Azure as the DR target for VMs on Hyper-V hosts](hyper-v-azure-tutorial.md) (Mit der Verwendung von Azure als Notfallwiederherstellungsziel für VMs auf Hyper-V-Hosts beginnen) aus.

> [!IMPORTANT]
> Beachten Sie, dass Ihre lokale Umgebung weiterhin über SCVMM verfügen kann, Sie aber Azure Site Recovery (ASR) mit Verweisen auf nur die Hyper-V-Hosts konfigurieren werden.

- Wenn Sie auswählen, dass Sie die Standort-zu-Standort-Replikation fortsetzen, aber die zugrunde liegende [Hyper-V-Replikat-Lösung](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) verwenden möchten, können Sie Konfigurationen für die Notfallwiederherstellung mithilfe von Azure Site Recovery im Azure-Portal nicht verwalten. 


## <a name="next-steps"></a>Nächste Schritte
Planen Sie für die Einstellung, und wählen Sie die alternative Option aus, die für Ihre Infrastruktur und Ihr Unternehmen am besten geeignet ist. Falls Sie Fragen dazu haben, wenden Sie sich bitte an den Microsoft-Support.

