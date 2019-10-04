---
title: 'Azure VMware-Lösung von CloudSimple: Dienst'
description: Bietet eine Übersicht über den CloudSimple-Dienst und die zugehörigen Konzepte.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d128a248c2e6e1e2e35e3b633975ba081e77f028
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877655"
---
# <a name="cloudsimple-service-overview"></a>Übersicht über den CloudSimple-Dienst

Der CloudSimple-Dienst ermöglicht es Ihnen, die Azure-VMware-Lösung von CloudSimple zu nutzen.  Nach Erstellung des Diensts können Sie Knoten erwerben, Knoten reservieren und private Clouds erstellen.  Sie erstellen den CloudSimple-Dienst in jeder Azure-Region, in der der CloudSimple-Dienst verfügbar ist. Der Dienst definiert das Umkreisnetzwerk der Azure-VMware-Lösung von CloudSimple. Dieses Umkreisnetzwerk unterstützt unter anderem Dienste wie VPN, ExpressRoute und Internetkonnektivität mit Ihren privaten Clouds.

## <a name="gateway-subnet"></a>Gatewaysubnetz

Ein Gatewaysubnetz ist für jeden CloudSimple-Dienst erforderlich und für die Region spezifisch, in der es erstellt wird. Das Gatewaysubnetz wird beim Erstellen des Umkreisnetzwerks verwendet und erfordert einen CIDR-Block vom Typ „/28“.  Der Adressraum des Gatewaysubnetzes muss eindeutig sein. Er darf sich nicht mit einem Netzwerk überschneiden, das mit der CloudSimple-Umgebung kommuniziert. Zu den Netzwerken, die mit CloudSimple kommunizieren, gehören unter anderem lokale Netzwerke und das virtuelle Azure-Netzwerk.  Ein Gatewaysubnetz kann nach der Erstellung nicht mehr gelöscht werden.  Das Gatewaysubnetz wird entfernt, wenn der Dienst gelöscht wird.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Erstellen eines CloudSimple-Diensts in Azure](quickstart-create-cloudsimple-service.md).
