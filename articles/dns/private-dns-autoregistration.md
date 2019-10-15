---
title: Was ist die Funktion zur automatischen Registrierung von privaten Azure DNS-Zonen?
description: Übersicht über die Funktion zur automatischen Registrierung von privaten Azure DNS-Zonen
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 9/24/2019
ms.author: rohink
ms.openlocfilehash: 9d1854b459e799d5cbb401de9ac717dd7d0fde1d
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963714"
---
# <a name="what-is-the-autoregistration-feature-of-azure-dns-private-zones"></a>Was ist die Funktion zur automatischen Registrierung von privaten Azure DNS-Zonen?

Die Funktion zur automatischen Registrierung von privaten Azure DNS-Zonen gestaltet die Verwaltung von DNS-Einträgen für virtuelle Computer, die in einem virtuellen Netzwerk bereitgestellt sind, mühelos. Wenn Sie [ein virtuelles Netzwerk mit einer privaten DNS-Zone verknüpfen](./private-dns-virtual-network-links.md) und die automatische Registrierung für alle virtuellen Computer aktivieren, werden die DNS-Einträge für die im virtuellen Netzwerk bereitgestellten virtuellen Computer automatisch in der privaten DNS-Zone erstellt. Zusätzlich zu den Forward-Lookup-Einträgen (A-Einträge) werden ebenfalls Reverse-Lookup-Einträge (PTR-Einträge) automatisch für die virtuellen Computer erstellt.
Wenn Sie dem virtuellen Netzwerk weitere virtuelle Computer hinzufügen, werden die DNS-Einträge für diese virtuellen Computer ebenfalls automatisch in der verknüpften privaten DNS-Zone erstellt.

Wenn Sie einen virtuellen Computer löschen, werden die DNS-Einträge für den virtuellen Computer automatisch in der verknüpften privaten DNS-Zone gelöscht.

Sie können die automatische Registrierung aktivieren, indem Sie beim Erstellen einer virtuellen Netzwerkverbindung die Option „Automatische Registrierung aktivieren“ auswählen.

![Automatische Registrierung aktivieren](./media/privatedns-concepts/enable-autoregistration.png)

## <a name="restrictions"></a>Einschränkungen

* Die automatische Registrierung funktioniert nur für virtuelle Computer. Für alle anderen Ressourcen wie interne Lastenausgleichsmodule usw., können Sie DNS-Einträge manuell in der privaten DNS-Zone erstellen, die mit dem virtuellen Netzwerk verknüpft ist.
* DNS-Einträge werden nur für die primäre NIC des virtuellen Computers automatisch erstellt. Wenn Ihre virtuellen Computer über mehr als eine Netzwerkkarte verfügen, können Sie die DNS-Einträge für andere Netzwerkschnittstellen manuell erstellen.
* Automatische Registrierung für IPv6 (AAAA-Einträge) wird nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie über [Azure PowerShell](./private-dns-getstarted-powershell.md) oder die [Azure-Befehlszeilenschnittstelle](./private-dns-getstarted-cli.md) eine private Zone in Azure DNS erstellen.

* Informieren Sie sich über einige gängige [Szenarien zu privaten Zonen](./private-dns-scenarios.md), die mit privaten Zonen in Azure DNS realisiert werden können.

* Allgemeine Fragen und Antworten zu privaten Zonen in Azure DNS, einschließlich des spezifischen Verhaltens für bestimmte Vorgänge, finden Sie unter [Häufig gestellte Fragen zu privatem Azure DNS](./dns-faq-private.md).
