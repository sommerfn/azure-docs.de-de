---
title: Installieren des Azure AD-Power BI-Inhaltspakets | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie das Azure AD-Power BI-Inhaltspaket installieren.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
ms.assetid: fd5604eb-1334-4bd8-bfb5-41280883e2b5
ms.service: active-directory
ms.workload: identity
ms.subservice: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: f088c8b49fa6485a21b630738149078b7ff45b7d
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68988102"
---
# <a name="quickstart-install-azure-active-directory-power-bi-content-pack"></a>Schnellstart: Installieren des Azure Active Directory-Power BI-Inhaltspakets

|  |
|--|
|Das Azure AD-Power-BI-Inhaltspaket verwendet derzeit die Azure AD Graph-APIs zum Abrufen von Daten des Azure AD-Mandanten. Daher ergibt sich möglicherweise ein Unterschied zwischen den Daten im Inhaltspaket und den Daten, die mit den [Microsoft Graph-APIs für die Berichterstellung](concept-reporting-api.md) abgerufen wurden. |
|  |

Mit dem Power BI-Inhaltspaket für Azure Active Directory (Azure AD) können Sie die Berichtsdaten aus Ihrer Umgebung visualisieren. Sie können das vorab erstellte Inhaltspaket herunterladen und verwenden, um alle Aktivitäten in Ihrem Verzeichnis zu melden, indem Sie die umfassende Visualisierungsumgebung von Power BI nutzen. Sie können auch Ihr eigenes Dashboard erstellen und es auf einfache Weise für alle anderen Benutzer Ihrer Organisation freigeben. 

In dieser Schnellstartanleitung wird beschrieben, wie Sie das Inhaltspaket installieren.

## <a name="prerequisites"></a>Voraussetzungen

Für die Durchführung dieses Schnellstarts benötigen Sie Folgendes:

* Ein Power BI-Konto. Dies ist dasselbe Konto wie Ihr O365- oder Azure AD-Konto. 
* Ihre Azure AD-Mandanten-ID. Dies ist die **Verzeichnis-ID** Ihres Verzeichnisses von der [Eigenschaftenseite](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties) des Azure-Portals.
* Eine Lizenz vom Typ Azure AD Premium (P1/P2). Unter [Erste Schritte mit Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) erfahren Sie, wie Sie ein Upgrade für Ihre Azure Active Directory-Edition durchführen.

## <a name="install-azure-ad-power-bi-content-pack"></a>Installieren des Azure AD-Power BI-Inhaltspakets 

1. Melden Sie sich mit Ihrem Power BI-Konto an [Power BI](https://app.powerbi.com/groups/me/getdata/services) an. Dies ist dasselbe Konto wie Ihr O365- oder Azure AD-Konto.

2. Suchen Sie auf der Seite**Apps** nach **Azure Active Directory-Aktivitätsprotokolle**, und wählen Sie die Option **Jetzt anfordern**. 

   ![Azure Active Directory-Power BI-Inhaltspaket](./media/quickstart-install-power-bi-content-pack/getitnow.png) 
    
3. Geben Sie im Popupfenster Ihre Azure AD-Mandanten-ID und dann **7** als Anzahl von Tagen für die Abfrage ein. Wählen Sie anschließend **Weiter**.
    
   ![Azure Active Directory-Power BI-Inhaltspaket](./media/quickstart-install-power-bi-content-pack/connect.png) 

4. Nachdem das Dashboard für die Azure Active Directory-Aktivitätsprotokolle erstellt wurde, können Sie es auswählen.

   ![Azure Active Directory-Power BI-Inhaltspaket](./media/quickstart-install-power-bi-content-pack/dashboard.png) 
    
## <a name="next-steps"></a>Nächste Schritte

* [Verwenden des Power BI-Inhaltspakets](howto-power-bi-content-pack.md)
* [Problembehandlung bei Fehlern mit dem Inhaltspaket](troubleshoot-content-pack.md)
