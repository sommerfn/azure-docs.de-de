---
title: Exportieren von Azure Security Center-Warnungen und -Empfehlungen in SIEMs | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie den fortlaufenden Export von Sicherheitswarnungen und -empfehlungen in SIEMs einrichten.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: cd26ed446ce676bcec85d8e413d3ec37ac236869
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73521492"
---
# <a name="export-security-alerts-and-recommendations-preview"></a>Exportieren von Sicherheitswarnungen und -empfehlungen (Vorschau)

Azure Security Center generiert detaillierte Sicherheitswarnungen und -empfehlungen. Sie können diese im Portal oder über programmgesteuerte Tools anzeigen. Möglicherweise müssen Sie diese Informationen auch exportieren oder an andere Überwachungstools in Ihrer Umgebung senden. 

In diesem Artikel wird der Satz von Tools (Vorschau) beschrieben, mit denen Sie Warnungen und Empfehlungen entweder manuell oder kontinuierlich fortlaufend exportieren können.

Mit diesen Tools können Sie folgende Aktionen ausführen:

* Generieren ausführlicher Berichte als CSV
* Exportieren in Log Analytics-Arbeitsbereiche
* Exportieren in Azure Event Hubs (für Integrationen mit Drittanbieter-SIEMs)

## <a name="setting-up-a-continuous-export"></a>Einrichten eines fortlaufenden Exports

1. Klicken Sie auf der Security Center-Randleiste auf **Preise & Einstellungen**.

1. Wählen Sie das Abonnement aus, für das Sie den Datenexport konfigurieren möchten.
    
1. Wählen Sie in der Randleiste der Seite „Einstellungen“ für dieses Abonnement **Fortlaufender Export (Vorschau)** aus.

    [![Exportoptionen in Azure Security Center](media/continuous-export/continuous-export-options-page.png)](media/continuous-export/continuous-export-options-page.png#lightbox) Hier werden die Exportoptionen angezeigt. Es gibt eine Registerkarte für jedes verfügbare Exportziel. 

1. Wählen Sie den Datentyp, den Sie exportieren möchten, und dann Filter für die einzelnen Typen aus (z.B. nur Warnungen mit hohem Schweregrad exportieren).

1. Wählen Sie im Bereich „Exportziel“ aus, wo die Daten gespeichert werden sollen. Daten können in einem Ziel in einem anderen Abonnement gespeichert werden (z.B. in einer Central Event Hub-Instanz oder in einem zentralen Log Analytics-Arbeitsbereich).

1. Klicken Sie auf **Speichern**.

## <a name="continuous-export-through-azure-event-hubs"></a>Fortlaufender Export über Azure Event Hubs  

> [!NOTE]
> Die effektivste Methode zum Streamen von Überwachungsdaten an externe Tools ist in den meisten Fällen die Verwendung von Azure Event Hubs. [Dieser Artikel](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs) enthält eine kurze Beschreibung der Vorgehensweise beim Streamen von Überwachungsdaten aus verschiedenen Quellen an einen Event Hub und Links zu ausführlichen Anleitungen.

> [!NOTE]
> Wenn Sie Security Center-Warnungen zuvor mithilfe des Azure-Aktivitätsprotokolls in eine SIEM-Lösung exportiert haben, wird diese Methodik durch das folgende Verfahren ersetzt.

### <a name="to-integrate-with-a-siem"></a>So führen Sie die Integration in eine SIEM-Lösung durch 

Nachdem Sie den fortlaufenden Export der ausgewählten Security Center-Daten in Azure Event Hubs konfiguriert haben, können Sie anhand der folgenden Anweisungen den entsprechenden Connector in Ihrer SIEM-Lösung einrichten.

Befolgen Sie die Anweisungen für Ihre SIEM-Lösung auf [dieser Seite](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/?cdn=disable), und verwenden Sie den relevanten Connector:

* **Splunk**: Verwenden Sie das [Azure Monitor-Add-On für Splunk](https://splunkbase.splunk.com/app/3534/)
* **IBM QRadar**: Verwenden Sie [eine manuell konfigurierte Protokollquelle](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/com.ibm.dsm.doc/t_logsource_microsoft_azure_event_hubs.html)
* **ArcSight**: Verwenden Sie [SmartConnector](https://community.microfocus.com/t5/ArcSight-Connectors/SmartConnector-for-Microsoft-Azure-Monitor-Event-Hub/ta-p/1671292)

Wenn Sie **Azure Sentinel** einsetzen, verwenden Sie den hier angebotenen nativen [Datenconnector](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center) für Azure Security Center-Warnungen.

Wenn Sie die kontinuierlich exportierten Daten automatisch aus dem konfigurierten Event Hub in Azure Data Explorer verschieben möchten, befolgen Sie zudem die Anweisungen unter [Erfassen von Daten aus Event Hub in Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/ingest-data-event-hub).


## <a name="continuous-export-to-log-analytics-workspace"></a>Fortlaufender Export in einen Log Analytics-Arbeitsbereich

Um einen Export in einen Log Analytics-Arbeitsbereich auszuführen, müssen Sie über Log Analytics-Lösungen für Ihren Arbeitsbereich im Security Center-Tarif „Free“ oder „Standard“ verfügen. Wenn Sie das Azure-Portal verwenden, wird die Security Center-Lösung im Free-Tarif automatisch aktiviert, sobald Sie den fortlaufenden Export aktivieren. Wenn Sie die Einstellungen für den fortlaufenden Export jedoch programmgesteuert konfigurieren, müssen Sie den Tarif „Free“ oder „Standard“ für den erforderlichen Arbeitsbereich in **Preise & Einstellungen** manuell auswählen.  

Sicherheitswarnungen und -empfehlungen werden in den Tabellen *SecurityAlert* und *SecurityRecommendations* gespeichert. Der Name der Log Analytics-Lösung, in der diese Tabellen enthalten sind, hängt davon ab, ob Sie den Tarif „Free“ oder „Standard“ nutzen (siehe [Preise](security-center-pricing.md)): „Security“ oder „SecurityCenterFree“.

![Die Tabelle *SecurityAlert* in Log Analytics](./media/continuous-export/log-analytics-securityalert-solution.png)

## <a name="manual-one-time-export-of-security-alerts"></a>Manueller einmaliger Export von Sicherheitswarnungen

Wenn Sie einen CSV-Bericht für Warnungen oder Empfehlungen herunterladen möchten, öffnen Sie die Seite **Sicherheitswarnungen** oder **Empfehlungen**, und klicken Sie auf **CSV-Bericht herunterladen (Vorschau)** .

[![Herunterladen von Warnungsdaten als CSV-Datei](media/continuous-export/download-alerts-csv.png)](media/continuous-export/download-alerts-csv.png#lightbox)

> [!NOTE]
> Diese Berichte enthalten Warnungen und Empfehlungen für Ressourcen aus den aktuell ausgewählten Abonnements unter dem Verzeichnis- und Abonnementfilter im Azure-Portal: ![Der Filter für die Auswahl von Verzeichnis und Abonnement](./media/continuous-export/filter-for-export-csv.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie fortlaufende Exporte ihrer Empfehlungen und Warnungen konfigurieren. Außerdem haben Sie gelernt, wie Sie Ihre Warnungsdaten als CSV-Datei herunterladen. 

Verwandtes Material finden Sie in der folgenden Dokumentation: 

- [Dokumentation zu Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/)
- [Dokumentation zu Azure Sentinel](https://docs.microsoft.com/azure/sentinel/)
- [Azure Monitor-Dokumentation](https://docs.microsoft.com/azure/azure-monitor/)