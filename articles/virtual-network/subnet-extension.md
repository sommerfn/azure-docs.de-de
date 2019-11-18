---
title: Subnetzerweiterung in Azure | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Subnetzerweiterung in Azure.
services: virtual-network
documentationcenter: na
author: anupam-p
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2019
ms.author: anupand
ms.openlocfilehash: f718471c3f79e9a33b0e03b088f8c8d2ae0231d3
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73586847"
---
# <a name="subnet-extension"></a>Subnetzerweiterung
Die Workloadmigration in die öffentliche Cloud erfordert sorgfältige Planung und Koordination. Eine der wichtigsten Überlegungen ist dabei die Möglichkeit, Ihre IP-Adressen beizubehalten. Dieser Punkt kann insbesondere dann wichtig sein, wenn Ihre Anwendungen von der IP-Adresse abhängig sind oder Sie Complianceanforderungen erfüllen müssen, um bestimmte IP-Adressen zu verwenden. Azure Virtual Network löst dieses Problem für Sie, da Sie damit VNET und Subnetze mit einem IP-Adressbereich Ihrer Wahl erstellen können.

Migrationen können etwas schwierig werden, wenn nicht nur die oben genannten Anforderung zu erfüllen ist, sondern auch noch einige Anwendungen in Ihrer lokalen Umgebung bleiben müssen. In einer solchen Situation müssen Sie die Anwendungen zwischen Azure und der lokalen Umgebung aufteilen, ohne die IP-Adressen auf einer Seite neu zu nummerieren. Außerdem müssen die Anwendungen so kommunizieren können, als ob sie sich im selben Netzwerk befinden.

Eine Lösung für das oben genannte Problem ist die Subnetzerweiterung. Durch die Erweiterung eines Netzwerks können Anwendungen über dieselbe Broadcastdomäne kommunizieren, wenn sie an verschiedenen physischen Standorten gespeichert sind, sodass die Netzwerktopologie nicht umstrukturiert werden muss. 

Die Erweiterung Ihres Netzwerks ist im Allgemeinen keine bewährte Vorgehensweise, aber in den folgenden Anwendungsfälle möglicherweise erforderlich.

- **Stufenweise Migration**: Das häufigste Szenario ist, dass die Migration in einzelnen Phasen durchführt werden soll. Sie können zunächst einige Anwendungen zu Azure migrieren und dann im Laufe der Zeit den Rest.
- **Latenz**: Die Notwendigkeit einer niedrigen Latenz kann ein weiterer Grund dafür sein, einige Anwendungen lokal zu speichern, um eine größtmögliche Nähe zum Rechenzentrum zu gewährleisten.
- **Compliance**: Ein weiterer Anwendungsfall besteht darin, dass es möglicherweise aufgrund der Compliance erforderlich ist, einige Ihrer Anwendungen in Ihrer lokalen Umgebung zu behalten.
 
> [!NOTE] 
> Sie sollten ihre Subnetze nur dann erweitern, wenn dies erforderlich ist. Wenn Sie Ihre Subnetze erweitern, sollten Sie versuchen, einen Zwischenschritt auszuführen. Im Laufe der Zeit sollten Sie versuchen, die Anwendungen in Ihrem lokalen Netzwerk neu zu nummerieren und Sie zu Azure zu migrieren.

Im nächsten Abschnitt wird erläutert, wie Sie Ihre Subnetze zu Azure erweitern können.


## <a name="extend-your-subnet-to-azure"></a>Erweitern ihres Subnetzes zu Azure
 Sie können Ihre lokalen Subnetze mithilfe einer netzwerkbasierten Layer-3-Overlaylösung zu Azure erweitern. Die meisten Lösungen verwenden eine Overlaytechnologie wie VXLAN, um das Layer-2-Netzwerk mithilfe eines Layer-3-Overlaynetzwerks zu erweitern. Das folgende Diagramm zeigt eine allgemeine Lösung. In dieser Lösung ist auf beiden Seiten das gleiche Subnetz vorhanden – in Azure und in der lokalen Umgebung. 

![Beispiel für eine Subnetzerweiterung](./media/subnet-extension/subnet-extension.png)

Die IP-Adressen aus dem Subnetz werden VMs in Azure und in der lokalen Umgebung zugewiesen. Sowohl in Azure als auch in der lokalen Umgebung ist ein virtuelles Netzwerkgerät in den Netzwerken eingefügt. Wenn eine VM in Azure versucht, mit einer VM im lokalen Netzwerk zu kommunizieren, erfasst das virtuelle Azure-Netzwerkgerät das Paket, kapselt es und sendet es über VPN/ExpressRoute an das lokale Netzwerk. Das lokale virtuelle Netzwerkgerät empfängt das Paket, entkapselt es und leitet es an den vorgesehenen Empfänger in seinem Netzwerk weiter. Der zurückkommende Datenverkehr verwendet einen ähnlichen Pfad und eine ähnliche Logik.

Im Beispiel oben kommunizieren das virtuelle Azure-Netzwerkgerät und das lokale virtuelle Netzwerkgerät miteinander und tauschen Informationen zu den IP-Adressen hinter den jeweiligen Komponenten aus. Komplexere Netzwerke können auch über einen Zuordnungsdienst verfügen, der die Zuordnung zwischen den virtuellen Netzwerkgeräten und dahinter befindlichen IP-Adressen aufrechterhält. Wenn ein virtuelles Netzwerkgerät ein Paket erhält, fragt es über den Zuordnungsdienst die Adresse der NVA-Komponente ab, hinter der sich die IP-Zieladresse befindet.

Im nächsten Abschnitt finden Sie weitere Informationen zu den Lösungen für die Subnetzerweiterung, die wir in Azure getestet haben.

## <a name="next-steps"></a>Nächste Schritte 
[Erweitern Ihres Netzwerks zu Azure mithilfe von Anbieterlösungen.](https://github.com/microsoft/Azure-LISP)