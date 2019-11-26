---
title: Verbessern des optimalen Betriebs für Ihre Azure-Abonnements mit Azure Advisor | Microsoft-Dokumentation
description: Verwenden Sie Advisor, um den optimalen Betrieb für Ihre Azure-Abonnements zu verbessern und weiterzuentwickeln.
services: advisor
documentationcenter: NA
author: sagupt
ms.service: advisor
ms.topic: article
ms.date: 10/24/2019
ms.author: sagupt
ms.openlocfilehash: 7cf3d3b34d0921cd111f8111bc2008ef0eced962
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73033205"
---
# <a name="achieve-operational-excellence-with-azure-advisor"></a>Sicherstellen des optimalen Betriebs mit Azure Advisor

Die Empfehlungen zum optimalen Betrieb von Azure Advisor unterstützen Kunden in Bezug auf die Prozess- und Workfloweffizienz, die Verwaltbarkeit von Ressourcen und bewährte Methoden für die Bereitstellung. Diese Empfehlungen von Advisor stehen auf der Registerkarte **Optimaler Betrieb** des Advisor-Dashboards zur Verfügung.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Erstellen von Azure Service Health-Warnungen für Benachrichtigungen bei Azure-Dienstproblemen

Es wird empfohlen, Azure Service Health-Warnungen einzurichten, um über Azure-Dienstprobleme benachrichtigt zu werden. [Azure Service Health](https://azure.microsoft.com/features/service-health/) ist ein kostenloser Dienst, der personalisierte Anleitungen und Unterstützung bietet, wenn Sie von einem Azure-Dienstproblem betroffen sind. Advisor ermittelt Abonnements, für die keine Warnungen konfiguriert sind, und empfiehlt, solche zu erstellen.

## <a name="design-your-storage-accounts-to-prevent-hitting-the-maximum-subscription-limit"></a>Entwerfen Ihrer Speicherkonten, damit die maximale Abonnementgrenze nicht erreicht wird

In einer Azure-Region können maximal 250 Speicherkonten pro Abonnement unterstützt werden. Wenn das Limit erreicht ist, können Sie keine weiteren Speicherkonten in dieser Region-Abonnement-Kombination mehr erstellen. Advisor überprüft Ihre Abonnements und zeigt Empfehlungen an, damit Sie weniger Speicherkonten für Abonnements entwerfen, bei denen das maximale Limit bald erreicht ist.

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>Sicherstellen, dass Sie bei Bedarf Zugang zu Azure-Cloudexperten haben

Bei unternehmenskritischen Workloads ist es wichtig, im Bedarfsfall Zugang zu technischem Support zu haben. Der Advisor ermittelt potenziell unternehmenskritische Abonnements, deren Supportplan keinen technischen Support enthält, und empfiehlt ein Upgrade auf eine Option mit technischem Support.

## <a name="repair-invalid-log-alert-rules"></a>Reparieren ungültiger Protokollwarnungsregeln

Azure Advisor erkennt Warnungsregeln mit ungültigen Abfragen im Bedingungsabschnitt. Protokollwarnungsregeln werden in Azure Monitor erstellt und verwendet, um in regelmäßigen Abständen Analytics-Abfragen durchzuführen. Anhand der Ergebnisse der Abfrage wird ermittelt, ob eine Warnung ausgelöst werden muss. Es kann vorkommen, dass Analytics-Abfragen aufgrund von Änderungen an referenzierten Ressourcen, Tabellen oder Befehlen im Laufe der Zeit ungültig werden. Von Advisor wird empfohlen, die Abfrage in der Warnungsregel zu korrigieren, um eine automatische Deaktivierung zu verhindern und die Überwachung Ihrer Ressourcen in Azure sicherzustellen. Weitere Informationen zur Behandlung von Problemen mit Warnungsregeln finden Sie [hier](https://aka.ms/aa_logalerts_queryrepair).

## <a name="follow-best-practices-using-azure-policy"></a>Befolgen der bewährten Methoden mithilfe von Azure Policy

Azure Policy ist ein Dienst in Azure, mit dem Sie Richtlinien erstellen, zuweisen und verwalten können. Mit diesen Richtlinien werden verschiedene Regeln und Auswirkungen für Ihre Ressourcen erzwungen. Die folgenden Azure Policy-Empfehlungen sollen Sie beim Sicherstellen des optimalen Betriebs unterstützen: 
1. Verwalten von Tags mit Azure Policy: Diese Richtlinie dient zum Hinzufügen oder Ersetzen des angegebenen Tags und des zugehörigen Werts, wenn eine Ressource erstellt oder aktualisiert wird. Bereits vorhandene Ressourcen können durch Auslösen eines Wartungstasks gewartet werden. Tags für Ressourcengruppen werden nicht geändert.
2. Erzwingen von geografischen Konformitätsanforderungen mit Azure Policy: Mit der Richtlinie können Sie die Speicherorte einschränken, die Ihre Organisation beim Bereitstellen von Ressourcen angeben kann. 
3. Angeben von zulässigen VM-SKUs für Bereitstellungen: Über diese Richtlinie können Sie einen Satz von SKUs für virtuelle Computer angeben, die Ihre Organisation bereitstellen kann.
4. Erzwingen von „Virtuelle Computer überwachen, die keine verwalteten Datenträger verwenden“ mit Azure Policy
5. Verwenden von „Tag von der Ressourcengruppe erben“ mithilfe von Azure Policy: Die Richtlinie dient zum Hinzufügen oder Ersetzen des angegebenen Tags aus der übergeordneten Ressourcengruppe und des zugehörigen Werts, wenn eine Ressource erstellt oder aktualisiert wird. Bereits vorhandene Ressourcen können durch Auslösen eines Wartungstasks gewartet werden.

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie weitere Informationen zu Empfehlungen des Advisor:
* [Einführung in Advisor](advisor-overview.md)
* [Erste Schritte](advisor-get-started.md)
* [Advisor-Empfehlungen zu Kosten](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen für Hochverfügbarkeit](advisor-high-availability-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)
