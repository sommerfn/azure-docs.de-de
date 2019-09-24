---
title: Einrichten der Registrierung und Anmeldung mit einem Weibo-Konto mithilfe von Azure Active Directory B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Weibo-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 4a019c3f197f123486dd28dc2bdb46e34f6cabc4
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064919"
---
# <a name="set-up-sign-up-and-sign-in-with-a-weibo-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Weibo-Konto mithilfe von Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-weibo-application"></a>Erstellen einer Weibo-Anwendung

Um ein Weibo-Konto als Identitätsanbieter in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie in Ihrem Mandanten eine Anwendung erstellen, die es darstellt. Wenn Sie noch über kein Weibo-Konto verfügen, können Sie sich unter [https://weibo.com/signup/signup.php?lang=en-us](https://weibo.com/signup/signup.php?lang=en-us) registrieren.

1. Melden Sie sich beim [Weibo-Entwicklerportal](https://open.weibo.com/) mit den Anmeldeinformationen für Ihr Weibo-Konto an.
1. Wählen Sie nach der Anmeldung oben rechts Ihren Anzeigenamen aus.
1. Wählen Sie in der Dropdownliste die Option **编辑开发者信息** (Entwicklerinformationen bearbeiten) aus.
1. Geben Sie die erforderlichen Informationen ein, und wählen Sie **提交** (Senden) aus.
1. Schließen Sie den E-Mail-Überprüfungsprozess ab.
1. Rufen Sie die Seite [Identitätsüberprüfung](https://open.weibo.com/developers/identity/edit) auf.
1. Geben Sie die erforderlichen Informationen ein, und wählen Sie **提交** (Senden) aus.

### <a name="register-a-weibo-application"></a>Registrieren einer Weibo-Anwendung

1. Navigieren Sie zur Seite für die [Registrierung einer neuen Weibo-App](https://open.weibo.com/apps/new).
1. Geben Sie die erforderlichen App-Informationen ein.
1. Wählen Sie **创建** (Erstellen) aus.
1. Kopieren Sie die Werte von **App-Schlüssel** und **App-Geheimnis**. Sie benötigen beide Angaben, um den Identitätsanbieter Ihrem Mandanten hinzuzufügen.
1. Laden Sie die erforderlichen Fotos hoch und geben Sie die benötigten Informationen ein.
1. Wählen Sie **保存以上信息** (Speichern) aus.
1. Wählen Sie **高级信息** (Weitere Informationen) aus.
1. Wählen Sie neben dem Feld **授权设置** (Umleitungs-URL) für OAuth2.0 die Option **编辑** (Bearbeiten) aus.
1. Geben Sie für **授权设置** (Umleitungs-URL) für OAuth2.0 die Zeichenfolge `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` ein. Wenn der Name Ihres Mandanten beispielsweise „contoso“ lautet, legen Sie die URL auf `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp` fest.
1. Wählen Sie **提交** (Senden) aus.

## <a name="configure-a-weibo-account-as-an-identity-provider"></a>Konfigurieren eines Weibo-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
1. Wählen Sie **Identitätsanbieter** und dann **Weibo (Vorschau)** aus.
1. Geben Sie einen **Namen** ein. Beispiel: *Weibo*.
1. Geben Sie für die **Client-ID** den App-Schlüssel der Weibo-Anwendung ein, die Sie zuvor erstellt haben.
1. Geben Sie das zuvor notierte App-Geheimnis als **Geheimer Clientschlüssel** ein.
1. Wählen Sie **Speichern** aus.
