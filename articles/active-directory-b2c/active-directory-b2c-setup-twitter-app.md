---
title: Einrichten der Registrierung und Anmeldung mit einem Twitter-Konto mithilfe von Azure Active Directory B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Twitter-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: ff795bbbd3cf136735499b571367cf5b8a6ec170
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69622155"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Twitter-Konto mithilfe von Azure Active Directory B2C

## <a name="create-an-application"></a>Erstellen einer Anwendung

Um Twitter als Identitätsanbieter in Azure AD B2C zu nutzen, müssen Sie eine Twitter-Anwendung erstellen. Wenn Sie noch über kein Twitter-Konto verfügen, können Sie eines unter [https://twitter.com/signup](https://twitter.com/signup) registrieren.

1. Melden Sie sich auf der Website für [Twitter-Entwickler](https://developer.twitter.com/en/apps) mit den Anmeldeinformationen für Ihr Twitter-Konto an.
1. Wählen Sie **Create an app** (App erstellen) aus.
1. Geben Sie in **App name** einen App-Namen und in **Application description** eine Anwendungsbeschreibung ein.
1. Gegen Sie unter der **Website-URL** `https://your-tenant.b2clogin.com` ein. Ersetzen Sie `your-tenant` durch den Namen Ihres Mandanten. Beispiel: https://contosob2c.b2clogin.com.
1. Geben Sie `https://your-tenant.b2clogin.com/your-tenant.onmicrosoft.com/your-user-flow-Id/oauth1/authresp` in **Callback URL** ein. Ersetzen Sie `your-tenant` durch den Namen Ihres Mandanten und `your-user-flow-Id` durch den Bezeichner Ihres Benutzerflows. Beispiel: `b2c_1A_signup_signin_twitter`. Bei der Eingabe Ihres Mandantennamens dürfen Sie nur Kleinbuchstaben verwenden, auch wenn der Mandant in Azure AD B2C Großbuchstaben enthält.
1. Lesen und akzeptieren Sie die Nutzungsbedingungen am Ende der Seite, und wählen Sie dann **Create** (Erstellen) aus.
1. Wählen Sie auf der Seite **App details** (Anwendungsdetails) **Edit > Edit details** (Bearbeiten > Details bearbeiten), aktivieren Sie das Kontrollkästchen für **Enable Sign in with Twitter** (Anmeldung bei Twitter aktivieren), und wählen Sie dann **Save** (Speichern).
1. Wählen Sie **Keys and tokens** (Schlüssel und Token) aus, und notieren Sie die Werte für **Consumer API Key** (Consumer-API-Schlüssel) und **Consumer API secret key** (Geheimer Consumer-API-Schlüssel) für die spätere Verwendung.

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Twitter als Identitätsanbieter in Ihrem Mandanten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
1. Wählen Sie **Identitätsanbieter** und dann **Twitter** aus.
1. Geben Sie einen **Namen** ein. Beispiel: *Twitter*.
1. Geben Sie für die **Client-ID** den Consumer-API-Schlüssel der Twitter-Anwendung ein, die Sie zuvor erstellt haben.
1. Geben Sie für den **geheimen Clientschlüssel** den notierten Consumer-API-Schlüssel ein.
1. Wählen Sie **Speichern** aus.
