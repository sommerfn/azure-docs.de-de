---
title: 'Hinzufügen von Handelspartnern für B2B-Integrationen: Azure Logic Apps'
description: Erstellen von Handelspartnern in Ihrem Integrationskonto für die Verwendung von Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: 681f16132c1de2ec5f3b27f80633d32879b0746c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2019
ms.locfileid: "67330277"
---
# <a name="add-trading-partners-to-integration-accounts-for-azure-logic-apps"></a>Hinzufügen von Handelspartnern zu Integrationskonten für Azure Logic Apps

In [Azure Logic Apps](../logic-apps/logic-apps-overview.md) können Sie automatisierte Workflows für die Business-to-Business-Integration (B2B) erstellen, indem Sie ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) für Ihre Logik-Apps verwenden. Erstellen Sie zur Darstellung Ihrer Organisation und anderer Parteien Handelspartner, und fügen Sie diese als Artefakten zu Ihrem Integrationskonto hinzu. Partner sind Entitäten, die an B2B-Transaktionen teilnehmen und untereinander Nachrichten austauschen.

Bevor Sie diese Partner erstellen, sollten Sie sich mit Ihren Partnern austauschen und Informationen darüber mit ihnen teilen, wie sie die Nachrichten, die der jeweils andere sendet, erkennen und überprüfen können. Sobald Sie diese Details festgelegt haben, können Sie Partner in Ihrem Integrationskonto erstellen.

## <a name="partner-roles-in-integration-accounts"></a>Partnerrollen in Integrationskonten

Sie können Details zum Nachrichtenaustausch mit Ihren Partnern festlegen, indem Sie [Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md) erstellen und als Artefakten zu Ihrem Integrationskonto hinzufügen. Für Vereinbarungen muss Ihr Integrationskonto mindestens zwei Partner enthalten. Ihre Organisation hat bei jeder Vereinbarung die Rolle des *Hostpartners*. Die Organisation, mit der Sie Nachrichten austauschen, wird als *Gastpartner* bezeichnet. Bei einem Gastpartner kann es sich um ein anderes Unternehmen oder sogar um eine Abteilung innerhalb Ihrer eigenen Organisation handeln. Nachdem Sie diese Partner hinzugefügt haben, können Sie eine Vereinbarung erstellen.

Geben Sie in der Vereinbarung die Details zum Umgang mit ein- und ausgehenden Nachrichten auf der Seite des Hostpartners an. Die **Empfangseinstellungen** in der Vereinbarung legen für eingehende Nachrichten fest, wie der Hostpartner Nachrichten vom Gastpartner empfängt. Die **Sendeeinstellungen** hingegen legen für ausgehende Nachrichten fest, wie der Hostpartner Nachrichten an den Gastpartner sendet.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement haben, [melden Sie sich für ein kostenloses Azure-Konto an](https://azure.microsoft.com/free/).

* Ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) zum Speichern Ihrer Partner, Vereinbarungen und anderer B2B-Artefakte. Dieses Integrationskonto muss mit Ihrem Azure-Abonnement verknüpft sein.

## <a name="create-partner"></a>Erstellen von Partnern

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im Azure-Hauptmenü die Option **Alle Dienste** aus. Geben Sie in das Suchfeld das Wort „Integration“ ein, und klicken Sie auf **Integrationskonten**.

   ![Auf „Integrationskonten“ klicken](./media/logic-apps-enterprise-integration-partners/find-integration-accounts.png)

1. Wählen Sie unter **Integrationskonten** das Integrationskonto aus, dem Sie Ihre Partner hinzufügen möchten.

   ![Auswählen des Integrationskontos](./media/logic-apps-enterprise-integration-partners/select-integration-account.png)

1. Wählen Sie die Kachel **Partner** aus.

   ![Auswählen der Kachel „Partner“](./media/logic-apps-enterprise-integration-partners/choose-partners.png)

1. Wählen Sie unter **Partner** die Option **Hinzufügen** aus. Geben Sie unter **Partner hinzufügen** entsprechend der nachfolgenden Tabelle die Partnerdetails ein.

   ![Auf „Hinzufügen“ klicken und Partnerdetails angeben](./media/logic-apps-enterprise-integration-partners/add-partners.png)

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Name** | Ja | Der Name des Partners |
   | **Qualifizierer** | Ja | Der authentifizierende Text, der Organisationen eindeutige Geschäftsidentitäten verleiht, z. B. **D-U-N-S (Dun & Bradstreet)** . <p>Partner können ihre Geschäftsidentitäten gemeinsam definieren. Wählen Sie dafür unter EDIFACT **Einvernehmlich definiert** oder unter X12 **Einvernehmlich festgelegt (X12)** aus. <p>Wählen Sie unter RosettaNet hingegen nur **DUNS** aus (die Standardeinstellung). |
   | **Wert** | Ja | Ein Wert, der die Dokumente ermittelt, die Ihre Logik-Apps empfangen. <p>Für RosettaNet muss dieser Wert neun Ziffern umfassen (die DUNS-Nummer). |
   ||||

   > [!NOTE]
   > Für Partner, die RosettaNet verwenden, können Sie zusätzliche Informationen angeben, indem Sie diese Partner zunächst erstellen und [anschließend bearbeiten](#edit-partner).

1. Wählen Sie **OK** aus, wenn Sie fertig sind.

   Ihr neuer Partner sollte nun auf der Liste der **Partner** angezeigt werden. Außerdem sollte in der Kachel **Partner** die Anzahl der Partner aktualisiert werden.

   ![Neuer Partner](./media/logic-apps-enterprise-integration-partners/new-partner.png)

<a name="edit-partner"></a>

## <a name="edit-partner"></a>Bearbeiten von Partnern

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrem Integrationskonto, und wählen Sie es aus.
Wählen Sie die Kachel **Partner** aus.

   ![Auswählen der Kachel „Partner“](./media/logic-apps-enterprise-integration-partners/edit.png)

1. Wählen Sie unter **Partner** den Partner aus, den Sie bearbeiten möchten, und klicken Sie auf **Bearbeiten**. Nehmen Sie unter **Bearbeiten** Ihre Änderungen vor.

   ![Vornehmen von Änderungen und Speichern](./media/logic-apps-enterprise-integration-partners/edit-partner.png)

   Sie können für RosettaNet unter **Eigenschaften des RosettaNet-Partners** die folgenden zusätzlichen Informationen angeben:

   | Eigenschaft | Erforderlich | BESCHREIBUNG |
   |----------|----------|-------------|
   | **Partner Classification** (Partnerklassifizierung) | Nein | Der Typ der Organisation des Partners |
   | **Lieferkettencode** | Nein | Der Lieferkettentyp des Partners, z. B. „IT“ oder „Elektronische Komponenten“. |
   | **Kontaktname** | Nein | Der Kontaktname des Partners |
   | **E-Mail** | Nein | Die E-Mail-Adresse des Partners |
   | **Fax** | Nein | Die Faxnummer des Partners |
   | **Telefon** | Nein | Die Telefonnummer des Partners |
   ||||

1. Klicken Sie auf **OK**, um Ihre Änderungen zu speichern, wenn Sie fertig sind.

## <a name="delete-partner"></a>Löschen eines Partners

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrem Integrationskonto, und wählen Sie es aus. Wählen Sie die Kachel **Partner** aus.

   ![Auswählen der Kachel „Partner“](./media/logic-apps-enterprise-integration-partners/choose-partners-to-delete.png)

1. Wählen Sie unter **Partner** den Partner aus, den Sie löschen möchten. Wählen Sie **Löschen** aus.

   ![Löschen eines Partners](./media/logic-apps-enterprise-integration-partners/delete-partner.png)

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md)