---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: 0b5c5fdddeea961858a2c3b8cd69c365bb28f26e
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73799842"
---
Zum Registrieren einer Anwendung in Ihrem Azure AD B2C-Mandanten können Sie die aktuelle Benutzeroberfläche für **Anwendungen** oder unsere neue einheitliche Benutzeroberfläche **App-Registrierungen (Vorschau)** verwenden. Weitere Informationen zu der neuen Oberfläche finden Sie [hier](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Anwendungen](#tab/applications/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **Anwendungen** und dann **Hinzufügen** aus.
1. Geben Sie einen Namen für die Anwendung ein. Beispiel: *nativeapp1*.
1. Wählen Sie unter **Nativer Client** die Option **Ja** aus.
1. Geben Sie einen **Benutzerdefinierte Umleitungs-URI** mit einem eindeutigen Schema ein. Beispiel: `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`. Bei der Auswahl eines Umleitungs-URIs sind zwei Aspekte zu berücksichtigen:
    * **Eindeutigkeit**: Das Schema für den Umleitungs-URI muss für jede Anwendung eindeutig sein. Im Beispiel `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect` ist `com.onmicrosoft.contosob2c.exampleapp` das Schema. Dieses Muster sollte befolgt werden. Wenn zwei Anwendungen dasselbe Schema verwenden, erhält der Benutzer die Möglichkeit, eine Anwendung auszuwählen. Wenn der Benutzer einen falschen Eintrag auswählt, tritt bei der Anmeldung ein Fehler auf.
    * **Vollständigkeit**: Der Umleitungs-URI muss ein Schema und einen Pfad aufweisen. Der Pfad muss mindestens einen Schrägstrich nach der Domäne enthalten. Beispielsweise funktioniert `//oauth/`, während bei `//oauth` ein Fehler auftritt. Verwenden Sie keine Sonderzeichen im URI, z. B. Unterstriche.
1. Klicken Sie auf **Erstellen**.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **App-Registrierungen (Vorschau)** und dann **Neue Registrierung** aus.
1. Geben Sie unter **Name** einen Namen für die Anwendung ein. Beispiel: *nativeapp1*.
1. Wählen Sie unter **Unterstützte Kontotypen** die Option **Konten in einem Organisationsverzeichnis oder ein beliebiger Identitätsanbieter** aus.
1. Verwenden Sie die Dropdownliste unter **Umleitungs-URI**, um **Öffentlicher Client/nativ (mobil und Desktop)** auszuwählen.
1. Geben Sie einen Umleitungs-URI mit einem eindeutigen Schema ein. Beispiel: `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`. Bei der Auswahl eines Umleitungs-URIs sind zwei Aspekte zu berücksichtigen:
    * **Eindeutigkeit**: Das Schema für den Umleitungs-URI muss für jede Anwendung eindeutig sein. Im Beispiel `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect` ist `com.onmicrosoft.contosob2c.exampleapp` das Schema. Dieses Muster sollte befolgt werden. Wenn zwei Anwendungen dasselbe Schema verwenden, erhält der Benutzer die Möglichkeit, eine Anwendung auszuwählen. Wenn der Benutzer einen falschen Eintrag auswählt, tritt bei der Anmeldung ein Fehler auf.
    * **Vollständigkeit**: Der Umleitungs-URI muss ein Schema und einen Pfad aufweisen. Der Pfad muss mindestens einen Schrägstrich nach der Domäne enthalten. Beispielsweise funktioniert `//oauth/`, während bei `//oauth` ein Fehler auftritt. Verwenden Sie keine Sonderzeichen im URI, z. B. Unterstriche.
1. Aktivieren Sie unter **Berechtigungen** das Kontrollkästchen *Administratoreinwilligung für openid- und offline_access-Berechtigungen erteilen*.
1. Wählen Sie **Registrieren**.