---
title: 'Azure Sentinel-Schnellstart: Erste Schritte mit Azure Sentinel (Vorschauversion) | Microsoft-Dokumentation'
description: 'Azure Sentinel-Schnellstart: Erste Schritte mit Azure Sentinel'
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5a4ae93c-d648-41fb-8fb8-96a025d2f73e
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/20/2019
ms.author: rkarlin
ms.openlocfilehash: 75bfdce02313ab7e799f6535f247407c239653b8
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2019
ms.locfileid: "67619806"
---
# <a name="quickstart-get-started-with-azure-sentinel-preview"></a>Schnellstart: Erste Schritte mit Azure Sentinel

> [!IMPORTANT]
> Azure Sentinel ist zurzeit als öffentliche Vorschauversion verfügbar.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


In diesem Schnellstart erfahren Sie, wie Sie die Vorgänge in Ihrer Umgebung mit Azure Sentinel schnell anzeigen und überwachen können. Nachdem Sie Ihre Datenquellen mit Azure Sentinel verbunden haben, stehen Ihnen sofort Datenvisualisierungen und -analysen zur Verfügung, die Informationen zu allen Vorgängen in Ihren verbundenen Datenquellen liefern. Azure Sentinel bietet Ihnen Dashboards mit den leistungsstarken Funktionen der bereits in Azure verfügbaren Tools sowie integrierte Tabellen und Diagramme mit Analysen für Ihre Protokolle und Abfragen. Sie können integrierte Dashboards verwenden, mühelos ein komplett neues Dashboard erstellen oder auf der Grundlage eines vorhandenen Dashboards ein eigenes Dashboard erstellen. 

## <a name="get-visualization"></a>Visualisierung

Bevor Sie die Vorgänge in Ihrer Umgebung visualisieren und analysieren, sollten Sie zunächst einen Blick auf das Übersichtsdashboard werfen, um eine Vorstellung vom Sicherheitsstatus Ihrer Organisation zu bekommen. Sie können auf jedes Element auf diesen Kacheln klicken, um Detailinformationen zu den zugehörigen Rohdaten anzuzeigen. Um Fehlinformationen zu reduzieren und die Anzahl von Warnungen zu minimieren, die Sie überprüfen und untersuchen müssen, korreliert Azure Sentinel Warnungen mithilfe einer Zusammenführungstechnik zu Fällen. **Fälle** sind Gruppen von verwandten Warnungen, die zusammen einen handlungsrelevanten Incident bilden, den Sie untersuchen und beheben können.

- Wählen Sie im Azure-Portal Azure Sentinel und anschließend den Arbeitsbereich aus, den Sie überwachen möchten.

  ![Azure Sentinel-Übersicht](./media/qs-get-visibility/overview.png)

- Auf der Symbolleiste am oberen Rand des Fensters werden die Anzahl von Ereignissen im ausgewählten Zeitraum und ein Vergleich mit den vorherigen 24 Stunden angezeigt. Der Symbolleiste können Sie entnehmen, welche Warnungen für diese Ereignisse ausgelöst wurden (die kleine Zahl stellt die Veränderung in den letzten 24 Stunden dar) und wie viele dieser Ereignisse offen, in Bearbeitung und geschlossen sind. Vergewissern Sie sich in dieser Ansicht, dass keine deutliche Zu- oder Abnahme der Anzahl von Ereignissen vorliegt. Im Fall einer Abnahme meldet eine Verbindung möglicherweise keine Daten mehr an Azure Sentinel. Eine Zunahme kann ein Anzeichen für eine verdächtige Aktivität sein. Überprüfen Sie, ob neue Warnungen vorhanden sind.

   ![Azure Sentinel-Trichter](./media/qs-get-visibility/funnel.png)

Der Hauptteil der Übersichtsseite bietet auf einen Blick Aufschluss über den Sicherheitsstatus Ihres Arbeitsbereichs:

- **Events and alerts over time** (Ereignisse und Warnungen im Zeitverlauf): Listet die Anzahl von Ereignissen und Warnungen auf, die aufgrund dieser Ereignisse erstellt wurden. Wenn Sie eine ungewöhnliche Spitze bemerken, sollten entsprechende Warnungen angezeigt werden. Falls aufgrund ungewöhnlicher Aktivitäten eine Spitze in der Anzahl von Ereignissen vorliegt, aber keine Warnungen angezeigt werden, kann dies Anlass zur Sorge sein.

- **Potential malicious events** (Potenziell schädliche Ereignisse): Wenn Datenverkehr aus bekanntermaßen schädlichen Quellen erkannt wird, zeigt Azure Sentinel Warnungen auf der Karte an. Orange steht für eingehenden Datenverkehr: Jemand versucht, über eine bekanntermaßen schädliche IP-Adresse auf Ihre Organisation zuzugreifen. Wenn ausgehende Aktivität (rot) angezeigt wird, bedeutet dies, dass Daten aus Ihrem Netzwerk außerhalb Ihrer Organisation zu einer bekanntermaßen schädlichen IP-Adresse gestreamt werden.

   ![Azure Sentinel-Karte](./media/qs-get-visibility/map.png)


- **Recent cases** (Aktuelle Fälle): Hier können Sie Ihre aktuellen Fälle, deren Schweregrad und die Anzahl von Warnungen im Zusammenhang mit dem Fall einsehen. Wenn Sie eine plötzliche Spitze bei einem bestimmten Warnungstyp feststellen, kann dies auf einen aktiven Angriff hinweisen. Bei einem plötzlichen Spitzenwert von 20 Pass-the-Hash-Ereignissen von Azure ATP ist es beispielsweise möglich, dass gerade ein Angriffsversuch stattfindet.

- **Data source anomalies** (Datenquellenanomalien): Die Datenanalysten von Microsoft haben Modelle erstellt, die die Daten aus Ihren Datenquellen ständig auf Anomalien durchsuchen. Wenn keine Anomalien vorliegen, wird nichts angezeigt. Wenn Anomalien erkannt werden, sollten Sie sie genau untersuchen, um herauszufinden, was passiert ist. Klicken Sie beispielsweise auf die Spitze in der Azure-Aktivität. Sie können auf **Diagramm** klicken, um zu überprüfen, wann die Spitze aufgetreten ist, und anschließend zum Ermitteln der Ursache nach Aktivitäten in diesem Zeitraum filtern.

   ![Azure Sentinel-Karte](./media/qs-get-visibility/anomolies.png)

## Verwenden der integrierten Dashboards<a name="dashboards"></a>

Integrierte Dashboards stellen integrierte Daten aus Ihren verbundenen Datenquellen bereit, mit deren Hilfe Sie die in diesen Diensten generierten Ereignisse untersuchen können. Die integrierten Dashboards umfassen die Azure-ID, Azure-Aktivitätsereignisse und Daten von lokalen Systemen, bei denen es sich um Daten aus „Windows-Ereignisse“ von Servern, Warnungen von Erstanbieterlösungen und Daten von Drittanbieterlösungen handeln kann, die auf Windows-Ereignissen basieren (einschließlich Protokolle zum Firewalldatenverkehr, Office 365 und unsichere Protokolle).

1. Wählen Sie unter **Einstellungen** die Option **Dashboards** aus. Unter **Installiert** werden alle installierten Dashboards angezeigt. Unter **Alle** wird der gesamte Katalog integrierter Dashboards angezeigt, die zur Installation verfügbar sind. 
2. Suchen Sie nach einem bestimmten Dashboard, um die ganze Liste und die Beschreibung des Funktionsumfangs der einzelnen Dashboards anzuzeigen. 
3. Sofern Sie Azure AD verwenden, empfehlen wir, zur Verwendung von Azure Sentinel mindestens die folgenden Dashboards zu installieren:
   - **Azure AD**: Verwenden Sie eines oder beide der folgenden Dashboards:
       - Das Dashboard **Azure AD sign-ins** (Azure AD-Anmeldungen) analysiert Anmeldungen im Zeitverlauf, um Anomalien zu ermitteln. In diesem Dashboard werden fehlgeschlagene Anmeldungen nach Anwendungen, Geräten und Orten sortiert angezeigt, sodass Sie auf einen Blick sehen können, ob etwas Ungewöhnliches geschieht. Achten Sie besonders auf mehrmals fehlgeschlagene Anmeldungen. 
       - Das Dashboard **Azure AD audit logs** (Azure AD-Überwachungsprotokolle) analysiert Administratoraktivitäten, z. B. Änderungen der Benutzer (Hinzufügen, Entfernen usw.), die Erstellung von Gruppen und andere Änderungen.  

   - Fügen Sie ein Dashboard für Ihre Firewall hinzu. Fügen Sie beispielsweise das Palo Alto-Dashboard hinzu. Das Dashboard analysiert Ihren Firewalldatenverkehr, zeigt Korrelationen zwischen Ihren Firewalldaten und Bedrohungsereignissen auf und hebt verdächtige Ereignisse entitätsübergreifend hervor. Dashboards liefern Ihnen Informationen zu Trends in Ihrem Datenverkehr und ermöglichen Ihnen das Anzeigen von Detailinformationen sowie das Filtern der Ergebnisse. 

      ![Palo Alto-Dashboard](./media/qs-get-visibility/palo-alto-week-query.png)


Sie können die Dashboards anpassen, indem Sie auf ![button](./media/qs-get-visibility/edit-query-button.png) klicken und die Hauptabfrage bearbeiten. Sie können auf die Schaltfläche ![button](./media/qs-get-visibility/go-to-la-button.png) klicken, um [zu Log Analytics zu wechseln und die Abfrage dort zu bearbeiten](../azure-monitor/log-query/get-started-portal.md). Sie können auch auf die Schaltfläche mit den Auslassungspunkten (...) klicken und **Kacheldaten anpassen** auswählen, um den Filter für die Hauptzeit zu bearbeiten oder bestimmte Kacheln aus dem Dashboard zu entfernen.

Weitere Informationen zum Verwenden von Abfragen finden Sie unter [Tutorial: Visualisieren von Daten in Log Analytics](../azure-monitor/learn/tutorial-logs-dashboards.md).

### <a name="add-a-new-tile"></a>Hinzufügen einer neuen Kachel

Wenn Sie eine neue Kachel hinzufügen möchten, können Sie sie einem vorhandenen Dashboard hinzufügen (einem von Ihnen erstellten Dashboard oder einem integrierten Azure Sentinel-Dashboard). 
1. Erstellen Sie in Log Analytics eine Kachel, indem Sie den Anweisungen in [Tutorial: Erstellen und Freigeben von Dashboards von Log Analytics-Daten](../azure-monitor/learn/tutorial-logs-dashboards.md) folgen. 
2. Nachdem die Kachel erstellt wurde, wählen Sie unter **Anheften** das Dashboard aus, auf dem die Kachel angezeigt werden soll.

## <a name="create-new-dashboards"></a>Erstellen neuer Dashboards
Sie können ein komplett neues Dashboard erstellen oder ein integriertes Dashboard als Grundlage für das neue Dashboard verwenden.

1. Klicken Sie zum Erstellen eines neuen Dashboards auf **Dashboards**, und wählen Sie dann **+ Neues Dashboard** aus.
2. Wählen Sie das Abonnement aus, in dem Sie das Dashboard erstellen, und geben Sie einen beschreibenden Namen ein. Jedes Dashboard ist eine normale Azure-Ressource. Sie können ihm Rollen zuweisen (rollenbasierte Zugriffssteuerung; Role-Based Access Control, RBAC), um die zugriffsberechtigten Benutzer festzulegen und einzuschränken. 
3. Damit das Dashboard in Ihren Dashboards angezeigt wird und Visualisierungen angeheftet werden können, müssen Sie es freigeben. Klicken Sie auf **Freigeben** und dann auf **Benutzer verwalten**. 
 
1. Verwenden Sie die Optionen **Zugriff überprüfen** und **Rollenzuweisungen** wie bei jeder anderen Azure-Ressource. Weitere Informationen finden Sie unter [Freigeben von Azure-Dashboards mithilfe der rollenbasierten Zugriffssteuerung](../azure-portal/azure-portal-dashboard-share-access.md).


## <a name="new-dashboard-examples"></a>Beispiele für neue Dashboards

Mit der folgenden Beispielabfrage können Sie Datenverkehrstrends über mehrere Wochen hinweg vergleichen. Sie können den Gerätehersteller und die Datenquelle, für den bzw. die Sie die Abfrage ausführen, ganz einfach ändern. In diesem Beispiel wird „SecurityEvent“ von Windows verwendet. Sie können die Abfrage ändern, um sie für eine beliebige andere Firewall für „AzureActivity“ oder „CommonSecurityLog“ auszuführen.

     |where DeviceVendor = = "Palo Alto Networks":
      // week over week query
      SecurityEvent
      | where TimeGenerated > ago(14d)
      | summarize count() by bin(TimeGenerated, 1d)
      | extend Week = iff(TimeGenerated>ago(7d), "This Week", "Last Week"), TimeGenerated = iff(TimeGenerated>ago(7d), TimeGenerated, TimeGenerated + 7d)


Möglicherweise möchten Sie eine Abfrage erstellen, die Daten aus mehreren Quellen umfasst. Sie können eine Abfrage erstellen, die in Azure Active Directory-Überwachungsprotokollen nach neu erstellten Benutzern sucht und dann in Ihren Azure-Protokollen überprüft, ob der jeweilige Benutzer innerhalb von 24 Stunden nach der Erstellung begonnen hat, Änderungen an Rollenzuweisungen vorzunehmen. Eine solche verdächtige Aktivität würde in diesem Dashboard angezeigt werden:

    AuditLogs
    | where OperationName == "Add user"
    | project AddedTime = TimeGenerated, user = tostring(TargetResources[0].userPrincipalName)
    | join (AzureActivity
    | where OperationName == "Create role assignment"
    | project OperationName, RoleAssignmentTime = TimeGenerated, user = Caller) on user
    | project-away user1

Sie können basierend auf der Rolle der Person, die die Daten anzeigt, und den gesuchten Daten verschiedene Dashboards erstellen. Sie können beispielsweise ein Dashboard für Ihren Netzwerkadministrator erstellen, das Firewalldaten enthält. Sie können Dashboards auch basierend auf der Häufigkeit erstellen, mit der Sie sie anzeigen möchten. Während Ihnen bei manchen Daten eine tägliche Überprüfung genügt, möchten Sie andere vielleicht stündlich einsehen (beispielsweise Ihre Azure AD-Anmeldungen, um einmal pro Stunde nach Anomalien zu suchen). 

## <a name="create-new-detections"></a>Erstellen neuer Erkennungen

Erstellen Sie Erkennungen für die [Datenquellen, die Sie mit Azure Sentinel verbunden haben](connect-data-sources.md), um Bedrohungen in Ihrer Organisation zu untersuchen.

Nutzen Sie beim Erstellen einer neuen Erkennung die von Microsoft-Sicherheitsexperten entwickelten integrierten Erkennungen, die auf die verbundenen Datenquellen zugeschnitten sind.

1. Navigieren Sie in der [GitHub-Community](https://github.com/Azure/Azure-Sentinel/tree/master/Detections) zum Ordner **Detections** (Erkennungen), und wählen Sie die entsprechenden Ordner aus.
   ![entsprechende Ordner](./media/qs-get-visibility/detection-folders.png)
 
3.  Navigieren Sie zur Registerkarte **Analytics**, und wählen Sie **Hinzufügen** aus.
   ![Erstellen einer Regel in Log Analytics](./media/qs-get-visibility/query-params.png)

3.  Kopieren Sie alle Parameter in die Regel, und klicken Sie auf **Erstellen**.
   ![Erstellen einer Warnungsregel](./media/qs-get-visibility/create-alert-rule.png)

 
## <a name="next-steps"></a>Nächste Schritte
In diesem Schnellstart wurden die ersten Schritte mit Azure Sentinel erläutert. Fahren Sie mit dem Tutorial zum [Erkennen von Bedrohungen](tutorial-detect-threats.md) fort.
> [!div class="nextstepaction"]
> Verwenden Sie die [Bedrohungserkennung](tutorial-detect-threats.md), um Ihre Maßnahmen zur Reaktion auf Bedrohungen zu automatisieren.

