---
title: Anzeigen von Dienstintegritätsbenachrichtigungen im Azure-Portal
description: Mit Dienstintegritätsbenachrichtigungen können Sie von Microsoft Azure veröffentlichte Dienstintegritätsmeldungen anzeigen.
author: stephbaron
ms.author: stbaron
services: monitoring
ms.service: service-health
ms.topic: conceptual
ms.date: 6/27/2019
ms.subservice: ''
ms.openlocfilehash: d2e18ae28e79590cb04ee0045341ea817be4a3bc
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538142"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Anzeigen von Dienstintegritätsbenachrichtigungen im Azure-Portal

Integritätsbenachrichtigungen werden von der Azure-Infrastruktur in das [Azure Activity Log](../azure-monitor/platform/activity-logs-overview.md) veröffentlicht.  Die Benachrichtigungen enthalten Informationen über die Ressourcen in Ihrem Abonnement. Angesichts der möglicherweise großen Menge an Informationen, die im Aktivitätsprotokoll gespeichert sind, gibt es eine separate Benutzeroberfläche, um die Anzeige und Einrichtung von Warnmeldungen zu Servicezustandsmeldungen zu erleichtern. 

Dienstintegritätsbenachrichtigungen können je nach Klasse informativ oder handlungsrelevant sein.

Weitere Informationen zu den verschiedenen Klassen von Integritätsbenachrichtigungen finden Sie unter [Eigenschaften von Integritätsbenachrichtigungen](service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Anzeigen Ihrer Dienstintegritätsbenachrichtigungen im Azure-Portal

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Überwachen** aus.

    ![Screenshot des Menüs im Azure-Portal mit ausgewählter Überwachungsoption](./media/service-notifications/home-monitor.png)

    Azure Monitor fasst alle Überwachungseinstellungen und -daten in einer konsolidierten Ansicht zusammen. Zunächst wird der Abschnitt **Aktivitätsprotokoll** geöffnet.

1. Wählen Sie **Warnungen**.

    ![Screenshot des Überwachungsaktivitätsprotokolls mit ausgewählten Warnungen](./media/service-notifications/service-health-summary.png)

1. Wählen Sie **+ Aktivitätsprotokollwarnung hinzufügen** aus, und richten Sie eine Warnung ein, um sicherzustellen, dass Sie zukünftige Dienstbenachrichtigungen erhalten. Weitere Informationen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts.md).
