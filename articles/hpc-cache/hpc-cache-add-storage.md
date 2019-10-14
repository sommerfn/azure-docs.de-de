---
title: Hinzufügen von Speicher zu einem Azure HPC Cache (Vorschauversion)
description: Definieren von Speicherzielen, damit Ihr Azure HPC Cache Ihr lokales NFS-System oder Azure-Blobcontainer für die langfristige Speicherung von Dateien verwenden kann
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: rohogue
ms.openlocfilehash: dbcc68bacf8a11a7a85d5fad7fb4435fd03c7f93
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72024563"
---
# <a name="add-storage-targets"></a>Hinzufügen von Speicherzielen

*Speicherziele* sind Back-End-Speicher für Dateien, auf die über eine Azure HPC Cache-Instanz zugegriffen wird. Sie können NFS-Speicher hinzufügen, z. B. ein lokales Hardwaresystem, oder Daten in Azure Blob Storage speichern.

Es können bis zu zehn verschiedene Speicherziele für einen Cache definiert werden. Der Cache stellt alle Speicherziele in einem aggregierten Namespace dar.

Beachten Sie, dass der Zugriff auf die Speicherexporte vom virtuellen Netzwerk Ihres Caches aus möglich sein muss. Für einen lokalen Hardwarespeicher müssen Sie möglicherweise einen DNS-Server einrichten, der Hostnamen für den NFS-Speicherzugriff auflösen kann. Weitere Informationen finden Sie unter [DNS-Zugriff](hpc-cache-prereqs.md#dns-access).

Fügen Sie nach dem Erstellen des Caches Speicherziele hinzu. Das Verfahren unterscheidet sich geringfügig, je nachdem, ob Sie Azure Blobspeicher oder einen NFS-Export hinzufügen. Details zu beiden Szenarien finden Sie unten.

## <a name="open-the-storage-targets-page"></a>Öffnen der Seite „Speicherziele“

Öffnen Sie im Azure-Portal Ihre Cache-Instanz, und klicken Sie in der linken Seitenleiste auf **Speicherziele**. Auf der Seite „Speicherziele“ werden alle vorhandenen Ziele und ein Link zum Hinzufügen eines neuen Ziels aufgelistet.

![Screenshot des Links „Speicherziele“ in der Randleiste unter der Überschrift „Konfigurieren“, die sich zwischen den Kategorieüberschriften „Einstellungen“ und „Überwachung“ befindet](media/hpc-cache-storage-targets-sidebar.png)

## <a name="add-a-new-azure-blob-storage-target"></a>Hinzufügen eines neuen Azure Blobspeicherziels

Für ein neues Blobspeicherziel ist ein leerer Blobcontainer oder ein Container erforderlich, der mit Daten im Format des Azure HPC Cache-Clouddateisystems aufgefüllt ist. Weitere Informationen zum Vorabladen eines Blobcontainers finden Sie unter [Daten in Azure-Blobspeicher verschieben](hpc-cache-ingest.md).

Geben Sie diese Informationen ein, um einen Azure Blobcontainer zu definieren.

![Screenshot der Seite „Speicherziel hinzufügen“, aufgefüllt mit Informationen für ein neues Azure-Blobspeicherziel](media/hpc-cache-add-blob.png)

<!-- need to replace screenshot after note text is updated with both required RBAC roles and also with correct search term -->

* **Name des Speicherziels**: Legen Sie einen Namen fest, der dieses Speicherziel im Azure HPC Cache identifiziert.
* **Zieltyp**: Wählen Sie **Blob** aus.
* **Speicherkonto**: Wählen Sie das Konto mit dem Container aus, auf den verwiesen werden soll.

  Sie müssen die Cache-Instanz für den Zugriff auf das Speicherkonto autorisieren, wie unter [Hinzufügen der Zugriffsrollen](#add-the-access-control-roles-to-your-account) beschrieben.

  Informationen zur Art des Speicherkontos, das Sie verwenden können, finden Sie unter [Blobspeicheranforderungen](hpc-cache-prereqs.md#blob-storage-requirements).

* **Speichercontainer**: Wählen Sie den Blobcontainer für dieses Ziel aus.

* **Pfad des virtuellen Namespace**: Legen Sie den clientseitigen Dateipfad für dieses Speicherziel fest. Weitere Informationen zum Feature virtueller Namespace finden Sie unter [Aggregierten Namespace konfigurieren](hpc-cache-namespace.md).

Klicken Sie abschließend auf **OK**, um das Speicherziel hinzuzufügen.

### <a name="add-the-access-control-roles-to-your-account"></a>Hinzufügen der Zugriffssteuerungsrollen zu Ihrem Konto

Azure HPC Cache verwendet die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/role-based-access-control/index), um die Cache-Anwendung für den Zugriff auf Ihr Speicherkonto für Azure-Blobspeicherziele zu autorisieren.

Der Besitzer des Speicherkontos muss die Rollen [Speicherkontomitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor) und [Mitwirkender an Storage-Blobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) für den Benutzer „HPC Cache-Ressourcenanbieter“ explizit hinzufügen.

Sie können dies im Voraus erledigen oder indem Sie auf einen Link auf der Seite klicken, auf der Sie ein Blobspeicherziel hinzufügen.

Schritte zum Hinzufügen der RBAC-Rollen:

1. Öffnen Sie die Seite **Zugriffssteuerung (IAM)** für das Speicherkonto. (Der Link auf der Seite **Speicherziel hinzufügen** öffnet diese Seite für das ausgewählte Konto automatisch.)

1. Klicken Sie auf die **+** oben auf der Seite, und wählen Sie **Rollenzuweisung hinzufügen** aus.

1. Wählen Sie in der Liste die Rolle "Speicherkontomitwirkender" aus.

1. Übernehmen Sie im Feld **Zugriff zuweisen zu** den Standardwert ("Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal").  

1. Suchen Sie im Feld **Auswählen** nach „hpc“.  Diese Zeichenfolge sollte mit einem Dienstprinzipal mit dem Namen "HPC Cache-Ressourcenanbieter" übereinstimmen. Klicken Sie auf diesen Prinzipal, um ihn auszuwählen.

   > [!NOTE]
   > Wenn eine Suche nach „hpc“ nicht funktioniert, versuchen Sie stattdessen, die Zeichenfolge „storagecache“ zu verwenden. Benutzer, die frühzeitig an der Vorschau teilgenommen haben, müssen möglicherweise den älteren Namen für den Dienstprinzipal verwenden.

1. Klicken Sie auf die Schaltfläche **Speichern**, um dem Speicherkonto die Rollenzuweisung hinzuzufügen.

1. Wiederholen Sie diesen Vorgang, um die Rolle "Mitwirkender an Storage-Blobdaten" zuzuweisen.  

![Screenshot des GUI zum Hinzufügen von Rollenzuweisungen](media/hpc-cache-add-role.png)

## <a name="add-a-new-nfs-storage-target"></a>Hinzufügen eines neuen NFS-Speicherziels

Ein NFS-Speicherziel weist einige zusätzliche Felder auf, um anzugeben, wie auf den Speicherexport zugegriffen werden soll und wie seine Daten effizient gespeichert werden. Darüber hinaus können Sie mehrere Namespacepfade von einem NFS-Host angeben, wenn für ihn mehrere Exporte verfügbar sind.

![Screenshot der Seite zum Hinzufügen von Speicherzielen mit definiertem NFS-Ziel](media/hpc-cache-add-nfs-target.png)

Geben Sie diese Informationen für ein NFS-gestütztes Speicherziel an:

* **Name des Speicherziels**: Legen Sie einen Namen fest, der dieses Speicherziel im Azure HPC Cache identifiziert.

* **Zieltyp**: Wählen Sie **NFS** aus.

* **Hostname**: Geben Sie die IP-Adresse oder den vollqualifizierten Domänennamen Ihres NFS-Speichersystems ein. (Verwenden Sie nur dann einen Domänennamen, wenn Ihr Cache Zugriff auf einen DNS-Server hat, der den Namen auflösen kann.)

* **Nutzungsmodell**: Wählen Sie auf der Grundlage Ihres Workflows eins der Datencacheprofile aus, die unten in [Auswählen eines Nutzungsmodells](#choose-a-usage-model) beschrieben sind.

### <a name="nfs-namespace-paths"></a>NFS-Namespacepfade

Ein NFS-Speicherziel kann mehrere virtuelle Pfade aufweisen, solange jeder Pfad ein anderes Export- oder Unterverzeichnis in demselben Speichersystem darstellt.

Erstellen Sie alle Pfade von einem Speicherziel aus.
<!-- You can create multiple namespace paths to represent different exports on the same NFS storage system, but you must create them all from one storage target. -->

Geben Sie diese Werte für die einzelnen Namespacepfade ein:

* **Pfad des virtuellen Namespace**: Legen Sie den clientseitigen Dateipfad für dieses Speicherziel fest. Weitere Informationen zum Feature virtueller Namespace finden Sie unter [Aggregierten Namespace konfigurieren](hpc-cache-namespace.md).

<!--  The virtual path should start with a slash ``/``. -->

* **NFS-Exportpfad**: Geben Sie den Pfad zum NFS-Export ein.

* **Unterverzeichnispfad**: Wenn Sie ein bestimmtes Unterverzeichnis des Exports einbinden möchten, geben Sie es hier ein. Lassen Sie dieses Feld andernfalls leer.

Klicken Sie abschließend auf **OK**, um das Speicherziel hinzuzufügen.

### <a name="choose-a-usage-model"></a>Auswählen eines Nutzungsmodells
<!-- referenced from GUI - update aka.ms link if you change this heading -->

Wenn Sie ein Speicherziel erstellen, das auf ein NFS-Speichersystem verweist, müssen Sie das *Nutzungsmodell* für dieses Ziel auswählen. Dieses Modell bestimmt, wie Ihre Daten zwischengespeichert werden.

* Leseintensiv: Wenn Sie den Cache größtenteils zum Beschleunigen des Lesezugriffs für Daten verwenden, wählen Sie diese Option aus.

* Lese-/Schreibzugriff: Wenn Clients den Cache zum Lesen und Schreiben verwenden, wählen Sie diese Option aus.

* Clients umgehen den Cache: Wählen Sie diese Option aus, wenn Ihre Clients Daten direkt in das Speichersystem schreiben, ohne zuvor in den Cache zu schreiben.

## <a name="next-steps"></a>Nächste Schritte

Ziehen Sie nach dem Erstellen von Speicherzielen eine dieser Aufgaben in Erwägung:

* [Einbinden der Azure HPC Cache-Instanz](hpc-cache-mount.md)
* [Daten in Azure-Blobspeicher verschieben](hpc-cache-ingest.md)
