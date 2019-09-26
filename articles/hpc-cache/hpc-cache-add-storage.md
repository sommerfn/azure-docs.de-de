---
title: Hinzufügen von Speicher zu einem Azure HPC Cache
description: Definieren von Speicherzielen, damit Ihr Azure HPC Cache Ihr lokales NFS-System oder Azure-Blobcontainer für die langfristige Speicherung von Dateien verwenden kann
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: ca8e13e322c3e192b697248f1252b65f6cbeda7f
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2019
ms.locfileid: "71037212"
---
# <a name="add-storage"></a>Hinzufügen von Speicher

*Speicherziele* sind Back-End-Speicher für Dateien, auf die über eine Azure HPC Cache-Instanz zugegriffen wird. Sie können NFS-Speicher hinzufügen, z. B. ein lokales Hardwaresystem, oder Daten im Azure-Blob speichern.

Es können bis zu zehn verschiedene Speicherziele für einen Cache definiert werden. Der Cache stellt alle Speicherziele in einem aggregierten Namespace dar.

Beachten Sie, dass der Zugriff auf die Speicherexporte vom virtuellen Netzwerk Ihres Caches aus möglich sein muss. Für einen lokalen Hardwarespeicher müssen Sie möglicherweise einen DNS-Server einrichten, der Hostnamen für den NFS-Speicherzugriff auflösen kann. Weitere Informationen finden Sie unter [DNS-Zugriff](hpc-cache-prereqs.md#dns-access).

Sie können Speicherziele beim Erstellen Ihres Azure HPC Caches hinzufügen oder später. Das Verfahren unterscheidet sich geringfügig, je nachdem, ob Sie Azure Blobspeicher oder einen NFS-Export hinzufügen. Details zu beiden Szenarien finden Sie unten.

## <a name="add-storage-targets-while-creating-the-cache"></a>Hinzufügen von Speicherzielen beim Erstellen des Caches

Verwenden Sie die Registerkarte **Speicherziele** des Assistenten zur Cache-Erstellung, um Speicher zu definieren, während Sie die Cache-Instanz erstellen.

![Screenshot der Seite „Speicherziele“](media/hpc-cache-storage-targets-pop.png)

Klicken Sie auf den Link **Speicherziel hinzufügen**, um Speicher hinzuzufügen.

## <a name="add-storage-targets-from-the-cache"></a>Hinzufügen von Speicherzielen aus dem Cache

Öffnen Sie im Azure-Portal Ihre Cache-Instanz, und klicken Sie in der linken Seitenleiste auf **Speicherziele**. Auf der Seite „Speicherziel“ werden alle vorhandenen Ziele und ein Link zum Hinzufügen eines neuen Ziels aufgelistet.

![Screenshot des Links „Speicherziele“ in der Randleiste unter der Überschrift „Konfigurieren“, die sich zwischen den Kategorieüberschriften „Einstellungen“ und „Überwachung“ befindet](media/hpc-cache-storage-targets-sidebar.png)

## <a name="add-a-new-azure-blob-storage-target"></a>Hinzufügen eines neuen Azure Blobspeicherziels

Für ein neues Blobspeicherziel ist ein leerer Blobcontainer oder ein Container erforderlich, der mit Daten im Format des Azure HPC Cache-Clouddateisystems aufgefüllt ist. Weitere Informationen zum Vorabladen eines Blobcontainers finden Sie unter [Daten in Azure-Blobspeicher verschieben](hpc-cache-ingest.md).

Geben Sie diese Informationen ein, um einen Azure Blobcontainer zu definieren.

![Screenshot der Seite „Speicherziel hinzufügen“, aufgefüllt mit Informationen für ein neues Azure-Blobspeicherziel](media/hpc-cache-add-blob.png)

* **Name des Speicherziels**: Legen Sie einen Namen fest, der dieses Speicherziel im Azure HPC Cache identifiziert.
* **Zieltyp**: Wählen Sie **Blob** aus.
* **Speicherkonto**: Wählen Sie das Konto mit dem Container aus, auf den verwiesen werden soll.

  Sie müssen die Cache-Instanz für den Zugriff auf das Speicherkonto autorisieren, wie unter [Hinzufügen der Zugriffsrollen](#add-the-access-control-roles-to-your-account) beschrieben.
* **Speichercontainer**: Wählen Sie den Blobcontainer für dieses Ziel aus.

* **Pfad des virtuellen Namespace**: Legen Sie den clientseitigen Dateipfad für dieses Speicherziel fest. Weitere Informationen zum Feature virtueller Namespace finden Sie unter [Aggregierten Namespace konfigurieren](hpc-cache-namespace.md).

Klicken Sie abschließend auf **OK**, um das Speicherziel hinzuzufügen.

### <a name="add-the-access-control-roles-to-your-account"></a>Hinzufügen der Zugriffssteuerungsrollen zu Ihrem Konto

Azure HPC Cache verwendet [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](https://docs.microsoft.com/azure/role-based-access-control/index), um die Cache-Anwendung für den Zugriff auf Ihr Speicherkonto für Azure-Blobspeicherziele zu autorisieren.

Der Besitzer des Speicherkontos muss die Rollen [Speicherkontomitwirkender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-contributor) und [Mitwirkender an Storage-Blobdaten](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor) für den Benutzer "StorageCache-Ressourcenanbieter" explizit hinzufügen.

Sie können dies im Voraus erledigen oder indem Sie auf einen Link auf der Seite klicken, auf der Sie ein Blobspeicherziel hinzufügen.

Schritte zum Hinzufügen der RBAC-Rollen:

1. Öffnen Sie die Seite **Zugriffssteuerung (IAM)** für das Speicherkonto. (Der Link auf der Seite **Speicherziel hinzufügen** öffnet diese Seite für das ausgewählte Konto automatisch.)

1. Klicken Sie auf die **+** oben auf der Seite, und wählen Sie **Rollenzuweisung hinzufügen** aus.

1. Wählen Sie in der Liste die Rolle "Speicherkontomitwirkender" aus.

1. Übernehmen Sie im Feld **Zugriff zuweisen zu** den Standardwert ("Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal").  

1. Suchen Sie im Feld **Auswählen** nach "storagecache".  Diese Zeichenfolge sollte mit einem Sicherheitsprinzipal mit dem Namen "HPC Cache-Ressourcenanbieter" übereinstimmen. Klicken Sie auf diesen Prinzipal, um ihn auszuwählen.

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

Sie können mehrere Namespacepfade erstellen, die verschiedene Exporte im gleichen NFS-Speichersystem darstellen, Sie müssen sie aber alle aus dem gleichen Speicherziel erstellen.

Geben Sie für jeden Export folgende Werte an:

* **Pfad des virtuellen Namespace**: Legen Sie den clientseitigen Dateipfad für dieses Speicherziel fest. Weitere Informationen zum Feature virtueller Namespace finden Sie unter [Aggregierten Namespace konfigurieren](hpc-cache-namespace.md).

<!--  The virtual path should start with a slash ``/``. -->

* **NFS-Exportpfad**: Geben Sie den Pfad zum NFS-Export ein.

* **Unterverzeichnispfad**: Wenn Sie ein bestimmtes Unterverzeichnis des Exports einbinden möchten, geben Sie es hier ein. Lassen Sie dieses Feld andernfalls leer. 

Klicken Sie abschließend auf **OK**, um das Speicherziel hinzuzufügen.

### <a name="choose-a-usage-model"></a>Auswählen eines Nutzungsmodells 
<!-- link in GUI to this heading -->

Wenn Sie ein Speicherziel erstellen, das auf ein NFS-Speichersystem verweist, müssen Sie das *Nutzungsmodell* für dieses Ziel auswählen. Dieses Modell bestimmt, wie Ihre Daten zwischengespeichert werden.

* Leseintensiv: Wenn Sie den Cache größtenteils zum Beschleunigen des Lesezugriffs für Daten verwenden, wählen Sie diese Option aus. 

* Lese-/Schreibzugriff: Wenn Clients den Cache zum Lesen und Schreiben verwenden, wählen Sie diese Option aus.

* Clients umgehen den Cache: Wählen Sie diese Option aus, wenn Ihre Clients Daten direkt in das Speichersystem schreiben, ohne zuvor in den Cache zu schreiben.

## <a name="next-steps"></a>Nächste Schritte

Ziehen Sie nach dem Erstellen von Speicherzielen eine dieser Aufgaben in Erwägung:

* [Einbinden der Azure HPC Cache-Instanz](hpc-cache-mount.md)
* [Daten in Azure-Blobspeicher verschieben](hpc-cache-ingest.md)