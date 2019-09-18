---
title: Verwalten von Sicherheitsvorfällen in Azure Security Center | Microsoft-Dokumentation
description: Dieses Dokument enthält hilfreiche Informationen zur Verwendung des Azure Security Centers zum Verwalten von Sicherheitsvorfällen.
services: security-center
author: memildin
manager: rkarlin
editor: ''
ms.service: security-center
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: memildin
ms.openlocfilehash: c492c4023df30eb51692aa4ae21fb938067c6ebb
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70873367"
---
# <a name="manage-security-incidents-in-azure-security-center"></a>Verwalten von Sicherheitsvorfällen in Azure Security Center

Das Selektieren und Untersuchen von Sicherheitswarnungen kann auch für sehr erfahrene Sicherheitsanalysten zeitaufwändig sein, und häufig ist nur schwer zu erkennen, wo überhaupt begonnen werden soll. Mithilfe von [Analysen](security-center-detection-capabilities.md) zum Verknüpfen der Informationen der einzelnen [Sicherheitswarnungen](security-center-managing-and-responding-alerts.md) kann Security Center Ihnen eine zentrale Übersicht über einen Angriffsversuch und alle zugehörigen Warnungen bieten. So können Sie schnell überblicken, welche Aktionen der Angreifer unternommen hat und welche Ressourcen betroffen sind.

In diesem Thema werden Vorfälle in Security Center erläutert und wie Sie deren Warnungen korrigieren können.

## <a name="what-is-a-security-incident"></a>Was ist ein Sicherheitsvorfall?

In Security Center ist ein Sicherheitsvorfall eine Aggregation aller Warnungen für eine Ressource, die [Kill Chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/)-Mustern entsprechen. Vorfälle werden in der Liste [Sicherheitswarnungen](security-center-managing-and-responding-alerts.md) angezeigt. Klicken Sie auf einen Vorfall, um die zugehörigen Warnungen anzuzeigen, wodurch Sie weitere Informationen zu den einzelnen Fällen erhalten können.

## <a name="managing-security-incidents"></a>Verwalten von Sicherheitsvorfällen

1. Im Security Center-Dashboard klicken Sie auf die Kachel **Sicherheitswarnungen**. Der Vorfall und die Warnungen werden aufgeführt. Beachten Sie, dass die Beschreibung des Sicherheitsvorfalls im Gegensatz zu anderen Warnungen ein anderes Symbol enthält.

    ![Sicherheitsvorfälle anzeigen](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. Um Details anzuzeigen, klicken Sie auf einen Vorfall. Auf dem Blatt **Sicherheitsvorfall erkannt** werden weitere Details angezeigt. Der Abschnitt **Allgemeine Informationen** bietet Erkenntnisse dazu, was die Sicherheitswarnung ausgelöst hat. Er enthält Informationen wie die Zielressource, die Quell-IP-Adresse (sofern zutreffend), ob die Warnung noch aktiv ist sowie Empfehlungen zur Korrektur.  

    ![Reagieren auf Sicherheitsvorfälle in Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-alert-incident.png)

1. Um weitere Informationen zu den einzelnen Warnungen anzuzeigen, klicken Sie auf die jeweilige. Die von Security Center vorgeschlagene Wiederherstellung variiert je nach Sicherheitshinweis.

   > [!NOTE]
   > Dieselbe Warnung kann als Teil eines Vorfalls vorhanden sein und als eigenständige Warnung angezeigt werden.

    ![Warnungsdetails](./media/security-center-incident/security-center-incident-alert.png)

1. Führen Sie die für jede Warnung angegebenen Korrekturschritte durch.

Weitere Informationen zu Warnungen finden Sie unter [Verwalten von und Reagieren auf Sicherheitswarnungen](security-center-managing-and-responding-alerts.md).

In den folgenden Themen finden Sie Informationen zu den verschiedenen Warnungen für bestimmte Ressourcentypen:

* [Warnungen für IaaS-VMs und -Server](security-center-alerts-iaas.md)
* [Warnungen für Native Compute](security-center-alerts-compute.md)
* [Warnungen für Datendienste](security-center-alerts-data-services.md)

In den folgenden Themen erfahren Sie, wie Security Center die verschiedenen Telemetriedaten nutzt, die im Rahmen der Azure-Infrastrukturintegration gesammelt werden, um zusätzliche Schutzebenen für in Azure bereitstellte Ressourcen zu implementieren:

* [Bedrohungserkennung für die Azure-Dienstebene in Azure Security Center](security-center-alerts-service-layer.md)
* [Integration mit Azure-Sicherheitsprodukten](security-center-alerts-integration.md)

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument haben Sie erfahren, wie Sie die Sicherheitsvorfallfunktion in Security Center verwenden. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Sicherheitswarnungen in Azure Security Center](security-center-alerts-overview.md).
* [Verwalten von Sicherheitswarnungen](security-center-managing-and-responding-alerts.md)
* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md)
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/)(Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.
