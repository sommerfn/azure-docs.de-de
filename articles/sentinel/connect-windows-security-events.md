---
title: Verknüpfen von Windows-Sicherheitsereignisdaten mit Azure Sentinel Preview | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Windows-Sicherheitsereignisdaten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: 36d38aa82b4f0ec8d7d9ef6ebb1145b1fcc334df
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190586"
---
# <a name="connect-windows-security-events"></a>Herstellen einer Verbindung mit Windows-Sicherheitsereignissen 

> [!IMPORTANT]
> Azure Sentinel ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können alle Sicherheitsereignisse von den mit dem Azure Sentinel-Arbeitsbereich verbundenen Windows-Servern streamen. Diese Verbindung ermöglicht es Ihnen, Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Dadurch erhalten Sie einen besseren Einblick in das Netzwerk Ihrer Organisation und bessere Möglichkeiten für Sicherheitsvorgänge.  Sie können die Ereignisse zum Streamen auswählen:

- **Alle Ereignisse:** alle Windows-Sicherheits- und AppLocker-Ereignisse.
- **Allgemein:** ein Standardsatz von Ereignissen zu Überwachungszwecken. In diesem Satz ist ein vollständiger Benutzerüberwachungspfad enthalten. So enthält die Gruppe beispielsweise sowohl Benutzeranmeldungen als auch Benutzerabmeldungen (Ereignis-ID 4634). Wir schließen Überwachungsaktionen wie Sicherheitsgruppenänderungen, wichtige Domänencontroller-Kerberos-Vorgänge und andere Ereignisse ein, die von Branchenorganisationen empfohlen werden.

Ereignisse mit sehr geringem Volumen wurden in die Gruppe „Allgemein“ aufgenommen, da das Hauptargument für ihre Verwendung (im Vergleich zu allen anderen Ereignissen) in der Verringerung des Volumens besteht (und nicht darin, bestimmte Ereignisse herauszufiltern).
- **Minimal:** ein kleiner Satz von Ereignissen, die auf potenzielle Bedrohungen hinweisen können. Wenn Sie diese Option aktivieren, wird kein vollständiger Überwachungspfad erstellt.  Dieser Satz deckt nur Ereignisse ab, die auf eine erfolgreiche Sicherheitsverletzung hindeuten, sowie wichtige Ereignisse, die nur sehr selten auftreten. So deckt diese Gruppe beispielsweise erfolgreiche und nicht erfolgreiche Benutzeranmeldungen (Ereignis-IDs 4624 und 4625), aber keine Abmeldungen ab. Diese sind zwar für die Überwachung wichtig, nicht aber für die Erkennung, und sie treten zudem relativ häufig auf. Der Großteil des Datenvolumens dieser Gruppe ist auf Anmeldeereignisse und auf das Prozesserstellungsereignis (Ereignis-ID 4688) zurückzuführen.
- **Keine:** keine Sicherheits- oder AppLocker-Ereignisse.

> [!NOTE]
> 
> - Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

Die folgende Liste bietet eine vollständige Aufschlüsselung der Sicherheits- und AppLocker-Ereignis-IDs für jeden Satz:

| Datenschicht | Indikatoren für gesammelte Ereignisse |
| --- | --- |
| Wenig | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Common | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

## <a name="set-up-the-windows-security-events-connector"></a>Einrichten des Connectors für Windows-Sicherheitsereignisse

So integrieren Sie Ihre Windows-Sicherheitsereignisse vollständig in Azure Sentinel

1. Wählen Sie im Azure Sentinel-Portal die Option **Data connectors** (Datenconnectors) aus, und klicken Sie dann auf die Kachel **Windows-Sicherheitsereignisse**. 
1. Wählen Sie die Datentypen aus, die Sie streamen möchten.
1. Klicken Sie auf **Aktualisieren**.
6. Um das relevante Schema für die Windows-Sicherheitsereignisse in Log Analytics zu verwenden, suchen Sie nach **SecurityEvent**.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann etwa 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 



## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Windows-Sicherheitsereignisse mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).

