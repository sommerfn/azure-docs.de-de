---
title: Verknüpfen von Azure AD-Daten mit der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Active Directory-Daten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2019
ms.author: rkarlin
ms.openlocfilehash: ce4a57b8c266fe474fc2e6dd8f811fc7440e7ac6
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190439"
---
# <a name="connect-data-from-azure-active-directory"></a>Verknüpfen von Daten aus Azure Active Directory

> [!IMPORTANT]
> Azure Sentinel ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit Azure Sentinel können Sie Daten aus [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) sammeln und in Azure Sentinel streamen. Sie können wählen, ob Sie [Anmeldeprotokolle](../active-directory/reports-monitoring/concept-sign-ins.md) und [Auditprotokolle](../active-directory/reports-monitoring/concept-audit-logs.md) streamen möchten.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie Anmeldedaten aus Active Directory exportieren möchten, müssen Sie über eine Azure AD P1- oder P2-Lizenz verfügen.

- Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen für den Mandanten, von dem aus Sie die Protokolle streamen möchten.

- Um den Verbindungsstatus sehen zu können, müssen Sie die Berechtigung zum Zugreifen auf Azure AD-Diagnoseprotokolle haben. 


## <a name="connect-to-azure-ad"></a>Stellen Sie eine Verbindung mit Azure AD her.

1. Klicken Sie in Azure Sentinel auf **Data connectors** (Datenconnectors) und anschließend auf die Kachel **Azure Active Directory**.

2. Klicken Sie neben den Protokollen, die Sie in Azure Sentinel streamen möchten, auf **Verbinden**.

6. Um das relevante Schema für die Azure AD-Warnungen in Log Analytics zu verwenden, suchen Sie nach **SigninLogs** und **AuditLogs**.




## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie Azure AD mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
