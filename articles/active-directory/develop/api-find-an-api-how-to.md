---
title: Suchen einer bestimmten API, die für eine benutzerdefinierte Anwendung benötigt wird | Microsoft-Dokumentation
description: Konfigurieren der Berechtigungen, die für den Zugriff auf eine bestimmte API in einer benutzerdefinierten Azure AD-Anwendung benötigt werden
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2492a9346585698132e7fd9cfcde068ffd60ebc5
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476165"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Suchen einer bestimmten API, die für eine benutzerdefinierte Anwendung benötigt wird

Für den Zugriff auf APIs müssen Zugriffsbereiche und Rollen konfiguriert werden. Wenn Sie die Web-APIs einer Ressourcenanwendung für Clientanwendungen verfügbar machen möchten, müssen Sie Zugriffsbereiche und Rollen für die API konfigurieren. Wenn eine Clientanwendung auf eine Web-API zugreifen soll, müssen Sie Zugriffsberechtigungen für die API in der App-Registrierung konfigurieren.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Konfigurieren einer Ressourcenanwendung zum Bereitstellen von Web-APIs

Wenn Sie Ihre Web-API verfügbar machen, wird die API in der Liste **Hiermit wählen Sie eine API aus.** angezeigt, wenn Berechtigungen zu einer App-Registrierung hinzugefügt werden. Befolgen Sie für das Hinzufügen von Zugriffsbereichen die Schritte in [Konfigurieren einer Anwendung für das Verfügbarmachen von Web-APIs](quickstart-configure-app-expose-web-apis.md).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs

Wenn Sie Ihrer App-Registrierung Berechtigungen hinzufügen, können Sie für verfügbar gemachte Web-APIs **API-Zugriff hinzufügen**. Befolgen Sie für den Zugriff auf Web-APIs die Schritte in [Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs](quickstart-configure-app-access-web-apis.md).

## <a name="next-steps"></a>Nächste Schritte

-   [Grundlegendes zum Azure Active Directory-Anwendungsmanifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


