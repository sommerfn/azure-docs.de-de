---
title: Einrichten eines Labs für ethisches Hacken mit Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Azure Lab Services ein Lab einrichten, um ethisches Hacken zu vermitteln.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2019
ms.author: spelluru
ms.openlocfilehash: df24f846f1600685803fdd485f1810d66e32ae37
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72028676"
---
# <a name="set-up-a-lab-to-teach-ethical-hacking-class"></a>Einrichten eines Labs zum Vermitteln von Kursen für ethisches Hacken 
In diesem Artikel erfahren Sie, wie Sie einen Kurs einrichten, der sich auf die forensische Seite des ethischen Hackens konzentriert. Bei Penetrationstests, eine von der Community für ethisches Hacken verwendete Vorgehensweise, versucht eine Person, auf das System oder Netzwerk zuzugreifen, um Schwachstellen zu demonstrieren, die ein böswilliger Angreifer ausnutzen könnte. 

In einem Kurs für ethisches Hacken erlernen die Kursteilnehmer moderne Techniken zum Schützen vor Schwachstellen kennen. Jeder Kursteilnehmer erhält einen virtuellen Windows Server-Hostcomputer mit zwei geschachtelten virtuellen Computern: einem virtuellen Computer mit einem **Metaspoiltable**-Image und einem anderen Computer mit einem [Kali Linux](https://www.kali.org/)-Image. Der virtuelle Metasploitable-Computer wird angegriffen, und der virtuelle Kali-Computer stellt die Tools zur Verfügung, die zum Ausführen forensischer Aufgaben erforderlich sind.

Dieser Artikel besteht aus zwei Hauptteilen. Im ersten Abschnitt wird erläutert, wie das Kurs-Lab erstellt wird. Im zweiten Abschnitt wird erläutert, wie der Vorlagencomputer mit aktivierter geschachtelter Virtualisierung sowie mit den benötigten Tools und Images erstellt wird. In diesem Fall ein Metasploitable-Image und ein Kali Linux-Image auf einem Computer, auf dem Hyper-V zum Hosten der Images aktiviert ist.

## <a name="lab-configuration"></a>Labkonfiguration
Zum Einrichten dieses Labs benötigen Sie als Einstieg ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Sobald Sie ein Azure-Abonnement erhalten, können Sie entweder ein neues Lab-Konto in Azure Lab Services erstellen oder ein vorhandenes Konto verwenden. Sehen Sie sich das folgende Tutorial zum Erstellen eines neuen Lab-Kontos an: [Tutorial zum Einrichten eines Lab-Kontos](tutorial-setup-lab-account.md).

Absolvieren Sie [dieses Tutorial](tutorial-setup-classroom-lab.md), um ein neues Lab mit den folgenden Einstellungen zu erstellen:

| Größe des virtuellen Computers | Image |
| -------------------- | ----- | 
| Mittel (geschachtelte Virtualisierung) | Windows Server 2019 Datacenter |

## <a name="template-machine"></a>Vorlagencomputer 

Nachdem der Vorlagencomputer erstellt wurde, starten Sie den Computer, und stellen Sie eine Verbindung mit ihm her, um die folgenden drei Hauptaufgaben auszuführen. 
 
1. Richten Sie den Computer für geschachtelte Virtualisierung ein. Hierdurch werden alle geeigneten Windows-Funktionen wie Hyper-V aktiviert und das Netzwerke für die Hyper-V-Images eingerichtet, damit sie miteinander und mit dem Internet kommunizieren können.
2. Richten Sie das [Kali](https://www.kali.org/) Linux-Image ein. Kali ist eine Linux-Distribution, die Tools für Penetrationstests und Sicherheitsüberwachung umfasst.
3. Richten Sie das Metasploitable-Image ein. In diesem Beispiel wird das [Metasploitable3](https://github.com/rapid7/metasploitable3)-Image verwendet. Dieses Image wurde vorsätzlich so erstellt, dass es Schwachstellen enthält.

### <a name="prepare-template-machine-for-nested-virtualization"></a>Vorbereiten des Vorlagencomputers für geschachtelte Virtualisierung
Befolgen Sie die Anweisungen in [diesem Artikel](how-to-enable-nested-virtualization-template-vm.md), um ihren virtuellen Vorlagencomputer für geschachtelte Virtualisierung vorzubereiten. 

### <a name="set-up-a-nested-virtual-machine-with-kali-linux-image"></a>Einrichten eines geschachtelten virtuellen Computers mit dem Kali Linux-Image
Kali ist eine Linux-Distribution, die Tools für Penetrationstests und Sicherheitsüberwachung umfasst.

1. Laden Sie das Image unter[https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/](https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/) herunter.  
    1. Laden Sie **Kali Linux Hyper-V 64 Bit** für Hyper-V herunter.
    1. Extrahieren Sie die .7z-Datei.  Wenn Sie 7 zip noch nicht besitzen, laden Sie es unter [https://www.7-zip.org/download.html](https://www.7-zip.org/download.html) herunter. Merken Sie sich den Speicherort des extrahierten Ordners, da Sie ihn später noch benötigen.
2. Öffnen Sie **Hyper-V-Manager** aus den Verwaltungstools.
1. Wählen Sie **Aktion** aus und dann **Virtuellen Computer importieren**. 
1. Wählen Sie auf der Seite **Ordner suchen** des Assistenten **Virtuellen Computer importieren** den Speicherort des extrahierten Ordners aus, der das Kali Linux-Image enthält.

    ![Dialogfeld „Ordner suchen“](../media/class-type-ethical-hacking/locate-folder.png)
1. Wählen Sie auf der Seite **Virtuellen Computer importieren** das Kali Linux-Image aus.  In diesem Fall ist das Image **kali-linux-2019.3-hyperv**.

    ![Auswählen des Kali-Images](../media/class-type-ethical-hacking/select-kali-image.png)
1.  Wählen Sie auf der Seite **Importtyp auswählen** den Eintrag **Virtuellen Computer kopieren (neue eindeutige ID erstellen)** aus.

    ![Auswählen des Importtyps](../media/class-type-ethical-hacking/choose-import-type.png)
1. Akzeptieren Sie die Standardeinstellungen auf den Seiten **Ordner für virtuelle Computerdateien auswählen** und **Ordner zum Speichern von virtuellen Festplatten**, und wählen Sie dann **Weiter** aus.
1. Wählen Sie auf der Seite **Netzwerk verbinden** den Eintrag **LabServicesSwitch** aus, den Sie zuvor im Abschnitt **Vorbereiten des Vorlagencomputers für geschachtelte Virtualisierung** in diesem Artikel erstellt haben, und wählen Sie dann **Weiter** aus.

    ![Seite „Netzwerk verbinden“](../media/class-type-ethical-hacking/connect-network.png)
1. Wählen Sie auf der Seite **Zusammenfassung** den Befehl **Fertig stellen** aus. Warten Sie, bis die Kopier- und Importvorgänge abgeschlossen sind. Der virtuelle Kali Linux-Computer ist nun in Hyper-V verfügbar.
1. Wählen Sie im **Hyper-V-Manager** **Aktion** -> **Starten** und dann **Aktion** -> **Verbinden** aus, um eine Verbindung mit dem virtuellen Computer herzustellen.  
12. Der Standardbenutzername ist `root`, und das Kennwort ist `toor`. 

    > [!NOTE]
    > Wenn Sie das Image entsperren müssen, drücken Sie die STRG-Taste, und ziehen Sie den Mauszeiger nach oben.

## <a name="set-up-a-nested-vm-with-metasploitable-image"></a>Einrichten eines geschachtelten virtuellen Computers mit dem Metasploitable-Image  
Das Rapid7 Metasploitable-Image ist ein Image, das absichtlich mit Schwachstellen konfiguriert ist. Sie verwenden dieses Image zum Testen und Ermitteln von Problemen. In den folgenden Anweisungen erfahren Sie, wie Sie ein vorgefertigtes Metasploitable-Image verwenden. Wenn jedoch eine neuere Version des Metasploitable-Images erforderlich ist, sehen Sie unter [https://github.com/rapid7/metasploitable3](https://github.com/rapid7/metasploitable3) nach.

1. Navigieren Sie zu [https://information.rapid7.com/download-metasploitable-2017.html](https://information.rapid7.com/download-metasploitable-2017.html). Füllen Sie das Formular zum Herunterladen des Images aus, und klicken Sie auf die Schaltfläche **Absenden**.
1. Wählen Sie die Schaltfläche **Metasploitable jetzt herunterladen** aus.
1. Wenn die ZIP-Datei heruntergeladen ist, extrahieren Sie sie, und merken Sie sich den Speicherort.
1. Konvertieren Sie die extrahierte VMDK-Datei in eine VHDX-Datei, damit Sie sie mit Hyper-V verwenden können. Öffnen Sie hierzu PowerShell mit Administratorrechten, navigieren Sie zu dem Ordner, in dem sich die VMDK-Datei befindet, und befolgen Sie diese Anweisungen:
    1. Laden Sie den [Microsoft Virtual Machine Converter](https://www.microsoft.com/download/details.aspx?id=42497) herunter, und führen Sie die Datei „mvmc_setup.msi“ aus, wenn Sie dazu aufgefordert werden.
    1. Importieren Sie das PowerShell-Modul.  Der Standardspeicherort, an dem das Modul installiert wird, lautet „C:\Programme\Microsoft Virtual Machine Converter\“.

        ```powershell
        Import-Module 'C:\Program Files\Microsoft Virtual Machine Converter\MvmcCmdlet.psd1'
        ```
    1. Konvertieren Sie die VMDK-Datei in eine VHD-Datei, die von Hyper-V verwendet werden kann. Dieser Vorgang kann einige Minuten dauern.
    
        ```powershell
        ConvertTo-MvmcVirtualHardDisk -SourceLiteralPath .\Metasploitable.vmdk -DestinationLiteralPath .\Metasploitable.vhdx -VhdType DynamicHardDisk -VhdFormat vhdx
        ```
    1. Kopieren Sie die neu erstellte „metasploitable.vhdx“-Datei in „C:\Benutzer\Öffentlich\Dokumente\Hyper-V\Virtual Hard Disks\“. 
1. Erstellen Sie einen neuen virtuellen Hyper-V-.Computer.
    1. Öffnen Sie den **Hyper-V-Manager**.
    1. Wählen Sie **Aktion** -> **Neu** -> **Virtueller Computer** aus.
    1. Klicken Sie auf der Seite **Vorbereitung** des **Assistenten für neue virtuelle Computer** auf **Weiter**.
    1. Geben Sie auf der Seite **Namen und Speicherort angeben**die **Metasploitable** als **Name**n ein, und wählen Sie **Weiter** aus.

        ![Assistent für neue VM-Images](../media/class-type-ethical-hacking/new-vm-wizard-1.png)
    1. Übernehmen Sie auf der Seite **Generation angeben** die Standardeinstellungen, und wählen Sie **Weiter** aus.
    1. Geben Sie auf der Seite **Arbeitsspeicher zuweisen** den Wert **512 MB** als **Startspeicher** ein, und wählen Sie **Weiter** aus. 

        ![Seite „Speicher zuweisen“](../media/class-type-ethical-hacking/assign-memory-page.png)
    1. Lassen Sie auf der Seite **Netzwerk konfigurieren** die Verbindung als **Nicht verbunden**. Den Netzwerkadapter richten Sie später ein.
    1. Wählen Sie auf der Seite **Virtuelle Festplatte verbinden** den Eintrag **Vorhandene virtuelle Festplatte verwenden** aus. Navigieren Sie zum Speicherort der Datei **metasploitable.vhdx**, die Sie im vorherigen Schritt erstellt haben, und wählen Sie **Weiter** aus. 

        ![Seite „Virtuelle Festplatte verbinden“](../media/class-type-ethical-hacking/connect-virtual-network-disk.png)
    1. Wählen Sie auf der Seite zum **Abschließen des Assistenten für neue virtuelle Computer** den Befehl **Fertig stellen** aus.
    1. Sobald der virtuelle Computer erstellt ist, wählen Sie ihn im Hyper-V-Manager aus. Schalten Sie den Computer noch nicht ein.  
    1. Wählen Sie **Aktion** -> **Einstellungen** aus.
    1. Wählen Sie im Dialogfeld **Einstellungen für Metasploitable** die Option **Hardware hinzufügen** aus. 
    1. Wählen Sie **Älterer Netzwerkadapter** aus, und wählen Sie **Hinzufügen** aus.

        ![Seite „Netzwerkadapter“](../media/class-type-ethical-hacking/network-adapter-page.png)
    1. Wählen Sie auf der Seite **Älterer Netzwerkadapter** den Eintrag **LabServicesSwitch** für die Einstellung **Virtueller Switch** aus, und wählen Sie **OK** aus. „LabServicesSwitch“ wurde während der Vorbereitung des Vorlagencomputers für Hyper-V im Abschnitt **Vorbereiten des Vorlagencomputers für geschachtelte Virtualisierung** erstellt.

        ![Seite „Älterer Netzwerkadapter“](../media/class-type-ethical-hacking/legacy-network-adapter-page.png)
    1. Das Metasploitable-Image ist jetzt einsatzbereit. Wählen Sie im **Hyper-V-Manager** **Aktion** -> **Starten** und dann **Aktion** -> **Verbinden** aus, um eine Verbindung mit dem virtuellen Computer herzustellen.  Der Standardbenutzername lautet **msfadmin** und das Kennwort **msfadmin**. 


Die Vorlage wird jetzt aktualisiert und verfügt über Images, die für einen Kurs zu Penetrationstests im Rahmen des ethischen Hackens erforderlich sind, ein Image mit Tools zur Ausführung der Penetrationstests sowie ein weiteres Image mit zu ermittelnden Schwachstellen. Das Vorlagenimage kann jetzt im Kurs veröffentlicht werden. Wählen Sie auf der Vorlagenseite die Schaltfläche **Veröffentlichen** aus, um die Vorlage im Lab zu veröffentlichen.
  

## <a name="cost"></a>Kosten  
Das folgende Beispiel dient der Einschätzung der Lab-Kosten: 
 
Für einen Kurs mit 25 Teilnehmern, 20 planmäßigen Kursstunden und 10 Stunden Hausaufgaben bzw. Arbeitsaufträgen entstünden folgende Kosten für das Lab: 

25 Kursteilnehmer x (20 + 10) Stunden x 55 Lab-Einheiten x 0,01 USD pro Stunde = 412,50 USD. 

Weitere Informationen zu Preisen finden Sie unter [Azure Lab Services-Preise](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="conclusion"></a>Zusammenfassung
In diesem Artikel wurden die Schritte zum Erstellen eines Labs für einen Kurs zum ethischen Hacken erläutert. Er enthält Schritte zum Einrichten der geschachtelten Virtualisierung, um zwei virtuelle Computer auf dem virtuellen Hostcomputer für Penetrationstests zu erstellen.

## <a name="next-steps"></a>Nächste Schritte
Die nächsten Schritte sind die gleichen für sämtliche Labs:

- [Hinzufügen von Benutzern](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kontingent festlegen](tutorial-setup-classroom-lab.md#set-quotas-for-users)
- [Zeitplan festlegen](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Registrierungslinks an Kursteilnehmer senden](tutorial-setup-classroom-lab.md#send-an-email-with-the-registration-link). 

