---
title: Konfigurieren von Kundenleads | Azure Marketplace
description: Konfigurieren von Kundenleads im kommerziellen Marketplace.
services: Azure, Marketplace, commercial marketplace, Partner Center
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: evansma
ms.openlocfilehash: 7ead8dee12d4376e6e1058b84a25b91c021a937c
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73812655"
---
# <a name="customer-leads-from-your-marketplace-offer"></a>Kundenleads aus Ihrem Marketplace-Angebot

Leads sind Kunden, die an Ihren Angeboten im [Azure Marketplace](https://azuremarketplace.microsoft.com) oder in [AppSource](https://appsource.microsoft.com) interessiert sind oder diese Angebote von dort bereitstellen. Sie erhalten Kundenleads, nachdem Ihr Angebot im Marketplace veröffentlicht wurde. In diesem Artikel wird Folgendes erläutert:

* Generieren von Kundenleads aus Ihrem Marketplace-Angebot, um sicherzustellen, dass Sie keine Geschäftschancen verpassen 
* Verbinden Ihres CRM-Systems mit Ihrem Angebot zum Verwalten Ihrer Leads an einem zentralen Ort
* Auswerten der von uns gesendeten Leaddaten, damit Sie Kunden nachverfolgen können, die sich an Sie wenden

## <a name="generate-customer-leads"></a>Generieren von Kundenleads

Stellen, an denen Leads generiert werden:

1. Ein Kunde erklärt sich durch Auswählen von „Kontakt mit mir aufnehmen“ im Marketplace mit der Weitergabe seiner Informationen einverstanden. Bei diesem Lead handelt es sich um einen Lead mit **erstem Interesse**, bei dem wir Informationen zu dem Kunden weitergeben, der Interesse am Erhalt Ihres Produkts bekundet hat. Der Lead befindet sich an oberster Stelle des Verkaufstrichters.

      ![Dynamics 365 – Kontakt mit mir aufnehmen](./media/commercial-marketplace-get-customer-leads/dynamics-365-contact-me.png)

2. Wenn ein Kunde „Jetzt anfordern“ oder „Erstellen“ (im [Azure-Portal](https://portal.azure.com/)) auswählt, um Ihr Angebot zu erhalten, handelt es sich um einen **aktiven Lead**. In diesem Fall geben wir Informationen zu dem Kunden, der mit der Bereitstellung Ihres Produkts begonnen hat, weiter.

    ![SQL – Jetzt holen](./media/commercial-marketplace-get-customer-leads/sql-get-it-now.png)

    ![Windows Server – Erstellen](./media/commercial-marketplace-get-customer-leads/windows-server-create.png)

3. Ein Kunde erhält eine Testversion oder startet eine kostenlose Testversion Ihres Angebots. Testversionen oder kostenlose Testversionen optimieren Ihre Chancen, Ihrem Unternehmen umgehend und ohne Zugangsbeschränkungen potenzielle Kunden zu vermitteln.

    ![Dynamics 365-Testversion](./media/commercial-marketplace-get-customer-leads/dynamics-365-test-drive.png)

    ![Dynamics 365-Testversion](./media/commercial-marketplace-get-customer-leads/dynamics-365-free-trial.png)

## <a name="connect-to-your-crm-system"></a>Verbinden mit Ihrem CRM-System

[!INCLUDE [Test drive content](./includes/connect-lead-management.md)]

## <a name="understand-lead-data"></a>Grundlegendes zu Leaddaten

Für jeden Lead, den Sie während des Kundengewinnungsprozesses empfangen, gibt es Daten in bestimmten Feldern. Das erste Feld, auf das Sie achten sollten, ist das Feld `LeadSource` in folgendem Format: **Quelle-Aktion** | **Angebot**.

**Quellen**: Der Wert für dieses Feld wird basierend auf dem Marketplace gefüllt, über den der Lead generiert wurde. Mögliche Werte sind `"AzureMarketplace"`, `"AzurePortal"` und `"AppSource (SPZA)"`.

**Aktionen**: Der Wert für dieses Feld wird basierend auf der vom Kunden vorgenommenen Aktion in dem Marketplace gefüllt, über den der Lead generiert wurde. 

Mögliche Werte:

- „INS“: Installation. Diese Aktion findet im Azure Marketplace oder in AppSource statt, wenn ein Kunde Ihr Produkt kauft.
- „PLT“: Steht für „Partner Led Trial“ (partnergesteuerte Testversion). Diese Aktion findet in AppSource statt, wenn ein Kunde die Option „Kontakt mit mir aufnehmen“ verwendet.
- „DNC“: Steht für „Do Not Contact“ (keinen Kontakt aufnehmen). Diese Aktion findet in AppSource statt, wenn ein Partner, zu dem es auf Ihrer App-Seite einen Querverweis gab, um eine Kontaktaufnahme gebeten wird. Wir übermitteln die Mitteilung, dass es für diesen Kunden einen Querverweis zu Ihrer App gab, es ist aber nicht erforderlich, Kontakt zu dem Kunden aufzunehmen.
- „Create“: Diese Aktion findet nur im Azure-Portal statt und wird generiert, wenn ein Kunde über sein Konto Ihr Angebot erwirbt.
- „StartTestDrive“: Diese Aktion findet nur für Testversionen (Test Drives) statt und wird generiert, wenn ein Kunde seine Testversion startet.

**Angebote**: Möglicherweise haben Sie mehrere Angebote im Marketplace veröffentlicht. Der Wert für dieses Feld wird basierend auf dem Angebot ausgefüllt, über das der Lead generiert wurde. Die Herausgeber-ID und die Angebots-ID werden in dieses Feld gesendet. Dabei handelt es sich um die Werte, die Sie beim Veröffentlichen des Angebots im Marketplace angegeben haben.

Die folgenden Beispiele zeigen Werte im erwarteten Format `publisherid.offerid`: 

1. `checkpoint.check-point-r77-10sg-byol`
1. `bitnami.openedxcypress`
1. `docusign.3701c77e-1cfa-4c56-91e6-3ed0b622145`

## <a name="customer-info"></a>Kundeninformationen

Die Informationen des Kunden werden über mehrere Felder gesendet. Das folgende Beispiel zeigt die Kundeninformationen, die in einem Lead enthalten sind.

- Vorname: John
- Nachname: Smith
- E-Mail: jsmith\@Microsoft.com
- Telefon: 1234567890
- Land: US
- Unternehmen: Microsoft
- Titel: CTO

>[!Note]
>Nicht alle Daten in diesem Beispiel sind immer für jeden Leads verfügbar. Da Sie Leads aus mehreren Schritten erhalten (wie im Abschnitt zu Kundenleads erwähnt), stellen Deduplizierung der Datensätze und Personalisierung der Nachverfolgungen die beste Möglichkeit im Umgang mit den Leads dar. Auf diese Weise erhält jeder Kunde eine auf ihn zugeschnittene Mitteilung und erhalten Sie die Möglichkeit, eine zielgerichtete Beziehung aufzubauen.

## <a name="best-practices-for-lead-management"></a>Bewährte Methoden bei der Leadverwaltung

1. *Prozess:* Definieren Sie einen klaren Vertriebsprozess mit Meilensteinen, KPIs und klarer Teamverantwortung.
2. *Qualifizierung:* Definieren Sie die Voraussetzungen, die angeben, ob ein Lead vollständig qualifiziert wurde. Stellen Sie sicher, dass Vertriebs-oder Marketingmitarbeiter die Leads sorgfältig qualifizieren, bevor diese den vollständigen Vertriebsprozess durchlaufen.
3. *Nachverfolgung:* Vergessen Sie die Nachverfolgung nicht. Eine typische Transaktion erfordert 5 bis 12 Folgeanrufe.
4. *Pflege:* Pflegen Sie Ihre Leads, um eine höhere Gewinnspanne zu erzielen.

## <a name="leads-frequently-asked-questions"></a>Häufig gestellte Fragen zu Leads

### <a name="where-can-i-get-help-in-setting-up-my-lead-destination"></a>Wo erhalte ich Hilfe bei der Einrichtung meines Leadziels?

Die entsprechende Dokumentation finden Sie [hier](#connect-to-your-crm-system). Sie können außerdem ein Supportticket über „aka.ms/marketplacepublishersupport“ übermitteln und dann **offer creation** (Angebotserstellung) → **Ihr Angebotstyp** → **lead management configuration** (Leadverwaltungskonfiguration) auswählen.

### <a name="am-i-required-to-configure-a-lead-destination-in-order-to-publish-an-offer-on-the-marketplace"></a>Muss ich ein Leadziel konfigurieren, um ein Angebot im Marketplace zu veröffentlichen?

Die Antwort hängt davon ab, welchen Angebotstyp Sie veröffentlichen. SaaS- und Dynamics 365 for Customer Engagement-Angebote, die unter „Kontakt mit mir aufnehmen“ aufgeführt werden, alle Dynamics 365 for Operations-Angebote, alle Dynamics 365 Business Central-Angebote und alle Beratungsdienstangebote erfordern eine Verbindung mit einem Leadziel. Wenn Ihr Angebotstyp nicht unter die genannten Typen fällt, muss kein Leadziel konfiguriert werden. Es wird jedoch empfohlen, ein Leadziel zu konfigurieren, damit Sie keine Geschäftschancen verpassen.

### <a name="how-can-i-find-the-test-lead"></a>Wie finde ich den Testlead?

Suchen Sie in Ihrem Leadziel nach `"MSFT_TEST"`. Beispiel für einen Testlead von Microsoft:

```
company = MSFT_TEST_636573304831318844
country = US
description = MSFT_TEST_636573304831318844
email = MSFT_TEST_636573304831318844@test.com
encoding = UTF-8
encoding = UTF-8
first_name = MSFT_TEST_636573304831318844
last_name = MSFT_TEST_636573304831318844
lead_source = MSFT_TEST_636573304831318844-MSFT_TEST_636573304831318844|<Offer Name>
oid = 00Do0000000ZHog
phone = 1234567890
title = MSFT_TEST_636573304831318844
```

### <a name="i-have-a-live-offer-but-im-not-seeing-any-leads"></a>Ich verfüge über ein Liveangebot, aber warum sehe ich keine Leads?

Stellen Sie sicher, dass die Verbindung mit dem Leadziel gültig ist. Wir senden Ihnen einen Testlead, nachdem Sie in Partner Center für Ihr Angebot auf „Veröffentlichen“ geklickt haben. Wenn der Testlead angezeigt wird, ist die Verbindung gültig. Sie können die Leadverbindung auch testen, indem Sie versuchen, das Angebot während des Vorschauschritts in der Vorschauumgebung zu erwerben. Klicken Sie dazu in der Liste im Marketplace auf „Jetzt anfordern“, „Kontakt mit mir aufnehmen“ oder „Kostenlose Testversion“.

Stellen Sie außerdem sicher, dass Sie nach den richtigen Daten suchen. Im Abschnitt [Grundlegendes zu Leaddaten](#understand-lead-data) in diesem Artikel werden die Leaddaten beschrieben, die wir an das Leadziel senden.

### <a name="i-have-configured-azure-blob-as-my-lead-destination-why-dont-i-see-the-lead"></a>Ich habe Azure Blob als Leadziel konfiguriert. Warum sehe ich den Lead nicht?

Azure Blob als Leadziel wird nicht mehr unterstützt, sodass alle über das Angebot generierten Kundenleads verloren gehen. Wechseln Sie zu einer der anderen [Optionen für das Leadziel](./commercial-marketplace-get-customer-leads.md). 

### <a name="i-received-an-email-from-marketplace-why-cant-i-find-the-lead-in-my-crm"></a>Ich habe eine E-Mail vom Marketplace erhalten. Warum kann ich den Lead in meinem CRM nicht finden?

Es ist möglich, dass die E-Mail-Domäne des Endbenutzers die Endung „.edu“ aufweist. Aus Datenschutzgründen geben wir keine personenbezogenen Informationen von EDU-Domänen weiter. Übermitteln Sie ein Supportticket über „aka.ms/marketplacepublishersupport“.

### <a name="i-have-configured-azure-table-as-my-lead-destination-how-can-i-view-the-leads"></a>Ich habe eine Azure-Tabelle als Leadziel konfiguriert. Wie kann ich die Leads anzeigen?

Sie können über das Azure-Portal auf die in der Azure-Tabelle gespeicherten Leaddaten zugreifen, oder Sie können [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/) kostenlos herunterladen und installieren, um die Tabellendaten Ihres Azure Storage-Kontos anzuzeigen.

### <a name="i-have-configured-azure-table-as-my-lead-destination-can-i-get-notified-whenever-a-new-lead-is-sent-by-marketplace"></a>Ich habe eine Azure-Tabelle als Leadziel konfiguriert. Kann ich mich benachrichtigen lassen, wenn ein neuer Lead vom Marketplace gesendet wird?

Ja, das ist möglich. Befolgen Sie die in [dieser](./commercial-marketplace-lead-management-instructions-azure-table.md) Dokumentation aufgeführten Anweisungen zum Einrichten eines Microsoft-Flows, über den eine E-Mail gesendet wird, wenn der Azure-Tabelle ein Lead hinzugefügt wird.

### <a name="i-have-configured-salesforce-as-my-lead-destination-why-cant-i-find-the-leads"></a>Ich habe Salesforce als Leadziel konfiguriert. Warum kann ich die Leads nicht finden?

Überprüfen Sie, ob das Formular „Web to Lead“ ein Pflichtfeld auf der Grundlage einer Auswahlliste ist. Ändern Sie das Feld in diesem Fall in ein nicht obligatorisches Textfeld.

### <a name="there-was-an-issue-with-my-lead-destination-and-i-missed-some-leads-can-i-have-them-sent-to-me-in-an-email"></a>Es gab ein Problem mit meinem Leadziel, und ich habe einige Leads verpasst. Kann ich sie mir in einer E-Mail senden lassen?

Aufgrund von Richtlinien für personenbezogene Informationen (Private Identifiable Information) können wir Leadinformationen nicht über ungesicherte E-Mails weitergeben.

### <a name="i-have-configured-azure-table-as-my-lead-destination-how-much-will-it-cost"></a>Ich habe eine Azure-Tabelle als Leadziel konfiguriert. Wie hoch sind die Kosten?

Daten für die Leadgenerierung sind niedrig (< 1 GB bei fast allen Herausgebern). Die Kosten hängen von der Anzahl der erhaltenen Leads ab. Wenn 1.000 Leads in einem Monat eingehen, kostet es etwa 50 Cent. Weitere Informationen zu den Preisen für Storage finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Wenn Sie keine Antwort auf Ihre Frage gefunden haben, wenden Sie sich über „aka.ms/marketplacepublishersupport“ an den Support, und wählen Sie dann **offer creation** (Angebotserstellung) → **Ihr Angebotstyp** → **lead management configuration** (Leadverwaltungskonfiguration) aus. 

## <a name="next-steps"></a>Nächste Schritte

Nach der technischen Einrichtung sollten Sie diese Leads in Ihre aktuelle Vertriebs- und Marketingstrategie sowie in Ihre Betriebsprozesse integrieren. Wir sind daran interessiert, einen besseren Überblick über Ihren allgemeinen Vertriebsprozess zu erhalten, und möchten bei der Bereitstellung hochwertiger Leads und ausreichender Daten eng mit Ihnen zusammenarbeiten, um zu Ihrem Erfolg beizutragen. Wir freuen uns, wenn Sie uns Ihr Feedback mitteilen würden, wie wir die Leads, die wir Ihnen zusenden, durch zusätzliche Daten optimieren und verbessern können, um diesen Kunden zum Erfolg zu verhelfen. Teilen Sie uns Ihr [Feedback](mailto:AzureMarketOnboard@microsoft.com) und Ihre Vorschläge mit, damit wir Ihrem Vertriebsteam durch Marketplace-Leads zu größerem Erfolg verhelfen können.
