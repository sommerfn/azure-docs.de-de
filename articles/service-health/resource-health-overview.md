---
title: Übersicht über Azure Resource Health | Microsoft-Dokumentation
description: Übersicht über Azure Resource Health
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.date: 05/10/2019
ms.openlocfilehash: 9c2096f94f38d13288c6ce3742252bc6d576835a
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854226"
---
# <a name="resource-health-overview"></a>Übersicht über Resource Health
 
Azure Resource Health unterstützt Sie bei der Diagnose und bei Supportanfragen, wenn Dienstprobleme Auswirkungen auf Ihre Azure-Ressourcen haben. Der Dienst erstellt Berichte zur aktuellen und früheren Integrität Ihrer Ressourcen.

[Azure-Status](https://status.azure.com) bietet Berichte zu Dienstproblemen, die sich auf eine breite Palette von Azure-Kunden auswirken. Resource Health stellt die Integrität Ihrer Ressourcen in einem personalisierten Dashboard dar. Resource Health zeigt alle Zeitpunkte an, zu denen Ihre Ressourcen aufgrund von Azure-Dienstproblemen nicht verfügbar waren. Mit diesen Daten können Sie leicht feststellen, ob gegen eine SLA verstoßen wurde.

## <a name="resource-definition-and-health-assessment"></a>Ressourcendefinition und Integritätsbewertung

*Eine Ressource* ist eine bestimmte Instanz eines Azure-Diensts, z. B. ein virtueller Computer, eine Web-App oder eine SQL-Datenbank. Resource Health ermittelt anhand von Signalen der verschiedenen Azure-Dienste, ob eine Ressource fehlerfrei ist. Wenn eine Ressource fehlerhaft ist, analysiert Resource Health zusätzliche Informationen, um die Quelle des Problems zu bestimmen. Der Dienst erstellt auch Berichte zu Aktionen, die von Microsoft zum Beheben des Problems ergriffen werden, und gibt Maßnahmen an, die Sie zur Behebung unternehmen können.

Weitere Informationen zur Bewertung der Integrität entnehmen Sie der Liste von Ressourcentypen und Integritätsprüfungen in [Azure Resource Health](resource-health-checks-resource-types.md).

## <a name="health-status"></a>Integritätsstatus

Die Integrität einer Ressource wird als einer der folgenden Status angezeigt.

### <a name="available"></a>Verfügbar

*Verfügbar* bedeutet, dass keine Ereignisse erkannt werden, die sich auf die Integrität der Ressource auswirken. In Fällen, in denen die Ressource während der letzten 24 Stunden nach einer ungeplanten Ausfallzeit wiederhergestellt wurde, wird die Benachrichtigung „Kürzlich behandelt“ angezeigt.

![Status „Verfügbar“ bei einem virtuellen Computer mit der Benachrichtigung „Kürzlich behandelt“](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Nicht verfügbar

*Nicht verfügbar* bedeutet, dass der Dienst ein aktives Plattform- oder Nicht-Plattformereignis erkannt hat, das Auswirkungen auf die Integrität der Ressource hat.

#### <a name="platform-events"></a>Plattformereignisse

Plattformereignisse werden von mehreren Komponenten der Azure-Infrastruktur ausgelöst. Dazu zählen sowohl geplante Aktionen (z.B. geplante Wartung) als auch unerwartete Vorfälle (z.B. ein nicht geplanter Neustart eines Hosts).

Resource Health stellt weitere Details zum Ereignis und zum Wiederherstellungsprozess bereit. Darüber hinaus können Sie sich selbst dann an den Microsoft-Support wenden, wenn Sie nicht über eine aktive Supportvereinbarung verfügen.

![Status „Nicht verfügbar“ bei einem virtuellen Computer aufgrund eines Plattformereignisses](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Nicht-Plattformereignisse

Nicht-Plattformereignisse werden durch Benutzeraktionen ausgelöst. Beispiele dafür sind das Anhalten eines virtuellen Computers oder das Erreichen der maximalen Anzahl von Verbindungen mit einer Azure Cache for Redis-Instanz.

![Status „Nicht verfügbar“ bei einem virtuellen Computer aufgrund eines Nicht-Plattformereignisses](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Unknown

*Unbekannt* bedeutet, dass Resource Health seit mehr als 10 Minuten keine Informationen mehr zu der Ressource empfangen hat. Dieser Status ist zwar keine definitive Angabe des Ressourcenzustands, aber ein wichtiger Datenpunkt für die Problembehandlung.

Wenn die Ressource wie erwartet ausgeführt wird, wird der Status der Ressource nach wenigen Minuten in *Verfügbar* geändert.

Wenn Probleme mit der Ressource auftreten, kann der Integritätsstatus *Unbekannt* bedeuten, dass die Ressource durch ein Ereignis auf der Plattform beeinträchtigt wird.

![Status „Unbekannt“ bei einem virtuellen Computer](./media/resource-health-overview/Unknown.png)

### <a name="degraded"></a>Heruntergestuft

*Heruntergestuft* bedeutet, dass die Ressource einen Leistungsverlust festgestellt hat, obwohl sie noch für die Nutzung verfügbar ist.

Verschiedene Ressourcen verwenden eigene Kriterien, anhand derer sie angeben, dass sie einen Leistungsverlust erleiden.

![Status „Heruntergestuft“ bei einem virtuellen Computer](./media/resource-health-overview/degraded.png)

## <a name="reporting-an-incorrect-status"></a>Melden eines falschen Status

Wenn Sie glauben, dass der aktuelle Integritätsstatus falsch ist, können Sie uns dies durch die Auswahl von **Falschen Integritätsstatus melden** mitteilen. In Fällen, in denen Sie von einem Azure-Problem betroffen sind, empfehlen wir Ihnen, sich in Resource Health an den Support zu wenden.

![Formular zum Übermitteln von Informationen zu einem falschen Status](./media/resource-health-overview/incorrect-status.png)

## <a name="history-information"></a>Verlaufsinformationen

Im Abschnitt **Integritätsverlauf** von Resource Health stehen bis zu 14 Tage alte Verlaufsdaten zur Verfügung.

![Liste der Resource Health-Ereignisse in den letzten zwei Wochen](./media/resource-health-overview/history-blade.png)

## <a name="get-started"></a>Erste Schritte

Öffnen von Resource Health für eine Ressource:

1. Melden Sie sich beim Azure-Portal an.
2. Navigieren Sie zu der Ressource.
3. Wählen Sie im linken Bereich im Ressourcenmenü die Option **Ressourcenintegrität** aus.

![Öffnen von Resource Health über die Ressourcenansicht](./media/resource-health-overview/from-resource-blade.png)

Sie können auch auf Resource Health zugreifen, indem Sie **Alle Dienste** auswählen und im Filtertextfeld **Resource Health** eingeben. Wählen Sie im Bereich **Hilfe und Support** die Option [Resource Health](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth) aus.

![Öffnen von Resource Health über „Alle Dienste“](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Resource Health finden Sie in den folgenden Referenzen:
-  [Ressourcentypen und Integritätsprüfungen in Azure Resource Health](resource-health-checks-resource-types.md)
-  [Azure Resource Health – FAQ](resource-health-faq.md)
