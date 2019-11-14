---
title: Rechnungen für die Azure-Unternehmensregistrierung
description: In diesem Artikel wird erläutert, wie Sie Ihre Azure-Unternehmensrechnung verwalten und Aktionen dafür ausführen.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 11/07/2019
ms.topic: conceptual
ms.service: billing
manager: boalcsva
ms.openlocfilehash: 6bd70a58552a0217cff82cad10b11783aec64347
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2019
ms.locfileid: "73888403"
---
# <a name="azure-enterprise-enrollment-invoices"></a>Rechnungen für die Azure-Unternehmensregistrierung

In diesem Artikel wird erläutert, wie Sie Ihre Azure-Unternehmensrechnung verwalten und Aktionen dafür ausführen. Ihre Rechnung stellt Ihre Abrechnungsdaten dar. Prüfen Sie daher, ob sie korrekt ist. Machen Sie sich außerdem auch mit anderen Aufgaben vertraut, die ggf. zum Verwalten Ihrer Rechnung erforderlich sind.

## <a name="change-a-po-number-for-an-overage-invoice"></a>Ändern einer Auftragsnummer für eine Überschreitungsrechnung

Das Azure EA-Portal generiert automatisch eine Standardauftragsnummer (Purchase Order, PO), sofern der EA-Administrator vor dem Rechnungsdatum keine Nummer festlegt. Ein EA-Administrator kann die Auftragsnummer bis zu sieben Tage nach dem Empfang einer automatisierten E-Mail-Rechnungsbenachrichtigung aktualisieren.

Eine Auftragsnummer kann auch für ein bestimmtes Konto oder Abonnement eingegeben werden. Für Berichte wird standardmäßig die niedrigste Auftragsnummer in der Hierarchie festgelegt, d. h., wenn keine Auftragsnummer für das Abonnement eingegeben wird, wird die Kontouftragsnummer verwendet, und wenn kein Kontoauftrag eingegeben wird, wird der Auftrag der Abteilung verwendet.

### <a name="to-update-the-azure-services-purchase-order-number"></a>So aktualisieren Sie die Auftragsnummer der Azure-Dienste:

1. Klicken Sie im Azure EA Portal auf **Bericht** und dann auf **Nutzungszusammenfassung**.
1. Wählen Sie in der oberen rechten Ecke **Auftragsnummern bearbeiten** .
1. Aktivieren Sie das Optionsfeld **Azure-Dienste**.
1. Wählen Sie im Dropdownmenü der Datumsbereiche einen **Rechnungszeitraum** aus. Auftragsnummern können bis zu sieben Tage nach der Rechnungsbenachrichtigung oder bis zur Zahlung der Rechnung bearbeitet werden, je nachdem, was zuerst eintritt.
1. Geben Sie eine neue Auftragsnummer in das Feld  **Auftragsnummer** ein.
1. Klicken Sie auf **Speichern** , um die Änderung zu übermitteln.

### <a name="to-update-the-marketplace-purchase-order-number"></a>So aktualisieren Sie die Marketplace-Auftragsnummer:

1. Klicken Sie im Azure EA Portal auf **Bericht** und dann auf **Nutzungszusammenfassung**.
1. Wählen Sie in der oberen rechten Ecke **Auftragsnummern bearbeiten** aus.
1. Aktivieren Sie das Optionsfeld **Marketplace**.
1. Wählen Sie im Dropdownmenü der Datumsbereiche einen **Rechnungszeitraum** aus. Auftragsnummern können bis zu sieben Tage nach der Rechnungsbenachrichtigung oder bis zur Zahlung der Rechnung bearbeitet werden, je nachdem, was zuerst eintritt.
1. Geben Sie eine neue Auftragsnummer in das Feld  **Auftragsnummer** ein.
1. Klicken Sie auf **Speichern** , um die Änderung zu übermitteln.

## <a name="cadence-of-azure-ea-billing"></a>Kadenz der Azure EA-Abrechnung

### <a name="billing-intervals"></a>Abrechnungsintervalle

Microsoft erstellt jährlich am jeweiligen Registrierungsdatum eine Rechnung für alle Verpflichtungskäufe von Microsoft Azure-Diensten und nachträglich für die Nutzung, die über die Verpflichtungsbeträge hinausgeht. Die Verpflichtungsgebühren werden basierend auf einem monatlichen Tarif festgelegt und im Voraus jährlich in Rechnung gestellt. Überschreitungsgebühren werden monatlich berechnet und am Ende des Abrechnungszeitraums nachträglich in Rechnung gestellt.

Je nachdem, wie Sie Ihre Verpflichtungskäufe vornehmen möchten, ist ihre jährliche Verpflichtung entweder mit dem Jahrestag Ihrer Registrierung oder mit dem Gültigkeitsdatum Ihres einjährigen Zusatzabonnements fällig.

Sie erhalten Ihre Überschreitungsrechnung abhängig vom Startdatum von Registrierung und Einrichtung.

**Direkte Registrierungen mit Startdatum vor dem 1. Mai 2018**: Für direkte Enterprise Azure-Kunden (EA) wird ein jährlicher Abrechnungszeitraum für Azure-Dienste eingerichtet (ausgenommen Marketplace-Dienste). Ihr Abrechnungszeitraum basiert auf dem Jahrestag. Das Jahrestag ist das Datum, an dem Ihre Vereinbarung wirksam wurde. Um die erste Azure-Dienst-Überschreitungsrechnung zu erhalten, müssen Sie 150 % des Schwellenwerts Ihres Mindestverbrauchs („MC“) überschreiten.  Sobald die gesamte Dienstnutzung 150 % Ihres MC-Schwellenwerts überschreitet, erfolgt automatisch die Konvertierung in einen vierteljährlichen Abrechnungszeitraum, der auf dem Jahrestag basiert.  Wenn Sie 150 % Ihres MC-Schwellenwerts nicht überschreiten, verbleibt die Registrierung in einem jährlichen Abrechnungszeitraum, und die Überschreitungsrechnung erhalten Sie am Ende des Verpflichtungsjahrs.

**Direkte Registrierungen mit Startdatum nach dem 1. Mai 2018**: Der Azure-Verbrauch des Kunden und separat abgerechnete Gebühren werden in einem monatlichen Abrechnungszeitraum abgerechnet.  Gebühren, die nicht durch den Azure-Mindestverbrauch abgedeckt werden, sind als Überschreitungszahlung fällig.  

**Indirekte Registrierungen mit Startdatum vor dem 1. Mai 2018**: Für indirekte Enterprise Azure-Kunden (EA) wird ein vierteljährlicher Abrechnungszyklus eingerichtet.  Der Channelpartner (CP) rechnet direkt mit dem Kunden ab.  

**Indirekte Registrierungen mit Startdatum nach dem 1. Mai 2018**: Jede indirekte Vereinbarung mit einem Startdatum ab dem 1. Mai 2018 wird monatlich abgerechnet.  

### <a name="increasing-commitment"></a>Erhöhung der Verpflichtung

Die Verpflichtung kann jederzeit erhöht werden, und es wird die Anzahl der Monate in Rechnung gestellt, die für den Verpflichtungszeitraum des betreffenden Jahrs verbleiben. Wenn Sie sich z. B. für ein einjähriges Zusatzabonnement registrieren und ihre Verpflichtung während des 6. Monats erhöhen, werden Ihnen die verbleibenden sechs Monate dieser Laufzeit in Rechnung gestellt. Die Höhe Ihrer Verpflichtung wird dann für die letzten sechs Monaten ihrer Verpflichtungslaufzeit aktualisiert, um Überschreitungsgebühren zu ermitteln.

### <a name="overage"></a>Überschreitung

Bei Überschreitung werden Ihnen die Nutzung oder Reservierungen in Rechnung gestellt, die ihre Verpflichtung während des Abrechnungszeitraums überschritten haben. Eine Aufschlüsselung der Berechnung der Überschreitungsmenge für einzelne Elemente finden Sie im Bericht der Nutzungszusammenfassung, oder wenden Sie sich an Ihren Channelpartner.

Für jedes Element in der Rechnung sehen Sie die Gesamtgebühren (Erweiterter Betrag), den zum Abdecken der Gebühren verwendeten Umfang der Verpflichtung (Nutzung der Verpflichtung) und den Betrag der Gebühren bei Überschreitung der Verpflichtung (Nettobetrag).  Die fälligen Steuern werden nur in Bezug auf den Nettobetrag bei Überschreitung der Verpflichtung berechnet.

Rechnungsstellung bei Überschreitung ist automatisiert.  Der Zeitpunkt von Benachrichtigungen und Rechnungen hängt vom Enddatum des Abrechnungszeitraums des Kunden ab.  Die Überschreitungsbenachrichtigungen werden in der Regel sieben Tage nach dem Abrechnungsenddatum des Kunden gesendet, währenddessen Kunden das Portal aufsuchen, ihre Gebühren überprüfen und die vom System generierten Auftragsnummern aktualisieren können (diese können auch zu einem beliebigen Zeitpunkt aktualisiert werden, bevor die Überschreitung veröffentlicht wird).  Überschreitungsrechnungen werden 7-9 Tage später gesendet.

### <a name="azure-marketplace"></a>Azure Marketplace

Ab dem Abrechnungszyklus von April 2019 erhalten Kunden eine einzelne Azure-Rechnung, da wir alle Azure- und Azure Marketplace-Gebühren in einer einzigen Rechnung zusammengefasst haben, anstatt zwei separate Rechnungen zu erstellen. (Diese Änderung gilt nicht für Kunden in Australien, Japan oder Singapur.) Während der Umstellung auf eine konsolidierte Rechnung erhalten Sie eine partielle Marketplace-Rechnung. Diese abschließende gesonderte Rechnung enthält Marketplace-Gebühren vor dem Datum Ihres Abrechnungsupdates. Marketplace-Gebühren nach diesem Datum sind in ihrer Azure-Rechnung enthalten. Nach dem Übergangszeitraum sind alle Azure- und Marketplace-Gebühren in der konsolidierten Rechnung enthalten.  

### <a name="purchase-order-numbers"></a>Auftragsnummern

Der Standard für den Auftrag ist eine systemgenerierte Auftragsnummer. Benutzer können Ihre Auftragsnummer im Enterprise Portal aktualisieren, indem sie sich als Unternehmensadministrator anmelden und zum Abschnitt „Berichte“ navigieren. In der oberen rechten Ecke des Fensters wird ein Feld für die Auftragsnummer angezeigt, in dem der Unternehmensadministrator die Auftragsnummer durch Klicken auf das Stiftsymbol bearbeiten kann.

## <a name="adjust-billing-frequency"></a>Ändern des Fakturierungsintervalls

Das Fakturierungsintervall eines Kunden kann jährlich, vierteljährlich oder monatlich sein. Der Abrechnungszeitraum wird festgelegt, wenn ein Kunde seine Vereinbarung unterzeichnet. Die monatliche Abrechnung ist das kleinste Abrechnungsintervall.

Die Genehmigung des Unternehmensadministrators ist erforderlich, um den Abrechnungszeitraum für direkte Registrierungen von jährlich in vierteljährlich zu ändern. Für indirekte Registrierungen ist die Genehmigung eines Partneradministrators erforderlich. Die Änderung wird am Ende des laufenden Abrechnungsquartals wirksam.

Um einen jährlichen oder vierteljährlichen Abrechnungszeitraum in monatlich zu ändern, ist ein Zusatz zur Vereinbarung erforderlich.  Jede Änderung am bestehenden Abrechnungszeitraum der Registrierung erfordert die Genehmigung eines Unternehmensadministrators oder der Person, die in Ihrer Vereinbarung als _Bill to Contact_ (Rechnungsempfänger) identifiziert wurde. Sie können Ihre Genehmigung über den [Azure EA-Portal-Support](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) übermitteln. Wählen Sie die Problemkategorie **Abrechnung und Fakturierung** aus.  Die Änderung wird am Ende des laufenden Abrechnungsquartals wirksam.

Wenn eine M503-Zusatzvereinbarung geschlossen wurde, können Sie jeden Vertrag von einem beliebigen Intervall in eine monatliche Abrechnung ändern. 

## <a name="credits-and-adjustments"></a>Gutschriften und Berichtigungen

Alle Gutschriften oder Berichtigungen in Bezug auf Ihre Registrierung erscheinen unter [https://ea.azure.com](https://ea.azure.com) im Abschnitt **Berichte**.

Führen Sie die folgenden Schritte aus, um die Gutschriften anzuzeigen:

1. Wählen Sie den Abschnitt **Berichte** aus.
1. Klicken Sie auf **Nutzungszusammenfassung**.
1. Ändern Sie oben rechts die _M_-Ansicht in die _C_-Ansicht.
1. Erweitern Sie das Anpassungsfeld in der Tabelle „Mindestabnahme für Azure-Dienst“.
1. In dieser Zeile sehen Sie die Gutschriften, die auf Ihre Registrierung angewendet werden, und eine kurze Erklärung, etwa: SLA-Gutschrift.

## <a name="request-an-invoice-copy"></a>Anfordern einer Rechnungskopie

Wenden Sie sich an Ihren Partner, um eine Kopie Ihrer Rechnung anzufordern.

## <a name="overage-offset-by-customers"></a>Überschreitungsausgleich durch Kunden

Wenn ein Kunde Überschreitungen durch einen Mindestverbrauch ausgleichen möchte, müssen die folgenden Kriterien erfüllt sein:

- Beim Kunden sind Überschreitungsgebühren angefallen, die noch nicht gezahlt wurden und innerhalb eines Jahres nach dem Enddatum des abgerechneten Diensts fällig sind.
- Der verfügbare Betrag für den Mindestverbrauch deckt die angefallenen Gebühren vollständig ab, einschließlich aller vergangenen unbezahlten Azure-Rechnungen.
- Der abzuschließende Abrechnungszeitraum muss vollständig geschlossen werden. Die Abrechnung wird am fünften Tag jedes Monats vollständig geschlossen.
- Der auszugleichende Abrechnungszeitraum muss vollständig geschlossen werden.
- Azure-Vertragsrabatte basieren auf dem neuen Mindestverbrauch abzüglich der Beträge, die für den vorherigen Verbrauch geplant wurden. Diese Anforderung gilt nur für angefallene Überschreitungsgebühren. Dies gilt nur für Dienste, für die ein Mindestverbrauch gültig ist, sodass Sie keine Marketplace-Gebühren abdecken können. Marketplace-Gebühren werden separat in Rechnung gestellt.
- Wenn ein Kunde einen Überschreitungsausgleich durchführen möchte, kann er eine Supportanfrage eröffnen. Alternativ kann auch das Kontoteam die Supportanfrage öffnen. Um den Vorgang abzuschließen, ist eine E-Mail-Genehmigung vom EA-Administrator des Kunden oder vom Rechnungsempfänger erforderlich.

## <a name="view-price-sheet-information"></a>Preisblattinformationen anzeigen

Unternehmensadministratoren können die mit ihrer Registrierung für Azure-Dienste verknüpfte Preisliste anzeigen.

So zeigen Sie das aktuelle Preisblatt an:

1. Klicken Sie im Azure EA-Portal auf **Berichte** und anschließend auf **Preisblatt**.
2. Zeigen Sie das Preisblatt an, oder klicken Sie auf **Herunterladen**.

![Beispiel für Preisblattinformationen](./media/billing-ea-portal-enrollment-invoices/ea-create-view-price-sheet-information.png)

So laden Sie eine Verlaufspreisliste herunter:

1. Klicken Sie im Azure EA-Portal auf **Berichte** und anschließend auf **Nutzung herunterladen**.
2. Laden Sie das Preisblatt herunter.

![Beispiel für das Herunterladen eines älteren Preisblatts](./media/billing-ea-portal-enrollment-invoices/create-ea-view-price-sheet-download-historical-price-list.png)

Wenn es zu Abweichungen bei den Preisen kommt, kann dies u. a. folgende Gründe haben:

Zwischen der vorherigen und der neuen Registrierung haben sich möglicherweise die Preise geändert. Preisänderungen treten auf, weil die Preise für eine bestimmte Registrierung vom Start- bis zum Enddatum einer Vereinbarung vertragsgebunden sind. Wenn eine Registrierung in eine neue Registrierung übertragen wird, gelten die Preise des neuen Vertrags. Die Preise werden durch das Preisblatt des Kunden definiert. Daher können die Preise bei der neuen Registrierung höher sein.

Bei einer Laufzeitverlängerung der Registrierung ändern sich auch die Preise. Die Preise ändern sich in Tarife für die nutzungsbasierte Bezahlung.

## <a name="request-detailed-usage-information"></a>Anfordern ausführlicher Informationen zur Verwendung

Unternehmensadministratoren können eine Zusammenfassung ihrer Nutzungsdaten, den verbrauchten Mindestverbrauch und die Gebühren für die zusätzliche Nutzung im Azure EA-Portal anzeigen. Die Gebühren werden auf der Übersichtsebene für alle Konten und Abonnements angezeigt.

Wenn Sie Verwendungsdetails für spezifische Konten anzeigen möchten, laden Sie den Bericht mit Verwendungsdetails herunter. Navigieren Sie dazu zu **Berichte** > **Nutzung herunterladen**. Der Bericht enthält keine anwendbaren Steuern. Zwischen der Nutzung und der Angabe im Bericht gibt es unter Umständen eine Wartezeit von bis zu acht Stunden.

Bei indirekten Registrierungen muss Ihr Partner die Markupfunktion aktivieren, bevor Sie kostenbezogene Informationen anzeigen können.

## <a name="reports"></a>Berichte

Unternehmensadministratoren können eine Zusammenfassung Ihrer Nutzungsdaten, den verbrauchten Mindestverbrauch und die Gebühren für die zusätzliche Nutzung im Enterprise Portal anzeigen. Die Gebühren werden auf der Übersichtsebene für alle Konten und Abonnements angezeigt.

**EA-Berichte**

- Nutzungszusammenfassung und Diagramme
- Dienstnutzungsbericht
- Saldo- und Gebühr-Bericht
- Verwendungsdetails-Bericht
- Marketplace-Gebührenbericht
- Preisblatt
- Download des erweiterten Berichts
- Formatierung von CSV-Berichten

**So zeigen Sie die Berichte und Diagramme zur Nutzungszusammenfassung an**

1. Klicken Sie im Azure EA Portal im linken Navigationsbereich auf **Berichte**, und zeigen Sie die Registerkarte **Nutzungszusammenfassung** an.
1. Wählen Sie die gewünschte Verpflichtungslaufzeit im Dropdownmenü mit den Datumsbereichen oben links aus.
1. Wählen Sie den gewünschten Zeitraum oder Monat im Diagramm aus, um weitere Details anzuzeigen.
1. Zeigen Sie das Diagramm der Nutzung pro Monat mit einer Aufschlüsselung der Nutzung, zu viel berechneten Diensten, separat berechneten Gebühren und Marketplace-Gebühren an.
1. Filtern Sie für den ausgewählten Monat nach Abteilungen, Konten und Abonnements unterhalb des Diagramms.
1. Wechseln Sie zwischen den Aufschlüsselungen „Gebühr nach Diensten“ und „Gebühr nach Hierarchie“.
1. Zeigen Sie Azure-Dienste, separat abgerechnete Gebühren und Azure Marketplace-Gebühren im Detail.

## <a name="service-usage-report"></a>Dienstnutzungsbericht

Auf der Seite „Dienstnutzungsbericht“ können Unternehmensadministratoren eine Zusammenfassung ihrer Dienstnutzungsdaten anzeigen. Während die Nutzung auf der Zusammenfassungsebene für alle Konten und Abonnements angezeigt wird, können Sie den Bericht auch nach Konten oder Abonnements filtern, um die Nutzung ausführlich anzuzeigen.

Beachten Sie, dass möglicherweise auch eine Latenz von bis zu fünf Tagen zwischen dem anfallenden Nutzungsdatum und dem Zeitpunkt, zu dem sich diese Nutzung im Bericht widerspiegelt, auftreten kann.

### <a name="to-view-the-report"></a>So zeigen Sie den Bericht an:

1. Melden Sie sich beim Enterprise Portal an.
1. Klicken Sie im linken Navigationsbereich auf **Berichte**.
1. Klicken Sie auf die Registerkarte **Nutzungszusammenfassung**.
1. Klicken Sie auf den gewünschten Datumsbereich.
1. Wählen Sie die anzuzeigenden Konten oder Abonnements aus.
1. Ändern Sie die Ansicht von „Gebühr nach Diensten“ und „Gebühr nach Hierarchie“, um unterschiedliche Aufschlüsselungen anzuzeigen.
1. Zeigen Sie Details einschließlich „Dienstname“, „Berechnungseinheit“, „Verbrauchte Einheiten“, „Effektivsatz“ und „Erweiterte Kosten“ an.

## <a name="download-csv-reports"></a>Herunterladen von CSV-Berichten

Auf der Seite „Download des monatlichen Berichts“ können Unternehmensadministratoren mehrere Berichte als CSV-Dateien herunterladen, z. B. einen „Saldo- und Gebühr-Bericht“, „Verwendungsdetails-Bericht“, „Marketplace-Gebührenbericht“ und „Preisblatt“.

## <a name="to-download-reports"></a>So laden Sie Berichte herunter:

1. Klicken Sie im Azure EA Portal auf **Berichte**.
1. Wählen Sie im oberen Menüband **Verwendungsdownload** aus.
1. Wählen Sie neben dem gewünschten Bericht des Monats **Herunterladen** aus.



## <a name="csv-report-formatting"></a>Formatierung von CSV-Berichten

Kunden, die die CSV-Berichte des Azure EA Portals in Euro anzeigen, können in Bezug auf Kommas und Punkte auf Formatierungsprobleme stoßen.

Beispielsweise sehen Sie vielleicht Folgendes:

| **ServiceResource** | **ResourceQtyConsumed** | **ResourceRate** | **ExtendedCost** |
| --- | --- | --- | --- |
| Stunden | 24 | 0,0535960591133005 | 12,863,054,187,192,100,000,000 |

Folgendes sollte angezeigt werden:

| ServiceResource | ResourceQtyConsumed | ResourceRate | ExtendedCost |
| --- | --- | --- | --- |
| Stunden | 24 | 0,0535960591133005 | 1,2863054187192120000000 |

### <a name="root-cause"></a>Grundursache

Excel importiert alle Felder als allgemeinen Text und geht davon aus, dass eine Zahl nach mathematischem Standard getrennt wird: „1,000.00“.  Daher wird eine europäische Währung, wo ein Punkt (.) als Tausendertrennzeichen und ein Komma als Dezimaltrennzeichen (,) verwendet wird – „1.000,00“ – nicht ordnungsgemäß angezeigt. Dies kann je nach regionaler Spracheinstellung variieren.

### <a name="when-importing-the-csv-use-the-following-steps"></a>Führen Sie beim Importieren der CSV-Datei die folgenden Schritte aus:

1.    Öffnen Sie Excel, und wechseln Sie zu „Datei“ > „Öffnen“.
1.    Der Textimport-Assistent wird angezeigt.
1.    Wählen Sie unter „Ursprünglicher Datentyp“ die Option _Mit Trennzeichen_ aus.  Der Standard ist _Feste Breite_.
1.    Klicken Sie auf **Weiter**.
1.    Aktivieren Sie unter „Trennzeichen“ das Kontrollkästchen für „Komma“. Der Standardwert ist „Tab“ (deaktivieren).
1.    Klicken Sie auf **Weiter**.
1.    Scrollen Sie über die Spalten „ResourceRate“ und „ExtendedCost“.
1.    Wählen Sie die Spalte „ResourceRate“ aus (wird schwarz hervorgehoben).
9.    Wählen Sie im Abschnitt „Datenformat der Spalten“ die Option „Text“ anstelle von „Allgemein“ aus.  Sie werden feststellen, dass die Spaltenüberschrift von „Allgemein“ in „Text“ geändert wird.
10. Wiederholen Sie die Schritte 8 und 9 für die Spalte „ExtendedCost“. Wählen Sie **Fertig stellen** aus.

Wenn Sie Excel so konfiguriert haben, dass \*.csv-Dateien automatisch geöffnet werden, müssen Sie stattdessen die Funktion „Öffnen“ in Excel verwenden. Öffnen Sie Excel, und wechseln Sie zu „Datei“ > „Öffnen“.

## <a name="balance-and-charge-report"></a>Saldo- und Gebühr-Bericht

Die API für Bilanz und Gebühren bietet eine monatliche Übersicht über Informationen zu Bilanzen, neuen Käufen, Gebühren für den Azure Marketplace, Korrekturen und Überschreitungsgebühren. Alle Zeilen in der Zusammenfassungstabelle für die Azure-Dienstverpflichtung bleiben im Laufe des Monats statisch, während Details zu allen Käufen und Anpassungen in den Abschnitten „Neue Kaufdetails“ und „Anpassungsdetails“ angezeigt werden.

### <a name="download-the-balance-and-charge-report"></a>Herunterladen des Saldo- und Gebühr-Berichts

1. Melden Sie sich beim Azure EA Portal als Unternehmensadministrator an.
1. Klicken Sie im linken Navigationsbereich auf **Berichte**.
1. Klicken Sie auf die Registerkarte **Berichtsdownload**.
1. Wählen Sie in der Spalte _Saldo- und Gebühr_ den entsprechenden Monat aus, und klicken Sie, um den Bericht herunterzuladen.

## <a name="usage-detail-report"></a>Verwendungsdetails-Bericht

Der „Verwendungsdetails-Bericht“ bietet eine monatliche Zusammenfassung der Dienste und Mengen, die von einer Registrierung beansprucht werden, einschließlich Informationen zu Verbrauchseinheiten, Messgerättypen und verbrauchten Mengen.

### <a name="download-the-usage-detail-report"></a>Herunterladen des Verwendungsdetails-Bericht

1. Melden Sie sich beim Azure EA Portal als Unternehmensadministrator an.
1. Klicken Sie im linken Navigationsbereich auf **Berichte**.
1. Klicken Sie auf die Registerkarte **Nutzung herunterladen**.
1. Wählen Sie in der Spalte _Verwendungsdetails_ den entsprechenden Monat aus, und klicken Sie, um den Bericht herunterzuladen.

## <a name="marketplace-charges-in-azure-ea-portal-reports"></a>Marketplace-Gebühren in Azure EA Portal-Berichten

Weitere Informationen zu Marketplace-Gebühren finden Sie [hier.](https://azure.microsoft.com/marketplace/faq/)

Es gibt zwei Arten von Marketplace-Gebühren:

- **Nutzungsbasiert:** Produkte, die in Transaktionseinheiten gemessen werden.  Virtuelle Computer werden z. B. stündlich abgerechnet, und Bing-API-Suchen nach Anzahl der Suchvorgänge.
- **Monatliche Gebühr:** Wird monatlich basierend auf Nutzung/Zugriff abgerechnet.

So zeigen Sie die verschiedenen Gebühren im Enterprise Portal an:

1. **Bericht der Nutzungszusammenfassung –** zeigt **sowohl** nutzungsbasierte als auch monatliche Marketplace-Gebühren an.
1. **Bericht über Marketplace-Gebühren –** zeigt **nur** die nutzungsbasierten Marketplace-Gebühren an.  Einmalige Gebühren werden nicht angezeigt.

Beachten Sie, dass Azure Marketplace für MPSA-Registrierungen nicht verfügbar ist.

## <a name="advanced-report-download"></a>Download des erweiterten Berichts

Zur Berichterstellung zu bestimmten Datumsbereichen oder Konten kann der Download des erweiterten Berichts verwendet werden. Seit dem 30. August 2016 hat sich das Format der Ausgabedatei von XLSX in CSV geändert, um größere Datensatzgruppen zu unterstützen.

1. Wählen Sie **Download des erweiterten Berichts** aus.
1. Wählen Sie **Entsprechender Datumsbereich** aus.
1. Wählen Sie **Entsprechende Konten** aus.
1. Wählen Sie **Nutzungsdaten anfordern** aus.
1. Wählen Sie die Schaltfläche **Aktualisieren** aus, bis der Status des Berichts auf „Herunterladen“ aktualisiert wird.
1. Laden Sie den Bericht herunter.

## <a name="reporting-for-non-enterprise-administrators"></a>Berichterstellung für Nicht-Unternehmensadministratoren

Unternehmensadministratoren können den Zugriff aktivieren, um Abteilungsadministratoren (Department Administrators, DA) und Kontobesitzern (Account Owners, AO) die Kosten für eine Registrierung anzuzeigen. Nach der Aktivierung kann ein Kontobesitzer CSV-Berichte für das Konto und die Abonnements herunterladen und die Informationen im Berichtsabschnitt des Enterprise Portals anzeigen.

### <a name="to-enable-access"></a>So aktivieren Sie den Zugriff:

 1. Melden Sie sich als Unternehmensadministrator an.
 1. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
 1. Klicken Sie auf die Registerkarte **Registrierung**.
 1. Wählen Sie im Abschnitt „Registrierungsdetails“ das Stiftsymbol aus neben:
    - Gebühren anzeigen für Abteilungsadministratoren
    - Gebühren anzeigen für Kontobesitzer
 1. Wählen Sie **Aktiviert**.
 1. Klicken Sie auf **Speichern**.

### <a name="to-view-reports"></a>So zeigen Sie Berichte an:

1. Melden Sie sich beim Azure EA Portal als Abteilungsadministrator oder Kontobesitzer an.
1. Klicken Sie im linken Navigationsbereich auf **Berichte**.
1. Klicken Sie auf die Registerkarte **Nutzungszusammenfassung**, um Informationen über das Konto und das Abonnement anzuzeigen.
1. Klicken Sie auf **Verwendungsdownload**, um die CSV-Berichte anzuzeigen.

Abteilungsadministratoren und Kontobesitzer können keine Gebühren anzeigen, wenn die Funktion _Download des erweiterten Berichts_ verwendet wird. Die Kosten werden als „0 USD“ angezeigt.

„Gebühren anzeigen für Kontobesitzer“ wird für alle Kontobesitzer und alle Benutzer erweitert, die über Berechtigungen für zugehörige Abonnements verfügen. Wenn Sie ein indirekter Kunde sind, müssen die Kostenfeatures von Ihrem Channelpartner aktiviert werden.

## <a name="move-usage-data-to-another-enrollment"></a>Verschieben von Nutzungsdaten in eine andere Registrierung

Nutzungsdaten werden nur verschoben, wenn eine Übertragung rückdatiert wird. Es gibt zwei Möglichkeiten, um Nutzungsdaten von einer Registrierung in eine andere zu verschieben:

- Kontoübertragungen von einer Registrierung zu einer anderen
- Registrierungsübertragungen von einer Registrierung zu einer anderen

Für beide Optionen müssen Sie eine [Supportanfrage](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) an das EA-Support-Team senden. 

## <a name="azure-ea-pricing-overview"></a>Übersicht über die Preise von Azure EA

Dieses Dokument enthält ausführliche Informationen zur Berechnung der Nutzung und beantwortet viele der am häufigsten gestellten Fragen zu den Preisen für verschiedene Azure-Dienste in Enterprise-Programmen, bei denen die Berechnungen komplexer sind.

### <a name="price-protection"></a>Preisschutz

Kunden und Channelpartner (im Fall unserer indirekten Kanäle) erhalten garantiert Preise, die höchstens den in ihrem Kundenpreisblatt (Customer Price Sheet, CPS) angegebenen entsprechen, oder den Preis, der zum Gültigkeitsdatum ihres Azure-Kaufs gültig ist. Dieser Preis wird als Baselinepreis bezeichnet. Für Dienste, die nach diesem Kauf eingeführt wurden, ist dies der Preis, der bei der Einführung des Diensts auf der gültigen Rabattebene gültig war. Dieser Preisschutz gilt für die Dauer der Verpflichtungslaufzeit, die je nach Enterprise-Programm ein oder drei Jahre betragen kann.

### <a name="price-changes"></a>Preisänderungen

Microsoft kann den aktuellen Enterprise Program-Preis für einzelne Azure-Dienste während der Laufzeit einer Registrierung löschen.  Wir werden diese reduzierten Tarife auf vorhandene Kunden ausweiten, während der niedrigere Preis in Kraft ist.  Wenn diese Tarife später erhöht werden, wird der Preis der Registrierung für einen Dienst nicht die oben beschriebene Preisschutzgrenze des Kunden überschreiten, kann jedoch relativ zum vorher gesenkten Preis steigen.  In beiden Fällen wird Microsoft Kunden und indirekte Channelpartner über bevorstehende Preisänderungen informieren und entsprechende E-Mails an die Unternehmens- und Partneradministratoren senden.

### <a name="current-effective-pricing"></a>Aktuell gültige Preise

Kunden und Channelpartner können Ihre aktuellen Preise für eine Registrierung anzeigen, indem sie sich beim [Azure Enterprise Portal](https://ea.azure.com/) anmelden und zur Preisblattseite für diese Registrierung navigieren.  Wenn Sie Azure indirekt über einen unserer Channelpartner erwerben, müssen Sie Ihre Preisaktualisierungen von Ihrem Channelpartner abrufen, wenn die Anzeige von Preisaufschlägen für Ihre Registrierung nicht aktiviert wurde.

### <a name="introduction-of-new-azure-services"></a>Einführung neuer Azure-Dienste

Wir verbessern Azure kontinuierlich und fügen regelmäßig neue Dienste hinzu, die separat von vorhandenen Diensten abgerechnet werden.  Einige Vorschaudienste sind automatisch verfügbar, während andere Kundenaktionen über das [Azure-Kontoportal](https://account.windowsazure.com/PreviewFeatures) erfordern. Beachten Sie auch, dass Preise steigen können, wenn Dienste von der Vorschau auf die allgemeine Verfügbarkeit umgestellt und SLAs für vollständige Leistung und Zuverlässigkeit wirksam werden. Schließlich werden einige Dienste zu Aktionspreisen bereitgestellt, die zur Einführung gelten und zu einem späteren Zeitpunkt erhöht werden können. Alle derartigen Erhöhungen im Zuge des Wechsels von Vorschau oder Aktionspreisen zur allgemeinen Verfügbarkeit werden nicht durch den normalen Baselinepreisschutz beschränkt und gelten für die zukünftige Nutzung dieser Dienste. Kunden können Gebühren im Zusammenhang mit diesen Diensten vermeiden, indem sie sich dafür entscheiden, den neuen Dienst nicht zu nutzen.

### <a name="introduction-of-regional-pricing"></a>Einführung regionaler Preise

Neben der Einführung neuer Dienste wechseln Dienste auch regelmäßig von einer einzelnen globalen Basis zu einem stärker regionsbezogenen Modell, da die regionale Unterstützung für diese Dienste zunimmt. Wenn die Regionalisierung eines Diensts eingeführt wird, gilt für diese neuen Regionen der Preisschutz im Vergleich zum vorherigen für die Registrierung gültigen globalen Preis. Weitere Regionen, die zu einem späteren Zeitpunkt für denselben Dienst eingeführt werden, gelten jedoch als neue Dienste und werden unabhängig vom Baselinepreisschutz zu individuellen Tarifen angeboten.

### <a name="enterprise-msdn-devtest"></a>Enterprise MSDN Dev/Test

Unternehmensadministratoren können Kontobesitzern ermöglichen, Abonnements auf dem EA MSDN Dev/Test-Angebot basierend zu erstellen. Damit dies ordnungsgemäß funktioniert, muss der Kontobesitzer die für die zugrunde liegenden MSDN-Abonnenten benötigten EA MSDN Dev/Test-Abonnements einrichten. Dadurch können aktive MSDN-Abonnenten Entwicklungs- und Testworkloads in Azure zu speziellen Dev/Test-Tarifen ausführen. Weitere Informationen finden Sie unter [EA MSDN Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/).

### <a name="enterprise-program-usage-calculation-guidelines"></a>Richtlinien zur Berechnung der Enterprise-Programmnutzung

Grundlegende öffentliche Preisinformationen, Maßeinheiten, FAQs und Richtlinien zur Erstellung von Nutzungsberichten für jeden einzelnen Dienst finden Sie unter [Azure-Produkte](https://azure.microsoft.com/services/) und [Azure-Preise](https://azure.microsoft.com/pricing/). Außerdem sollten beim Berechnen der Dienstnutzung die folgenden Richtlinien für das Enterprise-Programm verwendet werden.

### <a name="enterprise-program-units-of-measure"></a>Maßeinheiten des Enterprise-Programms

Die Maßeinheiten für Enterprise-Programme unterscheiden sich häufig von denen in anderen Programmen wie z. B. dem Microsoft Online Services-Abonnementvertrag-Programm (MOSA). Dies bedeutet, dass für eine Reihe von Diensten die Maßeinheit aggregiert wird, um die normalisierten Preise bereitzustellen. Die Maßeinheit, die in der Ansicht „Nutzungszusammenfassung“ des Enterprise Portals angezeigt wird, ist immer das Enterprise-Maß. Eine vollständige Liste der aktuellen Maßeinheiten und Konvertierungen für jeden Dienst wird in [Friendly_Service_Names.xlsx](https://azurepricing.blob.core.windows.net/supplemental/Friendly_Service_Names.xlsx) bereitgestellt.

### <a name="rounding-rules"></a>Rundungsregeln

Beim Runden innerhalb des Enterprise Portals wird kaufmännisches oder Gaußsches Runden nach IEEE-Standard verwendet. So wird bei Dezimalstellen mit dem Wert 5 auf die nächste gerade Dezimalstelle gerundet, während die gebräuchlicheren Funktionen zum Aufrunden Dezimalstellen mit dem Wert 5 immer auf die nächsthöhere Dezimalstelle runden. Diese Methode bietet im Vergleich zur standardmäßigen Excel-Logik eine genauere Gesamtsumme für die Gruppe.

Wenn die erste zu löschende Stelle den Wert 5 hat und keine Stellen folgen, bzw. die folgenden Stellen Nullen sind, erhält die vorangehende Stelle einen geraden Wert (d. h. wird auf die nächste gerade Ziffer gerundet). Beispielsweise ergibt so die Rundung auf die nächste Hundertsteldezimalstelle sowohl für 2,315 als auch 2,325 den Wert 2,32.

Wenn Sie Excel verwenden, um die Rundungs- und Konvertierungsregeln zu modellieren, die innerhalb des Enterprise Portals verwendet werden, sollten Sie die MROUND-Formeln wie unten gezeigt als Referenz verwenden.

| Szenario | Formel für kaufmännisches Runden |
| --- | --- |
| Verwendung der Rundung | =MROUND({_Quelle_}, 0,0002) |
| Preisrundung (2 Dezimalstellen) | =MROUND({_Quelle_}, 0,02) |
| Preisrundung (0 Dezimalstellen) | =MROUND({_Quelle_}, 2) |

**Tabelle** **2** **– Konvertierung von Clouddienst- und VM-Stunden**

### <a name="conversion-between-download-usage-file-and-usage-summary-portal-view"></a>Konvertierung zwischen Downloadnutzungsdatei und Portalansicht der Nutzungszusammenfassung

Rohdaten zur Ressourcenverwendung werden mit maximal sechs Dezimalstellen angezeigt, wie dem heruntergeladenen Nutzungsdatenbericht zu entnehmen ist. Allerdings rundet das Azure Enterprise Portal die Nutzung für Verpflichtungseinheiten auf vier Dezimalstellen und schneidet bei Überschreitungseinheiten alle Dezimalstellen ab. Die „Rohdatennutzung“ wird vor der Konvertierung in die Enterprise-Maßeinheit auf vier Stellen gerundet, und die resultierenden Enterprise-Einheiten werden wiederum auf vier Stellen gerundet. Die tatsächlich verbrauchten Stunden vor der Konvertierung werden nur im heruntergeladenen Nutzungsbericht und nicht in der Benutzeroberfläche selbst angezeigt.

Nehmen wir beispielsweise an, dass 694,533404 tatsächliche SQL Server-Stunden im heruntergeladenen Nutzungsbericht angezeigt werden. Diese Einheiten werden dann in 6,94533404 Einheiten von 100 Computestunden konvertiert, die dann zur Anzeige im Enterprise Portal auf 6,9453 gerundet werden.

Diese Einheiten werden dann einfach mit dem Preis je Einheit der Verpflichtung multipliziert, und das Ergebnis wird auf zwei Dezimalzahlen gekürzt, um den erweiterten Abrechnungsbetrag zu bestimmen. Für japanische Yen (JPY) und koreanische Won (KRW) wird der erweiterte Betrag auf null (0) Dezimalstellen gerundet.

Im selben Beispiel werden bei einer Überschreitung die abrechenbaren Einheiten auf sechs gekürzt und dann mit dem Preis je Einheit der Überschreitung multipliziert, um den erweiterten Abrechnungsbetrag zu bestimmen. Bei der Abrechnung für Anbieter verwalteter Dienste (Managed Service Provider, MSP) wird die gesamte als MSP gekennzeichnete Verwendung einer Abteilung nach der Konvertierung in die EA-Maßeinheit auf null (0) Dezimalstellen gekürzt, wie dies bei der Überschreitungsberichterstellung der Fall ist. Demzufolge könnte die Summe dieser Nutzung niedriger sein als die Summe aller in der Benutzeroberfläche gemeldeten Nutzung, je nachdem, ob der MSP noch im Rahmen des Saldos des Mindestverbrauchs liegt oder bereits eine Überschreitung vorliegt.

### <a name="graduated-pricing"></a>Gestaffelte Preise

Für Enterprise-Programme liegen derzeit keine gestaffelten Preise (wobei die Preise pro Einheit sinken, wenn die Auslastung zunimmt) vor. Beim Wechsel von einem MOSA-Programm mit gestaffelten Preisen zu einem Enterprise-Programm werden die Preise ggf. nach der Anpassung für Enterprise-Programmrabatte entsprechend des Basistarifs für diesen Dienst festgelegt.

### <a name="partial-hour-billing"></a>Teilweise Stundenabrechnung

Die Abrechnung erfolgt auf der Grundlage von Minuten, die in Stundeninkremente umgerechnet werden.  Die aufgelisteten Enterprise-Stundentarife werden einfach auf die Gesamtstunden einschließlich ggf. partieller Stunden angewendet.

### <a name="average-daily-consumption"></a>Durchschnittlicher täglicher Verbrauch

Wenn ein Dienst monatlich abgerechnet wird, die Nutzung jedoch täglich berichtet wird, wird diese Nutzung einmal pro Tag ausgewertet, durch 31 dividiert und mit der Anzahl der Tage in diesem Abrechnungsmonat summiert. Dies führt zu Tarifen, die für einen Monat nie höher als erwartet und für die Monate mit weniger als 31 Tagen etwas niedriger sind.

### <a name="compute-hours-conversion"></a>Konvertierung von Computestunden

In der Vergangenheit wurde die Nutzung von virtuellen Computern der Tarife „Standard“ und „Basic“ sowie Clouddiensten mit den Größen A0, A2, A3 und A4 als ein Bruchteil (bei A0) oder ein Mehrfaches (bei A2, A3 und A4) der Abrechnungsminuten für A1-VMs abgerechnet. Dies führte bei unseren Kunden zu Verwirrung, sodass eine Änderung eingeführt wurde, um die A0-, A2-, A3- und A4-Nutzung minutenbasiert abzurechnen.

Die neue Messung wurde zwischen dem 27. und 28. Januar 2016 wirksam. Im CSV-Download für Ihre Bereitstellung während dieses Übergangszeitraums werden Ihnen zwei Zeilenelemente auffallen: Eines für die Nutzung, die auf der A1-Verbrauchseinheit ausgegeben wird, und ein weiteres für die Nutzung, die auf der neuen dedizierten Verbrauchseinheit gemäß der Größe Ihrer Bereitstellung ausgegeben wird.

| **Bereitstellungsgröße** | **Berechnung der Nutzung als Mehrfaches von A1 (bis 26. Januar 2016)** | **Berechnung der Nutzung mit dedizierten Abrechnungseinheiten (ab 27. Januar 2016)** |
| --- | --- | --- |
| A0 | 0,25 einer A1-Stunde | 1 einer A0-Stunde |
| A2 | 2 einer A1-Stunde | 1 einer A2-Stunde |
| A3 | 4 einer A1-Stunde | 1 einer A3-Stunde |
| A4 | 8 einer A1-Stunde | 1 einer A4-Stunde |

### <a name="regions"></a>Regions

Für Dienste, bei denen Zone und Region die Preise beeinflussen, zeigt die folgende Tabelle die Zuordnung für geografische Bereiche und Regionen:

| geografischer Raum | Datenübertragungsregionen | CDN-Regionen |
| --- | --- | --- |
| Zone 1 | Europa, Norden <br> Europa, Westen <br> USA, Westen <br> USA, Osten <br> USA, Norden-Mitte <br> USA, Süden-Mitte <br> USA (Ost 2) <br> USA, Mitte | Nordamerika <br> Europa |
| Zone 2 | Asien-Pazifik, Osten <br> Asien-Pazifik, Südosten <br> Japan, Osten <br> Japan, Westen <br> Australien (Osten) <br> Australien, Südosten | Asien-Pazifik <br> Japan <br> Lateinamerika <br> Naher Osten/Afrika <br> Australien (Osten) <br> Australien, Südosten |
| Zone 3 | Brasilien Süd |   |

**Tabelle**  **4**  **– Datenübertragungsregionen**

Es fallen keine Gebühren für den Datenausgang zwischen Diensten (z. B. O365 und Azure) an, die im selben Rechenzentrum untergebracht sind.

### <a name="monetary-commitment-and-unbilled-usage"></a>Mindestverbrauch und nicht berechnete Nutzung

Der Mindestverbrauch von Azure ist ein Betrag, der vorab für Azure-Dienste gezahlt wird. Der Mindestverbrauch wird genutzt, wenn Dienste verwendet werden. Für Azure-Erstanbieterdienste kommt der Mindestverbrauch zum Einsatz. Ausnahmen sind getrennt abgerechnete Gebühren und Marketplace-Dienste.

### <a name="charges-billed-separately"></a>Separat abgerechnete Gebühren

Einige Produkte und Dienste, die aus Drittanbieterquellen bereitgestellt werden, werden nicht auf den Azure-Mindestverbrauch angerechnet. Stattdessen werden diese Elemente separat als Teil der Überschreitungsrechnung für den Standardabrechnungszyklus abgerechnet.

Wir haben alle Azure- und Marketplace-Gebühren in einer einzigen Rechnung zusammengefasst, die sich am Abrechnungszyklus der Registrierung orientiert. (Dies gilt nicht für Azure-Kunden in Australien, Japan oder Singapur.)

Die konsolidierte Rechnung zeigt zunächst die Azure-Nutzung, gefolgt von Marketplace-Gebühren. Kunden in Australien, Japan oder Singapur sehen die Marketplace-Gebühren weiterhin in einer separaten Rechnung.

Wenn am Ende des Standardabrechnungszyklus keine Überschreitungsrechnung erstellt wird, werden die Gebühren separat abgerechnet. (Gilt für Kunden in Australien, Japan und Singapur)

**Zu den separat in Rechnung gestellten Diensten zählen:**

- Canonical
- Citrix XenApp Essentials
- Registrierter Benutzer von Citrix XenDesktop
- OpenLogic
- Registrierter Benutzer von Remote Access Rights XenApp Essentials
- Ubuntu Advantage
- Visual Studio Enterprise (monatlich)
- Visual Studio Enterprise (jährlich)
- Visual Studio Professional (monatlich)
- Visual Studio Professional (jährlich)

## <a name="azure-marketplace-for-ea-customers"></a>Azure Marketplace für EA-Kunden

Direkten Kunden werden die Marketplace-Gebühren im Azure EA Portal angezeigt. Marketplace-Käufe und deren Nutzung werden außerhalb des Mindestverbrauchs abgerechnet, wobei die Rechnungsstellung vierteljährlich/monatlich und nachträglich erfolgt.

Indirekte Kunden finden ihre Azure Marketplace-Abonnements im Azure EA Portal auf der Seite „Abonnements verwalten“, wobei die Preise jedoch ausgeblendet sind. Kunden sollten ihren Lizenzierungsdienstanbieter zwecks Informationen zu Marketplace-Gebühren kontaktieren.

Für neue monatlich oder jährlich wiederkehrende Marketplace-Käufe wird der gesamte Betrag in dem Zeitraum in Rechnung gestellt, in dem die Marketplace-Artikel erworben wurden. Diese Artikel werden im folgenden Zeitraum am selben Tag des ursprünglichen Kaufs automatisch verlängert.

Dies wirkt sich nicht auf vorhandene wiederkehrende Marketplace-Gebühren aus. Monatlich wiederkehrende Gebühren werden weiterhin am ersten jedes Kalendermonats verlängert und die jährlichen Gebühren am Jahrestag des Kaufdatums.

Manche auf dem Azure Marketplace verfügbaren Wiederverkaufsdienste von Drittanbietern werden jetzt auf Ihren EA-Mindestverbrauchssaldo (Enterprise Agreement) angerechnet. Vorher wurden diese Dienste außerhalb des EA-Mindestverbrauchs und separat abgerechnet. Der EA-Mindestverbrauch für diese Dienste im Marketplace vereinfacht die Verwaltung von kundenseitigen Einkäufen und Zahlungen. Eine umfassende Liste der Dienste, die jetzt auf den Mindestverbrauch angerechnet werden, finden Sie auf der [Azure-Website](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment/?WT.mc_id=azurebg_email_Trans_33771_1695_Release_Direct_Tier2_March14).

### <a name="partners"></a>Partner

LSPs können eine für Marketplace spezifische Preisliste herunterladen, indem sie im Azure EA Portal zum Preisblatt navigieren und in der oberen rechten Ecke auf die **Marketplace-Preisliste** klicken. Die Marketplace-Preisliste spiegelt alle verfügbaren Dienste und ihre Preise wider.

**Um die Preisliste herunterzuladen, führen Sie die folgenden Schritte aus:**

1. Navigieren Sie zu „Berichte“ > „Preisblatt“.
1. Suchen Sie in der oberen rechten Ecke den Link zur Azure Marketplace-Preisliste unter Ihrem Benutzernamen.
1. Klicken Sie mit der rechten Maustaste auf den Link, und wählen Sie **Ziel speichern unter** aus.
1. Ändern Sie im Fenster „Speichern“ den Titel des Dokuments in _AzureMarketplacePricelist.zip_, wodurch die Datei von einer XLSX-Datei in eine ZIP-Datei geändert wird.
1. Nach dem Download verfügen Sie über eine ZIP-Datei mit länderspezifischen Preislisten.
1. LSPs sollten für länderspezifische Preise die jeweilige Länderdatei nutzen. LSPs sollten der Registerkarte „Benachrichtigungen“ entnehmen, welche SKUs im Marketplace neu sind, und welche entfernt wurden.
1. Preisänderungen sind selten, aber wenn sie auftreten, werden die LSPs 30 Tage im Voraus per E-Mail über Preissteigerungen und FX-Änderungen unterrichtet.
1. LSPs erhalten pro Registrierung, ISV und Quartal eine Rechnung.

### <a name="enabling-marketplace-purchases"></a>Aktivieren von Marketplace-Einkäufen

Unternehmensadministratoren können Marketplace-Käufe für alle Azure-Abonnements unter dieser Registrierung „deaktivieren“ oder „aktivieren“. Wenn der Unternehmensadministrator Käufe deaktiviert, und Azure-Abonnements vorhanden sind, die bereits über Marketplace-Abonnements verfügen, werden diese Marketplace-Abonnements nicht abgebrochen oder beeinträchtigt.

Obwohl Kunden ihre direkten Azure-Abonnements in EA konvertieren können, indem sie sie ihrer Registrierung im Azure EA Portal zuordnen, werden die untergeordneten Marketplace-Abonnements von dieser Aktion nicht automatisch konvertiert.

**So aktivieren Sie Marketplace-Einkäufe:**

1. Melden Sie sich beim Azure EA Portal als Unternehmensadministrator an.
1. Navigieren Sie zu _Verwalten_.
1. Klicken Sie unter _Registrierungsdetails_ auf das Stiftsymbol neben dem Zeilenelement _Azure Marketplace_.
1. Schalten Sie nach Bedarf zwischen _Aktiviert/Deaktiviert_ oder _Nur kostenlose/BYOL-SKUs\*_ um.
1. Klicken Sie auf **Speichern**.

### <a name="marketplace-charges-in-azure-ea-portal-reports"></a>Marketplace-Gebühren in Azure EA Portal-Berichten

Weitere Informationen zu Marketplace-Gebühren finden Sie [hier.](https://azure.microsoft.com/marketplace/faq/)

Es gibt zwei Arten von Marketplace-Gebühren:

- **Nutzungsbasiert:** Produkte werden in Transaktionseinheiten gemessen.  Virtuelle Computer werden z. B. stündlich abgerechnet, und Bing-API-Suchen nach Anzahl der Suchvorgänge.
- **Nicht nutzungsbasiert:** Einmalige Gebühr oder wiederkehrende monatliche Gebühr, die von der Nutzung unabhängig ist.

Nutzungsbasierte und nicht nutzungsbasierte Gebühren werden im Marketplace-Gebührenbericht aufgezeichnet.

Beachten Sie, dass Azure Marketplace für MPSA-Registrierungen nicht verfügbar ist.

\*BYOL (Bring-Your-Own-License) und die Option „Nur kostenlos“ beschränken Kaufen und Erwerben von Azure Marketplace-SKUs auf BYOL- und kostenlose SKUs.

### <a name="services-billed-hourly-for-ea"></a>Stündlich für EA abgerechnete Dienste

Application Delivery Network und Web Application Firewall werden im Gegensatz zum Monatstarif in MOSP für EA stündlich abgerechnet.

### <a name="remote-app"></a>Remote-App

EA-Kunden zahlen für die Remote-App basierend auf der EA-Preisstufe, und ihnen werden keine zusätzlichen Gebühren in Rechnung gestellt. Der Standardpreispunkt umfasst eine anfängliche Dauer von 40 Stunden, während der unbegrenzte Preispunkt eine anfängliche Dauer von 80 Stunden abdeckt. Die Remote-App verhindert die 80 Stunden überschreitende Nutzung.

## <a name="azure-marketplace-faq"></a>Azure Marketplace – häufig gestellte Fragen

In diesem FAQ-Dokument werden Aktualisierungen der Anwendbarkeit des Azure-Mindestverbrauchs für einige der im Azure Marketplace von Drittanbietern veröffentlichten Dienste überprüft.

### <a name="what-are-we-changing-with-respect-to-marketplace-services-and-azure-monetary-commitment"></a>Was ändern wir in Bezug auf Marketplace-Dienste und Azure-Mindestverbrauch?

Abgesehen von reservierten Azure-VM-Instanzen (RIs) erhalten Kunden eine separate Rechnung für alle Dienste, die sie über den Azure Marketplace erwerben. Wir erweitern die Nutzung des Azure-Mindestverbrauchs, um nun einige der von Drittanbietern im Azure Marketplace veröffentlichten Dienste einzubeziehen, die von unseren Kunden am häufigsten gekauft werden.

### <a name="why-are-we-making-this-change"></a>Warum nehmen wir diese Änderung vor?

Kunden suchen ständig nach zusätzlichen Möglichkeiten, um die Vorauszahlung zu nutzen, die sie für den Azure-Mindestverbrauch geleistet haben.  Wir gehen auf eine häufige Kundenanfrage ein, und die Ausweitung von MC auf diese Dienste betrifft einen großen Teil unserer Azure Marketplace-Kunden.

### <a name="what-is-the-customer-benefit"></a>Was ist der Vorteil für den Kunden?

Das Abrechnungsverfahren wird für die Kunden vereinfacht, und sie können sicherstellen, dass sie ihren Azure-Mindestverbrauch nutzen.  Dem vorab bezahlten Mindestverbrauch und MC nutzenden RIs diesen Vorteil hinzuzufügen, erhöht den Wert des Azure-Mindestverbrauchs.

### <a name="what-services-will-deduct-from-azure-monetary-commitment-and-how-will-my-customer-know"></a>Welche Dienste werden vom Azure-Mindestverbrauch abgezogen, und wie wird mein Kunde informiert?

Während des Azure Marketplace-Einkaufs versehen wir jeden Dienst, der den Mindestverbrauch verwendet, mit einem entsprechenden Hinweis. Derzeit werden bestimmte Dienste von Red Hat, SUSE, Autodesk und Oracle unterstützt. Dienste mit ähnlichen Benennungskonventionen, die jedoch von anderen, oben nicht aufgeführten Parteien veröffentlicht werden, werden nicht vom Mindestverbrauch abgezogen. Eine vollständige Liste ist am Ende dieser FAQ verfügbar.

### <a name="what-if-my-customer-runs-out-of-monetary-commitment"></a>Was geschieht, wenn mein Kunde seinen Mindestverbrauch überschreitet?

Für Kunden, die ihre gesamte MC genutzt haben und sich nun in der Überschreitung befinden, werden die Gebühren für diese Dienste in der nächsten Überschreitungsrechnung ggf. zusammen mit anderen Verbrauchsdiensten angezeigt.  Dies ist eine Änderung, da diese Gebühren früher mit anderen Azure Marketplace-Angeboten in einer eigenen Rechnung abgerechnet worden wären.

### <a name="why-are-we-not-enabling-azure-monetary-commitment-for-all-marketplace-purchases"></a>Warum wird der Azure-Mindestverbrauch nicht für alle Marketplace-Käufe aktiviert?

Wir arbeiten häufig daran, für die beste Kundenerfahrung im Zusammenhang mit dem Azure-Mindestverbrauch zu sorgen. Diese Änderung betrifft eine große Anzahl von Kunden und einen bedeutenden Teil der Gesamtausgaben im Azure Marketplace. Weitere Dienste können künftig hinzugefügt werden.

### <a name="how-does-this-impact-indirect-enrollment-and-partners"></a>Wie wirkt sich dies auf indirekte Registrierung und Partner aus?

Unsere Kunden oder Partner mit indirekter Registrierung sind nicht betroffen. Diese Dienste unterliegen denselben Partnermarkupfunktionen wie andere Verbrauchsdienste. Die einzige Änderung ist die Rechnung, auf der sie angezeigt werden, und die Zahlung von Gebühren aus dem Mindestverbrauch.

### <a name="list-of-services-that-will-deduct-from-azure-monetary-commitment"></a>Liste der Dienste, die vom Azure-Mindestverbrauch abgezogen werden

Bestimmte Azure Marketplace-Angebote können per Mindestverbrauchsguthaben bezahlt werden. Eine vollständige Liste der Produkte in diesem Programm finden Sie unter [Azure-Zahlungsverpflichtung](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment).

## <a name="additional-information"></a>Zusätzliche Informationen

Weitere Informationen finden Sie in diesen zusätzlichen Informationsquellen. Diese Dateien werden zweimal monatlich (am 6. und 20. jedes Monats) aktualisiert. Hier finden Sie ausführliche Informationen zu den Dateien:

| Anhangtitel | BESCHREIBUNG | URL-Namenskonvention |
| --- | --- | --- |
| [**Anzeigedienstnamen**](https://azurepricing.blob.core.windows.net/supplemental/Friendly_Service_Names.xlsx) | Enthält eine Liste aller aktiven Dienste mit Dienstkategorie, Anzeigedienstname, Verpflichtungsname und Teilenummer, Verbrauchsname und Teilenummer, Maßeinheiten und Faktoren zur Konvertierung zwischen gemeldeter Verwendung und angezeigter Enterprise Portal-Verwendung. | Friendly\_Service\_Names.xlsx |
| [**Dienstdownloadfelder**](https://azurepricing.blob.core.windows.net/supplemental/Service_Download_Fields.xlsx) | Diese Tabelle enthält eine Liste aller möglichen Kombinationen dienstbezogener Felder im Verwendungsdownloadbericht. | Service\_Download\_Fields.xlsx |

**Tabelle** **5** **– zusätzliche Informationsquellen**

## <a name="power-bi-reporting"></a>Power BI-Berichterstellung

### <a name="power-bi-pro"></a>Power BI Pro

Power BI Pro ist nun für EA-Kunden verfügbar. Mit Power BI Pro können Sie Berichte generieren und freigeben, um Ihre Kostendaten mit zusätzlichen Funktionen für Zusammenarbeit und Datenaktualisierung effektiv zu verwalten. Power BI Pro bietet höhere Datenkapazität und Datenstreaminggrenzwerte. Spannende neue Kostenverwaltungsfeatures für Azure Enterprise-Kunden sind in Kürze verfügbar.

Aktuelle Power BI Free-Benutzer, die das Microsoft Azure Consumption Insights-Inhaltspaket verwenden, können eine kostenlose 60-tägige Testversion von Power BI Pro nutzen. Wenn Sie Power BI Pro nach der kostenlosen Testphase weiterhin verwenden möchten, können Sie hierzu eine Lizenz hinzufügen.

Um sich für die kostenlose Testversion zu registrieren, klicken Sie auf das Zahnradsymbol, und wählen Sie **Persönlichen Speicher verwalten** aus. Wählen Sie dann auf der rechten Seite **Pro kostenlos testen** aus. Weitere Informationen zur kostenlosen Testversion von Power BI Pro finden Sie unter [Registrieren für Power BI als Einzelperson](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/#power-bi-pro-60-day-trial).

### <a name="microsoft-azure-ea-power-bi-pro-trial-terms"></a>Testbedingungen für Microsoft Azure EA Power BI Pro

- **Universell**: Das erweiterte Power BI Pro für das Inhaltspaket-Testangebot für „Microsoft Azure Enterprise“ (das „Angebot“) ist während der Laufzeit des Angebots für vorhandene qualifizierte Benutzer verfügbar, damit sie durch Verwendung eines spezifischen Power BI-Inhaltspakets bestimmte Erkenntnisse über ihren Microsoft Azure-Verbrauch gewinnen.
- **Berechtigung**: Benutzer unter einem Enterprise Agreement (EA) können das Angebot nutzen, wenn sie eine Funktion im Zusammenhang mit Abrechnung, Dienst, Dienst- und/oder Kostenverwaltung für Microsoft Azure in ihrer Organisation haben.
- **Ausschlüsse**:
  - Benutzer, die bereits am erweiterten Power BI Pro-Test teilnehmen, sind weiterhin für das bereits vorhandene Angebot qualifiziert und dürfen das Azure EA Power BI Pro-Testangebot nicht nutzen.
  - Benutzer, die das Angebot nutzen, dürfen Power BI Pro nur mit dem Microsoft Azure Enterprise-Inhaltspaket verwenden. Jegliche andere Verwendung von Power BI Pro ist unzulässig.
  - Laufzeit: Das Angebot beginnt am 1. Juni 2017 und endet am 31. Mai 2018.  Die Akzeptanz ist jederzeit während des 12-monatigen Zeitraums möglich, aber das Angebot endet am 31. Mai 2018 für alle Benutzer, unabhängig davon, wann sie das Angebot akzeptiert haben.

### <a name="to-access-the-microsoft-azure-consumption-insights-content-pack"></a>So greifen Sie auf das Inhaltspaket von Microsoft Azure Consumption Insights zu:

1. Navigieren Sie zu [Microsoft Azure Consumption Insights](https://app.powerbi.com/getdata/services/azureconsumption?cpcode=MicrosoftAzureConsumptionInsights&amp;getDataForceConnect=true&amp;WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26). Klicken Sie auf **Jetzt herunterladen**.
1. Geben Sie „Registrierungsnummer“ und „Anzahl von Monaten“ ein. Klicken Sie auf **Weiter**.
1. Geben Sie Ihren API-Zugriffsschlüssel ein, um die Verbindung herzustellen. Den Schlüssel für Ihre Registrierung finden Sie im [Enterprise Portal](https://ea.azure.com/?WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26). Klicken Sie auf **Anmelden**.
1. Der Importvorgang wird automatisch gestartet. Nach Abschluss des Vorgangs werden im Navigationsbereich ein neues Dashboard, ein Bericht und ein Modell angezeigt. Klicken Sie auf das Dashboard, um die importierten Daten anzuzeigen.

Weitere Informationen zum Generieren des API-Schlüssels für Ihre Registrierung finden Sie in der Hilfedatei zu API-Berichten im [Enterprise Portal](https://ea.azure.com/?WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26). Weitere Informationen zum neuen Inhaltspaket erhalten Sie, wenn Sie das [Microsoft Azure Consumption Insights](https://automaticbillingspec.blob.core.windows.net/spec/Microsoft%20Azure%20Consumption%20Insights.docx?WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26)-Dokument herunterladen.

### <a name="to-access-the-legacy-power-bi-ea-content-pack"></a>So greifen Sie auf das Legacy-Power BI EA-Inhaltspaket zu:

 1. Navigieren Sie zur [Power BI-Website](https://app.powerbi.com/getdata/services/azure-enterprise).
 1. Melden Sie sich mit einem gültigen Geschäfts-, Schul- oder Unikonto an.
    - Das Geschäfts-, Schul- oder Unikonto kann, muss aber nicht mit dem Konto identisch sein, das für den Zugriff auf die Registrierung über das Azure EA Portal verwendet wird.
 1. Wählen Sie auf dem Dashboard der Dienste **Microsoft Azure Enterprise** aus, und klicken Sie auf **Verbinden**.
 1. Wählen Sie auf dem Bildschirm „Verbindung mit Azure Enterprise herstellen“ Folgendes aus:
    - „URL der Azure-Umgebung“: [https://ea.azure.com](https://ea.azure.com/).
    - „Anzahl von Monaten“: Wählen Sie zwischen 1 und 36 aus.
    - „Registrierungsnummer“: Geben Sie die Registrierungsnummer ein.
    - Klicken Sie auf **Weiter**.
 1. Geben Sie im Feld „Authentifizierungsschlüssel“ den API-Schlüssel ein. Sie können den API-Schlüssel hier im Azure Enterprise-Portal erhalten; klicken Sie auf der Registerkarte „Nutzung herunterladen“ auf „API-Zugriffsschlüssel“.
    - Kopieren Sie den Schlüssel, und fügen Sie ihn in das Feld für „Kontoschlüssel“ ein.
 1. Das Laden von Daten in Power BI dauert je nach Größe der Datasets ungefähr 5-30 Minuten.

Die Power BI-Berichterstellung steht für direkte EA-Kunden, Partner und indirekte Kunden zur Verfügung, die Zugriff zur Anzeige von Abrechnungsinformationen haben.

## <a name="report-faq"></a>FAQ zu Berichten

In diesem Abschnitt des Artikels werden häufig gestellte Fragen zur Interpretation von Berichten beantwortet.

### <a name="why-is-my-cost-showing-as-0"></a>Warum werden meine Kosten als „0 USD“ angezeigt?

**Direkte Registrierung** Wenn Sie ein Kontobesitzer oder Abteilungsadministrator sind, wenden Sie sich an Ihren EA-Administrator, um das Preisfeature zu aktivieren:

1. Klicken Sie im linken Navigationsbereich auf **Verwalten**.
1. Klicken Sie auf den blauen Stift neben „DA-Ansichtsgebühren“ (Department Administrator, Abteilungsadministrator).
1. Wählen Sie **Aktiviert** aus, und speichern Sie.
1. Klicken Sie auf den blauen Stift neben „AO-Ansichtsgebühren“ (Account Owner, Kontobesitzer).
1. Wählen Sie **Aktiviert** aus, und speichern Sie.

Durch diese Aktion erhalten Kontobesitzer und Abteilungsadministratoren Zugriff auf Kosten-/Preisinformationen in den Verwendungsberichten.

**Indirekte Registrierung** Wenden Sie sich an Ihren Partner, damit er das Preisfeature für Sie aktiviert. Dies kann nur durch den Partner erfolgen, und sobald er das Feature aktiviert hat, können Sie die Kosten und Preise für Ihre Registrierung als EA-Administrator anzeigen.

Wenn Sie das Gebührenanzeigefeature für Ihre Kontobesitzer und Abteilungsadministratoren aktivieren möchten, befolgen Sie die oben unter **Direkte Registrierung** aufgeführten Schritte.

### <a name="there-is-no-sku-information-on-the-usage-detail-report"></a>Es sind keine SKU-Informationen im Verwendungsdetails-Bericht vorhanden

Der Verwendungsdetails-Bericht enthält keine SKU-Informationen. Allerdings können Sie die im Bericht verwendeten Dienstinformationen anzeigen. Anschließend können Sie den Preisblattbericht herunterladen, um die SKU-Informationen zu erhalten.

### <a name="the-total-amount-on-marketplace-does-not-match-on-usage-summary-and-csv-report"></a>Die Gesamtmenge im Marketplace stimmt nicht mit der Nutzungszusammenfassung und dem CSV-Bericht überein

Der Marketplace-Gebührenbericht zeigt nur die nutzungsbasierten Marketplace-Gebühren an. Einmalige Gebühren werden nicht angezeigt. Auf der Seite mit der Nutzungszusammenfassung können Sie sich über die aktuellen Nutzungen sowohl für nutzungsbasierte als auch einmalige Gebühren informieren.

### <a name="there-is-no-information-on-my-api-report"></a>Es sind keine Informationen zu meinem API-Bericht vorhanden.

API-Schlüssel laufen alle sechs Monate ab. Generieren Sie einen neuen API-Schlüssel, wenn ein Problem vorliegt. Es ist auch wichtig, dass Sie Ihren EA-Administrator bitten, die neuen API-Schlüssel zu generieren, und die Schritte in den FAQ zu API-Berichten befolgen.

### <a name="my-power-bi-report-isnt-working"></a>Mein Power BI-Bericht funktioniert nicht

Wenn Probleme mit Power BI auftreten, melden Sie sich mit einem technischen Ticket unter [https://support.powerbi.com](https://support.powerbi.com) beim Power BI-Team an, damit das Team Ihnen helfen kann.

### <a name="my-resource-tags-arent-showing-up-on-my-reports"></a>Meine Ressourcentags werden in meinen Berichten nicht angezeigt

Ressourcentags werden im Azure-Portal verwaltet. Sie können sich unter [https://portal.azure.com](https://portal.azure.com) an das Azure-Abonnement-Team wenden. Befolgen Sie die Schritte unter [diesem Link](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request), um eine Supportanfrage zu stellen.

### <a name="why-does-my-resource-rate-change-every-day"></a>Warum ändert sich mein Ressourcensatz jeden Tag?

Der Ressourcensatz im detaillierten Verwendungsbericht ist ein berechneter Wert und stellt die durchschnittliche monatliche Gebühr dar, die für einen Dienst berechnet wird. Diese Gebühr wird anhand des Durchschnitts des Mindestverbrauchs und der monatlichen Überschreitungsgebühren für eine Diensteinheit berechnet. Der Anteil der Nutzung, der mit Mindestverbrauch und Überschreitungsrate abgerechnet wird, ändert sich an dem Tag, an dem der Monat endet. Aus diesem Grund ändert sich der Ressourcensatz auch während des Monats. Der Ressourcensatz wird am fünften Tag nach dem Monatsende gesperrt.

### <a name="glossary-of-processes-for-calculating-the-resource-rate"></a>Glossar der Prozesse zum Berechnen des Ressourcensatzes

**Rohdateneinheiten insgesamt:** „Verbrauchte Menge“ innerhalb des detaillierten Verwendungsberichts.
**MOCP-Ressourcen pro Einheit:** Das Upstreamnutzungssystem gibt die Nutzungen für jeden Dienst in verschiedenen Einheiten aus. (Voreinstellung) **Verbrauch pro Einheit:** EA-Berechnungseinheit. (Voreinstellung) **Preis:** Preis je Einheit im Azure EA Portal.
**Gesamtkosten:** Erweiterte Kosten aus detailliertem Nutzungsbericht oder Nutzung der Verpflichtung + Überschreitung im Azure EA Portal.


### <a name="charges-calculation"></a>Gebührenberechnung

**Konvertieren in MOCP-Ressourcen pro Einheit** = ROUND(Rohdateneinheiten insgesamt * MOCP-Ressourcen pro Einheit,4) **Konvertieren in Einheiten** = Einheiten nach Konvertieren in MOCP-Ressourcen pro Einheit / Verbrauch pro Einheit **Gesamtkosten** = Einheiten * Preis

### <a name="download-usage-calculation-logic"></a>Herunterladen der Nutzungsberechnungslogik

**Ressourcensatz** = Gesamtkosten / (Rohdateneinheiten insgesamt / MOCP-Ressourcen pro Einheit)

Der Ressourcensatz wird von den Gebühren abgeleitet und entspricht oft nicht dem tatsächlichen Preis je Einheit im Preisblatt.

Zur Berechnung von Überschreitungsgebühren werden Rohdaten zur Ressourcenverwendung mit maximal sechs Dezimalstellen angezeigt, wie dem heruntergeladenen Nutzungsdatenbericht zu entnehmen ist. Allerdings rundet das Azure EA Portal die Nutzung für Verpflichtungseinheiten auf vier Dezimalstellen und schneidet bei Überschreitungseinheiten alle Dezimalstellen ab. Dies bedeutet, dass im Azure EA Portal für alle Nutzung, die als Überschreitung abgerechnet wird, nur die vollständigen Einheiten abgerechnet werden. Es gibt einen großen Unterschied zwischen dem Preis je Einheit und dem Ressourcensatz für Nutzung, die als Überschreitung oder in gemischten Monaten abgerechnet wird.

## <a name="next-steps"></a>Nächste Schritte
- Informationen zu Ihren Rechnungen und Gebühren finden Sie unter [Grundlegendes zu Ihrer Azure Enterprise-Rechnung](billing-understand-your-bill-ea.md).
- Informationen zur Verwendung des Azure EA-Portals finden Sie unter [Erste Schritte mit dem Azure EA-Portal](billing-ea-portal-get-started.md).
