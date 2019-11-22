---
title: 'Tutorial: Sitzungs- und geräteübergreifendes Freigeben von Azure Spatial Anchors mit einem Azure Cosmos DB-Back-End | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie Azure Spatial Anchors-Bezeichner zwischen Android/iOS-Geräten in Unity mit einem Back-End-Dienst und Azure Cosmos DB freigeben.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 7ddbff563f79992f21aef5182177f4fb60c61dab
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72882168"
---
# <a name="tutorial-sharing-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Tutorial: Sitzungs- und geräteübergreifendes Freigeben von Azure Spatial Anchors mit einem Azure Cosmos DB-Back-End

Dieses Tutorial ist eine Fortsetzung des Tutorials [Sitzungs- und geräteübergreifendes Freigeben von Azure Spatial Anchors](../../../articles/spatial-anchors/tutorials/tutorial-share-anchors-across-devices.md). Hier werden die Schritte zum Hinzufügen weiterer Funktionen erläutert, um Azure Cosmos DB als Back-End-Speicher bereitzustellen und Azure Spatial Anchors über Sitzungen und Geräte hinweg freizugeben.

![GIF zur Veranschaulichung der Objektpersistenz](./media/persistence.gif)

Beachten Sie Folgendes: Sie verwenden in diesem Tutorial zwar Unity und Azure Cosmos DB, aber dies dient nur als Beispiel für die geräteübergreifende Freigabe von Spatial Anchors-Bezeichnern. Sie können auch andere Sprachen und Back-End-Technologien verwenden, um dieses Ziel zu erreichen. Darüber hinaus setzt die in diesem Tutorial verwendete ASP.NET Core-Web-App .NET Core 2.2 SDK voraus. Es lässt sich problemlos auf Web-Apps für Windows ausführen, aber derzeit nicht auf Web-Apps für Linux.

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

Fügen Sie der zuvor erstellten Ressourcengruppe eine Azure Cosmos-Datenbank hinzu. 

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

Kopieren Sie den `Connection String`, da Sie ihn benötigen.

## <a name="make-minor-changes-to-the-sharingservice-files"></a>Vornehmen kleinerer Änderungen an den SharingService-Dateien

Öffnen Sie `SharingService\Startup.cs` im **Projektmappen-Explorer**.

Suchen Sie oben in der Datei nach `#define INMEMORY_DEMO`, und kommentieren Sie diese Zeile aus. Speichern Sie die Datei .

Öffnen Sie `SharingService\appsettings.json` im **Projektmappen-Explorer**.

Suchen Sie nach der `StorageConnectionString`-Eigenschaft, und legen Sie den Wert auf den `Connection String`-Text fest, den Sie im Schritt [Erstellen eines Datenbankkontos](#create-a-database-account) kopiert haben. Speichern Sie die Datei .

Sie können den Freigabedienst erneut veröffentlichen und die Beispiel-App ausführen.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Azure Cosmos DB verwendet, um Ankerbezeichner geräteübergreifend freizugeben. Weitere Informationen zur Verwendung von Azure Spatial Anchors in einer neuen Unity HoloLens-App finden Sie im nächsten Tutorial.

> [!div class="nextstepaction"]
> [Starten einer neuen Unity HoloLens-App](./tutorial-new-unity-hololens-app.md)
