---
title: Getaktete Abrechnung mit dem Marketplace Metering Service | Azure Marketplace
description: Diese Dokumentation ist ein Leitfaden für ISVs, die SaaS-Angebote mit flexiblen Abrechnungsmodellen veröffentlichen.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 4b24805cd59d1eb9d28591749d5169486e54d506
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68250117"
---
# <a name="metered-billing-using-the-marketplace-metering-service"></a>Getaktete Abrechnung mit dem Marketplace Metering Service | Azure Marketplace

Mit dem Marketplace Metering Service können Sie SaaS-Angebote (Software-as-a-Service) im Marketplace-Programm erstellen, die entsprechend den nicht standardmäßigen Einheiten abgerechnet werden.  Vor der Veröffentlichung dieses Angebots definieren Sie die Abrechnungsdimensionen wie Bandbreite, Tickets oder bearbeitete E-Mails.  Kunden zahlen dann nach ihrem Verbrauch dieser Dimensionen, wobei Ihr System Microsoft über die Marketplace Metering Service-API über abrechenbare Ereignisse informiert, sobald diese auftreten.  

## <a name="prerequisites-for-metered-billing"></a>Voraussetzungen für getaktete Abrechnung

Damit für ein SaaS-Angebot eine getaktete Abrechnung verwendet werden kann, müssen folgende Schritte durchgeführt werden:

* Erfüllen Sie alle Angebotsanforderungen für ein [Angebot für den Verkauf durch Microsoft ](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#sell-through-microsoft), wie unter [Erstellen eines SaaS-Angebots](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer) dargestellt.
* Integrieren Sie die [SaaS Fulfillment APIs](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2), damit Kunden Ihr Angebot bereitstellen und sich mit ihm verbinden können.  
* Sie können das **Pauschalpreis**-Modell für Rechnungsstellung nutzen.  Die Dimensionen sind eine optionale Erweiterung des Pauschalpreis-Modells. 
* Integrieren Sie die[Marketplace Metering Service-APIs ](./marketplace-metering-service-apis.md), um Microsoft über abrechenbare Ereignisse zu informieren.

>[!Note]
>Der Marketplace Metering Service ist nur für das pauschale Abrechnungsmodell verfügbar und gilt nicht für das Abrechnungsmodell pro Benutzer.

## <a name="how-metered-billing-fits-in-with-pricing"></a>Wie getaktetes Abrechnen mit der Preisgestaltung zusammenpasst

Bei der Definition des Angebots und seiner Preismodelle ist es wichtig, die Angebotshierarchie zu verstehen.

* Jedes SaaS-Angebot ist so konfiguriert, dass es entweder über Microsoft verkauft wird oder nicht.  Diese Einstellung kann nicht geändert werden, nachdem ein Angebot veröffentlicht wurde.
* Jedes SaaS-Angebot, das für den Vertrieb über Microsoft konfiguriert ist, kann über einen oder mehrere Pläne verfügen. Ein Benutzer abonniert das SaaS-Angebot, aber es wird über Microsoft im Rahmen eines Plans erworben.
* Jedem Plan ist ein Preismodell zugeordnet: **pauschal** oder **pro Benutzer**. Alle Pläne in einem Angebot müssen dem gleichen Preismodell zugeordnet werden. Beispielsweise kann es kein Angebot geben, bei dem einer der Pläne ein Pauschalpreismodell und ein anderer ein Pro-Benutzer-Preismodell ist.
* In jedem Plan, der für ein pauschales Abrechnungsmodell konfiguriert wurde, ist mindestens eine wiederkehrende Gebühr enthalten (z. B. 0 €):
    * Wiederkehrende **monatliche** Gebühr: Pauschale monatliche Gebühr, die bei einem monatlichen Wiederauftreten beim Kauf des Plans im Voraus bezahlt wird.
    * Wiederkehrende **Jahresgebühr**: Pauschale Jahresgebühr, die bei einem jährlichen Wiederauftreten beim Kauf des Plans im Voraus bezahlt wird.
* Zusätzlich zu den wiederkehrenden Gebühren kann der Plan auch optionale Dimensionen enthalten, die dem Kunden für die Nutzung von Diensten, die nicht in der Flatrate enthalten sind, berechnet werden.   Jede Dimension stellt eine abrechenbare Einheit dar, die Ihr Dienst über die [Marketplace Metering Service-API](./marketplace-metering-service-apis.md) an Microsoft übermittelt.

## <a name="sample-offer"></a>Beispielangebot

Contoso ist beispielsweise ein Publisher mit einem SaaS-Dienst namens Contoso Notification Services (CNS). Mit CNS können Kunden Benachrichtigungen entweder per e-Mail oder per SMS senden. Contoso ist als Publisher im Partner Center für das kommerzielle Marketplace-Programm zur Veröffentlichung von Angeboten für Azure-Kunden registriert.  Mit dem im folgenden aufgeführten CNS sind zwei Pläne verknüpft:

* Basisplan
    * 10.000 E-Mail-Nachrichten und 1.000 SMS-Nachrichten für 0 US-Dollar/Monat
    * Über die 10.000 E-Mail-Nachrichten hinaus bezahlen Sie 1 US-Dollar für alle weiteren 100 E-Mails
    * Über die 1.000 SMS hinaus zahlen Sie für jede weitere SMS 0,02 US-Dollar
* Premium-Tarif
    * 50.000 E-Mail-Nachrichten und 10.000 SMS-Nachrichten für 350 US-Dollar/Monat
    * Über die 50.000 E-Mail-Nachrichten hinaus bezahlen Sie 0,50 US-Dollar für alle weiteren 100 E-Mails
    * Über die 10.000 SMS hinaus zahlen Sie für jede weitere SMS 0,01 US-Dollar

Ein Azure-Kunde, der sich für den CNS-Service anmeldet, kann die enthaltene Menge an SMS und E-Mails pro Monat basierend auf dem ausgewählten Plan versenden.  Wenn Kunden mehr als die enthaltene Menge verbrauchen, müssen sie weder Pläne ändern noch etwas anderes tun.  Contoso misst die Überschreitung über die enthaltene Menge hinaus und beginnt damit, Nutzungsereignisse an Microsoft zur weiteren Nutzung über die [Marketplace Metering Service-API](./marketplace-metering-service-apis.md) zu senden.  Microsoft berechnet dem Kunden wiederum die zusätzliche Nutzung, wie vom Publisher angegeben.

## <a name="billing-dimensions"></a>Abrechnungsdimensionen

Abrechnungsdimensionen werden verwendet, um mit dem Kunden zu mitzuteilen, wie Ihnen die Nutzung der Software in Rechnung gestellt wird, und um Nutzungsereignisse an Microsoft zu übermitteln. Diese sind wie folgt definiert:

* **Dimensionsbezeichner**: Die unveränderliche Kennung, auf die bei der Ausgabe von Nutzungsereignissen verwiesen wird.
* **Dimensionsname**: der der Dimension zugeordnete Anzeigename, z. B. „Textnachrichten gesendet“.
* **Maßeinheit**: die Beschreibung der Abrechnungseinheit, z. B. „pro SMS“ oder „pro 100 E-Mails“.
* **Preis pro Einheit**: der Preis für eine Einheit der Dimension.  
* **Enthaltene Menge für monatliche Laufzeit**: Die Menge der pro Monat enthaltenen Dimension für Kunden, die die wiederkehrende monatliche Gebühr zahlen, muss eine ganze Zahl sein.
* **Enthaltene Menge für monatliche Laufzeit**: Die Menge der pro Monat enthaltenen Dimension für Kunden, die die wiederkehrende monatliche Gebühr zahlen, muss eine ganze Zahl sein.

Die Abrechnungsdimensionen sind über alle Pläne für ein Angebot verteilt.  Einige Attribute gelten für die Dimension über alle Pläne hinweg, andere Attribute sind planabhängig.

Die Attribute, die die Dimension selbst definieren, werden über alle Pläne zu einem Angebot hinweg gemeinsam genutzt.  Bevor Sie das Angebot veröffentlichen, wirkt sich eine Änderung dieser Attribute aus dem Kontext eines beliebigen Plans auf die Dimensionsdefinition über alle Pläne aus.  Nachdem Sie das Angebot veröffentlicht haben, können diese Attribute nicht mehr bearbeitet werden.  Zu diesen Attributen zählen:

* Bezeichner
* NAME
* Unit of measure

Die anderen Attribute einer Dimension sind planabhängig und können von Plan zu Plan unterschiedliche Werte haben.  Vor dem Veröffentlichen des Plans können Sie diese Werte bearbeiten, und nur dieser Plan ist betroffen.  Nachdem Sie den Plan veröffentlicht haben, können diese Attribute nicht mehr bearbeitet werden.  Zu diesen Attributen zählen:

* Preis pro Einheit
* Enthaltene Menge für monatliche Kunden 
* Enthaltene Menge für jährliche Kunden 

Dimensionen verfügen auch über zwei spezielle Konzepte: aktiviert und unbegrenzt:

* **Aktiviert** gibt an, dass dieser Plan Teil dieser Dimension ist.  Möglicherweise sollten Sie dies nicht überprüfen, wenn Sie einen neuen Plan erstellen, der keine Nutzungsereignisse basierend auf dieser Dimension versendet.  Außerdem werden alle neuen Dimensionen, die nach der ersten Veröffentlichung eines Plans hinzugefügt wurden, auf dem bereits veröffentlichten Plan als "nicht aktiviert" angezeigt.  Eine deaktivierte Dimension wird nun in jeder Liste von Dimensionen für einen Plan angezeigt, der von Kunden angezeigt wird.
* **Unbegrenzt**, dargestellt durch das Unendlichkeitssymbol ∞, gibt an, dass dieser Plan an dieser Dimension teilnimmt, aber die Verwendung dieser Dimension nicht gemessen wird.  Wenn Sie Ihren Kunden mitteilen möchten, dass die durch diese Dimension dargestellte Funktionalität im Plan enthalten ist, aber ohne Einschränkung der Nutzung.  Eine Dimension mit unbegrenzter Nutzung wird in Listen für Dimensionen für einen vom Kunden gesehenen Plan angezeigt, mit dem Hinweis, dass für diesen Plan nie eine Gebühr anfällt.

>[!Note] 
>Folgende Szenarios werden vollständig unterstützt: <br> – Sie können einem neuen Plan eine neue Dimension hinzufügen.  Die neue Dimension wird nicht für bereits veröffentlichte Pläne aktiviert. <br> – Sie können einen **pauschalen** Plan ohne Dimensionen veröffentlichen und dann einen neuen Plan hinzufügen und eine neue Dimension für diesen Plan konfigurieren. Die neue Dimension wird nicht für bereits veröffentlichte Pläne aktiviert.

## <a name="constraints"></a>Einschränkungen

### <a name="trial-behavior"></a>Testverhalten

Die getaktete Abrechnung mithilfe des Marketplace Metering Service ist nicht mit einer kostenlosen Testversion kompatibel.  Es ist nicht möglich, einen Plan für die Verwendung der getakteten Abrechnung und einer kostenlosen Testversion zu konfigurieren.

### <a name="locking-behavior"></a>Sperrverhalten

Da eine mit dem Marketplace Measuring Service verwendete Dimension ein Verständnis dafür darstellt, wie ein Kunde für den Dienst bezahlen wird, sind alle Details für eine Dimension nicht mehr bearbeitbar, wenn Sie diese veröffentlichen.  Es ist wichtig, dass Sie Ihre Dimensionen vollständig für einen Plan definiert haben, bevor Sie diese veröffentlichen.
  
Sobald ein Angebot mit einer Dimension veröffentlicht wurde, können die Details der Angebotsebene für diese Dimension nicht mehr geändert werden:

* Bezeichner
* NAME
* Unit of measure

Nachdem ein Plan veröffentlicht wurde, können die Details auf Planebene nicht mehr geändert werden:

* Preis pro Einheit
* Enthaltene Menge für die monatliche Laufzeit
* Enthaltene Menge für die jährliche Laufzeit
* Gibt an, ob die Dimension für den Plan aktiviert ist

### <a name="upper-limits"></a>Obergrenzen

Die maximale Anzahl von Dimensionen, die für ein einzelnes Angebot konfiguriert werden können, sind 18 einzigartige Dimensionen.

## <a name="get-support"></a>Support

Wenn Sie folgende Erfahrungen machen, können Sie ein Supportticket öffnen.

* Technische Probleme mit der Marketplace Measuring Service-API.
* Ein Problem, das aufgrund eines Fehlers oder Bugs auf Ihrer Seite eskaliert werden muss (z. B. eine Fehlnutzung).
* Alle weiteren Probleme im Zusammenhang mit der getakteten Abrechnung. 

Führen Sie die folgenden Schritte aus, um ein Supportticket zu erstellen:

1. Navigieren Sie zur [Supportseite](https://support.microsoft.com/supportforbusiness/productselection?sapId=48734891-ee9a-5d77-bf29-82bf8d8111ff). Die ersten Dropdownmenüs werden automatisch für Sie ausgefüllt. Für die Unterstützung des Marketplace identifizieren Sie die Produktfamilie als **Cloud- und Online-Services**, und das Produkt als **Marketplace-Publisher**.  Ändern Sie diese vorab ausgefüllten Dropdownmenü-Auswahlen nicht.
2. Wählen Sie unter „Produktversion auswählen“ die Option **Live-Angebotsverwaltung** aus.
3. Wählen Sie unter „Wählen Sie eine Kategorie, die das Problem am besten beschreibt“ die Option **SaaS-Apps** aus.
4. Unter „Wählen Sie ein Problem aus, das das Problem am besten beschreibt“ wählen Sie **getaktete Abrechnung** aus.
5. Wenn Sie auf die Schaltfläche **Weiter** klicken, werden Sie zur Seite mit den **Problemdaten** weitergeleitet, wo Sie weitere Details zu Ihrem Problem eingeben können.

Weitere Support-Optionen für Publisher finden Sie unter [Support für das kommerzielle Marketplace-Programm im Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/support).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Marketplace Metering Service-APIs](./marketplace-metering-service-apis.md).
