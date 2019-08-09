---
title: Verbinden von Syslog-Daten mit Azure Sentinel (Vorschauversion) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Syslog-Daten mit Azure Sentinel verbinden.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5dd59729-c623-4cb4-b326-bb847c8f094b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2019
ms.author: rkarlin
ms.openlocfilehash: dad74410562aa54aeb61675e7dc1c0adccc44797
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679212"
---
# <a name="connect-your-external-solution-using-syslog"></a>Verbinden Sie Ihre externe Lösung mithilfe von Syslog

> [!IMPORTANT]
> Azure Sentinel ist aktuell als Public Preview verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sie können jede lokale Appliance, die Syslog unterstützt, mit Azure Sentinel verbinden. Hierzu wird ein Agent verwendet, der sich auf einem Linux-Computer befindet, der sich zwischen der Appliance und Azure Sentinel befindet. Ist Ihr Linux-Computer in Azure angeordnet, können Sie die Protokolle von Ihrer Appliance oder Anwendung an einen dedizierten Arbeitsbereich streamen, den Sie in Azure erstellt und verbunden haben. Ist Ihr Linux-Computer nicht in Azure angeordnet, können Sie die Protokolle von Ihrer Appliance an einen dedizierten lokalen virtuellen Computer oder Computer streamen, auf dem Sie den Agent für Linux installiert haben. 

> [!NOTE]
> Wenn Ihre Appliance Syslog CEF unterstützt, ist die Verbindung vollständiger, und Sie sollten Sie diese Option wählen die Anweisungen unter [Verbinden von Daten aus CEF](connect-common-event-format.md) befolgen.

## <a name="how-it-works"></a>So funktioniert's

Syslog ist ein gängiges Protokoll zur Ereignisprotokollierung für Linux. Anwendungen senden Nachrichten, die auf dem lokalen Computer gespeichert oder an einen Syslog-Sammler übermittelt werden können. Wenn der Log Analytics-Agent für Linux installiert ist, konfiguriert er den lokalen Syslog-Daemon zum Weiterleiten von Nachrichten an den Agent. Der Agent sendet die Nachricht dann an Azure Monitor, wo ein entsprechender Datensatz erstellt wird.

Weitere Informationen finden Sie unter [Syslog-Datenquellen in Azure Monitor](../azure-monitor/platform/data-sources-syslog.md).

> [!NOTE]
> Der Agent kann Protokolle von mehreren Quellen erfassen, muss aber auf einem dedizierten Proxycomputer installiert sein.

## <a name="connect-your-syslog-appliance"></a>Verbinden Ihrer Syslog-Appliance

1. Wählen Sie im Azure Sentinel-Portal die Option **Datenconnectors** und anschließend in der Tabelle die Zeile **Syslog** aus, und klicken Sie danach im Syslog-Bereich auf der rechten Seite auf **Open connector page** (Connectorseite öffnen).
2. Befindet sich Ihr Linux-Computer in Azure, wählen Sie **Download and install agent on Azure Linux virtual machine** (Agent herunterladen und auf virtuellem Linux-Computer in Azure installieren) aus. Wählen Sie im Fenster „Virtuelle Computer“ die Computer aus, auf denen Sie den Agent installieren möchten, und klicken Sie anschließen im oberen Bereich auf **Verbinden**.
1. Befindet sich Ihr Linux-Computer nicht in Azure, wählen Sie **Download and install agent on Linux non-Azure machine** (Agent herunterladen und auf Azure-fremdem Linux-Computer installieren) aus. Kopieren Sie im Fenster **Direkt-Agent** den Befehl unter **Agent für Linux herunterladen und integrieren**, und führen Sie ihn auf Ihrem Computer aus. 
   > [!NOTE]
   > Stellen Sie sicher, dass Sie die Sicherheit des Computers gemäß der Sicherheitsrichtlinie Ihrer Organisation konfigurieren. Beispielsweise können Sie das Netzwerk so konfigurieren, dass es der Sicherheitsrichtlinie für das Unternehmensnetzwerk entspricht, und die Ports und Protokolle im Daemon Ihren Anforderungen entsprechend ändern. 

1. Gehen Sie im Fenster für die Einrichtung des Syslog-Connectors unter **Configure the logs to be connected** (Zu verbindende Protokolle konfigurieren) wie folgt vor:
    1. Klicken Sie auf den Link**zum Öffnen der Konfiguration der erweiterten Einstellungen Ihres Arbeitsbereichs.** 
    1. Wählen Sie **Daten** > **Syslog** aus.
    1. Legen Sie anschließend in der Tabelle fest, welche Einrichtungen von Syslog gesammelt werden sollen. Es empfiehlt sich, die Einrichtungen hinzuzufügen oder auszuwählen, die Ihre Syslog-Appliance in ihren Protokollheadern enthält. Diese Konfiguration finden Sie in Ihrer Syslog-Appliance im Ordner „/etc/rsyslog.d/security-config-omsagent.conf“ (Syslog-d) bzw. im Ordner „/etc/syslog-ng/security-config-omsagent.conf“ (r-Syslog). 
       > [!NOTE]
       > Wenn Sie das Kontrollkästchen **Nachstehende Konfiguration auf meine Computer anwenden** aktivieren, gilt diese Konfiguration für alle Linux-Computer, die mit diesem Arbeitsbereich verbunden sind. Diese Konfiguration finden Sie auf Ihrem Syslog-Computer. 
1. Klicken Sie auf **Press here to open the configuration blade** (Hier drücken, um das Konfigurationsblatt zu öffnen).
1. Wählen Sie **Daten** und dann **Syslog** aus.
   - Vergewissern Sie sich, dass jede Einrichtung, die Sie über Syslog senden, in der Tabelle enthalten ist. Legen Sie für jede Einrichtung, die Sie überwachen möchten, einen Schweregrad fest. Klicken Sie auf **Anwenden**.
1. Überprüfen Sie auf Ihrem Syslog-Computer, dass Sie diese Einrichtungen senden. 

1. Um das entsprechende Schema in Log Analytics für die Syslog-Protokolle zu verwenden, suchen Sie nach **Syslog**.
1. Sie können die unter [Verwenden von Funktionen in Azure Monitor-Protokollabfragen](../azure-monitor/log-query/functions.md) beschriebene Kusto-Funktion verwenden, um Ihre Syslog-Nachrichten zu analysieren, sie als neue Log Analytics Funktion speichern und die Funktion anschließend als neuen Datentyp verwenden.




## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie lokale Syslog-Appliances mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).
