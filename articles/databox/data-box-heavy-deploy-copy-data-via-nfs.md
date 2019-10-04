---
title: Tutorial zum Kopieren von Daten auf Azure Data Box Heavy über NFS | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten über NFS auf Ihr Azure Data Box Heavy-Gerät kopieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 4361cee3d07408c3abb5031d2ab18c15c92c5e0a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595809"
---
# <a name="tutorial-copy-data-to-azure-data-box-heavy-via-nfs"></a>Tutorial: Kopieren von Daten auf Azure Data Box Heavy über NFS

In diesem Tutorial erfahren Sie, wie Sie über die lokale Webbenutzeroberfläche eine Verbindung herstellen und Daten von Ihrem Hostcomputer auf Ihr Azure Data Box Heavy-Gerät kopieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Voraussetzungen
> * Herstellen einer Verbindung mit Data Box Heavy
> * Kopieren von Daten auf Data Box Heavy

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben die Schritte unter [Tutorial: Verkabeln und Herstellen einer Verbindung mit der Azure Data Box](data-box-heavy-deploy-set-up.md) ausgeführt.
2. Sie haben Ihr Data Box Heavy-Gerät erhalten, und die Bestellung wird im Portal mit dem Status **Geliefert** angezeigt.
3. Sie verfügen über einen Hostcomputer mit den Daten, die Sie auf Data Box Heavy kopieren möchten. Für Ihren Hostcomputer müssen die folgenden Bedingungen erfüllt sein:
    - Es muss ein [unterstütztes Betriebssystem](data-box-heavy-system-requirements.md) ausgeführt werden.
    - Er muss mit einem Hochgeschwindigkeitsnetzwerk verbunden sein. Zur Erzielung der höchstmöglichen Kopiergeschwindigkeit können zwei 40-GbE-Verbindungen (jeweils eine pro Knoten) parallel genutzt werden. Falls Sie über keine 40-GbE-Verbindung verfügen, sollten Sie zwei Verbindungen mit mindestens 10 GbE verwenden (jeweils eine pro Knoten). 

## <a name="connect-to-data-box-heavy"></a>Herstellen einer Verbindung mit Data Box Heavy

Je nach ausgewähltem Speicherkonto erstellt Data Box Heavy bis zu:
- Drei Freigaben für jedes verknüpfte Speicherkonto für GPv1 und GPv2
- Eine Freigabe für Storage Premium
- Eine Freigabe für das Blob Storage-Konto

Diese Freigaben werden auf beiden Knoten des Geräts erstellt.

Unter Blockblob- und Seitenblobfreigaben:
- Entitäten der ersten Ebene sind Container.
- Entitäten der zweiten Ebene sind Blobs.

Unter Freigaben für Azure Files:
- Entitäten der ersten Ebene sind Freigaben.
- Entitäten der zweiten Ebene sind Dateien.

Die folgende Tabelle enthält den UNC-Pfad zu den Freigaben auf Ihrem Data Box Heavy-Gerät und die Azure Storage-Pfad-URL für den Datenupload. Die endgültige URL des Azure Storage-Pfads kann aus dem UNC-Freigabepfad abgeleitet werden.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Azure-Blockblobs | <li>UNC-Pfad zu den Freigaben: `//<DeviceIPAddress>/<StorageAccountName_BlockBlob>/<ContainerName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Azure-Seitenblobs  | <li>UNC-Pfad zu den Freigaben: `//<DeviceIPAddres>/<StorageAccountName_PageBlob>/<ContainerName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Azure Files       |<li>UNC-Pfad zu den Freigaben: `//<DeviceIPAddres>/<StorageAccountName_AzFile>/<ShareName>/files/a.txt`</li><li>Azure Storage-URL: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |

Wenn Sie einen Linux-Hostcomputer verwenden, führen Sie die folgenden Schritte aus, um Ihr Gerät für NFS-Clientzugriff zu konfigurieren:

1. Geben Sie die IP-Adressen der zulässigen Clients an, die auf die Freigabe zugreifen können. Wechseln Sie in der lokalen Webbenutzeroberfläche zur Seite **Verbindung herstellen und Daten kopieren**. Klicken Sie unter **NFS-Einstellungen** auf **NFS-Clientzugriff**. 

    ![Konfigurieren des NFS-Clientzugriffs 1](media/data-box-deploy-copy-data/nfs-client-access.png)

2. Geben Sie die IP-Adresse des NFS-Clients an, und klicken Sie auf **Hinzufügen**. Sie können den Zugriff für mehrere NFS-Clients konfigurieren, indem Sie diesen Schritt wiederholen. Klicken Sie auf **OK**.

    ![Konfigurieren des NFS-Clientzugriffs 2](media/data-box-deploy-copy-data/nfs-client-access2.png)

2. Stellen Sie sicher, dass auf dem Linux-Hostcomputer eine [unterstützte Version](data-box-system-requirements.md) des NFS-Clients installiert ist. Verwenden Sie die jeweilige Version für Ihre Linux-Distribution. 

3. Führen Sie nach der Installation des NFS-Clients den folgenden Befehl aus, um die NFS-Freigabe auf Ihrem Data Box-Gerät einzubinden:

    `sudo mount <Data Box Heavy device IP>:/<NFS share on Data Box Heavy device> <Path to the folder on local Linux computer>`

    Das folgende Beispiel zeigt, wie Sie über NFS eine Verbindung mit einer Data Box Heavy-Freigabe herstellen. Die IP-Adresse von Data Box Heavy lautet `10.161.23.130`, und die Freigabe `Mystoracct_Blob` wird in den virtuellen Ubuntu-Computer eingebunden (Einbindungspunkt: `/home/databoxheavyubuntuhost/databoxheavy`).

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxheavyubuntuhost/databoxheavy`
    
    Für Mac-Clients müssen Sie eine zusätzliche Option wie folgt hinzufügen: 
    
    `sudo mount -t nfs -o sec=sys,resvport 10.161.23.130:/Mystoracct_Blob /home/databoxheavyubuntuhost/databoxheavy`

    **Erstellen Sie immer einen Ordner für die Dateien, die Sie unter die Freigabe kopieren möchten, und kopieren Sie die Dateien dann in diesen Ordner**. Der Ordner, der unter der Blockblob- und der Seitenblob Freigabe erstellt wurde, entspricht einem Container, in den Daten als Blobs hochgeladen werden. Es ist nicht möglich, Dateien direkt in den *root*-Ordner im Speicherkonto zu kopieren.

## <a name="copy-data-to-data-box-heavy"></a>Kopieren von Daten auf Data Box Heavy

Nachdem Sie eine Verbindung mit den Data Box Heavy-Freigaben hergestellt haben, müssen im nächsten Schritt die Daten kopiert werden. Bevor Sie mit dem Kopieren der Daten beginnen, sollten Sie folgende Aspekte beachten:

- Stellen Sie sicher, dass Sie die Daten in Freigaben kopieren, die das richtige Datenformat aufweisen. Kopieren Sie beispielsweise die Blockblobdaten in die Freigabe für Blockblobs. Kopieren Sie VHDs in Seitenblobs. Wenn das Datenformat nicht mit dem entsprechenden Freigabetyp übereinstimmt, tritt später beim Hochladen der Daten in Azure ein Fehler auf.
-  Achten Sie beim Kopieren der Daten auf die Einhaltung der Größenbeschränkungen, die im Artikel zu den [Grenzwerten für Azure Storage und Data Box Heavy](data-box-heavy-limits.md) beschrieben sind. 
- Falls von Data Box Heavy hochgeladene Daten gleichzeitig von anderen Anwendungen außerhalb von Data Box Heavy hochgeladen werden, kann dies zu Fehlern bei Uploadaufträgen und zu Datenbeschädigungen führen.
- Es wird empfohlen, SMB und NFS nicht gleichzeitig zu verwenden und Daten nicht an dasselbe Endziel in Azure zu kopieren. In solchen Fällen kann das endgültige Ergebnis nicht bestimmt werden.
- **Erstellen Sie immer einen Ordner für die Dateien, die Sie unter die Freigabe kopieren möchten, und kopieren Sie die Dateien dann in diesen Ordner**. Der Ordner, der unter der Blockblob- und der Seitenblob Freigabe erstellt wurde, entspricht einem Container, in den Daten als Blobs hochgeladen werden. Es ist nicht möglich, Dateien direkt in den *root*-Ordner im Speicherkonto zu kopieren.
- Bei der Erfassung von groß- und kleinschreibungsabhängigen Verzeichnis- und Dateinamen aus einer NFS-Freigabe in NFS auf einem Data Box Heavy-Gerät gilt Folgendes: 
    - Die Groß-/Kleinschreibung im Namen wird beibehalten.
    - Bei den Dateien wird die Groß-/Kleinschreibung nicht berücksichtigt.
    
    Falls Sie also beispielsweise `SampleFile.txt` und `Samplefile.Txt` auf das Gerät kopieren, bleiben die Groß-/Kleinbuchstaben im Namen erhalten. Die zweite Datei überschreibt jedoch die erste, da sie als die gleiche Datei betrachtet wird.


Wenn Sie einen Linux-Hostcomputer verwenden, verwenden Sie ein Kopierhilfsprogramm wie Robocopy. Einige der verfügbaren Alternativen in Linux sind [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) oder [Ultracopier](https://ultracopier.first-world.info/).  

Der `cp`-Befehl ist eine der besten Optionen zum Kopieren eines Verzeichnisses. Weitere Informationen zur Verwendung finden Sie auf den [Handbuchseiten zum cp-Befehl](http://man7.org/linux/man-pages/man1/cp.1.html).

Befolgen Sie die nachstehenden Richtlinien, wenn Sie die rsync-Option für einen Multithread-Kopiervorgang verwenden:

 - Installieren Sie je nach Dateisystem, das Ihr Linux-Client verwendet, das **CIFS Utils**- oder **NFS Utils**-Paket.

    `sudo apt-get install cifs-utils`

    `sudo apt-get install nfs-utils`

 -  Installieren Sie **Rsync** und **Parallel** (variiert abhängig von der Linux-Version).

    `sudo apt-get install rsync`
   
    `sudo apt-get install parallel` 

 - Erstellen Sie einen Bereitstellungspunkt.

    `sudo mkdir /mnt/databoxheavy`

 - Binden Sie das Volume ein.

    `sudo mount -t NFS4  //Databox-heavy-IP-Address/share_name /mnt/databoxheavy` 

 - Spiegeln Sie die Verzeichnisstruktur des Ordners.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databoxheavy`

 - Kopieren Sie Dateien. 

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databoxheavy/{}`

     Hierbei gibt „j“ die Anzahl von Parallelisierungen und „X“ die Anzahl paralleler Kopien an.

     Wir empfehlen, mit 16 parallelen Kopien zu beginnen und die Anzahl von Threads je nach verfügbaren Ressourcen zu erhöhen.

> [!IMPORTANT]
> Folgende Linux-Dateitypen werden nicht unterstützt: symbolische Verknüpfungen, Zeichendateien, Blockdateien, Sockets und Pipes. Diese Dateitypen führen zu Fehlern bei der **Versandvorbereitung**.

Öffnen Sie den Zielordner, um die kopierten Dateien anzuzeigen und zu überprüfen. Falls während des Kopierprozesses Fehler auftreten, laden Sie zur Problembehandlung die Fehlerdateien herunter. Weitere Informationen finden Sie unter [View error log during data copy](data-box-logs.md#view-error-log-during-data-copy) (Anzeigen des Fehlerprotokolls beim Kopieren von Daten). Eine detaillierte Liste mit Fehlern beim Kopieren von Dateien finden Sie unter [Troubleshoot issues related to Azure Data Box and Azure Data Box Heavy](data-box-troubleshoot.md) (Behandeln von Problemen mit Azure Data Box und Azure Data Box Heavy).

Um die Datenintegrität zu gewährleisten, wird inline eine Prüfsumme berechnet, während die Daten kopiert werden. Überprüfen Sie nach Abschluss des Kopiervorgangs den belegten Speicherplatz und den freien Speicherplatz auf Ihrem Gerät.
    
   ![Überprüfen des freien und belegten Speicherplatzes im Dashboard](media/data-box-deploy-copy-data/verify-used-space-dashboard.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Informationen zu Azure Data Box Heavy vermittelt:

> [!div class="checklist"]
> * Voraussetzungen
> * Herstellen einer Verbindung mit Data Box Heavy
> * Kopieren von Daten auf Data Box Heavy


Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Data Box zurück an Microsoft senden.

> [!div class="nextstepaction"]
> [Zurücksenden von Azure Data Box Heavy und Überprüfen des Datenuploads in Azure (Vorschauversion)](./data-box-heavy-deploy-picked-up.md)

