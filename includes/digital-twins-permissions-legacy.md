---
title: include file
description: include file
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 08/12/2019
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: 9f4bf6fb92b590e274e8880b5f900e5469f85727
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "69012138"
---
1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) im linken Bereich **Azure Active Directory**, und öffnen Sie dann den Bereich **Eigenschaften**. Kopieren Sie **Verzeichnis-ID** in eine temporäre Datei. Mit diesem Wert konfigurieren Sie im nächsten Abschnitt eine Beispielanwendung.

    ![ID des Azure Active Directory-Verzeichnisses](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png)

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) im linken Bereich **Azure Active Directory**, und öffnen Sie dann den Bereich **App-Registrierungen (Legacy)** . Wählen Sie die Schaltfläche **Registrierung einer neuen Anwendung** aus.

1. Geben Sie im Feld **Name** einen Anzeigenamen für diese App-Registrierung ein. Wählen Sie unter **Anwendungstyp** die Option **Nativ** und unter **Umleitungs-URI** die Option `https://microsoft.com` aus. Klicken Sie auf **Erstellen**.

    ![Bereich zum „Erstellen“](./media/digital-twins-permissions-legacy/aad-app-reg-create.png)

1.  Öffnen Sie die registrierte App, und kopieren Sie den Wert im Feld **Anwendungs-ID** in eine temporäre Datei. Dieser Wert identifiziert Ihre Azure Active Directory-App. Mit der Anwendungs-ID konfigurieren Sie in den folgenden Abschnitten Ihre Beispielanwendung.

    ![ID der Azure Active Directory-Anwendung](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png)

1. Öffnen des Bereichs Ihrer App-Registrierung Wählen Sie **Einstellungen** > **Erforderliche Berechtigungen** aus, und gehen Sie dann wie folgt vor:

   a. Wählen Sie links oben **Hinzufügen** aus, um den Bereich **API-Zugriff hinzufügen** zu öffnen.

   b. Wählen Sie **API auswählen**, und suchen Sie nach **Azure Digital Twins**. Wenn die API damit nicht gefunden wird, suchen Sie stattdessen nach **Azure Smart Spaces**.

   c. Wählen Sie die Option **Azure Digital Twins (Azure Smart Spaces Service)** und dann **Auswählen**.

   d. Wählen Sie **Berechtigungen auswählen** aus. Aktivieren Sie das Kontrollkästchen **Lese-/Schreibzugriff** für delegierte Berechtigungen, und wählen Sie **Auswählen** aus.

   e. Wählen Sie im Bereich **API-Zugriff hinzufügen** die Option **Fertig** aus.

   f. Wählen Sie im Bereich **Erforderliche Berechtigungen** die Schaltfläche **Berechtigungen erteilen**, und akzeptieren Sie die angezeigte Bestätigung. Wenn die Berechtigung für diese API nicht gewährt wird, wenden Sie sich an Ihren Administrator.

      ![Bereich „Erforderliche Berechtigungen“](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png)

 