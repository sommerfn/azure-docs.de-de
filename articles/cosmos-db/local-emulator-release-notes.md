---
title: 'Azure Cosmos-Emulator: Download und Versionshinweise'
description: Lesen Sie die Versionshinweise zum Azure Cosmos-Emulator, und laden Sie ihn herunter.
ms.service: cosmos-db
ms.topic: tutorial
author: markjbrown
ms.author: mjbrown
ms.date: 06/20/2019
ms.openlocfilehash: 5985d0d82341c76993ee91b8dff6927edd1ed8b4
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332151"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Verwenden des Azure Cosmos-Emulators für lokale Entwicklungs- und Testvorgänge

Dieser Artikel zeigt die Versionshinweise zum Azure Cosmos-Emulator mit einer Liste der Featureupdates, die in den Versionen vorgenommen wurden. Auch die zum Herunterladen und Verwenden verfügbare neueste Version des Emulators wird aufgeführt.

## <a name="download"></a>Download

| | |
|---------|---------|
|**MSI-Download**|[Microsoft Download Center](https://aka.ms/cosmosdb-emulator)|
|**Erste Schritte**|[Lokale Entwicklung mit dem Azure Cosmos-Emulator](local-emulator.md)|

## <a name="release-notes"></a>Versionshinweise

### <a name="243"></a>2.4.3

- Das Starten des MongoDB-Diensts ist standardmäßig deaktiviert. Nur der SQL-Endpunkt ist standardmäßig aktiviert. Der Benutzer muss den Endpunkt manuell über die Befehlszeilenoption „/EnableMongoDbEndpoint“ des Emulators starten. Jetzt ist es wie bei allen anderen Dienstendpunkten wie Gremlin, Cassandra und Table.
- Ein Fehler im Emulator beim Start mit „/AllowNetworkAccess“ wurde behoben, bei dem die Endpunkte Gremlin, Cassandra und Table Anfragen von externen Clients nicht korrekt behandelten.
- Direkte Verbindungsports wurden den Einstellungen für Firewallregeln hinzugefügt.

### <a name="240"></a>2.4.0

- Es wurde das Problem behoben, dass der Emulator nicht gestartet werden konnte, wenn Netzwerküberwachungs-Apps wie Pulse Client auf dem Hostcomputer vorhanden waren.
