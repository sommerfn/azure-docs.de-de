---
title: Getaktete Abrechnung mit dem Marketplace-Messungsdienst | Azure Marketplace
description: Diese Dokumentation ist ein Leitfaden für ISVs, die SaaS-Angebote mit flexiblen Abrechnungsmodellen veröffentlichen.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 54b59cfecbbba6b71cf301aa4872892825d4b675
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73827929"
---
# <a name="metered-billing-using-the-marketplace-metering-service"></a>Getaktete Abrechnung mit dem Marketplace-Messungsdienst

Mit dem Marketplace-Messungsdienst können Sie SaaS-Angebote (Software-as-a-Service) im Rahmen des kommerziellen Marketplace-Programms erstellen, die nach nicht standardmäßigen Einheiten abgerechnet werden.  Vor der Veröffentlichung dieses Angebots definieren Sie die Abrechnungsdimensionen wie Bandbreite, Tickets oder verarbeitete E-Mails.  Kunden zahlen dann auf der Grundlage ihres Verbrauchs dieser Dimensionen, wobei Ihr System Microsoft über die Marketplace-Messungsdienst-API über abrechenbare Ereignisse informiert, sobald diese auftreten.  

## <a name="prerequisites-for-metered-billing"></a>Voraussetzungen für die getaktete Abrechnung

Damit für ein SaaS-Angebot eine getaktete Abrechnung verwendet werden kann, müssen folgende Voraussetzungen erfüllt sein:

* Es muss alle Angebotsanforderungen für ein [Angebot für den Verkauf durch Microsoft](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#sell-through-microsoft) erfüllen, wie unter [Erstellen eines SaaS-Angebots](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer) beschrieben.
* Es muss in die [SaaS-Fulfillment-APIs](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) integriert werden, damit Kunden Ihr Angebot bereitstellen und eine Verbindung damit herstellen können.  
* Es muss für das **Pauschalpreismodell** konfiguriert werden, um Kunden Ihren Dienst in Rechnung stellen zu können.  Dimensionen sind eine optionale Erweiterung des Pauschalpreismodells. 
* Es muss in die [Marketplace-Messungsdienst-APIs](./marketplace-metering-service-apis.md) integriert werden, um Microsoft über abrechenbare Ereignisse zu informieren.

>[!Note]
>Der Marketplace-Messungsdienst ist nur für das pauschale Abrechnungsmodell verfügbar und gilt nicht für das Abrechnungsmodell pro Benutzer.

## <a name="how-metered-billing-fits-in-with-pricing"></a>Zusammenspiel von getakteter Abrechnung und Preisgestaltung

Bei der Definition des Angebots und seiner Preismodelle ist es wichtig, die Angebotshierarchie zu verstehen.

* Jedes SaaS-Angebot ist so konfiguriert, dass es entweder über Microsoft verkauft wird oder nicht.  Diese Einstellung kann nicht geändert werden, nachdem ein Angebot veröffentlicht wurde.
* Jedes SaaS-Angebot, das für den Verkauf über Microsoft konfiguriert ist, kann über einen oder mehrere Pläne verfügen. Ein Benutzer abonniert das SaaS-Angebot, aber es wird über Microsoft im Rahmen eines Plans erworben.
* Jedem Plan ist ein Preismodell zugeordnet: **pauschal** oder **pro Benutzer**. Alle Pläne in einem Angebot müssen dem gleichen Preismodell zugeordnet sein. Beispielsweise kann es kein Angebot geben, bei dem einer der Pläne ein Pauschalpreismodell und ein anderer ein benutzerbasiertes Preismodell ist.
* In jedem Plan, der für ein pauschales Abrechnungsmodell konfiguriert wird, ist mindestens eine wiederkehrende Gebühr enthalten (die allerdings 0 EUR betragen kann):
    * Wiederkehrende **monatliche** Gebühr: Pauschale monatliche Gebühr, die monatlich im Voraus bezahlt wird, wenn der Benutzer den Plan erwirbt.
    * Wiederkehrende **Jahresgebühr**: Pauschale Jahresgebühr, die jährlich im Voraus bezahlt wird, wenn der Benutzer den Plan erwirbt.
* Zusätzlich zu den wiederkehrenden Gebühren kann der Plan auch optionale Dimensionen enthalten, die dem Kunden für nicht in der Pauschale enthaltene Nutzung berechnet werden.   Jede Dimension stellt eine abrechenbare Einheit dar, die Ihr Dienst über die [Marketplace-Messungsdienst-API](./marketplace-metering-service-apis.md) an Microsoft übermittelt.

## <a name="sample-offer"></a>Beispielangebot

In diesem Beispiel ist Contoso ein Herausgeber mit einem SaaS-Dienst namens Contoso Notification Services (CNS). Mit CNS können Kunden Benachrichtigungen entweder per E-Mail oder per SMS senden. Contoso ist in Partner Center als Herausgeber für das kommerzielle Marketplace-Programm zur Veröffentlichung von Angeboten für Azure-Kunden registriert.  CNS sind zwei Pläne zugeordnet:

* Basisplan
    * Senden von 10.000 E-Mails und 1.000 SMS für 0 USD/Monat
    * Über 10.000 E-Mails: 1 USD pro 100 E-Mails
    * Über 1.000 SMS: 0,02 USD pro SMS
* Premium-Tarif
    * Senden von 50.000 E-Mails und 10.000 SMS für 350 USD/Monat
    * Über 50.000 E-Mails: 0,5 USD pro 100 E-Mails
    * Über 10.000 SMS: 0,01 USD pro SMS

Ein Azure-Kunde, der den CNS-Dienst abonniert, kann die Menge an SMS und E-Mails versenden, die pro Monat im ausgewählten Plan enthalten ist.  Die Nutzung bis zur enthaltenen Menge wird von Contoso gemessen, ohne dass Verwendungsereignisse an Microsoft gesendet werden.  Wenn Kunden mehr als die enthaltene Menge beanspruchen, müssen sie nicht zu einem anderen Plan wechseln oder ihr Verhalten ändern.  Contoso misst die Nutzung, die über die enthaltene Menge hinausgeht, und beginnt damit, über die [Marketplace-Messungsdienst-API](./marketplace-metering-service-apis.md) Nutzungsereignisse für die zusätzliche Nutzung an Microsoft zu senden.  Microsoft berechnet dem Kunden wiederum die zusätzliche Nutzung gemäß Angabe des Herausgebers.

## <a name="billing-dimensions"></a>Abrechnungsdimensionen

Abrechnungsdimensionen werden verwendet, um Kunden über die Abrechnung der Softwarenutzung zu informieren und um Nutzungsereignisse an Microsoft zu übermitteln. Sie sind wie folgt definiert:

* **Dimensionsbezeichner**: Die unveränderliche Kennung, auf die bei der Ausgabe von Nutzungsereignissen verwiesen wird.
* **Dimensionsname**: Der der Dimension zugeordnete Anzeigename (beispielsweise „Gesendete SMS“).
* **Maßeinheit**: Die Beschreibung der Abrechnungseinheit (beispielsweise „pro SMS“ oder „pro 100 E-Mails“).
* **Preis pro Einheit**: Der Preis für eine Einheit der Dimension.  
* **Enthaltene Menge für monatliche Laufzeit**: Die Menge der pro Monat enthaltenen Dimension für Kunden, die die wiederkehrende monatliche Gebühr bezahlen. Muss eine ganze Zahl sein.
* **Enthaltene Menge für jährliche Laufzeit**: Die Menge der pro Monat enthaltenen Dimension für Kunden, die die wiederkehrende jährliche Gebühr bezahlen. Muss eine ganze Zahl sein.

Abrechnungsdimensionen werden in allen Plänen für ein Angebot verwendet.  Manche Attribute gelten für die Dimension über alle Pläne hinweg, andere Attribute sind dagegen planspezifisch.

Die Attribute, die die Dimension selbst definieren, werden über alle Pläne zu einem Angebot hinweg gemeinsam genutzt.  Vor der Angebotsveröffentlichung wirkt sich eine Änderung dieser Attribute im Kontext eines beliebigen Plans auf die Dimensionsdefinition für alle Pläne aus.  Nachdem Sie das Angebot veröffentlicht haben, können diese Attribute nicht mehr bearbeitet werden.  Zu diesen Attributen zählen folgende:

* Bezeichner
* NAME
* Unit of measure

Die anderen Attribute einer Dimension sind planspezifisch und können von Plan zu Plan unterschiedliche Werte haben.  Vor dem Veröffentlichen des Plans können Sie diese Werte bearbeiten, und nur dieser Plan ist betroffen.  Nachdem Sie den Plan veröffentlicht haben, können diese Attribute nicht mehr bearbeitet werden.  Zu diesen Attributen zählen folgende:

* Preis pro Einheit
* Enthaltene Menge für monatliche Kunden 
* Enthaltene Menge für jährliche Kunden 

Dimensionen verfügen auch über zwei spezielle Konzepte: „aktiviert“ und „unbegrenzt“:

* **Aktiviert** gibt an, dass dieser Plan Teil dieser Dimension ist.  Lassen Sie diese Option ggf. deaktiviert, wenn Sie einen neuen Plan erstellen, der keine auf dieser Dimension basierenden Nutzungsereignisse versendet.  Außerdem werden alle neuen Dimensionen, die nach der ersten Veröffentlichung eines Plans hinzugefügt wurden, für den bereits veröffentlichten Plan als „nicht aktiviert“ angezeigt.  Eine deaktivierte Dimension wird in keiner Dimensionenliste für einen Plan angezeigt, der für Kunden sichtbar ist.
* **Unbegrenzt**, dargestellt durch das Unendlichkeitssymbol „∞“, gibt an, dass dieser Plan Teil dieser Dimension ist, die Nutzung aber nicht anhand dieser Dimension gemessen wird.  Dadurch können Sie Ihren Kunden vermitteln, dass die durch diese Dimension dargestellte Funktion im Plan enthalten und die Nutzung nicht begrenzt ist.  Eine Dimension mit unbegrenzter Nutzung wird in Dimensionenlisten für einen Plan, der für Kunden sichtbar ist, mit dem Hinweis angezeigt, dass dadurch für diesen Plan keine Kosten entstehen.

>[!Note] 
>Folgende Szenarien werden explizit unterstützt: <br> - Sie können einem neuen Plan eine neue Dimension hinzufügen.  Die neue Dimension wird nicht für bereits veröffentlichte Pläne aktiviert. <br> - Sie können einen **pauschalen** Plan ohne Dimensionen veröffentlichen und anschließend einen neuen Plan hinzufügen und eine neue Dimension für diesen Plan konfigurieren. Die neue Dimension wird nicht für bereits veröffentlichte Pläne aktiviert.

## <a name="constraints"></a>Einschränkungen

### <a name="trial-behavior"></a>Testverhalten

Die getaktete Abrechnung mit dem Marketplace-Messungsdienst ist nicht mit Angeboten für kostenlose Testversionen kompatibel.  Die getaktete Abrechnung und eine kostenlose Testversion können nicht gemeinsam in einem Plan konfiguriert werden.

### <a name="locking-behavior"></a>Sperrverhalten

Da eine mit dem Marketplace-Messungsdienst verwendete Dimension bestimmt, wie ein Kunde für den Dienst bezahlt, können die Details einer Dimension nach der Veröffentlichung nicht mehr bearbeitet werden.  Daher ist es wichtig, dass Sie Ihre Dimensionen vor der Veröffentlichung vollständig für einen Plan definiert wurden.
  
Sobald ein Angebot mit einer Dimension veröffentlicht wurde, können die Details der Angebotsebene für diese Dimension nicht mehr geändert werden:

* Bezeichner
* NAME
* Unit of measure

Nachdem ein Plan veröffentlicht wurde, können die Details auf der Planebene nicht mehr geändert werden:

* Preis pro Einheit
* Enthaltene Menge für die monatliche Laufzeit
* Enthaltene Menge für die jährliche Laufzeit
* Angabe, ob die Dimension für den Plan aktiviert ist

### <a name="upper-limits"></a>Obergrenzen

Für ein einzelnes Angebot können maximal 18 individuelle Dimensionen konfiguriert werden.

## <a name="get-support"></a>Support

Sollte bei Ihnen eines der folgenden Probleme auftreten, können Sie ein Supportticket erstellen:

* Technische Probleme mit der Marketplace-Messungsdienst-API
* Ein Problem, das aufgrund eines Fehlers auf Ihrer Seite eskaliert werden muss (beispielsweise ein falsches Nutzungsereignis)
* Ein anderes Problem im Zusammenhang mit der getakteten Abrechnung 

Führen Sie die folgenden Schritte aus, um Ihr Supportticket zu übermitteln:

1. Navigieren Sie zur [Supportseite](https://support.microsoft.com/supportforbusiness/productselection?sapId=48734891-ee9a-5d77-bf29-82bf8d8111ff). Die ersten Dropdownmenüs werden automatisch für Sie ausgefüllt. Geben Sie für den Marketplace-Support die Produktfamilie als **Cloud und Onlinedienste** und das Produkt als **Marketplace-Herausgeber** an.  Ändern Sie die vorab ausgefüllten Angaben in den Dropdownmenüs nicht.
2. Wählen Sie unter „Produktversion auswählen“ die Option **Live-Angebotsverwaltung** aus.
3. Wählen Sie unter „Wählen Sie eine Kategorie aus, die das Problem am besten beschreibt“ die Option **SaaS-Apps** aus.
4. Wählen Sie unter „Wählen Sie ein Problem aus, das das Problem am besten beschreibt“ die Option **getaktete Abrechnung** aus.
5. Durch Auswählen der Schaltfläche **Weiter** gelangen Sie zur Seite **Problemdetails**, wo Sie weitere Details zu Ihrem Problem eingeben können.

Weitere Supportoptionen für Herausgeber finden Sie unter [Support für das Programm „Kommerzieller Marketplace“ im Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/support).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Marketplace-Messungsdienst-APIs](./marketplace-metering-service-apis.md).
