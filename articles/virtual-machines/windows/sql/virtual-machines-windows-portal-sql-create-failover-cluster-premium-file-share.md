---
title: 'SQL Server-Failoverclusterinstanz mit Premium-Dateifreigabe: Azure Virtual Machines'
description: In diesem Artikel wird beschrieben, wie Sie für Azure Virtual Machines eine SQL Server-Failoverclusterinstanz mithilfe einer Premium-Dateifreigabe erstellen.
services: virtual-machines
documentationCenter: na
author: MashaMSFT
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/09/2019
ms.author: mathoma
ms.openlocfilehash: cebfb5315a7a0b7cf92dbb9e3e77ff4c35851aa8
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72757817"
---
# <a name="configure-sql-server-failover-cluster-instance-with-premium-file-share-on-azure-virtual-machines"></a>Konfigurieren der SQL Server-Failoverclusterinstanz mit einer Premium-Dateifreigabe für Azure Virtual Machines

In diesem Artikel wird erläutert, wie Sie auf Azure-VMs eine SQL Server-Failoverclusterinstanz (FCI) mithilfe einer [Premium-Dateifreigabe](../../../storage/files/storage-how-to-create-premium-fileshare.md) erstellen. 

Premium-Dateifreigaben sind SSD-gestützte Dateifreigaben mit durchgängig geringer Latenz, die für die Verwendung mit der Failoverclusterinstanz für SQL Server 2012 oder höher unter Windows Server 2012 oder höher vollständig unterstützt werden. Premium-Dateifreigaben bieten Ihnen mehr Flexibilität, sodass Sie ohne Ausfallzeiten die Größe der Dateifreigabe ändern und diese skalieren können. 


## <a name="before-you-begin"></a>Voraussetzungen

Es gibt einige Dinge, die Sie wissen müssen, und einige Voraussetzungen müssen erfüllt sein, bevor Sie fortfahren können.

Sie sollten die folgende Technologie verwenden können:

- [Windows-Clustertechnologie](/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server-Failoverclusterinstanzen](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server).

Ein wichtiger Unterschied besteht darin, dass für einen Azure IaaS-VM-Failovercluster ein einzelner Netzwerkadapter pro Server (Clusterknoten) und ein einzelnes Subnetz empfohlen wird. Azure-Netzwerktechnologie bietet physische Redundanz, die zusätzliche Netzwerkadapter und Subnetze in einem Azure IaaS-VM-Gastcluster überflüssig macht. Obwohl im Clusterprüfbericht die Warnung ausgegeben wird, dass die Knoten nur in einem einzigen Netzwerk erreichbar sind, kann diese Warnung für Azure IaaS-VM-Failovercluster einfach ignoriert werden. 

Außerdem sollten Sie über Grundlagenkenntnisse in Bezug auf die folgende Technologie verfügen:

- [Azure Premium-Dateifreigabe](../../../storage/files/storage-how-to-create-premium-fileshare.md)
- [Azure-Ressourcengruppen](../../../azure-resource-manager/manage-resource-groups-portal.md)

> [!IMPORTANT]
> Zurzeit werden Failoverclusterinstanzen von SQL Server auf virtuellen Azure-Computern nur mit dem [schlanken](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider) Verwaltungsmodus der [SQL Server IaaS-Agent-Erweiterung](virtual-machines-windows-sql-server-agent-extension.md) unterstützt. Um aus dem vollständigen Erweiterungsmodus in den schlanken Modus zu wechseln, löschen Sie die Ressource **Virtueller SQL-Computer** für die entsprechenden VMs, und registrieren Sie sie dann beim SQL VM-Ressourcenanbieter im Modus [Schlank](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider). Deaktivieren Sie das Kontrollkästchen neben dem richtigen virtuellen Computer, wenn Sie die Ressource **Virtueller SQL-Computer** mithilfe des Azure-Portals löschen. Die vollständige Erweiterung unterstützt Funktionen wie die automatische Sicherung, Patchen und erweiterte Portalverwaltung. Diese Funktionen stehen für SQL-VMs nach der erneuten Installation im [schlanken](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider) Verwaltungsmodus nicht zur Verfügung.

### <a name="workload-consideration"></a>Überlegungen zur Workload

Premium-Dateifreigaben bieten IOPS und Durchsatzkapazität, die den Anforderungen vieler Workloads gerecht werden. Für E/A-intensive Workloads sollten Sie jedoch eine [SQL Server-FCI mit „Direkte Speicherplätze“ ](virtual-machines-windows-portal-sql-create-failover-cluster.md) auf Basis von verwalteten Premium-Datenträgern oder Ultra Disks in Erwägung ziehen.  

Überprüfen Sie die IOPS-Aktivität Ihrer aktuellen Umgebung, und vergewissern Sie sich, dass Premium-Dateifreigaben die benötigten IOPS bereitstellen, bevor Sie eine Bereitstellung oder Migration einleiten. Verwenden Sie die Leistungsindikatoren für Datenträger des Windows-Systemmonitors, und überwachen Sie die gesamten IOPS (Datenträgerübertragung/Sek.) und den Durchsatz (Datenträgerbytes/Sek.), die für SQL Server-Daten, Protokoll- und Temp DB-Dateien erforderlich sind. Für viele Workloads werden E/A-Bursts geboten, sodass es ratsam ist, in Zeiten hoher Nutzung eine Prüfung vorzunehmen und die maximalen IOPS sowie die durchschnittlichen IOPS zu notieren. Premium-Dateifreigaben stellen IOPS basierend auf der Größe der Freigabe bereit. Premium-Dateifreigaben bieten auch einen kostenlosen Burstmodus, mit dem Sie Ihre E/A-Kapazität bis zu eine Stunde um das Dreifache erhöhen können. 

Weitere Informationen zur Leistung von Premium-Dateifreigaben finden Sie unter [Leistungsstufen von Dateifreigaben](https://docs.microsoft.com/azure/storage/files/storage-files-planning#file-share-performance-tiers). 

### <a name="licensing-and-pricing"></a>Lizenzierung und Preise

Unter Azure Virtual Machines können Sie SQL Server lizenzieren, indem Sie VM-Images des Typs „Nutzungsbasierte Bezahlung“ (PAYG) oder „Bring Your Own License“ (BYOL, Verwendung Ihrer eigenen Lizenz) verwenden. Der gewählte Imagetyp bestimmt, wie Ihnen die Gebühren berechnet werden.

Bei der PAYG-Lizenzierung fallen für eine Failoverclusterinstanz (FCI) von SQL Server auf Azure Virtual Machines Gebühren für alle FCI-Knoten an, einschließlich der passiven Knoten. Weitere Informationen finden Sie unter [Virtuelle SQL Server Enterprise-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-enterprise/). 

Kunden mit Enterprise Agreement und Software Assurance verfügen über das Recht, für jeden aktiven Knoten einen kostenlosen passiven FCI-Knoten zu verwenden. Um diesen Vorteil in Azure zu nutzen, verwenden Sie BYOL-VM-Images, und verwenden Sie dann die gleiche Lizenz sowohl auf den aktiven als auch auf den passiven Knoten der FCI. Weitere Informationen finden Sie unter [Enterprise Agreement](https://www.microsoft.com/Licensing/licensing-programs/enterprise.aspx).

Einen Vergleich der PAYG- und BYOL-Lizenzierung für SQL Server auf Azure Virtual Machines finden Sie unter [Erste Schritte mit virtuellen SQL-Computern](virtual-machines-windows-sql-server-iaas-overview.md#get-started-with-sql-vms).

Umfassende Informationen zur Lizenzierung von SQL Server finden Sie unter [Preise](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

### <a name="limitations"></a>Einschränkungen

- Filestream wird für einen Failovercluster mit einer Premium-Dateifreigabe nicht unterstützt. Um Filestream zu verwenden, stellen Sie Ihren Cluster mithilfe von [Direkte Speicherplätze](virtual-machines-windows-portal-sql-create-failover-cluster.md) bereit. 

## <a name="prerequisites"></a>Voraussetzungen 

Bevor Sie die Anweisungen in diesem Artikel befolgen, sollten Sie über Folgendes verfügen:

- Ein Microsoft Azure-Abonnement
- Eine Windows-Domäne auf virtuellen Azure-Computern
- Ein Konto mit der Berechtigung zum Erstellen von Objekten auf dem virtuellen Azure-Computer
- Ein virtuelles Azure-Netzwerk und ein Subnetz mit einem ausreichend großen IP-Adressraum für die folgenden Komponenten:
   - Beide virtuellen Computer
   - Die IP-Adresse des Failoverclusters
   - Eine IP-Adresse für jede FCI
- DNS-Konfiguration im Azure-Netzwerk mit Verweis auf die Domänencontroller
- Eine [Premium-Dateifreigabe ](../../../storage/files/storage-how-to-create-premium-fileshare.md) basierend auf dem Speicherkontingent Ihrer Datenbank für Ihre Datendateien. 
- Eine Dateifreigabe für Sicherungen, die sich von der für Ihre Datendateien verwendeten Premium-Dateifreigabe unterscheidet. Dabei kann es sich um eine Standard- oder Premium-Dateifreigabe handeln. 

Wenn diese Voraussetzungen erfüllt sind, können Sie mit dem Erstellen Ihres Failoverclusters fortfahren. Der erste Schritt ist die Erstellung der virtuellen Computer.

## <a name="step-1-create-virtual-machines"></a>Schritt 1: Erstellen von virtuellen Computern

1. Melden Sie sich mit den Daten Ihres Abonnements am [Azure-Portal](https://portal.azure.com) an.

1. [Erstellen Sie eine Azure-Verfügbarkeitsgruppe](../tutorial-availability-sets.md).

   Mit der Verfügbarkeitsgruppe werden virtuelle Computer über Fehler- und Updatedomänen hinweg gruppiert. Mithilfe einer Verfügbarkeitsgruppe wird sichergestellt, dass Ihre Anwendung nicht durch Ausfälle einzelner Komponenten, z.B. des Netzwerkswitchs oder der Stromversorgungseinheit eines Serverracks, beeinträchtigt wird.

   Falls Sie die Ressourcengruppe für Ihre virtuellen Computer noch nicht erstellt haben, können Sie dies beim Erstellen einer Azure-Verfügbarkeitsgruppe nachholen. Führen Sie die folgenden Schritte zum Erstellen der Verfügbarkeitsgruppe aus, wenn Sie das Azure-Portal verwenden:

   - Klicken Sie im Azure-Portal auf **+** , um den Azure Marketplace zu öffnen. Suchen Sie nach der Option **Verfügbarkeitsgruppe**.
   - Klicken Sie auf **Verfügbarkeitsgruppe**.
   - Klicken Sie auf **Create**.
   - Legen Sie auf dem Blatt **Verfügbarkeitsgruppe erstellen** die folgenden Werte fest:
      - **Name**: Ein Name für die Verfügbarkeitsgruppe.
      - **Abonnement**: Ihr Azure-Abonnement.
      - **Ressourcengruppe**: Falls Sie eine vorhandene Gruppe verwenden möchten, können Sie auf **Vorhandene verwenden** klicken und die Gruppe in der Dropdownliste auswählen. Wählen Sie andernfalls die Option **Neu erstellen**, und geben Sie einen Namen für die Gruppe ein.
      - **Standort**: Legen Sie den Standort fest, an dem Sie Ihre virtuellen Computer erstellen möchten.
      - **Fehlerdomänen**: Verwenden Sie den Standardwert (3).
      - **Updatedomänen**: Verwenden Sie den Standardwert (5).
   - Klicken Sie auf **Erstellen**, um die Verfügbarkeitsgruppe zu erstellen.

1. Erstellen Sie die virtuellen Computer in der Verfügbarkeitsgruppe.

   Stellen Sie zwei virtuelle SQL Server-Computer in der Azure-Verfügbarkeitsgruppe bereit. Eine Anleitung finden Sie unter [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md).

   Ordnen Sie beide virtuellen Computer wie folgt an:

   - In derselben Azure-Ressourcengruppe wie Ihre Verfügbarkeitsgruppe
   - In demselben Netzwerk wie Ihr Domänencontroller
   - In einem Subnetz mit einem ausreichend großen IP-Adressraum für beide virtuellen Computer und alle FCIs, die in diesem Cluster unter Umständen verwendet werden sollen
   - In der Azure-Verfügbarkeitsgruppe   

      >[!IMPORTANT]
      >Sie können die Verfügbarkeitsgruppe nicht mehr festlegen oder ändern, nachdem ein virtueller Computer erstellt wurde.

   Wählen Sie über den Azure Marketplace ein Image aus. Sie können ein Marketplace-Image verwenden, das Windows Server und SQL Server oder nur Windows Server enthält. Ausführliche Informationen finden Sie unter [Übersicht über SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

   Die offiziellen SQL Server-Images im Azure-Katalog enthalten eine installierte SQL Server-Instanz sowie die SQL Server-Installationssoftware und den erforderlichen Schlüssel.

   >[!IMPORTANT]
   > Entfernen Sie nach dem Erstellen des virtuellen Computers die vorinstallierte eigenständige SQL Server-Instanz. Sie verwenden die vorinstallierten SQL Server-Medien, um die SQL Server-FCI nach der Konfiguration des Failoverclusters und der Premium-Dateifreigabe als Speicher zu erstellen. 

   Alternativ dazu können Sie auch Azure Marketplace-Images verwenden, die nur das Betriebssystem enthalten. Wählen Sie ein Image des Typs **Windows Server 2016 Datacenter** aus, und installieren Sie die SQL Server-FCI nach der Konfiguration des Failoverclusters und der Premium-Dateifreigabe als Speicher. Dieses Image enthält keine SQL Server-Installationsmedien. Speichern Sie die Installationsmedien an einem Speicherort, an dem Sie die SQL Server-Installation für jeden Server ausführen können. 

1. Stellen Sie eine Verbindung mit jedem virtuellen Computer per RDP her, nachdem Ihre virtuellen Computer von Azure erstellt wurden.

   Wenn Sie die Verbindung mit einem virtuellen Computer per RDP zum ersten Mal herstellen, wird gefragt, ob Sie zulassen möchten, dass dieser PC über das Netzwerk ermittelt werden kann. Klicken Sie auf **Ja**.

1. Entfernen Sie die SQL Server-Instanz, wenn Sie eines der SQL Server-basierten VM-Images verwenden.

   - Klicken Sie unter **Programme und Features** mit der rechten Maustaste auf **Microsoft SQL Server 201_ (64-Bit)** , und klicken Sie dann auf **Deinstallieren/Ändern**.
   - Klicken Sie auf **Entfernen**.
   - Wählen Sie die Standardinstanz aus.
   - Entfernen Sie alle Funktionen unter **Datenbank-Engine-Dienst**. Achten Sie darauf, dass Sie **Freigegebene Funktionen** nicht entfernen. Sehen Sie sich die folgende Abbildung an:

      ![Entfernen von Funktionen](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - Klicken Sie auf **Weiter** und anschließend auf **Entfernen**.

1. <a name="ports"></a>Öffnen Sie die Firewallports.

   Öffnen Sie auf jedem virtuellen Computer die folgenden Ports in der Windows-Firewall.

   | Zweck | TCP-Port | Notizen
   | ------ | ------ | ------
   | SQL Server | 1433 | Dies ist der normale Port für Standardinstanzen von SQL Server. Falls Sie ein Image aus dem Katalog verwendet haben, ist dieser Port automatisch geöffnet.
   | Integritätstest | 59999 | Beliebiger geöffneter TCP-Port. In einem späteren Schritt konfigurieren Sie für den [Integritätstest](#probe) des Lastenausgleichs und den Cluster die Verwendung dieses Ports.   
   | Dateifreigabe | 445 | Der vom Dateifreigabedienst verwendete Port. 

1. [Fügen Sie die virtuellen Computer Ihrer bereits vorhandenen Domäne hinzu](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).

Nachdem die virtuellen Computer erstellt und konfiguriert wurden, können Sie die Premium-Dateifreigabe konfigurieren.

## <a name="step-2-mount-premium-file-share"></a>Schritt 2: Einbinden der Premium-Dateifreigabe

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu Ihrem Speicherkonto.
1. Navigieren Sie unter **Dateidienst** zu **Dateifreigaben**, und wählen Sie die Premium-Dateifreigabe aus, die Sie als SQL-Speicher verwenden möchten. 
1. Klicken Sie auf **Verbinden**, um die Verbindungszeichenfolge für Ihre Dateifreigabe anzuzeigen. 
1. Wählen Sie in der Dropdownliste den gewünschten Laufwerkbuchstaben aus, und kopieren Sie dann beide Codeblöcke in ein Editor-Dokument.

   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/premium-file-storage-commands.png" alt-text="Kopieren beider PowerShell-Befehle aus dem Portal zum Herstellen der Verbindung mit der Dateifreigabe":::

1. Stellen Sie eine RDP-Verbindung mit der SQL Server-VM her. Nutzen Sie dazu das Konto, dass Ihre SQL Server-FCI als Dienstkonto verwendet. 
1. Öffnen Sie eine administrative PowerShell-Befehlskonsole. 
1. Führen Sie die zuvor aus dem Portal kopierten und gespeicherten Befehle aus. 
1. Navigieren Sie über den Datei-Explorer oder das Dialogfeld **Ausführen** (WINDOWS-TASTE+R) unter Verwendung des Netzwerkpfads `\\storageaccountname.file.core.windows.net\filesharename` zur Freigabe. Beispiel: `\\sqlvmstorageaccount.file.core.windows.net\sqlpremiumfileshare`

1. Erstellen Sie in der neu verbundenen Dateifreigabe mindestens einen Ordner, in dem Sie Ihre SQL-Datendateien ablegen können. 
1. Wiederholen Sie diese Schritte auf jeder SQL Server-VM, die zum Cluster gehört. 

  > [!IMPORTANT]
  > Erwägen Sie den Einsatz einer gesonderten Dateifreigabe für Sicherungsdateien, um die IOPS- und Größenkapazität dieser Freigabe für Daten- und Protokolldateien zu reservieren. Sie können für Sicherungsdateien eine Standard- oder Premium-Dateifreigabe verwenden.

## <a name="step-3-configure-failover-cluster-with-file-share"></a>Schritt 3: Konfigurieren des Failoverclusters mit der Dateifreigabe 

Im nächsten Schritt konfigurieren Sie den Failovercluster. In diesem Schritt führen Sie die folgenden untergeordneten Schritte aus:

1. Hinzufügen des Windows-Failoverclusteringfeatures
1. Überprüfen des Clusters
1. Erstellen des Failoverclusters
1. Erstellen des Cloudzeugen


### <a name="add-windows-failover-clustering-feature"></a>Hinzufügen des Windows-Failoverclusteringfeatures

1. Stellen Sie per RDP zuerst eine Verbindung mit dem ersten virtuellen Computer her, indem Sie ein Domänenkonto verwenden, das Mitglied der Gruppe der lokalen Administratoren ist und über Berechtigungen zum Erstellen von Objekten in Active Directory verfügt. Verwenden Sie dieses Konto für den Rest der Konfiguration.

1. [Fügen Sie das Failoverclusteringfeature jedem virtuellen Computer hinzu](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).

   Führen Sie die folgenden Schritte auf beiden virtuellen Computern aus, um das Failoverclusteringfeature über die UI zu installieren.
   - Klicken Sie im **Server-Manager** auf **Verwalten** und dann auf **Rollen und Features hinzufügen**.
   - Klicken Sie im **Assistenten zum Hinzufügen von Rollen und Features** auf **Weiter**, bis Sie zu **Features auswählen** gelangen.
   - Klicken Sie unter **Features auswählen** auf **Failoverclustering**. Schließen Sie alle erforderlichen Features und Verwaltungstools ein. Klicken Sie auf **Features hinzufügen**.
   - Klicken Sie auf **Weiter** und dann auf **Fertig stellen**, um die Features zu installieren.

   Führen Sie zum Installieren des Failoverclusteringfeatures mit PowerShell das folgende Skript in einer PowerShell-Administratorsitzung auf einem der virtuellen Computer aus.

   ```powershell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

### <a name="validate-the-cluster"></a>Überprüfen des Clusters

Dieser Leitfaden basiert auf der Anleitung unter [Ausführen der Clustervalidierung](https://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).

Führen Sie die Validierung für den Cluster auf der Benutzeroberfläche oder mit PowerShell durch.

Führen Sie die folgenden Schritte auf einem der beiden virtuellen Computer aus, um den Cluster über die Benutzeroberfläche zu validieren.

1. Klicken Sie in **Server-Manager** auf **Tools** und dann auf **Failovercluster-Manager**.
1. Klicken Sie im **Failovercluster-Manager** auf **Aktion** und dann auf **Konfiguration überprüfen...** .
1. Klicken Sie auf **Weiter**.
1. Geben Sie unter **Server oder Cluster auswählen** den Namen der beiden virtuellen Computer ein.
1. Wählen Sie unter **Testoptionen** die Option **Nur ausgewählte Tests ausführen**. Klicken Sie auf **Weiter**.
1. Schließen Sie unter **Testauswahl** alle Tests mit Ausnahme von **Speicher** und **Direkte Speicherplätze** ein. Sehen Sie sich die folgende Abbildung an:

   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/cluster-validation.png" alt-text="Tests zur Überprüfung des Clusters":::

1. Klicken Sie auf **Weiter**.
1. Klicken Sie unter **Bestätigung** auf **Weiter**.

Mit dem **Konfigurationsüberprüfungs-Assistenten** werden die Validierungstests durchgeführt.

Führen Sie zum Validieren des Clusters mit PowerShell das folgende Skript in einer PowerShell-Administratorsitzung auf einem der virtuellen Computer aus.

   ```powershell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Inventory", "Network", "System Configuration"
   ```

Erstellen Sie nach dem Validieren des Clusters den Failovercluster.

### <a name="create-the-failover-cluster"></a>Erstellen des Failoverclusters


Für das Erstellen des Failoverclusters benötigen Sie Folgendes:
- Die Namen der virtuellen Computer, die zu Clusterknoten werden
- Einen Namen für den Failovercluster
- Eine IP-Adresse für den Failovercluster. Sie können eine IP-Adresse verwenden, die nicht in demselben virtuellen Azure-Netzwerk und Subnetz wie die Clusterknoten genutzt wird.

#### <a name="windows-server-2012-2016"></a>Windows Server 2012 bis 2016

Mit dem folgenden PowerShell-Befehl wird ein Failovercluster für **Windows Server 2012 bis 2016** erstellt. Aktualisieren Sie das Skript mit den Namen der Knoten (Namen virtueller Computer) und einer verfügbaren IP-Adresse aus dem Azure VNET:

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

#### <a name="windows-server-2019"></a>Windows Server 2019

Mit dem folgenden PowerShell-Befehl wird ein Failovercluster für Windows Server 2019 erstellt.  Weitere Informationen finden Sie im Blog [Failover Cluster: Cluster network Object](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97).  Aktualisieren Sie das Skript mit den Namen der Knoten (Namen virtueller Computer) und einer verfügbaren IP-Adresse aus dem Azure VNET:

```powershell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage -ManagementPointNetworkType Singleton 
```


### <a name="create-a-cloud-witness"></a>Erstellen eines Cloudzeugen

Ein Cloudzeuge ist eine neue Art von Clusterquorumzeuge, der in einem Azure Storage Blob gespeichert wird. Es ist dann nicht erforderlich, eine separate VM als Host für eine Zeugenfreigabe zu verwenden.

1. [Erstellen Sie einen Cloudzeugen für den Failovercluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).

1. Erstellen Sie einen Blobcontainer.

1. Speichern Sie die Zugriffsschlüssel und die Container-URL.

1. Konfigurieren Sie den Failovercluster-Quorumzeugen. Informationen hierzu finden Sie auf der Benutzeroberfläche unter [Cloudzeugen als Quorumzeugen konfigurieren](https://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness).


## <a name="step-4-test-cluster-failover"></a>Schritt 4: Testen des Failovers des Clusters

Testen Sie das Failover Ihres Clusters. Klicken Sie in Failovercluster-Manager mit der rechten Maustaste auf den Cluster > **Weitere Aktionen** > **Hauptclusterressource verschieben** > **Knoten auswählen**, und wählen Sie den anderen Knoten des Clusters aus. Verschieben Sie die Hauptclusterressource auf alle Knoten des Clusters und dann zurück auf den primären Knoten. Wenn Sie den Cluster erfolgreich auf jeden Knoten verschieben können, können Sie SQL Server installieren.  

:::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/test-cluster-failover.png" alt-text="Testen des Clusterfailovers durch Verschieben der Hauptressource auf die anderen Knoten":::

## <a name="step-5-create-sql-server-fci"></a>Schritt 5: Erstellen der SQL Server-FCI

Nachdem Sie den Failovercluster konfiguriert haben, können Sie die SQL Server-FCI erstellen.

1. Stellen Sie die Verbindung mit dem ersten virtuellen Computer per RDP her.

1. Stellen Sie im **Failovercluster-Manager** sicher, dass sich alle Kernressourcen des Clusters auf dem ersten virtuellen Computer befinden. Verschieben Sie alle Ressourcen auf diesen virtuellen Computer, falls dies erforderlich ist.

1. Suchen Sie nach den Installationsmedien. Wenn für den virtuellen Computer eines der Azure Marketplace-Images verwendet wird, befinden sich die Medien unter `C:\SQLServer_<version number>_Full`. Klicken Sie auf **Setup**.

1. Klicken Sie im **SQL Server-Installationscenter** auf **Installation**.

1. Klicken Sie auf **Neue SQL Server-Failoverclusterinstallation**. Befolgen Sie im Assistenten die Anleitung zum Installieren der SQL Server-FCI.

   Die FCI-Datenverzeichnisse müssen sich auf der Premium-Dateifreigabe befinden. Geben Sie den vollständigen Pfad der Freigabe im Format `\\storageaccountname.file.core.windows.net\filesharename\foldername` ein. Eine Warnung wird angezeigt, die Sie darüber informiert, dass Sie einen Dateiserver als Datenverzeichnis angegeben haben. Dies entspricht dem erwarteten Verhalten. Vergewissern Sie sich, dass das Konto, mit dem Sie die Dateifreigabe angelegt haben, das Konto ist, das der SQL Server-Dienst verwendet, um mögliche Ausfälle zu vermeiden. 

   :::image type="content" source="media/virtual-machines-windows-portal-sql-create-failover-cluster-premium-file-share/use-file-share-as-data-directories.png" alt-text="Verwenden der Dateifreigabe als SQL-Datenverzeichnisse":::

1. Nach Abschluss des Assistenten wird vom Setup eine SQL Server-FCI auf dem ersten Knoten installiert.

1. Nachdem das Setup die FCI erfolgreich auf dem ersten Knoten installiert hat, können Sie per RDP eine Verbindung mit dem zweiten Knoten herstellen.

1. Öffnen Sie das **SQL Server-Installationscenter**. Klicken Sie auf **Installation**.

1. Klicken Sie auf **Knoten einem SQL Server-Failovercluster hinzufügen**. Befolgen Sie die Anleitung im Assistenten, um SQL Server zu installieren und diese Serverinstanz der FCI hinzuzufügen.

   >[!NOTE]
   >Wenn Sie ein Azure Marketplace-Katalogimage mit SQL Server verwendet haben, sind die SQL Server-Tools im Image enthalten. Wenn Sie dieses Image nicht verwendet haben, müssen Sie die SQL Server-Tools separat installieren. Weitere Informationen finden Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="step-6-create-azure-load-balancer"></a>Schritt 6: Erstellen eines Azure Load Balancers

Auf virtuellen Azure-Computern wird für Cluster ein Lastenausgleich für eine IP-Adresse verwendet, die zu einem bestimmten Zeitpunkt auf einem Clusterknoten vorhanden sein muss. In dieser Lösung enthält der Lastenausgleich die IP-Adresse für die SQL Server-FCI.

Weitere Informationen finden Sie unter [Erstellen einer Azure Load Balancer-Instanz](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).

### <a name="create-the-load-balancer-in-the-azure-portal"></a>Erstellen des Lastenausgleichs im Azure-Portal

So erstellen Sie den Lastenausgleich

1. Navigieren Sie im Azure-Portal zu der Ressourcengruppe mit den virtuellen Computern.

1. Klicken Sie auf **+ Hinzufügen**. Durchsuchen Sie den Marketplace nach **Load Balancer**. Klicken Sie auf **Load Balancer**.

1. Klicken Sie auf **Create**.

1. Konfigurieren Sie Folgendes für den Load Balancer (Lastenausgleich):

   - **Abonnement**: Ihr Azure-Abonnement.
   - **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihre virtuellen Computer.
   - **Name**: Ein Name, mit dem der Lastenausgleich identifiziert wird.
   - **Region**: Verwenden Sie denselben Azure-Standort wie für Ihre virtuellen Computer.
   - **Typ**: Der Lastenausgleich kann entweder öffentlich oder privat sein. Auf einen privaten Lastenausgleich kann aus demselben VNET zugegriffen werden. Für die meisten Azure-Anwendungen kann ein privater Lastenausgleich verwendet werden. Verwenden Sie einen öffentlichen Lastenausgleich, wenn Ihre Anwendung direkten Zugriff auf SQL Server über das Internet benötigt.
   - **SKU**: Für den Lastenausgleich muss die Standard-SKU verwendet werden. 
   - **Virtuelles Netzwerk**: Dies ist dasselbe Netzwerk wie für die virtuellen Computer.
   - **IP-Adresszuweisung**: Die IP-Adresszuweisung muss statisch sein. 
   - **Private IP-Adresse**: Dies ist die gleiche IP-Adresse, die Sie der Clusternetzwerkressource der SQL Server-FCI zugewiesen haben.
   Sehen Sie sich die folgende Abbildung an:

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)
   

### <a name="configure-the-load-balancer-backend-pool"></a>Konfigurieren des Back-End-Pools für den Lastenausgleich

1. Wechseln Sie zurück zur Azure-Ressourcengruppe mit den virtuellen Computern, und suchen Sie nach dem neuen Lastenausgleich. Es kann sein, dass Sie hierfür die Ansicht mit der Ressourcengruppe aktualisieren müssen. Klicken Sie auf den Lastenausgleich.

1. Klicken Sie auf **Back-End-Pools** und dann auf **+ Hinzufügen**, um einen Back-End-Pool hinzuzufügen.

1. Ordnen Sie den Back-End-Pool mit der Verfügbarkeitsgruppe zu, die die VMs enthält.

1. Aktivieren Sie unter **Zielnetzwerk-IP-Konfigurationen** die Option **VIRTUELLER COMPUTER**, und wählen Sie die virtuellen Computer aus, die als Clusterknoten eingeschlossen werden. Schließen Sie dabei alle virtuellen Computer ein, die die FCI hosten. 

1. Klicken Sie auf **OK**, um den Back-End-Pool zu erstellen.

### <a name="configure-a-load-balancer-health-probe"></a>Konfigurieren eines Integritätstests für den Lastenausgleich

1. Klicken Sie auf dem Blatt für den Lastenausgleich auf **Integritätstests**.

1. Klicken Sie auf **+ Hinzufügen**.

1. Legen Sie auf dem Blatt **Add health probe**<a name="probe"></a> (Integritätstest hinzufügen) die Parameter für den Integritätstest fest:

   - **Name**: Ein Name für den Integritätstest.
   - **Protokoll:** TCP.
   - **Port:** Legen Sie den Port fest, den Sie [in diesem Schritt](#ports) in der Firewall für den Integritätstest erstellt haben. Im Beispiel dieses Artikels wird der TCP-Port `59999` verwendet.
   - **Intervall**: 5 Sekunden.
   - **Fehlerschwellenwert**: Zwei aufeinanderfolgende Fehler.

1. Klicken Sie auf OK.

### <a name="set-load-balancing-rules"></a>Festlegen von Lastenausgleichsregeln

1. Klicken Sie auf dem Blatt „Lastenausgleich“ auf **Lastenausgleichsregeln**.

1. Klicken Sie auf **+ Hinzufügen**.

1. Legen Sie die Parameter für die Lastenausgleichsregeln fest:

   - **Name**: Ein Name für die Lastenausgleichsregeln.
   - **Front-End-IP-Adresse**: Verwenden Sie die IP-Adresse für die Clusternetzwerkressource der SQL Server-FCI.
   - **Port:** Legen Sie den TCP-Port für die SQL Server-FCI fest. Der Standardport der Instanz lautet 1433.
   - **Back-End-Port**: Für diesen Wert wird der gleiche Port verwendet, den Sie als Wert für **Port** angeben, wenn Sie **Floating IP (Direct Server Return)** aktivieren.
   - **Back-End-Pool**: Verwenden Sie den Namen des Back-End-Pools, den Sie zuvor konfiguriert haben.
   - **Integritätstest**: Verwenden Sie den Integritätstest, den Sie zuvor konfiguriert haben.
   - **Sitzungspersistenz**: None (Keine):
   - **Leerlaufzeitüberschreitung (Minuten)** : 4.
   - **Floating IP (Direct Server Return)** : Enabled

1. Klicken Sie auf **OK**.

## <a name="step-7-configure-cluster-for-probe"></a>Schritt 7: Konfigurieren des Clusters für den Test

Legen Sie den Parameter für den Clustertestport in PowerShell fest.

Aktualisieren Sie die Variablen im folgenden Skript mit Werten aus Ihrer Umgebung, um den Parameter für den Clustertestport festzulegen. Entfernen Sie die spitzen Klammern `<>` aus dem Skript. 

   ```powershell
   $ClusterNetworkName = "<Cluster Network Name>"
   $IPResourceName = "<SQL Server FCI IP Address Resource Name>" 
   $ILBIP = "<n.n.n.n>" 
   [int]$ProbePort = <nnnnn>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

Legen Sie die Werte für Ihre Umgebung im vorherigen Skript fest. Die Werte werden in der folgenden Liste beschrieben:

   - `<Cluster Network Name>`: Name des Windows Server-Failoverclusters für das Netzwerk. Klicken Sie in **Failovercluster-Manager** > **Netzwerke** mit der rechten Maustaste auf das Netzwerk, und klicken Sie dann auf **Eigenschaften**. Der richtige Wert befindet sich auf der Registerkarte **Allgemein** unter **Name**. 

   - `<SQL Server FCI IP Address Resource Name>`: Name der IP-Adressressource der SQL Server-FCI. Klicken Sie in **Failovercluster-Manager** > **Rollen** unter der Rolle „SQL Server-FCI“ unter **Servername** mit der rechten Maustaste auf die IP-Adressressource, und klicken Sie dann auf **Eigenschaften**. Der richtige Wert befindet sich auf der Registerkarte **Allgemein** unter **Name**. 

   - `<ILBIP>`: Die ILB-IP-Adresse. Diese Adresse wird als ILB-Front-End-Adresse im Azure-Portal konfiguriert. Dies ist auch die FCI-IP-Adresse von SQL Server. Sie finden sie im **Failovercluster-Manager** auf der gleichen Eigenschaftenseite, auf der sich der `<SQL Server FCI IP Address Resource Name>` befindet.  

   - `<nnnnn>`: Der Testport, den Sie im Integritätstest des Lastenausgleichs konfiguriert haben. Alle nicht verwendeten TCP-Ports sind zulässig. 

>[!IMPORTANT]
>Die Subnetzmaske für den Clusterparameter muss die TCP/IP-Broadcastadresse sein: `255.255.255.255`.

Nach dem Festlegen des Clustertests werden in PowerShell alle Clusterparameter angezeigt. Führen Sie das folgende Skript aus:

   ```powershell
   Get-ClusterResource $IPResourceName | Get-ClusterParameter 
  ```

## <a name="step-8-test-fci-failover"></a>Schritt 8: Testen des FCI-Failovers

Testen Sie das Failover der FCI, um die Clusterfunktionalität zu validieren. Führen Sie folgende Schritte aus:

1. Stellen Sie per RDP eine Verbindung mit einem der SQL Server-FCI-Clusterknoten her.

1. Öffnen Sie den **Failovercluster-Manager**. Klicken Sie auf **Rollen**. Achten Sie darauf, welcher Knoten im Besitz der SQL Server-FCI-Rolle ist.

1. Klicken Sie mit der rechten Maustaste auf die SQL Server-FCI-Rolle.

1. Klicken Sie auf **Verschieben** und dann auf **Bestmöglicher Knoten**.

Unter **Failovercluster-Manager** wird die Rolle angezeigt, und die Ressourcen werden in den Offlinezustand versetzt. Die Ressourcen werden dann verschoben und auf dem anderen Knoten dann wieder in den Onlinezustand versetzt.

### <a name="test-connectivity"></a>Testen der Konnektivität

Melden Sie sich zum Testen der Konnektivität an einem anderen virtuellen Computer in demselben virtuellen Netzwerk an. Öffnen Sie **SQL Server Management Studio**, und stellen Sie eine Verbindung mit dem SQL Server-FCI-Namen her.

>[!NOTE]
>Bei Bedarf können Sie [SQL Server Management Studio herunterladen](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="limitations"></a>Einschränkungen

Virtuelle Azure-Computer unterstützen Microsoft Distributed Transaction Coordinator (MSDTC) auf Windows Server 2019 mit Speicher auf freigegebenen Clustervolumes (CSV) und ein [Load Balancer im Tarif „Standard“](../../../load-balancer/load-balancer-standard-overview.md).

Auf virtuellen Azure-Computern wird MSDTC unter Windows Server 2016 und früheren Versionen aus folgenden Gründen nicht unterstützt:

- Die gruppierte MSDTC-Ressource kann nicht für die Verwendung von freigegebenem Speicher konfiguriert werden. Wenn Sie mit Windows Server 2016 eine MSDTC-Ressource erstellen, wird kein freigegebener Speicher für die Verwendung angezeigt, selbst wenn der Speicher vorhanden ist. Dieses Problem wurde in Windows Server 2019 behoben.
- Der Load Balancer im Tarif „Basic“ verarbeitet keine RPC-Ports.

## <a name="see-also"></a>Siehe auch

- [Windows-Clustertechnologie](/windows-server/failover-clustering/failover-clustering-overview)
- [SQL Server-Failoverclusterinstanzen](/sql/sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server).
