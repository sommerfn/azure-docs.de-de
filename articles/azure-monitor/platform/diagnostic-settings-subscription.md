---
title: Erfassen des Azure-Aktivitätsprotokolls mit Diagnoseeinstellungen (Vorschau) – Azure Monitor | Microsoft-Dokumentation
description: Verwenden Sie Diagnoseeinstellungen, um Azure-Aktivitätsprotokolle an Azure Monitor-Protokolle, Azure Storage oder Azure Event Hubs weiterzuleiten.
author: bwren
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
ms.author: bwren
ms.date: 10/31/2019
ms.openlocfilehash: 9f8783dc6d3c14b086364639b60273dbae626cee
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73586991"
---
# <a name="collect-azure-activity-log-with-diagnostic-settings-preview"></a>Erfassen des Azure-Aktivitätsprotokolls mit Diagnoseeinstellungen (Vorschau)
Das [Azure-Aktivitätsprotokoll](activity-logs-overview.md) ist ein [Plattformprotokoll](platform-logs-overview.md), das einen Einblick in Ereignisse auf Abonnementebene bietet, die in Azure aufgetreten sind. Bisher haben Sie ein Protokollprofil erstellt, um Aktivitätsprotokolleinträge an [einen Event Hub oder ein Speicherkonto](activity-log-export.md) zu senden, und einen Connector verwendet, um sie in einem [Log Analytics-Arbeitsbereich](activity-log-collect.md) zu erfassen.

Sie können jetzt die Erfassung des Azure-Aktivitätsprotokolls mit denselben [Diagnoseeinstellungen](diagnostic-settings.md) konfigurieren, die auch zum Erfassen von [Ressourcenprotokollen](resource-logs-overview.md) verwendet werden. Das Verwenden von Diagnoseeinstellungen bietet gegenüber den derzeitigen Methoden die folgenden Vorteile:

- Konsistente Methode zum Erfassen aller Plattformprotokolle
- Erfassen des Aktivitätsprotokolls übergreifend für mehrere Abonnements und Mandanten
- Filtern der Erfassung, um nur Protokolle für bestimmte Kategorien zu erfassen

## <a name="considerations"></a>Überlegungen
Beachten Sie die folgenden Details der Aktivitätsprotokollerfassung mithilfe von Diagnoseeinstellungen, bevor Sie diese Funktion aktivieren.

- Sie sollten die vorhandene Erfassung der Aktivität deaktivieren, bevor Sie die Erfassung mithilfe von Diagnoseeinstellungen aktivieren. Wenn beide Erfassungsmethoden aktiviert sind, kann dies zu doppelten Daten führen.
- Die Aufbewahrungseinstellung für die Erfassung des Aktivitätsprotokolls in Azure Storage wurde entfernt. Das bedeutet, dass Daten unbegrenzt gespeichert werden, bis Sie sie entfernen.
- Sie können das Aktivitätsprotokoll von mehreren Abonnements an einen einzelnen Log Analytics-Arbeitsbereich senden. Sie können das Aktivitätsprotokoll von einem einzelnen Abonnement an bis zu fünf Log Analytics-Arbeitsbereiche senden.

## <a name="configure-diagnostic-settings"></a>Konfigurieren von Diagnoseeinstellungen
Mithilfe der folgenden Vorgehensweise können Sie eine Diagnoseeinstellung im Azure-Portal zum Erfassen des Azure-Aktivitätsprotokolls erstellen. Sie können derzeit mit keiner anderen Methode eine Diagnoseeinstellung auf Abonnementebene erstellen.

1. Deaktivieren Sie die vorhandene Erfassung im Log Analytics-Arbeitsbereich für das Aktivitätsprotokoll:
   1. Öffnen Sie im Azure-Portal das Menü **Log Analytics-Arbeitsbereiche**, und wählen Sie den Arbeitsbereich zum Erfassen des Aktivitätsprotokolls aus.
   2. Wählen Sie im Menü des Arbeitsbereichs im Abschnitt **Arbeitsbereichsdatenquellen** die Option **Azure-Aktivitätsprotokoll** aus.
   3. Klicken Sie auf das Abonnement, das Sie trennen möchten.
   4. Klicken Sie auf **Trennen** und dann auf **Ja**, wenn Sie zum Bestätigen Ihrer Auswahl aufgefordert werden.
2. Wählen Sie im Azure-Portal im Menü **Azure Monitor** die Option **Aktivitätsprotokoll** aus.
3. Klicken Sie auf **Diagnoseeinstellungen**.
   
   ![Diagnoseeinstellungen](media/diagnostic-settings-subscription/diagnostic-settings.png)
   
4. Klicken Sie auf das lila Banner, um die Legacybenutzeroberfläche anzuzeigen, und deaktivieren Sie alle derzeitigen Erfassungen im Speicher oder in Event Hubs. 

    ![Legacybenutzeroberfläche](media/diagnostic-settings-subscription/legacy-experience.png)

5. Führen Sie die Verfahren unter [Erstellen von Diagnoseeinstellungen im Azure-Portal](diagnostic-settings.md#create-diagnostic-settings-in-azure-portal) aus, um eine Diagnoseeinstellung zu erstellen. Eine Erläuterung der Kategorien, die Sie zum Filtern von Ereignissen aus dem Aktivitätsprotokoll verwenden können, finden Sie unter [Kategorien im Aktivitätsprotokoll](activity-logs-overview.md#categories-in-the-activity-log). 


## <a name="differences-in-data"></a>Unterschiede bei den Daten
Mit Diagnoseeinstellungen werden dieselben Daten erfasst wie mit den vorherigen Methoden zur Erfassung des Aktivitätsprotokolls, jedoch derzeit mit folgenden Unterschieden:

Die folgenden Eigenschaften wurden entfernt:

- ActivityStatus
- ActivitySubstatus
- OperationName
- ResourceProvider 

Die folgenden Eigenschaften wurden hinzugefügt:

- Authorization_d
- Claims_d
- Properties_d

## <a name="activity-log-monitoring-solution"></a>Überwachungslösung des Aktivitätsprotokolls
Die Azure Log Analytics-Überwachungslösung umfasst mehrere Protokollabfragen und Ansichten zum Analysieren der Aktivitätsprotokolldatensätze in Ihrem Log Analytics-Arbeitsbereich. Diese Lösung verwendet Protokolldaten, die in einem Log Analytics-Arbeitsbereich erfasst werden, und kann weiterhin ohne Änderungen verwendet werden, wenn Sie das Aktivitätsprotokoll mithilfe von Diagnoseeinstellungen erfassen. Ausführliche Informationen zu dieser Lösung finden Sie unter [Überwachungslösung der Aktivitätsprotokollanalyse](activity-log-collect.md#activity-logs-analytics-monitoring-solution).

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zum Aktivitätsprotokoll](../../azure-resource-manager/resource-group-audit.md)
* [Weitere Informationen zu Diagnoseeinstellungen](diagnostic-settings.md)
