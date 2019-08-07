---
title: Azure AD PowerShell-Cmdlets für die Berichterstellung | Microsoft-Dokumentation
description: Referenz der Azure AD PowerShell-Cmdlets für die Berichterstellung.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 07/12/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d9d5fdb9091d51c67fd5d778bac189c08647741
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359750"
---
# <a name="azure-ad-powershell-cmdlets-for-reporting"></a>Azure AD PowerShell-Cmdlets für die Berichterstellung

Mit Azure AD-Berichten (Azure Active Directory) können Sie Details zu Aktivitäten rund um alle Schreibvorgänge in Ihrem Verzeichnis (Überwachungsprotokolle) und Authentifizierungsdaten (Anmeldeprotokolle) erhalten. Diese Informationen stehen zwar schon über die Microsoft Graph-API zur Verfügung, aber jetzt können Sie die gleichen Daten mithilfe der Azure AD-PowerShell-Cmdlets für die Berichterstellung abrufen.

In diesem Artikel erhalten Sie eine Übersicht über die PowerShell-Cmdlets für Überwachungs- und Anmeldeprotokolle.

## <a name="audit-logs"></a>Überwachungsprotokolle

[Überwachungsprotokolle](concept-audit-logs.md) ermöglichen die Nachverfolgung sämtlicher Änderungen, die von verschiedenen Features in Azure AD vorgenommen wurden. Hierzu zählen unter anderem Änderungen an Ressourcen in Azure AD, z. B. das Hinzufügen oder Entfernen von Benutzern, Apps, Gruppen, Rollen und Richtlinien.

Sie erhalten mit dem Cmdlet „Get-AzureADAuditDirectoryLogs“ Zugriff auf die Überwachungsprotokolle.


| Szenario                      | PowerShell-Befehl |
| :--                           | :--                |
| Anwendungsanzeigename      | Get-AzureADAuditDirectoryLogs -Filter "initiatedBy/app/displayName eq 'Azure AD Cloud Sync'" |
| Category (Kategorie)                      | Get-AzureADAuditDirectoryLogs -Filter "category eq 'Application Management'" |
| Datum und Uhrzeit der Aktivität            | Get-AzureADAuditDirectoryLogs -Filter "activityDateTime gt 2019-04-18" |
| Alle oben genannten Möglichkeiten              | Get-AzureADAuditDirectoryLogs -Filter "initiatedBy/app/displayName eq 'Azure AD Cloud Sync' and category eq 'Application Management' and activityDateTime gt 2019-04-18"|


Die folgende Abbildung zeigt ein Beispiel für diesen Befehl. 

![Die Schaltfläche für die Datenzusammenfassung](./media/reference-powershell-reporting/get-azureadauditdirectorylogs.png)



## <a name="sign-in-logs"></a>Anmeldeprotokolle

Die [Anmeldeprotokolle](concept-sign-ins.md) bieten Informationen zur Nutzung von verwalteten Anwendungen und Benutzeranmeldungsaktivitäten.

Sie erhalten mit dem Cmdlet „Get-AzureADAuditSignInLogs“ Zugriff auf die Anmeldeprotokolle.


| Szenario                      | PowerShell-Befehl |
| :--                           | :--                |
| Anzeigename des Benutzers             | Get-AzureADAuditSignInLogs -Filter "userDisplayName eq 'Timothy Perkins'" |
| Datum und Uhrzeit der Erstellung              | Get-AzureADAuditSignInLogs -Filter "createdDateTime gt 2019-04-18T17:30:00.0Z" (alles seit 5:30 Uhr am 18.04.) |
| Status                        | Get-AzureADAuditSignInLogs -Filter "status/errorCode eq 50105" |
| Anwendungsanzeigename      | Get-AzureADAuditSignInLogs -Filter "appDisplayName eq 'StoreFrontStudio [wsfed enabled]'" |
| Alle oben genannten Möglichkeiten              | Get-AzureADAuditSignInLogs -Filter "userDisplayName eq 'Timothy Perkins' and status/errorCode ne 0 and appDisplayName eq 'StoreFrontStudio [wsfed enabled]'" |


Die folgende Abbildung zeigt ein Beispiel für diesen Befehl. 

![Die Schaltfläche für die Datenzusammenfassung](./media/reference-powershell-reporting/get-azureadauditsigninlogs.png)



## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure AD-Berichte](overview-reports.md)
- [Bericht „Überwachungsprotokolle“](concept-audit-logs.md) 
- [Programmgesteuerter Zugriff auf Azure AD-Berichte](concept-reporting-api.md)
