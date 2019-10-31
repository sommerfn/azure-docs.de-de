---
title: 'Azure VMware-Lösung: Migration mit Azure Data Box'
description: Es wird beschrieben, wie Sie Azure Data Box zum Durchführen einer Massenmigration von Daten zur Azure VMware-Lösung verwenden.
author: sharaths-cs
ms.author: dikamath
ms.date: 09/27/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 65167169248d83ebfec2c49c308673ec9315934e
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72552914"
---
# <a name="migrating-data-to-azure-vmware-solution-by-using-azure-data-box"></a>Migrieren von Daten zur Azure VMware-Lösung mit Azure Data Box

Mit der Cloudlösung Microsoft Azure Data Box können Sie Daten in Terabyte-Größe (TBs) schnell, kostengünstig und zuverlässig nach Azure übertragen. Die sichere Datenübertragung wird beschleunigt, indem Sie ein spezielles Data Box-Speichergerät erhalten. Jedes Speichergerät verfügt über eine maximal nutzbare Speicherkapazität von 80 TB und wird von einem regionalen Zustelldienst zu Ihrem Rechenzentrum transportiert. Das Gerät verfügt über ein widerstandsfähiges Gehäuse zum Schützen und Absichern Ihrer Daten während des Transports.

Mit Data Box können Sie für Ihre VMware-Daten eine Massenmigration zu Ihrer privaten Cloud durchführen. Daten aus Ihrer lokalen VMware vSphere-Umgebung werden per NFS-Protokoll (Network File System) nach Data Box kopiert. Die Massenmigration von Daten umfasst das Speichern einer Kopie der virtuellen Computer zu einem bestimmten Zeitpunkt, der Konfiguration und der zugehörigen Daten in Data Box und den anschließenden manuellen Versand an Azure.

In diesem Artikel lernen Sie Folgendes:

* Einrichten von Data Box
* Kopieren von Daten aus der lokalen VMware-Umgebung nach Data Box per NFS
* Vorbereiten der Data Box-Rücksendung
* Vorbereiten der Blobdaten für das Kopieren in die Azure VMware-Lösung
* Kopieren der Daten aus Azure in Ihre private Cloud

## <a name="scenarios"></a>Szenarien

Verwenden Sie Data Box in den folgenden Szenarien für die Massenmigration von Daten:

* Migrieren einer großen Datenmenge aus der lokalen Umgebung zur Azure VMware-Lösung. Mit dieser Methode wird eine Baseline festgelegt, und die Unterschiede werden über das Netzwerk synchronisiert.
* Migrieren einer großen Zahl von virtuellen Computern, die ausgeschaltet sind (kalte virtuelle Computer).
* Migrieren der Daten virtueller Computer zum Einrichten von Entwicklungs- und Testumgebungen.
* Migrieren einer großen Zahl von Vorlagen für virtuelle Maschinen, ISO-Dateien und Datenträgern virtueller Computer.

## <a name="before-you-begin"></a>Voraussetzungen

* Überprüfen Sie die Voraussetzungen, und [bestellen Sie Data Box](../databox/data-box-deploy-ordered.md) über Ihr Azure-Portal. Während des Bestellvorgangs müssen Sie ein Speicherkonto auswählen, das Blobspeicher zulässt. Nachdem Sie das Data Box-Gerät erhalten haben, können Sie es mit Ihrem lokalen Netzwerk verbinden und dafür eine IP-Adresse [einrichten](../databox/data-box-deploy-set-up.md), die von Ihrem vSphere-Verwaltungsnetzwerk aus erreichbar ist.

* Erstellen Sie ein virtuelles Netzwerk und ein Speicherkonto in derselben Region, in der die Azure VMware-Lösung bereitgestellt wird.

* Erstellen Sie eine [Verbindung des virtuellen Azure-Netzwerks](cloudsimple-azure-network-connection.md) aus Ihrer privaten Cloud mit dem virtuellen Netzwerk, in dem das Speicherkonto erstellt wird. Führen Sie hierzu die Schritte aus, die unter [Herstellen einer Verbindung des virtuellen Azure-Netzwerks mit CloudSimple mit ExpressRoute](virtual-network-connection.md) beschrieben sind.

## <a name="set-up-data-box-for-nfs"></a>Einrichten von Data Box für NFS

Stellen Sie eine Verbindung mit der lokalen Webbenutzeroberfläche von Data Box her, indem Sie die Schritte ausführen, die im Abschnitt „Herstellen der Verbindung mit dem Gerät“ unter [Tutorial: Verkabeln und Herstellen einer Verbindung mit Azure Data Box](../databox/data-box-deploy-set-up.md) beschrieben werden.  Konfigurieren Sie Data Box so, dass der Zugriff auf NFS-Clients zulässig ist:

1. Wechseln Sie auf der lokalen Webbenutzeroberfläche zur Seite **Verbindung herstellen und Daten kopieren**. Wählen Sie unter **NFS-Einstellungen** die Option **NFS-Clientzugriff** aus. 

    ![Konfigurieren des NFS-Clientzugriffs 1](media/nfs-client-access.png)

2. Geben Sie die IP-Adresse des VMware-ESXi-Hosts ein, und wählen Sie die Option **Hinzufügen** aus. Sie können den Zugriff für alle Hosts in Ihrem vSphere-Cluster konfigurieren, indem Sie diesen Schritt wiederholen. Klicken Sie auf **OK**.

    ![Konfigurieren des NFS-Clientzugriffs 2](media/nfs-client-access2.png)
> [!IMPORTANT]
> **Erstellen Sie immer einen Ordner für die Dateien, die Sie unter die Freigabe kopieren möchten, und kopieren Sie die Dateien dann in diesen Ordner**. Der Ordner, der unter der Blockblob- und der Seitenblob Freigabe erstellt wurde, entspricht einem Container, in den Daten als Blobs hochgeladen werden. Es ist nicht möglich, Dateien direkt in den *root*-Ordner im Speicherkonto zu kopieren.

Unter Blockblob- und Seitenblobfreigaben sind Entitäten der ersten Ebene Container, und Entitäten der zweiten Ebene sind Blobs. Unter Freigaben für Azure Files handelt es sich bei Entitäten erster Ebene um Freigaben und bei Entitäten zweiter Ebene um Dateien.

In der folgenden Tabelle sind der UNC-Pfad zu den Freigaben auf Ihrer Data Box und die Azure Storage-Pfad-URL aufgeführt, wohin die Daten hochgeladen werden. Die endgültige URL des Azure Storage-Pfads kann aus dem UNC-Freigabepfad abgeleitet werden.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Azure-Blockblobs | <li>UNC-Pfad zu den Freigaben: `//<DeviceIPAddress>/<StorageAccountName_BlockBlob>/<ContainerName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure-Seitenblobs  | <li>UNC-Pfad zu den Freigaben: `//<DeviceIPAddres>/<StorageAccountName_PageBlob>/<ContainerName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Files       |<li>UNC-Pfad zu den Freigaben: `//<DeviceIPAddres>/<StorageAccountName_AzFile>/<ShareName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |

> [!NOTE]
> Verwenden Sie Azure-Blockblobs zum Kopieren von VMware-Daten.

## <a name="mount-the-nfs-share-as-a-datastore-on-your-on-premises-vcenter-cluster-and-copy-the-data"></a>Einbinden der NFS-Freigabe als Datenspeicher für Ihren lokalen vCenter-Cluster und Kopieren der Daten

Die NFS-Freigabe von Ihrer Data Box-Einheit muss als Datenspeicher für Ihren lokalen vCenter-Cluster oder VMware-ESXi-Host eingebunden werden, um die Daten in den NFS-Datenspeicher zu kopieren:

1. Melden Sie sich bei Ihrem lokalen vCenter-Server an.

2. Klicken Sie mit der rechten Maustaste auf **Datacenter**, und wählen Sie **Speicher**, **Neuer Datenspeicher** und dann **Weiter** aus.

   ![Hinzufügen eines neuen Datenspeichers](media/databox-migration-add-datastore.png)

3. Wählen Sie in Schritt 1 des Assistenten zum Hinzufügen von Datenspeicher unter **Typ** die Option **NFS** aus.

   ![Hinzufügen eines neuen Datenspeichers: Typ](media/databox-migration-add-datastore-type.png)

4. Wählen Sie in Schritt 2 des Assistenten die Option **NFS 3** als NFS-Version und dann **Weiter** aus.

   ![Hinzufügen eines neuen Datenspeichers: NFS-Version](media/databox-migration-add-datastore-nfs-version.png)

5. Geben Sie in Schritt 3 des Assistenten den Namen für den Datenspeicher, den Pfad und den Server an. Sie können die IP-Adresse Ihrer Data Box-Einheit für den Server verwenden. Der Ordnerpfad weist das Format `/<StorageAccountName_BlockBlob>/<ContainerName>/` auf.

   ![Hinzufügen eines neuen Datenspeichers: NFS-Konfiguration](media/databox-migration-add-datastore-nfs-configuration.png)

6. Wählen Sie in Schritt 4 des Assistenten die ESXi-Hosts aus, auf denen der Datenspeicher bereitgestellt werden soll, und wählen Sie anschließend **Weiter** aus.  Wählen Sie in einem Cluster alle Hosts aus, um die Migration virtueller Computer sicherzustellen.

   ![Hinzufügen eines neuen Datenspeichers: Auswählen von Hosts](media/databox-migration-add-datastore-nfs-select-hosts.png)

7. Sehen Sie sich in Schritt 5 des Assistenten die Zusammenfassung an, und wählen Sie die Option **Fertig stellen** aus.

## <a name="copy-data-to-the-data-box-nfs-datastore"></a>Kopieren von Daten in den Data Box-NFS-Datenspeicher

Virtuelle Computer können zum neuen Datenspeicher migriert oder in ihn geklont werden.  Alle nicht verwendeten virtuellen Computer, die Sie migrieren möchten, können mit der Option **Storage vMotion** zum Data Box-NFS-Datenspeicher migriert werden. Aktive virtuelle Computer können in den Data Box-NFS-Datenspeicher geklont werden.

* Ermitteln Sie die virtuellen Computer, die **verschoben** werden können, und listen Sie sie auf.
* Ermitteln Sie die virtuellen Computer, die **geklont** werden müssen, und listen Sie sie auf.

### <a name="move-a-virtual-machine-to-a-data-box-datastore"></a>Verschieben eines virtuellen Computers in einen Data Box-Datenspeicher

1. Klicken Sie mit der rechten Maustaste auf den virtuellen Computer, den Sie in den Data Box-Datenspeicher verschieben möchten, und wählen Sie anschließend **Migrieren** aus.

    ![Migrieren eines virtuellen Computers](media/databox-migration-vm-migrate.png)

2. Wählen Sie als Migrationstyp die Option **Nur Speicher ändern** und dann **Weiter** aus.

    ![Migrieren eines virtuellen Computers: Nur Speicher](media/databox-migration-vm-migrate-change-storage.png)

3. Wählen Sie als Ziel die Option **Databox-Datastore** (Data Box-Datenspeicher) und dann **Weiter** aus.

    ![Migrieren eines virtuellen Computers: Auswählen des Datenspeichers](media/databox-migration-vm-migrate-change-storage-select-datastore.png)

4. Überprüfen Sie die Informationen, und wählen Sie **Fertig stellen** aus.

5. Wiederholen Sie die Schritte 1 bis 4 für weitere virtuelle Computer.

> [!TIP]
> Sie können mehrere virtuelle Computer auswählen, die den gleichen Zustand aufweisen (Ein- oder Ausgeschaltet), und diese zusammen migrieren (Massenmigration).

Der virtuelle Computer wird aus Data Box zum NFS-Datenspeicher migriert. Nachdem alle virtuellen Computer migriert wurden, können Sie die aktiven virtuellen Computer als Vorbereitung für die Migration von Daten zur Azure VMware-Lösung ausschalten (herunterfahren).

### <a name="clone-a-virtual-machine-or-a-virtual-machine-template-to-the-data-box-datastore"></a>Klonen eines virtuellen Computers oder einer Vorlage für virtuelle Computer in den Data Box-Datenspeicher

1. Klicken Sie mit der rechten Maustaste auf einen virtuellen Computer oder eine Vorlage für virtuelle Maschinen, den bzw. die Sie klonen möchten. Wählen Sie **Klonen** > **Clone to Virtual Machine** (In virtuellen Computer klonen) aus.

    ![Klon eines virtuellen Computers](media/databox-migration-vm-clone.png)

2. Wählen Sie einen Namen für den geklonten virtuellen Computer oder die Vorlage für virtuelle Maschinen aus.

3. Wählen Sie den Ordner, in dem Sie das geklonte Objekt anordnen möchten, und anschließend **Weiter** aus.

4. Wählen Sie den Cluster oder Ressourcenpool, in dem Sie das geklonte Objekt anordnen möchten, und anschließend **Weiter** aus.

5. Wählen Sie als Speicherort die Option **Databox-Datastore** (Data Box-Datenspeicher) und dann **Weiter** aus.

    ![Klon eines virtuellen Computers: Auswählen des Datenspeichers](media/databox-migration-vm-clone-select-datastore.png)

6. Wenn Sie Optionen für das geklonte Objekt anpassen möchten, wählen Sie die Anpassungsoptionen und dann **Weiter** aus.

7. Überprüfen Sie die Konfigurationen, und wählen Sie **Fertig stellen** aus.

8. Wiederholen Sie die Schritte 1 bis 7 für weitere virtuelle Computer oder Vorlagen für virtuelle Maschinen.

Virtuelle Computer werden geklont und aus Data Box im NFS-Datenspeicher gespeichert. Nachdem die virtuellen Computer geklont wurden, sollten Sie sicherstellen, dass sie zur Vorbereitung der Migration von Daten zur Azure VMware-Lösung heruntergefahren sind.

### <a name="copy-iso-files-to-the-data-box-datastore"></a>Kopieren von ISO-Dateien in den Data Box-Datenspeicher

1. Navigieren Sie in Ihrer lokalen vCenter-Webbenutzeroberfläche zu **Speicher**.  Wählen Sie **Databox-Datastore** (Data Box-Datenspeicher) und anschließend **Dateien** aus. Erstellen Sie einen neuen Ordner zum Speichern von ISO-Dateien.

    ![Kopieren von ISO-Dateien: Erstellen eines neuen Ordners](media/databox-migration-create-folder.png)

2. Geben Sie einen Namen für den Ordner an, in dem ISO-Dateien gespeichert werden.

3. Doppelklicken Sie auf den neu erstellten Ordner, um ihn zu öffnen.

4. Wählen Sie die Option **Dateien hochladen** und dann die ISO-Dateien aus, die Sie hochladen möchten.
    
    ![Kopieren von ISO-Dateien: Hochladen von Dateien](media/databox-migration-upload-iso.png)

> [!TIP]
> Wenn Sie in Ihrem lokalen Datenspeicher bereits über ISO-Dateien verfügen, können Sie die Dateien und die Option **Kopieren nach** auswählen, um die Dateien in den NFS-Datenspeicher von Data Box zu kopieren.


## <a name="prepare-data-box-for-return"></a>Vorbereiten von Data Box für die Rücksendung

Nachdem alle Daten des virtuellen Computers, Daten der Vorlagen für virtuelle Maschinen und ISO-Dateien in den NFS-Datenspeicher von Data Box kopiert wurden, können Sie die Verbindung des Datenspeichers mit Ihrer vCenter-Instanz trennen. Alle virtuellen Computer und Vorlagen für virtuelle Maschinen müssen aus dem Inventar entfernt werden, bevor Sie die Verbindung des Datenspeichers trennen.

### <a name="remove-objects-from-inventory"></a>Entfernen von Objekten aus dem Inventar

1. Navigieren Sie in Ihrer lokalen vCenter-Webbenutzeroberfläche zu **Speicher**. Wählen Sie **Databox-Datastore** (Data Box-Datenspeicher) und anschließend **VMs** aus.

    ![Entfernen virtueller Computer aus dem Inventar: Ausgeschaltete Computer](media/databox-migration-select-databox-vm.png)

2. Stellen Sie sicher, dass alle virtuellen Computer heruntergefahren sind.

3. Wählen Sie alle virtuellen Computer aus, klicken Sie mit der rechten Maustaste, und wählen Sie anschließend **Remove from inventory** (Aus Inventar entfernen) aus.

    ![Entfernen virtueller Computer aus dem Inventar](media/databox-migration-remove-vm-from-inventory.png)

4. Wählen Sie **VM Templates in Folders** (VM-Vorlagen in Ordnern) aus, und wiederholen Sie anschließend Schritt 3.

### <a name="remove-the-data-box-nfs-datastore-from-vcenter"></a>Entfernen des NFS-Datenspeichers von Data Box aus vCenter

Der NFS-Datenspeicher von Data Box muss von VMware-ESXi-Hosts getrennt werden, bevor die Rücksendung vorbereitet werden kann.

1. Navigieren Sie in Ihrer lokalen vCenter-Webbenutzeroberfläche zu **Speicher**.

2. Klicken Sie mit der rechten Maustaste auf **Databox-Datastore** (Data Box-Datenspeicher), und wählen Sie **Unmount Datastore** (Einbindung des Datenspeichers aufheben) aus.

    ![Aufheben der Einbindung des Data Box-Datenspeichers](media/databox-migration-unmount-datastore.png)

3. Wählen Sie alle ESXi-Hosts aus, für die der Datenspeicher eingebunden ist, und wählen Sie dann **OK** aus.

    ![Aufheben der Einbindung des Data Box-Datenspeichers: Auswählen der Hosts](media/databox-migration-unmount-datastore-select-hosts.png)

4. Überprüfen und akzeptieren Sie alle Warnungen, und wählen Sie **OK** aus.

### <a name="prepare-data-box-for-return-and-then-return-it"></a>Vorbereiten von Data Box für die Rücksendung und anschließendes Durchführen der Rücksendung

Führen Sie die im Artikel [Zurücksenden von Azure Data Box und Überprüfen des Datenuploads in Azure](../databox/data-box-deploy-picked-up.md), um die Data Box zurückzugeben. Überprüfen Sie den Status der Datenkopie in Ihrem Azure-Speicherkonto. Wenn der Status „Abgeschlossen“ angezeigt wird, können Sie die Daten in Ihrem Azure-Speicherkonto überprüfen.

## <a name="copy-data-from-azure-storage-to-azure-vmware-solution"></a>Kopieren von Daten aus Azure-Speicher in die Azure VMware-Lösung

Daten, die auf Ihr Data Box-Gerät kopiert werden, sind in Ihrem Azure-Speicherkonto verfügbar, wenn der Auftragsstatus für Ihre Data Box-Einheit als „Abgeschlossen“ angezeigt wird. Die Daten können jetzt in Ihre Azure VMware-Lösung kopiert werden. Daten im Speicherkonto müssen per NFS-Protokoll in den vSAN-Datenspeicher Ihrer privaten Cloud kopiert werden. 

Kopieren Sie zunächst Blobspeicherdaten auf einen verwalteten Datenträger eines virtuellen Linux-Computers in Azure, indem Sie **AzCopy** verwenden. Stellen Sie den verwalteten Datenträger per NFS bereit, binden Sie die NFS-Freigabe als Datenspeicher für Ihre private Cloud ein, und kopieren Sie anschließend die Daten. Diese Methode ermöglicht einen schnelleren Kopiervorgang der Daten in Ihre private Cloud.

### <a name="copy-data-to-your-private-cloud-using-a-linux-virtual-machine-and-managed-disks-and-then-export-as-nfs-share"></a>Kopieren von Daten in die private Cloud mithilfe eines virtuellen Linux-Computers und von verwalteten Datenträgern und anschließendes Exportieren als NFS-Freigabe

1. Erstellen Sie einen [virtuellen Linux-Computer](../virtual-machines/linux/quick-create-portal.md) in Azure, der in derselben Region angeordnet ist, in der das Speicherkonto erstellt wurde, und der über eine Verbindung des virtuellen Azure-Netzwerks mit Ihrer privaten Cloud verfügt.

2. Erstellen Sie einen verwalteten Datenträger, dessen Speicherkapazität größer als die Menge an Blobdaten ist, und [fügen Sie ihn an Ihren virtuellen Linux-Computer an](../virtual-machines/linux/attach-disk-portal.md).  Wenn die Menge der Blobdaten die Kapazität des größten verfügbaren verwalteten Datenträgers übersteigt, müssen die Daten in mehreren Schritten bzw. mit mehreren verwalteten Datenträgern kopiert werden.

3. Stellen Sie eine Verbindung mit dem virtuellen Linux-Computer her, und binden Sie den verwalteten Datenträger ein.

4. Installieren Sie [AzCopy auf dem virtuellen Linux-Computer](../storage/common/storage-use-azcopy-v10.md).

5. Laden Sie die Daten mithilfe von AzCopy aus Ihrem Azure-Blobspeicher auf den verwalteten Datenträger herunter.  Befehlssyntax: `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-directory-path>/"`.  Ersetzen Sie `<storage-account-name>` durch den Namen Ihres Azure-Speicherkontos und `<container-name>` durch den Container, der die per Data Box kopierten Daten enthält.

6. Installieren Sie den NFS-Server auf dem virtuellen Linux-Computer:

    - In einer Ubuntu-/Debian-Distribution: `sudo apt install nfs-kernel-server`.
    - In einer Enterprise Linux-Distribution: `sudo yum install nfs-utils`.

7. Ändern Sie die Berechtigung für den Ordner auf Ihrem verwalteten Datenträger, auf den die Daten aus dem Azure-Blobspeicher kopiert wurden.  Ändern Sie die Berechtigungen für alle Ordner, die Sie als NFS-Freigabe exportieren möchten.

    ```bash
    chmod -R 755 /<folder>/<subfolder>
    chown nfsnobody:nfsnobody /<folder>/<subfolder>
    ```

8. Weisen Sie Berechtigungen für IP-Clientadressen für den Zugriff auf die NFS-Freigabe zu, indem Sie die Datei `/etc/exports` bearbeiten.

    ```bash
    sudo vi /etc/exports
    ```
    
    Geben Sie in der Datei die folgenden Zeilen für jede ESXi-Host-IP-Adresse der privaten Cloud ein.  Fügen Sie alle Ordner hinzu, wenn Sie Freigaben für mehrere Ordner erstellen.

    ```bash
    /<folder>/<subfolder> <ESXiNode1IP>(rw,sync,no_root_squash,no_subtree_check)
    /<folder>/<subfolder> <ESXiNode2IP>(rw,sync,no_root_squash,no_subtree_check)
    .
    .
    ```

9. Exportieren Sie die NFS-Freigaben mit dem Befehl `sudo exportfs -a`.

10. Starten Sie den NFS-Kernelserver mit dem Befehl `sudo systemctl restart nfs-kernel-server` neu.


### <a name="mount-the-linux-virtual-machine-nfs-share-as-a-datastore-on-a-private-cloud-vcenter-cluster-and-then-copy-data"></a>Einbinden der NFS-Freigabe virtueller Linux-Computer als Datenspeicher für den vCenter-Cluster der privaten Cloud und anschließendes Kopieren von Daten

Die NFS-Freigabe Ihres virtuellen Linux-Computers muss als Datenspeicher für den vCenter-Cluster der privaten Cloud eingebunden werden. Nach der Einbindung können Daten aus dem NFS-Datenspeicher in den vSAN-Datenspeicher der privaten Cloud kopiert werden.

1. Melden Sie sich bei der vCenter Server-Instanz Ihrer privaten Cloud an.

2. Klicken Sie mit der rechten Maustaste auf **Datacenter**, und wählen Sie **Speicher**, **Neuer Datenspeicher** und dann **Weiter** aus.

   ![Hinzufügen eines neuen Datenspeichers](media/databox-migration-add-datastore.png)

3. Wählen Sie in Schritt 1 des Assistenten zum Hinzufügen von Datenspeicher den Typ **NFS** aus.

   ![Hinzufügen eines neuen Datenspeichers: Typ](media/databox-migration-add-datastore-type.png)

4. Wählen Sie in Schritt 2 des Assistenten die Option **NFS 3** als NFS-Version und dann **Weiter** aus.

   ![Hinzufügen eines neuen Datenspeichers: NFS-Version](media/databox-migration-add-datastore-nfs-version.png)

5. Geben Sie in Schritt 3 des Assistenten den Namen für den Datenspeicher, den Pfad und den Server an.  Sie können die IP-Adresse Ihres virtuellen Linux-Computers für den Server verwenden.  Der Ordnerpfad weist das Format `/<folder>/<subfolder>/` auf.

   ![Hinzufügen eines neuen Datenspeichers: NFS-Konfiguration](media/databox-migration-add-datastore-nfs-configuration.png)

6. Wählen Sie in Schritt 4 des Assistenten die ESXi-Hosts aus, auf denen der Datenspeicher bereitgestellt werden soll, und wählen Sie anschließend **Weiter** aus.  Wählen Sie in einem Cluster alle Hosts aus, um die Migration virtueller Computer sicherzustellen.

   ![Hinzufügen eines neuen Datenspeichers: Auswählen von Hosts](media/databox-migration-add-datastore-nfs-select-hosts.png)

7. Sehen Sie sich in Schritt 5 des Assistenten die Zusammenfassung an, und wählen Sie anschließend die Option **Fertig stellen** aus.

### <a name="add-virtual-machines-and-virtual-machine-templates-from-an-nfs-datastore-to-the-inventory"></a>Hinzufügen von virtuellen Computern und Vorlagen für virtuelle Maschinen aus einem NFS-Datenspeicher zum Inventar

1. Navigieren Sie in Ihrer vCenter-Webbenutzeroberfläche der privaten Cloud zu **Speicher**.  Wählen Sie einen virtuellen Linux-Computer mit NFS-Datenspeicher und anschließend die Option **Dateien** aus.

    ![Auswählen von Dateien aus dem NFS-Datenspeicher](media/databox-migration-datastore-select-files.png)

2. Wählen Sie einen Ordner aus, der einen virtuellen Computer oder eine Vorlage für virtuelle Maschinen enthält.  Wählen Sie im Detailbereich eine VMX-Datei für einen virtuellen Computer oder eine VMTX-Datei für eine Vorlage für virtuelle Maschinen aus.

3. Wählen Sie **VM registrieren** aus, um den virtuellen Computer in der vCenter-Instanz Ihrer privaten Cloud zu registrieren.

    ![Registrieren virtueller Computer](media/databox-migration-datastore-register-vm.png)

4. Wählen Sie das Datencenter, den Ordner und den Cluster/Ressourcenpool aus, in dem der virtuelle Computer registriert werden soll.

4. Wiederholen Sie die Schritte 3 und 4 für alle virtuellen Computer und Vorlagen für virtuelle Maschinen.

5. Navigieren Sie zum Ordner mit den ISO-Dateien.  Wählen Sie die ISO-Dateien und dann **Kopieren nach** aus, um die Dateien in einen Ordner in Ihrem vSAN-Datenspeicher zu kopieren.

Die virtuellen Computer und Vorlagen für virtuelle Maschinen sind nun in Ihrem vCenter der privaten Cloud verfügbar. Diese virtuellen Computer müssen aus dem NFS-Datenspeicher in den vSAN-Datenspeicher verschoben werden, bevor Sie sie hochfahren. Sie können die Option **Storage vMotion** verwenden und den vSAN-Datenspeicher als Ziel für die virtuellen Computer auswählen.

Die Vorlagen für virtuelle Maschinen müssen aus dem NFS-Datenspeicher Ihres virtuellen Linux-Computers in den vSAN-Datenspeicher geklont werden.

### <a name="clean-up-your-linux-virtual-machine"></a>Bereinigen Ihres virtuellen Linux-Computers

Nachdem alle Daten in Ihre private Cloud kopiert wurden, können Sie den NFS-Datenspeicher aus Ihrer privaten Cloud entfernen:

1. Stellen Sie sicher, dass alle virtuellen Computer und Vorlagen in den vSAN-Datenspeicher verschoben und geklont werden.

2. Entfernen Sie alle Vorlagen für virtuelle Maschinen des NFS-Datenspeichers aus dem Inventar.

3. Heben Sie die Einbindung des Datenspeichers des virtuellen Linux-Computers in Ihre vCenter-Instanz der privaten Cloud auf.

4. Löschen Sie den virtuellen Computer und den verwalteten Datenträger aus Azure.

5. Löschen Sie das Azure-Speicherkonto, falls Sie die Daten, die von Data Box in Ihr Speicherkonto übertragen wurden, nicht beibehalten möchten.  
    


## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich weiter über [Data Box](../databox/data-box-overview.md).
* Erfahren Sie mehr zu den verschiedenen Optionen für die [Migration von Workloads in Ihre private Cloud](migrate-workloads.md).
