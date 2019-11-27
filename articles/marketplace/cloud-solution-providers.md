---
title: Cloud Solution Provider (CSP) | Azure Marketplace
description: Herausgeber können jetzt ihre Angebote über den Microsoft Cloud Solution Provider (CSP)-Partnerkanal verkaufen.
services: Azure, Marketplace, Compute, Storage, Networking, Partner Center
author: ChJenk
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
ms.date: 11/12/2019
ms.author: v-chjen
ms.openlocfilehash: 546702af671cfe9506a4fc0448f40b7b8353960c
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74038819"
---
# <a name="cloud-solution-providers"></a>Cloud Solution Provider (CSP)

Softwareangebote können, zusätzlich zur öffentlichen Verfügbarkeit von Angeboten über die [Microsoft-Webstorefronts](https://docs.microsoft.com/azure/marketplace/comparing-appsource-azure-marketplace), Millionen von berechtigten Microsoft-Kunden erreichen, die von Partnern des Cloud Solution Provider-Programms (CSP-Programm) betreut werden.

Verleger konfigurieren Angebote für die Verfügbarkeit im CSP-Programm auf Abonnementbasis für ein neues oder vorhandenes Angebot, sodass Partner Ihre Produkte verkaufen und gebündelte Lösungen für Kunden erstellen können.

Die Herausgeber sind verantwortlich für die Bereitstellung des Supports zum Beheben von Problemen für den Endkunden. Außerdem müssen sie einen Mechanismus für Partner im CSP-Programm und/oder Kunden zur Kontaktaufnahme mit dem Support bereitstellen. Es hat sich bewährt, den Partnern im CSP-Programm Benutzerdokumentationen, Schulungen und Benachrichtigungen über Dienstintegrität/Dienstausfall (wie zutreffend) bereitzustellen, sodass die Partner im CSP-Programm für die Behandlung von Supportanforderungen der Ebene 1 von Kunden ausgerüstet sind.  

Die folgenden Angebote können abonniert werden, um von Partnern im CSP-Programm verkauft zu werden:

- Software-as-a-Service-Transaktionsangebote (SaaS)
- Virtuelle Computer (Virtual Machines, VMs)
- Lösungsvorlagen
- Verwaltete Anwendungen

> [!NOTE]
> Container und BYOL-VM-SKUs (Bring-Your-Own-License, Verwendung Ihrer eigenen Lizenz) sind standardmäßig für den Verkauf durch Partner im CSP-Programm abonniert.

## <a name="how-to-configure-an-offering"></a>Konfigurieren ein Angebots

Die „Abonnieren“-Einstellung für das CSP-Programm wird im Partner Center oder im Cloud-Partnerportal im Bereich für die Erstellung von Angeboten konfiguriert. [Erfahren Sie mehr über den Wechsel der Umgebung für Herausgeber](https://www.microsoftpartnercommunity.com/t5/Azure-Marketplace-and-AppSource/Cloud-Marketplace-In-Partner-Center/m-p/9738#M293).

### <a name="partner-center-opt-in"></a>Partner Center-Abonnement

Im Partner Center finden Sie die Abonnementoberfläche unter dem Modul „Cloud Solution Provider (CSP) – Zielgruppe ‚Handelspartner‘“

![CSP – Zielgruppe „Handelspartner“](media/marketplace-publishers-guide/csp-reseller-audience.png)

Im Modul „Cloud Solution Provider (CSP) – Zielgruppe ‚Handelspartner‘“ stehen Ihnen drei Optionen zur Auswahl:

- Option 1: Alle Partner im CSP-Programm
- Option 2: Bestimmte, von mir ausgewählte Partner im CSP-Programm
- Option 3: Keine Partner im CSP-Programm

#### <a name="option-one-any-partner-in-the-csp-program"></a>Option 1: Alle Partner im CSP-Programm

![Alle Partner im CSP-Programm](media/marketplace-publishers-guide/csp-reseller-option-one.png)

 Wenn Sie diese Option auswählen, können alle Partner im CSP-Programm Ihr Angebot an ihre Kunden weiterverkaufen.

#### <a name="option-two-specific-partners-in-the-csp-program-i-select"></a>Option 2: Bestimmte, von mir ausgewählte Partner im CSP-Programm

![Bestimmte, von mir ausgewählte Partner im CSP-Programm](media/marketplace-publishers-guide/csp-reseller-option-two.png)

Wenn Sie diese Option auswählen, autorisieren Sie die Partner im CSP-Programm, die zum Weiterverkaufen Ihres Angebots berechtigt sind.

Um Partner zu autorisieren, klicken Sie auf **CSP-Partner auswählen**. Ein Menü wird angezeigt, in dem Sie nach Partnername oder CSP-Azure Active Directory (AAD)-Mandanten-ID suchen können.

![Menü „CSP auswählen“](media/marketplace-publishers-guide/csp-pop-up-module.png)

Sie können Suchfilter anwenden, z.B. **Land**, **Kompetenz**oder **Qualifikation**.

![Länder-, Kompetenz- und Qualifikationsfilter für die Partnersuche](media/marketplace-publishers-guide/csp-add-resellers.png)

Nachdem Sie die Partnerliste zusammengestellt haben, wählen Sie **Hinzufügen** aus.

![Beispielliste autorisierter Partner im CSP-Programm](media/marketplace-publishers-guide/csp-add-resellers-details.png)

In einer Tabelle wird die Liste der von Ihnen ausgewählten Partner auf der Seite „Cloud Solution Provider (CSP) – Zielgruppe ‚Handelspartner‘“ angezeigt.

![Tabelle mit einer Liste der Partner auf der Seite „Cloud Solution Provider (CSP) – Zielgruppe ‚Handelspartner‘“](media/marketplace-publishers-guide/csp-option-two-add-reseller.png)

Wählen Sie **Entwurf speichern** aus, um Ihre Änderungen zu registrieren.

Wenn das Angebot noch nicht veröffentlicht wurde, müssen Sie es veröffentlichen, um es für Ihre ausgewählten Partner verfügbar zu machen.

>[!NOTE]
>Wenn Sie einen Partner im CSP-Programm in einer bestimmten Region autorisieren, kann dieser das Angebot an jeden seiner Kunden in dieser Region verkaufen. Weitere Informationen zur Klassifizierung von CSP-Angeboten unter Regionen finden Sie unter [Regionale Märkte und Währungen des Cloud Solution Provider-Programms](https://docs.microsoft.com/partner-center/regional-authorization-overview).

Wenn Sie die CSP-Liste eines bereits veröffentlichten Angebots aktualisieren, fügen Sie die zusätzlichen Partner hinzu, und wählen Sie dann **CSP-Zielgruppe synchronisieren** aus.

Wenn Sie über ein Angebot verfügen, das bereits eine Liste autorisierter Partner aufweist, und Sie dieselbe Liste für ein anderes Angebot verwenden möchten, verwenden Sie **Import/Export** aus. Navigieren Sie zum Angebot mit der CSP-Liste, und wählen Sie **CSPs exportieren** aus. Die-Funktion erstellt eine CSV-Datei, die in ein anderes Angebot importiert werden kann.

#### <a name="option-three-no-partners-in-the-csp-program"></a>Option 3: Keine Partner im CSP-Programm

![Keine Partner im CSP-Programm](media/marketplace-publishers-guide/csp-reseller-option-three.png)

Wenn Sie diese Option auswählen, entfernen Sie Ihr Angebot aus dem CSP-Programm. Sie können diese Auswahl jederzeit ändern.

### <a name="cloud-partner-portal-opt-in"></a>Cloud-Partnerportal-Abonnement

Im Cloud-Partnerportal wird die Abonnementeinstellung auf der Registerkarte „Marketplace“ oder „Storefront“ festgelegt. Die Möglichkeit, bestimmte Partner im CSP-Programm auszuwählen, ist nur in Partner Center verfügbar.

![Umgebung zum Abonnieren des CSP-Partnerkanals im CPP](media/marketplace-publishers-guide/csp-opt-in.png)

## <a name="deauthorize-partners-in-the-csp-program"></a>Entfernen der Autorisierung von Partnern im CSP-Programm

Wenn Sie einen Partner im CSP-Programm autorisiert haben und dieser Partner das Produkt bereits an seine Kunden verkauft hat, sind Sie nicht berechtigt, die Autorisierung dieses Partners zu entfernen.

Wenn ein Partner im CSP-Programm Ihr Produkt noch nicht an seine Kunden verkauft hat und Sie den CSP entfernen möchten, nachdem das Angebot veröffentlicht wurde, gehen Sie folgendermaßen vor:

1. Navigieren Sie zur Seite [Supportanfrage](https://aka.ms/marketplacepublishersupport). Die ersten Dropdownmenüs werden automatisch für Sie ausgefüllt.

   > [!NOTE]
   > Ändern Sie die vorab ausgefüllten Angaben in den Dropdownmenüs nicht.

2. Wählen Sie für **Produktversion auswählen** die Option **Live-Angebotsverwaltung** aus.
3. Wählen Sie für **Kategorie auswählen, die das Problem am besten beschreibt**, die Kategorie aus, die für Ihr Angebot gilt.
4. Wählen Sie für **Option auswählen, die das Problem am besten beschreibt** die Option **Vorhandenes Angebot aktualisieren** aus.
5. Wählen Sie **Weiter** aus, um zur Detailseite für das **Problem** zu gelangen und weitere Informationen zu Ihrem Problem einzugeben.
6. Verwenden Sie **Entfernen der Autorisierung eines CSP** als Titel für das Problem, und füllen Sie die restlichen Abschnitte aus.




## <a name="navigate-between-options"></a>Wechseln zwischen Optionen

Verwenden Sie diesen Abschnitt, um zwischen den drei Optionen für CSP-Handelspartner zu wechseln.

### <a name="navigate-from-option-one-any-partner-in-the-csp-program"></a>Wechseln von Option 1: Alle Partner im CSP-Programm

Wenn für Ihr Angebot derzeit **Option 1: Alle Partner im CSP-Programm** ausgewählt ist und Sie zu einer der beiden anderen Optionen wechseln möchten, gehen Sie folgendermaßen vor, um eine Anforderung zu erstellen:

1. Wechseln Sie zur Seite [Supportanfrage](https://aka.ms/marketplacepublishersupport). Die ersten Dropdownmenüs werden automatisch für Sie ausgefüllt.

   > [!NOTE]
   > Ändern Sie die vorab ausgefüllten Angaben in den Dropdownmenüs nicht.

2. Wählen Sie für **Produktversion auswählen** die Option **Live-Angebotsverwaltung** aus.
3. Wählen Sie für **Kategorie auswählen, die das Problem am besten beschreibt**, die Kategorie aus, die für Ihr Angebot gilt.
4. Wählen Sie für **Option auswählen, die das Problem am besten beschreibt** die Option **Vorhandenes Angebot aktualisieren** aus.
5. Wählen Sie **Weiter** aus, um zur Detailseite für das **Problem** zu gelangen und weitere Informationen zu Ihrem Problem einzugeben.
6. Verwenden Sie **Entfernen der Autorisierung eines CSP** als Titel für das Problem, und füllen Sie die restlichen Abschnitte aus.

> [!NOTE]
> Wenn Sie versuchen, zu Option 2 zu wechseln, und ein Partner im CSP-Programm das Angebot bereits an seine Kunden weiterverkauft hat, ist dieser Partner standardmäßig bereits in der Liste der autorisierten Partner enthalten.  

### <a name="navigate-from-option-two-specific-partners-in-the-csp-program-i-select"></a>Wechseln von Option 2: Bestimmte, von mir ausgewählte Partner im CSP-Programm

Wenn für Ihr Angebot derzeit **Option 2: Bestimmte, von mir ausgewählte Partner im CSP-Programm** ausgewählt ist und Sie zu **Option 1: Alle Partner im CSP-Programm** wechseln möchten, gehen Sie folgendermaßen vor, um eine Anforderung zu erstellen:

1. Wechseln Sie zur Seite [Supportanfrage](https://aka.ms/marketplacepublishersupport). Die ersten Dropdownmenüs werden automatisch für Sie ausgefüllt.

   > [!NOTE]
   > Ändern Sie die vorab ausgefüllten Angaben in den Dropdownmenüs nicht.

2. Wählen Sie für **Produktversion auswählen** die Option **Live-Angebotsverwaltung** aus.
3. Wählen Sie für **Kategorie auswählen, die das Problem am besten beschreibt**, die Kategorie aus, die für Ihr Angebot gilt.
4. Wählen Sie für **Option auswählen, die das Problem am besten beschreibt** die Option **Vorhandenes Angebot aktualisieren** aus.
5. Wählen Sie **Weiter** aus, um zur Detailseite für das **Problem** zu gelangen und weitere Informationen zu Ihrem Problem einzugeben.
6. Verwenden Sie **Entfernen der Autorisierung eines CSP** als Titel für das Problem, und füllen Sie die restlichen Abschnitte aus.

 Wenn für Ihr Angebot derzeit **Option 2: Bestimmte, von mir ausgewählte Partner im CSP-Programm** ausgewählt ist und Sie zu **Option 3: Keine Partner im CSP-Programm** wechseln möchten, können Sie nur dann zu dieser Option wechseln, wenn die zuvor autorisierten Partner im CSP-Programm Ihr Angebot noch nicht an Endkunden verkauft haben. Gehen Sie folgendermaßen vor, um eine Anforderung zu erstellen:

1. Wechseln Sie zur Seite [Supportanfrage](https://aka.ms/marketplacepublishersupport). Die ersten Dropdownmenüs werden automatisch für Sie ausgefüllt.

   > [!NOTE]
   > Ändern Sie die vorab ausgefüllten Angaben in den Dropdownmenüs nicht.

2. Wählen Sie für **Produktversion auswählen** die Option **Live-Angebotsverwaltung** aus.
3. Wählen Sie für **Kategorie auswählen, die das Problem am besten beschreibt**, die Kategorie aus, die für Ihr Angebot gilt.
4. Wählen Sie für **Option auswählen, die das Problem am besten beschreibt** die Option **Vorhandenes Angebot aktualisieren** aus.
5. Wählen Sie **Weiter** aus, um zur Detailseite für das **Problem** zu gelangen und weitere Informationen zu Ihrem Problem einzugeben.
6. Verwenden Sie **Entfernen der Autorisierung eines CSP** als Titel für das Problem, und füllen Sie die restlichen Abschnitte aus.

### <a name="navigate-from-option-3-no-partners-in-the-csp-program"></a>Wechseln von Option 3: Keine Partner im CSP-Programm

Wenn für Ihr Angebot derzeit **Option 3: Keine Partner im CSP-Programm** ausgewählt ist, können Sie jederzeit zu einer der beiden anderen Optionen wechseln.

## <a name="sharing-sales-and-support-materials-with-partners-in-the-csp-program"></a>Bereitstellen von Verkaufs- und Begleitmaterialien für Partner im CSP-Programm

Um es Partnern im Cloud Solution Provider-Programm zu ermöglichen, bestmöglich Ihr Angebot zu präsentieren und mit Ihrem Unternehmen in Kontakt zu treten, übermitteln Sie Verkaufs- und Begleitmaterialien, die den Handelspartnern zur Verfügung stehen. Diese Ressourcen werden Kunden in den digitalen Marketplace-Ladenzeilen nicht verfügbar gemacht.

### <a name="partner-center-csp-channel"></a>Partner Center-CSP-Kanal

Wenn Sie den CSP-Kanal im Partner Center abonniert haben, müssen Herausgeber im CSP-Kanal unter dem Angebotslisten-Modul eine URL eingeben, unter der die relevanten Marketingmaterialien und Kontaktinformationen für den Kanal gehostet werden:

![CSP-Zusatzinformationen im Partner Center](media/marketplace-publishers-guide/pc-csp-channel.png)

### <a name="cloud-partner-portal-csp-channel"></a>CSP-Kanal des Cloud-Partnerportals

Wenn Sie den CSP-Kanal abonniert haben, müssen Herausgeber im Cloud-Partnerportal eine URL eingeben, unter der die relevanten Marketingmaterialien und Kanalkontaktinformationen für den CSP-Kanal gehostet werden:

![CSP-Zusatzinformationen im Cloud-Partnerportal](media/marketplace-publishers-guide/cpp-csp-information.png)

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie die Seite [Herausgeberleitfaden für Azure Marketplace und AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).

Besuchen Sie [Go-To-Market Services](https://partner.microsoft.com/reach-customers/gtm), um weitere Informationen zu Marketplace-GTM-Diensten zu erhalten.

Melden Sie sich beim [Partner Center](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/azureisv) oder [Cloud-Partnerportal](https://cloudpartner.azure.com/) an, um Ihr Angebot zu erstellen und zu konfigurieren.
