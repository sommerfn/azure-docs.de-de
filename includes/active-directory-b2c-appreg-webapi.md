---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: a286f8d401ec98a5069c07b1a82acdf4af2b8185
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73641843"
---
Web-API-Ressourcen müssen bei Ihrem Mandanten registriert werden, damit sie geschützte Ressourcenanforderungen von Clientanwendungen, die ein Zugriffstoken bereitstellen, akzeptieren und darauf reagieren können.

Zum Registrieren einer Anwendung in Ihrem Azure AD B2C-Mandanten können Sie die aktuelle Benutzeroberfläche für **Anwendungen** oder unsere neue einheitliche Benutzeroberfläche **App-Registrierungen (Vorschau)** verwenden. Weitere Informationen zu der neuen Oberfläche finden Sie [hier](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Anwendungen](#tab/applications/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
1. Geben Sie einen Namen für die Anwendung ein. Beispiel: *webapi1*.
1. Wählen Sie für **Web-App/Web-API** die Option **Nein** aus.
1. Wählen Sie für **Impliziten Ablauf zulassen** die Option **Ja** aus.
1. Geben Sie für **Antwort-URLs** einen Endpunkt ein, an den Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Das Beispiel in diesem Tutorial wird lokal ausgeführt und lauscht an `https://localhost:5000`.
1. Fügen Sie für **App-ID-URI** eine API-Endpunkt-ID für den angezeigten URI hinzu. Geben Sie für dieses Tutorial `api` ein, sodass der vollständige URI ähnlich wie `https://contosob2c.onmicrosoft.com/api` ist.
1. Klicken Sie auf **Erstellen**.
1. Notieren Sie sich die **ANWENDUNGS-ID** zur Verwendung in einem späteren Schritt.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **App-Registrierungen (Vorschau)** aus, und wählen Sie dann **Neue Registrierung** aus.
1. Geben Sie unter **Name** einen Namen für die Anwendung ein. Beispiel: *webapi1*.
1. Wählen Sie unter **Umleitungs-URI** die Option **Web** aus, und geben Sie dann einen Endpunkt ein, an den Azure AD B2C von Ihrer Anwendung angeforderte Token zurückgeben soll. Das Beispiel in diesem Tutorial wird lokal ausgeführt und lauscht an `http://localhost:5000`.
1. Wählen Sie **Registrieren**.
1. Notieren Sie sich die **Anwendungs-ID (Client)** zur Verwendung in einem späteren Schritt.

Aktivieren Sie als Nächstes den Flow zur impliziten Genehmigung:

1. Wählen Sie unter **Verwalten** die Option **Authentifizierung** aus.
1. Wählen Sie **Neue Benutzeroberfläche ausprobieren** aus (sofern die Option angezeigt wird).
1. Aktivieren Sie unter **Implizite Genehmigung** die Kontrollkästchen **Zugriffstoken** und **ID-Token**.
1. Wählen Sie **Speichern** aus.