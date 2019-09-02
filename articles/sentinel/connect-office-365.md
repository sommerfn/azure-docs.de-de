---
title: Herstellen der Verbindung von Office 365-Daten mit Azure Sentinel Preview | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Verbindung von Office 365-Daten mit Azure Sentinel herstellen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: ff7c862e-2e23-4a28-bd18-f2924a30899d
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2019
ms.author: rkarlin
ms.openlocfilehash: 39245cb43dacfeec2b647936d5e5790d4a185467
ms.sourcegitcommit: 3f78a6ffee0b83788d554959db7efc5d00130376
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2019
ms.locfileid: "70018998"
---
# <a name="connect-data-from-office-365-logs"></a>Herstellen der Verbindung von Daten aus Office 365-Protokollen

> [!IMPORTANT]
> Azure Sentinel ist derzeit als öffentliche Vorschauversion (Public Preview) verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können Überwachungsprotokolle von [Office 365](https://docs.microsoft.com/office365/admin/admin-home?view=o365-worldwide) mit einem einzigen Klick an Azure Sentinel streamen. Sie können Überwachungsprotokolle von mehreren Mandanten an einen einzigen Arbeitsbereich in Azure Sentinel streamen. Der Office 365-Aktivitätsprotokoll-Connector bietet Einblick in die aktuellen Benutzeraktivitäten. Sie erhalten Informationen über unterschiedliche Benutzer-, Administrator-, System- und Richtlinienaktionen und -ereignisse aus Office 365. Durch Verbinden von Office 365-Protokollen mit Azure Sentinel können Sie diese Daten verwenden, um Dashboards anzuzeigen, benutzerdefinierte Warnungen zu erstellen und Ihren Untersuchungsprozess zu verbessern.

> [!IMPORTANT]
> Wenn Sie über eine E3-Lizenz verfügen, müssen Sie, bevor Sie über die Verwaltungsaktivitäts-API von Office 365 auf Daten zugreifen können, die einheitliche Überwachungsprotokollierung für Ihre Office 365-Organisation aktivieren. Hierzu aktivieren Sie das Office 365-Überwachungsprotokoll. Anweisungen finden Sie unter [Aktivieren oder Deaktivieren der Office 365-Überwachungsprotokollsuche](https://docs.microsoft.com/office365/securitycompliance/turn-audit-log-search-on-or-off). Weitere Informationen finden Sie unter [Office 365-Verwaltungsaktivitäts-API – Referenz](https://docs.microsoft.com/office/office-365-management-api/office-365-management-activity-api-reference).

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen globaler Administrator oder Sicherheitsadministrator für Ihren Mandanten sein.
- Stellen Sie sicher, dass auf dem Computer, von dem Sie sich bei Azure Sentinel zum Erstellen der Verbindung angemeldet haben, Port 4433 für Webdatenverkehr geöffnet ist.
- Wenn Ihr Mandant nicht über eine Office 365 E3- oder Office 365 E5-Lizenz verfügt, müssen Sie die einheitliche Überwachung auf Ihrem Mandanten mithilfe eines der folgenden Prozesse aktivieren:
    - [Verwenden des Cmdlets „Set-AdminAuditLogConfig“](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-audit/set-adminauditlogconfig?view=exchange-ps) und Aktivieren des Parameters „UnifiedAuditLogIngestionEnabled“.
    - [Oder Verwenden der Benutzeroberfläche des Security & Compliance Centers](https://docs.microsoft.com/office365/securitycompliance/search-the-audit-log-in-security-and-compliance#before-you-begin).

## <a name="connect-to-office-365"></a>Herstellen einer Verbindung mit Office 365

1. Wählen Sie in Azure Sentinel die Option **Datenkonnektoren** aus, und klicken Sie dann auf die Kachel **Office 365**.

2. Wenn Sie die Office 365-Lösung noch nicht aktiviert haben, verwenden Sie unter **Verbindung** die Schaltfläche **Aktivieren**, um die Aktivierung vorzunehmen. War die Office 365-Lösung bereits aktiviert, wird sie in der Verbindungsanzeige als bereits aktiviert gekennzeichnet.
1. Office 365 ermöglicht es Ihnen, Daten von mehreren Mandanten an Azure Sentinel zu streamen. Für jeden Mandanten, mit dem Sie eine Verbindung herstellen möchten, fügen Sie den Mandanten unter **Mandanten mit Azure Sentinel verbinden** (Connect tenants to Azure Sentinel) hinzu. 
1. Es wird eine Active Directory-Anzeige geöffnet. Sie werden aufgefordert, sich für jeden Mandanten, den Sie mit Azure Sentinel verbinden möchten, mit einem globalen Administratorbenutzer zu authentifizieren und für Azure Sentinel Berechtigungen bereitzustellen, um dessen Protokolle lesen zu können. 
5. Klicken Sie unter „Office 365-Aktivitätsprotokolle streamen“ auf **Auswählen**, und wählen Sie die Protokolltypen aus, die an Azure Sentinel gestreamt werden sollen. Derzeit unterstützt Azure Sentinel Exchange und SharePoint.

4. Klicken Sie auf **Änderungen übernehmen**.

3. Um das entsprechende Schema in Log Analytics für die Office 365-Protokolle zu verwenden, suchen Sie nach **OfficeActivity**.


## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie Office 365 mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).

