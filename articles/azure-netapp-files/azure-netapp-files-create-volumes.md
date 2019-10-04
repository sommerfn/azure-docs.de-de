---
title: Erstellen eines NFS-Volumes für Azure NetApp Files | Microsoft-Dokumentation
description: Beschreibt die Vorgehensweise zum Erstellen eines NFS-Volumes für Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 7/9/2019
ms.author: b-juche
ms.openlocfilehash: 45164acd89fc9634d6929bafb35e64a5dc9f2b86
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2019
ms.locfileid: "71178219"
---
# <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Erstellen eines NFS-Volumes für Azure NetApp Files

Azure NetApp Files unterstützt NFS- und SMBv3-Volumes. Der Kapazitätsverbrauch eines Volumes wird mit der bereitgestellten Kapazität des dazugehörigen Pools verrechnet. In diesem Artikel wird veranschaulicht, wie Sie ein NFS-Volume erstellen. Wenn Sie ein SMB-Volume erstellen möchten, lesen Sie unter [Erstellen eines SMB-Volumes für Azure NetApp Files](azure-netapp-files-create-volumes-smb.md) nach. 

## <a name="before-you-begin"></a>Voraussetzungen 
Sie müssen bereits einen Kapazitätspool eingerichtet haben.   
[Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)   
Ein Subnetz muss an Azure NetApp Files delegiert werden.  
[Delegieren eines Subnetzes für Azure NetApp Files](azure-netapp-files-delegate-subnet.md)

## <a name="create-an-nfs-volume"></a>Erstellen eines NFS-Volumes

1.  Klicken Sie auf dem Blatt „Kapazitätspools“ auf das Blatt **Volumes**. 

    ![Navigieren zu Volumes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2.  Klicken Sie auf **+ Volume hinzufügen**, um ein Volume zu erstellen.  
    Das Fenster „Volume erstellen“ wird angezeigt.

3.  Klicken Sie im Fenster „Volume erstellen“ auf **Erstellen**, und geben Sie Informationen für die folgenden Felder an:   
    * **Volumename**      
        Geben Sie den Namen für das Volume an, das Sie erstellen möchten.   

        Ein Volumename muss innerhalb der einzelnen Kapazitätspools eindeutig sein. Er muss mindestens drei Zeichen lang sein. Sie dürfen beliebige alphanumerische Zeichen verwenden.   

        `default` kann nicht als Volumename verwendet werden.

    * **Kapazitätspool**  
        Geben Sie den Kapazitätspool an, in dem das Volume erstellt werden soll.

    * **Kontingent**  
        Geben Sie die Menge an logischem Speicherplatz an, die dem Volume zugewiesen wird.  

        Das Feld **Verfügbares Kontingent** zeigt den ungenutzten Speicherplatz im ausgewählten Kapazitätspool an, den Sie beim Erstellen eines neuen Volumes verwenden können. Die Größe des neuen Volumes darf das verfügbare Kontingent nicht überschreiten.  

    * **Virtuelles Netzwerk**  
        Geben Sie das virtuelle Azure-Netzwerk (VNet) an, von dem aus Sie auf das Volume zugreifen möchten.  

        Das von Ihnen angegebene VNET muss über ein an Azure NetApp Files delegiertes Subnetz verfügen. Auf den Azure NetApp Files-Dienst kann nur vom gleichen VNET aus oder per VNET-Peering von einem VNET aus zugegriffen werden, das sich in der gleichen Region befindet wie das Volume. Sie können auch über ExpressRoute von Ihrem lokalen Netzwerk aus auf das Volume zugreifen.   

    * **Subnetz**  
        Geben Sie das Subnetz an, das Sie für das Volume verwenden möchten.  
        Das von Ihnen angegebene Subnetz muss an Azure NetApp Files delegiert werden. 
        
        Wenn Sie kein Subnetz delegiert haben, klicken Sie auf der Seite „Volume erstellen“ auf **Neu erstellen**. Geben Sie dann auf der Seite „Subnetz erstellen“ die Subnetzinformationen an, und wählen Sie **Microsoft.NetApp/volumes** aus, um das Subnetz für Azure NetApp Files zu delegieren. In jedem VNET kann nur ein Subnetz an Azure NetApp Files delegiert werden.   
 
        ![Erstellen eines Volumes](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Erstellen eines Subnetzes](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Klicken Sie auf **Protokoll**, und wählen Sie dann **NFS** als Protokolltyp für das Volume aus.   
    * Geben Sie den **Dateipfad** zum Erstellen des Exportpfads für das neue Volume an. Der Exportpfad dient zum Einbinden und Zugreifen auf das Volume.

        Der Dateipfadname darf nur Buchstaben, Zahlen und Bindestriche („-“) enthalten. Er muss 16 bis 40 Zeichen umfassen. 

        Der Dateipfad muss innerhalb der einzelnen Abonnements und Regionen eindeutig sein. 

    * Optional können Sie [die Exportrichtlinie für das NFS-Volume konfigurieren](azure-netapp-files-configure-export-policy.md).

    ![Angeben des NFS-Protokolls](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

5. Klicken Sie auf **Bewerten + erstellen**, um die Volumedetails zu überprüfen.  Klicken Sie dann auf **Erstellen**, um das NFS-Volume zu erstellen.

    Das von Ihnen erstellte Volume wird auf der Seite „Volumes“ angezeigt. 
 
    Ein Volume erbt Abonnement-, Ressourcengruppen- und Standortattribute aus dem Kapazitätspool. Den Volumebereitstellungsstatus können Sie auf der Benachrichtigungsregisterkarte überwachen.


## <a name="next-steps"></a>Nächste Schritte  

* [Einbinden oder Aufheben der Einbindung eines Volumes auf virtuellen Windows- oder Linux-Computern](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Konfigurieren der Exportrichtlinie für ein NFS-Volume](azure-netapp-files-configure-export-policy.md)
* [Ressourcenlimits für Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Erfahren Sie mehr über die Integration virtueller Netzwerke für Azure-Dienste](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
