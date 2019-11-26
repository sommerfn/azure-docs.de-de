---
title: 'Geschützte Web-API: Übersicht'
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine geschützte Web-API erstellen (Übersicht).
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
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64418138f9eff61cc94a57be4dc5e2625e5fdebb
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72803689"
---
# <a name="scenario-protected-web-api"></a>Szenario: Geschützte Web-API

In diesem Szenario lernen Sie, wie Sie eine Web-API verfügbar machen und wie Sie diese schützen können, damit nur authentifizierte Benutzer darauf zugreifen können. Sie sollten authentifizierten Benutzer mit Arbeits- und Uni-/Schulkonten oder Benutzern mit persönlichen Microsoft-Konten erlauben, Ihre Web-API zu verwenden.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Besonderheiten

Beachten Sie die folgenden Besonderheiten beim Schützen Ihrer Web-APIs:

- Ihre App-Registrierung muss mindestens einen Gültigkeitsbereich verfügbar machen. Die Tokenversion, die von Ihrer Web-API akzeptiert wird, hängt von der Zielgruppe der Anmeldung ab.
- Die Konfiguration des Codes für die Web-API muss das Token überprüfen, das beim Aufrufen der Web-API verwendet wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [App-Registrierung](scenario-protected-web-api-app-registration.md)
