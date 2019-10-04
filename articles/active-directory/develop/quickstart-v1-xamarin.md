---
title: Erste Schritte in Azure AD Xamarin | Microsoft-Dokumentation
description: Erstellen Sie Xamarin-Anwendungen, die für die Anmeldung in Azure AD integriert sind und über OAuth durch Azure AD geschützte APIs aufrufen.
services: active-directory
documentationcenter: xamarin
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36e354894700ec92a444f288b3e09f5d7af88329
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68321035"
---
# <a name="quickstart-build-a-xamarin-app-that-integrates-microsoft-sign-in"></a>Schnellstart: Erstellen einer Xamarin-App, die die Microsoft-Anmeldung integriert

[Microsoft Identity Platform](v2-overview.md) ist eine Weiterentwicklung der Azure AD-Entwicklerplattform (Azure Active Directory). Sie ermöglicht Entwicklern das Erstellen von Anwendungen, mit denen alle Microsoft-Identitäten angemeldet werden, sowie das Abrufen von Token zum Aufrufen von Microsoft-APIs, etwa Microsoft Graph oder von Entwicklern erstellte APIs.

Mithilfe der [Microsoft-Authentifizierungsbibliothek (MSAL)](msal-overview.md) können Entwickler Token vom Microsoft Identity Platform-Endpunkt abrufen, um auf geschützte Web-APIs zuzugreifen. Die Active Directory-Authentifizierungsbibliothek (ADAL) arbeitet mit dem Azure AD für Entwickler (v1.0)-Endpunkt zusammen, während MSAL und der Microsoft Identity Platform (v2.0)-Endpunkt integriert sind.

Für neue Xamarin-Anwendungen wird empfohlen, Microsoft Identity Platform (v2.0) und MSAL zu verwenden, um Token zu erhalten und auf sichere Web-APIs zuzugreifen: [Integrieren der Microsoft-Identität und von Microsoft Graph in eine Xamarin-Formularanwendung mit MSAL](https://github.com/azure-samples/active-directory-xamarin-native-v2#integrate-microsoft-identity-and-the-microsoft-graph-into-a-xamarin-forms-app-using-msal) (ohne die optionalen Schritte).

