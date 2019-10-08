---
title: 'Azure VMware-Lösung von CloudSimple: Migration mit Azure Data Box'
description: Massendatenmigration zur Azure VMware-Lösung von CloudSimple mit Azure Data Box
author: sharaths-cs
ms.author: dikamath
ms.date: 09/27/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5f19b98fbbbad2f43227bfa2f73eab47bf7b1699
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71817389"
---
# <a name="migrating-data-to-azure-vmware-solution-by-cloudsimple-using-azure-data-box"></a>Migrieren von Daten zur Azure VMware-Lösung von CloudSimple mit Azure Data Box

Mit der Cloudlösung Microsoft Azure Data Box können Sie Daten in Terabyte-Größe schnell, kostengünstig und zuverlässig in Azure übertragen. Die sichere Datenübertragung wird beschleunigt, indem Sie ein spezielles Data Box-Speichergerät erhalten. Jedes Speichergerät verfügt über eine maximal nutzbare Speicherkapazität von 80 TB und wird von einem regionalen Zustelldienst zu Ihrem Rechenzentrum transportiert. Das Gerät verfügt über ein widerstandsfähiges Gehäuse zum Schützen und Absichern von Daten während des Transports.

Mithilfe von Azure Data Box können Sie große VMware-Datenvolumen in Ihre private Cloud migrieren.  Daten aus Ihrer lokalen vSphere-Umgebung werden mithilfe des NFS-Protokolls in die Data Box kopiert.  Die Massendatenmigration umfasst das Kopieren einer Kopie der virtuellen Computer zu einem bestimmten Zeitpunkt, der Konfiguration und der zugehörigen Daten in die Data Box und die Auslieferung an Azure.

In diesem Artikel lernen Sie Folgendes:

* Einrichten von Azure Data Box.
* Kopieren von Daten aus einer lokalen VMware-Umgebung in die Data Box mithilfe von NFS.
* Vorbereiten der Rückgabe von Azure Data Box.
* Vorbereiten von Blobdaten für das Kopieren in die Azure VMware-Lösung von CloudSimple.
* Kopieren der Daten aus Azure in die private Cloud.

## <a name="scenarios"></a>Szenarien

Azure Data Box sollte in den folgenden Szenarien für die Massendatenmigration verwendet werden.

* Migrieren von großen Datenmengen aus einer lokalen Umgebung in die Azure VMware-Lösung von CloudSimple als Baseline und Synchronisieren von Unterschieden über das Netzwerk.
* Migrieren großer Mengen ausgeschalteter virtueller Computer (kalte virtuelle Computer).
* Migrieren der Daten virtueller Computer zum Einrichten von Entwicklungs- und Testumgebungen.
* Migrieren einer großen Anzahl von Vorlagen für virtuelle Maschinen, ISO-Dateien und Datenträgern virtueller Computer.

## <a name="before-you-begin"></a>Voraussetzungen

* Überprüfen Sie die Voraussetzungen, und [bestellen Sie die Data Box](../databox/data-box-deploy-ordered.md) über Ihr Azure-Portal.  Während des Bestellvorgangs müssen Sie ein Speicherkonto auswählen, das Blobspeicher zulässt.  Sobald Sie die Data Box erhalten haben, verbinden Sie sie mit Ihrem lokalen Netzwerk, und [richten Sie die Data Box](../databox/data-box-deploy-set-up.md) mit einer IP-Adresse ein, die von Ihrem vSphere Verwaltungsnetzwerk aus erreichbar ist.

* Erstellen Sie ein virtuelles Netzwerk und ein Speicherkonto in derselben Azure-Region, in der die Azure VMware-Lösung von CloudSimple bereitgestellt wird.

* Erstellen Sie die [Verbindung des virtuellen Azure-Netzwerks](cloudsimple-azure-network-connection.md) aus Ihrer privaten Cloud mit dem virtuellen Netzwerk, in dem das Speicherkonto erstellt wird, mithilfe der Schritte, die im Artikel [Herstellen einer Verbindung des virtuellen Azure-Netzwerks mit CloudSimple mit ExpressRoute](virtual-network-connection.md) beschrieben werden.

## <a name="set-up-azure-data-box-for-nfs"></a>Einrichten von Azure Data Box für NFS

Stellen Sie eine Verbindung mit der lokalen Web-UI Ihrer Azure Data Box her, indem Sie die Schritte verwenden, die im Abschnitt **Herstellen einer Verbindung mit Ihrem Gerät** im Artikel [Tutorial: Verkabeln und Herstellen einer Verbindung mit Azure Data Box](../databox/data-box-deploy-set-up.md) beschrieben werden.  Konfigurieren Sie Data Box, um den Zugriff auf NFS-Clients zuzulassen.

1. Wechseln Sie in der lokalen Webbenutzeroberfläche zur Seite **Verbindung herstellen und Daten kopieren**. Klicken Sie unter **NFS-Einstellungen** auf **NFS-Clientzugriff**. 

    ![Konfigurieren des NFS-Clientzugriffs 1](media/nfs-client-access.png)

2. Geben Sie die IP-Adresse des VMware-ESXi-Hosts ein, und klicken Sie auf **Hinzufügen**. Sie können den Zugriff für alle Hosts in Ihrem vSphere-Cluster konfigurieren, indem Sie diesen Schritt wiederholen. Klicken Sie auf **OK**.

    ![Konfigurieren des NFS-Clientzugriffs 2](media/nfs-client-access2.png)
> [!IMPORTANT]
> **Erstellen Sie immer einen Ordner für die Dateien, die Sie unter die Freigabe kopieren möchten, und kopieren Sie die Dateien dann in diesen Ordner**. Der Ordner, der unter der Blockblob- und der Seitenblob Freigabe erstellt wurde, entspricht einem Container, in den Daten als Blobs hochgeladen werden. Es ist nicht möglich, Dateien direkt in den *root*-Ordner im Speicherkonto zu kopieren.

Unter Blockblob- und Seitenblobfreigaben sind Entitäten der ersten Ebene Container, und Entitäten der zweiten Ebene sind Blobs. Unter Freigaben für Azure Files sind Entitäten erster Ebene Freigaben. Entitäten zweiter Ebene sind Dateien.

In der folgenden Tabelle sind der UNC-Pfad zu den Freigaben auf Ihrer Data Box und die Azure Storage-Pfad-URL aufgeführt, wohin die Daten hochgeladen werden. Die endgültige URL des Azure Storage-Pfads kann aus dem UNC-Freigabepfad abgeleitet werden.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Azure-Blockblobs | <li>UNC-Pfad zu den Freigaben: `//<DeviceIPAddress>/<StorageAccountName_BlockBlob>/<ContainerName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure-Seitenblobs  | <li>UNC-Pfad zu den Freigaben: `//<DeviceIPAddres>/<StorageAccountName_PageBlob>/<ContainerName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Files       |<li>UNC-Pfad zu den Freigaben: `//<DeviceIPAddres>/<StorageAccountName_AzFile>/<ShareName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |

> [!NOTE]
> Verwenden Sie Azure-Blockblobs zum Kopieren von VMware-Daten.

## <a name="mount-nfs-share-as-a-datastore-on-on-premises-vcenter-cluster-and-copy-data"></a>Einbinden der NFS-Freigabe als Datenspeicher für den lokalen vCenter-Cluster und Kopieren von Daten

Eine NFS-Freigabe aus Azure Data Box muss als Datenspeicher für Ihren lokalen vCenter-Cluster oder VMware-ESXi-Host eingebunden werden, um die Daten zu kopieren.  Nach der Einbindung können Daten in den NFS-Datenspeicher kopiert werden.

1. Melden Sie sich bei Ihrem lokalen vCenter-Server an.

2. Klicken Sie mit der rechten Maustaste auf das **Rechenzentrum**, wählen Sie **Speicher** aus, klicken Sie auf **Neuer Datenspeicher** und dann auf **Weiter**.

   ![Hinzufügen eines neuen Datenspeichers](media/databox-migration-add-datastore.png)

3. Wählen Sie in Schritt 1 des Assistenten zum Hinzufügen von Datenspeichern den Typ **NFS** aus.

   ![Hinzufügen eines neuen Datenspeichers: Typ](media/databox-migration-add-datastore-type.png)

4. Wählen Sie in Schritt 2 **NFS 3** als NFS-Version aus, und klicken Sie auf **Weiter**.

   ![Hinzufügen eines neuen Datenspeichers: NFS-Version](media/databox-migration-add-datastore-nfs-version.png)

5. Geben Sie in Schritt 3 den Namen für den Datenspeicher, den Pfad und den Server an.  Sie können die IP-Adresse Ihrer Data Box für den Server verwenden.  Der Ordnerpfad weist das Format `/<StorageAccountName_BlockBlob>/<ContainerName>/` auf.

   ![Hinzufügen eines neuen Datenspeichers: NFS-Konfiguration](media/databox-migration-add-datastore-nfs-configuration.png)

6. Wählen Sie in Schritt 4 die ESXi-Hosts aus, in die der Datenspeicher eingebunden werden soll, und klicken Sie dann auf **Weiter**.  Wählen Sie in einem Cluster alle Hosts aus, um die Migration virtueller Computer sicherzustellen.

   ![Hinzufügen eines neuen Datenspeichers: Auswählen von Hosts](media/databox-migration-add-datastore-nfs-select-hosts.png)

7. Überprüfen Sie in Schritt 5 die Zusammenfassung, und klicken Sie dann auf **Fertig stellen**.

## <a name="copy-data-to-data-box-nfs-datastore"></a>Kopieren von Daten in den NFS-Datenspeicher der Data Box

Virtuelle Computer können zum neuen Datenspeicher migriert oder in ihn geklont werden.  Nicht verwendete virtuelle Computer, die migriert werden müssen, können mithilfe von **storage VMotion** zum NFS-Datenspeicher der Data Box migriert werden.  Aktive virtuelle Computer können in den NFS-Datenspeicher der Data Box **geklont** werden.

* Identifizieren Sie die Liste der virtuellen Computer, die **verschoben** werden können.
* Identifizieren Sie die Liste der virtuellen Computer, die **geklont** werden müssen.

### <a name="move-virtual-machine-to-data-box-datastore"></a>Verschieben eines virtuellen Computers in den Data Box-Datenspeicher

1. Klicken Sie mit der rechten Maustaste auf den virtuellen Computer, den Sie in den Data Box-Datenspeicher verschieben möchten, und wählen Sie dann **Migrieren** aus.

    ![Migrieren eines virtuellen Computers](media/databox-migration-vm-migrate.png)

2. Wählen Sie **Nur Speicher ändern** als Migrationstyp aus, und klicken Sie dann auf **Weiter**.

    ![Migrieren eines virtuellen Computers: Nur Speicher](media/databox-migration-vm-migrate-change-storage.png)

3. Wählen Sie den Data Box-Datenspeicher als Ziel aus, und klicken Sie dann auf **Weiter**.

    ![Migrieren eines virtuellen Computers: Auswählen des Datenspeichers](media/databox-migration-vm-migrate-change-storage-select-datastore.png)

4. Überprüfen Sie die Informationen, und klicken Sie auf **Fertig stellen**.

5. Wiederholen Sie die Schritte 1 bis 4 für andere virtuelle Computer.

> [!TIP]
> Sie können mehrere virtuelle Computer im gleichen Energiezustand auswählen (eingeschaltet oder ausgeschaltet) und sie in einem Massenvorgang migrieren.

Der Speicher für den virtuellen Computer wird aus Azure Data Box zum NFS-Datenspeicher migriert.  Nachdem alle virtuellen Computer migriert wurden, können Sie die eingeschalteten virtuellen Computer als Vorbereitung für die Migration von Daten zur Azure VMware-Lösung herunterfahren.

### <a name="clone-a-virtual-machine-or-a-virtual-machine-template-to-data-box-datastore"></a>Klonen eines virtuellen Computers oder einer Vorlage für virtuelle Maschinen in den Data Box-Datenspeicher

1. Klicken Sie mit der rechten Maustaste auf einen virtuellen Computer oder eine Vorlage für virtuelle Maschinen, den bzw. die Sie klonen möchten.  Wählen Sie **Klonen**, **In virtuellen Computer klonen** aus.

    ![Klon eines virtuellen Computers](media/databox-migration-vm-clone.png)

2. Wählen Sie einen Namen für den geklonten virtuellen Computer oder die Vorlage für virtuelle Maschinen aus.

3. Wählen Sie den Ordner aus, in dem Sie das geklonte Objekt platzieren möchten, und klicken Sie auf **Weiter**.

4. Wählen Sie den Cluster oder den Ressourcenpool aus, der das geklonte Objekt enthalten soll, und klicken Sie auf **Weiter**.

5. Wählen Sie den NFS-Datenspeicher von Azure Data Box als Speicherort aus, und klicken Sie auf **Weiter**.

    ![Klon eines virtuellen Computers: Auswählen des Datenspeichers](media/databox-migration-vm-clone-select-datastore.png)

6. Wählen Sie die Optionen zum Anpassen aus, wenn Sie Optionen für das geklonte Objekt anpassen möchten, und klicken Sie auf **Weiter**.

7. Überprüfen Sie die Konfigurationen, und klicken Sie auf **Fertig stellen**.

8. Wiederholen Sie die Schritte 1 bis 7 für weitere virtuelle Computer oder Vorlagen für virtuelle Maschinen.

Virtuelle Computer werden geklont und aus Azure Data Box im NFS-Datenspeicher gespeichert.  Nachdem alle virtuellen Computer geklont wurden, stellen Sie sicher, dass die geklonten virtuellen Computer als Vorbereitung für die Migration von Daten zur Azure VMware-Lösung ausgeschaltet werden.

### <a name="copy-iso-files-to-data-box-datastore"></a>Kopieren von ISO-Dateien in den Data Box-Datenspeicher

1. Navigieren Sie in Ihrer lokalen vCenter-Web-UI zu **Speicher**.  Wählen Sie den NFS-Datenspeicher von Data Box aus, und klicken Sie auf **Dateien**.  Erstellen Sie einen **neuen Ordner** zum Speichern von ISO-Dateien.

    ![Kopieren von ISO-Dateien: Erstellen eines neuen Ordners](media/databox-migration-create-folder.png)

2. Geben Sie einen Namen für den Ordner an, in dem ISO-Dateien gespeichert werden.

3. Doppelklicken Sie auf den neu erstellten Ordner, um auf seinen Inhalt zuzugreifen.

4. Klicken Sie auf **Dateien hochladen**, und wählen Sie die ISO-Dateien aus, die Sie hochladen möchten.
    
    ![Kopieren von ISO-Dateien: Hochladen von Dateien](media/databox-migration-upload-iso.png)

> [!TIP]
> Wenn Sie bereits über ISO-Dateien in Ihrem lokalen Datenspeicher verfügen, können Sie die Dateien auswählen und in den NFS-Datenspeicher von Data Box **kopieren**.


## <a name="prepare-azure-data-box-for-return"></a>Vorbereiten von Azure Data Box für die Rückgabe

Nachdem alle Daten des virtuellen Computers, Vorlagendaten für virtuelle Maschinen und ISO-Dateien in den NFS-Datenspeicher von Data Box kopiert wurden, kann der Datenspeicher von Ihrem vCenter getrennt werden.  Alle virtuellen Computer und Vorlagen für virtuelle Maschinen müssen aus dem Inventar entfernt werden, bevor der Datenspeicher getrennt werden kann.

### <a name="remove-objects-from-inventory"></a>Entfernen von Objekten aus dem Inventar

1. Navigieren Sie in Ihrer lokalen vCenter-Web-UI zu **Speicher**.  Wählen Sie den NFS-Datenspeicher von Data Box aus, und klicken Sie auf **VMs**.

2. Stellen Sie sicher, dass alle virtuellen Computer ausgeschaltet sind.

    ![Entfernen virtueller Computer aus dem Inventar: Ausgeschaltete Computer](media/databox-migration-select-databox-vm.png)

3. Wählen Sie alle virtuellen Computer aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Aus Inventar entfernen** aus.

    ![Entfernen virtueller Computer aus dem Inventar](media/databox-migration-remove-vm-from-inventory.png)

4. Wählen Sie **VM-Vorlagen in Ordnern** aus den oberen Schaltflächen aus, und wiederholen Sie Schritt 3. 

### <a name="remove-azure-data-box-nfs-datastore-from-vcenter"></a>Entfernen des NFS-Datenspeichers von Azure Data Box aus vCenter

Der NFS-Datenspeicher von Data Box muss von VMware-ESXi-Hosts getrennt werden, bevor die Rückgabe vorbereitet werden kann.

1. Navigieren Sie in Ihrer lokalen vCenter-Web-UI zu **Speicher**.

2. Klicken Sie mit der rechten Maustaste auf den NFS-Datenspeicher von Data Box, und wählen Sie **Einbindung des Datenspeichers aufheben** aus.

    ![Aufheben der Einbindung des Data Box-Datenspeichers](media/databox-migration-unmount-datastore.png)

3. Wählen Sie alle ESXi-Hosts aus, für die der Datenspeicher eingebunden ist, und klicken Sie auf **OK**.

    ![Aufheben der Einbindung des Data Box-Datenspeichers: Auswählen der Hosts](media/databox-migration-unmount-datastore-select-hosts.png)

4. Überprüfen und akzeptieren Sie alle Warnungen, und klicken Sie auf **OK**.

### <a name="prepare-data-box-for-return-and-return-the-data-box"></a>Vorbereiten von Data Box für die Rückgabe und Rückgabe der Data Box

Führen Sie die im Artikel [Zurücksenden von Azure Data Box und Überprüfen des Datenuploads in Azure](../databox/data-box-deploy-picked-up.md), um die Data Box zurückzugeben.  Überprüfen Sie den Status der Datenkopie in Ihrem Azure-Speicherkonto. Sobald der Status als abgeschlossen angezeigt wird, können Sie die Daten in Ihrem Azure-Speicherkonto überprüfen.

## <a name="copy-data-from-azure-storage-to-azure-vmware-solution-by-cloudsimple"></a>Kopieren von Daten aus Azure-Speicher in die Azure VMware-Lösung von CloudSimple

Daten, die in Azure Data Box kopiert werden, sind in Ihrem Azure-Speicherkonto verfügbar, sobald der Auftragsstatus Ihrer Data Box als abgeschlossen angezeigt wird.  Die Daten können jetzt in Ihre Azure VMware-Lösung von CloudSimple kopiert werden.  Daten im Speicherkonto müssen mithilfe des NFS-Protokolls in den vSAN-Datenspeicher der privaten Cloud kopiert werden.  Kopieren Sie zunächst Blobspeicherdaten auf einen verwalteten Datenträger auf einem virtuellen Linux-Computer in Azure, indem Sie **AzCopy** verwenden.  Stellen Sie den verwalteten Datenträger über das NFS-Protokoll bereit, binden Sie die NFS-Freigabe als Datenspeicher für Ihre private Cloud ein, und kopieren Sie die Daten.  Diese Methode ermöglicht eine schnellere Kopie der Daten in Ihre private Cloud. 

### <a name="copy-data-to-your-private-cloud-using-a-linux-virtual-machine-and-managed-disks-and-export-as-nfs-share"></a>Kopieren von Daten in die private Cloud mithilfe eines virtuellen Linux-Computers und verwalteter Datenträger und Exportieren als NFS-Freigabe

1. Erstellen Sie einen [virtuellen Linux-Computer](../virtual-machines/linux/quick-create-portal.md) in Azure in derselben Region, in der das Speicherkonto erstellt wurde, der über eine Verbindung des virtuellen Azure-Netzwerks mit Ihrer privaten Cloud verfügt.

2. Erstellen Sie einen verwalteten Datenträger, der größer ist als die Menge an Blobdaten, und [fügen Sie ihn an Ihren virtuellen Linux-Computer an](../virtual-machines/linux/attach-disk-portal.md).  Wenn die Menge der Blobdaten größer als der größte verfügbare verwaltete Datenträger ist, müssen die Daten in mehreren Schritten kopiert oder mehrere verwaltete Datenträger verwendet werden.

3. Stellen Sie eine Verbindung mit dem virtuellen Linux-Computer her, und binden Sie den verwalteten Datenträger ein.

4. Installieren Sie [AzCopy auf dem virtuellen Linux-Computer](../storage/common/storage-use-azcopy-v10.md).

5. Laden Sie die Daten aus Azure-Blobspeicher auf den verwalteten Datenträger mithilfe von AzCopy herunter.  Befehlssyntax: `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-directory-path>/"`.  Ersetzen Sie `<storage-account-name>` durch den Namen Ihres Azure-Speicherkontos und `<container-name>` durch den Container, der die mit Azure Data Box kopierten Daten enthält.

6. Installieren Sie den NFS-Server auf dem virtuellen Linux-Computer.

    1. In einer Ubuntu-/Debian-Distribution: `sudo apt install nfs-kernel-server`.
    2. In einer Enterprise Linux-Distribution: `sudo yum install nfs-utils`.

7. Ändern Sie die Berechtigung für den Ordner auf dem verwalteten Datenträger, auf den die Daten aus dem Azure-Blobspeicher kopiert wurden.  Ändern Sie die Berechtigungen für alle Ordner, die Sie als NFS-Freigabe exportieren möchten.

    ```bash
    chmod -R 755 /<folder>/<subfolder>
    chown nfsnobody:nfsnobody /<folder>/<subfolder>
    ```

8. Weisen Sie Berechtigungen für IP-Clientadressen für den Zugriff auf die NFS-Freigabe zu, indem Sie die Datei `/etc/exports` bearbeiten.

    ```bash
    sudo vi /etc/exports
    ```
    
    Geben Sie die folgenden Zeilen in der Datei für jede ESXi-Host-IP-Adresse der privaten Cloud ein.  Wenn Sie Freigaben für mehrere Ordner erstellen, fügen Sie alle Ordner hinzu.

    ```bash
    /<folder>/<subfolder> <ESXiNode1IP>(rw,sync,no_root_squash,no_subtree_check)
    /<folder>/<subfolder> <ESXiNode2IP>(rw,sync,no_root_squash,no_subtree_check)
    .
    .
    ```

9. Exportieren Sie die NFS-Freigaben mithilfe des Befehls `sudo exportfs -a`.

10. Starten Sie den NFS-Kernelserver mithilfe des Befehls `sudo systemctl restart nfs-kernel-server` neu.


### <a name="mount-linux-virtual-machine-nfs-share-as-a-datastore-on-private-cloud-vcenter-cluster-and-copy-data"></a>Einbinden der NFS-Freigabe virtueller Linux-Computer als Datenspeicher für den vCenter-Cluster der privaten Cloud und Kopieren von Daten

Die NFS-Freigabe Ihres virtuellen Linux-Computers muss als Datenspeicher für den vCenter-Cluster der privaten Cloud eingebunden werden, um die Daten zu kopieren.  Nach der Einbindung können Daten aus dem NFS-Datenspeicher in den vSAN-Datenspeicher der privaten Cloud kopiert werden.

1. Melden Sie sich beim vCenter Server Ihrer privaten Cloud an.

2. Klicken Sie mit der rechten Maustaste auf das **Rechenzentrum**, wählen Sie **Speicher** aus, klicken Sie auf **Neuer Datenspeicher** und dann auf **Weiter**.

   ![Hinzufügen eines neuen Datenspeichers](media/databox-migration-add-datastore.png)

3. Wählen Sie in Schritt 1 des Assistenten zum Hinzufügen von Datenspeichern den Typ **NFS** aus.

   ![Hinzufügen eines neuen Datenspeichers: Typ](media/databox-migration-add-datastore-type.png)

4. Wählen Sie in Schritt 2 **NFS 3** als NFS-Version aus, und klicken Sie auf **Weiter**.

   ![Hinzufügen eines neuen Datenspeichers: NFS-Version](media/databox-migration-add-datastore-nfs-version.png)

5. Geben Sie in Schritt 3 den Namen für den Datenspeicher, den Pfad und den Server an.  Sie können die IP-Adresse Ihres virtuellen Linux-Computers für den-Server verwenden.  Der Ordnerpfad weist das Format `/<folder>/<subfolder>/` auf.

   ![Hinzufügen eines neuen Datenspeichers: NFS-Konfiguration](media/databox-migration-add-datastore-nfs-configuration.png)

6. Wählen Sie in Schritt 4 die ESXi-Hosts aus, in die der Datenspeicher eingebunden werden soll, und klicken Sie dann auf **Weiter**.  Wählen Sie in einem Cluster alle Hosts aus, um die Migration virtueller Computer sicherzustellen.

   ![Hinzufügen eines neuen Datenspeichers: Auswählen von Hosts](media/databox-migration-add-datastore-nfs-select-hosts.png)

7. Überprüfen Sie in Schritt 5 die Zusammenfassung, und klicken Sie dann auf **Fertig stellen**.

### <a name="add-virtual-machines-and-virtual-machine-templates-from-nfs-datastore-to-the-inventory"></a>Hinzufügen von virtuellen Computern und Vorlagen für virtuelle Maschinen aus dem NFS-Datenspeicher zum Inventar

1. Navigieren Sie in Ihrer vCenter-Web-UI der privaten Cloud zu **Speicher**.  Wählen Sie den NFS-Datenspeicher des virtuellen Linux-Computers aus, und klicken Sie auf **Dateien**.

    ![Auswählen von Dateien aus dem NFS-Datenspeicher](media/databox-migration-datastore-select-files.png)

2. Wählen Sie einen Ordner aus, der einen virtuellen Computer oder eine Vorlage für virtuelle Maschinen enthält.  Wählen Sie im Detailbereich die Datei `.vmx` für einen virtuellen Computer oder die Datei `.vmtx` für eine Vorlage für virtuelle Maschinen aus.

3. Klicken Sie auf **VM registrieren**, um den virtuellen Computer im vCenter Ihrer privaten Cloud zu registrieren.

    ![Registrieren virtueller Computer](media/databox-migration-datastore-register-vm.png)

4. Wählen Sie das Rechenzentrum, den Ordner und den Cluster/Ressourcenpool aus, in dem der virtuelle Computer registriert werden soll.

4. Wiederholen Sie die Schritte 3 und 4 für alle virtuellen Computer und Vorlagen für virtuelle Maschinen.

5. Navigieren Sie zu dem Ordner, der die ISO-Dateien enthält.  Wählen Sie ISO-Dateien aus, und **kopieren** Sie sie in einen Ordner in Ihrem vSAN-Datenspeicher.

Die virtuellen Computer und Vorlagen für virtuelle Maschinen sind nun in Ihrem vCenter der privaten Cloud verfügbar.  Diese virtuellen Computer müssen aus dem NFS-Datenspeicher in den vSAN-Datenspeicher verschoben werden, bevor Sie sie einschalten.  Sie können „storage vMotion“ der virtuellen Computer durchführen und den vSAN-Datenspeicher als Ziel für den virtuellen Computer auswählen.

Die Vorlagen für virtuelle Maschinen müssen aus dem NFS-Datenspeicher Ihres virtuellen Linux-Computers in den vSAN-Datenspeicher geklont werden.

### <a name="clean-up-of-your-linux-virtual-machine"></a>Bereinigen des virtuellen Linux-Computers

Nachdem alle Daten in die private Cloud kopiert wurden, können Sie den NFS-Datenspeicher aus der privaten Cloud entfernen.

1. Stellen Sie sicher, dass alle virtuellen Computer und Vorlagen in den vSAN-Datenspeicher verschoben und geklont werden.

2. Entfernen Sie alle Vorlagen für virtuelle Maschinen aus dem NFS-Datenspeicher aus dem Inventar.

3. Heben Sie die Einbindung des Datenspeichers des virtuellen Linux-Computers in Ihr vCenter der privaten Cloud auf.

4. Löschen Sie den virtuellen Computer und den verwalteten Datenträger aus Azure.

5. Wenn Sie die mit Data Box übertragenen Daten nicht in Ihrem Speicherkonto beibehalten möchten, löschen Sie das Azure-Speicherkonto.  
    


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Azure Data Box](../databox/data-box-overview.md)
* Weitere Informationen zu den verschiedenen Optionen für die [Migration von Workloads in die private Cloud](migrate-workloads.md)
