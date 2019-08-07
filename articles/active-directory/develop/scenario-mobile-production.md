---
title: Mobile App, die Web-APIs aufruft (Übergang in die Produktion) – Microsoft Identity Platform
description: Erfahren Sie, wie Sie eine mobile App erstellen, die Web-APIs aufruft (Übergang in die Produktion).
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
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 67788dd9257a0a4685313dce44c6a3dfb5e514df
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68413607"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Mobile App, die Web-APIs aufruft (Übergang in die Produktion)

In diesem Artikel wird beschrieben, wie Sie die Qualität und Zuverlässigkeit Ihrer App erhöhen, bevor Sie diese in die Produktion überführen.

## <a name="handling-errors-in-mobile-applications"></a>Behandeln von Fehlern in mobilen Anwendungen

An diesem Punkt können in der App verschiedene Fehler auftreten. Die Hauptszenarien bestehen in der Behandlung verborgener (stiller) Fehler und im Ausweichen auf eine Interaktion. Weitere Fehlerzustände in Produktionsumgebungen sind Netzwerkstörungen, Dienstausfälle, fehlende Administratoreinwilligungen und weitere Fälle, die auf das jeweilige Szenario bezogen sind.

Für jede MSAL-Bibliothek sind Beispielcode und Wiki-Inhalte verfügbar, in denen auf die Behandlung dieser Fehler eingegangen wird:

- [Android-Wiki für MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [iOS-Wiki für MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [Wiki für MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Untersuchen und Beheben von Problemen

Bei der Diagnose von App-Problemen kann es hilfreich sein, Daten zu sammeln. Informationen zu den Arten von Daten, die Sie sammeln können, finden Sie in den Wikis zur MSAL-Plattform.

- Wenn Benutzer auf Probleme treffen, können sie Unterstützung anfordern. Eine bewährte Methode besteht darin, Protokolle zu erfassen und vorübergehend zu speichern und einen Pfad anzugeben, unter dem sie von Benutzern hochgeladen werden können. MSAL stellt Protokollierungserweiterungen zur Verfügung, mit denen sich detaillierte Authentifizierungsinformationen erfassen lassen.
- Falls verfügbar, aktivieren Sie die Sammlung von Telemetriedaten mithilfe der MSAL, um zu ermitteln, wie sich Benutzer bei Ihrer App anmelden.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

Sie können auch weitere Beispiele testen, die unter [Beispiele | Öffentliche Desktop- und mobile Client-Apps](sample-v2-code.md#desktop-and-mobile-public-client-apps) verfügbar sind.
