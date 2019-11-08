---
title: Abhängigkeitsvisualisierung in Azure Migrate
description: Bietet eine Übersicht über Bewertungsberechnungen im Serverbewertungsdienst von Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: hamusa
ms.openlocfilehash: 17ba06d6ac09f220b4343092292275a1cc315377
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73489217"
---
# <a name="dependency-visualization"></a>Visualisierung von Abhängigkeiten

In diesem Artikel wird das Feature zur Abhängigkeitsvisualisierung in Azure Migrate beschrieben: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen.

Mit der Abhängigkeitsvisualisierung können Sie Abhängigkeiten zwischen Computern, die Sie bewerten und migrieren möchten, besser verstehen. Sie verwenden die Abhängigkeitszuordnung normalerweise, wenn Sie auf Computer mit höheren Zuverlässigkeitsgraden zugreifen möchten.

- In Azure Migrate: Serverbewertung fassen Sie die Computer, die Sie bewerten möchten, in Gruppen zusammen. Gruppen bestehen normalerweise aus Computern, die Sie zusammen migrieren möchten, und mithilfe der Abhängigkeitsvisualisierung können Sie Abhängigkeiten zwischen den Computern überprüfen, um die Computer korrekt zu gruppieren.
- Mit der Visualisierung können Sie alle unabhängigen Systeme ermitteln, die zusammen migriert werden müssen. Sie können festlegen, ob laufende Systeme noch verwendet werden oder ob Systeme außer Betrieb gesetzt werden können, anstatt migriert zu werden.
- Die Visualisierung von Abhängigkeiten trägt dazu bei, dass nichts zurückbleibt. Außerdem werden unerwartete Ausfälle während der Migration vermieden.
- Diese Funktion ist besonders nützlich, wenn Sie nicht genau wissen, welche Computer zu Apps gehören und daher zusammen zu Azure migriert werden sollten.


> [!NOTE]
> Die Funktion zur Visualisierung von Abhängigkeiten ist in Azure Government nicht verfügbar.

## <a name="agent-based-and-agentless"></a>Agent-basiert und ohne Agent

Es gibt zwei Optionen für die Bereitstellung der Abhängigkeitsvisualisierung:

- **Visualisierung von Abhängigkeiten ohne Agent:** Diese Option befindet sich zurzeit in der Vorschau. Sie erfordert keine Installation von Agents auf Computern. 
    - Sie erfasst TCP-Verbindungsdaten von den Computern, für die sie aktiviert wurde. [Weitere Informationen](how-to-create-group-machine-dependencies-agentless.md)
Nachdem die Abhängigkeitsermittlung gestartet wurde, sammelt die Appliance Daten von Computern in einem Abrufintervall von fünf Minuten.
    - Die folgenden Daten werden gesammelt:
        - TCP-Verbindungen
        - Namen von Prozessen mit aktiven Verbindungen
        - Namen der installierten Anwendungen, die die obigen Prozesse ausführen
        - Nein. von Verbindungen, die in jedem Abrufintervall erkannt werden
- **Agent-basierte Visualisierung von Abhängigkeiten:** Zur Verwendung der Agent-basierten Visualisierung von Abhängigkeiten müssen Sie die folgenden Agents auf alle lokalen Computer, die Sie analysieren möchten, herunterladen und dort installieren.  
    - Der [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) muss auf jedem Computer installiert werden. [Weitere Informationen](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies#install-the-mma) zum Installieren des MMA-Agent.
    - Der [Dependency-Agent](../azure-monitor/platform/agents-overview.md#dependency-agent) muss auf jedem Computer installiert werden. [Weitere Informationen](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies#install-the-dependency-agent) zum Installieren des Abhängigkeits-Agent.
    - Falls Sie über Computer ohne Internetverbindung verfügen, ist es außerdem erforderlich, auf diesen das Log Analytics-Gateway herunterzuladen und zu installieren.

## <a name="agent-based-requirements"></a>Agent-basierte Anforderungen

### <a name="what-do-i-need-to-deploy-dependency-visualization"></a>Was benötige ich für die Visualisierung von Abhängigkeiten?

Bevor Sie die Abhängigkeitsvisualisierung bereitstellen, sollten Sie über ein Azure Migrate Projekt verfügen, dem das Tool Azure Migrate: Serverbewertung hinzugefügt wurde. Sie stellen die Abhängigkeitsvisualisierung nach dem Einrichten einer Azure Migrate-Appliance zum Ermitteln Ihrer lokalen Computer bereit.

[Erfahren Sie mehr](how-to-assess.md) über das Hinzufügen des Tools und das Bereitstellen einer Appliance für [Hyper-V](how-to-set-up-appliance-hyper-v.md)-, [VMware](how-to-set-up-appliance-vmware.md)- oder physische Server.


### <a name="how-does-it-work"></a>Wie funktioniert dies?

Azure Migrate verwendet für die Abhängigkeitsvisualisierung die [Dienstzuordnung](../operations-management-suite/operations-management-suite-service-map.md) in [Azure Monitor-Protokolle](../log-analytics/log-analytics-overview.md).

- Sie müssen jedem Azure Migrate-Projekt einen Log Analytics-Arbeitsbereich (neu oder bereits vorhanden) zuordnen, um die Abhängigkeitsvisualisierung nutzen zu können.
- Der Arbeitsbereich muss sich im selben Abonnement befinden, in dem Sie das Azure Migrate-Projekt erstellen.
- Azure Migrate unterstützt die Erstellung von Arbeitsbereichen in den Regionen „USA, Osten“, „Asien, Südosten“ und „Europa, Westen“. Arbeitsbereiche in anderen Regionen können keinem Projekt zugeordnet werden. Beachten Sie außerdem, dass sich der Arbeitsbereich in einer Region befinden muss, in der die [Dienstzuordnung unterstützt wird](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites).
- Der Arbeitsbereich für ein Azure Migrate-Projekt kann nach dem Hinzufügen nicht mehr geändert werden.
- In Log Analytics wird der Arbeitsbereich, der Azure Migrate zugeordnet ist, mit dem Schlüssel des Migrationsprojekts und dem Projektnamen gekennzeichnet.

    ![Navigieren zum Log Analytics-Arbeitsbereich](./media/concepts-dependency-visualization/oms-workspace.png)



### <a name="do-i-need-to-pay-for-it"></a>Fallen dadurch Kosten an?

Für die Abhängigkeitsvisualisierung werden die Dienstzuordnung und ein Log Analytics-Arbeitsbereich benötigt. 

- Die Dienstzuordnungslösung verursacht in den ersten 180 Tagen keine Gebühren. Dies gilt ab dem Zeitpunkt, an dem Sie Ihren Log Analytics-Arbeitsbereich dem Azure Migrate-Projekt zuordnen.
- Nach 180 Tagen fallen die Log Analytics-Standardgebühren an.
- Für andere Lösungen als die Dienstzuordnung im zugeordneten Log Analytics-Arbeitsbereich fallen die [Log Analytics-Standardgebühren](https://azure.microsoft.com/pricing/details/log-analytics/) an.
- Wenn das Azure Migrate-Projekt gelöscht wurde, wird der Arbeitsbereich nicht zusammen mit dem Projekt gelöscht. Nach dem Löschen des Projekts ist die Nutzung der Dienstzuordnung nicht mehr kostenlos, und für jeden Knoten werden Kosten gemäß dem kostenpflichtigen Tarif für den Log Analytics-Arbeitsbereich berechnet.

Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

> [!NOTE]
> Wenn Sie über Projekte verfügen, die Sie vor der allgemeinen Verfügbarkeit von Azure Migrate am 28. Februar 2018 erstellt haben, fallen für Sie möglicherweise zusätzliche Gebühren für die Dienstzuordnung an. Um sicherzustellen, dass Sie wirklich erst nach 180 Tagen bezahlen müssen, sollten Sie ein neues Projekt erstellen, da vorhandene Arbeitsbereiche, die vor der allgemeinen Verfügbarkeit erstellt wurden, noch immer kostenpflichtig sind.



### <a name="how-do-i-manage-the-workspace"></a>Wie verwalte ich den Arbeitsbereich?

- Wenn Sie Agents im Arbeitsbereich registrieren, verwenden Sie die ID und den Schlüssel, die vom Azure Migrate-Projekt bereitgestellt werden.
- Sie können den Log Analytics-Arbeitsbereich außerhalb von Azure Migrate verwalten.
- Wenn Sie das zugehörige Azure Migrate-Projekt löschen, wird der Arbeitsbereich nicht automatisch gelöscht. Er muss [manuell gelöscht](../azure-monitor/platform/manage-access.md) werden.
- Löschen Sie den von Azure Migrate erstellten Arbeitsbereich nur, wenn Sie auch das Azure Migrate-Projekt löschen. Wenn Sie dies tun, funktioniert die Abhängigkeitsvisualisierung nicht wie erwartet.

## <a name="next-steps"></a>Nächste Schritte
- [Group machines using machine dependency mapping](how-to-create-group-machine-dependencies.md) (Gruppieren von Computern mithilfe der Computerabhängigkeitszuordnung)
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/migrate/resources-faq#what-is-dependency-visualization) über die häufig gestellten Fragen (FAQs) zur Visualisierung von Abhängigkeiten.


