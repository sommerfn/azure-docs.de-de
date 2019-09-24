---
title: Einrichten der Registrierung und Anmeldung mit einem LinkedIn-Konto – Azure Active Directory B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit LinkedIn-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: be347fe33e03b4c2a7ecc1015a407c5a58062326
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71065145"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem LinkedIn-Konto mithilfe von Azure Active Directory B2C

## <a name="create-a-linkedin-application"></a>Erstellen einer LinkedIn-Anwendung

Um ein LinkedIn-Konto als [Identitätsanbieter](active-directory-b2c-reference-oauth-code.md) in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie in Ihrem Mandanten eine Anwendung erstellen, die es darstellt. Wenn Sie noch über kein LinkedIn-Konto verfügen, können Sie sich unter [https://www.linkedin.com/](https://www.linkedin.com/) registrieren.

1. Melden Sie sich auf der [LinkedIn-Entwickler-Website](https://www.developer.linkedin.com/) mit den Anmeldeinformationen für Ihr LinkedIn-Konto an.
1. Wählen Sie **My Apps** (Meine Apps) aus, und klicken Sie dann auf **Create Application** (Anwendung erstellen).
1. Geben Sie Informationen für **Company Name** (Firmenname), **Application Name** (Anwendungsname), **Application Description** (Anwendungsbeschreibung), **Application Logo** (Anwendungslogo), **Application Use** (Anwendungsverwendung), **Website URL** (Website-URL), **Business Email** (E-Mail geschäftlich) und **Business Phone** (Telefon geschäftlich) ein.
1. Akzeptieren Sie die **LinkedIn API Terms of Use**, und klicken Sie auf **Submit**.
1. Kopieren Sie die Werte für **Client ID** und **Client Secret**. Sie finden sie unter **Authentication Keys** (Authentifizierungsschlüssel). Sie benötigen beide Angaben, um LinkedIn als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **geheime Clientschlüssel** ist eine wichtige Sicherheitsanmeldeinformation.
1. Geben Sie `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` unter **Authorized Redirect URLs** (Autorisierte Umleitungs-URLs) ein. Ersetzen Sie `your-tenant-name` durch den Namen Ihres Mandanten. Bei der Eingabe Ihres Mandantennamens dürfen Sie nur Kleinbuchstaben verwenden, auch wenn der Mandant in Azure AD B2C Großbuchstaben enthält. Wählen Sie **Add** (Hinzufügen) aus, und klicken Sie dann auf **Update** (Aktualisieren).

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Konfigurieren eines LinkedIn-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
1. Wählen Sie **Identitätsanbieter** und dann **LinkedIn** aus.
1. Geben Sie einen **Namen** ein. Beispiel: *LinkedIn*.
1. Geben Sie für die **Client-ID** die Client-ID der LinkedIn-Anwendung ein, die Sie zuvor erstellt haben.
1. Geben Sie für **Geheimer Clientschlüssel** den zuvor notierten geheimen Clientschlüssel ein.
1. Wählen Sie **Speichern** aus.

## <a name="migration-from-v10-to-v20"></a>Migration von Version 1.0 zu 2.0

LinkedIn hat kürzlich [seine APIs von Version 1.0 auf 2.0 aktualisiert](https://engineering.linkedin.com/blog/2018/12/developer-program-updates). Im Rahmen der Migration kann Azure AD B2C während der Registrierung nur den vollständigen Namen des LinkedIn-Benutzers abrufen. Wenn eine E-Mail-Adresse zu den Attributen gehört, die bei der Registrierung gesammelt werden, muss der Benutzer die E-Mail-Adresse manuell eingeben und validieren.
