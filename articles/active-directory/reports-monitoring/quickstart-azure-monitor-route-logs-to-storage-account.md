---
title: 'Tutorial: Archivieren von Verzeichnisprotokollen in einem Speicherkonto | Microsoft-Dokumentation'
description: Es wird beschrieben, wie Sie die Azure-Diagnose einrichten, um Azure Active Directory-Protokolle per Pushvorgang in ein Speicherkonto zu übertragen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa2cac7c89a2cb1bd99675727bcd51e82c4a18ef
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74007864"
---
# <a name="tutorial-archive-azure-ad-logs-to-an-azure-storage-account"></a>Tutorial: Archivieren von Azure AD-Protokollen in einem Azure-Speicherkonto

In diesem Tutorial erfahren Sie, wie Sie Azure Monitor-Diagnoseeinstellungen einrichten, um Azure AD-Protokolle (Azure Active Directory) an ein Azure-Speicherkonto weiterzuleiten.

## <a name="prerequisites"></a>Voraussetzungen 

Sie benötigen Folgendes, um dieses Feature verwenden zu können:

* Ein Azure-Abonnement mit einem Azure-Speicherkonto. Falls Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/free/).
* Einen Azure AD-Mandanten.
* Einen Benutzer, der als *globaler Administrator* oder *Sicherheitsadministrator* für den Azure AD-Mandanten fungiert.

## <a name="archive-logs-to-an-azure-storage-account"></a>Archivieren von Protokollen in einem Azure-Speicherkonto

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 

2. Klicken Sie auf **Azure Active Directory** > **Aktivität** > **Überwachungsprotokolle**. 

3. Klicken Sie auf **Einstellungen exportieren**. 

4. Führen Sie im Bereich **Diagnoseeinstellungen** einen der folgenden Schritte aus:
   * Klicken Sie zum Ändern vorhandener Einstellungen auf **Einstellung bearbeiten**.
   * Klicken Sie zum Hinzufügen neuer Einstellungen auf **Diagnoseeinstellungen hinzufügen**.  
     Sie können maximal drei Einstellungen verwenden. 

     ![Exporteinstellungen](./media/quickstart-azure-monitor-route-logs-to-storage-account/ExportSettings.png)

5. Geben Sie einen Anzeigenamen für die Einstellung ein, der den Zweck der Einstellung wiedergibt (beispielsweise *An Azure Storage-Konto senden*). 

6. Aktivieren Sie das Kontrollkästchen **In ein Speicherkonto archivieren**, und wählen Sie dann **Speicherkonto** aus. 

7. Wählen Sie das Azure-Abonnement und das Speicherkonto aus, an das Sie die Protokolle weiterleiten möchten.
 
8. Klicken Sie auf **OK**, um die Konfiguration zu beenden.

9. Führen Sie einen oder beide der folgenden Schritte aus:
    * Aktivieren Sie das Kontrollkästchen **AuditLogs**, um Überwachungsprotokolle an das Speicherkonto zu senden. 
    * Aktivieren Sie das Kontrollkästchen **SignInLogs**, um Anmeldeprotokolle an das Speicherkonto zu senden.

10. Verwenden Sie den Schieberegler, um die Aufbewahrungsdauer für Ihre Protokolldaten festzulegen. Standardmäßig ist dieser Wert auf *0* festgelegt, d.h. die Protokolle werden im Speicherkonto unbegrenzt beibehalten. Wenn Sie einen anderen Wert festlegen, werden Ereignisse, die älter als die ausgewählte Anzahl von Tagen sind, automatisch bereinigt.

11. Klicken Sie auf **Speichern**, um die Einstellung zu speichern.

    ![Diagnoseeinstellungen](./media/quickstart-azure-monitor-route-logs-to-storage-account/DiagnosticSettings.png)

12. Überprüfen Sie nach ungefähr 15 Minuten, ob die Protokolle in Ihr Speicherkonto gepusht werden. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), klicken Sie auf **Speicherkonten**, wählen Sie das Speicherkonto aus, das Sie zuvor verwendet haben, und klicken Sie dann auf **BLOBs**. Wählen Sie für **Überwachungsprotokolle** die Option **insights-log-audit** aus. Wählen Sie für **Anmeldeprotokolle** die Option **insights-logs-signin** aus.

    ![Speicherkonto](./media/quickstart-azure-monitor-route-logs-to-storage-account/StorageAccount.png)

## <a name="next-steps"></a>Nächste Schritte

* [Interpret audit logs schema in Azure Monitor](reference-azure-monitor-audit-log-schema.md) (Interpretieren des Überwachungsprotokollschemas in Azure Monitor)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Interpretieren des Anmeldeprotokollschemas in Azure Monitor)
* [Häufig gestellte Fragen und bekannte Probleme](concept-activity-logs-azure-monitor.md#frequently-asked-questions)