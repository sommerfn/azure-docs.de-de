---
title: Erstellen einer Bewertung mit Azure Migrate-Serverbewertung | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Bewertung mit dem Tool Azure Migrate-Serverbewertung ausführen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: cffde2a677650387dffd19733e082ff7002ccb55
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "68228977"
---
# <a name="create-an-assessment"></a>Erstellen einer Bewertung

In diesem Artikel wird beschrieben, wie Sie eine Bewertung für lokale VMware-VMs oder Hyper-V-VMs mit Azure Migrate erstellen: Serverbewertung.

[Azure Migrate](migrate-services-overview.md) hilft Ihnen bei der Migration zu Azure. Azure Migrate bietet einen zentralisierten Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration lokaler Infrastruktur, Anwendungen und Daten zu Azure. Der Hub stellt Azure-Tools für die Bewertung und Migration sowie unabhängige Drittanbietertools bereit. 

## <a name="before-you-start"></a>Vorbereitung

- Stellen Sie sicher, dass Sie ein Azure Migrate-Projekt [erstellt](how-to-add-tool-first-time.md) haben.
- Wenn Sie bereits ein Projekt erstellt haben, stellen vergewissern Sie sich, dass Sie das Tool Azure Migrate-Serverbewertung [hinzugefügt](how-to-assess.md) haben.
- Zum Erstellen einer Bewertung müssen Sie eine Azure Migrate-Appliance für [VMware](how-to-set-up-appliance-vmware.md) oder [Hyper-V](how-to-set-up-appliance-hyper-v.md) einrichten. Die Appliance ermittelt lokale Computer und sendet Metadaten und Leistungsdaten an Azure Migrate: Serverbewertung. [Weitere Informationen](migrate-appliance.md)


## <a name="assessment-overview"></a>Bewertungsübersicht
Es gibt zwei Arten von Bewertungen, die Sie mit Azure Migrate erstellen können: Serverbewertung.

**Bewertung** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen basierend auf gesammelten Leistungsdaten | **Empfohlene VM-Größe**: Basierend auf CPU- und Arbeitsspeicher-Nutzungsdaten.<br/><br/> **Empfohlener Datenträgertyp (Verwalteter Datenträger vom Typ Standard oder Premium)** : Basierend auf IOPS und Durchsatz der lokalen Datenträger.
**Wie lokal** | Bewertungen basierend auf lokaler Größenanpassung. | **Empfohlene VM-Größe**: Basierend auf der Größe des lokalen virtuellen Computers<br/><br> **Empfohlener Datenträgertyp**: Basierend auf der für die Bewertung ausgewählten Speichertypeinstellung.

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.

## <a name="run-an-assessment"></a>Durchführen einer Bewertung

Führen Sie eine Bewertung wie folgt aus:

1. Schauen Sie sich die [bewährten Methoden](best-practices-assessment.md) für das Erstellen von Bewertungen an.
2. Klicken Sie auf der Registerkarte **Server** auf der Kachel **Azure Migrate: Serverbewertung** auf **Bewerten**.

    ![Bewerten](./media/how-to-create-assessment/assess.png)

2. Geben Sie unter **Server bewerten** einen Namen für die Bewertung an.
3. Klicken Sie auf **Alle anzeigen**, um die Eigenschaften für die Bewertung zu überprüfen.

    ![Bewertungseigenschaften](./media/how-to-create-assessment//view-all.png)

3. Wählen Sie unter **Gruppe auswählen oder erstellen** die Option **Neu erstellen** aus, und geben Sie einen Namen für die Gruppe ein. Eine Gruppe sammelt mindestens einen virtuellen Computer zur Bewertung.
4. Wählen Sie unter **Computer zur Gruppe hinzufügen** die VMs aus, die der Gruppe hinzugefügt werden sollen.
5. Klicken Sie auf **Bewertung erstellen**, um die Gruppe zu erstellen und die Bewertung auszuführen.

    ![Erstellen einer Bewertung](./media/how-to-create-assessment//assessment-create.png)

6. Zeigen Sie die Bewertung nach der Erstellung in **Server** > **Azure Migrate: Serverbewertung** > **Bewertungen** an.
7. Klicken Sie auf **Bewertung exportieren**, um sie als Excel-Datei herunterzuladen.



## <a name="review-an-assessment"></a>Überprüfen einer Bewertung

Eine Bewertung beschreibt Folgendes:

- **Azure-Bereitschaft**: Gibt an, ob VMs für die Migration zu Azure geeignet sind.
- **Schätzung der monatlichen Kosten**: Die geschätzten monatlichen Compute-und Speicherkosten für die Ausführung der virtuellen Computer in Azure.
- **Schätzung der monatlichen Speicherkosten**: Geschätzte Kosten für den Datenträgerspeicher nach der Migration.

### <a name="view-an-assessment"></a>Anzeigen einer Bewertung

1. Klicken Sie **Migrationsziele** >  **Server** in **Azure Migrate: Serverbewertung** auf **Bewertungen**.
2. Klicken Sie in **Bewertungen** auf eine Bewertung, um sie zu öffnen.

    ![Zusammenfassung der Bewertung](./media/how-to-create-assessment/assessment-summary.png)

### <a name="review-azure-readiness"></a>Überprüfen der Azure-Bereitschaft

1. Überprüfen Sie in **Azure-Bereitschaft**, ob VMs für die Migration zu Azure bereit sind.
2. Überprüfen Sie den VM-Status:
    - **Bereit für Azure**: Azure Migrate empfiehlt in der Bewertung eine VM-Größe an und gibt Kostenschätzungen für VMs ab.
    - **Bereit mit Bedingungen**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Nicht bereit für Azure**: Zeigt Probleme und eine vorgeschlagene Abhilfe an.
    - **Bereitschaft unbekannt**: Wird verwendet, wenn Azure Migrate die Bereitschaft aufgrund von Problemen mit der Datenverfügbarkeit nicht bewerten kann.

2. Klicken Sie auf einen **Azure-Bereitschaftsstatus**. Sie können Details zur VM-Bereitschaft anzeigen und einen Drilldown ausführen, um VM-Details wie Compute-, Speicher- und Netzwerkeinstellungen anzuzeigen.



### <a name="review-cost-details"></a>Überprüfen der Kostendetails

In dieser Ansicht werden die geschätzten Compute- und Speicherkosten für die Ausführung der VMs in Azure angezeigt.

1. Überprüfen Sie die monatlichen Compute-und Speicherkosten. Die Kosten für alle VMs in der bewerteten Gruppe werden aggregiert.

    - Kostenschätzungen basieren auf den Größenempfehlungen für einen Computer sowie seinen Datenträgern und Eigenschaften.
    - Die geschätzten monatlichen Kosten für Computing und Speicher werden angezeigt.
    - Die Kostenschätzung gilt für die Ausführung der lokalen VMS als IaaS-VMS. Bei der Azure Migrate-Serverbewertung werden PaaS- oder SaaS-Kosten nicht berücksichtigt.

2. Sie können die Schätzungen der monatliche Speicherkosten überprüfen. Diese Ansicht zeigt aggregierte Speicherkosten für die bewertete Gruppe, aufgeteilt auf unterschiedliche Typen von Speicherdatenträgern.
3. Sie können einen Drilldown ausführen, um die Details für einen bestimmten virtuellen Computer anzuzeigen.


### <a name="review-confidence-rating"></a>Prüfen der Zuverlässigkeitsstufe

Wenn Sie leistungsbasierte Bewertungen ausführen, wird der Bewertung eine Zuverlässigkeitsstufe zugewiesen.

![Zuverlässigkeitsstufe](./media/how-to-create-assessment/confidence-rating.png)

- Es wird eine Bewertung von 1 Stern (niedrigste Stufe) bis 5 Sterne (höchste Stufe) vergeben.
- Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von der Bewertung bereitgestellten Größenempfehlungen besser einschätzen.
- Die Zuverlässigkeitsstufe basiert auf der Verfügbarkeit von Datenpunkten, die zum Berechnen der Bewertung erforderlich sind.

Die Zuverlässigkeitsstufen für eine Bewertung sehen wie folgt aus.

**Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
--- | ---
0 % bis 20 % | Ein Stern
21 % bis 40 % | Zwei Sterne
41 % bis 60 % | Drei Sterne
61 % bis 80 % | Vier Sterne
81 % bis 100 % | Fünf Sterne




## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das Erstellen von Gruppen mit hoher Vertrauenswürdigkeit mithilfe der [Zuordnung von Abhängigkeiten](how-to-create-group-machine-dependencies.md).
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
