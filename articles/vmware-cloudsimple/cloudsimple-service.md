---
title: VMware-Lösung von CloudSimple-Dienst
description: Bietet eine Übersicht über den CloudSimple-Dienst und die zugehörigen Konzepte.
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6a4c0bc6070d372a279b74f81ac1f84f565559c3
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165246"
---
# <a name="cloudsimple-service-overview"></a>Übersicht über den CloudSimple-Dienst

Mit dem CloudSimple-Dienst können Sie die Azure-VMware-Lösung von CloudSimple verwenden. Nachdem Sie den Dienst erstellt haben, können Sie Knoten bereitstellen, Knoten reservieren und private Clouds erstellen. Sie erstellen den CloudSimple-Dienst in jeder Azure-Region, in der der CloudSimple-Dienst verfügbar ist. Der Dienst definiert das Umkreisnetzwerk der Azure-VMware-Lösung von CloudSimple. Dieses Umkreisnetzwerk unterstützt unter anderem Dienste wie VPN, ExpressRoute und Internetkonnektivität mit Ihren privaten Clouds.

## <a name="gateway-subnet"></a>Gatewaysubnetz

Ein Gatewaysubnetz ist für jeden CloudSimple-Dienst erforderlich und für die Region spezifisch, in der es erstellt wird. Das Gatewaysubnetz wird verwendet, wenn Sie das Umkreisnetzwerk erstellen, und benötigt einen /28-CIDR-Block. Der Adressraum des Gatewaysubnetzes muss eindeutig sein. Er darf sich nicht mit einem Netzwerk überschneiden, das mit der CloudSimple-Umgebung kommuniziert. Zu den Netzwerken, die mit CloudSimple kommunizieren, gehören unter anderem lokale Netzwerke und virtuelle Azure-Netzwerke. Ein Gatewaysubnetz kann nach seiner Erstellung nicht gelöscht werden. Das Gatewaysubnetz wird entfernt, wenn der Dienst gelöscht wird.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich über das [Erstellen eines CloudSimple-Diensts in Azure](quickstart-create-cloudsimple-service.md).
