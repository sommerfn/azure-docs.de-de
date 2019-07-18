---
title: Ermitteln und Bewerten lokaler virtueller VMware-Computer für die Migration zu Azure mit Azure Migrate | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie lokale virtuelle VMware-Computer für die Migration zu Azure mithilfe des Azure Migrate-Diensts ermittelt und bewertet werden.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 06/10/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c16c7fdfdf3efe5518e2452871fdeace0a4a8526
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807964"
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Ermitteln und Bewerten lokaler virtueller VMware-Computer für die Migration zu Azure

Mit dem [Azure Migrate](migrate-overview.md)-Dienst werden lokale Workloads für die Migration zu Azure bewertet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Kontos, das von Azure Migrate zum Ermitteln lokaler virtueller Computer verwendet wird
> * Erstellen eines Azure Migrate-Projekts
> * Einrichten eines lokalen virtuellen Collectorcomputers zum Ermitteln lokaler virtueller VMware-Computer für die Bewertung
> * Gruppieren virtueller Computer und Erstellen einer Bewertung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

- **VMware**: Die virtuellen Computer, die Sie migrieren möchten, müssen von vCenter Server mit der Version 5.5, 6.0, 6.5 oder 6.7 verwaltet werden. Zusätzlich benötigen Sie einen ESXi-Host mit Version 5.5 oder höher, um die Collector-VM bereitzustellen.
- **vCenter Server-Konto:** Sie benötigen ein schreibgeschütztes Konto, um auf vCenter Server zugreifen zu können. Dieses Konto wird in Azure Migrate zum Ermitteln der lokalen virtuellen Computer verwendet.
- **Berechtigungen:** In vCenter Server benötigen Sie Berechtigungen zum Erstellen eines virtuellen Computers durch Importieren einer Datei im OVA-Format.

## <a name="create-an-account-for-vm-discovery"></a>Erstellen eines Kontos für die VM-Ermittlung

Azure Migrate benötigt Zugriff auf VMware-Server, um automatisch virtuelle Computer für die Bewertung ermitteln zu können. Erstellen Sie ein VMware-Konto mit folgenden Eigenschaften. Dieses Konto wird bei der Einrichtung von Azure Migrate angegeben.

- Benutzertyp: Mindestens ein Benutzer mit Lesezugriff
- Berechtigungen: Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt)
- Details: Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.
- Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle „No access“ (Kein Zugriff) mit „Propagate to child object“ (An untergeordnetes Objekt weitergeben) zu.


## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-project"></a>Erstellen eines Projekts

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Suchen Sie nach **Azure Migrate**, und wählen Sie in den Suchergebnissen den Dienst **Azure Migrate** aus. Klicken Sie dann auf **Erstellen**.
3. Geben Sie einen Projektnamen und das Azure-Abonnement für das Projekt an.
4. Erstellen Sie eine neue Ressourcengruppe.
5. Geben Sie die Region an, in der das Projekt erstellt werden soll, und klicken Sie dann auf **Erstellen**. Sie können ein Azure Migrate-Projekt nur in den folgenden geografischen Regionen erstellen. Sie können Ihre Migration jedoch trotzdem für jeden beliebigen Zielort von Azure planen. Die für das Projekt angegebene Region wird nur zum Speichern der Metadaten verwendet, die von den lokalen VMs erfasst werden.

**Geografie** | **Speicherort**
--- | ---
Azure Government | US Government, Virginia
Asien | Asien, Südosten
Europa | „Europa, Norden“ oder „Europa, Westen“
USA | „USA, Osten“ oder „USA, Westen-Mitte“

![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)


## <a name="download-the-collector-appliance"></a>Herunterladen der Collectorappliance

Azure Migrate erstellt einen lokalen virtuellen Computer, der als „Collectorappliance“ bezeichnet wird. Dieser virtuelle Computer ermittelt lokale virtuelle VMware-Computer und sendet Metadaten zu diesen Computern an den Azure Migrate-Dienst. Zum Einrichten der Collectorappliance laden Sie eine OVA-Datei herunter und importieren sie auf den lokalen vCenter-Server, um den virtuellen Computer zu erstellen.

1. Klicken Sie im Azure Migrate-Projekt auf **Erste Schritte** > **Ermitteln und bewerten** > **Computer ermitteln**.
2. Klicken Sie unter **Computer ermitteln** auf **Herunterladen**, um die Appliance herunterzuladen.

    Die Azure Migrate-Appliance kommuniziert mit vCenter Server und erstellt kontinuierlich Profile der lokalen Umgebung, um in Echtzeit Nutzungsdaten für jeden virtuellen Computer zu erfassen. Sie erfasst Spitzenleistungsindikatoren für jede Metrik (CPU-Auslastung, Arbeitsspeicherauslastung usw.). Dieses Modell ist nicht von den Statistikeinstellungen von vCenter Server für die Sammlung von Leistungsdaten abhängig. Sie können die kontinuierliche Profilerstellung jederzeit von der Appliance aus beenden.

    > [!NOTE]
    > Die Appliance zur einmaligen Ermittlung ist inzwischen veraltet, da diese Methode bei der Verfügbarkeit der Leistungsdatenpunkte von den Statistikeinstellungen von vCenter Server abhängig war und durchschnittliche Leistungsindikatoren erfasst hat, was zu unterdimensionierten virtuellen Computern für die Migration zu Azure führte.

    **Schnellbewertungen:** Mit der Appliance für die kontinuierliche Ermittlung können Sie nach Abschluss der Ermittlung (dauert je nach Anzahl von virtuellen Computern einige Stunden) sofort Bewertungen erstellen. Da die Erfassung von Leistungsdaten bereits beim Start der Ermittlung beginnt, empfiehlt es sich, das Größenkriterium in der Bewertung auf *Wie lokal* festzulegen, um Schnellbewertungen zu erhalten. Bei leistungsbasierten Bewertungen empfiehlt es sich, nach dem Start der Ermittlung mindestens einen Tag zu warten, um zuverlässige Größenempfehlungen zu erhalten.

    Die Appliance erfasst nur Leistungsdaten kontinuierlich. Sie erkennt keine Konfigurationsänderungen in der lokalen Umgebung (also hinzugefügte virtuelle Computer, Löschvorgänge, hinzugefügte Datenträger und Ähnliches). Wenn sich die Konfiguration in der lokalen Umgebung ändert, können Sie wie folgt vorgehen, damit die Änderungen im Portal berücksichtigt werden:

    - Hinzugefügte Elemente (virtuelle Computer, Datenträger, Kerne usw.): Damit diese Änderungen im Azure-Portal berücksichtigt werden, können Sie die Ermittlung über die Appliance beenden und anschließend wieder starten. Dadurch wird sichergestellt, dass die Änderungen im Azure Migrate-Projekt aktualisiert werden.

    - Gelöschte virtuelle Computer: Das Löschen von virtuellen Computern wird aufgrund des Entwurfs der Appliance auch dann nicht berücksichtigt, wenn Sie die Ermittlung beenden und wieder starten. Das liegt daran, dass Daten nachfolgender Ermittlungen älteren Ermittlungen angefügt und nicht überschrieben werden. In diesem Fall können Sie die VM im Portal einfach ignorieren, indem Sie sie aus Ihrer Gruppe entfernen und die Bewertung neu berechnen.


3. Kopieren Sie in **Projektanmeldeinformationen kopieren** die Projekt-ID und den Projektschlüssel. Diese benötigen Sie beim Konfigurieren des Collectors.

    ![Herunterladen der OVA-Datei](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Überprüfen der Collectorappliance

Überprüfen Sie, ob die OVA-Datei sicher ist, bevor Sie sie bereitstellen.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Der generierte Hash muss folgenden Einstellungen entsprechen.

#### <a name="continuous-discovery"></a>Kontinuierliche Ermittlung

  Für OVA-Version 1.0.10.15

  **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | dfa1838b1e64f7cde51915927220cf48
    SHA1 | 24bdbd9c37c7366567ff252db3a37a13dda9de42
    SHA256 | e9f8f16ceb970c27dd068f5a5f7a4b2fd336f2820e9d6247d510ba6824e3f06c

  Für OVA-Version 1.0.10.11

  **Algorithmus** | **Hashwert**
    --- | ---
    MD5 | 5f6b199d8272428ccfa23543b0b5f600
    SHA1 | daa530de6e8674a66a728885a7feb3b0a2e8ccb0
    SHA256 | 85da50a21a7a6ca684418a87ccc1dd4f8aab30152c438a17b216ec401ebb3a21

  Für OVA-Version 1.0.10.9

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | 169f6449cc1955f1514059a4c30d138b
  SHA1 | f8d0a1d40c46bbbf78cd0caa594d979f1b587c8f
  SHA256 | d68fe7d94be3127eb35dd80fc5ebc60434c8571dcd0e114b87587f24d6b4ee4d

  Für OVA-Version 1.0.10.4

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
  SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
  SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be


#### <a name="one-time-discovery-deprecated-now"></a>Einmalige Ermittlung (jetzt veraltet)

Dieses Modell ist inzwischen veraltet. Für bereits vorhandene Appliances wird jedoch noch Support bereitgestellt.

  Für OVA-Version 1.0.9.15

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
  SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
  SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

  Für OVA-Version 1.0.9.14

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
  SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
  SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c

  Für OVA-Version 1.0.9.12

  **Algorithmus** | **Hashwert**
  --- | ---
  MD5 | d0363e5d1b377a8eb08843cf034ac28a
  SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
  SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

## <a name="create-the-collector-vm"></a>Erstellen der Collector-VM

Importieren Sie die heruntergeladene Datei auf den vCenter Server.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).

    ![Bereitstellen der OVF-Vorlage](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. Geben Sie im Assistenten „Deploy OVF Template“ (OVF-Vorlage bereitstellen) > **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Speicherort** einen Anzeigenamen für den virtuellen Collectorcomputer und das Inventarobjekt an, in dem der virtuelle Computer gehostet wird.
5. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf dem der virtuelle Collectorcomputer ausgeführt wird.
7. Geben Sie im Speicher das Speicherziel für den virtuellen Collectorcomputer an.
8. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
9. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem der virtuelle Collectorcomputer eine Verbindung herstellt. Das Netzwerk benötigt eine Internetverbindung, damit Metadaten an Azure gesendet werden können.
10. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**.

## <a name="run-the-collector-to-discover-vms"></a>Ausführen des Collectors zum Ermitteln virtueller Computer

1. Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den virtuellen Computer, und wählen Sie **Konsole öffnen** aus.
2. Geben Sie für die Appliance die Einstellungen für Sprache, Zeitzone und Kennwort an.
3. Klicken Sie auf dem Desktop auf die Verknüpfung **Collector ausführen**.
4. Klicken Sie auf der oberen Leiste der Collector-Benutzeroberfläche auf **Nach Updates suchen**, und vergewissern Sie sich, dass die neueste Version des Collectors ausgeführt wird. Andernfalls können Sie über den Link das neueste Upgradepaket herunterladen und den Collector aktualisieren.
5. Öffnen Sie im Azure Migrate-Collector die Option **Erforderliche Komponenten einrichten**.
   - Wählen Sie die geplante Azure-Cloud für die Migration aus (Azure Global oder Azure Government).
   - Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
   - Der Collector überprüft, ob der virtuelle Computer über Internetzugriff verfügt.
   - Wenn der virtuelle Computer über einen Proxy auf das Internet zugreift, klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an. Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert. [Hier](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-prerequisites) finden Sie weitere Informationen zu den Anforderungen für Internetkonnektivität und die [Liste der URLs](https://docs.microsoft.com/azure/migrate/concepts-collector), auf die der Collector zugreift.

     > [!NOTE]
     > Die Proxyadresse muss im Format http:\//ProxyIPAddress oder http:\//ProxyFQDN eingegeben werden. Es werden nur HTTP-Proxys unterstützt. Bei Verwendung eines abfangenden Proxys tritt bei der Internetverbindung unter Umständen zunächst ein Fehler auf, wenn Sie das Proxyzertifikat nicht importiert haben. Wie Sie dieses Problem beheben, indem Sie das Proxyzertifikat als vertrauenswürdiges Zertifikat auf dem virtuellen Collectorcomputer importieren, erfahren Sie [hier](https://docs.microsoft.com/azure/migrate/concepts-collector).

   - Der Collector überprüft, ob der Collector-Dienst ausgeführt wird. Der Dienst wird standardmäßig auf dem virtuellen Collectorcomputer installiert.
   - Laden Sie die VMware PowerCLI herunter, und installieren Sie sie.

6. Nehmen Sie in **vCenter Server-Details angeben** die folgenden Einstellungen vor:
    - Geben Sie den Namen (FQDN) oder die IP-Adresse des vCenter-Servers an.
    - Geben Sie unter **Benutzername** und **Kennwort** die Anmeldeinformationen für das schreibgeschützte Konto an, über das der Collector virtuelle Computer auf dem vCenter-Server ermittelt.
    - Wählen Sie unter **Sammlungsbereich** einen Bereich für die Ermittlung virtueller Computer aus. Der Collector kann nur virtuelle Computer innerhalb des angegebenen Bereichs ermitteln. Der Bereich kann auf einen bestimmten Ordner, ein Rechenzentrum oder einen Cluster festgelegt werden. Er sollte nicht mehr als 1500 VMs umfassen. [Erfahren Sie mehr](how-to-scale-assessment.md) über die Möglichkeiten zum Erkennen einer größeren Umgebung.

       > [!NOTE]
       > Im **Sammlungsbereich** sind nur Ordner von Hosts und Clustern aufgeführt. Ordner von virtuellen Computern können nicht direkt als Sammlungsbereich ausgewählt werden. Die Ermittlung ist aber mit einem vCenter-Konto möglich, für das Zugriff auf die einzelnen virtuellen Computer besteht. [Erfahren Sie mehr](https://docs.microsoft.com/azure/migrate/how-to-scale-assessment#set-up-permissions) zur Bereichseinteilung eines Ordners mit virtuellen Computern.

7. Geben Sie unter **Migrationsprojekt angeben** die ID und den Schlüssel für das Azure Migrate-Projekt an, die sie im Portal kopiert haben. Wenn Sie diese Angaben nicht kopiert haben, öffnen Sie das Azure-Portal über den virtuellen Collectorcomputer. Klicken Sie auf der Seite **Übersicht** des Projekts auf **Computer ermitteln**, und kopieren Sie die Werte.  
8. Überwachen Sie in **Sammlungsfortschritt anzeigen** den Ermittlungsstatus. [Erfahren Sie mehr](https://docs.microsoft.com/azure/migrate/concepts-collector) über die vom Azure Migrate-Collector gesammelten Daten.

> [!NOTE]
> Der Collector unterstützt nur „Englisch (USA)“ als Sprache des Betriebssystems und die Sprache der Collectorschnittstelle.
> Wenn Sie die Einstellungen auf einem Computer ändern, auf den Sie zugreifen möchten, lösen Sie erneut die Ermittlung aus, bevor Sie die Bewertung ausführen. Verwenden Sie dazu im Collector die Option **Sammlung erneut starten**. Nachdem die Sammlung abgeschlossen ist, wählen Sie im Portal die Option **Neu berechnen** für die Bewertung aus, um die aktualisierten Bewertungsergebnisse zu erhalten.


### <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Die Collector-Appliance erstellt kontinuierlich Profile für die lokale Umgebung und sendet stündlich Leistungsdaten. Die Computer können eine Stunde nach Beginn der Ermittlung im Portal angezeigt werden.

1. Klicken Sie im Migrationsprojekt auf **Verwalten** > **Computer**.
2. Überprüfen Sie, ob die zu ermittelnden virtuellen Computer im Portal angezeigt werden.


## <a name="create-and-view-an-assessment"></a>Erstellen und Anzeigen einer Bewertung

Nach der Ermittlung der virtuellen Computer im Portal können Sie sie gruppieren und Bewertungen erstellen. Nachdem die virtuellen Computer im Portal ermittelt wurden, können Sie sofort Bewertungen vom Typ „Wie lokal“ erstellen. Bei leistungsbasierten Bewertungen empfiehlt es sich, mindestens einen Tag zu warten, um zuverlässige Größenempfehlungen zu erhalten.

1. Klicken Sie auf der Seite **Übersicht** des Projekts auf **+ Bewertung erstellen**.
2. Klicken Sie auf **Alle anzeigen**, um die Eigenschaften für die Bewertung zu überprüfen.
3. Erstellen Sie die Gruppe, und geben Sie einen Gruppennamen an.
4. Wählen Sie die Computer aus, die der Gruppe hinzugefügt werden sollen.
5. Klicken Sie auf **Bewertung erstellen**, um die Gruppe und die Bewertung zu erstellen.
6. Zeigen Sie die Bewertung nach der Erstellung in **Übersicht** > **Dashboard** an.
7. Klicken Sie auf **Bewertung exportieren**, um sie als Excel-Datei herunterzuladen.

> [!NOTE]
> Es wird dringend empfohlen, nach dem Start der Ermittlung mindestens einen Tag zu warten, bevor Sie eine Bewertung erstellen. Eine vorhandene Bewertung können Sie mithilfe des Befehls **Neu berechnen** für die Bewertung mit den neuesten Leistungsdaten aktualisieren.

### <a name="assessment-details"></a>Bewertungsdetails

Eine Bewertung enthält Informationen dazu, ob die lokalen virtuellen Computer mit Azure kompatibel sind, welche VM-Größe für die Ausführung des virtuellen Computers in Azure geeignet ist und wie hoch die voraussichtlichen monatlichen Azure-Kosten sind.

![Bewertungsbericht](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure-Bereitschaft

Die Azure-Bereitschaftsansicht in der Bewertung gibt Aufschluss über den Bereitschaftsstatus der einzelnen virtuellen Computer. Jeder virtuelle Computer kann abhängig von seinen Eigenschaften eine der folgenden Kennzeichnungen erhalten:
- Bereit für Azure
- Bedingt bereit für Azure
- Nicht bereit für Azure
- Bereitschaft unbekannt

Für virtuelle Computer, die bereit sind, empfiehlt Azure Migrate eine VM-Größe in Azure. Die Größenempfehlung von Azure Migrate basiert auf dem in den Bewertungseigenschaften angegebenen Größenkriterium. Bei Verwendung des leistungsbasierten Größenkriteriums wird bei der Größenempfehlung der Leistungsverlauf der virtuellen Computer (CPU und Arbeitsspeicher) sowie der Datenträger (IOPS und Durchsatz) berücksichtigt. Wenn als Größenkriterium „Wie lokal“ verwendet wird, berücksichtigt Azure Migrate keine Leistungsdaten für den virtuellen Computer und die Datenträger. Für die VM-Größenempfehlung in Azure wird die Größe des virtuellen Computers in der lokalen Umgebung herangezogen, und die Größe des virtuellen Computers orientiert sich an dem Speichertyp, der in den Bewertungseigenschaften angegeben ist (standardmäßig Premiumdatenträger). Weitere Informationen zur Größenanpassung in Azure Migrate finden Sie [hier](concepts-assessment-calculation.md).

Bei virtuellen Computern, die nicht oder nur bedingt für Azure bereit sind, informiert Azure Migrate über die Bereitschaftsprobleme sowie über mögliche Abhilfemaßnahmen.

Virtuelle Computer, für die Azure Migrate die Azure-Bereitschaft aufgrund fehlender Daten nicht bestimmen kann, werden mit „Bereitschaft unbekannt“ gekennzeichnet.

Neben den Angaben zu Azure-Bereitschaft und Größe schlägt Azure Migrate auch Tools vor, die Sie für die Migration des virtuellen Computers verwenden können. Dies erfordert eine tiefergehende Ermittlung in der lokalen Umgebung. Lesen Sie [hier](how-to-get-migration-tool.md), wie Sie durch Installieren von Agents auf den lokalen Computern eine tiefergehende Ermittlung durchführen können. Wenn die Agents nicht auf den lokalen Computern installiert sind, wird eine Lift & Shift-Migration mit [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) empfohlen. Wenn die Agents auf dem lokalen Computer installiert sind, untersucht Azure Migrate die auf dem Computer ausgeführten Prozesse und ermittelt, ob es sich um einen Datenbankcomputer handelt. Ist der Computer ein Datenbankcomputer, wird [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) empfohlen. Andernfalls wird Azure Site Recovery als Migrationstool empfohlen.

  ![Bewertungsbereitschaft](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Geschätzte monatliche Kosten

Diese Ansicht zeigt die gesamten Compute- und Speicherkosten für den Betrieb der virtuellen Computer in Azure zusammen mit den Details für die einzelnen Computer. Kostenschätzungen werden unter Berücksichtigung der Azure Migrate-Größenempfehlungen für einen Computer sowie unter Berücksichtigung seiner Datenträger und der Bewertungseigenschaften berechnet.

> [!NOTE]
> Die Kostenschätzung von Azure Migrate bezieht sich auf die Ausführung der lokalen virtuellen Computer als virtuelle Azure IaaS-Computer (Infrastructure-as-a-Service). Darin werden keine PaaS- (Platform-as-a-Service) oder SaaS-Kosten (Software-as-a-Service) berücksichtigt.

Die geschätzten monatlichen Kosten für Compute und Speicher werden für alle virtuellen Computer in der Gruppe aggregiert.

![Bewertung der Kosten für die virtuellen Computer](./media/tutorial-assessment-vmware/assessment-vm-cost.png)

#### <a name="confidence-rating"></a>Zuverlässigkeitsstufe

Jeder leistungsbasierten Bewertung in Azure Migrate wird eine Zuverlässigkeitsstufe zugeordnet. Die Skala reicht von einem Stern (niedrigster Wert) bis zu fünf Sternen (höchster Wert). Die Zuverlässigkeitsstufe wird einer Bewertung auf der Grundlage der Verfügbarkeit von Datenpunkten zugeordnet, die zum Berechnen der Bewertung erforderlich sind. Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von Azure Migrate bereitgestellten Größenempfehlungen besser einschätzen. Die Zuverlässigkeitsstufe gilt nicht für lokale „Ist-Zustand“-Bewertungen.

Bei Verwendung der leistungsbasierten Größenanpassung benötigt Azure Migrate Nutzungsdaten für CPU und Arbeitsspeicher des virtuellen Computers. Außerdem werden für jeden an den virtuellen Computer angefügten Datenträger Informationen zu IOPS und Durchsatzdaten benötigt. Analog dazu benötigt Azure Migrate für jeden Netzwerkadapter, der an einen virtuellen Computer angefügt ist, Informationen zu ein- und ausgehenden Netzwerkdaten, um die leistungsbasierte Größenanpassung durchführen zu können. Steht eine der oben aufgeführten Nutzungsangaben in vCenter Server nicht zur Verfügung, ist die Größenempfehlung von Azure Migrate unter Umständen nicht zuverlässig. Die Zuverlässigkeitsstufe für die Bewertung ist abhängig davon, wie viele Datenpunkte verfügbar sind (in Prozent), wie es nachfolgend angegeben ist:

   **Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
   --- | ---
   0 % bis 20 % | Ein Stern
   21 % bis 40 % | Zwei Sterne
   41 % bis 60 % | Drei Sterne
   61 % bis 80 % | Vier Sterne
   81 % bis 100 % | Fünf Sterne

Dass für eine Bewertung nicht alle Datenpunkte verfügbar sind, kann folgende Ursachen haben:

- Sie haben für den Zeitraum, für den Sie die Bewertung erstellen, kein Profil Ihrer Umgebung erstellt. Wenn Sie z.B. die Bewertung mit einer auf 1 Tag festgelegten Leistungsdauer erstellen, müssen Sie bis mindestens einen Tag nach dem Start der Ermittlung warten, bis alle Datenpunkte gesammelt sind.

- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, heruntergefahren. Wenn ein virtueller Computer für eine gewisse Zeit heruntergefahren wird, können für diesen Zeitraum keine Leistungsdaten gesammelt werden.

- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, erstellt. Ein Beispiel: Angenommen, Sie erstellen eine Bewertung für den Leistungsverlauf des letzten Monats, und in der Umgebung wurden letzte Woche einige virtuelle Computer erstellt. In solchen Fällen liegt für die neuen virtuellen Computer kein Leistungsverlauf für den gesamten Zeitraum vor.

> [!NOTE]
> Beträgt die Zuverlässigkeitsstufe weniger als fünf Sterne, warten Sie mindestens einen Tag, damit die Appliance ein Profil der Umgebung erstellen kann, und führen Sie dann eine *Neuberechnung* der Bewertung durch. Sollte das nicht möglich sein, ist die leistungsbasierte Größenanpassung möglicherweise nicht zuverlässig, und es empfiehlt sich, in den Bewertungseigenschaften stattdessen die Größenanpassung vom Typ *Wie lokal* festzulegen.

## <a name="next-steps"></a>Nächste Schritte

- [Informationen](how-to-modify-assessment.md) zum Anpassen einer Bewertung entsprechend Ihren Anforderungen
- Informationen zum Erstellen vertrauenswürdiger Bewertungsgruppen mithilfe der [Zuordnung von Computerabhängigkeiten](how-to-create-group-machine-dependencies.md)
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
- [Weitere Informationen](how-to-scale-assessment.md) zum Ermitteln und Bewerten einer umfangreichen VMware-Umgebung
- [Weitere Informationen](resources-faq.md) zu den häufig gestellten Fragen für Azure Migrate