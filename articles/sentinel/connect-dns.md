---
title: Verknüpfen von DNS-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie DNS-Daten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 77af84f9-47bc-418e-8ce2-4414d7b58c0c
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/24/2019
ms.author: rkarlin
ms.openlocfilehash: c5e58f496176ec0f1b8317c8b862a8ef2ffa434d
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71262734"
---
# <a name="connect-your-domain-name-server"></a>Verbinden eines Domänennamenservers

> [!IMPORTANT]
> Der DNS-Datenconnector in Azure Sentinel ist derzeit als Public Preview verfügbar.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können jeden Domänennamenserver (DNS), der unter Windows ausgeführt wird, mit Azure Sentinel verbinden. Dies erfolgt durch Installieren eines Agents auf dem DNS-Computer. Mithilfe von DNS-Protokollen können Sie sicherheits-, leistungs- und vorgangsbezogene Einblicke in die DNS-Infrastruktur Ihrer Organisation gewinnen, indem Sie Analyse- und Überwachungsprotokolle und andere zugehörige Daten von den DNS-Servern sammeln.

Das Aktivieren der DNS-Protokollverbindung ermöglicht Ihnen Folgendes:
- Identifizieren von Clients, die versuchen, schädliche Domänennamen aufzulösen
- Identifizieren von veralteten Ressourceneinträgen
- Identifizieren von häufig abgefragten Domänennamen und DNS-Clients mit hoher Aktivität
- Anzeigen der Anforderungslast auf DNS-Servern
- Anzeigen von Fehlern der dynamischen DNS-Registrierung

## <a name="connected-sources"></a>Verbundene Quellen

In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von der Lösung unterstützt werden:

| **Verbundene Quelle** | **Unterstützung** | **Beschreibung** |
| --- | --- | --- |
| [Windows-Agents](../azure-monitor/platform/agent-windows.md) | Ja | Die Lösung erfasst DNS-Informationen von Windows-Agents. |
| [Linux-Agents](../azure-monitor/learn/quick-collect-linux-computer.md) | Nein | Die Lösung erfasst keine DNS-Informationen von direkten Linux-Agents. |
| [System Center Operations Manager-Verwaltungsgruppe](../azure-monitor/platform/om-agents.md) | Ja | Die Lösung erfasst DNS-Informationen von Agents in einer verbundenen Operations Manager-Verwaltungsgruppe. Es ist keine direkte Verbindung zwischen dem Operations Manager-Agent und Azure Monitor erforderlich. Daten werden von der Verwaltungsgruppe an den Log Analytics-Arbeitsbereich weitergeleitet. |
| [Azure-Speicherkonto](../azure-monitor/platform/collect-azure-metrics-logs.md) | Nein | Azure-Speicher wird von der Lösung nicht verwendet. |

### <a name="data-collection-details"></a>Details zur Datensammlung

Die Lösung sammelt Daten zum DNS-Inventar und zu DNS-Ereignissen von den DNS-Servern, auf denen ein Log Analytics-Agent installiert ist. Inventardaten, z.B. die Anzahl der DNS-Server, Zonen und Ressourceneinträge, werden gesammelt, indem die DNS-PowerShell-Cmdlets ausgeführt werden. Die Daten werden einmal alle zwei Tage aktualisiert. Die Ereignisdaten werden nahezu in Echtzeit aus den [Analyse- und Überwachungsprotokollen](https://technet.microsoft.com/library/dn800669.aspx#enhanc) erfasst, die durch die verbesserte DNS-Protokollierung und -Diagnose in Windows Server 2012 R2 bereitgestellt werden.


## <a name="connect-your-dns-appliance"></a>Verbinden Ihrer DNS-Appliance

1. Wählen Sie im Azure Sentinel-Portal die Option **Data connectors** (Datenconnectors) aus, und klicken Sie auf die Kachel **DNS (Preview)** (DNS (Vorschau)).
1. Wenn sich Ihre DNS-Computer in Azure befinden:
    1. Klicken Sie auf **Install agent on Azure Windows virtual machine** (Agent auf virtuellem Windows-Computer in Azure installieren).
    1. Wählen Sie in der Liste **Virtual Machines** den DNS-Computer aus, für den Sie an Azure Sentinel streamen möchten. Stellen Sie sicher, dass dies ein virtueller Windows-Computer ist.
    1. Klicken Sie in dem Fenster, das für diesen virtuellen Computer wird geöffnet, auf **Verbinden**.  
    1. Klicken Sie auf **Enable** (Aktivieren) im Fenster **DNS-Connector**. 

2. Wenn Ihr DNS-Computer kein virtueller Azure-Computer ist:
    1. Klicken Sie auf **Install agent on non-Azure machines** (Agent auf Nicht-Azure-Computern installieren).
    1. Wählen Sie im Fenster **Direkt-Agent** entweder **Windows-Agent herunterladen (64 Bit)** oder **Windows-Agent herunterladen (32 Bit)** aus.
    1. Installieren Sie den Agent auf Ihrem DNS-Computer. Kopieren Sie **Arbeitsbereich-ID**, **Primärschlüssel** und **Sekundärschlüssel**, und verwenden Sie diese Werte, wenn Sie während der Installation zur Eingabe aufgefordert werden.

3. Um das entsprechende Schema in Log Analytics für die DNS-Protokolle zu verwenden, suchen Sie nach **DnsEvents**.

## <a name="validate"></a>Überprüfen 

Suchen Sie in Log Analytics nach dem Schema **DnsEvents**, und stellen Sie sicher, dass drei Ereignisse vorhanden sind.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie lokale DNS-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
