---
title: Schnellstart zur VMware-Lösung von CloudSimple – Nutzen von VMware-VMs in Azure
description: Erfahren Sie, wie Sie VMware-VMs aus dem Azure-Portal mit der Azure-VMware-Lösung von CloudSimple konfigurieren und nutzen können.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 504550358bb56602093e58c90506c9140afccadb
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69574491"
---
# <a name="quickstart---consume-vmware-vms-on-azure"></a>Schnellstart – Nutzen von VMware-VMs in Azure

Um einen virtuellen Computer im Azure-Portal zu erstellen, verwenden Sie VM-Vorlagen, die Ihr CloudSimple-Administrator für Ihr Abonnement aktiviert hat. Die VM-Vorlagen finden Sie in der VMware-Infrastruktur.

## <a name="cloudsimple-vm-creation-on-azure-requires-a-vm-template"></a>Für die CloudSimple-VM-Erstellung in Azure ist eine VM-Vorlage erforderlich.

Erstellen Sie einen virtuellen Computer über die vCenter-Benutzeroberfläche in Ihrer privaten Cloud. Um eine Vorlage zu erstellen, folgen Sie den Anweisungen unter [Klonen eines virtuellen Geräts in eine Vorlage im vSphere-Webclient](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). Speichern Sie die VM-Vorlage in vCenter der privaten Cloud.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Erstellen eines virtuellen Computers im Azure-Portal

1. Wählen Sie **Alle Dienste** aus.

2. Suchen Sie nach **CloudSimple-VMs**.

3. Klicken Sie auf **Hinzufügen**.

    ![Erstellen einer CloudSimple-VM](media/create-cloudsimple-virtual-machine.png)

4. Geben Sie grundlegende Informationen ein, und klicken Sie auf **Next:Size**.

    ![Erstellen einer CloudSimple-VM – Grundlagen](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Feld | BESCHREIBUNG |
    | ------------ | ------------- |
    | Subscription | Das mit Ihrer privaten Cloud verknüpfte Azure-Abonnement.  |
    | Ressourcengruppe | Die Ressourcengruppe, der die VM zugewiesen wird. Wählen Sie eine vorhandene Gruppe auswählen oder eine neue erstellen. |
    | NAME | Der Name zur Identifizierung der VM.  |
    | Location | Azure-Region, in der die VM gehostet wird.  |
    | Private Cloud | Die private CloudSimple-Cloud, in der Sie den virtuellen Computer erstellen möchten. |
    | Ressourcenpool | Zugeordneter Ressourcenpool für die VM. Wählen Sie aus den verfügbaren Ressourcenpools aus. |
    | vSphere-Vorlage | vSphere-Vorlage für die VM.  |
    | Benutzername | Benutzername des VM-Administrators (für Windows-Vorlagen).|
    | Kennwort |  Kennwort des VM-Administrators (für Windows-Vorlagen). |
    | Kennwort bestätigen | Bestätigen Sie das Kennwort. |

5. Wählen Sie die Anzahl der Kerne und die Speicherkapazität für die VM und klicken Sie auf **Weiter:Konfigurationen**. Aktivieren Sie das Kontrollkästchen, wenn Sie die vollständige CPU-Virtualisierung für das Gastbetriebssystem bereitstellen möchten. Anwendungen, die eine Hardwarevirtualisierung erfordern, können auf virtuellen Computern ohne binäre Übersetzung oder Paravirtualisierung ausgeführt werden. Weitere Informationen finden Sie im VMware-Artikel <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">Verfügbarmachen der hardwareunterstützten VMware-Virtualisierung</a>.

    ![Erstellen einer CloudSimple-VM – Größe](media/create-cloudsimple-virtual-machine-size.png)

6. Konfigurieren Sie Netzwerkschnittstellen und Datenträger wie in den folgenden Tabellen beschrieben und klicken Sie auf **Überprüfen + Erstellen**.

    ![Erstellen einer CloudSimple-VM – Konfigurationen](media/create-cloudsimple-virtual-machine-configurations.png)

    Klicken Sie bei Netzwerkschnittstellen auf **Netzwerkschnittstelle hinzufügen**, und konfigurieren Sie die folgenden Einstellungen.

    | Kontrolle | BESCHREIBUNG |
    | ------------ | ------------- |
    | NAME | Geben Sie einen Namen zur Identifizierung der Schnittstelle ein.  |
    | Netzwerk | Wählen Sie aus der Liste der konfigurierten verteilten Portgruppen in Ihrer Private Cloud vSphere aus.  |
    | Adapter | Wählen Sie einen vSphere-Adapter aus der Liste der verfügbaren Typen, die für die VM konfiguriert sind. Weitere Informationen finden Sie im VMware-Knowledge Base-Artikel <a href="https://kb.vmware.com/s/article/1001805" target="_blank">Auswählen eines Netzwerkadapters für Ihren virtuellen Computer</a>. |
    | Einschalten beim Starten | Wählen Sie, ob die NIC-Hardware beim Booten der VM aktiviert werden soll. Die Standardeinstellung ist **Aktiviert**. |

    Klicken Sie bei Datenträgern auf **Datenträger hinzufügen** und konfigurieren Sie die folgenden Einstellungen.

    | Item | BESCHREIBUNG |
    | ------------ | ------------- |
    | NAME | Geben Sie einen Namen zur Identifizierung des Datenträgers ein.  |
    | Size | Wählen Sie eine der verfügbaren Größen aus.  |
    | SCSI-Controller | Wählen Sie einen SCSI-Controller für den Datenträger aus.  |
    | Mode | Bestimmt, wie der Datenträger in Momentaufnahmen beteiligt ist. Wählen Sie eine der Optionen aus: <br> - Unabhängig dauerhaft: Alle Daten, die auf den Datenträger geschrieben werden, werden dauerhaft geschrieben.<br> - Unabhängig nicht dauerhaft: Auf dem Datenträger geschriebene Änderungen werden verworfen, wenn Sie den Computer ausschalten oder zurücksetzen.  Der unabhängige, nicht dauerhafte Modus ermöglicht es Ihnen, die VM immer im gleichen Zustand neu zu starten. Weitere Informationen finden Sie in der <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">VMware-Dokumentation</a>.

7. Wenn die Überprüfung abgeschlossen ist, überprüfen Sie die Einstellungen, und klicken Sie dann auf **Create** (Erstellen). Um Änderungen vorzunehmen, klicken Sie auf die Registerkarten oben.

    ![Erstellen einer CloudSimple-VM – Überprüfung](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Nächste Schritte

* [Anzeigen der Liste der virtuelle CloudSimple-VMs](azure-create-vm.md#view-list-of-cloudsimple-virtual-machines)
* [Verwalten eines virtuellen CloudSimple-Computers aus Azure](azure-manage-vm.md)
