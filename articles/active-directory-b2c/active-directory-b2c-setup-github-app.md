---
title: Einrichten der Registrierung und Anmeldung mit einem GitHub-Konto – Azure Active Directory B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit GitHub-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 74c663d8847c2829a5d9466f8e601dd44593a6f8
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71065203"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem GitHub-Konto mithilfe von Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-github-oauth-application"></a>Erstellen einer GitHub-OAuth-Anwendung

Um ein GitHub-Konto als [Identitätsanbieter](active-directory-b2c-reference-oauth-code.md) in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie in Ihrem Mandanten eine Anwendung erstellen, die es darstellt. Wenn Sie noch über kein GitHub-Konto verfügen, können Sie sich unter [https://www.github.com/](https://www.github.com/) registrieren.

1. Melden Sie sich auf der [GitHub-Entwickler-Website](https://github.com/settings/developers) mit Ihren GitHub-Anmeldeinformationen an.
1. Klicken Sie auf **OAuth Apps** (OAuth-Apps) und anschließend auf **New OAuth App** (Neue OAuth-App).
1. Geben Sie einen **Anwendungsnamen** und Ihre **Homepage-URL** ein.
1. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` unter **Authorization callback URL** (Autorisierungsrückruf-URL) ein. Ersetzen Sie `your-tenant-name` durch den Namen des Azure AD B2C-Mandanten. Verwenden Sie bei der Eingabe Ihres Mandantennamens nur Kleinbuchstaben, auch wenn der Mandant in Azure AD B2C Großbuchstaben enthält.
1. Klicken Sie zum Registrieren der Anwendung auf **Register application**.
1. Kopieren Sie die Werte für **Client ID** und **Client Secret**. Sie benötigen beide Angaben, um den Identitätsanbieter Ihrem Mandanten hinzuzufügen.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Konfigurieren eines GitHub-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
1. Wählen Sie **Identitätsanbieter** und dann **GitHub (Vorschau)** aus.
1. Geben Sie einen **Namen** ein. Beispiel: *GitHub*.
1. Geben Sie für die **Client-ID** die Client-ID der GitHub-Anwendung ein, die Sie zuvor erstellt haben.
1. Geben Sie für **Geheimer Clientschlüssel** den zuvor notierten geheimen Clientschlüssel ein.
1. Wählen Sie **Speichern** aus.
