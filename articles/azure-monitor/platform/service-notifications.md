---
title: Anzeigen von Dienstintegritätsbenachrichtigungen im Azure-Portal
description: Mit Dienstintegritätsbenachrichtigungen können Sie von Microsoft Azure veröffentlichte Dienstintegritätsmeldungen anzeigen.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2018
ms.author: dukek
ms.subservice: logs
ms.openlocfilehash: 8cf8c3eb86f55b5595ef9a1af83ea50580bf638b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069353"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Anzeigen von Dienstintegritätsbenachrichtigungen im Azure-Portal

Benachrichtigungen zur Dienstintegrität werden von der Azure-Infrastruktur veröffentlicht. Sie enthalten Informationen über die Ressourcen in Ihrem Abonnement. Diese Benachrichtigungen sind eine Unterklasse der Aktivitätsprotokollereignisse und daher auch im Aktivitätsprotokoll zu finden. Dienstintegritätsbenachrichtigungen können je nach Klasse informativ oder handlungsrelevant sein.

Weitere Informationen zu den verschiedenen Klassen von Integritätsbenachrichtigungen finden Sie unter [Eigenschaften von Integritätsbenachrichtigungen](../../service-health/service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Anzeigen Ihrer Dienstintegritätsbenachrichtigungen im Azure-Portal

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Überwachen** aus.

    ![Screenshot des Menüs im Azure-Portal mit ausgewählter Überwachungsoption](./media/service-notifications/home-monitor.png)

    Azure Monitor fasst alle Überwachungseinstellungen und -daten in einer konsolidierten Ansicht zusammen. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

1. Wählen Sie **Warnungen**.

    ![Screenshot des Überwachungsaktivitätsprotokolls mit ausgewählten Warnungen](./media/service-notifications/service-health-summary.png)

1. Wählen Sie **+ Aktivitätsprotokollwarnung hinzufügen** aus, und richten Sie eine Warnung ein, um sicherzustellen, dass Sie zukünftige Dienstbenachrichtigungen erhalten. Weitere Informationen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Nächste Schritte

* Erhalten Sie [Warnungsbenachrichtigungen, wenn eine Dienstintegritätsbenachrichtigung](../../azure-monitor/platform/alerts-activity-log-service-notifications.md) gesendet wird.  
* Erfahren Sie mehr über [Aktivitätsprotokollwarnungen](../../azure-monitor/platform/activity-log-alerts.md).
