---
title: 'Einrichten eines mobilen Geräts als Methode für die zweistufige Überprüfung: Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie ein mobiles Gerät als Methode für die zweistufige Überprüfung einrichten.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 08/12/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f03e92c1bfd86d15bf582487e3e92a3713b63e2
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69619821"
---
# <a name="set-up-a-mobile-device-as-your-two-factor-verification-method"></a>Einrichten eines mobilen Geräts als Methode für die zweistufige Überprüfung

Sie können Ihr mobiles Gerät als Methode für die zweistufige Überprüfung einrichten. Ihr Mobiltelefon kann entweder eine SMS mit einem Prüfcode oder einen Telefonanruf empfangen.

>[!Note]
> Ist die Option für das Authentifizierungstelefon abgeblendet, lässt Ihre Organisation unter Umständen die Nutzung einer Telefonnummer oder SMS für die Überprüfung nicht zu. In diesem Fall müssen Sie eine andere Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

## <a name="set-up-your-mobile-device-to-use-a-text-message-as-your-verification-method"></a>Einrichten Ihres mobilen Geräts zur Verwendung einer SMS als Überprüfungsmethode

1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** die Option **Telefon für Authentifizierung** im Bereich **Schritt 1: Auf welchem Weg sollen wir Sie kontaktieren?** aus. Wählen Sie anschließend in der Dropdownliste Ihr Land oder Ihre Region aus, und geben Sie die Telefonnummer Ihres mobilen Geräts ein.

2. Wählen Sie im Bereich **Methode** die Option **Code per SMS an mich senden** und anschließend **Weiter** aus.

    ![Seite „Zusätzliche Sicherheitsüberprüfung“ mit Authentifizierungstelefon und SMS](media/multi-factor-authentication-verification-methods/multi-factor-authentication-text-message.png)

3. Geben Sie den Prüfcode aus der SMS von Microsoft im Bereich **Step 2: We've sent a text message to your phone** (Schritt 2: Wir haben eine SMS an Ihr Telefon gesendet.) ein, und wählen Sie dann **Überprüfen** aus.

    ![Seite „Zusätzliche Sicherheitsüberprüfung“ mit Authentifizierungstelefon und SMS](media/multi-factor-authentication-verification-methods/multi-factor-authentication-text-message-test.png)

4. Kopieren Sie im Bereich **Schritt 3: Verwenden Sie vorhandene Anwendungen weiterhin** das angegebene App-Kennwort, und speichern Sie es an einem sicheren Ort.

    ![Bereich für App-Kennwörter der Seite „Zusätzliche Sicherheitsüberprüfung“](media/multi-factor-authentication-verification-methods/multi-factor-authentication-app-passwords.png)

    >[!Note]
    >Informationen zur Verwendung des App-Kennworts mit älteren Apps finden Sie unter [Verwalten von App-Kennwörtern für die zweistufige Überprüfung](multi-factor-authentication-end-user-app-passwords.md). App-Kennwörter sind nur erforderlich, wenn Sie weiterhin ältere Apps verwenden, die die zweistufige Überprüfung nicht unterstützen.

5. Wählen Sie **Fertig**aus.

## <a name="set-up-your-mobile-device-to-receive-a-phone-call"></a>Einrichten Ihres mobilen Geräts zum Empfangen eines Telefonanrufs

1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** die Option **Telefon für Authentifizierung** im Bereich **Schritt 1: Auf welchem Weg sollen wir Sie kontaktieren?** aus. Wählen Sie anschließend in der Dropdownliste Ihr Land oder Ihre Region aus, und geben Sie die Telefonnummer Ihres mobilen Geräts ein.

2. Wählen Sie im Bereich **Methode** die Option **Anruf an mich** und anschließend **Weiter** aus.

    ![Seite „Zusätzliche Sicherheitsüberprüfung“ mit Authentifizierungstelefon und Telefonanruf](media/multi-factor-authentication-verification-methods/multi-factor-authentication-phone-call.png)

3. Sie erhalten einen Telefonanruf von Microsoft, in dem Sie aufgefordert werden, die Rautetaste auf Ihrem mobilen Gerät zu drücken, um Ihre Identität zu bestätigen.

    ![Testen der angegebenen Telefonnummer](media/multi-factor-authentication-verification-methods/multi-factor-authentication-phone-call-test.png)

4. Kopieren Sie im Bereich **Schritt 3: Verwenden Sie vorhandene Anwendungen weiterhin** das angegebene App-Kennwort, und speichern Sie es an einem sicheren Ort.

    ![Bereich für App-Kennwörter der Seite „Zusätzliche Sicherheitsüberprüfung“](media/multi-factor-authentication-verification-methods/multi-factor-authentication-app-passwords.png)

    >[!Note]
    >Informationen zur Verwendung des App-Kennworts mit älteren Apps finden Sie unter [Verwalten von App-Kennwörtern für die zweistufige Überprüfung](multi-factor-authentication-end-user-app-passwords.md). App-Kennwörter sind nur erforderlich, wenn Sie weiterhin ältere Apps verwenden, die die zweistufige Überprüfung nicht unterstützen.

5. Wählen Sie **Fertig**aus.

## <a name="next-steps"></a>Nächste Schritte

Nach der Einrichtung der Methode für die zweistufige Überprüfung können Sie weitere Methoden hinzufügen, Ihre Einstellungen und App-Kennwörter verwalten, sich anmelden oder Hilfe bei allgemeinen Problemen im Zusammenhang mit der zweistufigen Überprüfung anfordern.

- [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md)

- [Verwalten von App-Kennwörtern](multi-factor-authentication-end-user-app-passwords.md)

- [Der Anmeldevorgang mit Azure Multi-Factor Authentication](multi-factor-authentication-end-user-signin.md)

- [Hilfe bei der zweistufigen Überprüfung](multi-factor-authentication-end-user-troubleshoot.md)
