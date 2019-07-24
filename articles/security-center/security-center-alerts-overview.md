---
title: Sicherheitswarnungen in Azure Security Center | Microsoft-Dokumentation
description: In diesem Dokument erfahren Sie, was Sicherheitswarnungen sind und welche Arten von Sicherheitswarnungen in Azure Security Center zur Verfügung stehen.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 1b71e8ad-3bd8-4475-b735-79ca9963b823
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 06f41bbfd97d5deb59e7bfd08615b2f28e256070
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571312"
---
# <a name="security-alerts-in-azure-security-center"></a>Sicherheitswarnungen in Azure Security Center

In diesem Artikel werden die verschiedenen Arten von Sicherheitswarnungen vorgestellt, die in Azure Security Center (ASC) verfügbar sind. Für die vielen verschiedenen Ressourcentypen stehen unterschiedliche Warnungen zur Verfügung. ASC generiert Warnungen sowohl für in Azure bereitgestellte Ressourcen als auch für Ressourcen, die lokal und in Hybrid Cloud-Umgebungen bereitgestellt wurden. 

## <a name="what-are-security-alerts"></a>Was sind Sicherheitswarnungen?

Warnungen sind die Benachrichtigungen, die Security Center generiert, wenn Bedrohungen für Ihre Ressourcen erkannt werden. Security Center priorisiert die Warnungen und listet sie zusammen mit den Informationen auf, die erforderlich sind, um das Problem schnell zu untersuchen. Security Center stellt außerdem Empfehlungen zur Reaktion auf einen Angriff bereit.

## <a name="how-does-security-center-detect-threats"></a>Wie funktioniert die Bedrohungserkennung von Security Center?

Security Center erfasst, analysiert und integriert Protokolldaten Ihrer Azure-Ressourcen, aus dem Netzwerk und aus verbundenen Partnerlösungen (etwa Firewalls und Lösungen zum Schutz von Endpunkten), um echte Bedrohungen zu erkennen und falsch positive Ergebnisse zu reduzieren. Security Center analysiert diese Informationen und korreliert sie häufig mit Informationen aus mehreren Quellen, um Bedrohungen zu identifizieren.

Die von ASC überwachten Ressourcen können in Azure oder auch lokal oder in Hybrid Cloud-Umgebungen bereitgestellt sein. Weitere Informationen zur Bedrohungserkennung und zur Reaktion auf Bedrohungen finden Sie unter [Erkennen von und Reagieren auf Bedrohungen mit Azure Security Center](security-center-detection-capabilities.md#asc-detects).

## <a name="security-alert-types"></a>Arten von Sicherheitswarnungen

In den folgenden Themen finden Sie Informationen zu den verschiedenen ASC-Warnungen für bestimmte Ressourcentypen:

* [Warnungen für IaaS-VMs und -Server](security-center-alerts-iaas.md)
* [Warnungen für Native Compute](security-center-alerts-compute.md)
* [Warnungen für Datendienste](security-center-alerts-data-services.md)

In den folgenden Themen erfahren Sie, wie Security Center die verschiedenen Telemetriedaten nutzt, die im Rahmen der Azure-Infrastrukturintegration gesammelt werden, um zusätzliche Schutzebenen für in Azure bereitstellte Ressourcen zu implementieren:

* [Bedrohungserkennung für die Azure-Dienstebene in Azure Security Center](security-center-alerts-service-layer.md)
* [Integration of Azure Security products in Azure Security Center](security-center-alerts-integration.md) (Integration von Azure-Sicherheitsprodukten in Azure Security Center)

## <a name="what-are-alert-incidents"></a>Was sind Warnungsincidents?

Ein Sicherheitsincident ist eine Sammlung verwandter Warnungen (anstelle einer Auflistung der einzelnen Warnungen). In Security Center werden verschiedene Warnungen und Signale mit geringer Genauigkeit mittels Fusion zu Sicherheitsincidents korreliert.

Dank Incidents bietet Ihnen Security Center eine zentrale Ansicht für einen Angriffsversuch und alle zugehörigen Warnungen. In dieser Ansicht können Sie schnell nachvollziehen, welche Aktionen der Angreifer ausgeführt hat und welche Ressourcen betroffen sind. Weitere Informationen hierzu finden Sie unter [Cloud smart alert correlation in Azure Security Center (incidents)](security-center-alerts-cloud-smart.md) (Korrelation von intelligenten Cloudwarnungen in Azure Security Center (Incidents)).

## <a name="get-started-with-alerts"></a>Erste Schritte mit Warnungen

Die folgenden Themen enthalten weitere Informationen zu den von ASC überwachten Ressourcen, und Sie erfahren, wie Sie auf die von ASC bereitgestellten Warnungen reagieren können.

* Welche Plattformen und Features durch ASC geschützt werden, erfahren Sie unter [Von Azure Security Center unterstützte Features und Plattformen](security-center-os-coverage.md).  
* Was Sicherheitsincidents sind und wie ASC auf sie reagiert, erfahren Sie unter [Behandeln von Sicherheitsincidents in Azure Security Center](security-center-incident.md). 
* Informationen zur Verwaltung der erhaltenen Warnungen finden Sie unter [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md).
* Unter [Warnungsüberprüfung in Azure Security Center](security-center-alert-validation.md) erfahren Sie, wie Sie überprüfen, ob Security Center ordnungsgemäß konfiguriert ist, und wie Sie eine Testwarnung auslösen.  


## <a name="upgrade-to-standard-for-advanced-detections"></a>Upgraden auf den Standard-Tarif für erweiterte Erkennungsfunktionen

Führen Sie ein Upgrade auf Azure Security Center Standard durch, um erweiterte Erkennungsfunktionen einzurichten. 

1. Wählen Sie im Menü von Security Center die Option **Sicherheitsrichtlinie** aus.
2. Klicken Sie für die Abonnements, die Sie auf den Standard-Tarif umstellen möchten, auf **Einstellungen bearbeiten**. 
3. Wählen Sie auf der Einstellungsseite die Option **Tarif** aus. 
   Eine kostenlose Testversion ist einen Monat lang verfügbar. Weitere Informationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/security-center/). 

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, was Sicherheitswarnungen sind und welche Arten von Sicherheitswarnungen in Security Center verfügbar sind. Weitere Informationen finden Sie in den folgenden Themen:

* [Planungs- und Betriebshandbuch für Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)
* [Azure Security Center – Häufig gestellte Fragen](https://docs.microsoft.com/azure/security-center/security-center-faq): Enthält häufig gestellte Fragen zur Verwendung des Diensts.

