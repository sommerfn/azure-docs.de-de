---
title: Zu erfassende Daten beim Öffnen eines Falls für Microsoft Azure Automation | Microsoft-Dokumentation
description: In diesem Artikel werden einige der grundlegenden Informationen beschrieben, die Sie sammeln sollten, bevor Sie beim Microsoft Azure-Support eine Anfrage für Azure Automation öffnen.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/23/2019
ms.author: v-miegge
ms.openlocfilehash: 83ee78d369af7fe99de8e7236fe1eb0bc63a942f
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2019
ms.locfileid: "72302147"
---
# <a name="data-to-collect-when-you-open-a-case-for-microsoft-azure-automation"></a>Zu erfassende Daten beim Öffnen eines Falls für Microsoft Azure Automation

In diesem Artikel werden einige der grundlegenden Informationen beschrieben, die Sie sammeln sollten, bevor Sie beim Microsoft Azure-Support eine Anfrage für Azure Automation öffnen. Diese Informationen sind nicht erforderlich, um eine Anfrage zu öffnen. Sie können Microsoft aber dabei helfen, Ihr Problem schneller zu lösen. Darüber hinaus besteht die Möglichkeit, dass der Supportmitarbeiter Sie nach dem Öffnen der Anfrage um diese Daten bittet.

## <a name="collect-more-information"></a>Sammeln weiterer Informationen

Bevor Sie eine Anfrage beim Support für Microsoft Azure Automation öffnen, empfehlen wir Ihnen, die folgenden Informationen zu sammeln.

### <a name="basic-data-collection"></a>Grundlegende Datensammlung

Sammelt die Daten, die im folgenden Knowledge Base-Artikel beschrieben werden:

* [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics) How to capture Azure Automation-scripted diagnostics (Skriptgesteuerte Erfassung von Azure Automation-Diagnosedaten)

## <a name="collect-data-depending-on-issue"></a>Problembezogenes Sammeln von Daten
 
### <a name="for-issues-that-affect-update-management-on-linux"></a>Für Probleme, die die Updateverwaltung unter Linux betreffen

1. Führen Sie über die in KB [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics) aufgeführten Punkte hinaus das folgende Tool zur Protokollerfassung aus:

   [OMS Linux Agent-Protokollsammler](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md)
 
2. Komprimieren Sie den Inhalt des folgenden Ordners, und senden Sie die komprimierte Datei dann an den Azure-Support:

   ``/var/opt/microsoft/omsagent/run/automationworker/``
 
3. Überprüfen Sie, ob die Arbeitsbereichs-ID, an die der OMS Linux-Agent berichtet, mit dem Arbeitsbereich übereinstimmt, der auf Updates überwacht wird.

### <a name="for-issues-that-affect-update-management-on-windows"></a>Für Probleme, die die Updateverwaltung unter Windows betreffen

Führen Sie über die in [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics) aufgelisteten Punkte hinaus Folgendes aus:

1. Exportieren Sie die folgenden Ereignisprotokolle in das EVTX-Format:

   * System
   * Anwendung
   * Sicherheit
   * Operations Manager
   * Microsoft-SMA/Operational

2. Überprüfen Sie, ob die Arbeitsbereichs-ID, an die der Agent berichtet, mit dem Arbeitsbereich übereinstimmt, der von Windows Updates überwacht wird.

### <a name="for-issues-that-affect-a-job"></a>For Probleme, die einen Auftrag betreffen

Erfassen Sie über die in [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics) aufgelisteten Punkte hinaus folgende Informationen:

1. Erfassen Sie die **JobID**-Nummer (für den Auftrag, bei dem ein Problem auftritt):

   1. Navigieren Sie im Azure-Portal zum Blatt **Automation-Konten**.
   2. Wählen Sie das **Automation-Konto** aus, für das Sie die Problembehandlung durchführen.
   3. Wählen Sie **Aufträge** aus.
   4. Wählen Sie den Auftrag aus, für den Sie die Problembehandlung durchführen.
   5. Suchen Sie unter **Auftragszusammenfassung** nach einer **Auftrags-ID** (GUID).

   ![Auftrags-ID im Bereich „Auftragszusammenfassung“](media/collect-data-microsoft-azure-automation-case/job-summary-job-id.png)

2. Erfassen Sie den Kontonamen:

   1. Navigieren Sie im Azure-Portal zum Blatt **Automation-Konten**.
   2. Rufen Sie den Namen des **Automation-Kontos** ab, für das Sie die Problembehandlung durchführen.

3. Erfassen Sie eine Stichprobe des Skripts, das Sie ausführen.

4. Sammeln der Protokolldateien:

   1. Navigieren Sie im Azure-Portal zum Blatt **Automation-Konten**.
   2. Wählen Sie das **Automation-Konto** aus, für das Sie die Problembehandlung durchführen.
   3. Wählen Sie **Aufträge** aus.
   4. Wählen Sie den Auftrag aus, für den Sie die Problembehandlung durchführen.
   5. Wählen Sie **Alle Protokolle** aus.
   6. Sammeln Sie die Daten auf dem resultierenden Blatt.

   ![Unter „Alle Protokolle“ aufgeführte Daten](media/collect-data-microsoft-azure-automation-case/all-logs-data.png)

### <a name="for-issues-that-affect-modules"></a>For Probleme, die Module betreffen

Sammeln Sie über die unter „Grundlegende Datensammlung“ aufgeführten Punkte hinaus die folgenden Informationen:

* Die Schritte, die Sie ausgeführt haben, damit das Problem reproduziert werden kann.
* Einen Screenshot von jeder Fehlermeldung.
* Einen Screenshot der aktuellen Module und ihrer Versionsnummern.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, wenden Sie sich an die Azure-Experten im [MSDN Azure-Forum oder im Stack Overflow-Forum](https://azure.microsoft.com/support/forums/).

Alternativ dazu können Sie eine Azure-Supportanfrage erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.