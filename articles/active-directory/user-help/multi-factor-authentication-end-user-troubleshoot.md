---
title: Beheben gängiger Problemen bei der zweistufigen Überprüfung – Azure AD
description: Erfahren Sie mehr über mögliche Lösungen für einige der gängigeren Probleme bei der zweistufigen Überprüfung.
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 10/28/2019
ms.author: lizross
ms.reviewer: kexia
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8d226d61f4b67541a6991fef3a435b4cfff500f
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73805299"
---
# <a name="troubleshoot-common-two-factor-verification-problems"></a>Beheben gängiger Problemen bei der zweistufigen Überprüfung

Ihre Organisation hat die zweistufige Überprüfung aktiviert, was bedeutet, dass für die Anmeldung bei Ihrem Geschäfts-, Schul- oder Unikonto nun eine Kombination aus Ihrem Benutzernamen, Ihrem Kennwort und einem mobilen Gerät oder Telefon erforderlich ist. Ihre Organisation hat diese zusätzliche Überprüfung aktiviert, da sie sicherer als ein bloßes Kennworts ist, denn sie basiert auf zwei Authentifizierungsfaktoren: Sie müssen etwas wissen (das Kennwort) und etwas haben (Ihr Smartphone oder ein ähnliches geeignetes Gerät). Die zweistufige Überprüfung trägt dazu bei, Angriffe durch Hacker zu verhindern, die sich für Sie ausgeben, denn auch wenn ein Hacker Ihr Kennwort weiß, ist die Wahrscheinlichkeit gering, dass er auch über Ihr Gerät verfügt.

Es gibt einige gängige Probleme bei der zweistufigen Überprüfung, die offenbar häufiger auftreten, als uns lieb ist. Wir haben diesen Artikel in der Hoffnung zusammengestellt, einige mögliche Lösungen für die häufigsten Probleme zu bieten.

>[!Important]
>Dieser Inhalt richtet sich an Benutzer. Administratoren finden in der [Azure Active Directory-Dokumentation](https://docs.microsoft.com/azure/active-directory) weitere Informationen zum Einrichten und Verwalten der Azure Active Directory-Umgebung (Azure AD).
>
>Dieser Inhalt ist auch für die Verwendung mit Ihrem Geschäfts-, Schul- oder Unikonto vorgesehen, also dem Konto, das Ihnen von Ihrer Organisation zur Verfügung gestellt wird (z. B. alain@contoso.com). Bei Problemen mit der zweistufigen Überprüfung und Ihrem persönlichen Microsoft-Konto, also dem Konto, das Sie für sich selbst einrichten (z. B. danielle@outlook.com) lesen Sie den Artikel [Aktivieren oder Deaktivieren der Überprüfung in zwei Schritten für Ihr Microsoft-Konto](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off).

## <a name="i-forgot-my-mobile-device-at-home"></a>Ich habe mein mobiles Gerät zu Hause vergessen

Das kommt vor. Sie haben Ihr Mobilgerät zu Hause gelassen, und jetzt können Sie es nicht mehr verwenden, um zu bestätigen, dass Sie derjenige sind, für den Sie sich ausgeben. Wenn Sie zuvor eine andere Methode zur Anmeldung bei Ihrem Konto hinzugefügt haben, z. B. Ihr Bürotelefon, sollten Sie diese Methode jetzt verwenden können. Wenn Sie keine zusätzliche Überprüfungsmethode hinzugefügt haben, müssen Sie Ihren Helpdesk kontaktieren und sich von ihm helfen lassen, wieder auf Ihr Konto zuzugreifen.

### <a name="to-sign-in-to-your-work-or-school-account-using-another-verification-method"></a>So melden Sie sich mit einer anderen Überprüfungsmethode bei Ihrem Geschäfts-, Schul- oder Unikonto an

1. Melden Sie sich wie gewohnt bei Ihrem Konto an, und klicken Sie auf der Seite **Zweistufige Überprüfung** auf den Link **Auf andere Weise anmelden**.

    ![Ändern der Überprüfungsmethode für die Anmeldung](./media/multi-factor-authentication-end-user-troubleshoot/two-factor-auth-signin-another-way.png)

    >[!Note]
    >Wenn Sie den Link **Auf andere Weise anmelden** nicht sehen, bedeutet dies, dass Sie keine anderen Überprüfungsmethoden eingerichtet haben. Wenden Sie sich an Ihren Administrator, damit er Ihnen bei der Anmeldung bei Ihrem Konto hilft.

2. Wählen Sie Ihre alternative Überprüfungsmethode aus, und fahren Sie mit dem zweistufigen Überprüfungsprozess fort.

## <a name="i-lost-my-mobile-device-or-it-was-stolen"></a>Ich habe mein mobiles Gerät verloren, oder es wurde gestohlen

Wenn Ihr mobiles Gerät verloren gegangen ist oder gestohlen wurde, können Sie sich entweder mit einer anderen Methode anmelden, oder Sie können Ihren Helpdesk bitten, Ihre Einstellungen zu löschen. Es wird dringend empfohlen, dass Sie Ihren Helpdesk informieren, wenn Sie Ihr Smartphone verloren haben oder es gestohlen wurde, damit Ihr Konto entsprechend aktualisiert werden kann. Nachdem Ihre Einstellungen gelöscht wurden, werden Sie bei der nächsten Anmeldung aufgefordert, sich für die [zweistufige Überprüfung zu registrieren](multi-factor-authentication-end-user-first-time.md).

## <a name="im-not-getting-the-verification-code-sent-to-my-mobile-device"></a>Ich empfange nicht den Überprüfungscode, der an mein mobiles Gerät gesendet wurde

Das Nichtempfangen des Überprüfungscodes ist ein gängiges Problem, das typischerweise mit Ihrem Mobilgerät und seinen Einstellungen zusammenhängt. Sie können Folgendes versuchen:

- **Starten Sie Ihr mobiles Gerät neu.** Gelegentlich benötigt Ihr Gerät nur einen Neustart. Beim Neustart Ihres Geräts werden alle Hintergrundprozesse oder -dienste beendet, die derzeit ausgeführt werden und Probleme verursachen könnten. Außerdem werden die Kernkomponenten Ihres Geräts aktualisiert und neu gestartet, falls sie irgendwann einmal abgestürzt sein sollten.

- **Überprüfen Sie, ob Ihre Sicherheitsinformationen ordnungsgemäß sind.** Stellen Sie sicher, dass die Informationen zu Ihrer Sicherheitsüberprüfungsmethode zutreffend sind, insbesondere Ihre Telefonnummern. Wenn Sie eine falsche Telefonnummer eingeben, landen alle Ihre Benachrichtigungen bei dieser falschen Nummer. Glücklicherweise kann dieser Benutzer mit den Benachrichtigungen nichts anfangen, aber es wird Ihnen auch nicht helfen, sich bei Ihrem Konto anzumelden. Um sicherzustellen, dass Ihre Informationen richtig sind, lesen Sie die Anweisungen im Artikel [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md).

- **Überprüfen Sie, ob Benachrichtigungen aktiviert sind.** Vergewissern Sie sich, dass auf Ihrem Mobilgerät Benachrichtigungen aktiviert sind und dass Sie eine Benachrichtigungsmethode ausgewählt haben, die es ermöglicht, dass über Anrufe, Ihre Authentifizierungs-App und Ihre Messaging-App (für SMS) sichtbare Benachrichtigungen an Ihr Mobilgerät gesendet werden.

- **Stellen Sie sicher, dass Sie über ein Gerätesignal und eine Internetverbindung verfügen.** Vergewissern Sie sich, dass Ihre Telefonanrufe und SMS-Nachrichten zu Ihrem mobilen Gerät gelangen. Bitten Sie einen Freund, Sie anzurufen und Ihnen eine SMS zu schicken, um sicherzustellen, dass beides möglich ist. Falls nicht, überprüfen Sie zunächst, ob Ihr Mobilgerät eingeschaltet ist. Wenn Ihr Gerät eingeschaltet ist, Sie aber immer noch keinen Anruf bzw. keine SMS erhalten, liegt höchstwahrscheinlich ein Problem mit Ihrem Mobilfunknetz vor, weshalb Sie sich an Ihren Mobilfunkanbieter wenden müssen. Wenn Sie häufig signalbedingte Probleme haben, empfiehlt sich die Installation und Verwendung der [Microsoft Authenticator-App](user-help-auth-app-download-install.md) auf Ihrem mobilen Gerät. Diese App kann zufällige Sicherheitscodes für die Anmeldung generieren, ohne ein Mobilfunksignal oder eine Internetverbindung vorauszusetzen.

- **Deaktivieren Sie „Nicht stören“.** Stellen Sie sicher, dass das Funktionsmerkmal **Nicht stören** für Ihr mobiles Gerät deaktiviert ist. Wenn dieses Funktionsmerkmal aktiviert ist, sind keine Benachrichtigungen auf Ihrem Mobilgerät möglich. In der Bedienungsanleitung Ihres Mobilgeräts finden Sie Anweisungen, wie Sie dieses Funktionsmerkmal deaktivieren können.

- **Aufheben der Blockierung von Telefonnummern** In den USA stammen Anrufe von Microsoft von folgenden Telefonnummern: + 1 (866) 539 4191, + 1 (855) 330 8653 und + 1 (877) 668 6536.

- **Überprüfen Sie akkubezogene Einstellungen.** Dies scheint auf den ersten Blick etwas seltsam zu sein, aber wenn Sie Ihre Akkuroptimierung so eingerichtet haben, dass weniger genutzte Anwendungen nicht im Hintergrund aktiv bleiben, ist Ihr Benachrichtigungssystem höchstwahrscheinlich betroffen. Um dieses Problem zu beheben, deaktivieren Sie die Akkuoptimierung für Ihre Authentifizierungs- und Messaging-App, und versuchen Sie dann, sich erneut bei Ihrem Konto anzumelden.

## <a name="im-not-getting-prompted-for-my-second-verification-information"></a>Ich werde nicht zur Eingabe meiner zweiten Überprüfungsinformation aufgefordert

Wenn Sie sich mit Ihrem Benutzernamen und Kennwort bei Ihrem Geschäfts-, Schul- oder Unikonto angemeldet haben, aber nicht zur Eingabe der zusätzlichen Informationen zur Sicherheitsüberprüfung aufgefordert wurden, haben Sie möglicherweise Ihr Gerät noch nicht eingerichtet. Ihr Mobilgerät muss speziell eingerichtet sein, um mit Ihrer zusätzlichen Sicherheitsüberprüfungsmethode zu funktionieren. Um sicherzustellen, dass Sie Ihr Mobilgerät eingeschaltet haben und es für die Verwendung mit Ihrer Überprüfungsmethode verfügbar ist, lesen Sie den Artikel [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md). Wenn Sie wissen, dass Sie Ihr Gerät oder Ihr Konto nicht eingerichtet haben, können Sie dies jetzt tun, indem Sie die Schritte im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md) ausführen.

## <a name="i-got-a-new-phone-number-how-do-i-change-it-for-two-factor-verification"></a>Ich habe eine neue Telefonnummer. Wie muss ich die Einstellungen für die zweistufige Überprüfung ändern?

Wenn Sie eine neue Telefonnummer haben, müssen Sie die Details Ihrer Sicherheitsüberprüfungsmethode aktualisieren, damit Überprüfungsaufforderungen an die richtige Stelle geleitet werden. Um die Überprüfungsmethode zu aktualisieren, führen Sie die Schritte im Abschnitt **Hinzufügen oder Ändern Ihrer Telefonnummer** des Artikels [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md#add-or-change-your-phone-number) aus.

## <a name="i-got-a-new-mobile-device-how-do-i-add-it"></a>Ich habe ein neues Mobilgerät. Wie füge ich es hinzu?

Wenn Sie ein neues Mobilgerät haben, müssen Sie es so einrichten, dass es mit der zweistufigen Überprüfung funktioniert. Dies ist ein mehrstufiger Prozess:

1. Richten Sie Ihr Gerät für Ihr Geschäfts-, Schul- oder Unikonto ein, indem Sie die Schritte im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md) ausführen.

2. Aktualisieren Sie Ihre Konto- und Geräteinformationen auf der Seite **Zusätzliche Sicherheitsüberprüfung**, indem Sie Ihr altes Gerät löschen und Ihr neues hinzufügen. Weitere Informationen finden Sie im Artikel [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md).

3. Optional. Sie können die Microsoft Authenticator-App auf Ihr Mobilgerät herunterladen, dort installieren und einrichten, indem Sie die Schritte im Artikel [Herunterladen und Installieren der Microsoft Authenticator-App](user-help-auth-app-download-install.md) ausführen.

4. Optional. Aktivieren Sie die zweistufige Überprüfung auf Ihren vertrauenswürdigen Geräten durch Ausführen der Schritte in **Erneutes Anfordern der zweistufigen Überprüfung auf einem als vertrauenswürdig markierten Gerät** des Artikels [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md#turn-on-two-factor-verification-prompts-on-a-trusted-device).

## <a name="im-having-problems-signing-in-on-my-mobile-device-while-traveling"></a>Ich habe auf Reisen Probleme bei der Anmeldung bei meinem mobilen Gerät

Möglicherweise ist es schwieriger, eine Überprüfungsmethode für mobile Geräte wie z. B. SMS zu nutzen, wenn Sie im Ausland sind. Es ist auch möglich, dass Ihr mobiles Gerät Roaminggebühren verursacht. Für diese Situation empfehlen wir Ihnen, die Microsoft Authenticator-App zu verwenden, die die Möglichkeit bietet, sich mit einem WLAN-Hotspot zu verbinden. Weitere Informationen zum Herunterladen, Installieren und Einrichten der Microsoft Authenticator-App auf Ihrem Mobilgerät finden Sie im Artikel [Herunterladen und Installieren der Microsoft Authenticator-App](user-help-auth-app-download-install.md).

## <a name="i-cant-get-my-app-passwords-to-work"></a>Meine App-Kennwörter funktionieren nicht

App-Kennwörter ersetzen normale Kennwörter für ältere Desktopanwendungen, die die zweistufige Überprüfung nicht unterstützen. Stellen Sie zunächst sicher, dass Sie das Kennwort korrekt eingegeben haben. Wenn das Problem dadurch nicht behoben wird, versuchen Sie, ein neues Kennwort für die App zu erstellen, indem Sie die Schritte im Abschnitt **Erstellen und Löschen von App-Kennwörtern im Portal „Meine Apps“** des Artikels [Verwalten von App-Kennwörtern für die zweistufige Überprüfung](multi-factor-authentication-end-user-app-passwords.md#create-and-delete-app-passwords-from-the-additional-security-verification-page) ausführen.

## <a name="why-cant-i-turn-two-factor-verification-off"></a>Warum kann ich die zweistufige Überprüfung nicht deaktivieren?

Wenn Sie die zweistufige Überprüfung mit Ihrem Geschäfts-, Schul- oder Unikonto (z. B. alain@contoso.com) verwenden, bedeutet dies höchstwahrscheinlich, dass Ihre Organisation entschieden hat, dass Sie dieses zusätzliche Sicherheitsfeature verwenden müssen. Da Ihre Organisation die Verwendung des Features verbindlich vorgeschrieben hat, kann es nicht individuell deaktiviert werden. Wenn Sie jedoch die zweistufige Überprüfung mit einem persönlichen Konto wie alain@outlook.comverwenden, können Sie das Feature aktivieren und deaktivieren. Anweisungen zum Steuern der zweistufigen Überprüfung für Ihre persönlichen Microsoft-Konten finden Sie unter [Aktivieren oder Deaktivieren der Überprüfung in zwei Schritten für Ihr Microsoft-Konto](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off).

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Ich konnte keine Lösung für mein Problem finden

Wenn Sie diese Schritte ausgeführt haben, aber weiterhin Probleme auftreten, wenden Sie sich an Ihren Helpdesk, um Hilfe zu erhalten.

## <a name="related-articles"></a>Verwandte Artikel

- [Verwalten der Einstellungen für die zweistufige Überprüfung](multi-factor-authentication-end-user-manage-settings.md)

- [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md)

- [Microsoft Authenticator-App – häufig gestellte Fragen](user-help-auth-app-faq.md)
