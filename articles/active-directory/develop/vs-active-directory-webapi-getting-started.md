---
title: Erste Schritte mit Azure AD in Visual Studio-WebApi-Projekten
description: Erfahren Sie etwas über die ersten Schritte mit Azure Active Directory in WebApi-Projekten nach dem Herstellen einer Verbindung oder dem Erstellen eines Azure AD mithilfe von verbundenen Visual Studio-Diensten.
author: ghogen
manager: jillfra
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev, vs-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: 225940052e1f40f8050059532e4657df2b5410a3
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68851905"
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Erste Schritte mit Azure Active Directory (WebApi-Projekte)

> [!div class="op_single_selector"]
> - [Erste Schritte](vs-active-directory-webapi-getting-started.md)
> - [Was ist passiert?](vs-active-directory-webapi-what-happened.md)

Dieser Artikel enthält weiterführende Informationen, nachdem Sie Active Directory in Visual Studio über **Projekt > Verbundene Dienste** einem ASP.NET-WebAPI-Projekt hinzugefügt haben. Sollten Sie den Dienst noch nicht Ihrem Projekt hinzugefügt haben, können Sie dies jederzeit nachholen.

Informationen zu den Änderungen, die an Ihrem Projekt vorgenommen werden, wenn der verbundene Dienst hinzugefügt wird, finden Sie unter [Was ist mit meinem WebAPI-Projekt passiert?](vs-active-directory-webapi-what-happened.md).

## <a name="requiring-authentication-to-access-controllers"></a>Erfordern von Authentifizierung für den Zugriff auf Controller

Alle Controller in Ihrem Projekt wurden mit dem Attribut `[Authorize]` versehen. Dieses Attribut erfordert, dass der Benutzer vor dem Zugriff auf die durch diese Controller definierten APIs authentifiziert werden muss. Wenn Sie anonymen Zugriff auf diesen Controller erlauben möchten, entfernen Sie dieses Attribut vom Controller. Wenn Sie die Berechtigungen präziser festlegen möchten, wenden Sie das Attribut auf jede Methode an, die Autorisierung erfordert, anstatt es auf die Controllerklasse anzuwenden.

## <a name="next-steps"></a>Nächste Schritte

- [Authentifizierungsszenarien für Azure Active Directory](authentication-scenarios.md)
- [Hinzufügen von „Mit Microsoft anmelden“ zu einer ASP.NET-Web-App](quickstart-v1-aspnet-webapp.md)
