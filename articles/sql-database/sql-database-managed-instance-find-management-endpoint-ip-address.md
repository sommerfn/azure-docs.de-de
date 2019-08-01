---
title: Ermitteln eines Verwaltungsendpunkts für eine verwaltete Azure SQL-Datenbank-Instanz | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die öffentliche IP-Adresse eines Verwaltungsendpunkts für eine verwaltete Azure SQL-Datenbank-Instanz abrufen und den integrierten Firewallschutz überprüfen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: c5304c62b29d842f9beeadb34eba1cb53048d179
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302277"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Ermitteln der IP-Adresse des Verwaltungsendpunkts

Der virtuelle Cluster der verwalteten Azure SQL-Datenbank-Instanz enthält einen Verwaltungsendpunkt, der von Microsoft für Verwaltungsvorgänge verwendet wird. Der Verwaltungsendpunkt ist durch eine integrierte Firewall auf Netzwerkebene und eine gegenseitige Zertifikatüberprüfung auf Anwendungsebene geschützt. Sie können die IP-Adresse des Verwaltungsendpunkts ermitteln, aber Sie können nicht auf diesen Endpunkt zugreifen.

Führen Sie einen DNS-Lookup für den vollqualifizierten Domänennamen `mi-name.zone_id.database.windows.net` Ihrer verwalteten Instanz durch, um die Verwaltungs-IP-Adresse zu ermitteln. Dadurch wird ein DNS-Eintrag zurückgegeben, der `trx.region-a.worker.vnet.database.windows.net` ähnelt. Anschließend können Sie einen DNS-Lookup für diesen vollqualifizierten Domänennamen ohne „.vnet“ durchführen. Dadurch wird die Verwaltungs-IP-Adresse zurückgegeben. 

Der folgende PowerShell-Befehl kann all das für Sie tun, wenn Sie \<MI FQDN\> durch den DNS-Eintrag Ihrer verwalteten Instanz (`mi-name.zone_id.database.windows.net`) ersetzen:
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Weitere Informationen zu verwalteten Instanzen und zur Konnektivität finden Sie unter [Azure SQL Database Managed Instance Connectivity Architecture](sql-database-managed-instance-connectivity-architecture.md) (Konnektivitätsarchitektur der verwalteten Azure SQL-Datenbank-Instanz).
