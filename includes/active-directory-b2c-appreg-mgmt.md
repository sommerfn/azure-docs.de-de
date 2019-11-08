---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: baf39c1fe72703d8ebc18b03bae1c963536d7ced
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73475210"
---
Zum Registrieren einer Anwendung in Ihrem Azure AD B2C-Mandanten können Sie die aktuelle Benutzeroberfläche für **Anwendungen** oder unsere neue einheitliche Benutzeroberfläche **App-Registrierungen (Vorschau)** verwenden. Weitere Informationen zur Vorschaubenutzeroberfläche finden Sie [hier](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Anwendungen](#tab/applications/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure Active Directory** (*nicht* „Azure AD B2C“) aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach **Azure Active Directory**, und wählen Sie dann diese Option aus.
1. Wählen Sie unter **Verwalten** die Option **App-Registrierungen (Legacy)** aus.
1. Wählen Sie **Registrierung einer neuen Anwendung** aus.
1. Geben Sie einen Namen für die Anwendung ein. Zum Beispiel *managementapp1*.
1. Wählen Sie für **Anwendungstyp** die Option **Web-App/API** aus.
1. Geben Sie im Feld **Anmelde-URL** eine gültige URL ein. Beispiel: `https://localhost`. Der Endpunkt muss nicht erreichbar sein, aber es muss sich dabei um eine gültige URL handeln.
1. Klicken Sie auf **Erstellen**.
1. Notieren Sie sich die **Anwendungs-ID**, die auf der Übersichtsseite **Registrierte App** angezeigt wird. Sie verwenden diesen Wert in einem späteren Schritt.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie im linken Menü die Option **Azure AD B2C** aus. Oder wählen Sie **Alle Dienste** aus, suchen Sie nach dem Eintrag **Azure AD B2C**, und wählen Sie ihn aus.
1. Wählen Sie **App-Registrierungen (Vorschau)** und dann **Neue Registrierung**  aus.
1. Geben Sie einen **Namen** für die Anwendung ein. Zum Beispiel *managementapp1*.
1. Wählen Sie **Nur Konten in diesem Organisationsverzeichnis** aus.
1. Deaktivieren Sie unter **Berechtigungen** das Kontrollkästchen *Administratoreinwilligung für openid- und offline_access-Berechtigungen erteilen*.
1. Wählen Sie **Registrieren**.
1. Notieren Sie die **Anwendungs-ID (Client)** , die auf der Übersichtsseite der Anwendung angezeigt wird. Sie verwenden diesen Wert in einem späteren Schritt.