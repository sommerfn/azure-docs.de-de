---
title: Vorbereiten von virtuellen VMware-Computern mit Linux für die Migration zu Azure mit Azure Migrate-Servermigration | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie einen virtuellen Linux-Computer für die Migration zu Azure mit Azure Migrate-Servermigration vorbereiten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/14/2018
ms.author: raynew
ms.openlocfilehash: efc410699ef6f4722857c812b38473c8f54b911b
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810217"
---
# <a name="prepare-linux-vmware-vms-for-migration-to-azure"></a>Vorbereiten von virtuellen Linux VMware-VMs für die Migration zu Azure 

In diesem Artikel wird beschrieben, wie Sie virtuelle VMware-Computer mit Linux vorbereiten, um sie unter Verwendung von [Azure Migrate](migrate-overview.md) zu Azure zu migrieren. 

> [!NOTE]
> Azure Migrate-Servermigration befindet sich derzeit in der öffentlichen Vorschau. Mithilfe der vorhandenen GA-Version von Azure Migrate können Sie VMs für die Migration ermitteln und bewerten, aber die tatsächliche Migration wird in der vorhandenen GA-Version nicht unterstützt.

Bereiten Sie Linux-Computer wie folgt vor:

1. Installieren Sie Linux Integration Services für Hyper-V. Bei neueren Versionen von Linux-Distributionen ist dies möglicherweise standardmäßig installiert.
2. Erstellen Sie das Linux-Initialisierungsimage neu, damit es die erforderlichen Hyper-V-Treiber enthält und damit der virtuelle Computer in Azure gestartet wird (für einige Verteilungen erforderlich).
3. Aktivieren Sie die Protokollierung der seriellen Konsole zur Problembehandlung. [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console)
4. Aktualisieren Sie die Gerätezuordnungsdatei mit Zuordnungen von Gerätenamen und Volumes so, dass persistente Gerätebezeichner verwendet werden.
5. Aktualisieren Sie fstab-Einträge so, dass persistente Volumebezeichner verwendet werden.
6. Entfernen Sie alle udev-Regeln, die Schnittstellennamen basierend auf der MAC-Adresse usw. reservieren.
7. Aktualisieren Sie Netzwerkschnittstellen zum Empfangen von DHCP-IP-Adressen.
8. Stellen Sie sicher, dass SSH aktiviert ist. Überprüfen Sie, ob der sshd-Dienst so konfiguriert ist, dass er beim Neustart automatisch gestartet wird.
9. Stellen Sie sicher, dass eingehende SSH-Verbindungsanforderungen nicht durch die Betriebssystemfirewall oder IP-Tabellenregeln blockiert werden.

[Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console) zu diesen Änderungen finden Sie in den beliebtesten Linux-Distributionen.

## <a name="sample-script"></a>Beispielskript

Dieses Skript (prepare-for-azure.sh) enthält ein Beispiel für die Vorbereitung von Linux-Computern. Das Skript funktioniert möglicherweise nicht für alle Verteilungen und Umgebungen, aber es ist ein nützlicher Ausgangspunkt.

Das Skript zeigt, wie Sie folgende Aufgaben ausführen: 

- Erneutes Generieren des Linux-Initialisierungsimages mit den erforderlichen Treibern, falls erforderlich.
- Aktualisieren der fstab-Einträge zur Verwendung persistenter Volumebezeichner.
- Weiterleiten der Konsolenprotokolle an den seriellen Port.
- Aktivieren der Azure-Konsole für den seriellen Zugriff
- Entfernen von udev net-Regeln
- Hinzufügen eines Skripts, das beim Starten des virtuellen Computers ausgeführt wird. Hiermit wird überprüft, ob der Computer in Azure ausgeführt wird. Ist dies der Fall, wird die Netzwerkkonfiguration auf dem virtuellen Computer aktualisiert, und die erste Ethernet-Schnittstelle wird so konfiguriert, dass DHCP zum Abrufen einer IP-Adresse verwendet wird.

### <a name="before-you-start"></a>Vorbereitung

- Das Beispielskript enthält Beispielschritte. Es sollte nicht auf Produktionssystemen ausgeführt werden. Der virtuelle Computer, auf dem das Beispielskript ausgeführt wird, könnte beschädigt werden.
- Es wird empfohlen, dass Sie das Skript auf einem virtuellen Testcomputer ausführen. Bevor Sie beginnen, erstellen Sie eine VM-Sicherung oder -Momentaufnahme, damit Sie den virtuellen Computer bei Bedarf wiederherstellen können. 
- Das Skript funktioniert, wenn eine der folgenden Linux-Distributionen auf dem virtuellen Computer ausgeführt wird:
    - Red Hat Enterprise Linux 6.5+, 7.1+
    - Cent OS 6.5+, 7.1+
    - SUSE Linux Enterprise Server 12 SP1,SP2, SP3
    - Ubuntu 14.04, 16.04, 18.04
    - Debian 7, 8

### <a name="run-the-script"></a>Ausführen des Skripts

1. Kopieren Sie das Skript mithilfe von sftp oder einem scp-Client wie FileZilla oder WinScp auf die Linux-Test-VM.
2. Verwenden Sie SSH im Linux-Computer unter Verwendung eines Administratorkontos.
3. Navigieren Sie zum Skriptverzeichnis.
4. Um das Skript in eine ausführbare Datei umzuwandeln, führen Sie **sudo chmod 777 prepare-for-azure.sh** aus.
5. Führen Sie das Skript mit **./prepare-for-azure.sh** aus.

So wird das Skript ausgeführt:

![Linux-Skript](./media/how-to-prepare-linux-for-migration/script1.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script2.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script3.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script4.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script5.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script6.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script7.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script8.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script9.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script10.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script11.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script12.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script13.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script14.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script15.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script16.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script17.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script18.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script19.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script20.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script21.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script22.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script23.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script24.png)
![Linux-Skript](./media/how-to-prepare-linux-for-migration/script25.png)



## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das Erstellen von Gruppen mit hoher Vertrauenswürdigkeit mithilfe der [Zuordnung von Computerabhängigkeiten](how-to-create-group-machine-dependencies.md).
- [Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
