---
title: 'Tutorial: Registrieren einer Anwendung – Azure Active Directory B2C'
description: Erfahren Sie, wie Sie eine Webanwendungen in Azure Active Directory B2C mithilfe des Azure-Portals registrieren.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 08/23/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 651c15c8206f7956bb35520f9c5837cb0c9308f9
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980708"
---
# <a name="tutorial-register-an-application-in-azure-active-directory-b2c"></a>Tutorial: Registrieren Sie eine Anwendung in Azure Active Directory B2C

Bevor Ihre [Anwendungen](active-directory-b2c-apps.md) mit Azure Active Directory (Azure AD) B2C interagieren können, müssen sie in einem von Ihnen verwalteten Mandanten registriert werden. In diesem Tutorial erfahren Sie, wie Sie eine Webanwendung mithilfe des Azure-Portals registrieren.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Registrieren einer Webanwendung
> * Erstellen eines Clientgeheimnisses

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie Ihren eigenen [Azure AD B2C-Mandanten](tutorial-create-tenant.md) noch nicht erstellt haben, erstellen Sie jetzt einen. Sie können einen vorhandenen Azure AD B2C-Mandanten verwenden.

## <a name="register-a-web-application"></a>Registrieren einer Webanwendung

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
1. Geben Sie einen Namen für die Anwendung ein. Beispiel: *webapp1*.
1. Wählen Sie für **Web-App/Web-API einschließen** und n**Impliziten Ablauf zulassen** die Option **Ja**.
1. Geben Sie für **Antwort-URLs** einen Endpunkt ein, an den Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Sie können sie so einrichten, dass sie lokal bei `https://localhost:44316` lauschen. Wenn Sie die Portnummer noch nicht kennen, können Sie einen Platzhalterwert eingeben und diesen später ändern.

    Zu Testzwecken wie in diesem Tutorial können Sie `https://jwt.ms` festlegen. Damit zeigen Sie den Inhalt eines Tokens für die Überprüfung an. Legen Sie in diesem Tutorial **Antwort-URL** auf `https://jwt.ms` fest.

    Für Antwort-URLs gelten die folgenden Einschränkungen:

    * Die Antwort-URL muss mit dem Schema `https` beginnen.
    * Bei der Antwort-URL muss die Groß-/Kleinschreibung beachtet werden. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. Wenn Ihre Anwendung z. B. als Teil des Pfads `.../abc/response-oidc` enthält, geben Sie in der Antwort-URL nicht `.../ABC/response-oidc` an. Weil der Webbrowser bei Pfaden die Groß-/Kleinschreibung beachtet, werden Cookies, die `.../abc/response-oidc` zugeordnet sind, möglicherweise ausgeschlossen, wenn eine Umleitung an die anders geschriebene (nicht übereinstimmende) URL `.../ABC/response-oidc` erfolgt.

1. Klicken Sie auf **Erstellen**, um die Anwendungsregistrierung abzuschließen.

## <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses

Wenn Ihre Anwendung einen Code für ein Token austauscht, müssen Sie ein Anwendungsgeheimnis erstellen.

1. Wählen Sie auf der Seite **Azure AD B2C – Anwendungen** die von Ihnen erstellte Anwendung aus, z.B. *webapp1*.
1. Wählen Sie **Schlüssel** und dann **Schlüssel generieren** aus.
1. Wählen Sie **Speichern**, um den Schlüssel anzuzeigen. Notieren Sie sich den Wert für **App-Schlüssel**. Sie verwenden diesen Wert als Anwendungsgeheimnis im Code Ihrer Anwendung.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Registrieren einer Webanwendung
> * Erstellen eines Clientgeheimnisses

Als Nächstes erfahren Sie, wie Sie Benutzerflows erstellen, damit sich die Benutzer registrieren, anmelden und ihre Profile verwalten können.

> [!div class="nextstepaction"]
> [Erstellen von Benutzerflows in Azure Active Directory B2C >](tutorial-create-user-flows.md)
