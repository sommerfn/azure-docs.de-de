---
title: Tutorial zum Importieren von VHD-Daten auf verwaltete Datenträger mithilfe von Microsoft Azure Data Box Heavy | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten von VHDs aus lokalen VM-Workloads auf Azure Data Box Heavy kopieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: a29cd142b3322c958f70aad8d5cad2bc30b87d76
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670836"
---
# <a name="tutorial-use-data-box-heavy-to-import-data-as-managed-disks-in-azure"></a>Tutorial: Verwenden von Data Box Heavy, um Daten als verwaltete Datenträger in Azure zu importieren

In diesem Tutorial erfahren Sie, wie Sie Azure Data Box Heavy verwenden, um Ihre lokalen VHDs zu verwalteten Datenträgern in Azure zu migrieren. Die VHDs lokaler virtueller Computer werden in Form von Seitenblobs auf Data Box Heavy kopiert und als verwaltete Datenträger in Azure hochgeladen. Diese verwalteten Datenträger können dann an virtuelle Azure-Computer angefügt werden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Überprüfen der Voraussetzungen
> * Herstellen einer Verbindung mit Data Box Heavy
> * Kopieren von Daten auf Data Box Heavy


## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben die Schritte unter [Tutorial: Verkabeln und Herstellen einer Verbindung mit der Azure Data Box](data-box-heavy-deploy-set-up.md) ausgeführt.
2. Sie haben Ihr Data Box Heavy-Gerät erhalten, und der Auftrag wird im Portal mit dem Status **Geliefert** angezeigt.
3. Sie verfügen über eine Verbindung mit einem Hochgeschwindigkeitsnetzwerk. Zur Erzielung der höchstmöglichen Kopiergeschwindigkeit können zwei 40-GbE-Verbindungen (jeweils eine pro Knoten) parallel genutzt werden. Falls Sie über keine 40-GbE-Verbindung verfügen, sollten Sie zwei Verbindungen mit mindestens 10 GbE verwenden (jeweils eine pro Knoten). 
4. Sie haben sich mit Folgendem vertraut gemacht:

    - Unterstützte Größen für verwaltete Datenträger und [Größenbeschränkungen für das Azure-Objekt](data-box-heavy-limits.md#azure-object-size-limits)
    - [Einführung in verwaltete Azure-Datenträger](/azure/virtual-machines/windows/managed-disks-overview) 

## <a name="connect-to-data-box-heavy"></a>Herstellen einer Verbindung mit Data Box Heavy

Data Box Heavy erstellt auf der Grundlage der angegebenen Ressourcengruppen pro Knoten jeweils eine Freigabe für die zugeordneten Ressourcengruppen. Wenn bei der Bestellung also beispielsweise `mydbmdrg1` und `mydbmdrg2` erstellt wurden, werden folgende Freigaben erstellt:

- `mydbmdrg1_MDisk`
- `mydbmdrg2_MDisk`

Innerhalb der einzelnen Freigaben werden jeweils die folgenden drei Ordner erstellt, die Containern in Ihrem Speicherkonto entsprechen:

- SSD Premium
- HDD Standard
- SSD Standard

Die folgende Tabelle enthält die UNC-Pfade der Freigaben auf Ihrem Data Box Heavy-Gerät:
 
|        Verbindungsprotokoll           |             UNC-Pfad der Freigabe                                               |
|-------------------|--------------------------------------------------------------------------------|
| SMB |`\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<Premium SSD>\file1.vhd`<br> `\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<Standard HDD>\file2.vhd`<br> `\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<Standard SSD>\file3.vhd` |  
| NFS |`//<DeviceIPAddress>/<ResourceGroup1_MDisk>/<Premium SSD>/file1.vhd`<br> `//<DeviceIPAddress>/<ResourceGroupName_MDisk>/<Standard HDD>/file2.vhd`<br> `//<DeviceIPAddress>/<ResourceGroupName_MDisk>/<Standard SSD>/file3.vhd` |

Die Schritte für die Verbindungsherstellung hängen davon ab, ob Sie SMB oder NFS verwenden, um die Verbindung mit den Data Box Heavy-Freigaben herzustellen.

> [!NOTE]
> - Die Verbindungsherstellung über REST wird für dieses Feature nicht unterstützt.
> - Führen Sie die Schritte für die Verbindungsherstellung erneut aus, um eine Verbindung mit dem zweiten Knoten von Data Box Heavy herzustellen.

### <a name="connect-to-data-box-heavy-via-smb"></a>Herstellen einer Verbindung mit Data Box Heavy über SMB

Wenn Sie einen Windows Server-Hostcomputer verwenden, führen Sie die folgenden Schritte aus, um eine Verbindung mit Data Box Heavy herzustellen:

1. Zunächst müssen Sie sich authentifizieren und eine Sitzung starten. Navigieren Sie zu **Verbindung herstellen und Daten kopieren**. Klicken Sie auf **Anmeldeinformationen abrufen**, um die Anmeldeinformationen für den Zugriff auf die mit Ihrer Ressourcengruppe verknüpften Freigaben abzurufen. Die Anmeldeinformationen für den Zugriff können auch über die **Gerätedetails** im Azure-Portal abgerufen werden.

    > [!NOTE]
    > Sie sind für alle Freigaben für verwaltete Datenträger identisch.

    ![Abrufen der Anmeldeinformationen für Freigaben 1](media/data-box-deploy-copy-data-from-vhds/get-share-credentials1.png)

2. Kopieren Sie im Dialogfeld „Auf Freigabe zugreifen und Daten kopieren“ die Werte für **Benutzername** und **Kennwort** für die Freigabe. Klicken Sie auf **OK**.
    
    ![Abrufen der Anmeldeinformationen für Freigaben 1](media/data-box-deploy-copy-data-from-vhds/get-share-credentials2.png)

3. Öffnen Sie ein Befehlsfenster, um über Ihren Hostcomputer auf die Freigaben zuzugreifen, die mit Ihrer Ressource (*mydbmdrg1* im folgenden Beispiel) verknüpft sind. Geben Sie an der Eingabeaufforderung Folgendes ein:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Die UNC-Freigabepfade lauten in diesem Beispiel wie folgt:

    - `\\169.254.250.200\mydbmdrg1_MDisk`
    - `\\169.254.250.200\mydbmdrg2_MDisk`
    
4. Geben Sie das Kennwort für die Freigabe ein, wenn Sie dazu aufgefordert werden. Das folgende Beispiel zeigt das Herstellen einer Verbindung mit einer Freigabe über den obigen Befehl.

    ```
    C:\>net use \\169.254.250.200\mydbmdrgl_MDisk /u:mdisk
    Enter the password for ‘mdisk’ to connect to '169.254.250.200':
    The command completed successfully.
    C: \>
    ```

4. Drücken Sie WINDOWS-TASTE+R. Geben Sie im Fenster **Ausführen** die `\\<device IP address>\<ShareName>` an. Klicken Sie auf **OK**, um den Datei-Explorer zu öffnen.
    
    ![Herstellen einer Verbindung mit der Freigabe über den Datei-Explorer 2](media/data-box-deploy-copy-data-from-vhds/connect-shares-file-explorer1.png)

    In jeder Freigabe sollten nun die folgenden vorab erstellten Ordner vorhanden sein.
    
    ![Herstellen einer Verbindung mit der Freigabe über den Datei-Explorer 2](media/data-box-deploy-copy-data-from-vhds/connect-shares-file-explorer2.png)


### <a name="connect-to-data-box-heavy-via-nfs"></a>Herstellen einer Verbindung mit Data Box Heavy über NFS

Wenn Sie einen Linux-Hostcomputer verwenden, führen Sie die folgenden Schritte aus, um Ihr Gerät für NFS-Clientzugriff zu konfigurieren:

1. Geben Sie die IP-Adressen der zulässigen Clients an, die auf die Freigabe zugreifen können. Wechseln Sie in der lokalen Webbenutzeroberfläche zur Seite **Verbindung herstellen und Daten kopieren**. Klicken Sie unter **NFS-Einstellungen** auf **NFS-Clientzugriff**.

    ![Konfigurieren des NFS-Clientzugriffs 1](media/data-box-deploy-copy-data-from-vhds/nfs-client-access1.png)

2. Geben Sie die IP-Adresse des NFS-Clients an, und klicken Sie auf **Hinzufügen**. Sie können den Zugriff für mehrere NFS-Clients konfigurieren, indem Sie diesen Schritt wiederholen. Klicken Sie auf **OK**.

    ![Konfigurieren des NFS-Clientzugriffs 2](media/data-box-deploy-copy-data-from-vhds/nfs-client-access2.png)

2. Stellen Sie sicher, dass auf dem Linux-Hostcomputer eine [unterstützte Version](data-box-system-requirements.md) des NFS-Clients installiert ist. Verwenden Sie die jeweilige Version für Ihre Linux-Distribution.

3. Führen Sie nach der Installation des NFS-Clients den folgenden Befehl aus, um die NFS-Freigabe auf Ihrem Gerät einzubinden:

    `sudo mount <Data Box or Data Box Heavy IP>:/<NFS share on Data Box or Data Box Heavy device> <Path to the folder on local Linux computer>`

    Das folgende Beispiel zeigt, wie Sie über NFS eine Verbindung mit einer Data Box- oder Data Box Heavy-Freigabe herstellen. Die IP-Adresse des Data Box- oder Data Box Heavy-Geräts lautet `169.254.250.200`, und die Freigabe `mydbmdrg1_MDisk` wird in den virtuellen Ubuntu-Computer eingebunden (Einbindungspunkt: `/home/databoxubuntuhost/databox`).

    `sudo mount -t nfs 169.254.250.200:/mydbmdrg1_MDisk /home/databoxubuntuhost/databox`


## <a name="copy-data-to-data-box-heavy"></a>Kopieren von Daten auf Data Box Heavy

Nach der Verbindungsherstellung mit dem Datenserver müssen im nächsten Schritt Ihre Daten kopiert werden. Die VHD-Datei wird als Seitenblob in das Stagingspeicherkonto kopiert. Das Seitenblob wird anschließend in einen verwalteten Datenträger konvertiert und in eine Ressourcengruppe verschoben.

Beachten Sie Folgendes, bevor Sie mit dem Kopieren der Daten beginnen:

- Kopieren Sie die VHDs immer in einen der vorab erstellten Ordner. Wenn Sie die VHDs an einen Ort außerhalb dieser Ordner oder in einen selbst erstellten Ordner kopieren, werden sie nicht als verwaltete Datenträger, sondern als Seitenblobs in das Azure Storage-Konto hochgeladen.
- Zur Erstellung von verwalteten Datenträgern können nur feste VHDs hochgeladen werden. VHDX-Dateien oder differenzierende VHDs werden nicht unterstützt.
- Der Name eines verwalteten Datenträgers muss in einer Ressourcengruppe über alle vorab erstellten Ordner hinweg eindeutig sein. Die VHDs, die in die vorab erstellten Ordner hochgeladen werden, müssen also jeweils einen eindeutigen Namen besitzen. Achten Sie darauf, dass der Name nicht mit dem Namen eines bereits vorhandenen verwalteten Datenträgers in einer Ressourcengruppe identisch ist.
- Informationen zu Grenzwerten für verwaltete Datenträger finden Sie unter [Größenbeschränkungen für das Azure-Objekt](data-box-heavy-limits.md#azure-object-size-limits).

Sie können Folgendes verwenden (abhängig davon, ob Sie die Verbindung über SMB oder über NFS herstellen):

- [Kopieren von Daten über SMB](data-box-heavy-deploy-copy-data.md#copy-data-to-data-box-heavy)
- [Kopieren von Daten über NFS](data-box-heavy-deploy-copy-data-via-nfs.md#copy-data-to-data-box-heavy)

Warten Sie, bis die Kopieraufträge abgeschlossen sind. Vergewissern Sie sich, dass die Kopieraufträge ohne Fehler abgeschlossen wurden, bevor Sie mit dem nächsten Schritt fortfahren.

![Keine Fehler auf der Seite **Verbindung herstellen und Daten kopieren**](media/data-box-deploy-copy-data-from-vhds/verify-no-errors-connect-and-copy.png)

Sollte während des Kopiervorgangs ein Fehler aufgetreten sein, laden Sie die Protokolle von der Seite **Verbindung herstellen und Daten kopieren** herunter.

- Falls Sie eine Datei kopiert haben, die keinem ganzzahligen Vielfachen von 512 Bytes entspricht, wird die Datei nicht als Seitenblob in Ihr Stagingspeicherkonto hochgeladen. Die Protokolle enthalten dann einen Fehler. Entfernen Sie die Datei, und kopieren Sie eine Datei, die einem ganzzahligen Vielfachen von 512 Bytes entspricht.

- Wenn Sie eine VHDX-Datei (nicht unterstützt) mit einem langen Namen kopiert haben, wird ein Fehler protokolliert.

    ![Fehler in den Protokollen der Seite **Verbindung herstellen und Daten kopieren**](media/data-box-deploy-copy-data-from-vhds/errors-connect-and-copy.png)

    Beheben Sie die Fehler, bevor Sie mit dem nächsten Schritt fortfahren.

Um die Datenintegrität zu gewährleisten, wird inline eine Prüfsumme berechnet, während die Daten kopiert werden. Überprüfen Sie nach Abschluss des Kopiervorgangs den belegten Speicherplatz und den freien Speicherplatz auf Ihrem Gerät.
    
![Überprüfen des freien und belegten Speicherplatzes im Dashboard](media/data-box-deploy-copy-data-from-vhds/verify-used-space-dashboard.png)

Nach Abschluss des Kopierauftrags können Sie zu **Für Versand vorbereiten** wechseln.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Informationen zu Azure Data Box Heavy vermittelt:

> [!div class="checklist"]
> * Überprüfen der Voraussetzungen
> * Herstellen einer Verbindung mit Data Box Heavy
> * Kopieren von Daten auf Data Box Heavy


Im nächsten Tutorial erfahren Sie, wie Sie Data Box Heavy an Microsoft zurücksenden.

> [!div class="nextstepaction"]
> [Zurücksenden von Azure Data Box Heavy und Überprüfen des Datenuploads in Azure (Vorschauversion)](./data-box-heavy-deploy-picked-up.md)

