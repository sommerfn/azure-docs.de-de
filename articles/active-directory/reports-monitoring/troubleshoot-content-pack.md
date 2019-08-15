---
title: Behandlung von Fehlern am Inhaltspaket von Azure Active Directory-Aktivitätsprotokollen | Microsoft-Dokumentation
description: In diesem Artikel werden eine Liste von Fehlermeldungen zum Inhaltspaket von Azure Active Directory-Aktivitätsprotokollen und die Schritte aufgeführt, um diese zu beheben.
services: active-directory
documentationcenter: ''
author: cawrites
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 06/07/2019
ms.author: chadam
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54a0a5b5306414eb50a1928ec8a1854f56055681
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68987905"
---
# <a name="troubleshooting-azure-active-directory-activity-logs-content-pack-errors"></a>Behandlung von Fehlern am Inhaltspaket von Azure Active Directory-Aktivitätsprotokollen 

|  |
|--|
|Das Azure AD-Power-BI-Inhaltspaket verwendet derzeit die Azure AD Graph-APIs zum Abrufen von Daten des Azure AD-Mandanten. Daher ergibt sich möglicherweise ein Unterschied zwischen den Daten im Inhaltspaket und den Daten, die mit den [Microsoft Graph-APIs für die Berichterstellung](concept-reporting-api.md) abgerufen wurden. |
|  |

Bei der Arbeit mit dem Power BI-Inhaltspaket für Azure Active Directory (Azure AD) können eventuell folgende Fehler auftreten: 

- [Fehler beim Aktualisieren](troubleshoot-content-pack.md#refresh-failed) 
- [Fehler beim Aktualisieren der Anmeldeinformationen für die Datenquelle](troubleshoot-content-pack.md#failed-to-update-data-source-credentials) 
- [Der Import von Daten dauert zu lange.](#data-import-is-too-slow) 

In diesem Artikel finden Sie Informationen zu den möglichen Ursachen und zur Behebung dieser Fehler.
 
## <a name="refresh-failed"></a>Fehler beim Aktualisieren 
 
**So werden Sie auf diesen Fehler aufmerksam**: E-Mail von Power BI oder Fehlerstatus im Aktualisierungsverlauf. 


| Ursache | So behebt man den Fehler |
| ---   | ---        |
| Aktualisierungsfehler können auftreten, wenn die Anmeldeinformationen der Benutzer, die eine Verbindung mit dem Inhaltspaket herstellen, zurückgesetzt, aber nicht in den Verbindungseinstellungen des Inhaltspakets aktualisiert wurden. | Suchen Sie in Power BI das Dataset, das dem Dashboard für Azure AD-Aktivitätsprotokolle (**Azure Active Directory-Aktivitätsprotokolle**) entspricht. Wählen Sie die geplante Aktualisierung aus, und geben Sie dann Ihre Azure AD-Anmeldeinformationen ein. |
| Eine Aktualisierung kann aufgrund großer Datasets fehlschlagen. | Zurzeit kann das Azure AD-Inhaltspaket mit Power BI aufgrund von Einschränkungen bei Zeitlimits im Power BI-Dienst nur kleine Datasets (weniger als 500,00 Zeilen) unterstützen. Wenn Drosselungsfehler auftreten oder die Aktualisierung aufgrund von Zeitlimitproblemen fehlschlägt, versuchen Sie möglicherweise, ein großes Dataset abzurufen. Reduzieren Sie die Zeitspanne in der Abfrage, und versuchen Sie es erneut.|
 
 
## <a name="failed-to-update-data-source-credentials"></a>Fehler beim Aktualisieren der Anmeldeinformationen für die Datenquelle 
 
**So werden Sie auf diesen Fehler aufmerksam**: In Power BI beim Herstellen einer Verbindung mit dem Inhaltspaket des Azure AD-Aktivitätsprotokolls. 

| Ursache | So behebt man den Fehler |
| ---   | ---        |
| Der Benutzer, der eine Verbindung herstellt, ist kein globaler Administrator, kein Sicherheitsleseberechtigter bzw. kein Sicherheitsadministrator. | Verwenden Sie für den Zugriff auf die Inhaltspakete ein Konto, dem entweder die Rolle eines globalen Administrators, eines Sicherheitsleseberechtigten oder eines Sicherheitsadministrators zugewiesen ist. |
| Ihr Mandant ist kein Premium-Mandant oder weist keinen Benutzer mit einer Premium-Lizenzdatei auf. | [Öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 


## <a name="data-import-is-too-slow"></a>Der Datenimport ist zu langsam 
 
**So werden Sie auf diesen Fehler aufmerksam**: Nachdem Sie in Power BI eine Verbindung mit Ihrem Inhaltspaket hergestellt haben, wird der Datenimportvorgang gestartet, und Ihr Dashboard wird für Azure AD-Aktivitätsprotokolle vorbereitet. Es wird folgende Nachricht angezeigt: **Daten werden importiert** wird ohne weitere Fortschritte angezeigt.  

| Ursache | So behebt man den Fehler |
| ---   | ---        |
| Je nach Größe Ihres Mandanten kann dieser Schritt bis zu 30 Minuten dauern. | Wenn die Meldung innerhalb einer Stunde nicht ausgeblendet und Ihr Dashboard nicht angezeigt wird, [öffnen Sie ein Supportticket](../fundamentals/active-directory-troubleshooting-support-howto.md).|

## <a name="next-steps"></a>Nächste Schritte

* [Installieren des Power BI-Inhaltspakets für Azure AD-Berichte](quickstart-install-power-bi-content-pack.md)
* [Verwenden des Power BI-Inhaltspakets für Azure AD-Berichte zum Visualisieren von Daten](howto-power-bi-content-pack.md)
* [Erhalten von Support für Azure Active Directory](../fundamentals/active-directory-troubleshooting-support-howto.md)
