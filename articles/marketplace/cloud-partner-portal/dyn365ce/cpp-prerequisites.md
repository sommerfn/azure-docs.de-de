---
title: Voraussetzungen für das Dynamics 365 for Customer Engagement-Angebot | Azure Marketplace
description: Die Voraussetzungen für das Veröffentlichen eines Azure-Anwendungsangebots im Azure Marketplace.
services: Dynamics 365 for Customer Engagement offer, Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: pabutler
ms.openlocfilehash: 9cfec65843012e781c8a8dd06cbc4d1225b0dd36
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67338897"
---
# <a name="dynamics-365-for-customer-engagement-prerequisites"></a>Voraussetzungen für „Dynamics 365 for Customer Engagement“

In diesem Artikel sind die technischen und geschäftlichen Voraussetzungen beschrieben, die erfüllt sein müssen, damit ein Dynamics 365 for Customer Engagement-Anwendungsangebot im AppSource Marketplace veröffentlicht werden kann.  Falls das noch nicht geschehen ist, lesen Sie den [Leitfaden zum Veröffentlichen von Office 365, Dynamics 365, PowerApps und Power BI-Angeboten](../../appsource-offer-publishing-guide.md).


## <a name="technical-requirements"></a>Technische Anforderungen

Ihre Dynamics 365 for Customer Engagement-Anwendung muss den [Microsoft AppSource app review guidelines (Richtlinien zur App-Prüfung von Microsoft AppSource)](https://smp-cdn-prod.azureedge.net/documents/AppsourceGuidelines/Microsoft%20AppSource%20app%20review%20guidelines_v5.pdf) entsprechen, die folgende Anforderungen umfassen:


|              Anforderung             |        BESCHREIBUNG           |
|            ---------------           |      ---------------         |
| Azure Active Directory-Integration   | Ihre App muss einmalige Azure Active Directory-Verbundanmeldung (AAD federated SSO, Azure Active Directory federated Single Sign-On) mit aktivierter Zustimmung zulassen. Weitere Informationen finden Sie unter [Zertifizieren von AppSource für Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-get-appsource-certified). |
| Integration in Microsoft Cloud-Dienste (optional) | Wenn diese Funktionalität erforderlich ist, sollte Ihre App in andere Microsoft Cloud-Dienste wie Microsoft Power BI, Microsoft Flow oder Microsoft Azure-Dienste wie Machine Learning oder Cognitive Services integriert werden. |
| Branchenspezifische Konzentrierung            |  Ihre App muss sich auf einen klar definierten Geschäftsprozess oder ein klar definiertes Problem konzentrieren, in erster Linie für Geschäftskunden bestimmt sein und Benutzern ermöglichen, sich mit ihren Unternehmensanmeldeinformationen (Benutzername und Kennwort) anzumelden.  |
| Kostenloser Testzeitraum und Testversion |  Ein Kunde muss in der Lage sein, Ihre App für einen begrenzten Zeitraum kostenlos zu nutzen: entweder mittels „Jetzt herunterladen“ für kostenlose Apps, einer „Kostenlosen Testversion“ für einen bestimmten Zeitraum, einer Demo-„Testversion“ oder einer „Kontakt mit mir aufnehmen“-Anforderungsoption.  |
| Keine/geringe Konfiguration                 | Ihre App muss sich einfach und schnell konfigurieren und einrichten lassen (keine Entwicklung oder Anpassung erforderlich).  |
| Kundendienst                     | Der Support für Ihre App muss einen Supportlink enthalten, über den Kunden Hilfe bekommen können.  |
| Verfügbarkeit/Betriebszeit                  | Ihre App muss eine Betriebszeit von mindestens 99,9% aufweisen. |
|  |  |


## <a name="business-requirements"></a>Geschäftliche Anforderungen

Die geschäftlichen Anforderungen umfassen die folgenden verfahrenstechnischen, vertraglichen und rechtlichen Verpflichtungen:

* Sie müssen im [Microsoft Partner Network (MPN)](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx) registriert sein oder ein registrierter Cloud Marketplace-Herausgeber sein. Sollten Sie sich nicht registriert haben, führen Sie die Schritte in [Weg zum Cloud Marketplace-Herausgeber](https://docs.microsoft.com/azure/marketplace/become-publisher) aus.  (Sie können außerdem auch Kontakt zum [Onboarding-Team für Dynamics 365 for Customer Engagement](https://experience.dynamics.com/isvengage/) aufnehmen.)

    >[!NOTE]
    >Sie sollten dasselbe Microsoft Developer Center-Registrierungskonto für die Anmeldung beim Cloud-Partnerportal verwenden. Sie sollten für Ihre Azure Marketplace-Angebote nur ein Microsoft-Konto verwenden. Dieses Konto sollte nicht speziell für einzelne Dienste oder Angebote vorgesehen sein.

* Da AppSource keine handelstaugliche Veröffentlichungsoption anbietet, müssen Sie Ihre derzeitige Bestellungs- und Abrechnungsinfrastruktur ohne zusätzliche Investitionen oder Änderungen verwenden.
* Sie sind dafür zuständig, technischen Support für Kunden in einer wirtschaftlich vertretbaren Weise zur Verfügung zu stellen. Der Support kann kostenlos, kostenpflichtig oder communitybasiert sein.
* Sie sind für die Lizenzierung Ihrer Software und sämtlicher Abhängigkeiten von Drittanbietersoftware verantwortlich.
* Sie sollten die zugeordneten Marketingbegleitmaterialien, z.B. offizieller App-Name, Beschreibung (im HTML-Format), Logobilder im PNG-Format (40 x 40, 90 x 90, 115 x 115 und 255 x 115 Pixel) sowie die Nutzungsbedingungen und eine Datenschutzrichtlinie erstellt haben.  


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie diese Anforderungen erfüllt haben, können Sie [ein Dynamics 365 Customer Engagement-Angebot erstellen](./cpp-create-offer.md). 
