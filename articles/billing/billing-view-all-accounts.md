---
title: Anzeigen Ihrer Abrechnungskonten im Azure-Portal | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Ihre Abrechnungskonten im Azure-Portal anzeigen.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2018
ms.author: banders
ms.openlocfilehash: 36430e9b0a4554761d53b537d3c32fa57068eabb
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "67490220"
---
# <a name="view-billing-accounts-in-azure-portal"></a>Anzeigen von Abrechnungskonten im Azure-Portal  

Ein Abrechnungskonto wird erstellt, wenn Sie sich für die Verwendung von Azure registrieren. Sie verwenden Ihr Abrechnungskonto zum Verwalten Ihrer Rechnungen und Zahlungen sowie zum Nachverfolgen von Kosten. Sie können über Zugriff auf mehrere Abrechnungskonten verfügen. Es kann beispielsweise sein, dass Sie sich für Ihre persönlichen Projekte für Azure registriert haben. Unter Umständen können Sie auch über das Enterprise Agreement Ihrer Organisation oder über eine Microsoft-Kundenvereinbarung zugreifen. In diesen Szenarien verfügen Sie dann jeweils über ein separates Abrechnungskonto.

Für das Azure-Portal werden derzeit die folgenden Arten von Abrechnungskonten unterstützt:

- **Microsoft Online Services-Programm**: Ein Abrechnungskonto für ein Microsoft Online Services-Programm wird erstellt, wenn Sie sich über die Azure-Website für Azure registrieren. Beispiele hierfür sind die Registrierung für ein [Kostenloses Azure-Konto](https://azure.microsoft.com/offers/ms-azr-0044p/), ein [Angebot mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) oder als [Visual Studio-Abonnent](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/).

- **Enterprise Agreement**: Ein Abrechnungskonto für ein Enterprise Agreement wird erstellt, wenn Ihre Organisation ein [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) für die Nutzung von Azure unterzeichnet.

- **Microsoft-Kundenvereinbarung**: Ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung wird erstellt, wenn Ihre Organisation eine Microsoft-Kundenvereinbarung über einen Microsoft-Vertreter unterzeichnet. Einige Kunden in ausgewählten Regionen, die sich über die Azure-Website für ein [Angebot mit nutzungsbasierter Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) registrieren oder ein Upgrade ihres [kostenlosen Azure-Kontos](https://azure.microsoft.com/offers/ms-azr-0044p/) ausführen, verfügen möglicherweise auch über ein Abrechnungskonto für eine Microsoft-Kundenvereinbarung. Weitere Informationen finden Sie unter [Erste Schritte mit Ihrem Abrechnungskonto für eine Microsoft-Kundenvereinbarung](billing-mca-overview.md).

<!--Todo Add section to identify the type of accounts -->

## <a name="scopes-for-billing-accounts"></a>Bereiche für Abrechnungskonten
Ein Bereich ist ein Knoten in einem Abrechnungskonto, den Benutzer zum Anzeigen und Verwalten der Abrechnung verwenden. Dort verwalten Benutzer Abrechnungsdaten, Zahlungen und Rechnungen und führen allgemeine Kontoverwaltungsaufgaben aus. 

### <a name="microsoft-online-services-program"></a>Microsoft Online Services-Programm

|`Scope`  |Definition  |
|---------|---------|
|Abrechnungskonto     | Stellt einen einzigen Besitzer (Kontoadministrator) für ein oder mehrere Azure-Abonnements dar. Ein Kontoadministrator ist autorisiert, verschiedene Abrechnungsaufgaben wie das Erstellen von Abonnements, das Anzeigen von Rechnungen oder das Ändern der Abrechnung für Abonnements auszuführen.  |
|Subscription     |  Stellt eine Gruppierung von Azure-Ressourcen dar. In diesem Bereich wird die Rechnung generiert. Für jedes Abonnement gelten eigene Zahlungsmethoden, die zum Bezahlen der Rechnung verwendet werden.|


### <a name="enterprise-agreement"></a>Enterprise Agreement

|`Scope`  |Definition  |
|---------|---------|
|Abrechnungskonto    | Stellt eine Enterprise Agreement-Registrierung dar. In diesem Bereich wird die Rechnung generiert. Die Strukturierung erfolgt mithilfe von Abteilungen und Registrierungskonten.  |
|Department     |  Optionale Gruppierung von Registrierungskonten.      |
|Registrierungskonto     |  Stellt einen einzigen Kontobesitzer dar. Azure-Abonnements werden unter diesem Bereich erstellt.  |


### <a name="microsoft-customer-agreement"></a>Microsoft-Kundenvereinbarung

|`Scope`  |Aufgaben  |
|---------|---------|
|Abrechnungskonto     |   Stellt eine Kundenvereinbarung für mehrere Microsoft-Produkte und -Dienste dar. Die Strukturierung erfolgt mithilfe von Abrechnungsprofilen und Rechnungsabschnitten.   |
|Abrechnungsprofil     |  Stellt eine Rechnung und die zugehörigen Zahlungsmethoden dar. In diesem Bereich wird die Rechnung generiert. Ein Abrechnungsprofil kann mehrere Rechnungsabschnitte enthalten.      |
|Rechnungsabschnitt     |   Stellt eine Gruppe von Kosten in einer Rechnung dar. Diesem Bereich sind Abonnements und andere Käufe zugeordnet.    |


## <a name="switch-billing-scope-in-the-azure-portal"></a>Wechseln des Abrechnungsbereichs im Azure-Portal


1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach **Kostenverwaltung + Abrechnung**.

   ![Screenshot, der die Suche im Azure-Portal zeigt](./media/billing-view-all-accounts/billing-search-cost-management-billing.png)

3. Wählen Sie auf der linken Seite die Option **Alle Abrechnungsbereiche** aus.

   ![Screenshot: Alle Abrechnungsbereiche](./media/billing-view-all-accounts/billing-list-of-accounts.png)

   ** Wenn Sie nur auf einen Bereich zugreifen können, wird die Option **Alle Abrechnungsbereiche** nicht angezeigt.

4. Wählen Sie einen Bereich aus, um Details dazu anzuzeigen.



## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Wenden Sie sich an uns.

Wenn Sie Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie mit dem [Analysieren von Kosten](../cost-management/quick-acm-cost-analysis.md) beginnen.