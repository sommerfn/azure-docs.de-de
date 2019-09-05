---
title: Unterstützte Azure HDInsight-Knotenkonfigurationen
description: Erfahren Sie, wie die Mindest- und die empfohlenen Konfigurationen für HDInsight-Clusterknoten aussehen.
keywords: VM-Größen, Clustergrößen, Clusterkonfiguration
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 08/26/2019
ms.openlocfilehash: e482cf9b5367beba00784e69c5bad88142df5225
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70076074"
---
# <a name="what-are-the-default-and-recommended-node-configurations-for-azure-hdinsight"></a>Was sind die Standard- und die empfohlenen Knotenkonfigurationen für Azure HDInsight?

In diesem Artikel sind die Standard- und die empfohlenen Knotenkonfigurationen für Azure HDInsight erläutert.

## <a name="default-and-recommended-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Standard- und empfohlene Knotenkonfigurationen sowie -VM-Größen für Cluster

In den folgenden Tabellen sind die Standard- und die empfohlenen VM-Größen für HDInsight-Cluster aufgeführt.  Diese Informationen sind erforderlich, um die VM-Größen zu verstehen, die beim Erstellen von PowerShell- oder Azure CLI-Skripten verwendet werden sollten, mit denen HDInsight-Cluster bereitgestellt werden. 

Wenn Sie mehr als 32 Workerknoten in einem Cluster benötigen, müssen Sie eine Hauptknotengröße mit mindestens 8 Kernen und 14 GB Arbeitsspeicher (RAM) auswählen. 

Die einzigen Clustertypen, die Datenträger haben, sind Kafka- und HBase-Cluster, wenn für diese das Accelerated Writes-Feature aktiviert ist. HDInsight unterstützt die Datenträgergröße P30 und S30 in diesen Szenarien.

Weitere Informationen zur Spezifikation jedes VM-Typs finden Sie in den folgenden Dokumenten:

* [Universelle VM-Größen: Dv2-Serie 1-5](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dv2-series)
* [Arbeitsspeicheroptimierte Größen virtueller Computer: Dv2-Serie 11-15](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#dv2-series-11-15)
* [Universelle VM-Größen: Av2-Serie 1-8](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#av2-series)

### <a name="all-supported-regions-except-brazil-south-and-japan-west"></a>Alle unterstützten Regionen, mit Ausnahme von „Brasilien, Süden“ und „Japan, Westen“

> [!Note]
> Um den SKU-Bezeichner für die Verwendung in PowerShell und anderen Skripts zu erhalten, fügen Sie `Standard_` am Anfang aller VM-SKUs in den folgenden Tabellen hinzu. Beispielsweise wird `D12_v2` zu `Standard_D12_v2`.

| Clustertyp | Hadoop | hbase | Interactive Query | Storm | Spark | ML Server | Kafka |
|---|---|---|---|---|---|---|---|
| Head – Standard-VM-Größe | D12_v2 | D12_v2 | D13_v2 | A4_v2 | D12_v2 | D12_v2 | D3_v2 |
| Head – empfohlene VM-Größen | D13_v2,<br/>D14_v2,<br/>D5_v2 | D3_v2,<br/>D4_v2,<br/>D12_v2 | D13_v2,<br/>D14_v2 | A4_v2,<br/>A8_v2 | D12_v2,<br/>D13_v2,<br/>D14_v2 | D12_v2,<br/>D13_v2,<br/>D14_v2 | D3_v2,<br/>D4_v2,<br/>D12_v2 |
| Worker – Standard-VM-Größe | D4_v2 | D4_v2 | D14_v2 | D3_v2 | D13_v2 | D4_v2 | 4 D12_v2 mit 2 S30-Datenträgern pro Broker |
| Worker – empfohlene VM-Größen | D5_v2,<br>D12_v2,<br/>D13_v2 | D3_v2,<br/>D4_v2,<br/>D13_v2 | D13_v2,<br/>D14_v2 | D3_v2<br/>D4_v2,<br/>D12_v2 | D12_v2,<br>D13_v2,<br>D14_v2 | D4_v2,<br/>D12_v2,<br>D13_v2,<br>D14_v2 | D3_v2,<br/>D4_v2,<br/>DS3_v2,<br/>DS4_v2 |
| Zookeeper – Standard-VM-Größen |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 | A4_v2 |
| Zookeeper – empfohlene VM-Größen |  | A4_v2, <br/>A8_v2, <br/>A2m_v2 | A4_v2,<br/>A8_v2,<br/>A2m_v2 | A4_v2,<br/>A2_v2,<br/>A8_v2 |  | A2_v2 | A4_v2,<br/> A8_v2,<br/>A2m_v2 |
| ML Services: VM-Standardgröße |  |  |  |  |  | D4_v2 |  |
| ML Services: empfohlene VM-Größe |  |  |  |  |  | D4_v2,<br/> D12_v2,<br/> D13_v2,<br/>D14_v2 |  |

### <a name="brazil-south-and-japan-west-only"></a>Nur „Brasilien, Süden“ und „Japan, Westen“

| Clustertyp | Hadoop | hbase | Interactive Query | Storm | Spark | ML Services |
|---|---|---|---|---|---|---|
| Head – Standard-VM-Größe | D12 | D12 | D13 | A4_v2 | D12 | D12 |
| Head – empfohlene VM-Größen | D5_v2,<br/> D13_v2,<br/> D14_v2 | D3_v2,<br/> D4_v2,<br/> D12_v2 | D13_v2,<br/> D14_v2 | A4_v2,<br/> A8_v2 | D12_v2,<br/> D13_v2,<br/> D14_v2 | D12_v2,<br/> D13_v2,<br/> D14_v2 |
| Worker – Standard-VM-Größe | D4 | D4 | D14 | D3 | D13 | D4 |
| Worker – empfohlene VM-Größen | D5_v2,<br/> D12_v2,<br/> D13_v2 | D3_v2,<br/> D4_v2,<br/> D13_v2 | D13_v2,<br/> D14_v2 | D3_v2,<br/> D4_v2,<br/> D12_v2 | D12_v2,<br/> D13_v2,<br/> D14_v2 | D4_v2,<br/> D12_v2,<br/> D13_v2,<br/> D14_v2 |
| Zookeeper – Standard-VM-Größen |  | A4_v2 | A4_v2 | A4_v2 |  | A2_v2 |
| Zookeeper – empfohlene VM-Größen |  | A4_v2,<br/> A8_v2,<br/> A2m_v2 | A4_v2,<br/> A8_v2,<br/> A2m_v2 | A4_v2,<br/> A8_v2 |  | A2_v2 |
| ML Services: VM-Standardgrößen |  |  |  |  |  | D4 |
| ML Services: empfohlene VM-Größen |  |  |  |  |  | D4_v2,<br/> D12_v2,<br/> D13_v2,<br/> D14_v2 |

> [!NOTE]
> - Head wird für den Storm-Clustertyp als *Nimbus* bezeichnet.
> - Worker wird für den Storm-Clustertyp als *Supervisor* bezeichnet.
> - Worker wird für den HBase-Clustertyp als *Region* bezeichnet.

## <a name="next-steps"></a>Nächste Schritte

* [Welche Apache Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md)
