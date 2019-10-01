---
title: Verknüpfen von Azure AD-Daten mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Active Directory-Daten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 43405bd3e97dbee325f0a5ed82c5848880775eee
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240815"
---
# <a name="connect-data-from-azure-active-directory"></a>Verknüpfen von Daten aus Azure Active Directory



Mit Azure Sentinel können Sie Daten aus [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) sammeln und in Azure Sentinel streamen. Sie können wählen, ob Sie [Anmeldeprotokolle](../active-directory/reports-monitoring/concept-sign-ins.md) und [Auditprotokolle](../active-directory/reports-monitoring/concept-audit-logs.md) streamen möchten.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie Anmeldedaten aus Active Directory exportieren möchten, müssen Sie über eine Azure AD P1- oder P2-Lizenz verfügen.

- Benutzer mit globalen Administrator- oder Sicherheitsadministratorberechtigungen für den Mandanten, von dem aus Sie die Protokolle streamen möchten.

- Um den Verbindungsstatus sehen zu können, müssen Sie die Berechtigung zum Zugreifen auf Azure AD-Diagnoseprotokolle haben. 


## <a name="connect-to-azure-ad"></a>Stellen Sie eine Verbindung mit Azure AD her.

1. Klicken Sie in Azure Sentinel auf **Data connectors** (Datenconnectors) und anschließend auf die Kachel **Azure Active Directory**.

1. Klicken Sie neben den Protokollen, die Sie in Azure Sentinel streamen möchten, auf **Verbinden**.

1. Sie können auswählen, ob die Warnungen von Azure AD automatisch Incidents in Azure Sentinel generieren sollen. Wählen Sie unter **Incidents erstellen** die Option **Aktivieren** aus, um die standardmäßige Analyseregel zu aktivieren, die automatisch Incidents aus im verbundenen Sicherheitsdienst generierten Warnungen erstellt. Anschließend können Sie diese Regel unter **Analytics** und anschließend unter **Aktive Regeln** bearbeiten.

1. Um das relevante Schema für die Azure AD-Warnungen in Log Analytics zu verwenden, suchen Sie nach **SigninLogs** und **AuditLogs**.




## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie Azure AD mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
