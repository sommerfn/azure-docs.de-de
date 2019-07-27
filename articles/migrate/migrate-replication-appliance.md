---
title: Architektur der Azure Migrate-Replikationsappliance | Microsoft-Dokumentation
description: Bietet eine Übersicht über die Azure Migrate-Replikationsappliance.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: raynew
ms.openlocfilehash: 4f4dc307bee4190a0e94ace493053e0cfd01150e
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810109"
---
# <a name="replication-appliance"></a>Replikationsappliance

In diesem Artikel wird die Replikationsappliance beschrieben, die von Azure Migrate: Serverbewertung bei der Migration von VMware-VMs, physischen Computern und privaten/öffentlichen Cloud-VMS zu Azure mithilfe einer Agent-basierten Migration verwendet wird. 

Das Tool ist im [Azure Migrate](migrate-overview.md)-Hub verfügbar. Der Hub stellt native Tools für die Bewertung und Migration sowie Tools aus anderen Azure-Diensten und von unabhängigen Softwareanbietern (ISVs) bereit.


## <a name="appliance-overview"></a>Appliance – Übersicht

Die Replikationsappliance wird als einzelner lokaler Computer bereitgestellt, entweder als VMware-VM oder als physischer Server. Sie führt folgende Komponenten aus:
- **Replikationsappliance**: Die Replikationsappliance koordiniert die Kommunikation und verwaltet die Datenreplikation für lokale VMware-VMs und physische Server, die nach Azure repliziert werden.
- **Prozessserver** Der Prozessserver, der standardmäßig auf der Replikationsappliance installiert ist, und folgende Schritte ausführt:
    - **Replikationsgateway**: Fungiert als Replikationsgateway. Empfängt Replikationsdaten von für die Replikation aktivierten Computern. Optimiert Replikationsdaten durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an Azure.
    - **Agent-Installer**: Führt die Pushinstallation des Mobility Service aus. Dieser Dienst muss auf jedem lokalen Computer, den Sie für die Migration replizieren möchten, installiert sein und ausgeführt werden.

## <a name="appliance-deployment"></a>Bereitstellung einer Appliance

**Bereitgestellt als** | **Verwendung** | **Details**
--- | --- |  ---
Virtueller VMware-Computer | Wird normalerweise verwendet, wenn VMware-VMs mit dem Azure Migrate Migrationstool mit Agent-basierter Migration migriert werden. | Sie laden die OVA-Vorlage aus dem Azure Migrate-Hub herunter und importieren sie in vCenter Server, um die Appliance-VM zu erstellen.
Ein physischer Computer | Wird bei der Migration lokaler physischer Server verwendet, wenn Sie über keine VMware-Infrastruktur verfügen oder keine VMware-VM mit einer OVA-Vorlage erstellen können. | Sie laden ein Software-Installationsprogramm vom Azure Migrate Hub herunter und führen es aus, um den Appliancecomputer einzurichten.

## <a name="appliance-deployment-requirements"></a>Anforderungen für die Appliancebereitstellung

Schauen Sie sich [Bereitstellungsanforderungen](migrate-support-matrix-vmware.md#agent-based-migration-replication-appliance-requirements) an.



## <a name="appliance-license"></a>Appliancelizenz
Die Appliance ist mit einer Windows Server 2016-Evaluierungslizenz ausgestattet, die 180 Tage lang gültig ist. Wenn der Evaluierungszeitraum fast abgelaufen ist, empfiehlt es sich, eine neue Appliance herunterzuladen und bereitzustellen oder die Betriebssystemlizenz der Appliance-VM zu aktivieren.

## <a name="replication-process"></a>Replikationsprozess

1. Wenn Sie die Replikation für einen virtuellen Computer aktivieren, beginnt die erste Replikation in Azure Storage mithilfe der angegebenen Replikationsrichtlinie. 
2. Der Datenverkehr wird über das Internet auf öffentliche Endpunkte von Azure Storage repliziert. Das Replizieren von Datenverkehr über ein virtuelles privates Site-to-Site-Netzwerk (VPN) von einem lokalen Standort nach Azure wird nicht unterstützt.
3. Nachdem die erste Replikation abgeschlossen ist, beginnt die Deltareplikation. Nachverfolgte Änderungen für einen Computer werden protokolliert.
4. Die Kommunikation erfolgt folgendermaßen:
    - VMs kommunizieren mit dem Replikationsserver über den Port HTTPS 443 für eingehenden Datenverkehr, um die Replikationsverwaltung durchzuführen.
    - Die Replikationsappliance orchestriert die Replikation mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr.
    - Virtuelle Computer senden Replikationsdaten an den Prozessserver (der auf der Replikationsappliance ausgeführt wird) über Port HTTPS 9443 für eingehenden Datenverkehr. Dieser Port kann geändert werden.
    - Der Prozessserver empfängt Replikationsdaten, optimiert und verschlüsselt sie und sendet sie über den ausgehenden Port 443 an den Azure-Speicher.
5. Die Replikationsdatenprotokolle werden zunächst in einem Cachespeicherkonto in Azure gespeichert. Diese Protokolle werden dann verarbeitet, und die Daten werden auf einem verwalteten Azure-Datenträger gespeichert.

![Architecture](./media/migrate-replication-appliance/architecture.png)

## <a name="appliance-upgrades"></a>Appliance-Upgrades

Das Upgrade der Appliance erfolgt manuell vom Azure Migrate-Hub. Sie sollten immer die neueste Version ausführen.

1. Klicken Sie in „Azure Migrate“ > „Server“ > „Azure Migrate: Server Bewertung“, „Infrastrukturserver“ auf **Konfigurationsserver**.
2. Wenn eine neue Version der Replikationsappliance verfügbar ist, wird in **Konfigurationsserver** unter **Agentversion** ein Link angezeigt. 
3. Laden Sie das Installationsprogramm auf den Replikationsappliancecomputer herunter, und installieren Sie das Upgrade. Das Installationsprogramm erkennt die aktuell auf dem Computer ausgeführte Version.
 
## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie, wie](tutorial-assess-vmware.md#set-up-the-appliance-vm) Sie die Appliance für VMware einrichten.
[Erfahren Sie, wie](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) Sie die Appliance für Hyper-V einrichten.

