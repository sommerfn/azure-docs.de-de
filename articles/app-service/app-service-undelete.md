---
title: Wiederherstellen von gelöschten App Service-Apps – Azure App Service
description: In diesem Artikel wird erläutert, wie eine gelöschte App Service-App mithilfe von PowerShell wiederhergestellt wird.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.service: app-service
ms.openlocfilehash: 7dc3934f486b205febd5be3c0b484dfd2c97bb8f
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72755543"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>Wiederherstellen einer gelöschten App Service-App mithilfe von PowerShell

Wenn Sie Ihre App versehentlich in Azure App Service gelöscht haben, können Sie sie mithilfe der Befehle im [Azure PowerShell-Modul](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0) wiederherstellen.

## <a name="list-deleted-apps"></a>Auflisten gelöschter Apps

Zum Aufrufen der Auflistung gelöschter Apps können Sie `Get-AzDeletedWebApp` verwenden.

Für Details zu einer bestimmten gelöschten App können Sie Folgendes verwenden:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app>
```

Die ausführlichen Informationen enthalten folgende Angaben:

- **DeletedSiteId**: Eindeutiger Bezeichner für die App, der für Szenarien verwendet wird, in denen mehrere Apps mit gleichem Namen gelöscht wurden
- **SubscriptionID**: Abonnement, das die gelöschte Ressource enthält
- **Standort**: Speicherort der ursprünglichen App
- **ResourceGroupName**: Name der ursprünglichen Ressourcengruppe
- **Name**: Name der ursprünglichen App
- **Slot**: Name des Slots
- **DeletionTime**: Zeitpunkt, an dem die App gelöscht wurde  

## <a name="restore-deleted-app"></a>Wiederherstellen einer gelöschten App

Wenn Sie die wiederherzustellende App identifiziert haben, können Sie sie mithilfe von `Restore-AzDeletedWebApp` wiederherstellen.

```powershell
Restore-AzDeletedWebApp -ResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```

Eingaben für den Befehl:

- **Ressourcengruppe**: Zielressourcengruppe, in der die App wiederhergestellt wird
- **Name**: Name für die App, muss global eindeutig sein
- **TargetAppServicePlanName**: App Service-Plan, der mit der App verknüpft ist

Standardmäßig wird von `Restore-AzDeletedWebApp` sowohl die Konfiguration der App als auch deren Inhalt wiederhergestellt. Wenn lediglich der Inhalt wiederhergestellt werden soll, verwenden Sie das Flag `-RestoreContentOnly` mit diesem Cmdlet.

> [!NOTE]
> Falls die App in einer App Service-Umgebung gehostet und anschließend daraus gelöscht wurde, kann sie nur wiederhergestellt werden, wenn die entsprechende App Service-Umgebung noch vorhanden sind.
>

Die vollständige Cmdlet-Referenz finden Sie hier: [Restore-AzDeletedWebApp](https://docs.microsoft.com/powershell/module/az.websites/restore-azdeletedwebapp).
