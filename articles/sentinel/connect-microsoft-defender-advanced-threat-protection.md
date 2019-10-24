---
title: Verknüpfen von Microsoft Defender ATP-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Microsoft Defender Advanced Threat Protection-Daten mit Azure Sentinel verknüpfen.
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
ms.date: 10/13/2019
ms.author: rkarlin
ms.openlocfilehash: 19d496ebb61a3ceb47f69f661e30ab529dc64f3d
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2019
ms.locfileid: "72257307"
---
# <a name="connect-alerts-from-microsoft-defender-advanced-threat-protection"></a>Verknüpfen von Microsoft Defender Advanced Threat Protection-Warnungen 


> [!IMPORTANT]
> Die Erfassung von Microsoft Defender Advanced Threat Protection-Protokollen befindet sich derzeit in der Public Preview-Phase.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 

Sie können Protokolle von [Microsoft Defender Advanced Threat Protection](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) mit nur einem Klick an Azure Sentinel streamen. Diese Verbindung ermöglicht es Ihnen, die Warnungen von Microsoft Defender Advanced Threat Protection an Azure Sentinel zu streamen. 

## <a name="prerequisites"></a>Voraussetzungen

- Eine gültige Lizenz für Microsoft Defender Advanced Threat Protection, die wie unter [Überprüfen der Lizenzbereitstellung und Abschließen der Einrichtung für Microsoft Defender Advanced Threat Protection](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/licensing) beschrieben aktiviert wurde. 
- Sie müssen Administrator oder Sicherheitsadministrator für den Azure Sentinel-Mandanten sein.


## <a name="connect-to-microsoft-defender-advanced-threat-protection"></a>Herstellen einer Verbindung mit Microsoft Defender Advanced Threat Protection

Wenn Microsoft Defender Advanced Threat Protection bereitgestellt wurde und Ihre Daten erfasst, können die Warnungen problemlos an Azure Sentinel gestreamt werden.


1. Wählen Sie in Azure Sentinel die Option **Datenconnectors** aus, klicken Sie auf die Kachel **Microsoft Defender Advanced Threat Protection**, und wählen Sie **Connectorseite öffnen** aus.
1. Klicken Sie auf **Verbinden**. 
1. Suchen Sie nach **SecurityAlert**, um das relevante Schema in Log Analytics für die Defender ATP-Warnungen zu verwenden. (Der **Anbietername** lautet **MDATP**.)




## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie gelernt, wie Sie Microsoft Defender ATP mit Azure Sentinel verknüpfen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
