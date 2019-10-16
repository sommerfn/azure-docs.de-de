---
title: Visualisierung von Abhängigkeiten in Azure Migrate | Microsoft-Dokumentation
description: Bietet eine Übersicht über Bewertungsberechnungen im Serverbewertungsdienst von Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: hamusa
ms.openlocfilehash: 5b71146f0c2aff51a0c2498705b047e9fa4632c8
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72178134"
---
# <a name="dependency-visualization"></a>Visualisierung von Abhängigkeiten

Von der Azure Die Serverbewertung bewertet Gruppen von lokalen Computern für die Migration zu Azure. Sie können die Funktionalität zur Visualisierung von Abhängigkeiten in der Serverbewertung zum Erstellen von Gruppen nutzen. Dieser Artikel enthält Informationen zu diesem Feature.

> [!NOTE]
> Die Funktion zur Visualisierung von Abhängigkeiten ist in Azure Government nicht verfügbar.

## <a name="overview"></a>Übersicht

Mithilfe der Abhängigkeitsvisualisierung in der Serverbewertung können Sie Gruppen mit hoher Vertrauenswürdigkeit für Migrationsbewertungen erstellen. Sie können die Abhängigkeitsvisualisierung verwenden, um Netzwerkabhängigkeiten von Computern anzuzeigen und zugehörige Computer zu identifizieren, die ebenfalls zu Azure migriert werden müssen. Diese Funktionalität ist in Szenarios nützlich, in denen nicht vollständig bekannt ist, welche Computer für Ihre Anwendung erforderlich sind und in die Migration zu Azure eingeschlossen werden müssen.

## <a name="before-you-start"></a>Vorbereitung

- Stellen Sie sicher, dass Sie ein Azure Migrate-Projekt [erstellt](how-to-add-tool-first-time.md) haben.
- Wenn Sie bereits ein Projekt erstellt haben, vergewissern Sie sich, dass Sie das Tool Azure Migrate-Serverbewertung[hinzugefügt](how-to-assess.md): Migrate-Serverbewertung bewerten.
- Stellen Sie sicher, dass Sie Ihre Computer in Azure Migrate ermittelt haben. Sie können dies tun, indem Sie eine Azure Migrate-Appliance für [VMware](how-to-set-up-appliance-vmware.md) oder [Hyper-V](how-to-set-up-appliance-hyper-v.md) einrichten. Die Appliance ermittelt lokale Computer und sendet Metadaten und Leistungsdaten an Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen. [Weitere Informationen](migrate-appliance.md)

## <a name="how-does-it-work"></a>Wie funktioniert dies?

Azure Migrate verwendet für die Abhängigkeitsvisualisierung die [Dienstzuordnung](../operations-management-suite/operations-management-suite-service-map.md) in [Azure Monitor-Protokolle](../log-analytics/log-analytics-overview.md).
- Sie müssen jedem Azure Migrate-Projekt einen neuen oder vorhandenen Log Analytics-Arbeitsbereich zuordnen, um die Abhängigkeitsvisualisierung nutzen zu können.
- Sie können einen Arbeitsbereich nur in dem Abonnement erstellen oder anfügen, in dem das Azure Migrate-Projekt erstellt wird.
- So fügen Sie einen Log Analytics-Arbeitsbereich einem Projekt an:
    1. Klicken Sie auf der Registerkarte **Server** unter der Kachel **Azure Migrate: Serverbewertung** auf **Übersicht**.
    2. Klicken Sie in der **Übersicht** auf den Abwärtspfeil, um **Essentials** zu erweitern.
    3. Klicken Sie im **OMS-Arbeitsbereich** auf **Konfiguration erforderlich**.
    4. Geben Sie unter **Arbeitsbereich konfigurieren** an, ob Sie einen neuen Arbeitsbereich erstellen oder einen bestehenden verwenden möchten:
    
    ![Arbeitsbereich hinzufügen](./media/how-to-create-group-machine-dependencies/workspace.png)

- Beim Zuordnen eines Arbeitsbereichs erhalten Sie die Möglichkeit, einen neuen Arbeitsbereich zu erstellen oder einen vorhandenen anzufügen:
  - Wenn Sie einen neuen Arbeitsbereich erstellen, müssen Sie für diesen einen Namen angeben. Sie können die [Region](https://azure.microsoft.com/global-infrastructure/regions/) auswählen, in der der Arbeitsbereich erstellt wird.
  - Wenn Sie einen vorhandenen Arbeitsbereich anfügen, können Sie zwischen allen verfügbaren Arbeitsbereichen im selben Abonnement wie das Migrationsprojekt auswählen. Beachten Sie, dass nur die Arbeitsbereiche aufgeführt sind, die in einer Region erstellt wurden, in der die [Dienstzuordnung unterstützt wird](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites). Um einen Arbeitsbereich anzufügen, stellen Sie sicher, dass Sie Leserzugriff auf den Arbeitsbereich haben.

  > [!NOTE]
  > Nachdem Sie einem Projekt einen Arbeitsbereich hinzugefügt haben, können Sie ihn nicht mehr ändern.

  > [!NOTE]
  > Azure Migrate unterstützt derzeit die Erstellung oder Zuordnung von Log Analytics-Arbeitsbereichen in den Regionen „USA, Osten“, „Asien, Südosten“ und „Europa, Westen“. Wenn der Arbeitsbereich außerhalb von Azure Migrate in einer nicht unterstützten Region erstellt wird, kann er derzeit keinem Azure Migrate-Projekt zugeordnet werden. 

- Der zugeordnete Arbeitsbereich wird mit dem Schlüssel **Migration Project** (Migrationsprojekt) und dem Wert **project name** (Projektname) markiert, die Sie für die Suche im Azure-Portal verwenden können.
- Um zum Arbeitsbereich zu navigieren, der dem Projekt zugeordnet ist, können Sie den Abschnitt **Essentials** (Zusammenfassung) der Seite **Übersicht** des Projekts öffnen und auf den Arbeitsbereich zugreifen.

    ![Navigieren zum Log Analytics-Arbeitsbereich](./media/concepts-dependency-visualization/oms-workspace.png)

Zur Verwendung der Abhängigkeitsvisualisierung müssen Sie Agents auf alle lokalen Computer, die Sie analysieren möchten, herunterladen und dort installieren.  

- Der [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) muss auf jedem Computer installiert werden. [Weitere Informationen](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies#install-the-mma) zum Installieren des MMA-Agent.
- Der [Dependency-Agent](../azure-monitor/platform/agents-overview.md#dependency-agent) muss auf jedem Computer installiert werden. [Weitere Informationen](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies#install-the-dependency-agent) zum Installieren des Abhängigkeits-Agent.
- Falls Sie über Computer ohne Internetverbindung verfügen, ist es außerdem erforderlich, auf diesen das Log Analytics-Gateway herunterzuladen und zu installieren.

Sie benötigen diese Agents auf Computern nur, wenn Sie die Abhängigkeitsvisualisierung verwenden.

## <a name="do-i-need-to-pay-for-it"></a>Fallen dadurch Kosten an?

Das Feature für die Abhängigkeitsvisualisierung steht ohne zusätzliche Gebühr zur Verfügung. Für die Verwendung des Features für die Abhängigkeitsvisualisierung in der Serverbewertung ist die Dienstzuordnung erforderlich. Außerdem müssen Sie dem Azure Migrate-Projekt einen neuen oder vorhandenen Log Analytics-Arbeitsbereich zuweisen. Die Funktionalität zur Visualisierung von Abhängigkeiten in der Serverbewertung kann in den ersten 180 Tagen kostenlos genutzt werden.

1. Für andere Lösungen als die Dienstzuordnung in diesem Log Analytics-Arbeitsbereich fallen die [Log Analytics-Standardgebühren](https://azure.microsoft.com/pricing/details/log-analytics/) an.
2. Für die Lösung „Dienstzuordnung“ fallen in den ersten 180 Tagen ab dem Tag der Zuordnung des Log Analytics-Arbeitsbereichs zum Azure Migrate-Projekt keine Kosten an, um Migrationsszenarios ohne weitere Gebühren zu unterstützen. Nach 180 Tagen fallen die Log Analytics-Standardgebühren an.

Verwenden Sie beim Registrieren von Agents für den Arbeitsbereich die ID und den Schlüssel, die bzw. der auf der Seite mit Schritten für die Agent-Installation für das Projekt angegeben wurde.

Wenn das Azure Migrate-Projekt gelöscht wurde, wird der Arbeitsbereich nicht zusammen mit dem Projekt gelöscht. Nach dem Löschen des Projekts ist die Nutzung der Dienstzuordnung nicht mehr kostenlos, und für jeden Knoten werden Kosten gemäß des kostenpflichtigen Tarifs für den Log Analytics-Arbeitsbereich berechnet.

> [!NOTE]
> Für das Feature für die Abhängigkeitsvisualisierung wird die Dienstzuordnung über einen Log Analytics-Arbeitsbereich verwendet. Seit dem 28. Februar 2018 ist das Feature mit der Ankündigung der allgemeinen Verfügbarkeit von Azure Migrate jetzt ohne weitere Kosten verfügbar. Sie müssen ein neues Projekt erstellen, um den kostenlosen Arbeitsbereich nutzen zu können. Für Arbeitsbereiche, die bereits vor der allgemeinen Verfügbarkeit vorhanden waren, können weiterhin Kosten anfallen. Daher empfehlen wir Ihnen, die Umstellung auf ein neues Projekt durchzuführen.

Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="how-do-i-manage-the-workspace"></a>Wie verwalte ich den Arbeitsbereich?

Sie können den Log Analytics-Arbeitsbereich außerhalb von Azure Migrate verwalten. Er wird nicht gelöscht, wenn Sie das Azure Migrate-Projekt löschen, in dem er erstellt wurde. Wenn Sie den Arbeitsbereich nicht mehr benötigen, [löschen Sie ihn](../azure-monitor/platform/manage-access.md) manuell.

Löschen Sie den von Azure Migrate erstellten Arbeitsbereich nur, wenn Sie auch das Azure Migrate-Projekt löschen. Wenn Sie dies tun, funktioniert die Abhängigkeitsvisualisierung nicht wie erwartet.

## <a name="next-steps"></a>Nächste Schritte
- [Group machines using machine dependency mapping](how-to-create-group-machine-dependencies.md) (Gruppieren von Computern mithilfe der Computerabhängigkeitszuordnung)
- [Erfahren Sie mehr](https://docs.microsoft.com/azure/migrate/resources-faq#what-is-dependency-visualization) über die häufig gestellten Fragen (FAQs) zur Visualisierung von Abhängigkeiten.
