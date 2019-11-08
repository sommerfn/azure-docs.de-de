---
title: Gruppieren von Computern in Azure Migrate mithilfe der Visualisierung von Abhängigkeiten ohne Agent
description: In diesem Abschnitt wird beschrieben, wie Gruppen mithilfe von Computerabhängigkeiten ohne Agent erstellt werden.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/23/2019
ms.author: hamusa
ms.openlocfilehash: d646187627d74810a846d7126562e2e796c5c9f7
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510275"
---
# <a name="set-up-agentless-dependency-visualization-for-assessment"></a>Einrichten einer Visualisierung der Abhängigkeit für die Bewertung ohne Agent

Dieser Artikel beschreibt, wie Sie die Zuordnung von Abhängigkeiten ohne Agent in Azure Migrate: Server Assessment einrichten. Diese Funktion ist zurzeit als Vorschauversion für VMware-Computer verfügbar, die mit einer Azure Migrate-Appliance ermittelt wurden. 

> [!IMPORTANT]
> Die Visualisierung von Abhängigkeiten ohne Agent ist derzeit als Vorschauversion für Azure VMware-VMs verfügbar, die mithilfe einer Azure Migrate-Appliance ermittelt wurden.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="about-dependency-mapping"></a>Informationen zur Abhängigkeitszuordnung 

Mit der Abhängigkeitszuordnung können Sie Abhängigkeiten zwischen Computern visualisieren, die Sie bewerten und migrieren möchten. Sie verwenden die Abhängigkeitszuordnung normalerweise, wenn Sie Computer mit höherer Zuverlässigkeit bewerten möchten.

- In Azure Migrate: Serverbewertung fassen Sie die Computer, die Sie bewerten möchten, in Gruppen zusammen. Gruppen bestehen normalerweise aus Computern, die Sie zusammen migrieren möchten. Mithilfe der Abhängigkeitszuordnung können Sie Abhängigkeiten zwischen den Computer überprüfen, um die Computer korrekt zu gruppieren.
- Mit der Zuordnung können Sie alle unabhängigen Systeme ermitteln, die zusammen migriert werden müssen. Sie können bestimmen, ob ein ausgeführtes System noch benötigt wird oder eher außer Betrieb gesetzt als migriert werden sollte.
- Die Visualisierung von Abhängigkeiten trägt dazu bei, dass nichts übersehen wird. Außerdem werden unerwartete Ausfälle während der Migration vermieden.

## <a name="about-agentless-visualization"></a>Informationen zur Visualisierung ohne Agent

Bei der Visualisierung von Abhängigkeiten ohne Agent müssen Sie keine Agents auf Computern installieren. Dazu werden TCP-Verbindungsdaten von den Computern erfasst, für die sie aktiviert wurde.

- Nachdem die Abhängigkeitsermittlung gestartet wurde, sammelt die Appliance Daten von Computern in einem Abrufintervall von fünf Minuten.
- Die folgenden Daten werden gesammelt:
    - TCP-Verbindungen
    - Namen von Prozessen mit aktiven Verbindungen
    - Namen der installierten Anwendungen, die die obigen Prozesse ausführen
    - Anzahl von Verbindungen, die in jedem Abrufintervall erkannt werden

## <a name="current-limitations"></a>Aktuelle Einschränkungen

- Die Visualisierung von Abhängigkeiten ohne Agent ist derzeit nur für VMware-VMs verfügbar.
- In der Abhängigkeitsanalyseansicht können Sie zurzeit keine Server hinzufügen oder aus einer Gruppe entfernen. 
- Eine Abhängigkeitszuordnung für eine Gruppe von Servern ist zurzeit nicht verfügbar.
- Die Abhängigkeitsdaten können derzeit nicht im Tabellenformat heruntergeladen werden.

## <a name="before-you-start"></a>Vorbereitung

- Stellen Sie sicher, dass Sie ein Azure Migrate-Projekt [erstellt](how-to-add-tool-first-time.md) haben.
- Die Abhängigkeitsanalyse ohne Agent ist zurzeit nur für VMware-Computer verfügbar.
- Wenn Sie bereits ein Projekt erstellt haben, vergewissern Sie sich, dass Sie das Tool Azure Migrate-Serverbewertung[hinzugefügt](how-to-assess.md): Migrate-Serverbewertung bewerten.
- Vergewissern Sie sich, dass Sie die VMware-Computer in Azure Migrate ermittelt haben. Sie können dazu eine Azure Migrate-Appliance für [VMware](how-to-set-up-appliance-vmware.md) einrichten. Die Appliance ermittelt lokale Computer und sendet Metadaten und Leistungsdaten an Azure Migrate: Server Assessment“ (Azure Migrate-Serverbewertung) erstellen. [Weitere Informationen](migrate-appliance.md)
- Vergewissern Sie sich, dass die VMware-VMs für die Visualisierung ohne Agent unterstützt werden (siehe Übersicht in der folgenden Tabelle).


### <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
 
Die folgenden Betriebssysteme werden für die Abhängigkeitsvisualisierung ohne Agent unterstützt.

**Typ** | **Unterstützte Betriebssysteme**
--- | --- 
**Windows** | Microsoft Windows Server 2016 <br/> Microsoft Windows Server 2012 R2 <br/> Microsoft Windows Server 2012 <br/> Microsoft Windows Server 2008 R2 (64-Bit) 
**Linux** | Red Hat Enterprise Linux 7, 6, 5 <br/> Ubuntu Linux 14.04, 16.04 <br/> Debian 7, 8 <br/> Oracle Linux 6, 7 <br/> CentOS 5, 6, 7  


## <a name="create-a-user-account-for-discovery"></a>Erstellen eines Benutzerkontos für die Ermittlung

Richten Sie ein Benutzerkonto ein, das über die erforderlichen Berechtigungen verfügt, damit die Serverbewertung für die Ermittlung auf die VM zugreifen kann. Sie können ein Benutzerkonto angeben.

- **Erforderliche Berechtigung für Windows-VMs**: Für das Benutzerkonto ist „Gast“-Zugriff erforderlich.
- **Erforderliche Berechtigung für Linux-VMs**: Für das Konto ist root-Berechtigung erforderlich. Alternativ benötigt das Benutzerkonto diese beiden Funktionen für /bin/netstat- und /bin/ls-Dateien: CAP_DAC_READ_SEARCH und CAP_SYS_PTRACE.

## <a name="add-the-user-account-to-the-appliance"></a>Hinzufügen des Benutzerkontos zur Appliance

Sie müssen das Benutzerkonto zur Appliance hinzufügen.

Fügen Sie das Konto wie folgt hinzu:

1. Öffnen Sie die Verwaltungs-App für die Appliance. Navigieren Sie zum Bereich **vCenter-Details angeben**.
2. Klicken Sie im Abschnitt **Anwendungen und Abhängigkeiten auf VMs ermitteln** auf **Anmeldeinformationen hinzufügen**.
3. Wählen Sie das **Betriebssystem** aus. 
4. Geben Sie einen Anzeigenamen für das Konto an.
5. Geben Sie den **Benutzernamen** und das **Kennwort** ein.
6. Klicken Sie auf **Speichern**.
7. Klicken Sie auf **Speichern und Ermittlung starten**.

    ![VM-Benutzerkonto hinzufügen](./media/how-to-create-group-machine-dependencies-agentless/add-vm-credential.png)

## <a name="start-dependency-discovery"></a>Starten der Abhängigkeitsermittlung

Wählen Sie die Computer aus, auf denen Sie die Abhängigkeitsermittlung aktivieren möchten.

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Klicken Sie auf das Symbol **Abhängigkeitsanalyse**.
3. Klicken Sie auf **Abhängigkeitsermittlung starten**.
3. Wählen Sie auf der Seite **Abhängigkeitsermittlung starten** die Appliance aus, die die relevanten Computer ermittelt.
4. Wählen Sie in der Liste der Computer die Computer aus.
5. Klicken Sie auf **Abhängigkeitsermittlung starten**.

    ![Starten der Abhängigkeitsermittlung](./media/how-to-create-group-machine-dependencies-agentless/start-dependency-discovery.png)

Nach dem Start der Abhängigkeitsermittlung können Sie die Abhängigkeiten 6 Stunden lang visualisieren.

## <a name="visualize-dependencies"></a>Visualisieren von Abhängigkeiten

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Suchen Sie den Computer, für den Sie die Abhängigkeitszuordnung anzeigen möchten.
3. Klicken Sie in der Spalte **Abhängigkeiten** auf **Abhängigkeiten anzeigen**.
4. Ändern Sie den Zeitraum, für den Sie die Zuordnung anzeigen möchten, über die Dropdown-Liste **Zeitdauer**.
5. Erweitern Sie die Gruppe **Client**, um die Computer aufzulisten, die eine Abhängigkeit zum ausgewählten Computer aufweisen. 
6. Erweitern Sie die Gruppe **Port**, um die Computer aufzulisten, die eine Abhängigkeit vom ausgewählten Computer aufweisen. 
7. Wenn Sie zur Zuordnungsansicht eines abhängigen Computers navigieren möchten, klicken Sie auf den Computernamen und dann auf **Serverzuordnung laden**.

    ![Erweitern der Portgruppe des Servers und Laden der Serverzuordnung](./media/how-to-create-group-machine-dependencies-agentless/load-server-map.png)

    ![Erweitern der Clientgruppe ](./media/how-to-create-group-machine-dependencies-agentless/expand-client-group.png)

8. Erweitern Sie den ausgewählten Computer, um Details auf Prozessebene für die einzelnen Abhängigkeiten anzuzeigen.

    ![Erweitern des Servers zum Anzeigen von Prozessen](./media/how-to-create-group-machine-dependencies-agentless/expand-server-processes.png)

> [!NOTE]
> Prozessinformationen für eine Abhängigkeit sind nicht immer verfügbar. Wenn keine Informationen verfügbar sind, wird die Abhängigkeit mit „Unbekannter Prozess“ als Prozess dargestellt.

## <a name="stop-dependency-discovery"></a>Beenden der Abhängigkeitsermittlung

Wählen Sie die Computer aus, auf denen Sie die Abhängigkeitsermittlung beenden möchten.

1. Klicken Sie in **Azure Migrate: Serverbewertung** auf **Ermittelte Server**.
2. Klicken Sie auf das Symbol **Abhängigkeitsanalyse**.
3. Klicken Sie auf **Abhängigkeitsermittlung beenden**.
3. Wählen Sie auf der Seite **Abhängigkeitsermittlung beenden** die **Appliance** aus, die die virtuellen Computer ermittelt, auf denen die Abhängigkeitsermittlung beendet werden soll.
4. Wählen Sie in der Liste der Computer die Computer aus.
5. Klicken Sie auf **Abhängigkeitsermittlung beenden**.


## <a name="next-steps"></a>Nächste Schritte

[Gruppieren der Computer](how-to-create-a-group.md)
