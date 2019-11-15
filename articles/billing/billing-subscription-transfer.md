---
title: Übertragen des Abrechnungsbesitzes eines Azure-Abonnements an ein anderes Konto | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie der Abrechnungsbesitz eines Azure-Abonnements an ein anderes Konto übertragen wird. Zudem finden Sie hier einige häufig gestellte Fragen (FAQs) zu diesem Vorgang.
keywords: Azure-Abonnement übertragen, Azure Übertragung Abonnement, Verschieben Azure-Abonnement in anderes Konto, Azure Änderung Abonnementsbesitzer, Übertragen Azure-Abonnement an anderes Konto, Azure Abrechnungsbesitz übertragen
author: bandersmsft
manager: amberb
tags: billing,top-support-issue
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: beef37b0ece9cf57c86927b9f57d85ad4a25dbd1
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73746320"
---
# <a name="transfer-billing-ownership-of-an-azure-subscription-to-another-account"></a>Übertragen des Abrechnungsbesitzes eines Azure-Abonnements an ein anderes Konto

Wenn Sie Ihre Organisation verlassen oder Ihr Abonnement einem anderen Konto in Rechnung gestellt werden soll, möchten Sie möglicherweise den Abrechnungsbesitz Ihres Azure-Abonnements übertragen. Das Übertragen des Abrechnungsbesitzes an ein anderes Konto gibt den Administratoren des neuen Kontos die Berechtigung, Rechnungsaufgaben (z. B. Zahlungsmethode ändern, Gebühren anzeigen usw.) auszuführen und das Abonnement zu kündigen.

Wenn Sie den Abrechnungsbesitz behalten, aber den Typ des Abonnements ändern möchten, finden Sie Informationen hierzu unter [Umstellen Ihres Azure-Abonnements auf ein anderes Angebot](billing-how-to-switch-azure-offer.md). Wenn Sie steuern möchten, wer im Abonnement Ressourcen verwalten kann, finden Sie Informationen hierzu unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

Wenn Sie ein Kunde mit Enterprise Agreement (EA) sind, können Administratoren in Ihrem Unternehmen den Abrechnungsbesitz Ihrer Abonnements zwischen den Konten übertragen. Weitere Informationen finden Sie unter [Übertragen des Abrechnungsbesitzes von Abonnements mit Enterprise Agreement (EA)](#transfer-billing-ownership-of-enterprise-agreement-ea-subscriptions).

## <a name="transfer-billing-ownership-of-an-azure-subscription"></a>Übertragen des Abrechnungsbesitzes eines Azure-Abonnements

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com) als Administrator für das Abrechnungskonto des Abonnements an, das Sie übertragen möchten. Wie Sie feststellen, ob Sie ein Administrator sind, ist unter [Häufig gestellte Fragen](#faq) beschrieben.

1. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

   ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-subscription-transfer/billing-search-cost-management-billing.png)

1. Wählen Sie **Abonnements** im linken Bereich aus. Je nach Zugriff müssen Sie möglicherweise einen Abrechnungsbereich auswählen. Wählen Sie dann **Abonnements** oder **Azure-Abonnements** aus.

1. Wählen Sie für das Abonnement, das Sie übertragen möchten, **Abrechnungsbesitz übertragen** aus.

   ![Auswählen des zu übertragenden Abonnements](./media/billing-subscription-transfer/billing-select-subscription-to-transfer.png)

1. Geben Sie die E-Mail-Adresse eines Benutzer ein, der ein Abrechnungsadministrator für das Konto ist, das der neue Besitzer des Abonnements sein soll.

1. Wenn Sie Ihr Abonnement an ein Konto in einem anderen Azure AD-Mandanten übertragen, wählen Sie aus, ob das Abonnement in den Mandanten des neuen Kontos verschoben werden soll. Weitere Informationen finden Sie unter [Übertragen eines Abonnements an ein Konto in einem anderen Azure AD-Mandanten](#transferring-subscription-to-an-account-in-another-azure-ad-tenant).

    > [!IMPORTANT]
    >
    > Wenn Sie das Abonnement in den Azure AD-Mandanten des neuen Kontos verschieben, werden alle Zuweisungen der [rollenbasierten Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md) zum Verwalten von Ressourcen im Abonnement dauerhaft entfernt. Nur der Benutzer des neuen Kontos, der Ihre Übertragungsanforderung angenommen hat, hat Zugriff auf die Verwaltung von Ressourcen im Abonnement. Weitere Informationen finden Sie unter [Übertragen des Abonnements an einen Benutzer in einem anderen Azure AD-Mandanten](../active-directory/managed-identities-azure-resources/known-issues.md). Alternativ können Sie das Kontrollkästchen deaktivieren, mit dem der Azure AD-Mandant des Abonnements den Abrechnungsbesitz überträgt, ohne das Abonnement in den Mandanten des neuen Kontos zu verschieben. In diesem Fall werden vorhandene RBAC-Berechtigungen zum Verwalten von Azure-Ressourcen beibehalten.

    ![Seite zum Senden der Übertragungsanforderung](./media/billing-subscription-transfer/billing-send-transfer-request.PNG)

1. Wählen Sie **Übertragungsanforderung senden** aus.

1. Der Benutzer erhält eine E-Mail mit der Aufforderung, Ihre Übertragungsanforderung zu prüfen.

   ![An den Empfänger gesendete E-Mail zur Abonnementübertragung](./media/billing-subscription-transfer/billing-receiver-email.png)

1. Um die Übertragungsanforderung zu genehmigen, muss der Benutzer auf den Link in der E-Mail klicken und den Anweisungen folgen. Der Benutzer muss eine Zahlungsmethode auswählen, die für die Bezahlung des Abonnements verwendet wird. Falls der Benutzer kein Azure-Konto hat, muss er sich außerdem für ein neues Konto registrieren.

   ![Erste Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/billing-accept-ownership-step1.png)

   ![Zweite Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/billing-accept-ownership-step2.png)

   ![Zweite Webseite zur Abonnementübertragung](./media/billing-subscription-transfer/billing-accept-ownership-step3.png)

1. Erfolg! Das Abonnement ist jetzt übertragen.

## <a name="transferring-subscription-to-an-account-in-another-azure-ad-tenant"></a>Übertragen eines Abonnements an ein Konto in einem anderen Azure AD-Mandanten

Wenn Sie sich für Azure registrieren, wird ein Azure Active Directory (AD)-Mandant für Sie erstellt. Der Mandant stellt Ihr Konto dar. Sie verwenden den Mandanten zum Verwalten des Zugriffs auf Ihre Abonnements und Ressourcen.

Wenn Sie ein neues Abonnement erstellen, wird es von dem Azure AD-Mandanten Ihres Kontos gehostet. Wenn Sie anderen Benutzern Zugriff auf Ihr Abonnement oder dessen Ressourcen gewähren möchten, müssen Sie diese einladen, Ihrem Mandanten beizutreten. Dadurch können Sie den Zugriff auf Ihre Abonnements und Ressourcen besser steuern.

Wenn Sie den Abrechnungsbesitz Ihres Abonnements an ein Konto in einem anderen Azure AD-Mandanten übertragen, können Sie das Abonnement in den Mandanten des neuen Kontos verschieben. In diesem Fall verlieren alle Benutzer, Gruppen oder Dienstprinzipale, die über die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/role-assignments-portal.md) Zugriff zum Verwalten von Abonnements und deren Ressourcen hatten, ihren Zugriff. Nur der Benutzer des neuen Kontos, der Ihre Übertragungsanforderung angenommen hat, hat Zugriff auf die Verwaltung der Ressourcen. Wenn der neue Besitzer den Benutzern, die ursprünglich Zugriff hatten, erneut Zugriff gewähren möchte, müsste er [diese Benutzer manuell dem Abonnement hinzufügen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).


## <a name="transferring-visual-studio-mpn-and-pay-as-you-go-devtest-subscriptions"></a>Übertragen von Visual Studio-, MPN- und Dev/Test-Abonnements mit nutzungsbasierter Bezahlung

Mit Abonnements für Visual Studio und Microsoft Partner Network sind monatliche Azure-Gutschriften verbunden. Wenn Sie derartige Abonnements übertragen, ist Ihr Guthaben im Ziel-Abrechnungskonto nicht verfügbar. Für das Abonnement wird das Guthaben des Ziel-Abrechnungskontos verwendet. Beispiel: Bob überträgt am 9. September ein Abonnement für Visual Studio Enterprise an das Konto von Jane, die die Übertragung annimmt. Nach der Übertragung wird für das Abonnement die Gutschrift für Janes Konto verwendet. Die Gutschrift erfolgt jeweils am 9. des Monats.


<a id="EA"></a>

## <a name="transfer-billing-ownership-of-enterprise-agreement-ea-subscriptions"></a>Übertragen des Abrechnungsbesitzes von Abonnements mit Enterprise Agreement (EA)

Der Unternehmensadministrator kann den Besitz von Abonnements zwischen den Konten in einer Registrierung übertragen. Weitere Informationen finden Sie unter [Übertragen des Kontobesitzes](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) im EA-Portal.

## <a name="next-steps-after-accepting-billing-ownership"></a>Nächste Schritte nach dem Übernehmen des Abrechnungsbesitzes

Wenn Sie den Abrechnungsbesitz an einem Azure-Abonnement übernommen haben, empfehlen wir, die folgenden weiteren Schritte zu prüfen:

1. Überprüfen und aktualisieren Sie die Rollen für Dienstadministrator und Co-Administratoren und andere RBAC-Rollen. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Azure-Abonnementadministratoren](billing-add-change-azure-subscription-administrator.md) und [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](../role-based-access-control/role-assignments-portal.md).
1. Aktualisieren Sie die Anmeldeinformationen für die Dienste dieses Abonnements, darunter:
   1. Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](../cloud-services/cloud-services-certs-create.md).
   1. Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md).
   1. Anmeldeinformationen für den Remotezugriff für Dienste wie Azure Virtual Machines.
1. Wenn Sie mit einem Partner arbeiten, sollten Sie die Partner-ID in diesem Abonnement aktualisieren. Sie können die Partner-ID im [Azure-Portal](https://portal.azure.com) aktualisieren. Weitere Informationen finden Sie unter [Verknüpfen einer Partner-ID mit Ihren Azure-Konten](billing-partner-admin-link-started.md).

<a id="supported"></a>

## <a name="supported-subscription-types"></a>Unterstützte Abonnementtypen

Die Übertragung von Abonnements im Azure-Portal ist für die unten aufgeführten Abonnementtypen verfügbar. Derzeit wird das Übertragen von [kostenlosen Testversionen](https://azure.microsoft.com/offers/ms-azr-0044p/) oder [AIO-Abonnements (Azure in Open)](https://azure.microsoft.com/offers/ms-azr-0111p/) nicht unterstützt. Wie Sie dies umgehen können, erfahren Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md). Zum Übertragen anderer Abonnements wie [Sponsorship](https://azure.microsoft.com/offers/ms-azr-0036p/) oder Supportpläne [wenden Sie sich an den Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

- [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*
- [Microsoft Partner Network](https://azure.microsoft.com/offers/ms-azr-0025p/)  
- [Visual Studio Enterprise-Abonnenten (MPN)](https://azure.microsoft.com/offers/ms-azr-0029p/)
- [MSDN-Plattformen](https://azure.microsoft.com/offers/ms-azr-0062p/)  
- [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)
- [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/)
- [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)
- [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)
- [Microsoft Azure-Plan](https://azure.microsoft.com/offers/ms-azr-0017g/)\*\*

\* [Über das EA-Portal](#EA).

\*\* Wird nur für Konten unterstützt, die bei der Registrierung auf der Azure-Website erstellt werden.

<a id="faq"></a>

## <a name="frequently-asked-questions-faq-for-senders"></a>Häufig gestellte Fragen (FAQ) bei der Übertragung

Diese häufig gestellten Fragen richten sich an Benutzer, die den Abrechnungsbesitz eines Azure-Abonnements an ein anderes Konto übertragen möchten.

### <a name="whoisaa"></a> Wer ist ein Abrechnungsadministrator für ein Konto?

Ein Abrechnungsadministrator ist eine Person, die über Berechtigungen zum Verwalten der Abrechnung für ein Konto verfügt. Sie ist berechtigt, im [Azure-Portal](https://portal.azure.com) auf die Abrechnung zuzugreifen und verschiedene Abrechnungsaufgaben auszuführen (z.B. Abonnements erstellen, Rechnungen anzeigen und bezahlen oder Zahlungsmethoden aktualisieren).

Führen Sie die folgenden Schritte aus, um die Konten zu ermitteln, für die Sie als Abrechnungsadministrator fungieren:

1. Navigieren Sie im Azure-Portal zur Seite [Kostenverwaltung + Abrechnung](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/Overview).
1. Wählen Sie im linken Bereich die Option **Alle Abrechnungsbereiche** aus.
1. Auf der Seite „Abonnements“ werden alle Abonnements aufgelistet, für die Sie als Abrechnungsadministrator fungieren.

Verwenden Sie die folgenden Schritte für die Ermittlung, wenn Sie nicht sicher sind, wer der Kontoadministrator für ein Abonnement ist.

1. Navigieren Sie [im Azure-Portal zur Seite „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
1. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Wird alles übertragen? Auch Ressourcengruppen, virtuelle Computer, Datenträger und andere aktive Dienste?

Alle Ressourcen wie virtuelle Computer, Datenträger und Websites werden zum neuen Konto übertragen. Wenn Sie jedoch ein Abonnement an ein Konto in einem anderen Azure AD-Mandanten übertragen, werden alle [Administratorrollen](billing-add-change-azure-subscription-administrator.md) und alle Zuweisungen der [rollenbasierten Zugriffssteuerung (RBAC)](../role-based-access-control/role-assignments-portal.md) für das Abonnement [nicht übertragen](#transferring-subscription-to-an-account-in-another-azure-ad-tenant). Auch [App-Registrierungen](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) und andere mandantenspezifische Dienste werden nicht mit dem Abonnement übertragen.

### <a name="can-i-transfer-ownership-to-an-account-in-another-country"></a>Kann ich den Besitz an ein Konto in einem anderen Land übertragen?
Leider können länderübergreifende Übertragungen im Azure-Portal nicht ausgeführt werden. Zum Übertragen Ihres Abonnements in ein anderes Land [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="i-am-an-administrator-on-two-accounts-can-i-transfer-a-subscription-from-one-of-my-accounts-to-another"></a>Ich bin Administrator bei zwei Konten. Kann ich ein Abonnement von einem Konto auf das andere übertragen?
Ja. Sie können Abonnements zwischen Ihren Konten übertragen. Vom Konzept her werden Ihre Konten als Konten von zwei verschiedenen Benutzern betrachtet. Daher können Sie mit den oben genannten Schritten Abonnements zwischen Ihren Konten übertragen.

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Führt eine Übertragung des Abonnements zu Ausfallzeiten?

Wenn Sie ein Abonnement an ein Konto im gleichen Azure AD-Mandanten übertragen, hat dies keine Auswirkungen auf die Ressourcen, die im Abonnement ausgeführt werden. Wenn Sie jedoch das Abonnement an ein Konto in einem anderen Mandanten übertragen und das Abonnement in den Mandanten verschieben, verlieren alle Benutzer, Gruppen und Dienstprinzipale, die über die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md) Zugriff zum Verwalten von Ressourcen im Abonnement hatten, ihren Zugriff. Dies kann zu Ausfallzeiten führen.

### <a name="do-users-in-new-account-have-access-to-usage-and-billing-history"></a>Verfügen Benutzer des neuen Kontos über Zugriff auf den Nutzungs- und Abrechnungsverlauf?

Die einzigen Informationen, die für Benutzer des neuen Kontos verfügbar sind, sind die Kosten für das Abonnement im letzten Monat. Der übrige Nutzungs- und Abrechnungsverlauf wird nicht mit dem Abonnement übertragen.

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Wie migriere ich Daten und Dienste für mein Azure-Abonnement in ein neues Abonnement?

Wenn Sie ein Abonnement nicht übertragen können, können Sie Ihre Ressourcen manuell migrieren. Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../azure-resource-manager/resource-group-move-resources.md).

### <a name="if-i-transfer-a-visual-studio-or-microsoft-partner-network-subscription-does-my-credit-carry-forward-with-the-subscription-in-the-new-account"></a>Wird meine Gutschrift mit dem Abonnement zum neuen Konto übertragen, wenn ich ein Abonnement für Visual Studio oder Microsoft Partner Network übertrage?

Nein. Ihre Gutschrift ist im neuen Konto nicht verfügbar. Der Benutzer, der die Übertragungsanforderung annimmt, muss eine Visual Studio-Lizenz haben, um die Übertragungsanforderung annehmen zu können. Für das Abonnement wird die Visual Studio-Gutschrift verwendet, die für das Konto des Benutzers verfügbar ist. Weitere Informationen finden Sie unter [Übertragen von Visual Studio-, MPN- und Dev/Test-Abonnements mit nutzungsbasierter Bezahlung](#transferring-visual-studio-mpn-and-pay-as-you-go-devtest-subscriptions).


## <a name="frequently-asked-questions-faq-for-recipients"></a>Häufig gestellte Fragen (FAQ) bei der Übernahme

Diese häufig gestellten Fragen richten sich an Benutzer, die den Abrechnungsbesitz eines Azure-Abonnements von einem anderen Konto annehmen.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-account-do-users-in-that-account-continue-to-have-access-to-my-resources"></a>Wenn ich den Abrechnungsbesitz eines Abonnements von einem anderen Konto übernehme, haben dann die Benutzer dieses Konto weiterhin Zugriff auf meine Ressourcen?

Ja. Wenn sich Ihr Konto jedoch in einem Azure AD-Mandanten befindet, der sich vom Mandanten des Abonnements unterscheidet, und der Benutzer, der die Übertragungsanforderung gesendet hat, das Abonnement in den Mandanten Ihres Kontos verschiebt, werden alle Zuweisungen von [Administratorrollen](billing-add-change-azure-subscription-administrator.md) und [rollenbasierter Zugriffssteuerung (RBAC)](../role-based-access-control/role-assignments-portal.md) entfernt. Um die Benutzer anzuzeigen, die über die [rollenbasierte Zugriffssteuerung (RBAC)](../role-based-access-control/overview.md) Zugriff zum Verwalten von Ressourcen im Abonnement haben, führen Sie die folgenden Schritte aus:

1. Navigieren Sie [im Azure-Portal zur Seite „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Wählen Sie das Abonnement aus, das Sie überprüfen möchten, und wählen Sie dann **Zugriffssteuerung (IAM)** im linken Bereich aus.
1. Wählen Sie oben auf der Seite **Rollenzuweisungen** aus. Auf der Seite „Rollenzuweisungen“ werden alle Benutzer mit RBAC-Zugriff auf das Abonnement aufgelistet.

Auch wenn die Zuweisungen der [rollenbasierten Zugriffssteuerung (RBAC)](../role-based-access-control/role-assignments-portal.md) während der Übertragung entfernt werden, haben die Benutzer des ursprünglichen Besitzerkontos möglicherweise über einige Sicherheitsmechanismen weiterhin Zugriff auf das Abonnement. Dazu gehören Folgende:

* Verwaltungszertifikate, die dem Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen eines Verwaltungszertifikats für Azure](../cloud-services/cloud-services-certs-create.md).
* Tastenkombinationen für Dienste wie Storage. Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md).
* Anmeldeinformationen für den Remotezugriff für Dienste wie Azure Virtual Machines.

Der neue Besitzer sollte sich überlegen, ob er die dem Dienst zugeordneten Geheimnisse aktualisiert, wenn er den Zugriff auf die Ressourcen einschränken muss. Die meisten Ressourcen können durch Ausführen der folgenden Schritte aktualisiert werden:

  1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
  2. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus.
  3. Wählen Sie die Ressource.
  4. Klicken Sie auf der Seite „Ressourcen“ auf **Einstellungen**. Hier können Sie die vorhandenen Schlüssel anzeigen und aktualisieren.

### <a name="if-i-take-over-the-billing-ownership-of-a-subscription-in-the-middle-of-the-billing-cycle-do-i-have-to-pay-for-the-entire-billing-cycle"></a>Muss ich den gesamten Abrechnungszyklus bezahlen, wenn ich ein Abonnement in der Mitte des Abrechnungszyklus übernehme?

Ihr Konto ist verantwortlich für die Bezahlung jeglicher Nutzung, die ab dem Zeitpunkt der Übertragung gemeldet wird. Möglicherweise gibt es Nutzung, die vor der Übertragung stattgefunden hat, aber danach gemeldet wurde. Diese Nutzung ist in der Abrechnung für Ihr Konto enthalten.

### <a name="can-i-use-a-different-payment-method"></a>Kann ich eine andere Zahlungsmethode verwenden?

Ja. Bei der Annahme der Übertragungsanforderung können Sie eine vorhandene Zahlungsmethode auswählen, die mit Ihrem Konto verknüpft ist, oder eine neue Zahlungsmethode hinzufügen.

### <a name="how-can-i-transfer-ownership-of-my-enterprise-agreement-ea-subscription-account-ownership-if-the-original-account-owner-is-no-longer-with-the-organization"></a>Wie kann ich den Kontobesitz meines EA-Abonnements (Enterprise Agreement) übertragen, wenn der ursprüngliche Kontobesitzer nicht mehr der Organisation angehört?

Der Unternehmensadministrator kann den Kontobesitz für beliebige Konten aktualisieren, auch wenn der ursprüngliche Kontobesitzer nicht mehr der Organisation angehört. Eine entsprechende Anleitung steht im EA-Portal unter [Übertragen des Kontobesitzes für alle Abonnements](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) zur Verfügung.

## <a name="troubleshooting"></a>Problembehandlung

### <a id="no-button"></a> Warum wird die Schaltfläche „Abonnement übertragen“ nicht angezeigt?

Die Self-Service-Abonnementübertragung ist für Ihr Abrechnungskonto nicht verfügbar. Derzeit wird die Übertragung des Abrechnungsbesitzes von Abonnements in Enterprise Agreement (EA)-Konten im Azure-Portal nicht unterstützt. Darüber hinaus wird die Übertragung des Abrechnungsbesitzes für Konten für die Microsoft-Kundenvereinbarung, die in Zusammenarbeit mit einem Microsoft-Vertreter erstellt werden, nicht unterstützt.

### <a id="no-button"></a> Warum unterstützt mein Abonnementtyp die Übertragung nicht?

Nicht alle Abonnementtypen unterstützen die Übertragung des Abrechnungsbesitzes. Unter [Unterstützte Abonnementtypen](#supported-subscription-types) sind die Abonnementtypen aufgelistet, die eine Übertragung unterstützen.

### <a id="no-button"></a> Warum erhalte ich den Fehler „Zugriff verweigert“, wenn ich versuche, den Abrechnungsbesitz eines Abonnements zu übertragen?

Dieser Fehler wird angezeigt, wenn Sie versuchen, ein Abonnement für einen Microsoft Azure-Plan zu übertragen und nicht über die erforderliche Berechtigung verfügen. Um das Abonnement für einen Microsoft Azure-Plan zu übertragen, müssen Sie im Rechnungsabschnitt entweder Besitzer oder Mitwirkender sein, dem das Abonnement in Rechnung gestellt wird. Weitere Informationen finden Sie unter [Verwalten von Abonnements für den Rechnungsabschnitt](billing-understand-mca-roles.md#manage-subscriptions-for-invoice-section).


## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- Überprüfen und aktualisieren Sie die Rollen für Dienstadministrator und Co-Administratoren und andere RBAC-Rollen. Weitere Informationen finden Sie unter [Hinzufügen oder Ändern von Azure-Abonnementadministratoren](billing-add-change-azure-subscription-administrator.md) und [Verwalten des Zugriffs mithilfe der RBAC und des Azure-Portals](../role-based-access-control/role-assignments-portal.md).
