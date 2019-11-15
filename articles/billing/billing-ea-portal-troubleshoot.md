---
title: Problembehandlung für den Zugriff auf das Azure EA-Portal
description: In diesem Artikel werden einige häufige Probleme beschrieben, die im Zusammenhang mit einem Azure Enterprise Agreement (EA) im Azure EA-Portal auftreten können.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/04/2019
ms.topic: troubleshooting
ms.service: billing
manager: boalcsva
ms.openlocfilehash: 1b0be54eea063399cc9ed2c3005808d5189e2663
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73888340"
---
# <a name="troubleshoot-azure-ea-portal-access"></a>Problembehandlung für den Zugriff auf das Azure EA-Portal

In diesem Artikel werden einige häufige Probleme beschrieben, die im Zusammenhang mit einem Azure Enterprise Agreement (EA) auftreten können. Das Azure EA-Portal dient zum Verwalten von Enterprise Agreement-Benutzern und -Kosten. Die folgenden Probleme können auftreten, wenn Sie den Zugriff auf das Azure EA-Portal konfigurieren oder aktualisieren.

## <a name="issues-adding-an-admin-to-an-enrollment"></a>Probleme beim Hinzufügen eines Administrators zu einer Registrierung

Für Enterprise-Registrierungen stehen verschiedene Arten von Authentifizierungsebenen zur Verfügung. Bei nicht ordnungsgemäßer Anwendung von Authentifizierungsebenen treten möglicherweise Probleme auf, wenn Sie versuchen, sich beim Azure EA-Portal anzumelden.

Das Azure EA-Portal wird verwendet, um Benutzern mit verschiedenen Authentifizierungsebenen Zugriff zu gewähren. Ein Unternehmensadministrator kann die Authentifizierungsebene aktualisieren, um die Sicherheitsanforderungen seiner Organisation zu erfüllen.

### <a name="authentication-level-types"></a>Arten von Authentifizierungsebenen

- Nur Microsoft-Konto: Für Organisationen, die Benutzer über Microsoft-Konten verwenden, erstellen und verwalten möchten.
- Geschäfts-, Schul- oder Unikonto: Für Organisationen, die Active Directory mit Cloudverbund eingerichtet haben und bei denen sich alle Konten in einem einzelnen Mandanten befinden.
- Geschäfts- oder Schulkonto – Mandanten-übergreifend: Für Organisationen, die Active Directory mit Cloudverbund eingerichtet haben und über Konten in mehreren Mandanten verfügen.
- Mischkonto: Diese Option ermöglicht das Hinzufügen von Benutzern mit einem Microsoft-Konto und/oder einem Geschäfts-, Schul- oder Unikonto.

Das erste Geschäfts-, Schul- oder Unikonto, das der Registrierung hinzugefügt wird, bestimmt die _Standarddomäne_ (oder _Masterdomäne_). Wenn Sie ein Geschäfts-, Schul- oder Unikonto mit einem anderen Mandanten hinzufügen möchten, müssen Sie die Authentifizierungsebene unter der Registrierung in die mandantenübergreifende Authentifizierung ändern.

So aktualisieren Sie die Authentifizierungsebene:

1. Melden Sie sich beim Azure EA-Portal als Unternehmensadministrator an.
2. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
3. Klicken Sie auf die Registerkarte **Registrierung**.
4. Wählen Sie unter **Registrierungsdetails** die Option **Authentifizierungsebene** aus.
5. Klicken Sie auf das Stiftsymbol.
6. Klicken Sie auf **Speichern**.

![Beispiel mit Authentifizierungsebenen ](./media/billing-ea-portal-troubleshoot/create-ea-authentication-level-types.png)

Für Microsoft-Konten muss auf [https://signup.live.com](https://signup.live.com/) eine zugehörige ID erstellt werden.

Geschäfts-, Schul- oder Unikonten stehen für Organisationen zur Verfügung, die Active Directory mit Verbund eingerichtet haben und bei denen sich alle Konten in einem einzelnen Mandanten befinden. Benutzer können mit der Verbundbenutzerauthentifizierung für Geschäfts-, Schul- oder Unikonten hinzugefügt werden, wenn das interne Active Directory des Unternehmens einem Verbund angehört.

Falls Ihre Organisation keinen Active Directory-Verbund verwendet, können Sie Ihre Geschäfts-, Schul- oder Uni-E-Mail-Adresse nicht verwenden. Registrieren oder erstellen Sie stattdessen eine neue E-Mail-Adresse, und registrieren Sie sie als Microsoft-Konto.

## <a name="unable-to-access-the-azure-ea-portal"></a>Zugriff auf das Azure EA-Portal nicht möglich

Sollte bei der Anmeldung beim Azure EA-Portal eine Fehlermeldung angezeigt werden, führen Sie die folgenden Problembehandlungsschritte aus:

- Vergewissern Sie sich, dass Sie die korrekte Azure EA-Portal-URL verwenden: https://ea.azure.com.
- Ermitteln Sie, ob Ihr Zugriff auf das Azure EA-Portal als Geschäfts-, Schul- oder Unikonto oder als Microsoft-Konto hinzugefügt wurde.
  - Geben Sie bei Verwendung eines Geschäftskontos Ihre geschäftliche E-Mail-Adresse und Ihr geschäftliches Kennwort ein. Ihr geschäftliches Kennwort erhalten Sie von Ihrer Organisation. Bei Problemen mit dem Kennwort können Sie sich zwecks Kennwortzurücksetzung an Ihre IT-Abteilung wenden.
  - Geben Sie bei Verwendung eines Microsoft-Kontos die E-Mail-Adresse und das Kennwort für Ihr Microsoft-Konto ein. Sollten Sie das Kennwort Ihres Microsoft-Kontos vergessen haben, können Sie es unter [https://account.live.com/password/reset](https://account.live.com/password/reset) zurücksetzen.
- Verwenden Sie für die Anmeldung eine InPrivate- oder Inkognito-Browsersitzung, damit keine Cookies oder zwischengespeicherten Informationen aus vorherigen oder vorhandenen Sitzungen gespeichert werden. Löschen Sie den Browsercache, und öffnen Sie https://ea.azure.com in einem InPrivate- oder Inkognito-Fenster.
- Sollte bei Verwendung eines Microsoft-Kontos ein Fehler vom Typ _Ungültiger Benutzer_ auftreten, besitzen Sie möglicherweise mehrere Microsoft-Konten. Bei dem für die Anmeldung verwendeten Konto handelt es sich nicht um die primäre E-Mail-Adresse.
Ein Fehler vom Typ _Ungültiger Benutzer_ kann aber auch auftreten, wenn beim Hinzufügen des Benutzers zur Registrierung der falsche Kontotyp verwendet wurde (beispielsweise ein Geschäfts-, Schul- oder Unikonto anstelle eines Microsoft-Kontos). In diesem Fall muss ein anderer EA-Administrator das richtige Konto hinzufügen, oder Sie müssen sich an den [Support](https://support.microsoft.com/supportforbusiness/productselection?sapId=cf791efa-485b-95a3-6fad-3daf9cd4027c) wenden.
  - Den primären Alias können Sie unter [https://account.live.com](https://account.live.com) überprüfen. Klicken Sie dann auf **Ihre Informationen** und **Anmeldung bei Microsoft verwalten**. Befolgen Sie die Anweisungen, um eine alternative E-Mail-Adresse zu verifizieren und einen Code für den Zugriff auf vertrauliche Informationen zu erhalten. Geben Sie den Sicherheitscode ein. Falls Sie keine zweistufige Authentifizierung einrichten möchten, wählen Sie **Später einrichten** aus.
  - Die Seite **Anmeldung bei Microsoft verwalten** wird geöffnet, auf der Sie Ihre Kontoaliase anzeigen können. Vergewissern Sie sich, dass der primäre Alias der Alias ist, den Sie für die Anmeldung beim Azure EA-Portal verwenden. Falls dies nicht der Fall ist, können Sie ihn als primären Alias festlegen. Alternativ können Sie den primären Alias für das Azure EA-Portal verwenden.

## <a name="no-activation-email-received"></a>Keine Aktivierungs-E-Mail erhalten

Vom Azure EA-Portal wird eine E-Mail mit dem Absender *waep@microsoft.com* gesendet. Sollten Sie keine Aktivierungs-E-Mail erhalten haben, durchsuchen Sie Ihren Spam- oder Junk-Ordner nach der E-Mail. Der Betreff lautet _Invitation to View/Manage the Microsoft Azure service_ (Einladung zum Anzeigen/Verwalten des Microsoft Azure-Diensts). Sie wird an jeden neu hinzugefügten EA-Administrator gesendet.

Sollten Sie wissen, dass Sie als EA-Administrator eingerichtet wurden, müssen Sie nicht auf die Aktivierungs-E-Mail warten, um sich beim Azure EA-Portal anzumelden. Stattdessen können Sie sich unter https://ea.azure.com mit Ihrer E-Mail-Adresse (Microsoft-Konto oder Geschäfts-, Schul- oder Unikonto) und Ihrem Kennwort anmelden.

## <a name="azure-ea-activation-faq"></a>Häufig gestellte Fragen zur Aktivierung von Azure EA

In diesem Abschnitt des Artikels finden Sie Lösungen zu einigen häufig auftretenden Probleme bei der Aktivierung von Azure EA.

### <a name="i-would-like-to-add-a-new-ea-administrator-to-my-enrollment"></a>Ich möchte einen neuen EA-Administrator zu meiner Registrierung hinzufügen.

Ein neuer Unternehmensadministrator kann von vorhandenen Unternehmensadministratoren hinzugefügt werden. Wenn Sie EA-Administrator sind, melden Sie sich beim EA-Portal an. Klicken Sie auf **Verwalten** und dann in der oberen rechten Ecke auf **+ Administrator hinzufügen**, um einen neuen EA-Administrator hinzuzufügen. Stellen Sie sicher, dass Sie die E-Mail-Adresse und bevorzugte Anmeldemethode des Benutzers kennen, z. B. eine Authentifizierung über ein Geschäfts-, Schul- und Unikonto oder eine Microsoft-Live-ID, damit Sie den Benutzer hinzufügen können.

Wenn Sie kein EA-Administrator sind, wenden Sie sich an die EA-Administratoren in Ihrem Unternehmen, um diese zu bitten, Sie zur Registrierung hinzuzufügen. Nachdem Sie zur Registrierung hinzugefügt wurden, erhalten Sie eine Aktivierungs-E-Mail.

Wenn die EA-Administratoren nicht in der Lage sind, Ihnen zu helfen, können wir Sie in deren Auftrag hinzuzufügen, wenn Sie uns folgende Informationen zur Verfügung stellen:
- Registrierungsnummer
- Hinzuzufügende E-Mail-Adresse und Authentifizierungstyp (Geschäfts-, Schul- oder Unikonto oder Microsoft-Konto)
- E-Mail-Genehmigung eines EA-Administrators

Sobald Sie alle erforderlichen Informationen erhalten haben, übermitteln Sie unter [https://aka.ms/AzureEntSupport](https://aka.ms/AzureEntSupport) eine Anfrage.

### <a name="i-would-like-to-update-the-first-ea-admin-on-the-enrollment"></a>Ich möchte den ersten EA-Administrator in der Registrierung aktualisieren.

Der erste EA-Administrator kann im Volume Licensing Service Center aktualisiert werden, indem die Kontaktinformationen und der Onlineadministrator für das Portal aktualisiert werden. Es dauert ca. 24 Stunden, bis das EA-Portal aktualisiert ist. Sobald die Aktualisierung abgeschlossen ist, erhält der neue EA-Administrator eine Aktivierungs-E-Mail.

Wenn Sie nicht auf das VLSC-Portal zugreifen können oder der ursprüngliche EA-Administrator die Registrierung nicht mehr verwalten und nicht mehr auf das EA-Portal zugreifen kann, übermitteln Sie unter [https://aka.ms/AzureEntSupport](https://aka.ms/AzureEntSupport) eine Anfrage für eine Aktualisierung, und geben Sie folgenden Informationen an:
- Registrierungsnummer
- Hinzuzufügende E-Mail-Adresse und Authentifizierungstyp (Geschäfts-, Schul- oder Unikonto oder Microsoft-Konto)
- Grund für das Ändern des ursprünglichen EA-Administrators
- E-Mail-Genehmigung des ursprünglichen EA-Administrators

### <a name="my-current-ea-admin-is-no-longer-with-the-company"></a>Mein aktueller EA-Administrator arbeitet nicht mehr beim Unternehmen.

In einer EA-Registrierung können mehrere EA-Administratoren vorhanden sein. Sie können sich an einen anderen dieser Administratoren werden, um einen neuen EA-Administrator, neue Kontobesitzer oder einen neuen Abteilungsadministrator hinzufügen zu lassen. Wenn Sie jedoch nicht sicher sind, wer der EA-Administrator im Unternehmen ist, oder wenn in der Registrierung kein anderer EA-Administrator verfügbar ist, wenden Sie sich an uns, und geben Sie folgende Informationen an:
- Registrierungsnummer
- Hinzuzufügende E-Mail-Adresse und Authentifizierungstyp (Geschäfts-, Schul- oder Unikonto oder Microsoft-Konto)
- Information, dass der aktuelle EA-Administrator nicht mehr beim Unternehmen arbeitet

Bitte beachten Sie Folgendes: Wenn in der Registrierung andere EA-Administratoren vorhanden sind, wenden wir uns an diese, um eine Genehmigung für administrative Änderungen der Registrierung anzufordern.

### <a name="my-enrollment-is-showing-in-pending-status-how-do-i-activate-my-enrollment"></a>Meine Registrierung wird im Status „Ausstehend“ angezeigt. Wie aktiviere ich meine Registrierung?

Registrierungen weisen den Status „Ausstehend“ auf, wenn der ursprüngliche EA-Administrator sich noch nicht bei der Registrierung angemeldet hat. Wenn Sie dieser EA-Administrator sind, melden Sie sich beim Azure-EA-Portal an. Auf der Landing Page mit allen Registrierungsnummern wird die ausstehende Registrierung möglicherweise nicht angezeigt. Deaktivieren Sie das Kontrollkästchen „Aktiv“ in der oberen rechten Ecke Ihres EA-Portals. Damit wird die ausstehende Registrierung angezeigt. Klicken Sie auf die Registrierung, um auf die Informationen zuzugreifen. Sobald Sie zur Seite „Verwalten“ der Registrierung gelangen, wird der Status von „Ausstehend“ zu „Aktiv“ geändert.

### <a name="why-is-my-account-stuck-in-pending-status"></a>Warum verbleibt mein Konto im Status „Ausstehend“?

Wenn der Registrierung neue Kontobesitzer (Account Owners, AO) hinzugefügt werden, wird unter ihrem Status immer „Ausstehend“ angezeigt. Nach Erhalt der Begrüßungs-E-Mail für die Aktivierung kann der Kontobesitzer sich anmelden, um sein Konto zu aktivieren. Durch die Anmeldung wird der Kontostatus von „Ausstehend“ zu „Aktiv“ geändert.

### <a name="i-received-an-error-when-signing-in-to-azure-ea-portal"></a>Beim Anmelden beim Azure-EA-Portal erhalte ich eine Fehlermeldung.

Es gibt verschiedene mögliche Gründe dafür, dass bei der Anmeldung beim Azure-EA-Portal eine Fehlermeldung angezeigt wird. Führen Sie die folgenden Schritte zur Problembehandlung aus:

 1. Stellen Sie sicher, dass Sie die richtige EA-Portal-URL verwenden: [https://ea.azure.com](https://ea.azure.com).
 1. Ermitteln Sie, ob Ihr Zugang zum Azure-EA Portal als Geschäfts-, Schul- oder Unikonto oder als Microsoft-Live-ID hinzugefügt wurde. Geben Sie bei Verwendung eines Geschäftskontos Ihre geschäftliche E-Mail-Adresse und das zugehörige Kennwort ein. Bei Verwendung einer Microsoft-Live-ID geben Sie die E-Mail-Adresse und das zugehörige Kennwort Ihres Microsoft-Live-ID-Kontos ein. Wenn Sie Ihr Microsoft-Live-ID-Kennwort vergessen haben, lassen Sie es unter [https://account.live.com/password/reset](https://account.live.com/password/reset) zurücksetzen.
 1. Bei der Anmeldung wird die Verwendung eines sicheren Browsers empfohlen, sodass keine Cookies oder Cachedaten aus vorherigen oder vorhandenen Sitzungen beibehalten werden. Löschen Sie den Cache, und verwenden Sie ein Fenster im privaten Browsermodus oder Inkognitomodus, um [https://ea.azure.com](https://ea.azure.com) zu öffnen.
 1. Wenn Sie bei Verwendung eines Microsoft-Kontos die Fehlermeldung „Ungültiger Benutzer“ erhalten, kann dies daran liegen, dass Sie über mehrere Microsoft-Konten verfügen und das Konto, mit dem Sie sich anzumelden versuchen, nicht der primäre Alias ist. Um den primären Alias zu überprüfen, wechseln Sie zu account.live.com:
    - Navigieren Sie zu „Ihre Informationen“ > „Anmelde-E-Mail oder Telefonnummer verwalten“.
    - Befolgen Sie die Anweisungen auf dem Bildschirm, um eine alternative E-Mail-Adresse zu verifizieren und einen Code für den Zugriff auf vertrauliche Informationen zu erhalten.
    - Geben Sie den Sicherheitscode ein.
    - Wenn Sie die zweistufige Authentifizierung zu einem späteren Zeitpunkt einrichten möchten, klicken Sie auf „Später einrichten“.
    - Sie gelangen zur Seite „Verwalten der Kontoaliase“, auf der die Kontoaliase angezeigt werden, über die Sie verfügen. Vergewissern Sie sich, dass der primäre Alias der Alias ist, den Sie für die Anmeldung beim Azure-EA-Portal verwenden. Ist dies nicht der Fall, können Sie entweder diesen Alias als primären Alias festlegen oder stattdessen den primären Alias für das Azure-EA-Portal verwenden.

Wenn die oben beschriebenen Schritte das Problem nicht lösen, übermitteln Sie unter [https://aka.ms/AzureEntSupport](https://aka.ms/AzureEntSupport) eine Anfrage mit folgenden Informationen:
- Verwendeter Browser einschließlich Version.
- Screenshot der Fehlermeldung.
- URL der Seite, auf der der Fehler angezeigt wird.  
- Datum, Uhrzeit und Zeitzone des Fehlers.
- Darüber hinaus ist auch die Bereitstellung einer Protokolldatei hilfreich. Mit folgenden Schritten erfassen Sie eine Netzwerkablaufverfolgung:
  1. Öffnen Sie Internet Explorer.
  1. Drücken Sie die Taste F12. Dadurch wird am unteren Rand des Internet Explorers ein Feld geöffnet.
  1. Wählen Sie die Registerkarte **Netzwerk** aus.
  1. Klicken Sie auf **Aufzeichnung starten**.
  1. Führen Sie die Aktion aus, die den Fehler verursacht.
  1. Sobald der Fehler auftritt, klicken Sie auf **Aufzeichnung beenden**.
  1. Speichern Sie die Datei, und fügen Sie die Informationen in die Supportanfrage ein.
  1. Stellen Sie sicher, dass Sie in der Supportanfrage Ihre Registrierungsnummer und E-Mail-Adresse angeben.

### <a name="what-is-the-difference-between-a-workschool-account-and-microsoft-account"></a>Worin besteht der Unterschied zwischen einem Geschäfts-, Schul- oder Unikonto und einem Microsoft-Konto?

**Microsoft-Konto**: Konten, die unter [https://signup.live.com](https://signup.live.com) mit einer Live-ID verknüpft sind.

**Geschäfts-, Schul- oder Unikonto**: Nur für Unternehmen verfügbar, die Active Directory mit Cloudverbund eingerichtet haben und bei denen sich alle Konten in einem einzelnen Mandanten befinden. Benutzer können mit dem Authentifizierungstyp „Geschäfts-, Schul- oder Unikonto“ hinzugefügt werden, wenn das interne Active Directory des Unternehmens einem Verbund angehört.

  Seit September 2016 gestattet Microsoft keine Registrierung von Geschäfts-, Schul- oder Uni-E-Mail-Adressen als Microsoft-Konten mehr. Weitere Details finden Sie hier: [https://blogs.technet.microsoft.com/enterprisemobility/2016/09/15/cleaning-up-the-azure-ad-and-microsoft-account-overlap/](https://blogs.technet.microsoft.com/enterprisemobility/2016/09/15/cleaning-up-the-azure-ad-and-microsoft-account-overlap/).

  Wenn in Ihrer Organisation kein Cloudverbund existiert, können Sie Ihre Geschäfts-, Schul- oder Uni-E-Mail-Adresse nicht verwenden. Registrieren oder erstellen Sie stattdessen eine neue E-Mail-Adresse, und registrieren Sie diese als Microsoft-Konto.

### <a name="i-forgot-my-password-to-azure-ea-portal"></a>Ich habe mein Kennwort für das Azure-EA-Portal vergessen.

Wenn Sie Ihr Microsoft-Live-ID-Kennwort vergessen haben, lassen Sie es unter [https://account.live.com/password/reset](https://account.live.com/password/reset) zurücksetzen.

Wenn Sie das Kennwort für Ihr Geschäftskonto vergessen haben, wenden Sie sich an den IT-Administrator Ihres Unternehmens.

### <a name="i-have-a-valid-work-or-school-account-but-i-cant-add-it-to-the-ea-portal"></a>Ich verfüge über ein gültiges Geschäfts-, Schul- oder Unikonto, kann es aber nicht zum EA-Portal hinzufügen.

Wenn Sie über ein Geschäfts-, Schul- oder Unikonto in einem anderen Mandanten verfügen, ändern Sie die Autorisierungsebene auf der Detailseite der Registrierung in „Geschäfts- oder Schulkonto – Mandanten-übergreifend“. Danach können Sie das Konto hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

- Azure EA-Portaladministratoren finden unter [Azure EA-Portalverwaltung](billing-ea-portal-administration.md) Informationen zu allgemeinen Verwaltungsaufgaben.
