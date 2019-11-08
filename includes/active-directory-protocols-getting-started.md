---
title: Übersicht über das Azure AD .NET-Protokoll | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie HTTP-Nachrichten zum Autorisieren des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten mit Azure AD verwenden.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2019
ms.author: priyamo
ms.openlocfilehash: 7772b3ee5d0e27c09e83f7d118eb9f67f17e0d07
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73522881"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Registrieren der Anwendung beim AD-Mandanten
Registrieren Sie zuerst Ihre Anwendung beim Azure Active Directory-Mandanten (Azure AD). Hierbei erhalten Sie eine Anwendungs-ID für die Anwendung, und die Aktivierung für den Empfang von Token wird durchgeführt.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
   
1. Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie Ihr Konto in der oberen rechten Ecke der Seite, anschließend das Navigationselement **Verzeichnis wechseln** und dann den entsprechenden Mandanten auswählen. 
   - Überspringen Sie diesen Schritt, wenn Ihr Konto nur einen Azure AD-Mandanten enthält oder wenn Sie den entsprechenden Azure AD-Mandanten bereits ausgewählt haben.
   
1. Suchen Sie im Azure-Portal nach **Azure Active Directory**, und wählen Sie es aus.
   
1. Wählen Sie im linken Menü **Azure Active Directory** die Option **App-Registrierungen** und dann **Neue Registrierung** aus.
   
1. Folgen Sie der Anleitung, und erstellen Sie eine neue Anwendung. Es spielt bei diesem Tutorial keine Rolle, ob es sich um eine Webanwendung oder eine öffentliche Clientanwendung (Mobilgerät und Desktop) handelt. Falls Sie jedoch spezielle Beispiele für Webanwendungen bzw. öffentliche Clientanwendungen wünschen, können Sie sich unsere [Schnellstarts](../articles/active-directory/develop/v1-overview.md) ansehen.
   
   - **Name** ist der Anwendungsname und beschreibt Ihre Anwendung für Endbenutzer.
   - Wählen Sie unter **Unterstützte Kontotypen** **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** aus.
   - Stellen Sie den **Umleitungs-URI** bereit. Bei Webanwendungen ist dies die Basis-URL Ihrer App, unter der sich Benutzer anmelden können.  Beispiel: `http://localhost:12345`. Bei öffentlichen Clients (Mobilgerät und Desktop) verwendet Azure AD den URI zum Zurückgeben von Tokenantworten. Geben Sie einen für Ihre Anwendung spezifischen Wert ein.  Beispiel: `http://MyFirstAADApp`.
   <!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->  
   
1. Nach Abschluss der Registrierung weist Azure AD Ihrer Anwendung eine eindeutige Client-ID (die **Anwendungs-ID**) zu. Diesen Wert benötigen Sie in den nächsten Abschnitten. Daher sollten Sie ihn von der Anwendungsseite kopieren.
   
1. Zum Suchen Ihrer Anwendung im Azure-Portal wählen Sie **App-Registrierungen** und dann **Alle Anwendungen anzeigen** aus.
