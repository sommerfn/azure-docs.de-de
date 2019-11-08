---
title: Aktualisieren auf die dynamische Überwachung der Einhaltung gesetzlicher Bestimmungen in Ihrem Azure Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen | Microsoft-Dokumentation
description: Aktualisieren der Compliancepakete (Vorschau)
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: f76b5443d6c1e3fd2cebf87cba39ba0a6bbdeaee
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73521428"
---
# <a name="update-to-dynamic-compliance-packages-in-your-regulatory-compliance-dashboard-preview"></a>Aktualisieren auf dynamische Compliancepakete in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen (Vorschau)

Azure Security Center vergleicht die Konfiguration Ihrer Ressourcen kontinuierlich mit den Anforderungen von Branchenstandards, Vorschriften und Benchmarks. Das **Dashboard für die Einhaltung gesetzlicher Bestimmungen** bietet Erkenntnisse zu Ihrem Compliancestatus basierend auf der Erfüllung bestimmter Compliancevorgaben und -anforderungen.

Ein Standard, für den Sie den Compliancestatus verfolgen können, ist [Azure CIS 1.1.0](https://www.cisecurity.org/benchmark/azure/) (genauer: „CIS Microsoft Azure Foundations Benchmark Version 1.1.0“). 

Die Darstellung von Azure CIS, die zu Beginn in Ihrem Compliance-Dashboard angezeigt wird, basiert auf einem statischen Satz von Regeln, der in Security Center enthalten ist.

Mit der Funktion **dynamischer Compliancepakete (Vorschauversion)** verbessert Security Center automatisch die Abdeckung von Branchenstandards. Compliancepakete sind im Wesentlichen in Azure Policy definierte Initiativen. Sie können dem ausgewählten Bereich (Abonnement, Verwaltungsgruppe usw.) zugewiesen werden. Um Compliancedaten als zugeordnete Bewertungen in Ihrem Dashboard anzuzeigen, fügen Sie der Verwaltungsgruppe oder dem Abonnement innerhalb der Sicherheitsrichtlinie ein Compliancepaket hinzu. Durch das Hinzufügen eines Compliancepakets wird die gesetzliche Complianceinitiative dem ausgewählten Bereich zugewiesen. Auf diese Weise können Sie neu veröffentlichte gesetzliche Initiativen als Compliancestandards in Ihrem Dashboard nachverfolgen. Wenn Microsoft neue Inhalte für die Initiative veröffentlicht (neue Richtlinien, die mehr Regeln im Standard entsprechen), werden die zusätzlichen Inhalte automatisch in Ihrem Dashboard hinzugefügt.

Das dynamische Compliancepaket für den Azure CIS-Benchmark, **Azure CIS 1.1.0 (neu)** , verbessert die ursprüngliche *statische* Version durch Folgendes:

* Einschließen weiterer Richtlinien
* Automatisches Aktualisieren mit neuer Abdeckung direkt beim Hinzufügen 

Aktualisieren Sie wie unten beschrieben auf das neue dynamische Paket.

## <a name="adding-a-dynamic-compliance-package"></a>Hinzufügen dynamischer Compliancepakete

In den folgenden Schritten wird erläutert, wie Sie das dynamische Paket zum Überwachen Ihrer Compliance mit dem Azure CIS-Benchmark Version 1.1.0 hinzufügen.   

### <a name="update-to-the-azure-cis-110-new-dynamic-compliance-package"></a>Aktualisieren des dynamischen Compliancepakets für Azure CIS 1.1.0 (neu) 

1. Öffnen Sie die Seite **Sicherheitsrichtlinie**. Auf dieser Seite werden die Anzahl von Verwaltungsgruppen, Abonnements und Arbeitsbereichen sowie Ihre Verwaltungsgruppenstruktur angezeigt.

1. Wählen Sie das Abonnement oder die Verwaltungsgruppe aus, für das bzw. die Sie den Status der Einhaltung gesetzlicher Bestimmungen verwalten möchten. Es wird empfohlen, den höchsten Bereich auszuwählen, für den der Standard gilt, damit Compliancedaten für alle geschachtelten Ressourcen aggregiert und nachverfolgt werden. 

1. Im Abschnitt „Industrie- und gesetzliche Standards (Vorschau)“ wird angezeigt, dass Azure CIS 1.1.0 für neue Inhalte aktualisiert werden kann. Klicken Sie auf **Jetzt aktualisieren**. 

1. Klicken Sie optional auf **Weitere Standards hinzufügen**, um die Seite **Standards zur Einhaltung gesetzlicher Bestimmungen hinzufügen** zu öffnen. Dort können Sie manuell nach **Azure CIS 1.1.0 (neu)** und dynamischen Paketen für andere Compliancestandards wie **NIST SP 800-53 R4**, **SWIFT CSP CSCF-v2020**, **UKO und UK NHS** und **Canada PBMM** suchen.
    
    ![Hinzufügen von Compliancepaketen zum Dashboard für die Einhaltung gesetzlicher Bestimmungen in Azure Security Center](./media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-additional-standards.png)


1. Wählen Sie auf der Seitenleiste in Security Center **Einhaltung gesetzlicher Bestimmungen** aus, um das Dashboard für die Einhaltung gesetzlicher Bestimmungen zu öffnen. 
    * Azure CIS 1.1.0 (neu) wird jetzt in der Liste der Branchen- und gesetzlichen Standards aufgeführt. 
    * Die ursprüngliche *statische* Ansicht Ihrer Azure CIS 1.1.0-Compliance bleibt ebenfalls erhalten. Sie wird möglicherweise zukünftig automatisch entfernt.

    > [!NOTE]
    > Es kann einige Stunden dauern, bis ein neu hinzugefügter Standard im Compliance-Dashboard angezeigt wird.


    [![Dashboard für die Einhaltung gesetzlicher Bestimmungen mit neuem und altem Azure CIS](media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-cis-old-and-new.png)](media/update-regulatory-compliance-packages/security-center-dynamic-regulatory-compliance-cis-old-and-new.png#lightbox)


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde Folgendes beschrieben:

* **Aktualisieren der in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen angezeigten Standards** auf die neuen *dynamischen* Pakete
* **Hinzufügen von Compliancepaketen** zum Überwachen Ihrer Compliance mit zusätzlichen Standards 

Weitere verwandte Informationen finden Sie in den folgenden Artikeln: 

- [Security Center-Dashboard für die Einhaltung gesetzlicher Bestimmungen](security-center-compliance-dashboard.md)
- [Arbeiten mit Sicherheitsrichtlinien](tutorial-security-policy.md)
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Erfahren Sie, wie Sie Empfehlungen in Azure Security Center nutzen, um Ihre Azure-Ressourcen zu schützen.
- [Azure Security Center – häufig gestellte Fragen:](security-center-faq.md) Erhalten Sie Antworten auf häufig gestellte Fragen zur Verwendung von Security Center.