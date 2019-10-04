---
title: Verbinden des virtuellen Azure-Netzwerks mit CloudSimple mithilfe von ExpressRoute
description: Beschreibt, wie Peeringinformationen für eine Verbindung zwischen dem virtuellen Azure-Netzwerk und Ihrer CloudSimple-Umgebung abgerufen werden.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 00d49d763dedc5d86557dadd10f5d727e7893dbe
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69563064"
---
# <a name="connect-azure-virtual-network-to-cloudsimple-using-expressroute"></a>Verbinden des virtuellen Azure-Netzwerks mit CloudSimple mithilfe von ExpressRoute

Sie können Ihr privates Cloudnetzwerk auf Ihr virtuelles Azure-Netzwerk und Azure-Ressourcen erweitern. Eine ExpressRoute-Verbindung ermöglicht Ihnen den Zugriff auf verschiedene Ressourcen, die in Ihrem Azure-Abonnement über Ihre private Cloud ausgeführt werden.

## <a name="request-authorization-key"></a>Anfordern eines Autorisierungsschlüssels

Ein Autorisierungsschlüssel ist für die ExpressRoute-Verbindung zwischen Ihrer privaten Cloud und dem virtuellen Azure-Netzwerk erforderlich. Um einen Schlüssel zu erhalten, müssen Sie ein <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">Supportticket</a> einreichen.  Verwenden Sie die folgenden Informationen für die Anforderung:

* Problemtyp: **Technisch**
* Abonnement: **Wählen Sie das Abonnement aus, in dem der CloudSimple-Dienst bereitgestellt wird.**
* Dienst: **VMware-Lösung von CloudSimple**
* Problemtyp: **Dienstanforderung**
* Problemuntertyp: **Autorisierungsschlüssel für Azure VNET-Verbindung**
* Antragsteller: **Anforderung eines Autorisierungsschlüssels für Azure VNET-Verbindung**

## <a name="obtain-peering-information-for-azure-virtual-network-to-cloudsimple-connection"></a>Abrufen von Peeringinformationen für eine Verbindung zwischen dem virtuellen Azure-Netzwerk und CloudSimple

Zum Einrichten der Verbindung müssen Sie eine Verbindung zwischen dem virtuellen Azure-Netzwerk und ihrer CloudSimple-Umgebung herstellen.  Als Teil dieses Verfahrens müssen Sie den Peerleitungs-URI und den Autorisierungsschlüssel angeben. Rufen Sie den URI und den Autorisierungsschlüssel aus dem [CloudSimple-Portal](access-cloudsimple-portal.md) ab.  Wählen Sie im seitlichen Menü **Network** (Netzwerk) aus, und wählen Sie dann **Azure Network Connection** (Azure-Netzwerkverbindung) aus. Oder wählen Sie im seitlichen Menü **Account** (Konto) aus, und wählen Sie dann **Azure Network Connection** (Azure-Netzwerkverbindung) aus.

Beachten Sie die Kopiersymbole für den Peerleitungs-URI und für den Autorisierungsschlüssel für jede der Regionen. Gehen Sie für jede private Cloud, die Sie verbinden möchten, folgendermaßen vor:

* Klicken Sie auf **Copy** (Kopieren), um den URI zu kopieren. Fügen Sie ihn in eine Datei ein, aus der er dem Azure-Portal hinzugefügt werden kann.  
* Klicken Sie auf **Copy** (Kopieren), um den Autorisierungsschlüssel zu kopieren und ebenfalls in die Datei einzufügen.

![Seite „Verbindung mit virtuellem Netzwerk“](media/network-virt-conn-page.png)

Weitere Informationen zum Einrichten der Verbindung zwischen dem virtuellen Azure-Netzwerk und CloudSimple finden Sie unter [Herstellen einer Verbindung aus Ihrer privaten CloudSimple-Cloudumgebung mit dem virtuellen Azure-Netzwerk mithilfe von ExpressRoute](azure-expressroute-connection.md).
