---
title: SaaS Fulfillment APIs – FAQ | Azure Marketplace
description: Auffindbarkeit und Kauferlebnis von Kunden eines SaaS-Angebots im Azure Marketplace.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: b324d3b9dca710dca6f5f99ad50ce4d973a42d2a
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67869333"
---
# <a name="saas-fulfillment-apis---faq"></a>SaaS-Fulfillment-APIs – Häufig gestellte Fragen

Die Integrationsanforderungen an den Azure Marketplace, damit Azure-Kunden das SaaS-Angebot abonnieren können, sind aufgeführt.

## <a name="discovery-experience"></a>Auffindbarkeit

Sobald das Angebot veröffentlicht ist, können Azure-Anwender das SaaS-Angebot im Azure Marketplace entdecken. Ihre Kunden können Angebote nach Produkttyp (SaaS) filtern und die SaaS-Services entdecken, die sie interessieren.

## <a name="purchase-experience"></a>Kauferfahrung

Sobald sich ein Benutzer für einen bestimmten SaaS-Service interessiert, kann er ihn über den Azure Marketplace abonnieren.

### <a name="what-does-it-mean-for-an-azure-user-to-subscribe-to-a-saas-offer-in-azure-marketplace"></a>Was bedeutet es für einen Azure-Nutzer, ein SaaS-Angebot auf dem Azure Marketplace zu abonnieren?

Das bedeutet, dass ein Benutzer die mit dem SaaS-Service verbundenen Nutzungsbedingungen und Datenschutzerklärungen einsehen und sich bereit erklären kann, sie gemäß den von Ihnen, dem Herausgeber des SaaS-Angebots, festgelegten Abrechnungsbedingungen über Microsoft zu erwerben. Benutzer können ihr bestehendes Zahlungsprofil in Azure verwenden, um den SaaS-Service zu bezahlen.

Dies ist aus vielen Gründen von Vorteil. Kunden können nun mit der Microsoft Cloud Platform als vertrauenswürdige Quelle an einem Ort alle Dienste entdecken und abonnieren, ohne jede ISV-Software untersuchen zu müssen, die sie verwenden wollen. Außerdem können Kunden ihr bestehendes Zahlungsprofil nutzen, ohne jede ISV-Software explizit selbst bezahlen zu müssen.

### <a name="is-the-user-charged-automatically-when-the-offer-is-subscribed"></a>Wird dem Benutzer bei der Anmeldung für das Angebot automatisch eine Gebühr berechnet?

Mit dem Abonnement des SaaS-Angebots hat sich der Nutzer bereit erklärt, die Nutzung des SaaS-Service über die Microsoft-Plattform zu bezahlen. Die Gebühren beginnen jedoch erst mit dem Einsatz des Angebots. Der Benutzer muss Ihr SaaS-Angebot aufrufen und die Account-Erstellung bestätigen, um mit der Nutzung des Angebots zu beginnen. Sie benachrichtigen Microsoft dann, dass die Abrechnung für dieses Kunden-SaaS-Abonnement beginnt.

### <a name="how-are-you-notified-when-a-user-subscribes-to-your-saas-offer"></a>Wie werden Sie benachrichtigt, wenn ein Benutzer Ihr SaaS-Angebot abonniert?

Nachdem er ein Angebot abonniert hat, kann der Azure-Benutzer alle seine Angebote in Azure entdecken und verwalten. Standardmäßig lautet der Status eines neu abonnierten SaaS-Angebots **„Bereitstellung, Erfüllung ausstehend“** . In diesem Zustand wird der Azure-Benutzer mit der Aktion **„Konto konfigurieren“** aufgefordert, um im Azure-Portal zu seiner SaaS-Abonnementverwaltung zu gelangen.

Wenn der Benutzer auf **„Konto konfigurieren“** klickt, wird er auf die SaaS-Service-Website weitergeleitet. Die URL, die er dabei aufruft, wird vom Herausgeber zum Zeitpunkt der Veröffentlichung des Angebots festgelegt. Diese Seite wird als Landing Page des Herausgebers bezeichnet. Azure-Benutzer sollten sich auf der SaaS-Landing Page anmelden können, basierend auf ihren bestehenden AAD-Anmeldeinformationen in Azure.

Wenn der Azure-Benutzer auf die Landing Page umgeleitet wird, wird der Such-URL ein Token hinzugefügt. Dieser Token ist temporär und gilt für eine Zeitdauer von 24 Stunden. Sie können dann die Existenz dieses Token erkennen und die Microsoft-API aufrufen, um mehr Kontext mit dem Token zu erhalten.

![Ablauf des Kundenabonnements](media/saas-metering-service-integration-flow-a.png)

Weitere Informationen zur Abwicklung von Transaktionsszenarien im Lebenszyklus eines SaaS-Angebots finden Sie im Dokument [„SaaS-Fulfillment-API“](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2).

### <a name="how-do-you-know-the-saas-offer-to-which-the-user-subscribes-in-azure"></a>Woher wissen Sie, welches SaaS-Angebot der Benutzer in Azure abonniert?

Die Antwort auf die `Resolve` API enthält Informationen über das Angebot und den Plan, die zum SaaS-Abonnement gehören.

### <a name="how-can-the-azure-user-change-the-plan-associated-with-this-azure-subscription"></a>Wie kann der Azure-Benutzer den mit diesem Azure-Abonnement verbundenen Plan ändern?

* Der Azure-Benutzer kann den mit dem SaaS-Abonnement verbundenen Plan direkt in der SaaS-Darstellung selber oder über die Microsoft-Plattform ändern.

* Die Konvertierung kann jederzeit im Abrechnungszeitraum durchgeführt werden. Sie müssen jede Konvertierung anerkennen, die mit der Bestätigung wirksam wird.

* Im Voraus bezahlte Plan (**monatlich** oder **jährlich**) wird anteilig berechnet. Alle bis zum Zeitpunkt der Konvertierung emittierten Überschüsse werden bei der nächsten Rechnung in Rechnung gestellt. Auf der Grundlage des neuen Plans werden neue Überschüsse emittiert.

>[!Note]
>Sie können Herabstufungen blockieren, wenn Sie bestimmte Konvertierungspfade nicht unterstützen möchten.

Die folgende Sequenz zeigt den Ablauf, wenn ein Azure-Kunde seinen Plan im SaaS-Bereich ändert:

![Änderungsablauf Kundenplan](media/saas-metering-service-integration-flow-b.png)

Die folgende Sequenz zeigt den Ablauf, wenn ein Azure-Kunde seinen Plan über Microsoft ändert

![Änderungsablauf durch den Kunden über die Storefront](media/saas-metering-service-integration-flow-c.png)

### <a name="how-can-the-azure-user-unsubscribe-from-the-plan-associated-with-azure-subscription"></a>Wie kann der Azure-Benutzer den mit dem Azure-Abonnement verbundenen Plan kündigen?

Ein Azure-Benutzer kann sich von einem gekauften SaaS-Angebot entweder direkt in der SaaS-Darstellung oder über die Microsoft-Plattform abmelden. Sobald sich der Benutzer abmeldet, wird ihm der nächste Abrechnungszeitraum nicht mehr in Rechnung gestellt.

Die folgende Sequenz erfasst den Ablauf, wenn ein Azure-Kunde das SaaS-Angebot in der SaaS-Darstellung storniert:

![Kunde kündigt Abonnement im Rahmen der SaaS-Darstellung](media/saas-metering-service-integration-flow-d.png)

Die folgende Sequenz zeigt den Ablauf, wenn sich der Azure-Benutzer das Abonnement in der Microsoft-Storefront storniert:

![Der Kunde kündigt sein Abonnement in der Microsoft-Storefront](media/saas-metering-service-integration-flow-e.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Marketplace metering service APIs](./marketplace-metering-service-apis.md) (Marketplace-Messungsdienst-APIs, in englischer Sprache).
