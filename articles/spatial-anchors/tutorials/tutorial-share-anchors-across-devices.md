---
title: 'Tutorial: Sitzungs- und geräteübergreifendes Freigeben von Azure Spatial Anchors | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie Sie Azure Spatial Anchors-Bezeichner zwischen Android/iOS-Geräten in Unity mit einem Back-End-Dienst freigeben.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: e8a60d5d90b684698d6fcb612278bcae6d4ed08e
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72882290"
---
# <a name="tutorial-share-azure-spatial-anchors-across-sessions-and-devices"></a>Tutorial: Sitzungs- und geräteübergreifendes Freigeben von Azure Spatial Anchors

In diesem Tutorial erfahren Sie, wie Sie [Azure Spatial Anchors](../overview.md) verwenden, um im Rahmen einer Sitzung Anker zu erstellen und diese anschließend auf dem gleichen oder auf einem anderen Gerät zu finden. Die gleichen Anker können auch von mehreren Geräten am gleichen Ort und zur selben Zeit gefunden werden.

![Persistenz](./media/persistence.gif)

Azure Spatial Anchors ist ein plattformübergreifender Entwicklerdienst, mit dem Sie Mixed Reality-Umgebungen mit Objekten erstellen können, die ihre Position im Zeitverlauf geräteübergreifend beibehalten. Nach Abschluss des Vorgangs verfügen Sie über eine App, die auf zwei oder mehr Geräten bereitgestellt werden kann. Von einer Instanz erstellte Azure Spatial Anchors-Bezeichner können für die anderen Instanzen freigegeben werden.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Bereitstellen einer ASP.NET Core-Web-App in Azure, die zum Freigeben von Ankern verwendet werden kann (Speicherung im Arbeitsspeicher für einen bestimmten Zeitraum)
> * Konfigurieren der AzureSpatialAnchorsLocalSharedDemo-Szene im Unity-Beispiel aus den Schnellstartanleitungen, um die Sharing Anchors-Web-App zu nutzen
> * Bereitstellen und Ausführen auf einem oder mehreren Geräten

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Share Anchors Sample Prerequisites](../../../includes/spatial-anchors-share-sample-prereqs.md)]

Beachten Sie Folgendes: Sie verwenden in diesem Tutorial zwar Unity und eine ASP.NET Core-Web-App, aber dies dient nur als Beispiel für die geräteübergreifende Freigabe von Azure Spatial Anchors-Bezeichnern. Sie können auch andere Sprachen und Back-End-Technologien verwenden, um dieses Ziel zu erreichen. Darüber hinaus verfügt die in diesem Tutorial verwendete ASP.NET Core-Web-App über eine Abhängigkeit vom .NET Core 2.2 SDK. Dies funktioniert gut für reguläre Azure-Web-Apps (für Windows), aber derzeit nicht für Azure-Web-Apps für Linux.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="download-the-sample-project"></a>Herunterladen des Beispielprojekts

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

## <a name="deploy-your-sharing-anchors-service"></a>Bereitstellen Ihres Diensts für die Freigabe von Ankern (Sharing Anchors)

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/VS)

Öffnen Sie Visual Studio und dann das Projekt im Ordner `Sharing\SharingServiceSample`.

[!INCLUDE [Publish Azure](../../../includes/spatial-anchors-publish-azure.md)]

## <a name="visual-studio-codetabvsc"></a>[Visual Studio Code](#tab/VSC)

Sie müssen eine Ressourcengruppe und einen App Service-Plan erstellen, bevor Sie den Dienst in VS Code bereitstellen.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Navigieren Sie zum <a href="https://portal.azure.com/" target="_blank">Azure-Portal</a>, und melden Sie sich bei Ihrem Abonnement an.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

Wählen Sie neben **Ressourcengruppe** die Option **Neu** aus.

Nennen Sie die Ressourcengruppe **myResourceGroup**, und wählen Sie **OK** aus.

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

[!INCLUDE [app-service-plan](../../../includes/app-service-plan.md)]

Wählen Sie neben **Hostingplan** die Option **Neu** aus.

Verwenden Sie im Dialogfeld **Hostingplan konfigurieren** die folgenden Einstellungen:

| Einstellung | Empfohlener Wert | BESCHREIBUNG |
|-|-|-|
|App Service-Plan| MySharingServicePlan | Name des App Service-Plans. |
| Location | USA (Westen) | Das Rechenzentrum, in dem die Web-App gehostet wird. |
| Size | Kostenlos | Der [Tarif](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), der die Hostingfunktionen festlegt. |

Klicken Sie auf **OK**.

Öffnen Sie Visual Studio Code und dann das Projekt im Ordner `Sharing\SharingServiceSample`. Führen Sie die Schritte in <a href="https://docs.microsoft.com/aspnet/core/tutorials/publish-to-azure-webapp-using-vscode?view=aspnetcore-2.2#open-it-with-visual-studio-code" target="_blank">diesem Tutorial</a> aus, um den Freigabedienst über Visual Studio Code bereitzustellen. Sie können die Schritte ab dem Abschnitt „Öffnen mit Visual Studio Code“ ausführen. Erstellen Sie kein weiteres MVC-Projekt wie in den obigen Schritten beschrieben, da Sie bereits über das Projekt für die Bereitstellung und Veröffentlichung verfügen: SharingServiceSample.

---

## <a name="deploy-the-sample-app"></a>Bereitstellen der Beispiel-App

[!INCLUDE [Run Share Anchors Sample](../../../includes/spatial-anchors-run-share-sample.md)]

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine ASP.NET Core-Web-App in Azure bereitgestellt und anschließend eine Unity-App konfiguriert und bereitgestellt. Sie haben Spatial Anchors-Bezeichner mit der App erstellt und mithilfe der ASP.NET Core-Web-App für andere Geräte freigegeben.

Sie können Ihre ASP.NET Core-Web-App so optimieren, dass sie Azure Cosmos DB zur Speicherung der freigegebenen Spatial Anchors-Bezeichner nutzt. Durch Hinzufügen der Azure Cosmos DB-Unterstützung kann Ihre ASP.NET Core-Web-App anhand des in Ihrer Web-App gespeicherten Ankerbezeichners einen Anker wiederfinden, den sie vor mehreren Tagen erstellt hat.

> [!div class="nextstepaction"]
> [Tutorial: Sitzungs- und geräteübergreifendes Freigeben von Azure Spatial Anchors mit einem Azure Cosmos DB-Back-End](./tutorial-use-cosmos-db-to-store-anchors.md)

