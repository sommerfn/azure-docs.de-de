---
title: Azure Traffic Manager | Microsoft Docs
description: Dieser Artikel enthält eine Übersicht zu Azure Traffic Manager. Erfahren Sie, ob dies die richtige Wahl für den Lastenausgleich des Benutzerdatenverkehrs für Ihre Anwendung ist.
services: traffic-manager
author: asudbring
manager: twooley
ms.service: traffic-manager
customer intent: As an IT admin, I want to learn about Traffic Manager and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/05/2019
ms.author: allensu
ms.openlocfilehash: 95ef493ed44da8fd8372c11f15c3ab712961783e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071024"
---
# <a name="what-is-traffic-manager"></a>Was ist Traffic Manager?
Azure Traffic Manager ist ein DNS-basierter Lastenausgleich für Datenverkehr, der es Ihnen ermöglicht, den Datenverkehr optimal auf Dienste in den globalen Azure-Regionen zu verteilen, und gleichzeitig Hochverfügbarkeit und Reaktionsfähigkeit sicherzustellen.

Traffic Manager verwendet DNS, um Clientanforderungen auf der Grundlage einer Datenverkehrsrouting-Methode und der Integrität der Endpunkte an den optimalen Endpunkt weiterzuleiten. Ein Endpunkt ist ein Dienst mit Internetzugriff, der innerhalb oder außerhalb von Azure gehostet wird. Traffic Manager bietet eine Reihe von [Datenverkehrsrouting-Methoden](traffic-manager-routing-methods.md) und [Endpunktüberwachungsoptionen](traffic-manager-monitoring.md), die verschiedene Anwendungsanforderungen erfüllen sowie automatisches Failover ermöglichen. Traffic Manager zeichnet sich durch eine geringe Fehleranfälligkeit aus, selbst wenn es zu einem Ausfall einer ganzen Azure-Region kommt.

>[!NOTE]
> Azure stellt eine Sammlung aus vollständig verwalteten Lastenausgleichslösungen für Ihre Szenarios bereit. Wenn Sie nach Informationen zur Beendigung der Transport Layer Security-Protokollierung oder zur Verarbeitung der Anwendungsschicht pro HTTP/HTTPS-Anforderung suchen, lesen Sie den Artikel [Application Gateway](../application-gateway/application-gateway-introduction.md). Informationen zum regionalen Lastenausgleich finden Sie unter [Load Balancer](../load-balancer/load-balancer-overview.md). Für Ihre End-to-End-Szenarios kann es von Vorteil sein, diese Lösungen nach Bedarf zu kombinieren.

Traffic Manager bietet folgende Funktionen:

## <a name="increase-application-availability"></a>Steigern der Anwendungsverfügbarkeit

Mit Traffic Manager können Sie die Hochverfügbarkeit wichtiger Anwendungen gewährleisten, indem Sie Ihre Endpunkte überwachen und beim Ausfall eines Endpunkts automatisches Failover bereitstellen.
    
## <a name="improve-application-performance"></a>Verbessern der Anwendungsleistung

Azure ermöglicht die Ausführung von Cloud-Diensten oder Websites in Datencentern auf der ganzen Welt. Traffic Manager kann die Reaktionsfähigkeit Ihrer Anwendungen verbessern und Endbenutzer an den Endpunkt mit der niedrigsten Netzwerklatenz für den Client weiterleiten.

## <a name="perform-service-maintenance-without-downtime"></a>Ausführen von Dienstwartungsarbeiten ohne Ausfallzeiten

Geplante Wartungsarbeiten für Ihre Anwendungen können ohne Ausfallzeiten für Ihre Anwendungen durchgeführt werden. Hierzu kann Traffic Manager den Datenverkehr während der Wartung an andere Endpunkte weiterleiten.

## <a name="combine-hybrid-applications"></a>Kombinieren von Hybridanwendungen

Traffic Manager unterstützt externe, Azure-fremde Endpunkte und kann somit in hybriden, cloudbasierten bzw. lokalen Bereitstellungen einschließlich „[burst-to-cloud](https://azure.microsoft.com/overview/what-is-cloud-bursting/)“ (Ausweichen in die Cloud bei Nachfragespitzen), „migrate-to-cloud“ (Migration in die Cloud) und „failover-to-cloud“ (Failover in die Cloud) verwendet werden.

## <a name="distribute-traffic-for-complex-deployments"></a>Verteilen des Datenverkehrs für komplexe Bereitstellungen

Mehrere Datenverkehrsrouting-Methoden können mit [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md) zu komplexen und flexiblen Regeln kombiniert werden, die auch den Anforderungen umfangreicher und komplexer Bereitstellungen gerecht werden.

## <a name="pricing"></a>Preise

Preisinformationen finden Sie unter [Traffic Manager – Preise](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Erstellen eines Traffic Manager-Profils](traffic-manager-create-profile.md).
- Erhalten Sie weitere Informationen zur [Funktionsweise von Traffic Manager](traffic-manager-how-it-works.md).
- Lesen Sie die [Häufig gestellten Fragen](traffic-manager-FAQs.md) zu Traffic Manager.




