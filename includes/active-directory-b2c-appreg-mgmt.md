---
author: mmacy
ms.service: active-directory-b2c
ms.topic: include
ms.date: 10/01/2019
ms.author: marsma
ms.openlocfilehash: 67a0832f868fecd47983aa8cddff9cdf139b3f2a
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71702185"
---
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
