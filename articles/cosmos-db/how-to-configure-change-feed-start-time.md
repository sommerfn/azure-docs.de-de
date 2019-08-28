---
title: Konfigurieren der Startzeit des Änderungsfeedprozessors – Azure Cosmos DB
description: Hier erfahren Sie, wie Sie den Änderungsfeedprozessor so konfigurieren, dass er an einem bestimmten Datum und zu einer bestimmten Uhrzeit mit dem Lesen von Daten beginnt.
author: ealsur
ms.service: cosmos-db
ms.topic: sample
ms.date: 08/13/2019
ms.author: maquaran
ms.openlocfilehash: 3ad9aff10b3bae36a173f776f55a434fd8b38b98
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69574271"
---
# <a name="how-to-configure-the-change-feed-processor-start-time"></a>Konfigurieren der Startzeit des Änderungsfeedprozessors

In diesem Artikel wird beschrieben, wie Sie den [Änderungsfeedprozessor](./change-feed-processor.md) so konfigurieren, dass er an einem bestimmten Datum und zu einer bestimmten Uhrzeit mit dem Lesen von Daten beginnt.

## <a name="default-behavior"></a>Standardverhalten

Wenn ein Änderungsfeedprozessor zum ersten Mal gestartet wird, initialisiert er den Leasecontainer und startet seinen [Verarbeitungslebenszyklus](./change-feed-processor.md#processing-life-cycle). Änderungen, die im Container aufgetreten sind, bevor der Änderungsfeedprozessor zum ersten Mal initialisiert wurde, werden nicht erkannt.

## <a name="reading-from-a-previous-date-and-time"></a>Lesen ab einem früheren Datum und einer früheren Uhrzeit

Es ist möglich, den Änderungsfeedprozessor so zu initialisieren, dass er **an einem bestimmten Datum und zu einer bestimmten Uhrzeit** mit dem Lesen von Daten beginnt. Hierfür muss eine Instanz von `DateTime` an die `WithStartTime`-Generator-Erweiterung übergeben werden:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=TimeInitialization)]

Der Änderungsfeedprozessor wird an diesem angegeben Datum und zu dieser angegebenen Uhrzeit initialisiert und beginnt mit dem Lesen der Änderungen, die danach auftreten.

## <a name="reading-from-the-beginning"></a>Lesen ab Anfang

In anderen Szenarien wie Datenmigrationen oder Analysen des gesamten Verlaufs eines Containers muss der Änderungsfeed vom **Anfang der Lebensdauer dieses Containers** gelesen werden. Zu diesem Zweck verwenden wir `WithStartTime` in der Generator-Erweiterung, übergeben aber `DateTime.MinValue.ToUniversalTime()`, wodurch die UTC-Darstellung des Mindestwerts von `DateTime` generiert wird. Beispiel:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartFromBeginningInitialization)]

Der Änderungsfeedprozessor wird initialisiert und beginnt mit dem Lesen von Änderungen ab dem Anfang der Lebensdauer des Containers.

> [!NOTE]
> Diese Anpassungsoptionen dienen nur dazu, den Startzeitpunkt für den Änderungsfeedprozessor festzulegen. Sobald der Leasecontainer zum ersten Mal initialisiert wurde, hat eine Änderung dieser Optionen keine Auswirkungen mehr.

> [!NOTE]
> Beim Angeben eines Zeitpunkts werden nur Änderungen für Elemente gelesen, die zu diesem Zeitpunkt im Container vorhanden sind. Wenn ein Element gelöscht wurde, wird dessen Verlauf im Änderungsfeed ebenfalls entfernt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [Verwendungsbeispiele auf GitHub](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/usage/changefeed)
* [Zusätzliche Beispiele auf GitHub](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über den Änderungsfeedprozessor:

* [Änderungsfeedprozessor in Azure Cosmos DB](change-feed-processor.md)
* [Use the change feed estimator](how-to-use-change-feed-estimator.md) (Verwenden des Änderungsfeed-Estimators)
