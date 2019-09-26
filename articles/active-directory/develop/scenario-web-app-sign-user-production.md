---
title: Web-App, die Benutzer anmeldet (Übergang in die Produktion) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer anmeldet (Übergang in die Produktion).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd6717c132d32d54c16e7678695a09e35181a057
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71086545"
---
# <a name="web-app-that-signs-in-users---move-to-production"></a>Web-App, die Benutzer anmeldet – Übergang in die Produktion

Da Sie nun wissen, wie Sie ein Token abrufen, um Web-APIs aufzurufen, erfahren Sie in diesem Artikel, wie Ihnen der Übergang in die Produktion gelingt.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Nächste Schritte

### <a name="calling-web-apis-scenario"></a>Szenario: Aufrufen von Web-APIs

Sobald Ihre Web-App Benutzer anmeldet, kann sie für diese Web-APIs aufrufen. Genau darum geht es im folgenden Szenario:

> [!div class="nextstepaction"]
> [Web-App, die Web-APIs aufruft](scenario-web-app-call-api-overview.md)

### <a name="deep-dive---aspnet-core-web-app-tutorial"></a>Ausführliche Betrachtung: Tutorial zur ASP.NET Core-Web-App

Lernen Sie im ASP.NET Core-Tutorial [ms-identity-aspnetcore-webapp-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) andere Möglichkeiten kennen, Benutzer anzumelden. Dieses Beispiel ist ein fortschreitendes Tutorial mit Web-App-Code, der in Produktionsumgebungen eingesetzt werden kann. Außerdem wird beschrieben, wie eine Anmeldung mit Konten hinzugefügt wird, die sich in folgenden Umgebungen befinden:

- Ihre Organisation
- Mehrere Organisationen
- Geschäfts-, Schul- oder Unikonten bzw. persönlichen Microsoft-Konten
- [Azure AD B2C](https://aka.ms/aadb2c)
- Nationale Clouds

### <a name="sample-code---java-web-app"></a>Beispielcode: Java-Web-App

Weitere Informationen zur Java-Web-App aus dem Beispiel auf GitHub: [Eine Java-Webanwendung, die Benutzer bei Microsoft Identity Platform anmeldet und Microsoft Graph aufruft](https://github.com/Azure-Samples/ms-identity-java-webapp)
