---
title: Was ist ein private Azure DNS-Zone?
description: Übersicht über eine private DNS-Zone
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 9/24/2019
ms.author: rohink
ms.openlocfilehash: 462c873221a4bdc622a9b118b6699a9719a5f88d
ms.sourcegitcommit: 4d177e6d273bba8af03a00e8bb9fe51a447196d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963734"
---
# <a name="what-is-a-private-azure-dns-zone"></a>Was ist eine private Azure DNS-Zone?

Privates Azure-DNS bietet einen zuverlässigen, sicheren DNS-Dienst zum Verwalten und Auflösen von Domänennamen in einem virtuellen Netzwerk, ohne dass Sie eine benutzerdefinierte DNS-Lösung hinzufügen müssen. Durch die Verwendung privater DNS-Zonen können Sie anstelle der momentan verfügbaren von Azure bereitgestellten Namen Ihre eigenen benutzerdefinierten Domänennamen verwenden. 

Die in einer privaten DNS-Zone enthaltenen Einträge sind nicht aus dem Internet auflösbar. Die DNS-Auflösung für eine private DNS-Zone funktioniert nur aus virtuellen Netzwerken, die damit verknüpft sind.

Sie können eine private DNS-Zone mit einem oder mehreren virtuellen Netzwerken verknüpfen, indem Sie [virtuelle Netzwerkverbindungen](./private-dns-virtual-network-links.md) erstellen.
Sie können auch die [automatische Registrierung](./private-dns-autoregistration.md)sfunktion aktivieren, um den Lebenszyklus der DNS-Einträge für die virtuellen Computer, die in einem virtuellen Netzwerk bereitgestellt sind, automatisch zu verwalten.

## <a name="limits"></a>Einschränkungen

Informationen dazu, wie viele private DNS-Zonen Sie in einem Abonnement erstellen können und wie viele Eintragssätze in einer privaten DNS-Zone unterstützt werden, finden Sie unter [Azure DNS-Limits](https://docs.microsoft.com/azure/azure-subscription-service-limits#azure-dns-limits).

## <a name="restrictions"></a>Einschränkungen

* Private DNS-Zonen mit einzelner Bezeichnung werden nicht unterstützt. Ihre private DNS-Zone muss über mindestens zwei Bezeichnungen verfügen. Beispielsweise besitzt „contoso.com“ zwei Bezeichnungen, die durch einen Punkt getrennt sind. Eine private DNS-Zone kann maximal 34 Bezeichnungen aufweisen.
* Sie können keine Zonendelegierungen (NS-Einträge) in einer privaten DNS-Zone erstellen. Wenn Sie beabsichtigen, eine untergeordnete Domäne zu verwenden, können Sie die Domäne direkt als private DNS-Zone erstellen und sie mit dem virtuellen Netzwerk verknüpfen, ohne eine Namenserverdelegierung aus der übergeordneten Zone einrichten zu müssen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie über [Azure PowerShell](./private-dns-getstarted-powershell.md) oder [Azure CLI](./private-dns-getstarted-cli.md) eine private Zone in Azure DNS erstellen.

* Informieren Sie sich über einige gängige [Szenarien zu privaten Zonen](./private-dns-scenarios.md), die mit privaten Zonen in Azure DNS realisiert werden können.

* Allgemeine Fragen und Antworten zu privaten Zonen in Azure DNS, einschließlich des spezifischen Verhaltens für bestimmte Vorgänge, finden Sie unter [Häufig gestellte Fragen zu privatem Azure DNS](./dns-faq-private.md).
