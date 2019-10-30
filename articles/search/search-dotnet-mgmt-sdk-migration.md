---
title: Upgrade auf Version 2 des Azure Search .NET Management SDK
titleSuffix: Azure Cognitive Search
description: Aktualisieren Sie von Vorgängerversionen auf Version 2 des Azure Search-.NET Management SDK. Hier finden Sie Informationen zu den Neuheiten und erfahren Sie, welche Änderungen am Code erforderlich sind.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: b81060bc3d8409a9ccb74ba85c85e6f779650ffd
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792423"
---
# <a name="upgrading-versions-of-the-azure-search-net-management-sdk"></a>Upgrade der Versionen des Azure Search .NET Management SDK

> [!Important]
> Dieser Inhalt ist noch in Bearbeitung. Version 3.0 des Azure Search-.NET Management SDK ist unter NuGet verfügbar. Wir arbeiten an der Ergänzung dieser Migrationsanleitung um eine Erläuterung, wie das Upgrade auf die neue Version durchgeführt wird. 
>

Wenn Sie die Version 1.0.2 oder eine frühere Version des [Azure Search-.NET Management SDK](https://aka.ms/search-mgmt-sdk) verwenden, unterstützt dieser Artikel Sie beim Upgrade Ihrer Anwendung auf Version 2.

Version 2 des Azure Search-.NET Management SDK enthält einige Änderungen gegenüber früheren Versionen. Dabei handelt es sich hauptsächlich um kleinere Änderungen, sodass zum Ändern Ihres Codes nur ein geringer Aufwand erforderlich sein sollte. Anweisungen zum Ändern Ihres Codes, um die neue SDK-Version zu verwenden, finden Sie unter [Schritte zum Upgrade](#UpgradeSteps) .

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2"></a>Neuerungen in Version 2
Version 2 des Azure Search-.NET Management SDK ist auf die gleiche allgemein verfügbare Version der Azure Search-Management-REST-API wie vorherige SDK-Versionen ausgerichtet, nämlich 2015-08-19. Die Änderungen am SDK sind ausschließlich clientseitige Änderungen zur Verbesserung der Verwendbarkeit des SDK selbst. Zu diesen Änderungen gehören folgende:

* `Services.CreateOrUpdate` und entsprechende asynchrone Versionen rufen jetzt automatisch den bereitstellenden `SearchService` ab und werden erst nach der Dienstbereitstellung abgeschlossen. Dadurch müssen Sie einen solchen Abrufcode nicht selbst schreiben.
* Wenn Sie die Dienstbereitstellung weiterhin manuell abrufen möchten, können Sie die neue `Services.BeginCreateOrUpdate`-Methode oder eine der entsprechenden asynchronen Versionen verwenden.
* Dem SDK wurden neue `Services.Update`-Methoden und entsprechende asynchrone Versionen hinzugefügt. Diese Methoden verwenden HTTP PATCH, um inkrementelle Aktualisierung eines Diensts zu unterstützen. Beispielsweise können Sie nun einen Dienst skalieren, indem Sie an diese Methoden eine `SearchService`-Instanz übergeben, die nur die gewünschten Eigenschaften `partitionCount` und `replicaCount` enthält. Die alte Variante, `Services.Get` aufzurufen und den zurückgegebenen `SearchService` zu ändern und an `Services.CreateOrUpdate` zu übergeben, wird weiterhin unterstützt, ist jedoch nicht mehr notwendig. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Schritte zum Upgrade
Aktualisieren Sie zunächst die NuGet-Referenz für `Microsoft.Azure.Management.Search` , indem Sie entweder die NuGet-Paket-Manager-Konsole verwenden oder mit der rechten Maustaste auf die Projektverweise klicken und in Visual Studio „NuGet-Pakete verwalten...“ auswählen.

Nachdem NuGet die neuen Pakete und deren Abhängigkeiten heruntergeladen hat, erstellen Sie Ihr Projekt neu. Je nachdem, wie Ihr Code strukturiert ist, verläuft die Neuerstellung erfolgreich. Wenn dies der Fall ist, sind Sie startbereit!

Wenn der Buildvorgang scheitert, kann dies daran liegen, dass Sie einige der SDK-Schnittstellen implementiert haben (z.B. für Komponententests), die geändert wurden. Zum Beheben dieses Problems müssen Sie die neuen Methoden implementieren, z.B. `BeginCreateOrUpdateWithHttpMessagesAsync`.

Sobald Sie alle Buildfehler behoben haben, können Sie Änderungen an Ihrer Anwendung vornehmen, um nach Bedarf die neue Funktionalität zu nutzen. Neue Features im SDK werden ausführlich unter [Neuerungen in Version 2](#WhatsNew) behandelt.

## <a name="conclusion"></a>Zusammenfassung
Wir freuen uns auf Ihr Feedback zum SDK! Wenn Probleme auftreten, können Sie sich gerne über das [Azure Search-MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch)an uns wenden. Wenn Sie einen Fehler finden, können Sie das Problem im [Azure .NET SDK-GitHub-Repository](https://github.com/Azure/azure-sdk-for-net/issues)melden. Stellen Sie sicher, dass Sie dem Titel des Problems „[Azure Search]“ voranstellen.

Vielen Dank für die Nutzung von Azure Search!
