---
title: Erstellen einer iOS-App für mobile Azure App Service-Apps | Microsoft Docs
description: Befolgen Sie dieses Tutorial für die ersten Schritte bei der Verwendung mobiler Azure-App-Back-Ends für die iOS-Entwicklung in Objective-C oder Swift.
services: app-service\mobile
documentationcenter: ios
author: elamalani
manager: crdun
editor: ''
ms.assetid: 6461a899-9340-42dd-b118-ffc5ba00e846
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 9140de50e23447f53fd1fa204a2a67c324672397
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449139"
---
# <a name="create-an-ios-app"></a>Erstellen einer iOS-App

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Im Rahmen von Visual Studio App Center wird in neue und integrierte Dienste investiert, die für die Entwicklung mobiler Apps von zentraler Bedeutung sind. Entwickler können **Build**-, **Test**- und **Verteilungs**dienste nutzen, um eine Pipeline für Continuous Integration und Delivery einzurichten. Nach der Bereitstellung der App können Entwickler den Status und die Nutzung ihrer App mithilfe der **Analyse**- und **Diagnose**dienste überwachen und mit Benutzern über den **Push**dienst interagieren. Entwickler können auch den **Authentifizierung**sdienst nutzen, um ihre Benutzer zu authentifizieren, und den **Daten**dienst, um App-Daten dauerhaft in der Cloud zu speichern und zu synchronisieren. Besuchen Sie noch heute das [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-ios-get-started).
>

## <a name="overview"></a>Übersicht

In diesem Tutorial erfahren Sie, wie Sie [Azure Mobile App Service-Apps](app-service-mobile-value-prop.md), einen Cloud-Back-End-Dienst, zu einer iOS-App hinzufügen können. Der erste Schritt besteht in der Erstellung eines neuen mobilen Back-Ends in Azure. Anschließend laden Sie eine einfache iOS-Beispiel-App vom Typ *Aufgabenliste* herunter, die Daten in Azure speichert.

Sie benötigen einen Mac und ein [Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)

## <a name="create-a-new-azure-mobile-app-backend"></a>Erstellen eines neuen mobilen Azure-App-Back-Ends

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Erstellen einer Datenbankverbindung und Konfigurieren des Client- und Serverprojekts
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-ios-app"></a>Ausführen der iOS-App

[!INCLUDE [app-service-mobile-ios-run-app](../../includes/app-service-mobile-ios-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
