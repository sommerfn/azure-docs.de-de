---
title: Einrichten der Registrierung und Anmeldung mit einem WeChat-Konto mithilfe von Azure Active Directory B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit WeChat-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: ad5e5b766c38b2ce3c48c22e62dd1fd2b9a79853
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064964"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem WeChat-Konto mithilfe von Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-wechat-application"></a>Erstellen einer WeChat-Anwendung

Um ein WeChat-Konto als Identitätsanbieter in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie in Ihrem Mandanten eine Anwendung erstellen, die es darstellt. Wenn Sie noch über kein WeChat-Konto verfügen, können Sie unter [https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html](https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html) eines erstellen.

### <a name="register-a-wechat-application"></a>Registrieren einer WeChat-Anwendung

1. Melden Sie sich unter [https://open.weixin.qq.com/](https://open.weixin.qq.com/) mit Ihren WeChat-Anmeldeinformationen an.
1. Wählen Sie **管理中心** (Verwaltungscenter) aus.
1. Führen Sie die Schritte zum Registrieren einer neuen Anwendung aus.
1. Geben Sie `https://your-tenant_name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` unter **授权回调域** (Rückruf-URL) ein. Wenn der Name Ihres Mandanten beispielsweise „contoso“ lautet, legen Sie die URL auf `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp` fest.
1. Kopieren Sie die **APP-ID** und den **APP-SCHLÜSSEL**. Sie benötigen diese Angaben, um den Identitätsanbieter Ihrem Mandanten hinzuzufügen.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von WeChat als Identitätsanbieter in Ihrem Mandanten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
1. Wählen Sie **Identitätsanbieter** und dann **WeChat (Vorschau)** aus.
1. Geben Sie einen **Namen** ein. Beispiel: *WeChat*.
1. Geben Sie für die **Client-ID** die App-ID der WeChat-Anwendung ein, die Sie zuvor erstellt haben.
1. Geben Sie das zuvor notierte App-Geheimnis als **Geheimer Clientschlüssel** ein.
1. Wählen Sie **Speichern** aus.
