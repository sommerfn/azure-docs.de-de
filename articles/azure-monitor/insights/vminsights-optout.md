---
title: Deaktivieren der Überwachung in Azure Monitor für VMs (Vorschauversion) | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie die Überwachung Ihrer virtuellen Computer in Azure Monitor für VMs beenden können.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 11/05/2018
ms.openlocfilehash: 70e71688fde5aff4002c7d49b1408bcefeab1eed
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72555158"
---
# <a name="disable-monitoring-of-your-vms-in-azure-monitor-for-vms-preview"></a>Deaktivieren der Überwachung Ihrer virtuellen Computer in Azure Monitor für VMs (Vorschauversion)

Nachdem Sie die Überwachung Ihrer virtuellen Computer (VMs) aktiviert haben, können Sie die Überwachung später in Azure Monitor für VMs deaktivieren. In diesem Artikel wird das Deaktivieren der Überwachung für eine oder mehrere VMs beschrieben.  

Zurzeit unterstützt Azure Monitor für VMs kein selektives Deaktivieren der VM-Überwachung. Möglicherweise unterstützt Ihr Log Analytics-Arbeitsbereich Azure Monitor für VMs und weitere Lösungen und kann auch andere Überwachungsdaten sammeln. Wenn Ihr Log Analytics-Arbeitsbereich diese Dienste bereitstellt, müssen Ihnen die Auswirkungen und die Methoden zum Deaktivieren der Überwachung bekannt sein, bevor Sie beginnen.

Azure Monitor für VMs nutzt die folgenden Komponenten:

* Einen Log Analytics-Arbeitsbereich, in dem Überwachungsdaten von VMs und anderen Quellen gespeichert werden.
* Eine Sammlung von Leistungsindikatoren, die im Arbeitsbereich konfiguriert ist. Diese Sammlung aktualisiert die Überwachungskonfiguration auf allen mit dem Arbeitsbereich verbundenen VMs.
* `InfrastructureInsights` und `ServiceMap`, bei denen es sich um im Arbeitsbereich konfigurierte Überwachungslösungen handelt. Diese Lösungen aktualisieren die Überwachungskonfiguration auf allen mit dem Arbeitsbereich verbundenen VMs.
* `MicrosoftMonitoringAgent` und `DependencyAgent`, bei denen es sich um Azure-VM-Erweiterungen handelt. Diese Erweiterungen sammeln Daten und senden diese an den Arbeitsbereich.

Beachten Sie bei den Vorbereitungen zum Deaktivieren der Überwachung Ihrer VMs die folgenden Aspekte:

* Wenn Sie das Verfahren nur für eine einzelne VM ausgeführt und den vorab ausgewählten Log Analytics-Arbeitsbereich verwendet haben, können Sie die Überwachung deaktivieren, indem Sie den Abhängigkeits-Agent auf der VM deinstallieren und den Log Analytics-Agent von diesem Arbeitsbereich trennen. Dieses Verfahren ist geeignet, wenn Sie die VM für andere Zwecke verwenden und später möglicherweise mit einem anderen Arbeitsbereich verbinden möchten.
* Wenn Sie einen bereits vorhandenen Log Analytics-Arbeitsbereich ausgewählt haben, der andere Überwachungslösungen und die Sammlung von Daten aus anderen Quellen unterstützt, können Sie Lösungskomponenten aus dem Arbeitsbereich entfernen, ohne dass dies zu Unterbrechungen oder anderen Auswirkungen auf den Arbeitsbereich führt.  

>[!NOTE]
> Nach dem Entfernen der Lösungskomponenten aus dem Arbeitsbereich werden möglicherweise weiterhin Daten zum Integritätszustand Ihrer Azure-VMs angezeigt, insbesondere werden Leistungs- und Zuordnungsdaten in den jeweiligen Ansichten des Portals angezeigt. In den Ansichten **Leistung** und **Zuordnung** werden schließlich keine Daten mehr angezeigt. In der Ansicht **Integrität** ist jedoch weiterhin der Integritätsstatus Ihrer VMs angegeben. Bei der ausgewählten Azure-VM steht die Option **Jetzt testen** zur Verfügung, sodass Sie die Überwachung zu einem späteren Zeitpunkt wieder aktivieren können.  

## <a name="remove-azure-monitor-for-vms-completely"></a>Vollständiges Entfernen von Azure Monitor für VMs

Wenn Sie den Log Analytics-Arbeitsbereich weiterhin benötigen, führen Sie die folgenden Schritte aus, um Azure Monitor für VMs vollständig zu entfernen. Sie werden die Lösungen `InfrastructureInsights` und `ServiceMap` aus dem Arbeitsbereich entfernen.  

>[!NOTE]
>Wenn Sie die ServiceMap-Überwachungslösung vor dem Aktivieren von Azure Monitor für VMs verwendet haben und sie weiterhin benötigen, entfernen Sie diese Lösung nicht, wie es im letzten Schritt des folgenden Verfahrens beschrieben ist.  
>

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, werden Vorschläge in der Liste auf Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.
3. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche den Arbeitsbereich aus, den Sie beim Aktivieren von Azure Monitor für VMs ausgewählt hatten.
4. Wählen Sie links die Option **Lösungen** aus.  
5. Wählen Sie in der Liste der Lösungen den Eintrag **InfrastructureInsights(Arbeitsbereichsname)** aus. Wählen Sie auf der Seite **Übersicht** für die Lösung die Option **Löschen** aus. Wenn Sie aufgefordert werden, die Auswahl zu bestätigen, klicken Sie auf **Ja**.  
6. Wählen Sie in der Liste der Lösungen den Eintrag **ServiceMap(Arbeitsbereichsname)** aus. Wählen Sie auf der Seite **Übersicht** für die Lösung die Option **Löschen** aus. Wenn Sie aufgefordert werden, die Auswahl zu bestätigen, klicken Sie auf **Ja**.  

Wenn Sie vor der Aktivierung von Azure Monitor für VMs für die Windows- oder Linux-basierten VMs in Ihrem Arbeitsbereich keine [Leistungsindikatoren erfasst haben](vminsights-enable-overview.md#performance-counters-enabled), [deaktivieren Sie diese Regeln](../platform/data-sources-performance-counters.md#configuring-performance-counters) für Windows und für Linux.

## <a name="disable-monitoring-and-keep-the-workspace"></a>Deaktivieren der Überwachung und Beibehalten des Arbeitsbereichs  

Wenn der Log Analytics-Arbeitsbereich weiterhin die Überwachung von Daten aus anderen Quellen unterstützen muss, führen Sie die folgenden Schritte aus, um die Überwachung auf dem virtuellen Computer zu deaktivieren, den Sie zum Auswerten von Azure Monitor für VMs verwendet haben. Bei virtuellen Azure-Computern entfernen Sie die VM-Erweiterungen für den Abhängigkeits-Agent und den Log Analytics-Agent für Windows oder Linux direkt vom virtuellen Computer. 

>[!NOTE]
>Entfernen Sie den Log Analytics-Agent in folgenden Fällen nicht: 
>
> * Azure Automation verwaltet den virtuellen Computer zur Orchestrierung von Prozessen oder zur Verwaltung von Konfiguration oder Updates. 
> * Azure Security Center verwaltet den virtuellen Computer in Hinsicht auf Sicherheit und Bedrohungserkennung. 
>
> Wenn Sie den Log Analytics-Agent entfernen, verhindern Sie eine proaktive Verwaltung Ihres virtuellen Computers durch diese Dienste und Lösungen. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Wählen Sie im Azure-Portal die Option **Virtual Machines** aus. 
3. Wählen Sie einen virtuellen Computer in der Liste aus. 
4. Klicken Sie links auf **Erweiterungen**. Wählen Sie auf der Seite **Erweiterungen** die Option **DependencyAgent** aus.
5. Klicken Sie auf der Eigenschaftenseite der Erweiterung auf **Deinstallieren**.
6. Wählen Sie auf der Seite **Erweiterungen** die Option **MicrosoftMonitoringAgent** aus. Klicken Sie auf der Eigenschaftenseite der Erweiterung auf **Deinstallieren**.  
