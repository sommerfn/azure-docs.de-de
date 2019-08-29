---
title: Einrichten der Registrierung und Anmeldung mit einem Amazon-Konto – Azure Active Directory B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Amazon-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 89dd592e6e5ea1ce71277035654068ce2f782890
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69622204"
---
# <a name="set-up-sign-up-and-sign-in-with-an-amazon-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Amazon-Konto mithilfe von Azure Active Directory B2C

## <a name="create-an-amazon-application"></a>Erstellen einer Amazon-Anwendung

Um ein Amazon-Konto als [Identitätsanbieter](active-directory-b2c-reference-oauth-code.md) in Azure Active Directory B2C (Azure AD) verwenden zu können, müssen Sie eine Anwendung in Ihrem Mandanten erstellen, die es darstellt. Wenn Sie noch nicht über ein Amazon-Konto verfügen, können Sie sich unter [https://www.amazon.com/](https://www.amazon.com/) registrieren.

1. Melden Sie sich beim [Amazon Developer Center](https://login.amazon.com/) mit den Anmeldeinformationen für Ihr Amazon-Konto an.
1. Wenn Sie dies nicht bereits getan haben, klicken Sie auf **Sign Up**, führen Sie die Schritte zur Registrierung von Entwicklern aus, und akzeptieren Sie die Richtlinie.
1. Wählen Sie **Register new application** (Neue Anwendung registrieren) aus.
1. Geben Sie einen **Namen**, unter **Description** eine Beschreibung und unter **Privacy Notice URL** eine URL für den Datenschutzhinweis ein, und klicken Sie dann auf **Save** (Speichern). Die Seite „Datenschutzhinweise“ wird von Ihnen verwaltet und bietet Benutzern Informationen zum Datenschutz.
1. Kopieren Sie im Abschnitt **Web Settings** (Webeinstellungen) den Wert für **Client ID** (Client-ID). Wählen Sie **Show Secret** (Geheimnis anzeigen) aus, um den geheimen Clientschlüssel abzurufen, und kopieren Sie ihn dann. Sie benötigen beide Angaben, um ein Amazon-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **geheime Clientschlüssel** ist eine wichtige Sicherheitsanmeldeinformation.
1. Wählen Sie im Abschnitt **Web Settings** (Webeinstellungen) die Option **Edit** (Bearbeiten) aus, und geben Sie dann `https://your-tenant-name.b2clogin.com` unter **Allowed JavaScript Origins** (Zulässige JavaScript-Quellen) und `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` unter **Allowed Return URLs** (Zulässige Rückgabe-URIs) ein. Ersetzen Sie `your-tenant-name` durch den Namen Ihres Mandanten. Bei der Eingabe Ihres Mandantennamens dürfen Sie nur Kleinbuchstaben verwenden, auch wenn der Mandant in Azure AD B2C Großbuchstaben enthält.
1. Klicken Sie auf **Speichern**.

## <a name="configure-an-amazon-account-as-an-identity-provider"></a>Konfigurieren eines Amazon-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
1. Wählen Sie **Identitätsanbieter** und dann **Amazon** aus.
1. Geben Sie einen **Namen** ein. Beispiel: *Amazon*.
1. Geben Sie für die **Client-ID** die Client-ID der Amazon-Anwendung ein, die Sie zuvor erstellt haben.
1. Geben Sie für **Geheimer Clientschlüssel** den zuvor notierten geheimen Clientschlüssel ein.
1. Wählen Sie **Speichern** aus.
