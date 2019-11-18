---
title: SaaS-Fulfillment-APIs – FAQ | Azure Marketplace
description: Auffindbarkeit und Kauferlebnis für Kunden eines SaaS-Angebots im Azure Marketplace.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: 2f8aa41604ac176181252fb7b30cdeb2f039d10f
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73822870"
---
# <a name="saas-fulfillment-apis---faq"></a>SaaS-Fulfillment-APIs – Häufig gestellte Fragen

Nachfolgend werden die Integrationsanforderungen für den Azure Marketplace aufgeführt, um Azure-Kunden das Abonnieren eines SaaS-Angebots zu ermöglichen.

## <a name="discovery-experience"></a>Auffindbarkeit

Nach der Veröffentlichung des SaaS-Angebots steht dieses für Azure-Benutzer im Azure Marketplace zur Verfügung. Ihre Kunden können Angebote nach Produkttyp (SaaS) filtern und nach für sie relevanten SaaS-Angeboten suchen.

## <a name="purchase-experience"></a>Kauferlebnis

Wenn sich ein Benutzer für ein bestimmtes SaaS-Angebot interessiert, kann er es über den Azure Marketplace abonnieren.

### <a name="what-does-it-mean-for-an-azure-user-to-subscribe-to-a-saas-offer-in-azure-marketplace"></a>Was bedeutet es für einen Azure-Benutzer, ein SaaS-Angebot im Azure Marketplace zu abonnieren?

Es bedeutet, dass der Benutzer die für das SaaS-Angebot geltenden Nutzungsbedingungen und Datenschutzbestimmungen einsehen und den von Ihnen als Herausgeber des SaaS-Angebots festgelegten Abrechnungskonditionen zustimmen kann, um es über Microsoft zu erwerben. Benutzer können ihr vorhandenes Zahlungsprofil in Azure verwenden, um für die Nutzung des SaaS-Angebots zu zahlen.

Dies ist aus vielen Gründen von Vorteil. Kunden können nun mit der Microsoft Cloud Platform als vertrauenswürdige Quelle zentral Dienste entdecken und abonnieren, ohne jede ISV-Software untersuchen zu müssen, die sie verwenden möchten. Außerdem können Kunden ihr vorhandenes Zahlungsprofil nutzen und müssen auf diese Weise nicht jede ISV-Software separat zahlen.

### <a name="is-the-user-charged-automatically-when-the-offer-is-subscribed"></a>Werden dem Benutzer bei der Anmeldung für das Angebot automatisch Gebühren berechnet?

Mit dem Abonnement des SaaS-Angebots hat der Benutzer zugestimmt, die Nutzung des SaaS-Angebots über die Microsoft-Plattform zu bezahlen. Die Gebührenabrechnung beginnt jedoch erst mit der Nutzung des Angebots. Der Benutzer muss Ihr SaaS-Angebot aufrufen und die Kontoerstellung bestätigen, um mit der Nutzung des Angebots zu beginnen. Sie benachrichtigen in diesem Fall Microsoft, dass die Abrechnung für dieses SaaS-Kundenabonnement beginnt.

### <a name="how-are-you-notified-when-a-user-subscribes-to-your-saas-offer"></a>Wie werden Sie benachrichtigt, wenn ein Benutzer Ihr SaaS-Angebot abonniert?

Nachdem ein Benutzer ein Angebot abonniert hat, kann er die erworbenen Angebote in Azure ausfindig machen und verwalten. Standardmäßig lautet der Status eines neu abonnierten SaaS-Angebots **Bereitstellung, Erfüllung ausstehend**. In diesem Zustand wird dem Azure-Benutzer die Aktion **Konto konfigurieren** angeboten, um zur SaaS-Abonnementverwaltung im Azure-Portal zu wechseln.

Wenn der Benutzer auf **Konto konfigurieren** klickt, wird er an die SaaS-Angebotswebsite weitergeleitet. Die hierbei verwendete Ziel-URL wird zum Zeitpunkt der Angebotsveröffentlichung durch den Herausgeber festgelegt. Diese Seite wird als Landing Page des Herausgebers bezeichnet. Azure-Benutzer sollten sich basierend auf ihren AAD-Anmeldeinformationen in Azure auf der SaaS-Landing Page anmelden können.

Wenn der Azure-Benutzer auf die Landing Page umgeleitet wird, wird der Abfrage-URL ein Token hinzugefügt. Es handelt sich hierbei um ein kurzlebiges Token mit einer Gültigkeitsdauer von 24 Stunden. Sie können anschließend das Vorhandensein dieses Tokens ermitteln und die Microsoft-API aufrufen, um weitere mit dem Token verknüpfte Kontextinformationen abzurufen.

![Ablauf für Kundenabonnement](media/saas-metering-service-integration-flow-a.png)

Weitere Informationen zur Abwicklung von Transaktionen im Lebenszyklus eines SaaS-Angebots finden Sie im Dokument [SaaS-Fulfillment-API](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2).

### <a name="how-do-you-know-the-saas-offer-to-which-the-user-subscribes-in-azure"></a>Woher wissen Sie, welches SaaS-Angebot der Benutzer in Azure abonniert?

Die Antwort der `Resolve`-API enthält die Angebots- und Planinformationen, die dem SaaS-Abonnement zugeordnet sind.

### <a name="how-can-the-azure-user-change-the-plan-associated-with-this-azure-subscription"></a>Wie kann der Azure-Benutzer den Plan ändern, der diesem Azure-Abonnement zugeordnet ist?

* Der Azure-Benutzer kann den mit dem SaaS-Abonnement verbundenen Plan direkt in der SaaS-Benutzeroberfläche selbst oder über die Microsoft-Plattform ändern.

* Konvertierungen können zu einem beliebigen Zeitpunkt im Abrechnungszeitraum durchgeführt werden. Sie müssen jede Konvertierung bestätigen, damit diese wirksam wird.

* Ein Plan mit Vorauszahlung (**monatlich** oder **jährlich**) wird anteilig berechnet. Alle bis zum Zeitpunkt der Konvertierung ermittelten Überschreitungen werden in der nächsten Rechnung abgerechnet. Neue Überschreitungen werden auf Grundlage des neuen Plans in Rechnung gestellt.

>[!Note]
>Sie können Downgrades blockieren, wenn Sie bestimmte Konvertierungspfade nicht unterstützen möchten.

Die folgende Sequenz zeigt den Ablauf, wenn der Azure-Kunde den Plan in der SaaS-Benutzeroberfläche ändert:

![Ablauf einer Planänderung durch den Kunden](media/saas-metering-service-integration-flow-b.png)

Die folgende Sequenz zeigt den Ablauf, wenn der Azure-Kunde seinen Plan über die Storefront von Microsoft ändert:

![Ablauf einer Planänderung über die Storefront](media/saas-metering-service-integration-flow-c.png)

### <a name="how-can-the-azure-user-unsubscribe-from-the-plan-associated-with-azure-subscription"></a>Wie kann der Azure-Benutzer den mit dem Azure-Abonnement verbundenen Plan kündigen?

Ein Azure-Benutzer kann das erworbene SaaS-Angebotsabonnement entweder direkt in der SaaS-Benutzeroberfläche oder über die Microsoft-Plattform kündigen. Sobald der Benutzer das Abonnement kündigt, fallen ab dem nächsten Abrechnungszyklus keine Gebühren mehr an.

Die folgende Sequenz zeigt den Ablauf, wenn ein Azure-Kunde das SaaS-Angebot in der SaaS-Benutzeroberfläche kündigt:

![Kündigung des Abonnements über die SaaS-Benutzeroberfläche](media/saas-metering-service-integration-flow-d.png)

Die folgende Sequenz zeigt den Ablauf, wenn der Azure-Benutzer das Abonnement über die Storefront von Microsoft kündigt:

![Kündigung des Abonnements über die Storefront von Microsoft](media/saas-metering-service-integration-flow-e.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Marketplace-Messungsdienst-APIs](./marketplace-metering-service-apis.md).
