---
title: Ermitteln der auf Ihren Computern installierten Software mit Azure Automation | Microsoft-Dokumentation
description: Ermitteln Sie mit der Bestandsfunktion, welche Software auf den Computern in Ihrer Umgebung installiert ist.
services: automation
keywords: Inventar, Inventur, Bestand, Automatisierung, Änderung, Nachverfolgung
author: jennyhunter-msft
ms.author: jehunte
ms.date: 04/11/2018
ms.topic: tutorial
ms.service: automation
ms.subservice: change-inventory-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 47313781756e460a8c30638661489874481b88a0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476819"
---
# <a name="discover-what-software-is-installed-on-your-azure-and-non-azure-machines"></a>Ermitteln der Software, die auf Ihren Azure-Computern und anderen Computern installiert ist

In diesem Tutorial wird beschrieben, wie Sie ermitteln können, welche Software in Ihrer Umgebung installiert ist. Sie können die Bestandsinformationen für Software, Dateien, Linux-Daemons, Windows-Dienste und Windows-Registrierungsschlüssel auf Ihren Computern sammeln und anzeigen. Durch das Nachverfolgen der Konfigurationen Ihrer Computer können Sie betriebsbezogene Problem in Ihrer Umgebung erkennen und ein besseres Verständnis des Zustands Ihrer Computer entwickeln.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Aktivieren der Lösung
> * Integrieren einer Azure-VM
> * Integrieren eines Azure-fremden virtuellen Computers
> * Anzeigen von installierter Software
> * Durchsuchen von Bestandsprotokollen nach installierter Software

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* Ein [Automation-Konto](automation-offering-get-started.md) für die Watcher- und Aktionsrunbooks und den Watchertask.
* Einen [virtuellen Computer](../virtual-machines/windows/quick-create-portal.md), der integriert werden soll.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="enable-change-tracking-and-inventory"></a>Aktivieren der Änderungsnachverfolgung und Bestandsfunktion

In diesem Tutorial müssen Sie zuerst die Änderungsnachverfolgung und die Bestandsfunktion aktivieren. Wenn Sie die Lösung **Änderungsnachverfolgung** bereits aktiviert haben, ist dieser Schritt nicht erforderlich.

Navigieren Sie zu Ihrem Automation-Konto, und wählen Sie unter **KONFIGURATIONSVERWALTUNG** die Option **Bestand** aus.

Wählen Sie den Log Analytics-Arbeitsbereich und das Automation-Konto aus, und klicken Sie auf **Aktivieren**, um die Lösung zu aktivieren. Es dauert ungefähr 15 Minuten, bis die Lösung aktiviert ist.

![Banner für die Konfiguration zur Integration der Bestandsfunktion](./media/automation-tutorial-installed-software/enableinventory.png)

Konfigurieren Sie den gewünschten Standort, den Log Analytics-Arbeitsbereich und das Automation-Konto, und klicken Sie auf **Aktivieren**, um die Lösung zu aktivieren. Wenn die Felder ausgegraut sind, bedeutet das, dass für den virtuellen Computer eine andere Automatisierungslösung aktiviert ist und derselbe Arbeitsbereich und dasselbe Automation-Konto verwendet werden müssen.

Mit einem [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-Arbeitsbereich werden Daten gesammelt, die von Features und Diensten wie der Bestandsfunktion generiert werden.
Der Arbeitsbereich ist ein zentraler Ort zum Überprüfen und Analysieren von Daten aus mehreren Quellen.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Das Aktivieren der Lösung kann bis zu 15 Minuten dauern. Während dieses Zeitraums sollten Sie das Browserfenster nicht schließen.
Nachdem die Lösung aktiviert wurde, werden Informationen zur installierten Software und Änderungen der VM-Datenflüsse zu Azure Monitor-Protokollen angezeigt.
Es kann zwischen 30 Minuten und 6 Stunden dauern, bis die Daten für die Analyse verfügbar sind.

## <a name="onboard-a-vm"></a>Integrieren eines virtuellen Computers

Navigieren Sie in Ihrem Automation-Konto unter **KONFIGURATIONSVERWALTUNG** zu **Bestand**.

Klicken Sie auf **+ Azure-VM hinzufügen**. Daraufhin wird die Seite **Virtuelle Computer** geöffnet, auf der Sie einen virtuellen Computer aus der Liste auswählen können. Wählen Sie den virtuellen Computer aus, den Sie integrieren möchten. Klicken Sie auf der daraufhin angezeigten Seite auf **Aktivieren**, um die Lösung für den virtuellen Computer zu aktivieren. Der Microsoft-Verwaltungs-Agent (Microsoft Management Agent, MMA) wird auf dem virtuellen Computer bereitgestellt und konfiguriert den Agent für die Kommunikation mit dem Log Analytics-Arbeitsbereich, den Sie beim Aktivieren der Lösung konfiguriert haben. Der Integrationsvorgang kann einige Minuten dauern. An diesem Punkt können Sie in der Liste einen neuen virtuellen Computer auswählen und integrieren.

## <a name="onboard-a-non-azure-machine"></a>Integrieren eines Nicht-Azure-Computers

Wenn Sie einen Azure-fremden Computer hinzufügen möchten, installieren den passenden Agent für das Betriebssystem ([Windows](../azure-monitor/platform/agent-windows.md) oder [Linux](automation-linux-hrw-install.md)). Navigieren Sie nach der Installation des Agents zu Ihrem Automation-Konto und dort unter **KONFIGURATIONSVERWALTUNG** zu **Bestand**. Wenn Sie auf **Computer verwalten** klicken, wird eine Liste mit den Computern angezeigt, die mit Ihrem Log Analytics-Arbeitsbereich verknüpft sind und für die die Lösung nicht aktiviert ist. Wählen Sie die passende Option für Ihre Umgebung aus:

* **Enable on all available machines** (Auf allen verfügbaren Computern aktivieren): Diese Option aktiviert die Lösung auf allen Computern, die derzeit mit Ihrem Log Analytics-Arbeitsbereich verknüpft sind.
* **Enable on all available machines and future machines** (Auf allen verfügbaren und zukünftigen Computern aktivieren): Diese Option aktiviert die Lösung auf allen Computern, die derzeit mit Ihrem Log Analytics-Arbeitsbereich verknüpft sind, sowie auf allen Computern, die dem Arbeitsbereich noch hinzugefügt werden.
* **Enable on selected machines** (Auf ausgewählten Computern aktivieren): Diese Option aktiviert die Lösung nur auf den ausgewählten Computern.

![Computer verwalten](./media/automation-tutorial-installed-software/manage-machines.png)

## <a name="view-installed-software"></a>Anzeigen von installierter Software

Nachdem die Lösung für die Änderungsnachverfolgung und den Bestand aktiviert wurde, können Sie die Ergebnisse auf der Seite **Bestand** anzeigen.

Wählen Sie innerhalb Ihres Automation-Kontos unter **KONFIGURATIONSVERWALTUNG** die Option **Bestand** aus.

Klicken Sie auf der Seite **Bestand** auf die Registerkarte **Software**.

Auf der Registerkarte **Software** wird eine Tabelle mit der gefundenen Software angezeigt. Die Software ist nach Name und Version der Software gruppiert.

Die allgemeinen Details zu den einzelnen Softwaredatensätzen können in der Tabelle angezeigt werden. Diese Details enthalten den Softwarenamen, die Version, den Herausgeber, den letzten Aktualisierungszeitpunkt (letzter Aktualisierungszeitpunkt, der von einem Computer der Gruppe gemeldet wurde) und die Computer (Anzahl von Computern mit der jeweiligen Software).

![Softwarebestand](./media/automation-tutorial-installed-software/inventory-software.png)

Klicken Sie auf eine Zeile, um die Eigenschaften des Softwaredatensatzes und die Namen der Computer mit dieser Software anzuzeigen.

Sie können im Textfeld direkt oberhalb der Softwareliste eine Suche durchführen, um nach einer bestimmten Software oder Softwaregruppe zu suchen.
Der Filter ermöglicht Ihnen die Suche anhand des Softwarenamens, der Version und des Herausgebers.

Wenn Sie beispielsweise nach „Contoso“ suchen, wird die gesamte Software zurückgegeben, deren Name, Herausgeber oder Version den Text „Contoso“ enthält.

## <a name="search-inventory-logs-for-installed-software"></a>Durchsuchen von Bestandsprotokollen nach installierter Software

Bei der Inventur werden Protokolldaten generiert, die an Azure Monitor-Protokolle gesendet werden. Um die Protokolle per Ausführung von Abfragen zu durchsuchen, klicken Sie oben im Fenster **Bestand** auf **Log Analytics**.

Die Bestandsdaten werden unter dem Typ **ConfigurationData** gespeichert.
Die folgende Log Analytics-Beispielabfrage gibt die Bestandsergebnisse zurück, bei denen der Herausgeber „Microsoft Corporation“ lautet.

```loganalytics
ConfigurationData
| where ConfigDataType == "Software"
| where Publisher == "Microsoft Corporation"
| summarize arg_max(TimeGenerated, *) by SoftwareName, Computer
```

Weitere Informationen zur Ausführung von Abfragen und zum Durchsuchen von Protokolldateien in Azure Monitor-Protokollen finden Sie unter [Azure Monitor-Protokolle](../azure-monitor/log-query/log-query-overview.md).

### <a name="single-machine-inventory"></a>Bestandsermittlung für einen einzelnen Computer

Zum Anzeigen des Softwarebestands für einen einzelnen Computer können Sie über die Seite mit den Azure-VM-Ressourcen auf die Bestandsfunktion zugreifen oder Azure Monitor-Protokolle zum Filtern nach dem entsprechenden Computer verwenden.
Mit der folgenden Log Analytics-Beispielabfrage wird eine Liste mit Software für einen Computer mit dem Namen ContosoVM zurückgegeben.

```loganalytics
ConfigurationData
| where ConfigDataType == "Software"
| summarize arg_max(TimeGenerated, *) by SoftwareName, CurrentVersion
| where Computer =="ContosoVM"
| render table
| summarize by Publisher, SoftwareName
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde beschrieben, wie Sie den Softwarebestand beispielsweise für folgende Zwecke anzeigen:

> [!div class="checklist"]
> * Aktivieren der Lösung
> * Integrieren einer Azure-VM
> * Integrieren eines Azure-fremden virtuellen Computers
> * Anzeigen von installierter Software
> * Durchsuchen von Bestandsprotokollen nach installierter Software

Fahren Sie mit der Übersicht über die Lösung für die Änderungsnachverfolgung und den Bestand fort, um mehr darüber zu erfahren.

> [!div class="nextstepaction"]
> [Lösung für die Änderungsnachverfolgung und den Bestand](automation-change-tracking.md)
