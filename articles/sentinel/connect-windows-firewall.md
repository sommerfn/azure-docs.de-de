---
title: Verknüpfen von Windows-Firewalldaten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Windows-Firewalldaten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 840e8b3bc86281a8c42689b1cb68917741ef2bd9
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240626"
---
# <a name="connect-windows-firewall"></a>Herstellen einer Verbindung mit der Windows-Firewall



Der Windows-Firewall-Connector ermöglicht es Ihnen, einfach eine Verbindung mit Ihren Windows-Firewall-Protokollen herzustellen, wenn diese mit Ihrem Azure Sentinel-Arbeitsbereich verbunden sind. Diese Verbindung ermöglicht es Ihnen, Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Untersuchungen zu verbessern. Dadurch erhalten Sie einen besseren Einblick in das Netzwerk Ihrer Organisation und bessere Möglichkeiten für Sicherheitsvorgänge. Die Lösung sammelt Windows-Firewallereignisse von den Windows-Computern, auf denen ein Log Analytics-Agent installiert ist. 


> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="enable-the-connector"></a>Aktivieren des Connectors 

1. Klicken Sie im Azure Sentinel-Portal auf **Data connectors** (Datenconnectors) und anschließend auf die Kachel **Windows-Firewall**. 
1.  Wenn sich Ihre Windows-Computer in Azure befinden:
    1. Klicken Sie auf **Install agent on Azure Windows virtual machine** (Agent auf virtuellem Windows-Computer in Azure installieren).
    1. Wählen Sie in der Liste **Virtual machines** (Virtuelle Computer) den Windows-Computer aus, für den Sie an Azure Sentinel streamen möchten. Stellen Sie sicher, dass dies ein virtueller Windows-Computer ist.
    1. Klicken Sie in dem Fenster, das für diesen virtuellen Computer wird geöffnet, auf **Verbinden**.  
    1. Klicken Sie auf **Enable** (Aktivieren) im Fenster **Windows firewall connector** (Windows-Firewall-Connector). 

2. Wenn Ihr Windows-Computer kein virtueller Azure-Computer ist:
    1. Klicken Sie auf **Install agent on non-Azure machines** (Agent auf Nicht-Azure-Computern installieren).
    1. Wählen Sie im Fenster **Direkt-Agent** entweder **Windows-Agent herunterladen (64 Bit)** oder **Windows-Agent herunterladen (32 Bit)** aus.
    1. Installieren Sie den Agenten auf Ihrem Windows-Computer. Kopieren Sie **Arbeitsbereich-ID**, **Primärschlüssel** und **Sekundärschlüssel**, und verwenden Sie diese Werte, wenn Sie während der Installation zur Eingabe aufgefordert werden.

4. Wählen Sie die Datentypen aus, die Sie streamen möchten.
5. Klicken Sie auf **Lösung installieren**.
6. Um das relevante Schema für die Windows-Firewall in Log Analytics zu verwenden, suchen Sie nach **SecurityEvent**.

## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden. 



## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie die Windows-Firewall mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).

