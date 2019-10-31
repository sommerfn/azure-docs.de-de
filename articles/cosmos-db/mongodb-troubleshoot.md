---
title: Behandeln häufiger Fehler in der Azure Cosmos DB-API für MongoDB
description: In diesem Dokument erfahren Sie, wie Sie häufige Probleme in der Azure Cosmos DB-API für MongoDB behandeln.
author: roaror
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: roaror
ms.openlocfilehash: ece975fa37e500b1c160210684a0cb46e719c48b
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72754971"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>Behandeln häufiger Probleme in der Azure Cosmos DB-API für MongoDB

Azure Cosmos DB implementiert die Wire Protocols gängiger NoSQL-Datenbanken (einschließlich MongoDB). Dank der Wire Protocol-Implementierung können Sie unter Verwendung der vorhandenen Client-SDKs, Treiber und Tools, die für NoSQL-Datenbanken geeignet sind, auf transparente Weise mit Azure Cosmos DB interagieren. Azure Cosmos DB verwendet keinerlei Quellcode der Datenbanken, um Wire Protocol-kompatible APIs für NoSQL-Datenbanken bereitzustellen. Jeder MongoDB-Clienttreiber, der diese Wire Protocol-Versionen unterstützt, kann eine Verbindung mit Azure Cosmos DB herstellen.

Die Azure Cosmos DB-API für MongoDB ist zwar mit Version 3.2 des Wire Protocols von MongoDB kompatibel (die in Version 3.4 hinzugefügten Abfrageoperatoren und Features sind aktuell als Vorschauversion verfügbar), es gibt jedoch einige spezielle Fehlercodes für Azure Cosmos DB-spezifische Fehler. In diesem Artikel werden verschiedene Fehler, Fehlercodes und entsprechende Lösungsschritte beschrieben.

## <a name="common-errors-and-solutions"></a>Häufige Fehler und Lösungen

| Error               | Code  | BESCHREIBUNG  | Lösung  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | Die Gesamtanzahl der verbrauchten Anforderungseinheiten hat die bereitgestellte Anforderungseinheitenrate für die Sammlung überschritten und wurde gedrosselt. | Skalieren Sie über das Azure-Portal ggf. den Durchsatz, der einem Container oder einer Gruppe von Containern zugewiesen ist, oder wiederholen Sie den Vorgang. |
| ExceededMemoryLimit | 16501 | Der Vorgang ist ein mehrinstanzenfähiger Dienst und hat die Speicherzuweisung des Clients überschritten. | Verringern Sie den Umfang des Vorgangs mithilfe restriktiverer Abfragekriterien, oder wenden Sie sich im [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) an den Support. Beispiel: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| Der Indexpfad zum angegebenen ORDER BY-Element ist ausgeschlossen./Die ORDER BY-Abfrage verfügt nicht über einen entsprechenden zusammengesetzten Index für die Bereitstellung. | 2 | Die Abfrage fordert eine Sortierung für ein Feld an, das nicht indiziert ist. | Erstellen Sie einen übereinstimmenden Index (oder zusammengesetzten Index) für die durchzuführende Sortierabfrage. |
| Probleme mit der MongoDB-Wire-Version | - | Ältere Versionen von MongoDB-Treibern können den Namen des Azure Cosmos-Kontos in den Verbindungszeichenfolgen nicht erkennen. | Fügen Sie am Ende der Verbindungszeichenfolge Ihrer Azure Cosmos DB-API für MongoDB *appName=@**Kontoname**@* hinzu, und ersetzen Sie dabei ***Kontoname*** durch den Namen Ihres Cosmos DB-Kontos. |


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Studio 3T](mongodb-mongochef.md) mit der API für MongoDB von Azure Cosmos DB verwenden.
- Erfahren Sie, wie Sie [Robo 3T](mongodb-robomongo.md) mit der API für MongoDB von Azure Cosmos DB verwenden.
- Untersuchen Sie MongoDB-[Beispiele](mongodb-samples.md) mit der API für MongoDB von Azure Cosmos DB.

