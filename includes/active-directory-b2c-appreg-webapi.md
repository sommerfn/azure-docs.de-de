---
author: mmacy
ms.service: active-directory-b2c
ms.topic: include
ms.date: 09/30/2019
ms.author: marsma
ms.openlocfilehash: 4ac69b51e69031a1a4ce83a2c09dc6cc010ccd62
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71720272"
---
Web-API-Ressourcen müssen bei Ihrem Mandanten registriert werden, damit sie geschützte Ressourcenanforderungen von Clientanwendungen, die ein Zugriffstoken bereitstellen, akzeptieren und darauf reagieren können.

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
