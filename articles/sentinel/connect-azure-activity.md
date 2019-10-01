---
title: Verknüpfen von Azure-Aktivitätsdaten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Aktivitätsdaten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 807a5ede3feee115b1a8dc51fe14966731fc7784
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240778"
---
# <a name="connect-data-from-azure-activity-log"></a>Verknüpfen von Daten aus dem Azure-Aktivitätsprotokoll



Sie können Protokolle aus dem [Azure-Aktivitätsprotokoll](../azure-monitor/platform/activity-logs-overview.md) mit einem einzigen Klick an Azure Sentinel streamen. Das Aktivitätsprotokoll ist ein Abonnementprotokoll, das Einblicke in Ereignisse auf Abonnementebene ermöglicht, die in Azure aufgetreten sind. Dies schließt einen Datenbereich von Azure Resource Manager-Betriebsdaten bis hin zu Aktualisierungen für Dienstintegritätsereignisse ein. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für jeden Schreibvorgang (PUT, POST, DELETE) ermitteln, der für die Ressourcen Ihres Abonnements ausgeführt wurde. Sie können auch den Status des Vorgangs und andere relevante Eigenschaften verstehen. Das Aktivitätsprotokoll umfasst keine Lesevorgänge (GET) oder Vorgänge für Ressourcen, die das klassische Modell/RDFE-Modell verwenden. 


## <a name="prerequisites"></a>Voraussetzungen

- Ein Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen.


## <a name="connect-to-azure-activity-log"></a>Herstellen einer Verbindung mit dem Aktivitätsprotokoll

1. Wählen Sie in Azure Sentinel die Option **Data connectors** (Datenconnectors) aus, und klicken Sie dann auf die Kachel **Azure-Aktivitätsprotokoll**.

2. Wählen Sie im Bereich für das Azure-Aktivitätsprotokoll die Abonnements aus, die Sie an Azure Sentinel streamen möchten. 

3. Klicken Sie auf **Verbinden**.

4. Um das relevante Schema für die Azure-Aktivitätswarnungen in Log Analytics zu verwenden, suchen Sie nach **AzureActivity**.


 

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie das Azure-Aktivitätsprotokoll mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
