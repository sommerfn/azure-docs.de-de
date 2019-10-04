---
title: Verknüpfen von Cloud App Security-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Cloud App Security-Daten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 6e71028855344d083dedf3493682e1e27685de48
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240126"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Verknüpfen von Microsoft Cloud App Security-Daten 



Sie können Protokolle von [Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) mit einem einzigen Klick in Azure Sentinel streamen. Durch diese Verbindung könen Sie die Warnungen von Cloud App Security in Azure Sentinel streamen. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen

## <a name="connect-to-cloud-app-security"></a>Herstellen einer Verbindung mit Cloud App Security

Wenn Sie Cloud App Security bereits haben, vergewissern Sie sich, dass es [in Ihrem Netzwerk aktiviert ist](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security).
Wenn Cloud App Security bereitgestellt ist und Daten erhält, können die Warnungsdaten problemlos an Azure Sentinel gestreamt werden.


1. Wählen Sie in Azure Sentinel die Option **Data connectors** (Datenconnectors) aus, und klicken Sie dann auf die Kachel **Cloud App Security**.

1. Wählen Sie aus, welche Protokolle in Azure Sentinel gestreamt werden sollen. Sie können **Warnungen** auswählen. 

1. Sie können auswählen, ob die Warnungen von Microsoft Cloud App Security automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Incidents erstellen** die Option **Aktivieren** aus, um die standardmäßige Analyseregel zu aktivieren, die automatisch Incidents aus im verbundenen Sicherheitsdienst generierten Warnungen erstellt. Anschließend können Sie diese Regel unter **Analytics** und dann unter **Aktive Regeln** bearbeiten.

1. Klicken Sie auf **Verbinden**.

1. Um das relevante Schema für die Cloud App Security-Warnungen in Log Analytics zu verwenden, suchen Sie nach **SecurityAlert**.




## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde erläutert, wie Sie Microsoft Cloud App Security mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
