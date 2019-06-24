---
title: Hinzufügen eines Skripts zu einem Wiederherstellungsplan für die Notfallwiederherstellung in Azure Site Recovery | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein VMM-Skript zu einem Wiederherstellungsplan für die Notfallwiederherstellung von Hyper-V-VMs in VMM-Clouds hinzufügen.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: ea6d969ed6612f947e3c73c438738bd98ac2bb30
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2019
ms.locfileid: "64700464"
---
# <a name="add-a-vmm-script-to-a-recovery-plan"></a>Hinzufügen eines VMM-Skripts zu einem Wiederherstellungsplan

Dieser Artikel beschreibt, wie Sie ein System Center Virtual Machine Manager-Skript (VMM) erstellen und einem Wiederherstellungsplan in [Azure Site Recovery](site-recovery-overview.md) hinzufügen.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="prerequisites"></a>Voraussetzungen

Sie können PowerShell-Skripts in Ihren Wiederherstellungsplänen verwenden. Sie müssen das Skript erstellen und in der VMM-Bibliothek ablegen, damit vom Wiederherstellungsplan darauf zugegriffen werden kann. Beachten Sie Folgendes beim Erstellen des Skripts:

* Achten Sie darauf, dass in Skripts try/catch-Blöcke verwendet werden, damit die Ausnahmen ordnungsgemäß behandelt werden.
    - Falls im Skript eine Ausnahme auftritt, wird die Ausführung gestoppt, und für die Aufgabe wird ein Fehler angezeigt.
    - Wenn ein Fehler auftritt, wird der Rest des Skripts nicht mehr ausgeführt.
    - Wenn beim Ausführen eines ungeplanten Failovers ein Fehler auftritt, wird der Wiederherstellungsplan fortgesetzt.
    - Wenn beim Ausführen eines geplanten Failovers ein Fehler auftritt, wird der Wiederherstellungsplan angehalten. Korrigieren Sie das Skript, überprüfen Sie, ob es wie erwartet ausgeführt wird, und führen Sie dann den Wiederherstellungsplan erneut aus.
        - Der Befehl `Write-Host` funktioniert nicht in einem Skript für einen Wiederherstellungsplan. Bei Verwendung des Befehls `Write-Host` in einem Skript führt das Skript zu einem Fehler. Um die Ausgabe zu erstellen, erstellen Sie ein Proxyskript, das wiederum Ihr Hauptskript ausführt. Um sicherzustellen, dass alle Ausgaben weitergeleitet werden, verwenden Sie den Befehl **\>\>** .
        - Die Zeitüberschreitung für das Skript wird erreicht, falls die Rückgabe nicht innerhalb von 600 Sekunden erfolgt.
        - Falls etwas in STDERR geschrieben wird, wird das Skript als nicht erfolgreich klassifiziert. Diese Informationen werden in den Details zur Skriptausführung angezeigt.

* Skripts in einem Wiederherstellungsplan werden im Kontext des VMM-Dienstkontos ausgeführt. Stellen Sie sicher, dass dieses Konto über Leseberechtigungen für die Remotefreigabe verfügt, auf der sich das Skript befindet. Testen Sie das Skript für die Ausführung mit der gleichen Stufe von Benutzerrechten wie das VMM-Dienstkonto.
* VMM-Cmdlets werden in einem Windows PowerShell-Modul bereitgestellt. Das Modul wird installiert, wenn Sie die VMM-Konsole installieren. Sie können das Modul mit dem folgenden Befehl in das Skript laden: 

    `Import-Module -Name virtualmachinemanager`

    Weitere Informationen finden Sie unter [Erste Schritte mit Windows PowerShell und VMM](https://technet.microsoft.com/library/hh875013.aspx).
* Stellen Sie sicher, dass Ihre VMM-Bereitstellung mindestens einen Bibliothekserver enthält. Standardmäßig befindet sich der Bibliotheksfreigabepfad für einen VMM-Server lokal auf dem VMM-Server. Der Ordnername lautet „MSCVMMLibrary“.

  Wenn Ihr Bibliotheksfreigabepfad remote ist (oder lokal, aber nicht für „MSCVMMLibrary“ freigegeben), konfigurieren Sie die Freigabe wie folgt (hier dient „\\libserver2.contoso.com\share\“ als Beispiel):
  
  1. Öffnen Sie den Registrierungs-Editor, und navigieren Sie zu **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Recovery\Registration**.

  1. Ändern Sie den Wert von **ScriptLibraryPath** in **\\\libserver2.contoso.com\share\\** . Geben Sie den vollqualifizierten Domänennamen vollständig ein. Geben Sie die Berechtigungen für den Speicherort der Freigabe an. Dies ist der Stammknoten der Freigabe. Wechseln Sie in VMM in der Bibliothek zum Stammknoten, um diesen zu überprüfen. Der Pfad, der geöffnet wird, ist der Stamm des Pfads. Dies ist der Pfad, den Sie in der Variable verwenden müssen.

  1. Testen Sie das Skript mit einem Benutzerkonto mit der gleichen Stufe von Benutzerrechten wie das VMM-Dienstkonto. Durch die Verwendung dieser Benutzerrechte vergewissern Sie sich, dass eigenständige, getestete Skripts auf die gleiche Weise wie in Wiederherstellungsplänen ausgeführt werden. Legen Sie für die Ausführungsrichtlinie auf dem VMM-Server wie folgt eine Umleitung fest:

     a. Öffnen Sie die **Windows PowerShell-Konsole (64 Bit)** als Administrator.
     
     b. Geben Sie Folgendes ein: **Set-executionpolicy bypass**. Weitere Informationen finden Sie unter [Verwenden des Cmdlets Set-ExecutionPolicy](https://technet.microsoft.com/library/ee176961.aspx).

     > [!IMPORTANT]
     > Legen Sie **Set-Executionpolicy Bypass** nur in der 64-Bit-Konsole von PowerShell fest. Wenn Sie den Wert in der 32-Bit-Konsole von PowerShell festlegen, werden die Skripts nicht ausgeführt.

## <a name="add-the-script-to-the-vmm-library"></a>Hinzufügen des Skripts zur VMM-Bibliothek

Falls Sie über eine VMM-Quellwebsite verfügen, können Sie ein Skript auf dem VMM-Server erstellen. Fügen Sie das Skript dann in Ihren Wiederherstellungsplan ein.

1. Erstellen Sie einen neuen Ordner in der Bibliothekfreigabe. Beispiel: „\<VMM-Servername >\MSSCVMMLibrary\RPScripts“. Platzieren Sie den Ordner auf dem VMM-Quell- und -Zielserver.
1. Erstellen Sie das Skript. Geben Sie dem Skript beispielsweise den Namen „RPScript“. Überprüfen Sie, ob das Skript erwartungsgemäß funktioniert.
1. Platzieren Sie das Skript im Ordner „\<VMM-Servername>\MSSCVMMLibrary“ auf den VMM-Quell- und -Zielservern.

## <a name="add-the-script-to-a-recovery-plan"></a>Hinzufügen des Skripts zu einem Wiederherstellungsplan

Nachdem Sie die virtuellen Computer oder Replikationsgruppen einem Wiederherstellungsplan hinzugefügt und den Plan erstellt haben, können Sie das Skript der Gruppe hinzufügen.

1. Öffnen Sie den Wiederherstellungsplan.
1. Wählen Sie in der Liste **Schritte** ein Element aus. Wählen Sie dann **Skript** oder **Manuelle Aktion** aus.
1. Geben Sie an, ob Sie das Skript oder die Aktion vor oder nach dem ausgewählten Eintrag hinzufügen möchten. Verwenden Sie die Schaltflächen **Nach oben** und **Nach unten**, um die Position des Skripts nach oben oder unten zu verschieben.
1. Wenn Sie ein VMM-Skript hinzufügen, wählen Sie **Failover to VMM script** (Failover zu VMM-Skript) aus. Geben Sie unter **Skriptpfad** den relativen Pfad zur Freigabe ein. Geben Sie z.B. „ **\RPScripts\RPScript.PS1**“ ein.
1. Wenn Sie ein Azure Automation-Runbook hinzufügen, geben Sie das Automation-Konto an, in dem sich das Runbook befindet. Wählen Sie dann das gewünschte Azure-Runbookskript aus.
1. Führen Sie ein Failover für den Wiederherstellungsplan aus, um sicherzustellen, dass das Skript wie erwartet funktioniert.


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum [Ausführen von Failovern](site-recovery-failover.md).

