---
title: Erstellen Sie eines neuen Dynamics 365 for Operations-Angebots im kommerziellen Marketplace
description: Erstellen eines neuen Dynamics 365 for Operations-Angebots (Software-as-a-Service) für das Auflisten oder Verkaufen in Azure Marketplace, AppSource oder über das CSP-Programm (Cloud Solution Provider) mit dem kommerziellen Marketplace-Portal im Microsoft Partner Center.
author: JnHs
manager: evansma
ms.author: jenhayes
ms.service: marketplace
ms.topic: conceptual
ms.date: 10/04/2019
ms.openlocfilehash: 4a8a8d7f6004ac32aeb7ce2b8cbffeca9ddf0079
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "72595535"
---
# <a name="create-a-new-dynamics-365-for-operations-offer"></a>Erstellen eines neuen Angebots für Dynamics 365 for Operations

In diesem Thema wird erläutert, wie Sie ein neues Angebot für Dynamics 365 for Operations erstellen. [Microsoft Dynamics 365 for Finance and Operations](https://dynamics.microsoft.com/finance-and-operations) ist ein ERP-Dienst (Enterprise Resource Planning), der erweiterte Features für Finanzierung, operative Vorgänge, Fertigung und Lieferkettenmanagement unterstützt. Alle Angebote für Dynamics 365 for Operations müssen unseren Zertifizierungsprozess durchlaufen.

Um mit dem Erstellen von Dynamics 365 for Operations-Angeboten zu beginnen, müssen Sie zunächst [ein Partner Center-Konto erstellen](./create-account.md), das [Dashboard „Kommerzieller Marketplace“](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) öffnen, und darin die Seite **Übersicht** auswählen.

![Dashboard „Kommerzieller Marketplace“ im Partner Center](./media/new-offer-overview.png)

>[!Note]
> Nach der Veröffentlichung eines Angebots werden in Partner Center am Angebot vorgenommene Änderungen erst nach einer erneuten Veröffentlichung im System und den Store Fronts aktualisiert. Sie müssen das Angebot nach dem Vornehmen von Änderungen für die Veröffentlichung einreichen.


## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

Wählen Sie die Schaltfläche **+ Neues Angebot** und dann das Menüelement **Dynamics 365 for Operations** aus. Das Dialogfeld **Neues Angebot** wird angezeigt.

### <a name="offer-id-and-alias"></a>Angebots-ID und Angebotsalias

- **Angebots-ID:** Eindeutiger Bezeichner für jedes Angebot in Ihrem Konto. Diese ID wird für Kunden in der URL-Adresse für das Marketplace-Angebot und ggf. in Azure Resource Manager-Vorlagen angezeigt. Die Angebots-ID muss aus alphanumerischen Zeichen bestehen und darf keine Großbuchstaben enthalten (Bindestriche und Unterstriche sind zulässig, Leerzeichen jedoch nicht). Sie ist auf 50 Zeichen beschränkt und kann nach der Auswahl von **Erstellen** nicht mehr geändert werden.  Wenn Sie hier z.B. *test-offer-1* eingeben, lautet die Angebots-URL `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.

- **Angebotsalias**: Der Name, der zum Verweisen auf das Angebot im Partner Center verwendet wird. Dieser Name wird im Marketplace nicht verwendet und unterscheidet sich vom Angebotsnamen und anderen Werten, die den Kunden angezeigt werden. Dieser Wert kann nach der Auswahl von **Erstellen** nicht mehr geändert werden.

Nachdem Sie Ihre **Angebots-ID** und ihren **Angebotsalias** eingegeben haben, wählen Sie **Erstellen** aus. Sie können dann alle anderen Teile Ihres Angebots bearbeiten.

## <a name="offer-setup"></a>Angebotseinrichtung

Auf der Seite **Angebotseinrichtung** wird um die folgenden Informationen gebeten. Vergessen Sie nicht, **Speichern** auszuwählen, nachdem Sie die Angaben in diesen Feldern vorgenommen haben.

### <a name="how-do-you-want-potential-customers-to-interact-with-this-listing-offer"></a>Wie sollen potenzielle Kunden mit diesem Angebot interagieren?

Wählen Sie die Option aus, die Sie für dieses Angebot verwenden möchten.

#### <a name="get-it-now-free"></a>Jetzt abrufen (kostenlos)

Listen Sie Ihr Angebot für Kunden als kostenlos, indem Sie eine gültige URL (beginnend mit *http* oder *https*) angeben, mit der die Kunden auf Ihre App zugreifen können.  Beispiel: `https://contoso.com/my-app`

#### <a name="free-trial-listing"></a>Kostenlose Testversion (Listing)

Listen Sie Ihr Angebot für Kunden mit einem Link zu einer kostenlosen Testversion auf, indem Sie eine gültige URL (mit *http* oder *https* beginnend) angeben, über die auf die Testversion zugegriffen werden kann.  Beispiel: `https://contoso.com/trial/my-app`. Kostenlose Testversionen in der Angebotsliste werden von Ihrem Dienst erstellt, verwaltet und konfiguriert und weisen keine von Microsoft verwalteten Abonnements auf.

> [!NOTE]
> Die Token, die Ihre Anwendung über den Testlink erhält, können nur verwendet werden, um zum Automatisieren der Kontoerstellung in Ihrer App Benutzerinformationen aus Azure Active Directory (Azure AD) abzurufen. Die Authentifizierung mit diesem Token wird für Microsoft-Konten nicht unterstützt.

#### <a name="contact-me"></a>Kontakt mit mir aufnehmen

Sammeln Sie über Ihr CRM-System (Customer Relationship Management) Kundenkontaktinformationen. Der Kunde wird gebeten, die Berechtigung zur Freigabe seiner Informationen zu erteilen. Diese Kundeninformationen sowie der Angebotsname, die Angebots-ID und der Marketplace, auf dem der Kunde Ihr Angebot gefunden hat, werden an das CRM-System gesendet, das Sie konfiguriert haben. Weitere Informationen zum Konfigurieren des CRM-Systems finden Sie unter [Einbinden der Leadverwaltung](#connect-lead-management). 

### <a name="test-drive"></a>Testversion

Die Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen die Möglichkeit geben, vor dem Kauf einen Test durchzuführen, was zu einer höheren Konvertierung und der Generierung von aussichtsreichen Leads führt. [Erfahren Sie mehr über Testversionen.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

Aktivieren Sie das Kontrollkästchen **Testversion aktivieren**, um eine Testversion zu aktivieren. Anschließend müssen Sie in der [technischen Konfiguration der Testversion](#test-drive-technical-configuration) eine Demo-Umgebung konfigurieren, damit Kunden Ihr Angebot für einen bestimmten Zeitraum testen können. 

#### <a name="type-of-test-drive"></a>Typ der Testversion

Sie können zwischen folgenden Optionen wählen:

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

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Weitere Informationen finden Sie in der [Übersicht über die Leadverwaltung](./commercial-marketplace-get-customer-leads.md).

Klicken Sie auf **Speichern**, bevor Sie mit dem nächsten Abschnitt fortfahren!

## <a name="properties"></a>Properties

Auf der **Eigenschaftenseite** können Sie die Kategorien und Branchen, die zum Gruppieren Ihres Angebots im Marketplace verwendet werden, die Version Ihrer App und die Verträge für Ihr Angebot definieren. Wählen Sie **Speichern** aus, nachdem Sie diese Seite ausgefüllt haben.

### <a name="category"></a>Category

Wählen Sie mindestens eine und höchstens drei Kategorien aus. Diese werden verwendet, um Ihr Angebot in die entsprechenden Marketplace-Suchbereiche zu platzieren. Vergessen Sie nicht, in der Angebotsbeschreibung anzugeben, wie Ihr Angebot diesen Kategorien entspricht. 

### <a name="industry"></a>Branche

Sie können optional bis zu zwei Branchen auswählen, um Ihr Angebot im Marketplace zu kategorisieren. Wenn Ihr Angebot nicht für eine bestimmte Branche bestimmt ist, lassen Sie diesen Abschnitt leer. Vergessen Sie nicht, in der Angebotsbeschreibung anzugeben, wie sich Ihr Angebot für die ausgewählten Branchen eignet. 

### <a name="app-version"></a>App-Version

Geben Sie die Versionsnummer Ihres Angebots ein. Für Kunden wird diese Version auf der Detailseite des Angebots aufgeführt.

### <a name="standard-contract"></a>Standardvertrag

Zur Vereinfachung des Beschaffungsprozesses für Kunden und zur Verringerung der rechtlichen Komplexität für Softwareanbieter stellt Microsoft die Vorlage „Standardvertrag“ bereit, um eine Transaktion im Marketplace zu erleichtern.

Statt benutzerdefinierte Geschäftsbedingungen zu erstellen, können Sie Ihre Software wahlweise unter dem Standardvertrag anbieten, den Kunden nur einmal überprüfen und akzeptieren müssen.

Der Standardvertrag ist hier zu finden: https://go.microsoft.com/fwlink/?linkid=2041178

Aktivieren Sie das Kontrollkästchen **Standardvertrag verwenden?** , um den Standardvertrag zu verwenden.

#### <a name="terms-of-use"></a>Nutzungsbedingungen

Wenn Sie das Kontrollkästchen **Standardvertrag verwenden?** nicht aktivieren, müssen Sie Ihre eigenen rechtlichen Bedingungen im Feld **Nutzungsbedingungen** angeben. Geben Sie bis zu 10.000 Textzeichen oder, wenn Ihre Nutzungsbedingungen eine längere Beschreibung erfordern, eine URL ein, über den Ihre zusätzlichen Lizenzbedingungen aufgerufen werden können. Kunden müssen diese Bedingungen akzeptieren, bevor sie Ihre App testen können.

## <a name="offer-listing"></a>Angebotsliste

Auf der Seite „Angebotsliste“ werden die Sprachen angezeigt, in denen Ihr Angebot aufgeführt wird. Beachten Sie, dass **Englisch (Vereinigte Staaten)** derzeit die einzige verfügbare Option ist.

Sie müssen für jede Sprache/jeden Markt Marketplace-Informationen (Angebotsname, Beschreibung, Bilder usw.) definieren. Wählen Sie die Sprache bzw. den Namen des Markts aus, um diese Informationen anzugeben.

> [!NOTE]
> Die Inhalte der Angebotsliste (z.B. Beschreibung, Dokumente, Screenshots, Nutzungsbedingungen usw.) müssen nicht in englischer Sprache vorliegen, solange die Angebotsbeschreibung mit dem folgenden Satz beginnt: „Diese Anwendung ist nur in [nicht englische Sprache] verfügbar.“ Es ist auch zulässig, eine *Nützlicher Link-URL* bereitzustellen, um Inhalte in einer anderen Sprache als der in der Angebotsliste verwendeten anzubieten.

### <a name="name"></a>NAME

Der hier eingegebene Name wird Kunden als Titel Ihrer Angebotsliste angezeigt. Dieses Feld ist bereits mit dem Text aufgefüllt, den Sie beim Erstellen des Angebots als **Angebotsalias** eingegeben haben. Sie können diesen Wert jedoch ändern. Dieser Name kann markenrechtlich geschützt sein (und Sie können Marken- oder Copyrightsymbole einschließen). Der Name darf nicht mehr als 50 Zeichen umfassen und keine Emojis enthalten.

### <a name="short-description"></a>Kurzbeschreibung

Geben Sie eine kurze Beschreibung Ihres Angebots an (bis zu 100 Zeichen). Diese kann in Marketplace-Suchergebnissen verwendet werden.

### <a name="description"></a>BESCHREIBUNG

Geben Sie eine längere Beschreibung Ihres Angebots an (bis zu 3.000 Zeichen). Diese Beschreibung wird Kunden in der Übersicht der Marketplace-Auflistung angezeigt. Geben Sie z.B ein Wertversprechen für Ihr Angebot, wichtige Vorteile, Kategorie- oder Branchenzuordnungen, Möglichkeiten für In-App-Käufe sowie alle erforderlichen Veröffentlichungen ein. 

Einige Tipps zum Verfassen Ihrer Beschreibung:  

- Schildern Sie in den ersten Sätzen Ihrer Beschreibung deutlich das Nutzenversprechen Ihres Angebots. Fügen Sie Folgendes in ihr Wertversprechen ein:
  - Beschreibung des Produkts
  - Benutzertyp, der von dem Produkt profitiert
  - Anforderungen oder Probleme der Kunden, die das Produkt erfüllt bzw. behebt
- Beachten Sie, dass die ersten Sätze in Suchmaschinenergebnissen angezeigt werden können.  
- Setzen Sie sich nicht nur auf Features und Funktionen, um Ihr Produkt zu verkaufen. Konzentrieren Sie stattdessen auf den Nutzen, den Sie bieten.  
- Verwenden Sie möglichst viel branchenspezifisches Vokabular oder eine nutzenorientierte Formulierung. 
- Verwenden Sie ggf. HTML-Tags, um Ihre Beschreibung zu formatieren und Sie ansprechender zu gestalten.

### <a name="search-keywords"></a>Suchbegriffe

Sie können optional bis zu drei Suchbegriffe eingeben, die Kunden helfen, Ihr Angebot im Marketplace zu finden. Um optimale Ergebnisse zu erzielen, sollten Sie diese Schlüsselwörter auch in ihrer Beschreibung verwenden.

### <a name="products-your-app-works-with"></a>Produkte, mit denen Ihre App zusammenarbeitet

Wenn Sie den Kunden mitteilen möchten, dass Ihre App mit bestimmten Produkten zusammenarbeitet, geben Sie hier bis zu drei Produktnamen ein.

### <a name="support-urls"></a>Support-URLs

In diesem Abschnitt können Sie Links bereitstellen, die Kunden dabei helfen, mehr über Ihr Angebot zu erfahren.

#### <a name="help-link"></a>Hilfelink

Geben Sie die URL ein, unter der Kunden mehr über Ihr Angebot erfahren können.

#### <a name="privacy-policy-url"></a>URL zur Datenschutzrichtlinie

Geben Sie die URL zur Datenschutzrichtlinie Ihrer Organisation ein. Sie müssen sicherzustellen, dass die App die Datenschutzgesetze und -bestimmungen erfüllt, und Sie müssen eine gültige Datenschutzrichtlinie bereitstellen.

### <a name="contacts"></a>Kontakte

In diesem Abschnitt müssen Sie den Namen, die E-Mail-Adresse und die Telefonnummer für einen **Supportkontakt** und einen **Engineering-Kontakt** angeben. Diese Informationen werden Kunden nicht angezeigt, sind aber für Microsoft verfügbar und können CSP-Partnern bereitgestellt werden.

Im Abschnitt **Supportkontakt** müssen Sie auch die **Support-URL** angeben, unter der CSP-Partner Support für Ihr Angebot erhalten.

### <a name="supporting-documents"></a>Unterstützende Dokumente

Hier müssen Sie mindestens ein und höchstens zugehörige Marketingdokumente angeben, z.B. Whitepaper, Broschüren, Checklisten oder Präsentationen. Diese Dokumente müssen im PDF-Format vorliegen.

### <a name="marketplace-images"></a>Marketplace-Bilder

In diesem Abschnitt können Sie Logos und Bilder bereitstellen, die beim Anzeigen Ihres Angebots für Kunden verwendet werden. Alle Bilder müssen das PNG-Format aufweisen.

#### <a name="store-logos"></a>Store-Logos

Sie müssen das Logo Ihres Angebots in zwei Größen angeben: **Klein (48 x 48)** und **Groß (216 x 216)** .

#### <a name="hero"></a>Hero

Das Herobild ist optional. Wenn Sie ein Herobild angeben, muss es in der Größe 815 x 290 Pixel vorliegen.

#### <a name="screenshots"></a>Screenshots

Fügen Sie Screenshots hinzu, die zeigen, wie Ihr Angebot funktioniert. Mindestens ein Screenshot ist erforderlich, und Sie können bis zu fünf hinzufügen. Alle Screenshots müssen in der Größe 1280 x 720 Pixel vorliegen.

#### <a name="videos"></a>Videos

Optional können Sie bis zu vier Videos hinzufügen, die Ihr Angebot veranschaulichen. Diese Videos sollten auf YouTube und/oder Vimeo gehostet werden. Geben Sie für jedes Video den Namen, seine URL und ein Miniaturbild (1280 x 720 Pixel) ein.

#### <a name="additional-marketplace-listing-resources"></a>Zusätzliche Ressourcen für Marketplace-Listen

- [Bewährte Methoden für Angebotslistung](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="availability"></a>Verfügbarkeit

Auf der Seite **Verfügbarkeit** finden Sie Optionen, wo und wie Sie Ihr Angebot verfügbar machen.

### <a name="markets"></a>Märkte

In diesem Abschnitt können Sie angeben, in welchen Märkten das Angebot verfügbar sein soll. Wählen Sie hierzu die Option **Märkte bearbeiten** aus. Dadurch wird das Popupfenster für die **Marktauswahl** angezeigt.

Standardmäßig sind keine Märkte ausgewählt, Sie müssen jedoch mindestens einen Markt auswählen, um Ihr Angebot zu veröffentlichen. Klicken Sie auf **Alle auswählen**, um Ihr Angebot in allen möglichen Märkten verfügbar zu machen, oder wählen Sie die gewünschten Märkte aus, die Sie hinzufügen möchten. Wenn Sie fertig sind, wählen Sie **Speichern** aus.

Beachten Sie, dass Ihre hier getroffene Auswahl nur für neue Käufe gilt. Wird Ihre App bereits von jemandem in einem bestimmten Markt verwendet, und entfernen Sie den Markt später, so können Benutzer, die in diesem Markt bereits über Ihr Angebot verfügen, es weiterhin nutzen, aber Ihr Angebot ist nicht für neue Kunden in diesem Markt erhältlich.

> [!IMPORTANT]
> Es liegt in ihrer Verantwortung, alle lokalen rechtlichen Anforderungen zu erfüllen, auch wenn diese Anforderungen nicht hier oder im Partner Center aufgeführt sind.

Beachten Sie Folgendes: Auch wenn Sie „Alle Märkte“ auswählen, kann es sein, dass bestimmte Angebote in einigen Ländern und Regionen aufgrund lokaler Gesetze und Einschränkungen oder anderer Faktoren nicht gelistet werden.

### <a name="preview-audience"></a>Vorschauzielgruppe

Bevor Sie Ihr Angebot im Marketplace für die breite Öffentlichkeit live schalten, müssen Sie es zunächst für eine begrenzte **Vorschauzielgruppe** verfügbar machen. Geben Sie hier einen **Hide-Schlüssel** (eine beliebige Zeichenfolge, die nur Kleinbuchstaben und/oder Ziffern enthält) ein. Mitglieder Ihrer Vorschauzielgruppe können diesen Hide-Schlüssel als Token verwenden, um eine Vorschau Ihres Angebots im Marketplace anzuzeigen.

Wenn Sie anschließend bereit sind, Ihr Angebot verfügbar zu machen und die Vorschaueinschränkung zu entfernen, müssen Sie den **Hide-Schlüssel** entfernen und das Angebot erneut veröffentlichen.

## <a name="technical-configuration"></a>Technische Konfiguration

Auf der Seite **Technische Konfiguration** werden die technischen Informationen definiert, die für die Verbindung zu Ihrem Angebot verwendet werden. Durch diese Verbindung können wir Ihr Angebot für den Endkunden bereitstellen, wenn er es kaufen möchte.

### <a name="solution-identifier"></a>Lösungsbezeichner

Geben Sie den Lösungsbezeichner (GUID) für Ihre Lösung an.

So finden Sie Ihren Lösungsbezeichner:
1. Wählen Sie in Microsoft Dynamics Lifecycle Services (LCS) die Option **Lösungsverwaltung** aus.
2. Wählen Sie Ihre Lösung aus, und suchen Sie in der **Paketübersicht** nach dem **Lösungsbezeichner**. Wenn der Bezeichner leer ist, wählen Sie **Bearbeiten** aus, veröffentlichen Sie Ihr Paket erneut, und versuchen Sie es dann noch einmal.

### <a name="release-version"></a>Releaseversion

Wählen Sie die Version von Dynamics 365 for Finance and Operations aus, mit der diese Lösung verwendet werden kann.

## <a name="test-drive-technical-configuration"></a>Technische Konfiguration der Testversion

Wenn Sie auf der Seite zum[ Einrichten des Angebots](#offer-setup) die Option **Testversion aktivieren** ausgewählt haben, müssen Sie hier Details angeben, damit Kunden eine Testversion Ihres Angebots abrufen können.

Auf der Seite **Testversion** können Sie eine Demonstration (oder „Testversion“) einrichten, sodass Kunden Ihr Angebot testen können, bevor sie sich zum Kauf entschließen. Erfahren Sie mehr im Artikel [Was ist die Testversion?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive). Wenn Sie nicht mehr eine Testversion für Ihr Angebot bereitstellen möchten, kehren Sie zur Seite **[Angebotseinrichtung](#offer-setup)** zurück, und deaktivieren Sie **Testversion aktivieren**.

Die folgenden Typen von Testversionen sind verfügbar, jeweils mit eigenen Anforderungen an die technische Konfiguration.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Logik-App](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (keine technische Konfiguration erforderlich)

### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Technical configuration for Azure Resource Manager test drive (Technische Konfiguration für Azure Resource Manager-Testversion)

Eine Bereitstellungsvorlage, die alle Azure-Ressourcen enthält, aus denen Ihre Lösung besteht. In den für dieses Szenario geeigneten Produkten werden nur Azure-Ressourcen verwendet. Erfahren Sie mehr über das Einrichten einer [Azure Resource Manager-Testversion](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

- **Regionen** (erforderlich): Es gibt derzeit 26 von Azure unterstützte Regionen, in denen Ihre Testversion zur Verfügung gestellt werden kann. In der Regel sollten Sie Ihre Testversion in den Regionen zur Verfügung stellen, in denen Sie die größte Anzahl von Kunden erwarten, damit sie für eine optimale Leistung die nächstgelegene Region auswählen können. Sie müssen sicherstellen, dass in Ihrem Abonnement alle Ressourcen in jeder von Ihnen ausgewählten Region bereitgestellt werden können.

- **Instances**: Wählen Sie den Typ („Heiß“ oder „Kalt“) und die Anzahl der verfügbaren Instanzen aus. Diese wird mit der Anzahl der Regionen multipliziert, in denen Ihr Angebot verfügbar ist.

**Hot**: Dieser Typ von Instanz wird bereits bereitgestellt, und auf ihn kann in jeder ausgewählten Region zugegriffen werden. Die Kunden haben sofort Zugriff auf Instanzen vom Typ *Heiß* und müssen nicht auf die Bereitstellung warten. Der Nachteil dabei ist, dass diese Instanzen immer unter Ihrem Azure-Abonnement ausgeführt werden, sodass höhere Betriebszeitkosten anfallen. Es wird dringend empfohlen, mindestens eine Instanz des Typs *Heiß* festzulegen, da die meisten Kunden nicht auf eine vollständige Bereitstellung warten möchten. Wenn keine Instanz vom Typ *Heiß* verfügbar ist, nimmt die Verwendung durch die Kunden ab.

**Kalt**: Dieser Typ von Instanz stellt die Gesamtzahl der Instanzen dar, die pro Region bereitgestellt werden können. Bei Instanzen des Typs „Kalt“ muss die Bereitstellung der gesamten Resource Manager-Vorlage für die Testversion durchgeführt werden, wenn der Kunde die Testversion anfordert. Daher dauert das Laden von Instanzen des Typs *Kalt* weitaus länger als das Laden von Instanzen des Typs *Heiß*. Sie müssen jedoch nur für die Dauer der Testversion bezahlen. Instanzen vom Typ „Kalt“ werden *nicht* wie Instanzen vom Typ *Heiß* stets im Azure-Abonnement ausgeführt.

- **Test drive Azure Resource Manager template** (Vorlage für Azure Resource Manager-Testversion): Laden Sie die ZIP-Datei hoch, die die Azure Resource Manager-Vorlage enthält.  Im Schnellstartartikel [Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal) erhalten Sie weitere Informationen zum Erstellen einer Azure Resource Manager-Vorlage.

- **Dauer der Testversion** (erforderlich): Geben Sie den Zeitraum, in dem die Testversion aktiv bleibt, als Anzahl von Stunden ein. Nach Ablauf dieses Zeitraums wird die Testversion automatisch beendet. Dies Dauer kann nur als ganze Zahl von Stunden festgelegt werden (Beispiel: „2“ ist zulässig, „1,5“ ist unzulässig).

### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Technical configuration for Dynamics 365 test drive (Technische Konfiguration für Dynamics 365-Testversion)

Microsoft kann mit diesem Typ von Testversion die Dienstbereitstellung hosten und verwalten, um das Einrichten einer Testversion zu vereinfachen. Die Konfiguration für diesen Typ von gehosteter Testversion ist gleich, unabhängig davon ob die Testversion für Business Central, Customer Engagement oder Operations vorgesehen ist.

- **Maximale Anzahl gleichzeitiger Testversionen** (erforderlich): Legen Sie die maximale Anzahl von Kunden fest, die Ihre Testversion gleichzeitig verwenden können. Jeder gleichzeitige Benutzer nutzt eine Dynamics 365-Lizenz, während die Testversion aktiv ist. Daher müssen Sie sicherstellen, dass genügend Lizenzen für die festgelegte maximale Anzahl verfügbar sind. Empfohlener Wert liegt zwischen 3 und 5.

- **Dauer der Testversion** (erforderlich): Geben Sie den Zeitraum ein, in dem die Testversion aktiv bleibt, indem Sie die Anzahl der Stunden festlegen. Nach Ablauf dieser Anzahl von Stunden wird die Sitzung beendet, und sie belegt nicht mehr eine Ihrer Lizenzen. Der empfohlene Wert beträgt 2 bis 24 Stunden, abhängig von der Komplexität Ihres Angebots. Dies Dauer kann nur als ganze Zahl von Stunden festgelegt werden (Beispiel: „2“ ist zulässig, „1,5“ ist unzulässig).  Der Benutzer kann eine neue Sitzung anfordern, wenn die Zeit abgelaufen ist und er erneut auf die Testversion zugreifen möchte.

- **Instanz-URL** (erforderlich): Die URL, über die der Kunde die Testversion startet. Dies ist in der Regel die URL Ihrer Dynamics 365-Instanz, auf der Ihre App mit installierten Beispieldaten ausgeführt wird (z.B. https://testdrive.crm.dynamics.com) ).

- **Web-API-URL der Instanz** (erforderlich): Rufen Sie die Web-API-URL für Ihre Dynamics 365-Instanz ab, indem Sie sich bei Ihrem Microsoft 365-Konto anmelden, zu **Einstellungen** \&gt; **Anpassung** \&gt; **Entwicklerressourcen** \&gt; **Instance Web API (Service Root URL)** (Instanz-Web-API (Stamm-URL des Diensts)) navigieren und die hier angegebene URL kopieren (z.B. https://testdrive.crm.dynamics.com/api/data/v9.0) ).

- **Rollenname** (erforderlich): Geben Sie den Namen der Sicherheitsrolle ein, den Sie in der benutzerdefinierten Dynamics 365-Testversion definiert haben. Dieser wird dem Benutzer während der Dauer der Testversion zugewiesen (z.B. „Testversionsrolle“).

### <a name="technical-configuration-for-logic-app-test-drive"></a>Technical configuration for Logic app test drive (Technische Konfiguration für Logik-App-Testversion)

Für alle benutzerdefinierten Produkte sollte dieser Typ von Testversionsbereitstellungsvorlage, der vielfältige komplexe Lösungsarchitekturen umfasst, verwendet werden. Um weitere Informationen über das Einrichten von Logik-App-Testversionen zu erhalten, besuchen Sie [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) und [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) auf GitHub (in englischer Sprache).

- **Region** (erforderlich, Dropdownliste für Einfachauswahl): Es gibt derzeit 26 von Azure unterstützte Regionen, in denen Ihre Testversion zur Verfügung gestellt werden kann. Die Ressourcen für Ihre Logik-App werden in der von Ihnen gewählten Region bereitgestellt. Wenn die Logik-App benutzerdefinierte Ressourcen umfasst, die in einer bestimmten Region gespeichert sind, müssen Sie diese Region hier auswählen. Dazu empfiehlt es sich, die Logik-App lokal in Ihrem Azure-Abonnement im Portal vollständig bereitzustellen und zu überprüfen, ob sie ordnungsgemäß ausgeführt wird, bevor Sie diese Auswahl vornehmen.

- **Maximale Anzahl gleichzeitiger Testversionen** (erforderlich): Legen Sie die maximale Anzahl von Kunden fest, die Ihre Testversion gleichzeitig verwenden können. Diese Testversionen sind bereits bereitgestellt, sodass Kunden sofort auf sie zugreifen können, ohne auf eine Bereitstellung warten zu müssen.

- **Dauer der Testversion** (erforderlich): Geben Sie den Zeitraum, in dem die Testversion aktiv bleibt, als Anzahl von Stunden ein. Nach Ablauf dieses Zeitraums wird die Testversion automatisch beendet.

- **Name der Azure-Ressourcengruppe** (erforderlich): Geben Sie den Namen der [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) ein, in dem die Logik-App-Testversion gespeichert wird.

- **Azure logic app name** (Name der Azure-Logik-App) (erforderlich): Geben Sie den Namen der Logik-App ein, von der die Testversion dem Benutzer zugewiesen wird. Diese Logik-App muss in der oben angegebenen Azure-Ressourcengruppe gespeichert werden.

- **Deprovision logic app name** (Logik-App-Name für Aufhebung der Bereitstellung) (erforderlich): Geben Sie den Namen der Logik-App ein, mit der die Bereitstellung der Testversion aufgehoben wird, wenn sie vom Kunden nicht mehr verwendet wird. Diese Logik-App muss in der oben angegebenen Azure-Ressourcengruppe gespeichert werden.

### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Technical configuration not required for Power BI test drives (Power BI-Testversionen erfordern keine technische Konfiguration)

Für Produkte, mit denen ein interaktives Power BI-Visual veranschaulicht werden soll, kann ein eingebetteter Link zum Freigeben eines benutzerdefinierten Dashboards als Testversion verwendet werden. Deshalb ist keine technische Konfiguration erforderlich. Erfahren Sie mehr über das Einrichten von[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)-Vorlagen-Apps.

### <a name="deployment-subscription-details"></a>Abonnementdetails für Bereitstellung der Testversion

Um die Testversion in Ihrem Auftrag bereitzustellen, erstellen Sie ein eigenes eindeutiges Azure-Abonnement, und stellen Sie es bereit. (Power BI-Testversionen nicht erforderlich).

- **Azure-Abonnement-ID** (erforderlich für Azure Resource Manager und Logik-Apps): Geben Sie die Abonnement-ID ein, um für Ressourcennutzungsberichte und Abrechnungszwecke Zugriff auf die Dienste Ihres Azure-Kontos zu gewähren. Es wird empfohlen, [ein eigenes Azure-Abonnement zu erstellen](https://docs.microsoft.com/azure/billing/billing-create-subscription), das für Testversionen verwendet wird, falls Sie es noch nicht getan haben. Sie finden die Azure-Abonnement-IDs, indem Sie sich beim [Azure-Portal](https://portal.azure.com/) anmelden und im Menü auf der linken Seite zu den **Abonnements** navigieren. Durch Auswahl der Registerkarte wird Ihre Abonnement-ID angezeigt (z.B. a83645ac-1234-5ab6-6789-1h234g764ghty).

- **ID des Azure AD-Mandanten** (erforderlich): Geben Sie Ihre [Mandanten-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) von Azure Active Directory (AD) ein. Um die ID zu ermitteln, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ aus, klicken Sie auf **Eigenschaften**, und suchen Sie dann die aufgeführte Nummer der **Verzeichnis-ID** (z.B. 50c464d3-4930-494c-963c-1e951d15360e). Sie können auch unter [https://www.whatismytenantid.com](https://www.whatismytenantid.com) die Mandanten-ID Ihrer Organisation mithilfe einer Domänennamen-URL nachschlagen.

- **Name des Azure AD-Mandanten** (erforderlich für Dynamics 365): Geben Sie den Namen Ihres Azure Active Directory (AD) ein. Um diesen Namen zu suchen, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Oben rechts wird der Mandantenname unter Ihrem Kontonamen angegeben.

- **Azure AD-App-ID** (erforderlich): Geben Sie die [Anwendungs-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) von Azure Active Directory (AD) ein. Um die ID zu ermitteln, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ aus, klicken Sie auf **App-Registrierungen**, und suchen Sie dann die aufgeführte Nummer der **Anwendungs-ID** (z.B. 50c464d3-4930-494c-963c-1e951d15360e).

- **Geheimer Azure AD-App-Clientschlüssel** (erforderlich): Geben Sie Ihren [geheimen Clientschlüssel](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets) für die Azure AD-Anwendung ein. Um diesen Wert zu finden, melden Sie sich beim [Azur-Portal](https://portal.azure.com/) an. Wählen Sie im linken Menü die Registerkarte **Azure Active Directory** aus, wählen Sie **App-Registrierungen** aus und dann Ihre Testversion-App. Wählen Sie als Nächstes **Zertifikate und Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus, geben Sie eine Beschreibung ein, wählen Sie **Nie** unter **Läuft ab** aus, und wählen Sie dann **Hinzufügen** aus. Stellen Sie sicher, dass Sie den Wert kopieren. (Navigieren Sie nicht von der Seite weg, bevor Sie dies tun, sonst haben Sie keinen Zugriff auf den Wert.)

Klicken Sie auf **Speichern**, bevor Sie mit dem nächsten Abschnitt fortfahren!

### <a name="test-drive-marketplace-listings"></a>Testversionen für Marketplace-Auflistungen

Die Option **Marketplace-Auflistungen** auf der Registerkarte **Testversion** zeigt die Sprachen an, in denen Ihre Testversion verfügbar ist. Beachten Sie, dass **Englisch (Vereinigte Staaten)** derzeit die einzige verfügbare Option ist. Wählen Sie den Namen der Sprache aus, um Informationen zur Beschreibung der Testversion einzugeben.

- **Beschreibung:** (erforderlich): Beschreiben Sie Ihre Testversion. Geben Sie an, was vorgeführt wird, nennen Sie die Ziele, mit denen der Benutzer experimentieren soll, sowie die zu erkundenden Funktionen, und geben Sie relevante Informationen an, die dem Benutzer helfen zu entscheiden, ob er Ihr Angebot erwerben soll. In diesem Feld können bis zu 3.000 Zeichen Text eingegeben werden. 

- **Zugriffsinformationen** (für Azure Resource Manager und Logik-Testversionen erforderlich): Erläutern Sie, was ein Kunde wissen muss, um auf die Testversion zuzugreifen und sie zu verwenden. Beschreiben Sie ein Szenario für die Verwendung Ihres Angebots, und teilen Sie genau mit, was der Kunde wissen sollte, um auf die Funktionen in der gesamten Testversion zuzugreifen. In diesem Feld können bis zu 10.000 Zeichen Text eingegeben werden.

- **Benutzerhandbuch** (erforderlich): Eine ausführliche Beschreibung der Verwendung Ihrer Testversion. Das Benutzerhandbuch sollte genau beschreiben, was der Kunde durch die Erkundung der Testversion erlangen soll, und als Referenz für die Fragen des Kunden dienen. Die Datei muss das PDF-Format aufweisen und nach dem Hochladen mit einem Namen (max. 255 Zeichen) versehen werden.

- **Videos: Videos hinzufügen** (optional): Videos können auf YouTube oder Vimeo hochgeladen werden, und hier kann mit einem Link und einer Miniaturansicht (533 x 324 Pixel) auf sie verwiesen werden. So erhalten die Kunden Informationen zu einzelnen Schritten, um ein besseres Verständnis der Testversion zu erlangen. Unter anderem können Sie ihnen zeigen, wie sie die Funktionen ihres Angebots erfolgreich verwenden, und Szenarien erläutern, in denen die Vorteile hervorgehoben werden.
  - **Name** (erforderlich)
  - **URL (nur YouTube oder Vimeo)** (erforderlich)
  - **Thumbnail (533 x 324px)** (Miniaturansicht (533 x 324 px)): Die Bilddatei muss im PNG-Format vorliegen.

## <a name="supplemental-content"></a>Ergänzender Inhalt

Auf dieser Seite können Sie zusätzliche Informationen zu Ihrem Angebot bereitstellen, um uns bei der Validierung Ihres Angebots zu helfen. Diese Informationen werden weder den Kunden angezeigt noch im Marketplace veröffentlicht.

### <a name="validation-assets"></a>Validierungsressourcen

In diesem Abschnitt müssen Sie einen [CAR (Customization Analysis Report, Bericht zur Anpassungsanalyse)](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/dev-tools/customization-analysis-report) hochladen. Dieser Bericht wird durch die Analyse der Anpassungs- und Erweiterungsmodelle basierend auf einem vordefinierten Satz von Regeln für bewährte Methoden generiert.

Diese Datei muss im XLS- oder XLSX-Format vorliegen. Wenn Sie über mehr als einen Bericht verfügen, können Sie eine ZIP-Datei mit allen Berichten hochladen.

### <a name="does-solution-include-localizations"></a>Umfasst die Lösung die Lokalisierungen?

Wählen Sie **Ja** aus, wenn die Lösung die Verwendung lokaler Standards und Richtlinien ermöglicht (z.B., wenn sie die verschiedenen Regeln zur Lohnbuchhaltung für verschiedene Länder/Regionen umfasst). Klicken Sie andernfalls auf **Nein**.

### <a name="does-solution-enable-translations"></a>Ermöglicht die Lösung die Übersetzungen?

Antworten Sie **Ja**, wenn der Text in Ihrer Lösung in andere Sprachen übersetzt werden kann. Klicken Sie andernfalls auf **Nein**.

## <a name="publish"></a>Veröffentlichen

### <a name="submit-offer-to-preview"></a>Übermitteln des Angebots für die Vorschau

Nachdem Sie alle erforderlichen Abschnitte des Angebots abgeschlossen haben, klicken Sie rechts oben im Portal auf **Veröffentlichen**. Sie werden zur Seite **Review and publish** (Überprüfen und veröffentlichen) weitergeleitet. 

Wenn Sie das Angebot zum ersten Mal veröffentlichen, haben Sie folgende Möglichkeiten:

- Anzeigen des Abschlussstatus für die einzelnen Abschnitte des Angebots.
    - *Nicht gestartet*: Der Abschnitt wurde nicht bearbeitet und muss abgeschlossen werden.
    - *Unvollständig*: Der Abschnitt enthält Fehler, die behoben werden müssen, oder erfordert eine Ergänzung der Informationen. Kehren Sie zu dem/den Abschnitten zurück, und nehmen Sie eine Aktualisierung vor.
    - *Abgeschlossen*: Der Abschnitt ist abgeschlossen. Alle erforderlichen Daten wurden angegeben, und es sind keine Fehler vorhanden. Alle Abschnitte des Angebots müssen abgeschlossen sein, bevor Sie das Angebot einreichen können.
- Geben Sie dem Zertifizierungsteam im Abschnitt **Hinweise zur Zertifizierung** Testanweisungen, um sicherzustellen, dass Ihre App ordnungsgemäß getestet wird, sowie ergänzende Hinweise, die das Verständnis Ihrer App erleichtern.
- Senden Sie dazu das Angebot zur Veröffentlichung, indem Sie auf **Senden** klicken. Wir senden Ihnen eine E-Mail, um Ihnen mitzuteilen, wann eine Vorschauversion des Angebots verfügbar ist, damit Sie es überprüfen und genehmigen können. Sie müssen zum Partner Center zurückkehren und **Live schalten** auswählen, um das Angebot für eine öffentliche Zielgruppe (oder bei einem privaten Angebot für eine private Zielgruppe) zu veröffentlichen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](./update-existing-offer.md)
