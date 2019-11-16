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
ms.date: 10/23/2019
ms.author: rkarlin
ms.openlocfilehash: 32e7e79465b68caca9a636019f65b9f08284fc72
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73928218"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Verknüpfen von Microsoft Cloud App Security-Daten 



Sie können Protokolle von [Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) mit einem einzigen Klick in Azure Sentinel streamen. Durch diese Verbindung könen Sie die Warnungen von Cloud App Security in Azure Sentinel streamen. 

## <a name="prerequisites"></a>Voraussetzungen

- Ein Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen.
- Um Cloud Discovery-Protokolle in Azure Sentinel zu streamen, [aktivieren Sie Azure Sentinel als SIEM in Microsoft Cloud App Security](https://aka.ms/AzureSentinelMCAS).

> [!IMPORTANT]
> Die Erfassung von Cloud Discovery-Protokollen befindet sich derzeit in der öffentlichen Vorschauversion.
> Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
## <a name="connect-to-cloud-app-security"></a>Herstellen einer Verbindung mit Cloud App Security

Wenn Sie Cloud App Security bereits haben, vergewissern Sie sich, dass es [in Ihrem Netzwerk aktiviert ist](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security).
Wenn Cloud App Security bereitgestellt ist und Daten erhält, können die Warnungsdaten problemlos an Azure Sentinel gestreamt werden.


1. Wählen Sie in Azure Sentinel die Option für **Datenconnectors** aus, klicken Sie auf die Kachel **Cloud App Security**, und wählen Sie dann **Connectorseite öffnen** aus.

1. Wählen Sie aus, welche Protokolle in Azure Sentinel gestreamt werden sollen. Sie können **Warnungen** und **Cloud Discovery-Protokolle** (Vorschau) auswählen. 

1. Klicken Sie auf **Verbinden**.

1. Um das relevante Schema für die Cloud App Security-Warnungen in Log Analytics zu verwenden, suchen Sie nach **SecurityAlert**.




## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde erläutert, wie Sie Microsoft Cloud App Security mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
