---
title: 'Microsoft Identity Platform: .NET Core-Daemon | Microsoft-Dokumentation'
description: Erfahren Sie, wie eine .NET Daemon-Anwendung erstellt wird, die sich in Azure AD integriert und über OAuth 2.0 durch Azure AD geschützte APIs aufruft.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 07/17/2019
ms.author: jmprieur
ms.reviewer: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f56f3f65919c4d1d9b59f7d0f67522a935579d64
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68327247"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-console-app-using-an-apps-identity"></a>Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API über eine Konsolen-App anhand der Identität einer App

[Microsoft Identity Platform](v2-overview.md) ist eine Weiterentwicklung der Azure AD-Entwicklerplattform (Azure Active Directory). Sie ermöglicht Entwicklern das Erstellen von Anwendungen, mit denen alle Microsoft-Identitäten angemeldet werden, sowie das Abrufen von Token zum Aufrufen von Microsoft-APIs, etwa Microsoft Graph oder von Entwicklern erstellte APIs.

Mithilfe der [Microsoft-Authentifizierungsbibliothek (MSAL)](msal-overview.md) können Entwickler Token vom Microsoft Identity Platform-Endpunkt abrufen, um auf geschützte Web-APIs zuzugreifen. Die Active Directory-Authentifizierungsbibliothek (ADAL) arbeitet mit dem Azure AD für Entwickler (v1.0)-Endpunkt zusammen, während MSAL und der Microsoft Identity Platform (v2.0)-Endpunkt integriert sind.

Für neue .NET Daemon-Anwendungen wird empfohlen, Microsoft Identity Platform (v2.0) und MSAL zu verwenden, um Token zu erhalten und auf sichere Web-APIs zuzugreifen: [Schnellstart: Abrufen eines Tokens und Aufrufen der Microsoft Graph-API über eine Konsolen-App anhand der Identität einer App](quickstart-v2-netcore-daemon.md).
