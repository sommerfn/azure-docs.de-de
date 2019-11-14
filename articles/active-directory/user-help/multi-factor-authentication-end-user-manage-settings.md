---
title: Verwalten der Einstellungen für die zweistufige Überprüfung – Azure AD
description: Erfahren Sie, wie Sie die Details Ihrer Methode für die Sicherheitsüberprüfung in Bezug auf die zweistufige Überprüfung ändern.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c402e0992a3a04d674477bc9dd19e0181fd6775
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73805259"
---
# <a name="manage-your-two-factor-verification-method-settings"></a>Verwalten der Einstellungen für die zweistufige Überprüfung

Nachdem Sie Ihre Methoden für die Sicherheitsüberprüfung für Ihre Geschäfts-, Schul- oder Unikonto eingerichtet haben, können Sie alle zugehörigen Details aktualisieren. Folgendes ist möglich:

- Sie können Ihre Standardmethode für die Sicherheitsüberprüfung auswählen.

- Sie können Details zu Ihrer Methode für die Sicherheitsüberprüfung hinzufügen oder aktualisieren, z. B. eine Telefonnummer.

- Sie können eine neue Authentifikator-App einrichten oder ein Gerät aus der Authentifikator-App löschen.

## <a name="using-the-additional-security-verification-page"></a>Verwenden der Seite „Zusätzliche Sicherheitsüberprüfung“

Wenn Ihre Organisation Ihnen bestimmte Schritte zum Aktivieren und Verwalten der zweistufigen Überprüfung zur Verfügung gestellt hat, sollten Sie diese Anweisungen befolgen. Andernfalls finden Sie die Einstellungen Ihrer Methode für die Sicherheitsüberprüfung auf der Seite [Zusätzliche Sicherheitsüberprüfung](https://aka.ms/mfasetup).

>[!Note]
>Wenn die auf Ihrem Bildschirm angezeigten Optionen nicht den in diesem Artikel behandelten Inhalten entsprechen, hat Ihr Administrator die Benutzeroberfläche „Sicherheitsinformation“ (Vorschau) aktiviert, oder Ihre Organisation verfügt über ein eigenes benutzerdefiniertes Portal. Weitere Informationen zur Benutzeroberfläche „Sicherheitsinformation“ finden Sie unter [Übersicht über die Sicherheitsinformationen (Vorschau)](user-help-security-info-overview.md). Um weitere Informationen zum benutzerdefinierten Portal Ihrer Organisation zu erhalten, wenden Sie sich an Ihren Helpdesk.

### <a name="to-get-to-the-additional-security-verification-page"></a>So gelangen Sie zur Seite „Zusätzliche Sicherheitsüberprüfung“

- Wechseln Sie zur Adresse https://aka.ms/mfasetup.

    ![Bildschirm der Seite „Zusätzliche Sicherheitsüberprüfung“ mit Details zu den verfügbaren Methoden für die Sicherheitsüberprüfung](./media/multi-factor-authentication-end-user-manage-settings/mfa-security-verification-page.png)

    Wenn Sie nicht auf diesen Link klicken können, können Sie auch die Seite **Zusätzliche Sicherheitsüberprüfung** aufrufen, indem Sie die folgenden Schritte ausführen:

    1. Melden Sie sich bei [https://myapps.microsoft.com](https://myapps.microsoft.com) an.

    2. Wählen Sie oben rechts Ihren Kontonamen aus, und wählen Sie dann **Profil**.

    3. Klicken Sie auf **Zusätzliche Sicherheitsüberprüfung**.  

        ![Meine Apps-Link zur Seite „Zusätzliche Sicherheitsüberprüfung“](./media/multi-factor-authentication-end-user-manage-settings/mfa-myapps-link.png)

>[!Note]
>Informationen zum Verwenden des Abschnitts **App-Kennwörter** der Seite **Zusätzliche Sicherheitsüberprüfung** finden Sie unter [Verwalten von App-Kennwörtern für die zweistufige Überprüfung](multi-factor-authentication-end-user-app-passwords.md). App-Kennwörter sollten nur für Apps verwendet werden, die noch keine zweistufige Überprüfung unterstützen.

## <a name="change-your-default-security-verification-method"></a>Ändern Ihrer Standardmethode für die Sicherheitsüberprüfung

Nachdem Sie sich mit Ihrem Benutzernamen und Kennwort bei Ihrem Geschäfts-, Schul- oder Unikonto angemeldet haben, wird Ihnen Ihre ausgewählte Methode für die Sicherheitsüberprüfung automatisch angezeigt. Abhängig von den Anforderungen Ihrer Organisation kann es sich hierbei um einen Benachrichtigungs- oder Überprüfungscode für eine Authentifikator-App, eine SMS oder einen Telefonanruf handeln.

Wenn Sie die von Ihnen verwendete Standardmethode für die Sicherheitsüberprüfung ändern möchten, können Sie dies hier tun.

### <a name="to-change-your-default-security-verification-method"></a>So ändern Sie Ihre Standardmethode für die Sicherheitsüberprüfung

1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** aus der Liste **Welche ist Ihre bevorzugte Option?** die gewünschte Methode aus. Ihnen werden alle Optionen angezeigt, Sie können aber nur diejenigen auswählen, die Ihnen von Ihrer Organisation zur Verfügung gestellt werden.

    - **Mich durch die App benachrichtigen**. Sie erhalten eine Benachrichtigung über Ihre Authentifikator-App, das eine Aufforderung zur Überprüfung auf Sie wartet.

    - **Mein Authentifizierungstelefon anrufen**. Sie erhalten einen Anruf auf Ihrem Mobilgerät, in dem Sie aufgefordert werden, Ihre Informationen zu bestätigen.

    - **Textcode an mein Authentifizierungstelefon**. Sie erhalten auf Ihrem Mobilgerät eine SMS mit einem Überprüfungscode. Diesen Code müssen Sie bei der Überprüfungsaufforderung für Ihr Geschäfts-, Schul- oder Unikonto eingeben.

    - **Meine geschäftliche Rufnummer anrufen**. Sie erhalten einen Anruf auf Ihrem geschäftlichen Telefonanschluss, in dem Sie aufgefordert werden, Ihre Informationen zu bestätigen.

    - **Prüfcode aus der App verwenden**. Sie verwenden Ihre Authentifikator-App, um einen Überprüfungscode abzurufen, den Sie in die Überprüfungsaufforderung für Ihr Geschäfts-, Schul- oder Unikonto eingeben.

2. Wählen Sie **Speichern** aus.

## <a name="add-or-change-your-phone-number"></a>Hinzufügen oder Ändern Ihrer Telefonnummer

Auf der Seite **Zusätzliche Sicherheitsüberprüfung** können Sie neue Telefonnummern hinzufügen oder vorhandene Nummern ändern.

>[!Important]
>Es wird dringend empfohlen, eine zweite Telefonnummer hinzuzufügen, um zu verhindern, dass Sie aus Ihrem Konto ausgesperrt sind, wenn Ihr primäres Telefon verloren geht oder gestohlen wird oder wenn Sie ein neues Telefon verwenden und nicht mehr über Ihre ursprüngliche primäre Telefonnummer verfügen.

### <a name="to-change-your-phone-numbers"></a>So ändern Sie Ihre Telefonnummern

1. Aktualisieren Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** im Abschnitt **Wie möchten Sie antworten?** die Telefonnummer für Ihr **Authentifizierungstelefon** (Ihr primäres mobiles Gerät) und Ihr **Bürotelefon**.

2. Wählen Sie das Feld neben der Option **Alternatives Authentifizierungstelefon** aus, und geben Sie eine sekundäre Telefonnummer ein, unter der Sie SMS oder Anrufe empfangen können, falls Sie nicht auf Ihr primäres Telefon zugreifen können.

3. Wählen Sie **Speichern** aus.

## <a name="add-a-new-account-to-the-microsoft-authenticator-app"></a>Hinzufügen eines neuen Kontos zur Microsoft Authenticator-App

Sie können Ihr Geschäfts-, Schul- oder Unikonto in der Microsoft Authenticator-App für [Android](https://play.google.com/store/apps/details?id=com.azure.authenticator) oder [iOS](https://apps.apple.com/app/microsoft-authenticator/id983156458) einrichten.

Wenn Sie Ihr Geschäfts-, Schul- oder Unikonto bereits in der Microsoft Authenticator-App eingerichtet haben, müssen Sie dies nicht erneut tun.

1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** im Abschnitt **Wie möchten Sie antworten?** die Schaltfläche **Authenticator-App einrichten** aus.

    ![Einrichten Ihres Geschäfts-, Schul- oder Unikontos in der Microsoft Authenticator-App](./media/multi-factor-authentication-end-user-manage-settings/mfa-security-verification-page-auth-app.png)

2. Befolgen Sie die Anweisungen auf dem Bildschirm (u.a. müssen Sie den QR-Code mit dem Mobilgerät scannen), und wählen Sie dann **Weiter** aus.

    Sie werden aufgefordert, eine Benachrichtigung über die Microsoft Authenticator-App zu genehmigen, um Ihre Informationen zu bestätigen.

3. Wählen Sie **Speichern** aus.

## <a name="delete-your-account-or-device-from-the-microsoft-authenticator-app"></a>Löschen Ihres Kontos oder Ihres Geräts aus der Microsoft Authenticator-App

Sie können Ihr Konto aus der Microsoft Authenticator-App löschen, und Sie können Ihr Gerät aus Ihrem Geschäfts-, Schul- oder Unikonto löschen. In der Regel löschen Sie ein Gerät, wenn Sie ein verlorenes, gestohlenes oder altes Gerät dauerhaft aus Ihrem Konto entfernen möchten. Das Löschen eines Kontos erfolgt meist, um Verbindungsprobleme zu beheben oder eine Kontoänderung umzusetzen, z. B. aufgrund eines neuen Benutzernamens.

### <a name="to-delete-your-device-from-your-work-or-school-account"></a>So löschen Sie Ihr Gerät aus Ihrem Geschäfts-, Schul- oder Unikonto

1. Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** im Abschnitt **Wie möchten Sie antworten?** die Schaltfläche **Authenticator-App einrichten** aus.

2. Wählen Sie **Speichern** aus.

### <a name="to-delete-your-account-from-the-microsoft-authenticator-app"></a>So löschen Sie Ihr Konto aus der Microsoft Authenticator-App

- Wählen Sie in der Microsoft Authenticator-App die Schaltfläche **Löschen** neben dem Gerät aus, das Sie löschen möchten.

## <a name="turn-on-two-factor-verification-prompts-on-a-trusted-device"></a>Aktivieren von Aufforderungen der zweistufigen Überprüfung auf einem vertrauenswürdigen Gerät

Abhängig von den Einstellungen Ihrer Organisation steht möglicherweise ein Kontrollkästchen **Die nächsten X Tage nicht erneut fragen** zur Verfügung, wenn Sie die zweistufige Überprüfung in Ihrem Browser ausführen. Wenn Sie dieses Kontrollkästchen aktiviert haben, um keine Aufforderungen für die zweistufige Überprüfung mehr zu erhalten und Ihr Gerät dann verloren geht oder möglicherweise kompromittiert wurde, sollten Sie die Aufforderungen wieder aktivieren, um Ihr Konto besser zu schützen. Leider können Sie die Aufforderungen nicht für ein einzelnes Gerät aktivieren. Sie müssen die Aufforderungen für all Ihre Geräte gleichzeitig aktivieren.

### <a name="to-turn-two-factor-verification-prompts-back-on-for-your-devices"></a>So aktivieren Sie die Aufforderungen für die zweistufige Überprüfung wieder für Ihre Geräte

- Wählen Sie auf der Seite **Zusätzliche Sicherheitsüberprüfung** die Option **Multi-Factor Authentication auf Geräten wiederherstellen, die zuvor als vertrauenswürdig eingestuft worden sind**.

    Bei der nächsten Anmeldung bei einem Gerät werden Sie aufgefordert, die zweistufige Überprüfung durchzuführen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Einstellungen für die zweistufige Überprüfung hinzugefügt oder aktualisiert haben, können Sie Ihre App-Kennwörter verwalten, sich anmelden oder Hilfe bei allgemeinen Problemen im Zusammenhang mit der zweistufigen Überprüfung erhalten.

- [Verwalten Sie App-Kennwörter für die zweistufige Überprüfung](multi-factor-authentication-end-user-app-passwords.md) für alle Apps, die die zweistufige Überprüfung nicht unterstützen.

- [Anmelden per zweistufiger Überprüfung](multi-factor-authentication-end-user-signin.md)

- [Hilfe bei der zweistufigen Überprüfung](multi-factor-authentication-end-user-troubleshoot.md)
