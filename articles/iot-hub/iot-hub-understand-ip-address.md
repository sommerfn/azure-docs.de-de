---
title: Grundlegendes zur IP-Adresse Ihres IoT-Hubs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die IP-Adresse Ihres IoT-Hubs und die zugehörigen Eigenschaften abfragen. Die IP-Adresse Ihres IoT-Hubs kann sich bei verschiedenen Szenarien wie z.B. einer Notfallwiederherstellung oder einem regionalen Failover ändern.
author: philmea
ms.author: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: 9c9d02eeead4ccbed372e9a55d9b551da83caa39
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68641591"
---
# <a name="understanding-the-ip-address-of-your-iot-hub"></a>Grundlegendes zur IP-Adresse Ihres IoT-Hubs

Die IP-Adresse Ihres IoT-Hubs ist ein öffentlicher Endpunkt für Ihren Hub mit Lastenausgleich, KEINE dedizierte IP-Adresse. Die Adresse wird durch den Netzwerkadressbereich der Azure-Region bestimmt, in der sie bereitgestellt wird. Diese IP-Adresse kann sich ohne vorherige Ankündigung ändern. Die IP-Adresse Ihres IoT-Hubs kann sich beispielsweise bei Aktualisierungen des Rechenzentrumsnetzwerks, bei einer Notfallwiederherstellung des Rechenzentrums oder bei einem regionalen Failover eines IoT-Hubs ändern. Weitere Informationen zum regionalen Failover und zur Verfügbarkeit von Azure IoT Hub-Instanzen finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für Azure IoT Hub](iot-hub-ha-dr.md).

Die IP-Adresse Ihres IoT-Hubs ändert sich nach einem Failover in eine andere Region. Sie können diese Funktionalität testen, indem Sie das Tutorial [Ausführen eines manuellen Failovers für einen IoT-Hub](tutorial-manual-failover.md) absolvieren.

## <a name="discover-your-iot-hub-ip-address"></a>Ermitteln der IP-Adresse Ihres IoT-Hubs

Sie können die IP-Adresse Ihres IoT-Hubs mit einem DNS-Reverse-Lookup des CNAME-Eintrags ([*Name-des-IoT-Hubs*].azure-devices.net) ermitteln. Sie können **nslookup** verwenden, um die IP-Adresse eines IoT-Hubs zu überprüfen:

```cmd/sh
nslookup {YourIoTHubName}.azure-devices.net
```

Diese IP-Adresse kann sich ohne vorherige Ankündigung ändern. Im Fall eines Failovers oder einer Notfallwiederherstellung müssen Sie die IP-Adresse Ihres IoT-Hubs in der sekundären Region abfragen.

## <a name="outbound-firewall-rules-for-iot-hub"></a>Ausgehende Firewallregeln für IoT-Hubs

Versuchen Sie, Firewallregeln und -filter basierend auf dem Hostnamen oder der Domäne des IoT-Hubs zu erstellen. Wenn Sie nur ausgehenden Datenverkehr an bestimmte Adressen zulassen können, fragen Sie die IP-Adresse Ihres IoT-Hubs regelmäßig ab, und aktualisieren Sie Ihre Firewallregeln.

