---
title: Beheben von Problemen bei der Azure-Anmeldung
description: Lösen eines Problems beim Registrieren eines neuen Kontos beim Microsoft Azure-Portal oder beim Azure-Kontocenter.
author: v-miegge
manager: na
editor: v-jesits
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/12/2019
ms.author: v-miegge
ms.openlocfilehash: 5032762c1275d3f1c8d1a020a54d20555f8ba3fe
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70242437"
---
# <a name="troubleshoot-azure-sign-up"></a>Beheben von Problemen bei der Azure-Anmeldung

Unter Umständen tritt ein Problem auf, wenn Sie sich beim Microsoft Azure-Portal oder beim Azure-Kontocenter für ein neues Konto registrieren. Überprüfen Sie Folgendes, bevor Sie mit der Problembehandlung beginnen:

- Die für Ihr Azure-Kontoprofil angegebenen Informationen (einschließlich Kontakt-E-Mail-Adresse, Anschrift und Telefonnummer) sind richtig.
- Die Kreditkarteninformationen sind richtig.
- Sie verfügen nicht bereits über ein Microsoft-Konto mit denselben Informationen.

## <a name="resolutions"></a>Lösungen

Zum Beheben des Fehlers wählen Sie das Problem aus, das auftritt, wenn Sie versuchen, sich bei Azure zu registrieren.

### <a name="error-we-cannot-proceed-with-sign-up-due-to-an-issue-with-your-account-please-contact-billing-support"></a>Fehler *Die Anmeldung kann aufgrund eines Problems mit Ihrem Konto nicht fortgesetzt werden. Wenden Sie sich an den Abrechnungssupport.*

Gehen Sie folgendermaßen vor, um das Problem zu beheben:

1. Melden Sie sich mit den Anmeldeinformationen des Kontoadministrators beim [Azure-Kontocenter](https://account.azure.com/Profile) an.

2. Wählen Sie **Details bearbeiten** aus.

3. Stellen Sie sicher, dass alle Adressfelder ausgefüllt und gültig sind.

4. Vergewissern Sie sich bei der Registrierung des Azure-Abonnements, dass die beim Registrieren der Kreditkarte eingegebene Rechnungsadresse mit den Angaben der Bank übereinstimmt.

Wenn die Fehlermeldung weiterhin angezeigt wird, verwenden Sie einen anderen Browser für die Registrierung.

### <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>Statusanzeige hängt im Abschnitt *Identitätsüberprüfung per Karte*.

Um die Identitätsprüfung mithilfe der Karte abzuschließen, müssen Cookies von Drittanbietern in Ihrem Browser zugelassen sein.

![Identitätsüberprüfung per Karte](./media/billing-troubleshoot-azure-sign-up/identify-verification-by-card.png)
 
Führen Sie die folgenden Schritte aus, um die Cookie-Einstellungen Ihres Browsers zu aktualisieren.

1. Wenn Sie Chrome verwenden, wählen Sie **Einstellungen** > **Erweiterte Einstellungen anzeigen** > **Datenschutz** > **Inhaltseinstellungen** aus. Deaktivieren Sie **Cookies und Websitedaten von Drittanbietern blockieren**.

2. Wenn Sie Microsoft Edge verwenden, wählen Sie **Einstellungen** > **erweiterte Einstellungen anzeigen** > **Cookies** > **Keine Cookies blockieren** aus.

3. Aktualisieren Sie die Azure Registrierungsseite, und überprüfen Sie dann, ob das Problem behoben ist.

4. Wenn das Problem durch die Aktualisierung nicht behoben wurde, beenden Sie den Browser, starten Sie ihn neu, und wiederholen Sie den Vorgang anschließend.

### <a name="credit-card-form-doesnt-support-my-billing-address"></a>Das Kreditkartenformular akzeptiert meine Rechnungsadresse nicht.

Ihre Rechnungsadresse muss sich in dem Land befinden, das Sie im Abschnitt **Informationen zu Ihrer Person** ausgewählt haben. Stellen Sie sicher, dass Sie das richtige Land auswählen.

### <a name="no-text-messages-or-calls-during-sign-up-account-verification"></a>Keine SMS oder Anrufe während der Kontoüberprüfung für die Registrierung

Der Prozess ist zwar in der Regel schnell, es kann aber bis zu vier Minuten dauern, bis ein Überprüfungscode übermittelt wird. Die zur Überprüfung eingegebene Telefonnummer wird nicht als Kontaktnummer für das Konto gespeichert.

Hier sind einige zusätzlichen Tipps:

- Für den Überprüfungsvorgang per Telefon kann keine VoIP-Telefonnummer (Voice-over-IP) verwendet werden.
- Überprüfen Sie im Dropdownmenü die eingegebene Telefonnummer einschließlich Landesvorwahl.
- Wenn Ihr Telefon keine Textnachrichten (SMS) empfängt, versuchen Sie es mit der Option **Mich anrufen**.
- Stellen Sie sicher, dass Ihr Telefon Anrufe oder SMS-Nachrichten von einer Telefonnummer aus den USA empfangen kann.

Wenn Sie die Textnachricht oder den Telefonanruf erhalten haben, geben Sie den empfangenen Code in das Textfeld ein.

### <a name="credit-card-declined-or-not-accepted"></a>Kreditkarte abgelehnt bzw. nicht akzeptiert

Virtuelle oder Prepaid-Kreditkarten oder -Debitkarten werden als Zahlungsmittel für Azure-Abonnements nicht akzeptiert. Weitere Informationen zu möglichen Ursachen für die Ablehnung Ihrer Karte finden Sie unter [Kreditkarte oder Debitkarte wird abgelehnt, wenn ich versuche, mich bei Azure zu registrieren](https://support.microsoft.com/help/4042960).

### <a name="free-trial-is-not-available"></a>Kostenlose Testversion ist nicht verfügbar

Haben Sie bereits einmal ein Azure-Abonnement verwendet? Laut Vereinbarung in den Azure-Nutzungsbedingungen ist die Aktivierung einer kostenlosen Testversion nur für Benutzer eingeschränkt, die neu bei Azure sind. Wenn Sie bereits einen anderen Typ von Azure-Abonnement verwendet haben, können Sie keine kostenlose Testversion aktivieren. Sie könnten sich auch für ein [Abonnement mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) registrieren.

### <a name="i-saw-a-charge-on-my-free-trial-account"></a>In meinem kostenlosen Testkonto werden Gebühren angezeigt.

Nach der Registrierung wird möglicherweise eine kleine Prüfsperre in Ihrem Kreditkartenkonto angezeigt. Diese Sperre wird innerhalb von 3 bis 5 Tagen entfernt. Wenn Sie Bedenken bezüglich der Verwaltung von Kosten haben, lesen Sie den Abschnitt [Vermeiden unerwarteter Kosten](billing-getting-started.md).

### <a name="cant-activate-azure-benefit-plan-like-msdn-bizspark-bizsparkplus-or-mpn"></a>Ich kann kein Azure-Vorteilsprogramm wie MSDN, BizSpark, BizSparkPlus oder MPN aktivieren

Überprüfen Sie, ob Sie die richtigen Anmeldeinformationen verwenden. Überprüfen Sie anschließend das Vorteilsprogramm, um sich zu vergewissern, dass Sie berechtigt sind.

- MSDN 
  - Überprüfen Sie auf der [MSDN-Kontoseite](https://msdn.microsoft.com/subscriptions/manage/default.aspx)Ihren Berechtigungsstatus.
  - Wenn Sie Ihren Status nicht überprüfen können, wenden Sie sich an das [Kundendienstcenter für MSDN-Abonnements](https://msdn.microsoft.com/library/aa493452.aspx).
- Microsoft für Startups
  - Melden Sie sich beim [Microsoft für Startups-Portal](https://startups.microsoft.com/#start-two) an, um Ihren Berechtigungsstatus für Microsoft für Startups zu überprüfen.
  - Wenn Sie Ihren Status nicht überprüfen können, erhalten Sie in den [Microsoft für Startups-Foren](https://www.microsoftpartnercommunity.com/t5/Microsoft-for-Startups/ct-p/Microsoft_Startups) Hilfe.
- MPN 
  - Melden Sie sich beim [MPN-Portal](https://mspartner.microsoft.com/Pages/Locale.aspx) an, um Ihren Berechtigungsstatus zu überprüfen. Ihnen stehen ggf. weitere Leistungen zu, wenn Sie über die entsprechenden [Kompetenzen für Cloudplattformen](https://mspartner.microsoft.com/pages/membership/cloud-platform-competency.aspx) verfügen.
  - Wenn Sie Ihren Status nicht überprüfen können, wenden Sie sich an den [MPN-Support](https://mspartner.microsoft.com/Pages/Support/Premium/contact-support.aspx).

### <a name="cant-activate-new-azure-in-open-subscription"></a>Ich kann kein neues Azure In Open-Abonnement aktivieren

Um ein Azure In Open-Abonnement zu erstellen, müssen Sie über einen gültigen OSA-Schlüssel (Online Service Activation) verfügen, dem mindestens ein Azure In Open-Token zugeordnet ist. Wenn Sie keinen OSA-Schlüssel haben, wenden Sie sich an einen der unter [Microsoft Pinpoint](https://pinpoint.microsoft.com/) aufgeführten Microsoft-Partner.

### <a name="error-you-are-not-eligible-for-an-azure-subscription"></a>Fehler *Sie besitzen keine Berechtigung für ein Azure-Abonnement*

Um dieses Problem zu beheben, überprüfen Sie, ob die folgenden Punkte zutreffen:

- Die für Ihr Azure-Kontoprofil angegebenen Informationen (einschließlich Kontakt-E-Mail-Adresse, Anschrift und Telefonnummer) sind richtig.
- Die Kreditkarteninformationen sind richtig.
- Sie verfügen nicht bereits über ein Microsoft-Konto, das dieselben Informationen verwendet.

### <a name="error-your-current-account-type-is-not-supported"></a>Fehler *Ihr aktueller Kontotyp wird nicht unterstützt*

Dieses Problem kann auftreten, wenn das Konto in einem [nicht verwalteten Azure AD-Verzeichnis](../active-directory/users-groups-roles/directory-self-service-signup.md) registriert ist und nicht im Azure AD Verzeichnis Ihrer Organisation vorhanden ist. 

Um dieses Problem zu beheben, registrieren Sie das Azure-Konto unter Verwendung eines anderen Kontos, oder übernehmen Sie das nicht verwaltete AD-Verzeichnis. Weitere Informationen finden Sie unter [Übernehmen eines nicht verwalteten Verzeichnisses als Administrator in Azure Active Directory](../active-directory/users-groups-roles/domains-admin-takeover.md).
 
## <a name="additional-help-resources"></a>Zusätzliche Hilferessourcen

Weitere Artikel für die Problembehandlung bei Azure-Abrechnung und -Abonnements

- [Karte abgelehnt](billing-troubleshoot-declined-card.md)
- [Probleme bei der Abonnementanmeldung](billing-troubleshoot-sign-in-issue.md)
- [Keine Abonnements gefunden](billing-no-subscriptions-found.md)
- [Enterprise-Kostenansicht deaktiviert](billing-enterprise-mgmt-grp-troubleshoot-cost-view.md)

## <a name="contact-us-for-help"></a>Setzen Sie sich mit uns in Verbindung, um Hilfe zu erhalten.

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Nächste Schritte

- [Dokumentation zur Azure-Abrechnung](index.md)
