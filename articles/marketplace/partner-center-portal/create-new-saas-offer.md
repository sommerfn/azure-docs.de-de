---
title: Erstellen eines neuen SaaS-Angebots im kommerziellen Marketplace
description: Erstellen eines neuen SaaS-Angebots (Software-as-a-Service) für das Auflisten oder Verkaufen in Azure Marketplace, AppSource oder über das CSP-Programm (Cloud Solution Provider) mit dem kommerziellen Marketplace-Portal im Microsoft Partner Center.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 10/04/2019
ms.openlocfilehash: d035f26e4b550eb1e5d2cca161f14880814a15f6
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244164"
---
# <a name="create-a-new-saas-offer"></a>Erstellen eines neuen SaaS-Angebots

Um mit dem Erstellen von SaaS-Angeboten (Software-as-a-Service) zu beginnen, müssen Sie zunächst [ein Partner Center-Konto erstellen](./create-account.md) und das [Dashboard „Kommerzieller Marketplace“](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) öffnen, in dem die Registerkarte **Übersicht** ausgewählt ist.

![Dashboard „Kommerzieller Marketplace“ im Partner Center](./media/new-offer-overview.png)

>[!Note]
> Nach der Veröffentlichung eines Angebots werden im Partner Center am Angebot vorgenommene Änderungen erst nach einer erneuten Veröffentlichung im System und den Store Fronts aktualisiert. Sie müssen das Angebot nach dem Vornehmen von Änderungen für die Veröffentlichung einreichen.

Klicken Sie auf die Schaltfläche **+ Neues Angebot…** , und wählen Sie dann das Menüelement **Software-as-a-Service** aus. 

Wenn Sie einen anderen Angebotstyp auswählen, werden Sie möglicherweise zum älteren [Cloud-Partnerportal](https://cloudpartner.azure.com/) umgeleitet. Im kommerziellen Marketplace im Partner Center-Portal sind derzeit nur SaaS- und Dynamics 365-Angebote verfügbar.

![Fenster „Neues Angebot“ im Partner Center](./media/new-offer-click.png)

Das Dialogfeld **Neues Angebot** wird angezeigt. 

![Dialogfeld „Neues Angebot“](./media/new-offer-popup.png)


## <a name="offer-id-and-alias"></a>Angebots-ID und Angebotsalias

- **Angebots-ID:** Eindeutiger Bezeichner für jedes Angebot in Ihrem Konto. Diese ID wird für Kunden in der URL-Adresse für das Marketplace-Angebot und ggf. in Azure Resource Manager-Vorlagen angezeigt. Die Angebots-ID darf keine Großbuchstaben enthalten und muss alphanumerisch sein (Bindestriche und Unterstriche sind zulässig, Leerzeichen jedoch nicht). Sie ist auf 50 Zeichen beschränkt und kann nach der Auswahl von *Erstellen* nicht mehr geändert werden.  
Beispiel: test-offer-1
<br>Dies resultiert in der folgenden URL: `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`

- **Angebotsalias**: Der Name, der zum Verweisen auf das Angebot im Partner Center-Portal verwendet wird. Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom *Angebotsnamen* und anderen Werten, die den Kunden angezeigt werden. Dieser Wert kann nach der Auswahl von *Erstellen* nicht mehr geändert werden.

<br>Beispiel: Testangebot 1&#8482;

Klicken Sie auf **Erstellen**.  Für das Angebot wird die Seite **Angebotsübersicht** erstellt.  

<!---
![Offer overview on Partner Center](./media/commercial-marketplace-offer-overview.png)
-->

## <a name="offer-overview"></a>Angebotsübersicht

Die Seite **Angebotsübersicht** enthält Folgendes: 

- **Veröffentlichungsstatus** enthält eine visuelle Darstellung der zum Veröffentlichen des Angebots erforderlichen Schritte und Angaben zur Dauer der einzelnen Schritte. Die Symbole für unvollständige Veröffentlichungsschritte sind abgeblendet. 

- Das Menü **Angebotsübersicht** enthält eine Liste mit Links zum Ausführen von Vorgängen für das Angebot. Die Liste der Vorgänge ändert sich basierend auf der Auswahl, die Sie für das Angebot vornehmen.  
    - Wenn das Angebot ein Entwurf ist: „Entwurf löschen“ 
    - Wenn das Angebot live: „Stop sell offer“ (Verkauf des Angebots einstellen) 
    - Wenn das Angebot in der Vorschau ist: „Live schalten“ 
    - Wenn keine Bestätigung durch den Herausgeber erfolgt ist: „Veröffentlichung abbrechen“

## <a name="offer-setup"></a>Angebotseinrichtung

Auf der Registerkarte **Angebotseinrichtung** wird um die folgenden Informationen gebeten. Klicken Sie auf **Speichern**, nachdem Sie die Angaben in diesen Feldern vorgenommen haben.

- **Möchten Sie über Microsoft verkaufen?** (Ja/Nein)
    - **Ja**: Sie möchten Ihr Angebot über Microsoft verkaufen, wobei Microsoft Marketplace-Transaktionen in Ihrem Auftrag hostet, oder 
    - **Nein**: Sie möchten Ihr Angebot lediglich über die Marketplaces auflisten und alle finanziellen Transaktionen unabhängig von Microsoft abwickeln.    

### <a name="sell-through-microsoft"></a>Über Microsoft verkaufen

Der Verkauf über Microsoft ermöglicht eine bessere Ermittlung und Gewinnung von Kunden sowie das Hosten von Marketplace-Transaktionen durch Microsoft in Ihrem Namen. Außerdem lassen sich die weltweit verfügbaren Handelsfunktionen von Microsoft nutzen.

#### <a name="saas-offer-requirements"></a>Anforderungen für SaaS-Angebote

Um SaaS-Angebote (Software-as-a-Service) mit dem kommerziellen Marketplace im Partner Center aufzulisten, müssen die folgenden Kriterien erfüllt sein:

- Das Angebot muss die Identitätsverwaltung und Benutzerauthentifizierung von [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) verwenden.
- Das Angebot muss für die Integration in den Azure Marketplace [SaaS-Fulfillment-APIs](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) verwenden.
- Weitere Informationen zu den weitergehenden Anforderungen finden Sie unter [SaaS-Anwendungen: Leitfaden für die Veröffentlichung von Angeboten](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

#### <a name="saas-pricing-and-billing-options"></a>Preis- und Abrechnungsoptionen für SaaS
Bei SaaS-Lösungen, die im Azure-Abonnement des Herausgebers ausgeführt werden, umfassen die von den Kunden entrichteten Lizenzgebühren die Kosten für die Infrastruktur, auf der die Software bereitgestellt wird. Die Nutzung der Azure-Infrastruktur wird verwaltet und Ihnen als Partner direkt in Rechnung gestellt. Tatsächliche Nutzungsgebühren für die Infrastruktur werden dem Kunden nicht angezeigt. Herausgeber sollten die Nutzungsgebühren für Azure-Infrastruktur in ihren Softwarelizenzpreis einfließen zu lassen. 

SaaS bietet Unterstützung für die monatliche oder jährliche Abrechnung auf der Grundlage einer Pauschalgebühr, pro Benutzer oder gemäß der über den Gebührenabrechnungsdienst ermittelten Nutzungsgebühren. Der kommerzielle Marketplace von Microsoft arbeitet nach einem Agenturmodell, bei dem die Herausgeber die Preise festlegen, Microsoft den Kunden Rechnungen stellt und Microsoft die Einnahmen dann unter Einbehaltung einer Agenturgebühr an den Herausgeber auszahlt.

Die folgende Tabelle zeigt eine exemplarische Aufschlüsselung der Kosten und Auszahlungen zur Veranschaulichung des Agenturmodells.

|**Ihre Lizenzkosten**|**100 USD pro Monat**|
|:---|:---|
|Azure-Nutzungskosten (D1/1-Kern)|Abrechnung direkt mit dem Herausgeber, nicht mit dem Kunden|
|Microsoft führt die Abrechnung mit dem Kunden durch|100,00 USD pro Monat (der Herausgeber muss alle anfallenden oder weiterzugebenden Infrastrukturkosten in der Lizenzgebühr berücksichtigen)|

|**Microsoft berechnet**|**100 USD pro Monat**|
|:---|:---|
|Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten <br>**Für qualifizierte SaaS-Apps zahlt Microsoft 90 % Ihrer Lizenzkosten*|80,00 USD pro Monat <br>90,00 *$* pro Monat*|

- In diesem Beispiel stellt Microsoft 100,00 USD dem Kunden für Ihre Softwarelizenz in Rechnung und zahlt 80,00 USD an den Herausgeber aus.
- Für Partner, die für die **reduzierte Marketplace-Dienstgebühr** qualifiziert sind, wird von Mai 2019 bis Juni 2020 eine reduzierte Transaktionsgebühr für die SaaS-Angebote angezeigt. In diesem Szenario stellt Microsoft 100,00 USD für Ihre Softwarelizenz in Rechnung und zahlt 90,00 USD an den Herausgeber aus.

> [!NOTE]
> **Reduzierte Marketplace-Dienstgebühr**: Für SaaS-Angebote, die Sie in unserem kommerziellen Marketplace veröffentlicht haben, reduziert Microsoft die Marketplace-Dienstgebühr von 20 % (wie im Microsoft-Herausgebervertrag angegeben) auf 10 %. Damit Ihr Angebot qualifiziert ist, muss mindestens eines Ihrer Angebote von Microsoft als bereit für IP-Co-Selling oder als priorisiert für IP-Co-Selling gekennzeichnet worden sein.  Die Anforderungen für die Berechtigung müssen mindestens fünf (5) Arbeitstage vor dem Ende eines jeden Kalendermonats erfüllt sein, damit die reduzierte Marketplace-Dienstgebühr für den Monat in Anspruch genommen werden kann.  Die reduzierte Marketplace-Dienstgebühr gilt nicht für virtuelle Computer, verwaltete Apps oder andere Produkte, die über unseren kommerziellen Marketplace zur Verfügung gestellt werden.  Die reduzierte Marketplace-Dienstgebühr ist nur für qualifizierte Angebote bezüglich Lizenzgebühren verfügbar, die von Microsoft zwischen dem 1. Mai 2019 und dem 30. Juni 2020 vereinnahmt wurden.  Nach diesem Zeitpunkt gilt für die Marketplace-Dienstgebühr wieder der normale Betrag. 




#### <a name="csp-program-opt-in"></a>Abonnement des CSP-Programms
Mit dem [CSP-Programm (Cloud Solution Provider)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) können Softwareangebote mit minimalen Investitionen für Marketing und Vertrieb Millionen von qualifizierten Microsoft-Kunden erreichen.

- **Channels: Make my offer available in the CSP program** (Kanäle: Mein Angebot im CSP-Programm verfügbar machen) (Kontrollkästchen)

Wenn Sie die Option wählen, Ihr Angebot im CSP-Programm verfügbar zu machen, können Cloud Solution Provider Ihr Produkt als Teil einer Paketlösung verkaufen. 

### <a name="list-through-microsoft"></a>List through Microsoft (Über Microsoft auflisten)

Erstellen Sie eine Marketplace-Liste, um mit Microsoft für Ihr Geschäft zu werben. Wenn Sie Ihr Angebot nur auflisten und die Transaktion nicht über Microsoft ausführen lassen, nimmt Microsoft nicht direkt an Softwarelizenztransaktionen teil. Es gibt keine zugehörige Transaktionsgebühr, und der Herausgeber behält 100 % der vom Kunden gezahlten Softwarelizenzgebühren. Der Herausgeber ist für die Unterstützung aller Aspekte der Softwarelizenztransaktion verantwortlich, einschließlich, aber nicht beschränkt auf: Auftragsabwicklung, Messung, Abrechnung, Rechnungsstellung, Zahlung und Inkasso. 

- **Wie sollen potenzielle Kunden mit diesem Angebot interagieren?**

##### <a name="get-it-now-free"></a>Jetzt abrufen (kostenlos)
Listen Sie Ihr Angebot für Kunden als kostenlos, indem Sie eine gültige URL (beginnend mit *http* oder *https*) angeben, mit der die Kunden auf Ihre App zugreifen können.  Beispiel: `https://contoso.com/saas-app`

##### <a name="free-trial-listing"></a>Kostenlose Testversion (Listing)
Listen Sie Ihr Angebot für Kunden mit einem Link zu einer kostenlosen Testversion, indem Sie eine gültige URL (beginnend mit *http* oder *https*) angeben, mit der die Kunden [per 1-Klick-Authentifizierung über Azure Active Directory](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide#using-azure-active-directory-to-enable-trials) eine Testversion erhalten können.  Beispiel: `https://contoso.com/trial/saas-app`. Kostenlose Testversionen in der Angebotsliste werden von Ihrem Dienst erstellt, verwaltet und konfiguriert und weisen keine von Microsoft verwalteten Abonnements auf.

> [!NOTE]
> Die Token, die Ihre Anwendung über den Testlink erhält, können nur verwendet werden, um zum Automatisieren der Kontoerstellung in Ihrer App Benutzerinformationen aus Azure AD abzurufen. Die Authentifizierung mit diesem Token wird für Microsoft-Konten (MSA) nicht unterstützt.

##### <a name="contact-me"></a>Kontakt mit mir aufnehmen
Sammeln Sie über Ihr CRM-System (Customer Relationship Management) Kundenkontaktinformationen. Der Kunde wird gebeten, die Berechtigung zur Freigabe seiner Informationen zu erteilen. Diese Kundeninformationen sowie der Angebotsname, die Angebots-ID und der Marketplace, auf dem der Kunde Ihr Angebot gefunden hat, werden an das CRM-System gesendet, das Sie konfiguriert haben. Weitere Informationen zum Konfigurieren des CRM-Systems finden Sie unter [Einbinden der Leadverwaltung](#connect-lead-management). 

## <a name="example-marketplace-offer-listing"></a>Liste der Marketplace-Beispielangebote

![Liste der Marketplace-Beispielangebote mit Hinweisen](./media/marketplace-offer.svg)

## <a name="enable-a-test-drive"></a>Aktivieren einer Testversion

Die Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen die Möglichkeit geben, vor dem Kauf einen Test durchzuführen, was zu einer höheren Konvertierung und der Generierung von aussichtsreichen Leads führt. [Erfahren Sie mehr über Testversionen.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

- **Enable a test drive** (Testversion aktivieren) (Kontrollkästchen) 

Wenn Sie eine Testversion aktivieren, werden Sie aufgefordert, eine Demoumgebung zu konfigurieren, in der Kunden Ihr Angebot während eines festen Zeitraums testen können. 

### <a name="type-of-test-drive"></a>Typ der Testversion

- **[Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)** : Eine Bereitstellungsvorlage, die alle Azure-Ressourcen enthält, aus denen Ihre Lösung besteht. In den für dieses Szenario geeigneten Produkten werden nur Azure-Ressourcen verwendet.
- **[Dynamics 365 for Business Central](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-business-central-offer)** : Microsoft hostet und verwaltet den Testversionsdienst (einschließlich Bereitstellung) für ein Business Central-Enterprise Resource Planning-System (Finanzen, Betrieb, Lieferkette, CRM usw.).  
- **[Dynamics 365 for Customer Engagement](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/dyn365ce/cpp-customer-engagement-offer)** : Microsoft hostet und verwaltet den Testversionsdienst (einschließlich Bereitstellung) für ein Customer Engagement-System (Vertrieb, Kundendienst, Projektservice, Vor-Ort-Service usw.).  
- **[Dynamics 365 for Operations](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-dynamics-365-operations-offer)** : Microsoft hostet und verwaltet den Testversionsdienst (einschließlich Bereitstellung) für ein Finance and Operations-Enterprise Resource Planning-System (Finanzen, Betrieb, Fertigung, Lieferkette usw.). 
- **[Logik-App](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)** : Eine Bereitstellungsvorlage, die alle komplexen Lösungsarchitekturen umfasst. Alle benutzerdefinierten Produkte sollten mit diesem Testversionstyp genutzt werden.
- **[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)** : Ein eingebetteter Link zu einem benutzerdefinierten Dashboard. Für Produkte, mit denen ein interaktives Power BI-Visual veranschaulicht werden soll, sollte dieser Testversionstyp verwendet werden. Sie müssen hier lediglich Ihre eingebettete Power BI-URL hochladen.

#### <a name="additional-test-drive-resources"></a>Zusätzliche Ressourcen zu Testversionen
- [Test Drive Technical Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte technische Methoden für Testversionen, in englischer Sprache)
- [Testversion: Marketing und Best Practices](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Test Drives](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (Testversionen, Informationsblatt in englischer Sprache)

## <a name="connect-lead-management"></a>Einbinden der Leadverwaltung

[!INCLUDE [Connect lead management](./includes/connect-lead-management-a.md)]

#### <a name="additional-lead-management-resources"></a>Zusätzliche Ressourcen zur Leadverwaltung
- [Häufig gestellte Fragen zur Leadverwaltung](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Häufige Leadkonfigurationsfehler](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Lead management at a glance](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) (Übersicht über die Leadverwaltung, Informationsblatt in englischer Sprache)

Klicken Sie auf **Speichern**, bevor Sie mit dem nächsten Abschnitt fortfahren!

## <a name="properties"></a>Properties
Auf der Registerkarte **Eigenschaften** werden Sie aufgefordert, die Kategorien und Branchen, die zum Gruppieren Ihres Angebots in den Marketplaces verwendet werden, die Verträge für Ihr Angebot und die Version Ihrer App zu definieren. 

Klicken Sie auf **Speichern**, nachdem Sie die Angaben in diesen Feldern vorgenommen haben. 

### <a name="category"></a>Category
Wählen Sie mindestens eine (1) und maximal drei (3) Kategorien zum Gruppieren Ihres Angebots in den entsprechenden Marketplace-Suchbereichen aus. Geben Sie in der Angebotsbeschreibung an, wie Ihr Angebot diesen Kategorien entspricht. 

### <a name="industry"></a>Branche
Wählen Sie bis zu zwei (2) Branchen, die zum Gruppieren Ihres Angebots in den entsprechenden Marketplace-Suchbereichen verwendet werden. Wenn Ihr Angebot nicht für eine bestimmte Branche geeignet ist, wählen Sie keine aus. Geben Sie in der Angebotsbeschreibung an, wie sich Ihr Angebot für die ausgewählten Branchen eignet. 

### <a name="app-version"></a>App-Version
Dies ist ein optionales Feld, das im AppSource Marketplace verwendet wird, um die Versionsnummer Ihres Angebots anzugeben. 

### <a name="standard-contract"></a>Standardvertrag

- **Standardvertrag verwenden?**

Zur Vereinfachung des Beschaffungsprozesses für Kunden und zur Verringerung der rechtlichen Komplexität für Softwareanbieter stellt Microsoft die Vorlage „Standardvertrag“ bereit, um eine Transaktion im Marketplace zu erleichtern. 

Statt benutzerdefinierte Geschäftsbedingungen zu erstellen, können Azure Marketplace-Herausgeber ihre Software wahlweise unter dem Standardvertrag anbieten, den Kunden nur einmal überprüfen und akzeptieren müssen. 

Der Standardvertrag ist hier zu finden: https://go.microsoft.com/fwlink/?linkid=2041178.

#### <a name="terms-of-use"></a>Nutzungsbedingungen

Wenn Ihre Lizenzbedingungen vom Standardvertrag abweichen, können Sie hier die eigenen Nutzungsbedingungen eingeben. Sie können in diesem Feld bis zu 10.000 Zeichen Text eingeben. Wenn Ihre Nutzungsbedingungen eine längere Beschreibung erfordern, geben Sie in diesem Feld einen einzelnen URL-Link ein, über den Ihre zusätzlichen Lizenzbedingungen aufgerufen werden können. Er wird für Kunden als aktiver Link angezeigt.

Kunden müssen diese Bedingungen akzeptieren, bevor sie Ihre App testen können. 

Klicken Sie auf **Speichern**, bevor Sie mit dem nächsten Abschnitt fortfahren!

## <a name="offer-listing"></a>Angebotsliste

Auf der Registerkarte „Angebotsliste“ werden die Sprachen (und Märkte) angezeigt, in denen Ihr Angebot verfügbar ist. Derzeit ist nur der Standort „Englisch (USA)“ verfügbar. Auf dieser Seite werden zudem der Status der sprachspezifischen Liste und das Datum/die Uhrzeit der Hinzufügung angezeigt. Sie müssen für jede Sprache/jeden Markt die Marketplace-Informationen (Angebotsname, Beschreibung, Suchbegriffe usw.) definieren.

> [!NOTE]
> Die Inhalte der Angebotsliste (z.B. Angebotsbeschreibung, Dokumente, Screenshots, Nutzungsbedingungen und Datenschutzrichtlinien) müssen nicht in englischer Sprache vorliegen, solange die Angebotsbeschreibung mit dem folgenden Satz beginnt: „Diese Anwendung ist nur in [nicht englische Sprache] verfügbar.“ Es ist auch zulässig, eine *Nützlicher Link-URL* bereitzustellen, um Inhalte in einer anderen Sprache als der in der Angebotsliste verwendeten anzubieten.

### <a name="offer-listings"></a>Angebotsauflistungen

Geben Sie Informationen an, die im Marketplace angezeigt werden sollen, einschließlich der Beschreibung des Angebots und der Marketingressourcen.

- **Name** (erforderlich): Der hier definierte Name wird als Titel der Angebotsliste in dem/den von Ihnen gewählten Marketplace(s) angezeigt. Der Name ist basierend auf Ihrer vorherigen Eingabe in **Neues Angebot** bereits eingetragen.  Er ist möglicherweise markenrechtlich geschützt.  Er darf keine Emojis (mit Ausnahme des Markenzeichen- und Copyrightsymbols) enthalten und muss auf 50 Zeichen begrenzt sein.
- **Zusammenfassung** (erforderlich): Geben Sie eine kurze Beschreibung Ihres Angebots ein, die in den Suchergebnissen für Marketplace-Liste(n) verwendet werden soll. In diesem Feld können bis zu 100 Zeichen Text eingegeben werden.
- **Beschreibung:** (erforderlich): Geben Sie eine Beschreibung Ihres Angebots ein, die in der Übersicht über die Marketplace-Liste(n) angezeigt werden soll. Sie können z.B ein Wertversprechen, wichtige Vorteile, Kategorie- oder Branchenzuordnungen, Möglichkeiten für In-App-Käufe, erforderliche Veröffentlichungen und einen Link zu weiteren Informationen eingeben.
In diesem Feld können bis zu 3.000 Zeichen Text eingegeben werden. Weitere Tipps finden Sie im Artikel [Erstellen einer interessanten App-Beschreibung](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description).
- **Suchbegriffe**: Geben Sie bis zu drei Suchbegriffe ein, mit denen Kunden in dem/den Marketplace(s) nach Ihrem Angebot suchen können.
- **Anweisungen für den Einstieg** (erforderlich): Erläutern Sie potenziellen Kunden, wie die App konfiguriert und mit ihrer Verwendung begonnen wird.  Diese Schnellstartanleitung kann Links zu ausführlicherer Onlinedokumentation enthalten. In diesem Feld können bis zu 3.000 Zeichen Text eingegeben werden. 

#### <a name="description"></a>**Beschreibung**

Dies ist ein Pflichtfeld. Ihre Beschreibung sollte folgende Elemente enthalten: 

* Schildern Sie in den ersten Sätzen Ihrer Beschreibung deutlich das Nutzenversprechen Ihres Angebots.  
* Beachten Sie, dass die ersten Sätze in Suchmaschinenergebnissen angezeigt werden können.  
* Setzen Sie sich nicht nur auf Features und Funktionen, um Ihr Produkt zu verkaufen. Konzentrieren Sie stattdessen auf den Nutzen, den Sie bieten.  
* Verwenden Sie möglichst viel branchenspezifisches Vokabular oder eine nutzenorientierte Formulierung. 

Die Kernbestandteile Ihres Nutzenversprechens sollten folgende Informationen umfassen: 

* Beschreibung des Produkts. 
* Benutzertyp, der von dem Produkt profitiert. 
* Anforderungen oder Probleme der Kunden, die das Produkt erfüllt bzw. behebt. 

Um Ihre Angebotsbeschreibung ansprechender zu gestalten, können Sie HTML-Tags zur Formatierung verwenden. 

1. Wenn Sie Absätze erstellen möchten fügen Sie `<p>` am Anfang und `</p>` am Ende des Texts ein.

    **Beispiel**: 

    `<p>` Dies ist mein erster Absatz. `</p>` <br>
    `<p>` Dies ist mein zweiter Absatz. `</p>` <br>

    Diese Formatierung führt zu folgender Darstellung:

    <p> Dies ist mein erster Absatz. </p>
    <p> Dies ist mein zweiter Absatz. </p>

1. Wenn Sie eine **Aufzählung** hinzufügen möchten, fügen Sie den entsprechenden Text zwischen die unten genannten `<li>`-Tags ein. Sie können innerhalb der Tags `<ul>` und `</ul>` weitere Aufzählungspunkte (Elemente zwischen den Tags `<li>` und `</li>`) einfügen. Stellen Sie sicher, dass Sie `<ul></ul>` hinzufügen. 

    **Beispiel**:

    ```
    <ul> 
        <li>add text here</li> 
        <li> add text here </li> 
        <li> add text here </li> 
    </ul> 
    ```

    Diese Formatierung führt zu folgender Darstellung:
    <ul> 
        <li>Text hier einfügen</li> 
        <li> Text hier einfügen </li> 
        <li> Text hier einfügen </li> 
    </ul> 

1. Um Inhalte in **Fettschrift** zu formatieren, fügen Sie `<b>` am Anfang des fett darzustellenden Texts und `</b>` am Ende dieses Texts ein. 

    **Beispiel**: `<b>` KOSTENLOSE TESTVERSION `</b>`
    
    Mit dieser Formatierung werden die Worte KOSTENLOSE TESTVERSION in der Beschreibung des Angebots in der Storefront in Fettschrift dargestellt. 

    **KOSTENLOSE TESTVERSION**

1. Um **Zeilenumbrüche** in Ihren Inhalt einzufügen, fügen Sie `<br>` vor dem Text ein, der auf einer neuen Zeile beginnen soll. Wenn Sie zusätzlich eine Leerzeile einfügen und sicherstellen möchten, dass Text auf einer neuen Zeile beginnt, fügen Sie `<br><br>` vor dem entsprechenden Text ein. 

    **Beispiel**:

    Dies ist eine Textzeile. `<br>` Dies ist eine Textzeile, die auf einer neuen Zeile beginnt. `<br><br>` Dies ist eine Textzeile, die zwei Zeilen darunter beginnt. 

    Diese Formatierung führt zu folgender Darstellung:

    Dies ist eine Textzeile. <br> Dies ist eine Textzeile, die auf einer neuen Zeile beginnt. <br><br> Dies ist eine Textzeile, die zwei Zeilen darunter beginnt. 

1. Wenn Sie den **Schriftgrad des Texts vergrößern** möchten, entscheiden Sie zunächst, wie groß der Text angezeigt werden soll. Im Folgenden finden Sie einige Beispiele. Nachdem Sie die Schriftgröße ausgewählt haben, fügen Sie am Anfang und am Ende des betreffenden Texts die entsprechenden `<H*></H*>`-Tags hinzu. 

    **Beispiel**:

    `<h1>`Dies ist Überschrift 1`</h1>` <br>
    `<h2>`Dies ist Überschrift 2`</h2>` <br>
    `<h3>`Dies ist Überschrift 3`</h3>` <br>
    `<h4>`Dies ist Überschrift 4`</h4>` <br>
    `<h5>`Dies ist Überschrift 5`</h5>` <br>
    `<h6>`Dies ist Überschrift 6`</h6>` 

    Diese Formatierung führt zu folgender Darstellung:

    ![Beispielüberschriften](./media/heading.png)

#### <a name="links"></a>Links

- **Datenschutzrichtlinie** (erforderlich): Erstellen Sie eine Verknüpfung zur Datenschutzrichtlinie Ihrer Organisation. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.
- **Marketingmaterial für das CSP-Programm** (optional): Sie müssen einen Link zu Marketingmaterial einfügen, wenn Sie Ihr Angebot auf das [CSP-Programm (Cloud Solution Provider)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) ausdehnen. Durch CSP wird Ihr Angebot auf ein breiteres Spektrum von qualifizierten Kunden erweitert, indem CSP-Partnern das Bündeln, Vermarkten und Verkaufen Ihres Angebots ermöglicht wird. Diese Handelspartner benötigen Zugriff auf Materialien für die Vermarktung Ihres Angebots. Weitere Informationen finden Sie unter [Go-To-Market-Dienste](https://partner.microsoft.com/reach-customers/gtm).
- **Nützliche Links** (optional): Optionale zusätzliche Onlinedokumente zu Ihrer App oder zugehörigen Diensten, die durch die Angabe eines **Titels** und einer **URL** aufgeführt werden. Fügen Sie weitere nützliche Links hinzu, indem Sie auf **+ URL hinzufügen** klicken.

#### <a name="contact-information"></a>Kontaktinformationen

- **Kontakte**: Geben Sie für jeden Kundenkontakt einen **Namen**, eine **Telefonnummer** und eine **E-Mail**-Adresse des Mitarbeiters ein.  (Diese werden *nicht* öffentlich angezeigt). Zudem ist eine **Support-URL** für die Gruppe **Supportkontakt** erforderlich.  (Diese Information *wird* öffentlich angezeigt).

**Supportkontakt** (erforderlich): Für allgemeine Supportfragen.

**Technischer Ansprechpartner** (erforderlich): Für technische Fragen.

**Channel Manager contact** (Channel-Manager-Kontakt) (erforderlich): Für Fragen von Handelspartnern zum CSP-Programm.

#### <a name="files-and-images"></a>Files and Images (Dateien und Bilder)

- **Dokumente** (erforderlich): Fügen Sie zugehörige Marketingdokumente im PDF-Format für Ihr Angebot hinzu, mindestens ein (1) und maximal drei (3) Dokumente pro Angebot.
- **Bilder** (optional): In dem/den Marketplace(s) können Logobilder Ihres Angebots an mehreren Stellen angezeigt werden, wobei die folgenden Größen erforderlich sind. „Small: 48 x 48 pixels“  _(Klein: 48 x 48 Pixel) (erforderlich),_ „Medium: 90 x 90 pixels“ (Mittel: 90 x 90 Pixel), „Large: 216 x 216 pixels“  _(Groß: 216 x 216 Pixel) (erforderlich),_ „Wide: 255 x 115 pixels“ (Breit: 255 x 115 Pixel) und „Hero: 815 x 290 pixels“ (Hero: 815 x 290 Pixel). Alle Bilder müssen das PNG-Format aufweisen.
- **Screenshots** (erforderlich): Fügen Sie Screenshots hinzu, um Ihr Angebot zu veranschaulichen. Es können maximal fünf (5) Screenshots hinzugefügt werden. Ihre Größe sollte 1280 x 720 Pixel betragen. Alle Bilder müssen das PNG-Format aufweisen.
- **Videos** (optional): Fügen Sie Links zu Videos hinzu, um Ihr Angebot zu veranschaulichen. Sie können Links zu YouTube und/oder Vimeo-Videos angeben, die zusammen mit Ihrem Angebot bei Kunden angezeigt werden. Sie müssen auch ein Miniaturbild des Videos hinzufügen. Es muss das PNG-Format und eine Größe von 1280 x 720 Pixel aufweisen. Sie können bis zu vier Videos pro Angebot anzeigen.

Klicken Sie auf **Speichern**, bevor Sie mit dem nächsten Abschnitt fortfahren!

#### <a name="additional-marketplace-listing-resources"></a>Zusätzliche Ressourcen für Marketplace-Listen

- [Bewährte Methoden für Angebotslistung](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)


## <a name="preview"></a>Vorschau

Sie können auf der Registerkarte **Vorschau** eine eingeschränkte **Vorschauzielgruppe** für das Freigeben Ihres Angebots definieren, bevor Sie das Angebot für die größere Marketplace-Zielgruppe live veröffentlichen.

> [!IMPORTANT]
> Sie müssen **Live schalten** auswählen, bevor Ihr Angebot live für die öffentliche Marketplace-Zielgruppe veröffentlicht wird, nachdem Sie es in der Vorschau überprüft haben.

- **Define a Preview Audience: Add a single AAD/MSA account email per line, along with an optional description.** (Definieren Sie eine Vorschauzielgruppe: Fügen Sie eine einzelne AAD/MSA-Konto-E-Mail-Adresse pro Zeile und optional eine Beschreibung hinzu.)

Fügen Sie manuell bis zu zehn (10) oder durch Hochladen einer CSV-Datei bis zu zwanzig (20) E-Mail-Adressen für vorhandene Microsoft-Konten (MSA) oder Azure Active Directory-Konten hinzu, um Unterstützung bei der Überprüfung Ihres Angebots vor der Liveveröffentlichung zu erhalten. Durch das Hinzufügen dieser Konten definieren Sie eine Zielgruppe, die Zugriff auf die Vorschau Ihres Angebot erhält, bevor es in dem/den Marketplace(s) veröffentlicht wird. Wenn Ihr Angebot bereits live geschaltet ist, können Sie dennoch eine Vorschauzielgruppe für das Testen von Änderungen oder Aktualisierungen Ihres Angebots definieren.

> [!NOTE]
> Die Vorschauzielgruppe unterscheidet sich von einer privaten Zielgruppe. Einer Vorschauzielgruppe wird Zugriff auf das Angebot gewährt, _bevor_ es in den Marketplaces live veröffentlicht wird. Sie können auch einen Plan erstellen und nur für eine private Zielgruppe verfügbar machen. Auf der Registerkarte **Planlisting** können Sie mit dem Kontrollkästchen **This is a private plan** (Dies ist ein privater Plan) eine private Zielgruppe definieren. Sie können mithilfe von Azure-Mandanten-IDs eine private Zielgruppe von bis zu 20.000 Kunden definieren.

## <a name="technical-configuration"></a>Technische Konfiguration

Auf der Registerkarte **Technische Konfiguration** werden die technischen Informationen (URL-Pfad, Webhook, Mandanten-ID und App-ID) für die Verbindung mit Ihrem Angebot definiert. Durch diese Verbindung können wir Ihr Angebot für den Endkunden bereitstellen, wenn er es kaufen möchte. Diagramme, die die Verwendung der gesammelten Felder beschreiben, finden Sie in der Dokumentation zu [SaaS-Fulfillment-APIs](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2).

- **URL der Angebotsseite** (erforderlich): Definieren Sie die URL der Website, zu der der Kunde weitergeleitet wird, nachdem er Ihr Angebot auf dem Marketplace erworben hat. Diese URL wird der Endpunkt sein, der ein Token erhält, wenn ein Kunde auf die Seite weitergeleitet wird. Dieses Token kann über die Auflösung in den Fulfillment-APIs gegen Bereitstellungsdetails ausgetauscht werden. Diese und alle anderen Daten, die Sie sammeln, können als Teil einer interaktiven, in Ihre Benutzeroberfläche integrierten Webseite für Kunden verwendet werden, wo sie die Registrierung abschließen und den Kauf aktivieren können.

- **Verbindungswebhook** (erforderlich): Für alle asynchronen Ereignisse, die Microsoft im Auftrag des Kunden an Sie senden muss (Beispiel: das SaaS-Abonnement ist ungültig geworden), müssen Sie einen Webhook für die Verbindung angeben. Wenn Sie noch nicht über ein Webhooksystem verfügen, ist die einfachste Konfiguration eine Logik-App am HTTP-Endpunkt, die auf alle veröffentlichten Ereignisse lauscht und sie entsprechend behandelt (z.B. https:\//prod-1westus.logic.azure.com:443/work). Weitere Informationen finden Sie unter [Aufrufen, Auslösen oder Schachteln von Workflows mit HTTP-Endpunkten in Logik-Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint).

- **ID des Azure AD-Mandanten** (erforderlich): Es ist im Azure-Portal erforderlich, dass Sie eine [Azure AD-App (Active Directory) erstellen](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal), damit überprüft werden kann, ob die Verbindung zwischen unseren beiden Diensten über eine authentifizierte Kommunikation erfolgt. Um die [Mandanten-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) zu ermitteln, wechseln Sie zu Azure Active Directory, wählen Sie **Eigenschaften** aus, und suchen Sie nach der aufgeführten Nummer der **Verzeichnis-ID** (z.B. 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD-App-ID** (erforderlich): Sie benötigen auch die [Anwendungs-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) und einen Authentifizierungsschlüssel. Um diese Werte zu ermitteln, wechseln Sie zu Azure Active Directory, und wählen Sie **App-Registrierungen** aus. Suchen Sie dann nach der aufgeführten Nummer der **Anwendungs-ID** (z.B. 50c464d3-4930-494c-963c-1e951d15360e). Um den Authentifizierungsschlüssel zu suchen, wechseln Sie zu **Einstellungen**, und wählen Sie **Schlüssel** aus. Sie müssen eine Beschreibung und eine Dauer angeben und erhalten dann einen Zahlenwert.

 Beachten Sie, dass die Azure-Anwendungs-ID Ihrer Herausgeber-ID zugeordnet ist. Stellen Sie also sicher, dass in allen Ihren Angeboten dieselbe Anwendungs-ID verwendet wird.

## <a name="plan-overview"></a>Planübersicht

Auf der Registerkarte **Planübersicht** können Sie eine Vielzahl von Planoptionen innerhalb desselben Angebots bereitstellen. Diese Pläne (auch als SKUs bezeichnet) können sich in Bezug auf Version, Monetarisierung und Dienstebenen unterscheiden. Sie müssen mindestens einen Plan einrichten, um Ihr Angebot im Marketplace zu verkaufen.

Nach der Erstellung werden die Plannamen, IDs und Preismodelle, die Verfügbarkeit (öffentlich oder privat), der aktuelle Veröffentlichungsstatus und alle verfügbaren Aktionen angezeigt.

Die unter **Planübersicht** verfügbaren **Aktionen** variieren je nach dem aktuellen Status des Plans und umfassen möglicherweise:

- Wenn der Planstatus **Entwurf** lautet: „Entwurf löschen“
- Wenn der Planstatus **Live** lautet: „Stop sell plan“ (Verkauf des Plans einstellen) oder „Sync private Audience“ (Private Zielgruppe synchronisieren)

**Neuen Plan erstellen** (mindestens ein Plan für diejenigen, die den Verkauf über Microsoft abwickeln möchten)

- **Plan-ID**: Erstellen Sie für jeden Plan im Angebot eine eindeutige Plan-ID. Diese ID wird für Kunden in der Produkt-URL-Adresse und ggf. in Azure Resource Manager-Vorlagen angezeigt. Verwenden Sie nur klein geschriebene alphanumerische Zeichen, Bindestriche und Unterstriche. Für die Plan-ID sind maximal 50 Zeichen zulässig. Die Plan-ID kann nach der Erstellung nicht geändert werden.
- **Planname:** Für Kunden wird dieser Name angezeigt, wenn sie einen Plan in Ihrem Angebot auswählen. Erstellen Sie für jeden Plan im Angebot einen eindeutigen Angebotsnamen. Der Planname wird verwendet, um Softwarepläne zu unterscheiden, die möglicherweise Teil desselben Angebots sind (Beispiel: Angebotsname: Windows Server, Pläne: Windows Server 2016, Windows Server 2019).

### <a name="plan-listing"></a>Planlisting

Auf der Registerkarte **Planlisting** werden die Sprachen (und Märkte) angezeigt, in denen Ihr Angebot verfügbar ist. Derzeit ist nur der Standort „Englisch (USA)“ verfügbar. Auf dieser Seite werden zudem der Status der sprachspezifischen Liste und das Datum/die Uhrzeit der Hinzufügung angezeigt. Sie müssen für jede Sprache/jeden Markt die Marketplace-Informationen (Angebotsname, Beschreibung, Suchbegriffe usw.) definieren.

#### <a name="plan-listing-details"></a>Planlistinginformationen

Nach Auswahl einer der Plansprachen werden Informationen zum **Planlisting** angezeigt, einschließlich **Name** und **Beschreibung**.

- **Name**: Ist auf Grundlage Ihre Vorschaueintrags **Neuer Plan** im Voraus ausgefüllt und wird als Titel des Softwareplans für Ihr Angebot im Marketplace angezeigt.
- **Beschreibung:** Diese Beschreibung bietet die Möglichkeit zu erklären, was diesen Softwareplan einzigartig macht und von anderen Softwareplänen in Ihrem Angebot unterscheidet. Sie darf bis zu 500 Zeichen enthalten.

Klicken Sie auf **Speichern**, nachdem Sie die Angaben in diesen Feldern vorgenommen haben.

#### <a name="plan-pricing-and-availability"></a>Preise und Verfügbarkeit des Plans

Auf der Registerkarte **Preise und Verfügbarkeit** können Sie die Märkte, in denen der Plan verfügbar ist, das gewünschte Monetarisierungsmodell, den Preis und den Abrechnungszeitraum konfigurieren. Darüber hinaus können Sie angeben, ob der Plan für alle Benutzer oder nur für bestimmte Kunden (eine private Zielgruppe) angezeigt werden soll.

##### <a name="enabling-free-trials"></a>Aktivieren kostenloser Testversionen

SaaS-Angebote über den kommerziellen Marketplace ermöglichen Ihnen beim Verkauf über Microsoft, eine einmonatige kostenlose Testversion bereitzustellen. Kostenlose Testversionen werden für alle Abrechnungsmodelle und -bedingungen mit Ausnahme von Volumentarifen unterstützt. Diese Option ermöglicht es Kunden, durch den kostenlosen Zugriff für einen Monat eine niedrige Schwelle für den Einstieg zu erhalten.  Wenn Sie eine kostenlose Testversion für Pläne in Ihrem Angebot aktivieren, kann der Kunde diese Version nicht vor Ende des ersten Monats in ein kostenpflichtiges Abonnement umwandeln.  Während dieses Zeitraums können Kunden, die Ihr Angebot erwerben, alle unterstützten Pläne ausprobieren, bei denen die kostenlose Testversion aktiviert ist, und zwischen diesen wechseln.  Die Umwandlung in ein kostenpflichtiges Abonnement erfolgt automatisch am Ende der Laufzeit.

>[!Note]
>Wenn sich der Kunde für die Umwandlung in einen Plan ohne kostenlose Testversionen entscheidet, wird dies umgesetzt, doch geht die kostenlose Testversion sofort verloren.  Auch sobald ein Kunde mit dem Bezahlen für einen Plan beginnt, kann er selbst dann keine kostenlose Testversion mehr für dasselbe Abonnement erhalten, wenn eine Umwandlung in eine SKU erfolgt, die kostenlose Testversionen unterstützt.

Die Möglichkeit, eine kostenlose Testversion zu konfigurieren, steht für jeden Plan in Ihrem Angebot zur Verfügung. Navigieren Sie einfach zu „Preise und Verfügbarkeit“ für jedes Angebot, und aktivieren Sie das Kontrollkästchen, um eine einmonatige Testversion zuzulassen.

![Kontrollkästchen für eine einmonatige kostenlose Testversion](./media/free-trial-enable.png)

>[!Note]
>Sobald Ihr transaktionsfähiges Angebot mit einer kostenlosen Testversion veröffentlicht wurde, kann es für diesen Plan nicht mehr deaktiviert werden. Vergewissern Sie sich, dass diese Einstellung für die erste Veröffentlichung korrekt ist, damit der Plan nicht neu erstellt werden muss.

Wenn Sie Informationen zu Kundenabonnements abrufen möchten, die derzeit an einer kostenlosen Testversion teilnehmen, verwenden Sie die neue API-Eigenschaft `isFreeTrial`, die als „true“ oder „false“ gekennzeichnet ist. Weitere Informationen finden Sie unter der [API zum Abrufen eines Saas-Abonnements](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2#get-subscription).

>[!Note]
>Kostenlose Testversionen werden für Pläne, die den Marketplace-Messungsdienst nutzen, nicht unterstützt.

#### <a name="markets"></a>Märkte

- **Edit markets** (Märkte bearbeiten) (optional)

Jeder Plan muss in mindestens einem Markt verfügbar sein. Aktivieren Sie das Kontrollkästchen für jeden Marktstandort, in dem Sie diesen Plan zur Verfügung stellen möchten. Zur Unterstützung sind ein Suchfeld und eine Schaltfläche zum Auswählen von Ländern vorhanden, in denen die Umsatz- und Gebrauchssteuer von Microsoft für Sie überwiesen wird. 


Wenn Sie bereits Preise in US-Dollar für Ihren Plan festgelegt haben und einen anderen Marktstandort hinzufügen, wird der Preis für den neuen Markt entsprechend dem aktuellen Wechselkurs berechnet. Sie sollten vor dem Veröffentlichen immer den Preis für jeden Markt überprüfen. Die Preise können nach dem Speichern der Änderungen mithilfe des Links „Export prices (xlsx)“ (Preise exportieren (.xlsx)) überprüft werden.

#### <a name="pricing"></a>Preise

- **Preismodell**: „Flat rate“ (Pauschalgebühr) oder „Seat based“ (Arbeitsplatzbasiert)

**Flat rate:** (Pauschalgebühr): Ermöglichen Sie den Zugriff auf Ihr Angebot mit einer einzelnen monatlichen oder jährlichen Pauschalgebühr. Dies wird manchmal als arbeitsplatzbasierter Preis bezeichnet. Dieses Preismodell ermöglicht Ihnen das optionale Definieren von Volumentarifen, bei denen die Marketplace-Messungsdienst-API zur Berechnung für Kunden nach nicht standardmäßigen Einheiten verwendet wird.  Weitere Informationen zur getakteten Abrechnung finden Sie unter [Getaktete Abrechnung mit dem Marketplace-Messungsdienst](./saas-metered-billing.md).

**Pro Benutzer:** Ermöglichen Sie den Zugriff auf Ihr Angebot mit einem Preis, der auf der Anzahl der Benutzer, die auf Ihr Angebot zugreifen, oder auf der Anzahl der belegten Arbeitsplätze basiert. Mithilfe des benutzerbasierten Modells können Sie die minimal und maximal zulässige Anzahl von Benutzern auf Grundlage des Preises festlegen. Auf diese Weise können durch das Konfigurieren mehrerer Pläne unterschiedliche Preise auf Grundlage der Anzahl der Benutzer festgelegt werden.  Diese Felder sind optional. Wenn keine Auswahl erfolgt, wird angenommen, dass die Anzahl der Arbeitsplätze unbegrenzt ist (mindestens 1 und maximal die vom System unterstützte Anzahl). Diese Felder können beim Aktualisieren des Plans bearbeitet werden.

Nach der Veröffentlichung kann das Abrechnungspreismodell nicht mehr geändert werden. Darüber hinaus müssen alle Pläne für das gleiche Angebot dasselbe Preismodell aufweisen.

- **Abrechnungszeitraum**: „Monatlich“ oder „Jährlich“

Wählen Sie aus, wie häufig Kunden den aufgeführten Preis zahlen müssen. Es muss mindestens ein monatlicher oder jährlicher Preis angegeben werden. Es ist auch möglich, für Kunden beide Optionen verfügbar zu machen.

- **Preis**: „USD per month“ (US-Dollar pro Monat) oder „USD per year“ (US-Dollar pro Jahr)

Preise in der lokalen Währung (USD = US-Dollar) werden während der Einrichtung gemäß dem aktuellen Wechselkurs in die lokale Währung aller ausgewählten Märkte konvertiert. Überprüfen Sie vor der Veröffentlichung die Preise in jedem Markt, indem Sie die Preistabellenkalkulation exportieren. Wenn Sie benutzerdefinierte Preise in einem einzelnen Markt festlegen möchten, ändern und importieren Sie die Preistabellenkalkulation. Sie sind für die Überprüfung des Preismodells und diese Einstellungen verantwortlich.
* *Sie müssen Ihre Preisänderungen speichern, damit die Preisdaten exportiert werden können.*

Überprüfen Sie Ihre Preise vor der Veröffentlichung sorgfältig, da nicht alle Elemente eines Plans nach seiner Veröffentlichung geändert werden können:

- Das Preismodell kann nach dem Veröffentlichen eines Plans nicht geändert werden.
- Nachdem ein Abrechnungszeitraum für einen Plan veröffentlicht wurde, kann er nicht mehr entfernt werden.
- Nachdem ein Preis für einen Markt in Ihrem Plan veröffentlicht wurde, kann er nicht mehr geändert werden.

### <a name="plan-audience"></a>Plan Audience (Planzielgruppe)

Sie können festlegen, dass jeder Plan für alle Benutzer oder nur für eine von Ihnen ausgewählte Zielgruppe angezeigt wird. Sie können mithilfe von Azure AD-Mandanten-IDs die Mitgliedschaft in dieser eingeschränkten Zielgruppe zuweisen.

#### <a name="privacy"></a>Datenschutz

- **This is a private plan** (Dies ist ein privater Plan) (optionales Kontrollkästchen)

Aktivieren Sie dieses Kontrollkästchen, um den Plan als privaten Plan festzulegen und nur für die von Ihnen ausgewählte eingeschränkte Zielgruppe anzuzeigen. Nachdem Sie den Plan als privaten Plan veröffentlicht haben, können Sie die Zielgruppe aktualisieren oder den Plan für alle Benutzer verfügbar machen. Nachdem ein Plan als für alle Benutzer sichtbar veröffentlicht wurde, muss er für alle Benutzer sichtbar bleiben. (Der Plan kann nicht wieder als privater Plan konfiguriert werden).

- **Restricted Audience (Tenant IDs)** (Eingeschränkte Zielgruppe (Mandanten-IDs))

Weisen Sie die Zielgruppe zu, die Zugriff auf den privaten Plan hat. Der Zugriff wird mit Mandanten-IDs zugewiesen. Dabei kann eine Beschreibung jeder zugewiesenen Mandanten-ID angegeben werden. Es können maximal 10 Mandanten-IDs oder, wenn eine CSV-Tabellenkalkulationsdatei importiert wird, 20.000 Mandanten-IDs von Kunden hinzugefügt werden.

Ein Mandant stellt eine Organisation dar, wobei die ID als global eindeutiger Bezeichner (Globally Unique Identifier, GUID – eine 128-Bit-Ganzzahl zum Identifizieren von Ressourcen) dargestellt wird. Es handelt sich um eine dedizierte Instanz von Azure AD, die ein Unternehmen oder ein App-Entwickler erhält, wenn das Unternehmen oder der App-Entwickler eine Beziehung zu Microsoft eingeht (z.B. die Registrierung für Azure, Microsoft Intune oder Microsoft 365). Jeder Azure AD-Mandant ist eindeutig und von anderen Azure AD-Mandanten getrennt. Um den Mandanten zu überprüfen, melden Sie sich beim Azure-Portal mit dem Konto an, das Sie für die Verwaltung Ihrer Anwendung verwenden möchten. Falls Sie einen Mandanten besitzen, werden Sie automatisch angemeldet, und der Mandantenname wird direkt unter dem Kontonamen angezeigt. Zeigen Sie rechts oben im Azure-Portal auf Ihren Kontonamen, um Ihren Namen, Ihre E-Mail-Adresse, Ihr Verzeichnis und Ihre Mandanten-ID (GUID) sowie Ihre Domäne anzuzeigen. Falls Ihr Konto mehreren Mandanten zugeordnet ist, können Sie durch Klicken auf Ihren Kontonamen ein Menü öffnen, über das Sie zwischen Mandanten wechseln können. Jeder Mandant besitzt eine eigene Mandanten-ID. Sie können auch unter [ https://www.whatismytenantid.com ](https://www.whatismytenantid.com) die Mandanten-ID Ihrer Organisation mithilfe einer Domänennamen-URL nachschlagen.

In SaaS-Angeboten werden private Zielgruppen mithilfe von Mandanten-IDs definiert, in anderen Angebotstypen können jedoch Azure-Abonnement-IDs (die auch als GUIDs dargestellt werden) verwendet werden.

> [!NOTE]
> Eine private Zielgruppe (oder eingeschränkte Zielgruppe) unterscheidet sich von einer Vorschauzielgruppe. Sie können auf der Registerkarte **[Vorschau](#preview)** eine Vorschauzielgruppe definieren. Einer Vorschauzielgruppe wird Zugriff auf das Angebot gewährt, *bevor* es im Marketplace live veröffentlicht wird. Die Festlegung als private Zielgruppe gilt nur für einen bestimmten Plan. Für die private Zielgruppe können alle Pläne (private und nicht private Pläne) angezeigt werden, jedoch nur während des eingeschränkten Vorschauzeitraums, in dem der Plan getestet und überprüft wird.

## <a name="example-list-of-plans-within-a-marketplace-offer"></a>Beispielliste der Pläne innerhalb eines Marketplace-Angebots

![Liste der Marketplace-Beispielpläne mit Hinweisen](./media/marketplace-plan.svg)

## <a name="test-drive"></a>Testversion

[!INCLUDE [Test drive content](./includes/commercial-marketplace-test-drive.md)]

## <a name="publish"></a>Veröffentlichen

#### <a name="submit-offer-to-preview"></a>Übermitteln des Angebots für die Vorschau

Nachdem Sie alle erforderlichen Abschnitte des Angebots abgeschlossen haben, klicken Sie rechts oben im Portal auf **Veröffentlichen**. Sie werden zur Seite **Review and publish** (Überprüfen und veröffentlichen) weitergeleitet. 

Wenn Sie das Angebot zum ersten Mal veröffentlichen, haben Sie folgende Möglichkeiten:

- Anzeigen des Abschlussstatus für die einzelnen Abschnitte des Angebots.
    - *Nicht gestartet*: Der Abschnitt wurde nicht bearbeitet und muss abgeschlossen werden.
    - *Unvollständig*: Der Abschnitt enthält Fehler, die behoben werden müssen, oder erfordert eine Ergänzung der Informationen. Kehren Sie zu dem/den Abschnitten zurück, und nehmen Sie eine Aktualisierung vor.
    - *Abgeschlossen*: Der Abschnitt ist abgeschlossen. Alle erforderlichen Daten wurden angegeben, und es sind keine Fehler vorhanden. Alle Abschnitte des Angebots müssen abgeschlossen sein, bevor Sie das Angebot einreichen können.
- Geben Sie dem Zertifizierungsteam Testanweisungen, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird, sowie ergänzende Hinweise, die das Verständnis Ihrer App erleichtern.
- Senden Sie dazu das Angebot zur Veröffentlichung, indem Sie auf **Senden** klicken. Wir senden Ihnen eine E-Mail, um Ihnen mitzuteilen, wann eine Vorschauversion des Angebots verfügbar ist, damit Sie es überprüfen und genehmigen können. Sie müssen zum Partner Center zurückkehren und **Live schalten** auswählen, um das Angebot für eine öffentliche Zielgruppe (oder bei einem privaten Angebot für eine private Zielgruppe) zu veröffentlichen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./update-existing-offer.md)
