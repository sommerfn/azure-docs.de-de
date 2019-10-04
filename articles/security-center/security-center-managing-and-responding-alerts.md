---
title: Verwalten von Sicherheitswarnungen in Azure Security Center | Microsoft-Dokumentation
description: In diesem Dokument erfahren Sie, wie Sie Azure Security Center-Funktionen verwenden, um Sicherheitswarnungen zu verwalten und auf diese zu reagieren.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: b88a8df7-6979-479b-8039-04da1b8737a7
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/27/2019
ms.author: memildin
ms.openlocfilehash: c3134988452efe16044ab7b7740cafd02e8cf27a
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71201920"
---
# <a name="manage-and-respond-to-security-alerts-in-azure-security-center"></a>Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center

In diesem Thema wird gezeigt, wie Sie die empfangenen Warnungen anzeigen und verarbeiten, um Ihre Ressourcen zu schützen. 

* Weitere Informationen zu den unterschiedlichen Arten von Warnungen finden Sie unter [Arten von Sicherheitswarnungen](security-center-alerts-overview.md#security-alert-types).
* Eine Übersicht darüber, wie Security Center Warnungen generiert, finden Sie unter [Erkennen von und Reagieren auf Bedrohungen mit Azure Security Center](security-center-alerts-overview.md#detect-threats).

> [!NOTE]
> Führen Sie ein Upgrade auf Azure Security Center Standard durch, um erweiterte Erkennungsfunktionen zu aktivieren. Eine kostenlose Testversion ist verfügbar. Wenn Sie ein Upgrade durchführen möchten, wählen Sie in der [Sicherheitsrichtlinie](tutorial-security-policy.md)die Tarifoption aus. Weitere Informationen finden Sie unter [Azure Security Center Preise](security-center-pricing.md).

## <a name="what-are-security-alerts"></a>Was sind Sicherheitswarnungen?
Security Center erfasst, analysiert und vereinigt automatisch Protokolldaten von Ihren Azure-Ressourcen, vom Netzwerk und von verbundenen Partnerlösungen, z.B. Lösungen zum Schutz von Firewalls und Endpunkten, um echte Bedrohungen zu erkennen und falsch positive Ergebnisse zu reduzieren. Eine Liste mit priorisierten Sicherheitswarnungen wird im Security Center zusammen mit den Informationen angezeigt, die Sie zum schnellen Untersuchen des Problems benötigen. Außerdem sind Empfehlungen zum Reagieren auf einen Angriff vorhanden.

> [!NOTE]
> Weitere Informationen darüber, wie die Erkennungsfunktionen von Security Center funktionieren, finden Sie unter [Erkennen von und Reagieren auf Bedrohungen mit Azure Security Center](security-center-alerts-overview.md#detect-threats).

## <a name="manage-your-security-alerts"></a>Verwalten von Sicherheitswarnungen

1. Auf dem Security Center-Dashboard finden Sie auf der Kachel **Bedrohungsschutz** eine Übersicht über die Warnungen.

    ![Kachel „Sicherheitswarnungen“ in Security Center](./media/security-center-managing-and-responding-alerts/security-center-dashboard-alert.png)

1. Klicken Sie auf die Kachel, um weitere Details zu den Warnungen anzuzeigen.

   ![Die Sicherheitswarnungen in Security Center](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Zum Filtern der angezeigten Warnungen klicken Sie auf **Filter** und wählen auf dem daraufhin geöffneten Blatt **Filter** die Filteroptionen aus, die Sie anwenden möchten. Die Liste wird entsprechend dem ausgewählten Filter aktualisiert. Diese Filterung kann sehr nützlich sein. Es könnte beispielsweise sein, dass Sie während der Untersuchung einer möglichen Sicherheitsverletzung im System die Sicherheitswarnungen überprüfen möchten, die in den letzten 24 Stunden aufgetreten sind.

    ![Filtern von Warnungen in Security Center](./media/security-center-managing-and-responding-alerts/security-center-filter-alerts.png)

## <a name="respond-to-security-alerts"></a>Reagieren auf Sicherheitswarnungen

1. Klicken Sie in der Liste **Sicherheitswarnungen** auf eine Sicherheitswarnung. Die betroffenen Ressourcen und die Schritte, die Sie zum Abwehren eines Angriffs ausführen müssen, werden angezeigt.

    ![Reagieren auf Sicherheitswarnungen](./media/security-center-managing-and-responding-alerts/security-center-alert.png)

1. Klicken Sie nach dem Überprüfen der Informationen auf eine Ressource, die angegriffen wurde.

    ![Vorschläge für Maßnahmen bei Sicherheitswarnungen](./media/security-center-managing-and-responding-alerts/security-center-alert-remediate.png)

    Der Abschnitt **Allgemeine Informationen** bietet Erkenntnisse dazu, was die Sicherheitswarnung ausgelöst hat. Er enthält Informationen wie die Zielressource, die Quell-IP-Adresse (sofern zutreffend), ob die Warnung noch aktiv ist sowie Empfehlungen zur Behebung.  

    > [!NOTE]
    >In einigen Fällen ist die IP-Quelladresse nicht verfügbar, da einige Windows-Sicherheitsereignisprotokolle die IP-Adresse nicht enthalten.

1. Die von Security Center vorgeschlagene Wartungsschritte variieren je nach Sicherheitswarnung. Befolgen Sie sie für jede Warnung. In einigen Fällen müssen Sie möglicherweise andere Azure-Steuerelemente oder -Dienste verwenden, um die empfohlene Maßnahme für eine Warnung der Bedrohungserkennung umzusetzen. 

    In den folgenden Themen finden Sie Informationen zu den verschiedenen Warnungen für bestimmte Ressourcentypen:
    
    * [Warnungen für IaaS-VMs und -Server](security-center-alerts-iaas.md)
    * [Warnungen für Native Compute](security-center-alerts-compute.md)
    * [Warnungen für Datendienste](security-center-alerts-data-services.md)
    
    In den folgenden Themen erfahren Sie, wie Security Center die verschiedenen Telemetriedaten nutzt, die im Rahmen der Azure-Infrastrukturintegration gesammelt werden, um zusätzliche Schutzebenen für in Azure bereitstellte Ressourcen zu implementieren:
    
    * [Bedrohungserkennung für die Azure-Dienstebene in Azure Security Center](security-center-alerts-service-layer.md)
    * [Integration mit Azure-Sicherheitsprodukten](security-center-alerts-integration.md)
    
## <a name="see-also"></a>Weitere Informationen

In diesem Dokument haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Security Center konfigurieren können. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Sicherheitswarnungen in Azure Security Center](security-center-alerts-overview.md)
* [Behandeln von Sicherheitsincidents](security-center-incident.md)
* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md)
* [Azure Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.
