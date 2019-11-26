---
title: Speicherkonfiguration für SQL Server-VMs | Microsoft Docs
description: In diesem Thema wird beschrieben, wie Azure den Speicher für SQL Server-VMs während der Bereitstellung konfiguriert (Resource Manager-Bereitstellungsmodell). Außerdem wird erläutert, wie Sie den Speicher für Ihre vorhandenen SQL Server-VMs konfigurieren können.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.assetid: 169fc765-3269-48fa-83f1-9fe3e4e40947
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/05/2017
ms.author: mathoma
ms.openlocfilehash: a91098d06f481afaae75eb497d5a076c3eb42c07
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2019
ms.locfileid: "72896950"
---
# <a name="storage-configuration-for-sql-server-vms"></a>Speicherkonfiguration für SQL Server-VMs

Wenn Sie in Azure einen virtuellen SQL Server-Computer konfigurieren, unterstützt Sie das Portal beim Automatisieren Ihrer Speicherkonfiguration. Hierzu gehören auch das Anfügen von Speicher an die VM, das Verfügbarmachen dieses Speichers für SQL Server und die anschließende Konfiguration, um eine Optimierung in Bezug auf Ihre besonderen Leistungsanforderungen zu erzielen.

In diesem Thema wird erläutert, wie der Speicher unter Azure für Ihre SQL Server-VMs konfiguriert wird – sowohl während der Bereitstellung als auch für vorhandene VMs. Diese Konfiguration basiert auf den [bewährten Methoden für die Leistung](virtual-machines-windows-sql-performance.md) für Azure-VMs, auf denen SQL Server ausgeführt wird.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung der Einstellungen für die automatische Speicherkonfiguration muss Ihr virtueller Computer über die folgenden Merkmale verfügen:

* Bereitstellung mit einem [SQL Server-Katalogimage](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo)
* Verwendung des [Resource Manager-Bereitstellungsmodells](../../../azure-resource-manager/resource-manager-deployment-model.md)
* Verwendung von [SSD Premium](../disks-types.md)

## <a name="new-vms"></a>Neue virtuelle Computer

In den folgenden Abschnitten wird beschrieben, wie Speicher für neue virtuelle SQL Server-Computer konfiguriert wird.

### <a name="azure-portal"></a>Azure-Portal

Wenn Sie eine Azure-VM mithilfe eines SQL Server-Katalogimages bereitstellen, wählen Sie **Konfiguration ändern** auf der Registerkarte **SQL Server-Einstellungen** aus, um die Seite „Leistungsoptimierte Speicherkonfiguration“ zu öffnen. Sie können die Standardeinstellungen der Werte übernehmen oder den Typ der Datenträgerkonfiguration ändern, der den Anforderungen für Ihre Workload am besten entspricht. 

![SQL Server-VM-Speicherkonfiguration während der Bereitstellung](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Wählen Sie unter **Speicheroptimierung** den Typ der Workload aus, für den Sie SQL Server bereitstellen. Mit der Optimierungsoption **Allgemein** verfügen Sie standardmäßig über einen Datenträger mit maximal 5.000 IOPS, und Sie verwenden dasselbe Laufwerk für Ihre Daten, das Transaktionsprotokoll und den TempDB-Speicher. Wenn Sie entweder **Transaktionale Verarbeitung** (OLTP) oder **Data Warehousing** auswählen, wird ein separater Datenträger für Daten, ein separater Datenträger für das Transaktionsprotokoll und eine lokale SSD für TempDB erstellt. Es gibt keine Speicherunterschiede zwischen **Transaktionale Verarbeitung** und **Datenlagerung**, aber die jeweilige Option ändert Ihre [Stripesetkonfiguration und Ablaufverfolgungsflags](#workload-optimization-settings). Wenn Sie Storage Premium auswählen, wird die Zwischenspeicherung auf *Schreibgeschützt* für das Datenlaufwerk und *Keine* für das Protokolllaufwerk festgelegt, wie unter [Bewährte Methoden für die SQL Server-VM-Leistung](virtual-machines-windows-sql-performance.md) beschrieben. 

![SQL Server-VM-Speicherkonfiguration während der Bereitstellung](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration.png)

Die Datenträgerkonfiguration ist vollständig anpassbar, sodass Sie die Speichertopologie, den Datenträgertyp und den IOPs konfigurieren können, die Sie für Ihre SQL Server-VM-Workload benötigen. Sie haben auch die Möglichkeit, UltraSSD (Vorschau) als Option für den **Datenträgertyp** zu verwenden, wenn sich Ihre SQL Server-VM in einer der unterstützten Regionen (USA, Osten 2, Asien, Südosten und Europa, Norden) befindet und Sie [Ultra-Datenträger für Ihr Abonnement](/azure/virtual-machines/windows/disks-enable-ultra-ssd) aktiviert haben.  

Zusätzlich haben Sie die Möglichkeit, die Zwischenspeicherung für die Datenträger festzulegen. Azure VMs verfügen über eine mehrschichtige Zwischenspeicherungstechnologie namens [Blobcache](/azure/virtual-machines/windows/premium-storage-performance#disk-caching), wenn sie mit [Premium-Datenträgern](/azure/virtual-machines/windows/disks-types#premium-ssd) verwendet werden. Blobcache verwendet für das Zwischenspeichern eine Kombination aus RAM des virtuellen Computers und lokalem SSD-Laufwerk. 

Die Datenträgerzwischenspeicherung für SSD Premium kann die Werte *ReadOnly*, *ReadWrite* oder *None* aufweisen. 

- Zwischenspeicherung vom Typ *ReadOnly* ist sehr vorteilhaft für SQL Server-Datendateien, die in Storage Premium gespeichert sind. Zwischenspeicherung vom Typ *ReadOnly* bringt niedrige Leselatenz, hohe Lese-IOPS und Durchsatz mit sich, da die Lesezugriffe aus dem Cache erfolgen, der sich im VM-Arbeitsspeicher und lokalen SSD befindet. Diese Lesezugriffe sind viel schneller als Lesezugriffe vom Datenträger, der aus dem Azure-Blobspeicher stammt. Storage Premium rechnet die aus dem Cache erfüllten Leseanforderungen nicht zur IOPS- und Durchsatzrate des Datenträgers. Aus diesem Grund kann Ihre Anwendung eine höhere Gesamtrate bei IOPS und Durchsatz erzielen. 
- Die Cachekonfiguration *None* sollte für die Datenträger verwendet werden, auf denen sich die SQL Server-Protokolldatei befindet, da die Protokolldatei sequenziell geschrieben wird und nicht von *ReadOnly*-Zwischenspeicherung profitiert. 
- Zwischenspeicherung vom Typ *ReadWrite* sollte nicht zum Hosten von SQL Server-Dateien verwendet werden, da SQL Server keine Datenkonsistenz mit dem *ReadWrite*-Cache unterstützt. Schreibvorgänge vergeuden die Kapazität des *ReadOnly*-Blobcaches, und die Latenzzeiten nehmen geringfügig zu, wenn die Schreibvorgänge durch *ReadOnly*-Blobcacheebenen erfolgen. 


   > [!TIP]
   > Stellen Sie sicher, dass Ihre Speicherkonfiguration mit den Einschränkungen der ausgewählten VM-Größe übereinstimmt. Die Auswahl von Speicherparametern, die die Leistungsgrenze der VM-Größe überschreiten, führt zu einem Fehler: `The desired performance might not be reached due to the maximum virtual machine disk performance cap.`. Verringern Sie den IOPS durch Ändern des Datenträgertyps, oder erhöhen Sie die Leistungsbegrenzung durch Vergrößern der VM-Größe. 


Je nach Ihrer Auswahl führt Azure nach dem Erstellen der VM die folgenden Aufgaben der Speicherkonfiguration durch:

* Erstellt SSD Premium-Datenträger und fügt sie an den virtuellen Computer an.
* Konfiguriert die Datenträger so, dass sie für SQL Server zugänglich sind.
* Konfiguriert die Datenträger in einem Speicherpool basierend auf der angegebenen Größe und den Leistungsanforderungen (IOPS und Durchsatz).
* Ordnet dem Speicherpool ein neues Laufwerk auf dem virtuellen Computer zu.
* Optimiert dieses neue Laufwerk basierend auf Ihrem angegebenen Workloadtyp („Data Warehousing“, „Transaktionsverarbeitung“ oder „Allgemein“).

Weitere Details dazu, wie unter Azure Speichereinstellungen konfiguriert werden, finden Sie im [Abschnitt zur Speicherkonfiguration](#storage-configuration). Eine vollständige exemplarische Vorgehensweise zur Erstellung einer SQL Server-VM im Azure-Portal finden Sie unter [Tutorial zur Bereitstellung](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Resource Manager-Vorlagen

Wenn Sie die folgenden Resource Manager-Vorlagen verwenden, werden standardmäßig zwei Premium-Datenträger ohne Speicherpoolkonfiguration angefügt. Sie können diese Vorlagen aber anpassen, um die Anzahl von Premium-Datenträgern zu ändern, die an den virtuellen Computer angefügt sind.

* [Create VM with Automated Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Create VM with Automated Patching](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [Create VM with AKV Integration](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault) (Erstellen eines virtuellen Computers mit AKV-Integration)

### <a name="quickstart-template"></a>Schnellstartvorlage

Sie können die folgende Schnellstartvorlage verwenden, um eine SQL Server-VM mithilfe von Speicheroptimierung bereitzustellen. 

* [Erstellen einer VM mit Speicheroptimierung](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-new-storage/)
* [Erstellen einer VM mit UltraSSD](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-new-storage-ultrassd)

## <a name="existing-vms"></a>Vorhandene virtuelle Computer

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Für vorhandene SQL Server-VMs können Sie im Azure-Portal einige Speichereinstellungen ändern. Öffnen Sie Ihre [SQL-VM-Ressource](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource), und wählen Sie **Übersicht** aus. Auf der Seite „SQL Server-Übersicht“ wird die aktuelle Speichernutzung Ihrer VM angezeigt. Alle Laufwerke, die auf Ihrer VM vorhanden sind, werden in diesem Diagramm angezeigt. Für jedes Laufwerk wird der Speicherplatz in vier Abschnitten angezeigt:

* SQL-Daten
* SQL-Protokoll
* Andere (anderer Speicher als SQL-Speicher)
* Verfügbar

Wählen Sie zum Ändern der Speichereinstellungen **Konfigurieren** unter **Einstellungen** aus. 

![Konfigurieren von Speicher für vorhandene SQL Server-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Sie können die Datenträgereinstellungen für die Laufwerke ändern, die während des Erstellungsprozesses der SQL Server-VM konfiguriert wurden. Durch die Auswahl von **Laufwerk erweitern** wird die Seite zur Änderung des Laufwerks geöffnet, auf der Sie den Datenträgertyp ändern und zusätzliche Datenträger hinzufügen können. 

![Konfigurieren von Speicher für vorhandene SQL Server-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-drive.png)



## <a name="storage-configuration"></a>Speicherkonfiguration

Dieser Abschnitt dient als Referenz für die Änderungen der Speicherkonfiguration, die von Azure automatisch vorgenommen werden, während die SQL-VM-Bereitstellung oder -konfiguration im Azure-Portal durchgeführt wird.

* Azure konfiguriert einen Speicherpool aus dem ausgewählten Speicher Ihrer VM. Im nächsten Abschnitt dieses Themas sind Details zur Speicherpoolkonfiguration angegeben.
* Bei der automatischen Speicherkonfiguration werden immer P30-Datenträger vom Typ [SSD Premium](../disks-types.md) verwendet. Daher besteht eine 1:1-Zuordnung zwischen der ausgewählten TB-Menge und der Anzahl von Datenträgern, die an die VM angefügt sind.

Preisinformationen finden Sie auf der Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage) auf der Registerkarte **Disk Storage** .

### <a name="creation-of-the-storage-pool"></a>Erstellung des Speicherpools

In Azure werden die folgenden Einstellungen verwendet, um den Speicherpool auf SQL Server-VMs zu erstellen:

| Einstellung | Wert |
| --- | --- |
| Stripegröße |256 KB (Data Warehousing), 64 KB (Transaktional) |
| Datenträgergrößen |1 TB jeder |
| Cache |Lesen |
| Zuordnungsgröße |Größe der NTFS-Zuordnungseinheit: 64 KB |
| Wiederherstellen | Einfache Wiederherstellung (keine Resilienz) |
| Anzahl von Spalten |Anzahl von Datenträgern für Daten bis zu 8<sup>1</sup> |


<sup>1</sup> Nach der Erstellung des Speicherpools können Sie die Anzahl von Spalten im Speicherpool nicht mehr ändern.


## <a name="workload-optimization-settings"></a>Einstellungen für die Workloadoptimierung

In der folgenden Tabelle sind die drei verfügbaren Optionen für den Workloadtyp und die entsprechenden Optimierungen beschrieben:

| Workloadtyp | BESCHREIBUNG | Optimierungen |
| --- | --- | --- |
| **Allgemein** |Standardeinstellung, die die meisten Workloads unterstützt |Keine |
| **Transaktionale Verarbeitung** |Optimiert den Speicher für herkömmliche OLTP-Datenbankworkloads |Ablaufverfolgungsflag 1117<br/>Ablaufverfolgungsflag 1118 |
| **Data Warehousing** |Optimiert den Speicher für Analyse- und Berichterstellungsworkloads |Ablaufverfolgungsflag 610<br/>Ablaufverfolgungsflag 1117 |

> [!NOTE]
> Beim Bereitstellen eines virtuellen SQL-Computers können Sie den Workloadtyp nur angeben, indem Sie ihn im Schritt für die Speicherkonfiguration auswählen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Ausführen von SQL Server auf virtuellen Azure-Computern finden Sie unter [SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).
