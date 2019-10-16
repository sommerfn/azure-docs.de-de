---
title: Benutzerdefiniertes DNS für verwaltete Azure SQL-Datenbank-Instanzen | Microsoft-Dokumentation
description: In diesem Thema werden die Konfigurationsoptionen für ein benutzerdefiniertes DNS mit einer verwalteten Azure SQL-Datenbank-Instanz beschrieben.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 07/17/2019
ms.openlocfilehash: 9b15ebc40e99c1cd454396ccde5cca6b1a46abbc
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244760"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Konfigurieren eines benutzerdefinierten DNS für eine verwaltete Azure SQL-Datenbank-Instanz

In einem [virtuellen Azure-Netzwerk (VNET)](../virtual-network/virtual-networks-overview.md) muss eine verwaltete Azure SQL-Datenbank-Instanz bereitgestellt werden. Es gibt einige Szenarien (z. B. Datenbank-E-Mail, Verbindungsserver für andere SQL-Instanzen in Ihrer Cloud- oder Hybridumgebung), bei denen private Hostnamen von der verwalteten Instanz aufgelöst werden müssen. In diesem Fall müssen Sie einen benutzerdefinierten DNS in Azure konfigurieren. 

Da die verwaltete Instanz intern den gleichen DNS verwendet, muss der benutzerdefinierte DNS-Server so konfiguriert werden, dass er öffentliche Domänennamen auflösen kann.

> [!IMPORTANT]
> Verwenden Sie immer die vollqualifizierten Domänennamen (Fully-Qualified Domain Name, FQDN) für E-Mail-Server, die SQL-Server-Instanz und andere Dienste – auch wenn sie sich in Ihrer privaten DNS-Zone befinden. Verwenden Sie beispielsweise `smtp.contoso.com` für den E-Mail-Server, da `smtp` nicht ordnungsgemäß aufgelöst wird. Zum Erstellen eines Verbindungsservers oder einer Replikation mit Verweis auf virtuelle SQL-Computer im selben virtuellen Netzwerk sind ebenfalls ein vollqualifizierter Domänenname und ein Standard-DNS-Suffix erforderlich. Beispiel: `SQLVM.internal.cloudapp.net`. Weitere Informationen finden Sie unter [Namensauflösung mithilfe eines eigenen DNS-Servers](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

> [!IMPORTANT]
> Die Aktualisierung von VNET-DNS-Servern wirkt sich nicht sofort auf die verwaltete Instanz aus. Die DNS-Konfiguration der verwalteten Instanz wird nach Ablauf der DHCP-Lease oder nach dem Plattformupgrade (je nachdem, was zuerst eintritt) aktualisiert. **Benutzer sollten ihre VNET-DNS-Konfiguration festlegen, bevor sie ihre erste verwaltete Instanz erstellen.**

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Ein Tutorial, in dem die Erstellung einer neuen verwalteten Instanz gezeigt wird, finden Sie unter [Erstellen einer verwalteten Instanz](sql-database-managed-instance-get-started.md).
- Informationen zum Konfigurieren eines VNet für eine verwaltete Instanz finden Sie unter [Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connectivity-architecture.md).
