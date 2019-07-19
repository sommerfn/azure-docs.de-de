---
title: Herunterfahren einer Microsoft Azure FXT Edge Filer-Einheit
description: Enthält eine Beschreibung der Verfahren zum Hochfahren und sicheren Herunterfahren eines Azure FXT Edge Filer-Knotens.
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: how-to
ms.date: 07/01/2019
ms.author: v-erkell
ms.openlocfilehash: 8d779547040da9855409d3408827af2e4acd17a7
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542865"
---
# <a name="how-to-safely-power-off-azure-fxt-edge-filer-hardware"></a>Sicheres Ausschalten von Azure FXT Edge Filer-Hardware

Sie können zwar den physischen Netzschalter verwenden, um einen einzelnen Knoten einzuschalten, aber Sie sollten ihn im Normalfall nicht nutzen, um die Einheit herunterzufahren.

Sobald ein Azure FXT Edge Filer-Knoten als Teil eines Clusters verwendet wird, sollte die Hardware über die Systemsteuerungssoftware des Clusters heruntergefahren werden. 

> [!NOTE] 
> Verwenden Sie zur Vermeidung von Datenverlust oder -beschädigungen immer die Systemsteuerungssoftware, um einen Azure FXT Edge Filer herunterzufahren. Nutzen Sie den physischen Netzschalter nur dann zum Herunterfahren, wenn Sie vom Microsoft-Kundendienst und -Support (CSS) dazu aufgefordert werden.
> 
> Ziehen Sie in einem elektrischen Notfall die Netzkabel ab, oder verwenden Sie den Mechanismus, der in Ihrem Rechenzentrum für das Schalten in den stromlosen Zustand vorgesehen ist.

## <a name="shut-down-a-node-from-the-control-panel"></a>Herunterfahren eines Knotens über die Systemsteuerung

Befolgen Sie diese Anleitung, um einen Azure FXT Edge Filer-Knoten sicher auszuschalten:

1. Melden Sie sich an der Systemsteuerung des Clusters an. (Anleitung unter [Öffnen der Einstellungsseiten](fxt-cluster-create.md#open-the-settings-pages))
1. Klicken Sie auf die Registerkarte **Einstellungen**, und laden Sie anschließend die Seite **Cluster** > **FXT Nodes** (FXT-Knoten).
1. Suchen Sie in der Liste mit den Clusterknoten nach dem Knoten, den Sie herunterfahren möchten. Klicken Sie in der zugehörigen Spalte **Aktionen** auf die Schaltfläche **Ausschalten**. 
1. Warten Sie einen Moment. Der Knoten wird heruntergefahren und schaltet sich selbst aus.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu den Status-LEDs und anderen Anzeigen finden Sie unter [Überwachen des Azure FXT Edge Filer-Hardwarestatus](fxt-monitor.md).
* Informieren Sie sich unter [Anschließen der Netzkabel](fxt-network-power.md#connect-power-cables) weiter über die Stromversorgung für Azure FXT Edge Filer.
