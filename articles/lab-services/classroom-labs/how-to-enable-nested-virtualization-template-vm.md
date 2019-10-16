---
title: Aktivieren der geschachtelten Virtualisierung auf einem virtuellen Vorlagencomputer in Azure Lab Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die geschachtelte Virtualisierung auf einem virtuellen Vorlagencomputer in Azure Lab Services aktivieren.
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
ms.openlocfilehash: 9a52fd958d646af5edd09065e9acf95796c8ce33
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981934"
---
# <a name="enable-nested-virtualization-on-a-template-virtual-machine-in-azure-lab-services"></a>Aktivieren der geschachtelten Virtualisierung auf einem virtuellen Vorlagencomputer in Azure Lab Services
Dieser Artikel behandelt die Vorgehensweise zum Einrichten der geschachtelten Virtualisierung auf einem virtuellen Vorlagencomputer in Azure Lab Services. Geschachtelte Virtualisierung wird in Azure Lab Services verwendet, wenn jeder Teilnehmer eines Kurses mehrere Computer benötigt.
 
## <a name="considerations"></a>Überlegungen
Vor dem Einrichten eines Labs mit geschachtelter Virtualisierung sind folgende Punkte zu berücksichtigen.

- Wenn Sie ein neues Lab erstellen, wählen Sie als Größe des virtuellen Computers **Mittel (geschachtelte Virtualisierung)** oder **Groß** aus. Diese Größen virtueller Computer unterstützen geschachtelte Virtualisierung. 
- Wählen Sie eine Größe aus, die sowohl für den Host als auch für den virtuellen Clientcomputer gute Leistung bereitstellt.  Denken Sie daran, dass bei der Verwendung von Virtualisierung die Größe, die Sie auswählen, nicht nur für einen Computer ausreichend sein muss, sondern sowohl für den Host als auch für alle Clientcomputer, die gleichzeitig ausgeführt werden sollen.
- Virtuelle Clientcomputer haben keinen Zugriff auf Azure-Ressourcen, z. B. DNS-Server im virtuellen Azure-Netzwerk.
- Beim virtuellen Hostcomputer muss die Einrichtung berücksichtigen, dass der Clientcomputer eine Internetverbindung besitzen muss. 
- Virtuelle Clientcomputer werden als unabhängige Computer lizenziert. Informationen zur Lizenzierung für Microsoft-Betriebssysteme und -Produkte finden Sie unter [Microsoft-Lizenzierung](https://www.microsoft.com/licensing/default). Überprüfen Sie die Lizenzverträge für jegliche andere verwendete Software, bevor Sie den Vorlagencomputer einrichten.

## <a name="enable-nested-virtualization-on-a-template-vm"></a>Aktivieren der geschachtelten Virtualisierung auf einem virtuellen Vorlagencomputer
Die Schritte in diesem Abschnitt konzentrieren sich auf das Einrichten der geschachtelten Virtualisierung für Windows Server 2016 bzw. Windows Server 2019. Sie verwenden dabei ein Skript, um den Vorlagencomputer mit Hyper-V einzurichten. Eine automatisierte Lösung finden Sie in den Skripts unter [Lab-Dienste Hyper-V-Skripts](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV). Die folgenden Schritte führen Sie durch die Verwendung des Skripts.

1. Wenn Sie Internet Explorer verwenden, müssen Sie der Liste vertrauenswürdiger Sites möglicherweise `https://github.com` hinzufügen. 
    1. Öffnen Sie Internet Explorer.
    1. Wählen Sie das Zahnradsymbol aus, und wählen Sie **Internetoptionen**.  
    1. Wenn das Dialogfeld **Internetoptionen** angezeigt wird, wählen Sie **Sicherheit**, **Vertrauenswürdige Sites** aus, und klicken Sie auf die Schaltfläche **Sites**.
    1. Wenn das Dialogfeld "**Vertrauenswürdige Sites** angezeigt wird, fügen Sie der Liste vertrauenswürdiger Websites `https://github.com` hinzu, und wählen Sie **Schließen** aus.

        ![Vertrauenswürdige Sites](../media/how-to-enable-nested-virtualization-template-vm/trusted-sites-dialog.png)
1.  Laden Sie die Git-Repositorydateien herunter, wie in den folgenden Schritten beschrieben.  Alternativ kann das Git-Repository von [https://github.com/Azure/azure-devtestlab.git](https://github.com/Azure/azure-devtestlab.git) geklont werden. 
    1. Wechseln Sie zu [https://github.com/Azure/azure-devtestlab/](https://github.com/Azure/azure-devtestlab/).
    1. Klicken Sie auf die Schaltfläche **Klonen oder herunterladen**.
    1. Klicken Sie auf **ZIP herunterladen**.
    1. Extrahieren Sie die ZIP-Datei.
1. Starten Sie die **PowerShell** im **Administrator**modus.
1. Navigieren Sie im PowerShell-Fenster zu dem Ordner mit dem heruntergeladenen Skript. Wenn Sie vom obersten Ordner der Repositorydateien aus navigieren, befindet sich das Skript in `azure-devtestlab\samples\ClassroomLabs\Scripts\HyperV\`.
1. Möglicherweise müssen Sie die Ausführungsrichtlinie ändern, damit das Skript erfolgreich ausgeführt wird. Führen Sie den folgenden Befehl aus:
    
    ```powershell
    Set-ExecutionPolicy bypass -force 
    ```
1. Ausführen des Skripts:
    
    ```powershell
    .\SetupForNestedVirtualization.ps1
    ```

    > [!NOTE]
    > Das Skript erfordert möglicherweise, dass der Computer neu gestartet wird. Befolgen Sie die Anweisungen des Skripts, und führen Sie das Skript erneut aus, bis in der Ausgabe **Skript abgeschlossen** angezeigt wird.
1. Vergessen Sie nicht, die Ausführungsrichtlinie zurückzusetzen. Führen Sie den folgenden Befehl aus: 

    ```powershell
    Set-ExecutionPolicy default -force
    ```

## <a name="conclusion"></a>Zusammenfassung
Jetzt ist Ihr Vorlagencomputer bereit, um virtuelle Hyper-V-Computer zu erstellen. Anweisungen zum Erstellen von virtuellen Hyper-V-Computern finden Sie unter [Erstellen eines virtuellen Computers in Hyper-V](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v). Verfügbare Betriebssysteme und Software finden Sie außerdem im [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/).  

## <a name="next-steps"></a>Nächste Schritte 
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Aktivieren der geschachtelten Virtualisierung auf einer Azure-VM](../../virtual-machines/windows/nested-virtualization.md) 
- [Installieren der Hyper-V-Rolle auf Windows Server](/windows-server/virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server)
- [Lab-Dienste Hyper-V-Skripts](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Scripts/HyperV)
