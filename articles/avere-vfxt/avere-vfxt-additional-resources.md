---
title: Weitere Links zu Avere vFXT für Azure
description: Links zu weiteren Informationen zu Avere vFXT für Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: rohogue
ms.openlocfilehash: c8ac08e3d03e8eb525cad7d73bece40c515e31a1
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72256294"
---
# <a name="additional-documentation"></a>Zusätzliche Dokumentation

Dieser Artikel enthält Links zu zusätzlichen Dokumentationen zur Verwaltungsoberfläche der Avere-Systemsteuerung und zu verwandten Themen. 

## <a name="avere-cluster-documentation"></a>Avere-Clusterdokumentation

Weitere Dokumente zum Avere-Cluster finden Sie auf der Website unter <https://azure.github.io/Avere/>. Diese Dokumente können Ihnen helfen, die Funktionen des Clusters und die Konfiguration seiner Einstellungen zu verstehen. 

* Das [Handbuch zur FXT-Clustererstellung](<https://azure.github.io/Avere/#fxt_cluster>) ist für Cluster konzipiert, die aus physischen Hardwareknoten bestehen, aber einige Informationen im Dokument sind auch für vFXT-Cluster relevant. Insbesondere neue vFXT-Clusteradministratoren können von den folgenden Abschnitten profitieren:
  * In [Anpassen von Support- und Überwachungseinstellungen](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_support.html#config-support>) wird erläutert, wie Sie die Uploadeinstellungen des Supports anpassen und die Remoteüberwachung aktivieren. 
  * [Konfigurieren von VServern und globalen Namespaces](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_vserver.html#config-vserver>) enthält Informationen zum Erstellen eines clientseitigen Namespace.
  * In [Konfigurieren von DNS für den Avere-Cluster](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_network.html#dns-overview>) wird erläutert, wie Sie Roundrobin-DNS konfigurieren.
  * In [Hinzufügen von Back-End-Speicher](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_core_filer.html#add-core-filer>) wird beschrieben, wie Sie Kernspeichereinheiten hinzuzufügen.

* Das [Handbuch zur Clusterkonfiguration](<https://azure.github.io/Avere/#operations>) ist eine vollständige Referenz der Einstellungen und Optionen für einen Avere-Cluster. Ein vFXT-Cluster verwendet eine Teilmenge dieser Optionen, aber die meisten der Konfigurationsseiten gelten dennoch.

* Im [Dashboard-Handbuch](<https://azure.github.io/Avere/#operations>) wird erläutert, wie die Features zur Clusterüberwachung der Avere-Systemsteuerung verwendet werden.

## <a name="vfxt-creation-and-management-documentation"></a>Dokumentation zur Erstellung und Verwaltung von vFXT

Eine vollständige Anleitung zur Verwendung von vfxt.py, dem Hilfsprogramm zur Erstellung und Verwaltung von Cloudclustern, finden Sie bei GitHub: [Cloud cluster management with vfxt.py](https://github.com/Azure/AvereSDK/blob/master/docs/README.md) (Cloudclusterverwaltung mit vfxt.py).  
