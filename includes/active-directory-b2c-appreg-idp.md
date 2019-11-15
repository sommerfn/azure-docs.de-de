---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: dc75d78f2e73d1aa42f5fc84d39a8b22d6a121bb
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73642597"
---
Zum Registrieren einer Anwendung in Ihrem Azure AD B2C-Mandanten können Sie die aktuelle Benutzeroberfläche für **Anwendungen** oder unsere neue einheitliche Benutzeroberfläche **App-Registrierungen (Vorschau)** verwenden. Weitere Informationen zu der neuen Oberfläche finden Sie [hier](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Anwendungen](#tab/applications/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
1. Geben Sie einen Namen für die Anwendung ein. Zum Beispiel *testapp1*.
1. Wählen Sie für **Web-App/Web-API** die Option **Ja** aus.
1. Geben Sie für **Antwort-URL** die URL `https://jwt.ms` ein.
1. Klicken Sie auf **Erstellen**.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **App-Registrierungen (Vorschau)** aus, und wählen Sie dann **Neue Registrierung** aus.
1. Geben Sie einen **Namen** für die Anwendung ein. Zum Beispiel *testapp1*.
1. Wählen Sie **Konten in einem Organisationsverzeichnis oder ein beliebiger Identitätsanbieter** aus.
1. Wählen Sie unter **Umleitungs-URI** die Option **Web** aus, und geben Sie `https://jwt.ms` in das URL-Textfeld ein.
1. Aktivieren Sie unter **Berechtigungen** das Kontrollkästchen *Administratoreinwilligung für openid- und offline_access-Berechtigungen erteilen*.
1. Wählen Sie **Registrieren**.

Aktivieren Sie nach Abschluss der Anwendungsregistrierung den Ablauf zur impliziten Gewährung:

1. Wählen Sie unter **Verwalten** die Option **Authentifizierung** aus.
1. Wählen Sie **Neue Benutzeroberfläche ausprobieren** aus (sofern die Option angezeigt wird).
1. Aktivieren Sie unter **Implizite Genehmigung** die Kontrollkästchen **Zugriffstoken** und **ID-Token**.
1. Wählen Sie **Speichern** aus.